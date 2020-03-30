---
title: Özel veriler ve Azure Sanal Makineleri
description: Azure Sanal Makinelerde Özel verilerin ve Cloud-Init'in kullanımıyla ilgili ayrıntılar
services: virtual-machines
author: mimckitt
ms.service: virtual-machines
ms.topic: article
ms.date: 03/06/2020
ms.author: mimckitt
ms.openlocfilehash: aadac082e90a19d1a185dd7e6181a490adb70a10
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80109634"
---
# <a name="custom-data-and-cloud-init-on-azure-virtual-machines"></a>Azure Sanal Makinelerde Özel Veriler ve Bulut-Init

## <a name="what-is-custom-data"></a>Özel veriler nedir?

Müşteriler genellikle bir komut dosyası veya diğer meta verileri sağlama zamanında microsoft Azure sanal makinesine nasıl enjekte edebileceklerini sorarlar.  Diğer bulutlarda, bu kavram genellikle kullanıcı verileri olarak adlandırılır.  Microsoft Azure'da, özel veri adı verilen benzer bir özelliğimiz vardır. 

Özel veriler sadece ilk önyükleme/ilk kurulum sırasında VM'ye sunulmuştur, biz buna 'sağlama' diyoruz. Sağlama, VM Oluşturma parametrelerinin (örneğin, ana bilgisayar adı, kullanıcı adı, parola, sertifikalar, özel veriler, anahtarlar vb.) VM'nin kullanımına sunulduğu ve [linux aracısı](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) ve bulut girişi gibi bir sağlama aracısının bunları işlediği bir [işlemdir.](https://docs.microsoft.com/azure/virtual-machines/linux/using-cloud-init#troubleshooting-cloud-init) 


## <a name="passing-custom-data-to-the-vm"></a>Özel verileri VM'ye aktarma
Özel verileri kullanmak için, AZ CLI gibi dönüştürmeyi sizin için yapan bir CLI aracı kullanmıyorsanız, içeriği API'ye geçirmeden önce 64 kodlamalısınız. Boyutu 64 KB'yi geçemez.

CLI'de, özel verilerinizi bir dosya olarak geçirebilirsiniz ve bu veri base64'e dönüştürülür.
```bash
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS-CI:7-CI:latest \
  --custom-data cloud-init.txt \
  --generate-ssh-keys
```

Azure Kaynak Yöneticisi'nde (ARM) [base64 işlevi](https://docs.microsoft.com/azure/azure-resource-manager/templates/template-functions-string#base64)vardır.

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
        "customDataBase64": "[variables('customData')]"
        },
```

## <a name="processing-custom-data"></a>Özel verileri işleme
VM'lere yüklenen sağlama aracıları platformla kesişen ve dosya sistemine yerleştirerek işler. 

### <a name="windows"></a>Windows
Özel veriler *%SYSTEMDRIVE%\AzureData\CustomData.bin'e* ikili dosya olarak yerleştirilir, ancak işlenmez. Bu dosyayı işlemek istiyorsanız, özel bir resim oluşturmanız ve CustomData.bin'i işlemek için kod yazmanız gerekir.

### <a name="linux"></a>Linux  
Linux OS'lerde, özel veriler ovf-env.xml dosyası üzerinden VM'ye aktarılır ve bu dosya, sağlama sırasında */var/lib/waagent* dizinine kopyalanır.  Microsoft Azure Linux Aracısı'nın yeni sürümleri de base64 kodlanmış verileri kolaylık sağlamak için */var/lib/waagent/CustomData'ya* kopyalar.

Azure şu anda iki sağlama aracısı destekler:
* Linux Agent - Varsayılan olarak aracı özel verileri işlemez, etkin özel bir görüntü oluşturmak gerekir. [Belgelere](https://github.com/Azure/WALinuxAgent#configuration) göre ilgili ayarlar şunlardır:
    * Provisioning.DecodeCustomData
    * Provisioning.ExecuteCustomData

Özel verileri etkinleştirdiğinizde ve bir komut dosyası çalıştırdığınızda, hazır olan VM raporlamasını veya komut dosyası tamamlanana kadar sağlamanın başarılı olduğunu geciktirir. Komut dosyası 40 dakika toplam VM sağlama süresi ödeneği aşarsa, VM Oluşturma başarısız olur. Not, komut dosyası yürütmek için başarısız olursa veya yürütme sırasında hatalar, bu önemli bir sağlama hatası olarak kabul edilmez, komut dosyasının tamamlanma durumu için sizi uyarmak için bir bildirim yolu oluşturmanız gerekir.

Özel veri yürütme sorunu gidermek için , gözden */var/log/waagent.log*

* cloud-init - Varsayılan olarak varsayılan olarak özel verileri işleyecek, cloud-init bulut init yapılandırması, komut dosyaları vb. gibi [birden çok](https://cloudinit.readthedocs.io/en/latest/topics/format.html) özel veri biçimini kabul eder. Linux Agent'ına benzer şekilde, bulut init özel verileri işler. Yapılandırma işleme veya komut dosyası yürütülmesi sırasında hatalar varsa, bu önemli bir sağlama hatası olarak kabul edilmez ve komut dosyasının tamamlanma durumu için sizi uyarmak için bir bildirim yolu oluşturmanız gerekir. Ancak, Linux Agent farklı, bulut-init VM hazır olduğunu platforma raporlama önce tamamlamak için kullanıcı özel veri yapılandırmaları beklemez. Azure'da bulut-init hakkında daha fazla bilgi için [belgeleri](https://docs.microsoft.com/azure/virtual-machines/linux/using-cloud-init)inceleyin.


Özel veri yürütme sorun larını gidermek için sorun giderme [belgelerini](https://docs.microsoft.com/azure/virtual-machines/linux/using-cloud-init#troubleshooting-cloud-init)gözden geçirin.


## <a name="faq"></a>SSS
### <a name="can-i-update-custom-data-after-the-vm-has-been-created"></a>VM oluşturulduktan sonra özel verileri güncelleştirebilir miyim?
Tek VM'ler için, VM modelindeki özel veriler güncelleştirilemez, ancak VMSS için, REST API (PS veya AZ CLI istemcileri için geçerli değildir) aracılığıyla VMSS özel verilerini güncelleştirebilirsiniz. VMSS modelinde özel verileri güncelleştirdiğinizde:
* VMSS'deki varolan örnekler, yalnızca yeniden görüntülenene kadar güncelleştirilmiş özel verileri almaz.
* VMSS'de yükseltilen varolan örnekler güncelleştirilmiş özel verileri almaz.
* Yeni örnekler yeni özel verileri alır.

### <a name="can-i-place-sensitive-values-in-custom-data"></a>Hassas değerleri özel verilere yerleştirebilir miyim?
Hassas verileri özel verilerde **depolamamanızı** tavsiye ederiz. Daha fazla bilgi için [Azure Güvenlik ve şifreleme en iyi uygulamaları](https://docs.microsoft.com/azure/security/fundamentals/data-encryption-best-practices)görün.


### <a name="is-custom-data-made-available-in-imds"></a>ÖZEL veriler IMDS'de kullanılabilir mi?
Hayır, bu özellik şu anda kullanılamıyor.
