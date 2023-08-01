---
title: "An Idiot's Journey into Automation with Ansible"
date: 2023-04-24T20:39:05-06:00
draft: false
tags:
- ansible
- homelab
- sre
- devops
- engineering
- raspberry-pi
- nix
- automation
---

# The "Joys" of Learning
I love the feeling of adding new skills and data points to my resume. For me, that feels like the exclamation point on top of whatever I was trying to accomplish. On the flip side, my resume has **occasionally**, **slightly** overstated some of my accomplishments or left my skill level... open to interpretation. Recently, I've had a few occasions to reflect on my current skillset versus where I want to be. Creating this website has also been a sobering reminder of the gap between where I am and where my goals lie.

Originally, the purpose of this website was to give myself an avenue where I can show off my curiosity and my ability to learn. Due to time constraints and any other number of excuses though, it still sits mostly barren. Something that is extra galling to me is the juxtaposition of the tag line on the front page and the content available - I explicitly mention diving into new things and writing through those experiences. In contrast, the two articles present are more related to some slightly more abstract concepts like goals setting and mindsets.

As I wrote about [previously](https://alexandersauceda.dev/posts/perfect-as-enemy/), it's better to release something that is "good enough" rather than hold out for something "perfect" and end up with no output at all. Admittedly, I would love to write *The Definitive Resource on Going from "Ansible Zero" to "Ansible Hero"*. However, I am unsure who would consider me an "Ansible Hero" outside of someone who either:
A) is inebriated beyond the point of coherency, or
B) hasn't seen or interacted with a computer since 1975.

Given my current reality, I will have to be be realistic and instead write:

# An Idiot's Journey into Automation with Ansible
My therapist probably won't like the way I'm talking about myself, but the good news is that she won't read it. Don't be a snitch!

<figure>
    <iframe src="https://giphy.com/embed/OAdoioNIb3lyPEgcBY" width="480" height="270" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/parksandrec-parks-and-recreation-rec-OAdoioNIb3lyPEgcBY">via GIPHY</a></p>
    <figcaption>Don't forget!</caption>
</figure>

Now that the standard "Internet recipe with a three-page backstory" formalities are out of the way, I should probably give a little bit of context on what I am attempting to do so readers can decide if this post will actually help them or not. This was my first time attempting to create an Ansible "role" *link to ansible role documentation here*, or a set of tasks that could be used in other playbooks. I already had a playbook that completed most of this task from a job interview, so this was mostly about migrating it to a role that could be used by others. The tasks I wanted my role to perform were:
1. Set up a new user so the default user on the machine could be deleted
2. Install SSHD and upload a lightly opinionated config file
3. Change the hostname on the machine
4. Validate said config file
5. Install a Tailscale client *link to Tailscale page* and start the Tailscale service
6. Install Prometheus node exporter to retrieve metrics

