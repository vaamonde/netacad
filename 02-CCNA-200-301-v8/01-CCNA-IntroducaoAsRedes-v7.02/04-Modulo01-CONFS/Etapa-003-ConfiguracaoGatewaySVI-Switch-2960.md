#Autor: Robson Vaamonde<br>
#Procedimentos em TI: http://procedimentosemti.com.br<br>
#Bora para Prática: http://boraparapratica.com.br<br>
#Robson Vaamonde: http://vaamonde.com.br<br>
#Facebook Procedimentos em TI: https://www.facebook.com/ProcedimentosEmTi<br>
#Facebook Bora para Prática: https://www.facebook.com/BoraParaPratica<br>
#Instagram Procedimentos em TI: https://www.instagram.com/procedimentoem<br>
#YouTUBE Bora Para Prática: https://www.youtube.com/boraparapratica<br>
#Data de criação: 02/03/2026<br>
#Data de atualização: 02/03/2026<br>
#Versão: 0.12<br>
#Testado e homologado no Cisco Packet Tracer 9.0.x x64 no Microsoft Windows ou GNU/Linux

Conteúdo estudado nessa configuração:<br>
#01_ PRIMEIRA ETAPA: Acessando o Modo de Configuração Global do Switch Cisco Catalyst 2960.<br>
#02_ SEGUNDA ETAPA: Configuração do Gateway padrão IPv4 no Cisco IOS.<br>
#03_ TERCEIRA ETAPA: Configuração da Interface SVI no Cisco IOS.<br>
#04_ QUARTA ETAPA: Automatizando a Configuração do Segundo Switch Cisco Catalyst 2960.<br>

## PRIMEIRA ETAPA: Acessando o Modo de Configuração Global do Switch Cisco Catalyst 2960.

01. Acessando o modo EXEC Privilegiado e o modo de Configuração Global de Comandos.

```bash
AVISO: acesso autorizado somente a funcionarios
User Access Verification
Username: SEU_USUÁRIO
Password: SUA_SENHA_SEGURA

sw-01> enable
Password: SUA_SENHA_SEGURA

sw-01# configure terminal
sw-01(config)#
```
---

## SEGUNDA ETAPA: Configuração do Gateway padrão IPv4 no Cisco IOS.

01. Configuração do Gateway Padrão IPv4 no Switch para Acesso Remoto.

**DICA-01:** configuração do __`Gateway IPv4`__ em Switch Cisco Catalyst Layer 2 serve somente para **Acesso Remoto** com finalidade de monitoramento/gerenciamento do Switch;

**DICA-02:** em Switch Cisco Catalyst Layer 3 o recurso de __`Gateway`__ é utilizado tanto para **Acesso Remoto ou para Roteamento de Redes/Sub-Redes** utilizando principalmente __`VLAN (Virtual-LAN)`__ ou Protocolos de Roteamento **IGP** (Interior Gateway Protocols) como o: __`RIP (Routing Information Protocol), EIGRP (Enhanced Interior Gateway Routing Protocol), OSPF (Open Shortest Path First), Rota Estática, etc.`__

> **OBSERVAÇÃO-01:** esse recurso é necessário para **Administração Remota ou Monitoramento** do Switch Cisco Catalyst Layer 2 ou Layer 3.

> **OBSERVAÇÃO-02:** existe a possibilidade da configuração do __`Gateway utilizar o endereço IPv6`__ em Switch Cisco Catalyst Layer 2 ou Layer 3, para essa configuração é recomendo utilizar o Cisco IOS na versão mínima DE: **15.x (versão padrão no Cisco Packet Tracer 8.2.2 - v15.0.-2-SE4 que já tem suporte ao IPv6).**

```bash
!Configurando o Gateway Padrão do Switch Cisco
sw-01(config)# ip default-gateway 192.168.1.254
```
---

## TERCEIRA ETAPA: Configuração da Interface SVI no Cisco IOS.

