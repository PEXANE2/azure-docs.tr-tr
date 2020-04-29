---
title: Eylem gruplarında SMS uyarı davranışı
description: SMS ileti biçimi ve abonelik kaldırma, yeniden gönderme veya yardım isteme için SMS iletilerine yanıt verme.
author: dkamstra
ms.author: dukek
services: monitoring
ms.topic: conceptual
ms.date: 02/16/2018
ms.subservice: alerts
ms.openlocfilehash: b75bda626f887f1224c1b0f18a80887983a2367d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77665315"
---
# <a name="sms-alert-behavior-in-action-groups"></a>Eylem gruplarında SMS uyarı davranışı

## <a name="overview"></a>Genel Bakış 
Eylem grupları, eylemlerin bir listesini yapılandırmanızı sağlar. Bu gruplar, uyarıları tanımlarken kullanılır; uyarı tetiklendiğinde belirli bir eylem grubuna bildirim gönderilmesini sağlama. Desteklenen eylemlerden biri SMS; SMS bildirimleri iki yönlü iletişimi destekler. Bir kullanıcı SMS 'ye yanıt verebilir:

- **Uyarılardan abonelik kaldırma:** Bir Kullanıcı tüm eylem grupları veya tek bir eylem grubu için tüm SMS uyarılarından abonelikten çıkabilir.
- **Uyarıları yeniden gönderme:** Kullanıcı, tüm eylem grupları veya tek bir eylem grubu için tüm SMS uyarılarını yeniden alabilir.  
- **Yardım iste:** Bir kullanıcı SMS hakkında daha fazla bilgi isteyebilir. Bu makaleye yeniden yönlendirilir.

Bu makale, SMS uyarılarının davranışını ve kullanıcının yerel ayar temelinde gerçekleştirebileceği yanıt eylemlerini içerir:

## <a name="receiving-an-sms-alert"></a>SMS uyarısı alma
Bir eylem grubunun parçası olarak yapılandırılmış bir SMS alıcısı, bir uyarı tetiklendiğinde SMS alır. SMS, aşağıdaki bilgileri içerir:
* Bu uyarının gönderildiği eylem grubunun ShortName 'ı
* Uyarının başlığı

| YANITLA | Açıklama |
| ----- | ----------- |
| Dıı`<Action Group Short name>` | Eylem grubundan daha fazla SMS 'yi devre dışı bırakır |
| ETKINLEŞTIREBILIR`<Action Group Short name>` | Eylem grubundan SMS 'yi yeniden etkinleştirilir |
| DURDURULMASı | Tüm eylem gruplarından daha fazla SMS 'yi devre dışı bırakır |
| BAŞıNDAN | TÜM eylem gruplarından SMS 'yi yeniden etkinleştirin |
| YARDIM | Bu makaleye bir bağlantı ile kullanıcıya bir yanıt gönderilir. |

>[!NOTE]
>Bir kullanıcı SMS uyarılarından aboneliği kaldırırsa, ancak yeni bir eylem grubuna eklenirse; Bu yeni eylem grubu için SMS uyarıları alırlar, ancak önceki tüm eylem gruplarından abone olunmayacak şekilde kalır.

## <a name="next-steps"></a>Sonraki Adımlar
[Etkinlik günlüğü uyarılarına genel bir bakış](alerts-overview.md) edinin ve nasıl uyarı alabileceğinizi öğrenin  
[SMS hız sınırlaması](alerts-rate-limiting.md) hakkında daha fazla bilgi edinin  
[Eylem grupları](../../azure-monitor/platform/action-groups.md) hakkında daha fazla bilgi edinin

