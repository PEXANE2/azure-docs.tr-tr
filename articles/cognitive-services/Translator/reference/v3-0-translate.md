---
title: Çevirmen Metin API Çevirme Yöntemi
titleSuffix: Azure Cognitive Services
description: Metni çevirmek için Azure Bilişsel Hizmetler Çevirmeni Metin API Çevirisi yönteminin parametrelerini, üstbilgilerini ve gövde mesajlarını anlayın.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 03/20/2020
ms.author: swmachan
ms.openlocfilehash: 1821623fbe2a22234af649934ac06e72897a19cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80052406"
---
# <a name="translator-text-api-30-translate"></a>Çevirmen Metin API 3.0: Çeviri

Metni çevirir.

## <a name="request-url"></a>İstek URL'si

Bir `POST` istek gönderin:

```HTTP
https://api.cognitive.microsofttranslator.com/translate?api-version=3.0
```

## <a name="request-parameters"></a>İstek parametreleri

Sorgu dizesinde geçirilen istek parametreleri şunlardır:

### <a name="required-parameters"></a>Gerekli parametreler

<table width="100%">
  <th width="20%">Sorgu parametresi</th>
  <th>Açıklama</th>
  <tr>
    <td>api-sürümü</td>
    <td><em>Gerekli parametre</em>.<br/>İstemci tarafından istenen API sürümü. Değer. <code>3.0</code></td>
  </tr>
  <tr>
    <td>-</td>
    <td><em>Gerekli parametre</em>.<br/>Çıktı metninin dilini belirtir. Hedef <code>translation</code> dil, kapsamda bulunan <a href="./v3-0-languages.md">desteklenen dillerden</a> biri olmalıdır. Örneğin, Almanca'ya çevirmek için kullanın. <code>to=de</code><br/>Sorgu dizesinde parametreyi yineleyerek aynı anda birden çok dile çevirmek mümkündür. Örneğin, Almanca <code>to=de&to=it</code> ve İtalyanca çevirmek için kullanın.</td>
  </tr>
</table>

### <a name="optional-parameters"></a>İsteğe bağlı parametreler

