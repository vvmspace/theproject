## Message Broker + WebSockets

Streaming support is critical for LLM responses to provide a smooth user experience (token-by-token display).

### Recommended Stack for Streaming

To stream LLM responses effectively, you need a combination of a Message Broker and WebSockets (or SSE):

**Message Brokers (Pub/Sub & Streaming):**
- **NATS JetStream:** Extremely fast, lightweight, and supports both pub/sub and persistent streams.
- **Apache Kafka:** The industry standard for high-throughput, distributed event streaming.
- **Redis (Streams/PubSub):** Excellent for low-latency notifications and simple streaming use cases.
- **RabbitMQ:** Reliable messaging with support for various protocols (MQTT, WebStomp).

**API Gateways (Streaming Support):**
- **FastAPI (Python):** Uses `StreamingResponse` for Server-Sent Events (SSE). It performs best with asynchronous generators, allowing the event loop to handle other requests while waiting for the next LLM token. Supports native WebSockets via the `WebSocket` class.
- **NestJS (Node.js):** Provides a built-in `@Sse()` decorator that works with RxJS `Observable` streams. For bi-directional streaming, WebSocket Gateways (using `ws` or `socket.io`) are highly recommended.
- **Axum (Rust):** Leverages Rust's `Stream` trait and zero-cost abstractions. Streaming is handled via `axum::response::sse::Sse`, providing industry-leading performance and safety for high-concurrency LLM workloads.
- **Spring Boot (Java/Kotlin):** Requires `Spring WebFlux` for true non-blocking streaming. Uses `Flux<ServerSentEvent>` for SSE and `WebSocketHandler` for reactive WebSocket streams.

## Core Concepts

### Why WebSockets for LLMs?
LLMs generate responses token-by-token. Standard HTTP requests (Request-Response) wait for the entire response to be ready. WebSockets establish a persistent, full-duplex connection that allows the server to push tokens to the client as they are generated.

### Role of the Message Broker
In a modular architecture, the inference engine (LLM service) may be separate from the API Gateway. The Message Broker acts as a high-speed buffer, allowing the inference engine to publish tokens which the Gateway then subscribes to and forwards to the specific user's WebSocket.

### Proxy Configuration
The proxy (like Nginx) is responsible for maintaining long-lived connections and ensuring that WebSocket traffic is correctly routed to the appropriate backend service while handling authentication and encryption (TLS).

```bash
# Example Proxy Environment
WEBSOCKET_HOST=%HOST%
WEBSOCKET_PORT=%PORT%
```
**Endpoint Interface:**
`ws://%WEBSOCKET_HOST%:%WEBSOCKET_PORT%/api/ws` - Proxies message broker events to the frontend via WebSockets.

### Security

Important: User should be able to receive events and streams only for himself.

