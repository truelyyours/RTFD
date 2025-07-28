Start the OpenSSL Listener on port 8443:
`openssl s_server -quiet -accept 8443 -cert cert.pem -key key.pem`

Once the listener is running, open two new terminal windows and initiate Suricata to begin real-time log monitoring:
`sudo suricata -c /etc/suricata/suricata.yaml -i eth0`

On the other window, monitor the generated logs using the command:
`tail -f /var/log/suricata/fast.log`


