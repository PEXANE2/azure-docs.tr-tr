---
title: Azure'da Linux VM'lerde özel komut dosyaları çalıştırma
description: Özel Komut Dosyası Uzantısı v2 kullanarak Linux VM yapılandırma görevlerini otomatikleştirin
services: virtual-machines-linux
documentationcenter: ''
author: mimckitt
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: cf17ab2b-8d7e-4078-b6df-955c6d5071c2
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/25/2018
ms.author: mimckitt
ms.openlocfilehash: 75970783a9408df0a8c128fb9540606e4e4a62f3
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478164"
---
# <a name="use-the-azure-custom-script-extension-version-2-with-linux-virtual-machines"></a>Linux sanal makineleriyle Azure Özel Betik Uzantısı Sürüm 2’yi kullanma
Özel Komut Dosyası Uzantısı Sürüm 2, Azure sanal makinelerinde komut dosyalarını indirir ve çalıştırZ. Bu uzantı, dağıtım sonrası yapılandırma, yazılım yüklemesi veya başka bir yapılandırma/yönetim görevi için yararlıdır. Komut dosyalarını Azure Depolama'dan veya erişilebilir başka bir Internet konumundan indirebilir veya bunları uzantı çalışma süresine sağlayabilirsiniz. 

Özel Komut Dosyası Uzantısı, Azure Kaynak Yöneticisi şablonlarıyla tümleşir. Azure CLI, PowerShell veya Azure Sanal Makineler REST API'yi kullanarak da çalıştırabilirsiniz.

Bu makalede, Azure CLI'den Özel Komut Dosyası Uzantısı'nın nasıl kullanılacağı ve Azure Kaynak Yöneticisi şablonu kullanarak uzantının nasıl çalıştırılacak olduğu ayrıntılı olarak anlatılıyor. Bu makalede, Linux sistemleri için sorun giderme adımları da sağlandığıbelirtilmiştir.


İki Linux Özel Komut Dosyası Uzantıları vardır:
* Sürüm 1 - Microsoft.OSTCExtensions.CustomScriptForLinux
* Sürüm 2 - Microsoft.Azure.Extensions.CustomScript

Bunun yerine yeni sürüm 2'yi kullanmak için lütfen yeni ve varolan dağıtımları değiştirin. Yeni sürümün öncekinin yerine bırakılması hedeflenmiştir. Dolayısıyla geçiş işlemi adı ve sürümü değiştirmek kadar kolaydır; uzantı yapılandırmanızı değiştirmeniz gerekmez.


### <a name="operating-system"></a>İşletim Sistemi

Linux için Özel Komut Dosyası Uzantısı uzantısı desteklenen uzantısı işletim sistemi üzerinde çalışacak, daha fazla bilgi için, bu [makaleye](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros)bakın.

### <a name="script-location"></a>Komut Dosyası Konumu

Uzantıyı Azure Blob depolama kimlik bilgilerinizi kullanmak ve Azure Blob depolama alanına erişmek için kullanabilirsiniz. Alternatif olarak, komut dosyası konumu, VM GitHub, dahili dosya sunucusu vb. gibi bu bitiş noktasına yönlendirebildiği sürece herhangi bir yerde olabilir.

### <a name="internet-connectivity"></a>İnternet Bağlantısı
GitHub veya Azure Depolama gibi harici bir komut dosyası indirmeniz gerekiyorsa, ek güvenlik duvarı/Ağ Güvenlik Grubu bağlantı noktalarının açılması gerekir. Örneğin komut dosyanız Azure Depolama'da bulunuyorsa, [Depolama](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags)için Azure NSG Hizmet Etiketleri'ni kullanarak erişime izin verebilirsiniz.

Komut dosyanız yerel bir sunucudaysa, yine de ek güvenlik duvarı/Ağ Güvenlik Grubu bağlantı noktalarının açılması gerekebilir.

