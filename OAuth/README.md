## What is OAuth?
OAuth is a standard that enables authorization between services and applications. OAuth is commonly used in Single Sign-on scenarios, and enables a user to log in to a single services to access  multiple different services.

There are four entities in the OAuth flow:
1. Resource Owner: Entity that owns the resource. Typically is the user
2. Client: Service requesting access to the resource on behalf of the resource owner
3. Authorization Server: Server that authenticates the resource owner and issues access token to the client
4. Resource Server: Server hosting the resources the client requests to access

OAuth Flow
1. Client requests authorization from resource owner
2. Client receives an authorization grant from resource owner
3. Client presents the authorization grant to the authorization server
4. Client receives an access token to the authorization server
5. Client presents the access token to the resource server
6. Client receives the resource from the resource server



## Basic Checklist
- [ ] Identify if there is improper verification of the `redirect_uri` parameter
	- [ ] Manipulate the `redirect_uri` to be our attack server
	- [ ] Obtain the callback `authorization code` and finish the OAuth flow
>Validation of the `redirect-uri` may be flawed. Depending on how it is implemented, we can try the following:
>```http
>http://academy.htb.attacker.htb/callback
>http://academy.htb@attacker.htb/callback
>http://attacker.htb/callback?a=http://academy.htb
>http://attacker.htb/callback#http://academy.htb
>```
- [ ] Identify if there is missing or improperly validated `state` parameter
	- [ ] Perform a `CSRF` attack with forcing the victim to authenticate as our user
	- [ ] Craft the URL for the `callback authorization code` request
- [ ] Identify that the `authorization` request are reflected as hidden values in the response
	- [ ] Attempt reflected XSS
	- [ ] Attempt all injection attacks
- [ ] Identify an open redirect within the application & chain this attack with `redirect_uri`
>If the `redirect_uri` has proper whitelist checks on the redirect URL, this is secure unless we find an open redirect vulnerability that can be redirected to our domain.
>
>We can exploit this by making the `redirect_uri` go to the open redirect, such as
>`redirect_uri=http://valid.domain/redirect?u=http://attacker.server/callback`
>



## Tools
