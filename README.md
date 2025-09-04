# Projetos HTML-CSS
 
Site da confraria feito em parceria com Gabriel de Souza(dev) e Gabriel Carvalho(dsg). Seus contatos podem ser encontrados na parte de baixo do site(footer) do index.html.

<a href="https://colegioobjetivojuazeiroba.com/confraria3ano/">site confraria literaria</a>









#!/bin/bash

# Atualiza pacotes
sudo apt update && sudo apt upgrade -y

# --- 1️⃣ Instalar Flatpak e Flathub ---
sudo apt install flatpak gnome-software-plugin-flatpak -y
sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# --- 2️⃣ Instalar utilitários GNOME ---
sudo apt install gnome-shell-extensions gnome-tweaks -y

# --- 3️⃣ Instalar extensões GNOME populares ---
declare -A extensions=(
  ["Dash to Dock"]="dash-to-dock@micxgx.gmail.com"
  ["User Themes"]="user-theme@gnome-shell-extensions.gcampax.github.com"
  ["Caffeine"]="caffeine@patapon.info"
  ["Clipboard Indicator"]="clipboard-indicator@tudmotu.com"
  ["GSConnect"]="gsconnect@andyholmes.github.io"
)

for ext in "${!extensions[@]}"; do
    sudo apt install "gnome-shell-extension-${ext,,}" -y 2>/dev/null
    gnome-extensions enable "${extensions[$ext]}" 2>/dev/null
done

# --- 4️⃣ Instalar Brave ---
sudo apt install apt-transport-https curl -y
sudo curl -fsSLo /usr/share/keyrings/brave-browser-archive-keyring.gpg https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg] https://brave-browser-apt-release.s3.brave.com/ stable main" | sudo tee /etc/apt/sources.list.d/brave-browser.list
sudo apt update
sudo apt install brave-browser -y

# --- 5️⃣ Remover Firefox ---
sudo apt remove firefox -y
sudo apt autoremove -y

# --- 6️⃣ Instalar Bibata Modern Ice (cursor) ---
mkdir -p ~/.icons
cd ~/.icons || exit
if [ ! -d "Bibata-Modern-Ice" ]; then
    git clone https://github.com/ful1e5/Bibata_Cursor.git
fi

cd Bibata_Cursor || exit
# Copiar apenas tema Modern Ice
cp -r Bibata-Modern-Ice ~/.icons/

# Aplicar o cursor Bibata Modern Ice
gsettings set org.gnome.desktop.interface cursor-theme "Bibata-Modern-Ice"
gsettings set org.gnome.desktop.interface cursor-size 24

# Corrigir cursor em apps Flatpak (como Brave)
mkdir -p ~/.var/app/com.brave.Browser/config/
ln -sf ~/.icons/Bibata-Modern-Ice ~/.var/app/com.brave.Browser/config/icons
flatpak override --user --env=GTK_CURSORSIZE=24 --env=XCURSOR_THEME=Bibata-Modern-Ice com.brave.Browser

# --- 7️⃣ Instalar GIMP e Inkscape ---
sudo apt install gimp inkscape -y

echo "✅ Tudo pronto! Reinicie a sessão (ou o PC) para que o cursor, extensões e aplicativos tenham efeito."