<table width="100%">
  <th width="20%">Sorgu parametresi</th>
  <th>Açıklama</th>
  <tr>
    <td>Kaynak</td>
    <td><em>İsteğe bağlı parametre</em>.<br/>Giriş metninin dilini belirtir. Kapsamı kullanarak <a href="./v3-0-languages.md">desteklenen dillere</a> bakarak hangi dillerin çevrilebilir olduğunu bulun. <code>translation</code> <code>from</code> Parametre belirtilmemişse, kaynak dili belirlemek için otomatik dil algılama uygulanır. <br/><br/><a href="https://docs.microsoft.com/azure/cognitive-services/translator/dynamic-dictionary">Dinamik sözlük</a> <code>from</code> özelliğini kullanırken otomatik algılama yerine parametreyi kullanmanız gerekir.</td>
  </tr>  
  <tr>
    <td>textType</td>
    <td><em>İsteğe bağlı parametre</em>.<br/>Çevrilen metnin düz metin mi yoksa HTML metni mi olduğunu tanımlar. Herhangi bir HTML iyi biçimlendirilmiş, tam bir öğe olması gerekir. Olası değerler <code>plain</code> şunlardır: <code>html</code>(varsayılan) veya .</td>
  </tr>
  <tr>
    <td>category</td>
    <td><em>İsteğe bağlı parametre</em>.<br/>Çevirinin kategorisini (etki alanını) belirten bir dize. Bu parametre, <a href="../customization.md">Özel Çevirmen</a>ile oluşturulmuş özelleştirilmiş bir sistemden çeviri almak için kullanılır. Dağıtılan özelleştirilmiş sisteminizi kullanmak için Bu parametreye Özel Çevirmen <a href="https://docs.microsoft.com/azure/cognitive-services/translator/custom-translator/how-to-create-project#view-project-details">proje ayrıntılarınızdaki</a> Kategori Kimliği'ni ekleyin. Varsayılan değer: <code>general</code>.</td>
  </tr>
  <tr>
    <td>küfürEylem</td>
    <td><em>İsteğe bağlı parametre</em>.<br/>Çevirilerde küfürlerin nasıl ele alınması gerektiğini belirtir. Olası değerler <code>NoAction</code> şunlardır: <code>Marked</code> <code>Deleted</code>(varsayılan) veya . Küfür tedavisinde yolları anlamak için, <a href="#handle-profanity">Küfür işleme</a>bakın.</td>
  </tr>
  <tr>
    <td>küfürMarker</td>
    <td><em>İsteğe bağlı parametre</em>.<br/>Çevirilerde küfürlerin nasıl işaretleneceğini belirtir. Olası değerler <code>Asterisk</code> şunlardır: <code>Tag</code>(varsayılan) veya . Küfür tedavisinde yolları anlamak için, <a href="#handle-profanity">Küfür işleme</a>bakın.</td>
  </tr>
  <tr>
    <td>includeAlignment</td>
    <td><em>İsteğe bağlı parametre</em>.<br/>Kaynak metinden çevrilmiş metne hizalama projeksiyonu eklenip eklenmeyeceğini belirtir. Olası değerler <code>true</code> şunlardır: veya <code>false</code> (varsayılan). </td>
  </tr>
  <tr>
    <td>includeSentenceLength</td>
    <td><em>İsteğe bağlı parametre</em>.<br/>Giriş metni ve çevrilen metin için cümle sınırları nın eklenip eklenmeyeceğini belirtir. Olası değerler <code>true</code> şunlardır: veya <code>false</code> (varsayılan).</td>
  </tr>
  <tr>
    <td>önerilenFrom</td>
    <td><em>İsteğe bağlı parametre</em>.<br/>Giriş metninin dili tanımlanamazsa bir geri dönüş dili belirtir. <code>from</code> Parametre atlandığında dil otomatik algılama uygulanır. Algılama başarısız olursa, <code>suggestedFrom</code> dil kabul edilecektir.</td>
  </tr>
  <tr>
    <td>fromScript</td>
    <td><em>İsteğe bağlı parametre</em>.<br/>Giriş metninin komut dosyasını belirtir.</td>
  </tr>
  <tr>
    <td>toScript</td>
    <td><em>İsteğe bağlı parametre</em>.<br/>Çevrilen metnin komut dosyasını belirtir.</td>
  </tr>
  <tr>
    <td>izinFallback</td>
    <td><em>İsteğe bağlı parametre</em>.<br/>Özel bir sistem olmadığında hizmetin genel bir sisteme geri dönmesine izin verildiğini belirtir. Olası değerler <code>true</code> şunlardır: <code>false</code>(varsayılan) veya .<br/><br/><code>allowFallback=false</code>çevirinin yalnızca istekte <code>category</code> belirtilen sistemler kullanması gerektiğini belirtir. X dilinden Y diline çeviri pivot dil E üzerinden zincirleme gerektiriyorsa, zincirdeki tüm sistemlerin (X->E ve E->Y) özel olması ve aynı kategoriye sahip olması gerekir. Belirli bir kategoride sistem bulunmazsa, istek 400 durum kodunu döndürecek. <code>allowFallback=true</code>özel bir sistem olmadığında hizmetin genel bir sisteme geri dönmesine izin verildiğini belirtir.
</td>
  </tr>
</table> 

İstek üstbilgisi şunları içerir:

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
  <tr>
    <td>Kimlik doğrulama üstbilgisi(lar)</td>
    <td><em>Gerekli istek üstbilgi</em>.<br/><a href="https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication">Kimlik doğrulama için kullanılabilir seçeneklere</a>bakın.</td>
  </tr>
  <tr>
    <td>İçerik Türü</td>
    <td><em>Gerekli istek üstbilgi</em>.<br/>Yükün içerik türünü belirtir.<br/> Kabul edilen <code>application/json; charset=UTF-8</code>değer.</td>
  </tr>
  <tr>
    <td>İçerik Uzunluğu</td>
    <td><em>Gerekli istek üstbilgi</em>.<br/>İstek gövdesinin uzunluğu.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td><em>İsteğe bağlı</em>.<br/>İsteği benzersiz olarak tanımlamak için istemci tarafından oluşturulan GUID. 'li bir sorgu parametresi <code>ClientTraceId</code>kullanarak sorgu dizesinde izleme kimliği eklerseniz, bu üstbilginin atlayabilirsiniz.</td>
  </tr>
