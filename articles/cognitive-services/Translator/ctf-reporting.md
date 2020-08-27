---
title: İşbirliğine dayalı Çeviri Çerçevesi (CTF) raporlama-Translator
titleSuffix: Azure Cognitive Services
description: Işbirliğine dayalı Çeviri Çerçevesi (CTF) raporlamasını kullanma.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: swmachan
ms.custom: devx-track-csharp
ms.openlocfilehash: 68b0de40940fa75dd4eb4e1572405f31ce1c22b8
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88934387"
---
# <a name="how-to-use-collaborative-translation-framework-ctf-reporting"></a>Collaborative Translation Framework (CTF) raporlamasını kullanma

> [!NOTE]
> Bu yöntem kullanım dışıdır. Bu, ' ın ' ın V 3.0 sürümünde kullanılamaz.
> 
> Önceden V 2.0 için sunulan Işbirliği çevirileri çerçevesi (CTF), 1 Şubat 2018 itibariyle kullanımdan kaldırılmıştır. AddTranslation ve AddTranslationArray işlevleri, kullanıcıların Işbirliğine dayalı çeviri çerçevesi aracılığıyla düzeltmeleri etkinleştirmesine olanak tanır. 31 Ocak 2018 ' den sonra bu iki işlev yeni tümce gönderilerini kabul etmedi ve kullanıcılar bir hata iletisi alır. Bu işlevler kullanımdan kalkmıştı ve değiştirilmeyecektir.

Işbirlikçi Çeviri Çerçevesi (CTF) Raporlama API 'SI, CTF deposundaki istatistikleri ve gerçek içeriği döndürür. Bu API, Getçeviriler () yönteminden farklıdır, çünkü:
* Çevrilen içeriği ve toplam sayısını yalnızca hesabınızdan (AppID veya Azure Market hesabı) döndürür.
* Kaynak cümle eşleşmesi gerekmeden çevrilmiş içeriği ve toplam sayısını döndürür.
* Otomatik çeviriyi döndürmez (makine çevirisi).

## <a name="endpoint"></a>Uç Noktası
CTF Raporlama API 'sinin uç noktası https://api.microsofttranslator.com/v2/beta/ctfreporting.svc .

## <a name="methods"></a>Yöntemler
| Ad | Açıklama|
|:---|:---|
| GetUserTranslationCounts yöntemi | Kullanıcı tarafından oluşturulan çevirilerin sayısını alır. |
| Getuserçeviriler yöntemi | Kullanıcı tarafından oluşturulan çevirileri alır. |

Bu yöntemler şunları sağlar:
* Kullanıcı çevirileri ve yükleme için hesap KIMLIĞINIZ altında düzeltmeler alın.
* Sık katkıda bulunanlar listesini alın. AddTranslation () içinde doğru Kullanıcı adının sağlandığından emin olun.
* Bir kullanıcı arabirimi (UI) oluşturun ve bu sayede, gerekli olan tüm adayları, URI ön ekine bağlı olarak sitenizin bir bölümüyle sınırlı olup olmadığını görebilirsiniz.

> [!NOTE]
> Her iki yöntem de nispeten yavaş ve pahalıdır. Bunların gelişigüzel kullanılması önerilir.

## <a name="getusertranslationcounts-method"></a>GetUserTranslationCounts yöntemi

Bu yöntem, Kullanıcı tarafından oluşturulan çevirilerin sayısını alır. Bu, uriPrefix, from, to, User, Minderecelendirme ve Maxderecelendirme istek parametrelerine göre gruplanmış çeviri sayısı listesini sağlar.

