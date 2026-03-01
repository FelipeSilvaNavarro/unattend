# autounattend.xml — Instalação automatizada do Windows (pt-BR)

Este `autounattend.xml` automatiza a instalação do Windows e executa rotinas de pós-instalação para deixar o sistema pronto, com idioma pt-BR, menos telas no OOBE, menos apps “pré-instalados” e algumas preferências já ajustadas.

Ele também embute scripts que são extraídos para `C:\Windows\Setup\Scripts\` e executados em fases diferentes do Setup.

---

## O que ele faz (por fase)

### 1) Boot / Windows PE (instalador)
- Define idioma/teclado/região do instalador para **pt-BR**.
- Aceita a EULA e reduz interações (tende a mostrar telas só quando precisa).
- Mantém o particionamento em modo **interativo** (você ainda escolhe/confirma disco/partições).

### 2) Specialize (antes do OOBE)
- Extrai e executa os scripts de customização do próprio XML.
- Remove uma lista de apps provisionados (exemplos):
  - 3D Viewer, Bing Search, Dev Home, Family, Feedback Hub, Get Help/Get Started,
  - News/Weather, People, Skype, Teams (MicrosoftTeams/MSTeams), Wallet, Your Phone, Zune Video,
  - Outlook (app), OneNote (app), Office Hub, Paint (app) e outros.
- Remove o “Steps Recorder” (recurso/capability do Windows).
- Desativa a instalação automática do “Chat/Teams” integrado.
- Remove atalhos/instaladores do OneDrive que costumam reaparecer no menu/inicialização.
- Ajusta o menu Iniciar para vir “limpo” (sem pins por padrão).
- Ativa o plano de energia **Ultimate Performance** (quando disponível).
- Define algumas políticas e preferências gerais do sistema (ex.: reduzir sugestões/conteúdo promocional, e ajustes do Edge para não “encher o saco” no primeiro uso — ele **não desinstala** o Edge, só ajusta comportamento).

### 3) OOBE (primeira experiência)
- Mantém o sistema em **pt-BR**.
- Cria o usuário local `Usuario` como **Administrador**.
- Faz **auto logon apenas uma vez** para conseguir rodar o pós-instalação automaticamente.
- Oculta a página da EULA e pula a etapa de Wi‑Fi no OOBE.

### 4) First Logon (pós-instalação)
- Instala o **Firefox** via `winget` (quando compatível).
- Baixa e instala o **Microsoft Visual C++ Redistributable (x64)** em modo silencioso.
- Habilita o **.NET Framework 3.5** usando `sources\sxs` (quando encontra a mídia/arquivos).
- Executa um instalador **Ninite** (`Ninite*Installer.exe`) automaticamente, se você colocar esse arquivo em alguma unidade/pendrive durante a instalação.
- Faz limpeza de arquivos do unattended ao final e salva logs.

---

## Ajustes de “preferências” aplicados
- Mostra extensões de arquivo por padrão (mais fácil ver `.ps1`, `.bat`, `.txt`, etc.).
- Deixa o Explorer abrindo em “Este Computador”.
- Esconde a busca da barra de tarefas (fica mais limpo).
- Habilita o menu de contexto “clássico” (estilo Windows 10).
- Desativa “Sticky Keys” e remove aceleração do mouse (sensação mais consistente).
- Habilita a opção de “End task” na barra de tarefas (quando disponível).

---

## Logs e rastreabilidade
- Scripts: `C:\Windows\Setup\Scripts\*.ps1`
- Logs: `C:\Windows\Setup\Scripts\Specialize.log`, `DefaultUser.log`, `FirstLogon.log`
  - Também existem logs específicos de remoção de apps/capabilities.

---

## Atenção
- Este arquivo contém credenciais/senha em **texto puro** para a conta de automação.
- Se o repositório for público, remova/mascare a senha antes de publicar.
- Algumas escolhas reduzem proteções/alertas do Windows e mudam políticas do PowerShell; revise antes de usar em produção.
