# Tutorial de Instalação do MaaS da Canonical

Bem-vindo ao tutorial de instalação do MaaS (Metal as a Service) da Canonical! Neste guia passo a passo, você aprenderá como instalar o MaaS em seu sistema para gerenciar e provisionar servidores físicos.

## O que é o MaaS?

O MaaS é uma solução de gerenciamento de infraestrutura que permite provisionar, monitorar e gerenciar servidores físicos em larga escala, facilitando o processo de implantação de serviços em nuvem e data centers.

## Pré-requisitos

Antes de começar, verifique se seu sistema atende aos seguintes pré-requisitos:

- Sistema operacional Ubuntu 18.04 ou superior (ou outra distribuição Ubuntu suportada).
- Pelo menos 8 GB de RAM e 50 GB de espaço em disco.
- Acesso de superusuário (ou sudo) para instalação de pacotes.

## Instalação

### Passo 1: Baixando o pacote de instalação

Para começar, baixe o pacote de instalação do MaaS a partir do repositório oficial. Abra um terminal e execute o seguinte comando:

```bash
sudo apt update
sudo snap install --channel=latest/edge maas
```

### Passo 2: Instalando dependências - Banco de dados PostgreSQL

O MaaS precisará de um banco de dados, neste tutorial utilizaremos o PostegreSQL. Para fazer sua instalação abra um terminal e execute o seguinte comando:

```bash
sudo apt update
sudo apt install postgresql
```

### Passo 3: Configurando o banco de dados PostgreSQL

Criaremos um banco de dados para o MaaS, um usuário no PostgreSQL e garantiremos o acesso total deste usuário ao banco recém criado. Para isso, abra um terminal e execute o seguinte comando:

```bash
create user 'nome_do_usuário' with encrypted password 'senha_do_usuário';
create database maas;
grant all privileges on database maas to 'nome_do_usuário';
\q 
```
Substitua 'nome_do_usuário' e 'senha_do_usuário' pelo nome e senha desejada (não coloque as aspas).

Agora editaremos o arquivo de configuração do PostgreSQL <b>pg_hba.conf</b>. Este arqivo encontra-se na pasta de configuração do PostgreSQL e seu endereço pode variar de acordo com a versão instalada do PostgreSQL no seu sistema. Neste tutorial utilizamos a versão <b>14</b>, logo a pasta de configuração onde o arquivo se encontra está em: /etc/postgresql/<b>14</b>/main.

Incluiremos no final do arquivo a seguinte linha de informações:

```bash
host    maas    "nome_do_usuário"    0/0     md5
```
Substitua 'nome_do_usuário' pelo nome desejado (não coloque as aspas).

### Passo 4: Integrando o MaaS ao banco de dados PostgreSQL

Integraremos o MaaS ao Postegres utilizando o seguinte comando:

```bash
sudo maas init region+rack --database-uri "postgres://"nome_do_usuário":"senha_do_usuário"@localhost/maas"
```
Substitua 'nome_do_usuário' e 'senha_do_usuário' pelo nome e senha desejada (não coloque as aspas).

Será solicitada a confirmação da URL de acesso ao sistema, neste ponto, basta pressionar a tecla Enter.

```bash
MAAS URL [default=http://ip_do_servidor_maas:5240/MAAS]:
```

### Passo 5: Criando o usuário admin para acessar o Dashboard do MaaS

Para criarmos o usuário admin e termos acesso ao Dashboard do MaaS utilizaremos o seguinte comando:

```bash
sudo maas createadmin
```
Será solicitado o nome do usuário, senha, confirmação de senha e e-mail. Todos os campos devem ser preenchidos.

### Passo 6: Acessando o Dashboard do MaaS

Abra um navegador e digite o endereço de ip do compudor onde o serviço está instalado seguido da porta do serviço que é a <b>5240</b> e do diretório do dashboard que é o <b>/MAAS</b>.

<b>Exemplo:</b> http://192.168.0.100:5240/MAAS

![image](https://github.com/zeraimundo/MaaS/assets/82219488/f2375384-5c8f-462b-959e-80cb8104f907)

Aqui você utilizará o usuário e senha criados no <b>Passo 5</b>.

Parabéns! Você concluiu a instalação e configuração do MaaS. Agora, seu serviço MaaS está pronto para ser configurado em seu servidor.

Ajuda e Suporte
Em caso de problemas durante a instalação ou uso do MaaS, consulte a <a href="https://maas.io/docs">documentação oficial do MaaS</a> ou visite o <a href="https://github.com/maas/maas">repositório do MaaS no GitHub</a> para obter suporte.

Contribuição
Se você deseja contribuir para este tutorial ou relatar problemas, sinta-se à vontade para abrir um problema ou enviar um pull request no repositório do GitHub.
