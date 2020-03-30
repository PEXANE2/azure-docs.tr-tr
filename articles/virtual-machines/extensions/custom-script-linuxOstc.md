---
title: Azure'da Linux VM'lerde özel komut dosyaları çalıştırma
description: Özel Komut Dosyası Uzantısı v1'i kullanarak Linux VM yapılandırma görevlerini otomatikleştirin
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/14/2018
ms.author: danis
ms.openlocfilehash: a3eae08510e57227b91deeeb7a7a608a6652cb4a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79535417"
---
# <a name="use-the-azure-custom-script-extension-version-1-with-linux-virtual-machines"></a>Linux sanal makineleri yle Azure Özel Komut Dosyası Uzantısı Sürüm 1'i kullanın

[!INCLUDE [virtual-machines-extensions-deprecation-statement](../../../includes/virtual-machines-extensions-deprecation-statement.md)]

Özel Komut Dosyası Uzantısı Sürüm 1, Azure sanal makinelerinde komut dosyalarını indirir ve çalıştırZ. Bu uzantı, dağıtım sonrası yapılandırma, yazılım yüklemesi veya başka bir yapılandırma/yönetim görevi için yararlıdır. Komut dosyalarını Azure Depolama'dan veya erişilebilir başka bir Internet konumundan indirebilir veya bunları uzantı çalışma süresine sağlayabilirsiniz.

Özel Komut Dosyası Uzantısı, Azure Kaynak Yöneticisi şablonlarıyla tümleşir. Azure CLI, PowerShell, Azure portalı veya Azure Sanal Makineler REST API'yi kullanarak da çalıştırabilirsiniz.

Bu makalede, Azure CLI'den Özel Komut Dosyası Uzantısı'nın nasıl kullanılacağı ve Azure Kaynak Yöneticisi şablonu kullanarak uzantının nasıl çalıştırılacak olduğu ayrıntılı olarak anlatılıyor. Bu makalede, Linux sistemleri için sorun giderme adımları da sağlandığıbelirtilmiştir.

İki Linux Özel Komut Dosyası Uzantıları vardır:

* Sürüm 1 - Microsoft.OSTCExtensions.CustomScriptForLinux

* Sürüm 2 - Microsoft.Azure.Extensions.CustomScript

Bunun yerine yeni sürümü[(Microsoft.Azure.Extensions.CustomScript)](custom-script-linux.md)kullanmak için lütfen yeni ve varolan dağıtımları değiştirin. Yeni sürümün öncekinin yerine bırakılması hedeflenmiştir. Dolayısıyla geçiş işlemi adı ve sürümü değiştirmek kadar kolaydır; uzantı yapılandırmanızı değiştirmeniz gerekmez.

### <a name="operating-system"></a>İşletim Sistemi

Desteklenen Linux Dağıtımları:

* CentOS 6.5 ve üzeri
* Debian 8 ve üzeri
  * Debian 8.7, CustomScriptForLinux'u kıran en son görüntülerde Python2 ile birlikte gönderilmez.
* FreeBSD
* OpenSUSE 13.1 ve üzeri
* Oracle Linux 6.4 ve üzeri
* SUSE Linux Enterprise Server 11 SP3 ve üzeri
* Ubuntu 12.04 ve üzeri

### <a name="script-location"></a>Komut Dosyası Konumu

Uzantıyı Azure Blob depolama kimlik bilgilerinizi kullanmak ve Azure Blob depolama alanına erişmek için kullanabilirsiniz. Alternatif olarak, komut dosyası konumu, VM GitHub, dahili dosya sunucusu vb. gibi bu bitiş noktasına yönlendirebildiği sürece herhangi bir yerde olabilir.

### <a name="internet-connectivity"></a>İnternet Bağlantısı

GitHub veya Azure Depolama gibi harici bir komut dosyası indirmeniz gerekiyorsa, ek güvenlik duvarı/Ağ Güvenlik Grubu bağlantı noktalarının açılması gerekir. Örneğin komut dosyanız Azure Depolama'da bulunuyorsa, [Depolama](../../virtual-network/security-overview.md#service-tags)için Azure NSG Hizmet Etiketleri'ni kullanarak erişime izin verebilirsiniz.

Komut dosyanız yerel bir sunucudaysa, yine de ek güvenlik duvarı/Ağ Güvenlik Grubu bağlantı noktalarının açılması gerekebilir.

### <a name="tips-and-tricks"></a>İpuçları ve Püf Noktalar

