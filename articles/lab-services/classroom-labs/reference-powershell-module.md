---
title: Azure Laboratuvar Hizmetleri için PowerShell modülü | Microsoft Dokümanlar
description: Bu makalede, Azure Lab Hizmetleri'ndeki yapıların yönetimine yardımcı olan bir PowerShell modülü hakkında bilgi verilmektedir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73609404"
---
# <a name="azlabservices-powershell-module-preview"></a>Az.LabServices PowerShell modülü (önizleme)
Az.LabServices, Azure Lab hizmetlerinin yönetimini kolaylaştıran bir PowerShell modülüdür. Laboratuvar hesaplarını, laboratuvarları, VM'leri ve Görüntüleri oluşturmak, sorgulamak, güncelleştirmek ve silmek için tek kullanımlık işlevler sağlar. Bu modül hakkında daha fazla bilgi için [GitHub'daki Az.LabServices ana sayfasına](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Modules/Library)bakın.

> [!NOTE]
> Bu modül **önizlemede.** 

## <a name="example-command"></a>Örnek komut
Aşağıda, tüm laboratuarlarda çalışan tüm VM'leri durdurmak için PowerShell komutu kullanmanın bir örneği verilmiştir.

```powershell
Get-AzLabAccount | Get-AzLab | Get-AzLabVm -Status Running | Stop-AzLabVm
```

## <a name="get-started"></a>Kullanmaya başlayın
1. Makinenizde yoksa [Azure PowerShell'i](https://docs.microsoft.com/powershell/azure/overview) yükleyin. 
2. [Az.LabServices.psm1'i](https://github.com/Azure/azure-devtestlab/blob/master/samples/ClassroomLabs/Modules/Library/Az.LabServices.psm1) makinenize indirin.
3. Modülü içe aktarın:

    ```powershell
    Import-Module .\Az.LabServices.psm1
    ```
4. Aboneliğinizdeki tüm laboratuvarları listelemek için aşağıdaki komutu çalıştırın.

    ```powershell
    Get-AzLabAccount | Get-AzLab
    ```

## <a name="next-steps"></a>Sonraki adımlar
[GitHub'daki Az.LabServices ana sayfasına](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Modules/Library)bakın.
