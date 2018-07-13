# ansible

Ansible est un logiciel de déploiement/configuration à distance, créé par M. De Hann ( Redhat : Cobbler et Func) et est écrit en python. Il utilise seulement SSH et ne nécessite pas de serveur : une simple station de travail peut suffire. Il est bon de noter qu'il fonctionne en mode push (de Ansible vers le serveur cible). La machine hébergeant Ansible ne nécessite que python 2.4+, et Ansible est extensible en n'importe quel langage.

Cette solution est plutôt dédiée à un usage professionnel.


Installation

sur Ubuntu

sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible

Key SSH :

ssh-keygen
ssh-copy-id -i ~/.ssh/id_rsa.pub login@ip

nous avons normalement 2 fichiers et un dossier etc/ansible:

 hosts : c'est dans ce fichier que nous allons indiquer nos serveurs, étape obligatoire pour la suite.
 roles : ce dossier nous servira surtout par la suite quand on commencera à avoir plusieurs rôles à nos playbooks.


Inventory / hosts

[Name of server]
ip 1
ip 2

Test SSH

ping

    Avant toute chose, est ce que l'on arrive bien à joindre les serveurs :

ansible -m ping all --one-line



Installation d'une liste de paquets (playbook.yml)

---
- hosts: all
  remote_user: root

  tasks:
  - name: install common packages for all servers
    apt: 
      update_cache=yes
      state=latest
      name={{item}}
    with_items: 
    - curl
    - htop
    - ncdu
    - pwgen
    - strace
    - sudo
    - tar
    - unzip
    - vim
    - wget
    - whois
    - screen

#run

Après l'avoir enregistré, il suffira de lancer la commande suivante pour installer cette liste de paquets sur tout les serveurs :


---> ansible-playbook -i hosts playbook.yml

for check

----> ansible-playbook  --syntax-check example-playbook.yml
 
run only task 

---> sudo ansible-playbook -vvv playbook.yml --step --start-at-task=


