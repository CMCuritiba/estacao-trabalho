# Caderno de Testes - Estação de Trabalho CMC

Este documento contém todos os testes necessários para validar a configuração correta da estação de trabalho CMC.

## Como usar este caderno

1. Execute cada teste na ordem apresentada
2. Marque como ✅ os testes que passaram
3. Marque como ❌ os testes que falharam e investigue o problema
4. Para testes automáticos, use o comando `molecule verify` na pasta `roles/estacao/`

## Testes Automatizados com Molecule

Para facilitar a validação, alguns testes podem ser executados automaticamente usando o Molecule:

```bash
cd roles/estacao/
molecule verify
```

### Testes automatizados disponíveis:

- **001-002**: Verificação de usuário suporte e grupos
- **004**: Verificação de programas removidos
- **005**: Verificação de repositórios brasileiros
- **006**: Verificação de programas essenciais instalados
- **007**: Verificação de instalação do Google Chrome
- **009**: Verificação de imagens CMC
- **011**: Verificação de diretórios skel
- **013**: Verificação de políticas do Chrome
- **019**: Verificação de TTY desabilitados
- **022**: Verificação de políticas do terminal
- **023**: Verificação de configuração DNS
- **025-026**: Verificação de script e serviço de boot
- **028-029**: Verificação de pacotes e configuração AD
- **034**: Verificação de configuração sudoers
- **036**: Verificação de unattended-upgrades
- **040-041**: Verificação de S3FS e mount point
- **045-046**: Verificação de SSH server
- **057**: Verificação de CUPS
- **066**: Verificação de script on-login
- **072**: Verificação de arquivo de versão
- **073**: Verificação de diretório de arquivos modificados

> **Nota**: Os testes automatizados cobrem validações que podem ser verificadas programaticamente. Testes que requerem interação do usuário, verificação visual ou testes de funcionalidade devem ser executados manualmente seguindo as instruções abaixo.

## Testes Manuais

### Categoria: Usuários e Grupos (010-user-group-suporte.yml)

| ID  | Teste                      | Como testar                                                                                                                |
| --- | -------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| 011 | Criação do usuário suporte | Testar login na estação com o usuário suporte usando a senha configurada                                                   |
| 012 | Grupo do usuário suporte   | Verificar se o usuário suporte está nos grupos corretos<br>No terminal: `id suporte`<br>Deve mostrar grupos: suporte, sudo |
| 013 | Senha de root              | Testar login com root com a senha configurada<br>No terminal: `su -`<br>Inserir a senha de root configurada                |

### Categoria: Remoção de Programas (020-remover-programas.yml)

| ID  | Teste               | Como testar                                                                                                                                                                                                                                               |
| --- | ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 021 | Programas removidos | Verificar se programas desnecessários foram removidos<br>Comando: `dpkg -l \| grep -E "(hexchat\|thunderbird\|hypnotix\|drawing\|celluloid\|pix\|webapp-manager\|warpinator\|sticky\|simple-scan\|transmission)"` <br> Não deve retornar nenhum resultado |

### Categoria: Instalação de Programas (030-instala-programas.yml)

| ID  | Teste                           | Como testar                                                                                                                                                                                                                                                                   |
| --- | ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 031 | Repositórios brasileiros        | Verificar se repositórios estão configurados para Brasil <br> Comando: `grep "br.archive.ubuntu.com" /etc/apt/sources.list.d/official-package-repositories.list` <br> Comando: `grep "mint-packages.c3sl.ufpr.br" /etc/apt/sources.list.d/official-package-repositories.list` |
| 032 | Programas essenciais instalados | Verificar instalação de programas essenciais <br> Comando: `dpkg -l \| grep -E "(curl\|vim\|git\|htop\|tree\|unzip\|wget\|ssh)"` <br> Todos devem estar instalados.                                                                                                           |

### Categoria: Imagens CMC (040-imagens-cmc.yml)

| ID  | Teste                       | Como testar                                                                                                                    |
| --- | --------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| 041 | Imagens de fundo CMC        | Verificar se imagens estão na pasta correta<br>Comando: `ls -la /usr/share/backgrounds/cmc/`<br>Deve conter arquivos de imagem |
| 042 | Papel de parede configurado | Verificar se papel de parede padrão está definido<br>Verificar nas configurações do sistema ou dconf                           |

