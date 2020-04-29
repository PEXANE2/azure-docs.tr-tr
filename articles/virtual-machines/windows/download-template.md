---
title: Azure VM için şablon indirme
description: Portal veya PowerShell kullanarak bir VM için şablonu indirin.
author: cynthn
manager: gwallace
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/17/2017
ms.author: cynthn
ms.openlocfilehash: af6905f0ba62a9053e44134348721312ade6b9d7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82085391"
---
# <a name="download-the-template-for-a-vm"></a>VM için şablon indirme
Portal veya PowerShell kullanarak Azure 'da bir VM oluşturduğunuzda, sizin için otomatik olarak bir Kaynak Yöneticisi şablonu oluşturulur. Bu şablonu, bir dağıtımı hızlı bir şekilde yinelemek için kullanabilirsiniz. Şablon, bir kaynak grubundaki tüm kaynaklarla ilgili bilgiler içerir. Bu, bir sanal makine için, şablonda ağ kaynakları dahil olmak üzere o kaynak grubundaki VM 'nin desteğiyle oluşturulan her şeyi içerdiği anlamına gelir.

## <a name="download-the-template-using-the-portal"></a>Portalı kullanarak şablonu indirme
1. [Azure Portal](https://portal.azure.com/)oturum açın.
2. Sol bir menü, **sanal makineler**' i seçin.
3. Listeden sanal makineyi seçin.
4. **Şablonu dışarı aktar**' ı seçin.
5. Üstteki menüden **İndir** ' i seçin ve. zip dosyasını yerel bilgisayarınıza kaydedin.
6. . Zip dosyasını açın ve dosyaları bir klasöre ayıklayın. . Zip dosyası şunları içerir:
   
   * Parameters. JSON
   * Template. JSON

Template. JSON dosyası şablondur.

## <a name="download-the-template-using-powershell"></a>PowerShell kullanarak şablonu indirme
. JSON şablon dosyasını [Export-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup) cmdlet 'ini kullanarak da indirebilirsiniz. . Json dosyasının dosya `-path` adını ve yolunu sağlamak için parametresini kullanabilirsiniz. Bu örnek, **Myresourcegroup** adlı kaynak grubunun şablonunun yerel bilgisayarınızdaki **c:\users\public\downloads** klasörüne nasıl indirileceği gösterilmektedir.

```powershell
    Export-AzResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Sonraki adımlar
Şablonları kullanarak kaynak dağıtma hakkında daha fazla bilgi için bkz. [Kaynak Yöneticisi Template izlenecek yol](../../azure-resource-manager/resource-manager-template-walkthrough.md).

