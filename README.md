# Gitlab Deploy


# System Requirements:

  - You need to run a Vagrantfile from Windows/Unix machine with Vagrant and Virtual box installed on it;
  - also please install vagrant plugin :
   ```
 vagrant plugin install hosts
```
  - As I use Windows machine for vagrant i configured "ansible_local" provisioner -to run ansible from guest host - You can change it on "ansible" if you plan to run it from Unix machine with ansible installed on it.
  

# Run:
Just use :
   ```
 vagrant up
```
It creates two machines - gitlab and runner;

Will install:
- ntp on both hosts;
- Gitlab and Git on gitlab host;
- Runner on runner host.

When install will be completed :
- please go to https://gitlab:4567 and change password on : "5iveL!fe";
- grab the shared-Runner token on the admin/runners page and paste it in:
/vagrant/provisioning/roles/runner/defaults/main.yml
```
gitlab_runner_registration_token: ''
```
- provision ansible:
```
vagrant provision
```
After that :
- Runner will be registered;
- default project will be created with test .gitlab-ci.yml;
- Runner automatically start to deploy all tasks with tag "deploy"
