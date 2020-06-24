---
title: Azure bakım olaylarını planlama
description: Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde planlı bakım olaylarına hazırlanma hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: aamalvea
ms.author: aamalvea
ms.reviewer: carlrab
ms.date: 01/30/2019
ms.openlocfilehash: 524eaa0cfd2137f2b88102bfa5c514d5248cc743
ms.sourcegitcommit: a8928136b49362448e992a297db1072ee322b7fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84718640"
---
# <a name="plan-for-azure-maintenance-events-in-azure-sql-database-and-azure-sql-managed-instance"></a>Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde Azure bakım olaylarını planlayın
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde veritabanınızda planlı bakım olaylarına hazırlanma hakkında bilgi edinin.

## <a name="what-is-a-planned-maintenance-event"></a>Planlı bakım olayı nedir?

Her veritabanı için, Azure SQL veritabanı ve Azure SQL yönetilen örneği, bir çoğaltmanın birincil olduğu veritabanı çoğaltmalarının çekirdeğini korur. Her zaman, birincil bir çoğaltmanın çevrimiçi bakım olması ve en az bir ikincil çoğaltmanın sağlıklı olması gerekir. Planlı bakım sırasında, veritabanı çekirdeğinin üyeleri tek seferde çevrimdışı olacak ve istemci kapalı kalma süresi olmamasını sağlamak için en az bir tane yanıt veren birincil çoğaltma ve çevrimiçi bir ikincil çoğaltma olduğundan emin olur. Birincil çoğaltmanın çevrimdışı hale getirilmesi gerektiğinde, bir ikincil çoğaltmanın yeni birincil hale gelmesi için bir yeniden yapılandırma/yük devretme işlemi meydana gelir.  

## <a name="what-to-expect-during-a-planned-maintenance-event"></a>Planlı bir bakım olayı sırasında beklenmeniz gerekenler

Yeniden yapılandırma/yük devretme işlemleri genellikle 30 saniye içinde tamamlanır. Ortalama 8 saniyedir. Zaten bağlıysa, uygulamanızın veritabanının yeni birincil çoğaltmasını sağlıklı bir şekilde kopyalaması gerekir. Yeni birincil çoğaltma çevrimiçi olmadan önce veritabanı yeniden yapılandırılması sırasında yeni bir bağlantı denendiğinde, 40613 hatasını alırsınız (veritabanı kullanılamıyor): "' {ServerName} ' sunucusundaki" veritabanı ' {DatabaseName} ' Şu anda kullanılamıyor. Lütfen bağlantıyı daha sonra yeniden deneyin." sorununu giderme Veritabanınızda uzun süreli bir sorgu varsa, bu sorgu yeniden yapılandırma sırasında kesintiye uğratılacaktır ve yeniden başlatılması gerekir.

## <a name="retry-logic"></a>Yeniden deneme mantığı

Bir bulut veritabanı hizmetine bağlanan tüm istemci üretim uygulamaları sağlam bir bağlantı [yeniden deneme mantığı](troubleshoot-common-connectivity-issues.md#retry-logic-for-transient-errors)uygulamalıdır. Bu durum, bu durumların azaltılmasına yardımcı olur ve genellikle hataları son kullanıcıya saydam hale getirir.

## <a name="frequency"></a>Sıklık

Ortalama olarak, 1,7 planlı bakım olayları her ay oluşur.

## <a name="resource-health"></a>Kaynak durumu

Veritabanınız oturum açma hatalarıyla karşılaşıyorsa, geçerli durum için [Azure portal](https://portal.azure.com) [kaynak durumu](../../service-health/resource-health-overview.md#get-started) penceresine bakın. Sistem durumu geçmişi bölümü her bir olayın kesinti süresini (kullanılabilir olduğunda) içerir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL veritabanı ve Azure SQL yönetilen örneği için [kaynak durumu](resource-health-to-troubleshoot-connectivity.md) hakkında daha fazla bilgi edinin.
- Yeniden deneme mantığı hakkında daha fazla bilgi için bkz. [geçici hatalar Için yeniden deneme mantığı](troubleshoot-common-connectivity-issues.md#retry-logic-for-transient-errors).
