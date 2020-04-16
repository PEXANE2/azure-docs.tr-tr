---
title: Çok cihazlı Konuşma (Önizleme) - Konuşma Hizmeti
titleSuffix: Azure Cognitive Services
description: ''
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: trbye
ms.openlocfilehash: 7c30ee2ef4a6ab0cd4241cac921a59eeadf5ce17
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81401055"
---
# <a name="what-is-multi-device-conversation-preview"></a>Çok Aygıtlı Konuşma (Önizleme) nedir?

**Çoklu aygıtlı konuşma,** birden çok istemci arasında bir konuşma veya metin görüşmesi oluşturmayı ve aralarında gönderilen iletileri koordine etmeyi kolaylaştırır.

**Çoklu cihazlı konuşma**ile şunları yapabilirsiniz:

- Birden çok istemciyi aynı konuşmaya bağlayın ve aralarındaki iletilerin gönderilmesini ve alınmasını yönetin.
- Her istemciden kolayca ses yazıya aktarın ve transkripsiyon'u isteğe bağlı çeviriyle diğerlerine gönderin.
- İsteğe bağlı çeviri ile istemciler arasında kolayca kısa mesaj gönderin.

Bir dizi aygıtta çalışan bir özellik veya çözüm oluşturabilirsiniz. Her cihaz diğer tüm aygıtlara bağımsız olarak ileti (ses veya anlık ileti transkripsiyonu) gönderebilir.

Konuşma [**Transkripsiyonu**](conversation-transcription.md) çok kanallı mikrofon dizisine sahip tek bir aygıtta çalışırken, **Çoklu aygıtlı Konuşma,** her biri tek bir mikrofona sahip birden çok cihaza sahip senaryolar için uygundur.

>[!IMPORTANT]
> Çoklu aygıtlı konuşma istemciler arasında ses dosyaları göndermeyi **desteklemez:** yalnızca transkripsiyon ve/veya çeviri.

## <a name="key-features"></a>Önemli özellikler

