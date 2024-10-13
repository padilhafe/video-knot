# Instalação DNS Recusrivo Knot
## Criar do usuário do Knot
```bash
useradd -r -M -s /usr/sbin/nologin knot-resolver
```
## Criar entrada tmpfs no fstab
```bash
echo "tmpfs /var/cache/knot-resolver tmpfs rw,size=11636M,uid=knot-resolver,gid=knot-resolver,nosuid,nodev,noexec,mode=0700 0 0" | tee -a /etc/fstab

mount -a
```

## Desligar o servidor DNS local do Ubuntu
```bash
systemctl stop systemd-resolved
systemctl disable systemd-resolved
rm /etc/resolv.conf
echo "nameserver 8.8.8.8" > /etc/resolv.conf
echo "nameserver 8.8.4.4" > /etc/resolv.conf
```

## Instalaçãao do repositório do Knot
```bash
apt-get update
apt-get -y install apt-transport-https ca-certificates wget

wget -O /usr/share/keyrings/cznic-labs-pkg.gpg https://pkg.labs.nic.cz/gpg

echo "deb [signed-by=/usr/share/keyrings/cznic-labs-pkg.gpg] https://pkg.labs.nic.cz/knot-resolver jammy main" > /etc/apt/sources.list.d/cznic-labs-knot-resolver.list 

apt update
```

## Instalação do Knot
```bash
apt-get install knot-resolver knot-resolver-module-http knot-dnsutils
```

## Configurar o Knot
```bash
nano /etc/knot-resolver/kresd.con
```

## Habilitar e iniciar os serviços
```bash
systemctl enable kresd@{1..7}
systemctl start kresd@{1..7}
```