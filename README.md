# Instalacion-y-configuracion-Hyprland-Arch-Linux
Instalación y configuración de Hyprland en una instalación base de Arch Linux

Instalar previamente

Apps
pacman - S curl dolphin dunst fastfetch firefox git grim htop hyprland iwd kitty nano micro openssh poltik-kde-agent qt5-wayland qt6-wayland slurp smartmontools uwsm vim wget Wireless_tools wofi wpa_supplicant xdg-desktop-portal-hyprland xdg-utils xdg-user-dirs

Drivers
intel-media-driver
libva-intel-driver
libva-mesa-driver
mesa
vulkan-intel
vulkan-nouveau
vulkan-radeon
xf86-video-amdgpu
xf86-video-ati
xf86-video-nouveau
xorg-server
xorg-xinit

*****************************************************************
Configurar teclado en español

nano .config/hypr/hyprland.conf

Ubicar el apartado INPUT y la linea 

kb_layout = us

cambiar el valor "us" a "es"

kb_layout = es

*****************************************************************

Configurar orden de monitores de izquierda a derecha

Detectar todos los monitores y sus resoluciones 
hyprctl monitors

Una vez obtenida la información hay que modificar el orden en el archivo de configuración de hyprland

nano .config/hypr/hyprland.conf

Buscar apartado de MONITORS

eliminar la linea

monitor=,preferred,auto,auto

y listar los monitores según el orden que necesites indicando en el tercer valor, el posicionamiento en pixeles si va de izquierda a derecha o viceversa 

monitor=HDMI-A-2,1920x1080,1920x0,1
monitor=DP-2,1600x900,auto,1

para mas referencias de configuración de los monitores, consulta la wiki
https://wiki.hypr.land/Configuring/Monitors/

*****************************************************************

Cambiar combinación de teclas para iniciar una terminal y cerrar ventanas

nano .config/hypr/hyprland.conf

Ubicar el apartado KEYBINDINGS y la linea 

bind = $mainMod, Q, exec, $terminal
bind = $mainMod, C, killactive,


Cambiar el valor "Q" a "T" y el "valor "C" a "Q"

bind = $mainMod, Q, exec, $terminal
bind = $mainMod, T, killactive,


*****************************************************************
Cambiar configuración de los bordes de las ventanas que se abren

nano .config/hypr/hyprland.conf

Ubicar el apartado LOOK AND FEEL

Cambiar el valor de la línea 

col.active_border = rgba(33ccffee) rgba(00ff99ee) 45deg

por 

col.active_border = rgba(56b6c2aa)

*****************************************************************
Activar redimensionamiento de ventana con cursor

nano .config/hypr/hyprland.conf

Ubicar el apartado LOOK AND FEEL

Cambiar el valor de la línea 

resize_on_border = false

por

resize_on_border = true

*****************************************************************

Activar carpetas por defecto de documentos de usuario

xdg-user-dirs-update

ó 

mkdir Descargas Documentos Escritorio Imagenes Musica Plantillas Publico Videos

*****************************************************************

Instalar tipografías

JetBrainsMono Nerd Font Propo
sudo pacman -S ttf-jetbrains-mono-nerd



*****************************************************************

Lanzador de aplicaciones

Instalar rofi

sudo pacman -S rofi

Configurar rofi

Seleccionar el tipo y estilo del menú del lanzador
Para visualizar los tipos y estilos disponibles consulta el repositorio
https://github.com/adi1090x/rofi.git

Descargar layouts y lanchers
git clone https://github.com/adi1090x/rofi.git --depth=1
(el --depth=1 es para traer el commit mas reciente sin el hostorial)

mkdir .config/rofi
cd rofi/files 
cp -r config.rasi colors launchers/ ~/.config/rofi
cd 
rm -rf rofi

una vez que seleccionado el tipo y estilo a utilizar, hay que ingresar a los archivos correspondientes
en este paso se va a usar el tipo 2 estilo 2

nano .config/rofi/launchers/type-2/launcher.sh

Ubicamos la linea

theme='style-1'

y la cambiamos por 

theme='style-2'

Cambiar tipografia que utilizara Rofi

nano .config/rofi/launchers/type-2/shared/fonts.rasi

cambiar 

* {
font: "Iosevka Nerd Font 10";
}

por 

* {
font: "JetBrainsMono Nerd Font Propo 10";
}

