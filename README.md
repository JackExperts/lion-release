# Lion

## Introdução
Lion é o componente de conexão do JumpServer para protocolos gráficos, com suporte para os protocolos RDP e VNC.

Lion é reconstruído em Golang e Vue a partir do cliente Guacamole, com seu nome inspirado no herói Lion do Dota ([Lion](https://www.dota2.com/hero/lion)).

Este repositório é principalmente para configuração e lançamento de versões.

## Configuração

Para configurações de inicialização do Lion, consulte [config_example](config_example.yml).

## Imagem Docker (Recomendado)

Você pode obter a imagem correspondente ao JumpServer de acordo com a versão. Por exemplo, para obter a imagem da versão v2.10.0:

```shell
docker pull jumpserver/lion:v2.10.0
```

Inicie o Docker:

```shell
docker run -d --name jms_lion -p 8081:8081 \
-v $(pwd)/data:/opt/lion/data \
-v $(pwd)/config.yml:/opt/lion/config.yml \
jumpserver/lion:v2.10.0
```

## Instalação Nativa

### Instalação do Servidor Guacamole
Lion é desenvolvido com base no [Apache Guacamole](http://guacamole.apache.org/). Para implantar o Lion nativamente, você precisa do [Guacamole Server](https://github.com/apache/guacamole-server) (versão >= 1.3.0).
A instalação do [Guacamole Server](https://github.com/apache/guacamole-server) pode ser consultada em [Guacamole Official Website](https://guacamole.apache.org/doc/gug/installing-guacamole.html).

### Instalação do Lion (Exemplo com v2.10.0)

Faça o download da versão correspondente do JumpServer Lion na página de releases. Baixe `lion-v2.10.0-linux-amd64.tar.gz` para o servidor e descompacte-o no diretório `/opt`.

```shell
tar -zxvf lion-v2.10.0-linux-amd64.tar.gz -C /opt/
```

Navegue até o diretório Lion com `cd /opt/lion-v2.10.0-linux-amd64`, crie um arquivo com `touch config.yml` e adicione as configurações necessárias:

```shell
CORE_HOST: http://127.0.0.1:8080 # Endereço da API do JumpServer

BOOTSTRAP_TOKEN: <PleasgeChangeSameWithJumpserver> # Chave pré-compartilhada de registro
```

Inicie o serviço:

```shell
./lion
```

#### Usando o serviço do sistema

No diretório `/etc/systemd/system`, crie um arquivo `lion-v2.10.0.service` e configure com o seguinte conteúdo:

```shell
[Unit]
Description=JumpServer Lion Service
After=network.target

[Service]
Type=simple
User=root
Group=root

WorkingDirectory=/opt/lion-v2.10.0-linux-amd64
ExecStart=/opt/lion-v2.10.0-linux-amd64/lion -f config.yml
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Recarregue o serviço do sistema:

```shell
systemctl daemon-reload
```

Inicie o serviço Lion:

```shell
systemctl start lion-v2.10.0
```

Verifique o status do serviço Lion:

```shell
systemctl status lion-v2.10.0
```

#### Observações

Se o Guacamole server estiver sendo executado com o Docker, é necessário montar o caminho de dados do Lion:

```shell
docker run -d -p 4200:4200 -v /opt/lion-v2.10.0-linux-amd64/data:/opt/lion-v2.10.0-linux-amd64/data guacamole/guacd:1.3.0
```

## Agradecimentos

Agradecimentos aos seguintes projetos pelas inspirações:
- [guacamole-client](https://github.com/apache/guacamole-client)
- [next-terminal](https://github.com/dushixiang/next-terminal)
