# desk
## Historique amélioré ##
export HISTCONTROL=ignoreboth
export HISTIGNORE="cd:ls:[bf]g:clear"

######### Alias #########
alias cp="cp -i"
alias mv="mv -i"
alias rm="rm -i"
alias md="mkdir"
alias rd="rmdir"
alias cd..="cd .."
alias ssh='ssh -l root'
alias processeur="cat /proc/cpuinfo"
alias mkdir="mkdir -p"
alias rezo="sudo watch netstat -alpe --ip"
alias update="sudo apt-get update"
alias upgrade="sudo apt-get update && sudo apt-get upgrade"
alias addppa="sudo add-apt-repository"
#alias ssh1="ssh root@localhost -p 2222"
#alias ssh2="ssh root@localhost -p 6666"
alias miche="export OPSCODE_USER="
alias vv="./hooks/pre-commit --ci"
alias kn="knife node show"
alias pg='ps ax | grep'


#############   Kvm   #################
  ssh root@localhost -p 2222    ==> ssh1

#############   Chef    ################

  // On utilise le serveur marmiton il faut donc l'indiquer a knife
  echo $OPSCODE_USER

  // On upload notre cookbook
  knife cookbook upload rebond

  // Sur le serveur on peut executer la commande chef
  chef -r "recipe[pci::1_partitions]"

  // Ajouter un role à un node
  knife node run_list add NomDuNode 'role[nomDuRole]'
  knife node run_list add LIN0093393-d8274-test-6x-01 'role[diffusion]'

  // Rechercher avec knife
  knife search node 'recipes:*nrpe*'
  knife search node '*:*' -a environment | grep 'production' |wc -l
  knife search node "chef_environment:production"
  knife status "chef_environment:recette" -H

  // Lancer chef sans appliquer les modifications
  chef -W -r "recipe[pci::server_pci]" > resume

  // Lancer Chef
  service chef-client restart;tail -f /var/log/chef/client.log

  script -c "/opt/chef/bin/chef-client -c /opt/chef/etc/client.rb --once -W -r "recipe[]""

########### Iptables  ##########

iptables -A INPUT -p tcp -i eth0 --dport ssh -j ACCEPT
/sbin/service iptables save
/sbin/service iptables restart

######### Generate password ############
apg -E @$;&'"" -n 1 -a 0 -l -t -m 15 -x 20 -M NCLS


################ LVM ################
// Visualiser les volume group
vgdisplay
pvs
// Visualiser un logical volume
lvdisplay /dev/mapper/vg_root-lv_var
// Agrandir un lv
lvresize -L +2G /dev/mapper/vg_root-lv_var
lvdisplay /dev/mapper/vg_root-lv_var
// Agrandir la partition dans le lv
resize2fs /dev/mapper/vg_root-lv_var
// Displays the amount of disk space available on the file system
df -h

#######################" GPG " #################"

gpg --homedir /home/tom/.gpg/ --gen-key

gpg --list-key

gpg --homedir /home/tom/.gpg/ --output qg.gpg --armor --export AAAAAA

#####################################################

// Monter /var/log sur /mnt
mount -t ext4 /dev/mapper/vg_root-lv_varlog /mnt
// Synchroniser le /var/log/ avec /mnt
rsync -ar /var/log/ /mnt/
echo "/dev/mapper/vg_root-lv_varlog /var/log ext4 defaults 1 2" >> /etc/fstab


mount -t ext4 /dev/mapper/vg_root-lv_test /mnt/
rsync -ar /var/log/test/ /mnt/
umount /mnt/
echo "/dev/mapper/vg_root-lv_test /var/log/test ext4 defaults 1 2" >> /etc/fstab

mount -o remount,nodev,noexec,nosuid /tmp

sysctl vm.swappiness
vm.swappiness = 60


ethtool -s eth0 speed 100 duplex full autoneg off
solrtux-rec


####### TESTER LES DNS #########
nslookup machin.test.com 10.10.1.1


########### KILL FORCE  ##########
kill -9 "pid"

########### Desinstaller sous debian ###########
sudo apt-get remove --purge le nomDuPaquet*
sudo apt-get clean
sudo apt-get autoremove

##### Contributing #####

    Fork the repository.
    Create a branch (git checkout -b my_branch)
    Commit your changes (git commit -am "Major fixup.")
    Push to the develop branch (git push develop my_branch)
    Open a Pull Request
    Enjoy a refreshing Diet Coke and wait :+1:

