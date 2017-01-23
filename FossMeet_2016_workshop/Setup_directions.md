##**Note: This document assumes that you are running Debian based distro.**

#**1. Set up Git**

Install git.

> sudo apt-get install git

After the installation, you need to tell git what your name and email address is, so that it can be used in the authorship information in the git commit. Create a file called .gitconfig and add lines like these to it:

> [user]
  ```
  name = Your Name
  email = your.email@example.com
  ```

#**2. Setup your Linux kernel code repository**

As we are going to work on staging directory files, I would recommand you to clone the
Greg's staging-testing directory.

> mkdir -p git/staging-next; cd git/staging-next

> git clone -b staging-testing git://git.kernel.org/pub/scm/linux/kernel/git/gregkh/staging.git

If you want to use linux-next then you can use following commands but then you need to check that your staging-testing branch is up-to date with Greg's staging-testing branch.

> mkdir -p git/linux-next; cd git/linux-next

> git clone git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git

If you have cloned mainline Linux repository, then add a remote tracking branch for linux-next:

> cd linux

> git remote add linux-next git://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git

Fetch linux-next

> git fetch linux-next


#**3. Configuring, building and compiling the kernel**

Kernel compilation takes lot of time. So, it would be better to complie it first time at home.

Duplicating current configuration

> cp /boot/config-`uname -r`* .config 

Building the kernel

> make

Installing the kernel

> sudo make modules_install install 

Now, to make the grub boot menu always appear under Ubuntu, run

> sudo vim /etc/default/grub 

And then delete following lines:

> GRUB_HIDDEN_TIMEOUT=0 GRUB_HIDDEN_TIMEOUT_QUIET=true 

Now, update the grub

> sudo update-grub2 

###**After this step reboot the machine, you should be able to boot in to your new kernel.**
Congratulations!

#**4. Installing tools**

Install vim

> sudo apt-get install vim

To send patches, you will need to be able to have a mail transport client installed:

> sudo apt-get install esmtp

If you want to send patches with mail client then install mutt:

> sudo apt-get install mutt

If you want to send patches with git send-email command:

> sudo apt-get install git-email

Install some packages:

> sudo apt-get install libncurses5-dev gcc make exuberant-ctags libssl-dev


#**5. Gmail setup** [This can be done during workshop as well]

In gmail, go click the gear icon, go to "Settings", go to the tab "Forwarding POP/IMAP", and click the "Configuration instructions" link at the very bottom of the page.

Then click "I want to set up IMAP". At the bottom of the page, under the paragraph about configuring your mail client, select "Other". Note the outgoing mail server information, and copy it into the .esmtprc file, as shown in the next section.

#**6. Setting up Vim** [extra]

We need to make sure to enable the C indentation module in our default text editor (vim). Turning on this module will ensure that lines automatically get indented to the right level as you're editing. It saves you from hitting <tab> a lot. You can turn on automatic indentation based on the file type.

> vim ~/.vimrc

And then add following lines:
> ```
filetype plugin indent on
syn on se title
set tabstop=8
set softtabstop=8
set shiftwidth=8
set noexpandtab
```

#**7. Configuring estmp and mutt** [This can be done during workshop as well]

First, create a .esmtprc file with the right permissions:

> ```
touch ~/.esmtprc
chmod g-rwx ~/.esmtprc
chmod o-rwx ~/.esmtprc
```

Edit the .esmtprc in your home directory, and add lines like this:

> ```
identity "my.email@gmail.com"
hostname smtp.gmail.com:587
username "my.email@gmail.com"
password "ThisIsNotARealPassWord"
starttls required
```

Next, set up the mail client, mutt, with some defaults, by creating a .muttrc file in your homedirectory:

> ```
set sendmail="/usr/bin/esmtp"
set envelope_from=yes
set from="Your Name <my.email@gmail.com>"
set use_from=yes
set edit_headers=yes
```

#**8. Configuring git** [This can be done during workshop as well]

If you want to use git send-email to send the patches, then you need to do the following settings in your .gitconfig file:

> ```
[user]
	email = my.email@gmail.com
	name = my name
[sendemail]
	smtpuser =my.email@gmail.com
	smtpserver = smtp.googlemail.com
	smtpencryption = tls
	smtpserverport = 587
	suppresscc = self
