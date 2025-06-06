Documentação do Projeto de GUI

Visão Geral

Este projeto é um sistema de GUI modular e customizável, desenvolvido em Rust, com suporte para múltiplos temas, layouts responsivos, barras de progresso, GUIs primárias e secundárias, componentes estilo React, e um sistema de watchdog para detectar travamentos.

A configuração da interface é feita através de um arquivo .gui.dnr, que permite aos desenvolvedores gerenciar a estrutura, as configurações e as funcionalidades da GUI.

Sumário

Configuração Principal e Janela
Registro de Funções e Callbacks de Eventos
Suporte a Múltiplos Idiomas
Configuração de Temas
Tela e Layouts Responsivos
Atlas de Textura e Sub-Regiões
GUIs Primárias e Secundárias
Contêineres para Organização Relativa
Sistema de Componentes Estilo React
Barras de Progresso
Sistema de Watchdog para Detecção de Travamentos
Exemplo Completo do Arquivo .gui.dnr
Prompts para Abrir o Chat

1. Configuração Principal e Janela
Define as propriedades principais da janela, incluindo:

NG: Nome da GUI.
language: Idioma padrão usado pelo sistema de tradução.
resizable: Define se a janela pode ser redimensionada.
minimizable: Define se a janela pode ser minimizada.
on_start: Callback para inicialização da GUI.
on_close: Callback para fechamento da GUI.

2. Registro de Funções e Callbacks de Eventos
A seção [fn] registra as funções permitidas pela GUI. Cada função é definida com:

fname: Nome da função em si.
type: Define se a função é síncrona (sync) ou assíncrona (async).
desc: Descrição da função.

3. Suporte a Múltiplos Idiomas
O TranslationManager carrega arquivos de idiomas (en.lang, pt.lang, etc.) em formato chave-valor. Os textos dentro da GUI fazem referência a chaves de tradução ao invés de textos fixos.

Exemplo de arquivo .lang:

lang
Copy code
button_confirm = "Confirmar"
text_welcome = "Bem-vindo ao app"

4. Configuração de Temas
Temas permitem customização de cores e estilos, definidos como [themes.nome_do_tema]. Cada elemento da GUI pode herdar o tema ativo, facilitando a troca entre temas claro e escuro.

toml
Copy code
[themes.light]
background_color = "#ffffff"
text_color = "#000000"

[themes.dark]
background_color = "#333333"
text_color = "#ffffff"

5. Tela e Layouts Responsivos
Suporta valores absolutos e em porcentagem para largura, altura e posição, permitindo design responsivo. A seção [screen] define as dimensões principais e a cor de fundo da GUI.

toml
Copy code
[screen]
width = "100%"
height = "100%"
background_color = "themes.light.background_color"

6. Atlas de Textura e Sub-Regiões
O sistema TextureAtlas suporta vários formatos de imagem (PNG, JPEG, BMP). Define uma textura única e permite extrair subimagens específicas.

toml
Copy code
[texture]
path = "assets/textures/atlas.png"

[texture.button_confirm]
x = 0
y = 0
width = 64
height = 64

7. GUIs Primárias e Secundárias
O sistema de GUI suporta múltiplas camadas:

GUIs Primárias (interfaces principais).
GUIs Secundárias (modais, popups).
Cada camada de GUI possui propriedades para gerenciar cor de fundo, opacidade e visibilidade.

toml
Copy code
[main_gui]
type = "gui"
is_primary = true
background_color = "#ffffff"

[modal_settings]
type = "gui"
is_primary = false
is_modal = true
background_opacity = 0.8
background_color = "#555555"

8. Contêineres para Organização Relativa
Contêineres atuam como elementos de agrupamento, similar a divs em HTML. Elementos dentro de um contêiner possuem posições relativas.

toml
Copy code
[container_sidebar]
type = "container"
position_x = "0%"
position_y = "0%"
width = "20%"
height = "100%"
background_color = "#f1f1f1"

[container_sidebar.button_home]
type = "button"
text = "Home"
position_x = "5%"
position_y = "10%"
width = "90%"
height = "10%"
text_color = "#000000"
background_color = "#cccccc"
action = "confirm"

9. Sistema de Componentes Estilo React
O sistema de componentes permite múltiplas interfaces em uma mesma janela, sendo cada uma ativável individualmente.

