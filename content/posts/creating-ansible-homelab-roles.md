---
title: "Creating Ansible Homelab Roles"
date: 2023-04-24T20:39:05-06:00
draft: true
tags:
- ansible
- homelab
- sre
- devops
- engineering
- raspberry-pi
---
# Tentative Plan
I want to use #tools/ansible to:
1. migrate OS on the Raspberry Pi units to an SSD from the SD card
2. Set up `alesauce` user on all Pis and ensure Ubuntu/crow-magnon users are deleted
3. provision a #raspberry-pi  unit with a headscale control server.
4. Provision multiple Pis with tailscale clients
5. Test configuration and DNS

# OS Migration
- If I want to make an Ansible role to handle this, I think I will need to start with creating an Ansible dev shell via Nix in my projects directory. Given that, think I'll start with creating a Git repo and a new repo on GitHub so I can have a backup/version control of my work.
- Down a bit of a rabbit hole about Nix development environments (of course), but I think I will use the `direnv` -> `venv` -> `pip install --user` method for now of getting Ansible and using all of those tools.
- I'm not able to run `docker` without `sudo`, but let's see if that will actually affect me.
- Planning to use [[Ansible Molecule]] to test out the new roles as well.
- The Tom's Hardware article (linked below) was more focused on a brand new installation of an OS on a Raspberry Pi. Found [this article](https://www.zdnet.com/article/booting-my-raspberry-pi-4-from-a-usb-device/) instead in search of an article that helps change the bootloader on a current install.
- Trying to find the version of bootloader I have using the command `vcgencmd bootloader_version` and got the error `VCHI initialization failed` - looking into that now. Found [this article](https://chewett.co.uk/blog/258/vchi-initialization-failed-raspberry-pi-fixed/) that says the issue is that my user is not part of the `video` group on the host. Use the command `sudo usermod -aG video crow-magnon` to fix this issue.
	- Don't forget to log out and log back in so you can re-source your permissions.
	- That fixed it!
- The articles I can find right now are all focused on Raspberry Pi OS instead of OS-agnostic.
- Found [this article](https://medium.com/xster-tech/move-your-existing-raspberry-pi-4-ubuntu-install-from-sd-card-to-usb-ssd-52e99723f07b) that almost specifically handles my issue. Really nice tbh. Going to read through that.
- I've decided to start from a fresh install of Ubuntu OS because I halfway borked `mariposa1` to be honest.
- Flashing Ubuntu 22.04 LTS directly to my SSD and modifying the bootloader from the rpi command line directly. Using the utility `rpi-eeprom-config` to modify the Pi to use usb boot first.
- Ran command `sudo -E rpi-eeprom-config --edit` which will open the config file in your environment's text editor.
	- Documentation used for these steps:
		- [Bootloader docs](https://www.raspberrypi.com/documentation//computers/raspberry-pi.html#raspberry-pi-4-bootloader-configuration)
		- [config.txt format](https://www.raspberrypi.com/documentation//computers/config_txt.html)
		- [Imager docs](https://www.raspberrypi.com/documentation//computers/raspberry-pi.html#imager)
		- [Options to update the bootloader](https://www.raspberrypi.com/documentation//computers/raspberry-pi.html#raspi-config)
		- Initially took a link at [this post](https://mutschler.eu/linux/install-guides/raspi-btrfs/) about migrating the boot to a USB but have not used it yet.
- Able to get Ubuntu server 22.04.2 LTS working by using the following commands. Note that this method only worked because I had a working installation that I used to modify the bootloader from, then switched over to new booter.
	```shell
	sudo -E rpi-eeprom-config --edit
	# Added the following line to the config file
	BOOT_ORDER=0xf14
	# based on this table: https://www.raspberrypi.com/documentation//computers/raspberry-pi.html#BOOT_ORDER
	# sets the bootloader to the following order:
	# USB
	# SD card
	# restart if neither is found
	sudo reboot
	```

## References
- Using [this guide from Tom's Hardware](https://www.tomshardware.com/how-to/boot-raspberry-pi-4-usb), [this StackExchange answer](https://raspberrypi.stackexchange.com/questions/127757/how-to-clone-os-from-sd-to-ssd-using-command-line), and the [GitHub repo for rpi-clone](https://github.com/billw2/rpi-clone) as my starting point to migrate the boot drive to the SSD


# Re-install Docker
- Unfortunately, I've changed my GitHub username since the last time I used Tailscale, so I will have to wait until support gets back to me so I can fix my Tailnet.
- I have completed up until the actual start of the `headscale serve` command in the initial installation and configuration section in the [repo](https://github.com/juanfont/headscale/blob/main/docs/running-headscale-linux.md). Once I have my tailnet renamed, I will test it from a few clients. then I will start working on creating the create new user roles for Ansible so I can provision all of my raspberry pis when they come online.
- I'm uninstalling my docker install on my desktop because it didn't work properly and I want to start from scratch.
- Tailscale also got back to me really quickly and my tailnet is back in operation - after some consideration, I've decided to not proceed with headscale for now, but will keep this task alive as the basis of "provision all nodes" so I can create an Ansible playbook to create the new `alesauce` user on my nodes along with passwordless sudo access
- I have now created an ansible dev environment (again) and completely removed Docker from my machine. Going to now work on getting Docker and molecule set up properly.
- Following the [Docker Engine installation instructions](https://docs.docker.com/engine/install/linux-postinstall/)
- This time the docker rootless instruction worked. Back to Ansible I go!

## References
- Using [this article](https://peterteszary.hashnode.dev/how-to-install-docker-on-pop-os) to re-install Docker for use with Molecule. It was basically a copy of the Docker Engine installation, lol

# Molecule Setup and Configuration
- I don't have any clue how to use Molecule, so I just re-created the role directory a few times until I remembered to use the `--driver-name docker` part of the command after `molecule init role`
- Following the guide in the [Molecule getting started](https://molecule.readthedocs.io/en/latest/getting-started/#inspecting-the-moleculeyml) guide to do some basic work with Molecule.
- My NVIM setup is giving me some weird errors and trouble on my desktop now. Really need to start instituting my "one dotfile task a day" thing - maybe add that in to weekly planning
- First issue with Ansible - getting an error trying to run ansible because of an interpreter issue. See error below:
```
fatal: [ubuntu_instance]: FAILED! => {"ansible_facts": {}, "changed": false, "failed_modules": {"ansible.legacy.setup": {"ansible_facts": {"discovered_interpreter_python": "/usr/bin/python"}, "failed": true, "module_stderr": "/bin/sh: 1: /usr/bin/python: not found\n", "module_stdout": "", "msg": "The module failed to execute correctly, you probably need to set the interpreter.\nSee stdout/stderr for the exact error", "rc": 127}}, "msg": "The following modules failed to execute: ansible.legacy.setup\n"}
```
- Found [this doc](https://docs.ansible.com/ansible/latest/reference_appendices/interpreter_discovery.html) on Ansible Interpreter Discovery and going to go through that to see if I can set the interpreter in config.
- [This is the most useful guide I've found so far](https://maciej.lasyk.info/2016/Jun/27/working-with-virtualenv-and-ansible/) to figure out why Ansible isn't working in my virtualenv. Tl;Dr is it doesn't read the config files apparently, and the variable has to be set in the inventory file to be read by Ansible. But I can't figure out how to do this in Molecule.
- After all of these issues, I found [this GitHub issue](https://github.com/ansible-community/molecule-docker/issues/151) where someone experienced the exact same issue. Tl;Dr: The error finding Python interpreter was on the remote host (e.g. container), not my local host. No more of that error!
- I get a new error now (below) but it at least it's a different one!
```
fatal: [debian]: UNREACHABLE! => {"changed": false, "msg": "Failed to create temporary directory. In some cases, you may have been able to authenticate and did not have permissions on the target directory. Consider changing the remote tmp path in ansible.cfg to a path rooted in \"/tmp\", for more error information use -vvv. Failed command was: ( umask 77 && mkdir -p \"` echo ~/.ansible/tmp `\"&& mkdir \"` echo ~/.ansible/tmp/ansible-tmp-1681796130.0149798-31391-117799164586022 `\" && echo ansible-tmp-1681796130.0149798-31391-117799164586022=\"` echo ~/.ansible/tmp/ansible-tmp-1681796130.0149798-31391-117799164586022 `\" ), exited with result 1", "unreachable": true}
```
- I think the above ^^ issue was because I forgot to `molecule reset` and `molecule create` a new target container with the create name and everything, so it had issues. After doing reset and create, the sample task works!
- I definitely almost uninstalled Ansible, reinstalled Pop!OS, and picked a new config management tool during this troubleshooting session. I wish I was kidding.
	- "Ansible is dumb. It's the tool's fault, not mine. I'm gonna use a REAL config manager tool now and it'll solve these problems."
- After pulling all those together, I do feel better now having accomplished something
- Next steps, migrate the expensify playbook instructions over to the new molecule role directory
- I want to get tailscale installed on all of my servers by tomorrow night preferably. Nice thing is only needing to get it right once, then it will be a lot easier(hopefully)
- **Note:** After coming back the next day, don't forget to `molecule reset` when logging off for the day/and or starting a new test. That way it will delete the old instance and start a new one for testing.

# Converting User Config Playbook to Role
- Found [this Quora answer](https://www.quora.com/How-do-I-switch-the-user-executing-tasks-in-Ansible-roles) for changing the user executing an Ansible playbook. Might be able to use that to log into the new user in the new create/configure role and delete the default ubuntu.
	- Question - do I want to delete the default `ubuntu` user from this playbook though? seems like I could just leave that from the role and allow the user of the role to clean up the default info at the end. Also leaves the playbook able to be used cross-distro.
- After looking at a lot of other Ansible roles for inspiration, back to working on this one, lol.
- Got some initial stuff down. Also had some initial dumb failures:
	- Forgetting that when templating values in an Ansible playbook (e.g. `{{ new_user_name }}`), if the template is the first value on the line it needs to be quoted
	- Not realizing that I had the first line of `tasks/main.yml` as `tasks:`, which isn't needed in a role's tasks file because it's supposed to be a list of tasks.
	- Also forgot that for Ansible's iteration (e.g. `with_items:`), need to use `item` in the task line or it's gonna think it's an undefined variable)
- After installing sshd and trying to validate new sshd file, I got the following error: `"Missing privilege separation directory: /run/sshd\r\n"`. I have never seen this error before, so should be interesting.
	- Found [this GitHub issue](https://github.com/ansible/ansible-container/issues/141). Looks like the issue is when you start an SSH server from outside of Debian/Ubuntu's usual init method.
	- Running `mkdir -p /var/run/sshd` solved this one
- Next was this error message from attempting to restart SSHD: `"System has not been booted with systemd as init system (PID 1). Can't operate.\nFailed to connect to bus: Host is down"`.
	- Based on some searching, seems the issue is that Docker containers do not come with SystemD because of compatibility issues.
	- Found [this StackOverflow answer](https://stackoverflow.com/questions/53383431/how-to-enable-systemd-on-dockerfile-with-ubuntu18-04) that suggests using a script in the container to essentially replicate `systemctl` functionality for Ansible's purposes
	- [GitHub repo](https://github.com/gdraheim/docker-systemctl-replacement) for the script mentioned above.
	- have so far been working on this by adding the Python script with the below commands.
```shell
# pull the script to the local machine
curl https://github.com/gdraheim/docker-systemctl-replacement/blob/master/files/docker/systemctl3.py > /usr/bin/systemctl
# check if the other systemctl file exists and link to new systemctl if so
test -L /bin/systemctl || ln -sf /usr/bin/systemctl /bin/systemctl
# make the script executable
chmod +x /usr/bin/systemctl
```
- And the role completely worked!!! Just need to do those extra config steps and I can upload this bad boy to GitHub/Ansible Galaxy
- Seems like the best way might just be to create a separate Dockerfile and upload it to DockerHub to test from there.
- I ran into the same directory error above (pretty misleading, tbh). This time the issue was because I had changed the command in the `molecule/defaults/molecule.yml` file and the container exited since the command was blank. I removed that and et voila, we're in business!
- Role is tested and works great on the new docker image! Going to push it to GitHub, then create a quick Ansible playbook using these roles to set up my whole cluster!
- Forgot one thing - also wanna change the hostname on each server when booting it up. Worth adding on real quick to the role.


## Changes Needed for Container Config for Role
I don't know how yet, but need to do some config to the created instance after creation. I can see an option for a configuration playbook when running `molecule create`.
- [x] Install openssh-server
- [x] Install sudo
- [x] Create the needed ssh directory with: `mkdir -p /var/run/sshd` 
- [x] Either of two options:
	- [ ] ~~Use an Ubuntu image with systemd enabled like [this example on DockerHub](https://hub.docker.com/r/jrei/systemd-ubuntu)~~
	- [x] Use [this Python script](https://github.com/gdraheim/docker-systemctl-replacement/blob/master/files/docker/systemctl3.py) that will emulate systemd functionality

# Building a new Raspberry Pi Configuration Playbook
- Might even be able to just use [this role](https://github.com/artis3n/ansible-role-tailscale) to install Tailscale for me (also in Installing Tailscale Client below)
- Well... this is weird. I think I need to reboot my switch to factory settings, because I can see that my Raspberry Pis are connected to the network, but when I try to ssh/ping them, they are unreachable. will have to figure this out tomorrow.
- After thinking about it last night (april 18th), I think I am going to install node_exporter on the cluster as is so I can immediately start getting metrics and set up a dashboard. Bringing in my notes on Grafana/prometheus:
- This is a definite MUST for installation at some point, just unsure if this will be split into its own project (same as Kubernetes above)
- [[Jeff Geerling]] has roles for [Prometheus node_exporter](https://galaxy.ansible.com/geerlingguy/node_exporter) and [nginx](https://galaxy.ansible.com/geerlingguy/nginx). I believe `node_exporter` is a requirement to use #tools/prometheus , and nginx was recommended somewhere for acting as the webserver for displaying #tools/grafana  dashboards.
- I think the rest of this stuff will definitely be separate projects, so I will add them into separate projects/notes.

## Iffy: Setting up Firewall rules
- I'm not exposing these to the Internet yet, so I don't know that I need to set that up yet.
- If/when I do, I can always use the [firewall setup playbook](https://github.com/alesauce/expensify-remote-infrastructure-challenge/blob/main/expensify-infra-challenge/cluster-lockdown/bastion-host-configure.yaml) I created for the Expensify remote infra challenge

## Iffy: Installing Kubernetes
- Could use `containerd` as container run time, [[Jeff Geerling]] has [a role](https://galaxy.ansible.com/geerlingguy/containerd) for that (of course).
- Jeff Geerling has a [Kubernetes role](https://galaxy.ansible.com/geerlingguy/kubernetes) which I don't think I can/will use, but I think will be useful for inspiration for setting up [[k3s]]
- Microk8s instead of k3s? idk
- Set up Docker with [[Jeff Geerling]]'s [Docker ARM Playbook](https://galaxy.ansible.com/geerlingguy/docker_arm)
- Install Pip with Jeff Geerling's [Pip role](https://galaxy.ansible.com/geerlingguy/pip)
- 
## Way out There: Redis
- I've wanted to play around with Redis for a while, and my boy [[Jeff Geerling]] has an [Ansible role to install redis](https://galaxy.ansible.com/geerlingguy/redis). I'd likely just use redis in Docker, but who knows.


# Installing Tailscale Client
- not at this step yet, but will follow the [Tailscale install instructions](https://tailscale.com/download/linux/ubuntu-2204) to create an Ansible playbook to install and configure Tailscale on a new client server.
- i did something similar on my [expensify infra challenge](https://github.com/alesauce/expensify-remote-infrastructure-challenge/blob/main/expensify-infra-challenge/load-balancer/envoy-archive/install-envoy.yaml) with the initial attempts at configuring Envoy. Figure that will be useful
- Might even be able to just use [this role](https://github.com/artis3n/ansible-role-tailscale) to do that for me :)

# Provisioning Raspberry Pi Cluster
- Last night, the Pis became unreachable for some reason from my desktop. I am still unsure why, given that I was attached to the network still.
- I am going to try resetting the switch to factory settings to see if that will fix the issue.
- Was able to trigger the reset via the web interface for the switch, under `System Tools -> System Reset`
- From what I can see online, I think there *might* be issues if you try to use the same hostname for different hosts. So I'm going to start executing my new Ansible playbook one host at a time and see if that resolves any other connection issues.
- Had a couple of dumbass errors, but I eventually got my inventory working properly and the playbook is now running with the following roles:
	- provision_new_stock_linux_server
	- tailscale
	- node_exporter
- Created one more basic playbook to delete the ubuntu user and reboot the Raspberry Pi so the hostname change could take effect. Let's see if it's still accessible via Tailscale admin panel.
- I'm getting a `key exchange: connection reset by peer` when attempting to SSH normally. Wonder if the issue is Tailscale now takes over SSH connections?
- So I think the issue was that I wasn't trying to use Tailscale IP address, which makes sense.
- But now, I still can't SSH into the server and the SSH connection is hanging while attempting to receive the version string. Found [this Q and A](https://superuser.com/questions/1374076/what-does-it-mean-if-ssh-hangs-after-connection-established) which says that the issue is on the server side here.
- This is similar to what happened last night, and when I look at the Xfinity router I only see one Raspberry Pi despite another one being connected. I think the switch might be the bottleneck here.
- For mariposa1 at least, I tried restarting tailscale on my desktop with the `--ssh` flag (`tailscale up --ssh --force-reauth`) and will see if that does the trick.
- Nope, still the same thing. The name is able to resolve to the proper Tailscale IP, but the connection hangs. I'm leaning back towards my switch being the bottleneck.
- Using the `ping` command works on both the LAN and tailscale IP. I'm wondering why the SSH connection is hanging then, and why it's not showing on XFinity router page.
- I think I might have to set up the OpnSense router to get the switch to work properly. apparently it doesn't route traffic at all??
- I changed the ACL and now I am able to SSH into mariposa1 via the web browser, but still hanging when I try via regular terminal command.
- Scratch that, looks like SSH client is also hanging while trying the browser-based SSH
- I think the issue is on the server itself - tailscale network can talk, and I can `tailscale ping`, but nothing else is happening. Weird
- Going to try from my macbook to see if the issue persists. At that point, might just be that my desktop/switch are the issues.
- Ok, so it's also not working from my macbook. thinking the server might be hanging or something. high-key tempted to just start spending some money on linode or digital ocean credits and at least then I could test it with multiple machines.

# [[2023-04-20]]
- Ok, I was able to get back in to the server after unplugging it and plugging it back in. I'm going to try plugging another node in and see if the same issue happens - in that case, I'm still not sure what it could be, but at least I'd have more of a clue.
- I plugged in a second node. This time, no traffic hangup on mariposa1 and I am still able to SSH into `mariposa1`. However, I am still unable to see the new node from the router page.
- I think this might be a good time to start with my plan of setting up OPNSense to get a good subnet going for my stuff so I have a router.
- Ok, I think the issue *may be* that the only Pi I've done the treatment to to boot from USB first is mariposa1... Or that the POE is insufficient to power the other ones and their SSDs. I might try to go through the ones I have and see if any of them boot correctly, but otherwise might just get that computer this weekend and call it good.
- I'm going to call this project good for now. Will clean these notes up later tonight/tomorrow.
