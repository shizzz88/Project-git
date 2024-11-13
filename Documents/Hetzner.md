sometimes the server on Hetzner does validate the SSH key. for that, first login via console on the Hetzner account by creating a new root password using rescue tab.
after that, login with the new root password and change the password using passwd command. 
Now restart the system using CTRL+ALT+DEL tab on the console.
Now login via the Terminal on the windows. It should create three lines in the known hosts file:
ssh-ed25519
ssh-rsa
ecdsa-sha2-nistp256
