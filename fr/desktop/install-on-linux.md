# Installer le client Seafile sur Linux

<p><div class="toc">
<ul>
<li><a href="#wiki-ubuntu">Ubuntu</a></li>
<li><a href="#wiki-ubuntu">Debian</a></li>
<li><a href="#wiki-ubuntu">Fedora</a></li>
<li><a href="#wiki-centos">Centos/RHEL</a></li>
<li><a href="#wiki-archlinux">Arch Linux</a></li>
<li><a href="#wiki-cli">Linux CLI</a></li>
</ul>
</p>

## <a id="wiki-ubuntu"></a> Ubuntu

## Ubuntu 17.10, 18.04, 20.04 et plus récentes

Seafile est disponible directement dans Ubuntu, en le cherchant dans Logiciels (catégorie Internet).

Dans certains cas, la version disponible dans Ubuntu 18.04 ne fonctionne pas, vous pouvez suivre les instructions [du site officiel de Seafile](https://help.seafile.com/syncing_client/install_linux_client/).

## <a id="wiki-debian"></a> Debian

### Debian 10 (Buster) ou 11 Bullseye

Si vous utilisez Debian 10 (Buster) ou 11 (Bullseye), Seafile est disponible dans les dépôts standards. Vous pouvez l'installer avec votre logiciel habituel, ou via la ligne de commande (en étant connecté en *root*) :

```sh
apt install seafile-gui
```

### Debian 9 (Stretch)
Si vous utilisez Debian 9 (Stretch), Seafile est disponible dans les [dépôts Backports](https://backports.debian.org/Instructions/).

Pour ajouter les backports, lancez (en étant connecté en *root*) :

```sh
echo deb http://ftp.debian.org/debian stretch-backports main | tee /etc/apt/sources.list.d/backports.list
apt update
```

Puis, pour installer Seafile :

```sh
apt -t stretch-backports install seafile-gui
```

## <a id="wiki-fedora"></a> Fedora (Maintenu par la communauté)

Il y a un paquet RPM du client Seafile maintenu par la communauté dans le [dépôt officiel de Fedora](https://admin.fedoraproject.org/pkgdb/package/rpms/seafile/).

## <a id="wiki-centos"></a> CentOS/RHEL (Maintenu par la communauté)

Il y a un paquet RPM du client Seafile maintenu par la communauté pour CentOS/RHEL https://copr.fedorainfracloud.org/coprs/pkerling/seafile/.

## <a id="wiki-archlinux"></a> Arch Linux (Maintenu par la communauté)

Il y a un paquet maintenu par la communauté pour Arch Linux :

https://aur.archlinux.org/packages/seafile-client/

## <a id="wiki-cli"></a> Utilisation en ligne de commande du client Linux

Consultez [cette documentation](linux-cli.md) pour des instruction sur l'utilisation du client en ligne de commande pour Linux.