* Bu uzantı için en çok karşılaşılan hatalar, betikteki söz dizimi hatalarıdır. Betiğin hatasız bir şekilde çalıştığından emin olun ve ayrıca hata noktalarını daha kolay bir şekilde belirlemek için betiğe ek günlük kaydı işlevleri ekleyin.
* Bir kez etkili betikler yazın. Bu sayede yanlışlıkla birden fazla çalıştırılan betikler, sistemde değişiklik gerçekleştirilmesine neden olmaz.
* Komut dosyalarının çalıştırıldığında kullanıcı girişi gerektirmediğinden emin olun.
* Orada 90 dakika komut dosyası çalıştırmak için izin verilir, bir şey daha uzun uzantısı başarısız bir hükmü neden olacaktır.
* Komut dosyasının içine yeniden başlatma koymayın, bu yüklenen diğer uzantıları ile ilgili sorunlara neden olur ve yeniden başlatma sonrası, uzantısı yeniden başlatıldıktan sonra devam etmez. 
* Yeniden başlatmaya neden olacak bir komut dosyanız varsa, uygulamaları yükleyin ve komut dosyalarını çalıştırın. Yeniden başlatmayı bir Cron işini kullanarak veya DSC veya Chef, Puppet uzantıları gibi araçları kullanarak zamanlamanız gerekir.
* Uzantı, her önyüklemede bir komut dosyası çalıştırmak istiyorsanız, bir komut dosyası yalnızca bir kez çalıştırır, o zaman [bulut init görüntü](../linux/using-cloud-init.md) kullanabilirsiniz ve [Önyükleme](https://cloudinit.readthedocs.io/en/latest/topics/modules.html#scripts-per-boot) modülü başına komut dosyası kullanabilirsiniz. Alternatif olarak, sistemli bir hizmet birimi oluşturmak için komut dosyasını kullanabilirsiniz.
* Bir komut dosyasının ne zaman çalışacağını zamanlamak istiyorsanız, bir Cron işi oluşturmak için uzantıyı kullanmanız gerekir.
* Betik çalışırken Azure portalı veya CLI üzerinden uzantı durumunu yalnızca "geçiş durumunda" şeklinde görürsünüz. Çalışan bir komut dosyasının daha sık durum güncelleştirmeleri istiyorsanız, kendi çözümünüzü oluşturmanız gerekir.
* Özel Komut Dosyası uzantısı yerel proxy sunucuları desteklemez, ancak *curl*gibi komut dosyası içinde proxy sunucuları destekleyen bir dosya aktarım aracı kullanabilirsiniz.
* Komut dosyalarınızın veya komutlarınızın güvenebileceği varsayılan olmayan dizin konumlarına dikkat edin, bunu işlemek için mantık bulundurun.

## <a name="extension-schema"></a>Uzantı şeması

Özel Komut Dosyası Uzantısı yapılandırması komut dosyası konumu ve çalıştırılacak komut gibi şeyleri belirtir. Bu yapılandırmayı yapılandırma dosyalarında depolayabilir, komut satırında belirtebilir veya Azure Kaynak Yöneticisi şablonunda belirtebilirsiniz. 

Hassas verileri, şifrelenmiş ve yalnızca sanal makinenin içinde şifresi çözülmüş korumalı bir yapılandırmada depolayabilirsiniz. Korumalı yapılandırma, yürütme komutu parola gibi sırlar içeriyorsa yararlıdır.

Bu öğeler hassas veri olarak ele alınmalıdır ve uzantılar korumalı ayar yapılandırmasında belirtilmelidir. Azure VM uzantı korumalı ayar verileri şifrelenir ve yalnızca hedef sanal makinede şifresi çözülür.

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.OSTCExtensions",
    "type": "CustomScriptForLinux",
    "typeHandlerVersion": "1.5",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "<url>"
      ],
      "enableInternalDNSCheck": true
    },
    "protectedSettings": {
      "storageAccountName": "<storage-account-name>",
      "storageAccountKey": "<storage-account-key>",
      "commandToExecute": "<command>"
    }
  }
}
```

### <a name="property-values"></a>Özellik değerleri

| Adı | Değer / Örnek | Veri Türü |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | date |
| yayımcı | Microsoft.OSTCUzanlar | string |
| type | CustomScriptForLinux | string |
| typeHandlerVersion | 1,5 | int |
| fileUris (örn. ) | https://github.com/MyProject/Archive/MyPythonScript.py | array |
| commandToExecute (örn. ) | python \<MyPythonScript.py my-param1\> | string |
| enableInternalDNSCheck | true | boole |
| storageAccountName (örn.) | örnek storageacct | string |
| storageAccountKey (örn.) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== | string |

### <a name="property-value-details"></a>Özellik değeri ayrıntıları

* `fileUris`: (isteğe bağlı, dize dizisi) komut dosyalarının uri listesi
* `enableInternalDNSCheck`: (isteğe bağlı, bool) varsayılan ı True'dur, DNS denetimini devre dışı bırakabilmek için False olarak ayarlanır.
* `commandToExecute`: (isteğe bağlı, dize) yürütmek için giriş noktası komut dosyası
* `storageAccountName`: (isteğe bağlı, dize) depolama hesabının adı
* `storageAccountKey`: (isteğe bağlı, dize) depolama hesabının erişim anahtarı

Aşağıdaki değerler ortak veya korumalı ayarlarda ayarlanabilir, bu değerlerin hem ortak hem de korumalı ayarlarda ayarlanmış altında olmamalıdır.

* `commandToExecute`

Genel ayarları kullanarak hata ayıklama için yararlı olabilir, ancak korumalı ayarları kullanmanız şiddetle tavsiye edilir.

Genel ayarlar, komut dosyasının yürütüleceği VM'ye açık metin olarak gönderilir.  Korumalı ayarlar, yalnızca Azure ve VM tarafından bilinen bir anahtar kullanılarak şifrelenir. Ayarlar gönderildikleri gibi VM'ye kaydedilir, yani ayarlar şifrelenmişse VM'de şifrelenir. Şifrelenmiş değerlerin şifresini çözmek için kullanılan sertifika VM'de depolanır ve çalışma zamanında ayarların şifresini çözmek için (gerekirse) kullanılır.

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Kaynak Yöneticisi şablonlarıyla dağıtılabilir. Önceki bölümde ayrıntılı olarak ayrıntılı olarak kullanılan JSON şeması, Azure Kaynak Yöneticisi şablonu dağıtımı sırasında Özel Komut Dosyası Uzantısı'nı çalıştırmak için bir Azure Kaynak Yöneticisi şablonunda kullanılabilir.

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.OSTCExtensions",
    "type": "CustomScriptForLinux",
    "typeHandlerVersion": "1.5",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": ["http://MyAccount.blob.core.windows.net/vhds/MyShellScript.sh"]
    },
    "protectedSettings": {
      "storageAccountName": "MyAccount",
      "storageAccountKey": "<storage-account-key>",
      "commandToExecute": "sh MyShellScript.sh"
    }
  }
}
```

