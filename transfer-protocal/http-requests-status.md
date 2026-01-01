# 📌 HTTP Status Codes Reference

HTTP status codes are **3-digit numbers** that tell the client how the request was handled.

---

## 1️⃣ 1xx – Informational
- **Meaning**: The request was received, and processing is continuing.
- **Examples**:
  - `100 Continue` → server is ready to receive the rest of the request.
  - `101 Switching Protocols` → usually for WebSockets.

👉 Rarely used in REST APIs.

---

## 2️⃣ 2xx – Success
- **Meaning**: The request was successfully received, understood, and processed.

Common ones you’ll use:
- `200 OK` → Request succeeded (GET, PUT, DELETE usually return this).
- `201 Created` → A new resource was created (POST usually returns this).
- `202 Accepted` → Request accepted but still processing (async jobs).
- `204 No Content` → Request succeeded but nothing to return (common for DELETE).

---

## 3️⃣ 3xx – Redirection
- **Meaning**: The client needs to do something else (follow a redirect).
- **Examples**:
  - `301 Moved Permanently` → resource moved to a new URL.
  - `302 Found` → temporary redirect.
  - `304 Not Modified` → client’s cached version is still valid.

👉 Mostly relevant in browsers, rarely in APIs.

---

## 4️⃣ 4xx – Client Errors
- **Meaning**: The client made a bad request (invalid input, unauthorized, etc.).

Common ones:
- `400 Bad Request` → malformed request (e.g., missing required field).
- `401 Unauthorized` → no/invalid authentication (token missing or expired).
- `403 Forbidden` → authenticated but not allowed (no permission).
- `404 Not Found` → resource doesn’t exist.
- `409 Conflict` → request conflicts with current state (e.g., duplicate entry).
- `422 Unprocessable Entity` → request understood, but validation failed (e.g., Sequelize validation errors).

---

## 5️⃣ 5xx – Server Errors
- **Meaning**: Something went wrong on the server.
- **Examples**:
  - `500 Internal Server Error` → generic error (bug, crash, etc.).
  - `502 Bad Gateway` → server acting as a proxy got an invalid response.
  - `503 Service Unavailable` → server is overloaded or down.
  - `504 Gateway Timeout` → server took too long to respond.

---

## 📌 Example with Your APIs

- **Create Feature (POST /feature/:departmentId)**
  - `201 Created` (if successful)
  - `422 Validation Error` (if name already exists)
  - `500 Internal Server Error` (if DB crashes)

- **Get Feature (GET /feature/:featureId)**
  - `200 OK` (if found)
  - `404 Not Found` (if no such feature)

- **Update Feature (PUT /feature/:featureId)**
  - `200 OK` (if updated)
  - `404 Not Found` (if feature doesn’t exist)

- **Delete Feature (DELETE /feature/:featureId)**
  - `200 OK` (if deleted)
  - `404 Not Found` (if feature doesn’t exist)
  - `500 Internal Server Error` (if DB fails)

---

## ⚡ In Short
- **2xx → success**  
- **4xx → client messed up**  
- **5xx → server messed up**


```js
const statusMessages = {
  //  Informational (1xx)
  100: "Continue → The server received the request headers, continue sending the body.",
  101: "Switching Protocols → The server is switching to another protocol (e.g., WebSocket).",
  102: "Processing → The request is being processed, but not finished yet (WebDAV).",
  103: "Early Hints → Server sends headers early (e.g., for preloading resources).",

  //  Success (2xx)
  200: "OK → The request succeeded and the server returned the requested data.",
  201: "Created → A new resource was successfully created.",
  202: "Accepted → The request was accepted, but processing happens later (async).",
  203: "Non-Authoritative Information → Response is from a cached/modified source, not the original server.",
  204: "No Content → Request succeeded but no response body is returned.",
  205: "Reset Content → Request succeeded, client should reset form/view state.",
  206: "Partial Content → Only part of the resource was returned (due to range requests).",

  // Redirection (3xx)
  300: "Multiple Choices → Multiple options available, client must choose one.",
  301: "Moved Permanently → Resource has a new permanent URL.",
  302: "Found → Temporary redirect to another URL.",
  303: "See Other → Redirect to another resource using GET.",
  304: "Not Modified → Resource not changed, use cached version.",
  307: "Temporary Redirect → Resource temporarily moved, method must not change.",
  308: "Permanent Redirect → Resource permanently moved, method must not change.",

  // Client Errors (4xx)
  400: "Bad Request → The server could not understand the request due to invalid syntax or missing parameters.",
  401: "Unauthorized → The client must authenticate itself to get the requested response. Usually missing or invalid token.",
  402: "Payment Required → Reserved for future use, often related to digital payment systems.",
  404: "Not Found → The requested resource could not be found on the server.",
  405: "Method Not Allowed → HTTP method not supported for this resource.",
  406: "Not Acceptable → Server can’t produce content matching the client’s Accept headers.",
  407: "Proxy Authentication Required → Client must authenticate with a proxy first.",
  408: "Request Timeout → Client took too long to send the request.",
  409: "Conflict → Request conflicts with the current state of the resource.",
  410: "Gone → Resource no longer available, no forwarding address.",
  411: "Length Required → Content-Length header is missing but required.",
  412: "Precondition Failed → Preconditions in request headers weren’t met.",
  413: "Payload Too Large → Request body too big for the server to handle.",
  414: "URI Too Long → Request URL is too long for the server.",
  415: "Unsupported Media Type → Request format is not supported by the server.",
  416: "Range Not Satisfiable → Requested range of the resource is invalid.",
  417: "Expectation Failed → Expectation in request headers couldn’t be met.",
  418: "I'm a teapot ☕ → Joke code (RFC 2324), server refuses to brew coffee.",
  422: "Unprocessable Entity → The server understands the content type and syntax, but the data is semantically invalid (e.g., validation error).",
  425: "Too Early → Request might be replayed, server refuses to process now.",
  426: "Upgrade Required → Client must switch to a different protocol (e.g., TLS).",
  428: "Precondition Required → Server requires conditional requests for safety.",
  429: "Too Many Requests → Client sent too many requests (rate limiting).",
  431: "Request Header Fields Too Large → Headers are too big for the server.",
  451: "Unavailable For Legal Reasons → Resource blocked due to legal demands.",

  // Server Errors (5xx)
  500: "Internal Server Error → A generic error happened on the server.",
  501: "Not Implemented → Server doesn’t support this request method/feature.",
  502: "Bad Gateway → Server got an invalid response from another upstream server.",
  503: "Service Unavailable → Server is temporarily down or overloaded.",
  504: "Gateway Timeout → Upstream server didn’t respond in time.",
  505: "HTTP Version Not Supported → Server doesn’t support the HTTP version used.",
  506: "Variant Also Negotiates → Server misconfigured with circular negotiation.",
  507: "Insufficient Storage → Server doesn’t have enough storage to complete request.",
  508: "Loop Detected → Infinite loop detected while processing request.",
  510: "Not Extended → Further extensions to the request are required.",
  511: "Network Authentication Required → Client must authenticate to use the network."
};