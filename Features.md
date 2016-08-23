### Application Features

* [Routing](Routing): attaches code to specific path/query/method/header and extract parameters from placeholders
* Sessions: stores and retrieves additional information attached to client session
* Content transformations: transforms response content on the fly and utilise unified mechanism to build a response
* Authentication: authenticates client using Basic, Digest, Form, OAuth (1a & 2)
* Custom status pages: sends custom content for specific status responses such as 404 Not Found
* Content type mapping: maps file extension to mime type for static file serving
* Template engines: uses content transformation to enable transparent template engine usage
* Static content: serves streams of data from local file system with full asynchronous support

### HTTP features

* Compression: enables gzip/deflate compression when client accepts it
* Conditional Headers: sends 304 Not Modified response when if-modified-since/etag indicate content is the same
* Partial Content: sends partial content for streaming ranges, like in video streams
* Automatic HEAD response: responds to HEAD requests by running through pipeline and dropping response body
* CORS: verifies and sends headers according to cross-origin resource sharing control
* HSTS and https redirect: supports strict transport security