### Categoria: Configuração Skel (050-skel.yml)

| ID  | Teste                     | Como testar                                                                                                                    |
| --- | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| 051 | Diretórios skel criados   | Verificar estrutura de diretórios em /etc/skel<br>Comando: `ls -la /etc/skel/`<br>Verificar se contém Desktop, Documents, etc. |
| 052 | Configurações padrão skel | Testar criando um novo usuário e verificar se herda as configurações<br>Login com novo usuário e verificar desktop             |

### Categoria: Browsers (060-configura-browsers.yml)

| ID  | Teste                 | Como testar                                                                                                                                                       |
| --- | --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 061 | Políticas do Chrome   | Verificar arquivo de políticas do Chrome<br>Comando: `cat /etc/opt/chrome/policies/managed/policies.json`<br>Deve conter configurações como bookmarks gerenciados |
| 062 | Firefox configurado   | Verificar configuração do Firefox<br>Comando: `ls -la /usr/lib/firefox/`<br>Verificar arquivos de configuração                                                    |
| 063 | Bookmarks gerenciados | Abrir Chrome e verificar se bookmarks da CMC estão presentes<br>Verificar favoritos: "Câmara Municipal", "Intranet", "Correio", etc.                              |

### Categoria: Autostart (070-autostart.yml)

| ID  | Teste                      | Como testar                                                                                                                |
| --- | -------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| 071 | Arquivos autostart criados | Verificar se arquivos de autostart estão presentes<br>Comando: `ls -la /etc/xdg/autostart/`<br>Verificar arquivos .desktop |
| 072 | ForceLogout configurado    | Verificar se forcelogout está configurado<br>Comando: `cat /etc/xdg/autostart/forcelogout.desktop`                         |

### Categoria: Política Informática (080-politica-informatica.yml)

| ID  | Teste                        | Como testar                                                                                                 |
| --- | ---------------------------- | ----------------------------------------------------------------------------------------------------------- |
| 081 | Prompt de aceite da política | Faça um novo login na Estação de Trabalho, você deve receber uma tela com a política para leitura e aceite. |

### Categoria: Desabilitar TTY (090-desabilita-tty.yml)

| ID  | Teste              | Como testar                                                                                           |
| --- | ------------------ | ----------------------------------------------------------------------------------------------------- |
| 091 | TTYs desabilitados | Tentar trocar para TTY2-6 usando Ctrl+Alt+F2 até F6<br>Não deve ser possível acessar outros terminais |

### Categoria: DConf (100-dconf.yml)

| ID  | Teste                         | Como testar                                                                                                                                                 |
| --- | ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 101 | Configurações dconf aplicadas | Verificar configurações do ambiente gráfico <br> Comando: `dconf dump / \| grep -i cinnamon` <br> Verificar se configurações personalizadas estão aplicadas |
| 102 | Applet calendário             | Verificar se applet de calendário está configurado<br>Verificar panel do Cinnamon                                                                           |

### Categoria: Terminal Policy (110-terminal-policy.yml)

| ID  | Teste                 | Como testar                                                                                                                                                         |
| --- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 111 | PolicyKit configurado | Verificar arquivo de política<br>Comando: `cat /usr/share/polkit-1/actions/org.freedesktop.policykit.terminal.policy` <br> Deve conter regras de acesso ao terminal |

### Categoria: DNS (120-configura-dns.yml)

| ID  | Teste                     | Como testar                                                                                                                                               |
| --- | ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 121 | Configuração DNS          | Verificar configuração do `systemd-resolved` <br> Comando: `resolvectl status` <br> Deve conter DNS e domínio configurados.                               |
| 122 | Resolução DNS funcionando | Testar resolução de nomes do domínio <br> Comandos: <br> `resolvectl query pc-daf-1` <br> `resolvectl query google.com` <br> Devem resolver corretamente. |

### Categoria: Script Boot (130-script-boot.yml)

