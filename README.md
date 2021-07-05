# tf-azure-create-vm-plus-ansible

### Apply

```tf apply -auto-approve```

##Add to /etc/hosts
```
sudo echo "$(tf output -json public_ip | jq -r '.[0]') host-vm0" >>/etc/hosts
sudo echo "$(tf output -json public_ip | jq -r '.[1]') host-vm1" >>/etc/hosts
```

```ansible-playbook -i ansible/hosts ansible/update.yml --private-key ~/localpem/-id_rsa```


##Add to known_hosts
```
ssh-keyscan -H host-vm0 >> ~/.ssh/known_hosts
ssh-keyscan -H host-vm1 >> ~/.ssh/known_hosts
```

## Destroy
```
sudo sed "/host-vm0/d" /etc/hosts
sudo sed "/host-vm1/d" /etc/hosts

ssh-keygen -R host-vm0
ssh-keygen -R host-vm1
```

```tf destroy```
