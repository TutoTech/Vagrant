# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV["LC_ALL"] = "fr_FR.UTF-8"

# Toute la configuration de Vagrant va se faire ci-dessous. Le "2" dans Vagrant.configure indique la version de la configuration. Ne le modifiez pas, sauf si vous savez ce que vous faites. ATTENTION : C'EST DU LANGAGE RUBY DONC LE RESPECT DE L'INDENTATION EST OBLIGATOIRE ! En cas de problème vous pouvez générer un autre Vagrantfile tout propre avec la commande "vagrant init" et vous pouvez consulter la documentation officielle ici : https://developer.hashicorp.com/vagrant/docs et plus particulèrement la partie sur VMware : https://developer.hashicorp.com/vagrant/docs/providers/vmware
Vagrant.configure("2") do |config|



  config.vm.box = "bento/debian-13" # Cela va utiliser une box Vagrant qui fonctionne avec VMware et recommandée officiellement par HashiCorp avec la version de Debian 13 pour créer une machine virtuelle. Pour en trouver d'autres compatibles VMware consultez le catalogue des boxes : https://app.vagrantup.com/boxes/search?provider=vmware (ATTENTION AUX BOXES MALVEILLANTES NON-OFFICIELLES !). Sur Windows, vous pouvez retrouver cette box une fois téléchargée à cet emplacement : « C:\Users\<votre_nom_d'utilisateur>\.vagrant.d\boxes\ ».
  
  config.vm.provider "vmware_workstation" do |v| # Cela va configurer les paramètres spécifiques au fournisseur de VMs (ici VMware Workstation) 
  
    v.vmx["displayName"] = "Debian 13 Server" # Cela va configurer le nom d'affichage de la machine virtuelle dans VMware Workstation.
 
    v.vmx["memsize"] = "2048" # Cela va configurer la taille de la RAM de la VM. L'application de ce paramètre peut être vérifiée avec la commande : free -h
 
    v.vmx["numvcpus"] = "2" # Cela va configurer le nombre de CPU de la VM. L'application de ce paramètre peut être vérifiée avec la commande : cat /proc/cpuinfo | grep -i "^processor" | wc -l
 
    v.gui = true # Cela va ajouter la VM et l'afficher dans l'interface graphique de VMware peu après le processus de création de la machine avec la commande vagrant up. Vous pouvez remplacer true par false pour ne pas l'afficher mais en cas de problème vous ne verrez pas ce qui se passe dans la VM.
 
  end
  
  #config.vm.synced_folder ".", "/vagrant", disabled: true # Si décommenté le paramètre "disabled: true" désactive le partage activé par défaut. Vous pouvez partager un dossier supplémentaire à la VM invitée. Le premier paramètre est un chemin vers un répertoire sur la machine hôte. Si le chemin est relatif, il est relatif à la racine du projet. La deuxième Le paramètre doit être un chemin absolu de l'endroit où partager le dossier dans la machine invitée. Ce dossier sera créé (récursivement, s'il le faut) s'il n'existe pas. Par défaut, Vagrant monte les dossiers synchronisés avec le propriétaire/groupe défini sur l'utilisateur SSH et tous les dossiers parents définis sur la racine. Et le troisième paramètre facultatif est un ensemble d'options non obligatoires. Pour en savoir plus : https://developer.hashicorp.com/vagrant/docs/synced-folders/basic_usage . 
  
  config.vm.define "host1" do |host1|
  
    host1.vm.hostname = "server.example.org" # Cela va configurer le nom d'hôte. L'application de ce paramètre peut être vérifiée avec la commande : hostname --fqdn
  
    host1.vm.base_mac = "AA:11:CC:11:08:08" # Cela va attribuer une adresse MAC à l'interface NAT (qui est l'interface ajoutée par défaut à la VM). L'application de ce paramètre peut être vérifiée avec la commande : ip link show
  
    host1.vm.base_address = "192.0.2.10" # Cela va faire une réservation DHCP pour l'adresse IP de l'interface NAT (vmnet8 par défaut) sur la machine invité. Adaptez la configuration à votre réseau virtuel NAT. Notez que l'adresse configurée doit faire partie de l'étendue DHCP autorisée. Notez que c'est une réservation DHCP et non pas une attribution d'IP fixe. L'application de ce paramètre peut être vérifiée avec la commande : ip a ; vous pouvez aussi afficher vos DNS avec : resolvectl status|grep "DNS Server" . L'option "base_mac" doit être configurée lorsque l'option "base_address" est configurée. 
  
    #host1.vm.network "private_network", ip: "198.21.100.10", :hostonly => "vmnet1" # Cela va ajouter une NIC dans le réseau host-only spécifié (en plus de la NIC dans le réseau NAT ajoutée par défaut) et lui attribuer l'adresse IP spécifiée. Adaptez la configuration à votre réseau virtuel vmnet1. 
  
    #host1.vm.network "public_network", ip: "203.0.113.10" # Cela va ajouter une NIC dans le réseau bridged et lui attribuer l'adresse IP spécifiée. Adaptez la configuration à votre réseau local.
 
    #host1.vm.network "forwarded_port", guest: 80, host: 8080 # Si décommenté cela va mettre en place un port forwarding dans les NAT Settings du Virtual Network Editor de VMware Workstation qui va permettre que l'accès à la machine hôte (localhost:8080) sur son port 8080 permet d'accéder à la machine invitée sur son port 80. REMARQUE : Cela permettra l'accès public au port ouvert. Sachez qu'un mappage de port transféré est déjà activé par défaut pour les ports 22 de l'invité et 2222 de l'hôte. L'application de ce paramètre peut être vérifiée en regardant les NAT Settings du Virtual Network Editor avec les droits d'administrateur.
  
 
  
  # Ce qui suit est un script shell de provisionnement qui va lancer une série de commandes automatiquement après la création de la VM. Chaque commande est expliquée par un echo qui sera affiché dans votre terminal : 
  host1.vm.provision "shell", inline: <<-SHELL
      # Remplacez ce script par le script bash de votre choix
      echo "DEMARRAGE DU SCRIPT DE PROVISIONNEMENT..."
 
      useradd -m -s /bin/bash user
      echo "L'UTILISATEUR user A ETE CREE."
    
      echo "user:Popopo000@@@" | sudo chpasswd
      echo "LE MOT DE PASSE DE user EST : Popopo000@@@."
 
      sudo usermod -aG sudo user
      echo "L'UTILISATEUR user A REÇU LES DROITS sudo."
    
      sudo apt-get -y update && sudo apt-get -y install open-vm-tools
      echo "LES VMWARE TOOLS ONT ETE MIS A JOUR."
 
      sudo apt-get install -y openssh-server
      echo "LE SERVICE SSH A ETE MIS A JOUR."
   
      echo "INFORMATIONS UTILES : LE MOT DE PASSE DE user EST : Popopo000@@@ ___ POUR CHANGER LE MOT DE PASSE DE user LANCEZ LA COMMANDE : sudo passwd user ___ LE MOT DE PASSE DE L'UTILISATEUR PAR DEFAUT vagrant EST : vagrant. VOUS DEVEZ LE CHANGER PAR MESURE DE SECURITE CAR IL A LES DROITS sudo. ___ NE SUPPRIMEZ ET NE RENOMMEZ PAS L'UTILISATEUR vagrant MAIS CHANGEZ PLUTOT SON MOT DE PASSE. POUR CHANGER LE MOT DE PASSE DE vagrant LANCEZ LA COMMANDE : sudo passwd vagrant ___ VOUS POUVEZ VOUS CONNECTER AVEC L'UTILISATEUR root AVEC LA COMMANDE : sudo su ___ LE CLAVIER EST CONFIGURE EN QWERTY PAR DEFAUT. POUR LE CONFIGURER EN AZERTY LANCEZ LA COMMANDE : sudo dpkg-reconfigure keyboard-configuration (TAPEZ : sudo dpkg)reconfigure keyboqrd)configurqtion SUR VOTRE CLAVIER AZERTY) ENSUITE CHOISISSEZ VOTRE CLAVIER PUIS VALIDEZ LE CHANGEMENT AVEC LA COMMANDE : sudo service keyboard-setup restart (TAPEZ : sudo service keyboqrd)setup restqrt SUR VOTRE CLAVIER AZERTY) ___ L'ACCES DISTANT PAR SSH EST POSSIBLE SUR SON PORT 22 PAR DEFAUT VIA UN TERMINAL AVEC LA COMMANDE : ssh <nom_d_utilisateur>@<adresse_ip_de_la_machine_invitee>___ VOUS POUVEZ AUSSI VOUS CONNECTER A VOTRE MACHINE DEPUIS CE TERMINAL AVEC L'UTILISATEUR vagrant PAR DEFAUT EN LANCANT LA COMMANDE : vagrant ssh"
 
      sudo reboot
 
  SHELL
  end
end
