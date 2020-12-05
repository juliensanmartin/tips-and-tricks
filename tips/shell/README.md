# Shell

- `sudo tail -f {some file}`

  > will keep track of all change in real time

- `ls -a`

  > show all file and hidden files

- `cd ~/.ssh`

  > goes to ssh folder

- `~/.ssh/authorized_keys`

  > store public ssh key to this user that can access the server, every ssh public key needs to be added there

- `sudo chown -R $USER:$USER {folder}`

  > will allow \$USER to access folder without having to add sudo in front

- `sudo npm i -g {package to install}`

  > install a nom package globally

- `find /bar -name foo.txt`

  > find in /bar filename that match foo.txt
  > Ex: find /var/log/nginx -type f -name "\*.log"
  > Ex: find / -type d -name log
  > All the directory with name log

- `grep -i ‘jem’ /var/www`

  > search jem inside every file in /var/www

- `ps aux | grep node`

  > search all node process

- `nmap {ip address}`
- `nmap -sV {ip address}`

  > scan open port - need to be installed

- `less /etc/services`

  > open all services with port

- `top`

  > real time on what’s going on

- `cat /etc/issue`

  > which linux distribution

- `ls -lsha`

  > list everything with permissions

- `cat package.json | grep version | head -1 | awk -F: '{ print \$2 }' | sed 's/[",] > g'`
  > get current version of package.json

![Linux command](https://i.redd.it/isnefnt32wn21.jpg)
