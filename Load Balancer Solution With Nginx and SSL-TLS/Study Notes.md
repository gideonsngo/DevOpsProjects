### NGINX for Load Balancing

NGINX is a high-performance web server and reverse proxy server that can be used to load balance traffic across multiple servers. Load balancing helps distribute incoming network traffic evenly across multiple backend servers to ensure no single server becomes overwhelmed, improving overall application performance and reliability.

#### Types of Load Balancing in NGINX

1. **Round Robin:**
   - Default method.
   - Distributes requests evenly across all servers.

2. **Least Connections:**
   - Directs traffic to the server with the fewest active connections.

3. **IP Hash:**
   - Directs requests from the same client IP to the same server.

4. **Weighting:**
   - Assigns different weights to servers, allowing more powerful servers to handle more requests.

### SSL/TLS

#### SSL/TLS Overview

SSL (Secure Sockets Layer) and TLS (Transport Layer Security) are cryptographic protocols designed to provide secure communication over a computer network. While SSL is the older protocol, it has been deprecated in favor of TLS.

#### Key Concepts

1. **Encryption:**
   - Ensures that the data sent between the client and server is unreadable to anyone who might intercept it.

2. **Authentication:**
   - Verifies that the parties exchanging information are who they claim to be. This is typically done using digital certificates.

3. **Integrity:**
   - Ensures that the data has not been altered in transit.

#### How SSL/TLS Works

1. **Handshake Process:**
   - The client and server exchange messages to establish a secure connection.
   - The client requests a secure connection and the server responds with its digital certificate.
   - The client verifies the server's certificate and sends back a pre-master secret, encrypted with the server’s public key.
   - The server decrypts the pre-master secret using its private key.
   - Both parties generate session keys from the pre-master secret, which are used to encrypt and decrypt the data during the session.

2. **Encryption Algorithms:**
   - SSL/TLS uses symmetric encryption for data exchange and asymmetric encryption for key exchange.

### Summary

**NGINX for Load Balancing:**
- Distributes traffic across multiple servers.
- Supports methods like Round Robin, Least Connections, and IP Hash.
- Improves performance and reliability.

**SSL/TLS:**
- Provides secure communication over networks.
- Uses encryption, authentication, and integrity checks.
- SSL is deprecated; TLS is the modern, secure protocol.
- Requires a digital certificate and private key for setup.
