https://sadservers.com/scenario/taipei
There is a web server on port :80 protected with [Port Knocking](https://wiki.archlinux.org/title/Port_knocking). Find the one "knock" needed (sending a SYN to a single port, not a sequence) so you can curl localhost.


# using knock tool and brace extension
`knock localhost {1..65535}`

# using netcat 
nc -z 127.0.0.1 {1..65535}
# test
echo $(curl localhost) | md5sum