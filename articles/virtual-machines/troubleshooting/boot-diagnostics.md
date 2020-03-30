---
title: Azure'da VM'ler için önyükleme tanılama | Microsoft Doküman
description: Azure'daki sanal makineler için iki hata ayıklama özelliğine genel bakış
services: virtual-machines
author: Deland-Han
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: delhan
ms.openlocfilehash: fe2427d008b49daa6222ca981994f0dc2fbea355
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476595"
---
# <a name="how-to-use-boot-diagnostics-to-troubleshoot-virtual-machines-in-azure"></a>Azure'daki sanal makineleri gidermek için önyükleme tanılama nasıl kullanılır?

Sanal bir makinenin önyükleme yapılamayan bir duruma girmesinin birçok nedeni olabilir. Kaynak Yöneticisi dağıtım modelini kullanarak oluşturulan sanal makinelerinizdeki sorunları gidermek için aşağıdaki hata ayıklama özelliklerini kullanabilirsiniz: Azure sanal makineleri için Konsol Çıktısı ve Ekran Görüntüsü desteği. 

Linux sanal makineleri için, Konsol günlüğünizin çıktısını Portal'dan görüntüleyebilirsiniz. Azure, hem Windows hem de Linux sanal makineleri için hypervisor'dan VM'nin ekran görüntüsünü görmenizi sağlar. Her iki özellik de tüm bölgelerdeki Azure sanal makineleri için desteklenir. Not, ekran görüntüleri ve çıktının depolama hesabınızda görünmesi 10 dakika kadar sürebilir.

Günlüğü ve ekran görüntüsünü görüntülemek için **Önyükleme tanılama** seçeneğini seçebilirsiniz.

![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)

## <a name="common-boot-errors"></a>Sık karşılaşılan önyükleme hataları

- [0xC000000E](https://support.microsoft.com/help/4010129)
- [0xC000000F](https://support.microsoft.com/help/4010130)
- [0xC0000011](https://support.microsoft.com/help/4010134)
- [0xC0000034](https://support.microsoft.com/help/4010140)
- [0xC0000098](https://support.microsoft.com/help/4010137)
- [0xC00000BA](https://support.microsoft.com/help/4010136)
- [0xC000014C](https://support.microsoft.com/help/4010141)
- [0xC0000221](https://support.microsoft.com/help/4010132)
- [0xC0000225](https://support.microsoft.com/help/4010138)
- [0xC0000359](https://support.microsoft.com/help/4010135)
- [0xC0000605](https://support.microsoft.com/help/4010131)
- [Bir işletim sistemi bulunamadı](https://support.microsoft.com/help/4010142)
- [Önyükleme hatası veya INACCESSIBLE_BOOT_DEVICE](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-virtual-machine-created-using-the-azure-portal"></a>Azure Portalı kullanılarak oluşturulan sanal bir makinede tanılamayı etkinleştirme

Aşağıdaki yordam, Kaynak Yöneticisi dağıtım modeli kullanılarak oluşturulan sanal bir makine içindir.

**Yönetim** sekmesinde, **İzleme** bölümünde, **Önyükleme tanılamanın** açık olduğundan emin olun. **Diagnostics depolama hesabı** açılır listesinden tanılama dosyalarını yerleştirecek bir depolama hesabı seçin.
 
![VM oluşturma](./media/virtual-machines-common-boot-diagnostics/enable-boot-diagnostics-vm.png)

> [!NOTE]
> Önyükleme tanılama özelliği premium depolama hesabını desteklemez. Önyükleme tanılama için premium depolama hesabını kullanırsanız, VM'yi başlattığınızda StorageAccountTypeNotSupported hatasını alabilirsiniz.
>

### <a name="deploying-from-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonundan dağıtım

Azure Kaynak Yöneticisi şablonundan dağıtım alıyorsanız, sanal makine kaynağınıza gidin ve tanılama profili bölümünü ekleyin. API sürüm üstbilgisini "2015-06-15" veya sonraki sürümolarak ayarlayın. En son sürümü "2018-10-01".

```json
{
  "apiVersion": "2018-10-01",
  "type": "Microsoft.Compute/virtualMachines",
  … 
```

Tanılama profili, bu günlükleri yerleştirmek istediğiniz depolama hesabını seçmenize olanak sağlar.

```json
    "diagnosticsProfile": {
    "bootDiagnostics": {
    "enabled": true,
    "storageUri": "[concat('https://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
    }
    }
    }
}
```

Şablonları kullanarak kaynak dağıtma hakkında daha fazla bilgi için [Bkz. Hızlı Başlangıç: Azure portalını kullanarak Azure Kaynak Yöneticisi şablonları oluşturun ve dağıtın.](../../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md)

## <a name="enable-boot-diagnostics-on-existing-virtual-machine"></a>Varolan sanal makinede önyükleme tanılamasını etkinleştirme 

Varolan bir sanal makinede Önyükleme tanılamasını etkinleştirmek için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com)oturum açın ve ardından sanal makineyi seçin.
2. Destek **+ sorun giderme** bölümünde **Önyükleme tanılamasını**seçin ve ardından **Ayarlar** sekmesini seçin.
3. **Önyükleme tanılama** ayarlarında, durumu **Açık**olarak değiştirin ve **Depolama hesabı** açılır listesinden bir depolama hesabı seçin. 
4. Yaptığınız değişikliği kaydedin.

    ![Mevcut VM’yi güncelleştirme](./media/virtual-machines-common-boot-diagnostics/enable-for-existing-vm.png)

Değişikliğin etkili olması için sanal makineyi yeniden başlatmanız gerekir.

### <a name="enable-boot-diagnostics-using-the-azure-cli"></a>Azure CLI kullanarak önyükleme tanılamasını etkinleştirme

Mevcut bir Azure sanal makinede önyükleme tanılamasını etkinleştirmek için Azure CLI'yi kullanabilirsiniz. Daha fazla bilgi için [az vm önyükleme-tanılama](
https://docs.microsoft.com/cli/azure/vm/boot-diagnostics?view=azure-cli-latest)bakın.
