# Design System

## Popular problems with scaling micro FE apps

### Drifting infrastruture

With time, our frontend infrastructure began to drift, leading to maintainability and code complexity issues.

### Keeping the entire service fleet up to date is hard

This led to services falling behind on security and performance updates.

### Proliferation (and divergence) of infrastructure code

Each service implemented frontend infrastructure (like Redux, or server-side rendering) in its own special way according to its own needs and team preferences, leading to heterogeneous implementation of common app patterns.

### Performance bottlenecks

As new technologies like dynamic imports and other bundle size optimizations became available, frontend services that had not been upgraded to our latest platform began to lose out on performance wins offered by newer platform updates.

### Common tasks are hard to apply at scale

Tasks that would normally be simple were difficult to apply at scale. For example, if we wanted to introduce styled-components to our service bundles, we would need to manually go into each service and add it in its own special way for each service’s implementation approach.

### Lack of standardization

Sharing code is very difficult due to our heterogeneous codebases. Our engineers must often reinvent the wheel when implementing patterns and modules instead of leveraging shared code and libraries.

## DNS (Domain Name system) - Ex: Route 53

![DNS](https://github.com/donnemartin/system-design-primer/blob/master/images/IOyLj4i.jpg)

DNS is hierarchical, with a few authoritative servers at the top level. Your router or ISP provides information about which DNS server(s) to contact when doing a lookup. Lower level DNS servers cache mappings, which could become stale due to DNS propagation delays. DNS results can also be cached by your browser or OS for a certain period of time, determined by the [time to live (TTL)](https://en.wikipedia.org/wiki/Time_to_live).

- NS record (name server) - Specifies the DNS servers for your domain/subdomain.
- MX record (mail exchange) - Specifies the mail servers for accepting messages.
- A record (address) - Points a name to an IP address.
- CNAME (canonical) - Points a name to another name or CNAME (example.com to www.example.com) or to an A record.

Services such as CloudFlare and Route 53 provide managed DNS services. Some DNS services can route traffic through various methods:

- Weighted round robin:
  - Prevent traffic from going to servers under maintenance
  - Balance between varying cluster sizes
  - A/B testing
- Latency-based
- Geolocation-based

## CDN (Content Delivery Network) - Ex: CloudFront

![CDN](https://github.com/donnemartin/system-design-primer/raw/master/images/h9TAuGI.jpg)

A content delivery network (CDN) is a globally distributed network of proxy servers, serving content from locations closer to the user. Generally, static files such as HTML/CSS/JS, photos, and videos are served from CDN, although some CDNs such as Amazon's CloudFront support dynamic content. The site's DNS resolution will tell clients which server to contact.

Serving content from CDNs can significantly improve performance in two ways:

- Users receive content from data centers close to them
- Your servers do not have to serve requests that the CDN fulfills

## Object Storage Service - Ex: S3

## Web Platform - Ex: AWS Amplify, Netlify, Vercel etc.

## Communication: HTTP, REST

## Architecture Components

### Load balancer

### Reverse Proxy

### Caching - cache invalidation

## Architecture Concepts

### Performance

If you have a performance problem, your system is slow for a single user.

### Scalability

If you have a scalability problem, your system is fast for a single user but slow under heavy load.

A service is scalable if it results in increased performance in a manner proportional to resources added. Generally, increasing performance means serving more units of work, but it can also be to handle larger units of work, such as when datasets grow.

### Latency

is the time to perform some action or to produce some result.

### Throughput

is the number of such actions or results per unit of time.

### Availability (CAP Theorem, Fail-over/Replication)

Every request receives a response, without guarantee that it contains the most recent version of the information

### Consitency (Weak, Eventual, Strong)

Every read receives the most recent write or an error

## CI/CD (Continuous Integration/Continuous Delivery)

## Security

### HTTPS

HTTPS (Hypertext Transfer Protocol Secure) is a secure version of the HTTP protocol that uses the SSL/TLS protocol for encryption and authentication. (Port 443)

### Authentication - Key, Ex: OTP(One time password), SSO(single sign on), MFA(Mulit factor authentication)

Authentication is the act of validating that users are whom they claim to be. This is the first step in any security process.

#### Popular authentication techniques

- Password-based authentication is a simple method of authentication that requires a password to verify the user’s identity.
- Passwordless authentication is where a user is verified through `OTP` or a magic link delivered to the registered email or phone number.
- `2FA/MFA` requires more than one security level, like an additional PIN or security question, to identify a user and grant access to a system.
- Single sign-on `SSO` allows users to access multiple applications with a single set of credentials.
- Social authentication verifies and authenticates users with existing credentials from social networking platforms.

#### Popular authorization techniques

- Role-based access controls `RBAC` can be implemented for system-to-system and user-to-system privilege management.
- JSON web token `JWT` is an open standard for securely transmitting data between parties, and users are authorized using a public/private key pair.
- `SAML` is a standard Single Sign-On format (SSO) where authentication information is exchanged through XML documents that are digitally signed.
- `OpenID` authorization verifies user identity based on an authorization server’s authentication.
- `OAuth` allows the API to authenticate and access the requested system or resource.

### Authorization - Permissions

Authorization in a system security is the process of giving the user permission to access a specific resource or function. This term is often used interchangeably with access control or client privilege.

### Security Vulnerabilties

#### Cross Site Scripting (XSS)

XSS vulnerabilities target scripts embedded in a page that are executed on the client-side (in the user’s web browser) rather than on the server-side

#### Denial of Service (DoS/DDoS)

A denial-of-service attack is when an attacker sends an enormous amount of traffic to a website in an attempt to overwhelm the hosting server to disrupt and even paralyze service.

#### Cross-site request forgery (CSRF)

Cross-site request forgery attacks (CSRF or XSRF for short) are used to send malicious requests from an authenticated user to a web application.

When you are browsing a website, it is common for that website to request data from another website on your behalf. For example, in most cases, a video that is shown on a website is not typically stored on the website itself. The video appears to be on the website but is actually being embedded from a video streaming site such as YouTube. That’s the idea behind Content Delivery Networks (CDNs), which are used to deliver content faster. Many websites store scripts, images, and other bandwidth-hungry resources on CDNs, so during browsing, images and script files are downloaded from a CDN source rather than the website itself.

While this improves the browsing experience, it might also be a source of a security problem if a website asks the web browser to retrieve data from another website without the user’s consent. If such requests are not handled correctly, an attacker can launch a cross-site request forgery attack

SameSite: If the session cookie is marked as a SameSite cookie, it is only sent along with requests that originate from the same domain. Therefore, when https://example.com/index.php wants to make a POST request to https://example.com/post_comment.php, it is allowed. However, https://attacker.com/ can’t send POST requests to https://example.com/post_comment.php, since the session cookie originates from a different domain, so it is not sent along with the request.

#### Same Origin Policy

The same-origin policy is a critical security mechanism that restricts how a document or script loaded from one origin can interact with a resource from another origin. It helps isolate potentially malicious documents, reducing possible attack vectors.

#### Cross-Origin Resource Sharing (CORS)

https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS

Cross-Origin Resource Sharing (CORS) is an HTTP-header based mechanism that allows a server to indicate any other origins (domain, protocol, or port) than its own from which a browser should permit loading of resources.

### SECURITY HEADERS & CONFIGURATIONS

- [ ] `Add` [CSP](https://en.wikipedia.org/wiki/Content_Security_Policy) header to mitigate XSS and data injection attacks. This is important.
- [ ] `Add` [CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery) header to prevent cross site request forgery. Also add [SameSite](https://tools.ietf.org/html/draft-ietf-httpbis-cookie-same-site-00) attributes on cookies.
- [ ] `Add` [HSTS](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security) header to prevent SSL stripping attack.
- [ ] `Add` your domain to the [HSTS Preload List](https://hstspreload.org/)
- [ ] `Add` [X-Frame-Options](https://en.wikipedia.org/wiki/Clickjacking#X-Frame-Options) to protect against Clickjacking.
- [ ] `Add` [X-XSS-Protection](https://www.owasp.org/index.php/OWASP_Secure_Headers_Project#X-XSS-Protection) header to mitigate XSS attacks.
- [ ] Update DNS records to add [SPF](https://en.wikipedia.org/wiki/Sender_Policy_Framework) record to mitigate spam and phishing attacks.
- [ ] Add [subresource integrity checks](https://en.wikipedia.org/wiki/Subresource_Integrity) if loading your JavaScript libraries from a third party CDN. For extra security, add the [require-sri-for](https://w3c.github.io/webappsec-subresource-integrity/#parse-require-sri-for) CSP-directive so you don't load resources that don't have an SRI sat.
- [ ] Use random CSRF tokens and expose business logic APIs as HTTP POST requests. Do not expose CSRF tokens over HTTP for example in an initial request upgrade phase.
- [ ] Do not use critical data or tokens in GET request parameters. Exposure of server logs or a machine/stack processing them would expose user data in turn.