Revisar si la fuente se ajusta adecuadamente a todo, si no se tiene que agregar la fuente localmente dentro del archivo de configuración base de cada estilo y ajustar el tamaño de la misma

En el caso de icono de lupa, se agrega la fuente y tamaño en el apartado de Inputbar / prompt

Configurar combo de teclas para ejecutar esta configuración en Hyprland
nano .config/hypr/hyprland.conf

configuración de teclas general de hyprland

Ubicar el apartado KEYBINDINGS y la linea 

bind = $mainMod, R, exec, $menu

y cambiar por 

bind =$mainMod, R, exec, ~/.config/rofi/launchers/type-2/launcher.sh || pkill rofi

Activar mostrar rofi al iniciar hyprland

nano .config/hypr/hyprland.conf

Ubicar el apartado AUTOSTART y agregar la linea 

exec-once = ~/.config/rofi/launchers/type-2/launcher.sh

*****************************************************************

Instalar asistente de instalación de paquetes AUR

actualizar la lista de complementos de git para crear compilar programas de repositorios de GitHub
sudo pacman -S --needed git base-devel 

Descargar e instalar el asistente yay
git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si   

*****************************************************************

Instalar navegador Brave

yay brave-bin

*****************************************************************

Cambiar fondos de pantalla
Descargar fondo de pantalla a usar y guardar en sitio Imagenes/Wallpaper

Instalar el gestor hyprpaper
sudo pacman -S hyprpaper

Crear y editar archivo de configuración
touch .config/hypr/hyprpaper.conf
nano .config/hypr/hyprpaper.conf

wallpaper {
    monitor = 
    path = Imagenes/Wallpaper/archivo.ext
    fit_mode = cover
}
#end

Iniciar servicio con hyprland

nano .config/hypr/hyprland.conf

Ubicar el apartado AUTOSTART y confirmar que siguiente linea no esta comentada
exec-once = waybar & hyprpaper

//////////////////////////////////////////////

Usar con swww

Instalar swww
sudo pacman -S swww

Activar el gestor swww
swww-deamon & disown

Iniciar servicio con hyprland

nano .config/hypr/hyprland.conf

Ubicar el apartado AUTOSTART y agregar la linea 
exec-once = swww


Vincular swww con el wallpaper
swww img Imagenes/Wallpaper/archivo.ext

*******************************************

cambiar bash por zsh

chsh -s /bin/zsh

*******************************************
Notificaciones

Instalar servicio de notificaciones
sudo pacman -S swaync

Iniciar servicio con hyprland

nano .config/hypr/hyprland.conf

Ubicar el apartado AUTOSTART y agregar la linea 
exec-once = swaync


comprobar que funcionan
notify-send hello

si no funciona hay que reiniciar todo el servicio y volver a comprobar
killall -9 swaync

para dejarlo activo y en segundo plano
swaync & disown

activar de arranque en hyprland
editar hyprland.conf
exec-once = swaync

*******************************************
Instalar fuente
nerdfonts.com
yay - S GeistMono Nerd Font Mono

*******************************************
configurar ventana de Kitty
editar archivo de configuración (si no esta hay que crearlo)

nano .config/kitty/kitty.conf
font_family GeistMono Nerd Font Mono
font_size 14

#cambiar la manera en que se fusionan los caracteres
#las opciones son yes o no
disable_ligatures no

#deshabilitar el sonido de campana
#las opciones son yes o no
enable_audio_bell no

#definicion de Shell a usar
shell zsh

#integrar opciones por defecto de la Shell en uso
shell_integration disable

#tipo de cursos a mostrar
cursor_shape block

#tipo de subrayado en las urls
url_style curly

#recordar el tamaño de la terminal
remember_window_size no

#definir tamaño iniciar de la ventana
initial_window_width 640
initial_window_height 480


#margenes internos
window_padding_width 15

#intervalo de parpadeo del cursos
cursor_blink_interval 1

#transparencia de la ventana
background_opacity 0.80

#transparencia de la barra de desplazamiento
scrollbar_handle_opacity 0
scrollbar_track_opacity 0
scrollbar_track_hover_opacity 0

#retirar la confirmación de cierre de ventana
confirm_os_window_close 0

#sincronizar refresh de monitor con valor del monitor
sync_to_monitor no

#colores según el tema seleccionado

*******************************************
configuración visual de Kitty


ohmyz.sh
ejecutar el instalar con curl
dar Y en la ventana de inicio

Configurar tema
GitHub powerlevel10k
git clone

