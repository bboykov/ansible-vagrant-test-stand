# Test ansible with vagrant

Test template for ansible roles.


## How it works

`bash setup-requirements.sh` will download roles described in `requirements.yml`.

`vagrant up` will start vagrant box described in `vagrantvms.yml`.

`depl_play.yml` can be used to test ansible deployment. Example: `ansible-playbook depl_play.yml`

`prov_play.yml` is used by vagrant to provision with ansible right after the vagrant box is up.
