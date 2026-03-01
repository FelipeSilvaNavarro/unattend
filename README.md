# autounattend.xml — Instalação automatizada do Windows (pt-BR) [file:1]

Este `autounattend.xml` automatiza a instalação do Windows e executa uma rotina de pós-instalação para padronizar idioma/regionais, criar um usuário local administrador, simplificar o OOBE e aplicar ajustes/remoções (“debloat”) via PowerShell e Registro. [file:1]  
Os scripts de automação ficam embutidos no próprio XML (seção `Extensions`), são extraídos para `C:\Windows\Setup\Scripts\` e executados em fases específicas do Setup. [file:1]

---

## Fluxo por fase [file:1]

### 1) Boot / Windows PE (instalador) [file:1]
- Define idioma/locale/teclado do Setup para **pt-BR**. [file:1]
- Aceita a EULA e segue com intervenção mínima (interface aparece apenas em caso de erro). [file:1]
- Mantém o particionamento em modo **interativo** (não força reparticionamento automático do disco). [file:1]

### 2) Specialize (pré-OOBE) [file:1]
- Extrai os scripts do XML para `C:\Windows\Setup\Scripts\`. [file:1]
- Executa `Specialize.ps1` para aplicar ajustes no nível do sistema (máquina). [file:1]
- Remove aplicativos provisionados e algumas “capabilities” do Windows (debloat). [file:1]
- Aplica políticas/ajustes de experiência (ex.: Start com pins vazios, redução de sugestões/consumer features e ajustes relacionados a Edge/Chat/OneDrive). [file:1]
- Prepara o perfil padrão (Default User) carregando `NTUSER.DAT`, aplicando configurações e descarregando o hive ao final. [file:1]
- Gera logs de execução em `C:\Windows\Setup\Scripts\`. [file:1]

### 3) OOBE (primeira experiência) [file:1]
- Define idioma/teclado/locale do sistema para **pt-BR**. [file:1]
- Cria o usuário local `Usuario` no grupo **Administrators**. [file:1]
- Configura **AutoLogon por 1 vez** para concluir o pós-instalação automaticamente. [file:1]
- Simplifica o OOBE (ex.: oculta EULA e pula a etapa de Wi‑Fi). [file:1]

### 4) First Logon (pós-instalação) [file:1]
- Executa `FirstLogon.ps1` via `FirstLogonCommands`. [file:1]
- Instala softwares e pré-requisitos (ex.: Firefox via `winget`, VC++ Redistributable x64) e habilita .NET 3.5 quando a mídia `sources\sxs` estiver disponível. [file:1]
- Opcionalmente executa um instalador `Ninite*Installer.exe` se encontrado em alguma unidade. [file:1]
- Faz limpeza de artefatos do unattended (incluindo `C:\Windows\Panther\unattend.xml`) e registra logs. [file:1]

---

## Logs e arquivos gerados [file:1]
- Scripts: `C:\Windows\Setup\Scripts\*.ps1` (extraídos do bloco `Extensions`). [file:1]
- Logs: `C:\Windows\Setup\Scripts\Specialize.log`, `DefaultUser.log`, `FirstLogon.log`, além de logs de remoções (packages/capabilities). [file:1]

---

## Atenção [file:1]
- Este arquivo contém credenciais/senha em **texto puro** para a conta de automação. [file:1]
- Se o repositório for público, remova/mascare a senha antes de publicar (ou substitua por um fluxo seguro). [file:1]
- Algumas configurações reduzem proteções/alertas (ex.: SmartScreen/relacionados) e alteram políticas do PowerShell; revise antes de usar em produção. [file:1]
