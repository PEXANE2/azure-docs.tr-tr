---
title: Arama için fiyatlandırma senaryoları (ses/video) ve sohbet
titleSuffix: An Azure Communication Services concept document
description: Iletişim Hizmetleri fiyatlandırma modeli hakkında bilgi edinin.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 09/29/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 779fa577b25bd4f2aa92aa8b8cc1244a58bdf1ae
ms.sourcegitcommit: 7dacbf3b9ae0652931762bd5c8192a1a3989e701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92126199"
---
# <a name="pricing-scenarios"></a>Fiyatlandırma senaryoları

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]


Azure Iletişim Hizmetleri için fiyatlara genellikle Kullandıkça Öde modeli temel alınır.

## <a name="voicevideo-calling-and-screen-sharing"></a>Ses/video çağırma ve ekran paylaşımı

Azure Iletişim Hizmetleri, uygulamalarınıza ses/video çağırma ve ekran paylaşımı ekleme için izin verir. JavaScript, amaç-C (Apple), Java (Android) veya .NET istemci kitaplıklarını kullanarak deneyimlerinizi uygulamalarınıza ekleyebilirsiniz. [Kullanılabilir istemci kitaplıklarının tam listesini](./sdk-options.md)inceleyin.

### <a name="pricing"></a>Fiyatlandırma

Çağrı ve ekran paylaşım hizmetleri, her katılımcı için dakikada $0,004, grup çağrıları için dakikada üzerinden ücretlendirilir. Olası çeşitli çağrı akışlarını anlamak için [Bu sayfaya](./call-flows.md)bakın.

Her çağrının katılımcısı, çağrıya bağlı oldukları her dakika için faturalandırmaya göre sayılır. Bu, kullanıcının video çağırma, sesli arama veya ekran paylaşımı olmasına bakılmaksızın doğru bir şekilde geçerlidir.

### <a name="pricing-example-group-audiovideo-call-using-js-and-ios-client-libraries"></a>Fiyatlandırma örneği: JS ve iOS istemci kitaplıklarını kullanarak ses/video çağrısını gruplayın

Çiğdem, iş arkadaşları, Bob ve Charlie ile bir grup çağrısı yaptı. Gamze ve Bob, JS istemci kitaplıklarını, Charlie iOS istemci kitaplıklarını kullandı.

- Çağrı toplam 60 dakika sürer.
- Çiğdem ve Bob tüm çağrı için katılmış. Çiğdem, videosunu beş dakika boyunca açmıştır ve ekranı 23 dakika boyunca paylaşıyordu. Bob, tüm çağrı (60 dakika) için videoyu ve 12 dakika boyunca paylaşılan ekranını içeriyordu.
- Çağrıyı 43 dakika sonra bıraktı. Charlie, katıldık süre boyunca ses ve video kullandı (43 dakika).

**Maliyet hesaplamaları**

- 2 katılımcılar x 60 dakikada katılımcı başına dakikada x $0,004 = $0,48 [hem video hem de ses aynı hızda ücretlendirilir]
- 1 katılımcı x 43 dakika başına katılımcı başına x $0,004 dakikası = $0,172 [hem video hem de ses aynı hızda ücretlendirilir]

**Grup çağrısı Için toplam maliyet**: $0,48 + $0,172 = $0,652

## <a name="chat"></a>Sohbet

Iletişim hizmetleriyle, 2 veya daha fazla kullanıcı arasında sohbet iletileri gönderme ve alma özelliğiyle uygulamanızı geliştirebilirsiniz. Sohbet istemci kitaplıkları JavaScript, .NET, Python ve Java için kullanılabilir. [İstemci kitaplıkları hakkında bilgi edinmek için bu sayfaya](./sdk-options.md) bakın

### <a name="price"></a>Fiyat

Gönderilen her sohbet iletisi için $0,0008 ücretlendirilirsiniz.

### <a name="pricing-example-chat-between-two-users"></a>Fiyatlandırma örneği: iki kullanıcı arasında sohbet 

Geeta, bir güncelleştirmeyi paylaşmak ve 5 ileti göndermesi için bir sohbet iş parçacığı başlatır. Sohbet, Geeta 'da 10 dakika sürer ve her biri bir adet 15 ileti gönderir.

**Maliyet hesaplamaları** 
- Gönderilen ileti sayısı (5 + 15 + 15) x $0,0008 = $0,028