########## OPENSTACK ##########

/usr/lib/monitoring/plugins/sfl/check_keystone -U http://10.1.2.12:5000/v2.0 -u admin -p password -t admin -s

## RECUPERATION TOKEN
curl -i http://10.1.2.12:35357/v2.0/tokens -X POST -H "Content-Type: application/json" -H "User-Agent: python-neutronclient" -d'{"auth":{"passwordCredentials":{"username": "admin", "password": "password"}}}'

curl -i http://10.1.2.12:35357/v2.0/tokens -X POST -H "Content-Type: application/json" -H "User-Agent: python-keystoneclient" -d'{"auth":{"passwordCredentials":{"username": "admin", "password": "password"}}}'

# UTILISATION TOKEN OpenStack
curl -i http://10.1.2.12:9696/v2.0/networks -X GET -H "X-Auth-Token: b6ab6be17fd441d498f49d05422f4e26" -H "Content-Type: application/json" -H "Accept: application/json" -H "User-Agent: python-neutronclient"

######## YUM ##########

// Lister les differentes version de paquets
yum --showduplicates list httpd | expandyum --showduplicates list httpd | expand

// Mettre à jour les paquets
yum clean all
yum list recent
yum update --disablerepo=sl-arkea

Renault : tous les jours un bruit nouv0

########## ZOOKEEPER ##########

# Tester Zookeeper
/opt/zookeeper/bin/zkCli.sh -server 10.0.0.2:2181
# Zookeeper doit répondre imok :
echo ruok | nc opsklctl1-v 2181
# Statistiques :
echo stat | nc opsklctl1-p 2181

########## KAFKA ##########

// Lister les topics Kafka
/opt/kafka/bin/kafka-topics.sh --list --zookeeper 192.168.20.5:2181

// Tester Kafka : Publish the string "Hello, World" to a topic called TutorialTopic
echo "Hello, World" | /opt/kafka/bin/kafka-console-producer.sh --broker-list 10.19.109.197:9092 --topic TutorialTopic > /dev/null

// And consumes messages from the topic we published to
/opt/kafka/bin/kafka-console-consumer.sh --zookeeper 10.19.109.197:2181 --topic TutorialTopic --from-beginning

########## STORM ##########

// Run an example Storm topology that comes packed with the Storm installation
/opt/storm/bin/storm jar /opt/storm/examples/storm-starter/storm-starter-topologies-0.9.6.jar storm.starter.ExclamationTopology exclamation-topology

// List the different topologies
/opt/storm/bin/storm list

########## INFLUXDB ##########

SHOW TAG KEYS FROM "disk.space_used_perc"

show series  where  _tenant_id='34215c2a10db44ccbc0218d6cfda4797'  and _region='region-testv'   limit 10001  offset 0

########## ADMIN SYS ##########

//systeme
vmstat -w

// Rechercher dans le systeme
grep -rn "It works" /var/www/

grep -rI --exclude-dir=proc --exclude-dir=sys "" *

find REPERTOIRE -type f -exec ls -altr {} \+

cat /etc/redhat-release

// Active connection et socket ouverts
netstat -anp |less

// IO de la machine
iostat

// Memoire
free -m
process=
ps aux | grep $process | grep -v grep | awk 'BEGIN { sum=0 } {sum=sum+$6; } END {printf("Taille RAM utilisée: %s Mo\n",sum / 1024)}'

// Taille d'un repertoire
du --max-depth=1 -a -h /usr | sort -n -r | head -n 20
du -h --max-depth=1 | sort -hr

// Untar
alias untar="tar -xzvf"
tar -zxvf

// Change shell
chsh -s /bin/bash username

// Change home diretory
usermod -m -d /newhome/username username


########## Paquets RPM ##########

// Turn off strip'ng of binaries
%global __os_install_post %{nil}
%global __strip /bin/true


#cloud-config
hostname: test1
fqdn: test1.com

Utiliser des logiciels propriétaires, c'est comme les plats préparés,
on est incapable de dire les conservateurs qu'ils contiennent,
on dira toujours que c'est bon, mais ça ne remplacera jamais le repas fait maison par sa maman.

Linux : y a moins bien, mais c'est plus cher

servers = cloud.nova_client.servers.list(search_opts={'status': 'DELETED', 'all_tenants': 1})