>[!NOTE]
>Bu özellik adları büyük/küçük harf duyarlıdır. Dağıtım sorunlarını önlemek için, burada gösterildiği gibi adları kullanın.

## <a name="azure-cli"></a>Azure CLI

Özel Komut Dosyası Uzantısı'nı çalıştırmak için Azure CLI kullanıyorsanız, bir yapılandırma dosyası veya dosya oluşturun. En azından 'commandToExecute' olmalıdır.

```azurecli
az vm extension set -n VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.5 \
  --vm-name MyVm --resource-group MyResourceGroup \
  --protected-settings '{"commandToExecute": "echo hello"}'
```

İsteğe bağlı olarak, komuttaki ayarları JSON biçimlendirilmiş dize olarak belirtebilirsiniz. Bu, yapılandırmanın yürütme sırasında ve ayrı bir yapılandırma dosyası olmadan belirtilmesine olanak tanır.

```azurecli
az vm extension set \
  --resource-group exttest \
  --vm-name exttest \
  --name CustomScriptForLinux \
  --publisher Microsoft.OSTCExtensions \
  --settings '{"fileUris": ["https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"],"commandToExecute": "./config-music.sh"}'
```

### <a name="azure-cli-examples"></a>Azure CLI örnekleri

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
  --vm-name myVM \
  --name CustomScriptForLinux \
  --publisher Microsoft.OSTCExtensions \
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
az vm extension set
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name CustomScriptForLinux \
  --publisher Microsoft.OSTCExtensions \
  --settings ./script-config.json \
  --protected-settings ./protected-config.json