### <a name="pricing-example-group-chat-with-multiple-users"></a>Fiyatlandırma örneği: birden çok kullanıcı ile sohbeti gruplayın 

Charlie, arkadaşlarını bir tatil planı için & Jasmine ile bir sohbet iş parçacığı başlatır. Bu kişiler, Charlie 'de, Casey & Jasmine 'nın sırasıyla 20, 30 ve 18 ileti gönderdikleri sırada bir sohbet edebilir. Bu kişilerin, arkadaşınızın yolculuğa katılması, sohbet iş parçacığına eklemesi ve tüm ileti geçmişini kendi ile paylaşması hakkında fark ederler. 

Gül iletileri görür ve sohbet başlatır. Bu arada, Casey bir çağrı alır ve konuşmayı daha sonra yakalamak üzere karar verir. Charlie, Jasmine & gül, seyahat tarihlerine karar verir ve sırasıyla bir 30, 25, 35 ileti gönderir.

**Maliyet hesaplamaları** 

- Gönderilen ileti sayısı (20 + 30 + 18 + 30 + 25 + 35) x $0,0008 = $0,1264


## <a name="telephony-and-sms"></a>Telefon ve SMS

## <a name="price"></a>Fiyat 

Telefon hizmetleri dakika başına fiyatlandırılır, ancak SMS ileti başına ücretlendirilir. Fiyatlandırma, kullandığınız sayının türü ve konumu ile çağrılarınızın ve SMS iletilerinizin hedefinin yerini belirler.

### <a name="telephone-number-leasing"></a>Telefon numarası Kiralama

Telefon numarası kiralama ücretleri önde ücretlendirilir ve ayda bir ay temelinde tekrarlardır:

|Sayı türü   |Aylık ücret   |
|--------------|-----------|
|Yerel (Birleşik Devletler)     |1/ay        |
|Ücretsiz (Birleşik Devletler) |2/ay |


### <a name="telephone-calling"></a>Telefon çağırma

Geleneksel telefon çağrısı (genel anahtarlı telefon ağı üzerinden oluşan çağrı) Birleşik Devletler göre telefon numaraları için Kullandıkça Öde fiyatlandırmasıyla birlikte kullanılabilir. Fiyat, kullanılan sayı türüne ve çağrının hedefine göre dakikalık bir ücretdir. En popüler arama hedeflerine ilişkin fiyatlandırma ayrıntıları aşağıdaki tabloya dahil edilmiştir. Hedeflerin tam listesi için lütfen [ayrıntılı fiyatlandırma listesine](https://github.com/Azure/Communication/blob/master/pricing/communication-services-pstn-rates.csv) bakın.


#### <a name="united-states-calling-prices"></a>Birleşik Devletler çağrı fiyatları

Aşağıdaki fiyatlar 30 Haziran 2021 ' ye kadar gerekli iletişim vergilerini ve ücretlerini içerir:

|Sayı türü   |Çağrı yapmak için   |Çağrı almak için|
|--------------|-----------|------------|
|Yerel     |Başlangıç fiyatı: $0.013/dk       |$0.0085/dk        |
|Ücretsiz |$0.013/dk   |$0.0220/dk |

#### <a name="other-calling-destinations"></a>Diğer çağrı hedefleri

Aşağıdaki fiyatlar 30 Haziran 2021 ' ye kadar gerekli iletişim vergilerini ve ücretlerini içerir:

|Çağrısı yap   |Dakika başına fiyat|
|-----------|------------|
|Kanada     |Başlangıç fiyatı: $0.013/dk   |
|Birleşik Krallık     |Başlangıç fiyatı: $0,015/dk   |
|Almanya     |Başlangıç fiyatı: $0,015/dk   |
|Fransa     |Başlangıç fiyatı: $0,016/dk   |


### <a name="sms"></a>SMS

SMS, Kullandıkça Öde fiyatlandırması sunmaktadır. Fiyat, iletinin hedefine göre ileti başına ücretlendirilir. İletiler, Birleşik Devletler içinde bulunan telefon numaralarına ücretsiz telefon numaraları tarafından gönderilebilir. Yerel (coğrafi) telefon numaralarının SMS iletileri göndermek için kullanılamayacağını unutmayın.

Aşağıdaki fiyatlar 30 Haziran 2021 ' ye kadar gerekli iletişim vergilerini ve ücretlerini içerir:

|Ülke   |İleti gönderme|İleti alma|
|-----------|------------|------------|
|ABD (ücretsiz)    |$0.0075/msg   | $0.0075/msg |