**Syntax**

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
| appId | **Gerekli** Yetkilendirme üst bilgisi kullanılırsa, AppID alanını boş bırakın başka bir "taşıyıcı" + "" + erişim belirteci içeren bir dize belirtin.|
| Uriön Eki | **Isteğe bağlı** Çeviri URI 'sinin önekini içeren bir dize.|
| Kaynak | **Isteğe bağlı** Çeviri metninin dil kodunu temsil eden bir dize. |
| şöyle değiştirin: | **Isteğe bağlı** Metnin çevrilmesi için dil kodunu temsil eden bir dize.|
| Minderecelendirme| **Isteğe bağlı** Çevrilmiş metin için en düşük kalite derecelendirmesini temsil eden bir tamsayı değeri. Geçerli değer-10 ile 10 arasındadır. Varsayılan değer 1’dir.|
| Maxderecelendirme| **Isteğe bağlı** Çevrilmiş metin için maksimum kalite derecelendirmesini temsil eden bir tamsayı değeri. Geçerli değer-10 ile 10 arasındadır. Varsayılan değer 1’dir.|
| kullanıcı | **Isteğe bağlı** Gönderimi oluşturana göre sonucu filtrelemek için kullanılan bir dize. |
| category| **Isteğe bağlı** Çevirinin kategorisini veya etki alanını içeren bir dize. Bu parametre yalnızca genel varsayılan seçeneğini destekler.|
| minDateUtc| **Isteğe bağlı** Çevirileri almak istediğiniz tarih. Tarih UTC biçiminde olmalıdır. |
| maxDateUtc| **Isteğe bağlı** Çevirileri almak istediğinize kadar olan tarih. Tarih UTC biçiminde olmalıdır. |
| Atla| **Isteğe bağlı** Bir sayfada atlamak istediğiniz sonuç sayısı. Örneğin, bu parametre için sonuçların ilk 20 satırını atla ve 21. sonuç kaydından göster ' i istiyorsanız, bu parametre için 20 belirtin. Bu parametre için varsayılan değer 0 ' dır.|
| take | **Isteğe bağlı** Almak istediğiniz sonuç sayısı. Her istek için maksimum sayı 100 ' dir. Varsayılan değer 100'dür.|

> [!NOTE]
> Atlama ve alma isteği parametreleri çok sayıda sonuç kaydı için sayfalandırmayı etkinleştirir.

**Dönüş değeri**

Sonuç kümesi **Usertranslationcount**dizisini içeriyor. Her UserTranslationCount aşağıdaki öğelere sahiptir:

| Alan | Açıklama |
|:---|:---|
| Count| Alınan sonuçların sayısı|
| Kaynak | Kaynak dili|
| Derecelendirme| AddTranslation () yöntem çağrısında gönderenin tarafından uygulanan derecelendirme|
| Amaç| Hedef dil|
| Kullanılmamışsa| AddTranslation () yöntem çağrısında uygulanan URI|
| Kullanıcı| Kullanıcı adı|

**Özel durumlar**

| Özel durum | İleti | Koşullar |
|:---|:---|:---|
| ArgumentOutOfRangeException | '**Maxdateutc**' parametresi '**mindateutc**' değerinden büyük veya buna eşit olmalıdır.| **Maxdateutc** parametresinin değeri **mindateutc**parametresinin değerinden daha küçük.|
| TranslateApiException | IP, kotanın üzerinde.| <ul><li>Dakika başına istek sayısı sınırına ulaşıldı.</li><li>İstek boyutu 10000 karakterle sınırlı kalır.</li><li>Saatlik ve günlük kota sınırı, çeviricisinin kabul edeceği karakter sayısını.</li></ul>|
| TranslateApiException | AppID, kotanın üzerinde.| Uygulama KIMLIĞI saatlik veya günlük kotayı aştı.|

> [!NOTE]
> Kota, hizmetin tüm kullanıcıları arasında eşitliği sağlamak için ayarlanacak.