```

## <a name="troubleshooting"></a>Sorun giderme

Özel Komut Dosyası Uzantısı çalıştığında, komut dosyası oluşturulur veya aşağıdaki örneğe benzer bir dizin içine indirilir. Komut çıktısı da bu dizine `stdout` `stderr` ve dosyalara kaydedilir.

```bash
/var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-<version>/download/1
```

Sorun gidermek için önce Linux Agent Log'u kontrol edin, uzantınların çalıştırdığından emin olun, aşağıdakileri kontrol edin:

```bash
/var/log/waagent.log
```

Uzatma yürütme aramak gerekir, bu gibi bir şey görünecektir:

```output
2018/04/26 15:29:44.835067 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Target handler state: enabled
2018/04/26 15:29:44.867625 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] [Enable] current handler state is: notinstalled
2018/04/26 15:29:44.959605 INFO Event: name=Microsoft.OSTCExtensions.CustomScriptForLinux, op=Download, message=Download succeeded, duration=59
2018/04/26 15:29:44.993269 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Initialize extension directory
2018/04/26 15:29:45.022972 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Update settings file: 0.settings
2018/04/26 15:29:45.051763 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Install extension [customscript.py -install]
2018/04/26 15:29:45 CustomScriptForLinux started to handle.
2018/04/26 15:29:45 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] cwd is /var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2
2018/04/26 15:29:45 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Change log file to /var/log/azure/Microsoft.OSTCExtensions.CustomScriptForLinux/1.5.2.2/extension.log
2018/04/26 15:29:46.088212 INFO Event: name=Microsoft.OSTCExtensions.CustomScriptForLinux, op=Install, message=Launch command succeeded: customscript.py -install, duration=1005
2018/04/26 15:29:46.133367 INFO [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Enable extension [customscript.py -enable]
2018/04/26 15:29:46 CustomScriptForLinux started to handle.
..
2018/04/26 15:29:47.178163 INFO Event: name=Microsoft.OSTCExtensions.CustomScriptForLinux, op=Enable, message=Launch command succeeded: customscript.py -enable, duration=1012
```

Dikkat edilmesi gereken bazı noktalar:

1. Enable, komutun çalışmaya başlamasıdır.
1. İndirme, CustomScript uzantı paketinin fileUris'te belirtilen komut dosyası dosyalarıyla değil, Azure'dan indirilmesiyle ilgilidir.
1. Ayrıca, hangi günlük dosyasına yazdığını da görebilirsiniz`/var/log/azure/Microsoft.OSTCExtensions.CustomScriptForLinux/1.5.2.2/extension.log`

Sonraki adım günlük dosyası bir kontrol gitmektir, bu biçimidir:

```bash
/var/log/azure/<extension-name>/<version>/extension.log file.
```

Bireysel yürütme için bakmak gerekir, bu gibi bir şey görünecektir:

```output
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Enable,transitioning,0,Launching the script...
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] sequence number is 0
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] setting file path is/var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2/config/0.settings
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] JSON config: {"runtimeSettings": [{"handlerSettings": {"protectedSettings": "MIIB0AYJKoZIhvcNAQcDoIIBwTCCAb0CAQAxggF+hnEXRtFKTTuKiFC8gTfHKupUSs7qI0zFYRya", "publicSettings": {"fileUris": ["https://dannytesting.blob.core.windows.net/demo/myBash.sh"]}, "protectedSettingsCertThumbprint": "4385AB21617C2452FF6998C0A37F71A0A01C8368"}}]}
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Config decoded correctly.
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Will try to download files, number of retries = 10, wait SECONDS between retrievals = 20s
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Downloading,transitioning,0,Downloading files...
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] No azure storage account and key specified in protected settings. Downloading scripts from external links...
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Converting /var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2/download/0/myBash.sh from DOS to Unix formats: Done
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Removing BOM of /var/lib/waagent/Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2/download/0/myBash.sh: Done
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Succeeded to download files, retry count = 0
2018/04/26 15:29:46 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Internal DNS is ready, retry count = 0
2018/04/26 15:29:47 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Command is finished.
2018/04/26 15:29:47 ---stdout---
2018/04/26 15:29:47
2018/04/26 15:29:47 ---errout---
2018/04/26 15:29:47
2018/04/26 15:29:47
2018/04/26 15:29:47 [Microsoft.OSTCExtensions.CustomScriptForLinux-1.5.2.2] Daemon,success,0,Command is finished.
2018/04/26 15:29:47 ---stdout---
2018/04/26 15:29:47
2018/04/26 15:29:47 ---errout---
2018/04/26 15:29:47
2018/04/26 15:29:47
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
Name                  ProvisioningState    Publisher                   Version  AutoUpgradeMinorVersion
--------------------  -------------------  ------------------------  ---------  -------------------------
CustomScriptForLinux  Succeeded            Microsoft.OSTCExtensions        1.5  True
```

## <a name="next-steps"></a>Sonraki adımlar

Kodu, güncel sorunları ve sürümleri görmek için [Bkz. CustomScript Uzantısı reposu.](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript)
