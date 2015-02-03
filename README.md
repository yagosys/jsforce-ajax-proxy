# JSforce AJAX Proxy

A proxy server to access Salesforce API from JSforce JavaScript apps served outside of Salesforce.

As the same origin policy restricts communication to the Salesforce API from outer domain,
you should serve cross-domain proxy server when you build a app using JSforce outside of Salesforce.

This app will become obsolete immediately when Salesforce API supports CORS.
I hope the day will come soon.

## Usage

Start proxy server in your server environment which can run Node.js app (Heroku is the one you might choose).

Install required packages :

```
$ npm install
```

Run proxy server :

```
$ node proxy.js
```

When you use JSforce in your JavaScript app, set `proxyUrl` when creating `Connection` instance. 

```
var conn = jsforce.Connection({
  accessToken: '<access_token>',
  instanceUrl: '<instance_url>',
  proxyUrl: 'https://your-ajax-proxy-service.herokuapp.com/proxy/'
});

conn.query('SELECT Id, Name FROM Account', function(err, res) {
  // ...
});
```

## Note

You don't have to use this app when you are building a JSforce app in Visualforce,
because it works in the same domain as Salesforce API.


deploy on apcera continnum

apc app create ajax 
apc app update ajax --start-cmd "node proxy.js"
apc app start

demo@vm:~/jsforce-ajax-proxy$ apc app show ajax
╭──────────────────────┬───────────────────────────────────────────────────────────────────────────────╮
│ Job:                 │ ajax                                                                          │
├──────────────────────┼───────────────────────────────────────────────────────────────────────────────┤
│ FQN:                 │ job::/sandbox/admin::ajax                                                     │
│ State:               │ started                                                                       │
│ Running Instances:   │ 1/1 started                                                                   │
│                      │                                                                               │
│ Resources            │                                                                               │
│   CPU:               │ (no limit)                                                                    │
│   Memory:            │ 256MB                                                                         │
│   Disk:              │ 1GB                                                                           │
│   Network:           │ 5Mbps                                                                         │
│   Netmax:            │ (no limit)                                                                    │
│                      │                                                                               │
│ Process              │                                                                               │
│   Name:              │ app                                                                           │
│   Start Command:     │ node proxy.js                                                                 │
│                      │                                                                               │
│ Exposed Ports:       │ 0 (chosen by system, env: $PORT)                                              │
│                      │                                                                               │
│ Routes:              │ http://ajax.ewagxig17.continuum-demo.io [to port 0]                           │
│                      │                                                                               │
│ Tags:                │ app: ajax                                                                     │
│                      │                                                                               │
│ Packages:            │ package::/apcera/pkg/os::ubuntu-14.04                                         │
│                      │ package::/apcera/pkg/os::ubuntu-14.04-build-essential                         │
│                      │ package::/apcera/pkg/runtimes::python-2.7.8                                   │
│                      │ package::/apcera/pkg/packages::git-2.0.3                                      │
│                      │ package::/apcera/pkg/runtimes::node-0.10.30                                   │
│                      │ package::/sandbox/admin::ajax*                                                │
│                      │                                                                               │
│ Package Environment: │ PATH="$HOME/app/bin:$HOME/app/node_modules/.bin:$PATH"                        │
│                      │ _C_INCLUDE_PATH="/opt/apcera/python-2.7.8/include/python2.7:$_C_INCLUDE_PATH" │
│                      │ _PYTHONHOME="/opt/apcera/python-2.7.8"                                        │
│                      │ _PYTHONPATH="/opt/apcera/python-2.7.8/include/python2.7:$_PYTHONPATH"         │
│                      │ START_PATH="/app"                                                             │
╰──────────────────────┴───────────────────────────────────────────────────────────────────────────────╯
demo@vm:~/jsforce-ajax-proxy$


