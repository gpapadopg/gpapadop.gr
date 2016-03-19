---
title: Improving performance of vagrant shared folders
tags:
    - Vagrant
    - Development
---

At first, install the nfs server package on your linux host. On ubuntu this can be done with:

	sudo apt-get install nfs-kernel-server

In the `Vagrantfile`, mount the shared folders using these options:

	type: "nfs", nfs_udp: false, mount_options: ["rw", "tcp", "nolock", "noacl", "async"]

Also, you can use `cachefilesd` on the guest machine like this:

	sudo apt-get install cachefilesd
	sudo echo "RUN=yes" > /etc/default/cachefilesd

and add the `"fsc"` option in the `mount_options` above.

If you are using the `chef` provisioner, you can install `cachefilesd` by using this recipe:

	package "cachefilesd" do
	  action :install
	end

	file "/etc/default/cachefilesd" do
	  content <<-EOF
	RUN=yes
	  EOF
	  action :create
	end