---
title: Eylem Gruplarında SMS Uyarısı davranışı
description: SMS mesaj biçimi ve abonelikten çıkmak, yeniden abone olmak veya yardım istemek için SMS mesajlarına yanıt vermek.
author: dkamstra
ms.author: dukek
services: monitoring
ms.topic: conceptual
ms.date: 02/16/2018
ms.subservice: alerts
ms.openlocfilehash: b75bda626f887f1224c1b0f18a80887983a2367d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77665315"
---
# <a name="sms-alert-behavior-in-action-groups"></a>Eylem Gruplarında SMS Uyarı Davranışı

## <a name="overview"></a>Genel Bakış 
Eylem grupları, eylemlerin listesini yapılandırmanızı sağlar. Bu gruplar uyarıları tanımlarken kullanılır; uyarı tetiklendiğinde belirli bir eylem grubuna bildirilir. Desteklenen eylemlerden biri SMS; SMS bildirimleri çift yönlü iletişimi destekler. Bir kullanıcı bir SMS'e şu şekilde yanıt verebilir:

- **Uyarılardan aboneliği iptal edin:** Bir kullanıcı, tüm eylem grupları veya tek bir eylem grubu için tüm SMS uyarılarından aboneliğini iptal edebilir.
- **Uyarılara yeniden abone olun:** Bir kullanıcı tüm eylem grupları veya tek bir eylem grubu için tüm SMS uyarılarına yeniden abone olabilir.  
- **Yardım isteyin:** Bir kullanıcı SMS hakkında daha fazla bilgi isteyebilir. Bunlar bu makaleye yönlendirilir.

Bu makalede, SMS uyarılarının davranışı ve kullanıcının kullanıcının yerel durumuna göre alabilecekleri yanıt eylemleri ele alınabilen:

## <a name="receiving-an-sms-alert"></a>SMS Uyarısı Alma
Eylem grubunun bir parçası olarak yapılandırılan bir SMS alıcısı, bir uyarı tetiklendiğinde SMS alır. SMS aşağıdaki bilgileri içerir:
* Eylem grubunun kısa adı bu uyarı gönderildi
* Uyarının başlığı

| YANITLA | Açıklama |
| ----- | ----------- |
| Devre dışı bırakmak`<Action Group Short name>` | Eylem Grubundan daha fazla SMS devre dışı |
| Etkinleştirmek`<Action Group Short name>` | Eylem Grubundan SMS'i yeniden etkinleştirir |
| Durdurmak | Tüm Eylem Gruplarından daha fazla SMS devre dışı |
| Başlatmak | TÜM Eylem Gruplarından SMS'leri yeniden etkinleştirir |
| YARDIM | Kullanıcıya bu makalenin bağlantısıyla bir yanıt gönderilir. |

>[!NOTE]
>Bir kullanıcı SMS uyarılarından aboneliğini iptal ettiyse, ancak daha sonra yeni bir eylem grubuna eklendiyse; onlar bu yeni eylem grubu için SMS uyarıları alırsınız, ancak önceki tüm eylem gruplarından aboneliğini kalır.

## <a name="next-steps"></a>Sonraki Adımlar
Etkinlik [günlüğü uyarılarına genel](alerts-overview.md) bir bakış alın ve nasıl uyarı alınacak larını öğrenin  
[SMS oranı sınırlaması](alerts-rate-limiting.md) hakkında daha fazla bilgi edinin  
[Eylem grupları](../../azure-monitor/platform/action-groups.md) hakkında daha fazla bilgi edinin

