# SPAMSINK - a Docker container with an ingest-it-all SMTP service
This is a Debian based setup that runs the Postfix tool "smtp-sink" to accept every mail that is thrown at it via SMTP.

# Overview
The tool "smtp-sink" is bundled with Postfix and has been created to conduct SMTP protocol testing as an SMTP receiver (in combination with "smtp-source", which is the sender in a test setup).

For doing SPAM related security research it can be used to create a "receive it all" kind of setup, the catch-all not being limited to specific domains or email addresses.
 
# Usage
Install the container with `docker pull hmlio/spamsink`.

Run the container with two environment variables, a volume mapping and a port mapping:

`docker run -d -e SINK_HOSTNAME="mail.example.com" -e SINK_PORT=2525 -v /tmp:/opt/spamsink/mails -p 25:2525 hmlio/spamsink`

The environment variable set as "SINK_HOSTNAME" will be used for HELO/EHLO responses of the SMTP server.

The environment variable set as "SINK_PORT" will be the port the container will listen on and should match the port forwarding configured with the "-p" switch.

The volume mapped into the container as /opt/spamsink/mails will be the folder where the received mails will be saved, one file per email, the naming configured to be "dincoming-%Y%%m%d%H%M.some_pseudo_random_id>"

Once the container runs, a connection can be established:

```
$ telnet localhost 25
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
220 mail.example.com ESMTP
```

Have fun! :-)
