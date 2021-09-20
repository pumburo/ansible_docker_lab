# ansible_docker_lab

Ansible test ve çalışmalarında kullanılmak üzere local docker containerları oluşturur.

# Hazırlık

Scripte çalıştırılabilrme yetkisi verilir ve çalıştırılabilir dosyaların olduğu pathlerden birine kopyalanır.

	sudo chmod +x ansiblelab
	sudo cp ansiblelab /usr/bin

# Değişkenler

Oluşturulacak container sayısı script içerisindeki **numOfCont** değişkeni içinde tutulur. Kaç container oluşturulmak isteniyor ise değişkene o değer verilir.

Ssh bağlantısı için kullanılacak olan public keyin dosya yolu **pubKeyPath** değişkeninde tutulur. Kullanılacak keyin dosya yolu bu değişkene girilir.

Hangi docker imajlarından containerların create edileceği **imageOne** ve **imageTwo** değişkeninde tutulur. Default olarak **pumburo/ansibleubuntutesthost** ve **pumburo/ansiblecentostesthost** imajları tanımlıdır. Harici bir imaj kullanılacak ise bu değişkenler editlenmelidir. Kullanılacak imajlarda openssh-server ve python yüklü olmalıdır. 

Oluşturulacak contailerların isimleri **nameOfImageOne** ve **nameOfImageTwo** değişkenlerinde tutulur. İsimlendirme için bu değişkenler editlenmelidir. 

# Basit Kullanım

Containerları oluşturmak için: **ansiblelab create**

Containerları kill etmek için: **ansiblelab kill**

Containerları başlatmak için:  **ansiblelab start**

Containerları silmek için:     **ansiblelab rm**

**ansiblelab create** komutu girildikten sonra önceden belirlenmiş sayıda container ayağa kaldırılır ve belirtilen dosya yolundaki ssh key containerların root userına kopyalanır. 

Tüm containerlar ayağa kaltıktan sonra containerların alan adları ve iplerini içeren **/tmp/hostsFileForAnsible** dosyası oluşmuş olur ve dosyanın içerisindekiler ekrana basılır. Containerlara bu isimler ile erişmek için bu bilgiler **/etc/hosts** dosyası içerisine eklenmelidir.

