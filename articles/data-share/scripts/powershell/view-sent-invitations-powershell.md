---
title: 'PowerShell betiği: tüketiciye gönderilen Azure veri paylaşma davetlerini listeleyin'
description: Bu PowerShell betiğinin bir tüketiciye gönderilen davetleri nasıl kullandığını öğrenin ve kullanabileceğiniz betiğin bir örneğini görüntüleyin.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: article
ms.date: 07/07/2019
ms.openlocfilehash: 5f75894094fa2a15dbc9e2809ed6c3631df96c3f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "92221222"
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
| [Get-Azdatasharedavetini](/powershell/module/az.datashare/get-azdatashareinvitation) | Gönderilen veri paylaşma davetlerini alın ve listeleyin. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/).

Ek Azure veri paylaşımının PowerShell betiği örnekleri, [Azure veri paylaşımında PowerShell örnekleri](../../samples-powershell.md)bölümünde bulunabilir.
