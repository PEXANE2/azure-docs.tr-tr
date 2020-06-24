---
title: Azure Lab Services için PowerShell modülü | Microsoft Docs
description: Bu makalede, Azure Lab Services yapıların yönetilmesine yardımcı olan bir PowerShell modülü hakkında bilgi sağlanır.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2019
ms.author: spelluru
ms.openlocfilehash: cc82355ee43f9fee4f9c2e1bb1bcc0481e4dcea3
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84895307"
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