Örnek:

	mahmut@S145:~$ ansiblelab create
	fdd262b60f198e8e2d5aaa3a8c9deb5bdd79a5a63cab17c3ae7d0ced38e8aeda
	557639916b06cdf7cd4fa72ca20f2f302b8611b5ff8ad49069a1439194322183
	d6705e4606a2830fb05bae6e59f22e4e7cb12a2d89ace127604552e01902c777
	0f3e62e8edc6cdc4b3e1faf84944f17e2d08576089dd72c8e1b6650c96f18313
	1a2207726cc987bd388fda0291d18ce1abb1f091bb19b9cafe485a00c8fc90b1
	e4b8f33c4bdda19fe376fba887a68747061f8a87b10e37b27f5f960e767070c7
	2b5fb3772bb63ea761b8cbf178a89d6ecf0edbf5c5eb8e0ca830bc0f0111be32
	4088cc205cff483de376088488ac68b72407366803edeb22fda773a5bb3c5289
	386b3d280b8cdbd7e959a3106e0f3924c73cd8ede8ca981cc7f87306ef7a5a57
	73921b534a80de4d64194076e504893e6c0595fa4d6c30bcdc7d423e75b79ce1
	729bddbc0bda0c3dc4d2925c2d7c57fd446b598f86d2862fc69cc7503cb5ba09
	a5a9c8a17ec90ef923f037e65cd47e8e2f525ed6f573eccbd1db6988a6cb1924
	a09b2ef288c5e96a85957c02c6d978ca81e8d9ff1cdd848b3be4bb4052706ae0
	bf7f458c12b332ed72123e2e78b2dea1bee931291c8b83f101bb600ffb46cd73
	acb7e0b5e1bf049033d535bf06bc5f7c8f95e361e428d618d639a334652e3804
	cf7e770d68e760a929149420e169679629e153c45396b4eded04fa7518b69e71
	a42a08261e323ed5015d71e0065d6cf162171cbc961ae242ebd7bd910e7f1c6b
	eb51ce3d775d4224b036cda27032e685f9e81e49789911bfc2d0f52af1c9feb9
	575d0df713557ab4ce0ad9859daae25a5a6ec2afe6db78a613d7ef2f6f501815
	950bf6555e4b6650c5a13bde71b29402bcb23fc4724eb56e44f8e8efe8b6f4be
	########## Add these to your /etc/hosts file ##########
	172.17.0.2 ansible_ubuntu_host1
	172.17.0.3 ansible_ubuntu_host2
	172.17.0.4 ansible_ubuntu_host3
	172.17.0.5 ansible_ubuntu_host4
	172.17.0.6 ansible_ubuntu_host5
	172.17.0.7 ansible_ubuntu_host6
	172.17.0.8 ansible_ubuntu_host7
	172.17.0.9 ansible_ubuntu_host8
	172.17.0.10 ansible_ubuntu_host9
	172.17.0.11 ansible_ubuntu_host10
	172.17.0.12 ansible_centos_host1
	172.17.0.13 ansible_centos_host2
	172.17.0.14 ansible_centos_host3
	172.17.0.15 ansible_centos_host4
	172.17.0.16 ansible_centos_host5
	172.17.0.17 ansible_centos_host6
	172.17.0.18 ansible_centos_host7
	172.17.0.19 ansible_centos_host8
	172.17.0.20 ansible_centos_host9
	172.17.0.21 ansible_centos_host10
	mahmut@S145:~$ sudo su -
	root@S145:~# cat << EOF >> /etc/hosts
	> 172.17.0.2 ansible_ubuntu_host1
	> 172.17.0.3 ansible_ubuntu_host2
	> 172.17.0.4 ansible_ubuntu_host3
	> 172.17.0.5 ansible_ubuntu_host4
	> 172.17.0.6 ansible_ubuntu_host5
	> 172.17.0.7 ansible_ubuntu_host6
	> 172.17.0.8 ansible_ubuntu_host7
	> 172.17.0.9 ansible_ubuntu_host8
	> 172.17.0.10 ansible_ubuntu_host9
	> 172.17.0.11 ansible_ubuntu_host10
	> 172.17.0.12 ansible_centos_host1
	> 172.17.0.13 ansible_centos_host2
	> 172.17.0.14 ansible_centos_host3
	> 172.17.0.15 ansible_centos_host4
	> 172.17.0.16 ansible_centos_host5
	> 172.17.0.17 ansible_centos_host6
	> 172.17.0.18 ansible_centos_host7
	> 172.17.0.19 ansible_centos_host8
	> 172.17.0.20 ansible_centos_host9
	> 172.17.0.21 ansible_centos_host10
	> EOF
	root@S145:~# ssh root@ansible_centos_host9
	The authenticity of host 'ansible_centos_host9 (172.17.0.20)' can't be established.
	ECDSA key fingerprint is SHA256:FzEs0+FC6QCpczdeeGJRMOl7t0imFThUBtjIDgnn0vE.
	Are you sure you want to continue connecting (yes/no)? ^C
	root@S145:~# exit 
	çıkış
	mahmut@S145:~$ ssh root@ansible_centos_host9
	Warning: Permanently added 'ansible_centos_host9,172.17.0.20' (ECDSA) to the list of known hosts.
	Last login: Mon Sep 20 12:39:19 2021 from 172.17.0.1
	[root@575d0df71355 ~]# 


Hostların Ansible'ın hosts dosyasına da eklenmesi gerekmektedir. Favori text editörünüz ile **/etc/ansible/hosts** dosyasını açarak aşağıdaki örneğe benzer bir editleme yapabilirsiniz. 

	[testcluster]
	[testcluster]
	ansible_ubuntu_host1 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_ubuntu_host2 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_ubuntu_host3 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_ubuntu_host4 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_ubuntu_host5 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_ubuntu_host6 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_ubuntu_host7 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_ubuntu_host8 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_ubuntu_host9 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_ubuntu_host10 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_centos_host1 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_centos_host2 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_centos_host3 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_centos_host4 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_centos_host5 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_centos_host6 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_centos_host7 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_centos_host8 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_centos_host9 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_centos_host10 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa 

Bu aşamaları tamamladıktan sonra Ansible'ın ping modülü ile bağlantıyı test edebilirsiniz.

	mahmut@S145:~$ ansible testcluster -m ping 
	ansible_ubuntu_host1 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_ubuntu_host5 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_ubuntu_host2 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_ubuntu_host3 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_ubuntu_host4 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_ubuntu_host6 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_ubuntu_host7 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_ubuntu_host9 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_ubuntu_host8 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_ubuntu_host10 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_centos_host1 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_centos_host2 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_centos_host5 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_centos_host4 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_centos_host3 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_centos_host6 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_centos_host7 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_centos_host8 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_centos_host10 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_centos_host9 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}


