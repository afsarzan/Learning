
* L4 (Transport Layer): Routes traffic based purely on IP addresses and TCP ports. It is blazingly fast because it doesn't look at the message content.
* L7 (Application Layer): Inspects the actual HTTP requests. This allows "smart" routing—for example, sending all requests for /images/* to a dedicated pool of static-asset servers, and /api/* to your heavy compute servers.
* 
