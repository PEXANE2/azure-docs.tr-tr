---
title: İşbirlikçi Çeviri Çerçevesi (CTF) Raporlama - Çevirmen Metin API
titleSuffix: Azure Cognitive Services
description: İşbirlikçi Çeviri Çerçevesi (CTF) raporlaması nasıl kullanılır?
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: swmachan
ms.openlocfilehash: 6a197095d97e67f7548e60375148cff57e47b797
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68595930"
---
# <a name="how-to-use-collaborative-translation-framework-ctf-reporting"></a>Collaborative Translation Framework (CTF) raporlamasını kullanma

> [!NOTE]
> Bu yöntem amortismana hazırdır. Çevirmen Metin API'sinin V3.0'ında kullanılamaz.
> 
> Daha önce Çevirmen Metin API'sinin V2.0 için kullanıma sunulduğu İşbirlikçi Çeviriler Çerçevesi (CTF), 1 Şubat 2018 itibariyle amortismana kaldırıldı. AddTranslation ve AddTranslationArray işlevleri, kullanıcıların Ortak Çeviri Çerçevesi üzerinden düzeltmelere olanak sağlamasına olanak tanır. 31 Ocak 2018'den sonra, bu iki işlev yeni cümle gönderimlerini kabul etmedi ve kullanıcılar bir hata iletisi aldı. Bu işlevler kullanımdan kaldırıldı ve değiştirilmeyecek.

İşbirliğine Dayalı Çeviri Çerçevesi (CTF) Raporlama API'si istatistikleri ve CTF deposundaki gerçek içeriği döndürür. Bu API GetTranslations() yöntemifarklıdır, çünkü:
* Çevrilen içeriği ve toplam sayısını yalnızca hesabınızdan (appId veya Azure Marketi hesabı) döndürür.
* Çevrilen içeriği ve toplam sayısını kaynak cümlenin eşleşmesine gerek kalmadan döndürür.
* Otomatik çeviri (makine çevirisi) döndürmez.

## <a name="endpoint"></a>Uç Nokta
CTF Raporlama API'sinin bitiş noktasıhttps://api.microsofttranslator.com/v2/beta/ctfreporting.svc


## <a name="methods"></a>Yöntemler
| Adı |    Açıklama|
|:---|:---|
| GetUserTranslationCounts Yöntemi | Kullanıcı tarafından oluşturulan çevirilerin sayımlarını alın. |
| GetUserTranslations Yöntemi | Kullanıcı tarafından oluşturulan çevirileri alır. |

Bu yöntemler şunları yapmanızı sağlar:
* İndirilmek için hesap kimliğinizin altındaki tüm kullanıcı çevirileri ve düzeltmeleri setini alın.
* Sık katkıda bulunanların listesini alın. AddTranslation() 'de doğru kullanıcı adının sağlandığından emin olun.
* Uri önekine göre, güvenilir kullanıcılarınızın gerektiğinde sitenizin bir bölümüyle sınırlı olması durumunda tüm kullanılabilir adayları görmesini sağlayan bir kullanıcı arabirimi (UI) oluşturun.

> [!NOTE]
> Her iki yöntem de nispeten yavaş ve pahalıdır. Dikkatli kullanılması tavsiye edilir.

## <a name="getusertranslationcounts-method"></a>GetUserTranslationCounts yöntemi

Bu yöntem, kullanıcı tarafından oluşturulan çevirilerin sayısını alır. UriPrefix'e göre gruplanan çeviri sayımlarının listesini sağlar, dan, kullanıcıya, minRating'e ve maxRating istek parametrelerine.

**Sözdizimi**

> [!div class="tabbedCodeSnippets"]
> ```cs
> UserTranslationCount[]GetUserTranslationCounts(
>            string appId,
>            string uriPrefix,
>            string from,
>            string to,
>            int? minRating,
>            int? maxRating,
>            string user,
>            string category
>            DateTime? minDateUtc,
>            DateTime? maxDateUtc,
>            int? skip,
>            int? take);
> ```

**Parametreler**

