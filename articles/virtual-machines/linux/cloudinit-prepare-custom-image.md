---
title: Azure VM görüntüsünü bulut kullanımıyla kullanıma hazırlayın
description: Bulut tabanlı dağıtım için önceden varolan bir Azure VM görüntüsünü hazırlama
author: danis
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 06/24/2019
ms.author: danis
ms.openlocfilehash: fef41f4dc90c03e3efbe4c8a75e495c26eec64b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066810"
---
# <a name="prepare-an-existing-linux-azure-vm-image-for-use-with-cloud-init"></a>Bulut init ile kullanıma mevcut bir Linux Azure VM görüntüsünü hazırlayın
Bu makalede, varolan bir Azure sanal makineyi nasıl alıp yeniden dağıtılacak ve bulut init'i kullanmaya hazır olarak nasıl hazırlayacağınızı gösterir. Elde edilen görüntü, yeni bir sanal makine veya sanal makine ölçek kümeleri dağıtmak için kullanılabilir - bunlardan biri daha sonra dağıtım sırasında bulut init tarafından daha da özelleştirilebilir.  Bu bulut init komut dosyaları, kaynaklar Azure tarafından sağlandıktan sonra ilk önyüklemede çalışır. Azure'da ve desteklenen Linux dağıtımlarında bulut init'in yerel olarak nasıl çalıştığı hakkında daha fazla bilgi için [bulut girişe genel bakış](using-cloud-init.md)

## <a name="prerequisites"></a>Ön koşullar
Bu belge, Linux işletim sisteminin desteklenen bir sürümünü çalıştıran çalışan bir Azure sanal makineniz olduğunu varsayar. Makineyi ihtiyaçlarınıza göre yapılandırmış, gerekli tüm modülleri yüklemiş, gerekli tüm güncelleştirmeleri işlemiş ve gereksinimlerinizi karşıladığından emin olmak için test etmişsinizdir. 

## <a name="preparing-rhel-76--centos-76"></a>RHEL 7.6 / CentOS 7.6 Hazırlama
Linux VM'nize SSH yapmanız ve bulut girişini yüklemek için aşağıdaki komutları çalıştırmanız gerekir.

```bash
sudo yum makecache fast
sudo yum install -y gdisk cloud-utils-growpart
sudo yum install - y cloud-init 
```

`cloud_init_modules` Aşağıdaki modülleri `/etc/cloud/cloud.cfg` içerecek şekilde bölümü güncelleştirin:

```bash
- disk_setup
- mounts
```

Burada genel amaçlı `cloud_init_modules` bir bölümün neye benzediğini bir örnektir.

```bash
cloud_init_modules:
 - migrator
 - bootcmd
 - write-files
 - growpart
 - resizefs
 - disk_setup
 - mounts
 - set_hostname
 - update_hostname
 - update_etc_hosts
 - rsyslog
 - users-groups
 - ssh
```

Geçici disklerin sağlanması ve işlenmesiyle ilgili bir dizi görevin `/etc/waagent.conf`güncelleştirilmesi gerekir. Uygun ayarları güncelleştirmek için aşağıdaki komutları çalıştırın.

```bash
sed -i 's/Provisioning.Enabled=y/Provisioning.Enabled=n/g' /etc/waagent.conf
sed -i 's/Provisioning.UseCloudInit=n/Provisioning.UseCloudInit=y/g' /etc/waagent.conf
sed -i 's/ResourceDisk.Format=y/ResourceDisk.Format=n/g' /etc/waagent.conf
sed -i 's/ResourceDisk.EnableSwap=y/ResourceDisk.EnableSwap=n/g' /etc/waagent.conf
cloud-init clean
```

Aşağıdaki satırla seçtiğiniz bir düzenleyiciyi kullanarak yeni bir `/etc/cloud/cloud.cfg.d/91-azure_datasource.cfg` dosya oluşturarak Azure Linux Aracısı için veri kaynağı olarak yalnızca Azure'a izin verin:

```bash
# Azure Data Source config
datasource_list: [ Azure ]
```

Varolan Azure resminizde bir takas dosyası yapılandırıldıysa ve bulut init'i kullanarak yeni görüntüler için takas dosyası yapılandırmasını değiştirmek istiyorsanız, varolan takas dosyasını kaldırmanız gerekir.

Red Hat tabanlı görüntüler için - takas dosyasının nasıl [kaldırılacağını](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/storage_administration_guide/swap-removing-file)açıklayan aşağıdaki Red Hat belgesindeki yönergeleri izleyin.

Swapfile etkin centOS görüntüler için, swapfile kapatmak için aşağıdaki komutu çalıştırabilirsiniz:

```bash
sudo swapoff /mnt/resource/swapfile
```

Swapfile başvurusu kaldırılır emin `/etc/fstab` olun - aşağıdaki çıktı gibi bir şey görünmelidir:

```output
# /etc/fstab
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
UUID=99cf66df-2fef-4aad-b226-382883643a1c / xfs defaults 0 0
UUID=7c473048-a4e7-4908-bad3-a9be22e9d37d /boot xfs defaults 0 0
```

Yer kazanmak ve takas dosyasını kaldırmak için aşağıdaki komutu çalıştırabilirsiniz:

```bash
rm /mnt/resource/swapfile
```

## <a name="extra-step-for-cloud-init-prepared-image"></a>Bulut init hazırlanmış görüntü için ekstra adım
> [!NOTE]
> Resminiz daha önce hazırlanmış ve yapılandırılmış bir **bulut** görüntüsüyse, aşağıdaki adımları yapmanız gerekir.

Aşağıdaki üç komut yalnızca özelleştirdiğiniz VM'nin daha önce bulut-init tarafından sağlanmış olması durumunda kullanılır.  Resminiz Azure Linux Aracısı kullanılarak yapılandırıldıysa bunları çalıştırmanız gerekmez.

```bash
sudo cloud-init clean --logs
sudo waagent -deprovision+user -force
```

## <a name="finalizing-linux-agent-setting"></a>Linux Agent ayarını sonuçlandırma 
Tüm Azure platform görüntüleri, bulut init tarafından yapılandırılmış olsun veya olmasın, Azure Linux Aracısı yüklüdür.  Kullanıcıyı Linux makinesinden silmeyi bitirmek için aşağıdaki komutu çalıştırın. 

```bash
sudo waagent -deprovision+user -force
```

Azure Linux Aracısı sağlama komutları hakkında daha fazla bilgi için daha fazla ayrıntı için [Azure Linux Aracısı'na](../extensions/agent-linux.md) bakın.

SSH oturumundan çıkın, ardından bash kabuğunuzdan, yeni bir Azure VM görüntüsünü bulmak, genelleştirmek ve oluşturmak için aşağıdaki AzureCLI komutlarını çalıştırın.  `myResourceGroup` Değiştirin `sourceVmName` ve sourceVM'nizi yansıtan uygun bilgileri alın.

```azurecli
az vm deallocate --resource-group myResourceGroup --name sourceVmName
az vm generalize --resource-group myResourceGroup --name sourceVmName
az image create --resource-group myResourceGroup --name myCloudInitImage --source sourceVmName
```

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişikliklerine yönelik ek bulut bindirme örnekleri için aşağıdakileri görün:
 
- [VM'ye ek bir Linux kullanıcısı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede varolan paketleri güncelleştirmek için bir paket yöneticisi çalıştırma](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirme](cloudinit-update-vm-hostname.md) 
- [Uygulama paketi yükleme, yapılandırma dosyalarını güncelleştirme ve anahtar ekleme](tutorial-automate-vm-deployment.md)
