# tf-azure-create-vm-plus-ansible

### Apply

```tf apply -auto-approve```

####Add to /etc/hosts
```
sudo echo "$(tf output -json public_ip | jq -r '.[0]') nina-vm0" >>/etc/hosts
sudo echo "$(tf output -json public_ip | jq -r '.[1]') nina-vm1" >>/etc/hosts
```

```ansible-playbook -i ansible/hosts ansible/update.yml --private-key ~/localpem/nina-v1-id_rsa```


####Add to known_hosts
```
ssh-keyscan -H nina-vm0 >> ~/.ssh/known_hosts
ssh-keyscan -H nina-vm1 >> ~/.ssh/known_hosts
```

### Destroy
```
sudo sed "/nina-vm0/d" /etc/hosts
sudo sed "/nina-vm1/d" /etc/hosts

ssh-keygen -R nina-vm0
ssh-keygen -R nina-vm1
```

```tf destroy```