| Parametre | Açıklama |
|:---|:---|
| appId | **Gerekli** Yetkilendirme üstbilgisi kullanılırsa, appid alanını boş bırakın, "Taşıyıcı" + " + erişim belirteci içeren bir dize belirtin.|
| uriPrefix | **İsteğe bağlı** Çevirinin URI önekisini içeren bir dize.|
| Kaynak | **İsteğe bağlı** Çeviri metninin dil kodunu temsil eden bir dize. |
| - | **İsteğe bağlı** Metni çevirmek için dil kodunu temsil eden bir dize.|
| minRating| **İsteğe bağlı** Çevrilen metnin minimum kalite derecelendirmesini temsil eden bir tamsayı değeri. Geçerli değer -10 ile 10 arasındadır. Varsayılan değer 1’dir.|
| maxRating| **İsteğe bağlı** Çevrilen metnin en yüksek kalite derecesini temsil eden bir tamsayı değeri. Geçerli değer -10 ile 10 arasındadır. Varsayılan değer 1’dir.|
| kullanıcı | **İsteğe bağlı** Gönderinin kaynağına göre sonucu filtrelemek için kullanılan bir dize. |
| category| **İsteğe bağlı** Çevirinin kategorisini veya etki alanını içeren bir dize. Bu parametre yalnızca varsayılan seçeneği genel destekler.|
| minDateUtc| **İsteğe bağlı** Çevirileri almak istediğiniz tarih. Tarih UTC formatında olmalıdır. |
| maxDateUtc| **İsteğe bağlı** Çevirileri almak istediğiniz tarihe kadar olan tarih. Tarih UTC formatında olmalıdır. |
| Atla| **İsteğe bağlı** Bir sayfada atlamak istediğiniz sonuç sayısı. Örneğin, sonuçların ilk 20 satırını atlamak ve 21. Bu parametrenin varsayılan değeri 0'dır.|
| almak | **İsteğe bağlı** Almak istediğiniz sonuç sayısı. Her isteğin maksimum sayısı 100'dür. Varsayılan değer 100'dür.|

> [!NOTE]
> Atlama ve alma isteği parametreleri çok sayıda sonuç kaydı için pagination sağlar.

**İade değeri**

Sonuç kümesi **UserTranslationCount**dizi içerir. Her UserTranslationCount aşağıdaki öğelere sahiptir:

| Alan | Açıklama |
|:---|:---|
| Sayı| Alınan sonuç sayısı|
| Başlangıç | Kaynak dil|
| Derecelendirme| AddTranslation() yöntemi çağrısında gönderen tarafından uygulanan derecelendirme|
| Alıcı| Hedef dil|
| Urı| AddTranslation() yöntemi çağrısında uygulanan URI|
| Kullanıcı| Kullanıcı adı|

**Özel durumlar**

| Özel durum | İleti | Koşullar |
|:---|:---|:---|
| Argumentoutofrangeexception | '**maxDateUtc**' parametresi '**minDateUtc**' dan büyük veya eşit olmalıdır.| **MaxDateUtc** parametresinin değeri **minDateUtc**parametresinin değerinden daha azdır.|
| TranslateApiException | IP kotanın üzerinde.| <ul><li>Dakika başına istek sayısı sınırına ulaşılır.</li><li>İstek boyutu 10000 karakterle sınırlı kalır.</li><li>Saatlik ve günlük kota, Microsoft Translator API'sinin kabul edeceği karakter sayısını sınırlar.</li></ul>|
| TranslateApiException | AppId kotanın üzerinde.| Başvuru kimliği saatlik veya günlük kotayı aştı.|

> [!NOTE]
> Kota, hizmetin tüm kullanıcıları arasında adaleti sağlamak için ayarlanır.

