---
title: include dosyası
description: include dosyası
services: event-hubs
author: sethmanheim
ms.service: event-hubs
ms.topic: include
ms.date: 05/22/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: a20481ea42a0772ab42322e912cfce67877734d6
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71830190"
---
Aşağıdaki tablolarda [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)'a özgü kotalar ve sınırlamalar sağlanmaktadır. Event Hubs fiyatlandırması hakkında daha fazla bilgi için bkz. [Event Hubs fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/).

Temel, standart ve adanmış katmanlarda aşağıdaki sınırlar yaygındır. 

| Sınır | Kapsam | Notlar | Değer |
| --- | --- | --- | --- |
| Abonelik başına Event Hubs ad alanı sayısı |Abonelik |- |100 |
| Ad alanı başına Olay Hub 'ları sayısı |Uzayına |Yeni bir olay hub 'ı oluşturmaya yönelik sonraki istekler reddedilir. |10 |
| Olay Hub 'ı başına bölüm sayısı |Kurum |- |32 |
| Bir olay hub 'ı adının en büyük boyutu |Kurum |- |50 karakter |
| Tüketici grubu başına dönem olmayan alıcıların sayısı |Kurum |- |5 |
| En fazla üretilen iş birimi |Uzayına |Üretilen iş birimi sınırının aşılması verilerinizin kısıtlanmasına neden olur ve [sunucu meşgul özel durumu](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)oluşturur. Standart katman için daha fazla sayıda üretilen iş birimi istemek üzere bir [destek isteği](/azure/azure-supportability/how-to-create-azure-support-request)dosyası. [Ek üretilen iş birimleri](../articles/event-hubs/event-hubs-auto-inflate.md) , taahhüt edilen satın alma esasına göre 20 blok halinde kullanılabilir. |20 |
| Ad alanı başına yetkilendirme kuralı sayısı |Uzayına|Yetkilendirme kuralı oluşturma için sonraki istekler reddedilir.|12 |
| GetRuntimeInformation metoduna yapılan çağrıların sayısı | Kurum | - | saniyede 50 | 
| Sanal ağ (VNet) ve IP yapılandırma kuralları sayısı | Kurum | - | 128 | 

### <a name="event-hubs-basic-and-standard---quotas-and-limits"></a>Temel ve standart kotaları ve limitleri Event Hubs
| Sınır | Kapsam | Notlar | Temel | Standart |
| --- | --- | --- | -- | --- |
| Event Hubs olayının en büyük boyutu|Kurum | &nbsp; | 256 KB | 1 MB |
| Olay Hub 'ı başına Tüketici grubu sayısı |Kurum | &nbsp; |1 |20 |
| Ad alanı başına AMQP bağlantısı sayısı |Uzayına |Daha sonraki ek bağlantı istekleri reddedilir ve çağıran kod tarafından bir özel durum alınır. |100 |5\.000|
| Olay verilerinin maksimum bekletme süresi |Kurum | &nbsp; |1 gün |1-7 gün |
|Apache Kafka etkin ad alanı|Uzayına |Event Hubs ad alanı, uygulamaları Kafka protokolünü kullanarak akışlar |Hayır | Yes |
|Yakalama |Kurum | Etkinleştirildiğinde, aynı akışta mikro toplu işler |Hayır |Yes |


### <a name="event-hubs-dedicated---quotas-and-limits"></a>Event Hubs Ayrılmış-kotalar ve sınırlar
Event Hubs Ayrılmış teklif, en az 4 saatlik kullanım ile sabit bir aylık fiyatla faturalandırılır. Adanmış katman, standart planın tüm özelliklerini sunar, ancak yoğun iş yükleri olan müşterilere yönelik kurumsal ölçekli kapasite ve sınırlara sahiptir. 

| Özellik | Sınırlar |
| --- | ---|
| Bant Genişliği |  20 cu düzeyinde kapsanır |
| Ad Alanları | CU başına 50 |
| Event Hubs |  ad alanı başına 1000 |
| Giriş olayları | Var |
| İleti Boyutu | 1 MB |
| Bölümler | CU başına 2000 |
| Tüketici grupları | Her saniyedeki limit yok, Olay Hub 'ı başına 1000 |
| Aracılı bağlantılar | 100 K dahil |
| İleti Saklama | 7 güne kadar, CU başına 10 TB dahildir |
| Yakalama | Var |
