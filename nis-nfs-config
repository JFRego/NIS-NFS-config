#NIS and NFS server side config debiand based


- first install nfs-kernel-server;

        apt install nfs-kernel-server -y
        
- then update /etc/hosts file with your nis and nfs server ip, domain and server name;

        172.31.128.11 central.inova.pt central inova.pt
        
- next open /etc/exports and add your users home directory plus the following command;

        /mnt/raid5 *(rw,sync,no_subtree_check,no_root_squash)
        
- now create your users home directory and run the following comand to export it;

        mkdir /var/homes

        exportfs -a
        
- after that enable and start nfs-kernel-server;

        systemctl enable --now nfs-kernel-server

- now add users to the home directory you have created;

        adduser sales --home /mnt/raid5/sales
        echo xfce4-session > /mnt/raid5/sales/.xsession
        chown sales:sales /mnt/raid5/sales/.xsession

- after that install nis;

        apt -y install nis
        
- then define your server domain;

        inova.pt
        
- now open /etc/default/nis and change to master;

        NISSERVER=master
        
- open /etc/ypserv.securenets, coment or delete 0.0.0.0 0.0.0.0 and set your clients subnet plus the mask;

        255.255.255.0           172.31.128.0

- now open /var/yp/Makefile and change from false to true;

        MERGE_PASSWD=true
        MERGE_GROUP=true
        
- then run this command to update the list of host who will run nis;

        /usr/lib/yp/ypinit -m
        
- after that restart nis;

        systemctl restart nis 
        
- then go to /var/yp directory and run;

        make 
        

#NIS and NFS client side config debian base

- install nfs;

        apt install nfs-common
        
- then create your users home directory in client;

        mkdir /mnt/raid5
        
- now mount mount your users home directory in client on to your users home directory in server;

        mount -t nfs 172.31.128.11:/mnt/raid5 /mnt/raid5
        
- then copy the last line from /etc/mtab equivalent to the mount of your users directory from server and paste it in /etc/fstab adding ,nofail;
        
        172.31.128.11:/mnt/raid5 /mnt/raid5 nfs4 rw,relatime,vers=4.2,rsize=131072,wsize=131072,namlen=255,hard,proto=tcp,timeo=600,nofail,retrans=2,sec=sys,clientaddr=172.31.128.13,local_lock=none,addr=172.31.128.11 0 0
        
- after that install nis;

        apt -y install nis
        
- set your server domain;

        inova.pt
        
- now open /etc/yp.conf and set your server domain and name server;

        domain  inova.pt server central.inova.pt
        
- then open /etc/nsswitch.conf edit following lines;

        passwd:         compat nis
        group:          compat nis
        shadow:         compat nis
        gshadow:        files

        hosts:          files dns nis
        
- after that open /etc/pam.d/common-session and add;

        session optional        pam_mkhomedir.so skel=/etc/skel umask=077
        
- now restart nis;

        systemctl restart nis 
        
- and after that reboot your client instance;






