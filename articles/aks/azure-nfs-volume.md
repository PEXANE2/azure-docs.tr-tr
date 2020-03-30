---
title: Azure Kubernetes Service (AKS) bölmeleri tarafından kullanılmak üzere bir NFS (Ağ Dosya Sistemi) Ubuntu Sunucusu oluşturun
description: Azure Kubernetes Hizmetinde (AKS) bölmelerle kullanılmak üzere el ile nasıl bir NFS Ubuntu Linux Server hacmi oluşturabilirsiniz öğrenin
services: container-service
author: ozboms
ms.topic: article
ms.date: 4/25/2019
ms.author: obboms
ms.openlocfilehash: e5676710bc47557318f3e2adcf36ec0ed13d47de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77596632"
---
# <a name="manually-create-and-use-an-nfs-network-file-system-linux-server-volume-with-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti (AKS) ile nfs (Ağ Dosya Sistemi) Linux Server hacmini el ile oluşturun ve kullanın
Verileri kapsayıcılar arasında paylaşmak genellikle kapsayıcı tabanlı hizmetlerin ve uygulamaların gerekli bir bileşenidir. Genellikle harici kalıcı birimde aynı bilgilere erişmeniz gereken çeşitli bölmelere sahipsiniz.    
Azure dosyaları bir seçenek olsa da, Azure VM'de NFS Sunucusu oluşturmak, kalıcı paylaşılan depolamanın başka bir biçimidir. 

Bu makalede, bir Ubuntu sanal makinede nasıl bir NFS Sunucusu oluşturacağınızı gösterecektir. Ayrıca AKS kapsayıcılarınıza bu paylaşılan dosya sistemine erişim hakkı verin.

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede, varolan bir AKS Kümeniz olduğunu varsayar. AKS Cluster'a ihtiyacınız varsa, Azure [CLI'yi veya Azure][aks-quickstart-cli] [portalını kullanarak][aks-quickstart-portal]AKS hızlı başlat'ına bakın.

AKS Kümenizin NFS Sunucusuyla aynı veya eşlenen sanal ağlarda yaşaması gerekir. Küme, VM'inizle aynı VNET olabilecek varolan bir VNET'te oluşturulmalıdır.

Varolan bir VNET ile yapılandırma adımları belgelerde açıklanmıştır: [varolan VNET'te AKS Kümesi oluşturma][aks-virtual-network] ve [Sanal ağları VNET eşlemesiyle bağlama][peer-virtual-networks]

Ayrıca bir Ubuntu Linux Sanal Makine (örneğin, 18,04 LTS) oluşturduğunuzvarsa. Ayarlar ve boyut beğendiğiniz olabilir ve Azure üzerinden dağıtılabilir. Linux quickstart için, [Linux VM yönetimine][linux-create]bakın.

Önce AKS Kümenizi dağıtırsanız, Azure Ubuntu makinenizi dağıtırken sanal ağ alanını otomatik olarak doldurarak aynı VNET içinde yaşamalarını sağlar. Ancak bunun yerine eşlenen ağlarla çalışmak istiyorsanız, yukarıdaki belgelere başvurun.

## <a name="deploying-the-nfs-server-onto-a-virtual-machine"></a>NFS Sunucusunu Sanal Makineye Dağıtma
Ubuntu sanal makinenizde bir NFS Sunucusu kurmak için komut dosyası aşağıdaverevettir:
```bash
#!/bin/bash

# This script should be executed on Linux Ubuntu Virtual Machine

EXPORT_DIRECTORY=${1:-/export/data}
DATA_DIRECTORY=${2:-/data}
AKS_SUBNET=${3:-*}

echo "Updating packages"
apt-get -y update

echo "Installing NFS kernel server"

apt-get -y install nfs-kernel-server

echo "Making data directory ${DATA_DIRECTORY}"
mkdir -p ${DATA_DIRECTORY}

echo "Making new directory to be exported and linked to data directory: ${EXPORT_DIRECTORY}"
mkdir -p ${EXPORT_DIRECTORY}

echo "Mount binding ${DATA_DIRECTORY} to ${EXPORT_DIRECTORY}"
mount --bind ${DATA_DIRECTORY} ${EXPORT_DIRECTORY}

echo "Giving 777 permissions to ${EXPORT_DIRECTORY} directory"
chmod 777 ${EXPORT_DIRECTORY}

parentdir="$(dirname "$EXPORT_DIRECTORY")"
echo "Giving 777 permissions to parent: ${parentdir} directory"
chmod 777 $parentdir

echo "Appending bound directories into fstab"
echo "${DATA_DIRECTORY}    ${EXPORT_DIRECTORY}   none    bind  0  0" >> /etc/fstab

echo "Appending localhost and Kubernetes subnet address ${AKS_SUBNET} to exports configuration file"
echo "/export        ${AKS_SUBNET}(rw,async,insecure,fsid=0,crossmnt,no_subtree_check)" >> /etc/exports
echo "/export        localhost(rw,async,insecure,fsid=0,crossmnt,no_subtree_check)" >> /etc/exports

nohup service nfs-kernel-server restart
```
Sunucu yeniden başlatılacaktır (komut dosyası nedeniyle) ve AKS NFS Server monte edebilirsiniz.

>[!IMPORTANT]  
>**AKS_SUBNET** kümenizden doğru olanla değiştirdiğinizden emin olun yoksa NFS Server'ınızı tüm bağlantı noktalarına ve bağlantılara "*" açar.

VM'nizi oluşturduktan sonra yukarıdaki komut dosyasını bir dosyaya kopyalayın. Ardından, yerel makinenizden veya komut dosyasının olduğu yerden VM'ye taşıyabilirsiniz: 
```console
scp /path/to/script_file username@vm-ip-address:/home/{username}
```
Komut dosyanız VM'nize girdikten sonra VM'ye girip komut la çalıştırabilirsiniz:
```console
sudo ./nfs-server-setup.sh
```
İnfazı reddedilen bir izin hatası nedeniyle başarısız olursa, yürütme izni komutu ile ayarlayın:
```console
chmod +x ~/nfs-server-setup.sh
```

## <a name="connecting-aks-cluster-to-nfs-server"></a>AKS Kümesini NFS Server'a Bağlama
NFS Sunucusu'nu, toplua nasıl erişebileceğimizi belirten kalıcı bir hacim ve kalıcı hacim talebi sağlayarak kümemize bağlayabiliriz.

İki hizmeti aynı veya eşlenen sanal ağlarda bağlamak gereklidir. Kümeyi aynı VNET'te ayarlama yönergeleri burada: [Varolan VNET'te AKS Kümesi Oluşturma][aks-virtual-network]

