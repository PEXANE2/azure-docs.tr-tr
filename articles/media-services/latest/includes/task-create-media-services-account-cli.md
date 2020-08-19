---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: 5c0341087cdd348e973da5faaa1f90081780c9c8
ms.sourcegitcommit: 37afde27ac137ab2e675b2b0492559287822fded
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88602509"
---
<!--Create a media services account -->

Aşağıdaki Azure CLI komutu yeni bir Media Services hesabı oluşturur. Aşağıdaki değerleri değiştirebilirsiniz: `amsaccount` `storageaccountforams` (depolama hesabınız için verdiğiniz değerle eşleşmelidir) ve `amsResourceGroup` (kaynak grubu için verdiğiniz değerle eşleşmesi gerekir).  

```azurecli
az ams account create --name amsaccount -g amsResourceGroup --storage-account storageaccountforams -l westus2
```