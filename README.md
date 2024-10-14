# Instalação DNS Recusrivo Knot

# Documentação e Referências
- [Documentação](https://knot-resolver.readthedocs.io/en/stable/)
- [Repositorio do Projeto](https://pkg.labs.nic.cz/doc/?project=knot-resolver)
- [Repositório do Código](https://gitlab.nic.cz/knot/knot-resolver)
- [Download Vagrant](https://developer.hashicorp.com/vagrant/install)

## Subir a máquina virtual
- Seguir tutorial conforme seu sistema operacional.
- Executar ```vagrant up``` no diretório que o Vagrantfile estiver.

## Criar do usuário do Knot
```bash
useradd -r -M -s /usr/sbin/nologin knot-resolver
```
## Criar entrada tmpfs no fstab
```bash
echo "tmpfs /var/cache/knot-resolver tmpfs rw,size=14336M,uid=knot-resolver,gid=knot-resolver,nosuid,nodev,noexec,mode=0700 0 0" | tee -a /etc/fstab
mkdir /var/cache/knot-resolver
mount -a
```

## Desligar o servidor DNS local do Ubuntu
```bash
systemctl stop systemd-resolved
systemctl disable systemd-resolved
rm /etc/resolv.conf
echo "nameserver 8.8.8.8" > /etc/resolv.conf
echo "nameserver 8.8.4.4" >> /etc/resolv.conf
```

## Instalaçãao do repositório do Knot
### Pré-requisitos
```bash
apt-get update
apt-get -y install apt-transport-https ca-certificates wget
```

### Configuração do Repositório
```bash
wget -O /usr/share/keyrings/cznic-labs-pkg.gpg https://pkg.labs.nic.cz/gpg
echo "deb [signed-by=/usr/share/keyrings/cznic-labs-pkg.gpg] https://pkg.labs.nic.cz/knot-resolver jammy main" > /etc/apt/sources.list.d/cznic-labs-knot-resolver.list 
apt update
```

## Instalação do Knot
```bash
apt-get install knot-resolver knot-resolver-module-http knot-dnsutils
```

## Configurar o Knot
- Copiar o conteúdo do arquivo kresd.conf para:
```bash
nano /etc/knot-resolver/kresd.conf
```

## Habilitar e iniciar os serviços
- Usar o valor {1..x}, no qual X é o total de núcleos da máquina menos 1.
```bash
systemctl enable kresd@{1..7}
systemctl start kresd@{1..7}
```