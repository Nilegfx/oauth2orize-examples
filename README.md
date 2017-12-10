#### my updates

for better understanding
https://developers.google.com/actions/identity/oauth2-code-flow 
---

from the browser
```bash
http://localhost:3000/dialog/authorize?response_type=code&client_id=xyz123&redirect_uri=http://localhost:3001
```


```bash
curl -X POST \
  http://localhost:3000/oauth/token/ \
  -H 'authorization: Basic eHl6MTIzOnNzaC1wYXNzd29yZA==' \
  -H 'content-type: application/x-www-form-urlencoded' \
  -d 'code=<<YOUR_CODE>>&grant_type=authorization_code&redirect_uri=http%3A%2F%2Flocalhost%3A3001'
```

  
```sh
 curl -X GET \
    http://localhost:3000/api/userinfo \
    -H 'authorization: Bearer <<ACCESS_TOKEN>>' \
    -H 'cache-control: no-cache' \
    -H 'postman-token: 897e5c0e-7f9e-beef-1fc5-03e1c728100a'
```  
 
---

oauth2orize: oauth2 provider example
===

This example shows a provider which grants tokens in exchange for codes for

  * The client application
  * A user of the client application

Install
===

```bash
git clone https://github.com/gerges-beshay/oauth2orize-examples.git
pushd oauth2orize-examples
npm install
```

Usage
===

```bash
node app.js
```

Visit <http://localhost:3000/login> to see the server running locally.

Provider / Consumer Walkthrough
===

Interacting with this provider directly doesn't showcase it's oauth2 functionality.

1. Visiting `/` takes you to a blank page... not too interesting
2. `/login` will ask you for credentials.
  * If you login before an oauth request you are taken directly to permission dialog when that request happens
  * Otherwise you will be redirected here and then to the permission dialog
3. `/account` will allow you to see your user details

In order to demo what this is actually accomplishing you'll need to run a consumer.

See <https://github.com/coolaj86/example-oauth2orize-consumer>

API
===

Below is a mapping of the API in the context of a passport-strategy

* `/dialog/authorize` is the `authorizationURL`.
* `/oauth/token` is the `tokenURL`
* `/api/userinfo` is a protected resource that requires user permission
* `/api/clientinfo` is a protected resource that requires a token generated from the client's id and secret
* Usage of `scope` is not demonstrated in this example.

The standalone usable resources are

* `GET /` nothing
* `GET /login` lets you login, presented by `/dialog/authorize` if you haven't logged in
* `POST /login` processes the login
* `GET /logout` lets you logout
* `GET /account` lets your view your user info

And then some internal resources that are of no concern for standalone users or consumers

* `POST /dialog/authorize/decision`, processes the allow / deny
