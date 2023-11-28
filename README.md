# 使用Ansible自动部署LNMP

使用Ansible自动化部署 MySQL-8, WordPress-6.4.1, Nginx-1.14, 和 PHP-8

## Requires

### 管理端

- Ansible 2.16

### 被控制的服务器

- 操作系统：Centos stream/RHEL 8.x
- Python 3.0 (默认已安装) 验证是否安装： `python3 -V`
- 具有root权限的用户，且已添加public-key

## 如何运行

### a. 管理端本地执行

1. 配置服务器地址到 **inventory.ini**； 配置用户到 **site.yml** 中的 **remote_user**

2. 添加SSH秘钥：

      ```bash
      eval "$(ssh-agent -s)"
      ssh-add ~/.ssh/private-key-file
      ```

3. 一键部署：

      ```bash
      ansible-playbook -i inventory.ini site.yml
      ```

4. 执行成功后，就可以访问你的wordpress了

### b. 或者GitHub Action 执行

 通过Github action 自动部署。

## 参考

  1. Ansible playbook 参照：[ansible-examples/wordpress-nginx](https://github.com/ansible/ansible-examples/tree/master/wordpress-nginx)

      修改点:

      + 删除了 selinux，iptables，firewall 相关配置
      + PHP 升级到8.0，调整 PHP-FPM 所需的模块[参照文档](https://cloud.tencent.com/document/product/213/49304)
      + wordpress 升级到6.4.1，删除自动更新等配置
      + ansible-lint 问题修改

  2. [Ansible: **managed-node-requirements**](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#managed-node-requirements)

  3. [Ansible collections: **builtin**](<https://docs.ansible.com/ansible/latest/collections/ansible/builtin/index.html>)

  4. [How to build your inventory](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html#how-to-build-your-inventory)
