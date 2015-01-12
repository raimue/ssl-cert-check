# ssl-cert-check

Usage:

 - `./ssl-cert-check <days> <certspec1,certspec2,...>`
 - `./ssl-cert-check <days> --list=<FILE>`

This tool will warn you if any of the specified certificates expires in the
next \<days> days. If the --list syntax is used, the file is expected to
contain one certspec per line.

## Parameters

The first parameter is the number of days to warn in advance for expiring
certificates. All following parameters are treated as certificate
specifications and can be in one of the following formats:

 - An absolute path to a x509 PEM certificate file<br>
   For example:
     * /etc/apache2/ssl/example.org.pem

 - A file://\<path> URI<br>
   For example:
     * file:///etc/apache2/ssl/example.org.pem

 - A ssl://\<host>:\<port> URI<br>
   For example:
       * ssl://example.org:443

 - A \<proto>://\<host>[:\<port>] URI, this is the same as ssl://\<host>:\<proto>.<br> 
   The real port number is usually looked up in /etc/services, note that you often need the one with the 's' suffix, like "https", "imaps", etc.<br>
   For example:
     * https://example.org
     * imaps://example.org    (same as ssl://example.org:993)

 - A \<proto>+starttls://\<host>[:\<port>] URI<br>
   Use the STARTTLS command to start a in-protocol TLS session after opening an unencrypted connection. The openssl s\_client needs to support this protocol. At time of this writing, the supported protocols are "smtp", "pop3", "imap", "ftp" and "xmpp".<br>
   For example:
     * imap+starttls://example.org
     * smtp+starttls://example.org:587

## Examples

Example for your crontab:

    MAILTO=root
    6       6    * * *   nobody /usr/local/bin/ssl-cert-check 30 /etc/apache2/ssl/*.crt /etc/ssl/certs/dovecot.pem https://localhost ssl://localhost:465 smtp+starttls://localhost:587
