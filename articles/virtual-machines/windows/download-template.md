---
title: Azure VM şablonu indirin
description: Portalı veya PowerShell'i kullanarak bir VM şablonundan indirin.
author: cynthn
manager: gwallace
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/17/2017
ms.author: cynthn
ms.openlocfilehash: af6905f0ba62a9053e44134348721312ade6b9d7
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82085391"
---
# <a name="download-the-template-for-a-vm"></a>VM için şablon indirme
Portalı veya PowerShell'i kullanarak Azure'da bir VM oluşturduğunuzda, sizin için otomatik olarak bir Kaynak Yöneticisi şablonu oluşturulur. Dağıtımı hızla çoğaltmak için bu şablonu kullanabilirsiniz. Şablon, bir kaynak grubundaki tüm kaynaklar hakkında bilgi içerir. Sanal bir makine için bu, şablonun ağ kaynakları da dahil olmak üzere bu kaynak grubunda VM'yi desteklemek için oluşturulan her şeyi içerdiği anlamına gelir.

## <a name="download-the-template-using-the-portal"></a>Portalı kullanarak şablonu indirin
1. [Azure portalına](https://portal.azure.com/)giriş yapın.
2. Bir sol menü, **Sanal Makineler**seçin.
3. Listeden sanal makineyi seçin.
4. **Dışa Aktar şablonu'nun**
5. Üstteki menüden **İndir'i** seçin ve .zip dosyasını yerel bilgisayarınıza kaydedin.
6. .zip dosyasını açın ve dosyaları bir klasöre ayıklayın. .zip dosyası şunları içerir:
   
   * parametreler.json
   * template.json

template.json dosyası şablondur.

## <a name="download-the-template-using-powershell"></a>PowerShell'i kullanarak şablonu indirin
[Export-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup) cmdlet'i kullanarak .json şablon dosyasını da indirebilirsiniz. .json `-path` dosyasının dosya adını ve yolunu sağlamak için parametreyi kullanabilirsiniz. Bu örnek, **myResourceGroup** adlı kaynak grubunun şablonunu yerel bilgisayarınızdaki **C:\users\public\downloads** klasörüne nasıl indirdiğinizi gösterir.

```powershell
    Export-AzResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Sonraki adımlar
Şablonları kullanarak kaynakları dağıtma hakkında daha fazla bilgi edinmek için [Kaynak Yöneticisi şablonu gözden geçirin.](../../azure-resource-manager/resource-manager-template-walkthrough.md)

