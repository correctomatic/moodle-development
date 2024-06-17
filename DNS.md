You need to temporarily set the DNS server to the IP of the DNS server in the Docker container. OR you can add the
entries defined in dns/dnsmasq.conf to your /etc/hosts file.

# -------------------------------------------
# Entradas para desarrollo de aplicaciones LTI
# -------------------------------------------
172.20.0.5  moodle.lti
172.20.0.10 app.lti
# (Estos no hacen falta en principio)
172.20.0.2  php.lti
172.20.0.3  postgress.lti

# This is the address of the LTI server
# It's run in a separate project, but MUST have this address



nslookup -port=3535 app.lti localhost
dig @127.0.0.1 -p 3535 moodle.lti

dig @172.20.0.15 -p 53 moodle.lti


docker run --rm -p 3535:53/udp --name lti_dns -ti lti_dns /bin/sh



nmcli connection show
sudo nmcli connection modify <connection-name> ipv4.dns "DNS server 1, DNS server 2"
sudo nmcli connection down <connection-name> && sudo nmcli connection up <connection-name>

CONNECTION="Cable"
sudo nmcli connection modify $CONNECTION ipv4.dns "172.20.0.15"
sudo nmcli connection up $CONNECTION
resolvectl flush-caches


sudo systemctl stop systemd-resolved
