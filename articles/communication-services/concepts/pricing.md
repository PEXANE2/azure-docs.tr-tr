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
ms.openlocfilehash: 0b82d7dc8f6aaaa28a5293966440f058fbf42dc1
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91460943"
---
# <a name="pricing-scenarios"></a>Fiyatlandırma senaryoları

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

<!--
> [!WARNING]
> This document is under construction and needs the following items to be addressed:
> - Looks like other resources point to a /pricing page that is managed by Commerce or Marketing? https://azure.microsoft.com/pricing/details/functions/ Should we? FOLLOWING UP WITH KRISTIN TO FIND THE RIGHT ACS PAGE
-->

Azure Iletişim Hizmetleri için fiyatlar, ön ücretler olmadan Kullandıkça Öde modeline dayanır. Yalnızca kullanım için ücretlendirilirsiniz ve hizmetlerin kullanımı için faturalandırılırsınız.

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

- Gönderilen her sohbet iletisi için $0,0008 ücretlendirilirsiniz.

### <a name="pricing-example-chat-between-two-users"></a>Fiyatlandırma örneği: iki kullanıcı arasında sohbet 

Geeta, bir güncelleştirmeyi paylaşmak ve 5 ileti göndermesi için bir sohbet iş parçacığı başlatır. Sohbet, Geeta 'da 10 dakika sürer ve her biri bir adet 15 ileti gönderir.

**Maliyet hesaplamaları** 
- Gönderilen ileti sayısı (5 + 15 + 15) x $0,0008 = $0,028

### <a name="pricing-example-group-chat-with-multiple-users"></a>Fiyatlandırma örneği: birden çok kullanıcı ile sohbeti gruplayın 

Charlie, arkadaşlarını bir tatil planı için & Jasmine ile bir sohbet iş parçacığı başlatır. Bu kişiler, Charlie 'de, Casey & Jasmine 'nın sırasıyla 20, 30 ve 18 ileti gönderdikleri sırada bir sohbet edebilir. Bu kişilerin, arkadaşınızın yolculuğa katılması, sohbet iş parçacığına eklemesi ve tüm ileti geçmişini kendi ile paylaşması hakkında fark ederler. 

Gül iletileri görür ve sohbet başlatır. Bu arada, Casey bir çağrı alır ve konuşmayı daha sonra yakalamak üzere karar verir. Charlie, Jasmine & gül, seyahat tarihlerine karar verir ve sırasıyla bir 30, 25, 35 ileti gönderir.

**Maliyet hesaplamaları** 

- Gönderilen ileti sayısı (20 + 30 + 18 + 30 + 25 + 35) x $0,0008 = $0,1264
