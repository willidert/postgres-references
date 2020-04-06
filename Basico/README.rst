BÁSICO de `postgres <https://www.postgresql.org>`_
===================

Entrando no postgres
--------------------

Abra uma guia do terminal (ctrl + alt + t)
e execute o seguinte comando. Insira sua senha para acessar o postgres como root.

.. code::
    user@server:~$ sudo -i -u postgres
    [sudo] password for user:

Após isso o terminal ficara assim:

.. code::
    postgres@server:~$ psql
    
    psql (10.12 (Ubuntu 10.12-0ubuntu0.18.04.1))
    Type "help" for help.

    postgres=#

Isso indica que você acessou o postgres como root. Próximo passo, comandos básicos para começar no SGBD (Sistema Gerenciador de Banco de Dados).

* Para listar os bancos disponíveis:

.. code::
    postgres=# \l

A saída será parecida com a seguir:

.. code::

                                      List of databases
       Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
    -----------+----------+----------+-------------+-------------+-----------------------
     HA        | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
     postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
     template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
               |          |          |             |             | postgres=CTc/postgres
     template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
               |          |          |             |             | postgres=CTc/postgres
    (4 rows)

+ CREATE DATABASE/ CREATE ROLE

Logado como superuser podemos criar/retirar banco de dados e roles (users que tem permissão no banco)

.. code::
    
    postgres=# CREATE ROLE admin LOGIN ENCRYPTED PASSWORD 'password1';
    CREATE ROLE    
    postgres=# CREATE DATABASE users OWNER teste;
    CREATE DATABASE

As aspas ('') não fazem parte da senha e as vígulas no final do comando são obrigatórias

Para listar os roles (usuários)

.. code::
    postgres=# \du

A saída será parecida com a seguir:

.. code::

                                            List of roles
     Role name  |                         Attributes                         | Member of 
    ------------+------------------------------------------------------------+-----------
     postgres   | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
     teste      |                                                            | {}
    
    (2 rows)

Fique a vontade para listar os bancos disponíveis com `\l`

+ DROP DATABASE/ DROP ROLE

Use sempre que um database (banco) ou role não for mais necessário

.. code::
    postgres=# DROP ROLE teste;
    DROP ROLE
    postgres=# DROP DATABASE users;
    DROP DATABASE

Você pode usar `\du` ou `\l` para verificar o resultado

+ Create a superuser

Criar superusers pode ser necessário em alguns casos:

.. code::
    postgres=# CREATE ROLE mysuperuser WITH SUPERUSER CREATEDB CREATEROLE LOGIN ENCRYPTED PASSWORD 'mysuperpass';

+ Saindo do psql

.. code::
    postgres=# \q
    ...
    postgres@server:~$

+ Usando comandos no shell

Você pode gerenciar roles usando `createuser` e `dropuser`

.. code::
    postgres@server:~$ createuser -PE teste

    Enter password for new role:
    Enter it again:
    ...
    postgres@server:~$

A flag `-P` é usada para configurar uma senha para o usuário e `-E` para criptografar a senha.
Para criar um superuser basta usar a flag `-s`
Você pode conferir o resultado acessando o `psql` e usando `du`

.. code::
    postgres@server:~$ psql
    postgres=# \du

                                                List of roles
     Role name  |                         Attributes                         | Member of 
    ------------+------------------------------------------------------------+-----------
     postgres   | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
     teste      |                                                            | {}
    
    (2 rows)

    postgres=# \q
    postgres@server:~$ dropuser -i teste
    ...
    Role "teste" will be permanently removed.
    Are you sure? (y/n) y
    ...
    postgres@server:~$
