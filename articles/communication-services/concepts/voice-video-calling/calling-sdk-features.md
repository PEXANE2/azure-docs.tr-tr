---
title: İstemci kitaplığına genel bakış arayan Azure Iletişim Hizmetleri
titleSuffix: An Azure Communication Services concept document
description: Çağıran istemci kitaplığına genel bir bakış sağlar.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 9cc00cfa9f44c69a5880d53c0b7ac623f60be16b
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948168"
---
# <a name="calling-client-library-overview"></a>İstemci kitaplığına genel bakış çağrılıyor

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

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
| Genel           | En fazla 350 katılımcı içeren bir grup çağrısına katılın                                                       |  ✔️  | ✔️            | ✔️    
|                   | Mikrofon, konuşmacı ve kameranızı bir ses sınama hizmeti (8 ' i çağırarak kullanılabilir: echo123) ile test edin                   |  ✔️  | ✔️            | ✔️   

## <a name="calling-client-library-browser-support"></a>İstemci kitaplığı tarayıcı desteğini çağırma

Aşağıdaki tablo, şu anda kullanılabilir olan desteklenen tarayıcı ve sürümlerin kümesini temsil eder.

|                                  | Windows          | Mac OS          | Ubuntu | Linux  | Android | iOS    |
| -------------------------------- | ---------------- | -------------- | ------- | ------ | ------ | ------ |
| **İstemci kitaplığı çağrılıyor** | Chrome *, yeni kenar | Chrome *, Safari** | Mu  | Mu | Mu | Safari * * |


* Önceki iki sürüme ek olarak Chrome 'un en son sürümünün desteklendiğini unutmayın.<br/>

* * Safari sürümlerinin 13.1 + desteklendiğini unutmayın. Safari macOS için giden video henüz desteklenmiyor, ancak iOS üzerinde destekleniyor. Giden ekran paylaşımı yalnızca masaüstü iOS üzerinde destekleniyor.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Çağırma ile çalışmaya başlayın](../../quickstarts/voice-video-calling/getting-started-with-calling.md)

Daha fazla bilgi için aşağıdaki makaleleri inceleyin:
- Genel [çağrı akışları](../call-flows.md) hakkında bilgi edinin
- [Çağrı türleri](../voice-video-calling/about-call-types.md) hakkında bilgi edinin
- [PSTN çözümünüzü planlayın](../telephony-sms/plan-solution.md)