| ID  | Teste              | Como testar                                                                                                                   |
| --- | ------------------ | ----------------------------------------------------------------------------------------------------------------------------- |
| 131 | Script boot criado | Verificar se script existe<br>Comando: `ls -la /usr/local/cmc/scripts/cmc-boot.sh` <br> Script deve existir e ser executável. |
| 132 | Serviço boot ativo | Verificar se serviço está ativo<br>Comando: `systemctl status cmc-boot.service` <br> Deve estar ativo e habilitado            |

### Categoria: Integração AD (140-integra-ad.yml)

| ID  | Teste                   | Como testar                                                                                                                                                                               |
| --- | ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 141 | Hostname configurado    | Verificar se hostname está no domínio<br>Comando: `hostname -f`<br>Deve retornar FQDN com domínio                                                                                         |
| 142 | Configuração SSSD       | Verificar configuração do domínio<br>Comandos: <br> `sudo sssctl domain-status exemplo.com` <br> `id seu.login` <br>Devem mostrar o domínio online e informações sobre seu usuário no AD. |
| 143 | Join no domínio         | Verificar se máquina está no domínio<br>Comando: `sudo realm list`<br>Deve mostrar o domínio configurado                                                                                  |
| 144 | Login domínio funcional | Testar login com usuário do domínio<br>Fazer logout e tentar login com usuário AD                                                                                                         |

### Categoria: PAM (150-configura-pam.yml)

| ID  | Teste                    | Como testar                                                                                                        |
| --- | ------------------------ | ------------------------------------------------------------------------------------------------------------------ |
| 151 | Configuração PAM         | Verificar arquivos PAM<br>Comando: `ls -la /etc/pam.d/`<br>Verificar se contém configurações adequadas             |
| 152 | Autenticação funcionando | Testar autenticação com usuário local e domínio<br>Comando: `su - <usuario>`<br>Deve funcionar para ambos os tipos |
| 153 | _Homedir_                | No primeiro login de cada usuário, o homedir deve ser criado automaticamente em `/home/novo.login`.                |

### Categoria: Sudoers (160-sudoers.yml)

| ID  | Teste                    | Como testar                                                                                                       |
| --- | ------------------------ | ----------------------------------------------------------------------------------------------------------------- |
| 161 | Grupo DTIC no sudoers    | Verificar se grupo tem acesso sudo<br>Comando: `sudo cat /etc/sudoers.d/cmc`<br>Deve conter regra para grupo DTIC |
| 162 | Sudo funcional para DTIC | Testar sudo com usuário do grupo DTIC<br>Login com usuário DTIC e executar: `sudo whoami`<br>Deve retornar "root" |

### Categoria: Unattended Upgrades (170-unattended-upgrades.yml)

| ID  | Teste                         | Como testar                                                                                                                                    |
| --- | ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| 171 | Unattended-upgrades instalado | Verificar se está instalado<br>Comando: `dpkg -l \| grep unattended-upgrades`<br>Deve estar instalado                                          |
| 172 | Configuração upgrades         | Verificar arquivo de configuração<br>Comando: `cat /etc/apt/apt.conf.d/50unattended-upgrades`<br>Deve ter configurações adequadas              |
| 173 | Validação dos upgrades        | Alguns dias após a implantação da ET, consulte os logs das atualizações: <br> `sudo less /var/log/unattended-upgrades/unattended-upgrades.log` |

### Categoria: Bashrc (180-bashrc.yml)

| ID  | Teste              | Como testar                                                                                                         |
| --- | ------------------ | ------------------------------------------------------------------------------------------------------------------- |
| 181 | Bashrc configurado | Verificar configuração personalizada do bash<br>Comando: `cat ~/.bash_aliases` <br> Deve conter personalizações CMC |
| 182 | Aliases funcionais | Abrir terminal e testar aliases personalizados<br>Testar comandos como `ll`, `la` se configurados                   |

### Categoria: S3FS Mount (190-s3fs-mount.yml)

