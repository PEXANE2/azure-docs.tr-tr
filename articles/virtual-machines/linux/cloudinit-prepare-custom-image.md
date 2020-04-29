---
title: Azure VM görüntüsünü Cloud-init ile kullanmak üzere hazırlama
description: Cloud-init ile dağıtım için önceden var olan bir Azure VM görüntüsünü hazırlama
author: danis
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 06/24/2019
ms.author: danis
ms.openlocfilehash: fef41f4dc90c03e3efbe4c8a75e495c26eec64b8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80066810"
---
# <a name="prepare-an-existing-linux-azure-vm-image-for-use-with-cloud-init"></a>Mevcut bir Linux Azure VM görüntüsünü Cloud-init ile kullanmak üzere hazırlama
Bu makalede, var olan bir Azure sanal makinesini nasıl ele alacak ve onu yeniden dağıtılması ve Cloud-init kullanmaya başlamaya hazırlanma gösterilmektedir. Elde edilen görüntü, yeni bir sanal makine veya sanal makine ölçek kümeleri dağıtmak için kullanılabilir; bunlardan biri daha sonra dağıtım zamanında Cloud-init tarafından daha da özelleştirilebilir.  Bu Cloud-init betikleri, kaynaklar Azure tarafından sağlandıktan sonra ilk önyüklemede çalışır. Cloud-init 'in Azure 'da ve desteklenen Linux korumalar 'daki yerel olarak nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Cloud-init Overview](using-cloud-init.md)

## <a name="prerequisites"></a>Ön koşullar
Bu belgede, Linux işletim sisteminin desteklenen bir sürümünü çalıştıran çalışan bir Azure sanal makinesine zaten sahip olduğunuz varsayılır. Makineyi gereksinimlerinize uyacak şekilde yapılandırdınız, gerekli tüm modülleri yüklemiş ve gerekli tüm güncelleştirmeleri karşılamış ve gereksinimlerinizi karşıladığından emin olmak için test ediyor. 

## <a name="preparing-rhel-76--centos-76"></a>RHEL 7,6/CentOS 7,6 hazırlanıyor
Cloud-init ' i yüklemek için Linux sanal makinenize SSH oluşturmanız ve aşağıdaki komutları çalıştırmanız gerekir.

```bash
sudo yum makecache fast
sudo yum install -y gdisk cloud-utils-growpart
sudo yum install - y cloud-init 
```

İçindeki `cloud_init_modules` `/etc/cloud/cloud.cfg` bölümünü aşağıdaki modülleri içerecek şekilde güncelleştirin:

```bash
- disk_setup
- mounts
```

Genel amaçlı `cloud_init_modules` bir bölümün nasıl görüneceğine ilişkin bir örnek aşağıda verilmiştir.

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

Kısa ömürlü disklerin sağlanması ve yönetilmesi ile ilgili bir dizi görevin ' de `/etc/waagent.conf`güncelleştirilmeleri gerekir. Uygun ayarları güncelleştirmek için aşağıdaki komutları çalıştırın.

```bash
sed -i 's/Provisioning.Enabled=y/Provisioning.Enabled=n/g' /etc/waagent.conf
sed -i 's/Provisioning.UseCloudInit=n/Provisioning.UseCloudInit=y/g' /etc/waagent.conf
sed -i 's/ResourceDisk.Format=y/ResourceDisk.Format=n/g' /etc/waagent.conf
sed -i 's/ResourceDisk.EnableSwap=y/ResourceDisk.EnableSwap=n/g' /etc/waagent.conf
cloud-init clean
```

Aşağıdaki satırla seçtiğiniz bir düzenleyiciyi kullanarak yeni bir dosya `/etc/cloud/cloud.cfg.d/91-azure_datasource.cfg` oluşturarak yalnızca Azure Linux Aracısı için veri kaynağı olarak Azure 'a izin verin:

```bash
# Azure Data Source config
datasource_list: [ Azure ]
```

