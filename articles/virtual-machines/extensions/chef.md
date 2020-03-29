---
title: Azure VM'ler için şef uzantısı
description: Chef VM Uzantısı'nı kullanarak Chef Client'ı sanal bir makineye dağıtın.
services: virtual-machines-linux
documentationcenter: ''
author: axayjo
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 09/21/2018
ms.author: akjosh
ms.openlocfilehash: a21b8f2fea7433e9f65fd790321a28ea47a38c79
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76544727"
---
# <a name="chef-vm-extension-for-linux-and-windows"></a>Linux ve Windows için Chef VM Uzantısı

Chef Software, Linux ve Windows için fiziksel ve sanal sunucu yapılandırmalarının yönetilmesine olanak sağlayan bir DevOps otomasyon platformu sunar. Chef VM Extension, Chef'in sanal makinelerde olmasını sağlayan bir uzantıdır.

## <a name="prerequisites"></a>Ön koşullar

### <a name="operating-system"></a>İşletim sistemi

Chef VM Uzantısı, Azure'daki tüm [Uzantı Destekli Işletim Sistemi'nde](https://support.microsoft.com/help/4078134/azure-extension-supported-operating-systems) desteklenir.

### <a name="internet-connectivity"></a>İnternet bağlantısı

Chef VM Uzantısı, chef client yükünü içerik dağıtım ağından (CDN) almak için hedef sanal makinenin internete bağlanmasını gerektirir.  

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON Chef VM Extension için şema gösterir. Uzantı, en azından Chef Server URL'sini, Doğrulama İstemci Adı'nı ve Chef Server için Doğrulama Anahtarını gerektirir; bu `knife.rb` [değerler, Chef Automate'i](https://azuremarketplace.microsoft.com/marketplace/apps/chef-software.chef-automate) veya bağımsız bir Chef [Server'ı](https://downloads.chef.io/chef-server)yüklediğinizde indirilen starter-kit.zip dosyasındaki dosyada bulunabilir. Doğrulama anahtarı hassas veri olarak ele alınması gerektiğinden, **protectedSettings** öğesi altında yapılandırılmalıdır, yani yalnızca hedef sanal makinede şifresi çözülecektir.

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'),'/', parameters('chef_vm_extension_type'))]",
  "apiVersion": "2017-12-01",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Chef.Bootstrap.WindowsAzure",
    "type": "[parameters('chef_vm_extension_type')]",
    "typeHandlerVersion": "1210.13",
    "settings": {
      "bootstrap_options": {
        "chef_server_url": "[parameters('chef_server_url')]",
        "validation_client_name": "[parameters('chef_validation_client_name')]"
      },
      "runlist": "[parameters('chef_runlist')]"
    },
    "protectedSettings": {
      "validation_key": "[parameters('chef_validation_key')]"
    }
  }
}  
```

### <a name="core-property-values"></a>Temel özellik değerleri

| Adı | Değer / Örnek | Veri Türü
| ---- | ---- | ----
| apiVersion | `2017-12-01` | dize (tarih) |
| yayımcı | `Chef.Bootstrap.WindowsAzure` | string |
| type | `LinuxChefClient`(Linux), `ChefClient` (Windows) | string |
| typeHandlerVersion | `1210.13` | dize (çift) |

### <a name="settings"></a>Ayarlar

| Adı | Değer / Örnek | Veri Türü | Gerekli mi?
| ---- | ---- | ---- | ----
| ayarlar/bootstrap_options/chef_server_url | `https://api.chef.io/organizations/myorg` | dize (url) | E |
| ayarlar/bootstrap_options/validation_client_name | `myorg-validator` | string | E |
| ayarlar/runlist | `recipe[mycookbook::default]` | string | E |

### <a name="protected-settings"></a>Korumalı ayarlar