### <a name="tips-and-tricks"></a>İpuçları ve Püf Noktalar
* Bu uzantı için en çok karşılaşılan hatalar, betikteki söz dizimi hatalarıdır. Betiğin hatasız bir şekilde çalıştığından emin olun ve ayrıca hata noktalarını daha kolay bir şekilde belirlemek için betiğe ek günlük kaydı işlevleri ekleyin.
* Bir kez etkili betikler yazın. Bu sayede yanlışlıkla birden fazla çalıştırılan betikler, sistemde değişiklik gerçekleştirilmesine neden olmaz.
* Komut dosyalarının çalıştırıldığında kullanıcı girişi gerektirmediğinden emin olun.
* Orada 90 dakika komut dosyası çalıştırmak için izin verilir, bir şey daha uzun uzantısı başarısız bir hükmü neden olacaktır.
* Komut dosyasının içine yeniden başlatma koymayın, bu yüklenen diğer uzantıları ile ilgili sorunlara neden olur ve yeniden başlatma sonrası, uzantısı yeniden başlatıldıktan sonra devam etmez. 
* Yeniden başlatmaya neden olacak bir komut dosyanız varsa, uygulamaları yükleyin ve komut dosyalarını çalıştırın. Yeniden başlatmayı bir Cron işini kullanarak veya DSC veya Chef, Puppet uzantıları gibi araçları kullanarak zamanlamanız gerekir.
* Uzantı, her önyüklemede bir komut dosyası çalıştırmak istiyorsanız, bir komut dosyası yalnızca bir kez çalıştırır, o zaman [bulut init görüntü](https://docs.microsoft.com/azure/virtual-machines/linux/using-cloud-init) kullanabilirsiniz ve [Önyükleme](https://cloudinit.readthedocs.io/en/latest/topics/modules.html#scripts-per-boot) modülü başına komut dosyası kullanabilirsiniz. Alternatif olarak, bir SystemD hizmet birimi oluşturmak için komut dosyasını kullanabilirsiniz.
* Bir komut dosyasının ne zaman çalışacağını zamanlamak istiyorsanız, bir Cron işi oluşturmak için uzantıyı kullanmanız gerekir. 
* Betik çalışırken Azure portalı veya CLI üzerinden uzantı durumunu yalnızca "geçiş durumunda" şeklinde görürsünüz. Çalışan bir komut dosyasının daha sık durum güncelleştirmeleri istiyorsanız, kendi çözümünüzü oluşturmanız gerekir.
* Özel Komut Dosyası uzantısı yerel proxy sunucuları desteklemez, ancak *curl*gibi komut dosyası içinde proxy sunucuları destekleyen bir dosya aktarım aracı kullanabilirsiniz. 
* Komut dosyalarınızın veya komutlarınızın güvenebileceği varsayılan olmayan dizin konumlarına dikkat edin, bunu işlemek için mantık bulundurun.
*  Üretim VMSS örneklerine özel komut dosyası dağıtılırken, json şablonu üzerinden dağıtmanız ve SAS belirteci üzerinde denetiminiz olan komut dosyası depolama hesabınızı depolamanız önerilir. 


## <a name="extension-schema"></a>Uzantı şeması

Özel Komut Dosyası Uzantısı yapılandırması komut dosyası konumu ve çalıştırılacak komut gibi şeyleri belirtir. Bu yapılandırmayı yapılandırma dosyalarında depolayabilir, komut satırında belirtebilir veya Azure Kaynak Yöneticisi şablonunda belirtebilirsiniz. 

Hassas verileri, şifrelenmiş ve yalnızca sanal makinenin içinde şifresi çözülmüş korumalı bir yapılandırmada depolayabilirsiniz. Korumalı yapılandırma, yürütme komutu parola gibi sırlar içeriyorsa yararlıdır.

Bu öğeler hassas veri olarak ele alınmalıdır ve uzantılar korumalı ayar yapılandırmasında belirtilmelidir. Azure VM uzantı korumalı ayar verileri şifrelenir ve yalnızca hedef sanal makinede şifresi çözülür.

```json
{
  "name": "config-app",
  "type": "Extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2019-03-01",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.1",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "skipDos2Unix":false,
      "timestamp":123456789          
    },
    "protectedSettings": {
       "commandToExecute": "<command-to-execute>",
       "script": "<base64-script-to-execute>",
       "storageAccountName": "<storage-account-name>",
       "storageAccountKey": "<storage-account-key>",
       "fileUris": ["https://.."],
        "managedIdentity" : "<managed-identity-identifier>"
    }
  }
}
```

>[!NOTE]
> yönetilenIdentity özelliği storageAccountName veya storageAccountKey özellikleri ile birlikte **kullanılmamalıdır**

### <a name="property-values"></a>Özellik değerleri

| Adı | Değer / Örnek | Veri Türü | 
| ---- | ---- | ---- |
| apiVersion | 2019-03-01 | date |
| yayımcı | Microsoft.Compute.Extensions | string |
| type | Özel Senaryo | string |
| typeHandlerVersion | 2.1 | int |
| fileUris (örn. ) | https://github.com/MyProject/Archive/MyPythonScript.py | array |
| commandToExecute (örn. ) | python \<MyPythonScript.py my-param1> | string |
| betiğini çalıştırın | IyEvYmluL3NoCmVjaG8gIlVwZGF0aW5nIHBhY2thZ2VzIC4uLiIKYXB0IHVwZGF0ZQphcHQgdXBncmFkZSAteQo= | string |
| skipDos2Unix (örn. ) | yanlış | boole |
| zaman damgası (örn. | 123456789 | 32 bit'lik bir sayı |
| storageAccountName (örn.) | örnek storageacct | string |
| storageAccountKey (örn.) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | string |
| yönetilen Kimlik (örn. ) | { } veya { "clientId": "31b403aa-c364-4240-a7ff-d85fb6cd7232" } veya { "objectId": "12dd289c-0583-46e5-b9b4-115d5c19ef4b" } | json nesne |

### <a name="property-value-details"></a>Özellik değeri ayrıntıları
* `apiVersion`: En güncel apiVersion [Kaynak Explorer](https://resources.azure.com/) veya Azure CLI aşağıdaki komutu kullanarak bulunabilir`az provider list -o json`
* `skipDos2Unix`: (isteğe bağlı, boolean) komut dosyası tabanlı dosya URL'leri veya komut dosyası dos2unix dönüştürme atlayın.
* `timestamp`(isteğe bağlı, 32 bit tamsayı) bu alanı yalnızca bu alanın değerini değiştirerek komut dosyasının yeniden çalışmasını tetiklemek için kullanın.  Herhangi bir tümsavar değeri kabul edilebilir; yalnızca önceki değerden farklı olmalıdır.
* `commandToExecute`: ( komut dosyası ayarlanmadıysa**gerekli,** dize) yürütmek için giriş noktası komut dosyası. Komutunuzun parolalar gibi sırlar içeriyorsa bunun yerine bu alanı kullanın.
* `script`: ( komutToExecute ayarlanmazsa**gereklidir,** string)//bin/sh tarafından yürütülen bir base64 kodlanmış (ve isteğe bağlı olarak gzip'ed) komut dosyası.
* `fileUris`: (isteğe bağlı, dize dizisi) dosya(lar) için URL'ler indirilecek.
* `storageAccountName`: (isteğe bağlı, string) depolama hesabının adı. Depolama kimlik bilgilerini belirtirseniz, tümü `fileUris` Azure Blobs için URL olmalıdır.
* `storageAccountKey`: (isteğe bağlı, dize) depolama hesabının erişim anahtarı
* `managedIdentity`: (isteğe bağlı, json nesnesi) dosya(lar) indirmek için [yönetilen kimlik](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)
  * `clientId`: (isteğe bağlı, dize) yönetilen kimliğin istemci kimliği
  * `objectId`: (isteğe bağlı, dize) yönetilen kimliğin nesne kimliği


Aşağıdaki değerler ortak veya korumalı ayarlarda ayarlanabilir, uzantı aşağıdaki değerlerin hem genel hem de korumalı ortamlarda ayarlandığı yapılandırmayı reddeder.
* `commandToExecute`
* `script`
* `fileUris`

Genel ayarları kullanarak hata ayıklama için yararlı olabilir, ancak korumalı ayarları kullanmanız şiddetle tavsiye edilir.

Genel ayarlar, komut dosyasının yürütüleceği VM'ye açık metin olarak gönderilir.  Korumalı ayarlar, yalnızca Azure ve VM tarafından bilinen bir anahtar kullanılarak şifrelenir. Ayarlar gönderildikleri gibi VM'ye kaydedilir, yani ayarlar şifrelenmişse VM'de şifrelenir. Şifrelenmiş değerlerin şifresini çözmek için kullanılan sertifika VM'de depolanır ve çalışma zamanında ayarların şifresini çözmek için (gerekirse) kullanılır.

#### <a name="property-skipdos2unix"></a>Özellik: skipDos2Unix

Varsayılan değer yanlıştır, bu da dos2unix dönüştürmenin **yürütüldüğü** anlamına gelir.

CustomScript önceki sürümü, Microsoft.OSTCExtensions.CustomScriptForLinux, otomatik olarak UNIX dosyalarına `\r\n` `\n`çevirerek DOS dosyaları dönüştürmek istiyorsunuz . Bu çeviri hala var ve varsayılan olarak açıktır. Bu dönüştürme, aşağıdaki ölçütlerden herhangi birini temel alan fileUris veya komut dosyası ayarı tarafından indirilen tüm dosyalara uygulanır.

* Uzantı `.sh`, , `.txt` `.py`, , `.pl` biri ise veya dönüştürülür. Komut dosyası ayarı her zaman bu ölçütlerle eşleşir, çünkü /bin/sh ile yürütülmüş bir komut dosyası olduğu varsayılır ve VM'de script.sh olarak kaydedilir.
* Dosya `#!`ile başlarsa.

Dos2unix dönüştürme, skipDos2Unix'i doğru ayarlayarak atlanabilir.

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>",
  "skipDos2Unix": true
}
```

####  <a name="property-script"></a>Özellik: komut dosyası

CustomScript, kullanıcı tanımlı bir komut dosyasının yürütülmesini destekler. Komut ToExecute ve fileUris tek bir ayarda birleştirmek için komut dosyaları ayarları. Azure depolama veya GitHub gist inden indirilen bir dosya kurmak yerine, komut dosyasını ayar olarak kodlamanız yeterlidir. Komut dosyası komutToExecute ve fileUris yerine kullanılabilir.

Komut dosyası base64 kodlanmış **olmalıdır.**  Komut dosyası **isteğe bağlı olarak** gzip'ed olabilir. Komut dosyası ayarı genel veya korumalı ayarlarda kullanılabilir. Komut dosyası parametresinin verilerinin maksimum boyutu 256 KB'dir. Komut dosyası bu boyutu aşarsa yürütülmez.

Örneğin, dosyaya /script.sh/ kaydedilen aşağıdaki komut dosyası verilir.

```sh
#!/bin/sh
echo "Updating packages ..."
apt update
apt upgrade -y
```

Doğru CustomScript komut dosyası ayarı, aşağıdaki komutun çıktısı alınarak oluşturulur.

```sh
cat script.sh | base64 -w0
```

```json
{
  "script": "IyEvYmluL3NoCmVjaG8gIlVwZGF0aW5nIHBhY2thZ2VzIC4uLiIKYXB0IHVwZGF0ZQphcHQgdXBncmFkZSAteQo="
}
```

Komut dosyası isteğe bağlı olarak boyutu daha da azaltmak için gzip'ed olabilir (çoğu durumda). (CustomScript, gzip sıkıştırma kullanımını otomatik olarak algılar.)

```sh
cat script | gzip -9 | base64 -w 0
```

CustomScript bir komut dosyası yürütmek için aşağıdaki algoritmayı kullanır.

 1. komut dosyasının değerinin uzunluğunun 256 KB'yi geçmediğini iddia edin.
 1. base64 komut dosyasının değerini çöz
 1. base64 deşifre değeri gunzip _girişimi_
 1. deşifre edilmiş (ve isteğe bağlı olarak dekomprese edilmiş) değeri diske (/var/lib/waagent/custom-script/#/script.sh) yazın
 1. komut dosyasını _/bin/sh -c /var/lib/waagent/custom-script/#/script.sh kullanarak uygulayın.

####  <a name="property-managedidentity"></a>Özellik: yönetilenIdentity
> [!NOTE]
> Bu özellik yalnızca korumalı ayarlarda **belirtilmelidir.**

CustomScript (sürüm 2.1'den itibaren), "fileUris" ayarında sağlanan URL'lerden dosya(lar) indirmek için [yönetilen kimliği](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) destekler. CustomScript'in, kullanıcı SAS belirteçleri veya depolama hesabı anahtarları gibi sırları aktarmak zorunda kalmadan Azure Depolama özel bloblarına veya kapsayıcılarına erişmesine olanak tanır.

Bu özelliği kullanmak için, kullanıcının CustomScript'in çalışması beklenen VM veya VMSS'ye [sistemle atanmış](https://docs.microsoft.com/azure/app-service/overview-managed-identity?tabs=dotnet#add-a-system-assigned-identity) veya [kullanıcı tarafından atanmış](https://docs.microsoft.com/azure/app-service/overview-managed-identity?tabs=dotnet#add-a-user-assigned-identity) bir kimlik eklemesi ve Azure Depolama [kapsayıcısına veya blob'a yönetilen kimlik erişimi vermesi](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/tutorial-vm-windows-access-storage#grant-access)gerekir.

Hedef VM/VMSS'de sistem tarafından atanan kimliği kullanmak için boş bir json nesnesine "yönetilen kimlik" alanını ayarlayın. 

> Örnek:
>
> ```json
> {
>   "fileUris": ["https://mystorage.blob.core.windows.net/privatecontainer/script1.sh"],
>   "commandToExecute": "sh script1.sh",
>   "managedIdentity" : {}
> }
> ```

Hedef VM/VMSS'de kullanıcı tarafından atanan kimliği kullanmak için, "yönetilen kimlik" alanını istemci kimliği yle veya yönetilen kimliğin nesne kimliğiyle yapılandırın.

> Örnekler:
>
> ```json
> {
>   "fileUris": ["https://mystorage.blob.core.windows.net/privatecontainer/script1.sh"],
>   "commandToExecute": "sh script1.sh",
>   "managedIdentity" : { "clientId": "31b403aa-c364-4240-a7ff-d85fb6cd7232" }
> }
> ```
> ```json
> {
>   "fileUris": ["https://mystorage.blob.core.windows.net/privatecontainer/script1.sh"],
>   "commandToExecute": "sh script1.sh",
>   "managedIdentity" : { "objectId": "12dd289c-0583-46e5-b9b4-115d5c19ef4b" }
> }
> ```

> [!NOTE]
> yönetilenIdentity özelliği storageAccountName veya storageAccountKey özellikleri ile birlikte **kullanılmamalıdır**

## <a name="template-deployment"></a>Şablon dağıtımı
Azure VM uzantıları Azure Kaynak Yöneticisi şablonlarıyla dağıtılabilir. Önceki bölümde ayrıntılı olarak ayrıntılı olarak kullanılan JSON şeması, Azure Kaynak Yöneticisi şablonu dağıtımı sırasında Özel Komut Dosyası Uzantısı'nı çalıştırmak için bir Azure Kaynak Yöneticisi şablonunda kullanılabilir. Özel Komut Dosyası Uzantısı içeren bir örnek şablon burada bulunabilir, [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).


```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2019-03-01",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.1",
    "autoUpgradeMinorVersion": true,
    "settings": {
      },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <param2>",
      "fileUris": ["https://github.com/MyProject/Archive/hello.sh"
      ]  
    }
  }
}
```

>[!NOTE]
>Bu özellik adları büyük/küçük harf duyarlıdır. Dağıtım sorunlarını önlemek için, burada gösterildiği gibi adları kullanın.

## <a name="azure-cli"></a>Azure CLI
Özel Komut Dosyası Uzantısı'nı çalıştırmak için Azure CLI kullanıyorsanız, bir yapılandırma dosyası veya dosya oluşturun. En azından 'commandToExecute' olmalıdır.

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --protected-settings ./script-config.json
```

