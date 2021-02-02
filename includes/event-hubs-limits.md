---
title: include dosyası
description: include dosyası
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 02/01/2021
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 45cc5120072f2a8c7742f79cfb79161042345505
ms.sourcegitcommit: d49bd223e44ade094264b4c58f7192a57729bada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99427364"
---
Aşağıdaki tablolarda [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)'a özgü kotalar ve sınırlamalar sağlanmaktadır. Event Hubs fiyatlandırması hakkında daha fazla bilgi için bkz. [Event Hubs fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="common-limits-for-all-tiers"></a>Tüm katmanlar için ortak sınırlar
Aşağıdaki sınırlar tüm katmanlarda ortaktır. 

| Sınır |  Notlar | Değer |
| --- |  --- | --- |
| Abonelik başına Event Hubs ad alanı sayısı |- |100 |
| Ad alanı başına Olay Hub 'ları sayısı | Yeni bir olay hub 'ı oluşturmaya yönelik sonraki istekler reddedilir. |10 |
| Olay Hub 'ı başına bölüm sayısı |- |32 |
| Bir olay hub 'ı adının boyutu |- | 256 karakter |
| Tüketici grubu adının boyutu |- | 256 karakter |
| Tüketici grubu başına dönem olmayan alıcıların sayısı |- |5 |
| Ad alanı başına yetkilendirme kuralı sayısı | Yetkilendirme kuralı oluşturma için sonraki istekler reddedilir.|12 |
| GetRuntimeInformation metoduna yapılan çağrıların sayısı |  - | saniyede 50 | 
| Sanal ağ sayısı (VNet) | - | 128 | 
| IP yapılandırma kuralları sayısı | - | 128 | 


### <a name="basic-vs-standard-tiers"></a>Temel ve Standart katmanlar
Aşağıdaki tabloda, temel ve Standart katmanlar için farklı olabilecek sınırlar gösterilmektedir. 

| Sınır | Notlar | Temel | Standart |
|---|---|--|---|
| Event Hubs olayının en büyük boyutu| &nbsp; | 256 KB | 1 MB |
| Olay Hub 'ı başına Tüketici grubu sayısı | &nbsp; |1 |20 |
| Ad alanı başına AMQP bağlantısı sayısı | Daha sonraki ek bağlantı istekleri reddedilir ve çağıran kod tarafından bir özel durum alınır. |100 |5.000|
| Olay verilerinin maksimum bekletme süresi | &nbsp; |1 gün |1-7 gün |
| En fazla üretilen iş birimi |Bu sınırın aşılması verilerinizin kısıtlanmasına neden olur ve [sunucu meşgul özel durumu](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)oluşturur. Standart katman için daha fazla sayıda üretilen iş birimi istemek üzere bir [destek isteği](../articles/azure-portal/supportability/how-to-create-azure-support-request.md)dosyası. [Ek üretilen iş birimleri](../articles/event-hubs/event-hubs-auto-inflate.md) , taahhüt edilen satın alma esasına göre 20 blok halinde kullanılabilir. |20 | 20 | 

### <a name="dedicated-tier-vs-standard-tier"></a>Adanmış katman ve Standart katman karşılaştırması
Event Hubs Ayrılmış teklif, en az 4 saatlik kullanım ile sabit bir aylık fiyatla faturalandırılır. Adanmış katman, standart planın tüm özelliklerini sunar, ancak yoğun iş yükleri olan müşterilere yönelik kurumsal ölçekli kapasite ve sınırlara sahiptir. 

Azure portal kullanarak adanmış Event Hubs kümesi oluşturma hakkında bu [belgeye](../articles/event-hubs/event-hubs-dedicated-cluster-create-portal.md) başvurun.

| Öne çıkan özelliği | Standart | Ayrılmış |
| --- |:---|:---|
| Bant genişliği | 20 s (40 'e kadar) | 20 cu düzeyinde kapsanır |
| Ad alanları |  1 | CU başına 50 |
| Event Hubs |  ad alanı başına 10 | ad alanı başına 1000 |
| Giriş olayları | Milyon olay başına ödeme | Dahil |
| İleti boyutu | 1.000.000 bayt | 1.000.000 bayt |
| Bölümler | Olay Hub 'ı başına 32 | Olay Hub 'ı başına 1024<br/>CU başına 2000 |
| Tüketici grupları | Olay Hub 'ı başına 20 | Her saniyedeki limit yok, Olay Hub 'ı başına 1000 |
| Aracılı bağlantılar | 1.000 dahil, en fazla 5.000 | 100 K dahil ve maks |
| İleti bekletme | 7 gün, TU başına 84 GB dahil | 90 gün, CU başına 10 TB dahildir |
| Yakalama | Saat başına ödeme | Dahil |


### <a name="schema-registry-limitations"></a>Şema kayıt defteri sınırlamaları

#### <a name="limits-that-are-the-same-for-standard-and-dedicated-tiers"></a>Standart ve ayrılmış katmanlar için aynı olan sınırlar 
| Özellik | Sınır | 
|---|---|
| Şema grubu adının maksimum uzunluğu | 50 |  
| En fazla şema adı uzunluğu | 100 |    
| Şema başına bayt cinsinden boyut | 1 MB |   
| Şema grubu başına özellik sayısı | 1024 |
| Grup Özellik anahtarı başına bayt cinsinden boyut | 256 | 
| Grup Özellik değeri başına bayt cinsinden boyut | 1024 | 


#### <a name="limits-that-are-different-for-standard-and-dedicated-tiers"></a>Standart ve adanmış katmanlar için farklı sınırlar 

| Sınır | Standart | Ayrılmış | 
|---|---|--|
| Şema kayıt defterinin (ad alanı) mega bayt cinsinden boyutu | 25 |  1024 |
| Bir şema kayıt defterindeki veya ad alanındaki şema gruplarının sayısı | 1-varsayılan grup hariç | 1000 |
| Tüm şema gruplarındaki şema sürümlerinin sayısı | 25 | 10000 |