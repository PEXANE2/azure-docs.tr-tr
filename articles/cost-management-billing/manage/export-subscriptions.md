---
title: Azure aboneliğinizin en üst düzey bilgilerini dışarı aktarma | Microsoft Docs
description: Hesabınızla ilişkili tüm Azure abonelik kimliklerini nasıl görüntüleyebileceğinizi açıklar.
keywords: ''
author: bandersmsft
ms.reviewer: matrive
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: banders
ms.openlocfilehash: 83b6a9db0107cb35448aec491c81c2630e87ddd7
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77199730"
---
# <a name="export-and-view-your-top-level-subscription-information"></a>En üst düzey Abonelik bilgilerinizi dışarı aktarma ve görüntüleme
Kullanıcı kimlik bilgilerinizle ilişkili abonelik kimliklerini görüntülemeniz gerekiyorsa, [Azure Hesap Merkezi’nden abonelik bilgilerinizi içeren .json dosyasını indirin](https://account.azure.com/subscriptions/download).

[!INCLUDE [gdpr-dsr-and-stp-note](../../../includes/gdpr-dsr-and-stp-note.md)]

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
