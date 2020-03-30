---
title: Azure bakım etkinlikleri için planlama
description: Azure SQL Veritabanınızda planlanan bakım etkinliklerine nasıl hazırlanacağınızı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aamalvea
ms.author: aamalvea
ms.reviewer: carlrab
ms.date: 01/30/2019
ms.openlocfilehash: ba882176fbe17f7b74c786f421dde8fadd58d9b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73821311"
---
# <a name="planning-for-azure-maintenance-events-in-azure-sql-database"></a>Azure SQL Veritabanı'nda Azure bakım olaylarını planlama

Azure SQL veritabanınızda planlanan bakım etkinliklerine nasıl hazırlanacağınızı öğrenin.

## <a name="what-is-a-planned-maintenance-event"></a>Planlı bakım olayı nedir

Her veritabanı için Azure SQL DB, bir yinelemenin birincil olduğu bir veritabanı yinelemesi çoğunluğu tutar. Her zaman birincil yineleme çevrimiçi hizmet olmalıdır ve en az bir ikincil yineleme sağlıklı olmalıdır. Planlanan bakım sırasında, veritabanı çoğunluk üyeleri, istemci kapalı kalma süresi sağlamak için bir yanıt birincil yineleme ve en az bir ikincil yineleme çevrimiçi olması niyetiyle, birer birer çevrimdışı gidecek. Birincil yinelemenin çevrimdışı getirilmesi gerektiğinde, ikincil bir yinelemenin yeni birincil olacağı bir yeniden yapılandırma/başarısız olma işlemi oluşur.  

## <a name="what-to-expect-during-a-planned-maintenance-event"></a>Planlanan bakım etkinliğinde neler beklenebilir?

Yeniden yapılandırmalar/arızalar genellikle 30 saniye içinde tamamlanır – ortalama 8 saniyedir. Zaten bağlıysa, uygulamanızın veritabanınızın sağlıklı kopya yeni birincil kopyasına yeniden bağlanması gerekir. Veritabanı yeni birincil yineleme çevrimiçi olmadan önce yeniden yapılandırma dan geçerken yeni bir bağlantı denenirse, hata 40613 (Database Kullanılamaz): "Veritabanı '{databasename}' sunucu '{servername}' şu anda kullanılamıyor. Lütfen bağlantıyı daha sonra yeniden deneyin.". Veritabanınızda uzun süre çalışan bir sorgu varsa, bu sorgu yeniden yapılandırma sırasında kesilir ve yeniden başlatılması gerekir.

## <a name="retry-logic"></a>Yeniden Deneme Mantığı

Bulut veritabanı hizmetine bağlanan herhangi bir istemci üretim uygulaması, sağlam bir bağlantı [yeniden deneme mantığı](sql-database-connectivity-issues.md#retry-logic-for-transient-errors)uygulamalıdır. Bu, bu durumların hafifletimine yardımcı olur ve genellikle hataları son kullanıcıya saydam hale getirmelidir.

## <a name="frequency"></a>Frequency

Her ay ortalama 1,7 planlı bakım olayı meydana gelir.

## <a name="resource-health"></a>Kaynak Durumu

SQL veritabanınızda oturum açma hataları yaşıyorsa, geçerli durum için [Azure portalındaki](https://portal.azure.com) [Kaynak Durumu](../service-health/resource-health-overview.md#get-started) penceresine bakın. Sistem Durumu Geçmişi bölümü, her olayın (varsa) kapalı kalma nedenini içerir.


## <a name="next-steps"></a>Sonraki adımlar

- SQL Veritabanı için [Kaynak Durumu](sql-database-resource-health.md) hakkında daha fazla bilgi edinin
- Yeniden deneme mantığı hakkında daha fazla bilgi için geçici [hatalar için Yeniden Deneme mantığına](sql-database-connectivity-issues.md#retry-logic-for-transient-errors) bakın
