# [Static OIDC Issuer](https://github.com/fdelamor/sso-dev/)

<small><a href="https://github.com/fdelamor/sso-dev/">github.com/fdelamor/sso-dev</a></small>

A real, working OpenID Connect Configuration for Development \
(host statically on GitHub Pages, or wherever)

# Usage

Add any of these issuers to your web app's OpenID issuer whitelist:

-   <https://fdelamor.github.io/sso-dev/> (primary, ecdsa)
-   <https://fdelamor.github.io/sso-dev/dev/> (same as primary, but using subpath)
-   <https://fdelamor.github.io/sso-dev/staging/> (a different set of keys)
-   <https://fdelamor.github.io/sso-dev/ec/> (both ecdsa keys)
-   <https://fdelamor.github.io/sso-dev/rsa/> (both rsa keys)

Then sign a token (with the corresponding key) and run with it:

```sh
b_auth_time="$(date '+%s')"
b_standard_claims='{
    "amr": ["pwd"],
    "aud": "https://beta.therootcompany.com",
    "auth_time": '"${b_auth_time}"',
    "email": "me@example.com",
    "email_verified": false,
    "iss": "https://fdelamor.github.io/sso-dev",
    "locale": "en-US",
    "sub": "xxxxxxxxxxxx",
    "zoneinfo": "America/Denver"
}'

keypairs sign --exp 1h ./key.ec.jwk.json \
    "${b_standard_claims}" \
    > token.jwt \
    2> sig.jws

curl https://example.com/api/profile \
    -H "Authorization: Bearer $(cat ./token.jwt)"
```

## Directory Structure

From the root of <https://fdelamor.github.io/sso-dev>:

<pre><code>
.
└─── oidc/
    └── <id>
        ├── .well-known/
        |   └── openid-configuration
        └── keys
</code></pre>

# LICENSE

Source: <https://github.com/fdelamor/sso-dev>
