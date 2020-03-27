---
title: Azure aboneliğinizin en üst düzey bilgilerini dışarı aktarma
description: Hesabınızla ilişkili tüm Azure abonelik kimliklerini nasıl görüntüleyebileceğinizi açıklar.
author: bandersmsft
ms.reviewer: matrive
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: banders
ms.openlocfilehash: 87135b309f0406528bcada1cd906dd2f8535d1ae
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79202905"
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
