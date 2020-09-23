---
title: 'PowerShell betiği: bir tüketiciye gönderilen Azure veri paylaşımının davetlerini listeleyin | Microsoft Docs'
description: Bu PowerShell betiğinin bir tüketiciye gönderilen davetleri nasıl kullandığını öğrenin ve kullanabileceğiniz betiğin bir örneğini görüntüleyin.
services: data-share
author: joannapea
ms.service: data-share
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/07/2019
ms.author: joanpo
ms.openlocfilehash: 3f0374ba5b98c145359d18143584e23fc543c552
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90985622"
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