| Adı | Örnek | Veri Türü | Gerekli mi?
| ---- | ---- | ---- | ---- |
| protectedAyarlar/validation_key | `-----BEGIN RSA PRIVATE KEY-----\nKEYDATA\n-----END RSA PRIVATE KEY-----` | string | E |

<!--
### Linux-specific settings

| Name | Value / Example | Data Type |
| ---- | ---- | ---- |

### Windows-specific settings

| Name | Value / Example | Data Type |
| ---- | ---- | ---- |
-->

## <a name="template-deployment"></a>Şablon dağıtımı

Azure VM uzantıları Azure Kaynak Yöneticisi şablonlarıyla dağıtılabilir. Şablonlar, bir veya daha fazla sanal makinedağıtmak, Chef Client'ı yüklemek, Chef Server'a bağlanmak ve [Sunucudaki](https://docs.chef.io/run_lists.html) ilk yapılandırmayı Run listesinde tanımlandığı şekilde gerçekleştirmek için kullanılabilir

Chef VM Uzantısı'nı içeren örnek bir Kaynak Yöneticisi şablonu [Azure hızlı başlatma galerisinde](https://github.com/Azure/azure-quickstart-templates/tree/master/chef-json-parameters-linux-vm)bulunabilir.

Sanal makine uzantısı için JSON yapılandırması sanal makine kaynağının içine yerleştirilebilir veya Kaynak Yöneticisi JSON şablonunun köküne veya üst seviyesine yerleştirilebilir. JSON yapılandırmasının yerleşimi kaynak adı ve türünün değerini etkiler. Daha fazla bilgi için [bkz.](../../azure-resource-manager/resource-manager-template-child-resource.md)

## <a name="azure-cli-deployment"></a>Azure CLI dağıtımı

Azure CLI, Chef VM Uzantısı'nı varolan bir VM'ye dağıtmak için kullanılabilir. **validation_key** doğrulama anahtarınızın içeriğiyle değiştirin `.pem` (bu dosya uzantı olarak).  Başlangıç Kiti'nizdeki **run_list** `knife.rb` dosyadaki bu değerlerle **validation_client_name,** **chef_server_url** ve run_list değiştirin.

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myExistingVM \
  --name LinuxChefClient \
  --publisher Chef.Bootstrap.WindowsAzure \
  --version 1210.13 --protected-settings '{"validation_key": "<validation_key>"}' \
  --settings '{ "bootstrap_options": { "chef_server_url": "<chef_server_url>", "validation_client_name": "<validation_client_name>" }, "runlist": "<run_list>" }'
```

## <a name="troubleshooting-and-support"></a>Sorun giderme ve destek

Uzantı lı dağıtımların durumuyla ilgili veriler Azure portalından ve Azure CLI kullanılarak alınabilir. Belirli bir VM uzantılarının dağıtım durumunu görmek için Azure CLI'yi kullanarak aşağıdaki komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myExistingVM -o table
```

Uzantı yürütme çıktısı aşağıdaki dosyaya kaydedilir:

### <a name="linux"></a>Linux

```bash
/var/lib/waagent/Chef.Bootstrap.WindowsAzure.LinuxChefClient
```

### <a name="windows"></a>Windows

```powershell
C:\Packages\Plugins\Chef.Bootstrap.WindowsAzure.ChefClient\
```

### <a name="error-codes-and-their-meanings"></a>Hata kodları ve anlamları

| Hata Kodu | Anlamı | Olası Eylem |
| :---: | --- | --- |
| 51 | Bu uzantı VM'nin işletim sisteminde desteklenmez | |

Ek sorun giderme bilgileri [Chef VM Extension readme](https://github.com/chef-partners/azure-chef-extension)bulunabilir.

> [!NOTE]
> Doğrudan Chef ile ilgili başka bir şey için [Chef Support](https://www.chef.io/support/)ile iletişime geçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve destek al'ı seçin. Azure Desteği'ni kullanma hakkında daha fazla bilgi için [Microsoft Azure destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.
