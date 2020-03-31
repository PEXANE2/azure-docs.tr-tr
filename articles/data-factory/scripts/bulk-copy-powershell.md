---
title: PowerShell kullanarak verileri toplu olarak kopyalama
description: Bu PowerShell komut dosyası, bir kaynak veri deposundan hedef veri deposuna toplu olarak veri kopyalamak için Azure Veri Fabrikası'nın nasıl kullanılacağını gösterir.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.custom: seo-lt-2019
ms.date: 10/31/2017
ms.openlocfilehash: efc79f82a2181099f832da0d4a17fc370bf4f7f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74929861"
---
# <a name="powershell-script---copy-multiple-tables-in-bulk-by-using-azure-data-factory"></a>PowerShell komut dosyası - Azure Veri Fabrikası'nı kullanarak birden çok tabloyu toplu olarak kopyalayın

Bu örnek PowerShell komut dosyası, azure SQL veritabanındaki birden çok tablodaki verileri Azure SQL veri ambarına kopyalar.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

[Öğreticiye bakın:](../tutorial-bulk-copy.md#prerequisites) Bu örneği çalıştırmak için ön koşullar için toplu kopya.

## <a name="sample-script"></a>Örnek betik

> [!IMPORTANT]
> Bu komut dosyası, c:\ 'daki sabit diskinizde Veri Fabrikası varlıklarını (bağlantılı hizmet, veri kümesi ve ardışık) tanımlayan JSON dosyaları oluşturur: \ Klasör.

[!code-powershell[main](../../../powershell_scripts/data-factory/bulk-copy-from-sql-databse-to-sql-data-warehouse/bulk-copy-from-sql-database-to-sql-data-warehouse.ps1 "Bulk copy from Azure SQL Database => Azure SQL Data Warehouse")]

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
| [Set-AzDataFactoryV2](/powershell/module/az.datafactory/set-azdatafactoryv2) | Veri fabrikası oluşturma. |
| [Set-AzDataFactoryV2LinkedService](/powershell/module/az.datafactory/set-azdatafactoryv2linkedservice) | Veri fabrikasında bağlantılı bir hizmet oluşturur. Bağlantılı bir hizmet, bir veri deposuveya bilgi işlem bağlantısını bir veri fabrikasına bağlar. |
| [Set-AzDataFactoryV2Dataset](/powershell/module/az.datafactory/set-azdatafactoryv2dataset) | Veri fabrikasında bir veri kümesi oluşturur. Veri kümesi, bir ardışık ardışık alandaki bir etkinlik için giriş/çıktıyı temsil eder. | 
| [Set-AzDataFactoryV2Pipeline](/powershell/module/az.datafactory/set-azdatafactoryv2pipeline) | Veri fabrikasında bir ardışık hat lar oluşturur. Bir ardışık işlem, belirli bir işlemi gerçekleştiren bir veya daha fazla etkinlik içerir. Bu ardışık işlemde, bir kopya etkinliği verileri bir konumdan başka bir konuma bir Azure Blob Depolama'da kopyalar. |
| [Invoke-AzDataFactoryV2Pipeline](/powershell/module/az.datafactory/invoke-azdatafactoryv2pipeline) | Boru hattı için bir çalışma oluşturur. Başka bir deyişle, boru hattını çalıştırın. |
| [Get-AzDataFactoryV2ActivityRun](/powershell/module/az.datafactory/get-azdatafactoryv2activityrun) | Ardışık ardışık ardışık alanda etkinlik (etkinlik çalışması) çalıştırıla ilgili ayrıntıları alır. 
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Veri Fabrikası PowerShell komut örnekleri [Azure Veri Fabrikası PowerShell komut dosyaları](../samples-powershell.md)bulunabilir.
