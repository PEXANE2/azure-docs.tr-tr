---
title: Veritabanı durumunu izlemek için Azure Kaynak Durumu'nun kullanımını
description: SQL Veritabanı durumunu izlemek için Azure Kaynak Durumu'nun kullanılması, bir Azure sorunu SQL kaynaklarınızı etkilediğinde tanılamanıza ve destek almanıza yardımcı olur.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79208864"
---
# <a name="use-resource-health-to-troubleshoot-connectivity-for-azure-sql-database"></a>Azure SQL Veritabanı bağlantı sorunlarını gidermek için Kaynak Durumu'nu kullanma

## <a name="overview"></a>Genel Bakış

SQL Veritabanı için [Kaynak Durumu,](../service-health/resource-health-overview.md#get-started) bir Azure sorunu SQL kaynaklarınızı etkilediğinde tanılamanıza ve destek almanıza yardımcı olur. Kaynaklarınızın güncel ve geçmiş durumu hakkında bilgiler sağlar ve sorunları azaltmaya yardımcı olur. Kaynak Durumu, Azure hizmet sorunları ile ilgili yardıma ihtiyacınız olduğunda teknik destek sağlar.

![Genel Bakış](./media/sql-database-resource-health/sql-resource-health-overview.jpg)

## <a name="health-checks"></a>Sistem durumu denetimleri

Kaynak Durumu, kaynağa girişlerin başarısını ve başarısızlığını inceleyerek SQL kaynağınızın durumunu belirler. Şu anda, SQL DB kaynağınız için Kaynak Durumu yalnızca kullanıcı hatası değil, sistem hatası nedeniyle oturum açma hatalarını inceler. Kaynak Durumu durumu her 1-2 dakikada bir güncellenir.

## <a name="health-states"></a>Sağlık Durumları

### <a name="available"></a>Kullanılabilir

**Kullanılabilir** durumu, Kaynak Durumu'nun SQL kaynağınızdaki sistem hataları nedeniyle oturum açma hatalarını algılamadığı anlamına gelir.

![Kullanılabilir](./media/sql-database-resource-health/sql-resource-health-available.jpg)

### <a name="degraded"></a>Düzeyi düşürüldü

Durumun **Düşürüldü** olması, Kaynak Durumu'nun oturum açma işlemlerinin çoğunun başarılı olduğunu ancak arada başarısız girişler de olduğunu gösterir. Bunlar büyük olasılıkla geçici oturum açma hatalarıdır. Geçici oturum açma hatalarından kaynaklanan bağlantı sorunlarının etkisini azaltmak için lütfen kodunuzda [yeniden deneme mantığını](./sql-database-connectivity-issues.md#retry-logic-for-transient-errors) uygulayın.

![Düzeyi düşürüldü](./media/sql-database-resource-health/sql-resource-health-degraded.jpg)

### <a name="unavailable"></a>Kullanılamaz

**Kullanılamayan'ın** durumu, Kaynak Durumu'nun SQL kaynağınızda tutarlı oturum açma hataları algıladığını anlamına gelir. Kaynağınız bu durumda uzun bir süre kalırsa, lütfen desteğe başvurun.

![Kullanılamaz](./media/sql-database-resource-health/sql-resource-health-unavailable.jpg)

### <a name="unknown"></a>Bilinmiyor

**Bilinmeyenin** sistem durumu, Kaynak Durumu'nun bu kaynak hakkında 10 dakikadan uzun süredir bilgi almadığını gösterir. Bu durum kaynağın durumunun kesin bir göstergesi olmasa da, sorun giderme işleminde önemli bir veri noktasıdır. Kaynak beklendiği gibi çalışıyorsa, kaynağın durumu birkaç dakika sonra Kullanılabilir olarak değişir. Kaynakla ilgili sorunlar yaşıyorsanız, Bilinmeyen sistem durumu, platformdaki bir olayın kaynağı etkilediğini düşündürebilir.

![Bilinmiyor](./media/sql-database-resource-health/sql-resource-health-unknown.jpg)

## <a name="historical-information"></a>Tarihsel bilgiler

Kaynak Sağlığı bölümünde 14 güne kadar sağlık geçmişine erişebilirsiniz. Bu bölümde, Kaynak Durumu tarafından bildirilen kesinti süreleri için kesinti nedeni (varsa) da yer alacaktır. Şu anda Azure, SQL veritabanı kaynağınız için iki dakikalık ayrıntı düzeyiyle kapalı kalma süresini gösteriyor. Gerçek kapalı kalma süresi büyük olasılıkla bir dakikadan kısadır (ortalama 8 sn).

### <a name="downtime-reasons"></a>Kapalı kalma nedenleri

SQL Veritabanınız kapalı kalma süresi yaşadığında, bir nedeni belirlemek için çözümleme gerçekleştirilir. Kullanılabilir olduğunda, kesinti nedeni Kaynak Durumu'nun Sistem Durumu bölümünde bildirilir. Kapalı kalma süresinin nedenleri genellikle bir olaydan 30 dakika sonra yayımlanır.

#### <a name="planned-maintenance"></a>Planlı bakım

Azure altyapısı, veri merkezinde donanım veya yazılım bileşenlerinin yükseltimi gibi planlı bakım lar düzenli olarak gerçekleştirir. Veritabanı bakımdan geçerken, SQL varolan bazı bağlantıları sonlandırabilir ve yenilerini reddedebilir. Planlanan bakım sırasında yaşanan oturum açma hataları genellikle geçicidir ve [yeniden deneme mantığı](./sql-database-connectivity-issues.md#retry-logic-for-transient-errors) etkiyi azaltmaya yardımcı olur. Oturum açma hataları yaşamaya devam ederseniz, lütfen desteğe başvurun.

#### <a name="reconfiguration"></a>Yeniden yapılandırma

Yeniden yapılandırmalar geçici koşullar olarak kabul edilir ve zaman zaman beklenmektedir. Bu olaylar yük dengeleme veya yazılım/donanım hataları tarafından tetiklenebilir. Bulut veritabanına bağlanan herhangi bir istemci üretim uygulaması, bu durumların hafifletmesine yardımcı olacağı ve hataları genellikle son kullanıcıya saydam hale getireceği için sağlam bir bağlantı [yeniden deneme mantığı](./sql-database-connectivity-issues.md#retry-logic-for-transient-errors)uygulamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Geçici hatalar için yeniden deneme mantığı](./sql-database-connectivity-issues.md#retry-logic-for-transient-errors) hakkında daha fazla bilgi edinin
- [SQL bağlantı hatalarını giderme, tanılama ve önleme](./sql-database-connectivity-issues.md)
- [Kaynak Durumu uyarılarını yapılandırma](../service-health/resource-health-alert-arm-template-guide.md) hakkında daha fazla bilgi edinin
- [Kaynak Durumu](../service-health/resource-health-overview.md) hakkında genel bilgi alın
- [Kaynak Durumu hakkında SSS](../service-health/resource-health-faq.md)