01. Configuração da Interface Virtual do Switch SVI (Switch Virtual Interface).

**DICA-03:** interfaces virtuais são criadas utilizando o recurso de __`VLAN (Virtual-LAN)`__ disponível nos Switch Cisco Catalyst Layer 2 ou Layer 3.

**DICA-04:** é recomendado utilizar **Outra VLAN para o SVI**, não é recomendado utilizar a __`VLAN Padrão 1`__ para essa finalidade, a **VLAN 1 (VLAN Default)** existe em todos os Switch e é por causa disso que Switches de Fabricantes Diferente se comunicam entre si, todos utilizam sempre a VLAN 1 como padrão para a comunicação.

> **OBSERVAÇÃO-03:** em Switch Cisco Catalyst Layer 2 utilizamos o SVI somente para __`Administração Remota ou Monitoramento do Equipamento`__, ela não será utilizada para **Gateway da Rede Local** ou para integração com **Protocolos de Roteamento**.

> **OBSERVAÇÃO-04:** o SVI é necessário para o __`Acesso Remoto nas Linhas Virtuais (VTY)`__ utilizando os protocolos: **Telnet, SSH** ou outro protocolo configurado, após a configuração da SVI o Switch irá possuir um __`Endereço Físico (MAC Address)`__ associado a um __`Endereço Lógico de Rede (IPv4 ou IPv6)`__ permitindo o seu acesso remoto.

```bash
!Acessando a Interface da VLAN 1 Padrão
sw-01(config)# interface vlan 1
```
---

  - Configuração da Descrição da Interface Virtual VLAN-1.

**DICA-05:** sempre utilizar o comando: **description** nas Interfaces para efeito de documentação.

> **OBSERVAÇÃO-05:** documentação de Interface facilita o processo de **Identificação e Função** dela na topologia de rede, configuração obrigatória em Switch ou Router.

```bash
!Configurando a Descrição da Interface de VLAN 1 do Switch Cisco
sw-01(config-if)# description Interface de Gerenciamento do Switch SW-01
```
---

  - Configuração do Endereçamento IPv4 da Interface Virtual VLAN-1.

**DICA-06:** o endereço IPv4 deve ser da __`Mesma Faixa de Rede ou Sub-Rede do Gateway Padrão`__ configurado no Switch na **Segunda Etapa**.

**DICA-07:** é recomendado que os _-`Endereços de Rede ou Sub-Redes`__ dos Switches sejam diferentes das **Redes dos Desktops, Notebook, Wi-Fi (Wireless/Sem-Fio), CFTV (Circuito Fechado de TV), etc.** para garantir a segurança de acesso aos equipamentos somente para a __`Equipe/Profissionais de TI`__ que esteja nessa mesma Rede/Sub-Rede.

> **OBSERVAÇÃO-06:** configuração do endereço IPv4 deve ser: **IPv4 + Máscara de Rede Completa (ClassFull)**, não utilizar __`CIDR (Classes Inter-Domain Routing)`__ nas configurações.

```bash
!Configurando o Endereço IPv4 e Máscara de Rede da Interface VLAN 1 do Switch Cisco
sw-01(config-if)# ip address 192.168.1.250 255.255.255.0
```
---

  - Inicializando a Interface Virtual da VLAN-1.

**DICA-07:** por padrão __`Todas as Interfaces de Rede`__ estão no status: **desligada (Shutdown)** no Switch ou Router.

**DICA-08:** por padrão __`Todas as Portas de Rede`__ estão no status: **ligada (No-Shutdown)** no Switch.

> **OBSERVAÇÃO-07:** o comando: **no** também é utilizado para ligar as Interfaces tirando do status: **shutdown (Desligada)** e mudando o seu status para: **no shutdown (Ligada)**.

```bash
!Inicializando a Interface VLAN 1 do Switch Cisco
sw-01(config-if)# no shutdown
```
---

  - Saindo de todos os níveis e voltando para o modo EXEC Privilegiado.