toml
Copy code
[component_dashboard]
type = "component"
is_active = true
position_x = "20%"
position_y = "0%"
width = "80%"
height = "100%"
background_color = "#ffffff"

[component_dashboard.button_refresh]
type = "button"
text = "Atualizar"
position_x = "10%"
position_y = "10%"
width = "15%"
height = "8%"
text_color = "#ffffff"
background_color = "#4CAF50"
action = "load_data"

10. Barras de Progresso
Exibe o progresso de uma tarefa com propriedades como min_value, max_value, current_value, e orientation.

toml
Copy code
[progress_bar_loading]
type = "progress_bar"
position_x = "20%"
position_y = "90%"
width = "60%"
height = "5%"
min_value = 0
max_value = 100
current_value = 0
orientation = "horizontal"
color = "#007bff"
background_color = "#cccccc"

11. Sistema de Watchdog para Detecção de Travamentos
O sistema de watchdog monitora a GUI e detecta travamentos com uma thread separada. A GUI envia heartbeats regulares para o watchdog. Se um timeout ocorrer, um callback é executado (ex.: log, reinicialização do app).

12. Exemplo Completo do Arquivo .gui.dnr
Aqui está um exemplo completo do arquivo .gui.dnr, combinando todas as funcionalidades descritas.

toml
Copy code
# Nome e configurações gerais da GUI
NG = "MyEnhancedWindow"
language = "en"

# Configurações da Janela
[window]
resizable = true
minimizable = true
on_start = "initialize_gui"
on_close = "cleanup_resources"

# Registro de funções e eventos permitidos
[fn]
initialize_gui = { fname = "start_up", type = "sync", desc = "Inicializa a GUI" }
cleanup_resources = { fname = "close_app", type = "sync", desc = "Limpa recursos" }
confirm = { fname = "on_click_confirm", type = "sync", desc = "Confirma a ação" }
load_data = { fname = "load_app_data", type = "async", desc = "Carrega dados da aplicação" }

# Configuração de Temas
[themes.light]
background_color = "#ffffff"
text_color = "#000000"

[themes.dark]
background_color = "#333333"
text_color = "#ffffff"

# Tela e dimensões
[screen]
width = "100%"
height = "100%"
background_color = "themes.light.background_color"

# Atlas de textura e sub-regiões
[texture]
path = "assets/textures/atlas.png"
[texture.button_confirm]
x = 0
y = 0
width = 64
height = 64
[texture.icon_check]
x = 64
y = 0
width = 32
height = 32

# GUIs primárias e secundárias
[main_gui]
type = "gui"
is_primary = true
background_color = "#ffffff"
[modal_settings]
type = "gui"
is_primary = false
is_modal = true
background_opacity = 0.8
background_color = "#555555"

# Contêiner Sidebar para botões
[container_sidebar]
type = "container"
position_x = "0%"
position_y = "0%"
width = "20%"
height = "100%"
background_color = "#f1f1f1"
[container_sidebar.button_home]
type = "button"
text = "Home"
position_x = "5%"
position_y = "10%"
width = "90%"
height = "10%"
text_color = "#000000"
background_color = "#cccccc"
action = "confirm"

# Componentes dinâmicos estilo React (dashboard)
[component_dashboard]
type = "component"
is_active = true
position_x = "20%"
position_y = "0%"
width = "80%"
height = "100%"
background_color = "#ffffff"
[component_dashboard.button_refresh]
type = "button"
text = "Atualizar"
position_x = "10%"
position_y = "10%"
width = "15%"
height = "8%"
text_color = "#ffffff"
background_color = "#4CAF50"
action = "load_data"

# Barra de progresso
[progress_bar_loading]
type = "progress_bar"
position_x = "20%"
position_y = "90%"
width = "60%"
height = "5%"
min_value = 0
max_value = 100
current_value = 0
orientation = "horizontal"
color = "#007bff"
background_color = "#cccccc"
13. Prompts para Abrir o Chat
Para configurar uma interação de chat com este projeto de GUI, considere os prompts como:

"Quais são os recursos principais do meu sistema de GUI em Rust?"
"Como configurar temas e traduções no .gui.dnr?"
"Qual é a estrutura de um arquivo completo .gui.dnr para o sistema de GUI?"
"Como detectar travamentos na aplicação GUI usando o watchdog?"
Este documento serve como guia completo para configurar e gerenciar seu sistema de GUI em Rust, cobrindo desde a configuração de layout até o monitoramento de travamentos.