| ID  | Teste              | Como testar                                                                                                                  |
| --- | ------------------ | ---------------------------------------------------------------------------------------------------------------------------- |
| 191 | S3FS instalado     | Verificar se s3fs está instalado<br>Comando: `which s3fs`<br>Deve retornar caminho do executável                             |
| 192 | Mount point criado | Verificar se ponto de montagem existe<br>Comando: `sudo ls /mnt/suporte/` <br>Diretório deve existir                         |
| 193 | Configuração fstab | Verificar entrada no fstab<br>Comando: `grep s3fs /etc/fstab`<br>Deve conter configuração de montagem                        |
| 194 | Pasta montada      | Verificar se o `/mnt/suporte` está montado corretamente: <br> `mount \| grep s3fs` <br>Deve conter configuração de montagem. |

### Categoria: Script Rede (200-script-rede.yml)

| ID  | Teste                  | Como testar                                                                                                           |
| --- | ---------------------- | --------------------------------------------------------------------------------------------------------------------- |
| 201 | Serviço rede ativo     | Verificar serviço de rede<br>Comando: `systemctl status cmc-network-script.service` <br>Verificar se está configurado |
| 202 | Script rede disponível | Verificar se script existe<br>Comando: `ls -lh /mnt/suporte/scripts/boot.sh` <br>Script deve existir                  |

### Categoria: SSH Server (210-ssh-server.yml)

| ID  | Teste                 | Como testar                                                                                                                            |
| --- | --------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| 211 | SSH server rodando    | Verificar se serviço SSH está ativo<br>Comando: `systemctl status ssh`<br>Deve estar ativo                                             |
| 212 | Conexão SSH funcional | Testar conexão SSH remoto <br> De outra estação de trabalho, tente se conectar: `ssh nome-da-et-remota` <br>Deve ser possível conectar |

### Categoria: VNC (220-vnc.yml)

| ID  | Teste                 | Como testar                                                                                                                                                                                                                                                            |
| --- | --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 221 | VNC instalado         | Verificar se VNC está instalado<br>Comando: `ps aux \| grep vino-server` <br>Deve estar rodando                                                                                                                                                                        |
| 222 | Conexão VNC funcional | Testar conexão VNC remota: <br> 1. Na ET 1, faça login <br> 2. Em outra estação de trabalho, tente se conectar: `vinagre nome-da-et1` <br> 3. O usuário na ET 1 deve receber um pedido de conexão. <br> 4. Após aprovação, deve ser possível controlar a tela da ET 1. |

### Categoria: Menu Items (230-menu-items.yml)

| ID  | Teste                    | Como testar                                                                                                                  |
| --- | ------------------------ | ---------------------------------------------------------------------------------------------------------------------------- |
| 231 | Itens menu desabilitados | Verificar se itens desnecessários estão ocultos no menu<br>Abrir menu principal e verificar quais aplicativos estão visíveis |
| 232 | Aplicações desabilitadas | Verificar se as aplicações não aparece no Menu Iniciar: `mintupdate` `mintreport` `lightdm-settings` `mintwelcome`           |

### Categoria: Login Gráfico (240-login-grafico.yml)

| ID  | Teste                    | Como testar                                                                                                               |
| --- | ------------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| 241 | LightDM configurado      | Verificar configuração do lightdm<br>Comando: `cat /etc/lightdm/lightdm.conf`<br>Deve conter configurações personalizadas |
| 242 | Tema login personalizado | Verificar se tema da CMC está aplicado<br>Fazer logout e verificar aparência da tela de login                             |
| 243 | Autologin desabilitado   | Verificar se autologin está desabilitado<br>Reiniciar e verificar se pede login                                           |

### Categoria: CUPS (250-configura-cups.yml)

| ID  | Teste                    | Como testar                                                                                           |
| --- | ------------------------ | ----------------------------------------------------------------------------------------------------- |
| 251 | CUPS instalado e rodando | Verificar se CUPS está ativo<br>Comando: `systemctl status cups`<br>Deve estar ativo                  |
| 252 | Interface CUPS acessível | Acessar interface web do CUPS<br>Abrir navegador em `http://localhost:631`<br>Interface deve carregar |
| 253 | Configuração impressoras | Verificar se pode configurar impressoras<br>Tentar adicionar impressora via interface                 |
| 254 | Validar impressão        | Verificar se é possível imprimir normalmente                                                          |

