---
title: Azure Lab Services için PowerShell modülü | Microsoft Docs
description: Bu makalede, Azure Lab Services yapıların yönetilmesine yardımcı olan bir PowerShell modülü hakkında bilgi sağlanır.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: d9c184f3917be378eb77d1bf4096bfebb5ee1884
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85445602"
---
# <a name="azlabservices-powershell-module-preview"></a>Az.LabServices PowerShell modülü (önizleme)
Az. LabServices, Azure Laboratuvar Hizmetleri 'nin yönetimini kolaylaştıran bir PowerShell modülüdür. Laboratuvar hesapları, laboratuvarlar, VM 'Ler ve görüntüler oluşturmak, sorgulamak, güncelleştirmek ve silmek için birleştirilebilir işlevler sağlar. Bu modülle ilgili daha fazla bilgi için [GitHub 'Daki az. LabServices giriş sayfasına](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Modules/Library)bakın.

> [!NOTE]
> Bu modül **önizlemededir**. 

## <a name="example-command"></a>Örnek komut
Tüm laboratuvarlarda çalışan tüm VM 'Leri durdurmak için bir PowerShell komutu kullanılmasına bir örnek aşağıda verilmiştir.

```powershell
Get-AzLabAccount | Get-AzLab | Get-AzLabVm -Status Running | Stop-AzLabVm
```

## <a name="get-started"></a>başlarken
1. Makinenizde yoksa [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) . 
2. Makinenize [az. LabServices. psm1](https://github.com/Azure/azure-devtestlab/blob/master/samples/ClassroomLabs/Modules/Library/Az.LabServices.psm1) indirin.
3. Modülü içeri aktar:

    ```powershell
    Import-Module .\Az.LabServices.psm1
    ```
4. Aboneliğinizdeki tüm laboratuvarları listelemek için aşağıdaki komutu çalıştırın.

    ```powershell
    Get-AzLabAccount | Get-AzLab
    ```

## <a name="next-steps"></a>Sonraki adımlar
[GitHub 'Daki az. LabServices giriş sayfasına](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Modules/Library)bakın.
