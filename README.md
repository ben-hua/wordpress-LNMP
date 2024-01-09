# Use Ansible to automatically install LNMP

Use Ansible to automatically install: MySQL-8, WordPress-6.4.1, Nginx-1.14, and PHP-8

## Requires

### Management side

- Ansible 2.16

### Controlled server

- Operating system: Centos stream/RHEL 8.x
- Python 3.0 (installed by default) Verify installation: `python3 -V`
- User with root privileges and public-key has been added

## How to run

### a. Local execution on the management side

1. Configure the server address to **inventory.ini**; configure the user to **remote_user** in **site.yml**

2. Add SSH key:
       Do not add passphrase to the secret key

       ```bash
       eval "$(ssh-agent -s)"
       ssh-add ~/.ssh/private-key-file
       ```

3. One-click deployment:

       ```bash
       ansible-playbook -i inventory.ini site.yml
       ```

4. After successful execution, you can access your wordpress

### b. Or GitHub Action execution

  1. Add in warehouse Actions secrets and variables
       + **SSH_PRIVATE_KEY**: your SSH private key
       + **WORDPRESS_SERVER_IP**: your server IP
       + **KNOW_HOSTS_FOR_WORDPRESS_SERVER**: know_hosts file content, [reference for obtaining method](#reference)

  2. Add a tag to Git: **v0.0.1** and submit, the script will be executed automatically
       For execution rules, see .github/workflows/deploy.yml

  3. After successful execution, you can access your wordpress

## refer to

   1. Ansible playbook reference: [ansible-examples/wordpress-nginx](https://github.com/ansible/ansible-examples/tree/master/wordpress-nginx)

       Modification points:

       + Removed selinux, iptables, firewall related configurations
       + Upgrade PHP to 8.0 and adjust the modules required for PHP-FPM [Refer to the document](https://cloud.tencent.com/document/product/213/49304)
       + Upgrade wordpress to 6.4.1, delete automatic updates and other configurations
       + ansible-lint problem modification

   2. [Ansible: **managed-node-requirements**](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#managed-node-requirements)

   3. [Ansible collections: **builtin**](<https://docs.ansible.com/ansible/latest/collections/ansible/builtin/index.html>)

   4. [How to build your inventory](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html#how-to-build-your-inventory)

   5. How to obtain the contents of the know_hosts file:

            ```bash
                  eval "$(ssh-agent -s)"
                  ssh-add ~/.ssh/private-key-file
                  ssh username@wordpress_server_ip -o UserKnownHostsFile=.ansible_known_hosts
            ```
