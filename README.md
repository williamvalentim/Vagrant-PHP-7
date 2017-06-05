# README #

Ambiente de desenvolvimento com PHP 7 e extensões já configuradas para conectar do principais banco de dados

### Como usar ###

Instale o Vagrant
Instale o Virtualbox
Clone o repositório
Dentro da pasta onde clonou o repositorio rode o comando
	vagrant up --provider virtualbox

### Para adicionar seus projetos
No arquivo Vagrantfile procure o trecho onde estão as entradas config.vm.synced_folder e substitua pelos paths dos seus projetos ou workspace
	config.vm.synced_folder "PATH DO PROJETO NO SEU COMPUTADOR", "PATH DO PROJETO NO SERVIDOR VIRTUAL", create: true, type: "nfs"

todos os virtuais hosts são configurados no arquivo VirtualHosts.conf

Para recarregar as alterações no arquivo Vagrantfile é necessário desligar o servidor e ligar novamente com os comandos abaixo
	vagrant halt # para desligar
	vargant up # para ligar

### How do I get set up? ###

* PHP 7.0
* Apache 2.4
* Mysql
* Sql Lite 3
* SQL Server
* Oracle
* CakePHP 3
