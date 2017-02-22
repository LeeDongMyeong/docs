<!-- TITLE: Troubleshooting -->
<!-- SUBTITLE: A collection of common issues / errors and their possible solutions. -->

# Wiki.js won't start
### Error: Listening on port XX requires elevated privileges!

**Cause**: Some Linux installations prevent Node.js from binding to ports in the lower range (e.g. < 1024). This occurs if you set a port such as **80** in config.yml.

**Solution A**: Allow the node process to safely access the port requested:
```shell
# Ubuntu / Debian

sudo apt-get install libcap2-bin
sudo setcap cap_net_bind_service=+ep `readlink -f \`which node\``
```

**Solution B**: Create a port redirection rule: *(In the example below, you must set the port to 3000 in config.yml)*
```shell
sudo iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 3000
```

**Solution C**: Use a web server in front of Wiki.js. For example, use nginx to listen to port 80 / 443 and proxy all requests to Wiki.js running on a higher port (e.g. 3000).