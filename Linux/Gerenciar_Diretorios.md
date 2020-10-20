## Analise de tamanho de diretorio - disco cheio 
```
$ cd /   
$ du -ksm * |sort -n   
$ df   
$ cd home/   
$ du -ksm * |sort -n   
$ cd opc/ 
```

## contagem de arquivos em um diretório
```
$ ls -la|grep -e "^-"|wc -l
```

### quantos diretórios a pasta contem?
```
$ ls -la|grep -e "^d"|wc -l
```

### CONCATENADO: contagem de arquivos em um diretório + quantos diretórios a pasta contem?
```
$ alias fid=`echo "Arquivos: $(ls -la |grep "^-"|wc -l)  Diretórios: $(ls -la|grep "^d"|wc -l)"` 
```
Para não ter que fazer isto sempre, coloque o alias no seu script de inicialização e toda vez que precisar saber o número de arquivos e diretórios dentro do diretório atual, basta usar o comando `fid`.


## montar disco compartilhado  
```
$ mount 10.52.4.218:/devops_nexus /mnt/devops_nexus
$ mount <server shared>:/<diretorio shared> /<pasta para mount>
```

### montar disco compartilhado + rsync + comparctar e descompactar
```
$ sudo mkdir -p /mnt/devops_nexus 
$ sudo mount 10.52.4.218:/devops_nexus /mnt/devops_nexus 
$ rsync --delete -aupHogv /mnt/storage/* /mnt/devops_nexus_sp/ 
$ rsync -aupHogv /mnt/sas/data_ia/* root@lxcprd01osmm02:/mnt/sas/data_ia 
$ sudo cp -p /mnt/storage/bkp_nexus.tar /mnt/devops_nexus_sp/ 
$ tar -xzvf /mnt/storage/bkp_nexus.tar 
```

### Compactar diretorio: 
```
$ tar -czvf bkp_sonar.tar bkp_sonar
```

### descompactar diretório: 
```
$ tar -xzvf bkp_sonar.tar
$ sudo chown jenkins jenkins
$ sudo chgrp jenkins jenkins
$ cp -rpu /mnt/storage/* .
```
