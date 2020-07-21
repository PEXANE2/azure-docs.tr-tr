---
title: Sağlama aracısını devre dışı bırakma veya kaldırma
description: Linux VM 'lerinde ve görüntülerinde sağlama aracısının nasıl devre dışı bırakılacağını veya kaldırılacağını öğrenin.
author: danielsollondon
ms.service: virtual-machines-linux
ms.subservice: imaging
ms.topic: how-to
ms.workload: infrastructure
ms.date: 07/06/2020
ms.author: danis
ms.reviewer: cynthn
ms.openlocfilehash: 2a17825d062496e6600966dc7c90b14749507e4d
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86494522"
---
# <a name="disable-or-remove-the-linux-agent-from-vms-and-images"></a>Linux aracısını VM 'lerden ve görüntülerden devre dışı bırakma veya kaldırma

Linux aracısını kaldırmadan önce, Linux Aracısı kaldırıldıktan sonra sanal makinenin ne yapamadığını anlamanız gerekir.

Azure sanal makinesi (VM) [uzantıları](../extensions/overview.md) , Azure VM 'lerinde dağıtım sonrası yapılandırma ve otomasyon görevleri sağlayan küçük uygulamalardır, uzantılar Azure denetim düzlemi tarafından yüklenir ve yönetilir. Bu, Platform Uzantısı komutlarını işlemek için [Azure Linux aracısının](../extensions/agent-linux.md) ve sanal makinenin içinde uzantının doğru durumunu güvence altına aldığından emin olmanızı sağlar.

Azure platformu, VM yapılandırması, izleme, güvenlik ve yardımcı program uygulamalarından çok çeşitli uzantılar barındırır. Birinci ve üçüncü taraf uzantıların büyük bir seçimi vardır, uzantıların kullanıldığı anahtar senaryolara örnek olarak şunlar verilebilir:
* Azure Backup, Izleme, disk şifreleme, güvenlik, site çoğaltma ve diğerleri gibi birinci taraf Azure hizmetlerini destekleme.
* SSH/parola sıfırlama
* VM yapılandırması-Özel betikler çalıştırılıyor, Chef, Pupevcil hayvan aracıları vb..
* AV ürünleri, VM Güvenlik Açığı araçları, VM ve uygulama izleme araçları gibi üçüncü taraf ürünleri.
* Uzantılar, yeni bir VM dağıtımıyla birlikte paketlenmiştir. Örneğin, bu uygulamalar daha büyük bir dağıtımın parçası olabilirler, VM sağlaması üzerinde uygulamaları yapılandırıyor veya desteklenen tüm uzantı çalıştırılan sistemlerin dağıtımı sonrasında çalıştırılabilir.

## <a name="disabling-extension-processing"></a>Uzantı işleme devre dışı bırakılıyor

Gereksinimlerinize bağlı olarak, uzantı işlemeyi devre dışı bırakmak için birkaç yol vardır, ancak devam etmeden önce, VM 'ye dağıtılan tüm uzantıları kaldırmanız **gerekir** ; ÖRNEĞIN, az CLI kullanarak, aşağıdakileri [listeleyebilir](/cli/azure/vm/extension?view=azure-cli-latest#az-vm-extension-list) ve [silebilirsiniz](/cli/azure/vm/extension?view=azure-cli-latest#az-vm-extension-delete):

```bash
az vm extension delete -g MyResourceGroup --vm-name MyVm -n extension_name
```
> [!Note]
> 
> Yukarıdaki işlem yapmazsanız, Platform, uzantı yapılandırmasını ve zaman aşımını 40min sonra göndermeye çalışır.

### <a name="disable-at-the-control-plane"></a>Denetim düzleminde devre dışı bırak
Gelecekte uzantılara ihtiyacınız olup olmadığından emin değilseniz, Linux aracısını VM 'de yüklü bırakıp uzantı işleme özelliğini platformdan devre dışı bırakabilirsiniz. Bu seçenek, `Microsoft.Compute` API sürümü `2018-06-01` veya üzeri sürümlerde kullanılabilir ve Linux Aracısı sürümünün yüklü olduğu bir bağımlılığa sahip değildir.

```bash
az vm update -g <resourceGroup> -n <vmName> --set osProfile.allowExtensionOperations=false
```
Yukarıdaki komutla birlikte bu uzantı işlemini platformdan kolayca yeniden etkinleştirebilirsiniz, ancak bunu ' true ' olarak ayarlayabilirsiniz.

## <a name="remove-the-linux-agent-from-a-running-vm"></a>Linux aracısını çalışan bir VM 'den kaldırma

Yukarıdaki tüm mevcut uzantıları VM 'den önce, yukarıdaki şekilde **kaldırmış** olduğunuzdan emin olun.

### <a name="step-1-remove-the-azure-linux-agent"></a>1. Adım: Azure Linux aracısını kaldırma

İlişkili yapılandırma yapıtlarını değil yalnızca Linux aracısını kaldırırsanız, daha sonraki bir tarihte yeniden yükleyebilirsiniz. Azure Linux aracısını kaldırmak için aşağıdakilerden birini kök olarak çalıştırın:

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

### <a name="step-2-optional-remove-the-azure-linux-agent-artifacts"></a>2. Adım: (Isteğe bağlı) Azure Linux Aracısı yapılarını kaldırma
> [!IMPORTANT] 
>
> Linux aracısının ilişkili tüm yapılarını kaldırabilirsiniz, ancak bu, daha sonraki bir tarihte yeniden yükleyemediği anlamına gelir. Bu nedenle, önce Linux aracısını devre dışı bırakmayı, Linux aracısını yalnızca yukarıdaki ' ı kullanarak kaldırmayı öneririz. 

Linux aracısını yeniden yeniden yüklemeyeceğimizi biliyorsanız, şunları çalıştırabilirsiniz:

#### <a name="for-ubuntu-1804"></a>Ubuntu için >= 18,04
```bash
apt -y purge walinuxagent
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

VM 'yi uzantılar ile dağıtmak için, [--Enable-Agent](/cli/azure/vm#az-vm-create)Ile Azure CLI kullanabilirsiniz.

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
