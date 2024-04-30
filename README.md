# Module 9 Reflection
> Soros Febriano | 2206083445

1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable? <br>

![image](https://github.com/sorfeb/tutorial-9-grpc/assets/112263712/0b2d3086-b64e-4191-8d9c-60c83d37c0f2)
*https://blog.knoldus.com/unary-streaming-via-grpc/*

- **Unary RPC:**
  - The client sends a single request to the server and will receive a single respnose back (like normal function call).
  - **Suitable Scenarios:**
    - Simple request-response communication.
    - When the client needs to send a small amount of data to the server and expects a single, relatively small response.
    - When the client's operation is not dependent on the server's response, as it blocks until the response is received.
    - Example: Fetching Weather Data

- **Server Streaming RPC:**
  - The client sends a single request to the server, and the server responds with a stream of messages.
  - **Suitable Scenarios:**
    - When the client needs to receive a large amount of data from the server but doesn't need to send any data back to the server.
    - When the server needs to push updates or a large dataset to the client in a sequential manner.
    - When the client needs to process data as it arrives rather than waiting for the entire response.
    - Example: Video Streaming Service

- **Bidirectional Streaming RPC:**
  - Both the client and server establish a persistent connection, allowing them to send a stream of messages to each other.
  - **Suitable Scenarios:**
    - Use when the client and server need to send a variable number of messages to each other in an asynchronous manner.
    - Ideal for scenarios where both the client and server need to send and receive data at any point during the communication.
    - When real-time interaction and updates are necessary, such as chat applications or collaborative editing.
    - Example: Online Multiplayer Game

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

  - **Authentication:**
    - Client Authentication: Ensure that clients are authenticated before allowing access to sensitive resources or services.
    - Options: Implement authentication mechanisms such as JWT (JSON Web Tokens), OAuth, or API keys.
  
  - **Authorization:**
    - Resource Access Control: Define and enforce access control policies to restrict client access to specific resources.
    - Options: Implement role-based access control (RBAC) or attribute-based access control (ABAC).
  
  - **Data Encryption:**
    - Transport Encryption: Encrypt data transmitted between client and server to prevent eavesdropping and tampering.
    - Options: Use TLS to encrypt data over the wire.

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

- Concurrency and Synchronization:
  - Managing concurrent streams and ensuring synchronization between multiple clients and the server will be a challenge because without proper synchronization, data races and race conditions can occur, leading to unpredictable behavior.

- Resource Management:
  - Efficiently managing resources, such as memory and CPU, when dealing with multiple concurrent streams will be a challenge because inefficient resource management can lead to excessive resource consumption and potential performance degradation.

- Connection Management:
  - Managing and maintaining bidirectional streaming connections between clients and servers will be a challenge because unmanaged connections can lead to resource leaks or stale connections, consuming system resources unnecessarily.

4. What are the advantages and disadvantages of using the `tokio_stream::wrappers::ReceiverStream` for streaming responses in Rust gRPC services?

- **Advantages:**
  - ReceiverStream integrates seamlessly with asynchronous programming in Rust, particularly with Tokio.
  - ReceiverStream is versatile and can handle various types of messages (e.g., structs, enums, strings) over the channel
  - ReceiverStream inherently supports backpressure handling, ensuring that the receiver does not get overwhelmed with messages.

- **Disadvantages:**
  - ReceiverStream is harder for scenarios where more control over resource management is required (e.g. Managing resources like memory and CPU).
    - Using ReceiverStream might introduce some performance overhead due to the asynchronous nature of Tokio. (e.g. additional context switching and scheduling overhead compared to synchronous streaming methods)
    - ReceiverStream might not be directly compatible with other streaming protocols or platforms outside of Rust's ecosystem.

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
  - Separation of Concerns: Define gRPC service interfaces separately from their implementations.
  - Modular Design: Organize Rust code into modules based on functionality.
  - Use of Traits and Generics: Define traits for gRPC service implementations, allowing multiple implementations.
  - Dependency Injection: Pass dependencies as arguments rather than hardcoding them.

6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?
  - Validate the request (checking whether the ID exists or the amount is > 0)
  - Connection with External Services (e.g. fraud detection services or payment gateways)
  - Handling errors
  - Interaction with the database
    
7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

- With gRPC, service contracts are defined using Protocol Buffers (.proto files), focusing on defining message types and service endpoints.
- API design becomes more streamlined and language-agnostic, promoting clear and concise service definitions.
- Protocol Buffers are supported by various programming languages, enabling interoperability across different technology stacks.
- gRPC generates strongly-typed client and server code from .proto files, providing type safety and consistency.
- Design Consideration: Developers work with generated code, reducing manual implementation effort and potential errors.
- Clients and servers written in different languages can communicate seamlessly, as long as they adhere to the same service contract.
- gRPC uses HTTP/2 for communication, allowing for multiplexed, bidirectional streaming, and header compression.
- Architectures can leverage these features for efficient data transfer and reduced latency.
- While HTTP/2 is widely supported, some legacy systems might not fully support it, potentially requiring gateway or proxy solutions for compatibility.
- With gRPC, microservices can be implemented in different languages, allowing teams to use the best tool for the job.

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

- **Advantages of HTTP/2**:
   -Multiplexing: allowing multiple requests and responses to be sent over a single connection simultaneously reduces latency and improves throughput, as it eliminates the need for multiple connections for parallel requests.
  - Binary Format: HTTP/2 uses a binary framing layer for communication, which is more efficient compared to the text-based format of HTTP/1.1, which reduces overhead and improves performance, especially for high-volume or low-latency applications.
  - Header Compression: HTTP/2 compresses header data, reducing the amount of data transmitted over the network, which results in lower bandwidth usage and faster transmission times, particularly for requests with large headers or cookies.

- **Disadvantages of HTTP/2**:
   -Complexity: HTTP/2's multiplexing and binary framing introduce complexity compared to HTTP/1.1, so implementing and debugging HTTP/2 servers and clients can be more challenging, especially for developers unfamiliar with its intricacies.
  - Interoperability:  Although widely supported, not all software or infrastructure fully supports HTTP/2, which results in issues that may arise when integrating with legacy systems or certain network configurations.
  - Resource Consumption: Multiplexing and header compression can increase resource consumption, especially for servers handling many concurrent connections, so servers may require more memory and processing power to handle HTTP/2 connections efficiently.

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
    
  **REST APIs** are suitable for real-time communication using techniques like long-polling or SSE, but may have limitations in terms of scalability and efficiency due to frequent polling and long-lived connections, while **gRPC** provides native support for bidirectional streaming, enabling highly responsive real-time communication with lower latency and higher efficiency, making it particularly well-suited for real-time applications requiring instant updates.

10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
    
gRPC with Protocol Buffers: Provides strong typing, efficiency, and schema evolution capabilities, suitable for performance-critical and evolving systems with well-defined contracts.
REST API with JSON: Offers flexibility, interoperability, and ease of use, ideal for rapidly changing requirements and environments where flexibility is prioritized over strict contract enforcement.