Finally, I wanted to test out new Ansible role using [Ansible Molecule](https://ansible.readthedocs.io/projects/molecule/) and push that new Ansible role to my GitHub and Ansible Galaxy profiles. Feel free to check out the repo for the Ansible role created in this article [here!](https://github.com/alesauce/provision-new-stock-linux-server) You can view the commit history to see some of the diffs where I figured out solutions to some of the problems I ran into. Also link to Ansible Galaxy here.

# Building the Supporting Cast
<figure>
    <iframe src="https://giphy.com/embed/l0Iy70zlRjH5ev3qw" width="480" height="199" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/filmstruck-kevin-spacey-benicio-del-toro-l0Iy70zlRjH5ev3qw">via GIPHY</a></p>
    <figcaption>Maybe not quite this level of a supporting cast...</caption>
</figure>
Before I could get too into the weeds, I needed to ensure that I had the tools to build and test this role. Specifically, I needed:
- an installation of Docker that did not require `sudo` to use,
- an installation of Ansible and Ansible Molecule to test the role,
- Molecule configured to use the Docker driver, and
- verification that all of the above worked correctly.

First, I followed the [guide to configure Docker for non-sudo access](https://docs.docker.com/engine/install/linux-postinstall/). When someone has already documented exact steps for you to follow, turns out pretty much anything is easy to accomplish!

Next, I started working on the [Ansible Molecule Getting Started guide](https://ansible.readthedocs.io/projects/molecule/getting-started/). I will neither confirm nor deny that I was a tad intoxicated when I started working on this part. Don't judge me - it was a Friday night after a long work week. Because of my... shall we say, less-than-optimal brainpower available, I had to create, then delete, then re-create a new Ansible role directory a couple of times until I remembered to use the `--driver-name docker` flag for the `molecule init role`. Was there probably a way to fix it after creating the role? Most definitely. But my thought process was it would be easier to just delete and re-create. If your mouth is hanging open in slack-jawed amazement at the level of my idiocy, just remember that I warned you in the title.

<figure>
    <iframe src="https://giphy.com/embed/xT5LMLJapSVPvCUEGQ" width="480" height="362" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/season-15-the-simpsons-15x8-xT5LMLJapSVPvCUEGQ">via GIPHY</a></p>
    <figcaption>I don't appreciate you attacking me like this Giphy, but well played nonetheless.</caption>
</figure>

Anyway, once I got the role set up, I wanted to test that it worked properly with the basic scaffold that a new Molecule project starts with before I started throwing my own nonsense in there. I started running the commands from the [Run test sequence commands](https://ansible.readthedocs.io/projects/molecule/getting-started/#run-test-sequence-commands) section of the guide. Since I had followed the guide religiously up to this point, obviously the commands all worked the first time and I was on easy street right? WRONG. I was able to create a container with `$ molecule create` and see said container with `$ molecule list`, but when I tried to run `$ molecule converge`, I promptly received the below error:

```shell
fatal: [ubuntu_instance]: FAILED! => {"ansible_facts": {}, "changed": false, "failed_modules": {"ansible.legacy.setup": {"ansible_facts": {"discovered_interpreter_python": "/usr/bin/python"}, "failed": true, "module_stderr": "/bin/sh: 1: /usr/bin/python: not found\n", "module_stdout": "", "msg": "The module failed to execute correctly, you probably need to set the interpreter.\nSee stdout/stderr for the exact error", "rc": 127}}, "msg": "The following modules failed to execute: ansible.legacy.setup\n"}
```

And, well...

## The Head-Banging Begins
While looking up this error message, I found [a few](https://docs.ansible.com/ansible/latest/reference_appendices/interpreter_discovery.html) [different articles](https://maciej.lasyk.info/2016/Jun/27/working-with-virtualenv-and-ansible/) that discussed setting the environment variable on the host running Ansible, and I tried different solutions for a few hours before I called it quits for the night. When I picked up the issue again the next day with a, ahem, clearer mind, I found [this GitHub issue](https://github.com/ansible-community/molecule-docker/issues/151) that helped me find the true root cause of the problem. Tl;Dr: The interpreter issue isn't with the local host running Ansible, but is instead on the remote host that I was trying to run Ansible on (in this case, a Docker container.) After using `molecule login` and installing Python on the container, I no longer got this error. To avoid this issue happening every time I booted up a new test, I then changed the base image the test was using to the Python official [python:3.11.3-bullseye](https://hub.docker.com/layers/library/python/3.11.3-bullseye/images/sha256-cbaa654007e0c2f2e2869ae69f9e9924826872d405c02647f65f5a72b597e853?context=explore) image.
 
During these trials, I also spent about 15 minutes staring at the wall and formulating plans to uninstall Ansible, install a fresh copy of Pop! OS, and pick a new configuration management tool. I also spent another 5 minutes comparing Chef, SaltStack, and Puppet to determine my new choice, all while repeating "Ansible is dumb. It's the tool's fault, not mine. I'm gonna use a REAL config manager tool now and it'll solve these problems." Seriously - I wish I was kidding. Thankfully, cooler heads prevailed and I decided to figure out the issue.

<figure>
    <iframe src="https://giphy.com/embed/lQCV6K36nJfJYNxXbC" width="480" height="405" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/maid-regret-hit-the-wall-lQCV6K36nJfJYNxXbC">via GIPHY</a></p>
    <figcaption>My forehead callus is pretty tough these days...</caption>
</figure>

After I installed Python on the Molecule-created container, I (of course) received a new error!

```shell
fatal: [debian]: UNREACHABLE! => {"changed": false, "msg": "Failed to create temporary directory. In some cases, you may have been able to authenticate and did not have permissions on the target directory. Consider changing the remote tmp path in ansible.cfg to a path rooted in \"/tmp\", for more error information use -vvv. Failed command was: ( umask 77 && mkdir -p \"` echo ~/.ansible/tmp `\"&& mkdir \"` echo ~/.ansible/tmp/ansible-tmp-1681796130.0149798-31391-117799164586022 `\" && echo ansible-tmp-1681796130.0149798-31391-117799164586022=\"` echo ~/.ansible/tmp/ansible-tmp-1681796130.0149798-31391-117799164586022 `\" ), exited with result 1", "unreachable": true}
```

This one was a bit simpler - because I had changed the base image for the test, Molecule was expecting a different image when I started the test. After running `molecule reset` and `molecule create`, I stopped receiving this error and the standard Molecule scaffold test worked!

## Starting to Build Some Confidence
Once I was able to run the Molecule test command, I started to move my existing playbook into the Molecule `tasks/main.yml` file. Initially, I made some dumb mistakes like forgetting to quote template variables in the YAML file. Example:

```yaml
# Bad version
example_task: {new_user_name}

# Good version
example_task: '{new_user_name}'
```
I'm not the biggest fan of all of Ansible's error messages, but for the dumb-dumb mistakes like these, they were super helpful. Note that this errors out only if the template variable is the first value for a given key.

Correcting dumb syntax errors took me a few minutes, then the actual tasks could start running on the Molecule testing image. The first issue I ran into here was an error after installing `sshd` and trying to validate the `sshd_config` file: `"Missing privilege separation directory: /run/sshd\r\n"`. Searching for a solution lead me to [this GitHub issue](https://github.com/ansible/ansible-container/issues/141), which directed me to create a new directory at `/var/run/sshd` to allow OpenSSH to create and validate the new config correctly.

However, SSH configuration wasn't done throwing curveballs at me. My next attempt lead to a new error: `"System has not been booted with systemd as init system (PID 1). Can't operate.\nFailed to connect to bus: Host is down"`. More searching and Stack Overflow browsing ensued, which lead me to realize that Docker containers do not come with SystemD because of compatibility issues with the virtualization. Thankfully, the wonderful wide world of the Internet meant that someone smarter than me had already encountered this issue and solved it. I found [this StackOverflow answer](https://stackoverflow.com/questions/53383431/how-to-enable-systemd-on-dockerfile-with-ubuntu18-04) that suggested using the Python script in [this GitHub repo](https://github.com/gdraheim/docker-systemctl-replacement) to emulate `systemctl` functionality for Ansible's benefit. I added this script to my running container with the below commands:

```shell
# pull the script to the local machine
curl https://github.com/gdraheim/docker-systemctl-replacement/blob/master/files/docker/systemctl3.py > /usr/bin/systemctl
# check if the other systemctl file exists and link to new systemctl if so
test -L /bin/systemctl || ln -sf /usr/bin/systemctl /bin/systemctl
# make the script executable
chmod +x /usr/bin/systemctl
```

After these changes to the running Molecule container, the new role to configure a new Linux server worked like a charm 💯💯.

<figure>
    <iframe src="https://giphy.com/embed/xULW8JVo4V7x9aqae4" width="462" height="480" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/crazy-ex-girlfriend-ex-girlfriend-the-cw-xULW8JVo4V7x9aqae4">via GIPHY</a></p>
    <figcaption>Dancing my way to the kitchen for a victory snack.</caption>
</figure>

## What's This? An Actual Artifact?
Alright, alright, I can hear the judgement already. "But Alexander," you're whining, "if you made these changes to the container manually, they aren't going to persist and your test case is useless." Well, thank you for your very valid criticism. I also had the same thought after I finished my brief celebration.

Since I'm an amazing engineer, I had taken notes of all of the janky workarounds I implemented to make the test setup with the Molecule container work. With those notes, it was a simple ten minute task to create and build a new custom [Docker image](https://hub.docker.com/repository/docker/alesauce/debian-ansible-testing-frankenstein/general) for this task. I couldn't think of any more fitting name than [`frankenstein-container`](https://github.com/alesauce/debian-ansible-testing-frankenstein) because the Dockerfile definitely gives off the "cobbled together by a mad scientist" vibe.

Of course, since I'm such an amazing engineer I had to fuck up at least one more thing before I was done. Initially, the `CMD` in the Dockerfile was blank, so the container exited immediately after starting and I received the same `failed to create tmp directory` error as above. Thanks to all of the time spent correcting my own mistakes throughout the duration of this project, it was a simple matter to inspect the container and the logs and find the issue. After adding a new `CMD`, the role worked as expected on the ~~pile of shit~~ beautiful new Docker container. Wrapping up with a few `git push` commands and a `docker image push` left me staring at the computer screen and marveling at my own genius creating a solution to a problem solved years earlier by countless other people.

<figure>
    <iframe src="https://giphy.com/embed/3o7qDHGyWcfBPRy5lS" width="480" height="480" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/studiosoriginals-parker-jackson-ferris-bueller-3o7qDHGyWcfBPRy5lS">via GIPHY</a></p>
    <figcaption>People staring at my GitHub profile like</caption>
</figure>

My next steps from here are to make it truly cross-distro and use some more of the Ansible distro-agnostic commands instead of Debian-focused. I also have a few other playbooks from the same job interview for installing Nagios, HAProxy as a load balancer (along with a custom Nagios plugin for monitoring HAProxy), locking down a cluster with firewall rules, and creating a bastion host for securing SSH access to a cluster of servers. Since I couldn't find those roles on Ansible Galaxy when I was doing the job interview, I am going to assume that the world is crying out for them. As a man of the people, I must answer this need.

## Wrap-Up
One thing that has frustrated me about online documentation is an implicit assumption of some knowledge of a tool, framework, or paradigm. It's not always the case, and it's a hard problem to solve. After all, a lot of documentation has been written by people who are intimately familiar with a tool and have likely forgotten more about it than the average user of said tool. That frustration was a source of inspiration for choosing to write this article and record the absolute beginner/idiotic mistakes I am prone to making. On that note, hopefully, this has been useful to at least someone and I'm not just shouting into the void.

Please feel free to contact me by any of the methods on this website's homepage if you have any questions, comments, or concerns. In the meantime, I will work on getting more articles written about my next experiments. Swearzies for realzies this time, I will write more often.

<figure>
    <iframe src="https://giphy.com/embed/Lcn0yF1RcLANG" width="480" height="361" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/food-ahead-upcoming-Lcn0yF1RcLANG">via GIPHY</a></p>
    <figcaption>Realizing that hard work and persistence lead to good things.</caption>
</figure>
