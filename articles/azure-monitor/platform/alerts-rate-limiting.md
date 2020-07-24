---
title: SMS, e-postalar, anında iletme bildirimleri için hız sınırlaması
description: Azure 'un olası SMS, e-posta, Azure Uygulama itme veya Web kancası bildirimlerinin sayısını bir eylem grubundan nasıl sınırlamasını anlayın.
author: dkamstra
ms.author: dukek
ms.topic: conceptual
ms.date: 3/12/2018
ms.subservice: alerts
ms.openlocfilehash: 39b9fd8396601d777aa802a147bee3acc4a22cd0
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87045295"
---
# <a name="rate-limiting-for-voice-sms-emails-azure-app-push-notifications-and-webhook-posts"></a>Ses, SMS, e-posta, Azure Uygulama anında iletme bildirimleri ve Web kancası gönderileri için hız sınırlaması
Hız sınırlaması, belirli bir telefon numarasına, e-posta adresine veya cihaza çok fazla gönderildiğinde oluşan bildirimlerin askıya alınma hızıdır. Hız sınırlaması, uyarıların yönetilebilir ve işlem yapılabilir olmasını sağlar.

Hız limiti eşikleri şunlardır:

- **SMS**: 5 dakikada bir en fazla 1 SMS.
- **Ses**: 5 dakikada bir en fazla 1 ses çağrısı yoktur.
- **E-posta**: bir saatte 100 ' den fazla e-posta yok.
 
  Diğer eylemler hız sınırlı değildir.

## <a name="rate-limit-rules"></a>Hız sınırı kuralları
- Belirli bir telefon numarası veya e-posta, eşiğin izin verdiğinden daha fazla ileti aldığında hız sınırlı olur.
- Telefon numarası veya e-posta, birçok abonelik üzerinde eylem gruplarının bir parçası olabilir. Tüm abonelikler arasında hız sınırlaması uygulanır. Bu, birden çok abonelikten iletiler gönderilse bile eşiğe ulaşıldığında geçerlidir.
- Bir e-posta adresi hız sınırlı olduğunda, hız sınırlaması ile iletişim kurmak için ek bir bildirim gönderilir. Bu e-posta, hız sınırlaması sona erdiğinde belirtilir.

## <a name="next-steps"></a>Sonraki adımlar ##
* [SMS uyarı davranışı](alerts-sms-behavior.md)hakkında daha fazla bilgi edinin.
* [Etkinlik günlüğü uyarılarına genel bir bakış](alerts-overview.md)elde edin ve uyarıları alma hakkında bilgi edinin.  
* [Bir hizmet durumu bildirimi gönderildiğinde uyarıların nasıl yapılandırılacağını](../../service-health/alerts-activity-log-service-notifications-portal.md)öğrenin.
