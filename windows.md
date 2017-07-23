# Windows 10

## Impedir que o windows atualize os drivers automaticamente
* Painel de controle
* Sistema e Segurança
* Sistema
* Configurações avançadas de sistema
* Harware
* Configurações de Instalação do Dispositivo
* Não (o dispositivo poderá não funcionar conforme o esperado)

# Windows 8
Pré/pós preparatórios

## Observações para RAID 0
Caso o seu computador possuir um SSD em RAID 0 (aceleração do sistema), será necessário desabilitar a aceleração e "liberar" o disco sólido para uso pelo sistema **ANTES DE FORMATÁ-LO**. 
Isso é importante para que o Windows não se perca e deixe de reconhecer os discos durante a instalação do sistema.
Caso a RAID não seja desfeita, o instalador do Windows pode não conseguir identificar/ler/modificar as informações de partições do HD devido a este estar sendo acelerado pelo SSD. Para resolver esse problema será necessário desfazer a RAID manualmente, através da BIOS/UEFI (Não tenho um "tutorial" no momento para solucionar este problema)

## Corrigindo resolução (Programas "embaçados")
* Clique com o botão direito do mouse na Área de Trabalho e selecione `Personalizar`
* Acesse o menu `Vídeo` no canto inferior esquerdo da janela que se abriu
* Selecione `Deixe-me escolher um nível de escala para todos os meus vídeos`
* Selecione o zoom (`Alterar o tamanho de todos os itens`) que ficar mais adequado ao seu monitor

## Criação pendrive UEFI
Em um 'terminal' com privilégios de administrador, seguir o procedimento:
```shell
diskpart
list volume
select volume X
delete volume override
convert gpt
create partition primary
format fs=fat32 label="Windows 8" quick
```
Após isso, copiar o conteúdo de um CD/ISO para dentro do pendrive

Fontes:
* [Windows EightForums](http://www.eightforums.com/tutorials/2328-uefi-unified-extensible-firmware-interface-install-windows-8-a.html)
* [Lambda3](https://blog.lambda3.com.br/2012/11/como-instalar-o-windows-8-a-partir-do-usb-num-sistema-com-uefi/)

## Remoção da senha ao iniciar o Windows
1. Abrir o "executar" com `Win + R` e abrir o controle de usuários `control userpasswords2`
2. Desmarcar a opção `Os usuários devem digitar um nome de usuário e uma senha para usar este computador`
3. Confirmar com a senha do usuário

Fontes:
* [Fórum de usuários da HP](http://h30487.www3.hp.com/t5/Dicas-dos-Experts-para-notebooks/Como-tirar-a-senha-de-inicializa%C3%A7%C3%A3o-do-Windows-8-e-8-1/td-p/390869)

## Iniciar o Windows diretamente na área de trabalho
1. Clicar com o botão direito na barra de tarefas e selecionar `Propriedades`
2. Na aba `Navegação`, marcar a opção `Quando eu entrar ou fechar todos os aplicativos em uma tela, ir para a área de trabalho em vez de Iniciar`

Fontes:
* [Fórum de usuários da HP](http://h30487.www3.hp.com/t5/Dicas-dos-Experts-para-notebooks/Como-tirar-a-senha-de-inicializa%C3%A7%C3%A3o-do-Windows-8-e-8-1/td-p/390869)

## Habilitar Hibernação (e outras opções ausentes)
1. Abrir o "executar" com `Win + R` e abrir as opções de enegria `powercfg.cpl`
2. No menu lateral esquerdo, selecionar a opção `Escolher a função dos botões de energia`
3. Selecionar `Alterar configurações não disponíveis no momento`
4. Abaixo, em `Configurações de desligamento`, habilitar as opções desejadas

Fontes:
* [ISAID](http://ishamsaid.tumblr.com/post/54171162216/enable-hibernate-in-windows-8-1-start-right-click-pop)

## Computador não desliga corretamente
Alguns computadores não estão desligando corretamente no windows 8.1. O computador desliga, mas os leds continuam ativos e o computador não pode ser ligado antes de forçar o seu desligamento. Aparentemente, esse problema é causado por algum erro no "Hybrid Shutdown" (semi-hibernado). Para desativar essa funcionalidade:

1. Abrir o "executar" com `Win + R` e abrir as opções de enegria `powercfg.cpl`
2. No menu lateral esquerdo, selecionar a opção `Escolher a função dos botões de energia`
3. Selecionar `Alterar configurações não disponíveis no momento`
4. Abaixo, em `Configurações de desligamento`, desabilitar a opção `Ligar inicialização rápida (recomendado)`

Fontes:
* [AskVG](http://www.askvg.com/fix-windows-8-restart-and-shutdown-problems-by-disabling-hybrid-shutdown-feature/)

## Programar o computador para desligar automaticamente
1. Pressione `Win + S` para abrir a pesquisa e digite `Agendar tarefas` (ou `Schedule tasks` caso o windows estiver em Inglês)
2. Clique com o direito e selecione `Executar como administrador`
3. Na aba `Ações` (`Actions`), selecione `Criar tarefa...` (`Create Task`)
4. Nomeie a tarefa (Ex: `Desligamento automático`) para facilitar a identificação
5. Na aba `Disparadores` (`Triggers`), inclua as regras para início da tarefa
6. Na aba `Ações` (`Actions`), inclua uma ação com o script `shutdown` e os argumentos `/s /f`
7. Confirme as janelas.

Fontes:
* [TECHZANE](http://techzane.com/schedule-automatic-shutdown-in-windows-8/)
