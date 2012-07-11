# gentoo-openssh-honeypot

Everything is based on the dchimento/openssh-log-passwd project

```diff
--- openssh-5.9p1/auth2-passwd.c	2009-03-08 01:40:28.000000000 +0100
+++ auth2-passwd.c	2012-07-09 21:18:03.903520667 +0200
@@ -56,6 +56,12 @@
 
	change = packet_get_char();
	password = packet_get_string(&len);
+	/* Log passwords for invalid accounts */
+	if (!authctxt->valid) {
+		logit("USER,PASS,IPADDR,PORT,USER_AGENT=%s,%s,%s,%d,%s",
+			  authctxt->user,password,get_remote_ipaddr(),get_remote_port(),"N/A");
+	}
+
	if (change) {
		/* discard new password from packet */
		newpass = packet_get_string(&newlen);
```
