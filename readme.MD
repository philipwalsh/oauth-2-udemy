# The Nuts and Bolts of OAuth 2.0

a udemy course, $9.99USD, presentd by Aaron Parecki (OAuth Expert and Author)

## Section 1

### history

before oauth, many companys doing their own thing, all trying to solve the same basic problem

oauth 1.0 was drafted and deployed at twitter

oauth 2.0 easier to understand, bearer tokens, at scale, mobile apps, auth server vs api server, etc..

now there are plenty of extensions

- oauth for native apps
- oauth for smart tvs
- etc

### oauth vs openid

oauth originally designed for apis, auth doesnt really care who the user is
same as a hotel keycard. clerk at desk gives out the key card (access token), the door doesnt care who is using that key card

openid connect takes oauth as foundation and sprinkles on the flavor, user identity information on top

open id is an extension of oauth

### key take away

oauth issues access token
openid connect issues id tokens

oauth = Access
OpenID Connect = ID

oauth for accessing apis
OpenID Connect for identifying users

## Section 2

### Terminolgy

user/device/app/api
resource owner/user agent/client/resource server

because the term application can mean many dirrent things to different people, its best to try not to use,

for oauth spec, application is the thing in the middle trying to access an api

authorization server manages access to the api it is protecting. auth server hands out the access token to the application.

Application never sees user password.

user redirect to the oauth server, gets a token that he brings back to the api

the above are roles, not necessarily distinct components

![oauth roles](images/oauth-roles.JPG)

### Application Types

server side apps can have secrets in a config or environment but client side apps cannot hold your secrets.

an app can have app credentials so the oauth server knows the app should be trying to authorize a user

without credentials, the ouath server wouldnt know its the real application making the request

knowing the apps identity and doling out an auth token are 2 distinct things

### key take aways

confidential apps - running on a server, can contain secrets
public clients - cannot have secrets, so auth server may never really know if the client is the real deal. alwys choose appropriate app type when registering the client

### user content (#7)

redirecting user to the auth server is critical to the flow

we can show user a consent form

consent form assures that the user is sitting at that computer, not some bot replaying password keystrokes

we can implement 2 form authentication at the auth server. now all apps have that extar level of sceurity without re writing them

first party and confidential apps typically can skip the consent form. rediect is still there, just possibly not the consent form. but you may want to use the consent form anyway, for extra security for the users.

### Front Channel vs Back Channel (#8)

back channel = https connection

we know the server (certificate) and the data is encrypted in transit, trusted response. a had delivered package.

front channel = url stored data
no direct link between sender and recipient. there is a middle man handling the package. not 100% sure.

oauth is a bit of a mix, adding in the user consent adds in the front channel portion

the flow - 1) send data to auth server 2) get access token

implicit flow - app sends data in query string to the auth server explaining what it is trying to do, auth then sends back the access token in query string, not safe and not recoemmended but it is in the spec. both flow steps in the front channel.

back channel should handle the access token

### Application Identity (#9)

each app has its own identifier, a Client ID.

better flow
a url is built up to redirect user to auth server.
params that describe the request the app is maiking

- scope of request
- rediret uri
- app cient id
- etc

browser takes user there, auth servers does his thing and has the redirect info back to the app, with an auth code and the client needs to use that auth code with client secret to validate it all. mobile apps and single page apps cannot prove their identity, no secrets, this is solved by the PKCE, proof key for code exchange. so before an interaction, client creates a tem secret, gives it to auth server, then uses it later during step 2. doesnt prove apps identity, just helps to keep a tken from being hijaaked

then the last step of client identification, the redirect uri, location where the auth server will send the user.

https redirect is part of the app identity, better than nothing

### Preparing for remainder of the course

- i created my account on okta
- i stored my okta creds in keepass
- i created a secrets.txt and added that to my git ignore

in the secrets file i am storing a few things from the okta ouath server for easy access during the course

- issuer uir
- authorization endpoint
- token endpoint
