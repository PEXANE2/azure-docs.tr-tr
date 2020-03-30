---
title: PostgreSQL için Azure Veritabanı'ndaki en iyi uygulamaları sorgula - Tek Sunucu
description: Bu makalede, PostgreSQL - Single Server için Azure Veritabanı'ndaki Sorgu Deposu için en iyi uygulamalar açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 51239f4cf49784dd47470e1272b90508eaf25e6f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70764228"
---
# <a name="best-practices-for-query-store"></a>Sorgu Deposu için en iyi uygulamalar

**Aşağıdakiler için geçerlidir:** PostgreSQL için Azure Veritabanı - Tek Sunucu sürümleri 9.6, 10, 11

Bu makalede, PostgreSQL için Azure Veritabanı'nda Query Store'u kullanmak için en iyi uygulamalar özetlanmaktadır.

## <a name="set-the-optimal-query-capture-mode"></a>En uygun sorgu yakalama modunu ayarlama
Sorgu Mağazası'nın sizin için önemli olan verileri yakalamasına izin verin. 

|**pg_qs.query_capture_mode** | **Senaryo**|
|---|---|
|_Tümü_  |Tüm sorgular ve bunların yürütme frekansları ve diğer istatistikler açısından iş yükünüzü iyice analiz edin. İş yüklerinizdeki yeni sorguları tanımlayın. Kullanıcı veya otomatik parametreleştirme fırsatlarını belirlemek için geçici sorguların kullanIlip kullanılmaymasını algıla. _Tüm_ artış kaynak tüketim maliyeti ile birlikte gelir. |
|_Sayfanın Üstü_  |Dikkatinizi en iyi sorgulara odakla - müşteriler tarafından verilenler.
|_Yok_ |Araştırmak istediğiniz bir sorgu kümesini ve zaman penceresini zaten yakaladınız ve diğer sorguların sunabileceği dikkat dağıtıcı şeyleri ortadan kaldırmak istiyorsunuz. _Hiçbiri_ test ve tezgah işaretleme ortamları için uygun değildir. Önemli yeni sorguları izleme ve en iyi duruma getirme fırsatını kaçırabileceğinizden _hiçbiri_ dikkatli kullanılmamalıdır. Geçmiş zaman pencerelerinde veri kurtaramazsınız. |

Sorgu Deposu, bekleme istatistikleri için bir mağaza da içerir. Bekleme istatistiklerini yöneten ek bir yakalama modu sorgusu vardır: **pgms_wait_sampling.query_capture_mode** _hiçbiri_ veya _tümü_olarak ayarlanabilir. 

> [!NOTE] 
> **pg_qs.query_capture_mode** **pgms_wait_sampling.query_capture_mode**yerini. pg_qs.query_capture_mode _yoksa,_ pgms_wait_sampling.query_capture_mode ayarı hiçbir etkisi yoktur. 


## <a name="keep-the-data-you-need"></a>İhtiyacınız olan verileri saklayın
**pg_qs.retention_period_in_days** parametresi, Sorgu Deposu için veri saklama süresini gün içinde belirtir. Eski sorgu ve istatistik verileri silinir. Varsayılan olarak, Query Store verileri 7 gün boyunca tutacak şekilde yapılandırılır. Kullanmayı planlamadığınız geçmiş verileri saklamaktan kaçının. Verileri daha uzun tutmanız gerekiyorsa değeri artırın.


## <a name="set-the-frequency-of-wait-stats-sampling"></a>Bekleme istatistikleri örneklemesinin sıklığını ayarlama 
**pgms_wait_sampling.history_period** parametresi bekleme olaylarının ne sıklıkta (milisaniye cinsinden) örneklenir olduğunu belirtir. Süre ne kadar kısaysa, örnekleme o kadar sık olur. Daha fazla bilgi alınır, ancak bu daha fazla kaynak tüketimi nin maliyetiyle birlikte gelir. Sunucu yük altındaysa veya parçalılığa ihtiyacınız yoksa bu süreyi artırın


## <a name="get-quick-insights-into-query-store"></a>Sorgu Mağazası hakkında hızlı bilgiler edinin
Sorgu Mağazası'ndaki veriler hakkında hızlı bilgi edinmek için Azure portalında Sorgu Performans Öngörüsün'yü kullanabilirsiniz. [Query Performance Insight](concepts-query-performance-insight.md) Görselleştirmeler, zaman içinde en uzun süren sorguları ve en uzun bekleme olaylarını yüzler.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure portalını](howto-configure-server-parameters-using-portal.md) veya [Azure CLI'yi](howto-configure-server-parameters-using-cli.md)kullanarak parametreleri nasıl elde edin veya ayarlayabilirsiniz öğrenin.