</table> 

## <a name="request-body"></a>İstek gövdesi

İsteğin gövdesi bir JSON dizisidir. Her dizi öğesi, çevrilecek dizeyi `Text`temsil eden bir dize özelliğine sahip bir JSON nesnesidir.

```json
[
    {"Text":"I would really like to drive your car around the block a few times."}
]
```

Aşağıdaki sınırlamalar geçerlidir:

* Dizi en fazla 100 öğeye sahip olabilir.
* İsteğe dahil edilen metnin tamamı boşluklar da dahil olmak üzere 5.000 karakteri geçemez.

## <a name="response-body"></a>Yanıt gövdesi

Başarılı bir yanıt, giriş dizisindeki her dize için bir sonucu olan bir JSON dizisidir. Sonuç nesnesi aşağıdaki özellikleri içerir:

  * `detectedLanguage`: Algılanan dili aşağıdaki özelliklerle açıklayan bir nesne:

      * `language`: Algılanan dilin kodunu temsil eden bir dize.

      * `score`: Sonuca olan güveni gösteren bir float değeri. Skor sıfır ve bir arasında dır ve düşük bir puan düşük bir güven gösterir.

    Özellik `detectedLanguage` yalnızca dil otomatik algılama istendiğinde sonuç nesnesinde bulunur.

  * `translations`: Bir dizi çeviri sonucu. Dizinin boyutu, sorgu parametresi aracılığıyla belirtilen `to` hedef dillerin sayısıyla eşleşir. Dizideki her öğe şunları içerir:

    * `to`: Hedef dilin dil kodunu temsil eden bir dize.

    * `text`: Çevrilmiş metni veren bir dize.

    * `transliteration`: `toScript` Parametre tarafından belirtilen komut dosyasında çevrilen metni veren nesne.

      * `script`: Hedef komut dosyasını belirten bir dize.   

      * `text`: Hedef komut dosyasında çevrilen metni veren bir dize.

    Çeviri `transliteration` gerçekleşmezse nesne dahil edilmez.

    * `alignment`: Çevrilmiş metne giriş `proj`metnini eşleyen , tek bir dize özelliğine sahip bir nesne. Hizalama bilgileri yalnızca istek `includeAlignment` parametresi `true`. Hizalama, aşağıdaki biçimin dize `[[SourceTextStartIndex]:[SourceTextEndIndex]–[TgtTextStartIndex]:[TgtTextEndIndex]]`değeri olarak döndürülür: .  Üst üste başlangıç ve bitiş dizini ayırır, tire dilleri ayırır ve boşluk sözcükleri ayırır. Bir sözcük diğer dilde sıfır, bir veya birden çok sözcük ile hizalanabilir ve hizalanmış sözcükler bitişik olmayabilir. Hizalama bilgisi olmadığında, hizalama öğesi boş olur. Bkz. Bir örnek ve kısıtlamalar için [hizalama bilgileri edinin.](#obtain-alignment-information)

    * `sentLen`: Giriş ve çıktı metinlerinde cümle sınırlarını döndüren nesne.

      * `srcSentLen`: Giriş metnindeki cümlelerin uzunluklarını temsil eden bir toplam dizi. Dizinin uzunluğu cümle sayısıdır ve değerler her cümlenin uzunluğudur.

      * `transSentLen`: Çevrilmiş metindeki cümlelerin uzunluklarını temsil eden bir tamsayı dizisi. Dizinin uzunluğu cümle sayısıdır ve değerler her cümlenin uzunluğudur.

    Cümle sınırları yalnızca istek parametresi `includeSentenceLength` . `true`

  * `sourceText`: Kaynak dilin varsayılan `text`komut dosyasında giriş metni veren , adlı tek bir dize özelliğine sahip bir nesne. `sourceText`özelliği yalnızca giriş, diliçin olağan komut dosyası olmayan bir komut dosyasında ifade edildiğinde bulunur. Örneğin, giriş Latin alfabesinde Yazılmış Arapça `sourceText.text` ise, arap alfabesine dönüştürülmüş aynı Arapça metin olurdu.

[JSON](#examples) yanıtları örneği örnekler bölümünde verilmiştir.

## <a name="response-headers"></a>Yanıt üst bilgileri

<table width="100%">
  <th width="20%">Üst bilgiler</th>
  <th>Açıklama</th>
    <tr>
    <td>X-RequestId</td>
    <td>İsteği tanımlamak için hizmet tarafından oluşturulan değer. Sorun giderme amacıyla kullanılır.</td>
  </tr>
  <tr>
    <td>X-MT Sistemi</td>
    <td>Çeviri için istenen her 'to' dili için çeviri için kullanılan sistem türünü belirtir. Değer, virgülle ayrılmış dizeler listesidir. Her dize bir tür gösterir:<br/><ul><li>Özel - İstek özel bir sistem içerir ve çeviri sırasında en az bir özel sistem kullanılmıştır.</li><li>Takım - Diğer tüm istekler</li></td>
  </tr>
</table> 

## <a name="response-status-codes"></a>Yanıt durum kodları

Aşağıda, bir isteğin döndürdettiği olası HTTP durum kodları vereme olasılığı ve 

<table width="100%">
  <th width="20%">Durum Kodu</th>
  <th>Açıklama</th>
  <tr>
    <td>200</td>
    <td>Başarılı.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>Sorgu parametrelerinden biri eksik veya geçersiz. Yeniden denemeden önce istek parametrelerini düzeltin.</td>
  </tr>
  <tr>
    <td>401</td>
    <td>İstek doğrulanamadı. Kimlik bilgilerinin belirtilmiş ve geçerli olup olmadığını denetleyin.</td>
  </tr>
  <tr>
    <td>403</td>
    <td>İstek yetkili değil. Ayrıntılar hata iletisini denetleyin. Bu genellikle deneme aboneliği ile sağlanan tüm ücretsiz çevirilerin kullanıldığını gösterir.</td>
  </tr>
  <tr>
    <td>408</td>
    <td>Kaynak eksik olduğundan istek yerine getirilemedi. Ayrıntılar hata iletisini denetleyin. Bir özel <code>category</code>kullanırken, bu genellikle özel çeviri sisteminin isteklere hizmet etmek için henüz kullanılmadığını gösterir. İstek bekleme süresinden sonra (örn. 1 dakika) yeniden denenmelidir.</td>
  </tr>
  <tr>
    <td>429</td>
    <td>İstemci istek sınırlarını aştığı için sunucu isteği reddetti.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Beklenmeyen bir hata oluştu. Hata devam ederse, bunu şu şekilde bildirin: hatanın tarih ve saati, <code>X-RequestId</code>yanıt üstbilgisinden identifier <code>X-ClientTraceId</code>ve istek üstbilgisinden istemci tanımlayıcısı.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>Sunucu geçici olarak kullanılamıyor. İsteği yeniden deneyin. Hata devam ederse, bunu şu şekilde bildirin: hatanın tarih ve saati, <code>X-RequestId</code>yanıt üstbilgisinden identifier <code>X-ClientTraceId</code>ve istek üstbilgisinden istemci tanımlayıcısı.</td>
  </tr>
</table> 

Bir hata oluşursa, istek de bir JSON hata yanıtı döndürecek. Hata kodu, hatayı daha fazla kategorilere ayırmak için 3 basamaklı HTTP durum kodunu ve ardından 3 basamaklı bir sayıyı birleştiren 6 basamaklı bir sayıdır. Yaygın hata kodları [v3 Translator Text API başvuru sayfasında](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#errors)bulunabilir. 

## <a name="examples"></a>Örnekler

### <a name="translate-a-single-input"></a>Tek bir girişi çevirme

Bu örnek, tek bir cümlenin İngilizce'den Basitleştirilmiş Çince'ye nasıl çevrileceklerini gösterir.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'Hello, what is your name?'}]"
```

Yanıt gövdesi:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"}
        ]
    }
]
```

Dizi, `translations` girişteki tek metin parçasının çevirisini sağlayan bir öğe içerir.

### <a name="translate-a-single-input-with-language-auto-detection"></a>Dil otomatik algılama ile tek bir girişi çevirme

Bu örnek, tek bir cümlenin İngilizce'den Basitleştirilmiş Çince'ye nasıl çevrileceklerini gösterir. İstek giriş dilini belirtmez. Bunun yerine kaynak dilin otomatik algılaması kullanılır.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'Hello, what is your name?'}]"
```

Yanıt gövdesi:

```
[
    {
        "detectedLanguage": {"language": "en", "score": 1.0},
        "translations":[
            {"text": "你好, 你叫什么名字？", "to": "zh-Hans"}
        ]
    }
]
```
Yanıt, önceki örnekteki yanıta benzer. Dil otomatik algılama istendiğinden, yanıt, giriş metni için algılanan dil hakkında da bilgi içerir. 

### <a name="translate-with-transliteration"></a>Çeviri ile çevir

Çeviri ekleyerek önceki örneği genişletelim. Aşağıdaki istek, Latince alfabeyle yazılmış bir Çince çeviri ister.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=zh-Hans&toScript=Latn" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'Hello, what is your name?'}]"
```

