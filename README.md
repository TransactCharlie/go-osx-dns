# go-osx-dns
A simple golang interface to monkey with osx dns settings.

This uses calls to `scutil` under the covers to fetch the current network interface and then modify it's DNS settings.

## How Does This Work?
osx networking is..... less simple than linux. Editing `resolv.conf` is not going to end up doing what you expect!

This program:
    
1. Finds which network service we are currently using by checking `State:/Network/Global/IPv4`
2. Retrieves that services DNS settings
3. Builds a new list of nameservers from those settings with localhost `172.0.0.1` prepended
4. Writes these to the State
5. Writes these to the current Setup of that interface so they become used. 


## Building
`go build .`

Will create `go-osx-dns` binary

## Usage
Run the binary as root to prepend a `172.0.0.1` nameserver entry 
```
sudo ./go-osx-dns
Password:
8EDAB026-0906-4D3C-A939-5542E501A70A
[192.168.1.1]
[127.0.0.1 192.168.1.1]
```

```
scutil --dns
<stuff>
resolver #1
  nameserver[0] : 127.0.0.1
  nameserver[1] : 192.168.1.1
  if_index : 6 (en0)
  flags    : Scoped, Request A records, Request AAAA records
  reach    : 0x00020002 (Reachable,Directly Reachable Address)
```