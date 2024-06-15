Lo más fácil va a ser meter en el /etc/hosts las entradas:
moodle.lti  172.20.0.5
app.lti     172.20.0.10

# Estos no hacen falta en principio
php.lti       172.20.0.2
postgress.lti 172.20.0.3

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