Yanıt gövdesi:

```
[
    {
        "detectedLanguage":{"language":"en","score":1.0},
        "translations":[
            {
                "text":"你好, 你叫什么名字？",
                "transliteration":{"script":"Latn", "text":"nǐ hǎo , nǐ jiào shén me míng zì ？"},
                "to":"zh-Hans"
            }
        ]
    }
]
```

Çeviri sonucu şimdi `transliteration` Latince karakterler kullanarak çevrilmiş metin veren bir özellik içerir.

### <a name="translate-multiple-pieces-of-text"></a>Birden çok metin parçasını çevirme

Aynı anda birden çok dize çevirmek, istek gövdesinde bir dizi dize belirtme meselesidir.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'Hello, what is your name?'}, {'Text':'I am fine, thank you.'}]"
```

Yanıt gövdesi:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"}
        ]
    },            
    {
        "translations":[
            {"text":"我很好，谢谢你。","to":"zh-Hans"}
        ]
    }
]
```

### <a name="translate-to-multiple-languages"></a>Birden çok dile çeviri yapın

Bu örnek, aynı girdinin tek bir istekte birden çok dile nasıl çevrilyiş yapılacağını gösterir.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans&to=de" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'Hello, what is your name?'}]"
```

Yanıt gövdesi:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"},
            {"text":"Hallo, was ist dein Name?","to":"de"}
        ]
    }
]
```