nano .zshrc
ZSH_THEME:"powerlevel10k/powerlevel10k"
configurar con el Wizard

Configuración de colores
buscar un tema de colores de Kitty
copiar los colores en el archivo 
nano .config/kitty/kitty.conf

*******************************************
Barra de inicio

Iniciar servicio con hyprland
nano .config/hypr/hyprland.conf

Ubicar el apartado AUTOSTART y confirmar que siguiente linea no esta comentada
exec-once = waybar & hyprpaper


Configuración de Waybar

mkdir .config/waybar .config/waybar/scripts .config/waybar/colors
cd .config/waybar
touch .config/waybar/config.jsonc .config/waybar/style.css .config/waybar/colors/one-dark.css .config/waybar/scripts/launch.sh
cdmod +x .config/waybar/scripts/launch.sh

nano .config/waybar/scripts/launch.sh

#!/bin/bash

waybar

killall -9 waybar
waybar &
#end

waybar wiki


nano config.jsonc

{//https://github.com/Alexays/Waybar/wiki/Module:-Custom:-Menu
	"position": "top", //ubicación de la barra
	"margin-top": 15, //margen de separación de la barra con relación a la pantalla
	"modules-left": ["clock", "hyprland/workspaces"], //bloque de modulos izquierdo
	"modules-center": ["tray"], //bloque de modulos central
	"modules-right": ["pulseaudio", "network", "battery", "custom/notification"], //bloque de modulos derecho

"clock": { #configuración del reloj
	"interval": 60,
	"format": "{:%H:%M},
	"max-length": 25
},
"hyperland/workspaces": {
	"format": "{name}",
	"format-icons": {
		"1": "",
		"2": "",
		"3": "",
		"4": "",
		"5": "",
		"active": ,
		"default": },
"persistent-workspace":{
	"*": 5, // 5 espacios de trabajo por defecto en todos los monitores
	"HDMI-A-1": 3 // solo el escritorio 3 en el monitor nombrado
},
"tray": {
"icon-size": 21,
"spacing": 10
},
"pulseaudio": {
    "format": "{volume}% {icon}",
    "format-bluetooth": "{volume}% {icon}",
    "format-muted": "",
    "format-icons": {
        "alsa_output.pci-0000_00_1f.3.analog-stereo": "",
        "alsa_output.pci-0000_00_1f.3.analog-stereo-muted": "",
        "headphone": "",
        "hands-free": "",
        "headset": "",
        "phone": "",
        "phone-muted": "",
        "portable": "",
        "car": "",
        "default": ["", ""]
    },
    "scroll-step": 1,
    "on-click": "pavucontrol",
    "ignored-sinks": ["Easy Effects Sink"]
}
"network": {
    "interface": "wlp2s0",
    "format": "{ifname}",
    "format-wifi": "{essid} ({signalStrength}%) ",
    "format-ethernet": "{ipaddr}/{cidr} 󰊗",
    "format-disconnected": "", //An empty format will hide the module.
    "tooltip-format": "{ifname} via {gwaddr} 󰊗",
    "tooltip-format-wifi": "{essid} ({signalStrength}%) ",
    "tooltip-format-ethernet": "{ifname} ",
    "tooltip-format-disconnected": "Disconnected",
    "max-length": 50
}
"battery": {
    "bat": "BAT2",
    "interval": 60,
    "states": {
        "warning": 30,
        "critical": 15
    },
    "format": "{capacity}% {icon}",
    "format-icons": ["", "", "", "", ""],
    "max-length": 25
}
  "custom/notification": { //Sway notification center GitHub https://github.com/ErikReider/SwayNotificationCenter
    "tooltip": true,
    "format": "<span size='16pt'>{icon}</span>",
    "format-icons": {
      "notification": "󱅫",
      "none": "󰂜",
      "dnd-notification": "󰂠",
      "dnd-none": "󰪓",
      "inhibited-notification": "󰂛",
      "inhibited-none": "󰪑",
      "dnd-inhibited-notification": "󰂛",
      "dnd-inhibited-none": "󰪑"
    },
    "return-type": "json",
    "exec-if": "which swaync-client",
    "exec": "swaync-client -swb",
    "on-click": "swaync-client -t -sw",
    "on-click-right": "swaync-client -d -sw",
    "escape": true
  },
}
}
#end


nano one-dark.css

