sometimes the server on Hetzner does validate the SSH key. for that, first login via console on the Hetzner account by creating a new root password using rescue tab.
after that, login with the new root password and change the password using passwd command. update the system
Now restart the system using CTRL+ALT+DEL tab on the console.
Now login via the Terminal on the windows. It should create three lines in the known hosts file:
ssh-ed25519
ssh-rsa
ecdsa-sha2-nistp256
In order to add another SSH-Key to the existing Server, you have to modify the authorized_keys file in .ssh folder in the server. Simply generate the SSH-key locally using ssh-keygen -t ed25519. Add this key either by opening the server via SSH or you can use the integrated console in the server page on Hetzner. Use any text editor like nano and paste the SSH-key in a new line. save the file, roboot the server and connect via SSH.