**DICA-09:** somente no __`Modo EXEC Privilegiado`__ você tem o comando: **copy** ou **write** para salvar as configurações.

**DICA-10:** existe também o comando: **do**, esse comando permite executar qualquer comando __`Fora do seu Nível Padrão`__.

**EXEMPLO:** __`sw-01(config-line)# do copy running-config startup-config | sw-01(config-line)# do show running-config | sw-01(config-line)# do write`__

```bash
!Saindo de todos os níveis do Switch Cisco
sw-01(config-if)# end
sw-01#
```
---

  - Salvando as configurações da memória RAM (Running-Config) para a memória NVRAM (Startup-Config)

**DICA-11:** nunca esqueça de salvar as configurações.

```bash
!Salvando as configurações da RAM para NVRAM do Switch Cisco
sw-01# copy running-config startup-config
  Destination filename [startup-config]? <Enter>
  Building configuration...
  [OK]
sw-01#
```

  - Visualizando as configurações da memória RAM (Running-Config).

**DICA-12** após a configuração da SVI verifique se tudo está configurado de forma correta utilizando os comandos: **show**.

```bash
!Visualizando as Configurações do Running-Config (RAM)
sw-01# show running-config
  Building configuration...

  Current configuration : 1763 bytes
  !
  ...
  !
  end
sw-01#
```
---

```bash
!Fazendo um Filtro na Visualização do Running-Config somente da Interface Vlan1
sw-01# show running-config | section include interface Vlan1
  interface Vlan1
    description Interface de SVI
    ip address 192.168.1.250 255.255.255.0
sw-01#
```
---

```bash
!Visualizando as configurações das interfaces e portas de rede do Switch
sw-01# show ip interface brief
  Interface              IP-Address      OK? Method Status                Protocol 
  FastEthernet0/1        unassigned      YES manual up                    up 
  FastEthernet0/2        unassigned      YES manual up                    up 
  FastEthernet0/3        unassigned      YES manual down                  down
  ...
  FastEthernet0/24       unassigned      YES manual up                    up 
  GigabitEthernet0/1     unassigned      YES manual up                    up 
  GigabitEthernet0/2     unassigned      YES manual up                    up 
  Vlan1                  192.168.1.250   YES manual up                    up
sw-01#
```
---

```bash
!Visualizando as configurações das VLANs padrão do Switch
sw-01# show vlan brief
  VLAN Name                             Status    Ports
  ---- -------------------------------- --------- -------------------------------
  1    default                          active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                  Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                  Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                  Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                  Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                  Fa0/21, Fa0/22, Fa0/23, Fa0/24
                                                  Gig0/1, Gig0/2
  1002 fddi-default                     active    
  1003 token-ring-default               active    
  1004 fddinet-default                  active    
  1005 trnet-default                    active
sw-01#
```
---

  - Testando a conectividade entre o Switch e os Desktops da Rede

**DICA-13** depois da configuração do __`SVI no Switch Cisco Catalyst Layer 2`__ você consegue agora pingar os **Desktops da Rede** utilizado o Protocolo __`ICMP (Internet Control Message Protocol)`__ com o comando: **ping** para testar a interconectividade de rede.

> **OBSERVAÇÃO-08** o carácter: **! (exclamação)** utilizado no comando: **ping** significa que os __`Pacotes ICMP Enviado para o Destino`__ foi recebido com **Sucesso**, o padrão é enviar: **5 Pacotes (Sending 5)** já o carácter: **. (ponto)** significa que os __`Pacotes ICMP foram Perdidos`__ ou o destino não recebeu os pacotes.

