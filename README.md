```
mkdir foo && cd foo
npm init
[press enter a lot]
npm install --save -E app-context
vi .babelrc
[
{
  "presets": ["es2015", "stage-0"],
  "plugins": ["add-module-exports", "transform-decorators-legacy"]
}
]
vi app-context.js
[
export default function() {
  this.runlevel('configured').use('connie', 'file', 'config.json');
  this.runlevel('connected').use('access-mongo', '$mongodb');
  this.runlevel('running').use('express');
}
]
app-context install
[follow instructions for babel dependencies]
app-context install
vi config.json
[
{
  "mongodb": "mongodb://localhost/foo"
}
]

-- at this point you could `app-context console` or `app-context start`

npm install --save -E routers
vi router.js
[
import { classApiResolver } from 'routers'

export default function(app) {
  const resolve = classApiResolver('routes');

  app.get('/test', resolve('test#index'));
}
]
mkdir routes
vi routes/test.js
[
class TestRoutes {
  index() {
    return 'foo';
  }
}

export default TestRoutes;
]

-- you could now run `app-context start` and go to http://localhost:3000/test

vi routes/test.js
[
const { mongodb } = APP;
const User = mongodb.createModel('users');

class TestRoutes {
  index() {
    return User.array();
  }
}

export default TestRoutes;
]

-- try `app-context start` and go to http://localhost:3000/test again

[for fun...]
app-context console
[
APP.mongodb.createModel('users').save({name: 'Matt Insler', email: 'matt.insler@gmail.com'})
]

-- now try the /test route again
```
