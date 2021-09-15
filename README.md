# ansible_docker_lab

Ansible test ve çalışmalarında kullanılmak üzere local docker containerları oluşturur.

# Hazırlık

Scripte çalıştırılabilrme yetkisi verilir ve çalıştırılabilir dosyaların olduğu pathlerden birine kopyalanır.

	sudo chmod +x ansiblelab
	sudo cp ansiblelab /usr/bin

# Değişkenler

Oluşturulacak container sayısı script içerisindeki **numOfCont** değişkeni içinde tutulur. Kaç container oluşturulmak isteniyor ise değişkene o değer verilir.

Ssh bağlantısı için kullanılacak olan public keyin dosya yolu **pubKeyPath** değişkeninde tutulur. Kullanılacak keyin dosya yolu bu değişkene girilir.

Hangi docker imajından containerların create edileceği **image** değişkeninde tutulur. Default olarak **pumburo/ansibletesthost** imajı tanımlıdır. Harici bir imaj kullanılacak ise bu değişken editlenmelidir. Kullanılacak imajda openssh-server ve python yüklü olmalıdır. 

# Basit Kullanım

Containerları oluşturmak için: **ansiblelab create**

Containerları kill etmek için: **ansiblelab kill**

Containerları başlatmak için:  **ansiblelab start**

Containerları silmek için:     **ansiblelab rm**

**ansiblelab create** komutu girildikten sonra önceden belirlenmiş sayıda container ayağa kaldırılır ve belirtilen dosya yolundaki ssh key containerların root userına kopyalanır. 

Tüm containerlar ayağa kaltıktan sonra containerların alan adları ve iplerini içeren **/tmp/hostsFileForAnsible** dosyası oluşmuş olur ve dosyanın içerisindekiler ekrana basılır. Containerlara bu isimler ile erişmek için bu bilgiler **/etc/hosts** dosyası içerisine eklenmelidir.

Örnek:

	mahmut@S145:~$ ansiblelab create
	30e6cfe617c17abe9ee429ceb5044b178fc03b2b8bd66ed77a9b1176c46616e3
	dd680076d659325b4819883d7f5a2fb6254c75be7ee92696bf4052ccf78cc94b
	c4913d933776337d8cdaf9226bfc97b1d9e7c337258fab0fc871ef27a0138a4a
	81b3b98e9736660b98ce7192a543b1d5fc91a596a072c715dacae591a021a07a
	d9e37334b70de8829f8fea26536d56d4a75f5ba97b7722406238b31a9e755942
	8232f412b1907b6f905541b71965420bdcf71fdb9f2fdb86faafa88313b371ca
	8d6572506da51df5e756b54e39fc9b8b9cb9df052146d4c2af52400a913465f1
	87aa11d98257aeb032d69a1cf7cff5c5f1a64655967c5ceeb66323213201477c
	ce1c9170d5529f4e7d678fcffd3bef2162057e10bae09b1ef3bb976be6ff742f
	65a52dd167051305d0100348f8e85b390b33bacb106600a4103d44deee06155d
	########## Add these to your /etc/hosts file ##########
	172.17.0.2 ansible_test_host1
	172.17.0.3 ansible_test_host2
	172.17.0.4 ansible_test_host3
	172.17.0.5 ansible_test_host4
	172.17.0.6 ansible_test_host5
	172.17.0.7 ansible_test_host6
	172.17.0.8 ansible_test_host7
	172.17.0.9 ansible_test_host8
	172.17.0.10 ansible_test_host9
	172.17.0.11 ansible_test_host10
	mahmut@S145:~$ sudo su -
	[sudo] password for mahmut: 
	root@S145:~# cat << EOF >> /etc/hosts
	> 172.17.0.2 ansible_test_host1
	172.17.0.3 ansible_test_host2
	172.17.0.4 ansible_test_host3
	172.17.0.5 ansible_test_host4
	172.17.0.6 ansible_test_host5
	172.17.0.7 ansible_test_host6
	172.17.0.8 ansible_test_host7
	172.17.0.9 ansible_test_host8
	172.17.0.10 ansible_test_host9
	172.17.0.11 ansible_test_host10
	> EOF
	root@S145:~# exit
	mahmut@S145:~$ ssh root@ansible_test_host3
	Linux c4913d933776 5.4.0-81-generic #91~18.04.1-Ubuntu SMP Fri Jul 23 13:36:29 UTC 2021 x86_64
	The programs included with the Debian GNU/Linux system are free software;
	the exact distribution terms for each program are described in the
	individual files in /usr/share/doc/*/copyright.
	Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
	permitted by applicable law.
	root@c4913d933776:~# exit 
	logout
	Connection to ansible_test_host3 closed.
	mahmut@S145:~$ 

Hostların Ansible'ın hosts dosyasına da eklenmesi gerekmektedir. Favori text editörünüz ile **/etc/ansible/hosts** dosyasını açarak aşağıdaki örneğe benzer bir editleme yapabilirsiniz. 

	[testcluster]
	ansible_test_host1 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_test_host2 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_test_host3 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_test_host4 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_test_host5 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_test_host6 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_test_host7 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_test_host8 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_test_host9 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa
	ansible_test_host10 ansible_ssh_user=root ansible_ssh_private_key_file=/home/mahmut/.ssh/id_rsa

