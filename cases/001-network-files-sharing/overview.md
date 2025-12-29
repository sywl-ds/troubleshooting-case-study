# NFS Sharing (Linux ↔ Linux)

## Troubleshooting & Operational Notes

Dokumentasi ini berisi catatan troubleshooting, validasi, dan praktik terbaik saat mengimplementasikan **NFS (Network File System)** untuk sharing folder **Linux ke Linux** pada jaringan internal.

---

## 📌 Scope

- Linux Server sebagai **NFS Server**
- Linux Client sebagai **NFS Client**
- Jaringan LAN / Private Network
- Tidak mencakup NFS over WAN / Internet

---

## 🏗️ Arsitektur

- bisa dilihat dari topology-nfs.drawio

---

## Sisi Server

#### 1️⃣ Install NFS Server pada Komputer yang bertindak sebagai sharing-center

> sudo apt update
> sudo apt install nfs-kernel-server -y

#### 2️⃣ Buat folder sharing

> sudo mkdir -p /srv/sharing
> sudo chown nobody:nogroup /srv/sharing
> sudo chmod 2775 /srv/sharing

#### 3️⃣ Atur Export NFS

> sudo nano /etc/exports

Tambahkan :

> /srv/sharing 192.168.1.0/24(rw,sync,no_subtree_check)

Ganti 192.168.1.0/24 sesuai dengan subnet komputer server

#### 4️⃣ Aktifkan NFS

> sudo exportfs -a
> sudo systemctl restart nfs-server
> sudo systemctl enable nfs-server

---

## Sisi Client

#### 1️⃣ Install NFS Client

> sudo apt install nfs-common -y

#### 2️⃣ Mount Folder

> sudo mkdir /mnt/sharing
> sudo mount 192.168.1.10:/srv/sharing /mnt/sharing

#### 3️⃣ Auto Mount

> 192.168.1.10:/srv/sharing /mnt/sharing nfs defaults 0 0
