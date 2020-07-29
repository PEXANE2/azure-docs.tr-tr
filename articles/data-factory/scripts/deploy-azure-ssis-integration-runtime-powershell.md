---
title: PowerShell kullanarak Azure SSIS tümleştirme çalışma zamanı dağıtma
description: Bu PowerShell betiği, bulutta SSIS paketlerini çalıştırabilirler bir Azure-SSIS tümleştirme çalışma zamanı oluşturur.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.author: sawinark
author: swinarko
manager: mflasko
ms.custom: seo-lt-2019
ms.date: 09/12/2017
ms.openlocfilehash: c6d9a9299fa25013a440ee6ac45f5eae407225b8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74929814"
---
# <a name="powershell-script---deploy-azure-ssis-integration-runtime"></a>PowerShell betiği-Azure-SSIS tümleştirme çalışma zamanını dağıtma

Bu örnek PowerShell betiği, Azure 'da SSIS paketlerinizi çalıştırabilirler bir Azure-SSIS tümleştirme çalışma zamanı oluşturur.  

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/data-factory/deploy-azure-ssis-integration-runtime/deploy-azure-ssis-integration-runtime.ps1 "Deploy Azure-SSIS Integration Runtime")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Örnek betiği çalıştırdıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanabilirsiniz:

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
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Set-AzDataFactoryV2](/powershell/module/az.datafactory/set-Azdatafactoryv2) | Veri fabrikası oluşturma. |
| [Set-AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/set-Azdatafactoryv2integrationruntime) | Bulutta SSIS paketlerini çalıştırabilirler bir Azure-SSIS tümleştirme çalışma zamanı oluşturur |
| [Start-AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/start-Azdatafactoryv2integrationruntime) | Azure-SSIS tümleştirme çalışma zamanını başlatır. |
| [Get-AzDataFactoryV2IntegrationRuntime](/powershell/module/az.datafactory/get-Azdatafactoryv2integrationruntime) | Azure-SSIS tümleştirme çalışma zamanı hakkında bilgi alır. |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Data Factory PowerShell betiği örnekleri [Azure Data Factory PowerShell örneklerinde](../samples-powershell.md)bulunabilir.
