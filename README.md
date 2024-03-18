# ZLab-Startup

## iTern2
If you are on Mac, I prefer to user [iTerm2](https://iterm2.com/) as opposed to the stock terminal, it supports multiple tabs, is much more customizable, and has many additional features


## Update ssh config file
Your ssh config file should be placed in `~/.ssh/config`

If `~/.ssh` directory does not exists, run `mkdir ~/.ssh`

On a Mac, you can create a blank ssh config file with `touch ~/.ssh/config`, and then open it in TextEdit with `Open -a TextEdit ~/.ssh/config`, then you can just copy and paste the contents below, save and exit

replace `<username>` with your ZLab username

you will have to make `~/.ssh/sockets` if it does not exist with `mkdir ~/.ssh/sockets`
```
Host *
        TCPKeepAlive = yes
        ServerAliveCountMax = 3
        ServerAliveInterval = 30
        ForwardX11 = yes
        ForwardX11Trusted = yes
        ControlMaster auto
        ControlPath ~/.ssh/sockets/%r@%h:%p
        ControlPersist 60

Host z010 z011 z012 z013 z014
     ProxyCommand=ssh -W %h:%p -l %r bastion.wenglab.org
     ForwardX11 yes
     ForwardAgent yes
     ForwardX11Timeout 7d
     ServerAliveCountMax 3
     ServerAliveInterval 15
     GSSAPIAuthentication yes
     User <username>
```
## Logging in
`ssh <username>@bastion.wenglab.org`
If it's your first time, you will be prompted to scan the QR code with any two-factor authentication app (I prefer google authenticator but feel free to use any app you are comfortable with). You should also be prompted to change your password. From this point forward, everytime you login, you will provide your password + two-factor code with no spaces

From bastion, you can then `ssh` into any of the ZLab servers, for example, `ssh z011`

## Build a pre-built container that has a host of bioinformatic software already installed
`singularity build /zata/zippy/<username>/bin/bioinformatics.sif docker://andrewsg/bioinformatics`

## Start JupyterLab server
`singularity exec -B /data:/data -B /zata/:/zata/ /zata/zippy/andrewsg/bin/bioinformatics.sif jupyter-lab --port=8888 --ip=z011 --no-browser --notebook-dir=/data/<group>/<username>`

Replace `z011` with the server you launched your notebook on
Replace `<username> with your ZLab username
Replace '<group>` with `zusers` if you are in ZLab, `musers` if you are in Moore Lab, or `rusers` if you are a rotation student in either group 
Access the notebook server from another terminal on your computer with `ssh -N -L 8888:<z011>:8888 <username>@z011`
You can then open your notebook server in your favorite web browser by navigating to `localhost:8888`
