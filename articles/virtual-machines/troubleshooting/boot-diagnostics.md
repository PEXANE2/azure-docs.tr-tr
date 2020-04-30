---
title: Azure 'da VM 'Ler için önyükleme tanılaması | Microsoft doc
description: Azure 'daki sanal makineler için iki hata ayıklama özelliğine genel bakış
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79476595"
---
# <a name="how-to-use-boot-diagnostics-to-troubleshoot-virtual-machines-in-azure"></a>Azure 'da sanal makinelerde sorun gidermek için önyükleme tanılamayı kullanma

Bir sanal makinenin önyüklenebilir olmayan bir duruma girmesinin pek çok nedeni olabilir. Kaynak Yöneticisi dağıtım modeli kullanılarak oluşturulan sanal makinelerinizdeki sorunları gidermek için aşağıdaki hata ayıklama özelliklerini kullanabilirsiniz: konsol çıktısı ve Azure sanal makineleri için ekran görüntüsü desteği. 

Linux sanal makineleri için, portaldan konsol günlüğlerinizin çıkışını görüntüleyebilirsiniz. Azure, hem Windows hem de Linux sanal makineleri için Hiper yöneticide VM 'nin ekran görüntüsünü görmenizi sağlar. Her iki özellik de tüm bölgelerdeki Azure sanal makineleri için desteklenir. Ekran görüntülerinin ve çıktının depolama hesabınızda görünmesi 10 dakikaya kadar sürebilir.

Günlüğü ve ekran görüntüsünü görüntülemek için **önyükleme tanılama** seçeneğini belirleyebilirsiniz.

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

## <a name="enable-diagnostics-on-a-virtual-machine-created-using-the-azure-portal"></a>Azure portalını kullanarak oluşturulan bir sanal makinede tanılamayı etkinleştirme

Aşağıdaki yordam, Kaynak Yöneticisi dağıtım modeli kullanılarak oluşturulan bir sanal makine içindir.

**Yönetim** sekmesinde, **Izleme** bölümünde, **önyükleme Tanılamanın** açık olduğundan emin olun. **Tanılama depolama hesabı** açılır listesinden, tanılama dosyalarının yerleştirileceği bir depolama hesabı seçin.
 
![VM oluşturma](./media/virtual-machines-common-boot-diagnostics/enable-boot-diagnostics-vm.png)

> [!NOTE]
> Önyükleme tanılaması özelliği Premium Depolama hesabını desteklemez. Önyükleme tanılaması için Premium depolama hesabı kullanıyorsanız, VM 'yi başlattığınızda StorageAccountTypeNotSupported hatasını alabilirsiniz.
>

### <a name="deploying-from-an-azure-resource-manager-template"></a>Azure Resource Manager şablonundan dağıtma

Azure Resource Manager şablondan dağıtıyorsanız, sanal makine kaynağınız ' ne gidin ve tanılama profili bölümüne ekleyin. API sürüm üstbilgisini "2015-06-15" veya üzeri olarak ayarlayın. En son sürüm "2018-10-01" dir.

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

Şablonları kullanarak kaynakları dağıtma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Azure Portal kullanarak Azure Resource Manager şablonları oluşturma ve dağıtma](../../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md).

## <a name="enable-boot-diagnostics-on-existing-virtual-machine"></a>Mevcut sanal makinede önyükleme tanılamayı etkinleştir 

Mevcut bir sanal makinede önyükleme tanılamayı etkinleştirmek için şu adımları izleyin:

1. [Azure Portal](https://portal.azure.com)oturum açın ve sonra sanal makineyi seçin.
2. **Destek + sorun giderme** bölümünde **önyükleme tanılaması**' nı seçin, sonra **Ayarlar** sekmesini seçin.
3. **Önyükleme tanılaması** ayarları ' nda, durumu **Açık**olarak değiştirin ve **depolama hesabı** açılır listesinden bir depolama hesabı seçin. 
4. Yaptığınız değişikliği kaydedin.

    ![Mevcut VM’yi güncelleştirme](./media/virtual-machines-common-boot-diagnostics/enable-for-existing-vm.png)

Değişikliğin etkili olması için sanal makineyi yeniden başlatmanız gerekir.

### <a name="enable-boot-diagnostics-using-the-azure-cli"></a>Azure CLI kullanarak önyükleme tanılamasını etkinleştirme

Azure CLı 'yi, var olan bir Azure sanal makinesinde önyükleme tanılamayı etkinleştirmek için kullanabilirsiniz. Daha fazla bilgi için bkz. [az VM Boot-Diagnostics](
https://docs.microsoft.com/cli/azure/vm/boot-diagnostics?view=azure-cli-latest).
