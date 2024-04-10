---
type: 'docs'
bookCollapseSection: false
bookHidden: false
bookToC: true
bookComments: false
bookSearchExclude: false
bookFlatSection: true
weight: 1
---

> https://darutk.medium.com/oauth-2-0-client-authentication-4b5f929305d4

# Client Type

A client application has a client type. ([RFC6749 2.1](https://datatracker.ietf.org/doc/html/rfc6749#section-2.1))

The value of attribute is either 'public' or 'confidential'.

If the client type is 'confidential', when the client application accesses the token endpoint, client authentication is required.⭐

> Read more about [Client Types](https://datatracker.ietf.org/doc/html/rfc6749#section-2.1)

# Client Authentication Methods

## client_secret_post

When client application requests an access token, it sends the 'Client ID', 'Client Secret' in the request body.

This name 'client_secret_post' is shown from [OIDC Core : 9. Client Authentication](https://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication).

## client_secret_basic

Another way to include a 'Client ID', 'Client Secret' in the request is to use [Basic Authentication](https://datatracker.ietf.org/doc/html/rfc7617).

Embed the `Base64($ClientID:$ClientSecret)` in the `Authorization` header.

This name 'client_secret_basic' is shown from [OIDC Core : 9. Client Authentication](https://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication).

'client_secret_post' and 'client_secret_basic' are client authentication methods described in [RFC 6749 : 2.3.1. Client Password](https://datatracker.ietf.org/doc/html/rfc6749#section-2.3.1).

## client_secret_jwt

There is **an indirect way to prove that a client application has a client secret** without including the client secret in the request.

This method is using signature. For example, the client application signs some data with the client secret and sends it to the token endpoint.

By verifying the signature, the auth server can confirm that client application has the client secret.

There is a rule for the format of the data. Details are described in [RFC 7523 : 2.2. Using JWTs for Client Authentication](https://datatracker.ietf.org/doc/html/rfc7523#section-2.2).

Signing this JSON is conducted by the way defined in [RFC 7515](https://datatracker.ietf.org/doc/html/rfc7515). As a result of the signing, a JWT is generated. In the context of client authentication, this JWT is called a **client assertion.**

The client assertion is included in a token request as the value of the 'client_assertion' request parameter. At the same time, the 'client_assertion_type' request parameter needs to be included. The value is a fixed string, 'urn:ietf:params:oauth:client-assertion-type:jwt-bearer'. ⭐

This name 'client_secret_jwt' is shown from [OIDC Core : 9. Client Authentication](https://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication).

## private_key_jwt

In 'client_secret_jwt', the client application signs the data with a shared key(client secret).

On the other hand, in 'private_key_jwt', the client application signs the data with a asymmetric key(private key).

This name 'private_key_jwt' is shown from [OIDC Core : 9. Client Authentication](https://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication).

## tls_client_auth

There is a specification titled [RFC8705 OAuth 2.0 Mutual-TLS Client Authentication and Certificate-Bound Access Tokens](https://www.rfc-editor.org/rfc/rfc8705.html). [2. Mutual-TLS for OAuth Client Authentication](https://www.rfc-editor.org/rfc/rfc8705.html#name-mutual-tls-for-oauth-client) of the specification defines client authentication methods which use a client certificate.

_skip_

To use a PKI certificate in this client authentication method, a client must register information which identifies the subject of the certificate into the auth server in advance.⭐

The auth server confirms that the subject of the client certificate matches the one registered in advance. ⭐

Because a client certificate dosen't include a 'Client ID' for the OAuth 2.0 context, the client can't be identified only by the client certificate. Therefore, when a certificate-based client authentication method is used, a 'Client ID' needs to be included in the request. Basically, the 'Client ID' is included in the 'client_id' request parameter.⭐

This name 'tls_client_auth' is shown from [MTLS, 2.1.1 PKI Method Metadata Value](https://www.rfc-editor.org/rfc/rfc8705.html#name-pki-method-metadata-value).

## self_signed_tls_client_auth

Instead of a PKI certificate, a self-signed certificate can be used for certificate-based client authentication. To use a self-signed certificate, the client must register the certificate in advance.⭐

This name 'self_signed_tls_client_auth' is shown from [MTLS, 2.2.1 Self-Signed Method Metadata Value ](https://www.rfc-editor.org/rfc/rfc8705.html#name-self-signed-method-metadata).