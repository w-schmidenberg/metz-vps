1. Add user

```sh
adduser metzadmin
```

2. Sudo Group

```sh
usermod -aG sudo metzadmin
```

3. Add SSH keys and log in:

```sh
ssh root@<METZ-VPS-IP-ADDR> 'mkdir -p /home/metzadmin/.ssh && echo "<PUBLIC_KEY_CONTENT>" >> /home/metzadmin/.ssh/authorized_keys && chmod 700 /home/metzadmin/.ssh && chmod 600 /home/metzadmin/.ssh/authorized_keys && chown -R metzadmin:metzadmin /home/metzadmin/.ssh'
```

```sh
sudo apt get tmux
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
sudo vim /etc/ssh/sshd_config
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

`sudo ufw enable`