# VPS Setup and Hardening

Login via SSH to your VPS server and do the following steps:

1. Add user

```sh
adduser metzadmin
```

2. Sudo Group

```sh
usermod -aG sudo metzadmin
```

3. Add SSH keys and log in: 
*You might need to [create a ssh key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) if you don't have one already 

```sh
ssh root@<METZ-VPS-IP-ADDR> 'mkdir -p /home/metzadmin/.ssh && echo "<PUBLIC_KEY_CONTENT>" >> /home/metzadmin/.ssh/authorized_keys && chmod 700 /home/metzadmin/.ssh && chmod 600 /home/metzadmin/.ssh/authorized_keys && chown -R metzadmin:metzadmin /home/metzadmin/.ssh'
```

- This is optional, it will install [tmux](https://github.com/tmux/tmux/wiki) for convenience.
```sh
sudo apt install tmux
```

```sh
tmux new -s vps
ssh metzadmin@<METZ-VPS-IP-ADDR>
```

Detach with `Ctrl+b`, release, `d`

4. Install Docker:
   https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository

```sh
sudo systemctl enable docker
sudo usermod -aG docker metzadmin
su - $USER
```

5. Harden SSH:

```sh
sudo nano /etc/ssh/sshd_config
```

Make this changes

```bash
PermitRootLogin no
PasswordAuthentication no
UsePAM no
```


6. Firewall
```sh
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow OpenSSH
sudo ufw allow http
sudo ufw allow https
```

```sh
sudo ufw show added
```

# THIS WILL ENABLE THE FIREWALL, MAKE SURE YOUR SSH KEY WORKS OR YOU WILL GET LOCKED OUT OF THE VPS!
```sh
sudo ufw enable
```
