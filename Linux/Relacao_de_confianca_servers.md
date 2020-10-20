**RELACAO DE CONFIANCA ENTRE MAQUINAS ATRAVES DO SERVICO SSH**

Para fazer uma relação de confiança entre máquinas através do serviço SSH, que é usado para fazer acesso remoto, é necessário criar uma chave na máquina servidora para que esta possa ser usada nos clientes.

Primeiro e mais importante, é preciso que as máquinas tenham o SSH instalado. Para instalá-lo, usamos o comando:

```
# apt-get install openssh-server
```

Após ter instalado o programa, devemos criar a chave pública, é através dela que as máquinas poderão testar se há confiança para permitir acesso sem pedir senha.

Para criar a chave, precisamos estar na pasta HOME da máquina que será o servidor, caso não esteja, digite:

```
# cd ~
```

Então, usamos o comando para criar a chave:

```
# ssh-keygen -b 1024 -t dsa
```
Ou:
```
# ssh-keygen -b 1024 -t rsa
```

Pode-se usar qualquer um dos comandos. No primeiro comando é criado uma chave do tipo DSA de 1024 bits e a diferença dele pro RSA é o tipo da chave. A chave DSA é mais segura, só que um pouco mais lenta na hora da autenticação, mas a diferença é quase imperceptível.

Veja o resultado do comando:
Generating public/private dsa key pair.
Enter file in which to save the key (/home/aluno/.ssh/id_dsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/aluno/.ssh/id_dsa.
Your public key has been saved in /home/aluno/.ssh/id_dsa.pub.
The key fingerprint is:
8c:88:31:b2:49:b4:05:79:29:12:8a:6f:07:6e:27:54 aluno@lab05-008.cefet-to.org
The key's randomart image is:
+--[ DSA 1024]----+
|.+o.E            |
|=oo+             |
|=oB              |
|.B = . o         |
|o B + . S        |
| o +             |
|                 |
|                 |
|                 |
+-----------------+

Não precisa digitar nada em nenhum campo, basta pressionar ENTER.

Após ter criado a chave, precisamos copiá-la para a máquina que será o cliente.

**COPIANDO ARQUIVOS PARA O CLIENTE**
A chave recém criada encontra-se na pasta oculta ".ssh" que está na pasta home do usuário. O nome dela é "id_dsa.pub":

```
# cd .ssh
```

Agora, vamos copiar a chave pela rede, usando o programa SCP.

O scp é um comando muito útil para transferência de arquivos via console, de micro para micro:

```
# scp id_dsa.pub cliente@192.168.1.2:/home/cliente/.ssh/authorized_keys
```

Com este comando, copiamos o arquivo "id_dsa.pub" para o usuário cliente que tem o IP 192.168.1.2 [cliente@192.168.1.2], este arquivo copiado terá como destino o diretório /home/cliente/.ssh e será salvo na máquina no cliente com um nome novo: authorized_keys

* É importante que a chave tenha o nome "id_dsa.pub" no servidor e "authorized_keys" no cliente, do contrário, não haverá confiança entre as máquinas.

** Outro detalhe, essa confiança só acontecerá no momento que o servidor for acessar o cliente, e não quando o cliente for acessar o servidor.

Para finalizar, digite o seguinte comando no servidor:

```
# ssh cliente@192.168.1.2
```

Se ele se conectar sem pedir senha, significa que está tudo certo.

Esta configuração é bastante usada na criação de Clusters.

Fonte: [Viva o Linux](https://www.vivaolinux.com.br/dica/Relacao-de-confianca-entre-maquinas-atraves-do-servico-SSH)
