# MELD/CORS issues

## Notes from 2021-09-15 teleconference DL/MS/GK

MELD web site access discussion with DL/MS

https://d-nb.info/gnd/1124865284

MELD app is https://github.com/oerc-music/StarBrightlyShining

Can access Wikidata OK, but GND/GeoNames give access error - looks like CORS-related issue.

GK ACTIONS (subject to time):

1. [ ] Characterize problem; mention application (MELD/BiTH) and collaborators; explain why it is safe
2. [ ] Investigate issue trackers for GND/GeoNames
    1. [x] No joy d-nb.info
3. [ ] Get app running locally (https://github.com/oerc-music/StarBrightlyShining)
4. [ ] Read up CORS specs
5. [ ] Map design for proxy in MELD service

…

https://d-nb.info/gnd/1124865284

MS to Everyone (12:10)
Access to fetch at 'https://d-nb.info/gnd/1072942461/about/lds.jsonld' from origin 'http://localhost:8080' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.
VM146:1 GET https://d-nb.info/gnd/1072942461/about/lds.jsonld net::ERR_FAILED

DL to Everyone (12:12)
curl   --http1.1 -H "Origin: http://www.example.com" -D - -H "accept: application/rdf+xml" https://d-nb.info/gnd/1124865284/about/lds.rdf
curl -D - -H "accept: application/json" https://d-nb.info/gnd/1124865284
curl -D - -H "accept: application/rdf+xml" https://d-nb.info/gnd/1124865284
David Lewis to Everyone (12:18)
curl   -H "Access-Control-Request-Method: POST" -H "Access-Control-Request-Headers: X-Requested-With" -X OPTIONS --verbose -D - http://localhost:8080



## Characterization: feature request

### Please support CORS protocol

Much information presented by d-nb.info is publicly accessible, and is easily accessed directly using a browser of other tools.  But when running a browser application (Javascript) which has been loaded from a location other than d-nb.info, the script is unable to access these same resources and presents an error condition like this:

> Access to fetch at 'https://d-nb.info/gnd/1072942461/about/lds.jsonld' from origin 'http://localhost:8080' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.

NOTE: it is conceivable that this could be fixed in MELD if a `fetch` operation is issued with a `request.mode` value of `no-cors`: see https://developer.mozilla.org/en-US/docs/Web/API/Request/mode.  This might need a change to be applied in the Solid access code.


The [CORS protocol](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) is enforced by modern browsers, and is designed to prevent leakage of secure information by Javascript applications running in the browser.  But the browser does not have _a priori_ knowledge of what information is considered to be sensitive, so its default behaviours are somewhat conservative.  Apart from very [simple requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#simple_requests) that do not risk leaking sensitive information, the browser depends on the server indicating what source pages can be permitted to access the response from any response.  Without such information, the browser must assume that allowing access to the response could be a security violation, and blocks the requesting application from seeing it.

A simple fix on the server side would be to include

    Access-Control-Allow-Origin: *

in the response to any simple request for public information.  This can be handled by a front-end web server (e.g. [Apache](https://enable-cors.org/server_apache.html), [nginx](https://enable-cors.org/server_nginx.html)).

But this will work only when the request sent does not include credentials (and hence may be subject to access control on the server).   In this case, the server would need to send additional headers with any success response:

    Access-Control-Allow-Origin: <specific-origin>
    Vary: Origin
    Access-Control-Allow-Credentials: true

where `<specific-origin>` is that indicated by an `Origin:` header in the corresponding request.










