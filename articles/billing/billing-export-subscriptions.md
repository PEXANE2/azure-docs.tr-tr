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
ms.openlocfilehash: 87c31e663529ea20d119753094133941b02e2756
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74226274"
---
# <a name="export-and-view-your-top-level-subscription-information"></a>En üst düzey Abonelik bilgilerinizi dışarı aktarma ve görüntüleme
Kullanıcı kimlik bilgilerinizle ilişkili abonelik kimliklerini görüntülemeniz gerekiyorsa, [Azure Hesap Merkezi’nden abonelik bilgilerinizi içeren .json dosyasını indirin](https://account.azure.com/subscriptions/download).

[!INCLUDE [gdpr-dsr-and-stp-note](../../includes/gdpr-dsr-and-stp-note.md)]

İndirilen .json dosyası şu bilgileri sağlar:
- E-posta: Hesabınızla ilişkili e-posta adresi.
- Puid: Faturalama hesabınızla ilişkili benzersiz tanımlayıcı.
- Abonelik Kimlikleri: Hesabınıza ait olan ve abonelik kimliğine göre numaralandırılan aboneliklerin listesi.

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
