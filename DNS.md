nslookup -port=3535 app.lti localhost
dig @127.0.0.1 -p 3535 moodle.lti

dig @172.20.0.15 -p 53 moodle.lti


docker run --rm -p 3535:53/udp --name lti_dns -ti lti_dns /bin/sh