### Categoria: Mount Pendrive (260-mount-pendrive.yml)

| ID  | Teste                           | Como testar                                                                                                    |
| --- | ------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| 261 | Regras udev criadas             | Verificar regras para dispositivos USB<br>Comando: `ls -la /etc/udev/rules.d/`<br>Verificar arquivos de regras |
| 262 | Montagem automática funcionando | Conectar pendrive e verificar montagem<br>Plugar USB e verificar se monta automaticamente                      |

### Categoria: Bluetooth Pairing (265-pairing-bluetooth.yml)

| ID  | Teste                 | Como testar                                                                                                |
| --- | --------------------- | ---------------------------------------------------------------------------------------------------------- |
| 266 | Bluetooth configurado | Verificar se bluetooth está funcionando<br>Comando: `bluetoothctl show`<br>Deve mostrar adaptador          |
| 267 | Pareamento funcional  | Testar pareamento com dispositivo<br>Tentar parear com dispositivo bluetooth via interface gráfica do Mint |

### Categoria: Bloqueio Cinnamon (270-bloqueio-cinnamon.yml)

| ID  | Teste                            | Como testar                                                                                                                                             |
| --- | -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 271 | Programas bloqueados             | Tentar executar programas restritos como usuário comum:<br> `gnome-terminal` `mintupdate` `mintreport` `cinnamon-menu-editor` `cinnamon-desktop-editor` |
| 272 | Configurações bloqueadas         | Tentar abrir configurações do sistema<br>Deve estar restrito para usuários normais                                                                      |
| 273 | Programas liberados para Suporte | Tentar executar programas restritos como usuário da DTIC:<br> `gnome-terminal` `cinnamon-desktop-editor`                                                |

### Categoria: On Login (280-on-login.yml)

| ID  | Teste                     | Como testar                                                                                               |
| --- | ------------------------- | --------------------------------------------------------------------------------------------------------- |
| 282 | Script on-login criado    | Verificar se script existe<br>Comando: `ls -lh /usr/local/cmc/scripts/on-login.sh`<br>Script deve existir |
| 283 | Ícones desktop protegidos | Fazer login e verificar ícones<br>Tentar remover ícones do desktop<br>Deve estar protegido (chattr +i)    |

### Categoria: Logs (290-configura-logs.yml)

| ID  | Teste                | Como testar                                                                                                         |
| --- | -------------------- | ------------------------------------------------------------------------------------------------------------------- |
| 292 | Configuração rsyslog | Verificar configuração de logs<br>Comando: `cat /etc/rsyslog.conf`<br>Verificar se tem configurações personalizadas |
| 293 | Logs funcionando     | Verificar se logs estão sendo gerados<br>Comando: `sudo less /var/log/syslog` <br>Deve mostrar atividade do sistema |

### Categoria: Versionamento (990-versionamento.yml)

| ID  | Teste                 | Como testar                                                                                                                     |
| --- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| 991 | Arquivo versão criado | Verificar arquivo de versão<br>Comando: `cat /usr/local/cmc/version`<br>Deve conter informações da versão no formato `ET-X.Y.Z` |

### Categoria: Arquivos Modificados (999-arquivos-modificados.yml)

| ID  | Teste                    | Como testar                                                                                                                            |
| --- | ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| 996 | Links simbólicos criados | Verificar links para arquivos modificados<br>Comando: `ls -lh /usr/local/cmc/modificados`<br>Deve conter links para arquivos alterados |

## Teste Final Integrado

| ID   | Teste                    | Como testar                                                                                                                                                                                                                                                                                            |
| ---- | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1001 | Reinicialização completa | Reiniciar sistema e verificar se tudo funciona<br>1. Reiniciar a máquina<br>2. Fazer login com usuário do domínio<br>3. Verificar se desktop carrega corretamente<br>4. Abrir aplicativos principais (Chrome, Firefox)<br>5. Verificar conectividade de rede<br>6. Testar acesso a recursos do domínio |
| 1002 | Performance geral        | Verificar se sistema está responsivo<br>Abrir múltiplas aplicações e verificar performance                                                                                                                                                                                                             |