### <a name="handle-profanity"></a>Küfür sapla

Normalde Çevirmen hizmeti, çeviride kaynakta bulunan küfürleri saklar. Küfür derecesi ve kelimeleri küfür yapan bağlam kültürler arasında farklılık gösterir ve sonuç olarak hedef dildeki küfür derecesi yükseltilebilir veya azaltılabilir.

Eğer çeviride küfür almaktan kaçınmak istiyorsanız, kaynak metinde küfür varlığından bağımsız olarak, küfür filtreleme seçeneğini kullanabilirsiniz. Bu seçenek, küfürlerin silindiğini görmek isteyip istemediğinizi, küfürleri uygun etiketlerle işaretlemek isteyip istemediğinizi (size kendi işlem sonrası ekleme seçeneği vererek) veya hiçbir işlemin yapılmadığını seçmenize olanak tanır. Kabul edilen değerler `ProfanityAction` `Deleted`ve `Marked` `NoAction` (varsayılan).

<table width="100%">
  <th width="20%">KüfürEylem</th>
  <th>Eylem</th>
  <tr>
    <td><code>NoAction</code></td>
    <td>Bu varsayılan davranıştır. Küfür kaynaktan hedefe geçer.<br/><br/>
    <strong>Örnek Kaynak (Japonca)</strong>: Japonca<br/>
    <strong>Örnek Çeviri (İngilizce)</strong>: O bir ahmaktır.
    </td>
  </tr>
  <tr>
    <td><code>Deleted</code></td>
    <td>Saygısız kelimeler değiştirilmeden çıktıdan çıkarılır.<br/><br/>
    <strong>Örnek Kaynak (Japonca)</strong>: Japonca<br/>
    <strong>Örnek Çeviri (İngilizce)</strong>: O bir.
    </td>
  </tr>
  <tr>
    <td><code>Marked</code></td>
    <td>Saygısız sözcükler in çıkışta bir işaretçi ile değiştirilir. İşaretçi parametreye <code>ProfanityMarker</code> bağlıdır.<br/><br/>
