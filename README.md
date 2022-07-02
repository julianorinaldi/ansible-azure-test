Repositório usado para aprimorar conhecimento em Ansible
- Documentação:
  - [docs.ansible.com](https://docs.ansible.com/ansible/latest/index.html)
  - ansible-doc -l
___
## Como instalar instalar
Para executar o ansible, é necessário estar instalado nas máquinas target e na sua máquina de execução.

- Ubuntu:
  - sudo apt update
  - sudo apt install ansible
  - Caso fique com warning sobre o python, pode-se configurar no arquivo `hosts` a variável:
    ```
    [all:vars]
    ansible_python_interpreter=/usr/bin/python
    ```
- Python:
  - sudo pip install ansible

## Config hosts
- Em `/etc/ansible/hosts` ficam as máquinas regristradas para uso do Ansible.
- Podemos usar inclusive grupos de máquina como:
```
[servers]
20.108.252.225
20.106.124.115
20.108.255.26
```

## Config ansible cfg
- Em `/etc/ansible/ansible.cfg` fica arquivo responsável por todas as configurações do Ansible.
- Aqui vamos alterar apenas o parâmetro `private_key_file` que é o endereço onde fica as chaves ssh da máquina.

## Ansible setup - informações do host
- Tem objetivo de trazer todas as informações dos host, relacionadas aos sistemas.
```
ansible servers -m setup
```
- É possível passar filtro
```
ansible servers -m setup -a "filter=ansible_distribution"
```
  - `-a`: permite passar argumento
  - `filter=ansible_distribution`: filtra uma chave do resultado do `setup`

## Módulos ansible
- shell:
  - Executa comandos do Shell
  ```
  ansible servers -m shell -a "cat /etc/hosts"
  ```
- apt
  - Instala pacotes
  ```
  ansible servers -m apt -a "name=wget state=present update_cache=yes" --become
  ```
    - `name=wget`: nome do pacote a instalar
    - `update_cache`: informa que precisa de atualização da lista de pacotes
    - `--become`: leva os privilégios de usúario para instalação
    - `--ask-become-pass`: usado quando o usuário não possui privilégios para execução.

## Trabalhando com plybooks
- São arquivos `.yml` com instruções para executar tarefas.
- Execução
```
ansible-playbook playbooks/transferir_arquivos.yml
```
![](https://i.imgur.com/SkVtslc.png)
- Lembrando que o ansible trabalha com **idempotência**, que significa uma característica que poderá executar quantas vezes quiser o script e terá mesmo resultado.
![](https://i.imgur.com/hHyHLbi.png)
- Verificando as alterações:
```
ansible all -m shell -a "cat /etc/resolv.conf"
```
![](https://i.imgur.com/JRdt2PJ.png)

## Plybooks - Docker install
- No exemplo usamos módulo shell para instalar docker.
```
ansible-playbook playbooks/install_docker.yml
```