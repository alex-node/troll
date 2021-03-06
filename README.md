## troll
#### Language sentiment analysis and neural networks... for trolls.

[![Build Status](https://secure.travis-ci.org/thisandagain/troll.png)](http://travis-ci.org/thisandagain/troll)

Troll is a tool for performing sentiment analysis (ie: "is this naughty or nice") on arbitrary blocks of text and associating it with a unique user. Using this data, combined with a rather naïve neural network and some training data, users can be indentified as "trolls".

### Installation
Troll uses [Redis](http://redis.io/) for data storage. Once Redis is up and running, you can install Troll using NPM:
```bash
npm install troll
```

### String Analysis
```javascript
var troll   = require('troll');

troll.analyze('This is totally awesome!', 'user123', function (err, result) {
    console.log(result);    // 2.5
});

troll.analyze('This is lame.', 'user456', function (err, result) {
    console.log(result);    // -1.333
});
```

### Training
Before attempting to classify a user, you'll need to train Troll. You can specify your own training data or use a basic set that is included. To load the included training set:
```javascript
troll.train(function (err, result) {
    console.dir(result);    // { error: 0.0049931996067587685, iterations: 802 }
});
```

### User Classification
Once trained, now you can classify:
```javascript
troll.classify('user123', function (err, result) {
    console.dir(result);    // { total: 9, sum: 36, troll: 0.010294962292857838 }
});
```

The value returned for the `troll` key represents the probability of that user being a troll. In other words, a value of `0` would likely represent a particularly friendly user, while a value of `1` would be... uh, Ted Dziuba?

---

### Modifing The Sentiment Engine
The underlying [sentiment analysis module](https://github.com/thisandagain/sentiment) supports "injecting" additional key/value pairs. This is useful in certain situations where you may want to exclude or even blacklist certain words based on a particular use case. For example:
```javascript
troll.inject({
    'stupid': -5,
    'lame': -5
});

troll.analyze('This is totally stupid and lame!', 'user123', function (err, result) {
    console.log(result);    // -5.833
});
```

### Redis Connection Options
Troll uses your environment by looking at `process.env` for connection settings. If none are found, default [Redis](http://redis.io/) connection settings are used:
```
TROLL_HOST: null
TROLL_PORT: null
TROLL_PASS: null
```

---

### Testing
```bash
npm test
```

### Credits
- Neural network by [Heather Arthur](https://github.com/harthur)
- Training data inferred and subsequently condensed by scraping [Boing Boing's](http://boingboing.net) reader comments.