Çünkü, <code>ProfanityMarker=Asterisk</code>küfürlü sözcüklerin <code>***</code>yerine:<br/>
    <strong>Örnek Kaynak (Japonca)</strong>: Japonca<br/>
    <strong>Örnek Çeviri (İngilizce)</strong>: \* \* \*O bir .<br/><br/>
Için <code>ProfanityMarker=Tag</code>, küfürlü kelimeler XML &lt;etiketleri&gt; küfür &lt;ve /&gt;küfür ile çevrilidir:<br/>
    <strong>Örnek Kaynak (Japonca)</strong>: Japonca<br/>
    <strong>Örnek Çeviri (İngilizce)</strong>: &lt;O&gt;bir küfür&lt;jackass&gt;/ küfür olduğunu .
  </tr>
</table> 

Örnek:

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&profanityAction=Marked" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'This is a freaking good idea.'}]"
```
Şunu döndürür:

```
[
    {
        "translations":[
            {"text":"Das ist eine *** gute Idee.","to":"de"}
        ]
    }
]
```

Şuyla karşılaştırın:

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&profanityAction=Marked&profanityMarker=Tag" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'This is a freaking good idea.'}]"
```

Bu son istek döner:

```
[
    {
        "translations":[
            {"text":"Das ist eine <profanity>verdammt</profanity> gute Idee.","to":"de"}
        ]
    }
]
```

### <a name="translate-content-with-markup-and-decide-whats-translated"></a>İçeriği biçimlendirmeyle çevirin ve neyin çevrileceğine karar verin

HTML sayfasından içerik veya bir XML belgesinden içerik gibi biçimlendirme içeren içeriği niçin çevirmek yaygındır. Etiketlerle içerik `textType=html` çevirirken sorgu parametresi ekleyin. Ayrıca, bazen belirli içeriği çeviriden hariç tutmak yararlı olabilir. Özniteliği, `class=notranslate` özgün dilinde kalması gereken içeriği belirtmek için kullanabilirsiniz. Aşağıdaki örnekte, ilk `div` öğenin içindeki içerik çevrilmezken, ikinci `div` öğedeki içerik çevrilecektir.

```
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

Burada göstermek için örnek bir istektir.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans&textType=html" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'<div class=\"notranslate\">This will not be translated.</div><div>This will be translated.</div>'}]"
```

Yanıt:

```
[
    {
        "translations":[
            {"text":"<div class=\"notranslate\">This will not be translated.</div><div>这将被翻译。</div>","to":"zh-Hans"}
        ]
    }
]
```

### <a name="obtain-alignment-information"></a>Hizalama bilgilerini edinme

