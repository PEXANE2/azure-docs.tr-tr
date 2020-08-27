---
title: Azure bakım olaylarını planlayın
description: Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde planlı bakım olaylarına hazırlanma hakkında bilgi edinin.
services: sql-database
ms.service: sql-db-mi
ms.subservice: service
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: aamalvea
ms.author: aamalvea
ms.reviewer: carlrab
ms.date: 08/25/2020
ms.openlocfilehash: 4c7b78f14602632068a19d520aeeb940b543be61
ms.sourcegitcommit: e69bb334ea7e81d49530ebd6c2d3a3a8fa9775c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88948224"
---
# <a name="plan-for-azure-maintenance-events-in-azure-sql-database-and-azure-sql-managed-instance"></a>Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde Azure bakım olaylarını planlayın
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde veritabanınızda planlı bakım olaylarına hazırlanma hakkında bilgi edinin.

## <a name="what-is-a-planned-maintenance-event"></a>Planlı bakım olayı nedir?

Azure SQL veritabanı ve Azure SQL yönetilen örnek hizmetlerini güvenli, uyumlu, kararlı ve iyi durumda tutmak için güncelleştirmeler neredeyse sürekli hizmet bileşenleri üzerinden gerçekleştiriliyor. [Sık yama](https://aka.ms/azuresqlhotpatching)gibi modern ve sağlam hizmet mimarisi ve yenilikçi teknolojiler sayesinde güncelleştirmelerin büyük bölümü, hizmet kullanılabilirliği bakımından tamamen saydamdır ve etkili değildir. Hala, bazı güncelleştirme türleri kısa hizmet kesintileri oluşmasına neden olur ve özel bir işleme gerektirir. 

Her veritabanı için, Azure SQL veritabanı ve Azure SQL yönetilen örneği, bir çoğaltmanın birincil olduğu veritabanı çoğaltmalarının çekirdeğini korur. Her zaman, birincil bir çoğaltmanın çevrimiçi bakım olması ve en az bir ikincil çoğaltmanın sağlıklı olması gerekir. Planlı bakım sırasında, veritabanı çekirdeğinin üyeleri tek seferde çevrimdışı olacak ve istemci kapalı kalma süresi olmamasını sağlamak için en az bir tane yanıt veren birincil çoğaltma ve çevrimiçi bir ikincil çoğaltma olduğundan emin olur. Birincil çoğaltmanın çevrimdışı hale getirilmesi gerektiğinde, bir ikincil çoğaltmanın yeni birincil hale gelmesi için bir yeniden yapılandırma/yük devretme işlemi meydana gelir.  

## <a name="what-to-expect-during-a-planned-maintenance-event"></a>Planlı bir bakım olayı sırasında beklenmeniz gerekenler

Bakım olayı, bakım olayının başlangıcında birincil ve ikincil çoğaltmaların yarışmasına bağlı olarak tek veya birden fazla yük devretme üretebilir. Ortalama olarak, 1,7 yük devretme planlı bakım olayı başına gerçekleşir. Yeniden yapılandırma/yük devretme işlemleri genellikle 30 saniye içinde tamamlanır. Ortalama 8 saniyedir. Zaten bağlantı varsa, uygulamanızın veritabanınızın yeni birincil çoğaltmasına yeniden bağlanması gerekir. Yeni birincil çoğaltma çevrimiçi olmadan önce veritabanı yeniden yapılandırılması sırasında yeni bir bağlantı denendiğinde, 40613 hatasını alırsınız (veritabanı kullanılamıyor): *"' {ServerName} ' sunucusundaki" veritabanı ' {DatabaseName} ' Şu anda kullanılamıyor. Lütfen bağlantıyı daha sonra yeniden deneyin. "* Veritabanınızda uzun süreli bir sorgu varsa, bu sorgu yeniden yapılandırma sırasında kesintiye uğratılacaktır ve yeniden başlatılması gerekir.

## <a name="how-to-simulate-a-planned-maintenance-event"></a>Planlı bakım olayının benzetimini yapma

İstemci uygulamanızın üretime dağıtmadan önce bakım olaylarına dayanıklı olmasını sağlamak, uygulama hatalarının riskini azaltmaya yardımcı olur ve son kullanıcılarınız için uygulama kullanılabilirliğine katkıda bulunur. PowerShell, CLı veya REST API aracılığıyla [el ile yük devretmeyi başlatarak](https://aka.ms/mifailover-techblog) , planlı bakım olayları sırasında istemci uygulamanızın davranışını test edebilirsiniz. Birincil çoğaltmayı çevrimdışına getiren bakım olayı olarak aynı davranışı üretir.

## <a name="retry-logic"></a>Yeniden deneme mantığı

Bir bulut veritabanı hizmetine bağlanan tüm istemci üretim uygulamaları sağlam bir bağlantı [yeniden deneme mantığı](troubleshoot-common-connectivity-issues.md#retry-logic-for-transient-errors)uygulamalıdır. Bu, yük devretmeleri son kullanıcılara şeffaf hale getirmenize veya en azından negatif etkileri en aza indirmenize yardımcı olur.

## <a name="resource-health"></a>Kaynak durumu

Veritabanınız oturum açma hatalarıyla karşılaşıyorsa, geçerli durum için [Azure portal](https://portal.azure.com) [kaynak durumu](../../service-health/resource-health-overview.md#get-started) penceresine bakın. Sistem durumu geçmişi bölümü her bir olayın kesinti süresini (kullanılabilir olduğunda) içerir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL veritabanı ve Azure SQL yönetilen örneği için [kaynak durumu](resource-health-to-troubleshoot-connectivity.md) hakkında daha fazla bilgi edinin.
- Yeniden deneme mantığı hakkında daha fazla bilgi için bkz. [geçici hatalar Için yeniden deneme mantığı](troubleshoot-common-connectivity-issues.md#retry-logic-for-transient-errors).
