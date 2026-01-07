# Client-Server Architecture - System Design

**Source Summary:** [GeeksforGeeks: Client-Server Architecture - System Design](https://www.geeksforgeeks.org/system-design/client-server-architecture-system-design/)

## Overview
**Client–Server Architecture** is a distributed system model that partitions tasks or workloads between the providers of a resource or service, called **servers**, and service requesters, called **clients**.
This architecture is fundamental to modern networking, enabling efficient, scalable, and organized distributed systems like the World Wide Web.

### Core Concept
*   **Clients**: Initiate specific requests and wait for responses (e.g., Web Browsers, Mobile Apps). They handle user interaction and presentation.
*   **Servers**: Listen for requests, process them, and send back responses. They handle data storage, heavy computation, and resource management.
*   **Communication**: Occurs over a network using standardized protocols (like HTTP, TCP/IP).

## Key Components
1.  **Client**: The interface for the user. It sends requests to the server and displays the results.
    > *Examples: Web browsers, mobile apps, desktop applications.*
2.  **Server**: The backend powerhouse. It listens for requests, validates them, performs logic/computations, interacts with databases, and returns responses.
3.  **Network**: The communication medium (Internet, LAN) enabling data exchange.
4.  **Protocols**: Standardized rules defining how data is transmitted and formatted.
    > *Examples: HTTP/HTTPS (Web), TCP/IP (Transport), SQL (Database).*
5.  **Middleware**: Intermediary software bridging clients and servers, handling cross-cutting concerns.
    > *Examples: Authentication servers, logging systems, message queues.*
6.  **Database**: Organized storage for persistent data, managed by the server to ensure consistency and integrity.
7.  **User Interface (UI)**: The visual part of the client application users interact with.
8.  **Application Logic**: The business rules and algorithms processing data flow between client and server.

## Design Principles
A robust client-server system relies on:

*   **Modularity**: Breaking the system into independent components (Client, Server, DB). This ensures **Separation of Concerns**, making the system easier to develop, test, and maintain.
*   **Scalability**: The ability to handle growth.
    *   **Horizontal Scalability**: Adding *more* servers (scaling out) to distribute load.
    *   **Vertical Scalability**: Adding *more power* (CPU, RAM) to existing servers (scaling up).
*   **Reliability & Availability**:
    *   **Redundancy**: eliminating single points of failure (e.g., multiple server instances).
    *   **Load Balancing**: Distributing incoming traffic efficiently across servers.
    *   **Fault Tolerance**: Mechanisms to handle failures gracefully.
*   **Performance Optimization**:
    *   **Caching**: Storing frequently visited data closer to the client (CDN, Browser Cache, Server Cache) to reduce latency.
    *   **Efficient Communication**: Optimizing protocol usage (e.g., HTTP/2, WebSocket).
*   **Security**:
    *   **Authentication/Authorization**: Verifying identity and permissions.
    *   **Encryption**: Using SSL/TLS for secure data transmission.
    *   **Audits**: Regular security checks.
*   **Maintainability**: Writing clean, documented code and using Version Control (Git).
*   **Interoperability**: Using standard protocols (REST, SOAP) so different platforms can work together easily.

## Communication Patterns
*   **Request-Response**: Synchronous. Client sends request, waits for server response. (Standard Web).
*   **Publish-Subscribe (Pub/Sub)**: Event-driven. Clients subscribe to topics; Server pushes updates when events occur. (Real-time notifications).
*   **Remote Procedure Call (RPC)**: Client executes a function on the server as if it were local code.

## Design Flows (Step-by-Step)

### 1. Client-Side Design (Client-Side Rendering Focus)
This flow emphasizes the browser's role in rendering.
1.  **User Request**: User enters URL.
2.  **CDN Delivery**: CDN serves static HTML (often an empty shell) and links to JS bundles.
3.  **Download**: Browser downloads HTML and then heavy JavaScript files. *Site is not yet visible/interactive.*
4.  **Execution & Fetch**: Browser executes JS. The JS code makes API calls to the server for data. *Placeholders/Loaders might be shown.*
5.  **Render**: Server returns JSON data. JS populates the UI. *Page becomes fully interactive.*

### 2. Server-Side Design (Server-Side Rendering Focus)
This flow emphasizes the server's role in delivering content.
1.  **User Request**: User enters URL.
2.  **Processing**: Server processes the request, fetches data, and generates a full HTML page.
3.  **Fast Render**: Browser receives "Ready to Render" HTML. *Content is visible immediately.*
4.  **Hydration**: Browser downloads and executes JS frameworks (like React/Angular).
5.  **Interactive**: The static HTML "hydrates" into a dynamic app. Recorded user interactions are replayed.

## Frameworks and Tools
*   **Server-Side**: Node.js, Express.js, Django (Python), Spring Boot (Java), Ruby on Rails.
*   **Client-Side**: React, Angular, Vue.js, Svelte, Bootstrap (CSS).
*   **Databases**: MySQL, PostgreSQL (Relational); MongoDB (NoSQL); SQLite (Embedded).
*   **Protocols/APIs**: REST (Stateless), GraphQL (Flexible queries), WebSocket (Real-time).
*   **Dev Tools**: Postman (API Testing), Docker (Containerization), Kubernetes (Orchestration), Git (Version Control).

## Real-World Examples
*   **Banking Systems**: Secure web/mobile clients connecting to core banking servers for transactions.
*   **Enterprise Apps (ERP/CRM)**: Employees (clients) accessing centralized business data.
*   **Telecommunications**: VoIP and messaging apps using servers for signaling and routing.
*   **IoT**: Smart devices (sensors) sending data to cloud servers for analysis.
*   **Healthcare**: EHR systems managing patient records securely.

---
**Reference:**
[Client-Server Architecture - System Design - GeeksforGeeks](https://www.geeksforgeeks.org/system-design/client-server-architecture-system-design/)