Bu aşamaları tamamladıktan sonra Ansible'ın ping modülü ile bağlantıyı test edebilirsiniz.

	mahmut@S145:~$ ansible testcluster -m ping
	ansible_test_host1 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_test_host4 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_test_host5 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_test_host3 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_test_host2 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_test_host6 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_test_host9 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_test_host7 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_test_host10 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}
	ansible_test_host8 | SUCCESS => {
	    "changed": false, 
	    "ping": "pong"
	}

Containerlar kill edildikten sonra arada farklı containerlar başlamış olabileceğinden start sonrasında yeni ipleri içeren yeni bir **/tmp/hostsFileForAnsible** dosyası oluşturulur. Güncel bilgiler **/etc/hosts** dosyasında da güncellenmelidir. 

	mahmut@S145:~$ ansiblelab kill
	65a52dd16705
	ce1c9170d552
	87aa11d98257
	8d6572506da5
	8232f412b190
	d9e37334b70d
	81b3b98e9736
	c4913d933776
	dd680076d659
	30e6cfe617c1
	mahmut@S145:~$ docker run -itd debian bash 
	99a7c746ed1349eae51dc9fcf3e3f21250738aa85de9255716c1d0e09791d5b1
	mahmut@S145:~$ ansiblelab start 
	65a52dd16705
	ce1c9170d552
	87aa11d98257
	8d6572506da5
	8232f412b190
	d9e37334b70d
	81b3b98e9736
	c4913d933776
	dd680076d659
	30e6cfe617c1
	########## Add these to your /etc/hosts file ##########
	172.17.0.3 ansible_test_host1
	172.17.0.4 ansible_test_host2
	172.17.0.5 ansible_test_host3
	172.17.0.6 ansible_test_host4
	172.17.0.7 ansible_test_host5
	172.17.0.8 ansible_test_host6
	172.17.0.9 ansible_test_host7
	172.17.0.10 ansible_test_host8
	172.17.0.11 ansible_test_host9
	172.17.0.12 ansible_test_host10
	
**ansiblelab rm** komutu girildikten sonra oluşturulmuş olan containerlar eğer ayakta ise önce kill edilerek, eğer değilse doğrudan silinir. 

	mahmut@S145:~$ ansiblelab rm
	65a52dd16705
	65a52dd16705
	ce1c9170d552
	ce1c9170d552
	87aa11d98257
	87aa11d98257
	8d6572506da5
	8d6572506da5
	8232f412b190
	8232f412b190
	d9e37334b70d
	d9e37334b70d
	81b3b98e9736
	81b3b98e9736
	c4913d933776
	c4913d933776
	dd680076d659
	dd680076d659
	30e6cfe617c1
	30e6cfe617c1
	mahmut@S145:~$ docker ps -a
	CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS         PORTS     NAMES
	99a7c746ed13   debian    "bash"    3 minutes ago   Up 3 minutes             pensive_driscoll

Containerları ayağa kaldırıp bağlatıyı teyit ettikten sonra hostları farklı şekilde gruplayarak ansible testlerinizi localinizde yapabilirsiniz.
