---
title: Azure Bildirim Hub'ları şablonları
description: Azure Bildirim Hub'ları için şablonları kullanma hakkında bilgi edinin.
services: notification-hubs
documentationcenter: .net
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: a41897bb-5b4b-48b2-bfd5-2e3c65edc37e
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 7d88f57fe92b9da62cc9f90d64bdec4c27642fb0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76263753"
---
# <a name="templates"></a>Şablonlar

Şablonlar, istemci uygulamasının almak istediği bildirimlerin tam biçimini belirtmesini sağlar. Şablonları kullanarak, bir uygulama aşağıdakiler de dahil olmak üzere birkaç farklı avantaj gerçekleştirebilir:

- Bir platform-agnostik arka uç
- Kişiselleştirilmiş bildirimler
- İstemci sürümü bağımsızlığı
- Kolay yerelleştirme

Bu bölümde, platformlar arasında tüm cihazlarınızı hedefleyen platform agnostik bildirimler göndermek ve her cihaza yayın bildirimini kişiselleştirmek için şablonların nasıl kullanılacağına dair iki ayrıntılı örnek verilmektedir.

## <a name="using-templates-cross-platform"></a>Şablonları platform lar arası kullanma

Anında iletme bildirimleri göndermenin standart yolu, gönderilecek her bildirim için platform bildirim hizmetlerine (WNS, APNS) belirli bir yük göndermektir. Örneğin, APNS'ye uyarı göndermek için yük aşağıdaki formun Bir JSON nesnesidir:

```json
{"aps": {"alert" : "Hello!" }}
```

Bir Windows Mağazası uygulamasına benzer bir tost iletisi göndermek için XML yükü aşağıdaki gibidir:

```xml
<toast>
  <visual>
    <binding template=\"ToastText01\">
      <text id=\"1\">Hello!</text>
    </binding>
  </visual>
</toast>
```

MPNS (Windows Phone) ve FCM (Android) platformları için benzer yükler oluşturabilirsiniz.

Bu gereksinim, uygulamanın arka ucunu her platform için farklı yükler üretmeye zorlar ve arka ucu uygulamanın sunu katmanının bir kısmından etkili bir şekilde sorumlu kılar. Bazı endişeler arasında yerelleştirme ve grafik düzenleri (özellikle çeşitli kutucuk türleri için bildirimler içeren Windows Mağazası uygulamaları için) yer almaktadır.

Bildirim Hub'ları şablon özelliği, istemci uygulamasının, etiket kümesine ek olarak şablon kayıtları adı verilen özel kayıtlar oluşturmasını sağlar. Bildirim Hub'ları şablonözelliği, bir istemci uygulamasının, Yüklemeler (tercih edilen) veya Kayıtlarla çalışıp çalışmadığınızı şablonlarla ilişkilendirmesini sağlar. Önceki yük örnekleri göz önüne alındığında, tek platform bağımsız bilgi gerçek uyarı iletisi (Hello!). Şablon, Bildirim Merkezi'nin belirli bir istemci uygulamasının kaydı için platformdan bağımsız bir iletiyi nasıl biçimlendireceklerine ilişkin bir yönerge ler kümesidir. Önceki örnekte, platformdan bağımsız ileti tek bir `message = Hello!`özelliktir: .

Aşağıdaki resim işlemi göstermektedir:

![](./media/notification-hubs-templates/notification-hubs-hello.png)

iOS istemci uygulaması kaydı için şablon aşağıdaki gibidir:

```json
{"aps": {"alert": "$(message)"}}
```

Windows Mağazası istemci uygulaması için ilgili şablon:

```xml
<toast>
    <visual>
        <binding template=\"ToastText01\">
            <text id=\"1\">$(message)</text>
        </binding>
    </visual>
</toast>
```

Gerçek iletinin $(ileti) ifadesinin yerine geçilmesini unutmayın. Bu ifade, Bildirim Hub'ına, bu özel kayda bir ileti gönderdiğinde, onu izleyen ve ortak değerde geçiş yapan bir ileti oluşturması talimatını verir.

Yükleme modeliyle çalışıyorsanız, yükleme "şablonları" anahtarı birden çok şablondan oluşan bir JSON tutar. Kayıt modeliyle çalışıyorsanız, istemci uygulaması birden çok şablon kullanmak için birden çok kayıt oluşturabilir; örneğin, uyarı iletileri için bir şablon ve döşeme güncelleştirmeleri için bir şablon. İstemci uygulamaları, yerel kayıtları (şablonsuz kayıtlar) ve şablon kayıtlarını da karıştırabilir.

Bildirim Hub' ı, her şablon için aynı istemci uygulamasına ait olup olmadıklarını düşünmeden bir bildirim gönderir. Bu davranış, platformdan bağımsız bildirimleri daha fazla bildirime çevirmek için kullanılabilir. Örneğin, Bildirim Hub'ına gönderilen aynı platformdan bağımsız ileti, arka ucun bunun farkında olmasını gerektirmeden, tost uyarısı ve döşeme güncelleştirmesi olarak sorunsuz bir şekilde çevrilebilir. Bazı platformlar (örneğin, iOS), kısa bir süre içinde gönderilirse aynı aygıta birden çok bildirimi daraltabilir.

## <a name="using-templates-for-personalization"></a>Kişiselleştirme için şablonları kullanma

