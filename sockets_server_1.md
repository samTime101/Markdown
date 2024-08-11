
# samip101

# <ins>sockets in C</ins>
```
date: 2024/8/11 | २७ साउन २०८१, आइतवार
Author: samip101
```
## server.c
**A simple C program where *server* receives and prints messages from *client*.**

>This code works on *local* computers only and cannot be used to communicate with **remote servers**.

I have used linux sockets 
```C
#include <stdio.h>
#include <unistd.h>
#include <strings.h>
#include <sys/socket.h>
#include <sys/types.h>
#include <netinet/in.h>
#include <arpa/inet.h>
```
After we have all the required includes , lets define some macros
```
#define MAX 256
#define PORT 808
```
so MAX will be used in char which we will receive from client
Now the following below code is in `main()` function
```C
    socket_return = socket(AF_INET, SOCK_STREAM, 0);
    if (socket_return == -1) {
        perror("socket creation failed\n");
        return -1;
    } else {
        printf("socket created\n");
    }
```
To see the return type and more info we can do the following in linux terminal
```sh
man socket
```
>int socket(int domain, int type, int protocol);

lets continue and define some required socket structures and variable returns
```c
    int socket_return, accept_return, bind_return, listen_return;
    struct sockaddr_in server_address, peer_address;
```
Lets now bind the socket
```C
    bind_return = bind(socket_return, (struct sockaddr *)&server_address, sizeof(server_address));
    if (bind_return == -1) {
        perror("binding failed\n");
        close(socket_return);
        return -1;
    } else {
        printf("binding done\n");
    }

```
now that we have our socket binded , we shall now listen
```C

    // listening the socket
    listen_return = listen(socket_return, 5);
    if (listen_return == -1) {
        perror("listening failed");
        close(socket_return);
        return -1;
    } else {
        printf("server is  listening...\n");
    }
```
now that we *have* done all the configs
```c
    socklen_t peer_address_len = sizeof(peer_address);
    // accept connection
    accept_return = accept(socket_return, (struct sockaddr *)&peer_address, &peer_address_len);
    if (accept_return == -1) {
        perror("accepting failed\n");
        close(socket_return);
        return -1;
    } else {
        printf("connection accepted\n");
    }
```

now lets receive the message from client and close it
```C
    char msg[MAX];
    while (1) {
        bzero(msg, sizeof(msg));

        // Read message
        ssize_t bytes_read = read(accept_return, msg, sizeof(msg) );
        if (bytes_read == 0) {
            printf("client disconnected\n");
            break;
        } else if (bytes_read < 0) {
            perror("failed to read message\n");
            break;
        }
        //displaying client message
        printf("From client: %s", msg);

    }

    // closing
    close(accept_return);
    close(socket_return);

    return 0;
```
In next instance we will be writing **client** code so ,

# Stay Tuned

> ## samip101.web.app(c) 