**GitHib'deki kod örneklerini görüntüleyin**
* [C #](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslationcounts-example-csharp.md)
* [PHP](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslationcounts-example-php.md)

## <a name="getusertranslations-method"></a>GetUserTranslations yöntemi

Bu yöntem, kullanıcı tarafından oluşturulan çevirileri alır. UriPrefix tarafından gruplanan çevirileri, from, to, user ve minRating ve maxRating istek parametrelerini sağlar.

**Sözdizimi**

> [!div class="tabbedCodeSnippets"]
> ```cs
> UserTranslation[] GetUserTranslations (
>             string appId,
>             string uriPrefix,
>             string from,
>             string to,
>             int? minRating,
>             int? maxRating,
>             string user,
>             string category
>             DateTime? minDateUtc,
>             DateTime? maxDateUtc,
>             int? skip,
>             int? take);
> ```

**Parametreler**

| Parametre | Açıklama |
|:---|:---|
| appId | **Gerekli** Yetkilendirme üstbilgisi kullanılırsa, appid alanını boş bırakın, "Taşıyıcı" + " + erişim belirteci içeren bir dize belirtin.|
| uriPrefix| **İsteğe bağlı** Çevirinin URI önekisini içeren bir dize.|
| Kaynak| **İsteğe bağlı** Çeviri metninin dil kodunu temsil eden bir dize.|
| -| **İsteğe bağlı** Metni çevirmek için dil kodunu temsil eden bir dize.|
| minRating| **İsteğe bağlı** Çevrilen metnin minimum kalite derecelendirmesini temsil eden bir tamsayı değeri. Geçerli değer -10 ile 10 arasındadır. Varsayılan değer 1’dir.|
| maxRating| **İsteğe bağlı** Çevrilen metnin en yüksek kalite derecesini temsil eden bir tamsayı değeri. Geçerli değer -10 ile 10 arasındadır. Varsayılan değer 1’dir.|
| kullanıcı| **Isteğe bağlı. Gönderinin kaynağına göre sonucu filtrelemek için kullanılan bir dize**|
| category| **İsteğe bağlı** Çevirinin kategorisini veya etki alanını içeren bir dize. Bu parametre yalnızca varsayılan seçeneği genel destekler.|
| minDateUtc| **İsteğe bağlı** Çevirileri almak istediğiniz tarih. Tarih UTC formatında olmalıdır.|
| maxDateUtc| **İsteğe bağlı** Çevirileri almak istediğiniz tarihe kadar olan tarih. Tarih UTC formatında olmalıdır.|
| Atla| **İsteğe bağlı** Bir sayfada atlamak istediğiniz sonuç sayısı. Örneğin, sonuçların ilk 20 satırını atlamak ve 21. Bu parametrenin varsayılan değeri 0'dır.|
| almak| **İsteğe bağlı** Almak istediğiniz sonuç sayısı. Her isteğin maksimum sayısı 100'dür. Varsayılan değer 50'dir.|

> [!NOTE]
> Atlama ve alma isteği parametreleri çok sayıda sonuç kaydı için pagination sağlar.

**İade değeri**

Sonuç kümesi **UserTranslation**diziiçerir. Her UserTranslation aşağıdaki öğelere sahiptir:

| Alan | Açıklama |
|:---|:---|
| CreatedDateUtc| AddTranslation() kullanarak girişin oluşturulma tarihi|
| Başlangıç| Kaynak dil|
| Orijinal Metin| İstek gönderirken kullanılan kaynak dil metni|
|Derecelendirme |AddTranslation() yöntemi çağrısında gönderen tarafından uygulanan derecelendirme|
|Alıcı|    Hedef dil|
|Çevrilmiş Metin|    AddTranslation() yöntemi çağrısında gönderilen çeviri|
|Urı|   AddTranslation() yöntemi çağrısında uygulanan URI|
|Kullanıcı   |Kullanıcı adı|

**Özel durumlar**

| Özel durum | İleti | Koşullar |
|:---|:---|:---|
| Argumentoutofrangeexception | '**maxDateUtc**' parametresi '**minDateUtc**' dan büyük veya eşit olmalıdır.| **MaxDateUtc** parametresinin değeri **minDateUtc**parametresinin değerinden daha azdır.|
| TranslateApiException | IP kotanın üzerinde.| <ul><li>Dakika başına istek sayısı sınırına ulaşılır.</li><li>İstek boyutu 10000 karakterle sınırlı kalır.</li><li>Saatlik ve günlük kota, Microsoft Translator API'sinin kabul edeceği karakter sayısını sınırlar.</li></ul>|
| TranslateApiException | AppId kotanın üzerinde.| Başvuru kimliği saatlik veya günlük kotayı aştı.|

> [!NOTE]
> Kota, hizmetin tüm kullanıcıları arasında adaleti sağlamak için ayarlanır.

**GitHib'deki kod örneklerini görüntüleyin**
* [C #](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslations-example-csharp.md)
* [PHP](https://github.com/MicrosoftTranslator/Documentation-Code-TextAPI/blob/master/ctf/ctf-getusertranslations-example-php.md)