Containerlar kill edildikten sonra arada farklı containerlar başlamış olabileceğinden start sonrasında yeni ipleri içeren yeni bir **/tmp/hostsFileForAnsible** dosyası oluşturulur. Güncel bilgiler **/etc/hosts** dosyasında da güncellenmelidir. 

	mahmut@S145:~$ ansiblelab kill
	950bf6555e4b
	575d0df71355
	eb51ce3d775d
	a42a08261e32
	cf7e770d68e7
	acb7e0b5e1bf
	bf7f458c12b3
	a09b2ef288c5
	a5a9c8a17ec9
	729bddbc0bda
	73921b534a80
	386b3d280b8c
	4088cc205cff
	2b5fb3772bb6
	e4b8f33c4bdd
	1a2207726cc9
	0f3e62e8edc6
	d6705e4606a2
	557639916b06
	fdd262b60f19
	mahmut@S145:~$ docker run -itd debian bash 
	99a7c746ed1349eae51dc9fcf3e3f21250738aa85de9255716c1d0e09791d5b1
	mahmut@S145:~$ ansiblelab start
	fdd262b60f19
	557639916b06
	d6705e4606a2
	0f3e62e8edc6
	1a2207726cc9
	e4b8f33c4bdd
	2b5fb3772bb6
	4088cc205cff
	386b3d280b8c
	73921b534a80
	729bddbc0bda
	a5a9c8a17ec9
	a09b2ef288c5
	bf7f458c12b3
	acb7e0b5e1bf
	cf7e770d68e7
	a42a08261e32
	eb51ce3d775d
	575d0df71355
	950bf6555e4b
	########## Add these to your /etc/hosts file ##########
	172.17.0.2 ansible_ubuntu_host1
	172.17.0.3 ansible_ubuntu_host2
	172.17.0.4 ansible_ubuntu_host3
	172.17.0.5 ansible_ubuntu_host4
	172.17.0.6 ansible_ubuntu_host5
	172.17.0.7 ansible_ubuntu_host6
	172.17.0.8 ansible_ubuntu_host7
	172.17.0.9 ansible_ubuntu_host8
	172.17.0.10 ansible_ubuntu_host9
	172.17.0.11 ansible_ubuntu_host10
	172.17.0.12 ansible_centos_host1
	172.17.0.13 ansible_centos_host2
	172.17.0.14 ansible_centos_host3
	172.17.0.15 ansible_centos_host4
	172.17.0.16 ansible_centos_host5
	172.17.0.17 ansible_centos_host6
	172.17.0.18 ansible_centos_host7
	172.17.0.19 ansible_centos_host8
	172.17.0.20 ansible_centos_host9
	172.17.0.21 ansible_centos_host10

	
**ansiblelab rm** komutu girildikten sonra oluşturulmuş olan containerlar eğer ayakta ise önce kill edilerek, eğer değilse doğrudan silinir. 

	mahmut@S145:~$ ansiblelab rm
	950bf6555e4b
	950bf6555e4b
	575d0df71355
	575d0df71355
	eb51ce3d775d
	eb51ce3d775d
	a42a08261e32
	a42a08261e32
	cf7e770d68e7
	cf7e770d68e7
	acb7e0b5e1bf
	acb7e0b5e1bf
	bf7f458c12b3
	bf7f458c12b3
	a09b2ef288c5
	a09b2ef288c5
	a5a9c8a17ec9
	a5a9c8a17ec9
	729bddbc0bda
	729bddbc0bda
	73921b534a80
	73921b534a80
	386b3d280b8c
	386b3d280b8c
	4088cc205cff
	4088cc205cff
	2b5fb3772bb6
	2b5fb3772bb6
	e4b8f33c4bdd
	e4b8f33c4bdd
	1a2207726cc9
	1a2207726cc9
	0f3e62e8edc6
	0f3e62e8edc6
	d6705e4606a2
	d6705e4606a2
	557639916b06
	557639916b06
	fdd262b60f19
	fdd262b60f19
	mahmut@S145:~$ docker ps -a
	CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS         PORTS     NAMES
	99a7c746ed13   debian    "bash"    3 minutes ago   Up 3 minutes             pensive_driscoll

Containerları ayağa kaldırıp bağlatıyı teyit ettikten sonra hostları farklı şekilde gruplayarak ansible testlerinizi localinizde yapabilirsiniz.
