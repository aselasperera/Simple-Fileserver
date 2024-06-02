# Simple Go File Server

A concise and straightforward Go application to serve the contents of a local directory over HTTP. This is useful for quickly sharing files and directories over the internet.

## Features

- Serves static files from a specified directory
- Easy to set up and run
- Ideal for quick file sharing and testing purposes

## Getting Started

### Prerequisites

- [Go](https://golang.org/doc/install) (version 1.16 or higher)

### Installation

1. Clone the repository:

    ```sh
    git clone https://github.com/yourusername/simple-go-file-server.git
    cd simple-go-file-server
    ```

2. (Optional) Modify the directory path in `main.go` if you want to serve a different directory:

    ```go
    directoryPath := "."  // Change this to your desired directory
    ```

### Usage

1. Run the file server:

    ```sh
    go run main.go
    ```

2. Open your web browser and navigate to `http://localhost:8080` to see the contents of the directory being served.

### Code Explanation

The main code for the file server is in `main.go`:

```go
package main

import (
    "fmt"
    "net/http"
    "os"
)

func main() {
    // Replace "." with the actual path of the directory you want to expose.
    directoryPath := "."

    // Check if the directory exists
    _, err := os.Stat(directoryPath)
    if os.IsNotExist(err) {
        fmt.Printf("Directory '%s' not found.\n", directoryPath)
        return
    }

    // Create a file server handler to serve the directory's contents
    fileServer := http.FileServer(http.Dir(directoryPath))

    // Create a new HTTP server and handle requests
    http.Handle("/", fileServer)

    // Start the server on port 8080
    port := 8080
    fmt.Printf("Server started at http://localhost:%d\n", port)
    err = http.ListenAndServe(fmt.Sprintf(":%d", port), nil)
    if err != nil {
        fmt.Printf("Error starting server: %s\n", err)
    }
}
