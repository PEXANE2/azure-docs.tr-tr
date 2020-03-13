---
title: Veritabanı sistem durumunu izlemek için Azure Kaynak Durumu kullanma
description: Azure kaynak durumu, SQL veritabanı sistem durumu, tanılamanıza ve bir Azure sorunu SQL kaynaklarınızı etkilediğinde, destek almanıza yardımcı izlemek için kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aamalvea
ms.author: aamalvea
ms.reviewer: jrasnik, carlrab
ms.date: 02/26/2019
ms.openlocfilehash: 9e19e904b47d69444b491dd88ffe49ff812aafc3
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79208864"
---
# <a name="use-resource-health-to-troubleshoot-connectivity-for-azure-sql-database"></a>Azure SQL veritabanı için bağlantı sorunlarını gidermek için kaynak durumu kullanın

## <a name="overview"></a>Genel Bakış

SQL veritabanı için [kaynak durumu](../service-health/resource-health-overview.md#get-started) , SQL kaynaklarınızı etkileyen bir Azure sorunu hakkında tanılamanıza ve destek almanıza yardımcı olur. Kaynaklarınızın güncel ve geçmiş durumu hakkında bilgiler sağlar ve sorunları azaltmaya yardımcı olur. Kaynak Durumu, Azure hizmet sorunları ile ilgili yardıma ihtiyacınız olduğunda teknik destek sağlar.

![Genel Bakış](./media/sql-database-resource-health/sql-resource-health-overview.jpg)

## <a name="health-checks"></a>Sistem durumu denetimleri

Kaynak durumu, SQL kaynak durumu, başarı ve başarısızlık kaynağa oturumlarının inceleyerek belirler. Şu anda, SQL veritabanı kaynağınızın kaynak durumu, yalnızca sistem hatası ve kullanıcı hatası nedeniyle oturum açma hataları inceler. Kaynak sistem durumu, 1-2 dakikada bir güncelleştirilir.

## <a name="health-states"></a>Sistem sağlığı durumları

### <a name="available"></a>Kullanılabilir

**Mevcut** durumu, SQL kaynağınızın sistem hataları nedeniyle kaynak durumu oturum açma hatalarının algılanmadığı anlamına gelir.

![Kullanılabilir](./media/sql-database-resource-health/sql-resource-health-available.jpg)

### <a name="degraded"></a>Düzeyi düşürüldü

Durumun **Düşürüldü** olması, Kaynak Durumu'nun oturum açma işlemlerinin çoğunun başarılı olduğunu ancak arada başarısız girişler de olduğunu gösterir. Bunlar büyük olasılıkla geçici bir oturum açma hatalardır. Geçici oturum açma hatalarından kaynaklanan bağlantı sorunlarının etkisini azaltmak için lütfen kodunuzda [yeniden deneme mantığını](./sql-database-connectivity-issues.md#retry-logic-for-transient-errors) uygulayın.

![Düzeyi düşürüldü](./media/sql-database-resource-health/sql-resource-health-degraded.jpg)

### <a name="unavailable"></a>Kullanılamaz

**Kullanım dışı** durumu kaynak durumu, SQL kaynağınız için tutarlı oturum açma hatalarının algılandığı anlamına gelir. Kaynağınızı uzun bir süre bu durumda kalırsa, lütfen desteğe başvurun.

![Kullanılamaz](./media/sql-database-resource-health/sql-resource-health-unavailable.jpg)

### <a name="unknown"></a>Bilinmiyor

**Bilinmeyen** sistem durumu, kaynak durumu 10 dakikadan uzun süre boyunca bu kaynakla ilgili bilgi almadığını gösterir. Bu durum kaynak durumunun eksiksiz bir gösterimi olmasa da, bir sorun giderme işlemi önemli veri noktası var. Kaynak beklenen şekilde çalışıyorsa, kaynak durumu için kullanılabilen birkaç dakika sonra değiştirin. Kaynağı ile ilgili sorunlar yaşıyorsanız, bilinmeyen durumunu platform içindeki bir olay kaynağı etkilediğini önerebilir.

![Bilinmiyor](./media/sql-database-resource-health/sql-resource-health-unknown.jpg)

## <a name="historical-information"></a>Geçmiş bilgileri

Kaynak durumu sistem durumu geçmişi bölümünde sistem durumu geçmiş 14 güne kadar erişebilirsiniz. Bölüm için kaynak durumu tarafından bildirilen kapalı kalma süreleri (varsa) kapalı kalma süresi nedeni de içerecektir. Şu anda Azure, SQL veritabanı kaynağınız için iki dakikalık ayrıntı düzeyiyle kapalı kalma süresini gösteriyor. Gerçek kapalı kalma süresi büyük olasılıkla bir dakikadan kısadır (ortalama 8 sn).

### <a name="downtime-reasons"></a>Kapalı kalma nedenleri

SQL veritabanınız kesinti yaşandığında analiz bir nedenini belirlemek için gerçekleştirilir. Kullanılabilir olduğunda, kapalı kalma süresi nedeni kaynak durumu sistem durumu geçmişi bölümünde raporlanır. Kapalı kalma süresinin nedenleri genellikle bir olaydan 30 dakika sonra yayımlanır.

#### <a name="planned-maintenance"></a>Planlı bakım

Azure altyapısı, düzenli olarak planlanan bakım – veri merkezindeki donanım veya yazılım bileşenlerini yükseltme yapar. Bakım veritabanı uygulanır, ancak SQL var olan bazı bağlantılar sonlandırmak ve yenilerini reddet. Planlı bakım sırasında karşılaşılan oturum açma hatalarıyla genellikle geçici ve [yeniden deneme mantığı](./sql-database-connectivity-issues.md#retry-logic-for-transient-errors) etkisini azaltmaya yardımcı olur. Oturum açma hataları yaşamaya devam ederseniz Lütfen desteğe başvurun.

#### <a name="reconfiguration"></a>Yeniden yapılandırma

Yeniden yapılandırmalar, geçici koşullar kabul edilir ve zaman zaman beklenir. Bu olaylar, Yük Dengeleme veya yazılım/donanım arızaları tetiklenebilir. Bulut veritabanına bağlanan tüm istemci üretim uygulamaları, bu durumların azaltılmasına yardımcı olacağından ve genellikle hataları son kullanıcıya saydam hale getirmek için sağlam bir bağlantı [yeniden deneme mantığı](./sql-database-connectivity-issues.md#retry-logic-for-transient-errors)uygulamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Geçici hatalar için yeniden deneme mantığı](./sql-database-connectivity-issues.md#retry-logic-for-transient-errors) hakkında daha fazla bilgi edinin
- [SQL bağlantı hatalarını giderme, tanılama ve önleme](./sql-database-connectivity-issues.md)
- [Kaynak durumu uyarılarını yapılandırma](../service-health/resource-health-alert-arm-template-guide.md) hakkında daha fazla bilgi edinin
- [Kaynak durumu](../service-health/resource-health-overview.md) bir genel bakış alın
- [Kaynak Durumu SSS](../service-health/resource-health-faq.md)
