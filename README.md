# ESP32 RPC(Remote Procedure Call)

Comunicação remota utilizando RPC e ESP32.

## Introdução

Esse projeto tem como objetivo estudar e explorar técnicas utilizando RPC como meio de comunicação em cima do ESP32.

Essa vontade de implementar algo surgiu a algum tempo atrás quando testei o Mongoose-os no ESP32, onde esse meio de comunicação era disponível através do binário(mos) do serviço como também na dashboard do mesmo.

Portanto resolvi pesquisar um pouco mais sobre o tema a fim de curiosidade e como de costume resolvi fazer algumas implementações simples porém com um contexto que case com a vida real.

## RPC

Chamada remota de procedimento (RPC, acrônimo de Remote Procedure Call) é uma tecnologia de comunicação entre processos que permite a um programa de computador chamar um procedimento em outro espaço de endereçamento (geralmente em outro computador, conectado por uma rede). O programador não se preocupa com detalhes de implementação dessa interação remota: do ponto de vista do código, a chamada se assemelha a chamadas de procedimentos locais.

RPC é uma tecnologia popular para a implementação do modelo cliente-servidor de computação distribuída. Uma chamada de procedimento remoto é iniciada pelo cliente enviando uma mensagem para um servidor remoto para executar um procedimento específico. Uma resposta é retornada ao cliente. Uma diferença importante entre chamadas de procedimento remotas e chamadas de procedimento locais é que, no primeiro caso, a chamada pode falhar por problemas da rede. Nesse caso, não há nem mesmo garantia de que o procedimento foi invocado.

Fonte: [Wikédia](https://pt.wikipedia.org/wiki/Chamada_de_procedimento_remoto)

## Dashboard - webapp

Assim que possível vou liberar o webapp no repositório citado abaixo para que fique mais fácil ver as coisas fluindo.

[Repositório](https://github.com/douglaszuqueto/esp32-ota)

![img](https://raw.githubusercontent.com/douglaszuqueto/esp32-rpc/master/.github/webapp-v1.png)

## Comandos

    * ESP
        * Info
        * Reboot
    * Wifi
        * Info
        * SetCredentials
    * Led
        * Write
        * State
        * Toggle
    * OTA (não implementado)
        * Update
        * Commit
        * Rollback
    * Config
        * Set
        * Get
    * Log
        * All
        * Get

## Arquitetura

A fins de estudo, este projeto terá suporte a 2 protocolos que uso no dia a dia, são eles: HTTP e MQTT

Toda requisição para um método RPC segue no formato JSON(assim como a resposta também).

* Request
```bash
{
  "method": "Nome.Metodo",
  "params": {
    "key": "value"
  }
}
```

* Response
```bash
{
  "method": "Nome.Metodo",
  "data": {
    "key": "value"
  }
}
```
### HTTP

![img](https://raw.githubusercontent.com/douglaszuqueto/esp32-rpc/master/.github/esp32-rpc-http-v1.png)

#### Firmware
[Repositório](https://github.com/douglaszuqueto/esp32-rpc-http)

#### Comandos

* ESP.info
```bash
curl --request POST \
  --url http://192.168.0.103:8080/rpc \
  --header 'content-type: application/json' \
  --data '{
	"method": "ESP.Info",
	"params": {}
}'
```

* Wifi.SetCredentials
```bash
curl --request POST \
  --url http://192.168.0.103:8080/rpc \
  --header 'content-type: application/json' \
  --data '{
	"method": "Wifi.SetCredentials",
	"params": {
		"ssid": "YOUR_SSID",
		"password": "YOUR_PASSWORD"
	}
}'
```

* Led.Toggle
```bash
curl --request POST \
  --url http://192.168.0.103:8080/rpc \
  --header 'content-type: application/json' \
  --data '{
	"method": "Led.Toggle",
	"params": {}
}'
```

### MQTT

![img](https://raw.githubusercontent.com/douglaszuqueto/esp32-rpc/master/.github/esp32-rpc-mqtt-v1.png)

#### Firmware
[Repositório](https://github.com/douglaszuqueto/esp32-rpc-mqtt)

#### Comandos

* ESP.Info
```bash
mosquitto_pub -h 192.168.0.150 -t esp32/rpc/request -m '{"method":"ESP.Info","params":{}}'
```

* Wifi.SetCredentials
```bash
mosquitto_pub -h 192.168.0.150 -t esp32/rpc/request -m '{"method":"Wifi.SetCredentials","params":{"ssid":"YOUR_SSID","password":"YOUR_PASSWORD"}}'
```

* Led.Toggle
```bash
mosquitto_pub -h 192.168.0.150 -t esp32/rpc/request -m '{"method":"Led.Toggle","params":{}}'
```

## Referências

* [Mongoose-os](https://mongoose-os.com/docs/mongoose-os/userguide/rpc.md)
* [ThingsBoard](https://thingsboard.io/docs/user-guide/rpc/)
