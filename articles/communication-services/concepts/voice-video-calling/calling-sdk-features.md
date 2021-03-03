---
title: İstemci kitaplığına genel bakış arayan Azure Iletişim Hizmetleri
titleSuffix: An Azure Communication Services concept document
description: Çağıran istemci kitaplığına genel bir bakış sağlar.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 09/30/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: ed56d0cf69710173d41c5262e7a3121a0cad1304
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101660149"
---
# <a name="calling-client-library-overview"></a>Arama istemci kitaplığına genel bakış

İstemci kitaplıkları, *istemciler* ve hizmetler için iki ayrı iki farklı aileleri vardır *.* Şu anda kullanılabilir olan istemci kitaplıkları son kullanıcı deneyimleri için tasarlanmıştır: Web siteleri ve yerel uygulamalar.

Hizmet istemci kitaplıkları henüz kullanılabilir değildir ve botların ve diğer hizmetlerle tümleştirilmesine uygun ham ses ve video veri düzlemleri için erişim sağlar.

## <a name="calling-client-library-capabilities"></a>İstemci kitaplığı yeteneklerini çağırma

Aşağıdaki liste, şu anda istemci kitaplıklarını çağıran Azure Iletişim hizmetlerinde bulunan özellik kümesini gösterir.

| Özellik grubu | Özellik                                                                                                          | JS  | Java (Android) | Objective-C (iOS)
| ----------------- | ------------------------------------------------------------------------------------------------------------------- | ---  | -------------- | -------------
| Temel yetenekler | İki kullanıcı arasında bire bir çağrı yerleştir                                                                           | ✔️   | ✔️            | ✔️
|                   | İkiden fazla kullanıcısı olan bir grup çağrısı Yerleştir (en fazla 350 Kullanıcı)                                                       | ✔️   | ✔️            | ✔️
|                   | İki kullanıcıyla daha fazla kullanıcı içeren bir grup çağrısında bir tek-bir çağrıyı yükseltin                                 | ✔️   | ✔️            | ✔️
|                   | Başlatıldıktan sonra bir grup çağrısına katılır                                                                              | ✔️   | ✔️            | ✔️
|                   | Başka bir VoIP katılımcısını devam eden bir grup çağrısına katılmaya davet etme                                                       | ✔️   | ✔️            | ✔️
|                   | Videonuzu açma/kapatma                                                         | ✔️   | ✔️            | ✔️
|                   | Mikrofonu sustur/aç                                                                                                     | ✔️   | ✔️            | ✔️
|                   | Kameralar arasında geçiş yapma                                                                                              | ✔️   | ✔️            | ✔️
|                   | Yerel saklama/tutma                                                                                                  | ✔️   | ✔️            | ✔️
|                   | Etkin konuşmacı                                                                                                      | ✔️   | ✔️            | ✔️
|                   | Çağrılar için konuşmacı seçin                                                                                            | ✔️   | ✔️            | ✔️
|                   | Çağrılar için mikrofon seçin                                                                                         | ✔️   | ✔️            | ✔️
|                   | Katılımcının durumunu göster<br/>*Boşta, erken medya, bağlanma, bağlı, bekleme süresi, giriş, bağlantısız*         | ✔️   | ✔️            | ✔️
|                   | Bir çağrının durumunu göster<br/>*Erken medya, gelen, bağlantı, çalma, bağlı, bekletme, bağlantısı kesiliyor, bağlantısı kesildi* | ✔️   | ✔️            | ✔️
|                   | Bir katılımcının kapalı olup olmadığını göster                                                                                      | ✔️   | ✔️            | ✔️
|                   | Katılımcının bir çağrı bıraktı nedenini göster                                                                       | ✔️   | ✔️            | ✔️
| Ekran paylaşımı    | Uygulamanın içinden ekranın tamamını paylaşma                                                                 | ✔️   | ❌            | ❌
|                   | Belirli bir uygulamayı paylaşma (çalışan uygulamalar listesinden)                                                | ✔️   | ❌            | ❌
|                   | Açık sekmeler listesinden bir Web tarayıcısı sekmesi paylaşma                                                                  | ✔️   | ❌            | ❌
|                   | Katılımcı, uzak ekran paylaşımından görüntüleyebilir                                                                            | ✔️   | ✔️            | ✔️
| Listesi            | Katılımcıları Listele                                                                                                   | ✔️   | ✔️            | ✔️
|                   | Katılımcıyı kaldırma                                                                                                | ✔️   | ✔️            | ✔️
| PSTN              | PSTN katılımcısı ile bire bir çağrı yerleştirme                                                                     | ✔️   | ✔️            | ✔️
|                   | PSTN katılımcıları ile bir grup çağrısı yerleştirme                                                                           | ✔️   | ✔️            | ✔️
|                   | Bir PSTN katılımcısı ile bire bir çağrıyı bir grup çağrısına yükseltme                                                 | ✔️   | ✔️            | ✔️
|                   | Bir grup çağrısından PSTN katılımcısı olarak dışarıyı arama                                                                    | ✔️   | ✔️            | ✔️
| Genel           | Mikrofon, konuşmacı ve kameranızı bir ses sınama hizmeti (8 ' i çağırarak kullanılabilir: echo123) ile test edin                   |  ✔️  | ✔️            | ✔️

## <a name="javascript-calling-client-library-support-by-os-and-browser"></a>İşletim sistemi ve tarayıcı tarafından istemci kitaplığı desteğini çağıran JavaScript

Aşağıdaki tablo, şu anda kullanılabilir olan desteklenen tarayıcı ve sürümlerin kümesini temsil eder.

|                                  | Windows          | Mac OS          | Ubuntu | Linux  | Android | iOS    | iPad işletim sistemi|
| -------------------------------- | ---------------- | -------------- | ------- | ------ | ------ | ------ | -------|
| **İstemci kitaplığı çağrılıyor** | Chrome *, yeni kenar | Chrome *, Safari** | Mu  | Mu | Mu | Safari * * | Safari * * |


* Önceki iki sürüme ek olarak Chrome 'un en son sürümünün desteklendiğini unutmayın.<br/>

* * Safari sürümlerinin 13.1 + desteklendiğini unutmayın. Safari macOS için giden video henüz desteklenmiyor, ancak iOS üzerinde destekleniyor. Giden ekran paylaşımı yalnızca masaüstü iOS üzerinde destekleniyor. 1:1 ve grup çağrıları şu anda Safari 'de kullanılamıyor.

## <a name="calling-client---browser-security-model"></a>Çağıran istemci-tarayıcı güvenlik modeli

### <a name="user-webrtc-over-https"></a>HTTPS üzerinden Kullanıcı WebRTC

Gibi WebRTC API 'Leri `getUserMedia` , bu API 'leri çağıran UYGULAMANıN https üzerinden sunulmasını gerektirir.

Yerel geliştirme için kullanabilirsiniz `http://localhost` .

### <a name="embed-the-communication-services-calling-sdk-in-an-iframe"></a>SDK 'Yı çağıran Iletişim hizmetlerini iframe 'e ekleme

Yeni bir [izinler ilkesi (özellik ilkesi de denir)](https://www.w3.org/TR/permissions-policy-1/#iframe-allow-attribute) çeşitli tarayıcılar tarafından benimsenmekte. Bu ilke, uygulamaların bir çapraz kaynak iframe öğesi aracılığıyla bir cihazın kameraya ve mikrofona nasıl erişebileceğini denetleyerek çağrı senaryolarını etkiler.

Uygulamanın bir parçasını farklı bir etki alanından barındırmak için bir iframe kullanmak istiyorsanız, `allow` özniteliğini doğru değeri olan IFRAME 'nize eklemeniz gerekir.

Örneğin, bu iframe hem kamera hem de mikrofon erişimine izin verir:

```html
<iframe allow="camera *; microphone *">
```

## <a name="calling-client-library-streaming-support"></a>İstemci kitaplığı akış desteğini çağırma
İstemci kitaplığını çağıran Iletişim Hizmetleri aşağıdaki akış yapılandırmasını destekler:

|           |Web | Android/iOS|
|-----------|----|------------|
|**aynı anda gönderilebilecek giden akış sayısı** |1 ses/video veya 1 ses/ekran paylaşımı | 1 ses/video |
|**aynı anda işlenebilen gelen akış sayısı** |1 ses/video veya 1 ses/ekran paylaşımı| 6 ses/video veya 1 ekran paylaşımı |

Grup senaryolarında, tüm ses katılımcılarını desteklemek için bir karma ses akışı kullanıldığını unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Çağırma ile çalışmaya başlayın](../../quickstarts/voice-video-calling/getting-started-with-calling.md)

Daha fazla bilgi için aşağıdaki makaleleri inceleyin:
- Genel [çağrı akışları](../call-flows.md) hakkında bilgi edinin
- [Çağrı türleri](../voice-video-calling/about-call-types.md) hakkında bilgi edinin
- [Telefon numarası türleri](../telephony-sms/plan-solution.md) hakkında bilgi edinin