**GitHub 'daki kod örneklerini görüntüle**
* [C#](https://github.com/MicrosoftTranslator/CustomTranslator-API-CSharp)
* [PHP](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-PHP)

## <a name="getusertranslations-method"></a>Getuserçeviriler yöntemi

Bu yöntem, Kullanıcı tarafından oluşturulan çevirileri alır. Bu, uriPrefix, from, to, User ve Minderecelendirme ve Maxderecelendirme istek parametrelerine göre gruplanmış çevirileri sağlar.

**Syntax**

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
| appId | **Gerekli** Yetkilendirme üst bilgisi kullanılırsa, AppID alanını boş bırakın başka bir "taşıyıcı" + "" + erişim belirteci içeren bir dize belirtin.|
| Uriön Eki| **Isteğe bağlı** Çeviri URI 'sinin önekini içeren bir dize.|
| Kaynak| **Isteğe bağlı** Çeviri metninin dil kodunu temsil eden bir dize.|
| şöyle değiştirin:| **Isteğe bağlı** Metnin çevrilmesi için dil kodunu temsil eden bir dize.|
| Minderecelendirme| **Isteğe bağlı** Çevrilmiş metin için en düşük kalite derecelendirmesini temsil eden bir tamsayı değeri. Geçerli değer-10 ile 10 arasındadır. Varsayılan değer 1’dir.|
| Maxderecelendirme| **Isteğe bağlı** Çevrilmiş metin için maksimum kalite derecelendirmesini temsil eden bir tamsayı değeri. Geçerli değer-10 ile 10 arasındadır. Varsayılan değer 1’dir.|
| kullanıcı| **Seçim. Gönderimi oluşturana göre sonucu filtrelemek için kullanılan bir dize**|
| category| **Isteğe bağlı** Çevirinin kategorisini veya etki alanını içeren bir dize. Bu parametre yalnızca genel varsayılan seçeneğini destekler.|
| minDateUtc| **Isteğe bağlı** Çevirileri almak istediğiniz tarih. Tarih UTC biçiminde olmalıdır.|
| maxDateUtc| **Isteğe bağlı** Çevirileri almak istediğinize kadar olan tarih. Tarih UTC biçiminde olmalıdır.|
| Atla| **Isteğe bağlı** Bir sayfada atlamak istediğiniz sonuç sayısı. Örneğin, bu parametre için sonuçların ilk 20 satırını atla ve 21. sonuç kaydından göster ' i istiyorsanız, bu parametre için 20 belirtin. Bu parametre için varsayılan değer 0 ' dır.|
| take| **Isteğe bağlı** Almak istediğiniz sonuç sayısı. Her istek için maksimum sayı 100 ' dir. Varsayılan değer 50 ' dir.|

> [!NOTE]
> Atlama ve alma isteği parametreleri çok sayıda sonuç kaydı için sayfalandırmayı etkinleştirir.

**Dönüş değeri**

Sonuç kümesi **Usertranslation**dizisini içerir. Her UserTranslation aşağıdaki öğelere sahiptir:

| Alan | Açıklama |
|:---|:---|
| CreatedDateUtc| AddTranslation () kullanılarak girişin oluşturulma tarihi|
| Kaynak| Kaynak dili|
| OriginalText| İstek gönderilirken kullanılan kaynak dili metni|
|Derecelendirme |AddTranslation () yöntem çağrısında gönderenin tarafından uygulanan derecelendirme|
|Amaç|    Hedef dil|
|TranslatedText|    AddTranslation () yöntem çağrısında gönderildiği şekilde çeviri|
|Kullanılmamışsa|   AddTranslation () yöntem çağrısında uygulanan URI|
|Kullanıcı   |Kullanıcı adı|

**Özel durumlar**

| Özel durum | İleti | Koşullar |
|:---|:---|:---|
| ArgumentOutOfRangeException | '**Maxdateutc**' parametresi '**mindateutc**' değerinden büyük veya buna eşit olmalıdır.| **Maxdateutc** parametresinin değeri **mindateutc**parametresinin değerinden daha küçük.|
| TranslateApiException | IP, kotanın üzerinde.| <ul><li>Dakika başına istek sayısı sınırına ulaşıldı.</li><li>İstek boyutu 10000 karakterle sınırlı kalır.</li><li>Saatlik ve günlük kota sınırı, çeviricisinin kabul edeceği karakter sayısını.</li></ul>|
| TranslateApiException | AppID, kotanın üzerinde.| Uygulama KIMLIĞI saatlik veya günlük kotayı aştı.|

> [!NOTE]
> Kota, hizmetin tüm kullanıcıları arasında eşitliği sağlamak için ayarlanacak.

**Githıb 'teki kod örneklerini görüntüle**
* [C#](https://github.com/MicrosoftTranslator/CustomTranslator-API-CSharp)
* [PHP](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-PHP)