- **Gerçek zamanlı transkripsiyon** – Herkes konuşmanın bir dökümünü alır, böylece metni gerçek zamanlı olarak takip edebilir veya daha sonraya saklayabilir.
- **Gerçek zamanlı çeviri** – Metin çevirisi için desteklenen 60'tan fazla [dil](language-support.md#text-languages) ile kullanıcılar konuşmayı tercih ettikleri dile çevirebilirler.
- **Okunabilir transkriptler** – Transkripsiyon ve çeviri, noktalama işaretleri ve cümle molaları ile takip etmek kolaydır.
- **Ses veya metin girişi** – Her kullanıcı, katılımcının seçtiği dil için etkinleştirilen dil destek özelliklerine bağlı olarak kendi cihazında konuşabilir veya yazabilir. Lütfen [Dil desteğine](language-support.md#speech-to-text)bakın.
- **İleti rölesi** - Çok cihazlı konuşma hizmeti, bir istemci tarafından gönderilen iletileri kendi seçtikleri dilde tüm diğerlerine dağıtır.
- **İleti tanımlaması** – Kullanıcıların konuşmada aldıkları her ileti, onu gönderen kullanıcının takma adı ile etiketlenir.

## <a name="use-cases"></a>Uygulama alanları

### <a name="lightweight-conversations"></a>Hafif konuşmalar

Bir konuşma oluşturmak ve katılmak kolaydır. Bir kullanıcı 'ana bilgisayar' olarak hareket edecek ve rasgele beş harfli konuşma kodu ve QR kodu üreten bir konuşma oluşturacaktır. Diğer tüm kullanıcılar konuşma kodunu yazarak veya QR kodunu tarayarak konuşmaya katılabilir. 

Kullanıcılar konuşma kodu üzerinden katıldıklarına ve kişi bilgilerini paylaşmaları gerekmediğinden, hızlı ve yerinde konuşmalar oluşturmak kolaydır.

### <a name="inclusive-meetings"></a>Dahil toplantılar

Gerçek zamanlı transkripsiyon ve çeviri, farklı dillerkonuşan ve/veya işitme engelli veya işitme engelli kişiler için konuşmaları erişilebilir hale getirmeye yardımcı olabilir. Her kişi tercih ettiği dili konuşarak veya anlık ileti göndererek konuşmaya aktif olarak katılabilir.

### <a name="presentations"></a>Sunular

Ayrıca, sunumlar ve dersler için hem ekranda hem de izleyicilerin kendi cihazlarında altyazılar sağlayabilirsiniz. Dinleyiciler konuşma koduyla birleştikten sonra, transkripti tercih ettikleri dilde, kendi aygıtlarında görebilirler.

> [!NOTE]
> Bir örnek görmek için, çok cihazlı konuşma hizmetini kullanan bir PowerPoint eklentisi olan [Sunu Çevirmeni'ne](https://www.microsoft.com/translator/apps/presentation-translator/)göz atın. Söz konusu dosyayı [buradan](https://www.microsoft.com/download/details.aspx?id=55024) indirebilirsiniz.

## <a name="how-it-works"></a>Nasıl çalışır?

Tüm istemciler konuşma oluşturmak veya bir konuşmaya katılmak için Konuşma SDK'yı kullanır. Speech SDK, katılımcıların listesi, her müşterinin seçtiği dil(ler) ve gönderilen iletiler de dahil olmak üzere, konuşmanın kullanım ömrünü yöneten çok aygıtlı konuşma hizmetiyle etkileşime girecektir.  

Her istemci sesli veya anlık ileti gönderebilir. Hizmet, sesi metne dönüştürmek ve olduğu gibi anlık ileti göndermek için konuşma tanıma özelliğini kullanır. İstemciler farklı diller seçerse, hizmet tüm iletileri her istemcinin belirtilen dile(ler) çevirir.

![Çok cihazlı Konuşma Genel Bakış Diyagramı](media/scenarios/multi-device-conversation.png)

## <a name="overview-of-conversation-host-and-participant"></a>Konuşmaya Genel Bakış, Ev Sahibi ve Katılımcı

**Konuşma,** bir kullanıcının diğer katılımcı kullanıcıların katılması için başlattığı oturumdur. Tüm istemciler beş harfli **konuşma kodunu**kullanarak konuşmaya bağlanır.

Her konuşma, şunları içeren meta veriler oluşturur:
-    Konuşmanın ne zaman başladığına ve sona erdirileceye ait zaman damgaları
-    Konuşmadaki tüm katılımcıların listesi, her kullanıcının seçtiği takma adı ve konuşma veya metin girişi için birincil dili içerir.


Bir konuşmada iki tür kullanıcı vardır: **ana bilgisayar** ve **katılımcı.**

**Ana bilgisayar,** bir konuşmayı başlatan ve bu konuşmanın yöneticisi olarak hareket eden kullanıcıdır.
- Her konuşmanın yalnızca bir ana bilgisayarı olabilir
- Ana bilgisayar konuşma süresince konuşmaya bağlı olmalıdır. Ev sahibi konuşmayı terk ederse, konuşma diğer tüm katılımcılar için sona erer.
- Ev sahibinin konuşmayı yönetmek için birkaç ek denetimi vardır: 
    - Konuşmayı kilitleme - ek katılımcıların katılmasını engelleyin
    - Tüm katılımcıları sessize al - konuşma veya anlık iletilerden aktarılmış olsun, diğer katılımcıların konuşmaya mesaj göndermesini engelleyin
    - Bireysel katılımcıları sessize al
    - Tüm katılımcıların sessize al
    - Bireysel katılımcıların sessize al

**Katılımcı,** bir konuşmaya katılan bir kullanıcıdır.
- Bir katılımcı, diğer katılımcılar için konuşmayı sona erdirmeden aynı konuşmayı istediği zaman terk edebilir ve yeniden katılabilir.
- Katılımcılar konuşmayı kilitleyemez veya diğerlerini sessize alamaz/sessize alamaz

> [!NOTE]
> Her konuşmanın 100'e kadar katılımcısı olabilir ve bunların 10'u herhangi bir zamanda aynı anda konuşulabilir.

## <a name="language-support"></a>Dil desteği

Bir konuşma oluştururken veya katılırken, her kullanıcı bir **ana dili**seçmelidir: konuşacağı ve anlık ileti göndereceği dil ve ayrıca diğer kullanıcıların iletilerini görecekleri dil.

İki tür dil vardır: **yalnızca metinden metne ve metne:** **text-only**
- Kullanıcı birincil dili olarak **bir konuşma-metin** dili seçerse, konuşmada hem konuşma hem de metin girişi kullanabilir.

- Kullanıcı **yalnızca metin** dili seçerse, yalnızca metin girişini kullanabilir ve konuşmaya anlık ileti gönderebilir. Yalnızca metin dilleri metin çevirisi için desteklenen dillerdir, ancak metinden metne kadar değil. Kullanılabilir dilleri dil [destek](supported-languages.md) sayfasında görebilirsiniz.

Her katılımcı, birincil dilinin yanı sıra, konuşmayı çevirmek için ek diller de belirtebilir.

Aşağıda, kullanıcının seçtikleri ana dile dayalı çok aygıtlı bir konuşmada neler yapabileceğinin bir özeti verilmiştir.


| Kullanıcının konuşmada yapabilecekleri | Konuşmayı metne dönüştürme | Yalnızca metin |
|-----------------------------------|----------------|------|
| Konuşma girişi kullanma | ✔️ | ❌ |
| Anlık ileti gönderme | ✔️ | ✔️ |
| Konuşmayı çevirin | ✔️ | ✔️ |

> [!NOTE]
> Kullanılabilir konuşmadan metne ve metin çeviri dillerinin listeleri için [desteklenen dillere](supported-languages.md)bakın.



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşmaları gerçek zamanlı olarak çevirme](quickstarts/multi-device-conversation.md)