> **OBSERVAÇÃO-09** na última linha do comando: **ping** do Cisco IOS mostra a opção: **Success rate is 100 percent (5/5)** que representa que: __`100% dos Pacotes foram Enviado e Recebidos totalizando: *5 (cinco) Pacotes Enviados e 5 (cinco) Pacotes Recebidos`__, na opção: **round-trip** que é o: __`Tempo de Vida de Ida e Volta dos Pacotes (RTT - Round Trip Time)`__ medido em **Milissegundos**, Mínimo (min): 8 ms — O menor tempo registrado, Médio (avg): 10 ms — Média dos tempos e Máximo (max): 15 ms — O maior tempo registrado.

**DICA-14:** RTT é o tempo que um pacote leva para __`Sair do Dispositivo de Origem, chegar ao Destino e Retornar`__. Esse tempo inclui: **Latência da Rede, Processamento dos dispositivos intermediários e Variações momentâneas (jitter).**

```bash
!Pingando a SVI do Switch Layer 2 2960 sw-01
sw-01# ping 192.168.1.250
  Type escape sequence to abort.
  Sending 5, 100-byte ICMP Echos to 192.168.1.250, timeout is 2 seconds:
  !!!!!
  Success rate is 100 percent (5/5), round-trip min/avg/max = 8/10/15 ms
sw-01#
```
---

```bash
!Pingando o endereço IPv4 do Servidor
sw-01# ping 192.168.1.1
  Type escape sequence to abort.
  Sending 5, 100-byte ICMP Echos to 192.168.1.1, timeout is 2 seconds:
  .!!!!
  Success rate is 80 percent (4/5), round-trip min/avg/max = 0/0/0 ms
sw-01#
```
---

## QUARTA ETAPA: Automatizando a Configuração do Segundo Switch Cisco Catalyst 2960.

01. Utilizando o Visual Studio Code (VSCode) para automatizar as configurações do Cisco IOS.

> **OBSERVAÇÃO-10:** recomendo sempre utilizar um __`Editor de Texto Profissional`__ para criar os scripts e automatizar as tarefas de configuração do Cisco IOS, hoje em dia é indicado utilizar o Visual Studio Code (VSCode) junto com as Extensões: **Cisco IOS Syntax e Cisco Config Highlight** para facilitar essa configuração.

**DICA-15:** o caractere: *! (exclamação)* é utilizado como um recurso de *Comentário*, sua utilização server para comentar o código de automação do Cisco IOS ou para desativar um comando para não ser executado, *RECOMENDO FORTEMENTE DOCUMENTAR TODOS OS COMANDOS E PROCEDIMENTOS DE CONFIGURAÇÃO PARA FACILITAR O ENTENDIMENTO.*

**DICA-16:** para facilitar a leitura do código, recomendo utilizar o recurso de **Indentação de Código** usando a Tecla TAB (Tabulador/Tabulação) para cada nível que você está configurando o Cisco IOS, isso facilitada a análise de erros (Debug) do código.

  - Acessando o modo EXEC Privilegiado e o modo de Configuração Global de Comandos.
```bash
AVISO: acesso autorizado somente a funcionarios
User Access Verification
Username: SEU_USUÁRIO
Password: SUA_SENHA_SEGURA

sw-02> enable
Password: SUA_SENHA_SEGURA

sw-02#
```
---

```python
!Acessando o modo de Configuração Global de comandos
configure terminal

  !Configuração do Gateway padrão IPv4 no Switch
  ip default-gateway 192.168.1.254

  !Configuração da Interface Virtual do Switch SVI
  interface vlan 1

    !Configuração da Descrição da Interface Virtual
    description Interface de Gerenciamento do Switch SW-02

    !Configuração do Endereçamento IPv4 da Interface Virtual
    ip address 192.168.1.251 255.255.255.0

    !Inicializando a Interface Virtual do Switch
    no shutdown

    !Saindo de todos os níveis e voltando para o modo EXEC Privilegiado
    end

!Salvando as configurações da memória RAM para a memória NVRAM
!OBSERVAÇÃO IMPORTANTE: deixar uma linha em branco no final do script para
!salvar automaticamente o script na hora da execução, fazendo a função de
!<Enter> no final do script.
write

```