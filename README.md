# 🌐 ft_irc - Internet Relay Chat Server

**ft_irc** is a comprehensive Group Project from the **42 School** curriculum. The goal is to implement a fully functional IRC server from scratch, strictly adhering to the **RFC 1459** and **RFC 2812** protocols.

Unlike modern web servers, this project requires manual management of TCP sockets, buffers, and client states using **Non-Blocking I/O** and **I/O Multiplexing**.

---

## 📑 Features

### 🚀 Core Functionalities
* **Multi-Client Support:** Handles multiple simultaneous connections without hanging.
* **Authentication:** Password verification mechanism for client connection.
* **Channels:** Support for creating, joining, and listing channels.
* **Privmsg:** Real-time private messaging between users and broadcasting to channels.
* **Operators:** Special privileges for channel operators (Kick, Invite, Topic, Mode).

### 🤖 Bonus Features (Implemented)
* **File Transfer:** (If implemented in your version, otherwise remove this line).
* **IRC Bot:** An automated bot responding to custom commands.
* **Signal Handling:** Graceful shutdown on `SIGINT` / `SIGQUIT`.

---

## ⚙️ Technical Implementation

The server is built on a single-threaded **Event-Driven Architecture**.

1.  **Non-Blocking I/O:** All file descriptors (sockets) are set to non-blocking mode (`fcntl`). This ensures that a slow client (reading/writing) never freezes the entire server.
2.  **Polling:** We use the `poll()` system call to monitor the state of all active sockets simultaneously.
3.  **Buffering:** Since TCP sends data in streams (not packets), the server implements a custom buffering system to handle partial commands (e.g., waiting for `\r\n`).

---

## 🛠️ Commands Supported

The server parses and executes the following IRC commands:

| Command | Description | Syntax |
| :--- | :--- | :--- |
| **PASS** | Authenticate with the server. | `PASS <password>` |
| **NICK** | Set or change nickname. | `NICK <nickname>` |
| **USER** | Register user details. | `USER <username> 0 * <realname>` |
| **JOIN** | Join a channel. | `JOIN <#channel>` |
| **PRIVMSG** | Send messages. | `PRIVMSG <target> :<message>` |
| **KICK** | Eject a user (Ops only). | `KICK <#channel> <user> :<reason>` |
| **INVITE** | Invite a user to a channel. | `INVITE <user> <#channel>` |
| **TOPIC** | View/Set channel topic. | `TOPIC <#channel> :<topic>` |
| **MODE** | Change channel modes (`i`, `t`, `k`, `o`, `l`). | `MODE <#channel> <modes> <args>` |
| **BOT** | Interact with the server bot. | `BOT <command>` |

---

## 🚀 Installation & Usage

### 1. Compilation

Use the provided `Makefile` to build the executable.

```bash
git clone [https://github.com/giopiro99/ft_irc.git](https://github.com/giopiro99/ft_irc.git)
cd ft_irc
make
```
### 2. Start the Server
```bash

The server requires a Port and a Password as arguments.

./ircserv <port> <password>

Example:

./ircserv 6667 42password
```
### 3. Connect a Client
```bash

You can connect using nc (Netcat) for raw testing or a GUI client like Irssi or HexChat.

Using Netcat:

nc localhost 6667

PASS 42password

NICK my_user

USER my_user 0 * :Real Name
```
🧪 Testing
```bash
This repository includes a stress test script to verify the server's stability under load.

# Run the stress test
./stress_test.sh
```
### 📂 Project Structure
```text
ft_irc/
├── Makefile
├── main.cpp                # Entry point & validation
├── includes/               # Header files (.hpp)
├── srcs/
│   ├── Server.cpp          # Socket setup & poll loop
│   ├── Client.cpp          # Client state management
│   ├── Channel.cpp         # Channel logic
│   ├── CommandHandler.cpp  # Parsing & Execution
│   └── commands/           # Individual command files
│       ├── join.cpp
│       ├── nick.cpp
│       ├── ...
│       └── bot.cpp         # Bonus Bot Logic
```
🧠 What We Learned

Socket Programming: Deep understanding of the TCP/IP stack, socket(), bind(), listen(), and accept().

RFC Compliance: The importance of strictly following technical documentation (RFCs) to ensure interoperability with existing software.

Concurrency: Managing multiple states without threads using poll().

C++ Containers: Extensive use of std::map and std::vector to manage users and channels efficiently.