Mevcut Azure görüntünüz bir takas dosyası kullanıyorsa ve Cloud-init kullanarak yeni görüntüler için takas dosyası yapılandırmasını değiştirmek istiyorsanız, varolan takas dosyasını kaldırmanız gerekir.

Red Hat tabanlı görüntüler için, [takas dosyasının nasıl kaldırılacağını](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/storage_administration_guide/swap-removing-file)açıklayan aşağıdaki Red Hat belgesinde bulunan yönergeleri izleyin.

Swapfile etkin olan CentOS görüntüleri için, Swapfile 'ı devre dışı bırakmak üzere aşağıdaki komutu çalıştırabilirsiniz:

```bash
sudo swapoff /mnt/resource/swapfile
```

Swapfile başvurusunun ' dan `/etc/fstab` kaldırıldığından emin olun-bu, aşağıdaki çıkışa benzer bir şekilde görünmelidir:

```output
# /etc/fstab
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
UUID=99cf66df-2fef-4aad-b226-382883643a1c / xfs defaults 0 0
UUID=7c473048-a4e7-4908-bad3-a9be22e9d37d /boot xfs defaults 0 0
```

Alanı kaydetmek ve takas dosyasını kaldırmak için aşağıdaki komutu çalıştırabilirsiniz:

```bash
rm /mnt/resource/swapfile
```

## <a name="extra-step-for-cloud-init-prepared-image"></a>Cloud-init hazırlanan görüntü için ek adım
> [!NOTE]
> Resminiz daha önce bir **Cloud-init** hazırlanan ve yapılandırılmış bir görüntü ise, aşağıdaki adımları gerçekleştirmeniz gerekir.

Aşağıdaki üç komut yalnızca, daha önce Cloud-init tarafından sağlanmakta olduğunuz VM yeni bir özelleştirilmiş kaynak görüntü olarak sağlanmışsa kullanılır.  Görüntünüz Azure Linux Aracısı kullanılarak yapılandırıldıysa, bunları çalıştırmanız gerekmez.

```bash
sudo cloud-init clean --logs
sudo waagent -deprovision+user -force
```

## <a name="finalizing-linux-agent-setting"></a>Linux Aracısı ayarı sonlandırılıyor 
Tüm Azure platformu görüntülerinin, bulut-init tarafından yapılandırılmış olmasına bakılmaksızın Azure Linux aracısının yüklü olması gerekir.  Linux makinesinden kullanıcının sağlamasını kaldırma işlemini yapmak için aşağıdaki komutu çalıştırın. 

```bash
sudo waagent -deprovision+user -force
```

Azure Linux Aracısı sağlama kaldırma komutları hakkında daha fazla bilgi için bkz. [Azure Linux Aracısı](../extensions/agent-linux.md) daha fazla ayrıntı için.

SSH oturumundan çıkın, sonra Bash kabuğınızdan, yeni bir Azure VM görüntüsünü serbest bırakmak, genelleştirmek ve oluşturmak için aşağıdaki AzureCLI komutlarını çalıştırın.  Ve `myResourceGroup` ' `sourceVmName` i, sourcevm 'nizi yansıtan uygun bilgilerle değiştirin.

```azurecli
az vm deallocate --resource-group myResourceGroup --name sourceVmName
az vm generalize --resource-group myResourceGroup --name sourceVmName
az image create --resource-group myResourceGroup --name myCloudInitImage --source sourceVmName
```

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişikliklerine yönelik ek Cloud-init örnekleri için aşağıdakilere bakın:
 
- [VM 'ye ek bir Linux kullanıcısı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede var olan paketleri güncelleştirmek için bir paket yöneticisi çalıştırın](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştir](cloudinit-update-vm-hostname.md) 
- [Uygulama paketi yüklemesi, yapılandırma dosyalarını güncelleştirme ve anahtarları ekleme](tutorial-automate-vm-deployment.md)
