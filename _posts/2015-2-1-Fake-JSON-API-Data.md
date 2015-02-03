---
layout: post
title: Faking JSON API data
---

Quick tip for when throwing together a front-end for a JSON API that you haven't yet finished, or maybe even started!

Rather than kludging together a giant Hash/Array of fake data in a service or whatnot, you can use one of these wonderful services:

# [JSON Generator](http://www.json-generator.com/)
JSON Generator will allow you quickly and easily build your data where you can then save it and conveniently access it using a special URL.
Use this if your result is non-trivial as it helps to see the generator template infront of you.

# [FillText](http://www.filltext.com/)
Using FillText, you can start generating data in lightening speed by just adding query parameters to the URL, check it out:

http://filltext.com/?rows=20&fname={firstName}&lname={lastName}

```JSON
[{"fname":"Ghaffar","lname":"Poole"},{"fname":"Craig","lname":"Wiedenmann"},{"fname":"Segun","lname":"Harmati"},{"fname":"Clyde","lname":"Birnstill"},{"fname":"Omar","lname":"Reyes"}]
```

Use this if you want plain structured data and don't forget the delay=n parameter!
