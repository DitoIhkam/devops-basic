# Firewall Limitations

Ini akan dipelajari nanti, berhubungan dengan security. Untuk sekarang disable terlebih dahulu firewall agar proses berjalan lancar

# Set Up Docker's `apt` repo

## Add Docker's official GPG key:
```
sudo apt-get update
```
```
sudo apt-get install ca-certificates curl
```
```
sudo install -m 0755 -d /etc/apt/keyrings
```
```
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```
```
sudo chmod a+r /etc/apt/keyrings/docker.asc
```
## Add the repository to Apt sources:
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```
sudo apt-get update
```

# Instal the docker packages

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

# Verify that the installation is successful by running the hello-world image:
```
sudo docker run hello-world
```
OR
Check the docker version
```
docker version
```
> docker version tanpa sudo dan dengan sudo tampilannya akan berbeda
# Post-installation steps for Docker Engine

Mengelola Docker sebagai pengguna non-root

1. Masalah: 
- Secara default, Docker daemon berjalan sebagai pengguna root. Ini berarti Anda harus menggunakan perintah sudo sebelum setiap perintah docker, yang bisa menjadi merepotkan.
2. Solusi:
- Buat sebuah grup pengguna bernama docker.
- Tambahkan pengguna yang ingin menggunakan Docker tanpa sudo ke dalam grup docker.
- Ketika Docker daemon dimulai, ia akan membuat sebuah soket Unix yang dapat diakses oleh anggota grup docker.
- Beberapa distribusi Linux secara otomatis membuat grup ini saat Anda menginstal Docker menggunakan manajer paket.

## Buat grup docker
```
sudo groupadd docker
```

```
sudo usermod -aG docker $USER
```

## Tambahkan User kita ke grup docker
```
sudo usermod -aG docker $USER
```
> cek variable $USER itu menggunakan `echo $USER`

## Logout dan login lagi

gunakan command dibawah untuk mengecek perbedaan sebelum dan sesudah. Saya sudah jelaskan sebelumnya bahwa mengecek docker version dengan dan tanpa sudo akan berbeda. tetapi sekarang tanpa sudo akan serasa menggunakan sudo karna sudah diatur.
```
docker version
```

# Sumber 
## Docker Install
https://docs.docker.com/engine/install/ubuntu/

## Post Installation
https://docs.docker.com/engine/install/linux-postinstall/

# Tambahan : Ansible

Silahkan copy script dibawah dan generate menggunakan ansible

```
sudo apt-get update
sudo apt-get install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo groupadd docker
sudo usermod -aG docker $USER
```

### Catatan

Beberapa kali saya generate script diatas gagal di apt repo nya yang akhirnya finalnya bisa didapat disini
[Install docker using ansible](https://github.com/DitoIhkam/devops-basic/blob/main/docker/installation/source/docker_install_ansible.yml)
