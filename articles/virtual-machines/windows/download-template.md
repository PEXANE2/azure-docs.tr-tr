---
title: Azure VM şablonu indirin
description: Kaynak Yöneticisi dağıtım modelindeki dağıtımları otomatikleştirmek için VM şablonlarını indirin
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 51ef4f51-0942-4249-afea-4a3f87ce1ff8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 11/17/2017
ms.author: cynthn
ms.openlocfilehash: c73026515f0d7fde4e2f82838696700b1bb17c77
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74033558"
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

