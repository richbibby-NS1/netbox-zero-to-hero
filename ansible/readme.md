## Installing Ansible
Ansible runs on Linux based systems, and is installed as a Python package. Follow these steps to set up Ansible on your own system (they assume [Python](https://www.python.org/downloads/) and [Git](https://github.com/git-guides/install-git) are already installed and set up on Ansible host):

1. Clone the NetBox Zero to Hero Git repository and change into the Ansible directory:

```
git clone https://github.com/netbox-community/netbox-zero-to-hero.git
cd netbox-zero-to-hero/ansible
```

2. Create a new Python Virtual Environment and activate it:
```
python3 -m venv venv
source venv/bin/activate
```
3. Upgrade PIP (Python package manager) and install Pynetbox (NetBox API client library), Ansible and the NetBox modules for Ansible using Ansible Collections:
```
python3 -m pip install --upgrade pip
pip3 install pynetbox
pip3 install ansible
pip3 install netaddr
pip3 install pytz
ansible-galaxy collection install netbox.netbox
```
4. Set up environment variables for NetBox (these are referenced by the Ansible playbooks):
```
export NETBOX_TOKEN=< YOUR_API_TOKEN >
export NETBOX_API=< YOUR_NETBOX_URL >
```
5. When you have finished working deactivate the Python virtual environment:
```
deactivate
```
