# Windows 8
Pré/pós preparatórios

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

## Remoção da senha ao iniciar o Windows
1. Abrir o "executar" com `Win + R` e abrir o controle de usuários `control userpasswords2`
2. Desmarcar a opção `Os usuários devem digitar um nome de usuário e uma senha para usar este computador`
3. Confirmar com a senha do usuário

## Iniciar o Windows diretamente na área de trabalho
1. Clicar com o botão direito na barra de tarefas e selecionar `Propriedades`
2. Na aba `Navegação`, marcar a opção `Quando eu entrar ou fechar todos os aplicativos em uma tela, ir para a área de trabalho em vez de Iniciar`

Fontes:
* [Windows EightForums](http://www.eightforums.com/tutorials/2328-uefi-unified-extensible-firmware-interface-install-windows-8-a.html)
* [Lambda3](https://blog.lambda3.com.br/2012/11/como-instalar-o-windows-8-a-partir-do-usb-num-sistema-com-uefi/)
* Fórum de usuários da HP [link 1](http://h30487.www3.hp.com/t5/Dicas-dos-Experts-para-notebooks/Como-tirar-a-senha-de-inicializa%C3%A7%C3%A3o-do-Windows-8-e-8-1/td-p/390869) e [link 2](http://h30487.www3.hp.com/t5/Dicas-dos-Experts-para-notebooks/Iniciar-Windows-8-1-direto-na-%C3%A1rea-de-trabalho/td-p/389389)
