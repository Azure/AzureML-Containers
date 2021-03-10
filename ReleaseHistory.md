Release History
===============

dev
---

-   \[Short description of non-trivial change.\]

2.25.1 (2020-12-16)
-------------------

**Bugfixes**

- Requests now treats `application/json` as `utf8` by default. Resolving
  inconsistencies between `r.text` and `r.json` output. (#5673)

**Dependencies**

- Requests now supports chardet v4.x.

2.25.0 (2020-11-11)
------------------

**Improvements**

- Added support for NETRC environment variable. (#5643)

**Dependencies**

- Requests now supports urllib3 v1.26.

**Deprecations**

- Requests v2.25.x will be the last release series with support for Python 3.5.
- The `requests[security]` extra is officially deprecated and will be removed
  in Requests v2.26.0.

2.24.0 (2020-06-17)
-------------------

**Improvements**

- pyOpenSSL TLS implementation is now only used if Python
  either doesn't have an `ssl` module or doesn't support
  SNI. Previously pyOpenSSL was unconditionally used if available.
  This applies even if pyOpenSSL is installed via the
  `requests[security]` extra (#5443)

- Redirect resolution should now only occur when
  `allow_redirects` is True. (#5492)

- No longer perform unnecessary Content-Length calculation for
  requests that won't use it. (#5496)

2.23.0 (2020-02-19)
-------------------

**Improvements**

- Remove defunct reference to `prefetch` in Session `__attrs__` (#5110)

**Bugfixes**

- Requests no longer outputs password in basic auth usage warning. (#5099)

**Dependencies**

- Pinning for `chardet` and `idna` now uses major version instead of minor.
  This hopefully reduces the need for releases everytime a dependency is updated.
s remove
    requests.packages.urllib3 the import machinery will continue to let
    those same symbols work. Example usage in requests' documentation
    and 3rd-party libraries relying on the vendored copies of urllib3
    will work without having to fallback to the system urllib3.
-   Attempt to quote parts of the URL on redirect if unquoting and then
    quoting fails. (\#2356)
-   Fix filename type check for multipart form-data uploads. (\#2411)
-   Properly handle the case where a server issuing digest
    authentication challenges provides both auth and auth-int
    qop-values. (\#2408)
-   Fix a socket leak.
    ([shazow/urllib3\#549](https://github.com/shazow/urllib3/pull/549))
-   Fix multiple `Set-Cookie` headers properly.
    ([shazow/urllib3\#534](https://github.com/shazow/urllib3/pull/534))
-   Disable the built-in hostname verification.
    ([shazow/urllib3\#526](https://github.com/shazow/urllib3/pull/526))
-   Fix the behaviour of decoding an exhausted stream.
    ([shazow/urllib3\#535](https://github.com/shazow/urllib3/pull/535))

**Security**

-   Pulled in an updated `cacert.pem`.
-   Drop RC4 from the default cipher list.
    ([shazow/urllib3\#551](https://github.com/shazow/urllib3/pull/551))

2.5.1 (2014-12-23)
------------------

**Behavioural Changes**

-   Only catch HTTPErrors in raise\_for\_status (\#2382)

**Bugfixes**

-   Handle LocationParseError from urllib3 (\#2344)
-   Handle file-like object filenames that are not strings (\#2379)
-   Unbreak HTTPDigestAuth handler. Allow new nonces to be negotiated
    (\#2389)

2.5.0 (2014-12-01)
------------------

**Improvements**

-   Allow usage of urllib3's Retry object with HTTPAdapters (\#2216)
-   The `iter_lines` method on a response now accepts a delimiter with
    which to split the content (\#2295)

**Behavioural Changes**

-   Add deprecation warnings to functions in requests.utils that will be
    removed in 3.0 (\#2309)
-   Sessions used by the functional API are always closed (\#2326)
-   Restrict requests to HTTP/1.1 and HTTP/1.0 (stop accepting HTTP/0.9)
    (\#2323)

**Bugfixes**

-   Only parse the URL once (\#2353)
-   Allow Content-Length header to always be overridden (\#2332)
-   Properly handle files in HTTPDigestAuth (\#2333)
-   Cap redirect\_cache size to prevent memory abuse (\#2299)
-   Fix HTTPDigestAuth handling of redirects after authenticating
    successfully (\#2253)
-   Fix crash with custom method parameter to Session.request (\#2317)
-   Fix how Link headers are parsed using the regular expression library
    (\#2271)

**Documentation**

-   Add more references for interlinking (\#2348)
-   Update CSS for theme (\#2290)
-   Update width of buttons and sidebar (\#2289)
-   Replace references of Gittip with Gratipay (\#2282)
-   Add link to changelog in sidebar (\#2273)

2.4.3 (2014-10-06)
------------------

**Bugfixes**

-   Unicode URL improvements for Python 2.
-   Re-order JSON param for backwards compat.
-   Automatically defrag authentication schemes from host/pass URIs.
    ([\#2249](https://github.com/psf/requests/issues/2249))

2.4.2 (2014-10-05)
------------------

**Improvements**

-   FINALLY! Add json parameter for uploads!
    ([\#2258](https://github.com/psf/requests/pull/2258))
-   Support for bytestring URLs on Python 3.x
    ([\#2238](https://github.com/psf/requests/pull/2238))

**Bugfixes**

-   Avoid getting stuck in a loop
    ([\#2244](https://github.com/psf/requests/pull/2244))
-   Multiple calls to iter\* fail with unhelpful error.
    ([\#2240](https://github.com/psf/requests/issues/2240),
    [\#2241](https://github.com/psf/requests/issues/2241))

**Documentation**

-   Correct redirection introduction
    ([\#2245](https://github.com/psf/requests/pull/2245/))
-   Added example of how to send multiple files in one request.
    ([\#2227](https://github.com/psf/requests/pull/2227/))
-   Clarify how to pass a custom set of CAs
    ([\#2248](https://github.com/psf/requests/pull/2248/))

2.4.1 (2014-09-09)
------------------

-   Now has a "security" package extras set,
    `$ pip install requests[security]`
-   Requests will now use Certifi if it is available.
-   Capture and re-raise urllib3 ProtocolError
-   Bugfix for responses that attempt to redirect to themselves forever
    (wtf?).

2.4.0 (2014-08-29)
------------------

**Behavioral Changes**

-   `Connection: keep-alive` header is now sent automatically.

**Improvements**

-   Support for connect timeouts! Timeout now accepts a tuple (connect,
    read) which is used to set individual connect and read timeouts.
-   Allow copying of PreparedRequests without headers/cookies.
-   Updated bundled urllib3 version.
-   Refactored settings loading from environment -- new
    Session.merge\_environment\_settings.
-   Handle socket errors in iter\_content.

2.3.0 (2014-05-16)
------------------

**API Changes**

-   New `Response` property `is_redirect`, which is true when the
    library could have processed this response as a redirection (whether
    or not it actually did).
-   The `timeout` parameter now affects requests with both `stream=True`
    and `stream=False` equally.
-   The change in v2.0.0 to mandate explicit proxy schemes has been
    reverted. Proxy schemes now default to `http://`.
-   The `CaseInsensitiveDict` used for HTTP headers now behaves like a
    normal dictionary when references as string or viewed in the
    interpreter.

**Bugfixes**

-   No longer expose Authorization or Proxy-Authorization headers on
    redirect. Fix CVE-2014-1829 and CVE-2014-1830 respectively.
-   Authorization is re-evaluated each redirect.




