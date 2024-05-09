# Study Notes for 101

## SDLC
SDLC or Software Development Life Cycle is a systematic process which provides a framework for developers to produce software that meets users expectations and projects requirements saving. It covers the process from planning, creating, testing, deploying and maintaining software. 
The main phases of the SDLC include:
  - **Requirements Gathering**: Developers and project managers meet with stakeholders (could be clients or potential users) to understand what they need from the software.
  - **Design**: Once everyone agrees on what the software should do, the next step is designing how it will work. Decisions made here affect how well the software works and how easy it is to use.
  - **Implementation (coding)**: This phase is where the developers start coding in the chosen programming language, following the plans laid out in the design phase.
  - **Testing**: In this phase, the software is put through various tests to look for anything that doesn’t work as intended, ensuring that the software meets the quality standards set out at the beginning.
  - **Deployment**: Deployment is when the software is finally released to the users. It could be launched all at once or in stages, depending on the project plan.
  - **Maintenance**: This phase involves making sure the software continues to work well over time, fixing any issues that pop up, and possibly updating it with new features or improvements based on user feedback.

## Technology Stack
Technology Stack is a set of frameworks and tools used to develop a software product. The technology stacks to be considered are listed below:
 - **LAMP** (Linux, Apache, MySQL, PHP)
 - **LEMP** (Linux, Nginx, MySQL, PHP)
 - **MERN** (MongoDB, ExpressJS, ReactJS, NodeJS)
 - **MEAN** (MongoDB, ExpressJS, AngularJS, NodeJS)
### LAMP Stack
**LAMP** is an acronym for Linux, Apache, MySQL, PHP. It is used for building and deploying web applications. Linux is the Operation System (OS) of the Host Machine, Apache is the Web Server which runs on the OS, MySQL is the database for storing application data and PHP is the programming language used in coding the application.

## Using the `Chmod` and `Chown` Linux Commands

 - **`Chmod`** : This Command is used to change permissions of a file/directory. 
The Permission String looks like this: `-rwxr-xr-x` 1st Section contains 1 Character which represents the type of file (- file, d for directory/folder). 2nd Section contains 3 characters which represent the user that owns the file (r-read, w-write, x-executable), 3rd section contains 3 characters which represents the group and the 4th contains 3 characters represents world or other permissions. An example of how this command is used: `chmod g+w ansible_bootstrap.sh` adds the w permission to the group permissions. Permissions can also be changed using numbers where `r=4 w=2 x=1` for example `chmod 644 ansible_bootstrap.sh`.
 - **`Chown`** : This Command is used to change the ownership or group of a file/directory. It can be used by initiating the `chown` command in the following syntax: `chown [options] new_owner [:new_group] file(s)`. The components are broken down below:
 - `chown`: The base command options: Optional flags that modify the behavior of the chown command 
 - `new_owner [:new_group]` : The new owner and optionally the new hroup. If new_group is omitted, only owner is changed 
 - `file(s)`: The file or files for which ownership is to be changed.

## TCP vs UDP

TCP (Transmission Control Protocol) and UDP (User Datagram Protocol) are two common transport layer protocols used in computer networks, but they differ in several key aspects:
1. **Connection-Oriented vs. Connectionless**: 
   - TCP is connection-oriented, meaning it establishes a connection between the sender and receiver before transmitting data. This ensures that data arrives in the correct order and without errors.
   - UDP is connectionless, meaning it does not establish a connection before sending data. Each datagram (unit of data) is transmitted independently, which can lead to faster transmission but offers no guarantee of delivery or order.
2. **Reliability**:
   - TCP provides reliable communication by using mechanisms such as acknowledgment, retransmission, and flow control to ensure that data arrives intact and in the correct order. It's suitable for applications where data integrity is critical, such as web browsing, email, and file transfer.
   - UDP does not provide reliability mechanisms like TCP. It's often used in applications where real-time communication is more important than guaranteed delivery, such as streaming media, online gaming, and VoIP (Voice over Internet Protocol).
3. **Header Overhead**:
   - TCP has a larger header size compared to UDP due to its additional control information for connection management, flow control, and error recovery. This can result in slightly higher overhead.
   - UDP has a smaller header size, making it more efficient for transmitting small amounts of data or for applications that require minimal overhead.
4. **Order of Delivery**:
   - TCP guarantees the order of delivery, meaning data sent from the sender will arrive at the receiver in the same order.
   - UDP does not guarantee the order of delivery. While datagrams may be sent in a specific order, they may arrive out of order at the receiver.

In summary, TCP provides reliable, ordered, and connection-oriented communication, making it suitable for applications that prioritize data integrity and correctness. UDP, on the other hand, offers faster, connectionless, and unreliable communication, making it ideal for real-time applications where speed and low latency are more important than guaranteed delivery.

## Common Ports
 - **HTTP (Hypertext Transfer Protocol)**: 80
 - **HTTPS (Hypertext Transfer Protocol Secure)**: 443
 - **SSH (Secure Shell)**: 22
 - **Telnet**: 23
 - **FTP (File Transfer Protocol)**: 21
 - **SFTP (SSH File Transfer Protocol)**: SFTP doesn't have its own port number; it runs over SSH on port 22 (the same port used for SSH).
