---
title:  "TJCTF 2022 -- web/game-leaderboard"
date:   2022-05-17 03:00:00 -0400
categories: TJCTF2022 web
tags: TJCTF2022 web SQLi
# classes: wide
---
A leaderboard with a filter input. What could go wrong?

# Prompt
![Challenge Prompt][img-prompt]

# Challenge
To start off the web challenges, we get a link to [game-leaderboard.tjc.tf][challenge-link], where we're shown a leaderboard table with a input box for filtering scores:

![Leaderboard][img-page]

Immediately, I knew it was likely an SQL injection. I'm guessing the data is pulled from some database based on the filter that is submitted from the user.

We're also provided the [index.js][src-index], for which the contents are shown below:

```js
const express = require('express')
const cookieParser = require('cookie-parser')
const fs = require('fs')
const app = express()

app.use(cookieParser())

app.set('view engine', 'ejs')
app.use(express.static(`${__dirname}/public`))
app.use(express.urlencoded({ extended: false }))

const db = require('./db')

const FLAG = fs.readFileSync(`${__dirname}/flag.txt`).toString().trim()

const getLeaderboard = (minScore) => {
    const where = (typeof minScore !== 'undefined' && minScore !== '') ? ` WHERE score > ${minScore}` : ''
    const query = `SELECT profile_id, name, score FROM leaderboard${where} ORDER BY score DESC`
    const stmt = db.prepare(query)

    const leaderboard = []
    for (const row of stmt.iterate()) {
        if (leaderboard.length === 0) {
            leaderboard.push({ rank: 1, ...row })
            continue
        }
        const last = leaderboard[leaderboard.length - 1]
        const rank = (row.score == last.score) ? last.rank : last.rank + 1
        leaderboard.push({ rank, ...row })
    }

    return leaderboard
}

app.get('/', (req, res) => {
    const leaderboard = getLeaderboard()
    return res.render('leaderboard', { leaderboard })
})

app.post('/', (req, res) => {
    const leaderboard = getLeaderboard(req.body.filter)
    return res.render('leaderboard', { leaderboard })
})

app.get('/user/:userID', (req, res) => {
    const leaderboard = getLeaderboard()
    const total = leaderboard.length

    const profile = leaderboard.find(x => x.profile_id == req.params.userID)
    if (typeof profile === 'undefined') {
        return res.render('user_info', { notFound: true })
    }

    const flag = (profile.rank === 1) ? FLAG : 'This is reserved for winners only!'
    return res.render('user_info', { total, flag, ...profile })
})

app.listen(3000, () => {
    console.log('server started')
})
```

## Problem Description
After reading through the file, it seems like the flag is read from a file. It can only be viewed from the `GET /user/:userID` endpoint where the `userID` parameter is the id of the team ranking #1.

The SQL query fetches the fields `profile_id`, `name`, and `score`, but in the leaderboard, the profile ID is not shown. We need to find that somehow.

## Exploitation
The very first thing I did was to see if it was vulnerable to SQL injection attacks.

I tried applying the filter of `'`, a single quote. The application did not allow this, but using Firefox's network tab in the developer tools or Burp suite, I was able to change the query to `'`. The response was a `500 Internal Server Error`.

Then I took a look at the source again and saw how they were building the query. The code was formatting the input string directly into the query.

The query was put together with the query below where `${minScore}` is replaced by the input we submit.

```sql
SELECT profile_id, name, score FROM leaderboard WHERE score > ${minScore} ORDER BY score DESC
```

This is damn vulnerable to SQLi. I then tried submitting `0 or 1`, which should result in the query below and return all the entries in the table.

```sql
SELECT profile_id, name, score FROM leaderboard WHERE score > 0 or 1 ORDER BY score DESC
```

The above query worked and returned all of the entries, including the ones with negative scores.

Now, how do we get the leaderboard to show us the `profile_id` column? If I had control of the entire query, I would've just done something like:

```sql
select profile_id, profile_id AS name, score FROM leaderboard ORDER BY score DESC
```

But we only have control of the query after the `WHERE` in the original query. I used a `UNION` to append entries another query result where the `name` column, which gets displayed, contains the `profile_id`.

## Solution
The input I gave was `0 AND 0 UNION SELECT profile_id, profile_id as name, score FROM leaderboard`, which should result in the query below, which puts the `profile_id` values in the `name` column instead.

```sql
SELECT profile_id, name, score FROM leaderboard WHERE score > 0 AND 0 UNION SELECT profile_id, profile_id as name, score FROM leaderboard ORDER BY score DESC
```

![Leaderboard shows profile_id instead of name][img-ids]

Now, if we go the the user page with the user ID (/user/ed38f25b3f962336), we get the flag.

![User page with flag displayed][img-flag]

## Flag
The flag is: `tjctf{h3llo_w1nn3r_0r_4re_y0u?}`

[img-prompt]: /assets/img/tjctf2022/game-leaderboard/0-prompt.png
[challenge-link]: https://game-leaderboard.tjc.tf
[img-page]: /assets/img/tjctf2022/game-leaderboard/1-page.png
[src-index]: /assets/data/tjctf2022/game-leaderboard/index.js
[img-ids]: /assets/img/tjctf2022/game-leaderboard/2-ids.png
[img-flag]: /assets/img/tjctf2022/game-leaderboard/3-flag.png
