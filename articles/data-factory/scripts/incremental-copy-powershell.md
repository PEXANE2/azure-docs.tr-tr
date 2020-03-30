---
title: PowerShell kullanarak verileri kademeli olarak yükleyin
description: Bu PowerShell komut dosyası, verileri Bir Azure SQL Veritabanından Azure Blob Depolamasına artımlı olarak kopyalamak için Azure Veri Fabrikası'nın nasıl kullanılacağını gösterir.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.custom: seo-lt-2019
ms.date: 03/12/2020
ms.openlocfilehash: 1919f89d2e39981effe14c1203446f8f9d930f41
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79462498"
---
# <a name="powershell-script---incrementally-load-data-by-using-azure-data-factory"></a>PowerShell komut dosyası - Azure Veri Fabrikası'nı kullanarak verileri aşamalı olarak yükleyin

Bu örnek PowerShell komut dosyası, kaynaktan lavaboya ilk tam veri kopyasından sonra yalnızca kaynak veri deposundan yeni veya güncelleştirilmiş kayıtları bir lavabo veri deposuna yükler.  

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

Öğreticiye bakın: Bu örneği çalıştırmak için ön koşullar için [artımlı kopya.](../tutorial-incremental-copy-powershell.md#prerequisites) 

## <a name="sample-script"></a>Örnek betik

> [!IMPORTANT]
> Bu komut dosyası, c:\ 'daki sabit diskinizde Veri Fabrikası varlıklarını (bağlantılı hizmet, veri kümesi ve ardışık) tanımlayan JSON dosyaları oluşturur: \ Klasör.

:::code language="powershell" source="~/powershell_scripts/data-factory/incremental-copy-from-azure-sql-to-blob/incremental-copy-from-azure-sql-to-blob.ps1":::

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Örnek komut dosyasını çalıştırdıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanabilirsiniz:

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```
Veri fabrikasını kaynak grubundan kaldırmak için aşağıdaki komutu çalıştırın: 

```powershell
Remove-AzDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Set-AzDataFactoryV2](/powershell/module/az.datafactory/set-Azdatafactoryv2) | Veri fabrikası oluşturma. |
| [Set-AzDataFactoryV2LinkedService](/powershell/module/az.datafactory/Set-Azdatafactoryv2linkedservice) | Veri fabrikasında bağlantılı bir hizmet oluşturur. Bağlantılı bir hizmet, bir veri deposuveya bilgi işlem bağlantısını bir veri fabrikasına bağlar. |
| [Set-AzDataFactoryV2Dataset](/powershell/module/az.datafactory/Set-Azdatafactoryv2dataset) | Veri fabrikasında bir veri kümesi oluşturur. Veri kümesi, bir ardışık ardışık alandaki bir etkinlik için giriş/çıktıyı temsil eder. | 
| [Set-AzDataFactoryV2Pipeline](/powershell/module/az.datafactory/Set-Azdatafactoryv2pipeline) | Veri fabrikasında bir ardışık hat lar oluşturur. Bir ardışık işlem, belirli bir işlemi gerçekleştiren bir veya daha fazla etkinlik içerir. Bu ardışık işlemde, bir kopya etkinliği verileri bir konumdan başka bir konuma bir Azure Blob Depolama'da kopyalar. |
| [Invoke-AzDataFactoryV2Pipeline](/powershell/module/az.datafactory/Invoke-Azdatafactoryv2pipeline) | Boru hattı için bir çalışma oluşturur. Başka bir deyişle, boru hattını çalıştırın. |
| [Get-AzDataFactoryV2ActivityRun](/powershell/module/az.datafactory/get-Azdatafactoryv2activityrun) | Ardışık ardışık ardışık alanda etkinlik (etkinlik çalışması) çalıştırıla ilgili ayrıntıları alır. 
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Veri Fabrikası PowerShell komut örnekleri [Azure Veri Fabrikası PowerShell komut dosyaları](../samples-powershell.md)bulunabilir.
