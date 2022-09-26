[![Codacy Badge](https://api.codacy.com/project/badge/Grade/5cc2fb7bdfb240eabe772b8bcbcbc461)](https://app.codacy.com/app/ivopetiz/portscanner?utm_source=github.com&utm_medium=referral&utm_content=ivopetiz/portscanner&utm_campaign=Badge_Grade_Dashboard)
[![Build Status](https://travis-ci.com/ivopetiz/portscanner.svg?branch=master)](https://travis-ci.com/ivopetiz/network-scanner)
[![Coverage Status](https://coveralls.io/repos/github/ivopetiz/portscanner/badge.svg?branch=master)](https://coveralls.io/github/ivopetiz/portscanner?branch=master)
# Go-Portscanner

TCP Network Port Scanner written in Go, nmap style.

## Lib

### Usage examples

#### Scan specific ports from an IP address

```go
package main

// imports fmt lib and portscanner lib.
import "fmt"
import "github.com/ivopetiz/portscanner"

// Main function.
func main() {

	// ports is an slice of strings corresponding to ports we want
	// to scan.
	ports := []string{"21", "22", "80"}

	// ip_t is a slice of strings containing an IP address.
	ip_t := []string{"192.168.1.100"}

	// open_ports has the results from PortScanner.
	open_ports := portscanner.PortScanner(ip, ports)

	fmt.Println(open_ports)
}
```

#### Scan port 4028 in a range of IP addresses

```go
package main

// imports fmt lib and portscanner lib.
import "github.com/ivopetiz/portscanner"

// Main function.
package main

// imports fmt lib and portscanner lib.
import (
	"fmt"
	"log"
	"net"

	"github.com/ivopetiz/portscanner"
)

// Main function.
func main() {

	// ports is a slice of ports to test.
	ports := []string{"4028"}

	GetLocalIP := GetOutboundIP().String()

	//fmt.Println(GetLocalIP[:len(GetLocalIP)-4])

	var TrimPos int
	_ = TrimPos
	for i, v := range GetLocalIP {
		if v == '.' {
			TrimPos = i
			//fmt.Print("TrimPos ", TrimPos, "\n")
			continue
		}
	}
	fmt.Println(GetLocalIP[:len(GetLocalIP)-(len(GetLocalIP)-TrimPos)])

	fmt.Println(GetLocalIP)

	fmt.Printf("GetLocalIP %T\n", GetLocalIP)

	// IP sequence is defined by a '-' between first and last IP address .

	//ips_sequence := []string{"192.168.88.1-254"}
	Ip1_254 := GetLocalIP[:len(GetLocalIP)-(len(GetLocalIP)-TrimPos)]
	Ip1_254 = Ip1_254 + ".1-254"
	ips_sequence := []string{Ip1_254}

	// results returns a map with open ports for each IP address.
	results := portscanner.IPScanner(ips_sequence, ports, true)
	// Once IPScanner has true for print_results, lib will present
	// results in CL with a nice presentation.
	fmt.Println(results)
	//fmt.Println(GetOutboundIP())
}
func GetOutboundIP() net.IP {
	conn, err := net.Dial("udp", "8.8.8.8:80")
	if err != nil {
		log.Fatal(err)
	}
	defer conn.Close()
	localAddr := conn.LocalAddr().(*net.UDPAddr)
	return localAddr.IP
}

```

## App

### Installation

```sh
 go install https://github.com/ivopetiz/portscanner
 git clone https://github.com/ivopetiz/portscanner.git portscanner
 cd portscanner
 go build cmd/portscanner/portscanner.go
```

## Usage examples

Different ways to use Go-Portscanner.

### Get help

```sh
 portscanner -h
 Usage of portscanner:
   -A   Scans all ports from port 1 to 1024
   -p   Port or ports to scan (default "80")
```

Shows the output above, presenting the available options.

### Localhost Portscanner

```sh
$ portscanner
```

Scans all local machine ports, from 1 to 1024.

### Network Machine Full Port Scan

```sh
$ portscanner -p 1-65535 210.67.210.76
```

Scan all TCP ports from a network machine.

### Network Discovery

```sh
$ portscanner -p 21,80 192.168.0.1-192.168.0.254
```

Looks for HTTP and FTP servers on 192.168.0.0/24.

### Ping Servers

```sh
$ portscanner -p 22 113.213.200.101 4.0.75.4 84.188.238.94 11.2.224.214 153.194.246.247
```

Checks if SSH servers are up.

## TODO

-   [ ] Improve performance.
-   [ ] Improve presentation.
-   [ ] Read IP:Port pair from file.
-   [x] Tests
-   [ ] Docker 
