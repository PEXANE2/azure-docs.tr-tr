---
title: Özel veriler ve Azure sanal makineleri
description: Azure sanal makinelerinde özel veriler ve Cloud-Init kullanımıyla ilgili ayrıntılar
services: virtual-machines
author: mimckitt
ms.service: virtual-machines
ms.topic: article
ms.date: 03/06/2020
ms.author: mimckitt
ms.openlocfilehash: 1dcba7da09cff3b7123521a4daf1028ab17e199a
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87029153"
---
# <a name="custom-data-and-cloud-init-on-azure-virtual-machines"></a>Azure sanal makinelerinde özel veriler ve Cloud-Init

Sağlama zamanında bir Microsoft Azure sanal makinesine bir betik veya diğer meta veri eklemeniz gerekebilir.  Diğer bulutlarda, bu kavram genellikle kullanıcı verileri olarak adlandırılır.  Microsoft Azure, özel veri adlı benzer bir özelliktir. 

Özel veriler yalnızca ilk önyükleme/ilk kurulum sırasında VM için kullanılabilir hale getirilir, bu ' sağlama ' işlemini çağıracağız. Sağlama, sanal makine oluşturma parametrelerinin (örneğin, ana bilgisayar adı, Kullanıcı adı, parola, sertifikalar, özel veriler, anahtarlar vb.) VM için kullanılabilir hale getirilme işlemidir ve [Linux Aracısı](./extensions/agent-linux.md) ve [Cloud-init](./linux/using-cloud-init.md#troubleshooting-cloud-init)gibi bir sağlama Aracısı tarafından işlenir. 


## <a name="passing-custom-data-to-the-vm"></a>Özel verileri VM 'ye geçirme
Özel verileri kullanmak için, dönüştürme yapan AZ CLı gibi bir CLı Aracı kullanmadığınız sürece, API 'ye geçirmeden önce içerikleri Base64 olarak kodlamanız gerekir. Boyut 64 KB 'ı aşamaz.

CLı 'da, özel verilerinizi bir dosya olarak geçirebilir ve Base64 'e dönüştürülecektir.
```bash
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS-CI:7-CI:latest \
  --custom-data cloud-init.txt \
  --generate-ssh-keys
```

Azure Resource Manager (ARM) içinde, bir [Base64 işlevi](../azure-resource-manager/templates/template-functions-string.md#base64)vardır.

```json
"name": "[parameters('virtualMachineName')]",
"type": "Microsoft.Compute/virtualMachines",
"apiVersion": "2019-07-01",
"location": "[parameters('location')]",
"dependsOn": [
..],
"variables": {
        "customDataBase64": "[base64(parameters('stringData'))]"
    },
"properties": {
..
    "osProfile": {
        "computerName": "[parameters('virtualMachineName')]",
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "customData": "[variables('customDataBase64')]"
        },
```

## <a name="processing-custom-data"></a>Özel verileri işleme
VM 'lerde yüklü olan sağlama aracıları, platformla ve dosya sistemine yerleştirilerek bir arabirim işleme alır. 

### <a name="windows"></a>Windows
Özel veriler bir ikili dosya olarak *%SYSTEMDRIVE%\AzureData\CustomData.bin* 'e yerleştirilir, ancak işlenmemiştir. Bu dosyayı işlemek isterseniz, özel bir görüntü oluşturmanız ve CustomData. bin işlemek için kod yazmanız gerekir.

### <a name="linux"></a>Linux  
Linux IŞLETIM sisteminde, özel veriler, sağlama sırasında */var/lib/waagent* dizinine kopyalanmış ovf-env.xml dosyası aracılığıyla sanal makineye geçirilir.  Microsoft Azure Linux aracısının daha yeni sürümleri, Base64 kodlamalı verileri de */var/lib/waagent/CustomData dizinine* kopyalar ve kolaylık sağlar.

Azure Şu anda iki sağlama aracısını desteklemektedir:
* Linux Aracısı-varsayılan olarak aracı özel verileri işlemez, etkin olan özel bir görüntü oluşturmanız gerekir. [Belgelere](https://github.com/Azure/WALinuxAgent#configuration) göre ilgili ayarlar şunlardır:
    * . DecodeCustomData sağlama
    * CuteCustomData Provisioning.Exe

Özel verileri etkinleştirdiğinizde ve bir betiği yürüttüğünüzde, bu, hazırlama işlemi olan VM raporlanmasını erteleyebilir veya komut dosyası tamamlanana kadar sağlama başarılı olur. Betik 40 dakikalık toplam VM sağlama süresi indirimini aşarsa, VM oluşturma başarısız olur. Not, komut dosyası yürütülemezse veya yürütme sırasında hatalar ortaya çıkarsa, önemli bir sağlama hatası kabul edilmez, betiğin tamamlanma durumu için sizi uyarmak üzere bir bildirim yolu oluşturmanız gerekecektir.

Özel veri yürütme sorunlarını gidermek için */var/log/waagent.log* inceleyin

* Cloud-init varsayılan olarak özel verileri varsayılan olarak işler, Cloud-init, bulut-init yapılandırması, betikler vb. gibi özel verilerin [birden çok biçimini](https://cloudinit.readthedocs.io/en/latest/topics/format.html) kabul eder. Linux aracısına benzer ve Cloud-init özel verileri işler. Yapılandırma işleminin veya betiklerin yürütülmesi sırasında hatalar oluşursa, önemli bir sağlama hatası değildir ve betiğin tamamlanma durumu için sizi uyarmak üzere bir bildirim yolu oluşturmanız gerekir. Ancak, Linux aracısıyla farklı olarak, Cloud-init, Kullanıcı özel veri yapılandırmalarının VM 'ye hazırlanmadan önce tamamlanmasını beklemez. Azure 'da Cloud-init hakkında daha fazla bilgi için [belgeleri](./linux/using-cloud-init.md)gözden geçirin.


Özel veri yürütme sorunlarını gidermek için sorun giderme [belgelerini](./linux/using-cloud-init.md#troubleshooting-cloud-init)gözden geçirin.


## <a name="faq"></a>SSS
### <a name="can-i-update-custom-data-after-the-vm-has-been-created"></a>VM oluşturulduktan sonra özel verileri güncelleştirebilir miyim?
Tek VM 'Lerde, VM modelindeki özel veriler güncelleştirilemez, ancak VMSS için, [REST API](/rest/api/compute/virtualmachinescalesets/update) aracılığıyla VMSS özel verilerini GÜNCELLEŞTIREBILIRSINIZ (PS veya az CLI istemcileri için geçerli değildir). VMSS modelinde özel verileri güncelleştirdiğinizde:
* VMSS 'deki mevcut örnekler, yalnızca yeniden yansıma oluşturuluncaya kadar güncelleştirilmiş özel verileri almaz.
* VMSS yükseltilen mevcut örnekler güncelleştirilmiş özel verileri almaz.
* Yeni örnekler yeni özel verileri alır.

### <a name="can-i-place-sensitive-values-in-custom-data"></a>Gizli değerleri özel verilere ekleyebilir miyim?
Gizli verileri **not** özel verilerde depolamadığımğiz. Daha fazla bilgi için bkz. [Azure Güvenlik ve şifreleme en iyi uygulamaları](../security/fundamentals/data-encryption-best-practices.md).


### <a name="is-custom-data-made-available-in-imds"></a>Özel veriler IMDS 'de kullanıma sunuldu mu?
Hayır, bu özellik şu anda kullanılamıyor.