@define-color bg0 #282c34;
@define-color bg1 #21252b;
@define-color bg2 #2c313a;
@define-color bg3 #3e4451;
@define-color bg4 #4b5263;

@define-color red #e08c75;
@define-color orange #d19a66;
@define-color yellow #e5c07b;
@define-color green #98c379;
@define-color aqua #56b6c2;
@define-color blue #61afef;
@define-color purple #c678dd;

@define-color fg #abb2bf;

@define-color grey0 #5c6370;
@define-color grey1 #828997;
@define-color grey2 #abb2bf;
#end


nano style.css

@import 'colors/one-dark.css'

* {
border: none;
border-radius: 0px;

Font.family: "Adwaita Sans", "JetBrains Mono Nerd Font Propo", sans-serif;
font-wight: bold;

min-heigth: 0;
padding: 0;
margin:0;
}

window#waybar{
background: transparent;
}

tooltip{
background: @bg0;
border: 1px solid @bg3;
border-radius: 12px;
}

tooltip label{
 color: @fg;
padding: 6px;
}

#workspaces {
background-color: @bg0;
padding: 5px 3px;
margin: 0 0 0 12px;
border-radius: 18px;
border: 1px solid @bg1;
color: @fg;
}


#workspaces button{
padding: 0px 6px;
margin: 0 3px;
border-radius: 50px;
color: transparent;
background-color: @bg1;
transition: all 0.3s ease-in-out;
}

#workspaces button.active{
background-color: @blue;
color: @bg0;
min-width: 45px;
transition: all 0.3s ease-in-out;
font-size: 13px;
}

#workspaces button.hover {
background-color: @purple;
color: @bg0;
border-radius: 16px;
min-width: 45px;
background-size: 400% 400%;
}

#workspaces button.urgent {
background-color: @red;
color: @bg0;
border-radius: 16px;
min-width: 45px;
background-size: 400% 400%;
transition: all 0.3s ease-in-out;
}

#battery,
#pulseaudio,
#network,
#clock,
#custom-notification {
background-color: @bg0;
padding: 0 15px 0 15px;
margin: 0 0 0 12px;
border-radius: 45px;
border: 1px solid @bg1;
}

#clock{
color: @blue
}

#custom-notification{
color: @fg;
margin: 0 12px 0 12px;
}

#pulseaudio{
color: @yellow
}

#network{
color: @purple
}

#battery{
color: @green;
}
#end


abrir Virtual Machine Manager

Edit - Preferencias
enable system tray icon


Abrir GTK Settings
adw-gtk3-dark / aplicar

descargar wallpaper
ponerlo en imágenes carpeta wallpaper
swww img ~./directorio/archivo


***************************************************
Notificaciones

https://www.youtube.com/watch?v=hsltJnBm2g4

***************************************************

Activar Barras de titulo de las ventanas

https://www.youtube.com/watch?v=l8knQtTKJ1Y

***************************************************
Activar pantalla de bloqueo

https://www.youtube.com/watch?v=wuohFzr2QB4

***************************************************

Montar usb de forma automática

https://www.youtube.com/watch?v=yX1fo10rTPo


***************************************************



pacman ((yay)) -S swww hyprpaper zsh rofi waybar swaync --needed  grim nwg-look adw-gtk-theme 

Iniciar gestor de inicio de sesión sddm
sudo systemctl enable sddm
reboot

Configurar tema
https://aur.archlinux.org/packages?O=0&K=sddm
yay -S sddm-sugar-candy-git - Se guardan en /usr/share/sddm/themes
reboot
nano /etc/sddm.conf
theme
current=sddm-sugar-candy-git



Configurar movimientos entre ventanas

Decorador de ventana -  decoration
sombras
opacidad
colores

transparencia - blur

Bordes - General - border_size = 0
colores de bordes


Variables de entorno - Environment Variables para capturar video de pantalla
env = XCURSOR_SIZE, 24
env = HYPTCURSOR_SIZE, 24
env = XDG_CURRENT_DESKTOP, Hyprland
env = XDG_SESSION_TYPE, wayland
env = XDG_SESSION_DESKTOP, Hyprland
env = MQZ_ENABLE_WAYLAND, 1
env = QT_QPA_PLATFORMTHEME, qt6ct
env = QT_QPA_PLATFORM, wayland, xcb
env = QT_AUTO_SCREEN_SCALE_FACTOR,1 
env = QT_WAYLAND_DISABLE_WINDOWS_DECORATION, 1
env = GDK_SCALE, 1

