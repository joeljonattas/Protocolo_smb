# **Servidor Samba - Instalação e configuração**

Este README fornecerá um guia passo a passo para a instalação e configuração do servidor Samba em um sistema Linux. O Samba é uma implementação do protocolo SMB/CIFS, permitindo que servidores Linux compartilhem arquivos e recursos com dispositivos Windows e outros sistemas operacionais compatíveis com o protocolo SMB.

## **Pré-requisitos**

Antes de começar, verifique se você tem os seguintes pré-requisitos:

- Um sistema operacional Linux instalado (por exemplo, Ubuntu, CentOS, Debian).
- Acesso privilegiado/root no sistema para instalar pacotes e fazer configurações.

## **Passo 1: Instalação do Samba**

1. Abra o terminal em seu sistema Linux.
2. Execute o seguinte comando para atualizar os pacotes do sistema:

```bash
  sudo apt update (para sistemas baseados em Debian/Ubuntu)
```

```bash
  sudo yum update (para sistemas baseados em CentOS/RHEL)
```

3. Instale o Samba excutando os seguintes comandos:

```bash
  sudo apt install samba (para sistemas baseados em Debian/Ubuntu)
```

```bash
  sudo yum install samba (para sistemas baseados em CentOS/RHEL)
```

4. Aguarde até que o processo de instalação seja concluído.

## **Passo 2: Criando novo usuário**

1. Crie o usuário e senha para acesso ao servidor(se o servidor for público não é necessário):

- Primeiro é criado o usuário no sistema:

```bash
      sudo adduser nome_de_usuário
```

- Após criar o usuário no sistema, deve ser criado o usuário no Samba:

```bash
      sudo smbpasswd -a nome_de_usuário
```

## **Passo 3: Configurando o Compartilhamento**

1. Crie uma cópia de backup do arquivo de configuração original do Samba:

```bash
  sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.backup
```

2. Abra o arquivo de configuração do Samba com um editor de texto:

```bash
  sudo nano /etc/samba/smb.conf
```

3. Dentro do arquivo de configuração, você encontrará exemplos e diretivas comentadas. Edite-o de acordo com as necessidades do seu ambiente. Aqui estão algumas diretivas comuns:

- 'workgroup': Define o nome do grupo de trabalho (por exemplo, WORKGROUP).
- 'security': Define o nível de segurança do compartilhamento (por exemplo, user).
- 'map to guest': Define a ação a ser tomada quando um usuário não autenticado tenta acessar um compartilhamento (por exemplo, bad user).

4. No final do arquivo adicione as configurações necessárias para o compartilhamento ser realizado:

```bash
  [nome_do_compartilhamento]
        path = /caminho/para/compartilhamento
        browseable = yes
        writable = yes
        read only = no
        guest ok = no
        create mask = 0777
        directory mask = 0777
```
>Certifique-se de substituir **nome_do_compartilhamento**, **/caminho/para/compartilhamento** e **nome_do_grupo** pelos valores apropriados para o seu ambiente.

5. Salve e feche o arquivo de configuração.

## **Passo 4: Reiniciar o Serviço Samba**

1. Reinicie o serviço Samba para aplicar as configurações:

```bash
  sudo systemctl restart smbd
```
2. Verifique se o serviço Samba está em execução corretamente:

```bash
  sudo systemctl status smbd
```
> Certifique-se de que o status do serviço esteja ativo (running).

## **Passo 5: Configuração do Firewall**

Se você estiver executando um firewall no seu sistema, como o UFW, é necessário permitir o tráfego do Samba. Execute os seguintes comandos para permitir o tráfego Samba:
- Para UFW:

```bash
  sudo ufw allow Samba
```
>Certifique-se de adaptar os comandos de acordo com o seu firewall específico, se necessário.

## **Passo 6: Acesso ao Compartilhamento**

Agora que o servidor Samba está configurado, você poderá acessar o compartilhamento a partir de outros dispositivos na mesma rede. No dispositivo cliente (por exemplo, um computador Windows), abra o explorador de arquivos e insira o endereço UNC (Universal Naming Convention) para acessar o compartilhamento. O formato é **\\\endereço_ip_servidor\nome_do_compartilhamento**.

## **Considerações de Segurança**

Ao configurar o servidor Samba, é importante levar em consideração as práticas de segurança recomendadas, como:

- Definir senhas fortes para as contas de usuário do Samba.
- Limitar o acesso ao compartilhamento apenas aos usuários necessários.
- Utilizar autenticação segura, como o SMBv3, para proteger os dados transmitidos pela rede.
- Manter o sistema operacional e o software de segurança atualizados com as versões mais recentes.