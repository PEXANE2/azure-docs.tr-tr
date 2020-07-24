---
title: Azure Lab Services için PowerShell modülü | Microsoft Docs
description: Bu makalede, Azure Lab Services yapıların yönetilmesine yardımcı olan bir PowerShell modülü hakkında bilgi sağlanır.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 6f5809dcf981a1ecf30cda06af03d2b8a06d3694
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87078830"
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
1. Makinenizde yoksa [Azure PowerShell](https://docs.microsoft.com/powershell/azure/) . 
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
