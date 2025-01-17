# TP3 Admin : Vagrant

## 1. Une première VM

🌞 Générer un Vagrantfile

```
PS C:\Users\gogob_jaotmdf\TP_Linux_B2\tp 3> vagrant init generic/ubuntu2204 --box-version 4.3.12
```

🌞 Modifier le Vagrantfile

```Vagrantfile
PS C:\Users\gogob_jaotmdf\TP_Linux_B2\tp 3> cat .\Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2204"
  config.vm.box_version = "4.3.12"
  config.vm.box_check_update = false
  config.vm.synced_folder ".", "/vagrant", disabled: true
end
```

# 2. Repackaging

🌞 Ecrivez un Vagrantfile qui lance une VM à partir de votre Box

```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "super_box"
  config.vm.box_version = "0"
  config.vm.box_check_update = false 
  config.vm.synced_folder ".", "/vagrant", disabled: true
end
```



