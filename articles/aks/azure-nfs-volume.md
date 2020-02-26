---
title: Azure Kubernetes Service (AKS) tarafından kullanılmak üzere bir NFS (ağ dosya sistemi) Ubuntu sunucusu oluşturun
description: Azure Kubernetes Service (aks) içinde Pod ile kullanmak üzere bir NFS Ubuntu Linux sunucusu birimini el ile oluşturmayı öğrenin
services: container-service
author: ozboms
ms.topic: article
ms.date: 4/25/2019
ms.author: obboms
ms.openlocfilehash: e5676710bc47557318f3e2adcf36ec0ed13d47de
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77596632"
---
# <a name="manually-create-and-use-an-nfs-network-file-system-linux-server-volume-with-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) ile bir NFS (ağ dosya sistemi) Linux sunucu birimini el ile oluşturma ve kullanma
Kapsayıcılar arasında veri paylaşımı, genellikle kapsayıcı tabanlı hizmet ve uygulamaların gerekli bir bileşenidir. Genellikle, bir dış kalıcı birimle aynı bilgilere erişmesi gereken çeşitli yığınlara sahip olursunuz.    
Azure dosyaları bir seçenek olmakla çalışırken, bir Azure VM 'de NFS sunucusu oluşturmak kalıcı bir paylaşılan depolama biçimidir. 

Bu makalede, bir Ubuntu sanal makinesinde NFS sunucusu oluşturma gösterilmektedir. Ayrıca, AKS kapsayıcılarınızın bu paylaşılan dosya sistemine erişmesini sağlayın.

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede, mevcut bir AKS kümeniz olduğunu varsaymaktadır. AKS kümesine ihtiyacınız varsa bkz. [Azure CLI kullanarak][aks-quickstart-cli] aks hızlı başlangıç veya [Azure Portal kullanımı][aks-quickstart-portal].

AKS kümenizin, NFS sunucusuyla aynı veya eşlenmiş sanal ağlarda canlı olması gerekir. Küme, VM 'niz ile aynı VNET olabilen mevcut bir VNET 'te oluşturulmalıdır.

Mevcut bir VNET ile yapılandırma adımları belgelerde açıklanmaktadır: [mevcut VNET 'TE AKS kümesi oluşturma][aks-virtual-network] ve [sanal ağları VNET eşlemesi ile bağlama][peer-virtual-networks]

Ayrıca, Ubuntu Linux bir sanal makine oluşturmuş olduğunuzu varsayar (örneğin, 18,04 LTS). Ayarlar ve boyut, sizin için kullanılabilir ve Azure üzerinden dağıtılabilir. Linux hızlı başlangıcı için bkz. [LINUX VM yönetimi][linux-create].

AKS kümenizi önce dağıtırsanız, Azure, Ubuntu makinenizi dağıtırken sanal ağ alanını otomatik olarak doldurur ve bunları aynı VNET içinde canlı hale getirir. Ancak, bunun yerine eşlenmiş ağlarla çalışmak istiyorsanız Yukarıdaki belgelere başvurun.

## <a name="deploying-the-nfs-server-onto-a-virtual-machine"></a>NFS sunucusunu bir sanal makineye dağıtma
Ubuntu sanal makinenizde bir NFS sunucusu kurmak için betik aşağıda verilmiştir:
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
Sunucu yeniden başlatılır (komut dosyası nedeniyle) ve NFS sunucusunu AKS 'e bağlayabilirsiniz.

>[!IMPORTANT]  
>**AKS_SUBNET** kümenizdeki doğru bir ile değiştirdiğinizden emin olun, aksi takdirde "*", NFS sunucunuzu tüm bağlantı noktalarında ve bağlantılarda açacak.

VM 'nizi oluşturduktan sonra, yukarıdaki betiği bir dosyaya kopyalayın. Daha sonra, bunu kullanarak yerel makinenizden veya betiğin bulunduğu yere taşıyabilirsiniz: 
```console
scp /path/to/script_file username@vm-ip-address:/home/{username}
```
Betiğinizin sanal makinenizde olması durumunda VM 'de SSH oluşturabilir ve komutunu komut aracılığıyla yürütebilirsiniz:
```console
sudo ./nfs-server-setup.sh
```
Bir izin reddedildi hatası nedeniyle yürütülmesi başarısız olursa, yürütme iznini komut aracılığıyla ayarlayın:
```console
chmod +x ~/nfs-server-setup.sh
```

## <a name="connecting-aks-cluster-to-nfs-server"></a>AKS kümesini NFS sunucusuna bağlama
Birime nasıl erişebileceğinizi belirten kalıcı bir birim ve kalıcı birim talebi sağlayarak, NFS sunucusunu kümenize bağlayabiliriz.

Aynı veya eşlenmiş sanal ağlardaki iki hizmeti bağlamak gereklidir. Aynı VNET 'te küme ayarlamaya yönelik yönergeler şunlardır: [mevcut VNET 'TE AKS kümesi oluşturma][aks-virtual-network]

Aynı sanal ağda (veya eşlenmiş) olduktan sonra, AKS kümenizde kalıcı bir birim ve kalıcı bir birim talebi sağlamanız gerekir. Kapsayıcılar daha sonra NFS sürücüsünü yerel dizinleriyle bağlayabilir.

Kalıcı birim için örnek bir Kubernetes tanımı aşağıda verilmiştir (Bu tanım, kümenizin ve sanal makinenizin aynı VNET 'te olduğunu varsayar):

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
**NFS_INTERNAL_IP**, **NFS_NAME** ve **NFS_EXPORT_FILE_PATH** NFS sunucu bilgileriyle değiştirin.

Ayrıca, kalıcı bir birim talep dosyası da gerekir. Şunları içerecek bir örnek aşağıda verilmiştir:

>[!IMPORTANT]  
>**"Storageclassname"** boş bir dize olmalıdır, aksi durumda talep çalışmaz.

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
Sunucuya bir kümeden bağlanamıyorsanız, bir sorun verilecek dizin ya da üst öğesi, sunucuya erişmek için yeterli izinlere sahip olmayabilir.

Hem dışa aktarma dizininizin hem de onun üst dizininin 777 izni olup olmadığını denetleyin.

Aşağıdaki komutu çalıştırarak izinleri kontrol edebilirsiniz ve dizinler *' drwxrwxrwx '* izinlerine sahip olmalıdır:
```console
ls -l
```

## <a name="more-information"></a>Daha fazla bilgi
Tam bir anlatım almak veya NFS sunucusu kurulumunuzu hata ayıklamanıza yardımcı olmak için, aşağıda ayrıntılı bir öğretici verilmiştir:
  - [NFS öğreticisi][nfs-tutorial]

## <a name="next-steps"></a>Sonraki adımlar

İlişkili en iyi uygulamalar için bkz. [AKS 'de depolama ve yedeklemeler Için en iyi uygulamalar][operator-best-practices-storage].

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
