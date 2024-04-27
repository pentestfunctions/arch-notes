
# Always make sure the internet adapter is started otherwise blackarch has issues setting up. Makes it easier.
systemctl start dhcpcd

512M (Set to EFI Partition type)
6G (Set to Linux Swap)
Rest of storage to drive /dev/sda3

# Only need to do this if I forget to configure dhcpcd during install
echo "[Match]" > filename.conf
echo "Name=en*" >> filename.conf
echo "Name=eth*" >> filename.conf
echo "" >> filename.conf
echo "[Network]" >> filename.conf
echo "DHCP=yes" >> filename.conf
sudo systemctl start systemd-networkd
sudo systemctl start systemd-resolved

# Install xfce shit
pacman -S xfce4 xfce4-goodies --noconfirm

# Install all the shit we need
sudo pacman -Syu go vi xrdp hyperv ufw sddm nano --noconfirm

# Set the login screen
sudo systemctl set-default graphical.target
sudo systemctl enable sddm

# Add ourselves to sudo
sudo EDITOR=nano visudo
### robot ALL=(ALL) ALL

## Install yay
sudo pacman -S --needed base-devel git
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
yay --version

# Setup RDP
sudo ufw allow 3389/tcp
echo "exec startxfce4" >> ~/.xinitrc
systemctl enable xrdp
systemctl start xrdp
sudo systemctl enable --now hv_fcopy_daemon.service
sudo systemctl enable --now hv_kvp_daemon.service
sudo systemctl enable --now hv_vss_daemon.service
