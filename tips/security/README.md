# Security

## Cross Site Scripting (XSS)

XSS vulnerabilities target scripts embedded in a page that are executed on the client-side (in the user’s web browser) rather than on the server-side

## Denial of Service (DoS/DDoS)

A denial-of-service attack is when an attacker sends an enormous amount of traffic to a website in an attempt to overwhelm the hosting server to disrupt and even paralyze service.

## Cross-site request forgery (CSRF)

Cross-site request forgery attacks (CSRF or XSRF for short) are used to send malicious requests from an authenticated user to a web application.

When you are browsing a website, it is common for that website to request data from another website on your behalf. For example, in most cases, a video that is shown on a website is not typically stored on the website itself. The video appears to be on the website but is actually being embedded from a video streaming site such as YouTube. That’s the idea behind Content Delivery Networks (CDNs), which are used to deliver content faster. Many websites store scripts, images, and other bandwidth-hungry resources on CDNs, so during browsing, images and script files are downloaded from a CDN source rather than the website itself.

While this improves the browsing experience, it might also be a source of a security problem if a website asks the web browser to retrieve data from another website without the user’s consent. If such requests are not handled correctly, an attacker can launch a cross-site request forgery attack

SameSite: If the session cookie is marked as a SameSite cookie, it is only sent along with requests that originate from the same domain. Therefore, when https://example.com/index.php wants to make a POST request to https://example.com/post_comment.php, it is allowed. However, https://attacker.com/ can’t send POST requests to https://example.com/post_comment.php, since the session cookie originates from a different domain, so it is not sent along with the request.

## Same Origin Policy

The same-origin policy is a critical security mechanism that restricts how a document or script loaded from one origin can interact with a resource from another origin. It helps isolate potentially malicious documents, reducing possible attack vectors.

## Cross-Origin Resource Sharing (CORS)

https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS

Cross-Origin Resource Sharing (CORS) is an HTTP-header based mechanism that allows a server to indicate any other origins (domain, protocol, or port) than its own from which a browser should permit loading of resources.
