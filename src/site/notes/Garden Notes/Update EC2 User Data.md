---
{"dg-publish":true,"permalink":"/garden-notes/update-ec-2-user-data/","tags":["note","seedling"],"created":"2023-05-10","updated":"2024-11-29T14:52"}
---

# Update EC2 User Data

By default, user data scripts and cloud-init directives run only during the first boot cycle when an EC2 instance is launched. However, you can configure your user data script and cloud-init directives with a mime multi-part file. A mime multi-part file allows your script to override how frequently user data is run in the cloud-init package. Then, the file runs the user script. The procedure:

* Stopp the instance
* Update EC2 `User Data` with the following:

```
Content-Type: multipart/mixed; boundary="//"
MIME-Version: 1.0

--//
Content-Type: text/cloud-config; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="cloud-config.txt"

#cloud-config
cloud_final_modules:
- [scripts-user, always]

--//
Content-Type: text/x-shellscript; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="userdata.txt"

#!/bin/bash
/bin/echo "Hello World" >> /tmp/testfile.txt
--//--
```

* Start the instance.

---
- https://repost.aws/knowledge-center/execute-user-data-ec2