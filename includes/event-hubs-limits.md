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
ms.openlocfilehash: 31031462d9904e3554c19f47bc82f87746443693
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91371773"
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
| Sanal ağ (VNet) ve IP yapılandırma kuralları sayısı | - | 128 | 


### <a name="basic-and-standard-tiers"></a>Temel ve Standart katmanlar
Aşağıdaki tabloda, temel ve Standart katmanlar için farklı olabilecek sınırlar gösterilmektedir. 

| Sınır | Notlar | Temel | Standart |
| --- |  --- | -- | --- |
| Event Hubs olayının en büyük boyutu| &nbsp; | 256 KB | 1 MB |
| Olay Hub 'ı başına Tüketici grubu sayısı | &nbsp; |1 |20 |
| Ad alanı başına AMQP bağlantısı sayısı | Daha sonraki ek bağlantı istekleri reddedilir ve çağıran kod tarafından bir özel durum alınır. |100 |5.000|
| Olay verilerinin maksimum bekletme süresi | &nbsp; |1 gün |1-7 gün |
| En fazla üretilen iş birimi |Üretilen iş birimi sınırının aşılması verilerinizin kısıtlanmasına neden olur ve [sunucu meşgul özel durumu](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)oluşturur. Standart katman için daha fazla sayıda üretilen iş birimi istemek üzere bir [destek isteği](/azure/azure-portal/supportability/how-to-create-azure-support-request)dosyası. [Ek üretilen iş birimleri](../articles/event-hubs/event-hubs-auto-inflate.md) , taahhüt edilen satın alma esasına göre 20 blok halinde kullanılabilir. |20 | 20 | 

### <a name="dedicated-tier"></a>Adanmış katman
Event Hubs Ayrılmış teklif, en az 4 saatlik kullanım ile sabit bir aylık fiyatla faturalandırılır. Adanmış katman, standart planın tüm özelliklerini sunar, ancak yoğun iş yükleri olan müşterilere yönelik kurumsal ölçekli kapasite ve sınırlara sahiptir. 

| Öne çıkan özelliği | Sınırlar |
| --- | ---|
| Bant genişliği |  20 cu düzeyinde kapsanır |
| Ad alanları | CU başına 50 |
| Event Hubs |  ad alanı başına 1000 |
| İleti boyutu | 1 MB |
| Bölümler | CU başına 2000 |
| Tüketici grupları | Her saniyedeki limit yok, Olay Hub 'ı başına 1000 |
| Aracılı bağlantılar | 100 K dahil |
| İleti saklama süresi | 90 gün, CU başına 10 TB dahildir |
| Giriş olayları | Dahil |
| Yakalama | Dahil |


### <a name="schema-registry-limitations"></a>Şema kayıt defteri sınırlamaları

#### <a name="limits-that-are-the-same-for-standard-and-dedicated-tiers"></a>**Standart** ve **ayrılmış** katmanlar için aynı olan sınırlar 
| Öne çıkan özelliği | Sınır | 
| --- |  --- | -- |
| Şema grubu adının maksimum uzunluğu | 50 |  
| En fazla şema adı uzunluğu | 100 |    
| Şema başına bayt cinsinden boyut | 1 MB |   
| Şema grubu başına özellik sayısı | 1024 |
| Grup Özellik anahtarı başına bayt cinsinden boyut | 256 | 
| Grup Özellik anahtarı başına bayt cinsinden boyut | 1024 | 


#### <a name="limits-that-are-different-for-standard-and-dedicated-tiers"></a>**Standart** ve **adanmış** katmanlar için farklı sınırlar 

| Sınır | Standart | Ayrılmış | 
| --- |  --- | -- | --- |
| Şema kayıt defterinin (ad alanı) mega bayt cinsinden boyutu | 25 |  1024 |
| Şema kayıt defterindeki şema gruplarının sayısı (ad alanı)| 1 (varsayılan bir tane hariç) | 1000 |
| Tüm şema gruplarındaki şema sürümlerinin sayısı | 25 | 10000 |





