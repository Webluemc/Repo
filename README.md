wget -O bios64.bin "https://github.com/BlankOn/ovmf-blobs/raw/master/bios64.bin"
wget -O win10.iso "https://go.microsoft.com/fwlink/p/?LinkID=2195280&clcid=0x409&culture=en-us&country=US"
wget -O ngrok.tgz "https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-linux-amd64.tgz"
tar -xf ngrok.tgz
rm -rf ngrok.tgz
./ngrok config add-authtoken 2BwGKVG7zradGaHgOZd7TNM1aW7_2cQuRHEpmv2GVqHeAUjDF
./ngrok tcp 5900
sudo apt update
sudo apt install qemu-kvm -y
qemu-img create -f raw win10.img
qemu-img create -f raw win10.img 32G
sudo qemu-system-x86_64 -m 11G -cpu host -boot order=c -drive file=win10.iso,media=cdrom -drive file=win10.img,format=raw -device usb-ehci,id=usb,bus=pci.0,addr=0x4 -device usb-tablet -vnc :0 -smp cores=4 -device rtl8139,netdev=n0 -netdev user,id=n0 -vga qxl -enable-kvm -accel kvm -bios bios64.bin
sudo apt-get update && sudo apt-get upgrade
DISM /Online /Set-Edition:ServerDatacenter /ProductKey:WX4NM-KYWYW-QJJR4-XV3QB-6VM33 /AcceptEula
