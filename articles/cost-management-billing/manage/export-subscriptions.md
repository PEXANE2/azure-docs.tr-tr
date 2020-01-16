---
title: Azure aboneliğinizin en üst düzey bilgilerini dışarı aktarma | Microsoft Docs
description: Hesabınızla ilişkili tüm Azure abonelik kimliklerini nasıl görüntüleyebileceğinizi açıklar.
keywords: ''
services: billing
documentationcenter: ''
author: adpick
manager: adpick
editor: ''
tags: billing
ms.service: cost-management-billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: banders
ms.openlocfilehash: 553cc7fd27571ebc925e33f824060c023664a369
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75991782"
---
# <a name="export-and-view-your-top-level-subscription-information"></a>En üst düzey Abonelik bilgilerinizi dışarı aktarma ve görüntüleme
Kullanıcı kimlik bilgilerinizle ilişkili abonelik kimliklerini görüntülemeniz gerekiyorsa, [Azure Hesap Merkezi’nden abonelik bilgilerinizi içeren .json dosyasını indirin](https://account.azure.com/subscriptions/download).

[!INCLUDE [gdpr-dsr-and-stp-note](../../../includes/gdpr-dsr-and-stp-note.md)]

İndirilen .json dosyası şu bilgileri sağlar:
- E-posta: hesabınızla ilişkili e-posta adresi.
- PUID: Faturalandırma hesabınızla ilişkili benzersiz tanımlayıcı.
- Subscriptionıds: hesabınıza ait olan ve abonelik KIMLIĞI tarafından numaralandırılan aboneliklerin listesi.

### <a name="subscriptionsjson-sample"></a>subscriptions.json örneği

```json
{
  "Email":"admin@contoso.com",
  "Puid":"00052xxxxxxxxxxx",
  "SubscriptionIds":[
    "38124d4d-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "7c8308f1-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "39a25f2b-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "52ec2489-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "e42384b2-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "90757cdc-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
  ]
}
```
