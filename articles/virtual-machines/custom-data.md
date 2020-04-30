---
title: Özel veriler ve Azure sanal makineleri
description: Azure sanal makinelerinde özel veriler ve Cloud-Init kullanımıyla ilgili ayrıntılar
services: virtual-machines
author: mimckitt
ms.service: virtual-machines
ms.topic: article
ms.date: 03/06/2020
ms.author: mimckitt
ms.openlocfilehash: 9497e665d024b583c261ade3e6fb5393a9322ce0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81759127"
---
# <a name="custom-data-and-cloud-init-on-azure-virtual-machines"></a>Azure sanal makinelerinde özel veriler ve Cloud-Init

## <a name="what-is-custom-data"></a>Özel veriler nedir?

Müşteriler genellikle, sağlama zamanında bir Microsoft Azure sanal makinesine bir betiği veya diğer meta verileri nasıl ekleyebilecekleri hakkında sorun.  Diğer bulutlarda, bu kavram genellikle kullanıcı verileri olarak adlandırılır.  Microsoft Azure, özel veri adlı benzer bir özelliktir. 

Özel veriler yalnızca ilk önyükleme/ilk kurulum sırasında VM için kullanılabilir hale getirilir, bu ' sağlama ' işlemini çağıracağız. Sağlama, sanal makine oluşturma parametrelerinin (örneğin, ana bilgisayar adı, Kullanıcı adı, parola, sertifikalar, özel veriler, anahtarlar vb.) VM için kullanılabilir hale getirilme işlemidir ve [Linux Aracısı](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) ve [Cloud-init](https://docs.microsoft.com/azure/virtual-machines/linux/using-cloud-init#troubleshooting-cloud-init)gibi bir sağlama Aracısı tarafından işlenir. 


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

Azure Resource Manager (ARM) içinde, bir [Base64 işlevi](https://docs.microsoft.com/azure/azure-resource-manager/templates/template-functions-string#base64)vardır.

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
Linux IŞLETIM sisteminde, özel veriler VM 'ye, sağlama sırasında */var/lib/waagent* dizinine kopyalanmış olan ovf-env. xml dosyası aracılığıyla geçirilir.  Microsoft Azure Linux aracısının daha yeni sürümleri, Base64 kodlamalı verileri de */var/lib/waagent/CustomData dizinine* kopyalar ve kolaylık sağlar.

Azure Şu anda iki sağlama aracısını desteklemektedir:
* Linux Aracısı-varsayılan olarak aracı özel verileri işlemez, etkin olan özel bir görüntü oluşturmanız gerekir. [Belgelere](https://github.com/Azure/WALinuxAgent#configuration) göre ilgili ayarlar şunlardır:
    * . DecodeCustomData sağlama
    * Sağlama. ExecuteCustomData

Özel verileri etkinleştirdiğinizde ve bir betiği yürüttüğünüzde, bu, hazırlama işlemi olan VM raporlanmasını erteleyebilir veya komut dosyası tamamlanana kadar sağlama başarılı olur. Betik 40 dakikalık toplam VM sağlama süresi indirimini aşarsa, VM oluşturma başarısız olur. Not, komut dosyası yürütülemezse veya yürütme sırasında hatalar ortaya çıkarsa, önemli bir sağlama hatası kabul edilmez, betiğin tamamlanma durumu için sizi uyarmak üzere bir bildirim yolu oluşturmanız gerekecektir.

Özel veri yürütme sorunlarını gidermek için */var/log/waagent.log* inceleyin

* Cloud-init varsayılan olarak özel verileri varsayılan olarak işler, Cloud-init, bulut-init yapılandırması, betikler vb. gibi özel verilerin [birden çok biçimini](https://cloudinit.readthedocs.io/en/latest/topics/format.html) kabul eder. Linux aracısına benzer ve Cloud-init özel verileri işler. Yapılandırma işleminin veya betiklerin yürütülmesi sırasında hatalar oluşursa, önemli bir sağlama hatası değildir ve betiğin tamamlanma durumu için sizi uyarmak üzere bir bildirim yolu oluşturmanız gerekir. Ancak, Linux aracısıyla farklı olarak, Cloud-init, Kullanıcı özel veri yapılandırmalarının VM 'ye hazırlanmadan önce tamamlanmasını beklemez. Azure 'da Cloud-init hakkında daha fazla bilgi için [belgeleri](https://docs.microsoft.com/azure/virtual-machines/linux/using-cloud-init)gözden geçirin.


Özel veri yürütme sorunlarını gidermek için sorun giderme [belgelerini](https://docs.microsoft.com/azure/virtual-machines/linux/using-cloud-init#troubleshooting-cloud-init)gözden geçirin.


## <a name="faq"></a>SSS
### <a name="can-i-update-custom-data-after-the-vm-has-been-created"></a>VM oluşturulduktan sonra özel verileri güncelleştirebilir miyim?
Tek VM 'Lerde, VM modelindeki özel veriler güncelleştirilemez, ancak VMSS için, REST API aracılığıyla VMSS özel verilerini güncelleştirebilirsiniz (PS veya AZ CLı istemcileri için geçerli değildir). VMSS modelinde özel verileri güncelleştirdiğinizde:
* VMSS 'deki mevcut örnekler, yalnızca yeniden yansıma oluşturuluncaya kadar güncelleştirilmiş özel verileri almaz.
* VMSS yükseltilen mevcut örnekler güncelleştirilmiş özel verileri almaz.
* Yeni örnekler yeni özel verileri alır.

### <a name="can-i-place-sensitive-values-in-custom-data"></a>Gizli değerleri özel verilere ekleyebilir miyim?
Gizli verileri **not** özel verilerde depolamadığımğiz. Daha fazla bilgi için bkz. [Azure Güvenlik ve şifreleme en iyi uygulamaları](https://docs.microsoft.com/azure/security/fundamentals/data-encryption-best-practices).


### <a name="is-custom-data-made-available-in-imds"></a>Özel veriler IMDS 'de kullanıma sunuldu mu?
Hayır, bu özellik şu anda kullanılamıyor.
