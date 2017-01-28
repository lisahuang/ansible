This is a guide to setup ansible step by step


1. Set up environment
pip install ansible
#pip install bigsuds
virtualenv env
source env/bin/activate

2. check ansible version
(env) SFO1502639594:1st 502639594$ ansible --version
ansible 2.2.1.0
  config file =
  configured module search path = Default w/o overrides

3. create a hosts file and test with ping

file <<hosts>>:
[sandbox]
10.1.226.55

>>ansible all -i hosts -m ping -u lisa.huang --ask-pass

$ ansible all -i ../hosts -m ping -u lisa.huang --ask-pass
SSH password:
10.1.226.55 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}

4. create a file locally

502639594$ more local.yaml
- name: My first play
  hosts: localhost
  connection: local
  gather_facts: true

  tasks:
      - name: Create a file
        copy:
            dest: "/tmp/test.txt"
            content: "This is some content”

>>ansible-playbook -i hosts local.yaml -vvvv

5. create a file remotely
(env) SFO1502639594:1st 502639594$ more jumpbox.yaml
  hosts: jumpbox
  connection: ssh
  gather_facts: true

  tasks:
      - name: Create a file
        copy:
            dest: "/tmp/test.txt"
            content: "This is some content"

>> ansible-playbook jumpbox.yaml -i ../jumpbox -u lisa.huang -c ssh -vvvv

Common issues:
1. for mac if ask for sshpass:
brew install https://raw.githubusercontent.com/kadwanev/bigboybrew/master/Library/Formula/sshpass.rb


2. ansible 2.2.1 needs vars in hosts file
(env) SFO1502639594:1st 502639594$ more ../jumpbox
[jumpbox]
10.1.36.120

[jumpbox:vars]
ansible_password=password




