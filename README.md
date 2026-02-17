Where do ANSIBLE_SSH_USER and ANSIBLE_SSH_KEY come from?
1️⃣ ANSIBLE_SSH_USER → the Linux username of your target machine
Examples:

Cloud / OSCommon SSH user

Ubuntu VM (Azure/AWS/Local)   --> ubuntu
CentOS / RHEL  --> ec2-user or centos
Azure Linux -->VMazureuser
Your custom VM  -->the username you created
Bare-metal Linux servers   ->normal SSH user

To check on your server:
whoami


This is what you put into:
GitHub → Settings → Secrets → ANSIBLE_SSH_USER

2️⃣ ANSIBLE_SSH_KEY → your private SSH key
This is the private key (id_rsa) that connects to the target server.
You can generate one if you don’t already have it:

Generate new key on your machine:
Shell
ssh-keygen -t rsa -b 4096 -C "ansible"

This creates:
Private key → ~/.ssh/id_rsa  ← Put this inside GitHub Secret
Public key → ~/.ssh/id_rsa.pub ← Add this to your server

Add public key to your server:
Shell
ssh-copy-id -i ~/.ssh/id_rsa.pub ubuntu@<your-server-ip>

manually:
Shell
cat ~/.ssh/id_rsa.pub | ssh ubuntu@<server-ip> "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

Then copy the private key (id_rsa) content and paste into:

GitHub → Settings → Secrets → Actions → New secret → ANSIBLE_SSH_KEY

Make sure GitHub never gets your public key—only the private.

📌 Example (Ubuntu VM)
On your machine:
Shell
ssh-keygen -t rsa -b 4096S

Put this in secrets:
ANSIBLE_SSH_USER → ubuntu
ANSIBLE_SSH_KEY → content of ~/.ssh/id_rsa