Hizalama bilgilerini almak `includeAlignment=true` için sorgu dizesini belirtin.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=fr&includeAlignment=true" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'The answer lies in machine translation.'}]"
```

Yanıt:

```
[
    {
        "translations":[
            {
                "text":"La réponse se trouve dans la traduction automatique.",
                "to":"fr",
                "alignment":{"proj":"0:2-0:1 4:9-3:9 11:14-11:19 16:17-21:24 19:25-40:50 27:37-29:38 38:38-51:51"}
            }
        ]
    }
]
```

Hizalama `0:2-0:1`bilgileri, kaynak metindeki ilk üç karakterin (`The`) çevrilmiş metindeki ilk iki karakterle`La`eşlendiği anlamına gelir.

#### <a name="limitations"></a>Sınırlamalar
Hizalama bilgilerini elde etmek, potansiyel ifade eşlemeleri ile araştırma ve deneyimlerin prototiplemesi için sağladığımız deneysel bir özelliktir. Gelecekte bunu desteklemeyi bırakmayı seçebiliriz. Hizalamaların desteklenmediği önemli kısıtlamalardan bazıları şunlardır:

* Hizalama HTML formatında metin için kullanılabilir değil yani, textType=html
* Hizalama yalnızca dil çiftlerinin bir alt kümesi için döndürülür:
  - İngilizce'den başka bir dile;
  - Çince Basitleştirilmiş, Geleneksel Çince ve Letonca'dan İngilizce'ye başka bir dilden İngilizce'ye;
  - Japonca'dan Korece'ye ya da Korece'den Japonca'ya.
* Cümle konserve bir çeviriyse hizalama almazsınız. Konserve çeviri örneği "Bu bir test", "Seni seviyorum" ve diğer yüksek frekanslı cümlelerdir.
* [Burada](../prevent-translation.md) açıklandığı gibi çeviriyi önlemek için yaklaşımlardan herhangi birini uyguladığınız da hizalama kullanılamaz

### <a name="obtain-sentence-boundaries"></a>Cümle sınırlarını edinme

Kaynak metindeki ve çevrilmiş metindeki cümle uzunluğu `includeSentenceLength=true` hakkında bilgi almak için sorgu dizesini belirtin.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=fr&includeSentenceLength=true" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'The answer lies in machine translation. The best machine translation technology cannot always provide translations tailored to a site or users like a human. Simply copy and paste a code snippet anywhere.'}]"
```

Yanıt:

```
[
    {
        "translations":[
            {
                "text":"La réponse se trouve dans la traduction automatique. La meilleure technologie de traduction automatique ne peut pas toujours fournir des traductions adaptées à un site ou des utilisateurs comme un être humain. Il suffit de copier et coller un extrait de code n’importe où.",
                "to":"fr",
                "sentLen":{"srcSentLen":[40,117,46],"transSentLen":[53,157,62]}
            }
        ]
    }
]
```

### <a name="translate-with-dynamic-dictionary"></a>Dinamik sözlük ile çeviri yapın

Bir sözcüğe veya tümcecikiçin uygulamak istediğiniz çeviriyi zaten biliyorsanız, istek içinde biçimlendirme olarak sağlayabilirsiniz. Dinamik sözlük yalnızca uygun adlar ve ürün adları gibi bileşik adlar için güvenlidir.

Tedarik etmek için biçimlendirme aşağıdaki sözdizimini kullanır.

``` 
<mstrans:dictionary translation="translation of phrase">phrase</mstrans:dictionary>
```

Örneğin, İngilizce "Wordomatic kelimesi sözlük girişidir" cümlesini göz önünde bulundurun. Çeviride _kelime wordomatic_ korumak için, istek gönderin:

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.'}]"
```

Sonuç şudur:

```
[
    {
        "translations":[
            {"text":"Das Wort \"wordomatic\" ist ein Wörterbucheintrag.","to":"de"}
        ]
    }
]
```

Bu özellik, "" `textType=text` ile `textType=html`veya ' ile aynı şekilde çalışır. Özellik dikkatli kullanılmalıdır. Çeviriözelleştirmenin uygun ve çok daha iyi yolu Özel Çevirmen kullanmaktır. Özel Çevirmen bağlam ve istatistiksel olasılıkları tam olarak kullanır. Çalışmanızı veya ifadenizi bağlam içinde gösteren eğitim verileriniz varsa veya oluşturabiliyorsanız, çok daha iyi sonuçlar alırsınız. [Özel Çevirmen hakkında daha fazla bilgi edinin.](../customization.md)
