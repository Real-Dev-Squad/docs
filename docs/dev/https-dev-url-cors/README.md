# Security

The same-origin policy is a critical security mechanism that restricts how a document or script loaded from one origin can interact with a resource from another origin. It helps isolate potentially malicious documents, reducing possible attack vectors.

> Check out this [MDN link](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) to understand what constitutes as Same Origin

Two URLs have same origin if the [protocol](https://developer.mozilla.org/en-US/docs/Glossary/Protocol), [port](https://developer.mozilla.org/en-US/docs/Glossary/Port) and [host](https://developer.mozilla.org/en-US/docs/Glossary/Host) is same for both.

We only allow `*.realdevsquad.com` to access our backend APIs via [Access-Control-Allow-Origin](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Origin)

When calling our APIs, our cookies are ensured to be restricted for access (Read why [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies#restrict_access_to_cookies))

- HttpOnly
- Secure

## Expected problem

During most frontend development, when you run a local dev server, it will run on `http://localhost:<some-port>`, e.g `http://localhost:3000`

This `localhost` server, you will **NOT** be able to use our backend APIs directly from the browser, because the cookies will not be sent by the browser according to the origins not matching because:

- **http** : Cookie is meant to be for secure `https`
- **localhost** : Domain doesn't match `*.realdevsquad.com`
- **port** : Should be default `443` for https (or 80 for http)

## Solution

If we want to make calls to our APIs beloging to \*.realdevsquad.com, then we need to be originating those requests from the same origin.

First We also need to run our local development server on a url that is serving over **https** so that our prod/staging _secure_ cookies can be sent in our requests originating form the browser, automatically as expected. To do so you can right click in vs code ans select open with live server.

We basically need this kind of traffic redirection:

> https://dev.realdevsquad.com -> http://localhost:`<some-port>`

### 1. 🔗 DNS

We need to resolve `dev.realdevsquad.com` to `localhost`.

We do that by adding the entry `127.0.0.1 dev.realdevsquad.com` to our `/etc/hosts` file on our local computer.

To do so you can follow [this](https://www.groovypost.com/howto/edit-hosts-file-windows-10/) or [this](https://www.howtogeek.com/howto/27350/beginner-geek-how-to-edit-your-hosts-file/);

After that you hosts file should look like this ![hosts file](public\assets\hosts.jpeg). Make sure `127.0.0.1 localhost`
`::1 localhost` `127.0.0.1 dev.realdevsquad.com` should not be commented.

Now, if you go to `dev.realdevsquad.com` on your browser, the OS will automatically resolve it to `localhost`.
If you go to `dev.realdevsquad.com:3000`, it will point to `localhost:3000` and show you your running app. (if you have a dev server running on port 3000)

(Read more [here](https://www.howtogeek.com/howto/27350/beginner-geek-how-to-edit-your-hosts-file/) and [here](https://tldp.org/LDP/solrhe/Securing-Optimizing-Linux-RH-Edition-v1.3/chap9sec95.html))

### 2. 🔐 HTTPS

We have to ensure that we can now visit `https://dev.realdevsquad.com` (which by default is port 443).

Opening this now will show you a problem of your TLS/SSL certificate. This is a security feature of the browser to ensure that your site is legit.

We need to setup a **reverse proxy**(highly encourage you to read about it [here](https://www.cloudflare.com/learning/cdn/glossary/reverse-proxy/)), that runs directs our traffic to port 443 to our dev server

A possible solution is installing [local-ssl-proxy](https://www.npmjs.com/package/local-ssl-proxy).
This is a simple solution of running a simple SSL HTTP proxy using a self-signed certificate.(Intended for local development only)

Run `local-ssl-proxy --source 443 --target <your-port e.g 3000> --hostname dev.realdevsquad.com`

Now if you visit https://dev.realdevsquad.com on your browser, expect to get a warning because the certificate is self-signed. But since we set this up on purpose, it's safe to ignore during development.

Extra: For a more powerful proxy solution, try out [mitmproxy](https://mitmproxy.org/)

## Final

🎉 Once you get your development app to run on `https://dev.realdevsquad.com`, you will be able to access the backend APIs without any CORS issues. Happy development!

If you want to have a single command that runs the dev server over https, check out [this package.json](https://github.com/Real-Dev-Squad/website-crypto/pull/203/)
