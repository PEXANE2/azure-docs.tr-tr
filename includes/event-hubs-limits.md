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
ms.openlocfilehash: 2aca4f2c236112b80e9fc985cf80ccad6d82bde3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75901813"
---
Aşağıdaki [tablolar, Azure Etkinlik Hub'larına](https://azure.microsoft.com/services/event-hubs/)özgü kotalar ve sınırlar sağlar. Olay Hub'ları fiyatlandırması hakkında daha fazla bilgi için [Olay Hub'ları fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/)konusuna bakın.

Aşağıdaki sınırlar temel, standart ve özel katmanlar arasında yaygındır. 

| Sınır | Kapsam | Notlar | Değer |
| --- | --- | --- | --- |
| Abonelik başına Olay Hub'ı ad alanları nın sayısı |Abonelik |- |100 |
| Ad alanı başına olay hub'ı sayısı |Ad Alanı |Sonraki yeni bir olay hub'ı oluşturma istekleri reddedilir. |10 |
| Olay hub'ı başına bölüm sayısı |Varlık |- |32 |
| Olay hub adının maksimum boyutu |Varlık |- |50 karakter |
| Tüketici grubuna göre çağ dışı alıcı sayısı |Varlık |- |5 |
| Maksimum verim birimleri |Ad Alanı |İş birimi sınırını aşmak verilerinizin daraltılmasına neden olur ve [sunucu meşgul özel bir durum](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)oluşturur. Standart katman için daha fazla sayıda üretim birimi istemek için bir [destek isteği](/azure/azure-portal/supportability/how-to-create-azure-support-request)dosyala. [Ek üretim birimleri,](../articles/event-hubs/event-hubs-auto-inflate.md) taahhüt edilen bir satın alma bazında 20 blok halinde kullanılabilir. |20 |
| Ad alanı başına yetkilendirme kuralları nın sayısı |Ad Alanı|Sonraki yetkilendirme kuralı oluşturma istekleri reddedilir.|12 |
| GetRuntimeInformation yöntemine yapılan arama ların sayısı | Varlık | - | Saniyede 50 | 
| Sanal ağ (VNet) ve IP Config kuralları sayısı | Varlık | - | 128 | 

### <a name="event-hubs-basic-and-standard---quotas-and-limits"></a>Etkinlik Hub'ları Temel ve Standart - kotalar ve limitler
| Sınır | Kapsam | Notlar | Temel | Standart |
| --- | --- | --- | -- | --- |
| Olay Hub'ları etkinliğinin maksimum boyutu|Varlık | &nbsp; | 256 KB | 1 MB |
| Etkinlik merkezi başına tüketici gruplarının sayısı |Varlık | &nbsp; |1 |20 |
| Ad alanı başına AMQP bağlantı sayısı |Ad Alanı |Sonraki ek bağlantılar için istekler reddedilir ve arama kodu tarafından bir özel durum alınır. |100 |5.000|
| Olay verilerinin maksimum saklama süresi |Varlık | &nbsp; |1 gün |1-7 gün |
|Apache Kafka etkin ad alanı|Ad Alanı |Olay Hubs namespace Kafka protokolünü kullanarak uygulamaları akışları |Hayır | Evet |
|Capture |Varlık | Etkinleştirildiğinde, aynı akışta mikro toplu iş |Hayır |Evet |


### <a name="event-hubs-dedicated---quotas-and-limits"></a>Etkinlik Hub'ları Adanmış - kotalar ve limitler
Etkinlik Hub'ları Özel teklifi, en az 4 saatlik kullanımla sabit bir aylık fiyata faturalandırılır. Özel katman, Standart planın tüm özelliklerini sunar, ancak kurumsal ölçek kapasitesi ve zorlu iş yüküne sahip müşteriler için sınırlar sunar. 

| Özellik | Sınırlar |
| --- | ---|
| Bant genişliği |  20 cus |
| Ad Alanları | CU başına 50 |
| Event Hubs |  Ad alanı başına 1000 |
| Giriş olayları | Dahil |
| İleti Boyutu | 1 MB |
| Bölümler | CU başına 2000 |
| Tüketici grupları | CU başına sınır yok, etkinlik hub'ı başına 1000 |
| Aracılı bağlantılar | 100 K dahil |
| İleti Bekletme | 90 gün, CU başına 10 TB dahil |
| Capture | Dahil |
