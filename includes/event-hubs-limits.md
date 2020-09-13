---
title: dosya dahil etme
description: dosya dahil etme
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 09/10/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 8700bbfe697a6b5fb81380831950d704fcb1f5ff
ms.sourcegitcommit: 5d7f8c57eaae91f7d9cf1f4da059006521ed4f9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "90012925"
---
Aşağıdaki tablolarda [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)'a özgü kotalar ve sınırlamalar sağlanmaktadır. Event Hubs fiyatlandırması hakkında daha fazla bilgi için bkz. [Event Hubs fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/).

Aşağıdaki sınırlar temel ve Standart katmanlarda ortaktır. 

| Sınır | Kapsam | Notlar | Değer |
| --- | --- | --- | --- |
| Abonelik başına Event Hubs ad alanı sayısı |Abonelik |- |100 |
| Ad alanı başına Olay Hub 'ları sayısı |Ad Alanı |Yeni bir olay hub 'ı oluşturmaya yönelik sonraki istekler reddedilir. |10 |
| Olay Hub 'ı başına bölüm sayısı |Varlık |- |32 |
| Bir olay hub 'ı adının en büyük boyutu |Varlık |- | 256 karakter |
| Bir tüketici grubu adının en büyük boyutu |Varlık |- | 256 karakter |
| Tüketici grubu başına dönem olmayan alıcıların sayısı |Varlık |- |5 |
| En fazla üretilen iş birimi |Ad Alanı |Üretilen iş birimi sınırının aşılması verilerinizin kısıtlanmasına neden olur ve [sunucu meşgul özel durumu](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)oluşturur. Standart katman için daha fazla sayıda üretilen iş birimi istemek üzere bir [destek isteği](/azure/azure-portal/supportability/how-to-create-azure-support-request)dosyası. [Ek üretilen iş birimleri](../articles/event-hubs/event-hubs-auto-inflate.md) , taahhüt edilen satın alma esasına göre 20 blok halinde kullanılabilir. |20 |
| Ad alanı başına yetkilendirme kuralı sayısı |Ad Alanı|Yetkilendirme kuralı oluşturma için sonraki istekler reddedilir.|12 |
| GetRuntimeInformation metoduna yapılan çağrıların sayısı | Varlık | - | saniyede 50 | 
| Sanal ağ (VNet) ve IP yapılandırma kuralları sayısı | Varlık | - | 128 | 

### <a name="event-hubs-basic-and-standard---quotas-and-limits"></a>Temel ve standart kotaları ve limitleri Event Hubs
| Sınır | Kapsam | Notlar | Temel | Standart |
| --- | --- | --- | -- | --- |
| Event Hubs olayının en büyük boyutu|Varlık | &nbsp; | 256 KB | 1 MB |
| Olay Hub 'ı başına Tüketici grubu sayısı |Varlık | &nbsp; |1 |20 |
| Ad alanı başına AMQP bağlantısı sayısı |Ad Alanı |Daha sonraki ek bağlantı istekleri reddedilir ve çağıran kod tarafından bir özel durum alınır. |100 |5.000|
| Olay verilerinin maksimum bekletme süresi |Varlık | &nbsp; |1 gün |1-7 gün |
|Apache Kafka etkin ad alanı|Ad Alanı |Event Hubs ad alanı, uygulamaları Kafka protokolünü kullanarak akışlar. Daha fazla bilgi için bkz. [Apache Kafka uygulamalardan Azure Event Hubs kullanma](../articles/event-hubs/event-hubs-for-kafka-ecosystem-overview.md). |Hayır | Yes |
|Yakalama |Varlık | Etkinleştirildiğinde, aynı akışta mikro toplu işler. Daha fazla bilgi için bkz. Azure [BLOB depolama veya Azure Data Lake Storage azure Event Hubs aracılığıyla olayları yakalama](../articles/event-hubs/event-hubs-capture-overview.md). |Hayır |Yes |


### <a name="event-hubs-dedicated---quotas-and-limits"></a>Event Hubs Ayrılmış-kotalar ve sınırlar
Event Hubs Ayrılmış teklif, en az 4 saatlik kullanım ile sabit bir aylık fiyatla faturalandırılır. Adanmış katman, standart planın tüm özelliklerini sunar, ancak yoğun iş yükleri olan müşterilere yönelik kurumsal ölçekli kapasite ve sınırlara sahiptir. 

| Öne çıkan özelliği | Sınırlar |
| --- | ---|
| Bant genişliği |  20 cu düzeyinde kapsanır |
| Ad alanları | CU başına 50 |
| Event Hubs |  ad alanı başına 1000 |
| Giriş olayları | Dahil |
| İleti boyutu | 1 MB |
| Bölümler | CU başına 2000 |
| Tüketici grupları | Her saniyedeki limit yok, Olay Hub 'ı başına 1000 |
| Aracılı bağlantılar | 100 K dahil |
| İleti bekletme | 90 gün, CU başına 10 TB dahildir |
| Yakalama | Dahil |
