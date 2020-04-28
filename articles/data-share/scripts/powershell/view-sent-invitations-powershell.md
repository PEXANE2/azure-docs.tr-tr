---
title: 'PowerShell betiği: bir tüketiciye gönderilen Azure veri paylaşımının davetlerini listeleyin | Microsoft Docs'
description: Bu PowerShell betiği, var olan bir veri paylaşımından davetleri kabul eder.
services: data-share
author: joannapea
ms.service: data-share
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: 157b844afd9d59b6c03c7ae8585bc2182370d3a3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "70307145"
---
# <a name="use-powershell-to-get-a-data-share-invitation"></a>PowerShell kullanarak veri paylaşma daveti alın

Bu PowerShell betiği bir tüketiciye gönderilen davetleri alır.

## <a name="sample-script"></a>Örnek betik
```powershell
# Set variables with your own values
$invitationId = "<invitationId>"
$location = "<location>"

#List invitations sent to a consumer
Get-AzDataShareInvitation

#Get a specific invitation sent to a consumer
Get-AzDataShareInvitation -location -invitationId 

```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır: 

| Komut | Notlar |
|---|---|
| [Get-Azdatasharedavetini](/powershell/module/az.datashare/get-azdatashareinvitation?view=azps-2.6.0) | Gönderilen veri paylaşma davetlerini alın ve listeleyin. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure veri paylaşımının PowerShell betiği örnekleri, [Azure veri paylaşımında PowerShell örnekleri](../../samples-powershell.md)bölümünde bulunabilir.