İsteğe bağlı olarak, komuttaki ayarları JSON biçimlendirilmiş dize olarak belirtebilirsiniz. Bu, yapılandırmanın yürütme sırasında ve ayrı bir yapılandırma dosyası olmadan belirtilmesine olanak tanır.

```azurecli
az vm extension set \
  --resource-group exttest \
  --vm-name exttest \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --protected-settings '{"fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],"commandToExecute": "./config-music.sh"}'
```

### <a name="azure-cli-examples"></a>Azure CLI örnekleri

#### <a name="public-configuration-with-script-file"></a>Komut dosyası dosyası yla ortak yapılandırma

```json
{
  "fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],
  "commandToExecute": "./config-music.sh"
}
```

Azure CLI komutu:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings ./script-config.json
```

#### <a name="public-configuration-with-no-script-file"></a>Komut dosyası dosyası olmadan genel yapılandırma

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Azure CLI komutu:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings ./script-config.json
```

#### <a name="public-and-protected-configuration-files"></a>Genel ve korumalı yapılandırma dosyaları

KOMUT dosyası dosyası URI belirtmek için ortak yapılandırma dosyası kullanın. Çalıştırılacak komutu belirtmek için korumalı bir yapılandırma dosyası kullanırsınız.

Genel yapılandırma dosyası:

```json
{
  "fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"]
}
```

Korumalı yapılandırma dosyası:  

```json
{
  "commandToExecute": "./config-music.sh <param1>"
}
```

Azure CLI komutu:

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \ 
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings ./script-config.json \
  --protected-settings ./protected-config.json
```

## <a name="troubleshooting"></a>Sorun giderme
Özel Komut Dosyası Uzantısı çalıştığında, komut dosyası oluşturulur veya aşağıdaki örneğe benzer bir dizin içine indirilir. Komut çıktısı da bu dizine `stdout` `stderr` ve dosyalara kaydedilir.

```bash
/var/lib/waagent/custom-script/download/0/
```

Sorun gidermek için önce Linux Agent Log'u kontrol edin, uzantınların çalıştırdığından emin olun, aşağıdakileri kontrol edin:

```bash
/var/log/waagent.log 
```

Uzatma yürütme aramak gerekir, bu gibi bir şey görünecektir:

```output
2018/04/26 17:47:22.110231 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] [Enable] current handler state is: notinstalled
2018/04/26 17:47:22.306407 INFO Event: name=Microsoft.Azure.Extensions.customScript, op=Download, message=Download succeeded, duration=167
2018/04/26 17:47:22.339958 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] Initialize extension directory
2018/04/26 17:47:22.368293 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] Update settings file: 0.settings
2018/04/26 17:47:22.394482 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] Install extension [bin/custom-script-shim install]
2018/04/26 17:47:23.432774 INFO Event: name=Microsoft.Azure.Extensions.customScript, op=Install, message=Launch command succeeded: bin/custom-script-shim install, duration=1007
2018/04/26 17:47:23.476151 INFO [Microsoft.Azure.Extensions.customScript-2.0.6] Enable extension [bin/custom-script-shim enable]
2018/04/26 17:47:24.516444 INFO Event: name=Microsoft.Azure.Extensions.customScript, op=Enable, message=Launch command succeeded: bin/custom-sc
```

Dikkat edilmesi gereken bazı noktalar:
1. Enable, komutun çalışmaya başlamasıdır.
2. İndirme, CustomScript uzantı paketinin fileUris'te belirtilen komut dosyası dosyalarıyla değil, Azure'dan indirilmesiyle ilgilidir.


Azure Komut Dosyası Uzantısı, burada bulabileceğiniz bir günlük üretir:

```bash
/var/log/azure/custom-script/handler.log
```

Bireysel yürütme için bakmak gerekir, bu gibi bir şey görünecektir:

```output
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event=start
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event=pre-check
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="comparing seqnum" path=mrseq
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="seqnum saved" path=mrseq
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="reading configuration"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="read configuration"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="validating json schema"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="json schema valid"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="parsing configuration json"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="parsed configuration json"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="validating configuration logically"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="validated configuration"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="creating output directory" path=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="created output directory"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 files=1
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 file=0 event="download start"
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 file=0 event="download complete" output=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="executing command" output=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="executing protected commandToExecute" output=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event="executed command" output=/var/lib/waagent/custom-script/download/0
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event=enabled
time=2018-04-26T17:47:23Z version=v2.0.6/git@1008306-clean operation=enable seq=0 event=end
```

Burada görebilirsiniz:
* Etkinleştir komutu bu günlük
* Uzantıya geçirilen ayarlar
* Uzantısı indirme dosyası ve bunun sonucu.
* Çalıştırılan komut ve sonuç.

Azure CLI'yi kullanarak Özel Komut Dosyası Uzantısı'nın yürütme durumunu da alabilirsiniz:

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

Çıktı aşağıdaki metne benzer:

```output
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Sonraki adımlar
Kodu, güncel sorunları ve sürümleri görmek için [bkz.](https://github.com/Azure/custom-script-extension-linux)
