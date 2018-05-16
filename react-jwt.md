# React JWTs

## Goal
1. Build authentication for React with Passport

## JWTs steps
[Why use jwts](https://cdn-images-1.medium.com/max/1600/1*d6YcPvq7TeU0DTamj629xw.png)
1. Log in to server
2. Server sends back a JWT
3. Every time you request a page, your browser sends the JWT with it
4. If the JWT is approved, you are shown the page. If not, error.

## JWT parts
1. Header:
  * algorithm
  * type: JWT
2. Payload
  * subject (registered claim)
  * issuer (registered claim)
  * expiration (registered claim)
  * username, name, privileges (public claims)
3. Signature
  * encoded with secret key

