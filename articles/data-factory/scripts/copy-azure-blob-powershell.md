---
title: PowerShell'i kullanarak buluttaki verileri kopyalama
description: Bu PowerShell komut dosyası, verileri Azure Blob Depolama'daki bir konumdan aynı Blob Depolama'daki başka bir konuma kopyalar.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.custom: seo-lt-2019
ms.date: 03/12/2020
ms.openlocfilehash: 2e289f30ef1c1883c38884eb563a41bccc841329
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79462660"
---
# <a name="use-powershell-to-create-a-data-factory-pipeline-to-copy-data-in-the-cloud"></a>Buluttaki verileri kopyalamak için bir veri fabrikası ardışık hattı oluşturmak için PowerShell'i kullanın

Bu örnek PowerShell komut dosyası, Azure Veri Fabrikası'nda verileri bir konumdan başka bir konuma kopyalayan bir ardışık işlem aygıtı oluşturur.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>Ön koşullar
* **Azure Depolama hesabı.** Blob depolama alanını hem **kaynak** hem de **havuz** veri deposu olarak kullanabilirsiniz. Azure depolama hesabınız yoksa, bir depolama alanı oluştururken [bir depolama hesabı oluştur'a](../../storage/common/storage-account-create.md) bakın. 
* Blob Depolama içinde bir **blob kapsayıcısı** oluşturun, kapsayıcıda bir giriş **klasörü** oluşturun ve bazı dosyaları klasöre yükleyin. [Azure Depolama gezgini](https://azure.microsoft.com/features/storage-explorer/) gibi araçları kullanarak Azure Blob depolama hesabına bağlanabilir, bir blob kapsayıcısı oluşturabilir, giriş dosyasını karşıya yükleyebilir ve çıktı dosyasını doğrulayabilirsiniz.

## <a name="sample-script"></a>Örnek betik

> [!IMPORTANT]
> Bu komut dosyası, c:\ 'daki sabit diskinizde Veri Fabrikası varlıklarını (bağlantılı hizmet, veri kümesi ve ardışık) tanımlayan JSON dosyaları oluşturur: \ Klasör.

[!code-powershell[main](../../../powershell_scripts/data-factory/copy-from-azure-blob-to-blob/copy-from-azure-blob-to-blob.ps1 "Copy from Blob Storage -> Blob Storage")]


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

Ek Azure Veri Fabrikası PowerShell komut dosyası örnekleri [Azure Veri Fabrikası PowerShell örneklerinde](../samples-powershell.md)bulunabilir.
