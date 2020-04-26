```
global
    ulimit-n  51200
 
defaults
    log    global
    mode    tcp
    option    dontlognull
        timeout connect 5000
        timeout client  50000
        timeout server  50000
 
frontend ss-in
    bind *:relay_server_port
    default_backend ss-out
 
backend ss-out
    server server1 proxy_server_ip:proxy_server_port send-proxy-v2 maxconn 20480

```