Şablonları kullanmanın bir diğer avantajı da bildirimlerin kayıt başına kişiselleştirmesini gerçekleştirmek için Bildirim Hub'larını kullanabilme özelliğidir. Örneğin, belirli bir konumda hava koşulları ile bir kiremit görüntüleyen bir hava uygulaması düşünün. Bir kullanıcı santigrat veya Fahrenheit dereceve tek veya beş günlük tahmin arasında seçim yapabilirsiniz. Şablonları kullanarak, her istemci uygulama yüklemesi gerekli biçime kaydolabilir (1 günlük Santigrat, 1 günlük Fahrenheit, 5 günlük Santigrat, 5 günlük Fahrenheit) ve arka uç bu şablonları doldurmak için gerekli tüm bilgileri içeren tek bir ileti göndermek zorunda (örneğin, santigrat ve Fahrenhayt dereceile beş günlük bir tahmin).

Santigrat sıcaklıkları ile bir günlük tahmin için şablon aşağıdaki gibidir:

```xml
<tile>
  <visual>
    <binding template="TileWideSmallImageAndText04">
      <image id="1" src="$(day1_image)" alt="alt text"/>
      <text id="1">Seattle, WA</text>
      <text id="2">$(day1_tempC)</text>
    </binding>  
  </visual>
</tile>
```

Bildirim Hub'ına gönderilen ileti aşağıdaki tüm özellikleri içerir:

```html
<table border="1">

<tr><td>day1_image</td><td>day2_image</td><td>day3_image</td><td>day4_image</td><td>day5_image</td></tr>

<tr><td>day1_tempC</td><td>day2_tempC</td><td>day3_tempC</td><td>day4_tempC</td><td>day5_tempC</td></tr>

<tr><td>day1_tempF</td><td>day2_tempF</td><td>day3_tempF</td><td>day4_tempF</td><td>day5_tempF</td></tr>
</table><br/>
```

Bu deseni kullanarak, arka uç yalnızca uygulama kullanıcıları için belirli kişiselleştirme seçeneklerini depolamak zorunda kalmadan tek bir ileti gönderir. Aşağıdaki resim bu senaryoyu göstermektedir:

![](./media/notification-hubs-templates/notification-hubs-registration-specific.png)

## <a name="how-to-register-templates"></a>Şablonlar nasıl kaydedilir?

Yükleme modelini (tercih edilen) veya Kayıt modelini kullanarak şablonlara kaydolmak için [Kayıt Yönetimi'ne](notification-hubs-push-notification-registration-management.md)bakın.

## <a name="template-expression-language"></a>Şablon ifade dili

Şablonlar XML veya JSON belge biçimleriyle sınırlıdır. Ayrıca, ifadeleri yalnızca belirli yerlere yerleştirebilirsiniz; örneğin, XML için düğüm öznitelikleri veya değerleri, JSON için dize özellik değerleri.

Aşağıdaki tablo, şablonlarda izin verilen dili gösterir:

| İfadeler       | Açıklama |
| ---------------- | --- |
| $(sahne)          | Verilen ada sahip bir olay özelliğine başvuru. Özellik adları büyük/küçük harf duyarlı değildir. Bu ifade, özellik yoksa özelliğin metin değerine veya boş bir dize ye giderir. |
| $(sahne, n)       | Yukarıdaki gibi, ancak metin açıkça n karakterleri kırpılmış, örneğin $(başlık, 20) 20 karakter de başlık özelliğinin içeriğini klipler. |
| . (pervane, n)       | Yukarıdaki gibi, ancak metin kırpılmış olarak üç nokta ile suffixed. Kırpılayan dize ve sonekin toplam boyutu n karakterini geçmez. . (başlık, 20) bir giriş özelliği ile "Bu başlık satırı" sonuçları **Bu başlık ...** |
| %(pervane)          | Çıktının URI kodlanmış olması dışında $(name) benzer. |
| #(pervane)          | JSON şablonlarında kullanılır (örneğin, iOS ve Android şablonları için).<br><br>Bu işlev, JSON şablonlarında (örneğin, Apple şablonları) kullanılmadığı sürece, daha önce belirtilen $(prop) ile tam olarak aynı şekilde çalışır. Bu durumda, bu işlev "{','}" (örneğin, 'myJsonProperty' : '#(name)' ile çevrili değilse ve javascript biçiminde bir sayıya göre değerlendirilmiyorsa, örneğin, regexp: (0&#124;(&#91;1-9&#93;&#91;0-9&#93;*)(&#91;\. 0-9&#93;+)? ((e&#124;E)(+&#124;-)?&#91;0-9&#93;+)?, sonra çıkış JSON bir sayıdır.<br><br>Örneğin, 'rozet: '#(name)' 'rozet' olur: 40 ('40' değil). |
| 'metin' veya "metin" | Gerçek bir şey. Literals tek veya çift tırnak içinde kapalı rasgele metin içerir. |
| expr1 + expr2    | İki ifadeyi tek bir dize halinde birleştiren birleştirme işleci. |

İfadeler önceki formlardan herhangi biri olabilir.

Birlikteleştirme kullanırken, tüm ifade `{}`. Örneğin, `{$(prop) + ‘ - ’ + $(prop2)}`.

Örneğin, aşağıdaki şablon geçerli bir XML şablonu değildir:

```xml
<tile>
  <visual>
    <binding $(property)>
      <text id="1">Seattle, WA</text>
    </binding>  
  </visual>
</tile>
```

Daha önce açıklandığı gibi, concatenation kullanırken, ifadeler kıvırcık parantez içinde sarılmış olmalıdır. Örnek:

```xml
<tile>
  <visual>
    <binding template="ToastText01">
      <text id="1">{'Hi, ' + $(name)}</text>
    </binding>  
  </visual>
</tile>
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure Bildirim Hub'ları hakkında bilgi edinin](notification-hubs-push-notification-overview.md)