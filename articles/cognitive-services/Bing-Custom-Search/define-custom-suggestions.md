---
title: Özel Otomatik Önerme önerilerini tanımlayın - Bing Özel Arama
titleSuffix: Azure Cognitive Services
description: Özel Otomatik Öner, arama deneyiminiz ile alakalı önerilen arama sorgusu dizelerinin bir listesini döndürür.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: maheshb
ms.openlocfilehash: 5b2b8871d868e827532f23c7ef4f14fb00afb5bc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74072796"
---
# <a name="configure-your-custom-autosuggest-experience"></a>Özel otomatik önerme deneyiminizi yapılandırın

Özel Otomatik Öner, arama deneyiminiz ile alakalı önerilen arama sorgusu dizelerinin bir listesini döndürür. Önerilen sorgu dizeleri, kullanıcının arama kutusunda sağladığı kısmi bir sorgu dizesini temel adar. Liste en fazla 10 öneri içerecektir. 

Yalnızca özel önerileri mi döndürecek yoksa Bing önerilerini mi ekleyin. Bing önerilerini eklerseniz, Bing önerilerinden önce özel öneriler görünür. Yeterli sayıda alakalı öneri sağlarsanız, döndürülen öneri listesi Bing önerilerini içermez. Bing önerileri her zaman Özel Arama örneğinizin bağlamındadır. 

Örneğiniz için arama sorgusu önerilerini yapılandırmak için **Otomatik Öner** sekmesini tıklatın.  

> [!NOTE]
> Bu özelliği kullanmak için, uygun düzeyde Özel Arama'ya abone olmalısınız [(fiyatlandırmaya](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)bakın).

Önerilerin hizmet bitiş noktasına (API veya barındırılan Kişisel Arama Arabirimi) yansıtılması 24 saat kadar sürebilir.

## <a name="enable-bing-suggestions"></a>Bing önerilerini etkinleştirme

Bing önerilerini etkinleştirmek için **Otomatik Bing önerilerini** bağlı konuma kaydırın. Kaydırıcı maviye dönüşür.

## <a name="add-your-own-suggestions"></a>Kendi önerilerinizi ekleyin

Kendi sorgu dize önerilerinizi eklemek için, **bunları Kullanıcı tanımlı öneriler**altında listeye ekleyin. Listeye bir öneri ekledikten sonra enter tuşuna **+** basın veya simgeyi tıklatın. Öneriyi herhangi bir dilde belirtebilirsiniz. En fazla 5.000 sorgu dize önerisi ekleyebilirsiniz.

## <a name="upload-suggestions"></a>Öneriler yükle

Seçenek olarak, bir dosyadan önerilerin listesini yükleyebilirsiniz. Dosya, satır başına bir arama sorgu dizesi içermelidir. Dosyayı yüklemek için yükleme simgesini tıklatın ve yüklemek için dosyayı seçin. Hizmet, önerileri dosyadan ayıklar ve listeye ekler.

## <a name="remove-suggestions"></a>Önerileri kaldırma

Sorgu dizeönerisini kaldırmak için kaldırmak istediğiniz önerinin yanındaki kaldır simgesini tıklatın.

## <a name="block-suggestions"></a>Engelleme önerileri

Bing önerilerini eklerseniz, Bing'in dönmesini istemediğiniz arama sorgusu dizelerinin bir listesini ekleyebilirsiniz. Engellenen sorgu dizeleri eklemek için **engellenen önerileri göster'i**tıklatın. Listeye sorgu dizesini ekleyin ve enter tuşuna basın veya simgeyi **+** tıklatın. En fazla 50 engellenen sorgu dizeleri ekleyebilirsiniz.



[!INCLUDE [publish or revert](./includes/publish-revert.md)]

>[!NOTE]  
>Özel Otomatik Önerme yapılandırma değişikliklerinin etkili olması 24 saat kadar sürebilir.


## <a name="enabling-autosuggest-in-hosted-ui"></a>Barındırılan Web'de Otomatik Önerme'yi etkinleştirme

Barındırılan UI'niz için sorgu dize önerileri etkinleştirmek için **Barındırılan UI'yi**tıklatın. **Ek Yapılandırma** bölümüne gidin. **Web araması**altında, **Etkinleştir için On'u seçin.** **On** Otomatik Öner'i etkinleştirmek için, arama kutusu içeren bir düzen seçmeniz gerekir.


## <a name="calling-the-autosuggest-api"></a>Otomatik Öner API'yi arama

Bing Özel Arama API'sini kullanarak önerilen sorgu `GET` dizelerini almak için aşağıdaki bitiş noktasına bir istek gönderin.

```
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions 
```

Yanıt, önerilen sorgu `SearchAction` dizeleri içeren nesnelerin listesini içerir.

```
        {  
            "displayText" : "sailing lessons seattle",  
            "query" : "sailing lessons seattle",  
            "searchKind" : "CustomSearch"  
        },  
```

Her öneri `displayText` bir `query` ve alan içerir. Alan, `displayText` arama kutunuzun açılır listesinin doldurulması için kullandığınız önerilen sorgu dizesini içerir.

Kullanıcı açılır listeden önerilen bir sorgu dizesi seçerse, `query` [Bing Özel Arama API'sını](overview.md)ararken alandaki sorgu dizesini kullanın.


## <a name="next-steps"></a>Sonraki adımlar

- [Özel öneriler alma](./get-custom-suggestions.md)
- [Özel örneğini arama](./search-your-custom-view.md)
- [Özel barındırılan kullanıcı aynasyonlarını yapılandırma ve kullanma](./hosted-ui.md)
