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
#02_ SEGUNDA ETAPA: Configuração das Linhas Virtuais (VTY) do Cisco IOS.<br>
#03_ TERCEIRA ETAPA: Automatizando a Configuração do Segundo Switch Cisco Catalyst 2960.<br>

## PRIMEIRA ETAPA: Acessando o Modo de Configuração Global do Switch Cisco Catalyst 2960.

01. Acessando o modo EXEC Privilegiado e o modo de Configuração Global de Comandos.

```bash
AVISO: acesso autorizado somente a funcionarios
User Access Verification
Username: SEU_USUÁRIO
Password: SUA_SENHA

sw-01> enable
Password: SUA_SENHA_SEGURA

sw-01# configure terminal
sw-01(config)#
```

## SEGUNDA ETAPA: Configuração das Linhas Virtuais (VTY) do Cisco IOS.

01. Acessando as Linhas (Lines) Virtuais de Acesso remoto do Switch no Cisco IOS.

**DICA-01:** por padrão o Switch Cisco possui **16 (0 até 15)** linhas virtuais de acesso remoto.

> **OBSERVAÇÃO-01:** as linhas virtuais são utilizadas para __`Acessar Remotamente o Terminal`__ do Switch ou Router para facilitar a sua configuração ou administração em locais onde o acesso físico e complicado ou unidades remotas, **exemplo:** *Switch em outro Andar do Prédio, Switch em outra Unidade da Empresa, Switch Remotos em Cidades/Estados diferentes*.

> **OBSERVAÇÃO-02:** linhas virtuais **Não são utilizadas para Monitoramento**, para isso usamos os Protocolos: __`SNMP (Simple Network Management Protocol), Netflow (Network Traffic Analyzer), Syslog (System Log Analyzer), etc.`__ com as configurações do **SVI (Switch Virtual Interface)**.

**DICA-02:** não é recomendado habilitar __`Poucas Linhas ou Todas as Linhas`__ virtuais no Cisco IOS.

> **OBSERVAÇÃO-03:** as linhas virtuais é bem parecida com a **Linha Console**, a diferença é que o acesso e feito remotamente utilizando Protocolo: __`TCP (Transmission Control Protocol) e Endereçamento IPv4 ou IPv6`__.

**DICA-03:** por padrão as linhas virtuais estão desabilitadas no Cisco IOS, elas dependem da configuração do **SVI (Switch Virtual Interface)** para funcionar.

```bash
!Acessando o modo de configuração das Linhas Virtuais de 0 até 5 no Switch Cisco
sw-01(config)# line vty 0 4
```

  - Forçando fazer login local utilizando os usuários e senhas locais criados no Switch (usuários criados na etapa de configuração básica do Switch).

**DICA-04:** por padrão a configuração da **Linha Virtual** é não permitir nenhuma conexão, se você utilizar a opção do comando: **login** o acesso será feito sem autenticação, sendo necessário no mínimo configurar a opção do comando: **password**

```bash
!Configuração da autenticação local do Switch Cisco
sw-01(config-line)# login local
```

  - Habilitando a senha de acesso do Tipo-7 Password (senha fraca).

**DICA-05:** igual na configuração da Line Console, essa regra só irá funcionar se não existir usuários no Switch e se você não configurou a opção: **login local,** deixando apenas a opção: **local**.

```bash
!Configuração da senha de acesso ao Console do Switch Cisco
sw-01(config-line)# password SUA_SENHA_NÃO_SEGURA
```

  - Habilitando o sincronismo das mensagens de Logs na tela do terminal do Cisco IOS.

**DICA-06:** essa configuração é fundamental na __`Line VTY`__, igual a **Line Console** o sincronismos dos Logs e comandos precisa ser configurado para facilitar a administração do Switch ou Router.

```bash
!Configuração do Sincronismo dos Logs do Switch Cisco
sw-01(config-line)# logging synchronous
```

  - Habilitando o tempo de inatividade de uso do linha virtual.

**DICA-07:** na Line Virtual a desconexão por falta de interatividade é obrigatória, esse opção minimizar as falhas de segurança de acesso Remoto não Autorizado ao Switch ou Router.

```bash
!Configuração do tempo de inatividade das Linhas Virtuais do Switch Cisco
sw-01(config-line)# exec-timeout 5 30
```

  - Saindo de todos os níveis e voltando para o modo EXEC Privilegiado.

**DICA-08:** lembre-se sempre de sair de todos os níveis para salvar as configurações.

```bash
!Saindo de todos os níveis do Switch Cisco
sw-01(config-line)# end
sw-01#
```

   - Salvando as configurações da memória RAM (Running-Config) para a memória NVRAM (Startup-Config).

**DICA-09:** nunca esqueça de salvar as configurações.

```bash
!Salvando as configurações da RAM para NVRAM do Switch Cisco
sw-01# copy running-config startup-config
  Destination filename [startup-config]? <Enter>
  Building configuration...
  [OK]
sw-01#
```

09. Visualizando as configurações da memória RAM (Running-Config).

**DICA-10** após a configuração da Line Virtual verifique se tudo está configurado de forma correta utilizando os comandos: **show**.

```bash
!Visualizando as Configurações do Running-Config (Memória RAM)
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
!Fazendo um Filtro na Visualização do Running-Config somente da Sessão Line VTY
sw-01# show running-config | section include line vty
  line vty 0 4
    exec-timeout 5 30
    password 7 08701E1D290A00191308
    logging synchronous
    login local
  line vty 5 15
    login
sw-01#
```

## TERCEIRA ETAPA: Automatizando a Configuração do Segundo Switch Cisco Catalyst 2960.

01. Utilizando o Visual Studio Code (VSCode) para automatizar as configurações do Cisco IOS.

> **OBSERVAÇÃO-04:** recomendo sempre utilizar um __`Editor de Texto Profissional`__ para criar os scripts e automatizar as tarefas de configuração do Cisco IOS, hoje em dia é indicado utilizar o Visual Studio Code (VSCode) junto com as Extensões: **Cisco IOS Syntax e Cisco Config Highlight** para facilitar essa configuração.

**DICA-11:** o caractere: *! (exclamação)* é utilizado como um recurso de *Comentário*, sua utilização server para comentar o código de automação do Cisco IOS ou para desativar um comando para não ser executado, *RECOMENDO FORTEMENTE DOCUMENTAR TODOS OS COMANDOS E PROCEDIMENTOS DE CONFIGURAÇÃO PARA FACILITAR O ENTENDIMENTO.*

**DICA-12:** para facilitar a leitura do código, recomendo utilizar o recurso de **Indentação de Código** usando a Tecla TAB (Tabulador/Tabulação) para cada nível que você está configurando o Cisco IOS, isso facilitada a análise de erros (Debug) do código.

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

  !Acessando as linhas virtuais de acesso remoto do Switch
  line vty 0 4

    !Forçando fazer login local utilizando usuário e senha locais do switch
    login local

    !Habilitando senha de acesso do Tipo-7 Password
    password SUA_SENHA_NÃO_SEGURA

    !Sincronizando as mensagens de logs na tela
    logging synchronous

    !Habilitando o tempo de inatividade de uso da linha virtual
    exec-timeout 5 30

    !Saindo de todos os níveis e voltando para o modo EXEC Privilegiado
    end

!Salvando as configurações da memória RAM para a memória NVRAM
!OBSERVAÇÃO IMPORTANTE: deixar uma linha em branco no final do script para
!salvar automaticamente o script na hora da execução, fazendo a função de
!<Enter> no final do script.
write

```