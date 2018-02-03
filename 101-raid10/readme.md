# Storage

Before we create anything, we need the fastest resilient cloud storage. Let us do RAID 10 over data disks


0. Prep 
```
export LABEL="luxor" # used rg name, vm name, dns label
export LOCATION="centralus"
export KEY_LOCATION="~/keys/zclusters"

```

1. Create resource group
```
az group create --name "${LABEL}" --location "${LOCATION}"
```

2. Create vm
```
az vm create \
	--resource-group "${LABEL}" \
	--name luxor \
	--public-ip-address-dns-name "${LABEL}" \
	--size Standard_F2s \
	--image ubuntults \
	--data-disk-sizes-gb 4056 4095 4095 4095 \
	--ssh-key-value "${KEY_LOCATION}" \
	--data-disk-caching ReadOnly
```

3. Connect to VM

```
ssh -i ~/keys/xclusters "${LABEL}"."${LOCATION}".cloudapp.azure.com
```

> The rest of the steps are running on the VM itself

4. Configure 

```
# Create array
# Data disks are usually attached at sdc++ -- It is a good idea to check yours using lsblk 
sudo mdadm --create /dev/md0 --level raid10 --name data --raid-disks 4 /dev/sdc /dev/sdd /dev/sde /dev/sdf

# Persist it
sudo mdadm --detail --scan | sudo tee -a /etc/mdadm/mdadm.conf

# mount point
sudo mkdir /opt/raid10

# Prep systemd mount
arrayDisk="$(cat /etc/mdadm/mdadm.conf | awk -F '=' '{print $4}')"

cat << EOF | sudo tee /etc/systemd/system/opt-raid10.mount
[Unit]
Description=Mount Array

[Mount]
What=/dev/disk/by-id/md-uuid-${arrayDisk}
Where=/opt/raid10
Type=ext4

[Install]
WantedBy=multi-user.target
EOF

# reload/enable/start
sudo systemctl daemon-reload
sudo systemctl enable opt-raid10.mount
sudo systemctl start opt-raid10.mount
```

5. Validate 
```
#if you have multiple arrays this won't work
arrayDisk="$(cat /etc/mdadm/mdadm.conf | awk -F '=' '{print $4}')"

# check config
sudo mdadm --detail /dev/disk/by-id/md-uuid-${arrayDisk}

# ** check the resync status (output is resounded) **
# Resync status (not rounded)
cat /proc/mdstat
```

