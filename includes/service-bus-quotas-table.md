---
title: include dosyası
description: include dosyası
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 12/13/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: b19dc7a85fafa1a4d875c84db9bbefabb3cd5a7d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77651618"
---
Aşağıdaki tabloda Azure Servis Veri Servisi iletisine özgü kota bilgileri listelanmaktadır. Servis Otobüsü fiyatlandırması ve diğer kotalar hakkında bilgi için [Servis Otobüsü fiyatlandırması](https://azure.microsoft.com/pricing/details/service-bus/)bölümüne bakın.

| Kota adı | Kapsam | Notlar | Değer |
| --- | --- | --- | --- |
| Azure aboneliği başına maksimum Temel veya Standart ad alanı sayısı |Ad Alanı |Sonraki ek Temel veya Standart ad alanları için istekler Azure portalı tarafından reddedilir. |100|
| Azure aboneliği başına maksimum Premium ad alanı sayısı |Ad Alanı |Sonraki ek Premium ad alanları için sonraki istekler portal tarafından reddedilir. |100 |
| Sıra veya konu boyutu |Varlık |Sıra nın veya konunun oluşturulması üzerine tanımlanır. <br/><br/> Sonraki gelen iletiler reddedilir ve arama kodu tarafından bir özel durum alınır. |1, 2, 3, 4 GB veya 5 GB.<br /><br />Premium SKU'da ve [bölümleme](/azure/service-bus-messaging/service-bus-partitioning) etkinleştirilmiş Standart SKU'da maksimum sıra veya konu boyutu 80 GB'dır. |
| Ad alanında eşzamanlı bağlantı sayısı |Ad Alanı |Sonraki ek bağlantılar için istekler reddedilir ve arama kodu tarafından bir özel durum alınır. REST işlemleri eşzamanlı TCP bağlantılarına dahil değildir. |NetMessaging: 1.000.<br /><br />AMQP: 5,000. |
| Bir kuyruk, konu veya abonelik varlığındaki eşzamanlı alma isteklerinin sayısı |Varlık |Sonraki alma istekleri reddedilir ve arama kodu tarafından bir özel durum alınır. Bu kota, bir konuyla ilgili tüm abonelikler arasında eşzamanlı alma işlemlerinin birleştirilmiş sayısı için geçerlidir. |5.000 |
| Ad alanı başına konu veya sıra sayısı |Ad Alanı |Ad alanında yeni bir konu veya kuyruk oluşturulması için sonraki istekleri reddedilir. Sonuç olarak, [Azure portalı][Azure portal]üzerinden yapılandırılırsa, bir hata iletisi oluşturulur. Yönetim API'sinden çağrılırsa, arama kodu tarafından bir özel durum alınır. |Temel veya Standart katman için 10.000. Ad alanındaki toplam konu ve sıra sayısı 10.000'den az veya eşit olmalıdır. <br/><br/>Premium katman için, mesajlaşma birimi (MU) başına 1.000. Maksimum limit 4,000'dir. |
| Ad alanı başına [bölümlenmiş konu veya sıra](/azure/service-bus-messaging/service-bus-partitioning) sayısı |Ad Alanı |Ad alanında yeni bir bölümlenmiş konu veya kuyruk oluşturulması için sonraki istekleri reddedilir. Sonuç olarak, [Azure portalı][Azure portal]üzerinden yapılandırılırsa, bir hata iletisi oluşturulur. Yönetim API'sinden çağrılması durumunda, özel durum **QuotaExceededException** arama kodu tarafından alınır. |Temel ve Standart katmanlar: 100.<br/><br/>Bölümlenmiş varlıklar [Premium](../articles/service-bus-messaging/service-bus-premium-messaging.md) katmanında desteklenmez.<br/><br />Bölümlenmiş her sıra veya konu, ad alanı başına 1.000 varlık kotası doğru sayar. |
| Herhangi bir ileti varlık yolunun maksimum boyutu: sıra veya konu |Varlık |- |260 karakter. |
| Herhangi bir ileti varlık adının maksimum boyutu: ad alanı, abonelik veya abonelik kuralı |Varlık |- |50 karakter. |
| [İleti kimliğinin](/dotnet/api/microsoft.azure.servicebus.message.messageid) maksimum boyutu | Varlık |- | 128 |
| İleti [oturum kimliğinin](/dotnet/api/microsoft.azure.servicebus.message.sessionid) maksimum boyutu | Varlık |- | 128 |
| Bir kuyruk, konu veya abonelik varlığı için ileti boyutu |Varlık |Bu kotaları aşan gelen iletiler reddedilir ve arama kodu tarafından bir özel durum alınır. |Maksimum mesaj boyutu: [Standart katman](../articles/service-bus-messaging/service-bus-premium-messaging.md)için 256 KB, Premium [katman](../articles/service-bus-messaging/service-bus-premium-messaging.md)için 1 MB. <br /><br />Sistem yükü nedeniyle, bu sınır bu değerlerden daha azdır.<br /><br />Maksimum başlık boyutu: 64 KB.<br /><br />Özellik çantasında üstbilgi özelliklerinin maksimum sayısı: **bayt/int. MaxValue**.<br /><br />Özellik çantasında maksimum özellik boyutu: Açık bir sınır yoktur. Maksimum üstbilgi boyutuyla sınırlıdır. |
| Bir kuyruk, konu veya abonelik varlığı için ileti özelliği boyutu |Varlık | Özel durum **SerializationException** oluşturulur. |Her özellik için maksimum ileti özelliği boyutu 32.000'dir. Tüm özelliklerin kümülatif boyutu 64.000'i geçemez. Bu sınır, [SequenceNumber,](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber) [Label](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label)ve [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid)gibi hem kullanıcı özellikleri hem de sistem özellikleri olan [BrokeredMessage'ın](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)tüm üstbilgisi için geçerlidir. |
| Konu başına abonelik sayısı |Varlık |Konu için ek abonelikler oluşturmak için sonraki istekleri reddedilir. Sonuç olarak, portal üzerinden yapılandırılırsa, bir hata iletisi gösterilir. Yönetim API'sinden çağrılırsa, arama kodu tarafından bir özel durum alınır. |Standart katman için konu başına 2.000. |
| Konu başına SQL filtresi sayısı |Varlık |Konuyla ilgili ek filtreler oluşturulması için sonraki istekler reddedilir ve arama kodu tarafından bir özel durum alınır. |2.000 |
| Konu başına korelasyon filtresi sayısı |Varlık |Konuyla ilgili ek filtreler oluşturulması için sonraki istekler reddedilir ve arama kodu tarafından bir özel durum alınır. |100.000 |
| SQL filtrelerinin veya eylemlerinin boyutu |Ad Alanı |Sonraki ek filtreler oluşturma istekleri reddedilir ve arama kodu tarafından bir özel durum alınır. |Maksimum filtre koşul dizesi uzunluğu: 1.024 (1 K).<br /><br />Maksimum kural eylem dizesi uzunluğu: 1.024 (1 K).<br /><br />Kural eylemi başına maksimum ifade sayısı: 32. |
| Ad alanı, sıra veya konu başına [Paylaşılan Erişim Yetkilendirme Kuralı](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) kurallarının sayısı |Varlık, ad alanı |Sonraki ek kurallar oluşturulması için istekler reddedilir ve arama kodu tarafından bir özel durum alınır. |Varlık türü başına maksimum kural sayısı: 12. <br /><br /> Hizmet Veri Aracı ad alanında yapılandırılan kurallar tüm türler için geçerlidir: kuyruklar, konular. |
| Hareket başına ileti sayısı | İşlem | Ek gelen iletiler reddedilir ve arama kodu tarafından "Tek bir işlemde 100'den fazla ileti gönderilemez" belirten bir özel durum alınır. | 100 <br /><br /> Hem **Send()** hem de **SendAsync()** işlemleri için. |
| Sanal ağ ve IP filtresi kuralları sayısı | Ad Alanı | &nbsp; | 128 | 

[Azure portal]: https://portal.azure.com
