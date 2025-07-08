# **Tutorial de instalação do Apache Guacamole + Gravador de sessão**

## **Introdução**
O Apache Guacamole é um gateway de desktop remoto sem cliente que suporta protocolos padrão como VNC, RDP e SSH. Ele permite que os usuários acessem seus desktops remotamente usando apenas um navegador web, sem a necessidade de softwares ou plugins adicionais. Esta ferramenta poderosa é inestimável para administradores de sistemas e profissionais de TI que precisam gerenciar e acessar múltiplos sistemas com segurança e eficiência a partir de um local central. A interface web do Guacamole garante acessibilidade a partir de qualquer dispositivo com conexão à internet, tornando o trabalho remoto e o gerenciamento de sistemas mais flexíveis e convenientes.

Etapas completas para instalar o Apache Guacamole no Ubuntu 22.04
Antes de iniciar a instalação, certifique-se de estar logado como usuário root ou não root com privilégios sudo. As etapas a seguir guiarão você por todo o processo de instalação, incluindo a configuração do acesso SSH com uma máquina virtual (VM).

### **Etapa 1 — Instalar pacotes e dependências necessários**
Atualizar os pacotes do sistema: 
```bash
sudo apt update
```
![image](https://github.com/user-attachments/assets/1b3b8b9a-b451-45b2-929b-5e251647790e)
**Em seguida, use o seguinte comando para instalar os pacotes e dependências necessários para a instalação do Guacamole:**
```bash
sudo apt install -y \
  build-essential \
  libcairo2-dev \
  libjpeg-turbo8-dev \
  libpng-dev \
  libtool-bin \
  libossp-uuid-dev \
  libvncserver-dev \
  freerdp2-dev \
  libssh2-1-dev \
  libtelnet-dev \
  libwebsockets-dev \
  libpulse-dev \
  libvorbis-dev \
  libwebp-dev \
  libssl-dev \
  libpango1.0-dev \
  libswscale-dev \
  libavcodec-dev \
  libavutil-dev \
  libavformat-dev
```
![image](https://github.com/user-attachments/assets/fafe63fa-f961-4971-8615-235a5270ad20)
**Quando terminar, prossiga para a próxima etapa para baixar e instalar o Apache Guacamole no Ubuntu 22.04.**

### **Etapa 2 — Baixe e instale o Apache Guacamole a partir da fonte (https://downloads.apache.org/guacamole/)**
**Neste ponto, você deve visitar a página oficial de downloads e usar o seguinte comando wget para baixar o pacote de origem mais recente do Apache Guacamole :**
```bash
sudo wget https://downloads.apache.org/guacamole/1.6.0/source/guacamole-server-1.6.0.tar.gz
```
![image](https://github.com/user-attachments/assets/cade8936-6dbd-4d31-a93e-0242e936a113)

Em seguida, extraia o pacote de download e navegue até o diretório do Apache Guacamole com os seguintes comandos:

![image](https://github.com/user-attachments/assets/cf627020-51ea-4a07-b49f-be5d17fbc4ed)

descompacte o arquivo tar
sudo tar -xvf guacamole-server-1.6.0.tar.gz
sudo cd guacamole-server-1.6.0

Agora você pode compilar e instalar o Apache Guacamole no Ubuntu 22.04. Para isso, execute os seguintes comandos:
sudo ./configure --with-init-dir=/etc/init.d --enable-allow-freerdp-snapshots
sudo make
sudo make install

![image](https://github.com/user-attachments/assets/11e4f11e-fa9e-4d29-b4ba-62e61bf7a2e7)
![image](https://github.com/user-attachments/assets/78c83d34-0cd9-4e59-a59b-96718f14c34c)
![image](https://github.com/user-attachments/assets/2ca14dc7-cdaf-4d0c-8ee1-43520869e129)

O processo de compilação e instalação pode levar algum tempo para ser concluído. Após a conclusão, atualize o cache da biblioteca instalada com o comando abaixo:
sudo ldconfig

Etapa 3 — Iniciar e habilitar o servidor Apache Guacamole no Ubuntu 22.04
Neste ponto, você aprendeu a compilar e instalar o Apache Guacamole no Ubuntu 22.04. Agora você precisa recarregar o systemd para aplicar as alterações:

sudo systemctl daemon-reload

Em seguida, inicie e ative o servidor Guacamole com os seguintes comandos:
sudo systemctl start guacd
sudo systemctl enable guacd

Além disso, você pode verificar se o servidor Guacamole está ativo e em execução no Ubuntu 22.04 com o seguinte comando:
sudo systemctl status guacd

Na sua saída, você deverá ver:
![image](https://github.com/user-attachments/assets/e59950b6-0f34-40c4-9f38-efb560ef1c33)

Observação : você deve criar os arquivos de configuração e extensões do Guacamole. Esses arquivos serão usados ​​nas etapas posteriores. Para fazer isso, você pode executar:
sudo mkdir -p /etc/guacamole/{extensions,lib}

Etapa 4 — Instale o aplicativo web Guacamole no Ubuntu 22.04
Neste ponto, você precisa instalar o aplicativo web Guacamole no seu Ubuntu 22.04. Ele é a interface de front-end do Apache Guacamole. Para isso, você precisa instalar o Tomcat 9 usando o comando abaixo:
sudo apt install -y tomcat9 tomcat9-admin tomcat9-common tomcat9-user

![image](https://github.com/user-attachments/assets/5263c818-d1f8-4f2a-b2a8-d1afcd901878)

Em seguida, baixe o cliente do Guacamole Web App usando o seguinte comando: Observe que você deve baixar a versão com a versão do Guacamole que você instalou .

sudo wget https://downloads.apache.org/guacamole/1.6.0/binary/guacamole-1.6.0.war
![image](https://github.com/user-attachments/assets/270798dc-3553-4eb9-9649-8aa7b3fe3cb5)

Agora você precisa mover o cliente web Guacamole para o diretório web do Tomcat. Para isso, execute o comando abaixo:
sudo mv guacamole-1.6.0.war /var/lib/tomcat9/webapps/guacamole.war

Para aplicar as alterações, reinicie os serviços Apache Guacamole e Tomcat:
sudo systemctl restart tomcat9 guacd

![image](https://github.com/user-attachments/assets/4dda0951-9814-4753-9b68-9336b167dd18)

Etapa 5 — Configurar a autenticação do banco de dados Apache Guacamole
Neste ponto, você aprendeu a instalar o Apache Guacamole no Ubuntu 22.04 e a configurar o aplicativo web cliente. Como você deve saber, o Guacamole suporta autenticação básica de usuário, usada para testes. Neste guia, queremos usar a autenticação de banco de dados pronta para produção por meio do MariaDB.

Primeiro, instale o MariaDB no Ubuntu 22.04 com o comando abaixo:
sudo apt install mariadb-server -y

![image](https://github.com/user-attachments/assets/6026deee-5dd8-45b5-b8ad-c617782dd0bf)

Em seguida, execute o seguinte comando para proteger sua instalação do MariaDB e definir uma senha para ela.
sudo mysql_secure_installation

Obs. nos prints abaixo siga as mesmas etapas de Y ou N conforme as perguntas
![image](https://github.com/user-attachments/assets/d5f7cc02-0cb5-4f51-b65d-5fdc7f3bff34)
![image](https://github.com/user-attachments/assets/0690a962-9982-445a-9be8-f7fcd42244ff)

Após concluir, você precisa instalar a biblioteca MySQL Connector/J e o plugin autenticador JDBC Guacamole. Para isso, você pode baixar o conector Java com o comando abaixo:
sudo wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-8.0.26.tar.gz

![image](https://github.com/user-attachments/assets/027d2cb6-f1cc-43b9-b6e6-dbca4b430144)

Em seguida, você deve extrair o arquivo baixado e copiá-lo para o diretório /etc/guacamole/lib/ :
sudo tar -xf mysql-connector-java-8.0.26.tar.gz
sudo cp mysql-connector-java-8.0.26/mysql-connector-java-8.0.26.jar /etc/guacamole/lib/
![image](https://github.com/user-attachments/assets/409c8690-d8ec-40c5-a330-9f2f1031d233)

Em seguida, você precisa baixar o plugin Apache Guacamole JDBC AUTH. Para isso, execute o comando abaixo:
sudo wget https://downloads.apache.org/guacamole/1.6.0/binary/guacamole-auth-jdbc-1.6.0.tar.gz

Observe que você deve baixar a versão com a versão do Guacamole que você instalou.

Agora extraia o arquivo baixado e copie-o para o diretório /etc/guacamole/extensions/:

sudo tar -xf guacamole-auth-jdbc-1.6.0.tar.gz
sudo mv guacamole-auth-jdbc-1.6.0/mysql/guacamole-auth-jdbc-mysql-1.6.0.jar /etc/guacamole/extensions/
![image](https://github.com/user-attachments/assets/9e4b4d56-73ea-4832-b14a-e4e43f02a13e)

Etapa 6 — Crie um banco de dados e um usuário do Guacamole
Neste ponto, você deve efetuar login no seu shell MariaDB e criar um usuário e um banco de dados para o Apache Guacamole no Ubuntu 22.04.

Primeiro, faça login no shell do MariaDB com a senha que você configurou:

sudo mysql -u root -p p

No shell do MariaDB, execute os comandos abaixo para criar o usuário e o banco de dados e conceder os privilégios:
(Altere guac_user e password para o usuario e senha de sua preferencia, atente-se que há dois locais para trocar o usuario no comando abaixo)

CREATE DATABASE guac_db;
CREATE USER 'guac_user'@'localhost' IDENTIFIED BY 'password';
GRANT SELECT, INSERT, UPDATE, DELETE ON guac_db.* TO 'guac_user'@'localhost';
FLUSH PRIVILEGES;

Quando terminar, saia do shell do MariaDB:
Exit
![image](https://github.com/user-attachments/assets/391b7e58-c5e1-4367-ade0-ab943c07de95)

Etapa 7 — Importar arquivos de esquema SQL e criar arquivos de propriedades para o Guacamole
Neste ponto, você deve navegar até o diretório MySQL Schema com o comando abaixo:

cd guacamole-auth-jdbc-1.6.0/mysql/schema

A partir daí, execute o comando abaixo para importar os arquivos de esquema SQL para o banco de dados MySQL:

cat *.sql | mysql -u root -p guac_db
![image](https://github.com/user-attachments/assets/4201ebf1-f3dc-4ae0-9b19-2be0127ecf0b)

Em seguida, use seu editor de texto favorito, como o editor Vi ou o editor Nano, para criar o arquivo de propriedades para o Apache Guacamole:
sudo vi /etc/guacamole/guacamole.properties

Adicione as seguintes configurações ao arquivo com suas credenciais de banco de dados:

altere mysql-username e mysql-password pelo usuario e senha que você configurou na etapa 6

# MySQL properties
mysql-hostname: 127.0.0.1
mysql-port: 3306
mysql-database: guac_db
mysql-username: guac_user
mysql-password: password
![image](https://github.com/user-attachments/assets/2167bcd1-5253-4c33-9a5e-0d043029e81e)


salve e feche o arquivo

reinicie os serviços para aplicar as alterações:
sudo systemctl restart tomcat9 guacd mysql
![image](https://github.com/user-attachments/assets/9cca47be-a36c-4f28-a400-9480290e08c6)

Etapa 8 — Acesse o Apache Guacamole pela interface da Web
Neste ponto, você concluiu as etapas para instalar o Apache Guacamole no Ubuntu 22.04. Agora você pode acessar o painel do Guacamole seguindo o URL abaixo no seu navegador:

http://ip-do-servidor:8080/guacamole
![image](https://github.com/user-attachments/assets/c70087f0-60ef-488f-b883-956a80319fc7)

Você verá a tela de login do Apache Guacamole. Insira as seguintes credenciais para efetuar login:
username: guacadmin
password: guacadmin
![image](https://github.com/user-attachments/assets/b27ddfbd-c1b5-4f33-8f92-727f4b9e6d4c)

Etapa 9 — Crie um novo usuário de administrador e senha para o Apache Guacamole
Neste ponto, é altamente recomendável criar um novo usuário e senha de administrador e excluir as credenciais padrão. Para isso, no perfil guacadmin, clique em Configurações .

![image](https://github.com/user-attachments/assets/d1d8c1c8-3996-46e2-bb6b-0e488d04be28)

Em seguida, vá para a aba Usuários e clique em Novo Usuário
![image](https://github.com/user-attachments/assets/ed374e35-ff48-4520-b008-c0bdc69aedc5)

Na seção Editar Usuário , insira seu novo nome de usuário e senha. Em seguida, na seção Permissões, marque todas as caixas. Quando terminar, clique em Salvar

![image](https://github.com/user-attachments/assets/bc97cc21-6d78-4178-80d1-af5f0196677c)
![image](https://github.com/user-attachments/assets/fde22ec1-58d8-4263-8af4-9f6c3f7b1794)

Agora, saia do usuário padrão e faça login novamente no Apache Guacamole com o novo usuário criado. Em seguida, navegue até as configurações e a aba de usuários e exclua o usuário guacadmin

Etapa 10 - Configurar a conexão a um servidor Windows (RDP) 
Clique no menu de usuario, vá para Settings
![image](https://github.com/user-attachments/assets/eaba6f02-1ba1-4427-8a32-b0f15a3b639d)

Clique em Connections e depois New Group
![image](https://github.com/user-attachments/assets/04ef54fe-fe0e-436a-b0bd-634b8bb79897)

Defina o nome do Pool conforme seu ambiente, Servidores, Windows, Linux etc... conforme a melhor segregação e salve
![image](https://github.com/user-attachments/assets/60808ad3-f1a1-44b4-a560-c32dee89550d)

Ainda em Connections clique em NJew Connection

![image](https://github.com/user-attachments/assets/cf3f3280-1a97-4bbb-831c-150d4013abf6)

Em Edit Connection insira o nome do servidor, em location selecione a pasta criada anteriormente, em protocolo insira RDP
![image](https://github.com/user-attachments/assets/48a3e3e1-6b83-401e-8761-f8a5ac05ea29)

Em Parameters > Network, insira o IP ou DNS do seu servidor, porta insira 3389 para conexão RDP

Em Authentication > Insira Usuario e senha criado no seu Windows, se estiver em dominio deve preencher
Em Authentication > Em Security Mode selecione Any ou NLA e marque a caixa Ignore Server Certificate caso não possua certificado
![image](https://github.com/user-attachments/assets/f243a187-5613-4de8-b083-78ab9b763ed1)

Etapa 11 - Configurando gravador de sessão remota

Ainda nas configurações de Conexões
Em Screen Recording :

Recording Path: ${HISTORY_PATH}
Recording Name: ${HISTORY_UUID}

Selecione a caixinha de Automaticaly create recording path

![image](https://github.com/user-attachments/assets/4cc6579d-3ad7-4a58-9a39-9072606be917)

Clique em Save
![image](https://github.com/user-attachments/assets/2bcb9ae7-57fb-479a-9921-7e2deb9cc2d1)


Baixe o guacamole-history-recording-storage-1.6.0.tar.gz (https://apache.org/dyn/closer.lua/guacamole/1.6.0/binary/guacamole-history-recording-storage-1.6.0.tar.gz?action=download) (https://guacamole.apache.org/doc/gug/recording-playback.html)

Extraia e envie o arquivo para /etc/guacamole/extensions/
No meu caso fiz utilizando WinSCP para transferencia de arquivos

![image](https://github.com/user-attachments/assets/d80e9fea-84e5-48a3-8eb3-5b352d8324d7)

Criar o diretorio onde hospedará a gravação:
sudo mkdir -p /var/lib/guacamole/recordings

Verifique se o usuario guacd exite
id guacd

Se não existir, criaremos um usuario de serviço
sudo useradd -r -s /bin/false guacd

Mudar o owner do diretorio para daemon:tomcat

sudo chown -R daemon:tomcat /var/lib/guacamole/recordings
sudo chmod -R 770 /var/lib/guacamole/recordings

Adicionar o path de gravação no arquivo guacamole.properties
 cd /etc/guacamole/
 nano guacamole.properties

Adicione a linha:
recording-search-path: /var/lib/guacamole/recordings

![image](https://github.com/user-attachments/assets/a1f22c29-f1a2-425e-8c96-d4626e5c4e9e)


Reinicie os serviços:
sudo systemctl restart guacd
sudo systemctl restart tomcat9

Pronto, os proximos passos é testar e desfrutar!

Faça uma sessão de teste, (em Home)
![image](https://github.com/user-attachments/assets/db6b9859-c437-43c0-84da-c8618e701d6c)

![image](https://github.com/user-attachments/assets/611eb1f4-a9f5-41b8-825f-09272a2356f2)

Após, vá em Settings
![image](https://github.com/user-attachments/assets/a32a82bd-24fb-4901-a0fd-87bf7f226071)

Em History > Coluna Logs, clique em View para visualizar a gravação da sessão

![image](https://github.com/user-attachments/assets/c4c12343-c4da-4ad5-b08a-fc9112f5488e)


Fim!