Aynı sanal ağa (veya bakıldı) bir kez geldiklerinde, AKS Kümenizde kalıcı bir birim ve kalıcı bir birim talebi sağlamanız gerekir. Konteynerler daha sonra NFS sürücüsünün yerel dizinine monte edilebilir.

Kalıcı hacim için örnek bir Kubernetes tanımı (Bu tanım kümenizi ve VM'nizin aynı VNET'te olduğunu varsayar):

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: <NFS_NAME>
  labels:
    type: nfs
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany
  nfs:
    server: <NFS_INTERNAL_IP>
    path: <NFS_EXPORT_FILE_PATH>
```
nfs server bilgileri ile **NFS_INTERNAL_IP,** **NFS_NAME** ve **NFS_EXPORT_FILE_PATH** değiştirin.

Ayrıca kalıcı bir birim talep dosyası gerekir. Aşağıda, nelerin dahil ediletilene bir örneği verilmiştir:

>[!IMPORTANT]  
>**"storageClassName"** boş bir dize kalması gerekir veya iddia işe yaramaz.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: <NFS_NAME>
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: ""
  resources:
    requests:
      storage: 1Gi
  selector: 
    matchLabels:
      type: nfs
```

## <a name="troubleshooting"></a>Sorun giderme
Bir kümeden sunucuya bağlanamıyorsanız, bir sorun dışa aktarılan dizin veya üst öğesi olabilir, sunucuya erişmek için yeterli izine sahip değildir.

Hem ihracat dizininizin hem de ana dizininizin 777 izni olduğundan kontrol edin.

Aşağıdaki komutu çalıştırarak izinleri kontrol edebilirsiniz ve dizinlerde *'drwxrwxrwx'* izinleri olmalıdır:
```console
ls -l
```

## <a name="more-information"></a>Daha fazla bilgi
Tam bir izbin almak veya NFS Server kurulumunuzu hata ayıklamanıza yardımcı olmak için ayrıntılı bir öğretici aşağıda verebilirsiniz:
  - [NFS Öğretici][nfs-tutorial]

## <a name="next-steps"></a>Sonraki adımlar

İlişkili en iyi uygulamalar [için, AKS'deki depolama ve yedekleme ler için en iyi uygulamalara][operator-best-practices-storage]bakın.

<!-- LINKS - external -->
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[linux-create]: https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm
[nfs-tutorial]: https://help.ubuntu.com/community/SettingUpNFSHowTo#Pre-Installation_Setup
[aks-virtual-network]: https://docs.microsoft.com/azure/aks/configure-kubenet#create-an-aks-cluster-in-the-virtual-network
[peer-virtual-networks]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[operator-best-practices-storage]: operator-best-practices-storage.md
