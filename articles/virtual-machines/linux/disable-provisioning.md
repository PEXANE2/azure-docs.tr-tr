---
title: Sağlama aracısını devre dışı bırakma veya kaldırma
description: Linux VM 'lerinde ve görüntülerinde sağlama aracısının nasıl devre dışı bırakılacağını veya kaldırılacağını öğrenin.
author: danielsollondon
ms.service: virtual-machines-linux
ms.subservice: imaging
ms.topic: how-to
ms.workload: infrastructure
ms.date: 06/22/2020
ms.author: danis
ms.reviewer: cynthn
ms.openlocfilehash: d5d173e0b0204ee9e9dbe6e8b51d38d4e42d4fc2
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85306936"
---
# <a name="disable-or-remove-the-linux-agent-from-vms-and-images"></a>Linux aracısını VM 'lerden ve görüntülerden devre dışı bırakma veya kaldırma

Linux aracısını kaldırmadan önce, Linux Aracısı kaldırıldıktan sonra sanal makinenin ne yapamadığını anlamanız gerekir.

Azure sanal makinesi (VM) [uzantıları](https://docs.microsoft.com/azure/virtual-machines/extensions/overview) , Azure VM 'lerinde dağıtım sonrası yapılandırma ve otomasyon görevleri sağlayan küçük uygulamalardır, uzantılar Azure denetim düzlemi tarafından yüklenir ve yönetilir. Bu, Platform Uzantısı komutlarını işlemek için [Azure Linux aracısının](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) ve sanal makinenin içinde uzantının doğru durumunu güvence altına aldığından emin olmanızı sağlar.

Azure platformu, VM yapılandırması, izleme, güvenlik ve yardımcı program uygulamalarından çok çeşitli uzantılar barındırır. Birinci ve üçüncü taraf uzantıların büyük bir seçimi vardır, uzantıların kullanıldığı anahtar senaryolara örnek olarak şunlar verilebilir:
* Azure Backup, Izleme, disk şifreleme, güvenlik, site çoğaltma ve diğerleri gibi birinci taraf Azure hizmetlerini destekleme.
* SSH/parola sıfırlama
* VM yapılandırması-Özel betikler çalıştırılıyor, Chef, Pupevcil hayvan aracıları vb..
* AV ürünleri, VM Güvenlik Açığı araçları, VM ve uygulama izleme araçları gibi üçüncü taraf ürünleri.
* Uzantılar, yeni bir VM dağıtımıyla birlikte paketlenmiştir. Örneğin, bu uygulamalar daha büyük bir dağıtımın parçası olabilirler, VM sağlaması üzerinde uygulamaları yapılandırıyor veya desteklenen tüm uzantı çalıştırılan sistemlerin dağıtımı sonrasında çalıştırılabilir.

## <a name="disabling-extension-processing"></a>Uzantı işleme devre dışı bırakılıyor

Gereksinimlerinize bağlı olarak, uzantı işlemeyi devre dışı bırakmak için birkaç yol vardır, ancak devam etmeden önce, VM 'ye dağıtılan tüm uzantıları kaldırmanız **gerekir** ; ÖRNEĞIN, az CLI kullanarak, aşağıdakileri [listeleyebilir](https://docs.microsoft.com/cli/azure/vm/extension?view=azure-cli-latest#az-vm-extension-list) ve [silebilirsiniz](https://docs.microsoft.com/cli/azure/vm/extension?view=azure-cli-latest#az-vm-extension-delete):

```bash
az vm extension delete -g MyResourceGroup --vm-name MyVm -n extension_name
```

### <a name="disable-at-the-control-plane"></a>Denetim düzleminde devre dışı bırak
Gelecekte uzantılara ihtiyacınız olup olmadığından emin değilseniz, Linux aracısını VM 'de yüklü bırakıp uzantı işleme özelliğini platformdan devre dışı bırakabilirsiniz. Bu seçenek, `Microsoft.Compute` API sürümü `2018-06-01` veya üzeri sürümlerde kullanılabilir ve Linux Aracısı sürümünün yüklü olduğu bir bağımlılığa sahip değildir.

```bash
az vm update -g <resourceGroup> -n <vmName> --set osProfile.allowExtensionOperations=false
```
Yukarıdaki komutla birlikte bu uzantı işlemini platformdan kolayca yeniden etkinleştirebilirsiniz, ancak bunu ' true ' olarak ayarlayabilirsiniz.

### <a name="optional---reduce-the-functionality"></a>İsteğe bağlı-işlevselliği azaltma 

Linux aracısını azaltılmış işlevsellik moduna da yerleştirebilirsiniz. Bu modda, Konuk Aracısı yine de Azure Fabric ile iletişim kurar ve konuk durumunu çok daha sınırlı bir şekilde bildirir, ancak uzantı güncelleştirmelerini işlemez. İşlevselliği azaltmak için VM 'de bir yapılandırma değişikliği yapmanız gerekir. Yeniden etkinleştirmek için VM 'ye SSH oluşturmanız gerekir, ancak VM 'den kilitlediyseniz, bir SSH veya parola sıfırlaması yapmanız gerekiyorsa, bu durum belki bir sorun olabilir.

Bu modu etkinleştirmek için, Walınuxagent sürüm 2.2.32 veya üzeri gereklidir ve/etc/waagent.exe için aşağıdaki seçeneği ayarlayın:

```bash
Extensions.Enabled=n
```

Bu, ' denetim düzleminde devre dışı bırak ' ile **birlikte yapılmalıdır.**

## <a name="remove-the-linux-agent-from-a-running-vm"></a>Linux aracısını çalışan bir VM 'den kaldırma

Yukarıdaki tüm mevcut uzantıları VM 'den önce, yukarıdaki şekilde **kaldırmış** olduğunuzdan emin olun.

### <a name="step-1-disable-extension-processing"></a>1. Adım: uzantı işlemeyi devre dışı bırakma

Uzantı işlemeyi devre dışı bırakmanız gerekir.

```bash
az vm update -g <resourceGroup> -n <vmName> --set osProfile.allowExtensionOperations=false
```
> [!Note]
> 
> Yukarıdaki işlem yapmazsanız, Platform, uzantı yapılandırmasını ve zaman aşımını 40min sonra göndermeye çalışır.

### <a name="step-2-remove-the-azure-linux-agent"></a>2. Adım: Azure Linux aracısını kaldırma

Azure Linux aracısını kaldırmak için aşağıdakilerden birini kök olarak çalıştırın:

#### <a name="for-ubuntu-1804"></a>Ubuntu için >= 18,04
```bash
apt -y remove walinuxagent
```

#### <a name="for-redhat--77"></a>RedHat için >= 7,7
```bash
yum -y remove WALinuxAgent
```

#### <a name="for-suse"></a>SUSE için
```bash
zypper --non-interactive remove python-azure-agent
```

### <a name="step-3-optional-remove-the-azure-linux-agent-artifacts"></a>3. Adım: (Isteğe bağlı) Azure Linux Aracısı yapılarını kaldırma
> [!IMPORTANT] 
>
> Linux aracısının tüm yapılarını kaldırabilirsiniz, ancak bu, daha sonraki bir tarihte yeniden yükleyemeyecek anlamına gelir. Bu nedenle, önce Linux aracısını devre dışı bırakmayı, Linux aracısını yalnızca yukarıdaki ' ı kullanarak kaldırmayı öneririz. 

Linux aracısını yeniden yeniden yüklemeyeceğimizi biliyorsanız, şunları çalıştırabilirsiniz:

#### <a name="for-ubuntu-1804"></a>Ubuntu için >= 18,04
```bash
apt -y remove walinuxagent
rm -f /etc/waagent.conf
rm -rf /var/lib/waagent
rm -f /var/log/waagent.log
```

#### <a name="for-redhat--77"></a>RedHat için >= 7,7
```bash
yum -y remove WALinuxAgent
rm -f /etc/waagent.conf.rpmsave
rm -rf /var/lib/waagent
rm -f /var/log/waagent.log
```

#### <a name="for-suse"></a>SUSE için
```bash
zypper --non-interactive remove python-azure-agent
rm -f /etc/waagent.conf.rpmsave
rm -rf /var/lib/waagent
rm -f /var/log/waagent.log
```

## <a name="preparing-an-image-without-the-linux-agent"></a>Linux Aracısı olmadan görüntü hazırlama
Zaten Cloud-init içeren bir görüntünüz varsa ve Linux aracısını kaldırmak, ancak yine de Cloud-init kullanarak sağlamak istiyorsanız 2. adımdaki adımları (ve isteğe bağlı olarak adım 3) Azure Linux aracısını kaldırmak için kök olarak çalıştırın ve ardından bulut-init yapılandırmasını ve önbelleğe alınmış verileri kaldırır ve VM 'yi özel bir görüntü oluşturmak üzere hazırlar.

```bash
cloud-init clean --logs --seed 
```

## <a name="deprovision-and-create-an-image"></a>Bir görüntü sağlamayı kaldırma ve oluşturma
Linux Aracısı, "waagent-deprovision + user" adımıyla mevcut görüntü meta verilerinden bazılarını temizleyebilir, ancak kaldırıldıktan sonra, aşağıdaki gibi eylemler gerçekleştirmeniz ve diğer tüm hassas verileri kaldırmanız gerekir.

- Var olan tüm SSH ana bilgisayar anahtarlarını kaldır

   ```bash
   rm /etc/ssh/ssh_host_*key*
   ```
- Yönetici hesabını silme

   ```bash
   touch /var/run/utmp
   userdel -f -r <admin_user_account>
   ```
- Kök parolayı silme

   ```bash
   passwd -d root
   ```

Yukarıdaki işlemi tamamladıktan sonra, Azure CLı 'yi kullanarak özel görüntü oluşturabilirsiniz.


**Düzenli olarak yönetilen görüntü oluşturma**
```bash
az vm deallocate -g <resource_group> -n <vm_name>
az vm generalize -g <resource_group> -n <vm_name>
az image create -g <resource_group> -n <image_name> --source <vm_name>
```

**Paylaşılan görüntü galerisinde görüntü sürümü oluşturma**

```bash
az sig image-version create \
    -g $sigResourceGroup 
    --gallery-name $sigName 
    --gallery-image-definition $imageDefName 
    --gallery-image-version 1.0.0 
    --managed-image /subscriptions/00000000-0000-0000-0000-00000000xxxx/resourceGroups/imageGroups/providers/images/MyManagedImage
```
### <a name="creating-a-vm-from-an-image-that-does-not-contain-a-linux-agent"></a>Linux Aracısı içermeyen bir görüntüden VM oluşturma
Linux Aracısı içermeyen görüntüden sanal makineyi oluşturduğunuzda, VM dağıtımı yapılandırmasının bu VM 'de uzantıların desteklenmediğini belirttiğinden emin olmanız gerekir.

> [!NOTE] 
> 
> Yukarıdaki işlem yapmazsanız, Platform, uzantı yapılandırmasını ve zaman aşımını 40min sonra göndermeye çalışır.

VM 'yi uzantılar ile dağıtmak için, [--Enable-Agent](https://docs.microsoft.com/cli/azure/vm#az-vm-create)Ile Azure CLI kullanabilirsiniz.

```bash
az vm create \
    --resource-group $resourceGroup \
    --name $prodVmName \
    --image RedHat:RHEL:8.1-ci:latest \
    --admin-username azadmin \
    --ssh-key-value "$sshPubkeyPath" \
    --enable-agent false
```

Alternatif olarak, bunu ayarlayarak Azure Resource Manager (ARM) şablonları kullanarak yapabilirsiniz `"provisionVMAgent": false,` .

```json
"osProfile": {
    "computerName": "[parameters('virtualMachineName')]",
    "adminUsername": "[parameters('adminUsername')]",
    "linuxConfiguration": {
        "disablePasswordAuthentication": "true",
        "provisionVMAgent": false,
        "ssh": {
            "publicKeys": [
                {
                    "path": "[concat('/home/', parameters('adminUsername'), '/.ssh/authorized_keys')]",
                    "keyData": "[parameters('adminPublicKey')]"
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Linux sağlama](provisioning.md).
