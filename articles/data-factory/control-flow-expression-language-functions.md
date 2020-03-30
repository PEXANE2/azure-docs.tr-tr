---
title: Azure Veri Fabrikası'nda ifade ve işlevler
description: Bu makalede, veri fabrikası varlıkları oluştururken kullanabileceğiniz ifadeler ve işlevler hakkında bilgi sağlar.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 11/25/2019
ms.openlocfilehash: 9ef4b569fd8413d2825374c963fb272dd450cf0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74533130"
---
# <a name="expressions-and-functions-in-azure-data-factory"></a>Azure Data Factory’deki ifadeler ve işlevler

> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-functions-variables.md)
> * [Geçerli sürüm](control-flow-expression-language-functions.md)

Bu makalede, Azure Veri Fabrikası tarafından desteklenen ifadeler ve işlevler hakkında ayrıntılar verilmektedir. 

## <a name="expressions"></a>İfadeler

Tanımdaki JSON değerleri çalışma zamanında değerlendirilen gerçek veya ifadeler olabilir. Örnek:  
  
```json
"name": "value"
```

 or  
  
```json
"name": "@pipeline().parameters.password"
```

İfadeler json dize değeri herhangi bir yerde görünebilir ve her zaman başka bir JSON değeri neden. JSON değeri bir ifade ise, ifadenin gövdesi at-işareti kaldırılarak ayıklanır (\@). Ile başlayan bir edebi dize \@gerekiyorsa, kullanılarak \@ \@kaçmış olmalıdır. Aşağıdaki örnekler ifadelerin nasıl değerlendirildiğini göstermektedir.  
  
|JSON değeri|Sonuç|  
|----------------|------------|  
|"parametreler"|'Parametreler' karakterleri döndürülür.|  
|"parametreler[1]"|'parametreler[1]' karakterleri döndürülür.|  
|"\@\@"|' ' '\@içeren 1 karakterli dize döndürülür.|  
|" \@"|' ' ' \@içeren 2 karakterli dize döndürülür.|  
  
 İfadeler, ifadelerin `@{ ... }`''e sarılmış olduğu string *enterpolasyonu* adlı bir özelliği kullanarak dizelerin içinde de görünebilir. Örneğin, `"name" : "First Name: @{pipeline().parameters.firstName} Last Name: @{pipeline().parameters.lastName}"`  
  
 String enterpolasyon kullanarak, sonuç her zaman bir dize. Ben `myNumber` olarak `42` tanımlamış `myString` `foo`ve say:  
  
|JSON değeri|Sonuç|  
|----------------|------------|  
|"\@pipeline().parameters.myString"| Dize olarak döner. `foo`|  
|"\@{pipeline().parameters.myString}"| Dize olarak döner. `foo`|  
|"\@pipeline().parameters.myNumber"| Sayı `42` olarak *döndürür.*|  
|"\@{pipeline().parameters.myNumber}"| Dize olarak `42` *döndürür.*|  
|"Cevap: @{pipeline().parameters.myNumber}"| Dizeyi `Answer is: 42`döndürür.|  
|"\@concat('Cevap: ', string(pipeline().parameters.myNumber))"| Dizeyi döndürür`Answer is: 42`|  
|"Cevap: \@ \@{pipeline().parameters.myNumber}"| Dizeyi `Answer is: @{pipeline().parameters.myNumber}`döndürür.|  
  
## <a name="examples"></a>Örnekler

### <a name="a-dataset-with-a-parameter"></a>Parametreli bir veri kümesi
Aşağıdaki örnekte, BlobDataset **yol**adlı bir parametre alır. Değeri, aşağıdaki ifadeyi kullanarak **folderPath** özelliği için bir `dataset().path`değer ayarlamak için kullanılır: . 

```json
{
    "name": "BlobDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "@dataset().path"
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "parameters": {
            "path": {
                "type": "String"
            }
        }
    }
}
```

### <a name="a-pipeline-with-a-parameter"></a>Parametreli bir boru hattı
Aşağıdaki örnekte, ardışık hatlar **inputPath** ve **outputPath** parametrelerini alır. Parametreli blob veri kümesinin **yolu** bu parametrelerin değerleri kullanılarak ayarlanır. Burada kullanılan sözdizimi: `pipeline().parameters.parametername`. 

```json
{
    "name": "Adfv2QuickStartPipeline",
    "properties": {
        "activities": [
            {
                "name": "CopyFromBlobToBlob",
                "type": "Copy",
                "inputs": [
                    {
                        "referenceName": "BlobDataset",
                        "parameters": {
                            "path": "@pipeline().parameters.inputPath"
                        },
                        "type": "DatasetReference"
                    }
                ],
                "outputs": [
                    {
                        "referenceName": "BlobDataset",
                        "parameters": {
                            "path": "@pipeline().parameters.outputPath"
                        },
                        "type": "DatasetReference"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                }
            }
        ],
        "parameters": {
            "inputPath": {
                "type": "String"
            },
            "outputPath": {
                "type": "String"
            }
        }
    }
}
```
### <a name="tutorial"></a>Öğretici
Bu [öğretici,](https://azure.microsoft.com/mediahandler/files/resourcefiles/azure-data-factory-passing-parameters/Azure%20data%20Factory-Whitepaper-PassingParameters.pdf) bir boru hattı ve etkinlik arasında parametrelerin yanı sıra etkinlikler arasında nasıl geçirilebilirsiniz size yol eder.

  
## <a name="functions"></a>İşlevler

İfadeler içindeki işlevleri arayabilirsiniz. Aşağıdaki bölümlerde bir ifadede kullanılabilecek işlevler hakkında bilgi verilmiştir.  

## <a name="string-functions"></a>Dize işlevleri  

Dizeleri ile çalışmak için, bu dize işlevleri ve aynı zamanda bazı [toplama işlevleri](#collection-functions)kullanabilirsiniz.
Dize işlevleri yalnızca dizeleri üzerinde çalışır.

| String fonksiyonu | Görev |
| --------------- | ---- |
| [Concat](control-flow-expression-language-functions.md#concat) | İki veya daha fazla dizebirleştirin ve birleştirilmiş dizeyi döndürün. |
| [Endswith](control-flow-expression-language-functions.md#endswith) | Bir dize belirtilen sub string ile bitene denetleyin. |
| [Guıd](control-flow-expression-language-functions.md#guid) | Dize olarak genel olarak benzersiz bir tanımlayıcı (GUID) oluşturun. |
| [ındexof](control-flow-expression-language-functions.md#indexof) | Bir alt dize için başlangıç konumunu döndürün. |
| [Lastındexof](control-flow-expression-language-functions.md#lastindexof) | Bir alt dizenin son oluşumu için başlangıç konumunu döndürün. |
| [Değiştirmek](control-flow-expression-language-functions.md#replace) | Bir alt dizeyi belirtilen dizeyle değiştirin ve güncelleştirilmiş dizeyi döndürün. |
| [Split](control-flow-expression-language-functions.md#split) | Özgün dizedeki belirtilen sınırlayıcı karaktere dayalı daha büyük bir dizeden virgülle ayrılmış alt dizeleri içeren bir dizi döndürün. |
| [Startswith](control-flow-expression-language-functions.md#startswith) | Bir dize belirli bir alt dize ile başlayıp başlamadığını denetleyin. |
| [Dize](control-flow-expression-language-functions.md#substring) | Belirtilen konumdan başlayarak karakterleri bir dizeden döndürün. |
| [Tolower](control-flow-expression-language-functions.md#toLower) | Küçük harf biçiminde bir dize döndürün. |
| [Toupper](control-flow-expression-language-functions.md#toUpper) | Büyük harf biçiminde bir dize döndürün. |
| [Döşeme](control-flow-expression-language-functions.md#trim) | Satır aralığını ve sondaki beyaz boşluğu bir dizeden kaldırın ve güncelleştirilmiş dizeyi döndürün. |

## <a name="collection-functions"></a>Toplama işlevleri

Koleksiyonlarla, genellikle dizilerle, dizeleri ve bazen sözlüklerle çalışmak için bu koleksiyon işlevlerini kullanabilirsiniz.

| Toplama fonksiyonu | Görev |
| ------------------- | ---- |
| [Içerir](control-flow-expression-language-functions.md#contains) | Koleksiyonun belirli bir öğesi olup olmadığını denetleyin. |
| [empty](control-flow-expression-language-functions.md#empty) | Koleksiyonun boş olup olmadığını kontrol edin. |
| [Ilk](control-flow-expression-language-functions.md#first) | Koleksiyondaki ilk öğeyi döndürün. |
| [Kesişim](control-flow-expression-language-functions.md#intersection) | Belirtilen koleksiyonlar arasında *yalnızca* ortak öğelere sahip bir koleksiyon döndürün. |
| [Katılın](control-flow-expression-language-functions.md#join) | Belirtilen karakterle ayrılmış bir dizideki *tüm* öğeleri içeren bir dize döndürün. |
| [Son](control-flow-expression-language-functions.md#last) | Koleksiyondaki son öğeyi döndürün. |
| [Uzun -luğu](control-flow-expression-language-functions.md#length) | Bir dize veya dizideki öğe sayısını döndürün. |
| [Atlamak](control-flow-expression-language-functions.md#skip) | Öğeleri koleksiyonun önünden kaldırın ve diğer tüm öğeleri iade *edin.* |
| [almak](control-flow-expression-language-functions.md#take) | Öğeleri koleksiyonun önünden döndürün. |
| [Birliği](control-flow-expression-language-functions.md#union) | Belirtilen koleksiyonlardan *tüm* öğeleri içeren bir koleksiyon döndürün. | 

## <a name="logical-functions"></a>Mantıksal işlevler  

Bu işlevler iç koşullar yararlıdır, onlar mantık her türlü değerlendirmek için kullanılabilir.  
  
| Mantıksal karşılaştırma fonksiyonu | Görev |
| --------------------------- | ---- |
| [Ve](control-flow-expression-language-functions.md#and) | Tüm ifadelerin doğru olup olmadığını denetleyin. |
| [equals](control-flow-expression-language-functions.md#equals) | Her iki değerin de eşdeğer olup olmadığını denetleyin. |
| [büyüktür](control-flow-expression-language-functions.md#greater) | İlk değerin ikinci değerden büyük olup olmadığını denetleyin. |
| [greaterOrEquals](control-flow-expression-language-functions.md#greaterOrEquals) | İlk değerin ikinci değerden büyük mü yoksa eşit mi olduğunu denetleyin. |
| [if](control-flow-expression-language-functions.md#if) | Bir ifadenin doğru mu yanlış mı olduğunu kontrol edin. Sonuca bağlı olarak, belirli bir değeri döndürün. |
| [daha az](control-flow-expression-language-functions.md#less) | İlk değerin ikinci değerden küçük olup olmadığını denetleyin. |
| [lessOrEquals](control-flow-expression-language-functions.md#lessOrEquals) | İlk değerin ikinci değerden küçük mü yoksa eşit mi olduğunu denetleyin. |
| [Değil](control-flow-expression-language-functions.md#not) | İfadenin yanlış olup olmadığını kontrol edin. |
| [Veya](control-flow-expression-language-functions.md#or) | En az bir ifadenin doğru olup olmadığını kontrol edin. |
  
## <a name="conversion-functions"></a>Dönüştürme işlevleri  

 Bu işlevler, dildeki her bir yerel türü dönüştürmek için kullanılır:  
-   string
-   integer
-   float
-   boole
-   Dizi
-   Sözlük

| Dönüştürme işlevi | Görev |
| ------------------- | ---- |
| [Dizi](control-flow-expression-language-functions.md#array) | Belirli bir girişten bir dizi döndürün. Birden çok giriş için [createArray'e](control-flow-expression-language-functions.md#createArray)bakın. |
| [base64](control-flow-expression-language-functions.md#base64) | Bir dize için base64 kodlanmış sürümü döndürün. |
| [base64ToBinary](control-flow-expression-language-functions.md#base64ToBinary) | Base64 kodlanmış bir dize için ikili sürümü döndürün. |
| [base64ToString](control-flow-expression-language-functions.md#base64ToString) | Base64 kodlanmış bir dize için dize sürümünü döndürün. |
| [ikili](control-flow-expression-language-functions.md#binary) | Bir giriş değeri için ikili sürümü döndürün. |
| [bool](control-flow-expression-language-functions.md#bool) | Giriş değeri için Boolean sürümünü döndürün. |
| [Coalesce](control-flow-expression-language-functions.md#coalesce) | Bir veya daha fazla parametreden ilk null olmayan değeri döndürün. |
| [createArray](control-flow-expression-language-functions.md#createArray) | Birden çok girişten bir dizi döndürün. |
| [dataUri](control-flow-expression-language-functions.md#dataUri) | Giriş değeri için veri URI'yi döndürün. |
| [dataUritoBinary](control-flow-expression-language-functions.md#dataUriToBinary) | Bir veri URI için ikili sürümü döndürün. |
| [dataUritostring](control-flow-expression-language-functions.md#dataUriToString) | Veri URI için dize sürümünü döndürün. |
| [decodeBase64](control-flow-expression-language-functions.md#decodeBase64) | Base64 kodlanmış bir dize için dize sürümünü döndürün. |
| [decodeDataUri](control-flow-expression-language-functions.md#decodeDataUri) | Bir veri URI için ikili sürümü döndürün. |
| [decodeUriComponent](control-flow-expression-language-functions.md#decodeUriComponent) | Kaçış karakterlerinin yerine deşifre edilmiş sürümlerle bir dize döndürün. |
| [kodlamaUriComponent](control-flow-expression-language-functions.md#encodeUriComponent) | URL'de güvenli olmayan karakterlerin yerine kaçış karakterleri getiren bir dize döndürün. |
| [float](control-flow-expression-language-functions.md#float) | Giriş değeri için kayan nokta numarası döndürün. |
| [int](control-flow-expression-language-functions.md#int) | Tamsayı sürümünü bir dize için döndürün. |
| [Json](control-flow-expression-language-functions.md#json) | Bir dize veya XML için JavaScript Nesne Gösterimi (JSON) türü değerini veya nesnesini döndürün. |
| [Dize](control-flow-expression-language-functions.md#string) | Giriş değeri için dize sürümünü döndürün. |
| [uriComponent](control-flow-expression-language-functions.md#uriComponent) | URL'de güvenli olmayan karakterleri kaçış karakterleriyle değiştirerek URI kodlu sürümü giriş değeri için döndürün. |
| [uriComponentToBinary](control-flow-expression-language-functions.md#uriComponentToBinary) | URI kodlu bir dize için ikili sürümü döndürün. |
| [uriComponentToString](control-flow-expression-language-functions.md#uriComponentToString) | URI kodlanmış bir dize için dize sürümünü döndürün. |
| [Xml](control-flow-expression-language-functions.md#xml) | XML sürümünü bir dize için döndürün. |
| [Xpath](control-flow-expression-language-functions.md#xpath) | XML'yi, XPath (XML Path Language) ifadesiyle eşleşen düğümler veya değerler için denetleyin ve eşleşen düğümleri veya değerleri döndürün. |

## <a name="math-functions"></a>Matematik İşlevleri  
 Bu işlevler her iki sayı türü için de kullanılabilir: **hem de sayacılar** ve **şamandıralar.**  

| Matematik fonksiyonu | Görev |
| ------------- | ---- |
| [add](control-flow-expression-language-functions.md#add) | İki sayı ekleyerek sonucu döndürün. |
| [div](control-flow-expression-language-functions.md#div) | Sonucu iki sayıyı bölerek döndürün. |
| [Max](control-flow-expression-language-functions.md#max) | Bir sayı kümesinden veya diziden en yüksek değeri döndürün. |
| [Dk](control-flow-expression-language-functions.md#min) | Bir sayı kümesinden veya diziden en düşük değeri döndürün. |
| [mod](control-flow-expression-language-functions.md#mod) | Kalan sayıyı iki sayıyı bölerek döndürün. |
| [Mul](control-flow-expression-language-functions.md#mul) | Ürünü iki sayıyı çarparak döndürün. |
| [Rand](control-flow-expression-language-functions.md#rand) | Belirli bir aralıktan rasgele bir tamsayı döndürün. |
| [Aralığı](control-flow-expression-language-functions.md#range) | Belirtilen bir tamsededen başlayan bir tamsayı dizini döndürün. |
| [Alt](control-flow-expression-language-functions.md#sub) | İkinci sayıyı ilk sayıdan çıkarmanın sonucunu döndürün. |
  
## <a name="date-functions"></a>Tarih işlevleri  

| Tarih veya saat fonksiyonu | Görev |
| --------------------- | ---- |
| [Adddays](control-flow-expression-language-functions.md#addDays) | Zaman damgasına birkaç gün ekleyin. |
| [Addhours](control-flow-expression-language-functions.md#addHours) | Zaman damgasına birkaç saat ekleyin. |
| [Addminutes](control-flow-expression-language-functions.md#addMinutes) | Zaman damgasına birkaç dakika ekleyin. |
| [Addseconds](control-flow-expression-language-functions.md#addSeconds) | Zaman damgasına birkaç saniye ekleyin. |
| [addToTime](control-flow-expression-language-functions.md#addToTime) | Zaman damgasına birkaç zaman birimi ekleyin. Ayrıca [bkz.](control-flow-expression-language-functions.md#getFutureTime) |
| [convertFromUtc](control-flow-expression-language-functions.md#convertFromUtc) | Evrensel Zaman Eşgüdümlü'nden (UTC) bir zaman damgasını hedef saat dilimine dönüştürün. |
| [dönüştürmeTimeZone](control-flow-expression-language-functions.md#convertTimeZone) | Kaynak saat diliminden bir zaman damgasını hedef saat dilimine dönüştürün. |
| [convertToUtc](control-flow-expression-language-functions.md#convertToUtc) | Kaynak saat dilimindeki bir zaman damgasını Evrensel Saat Koordinasyonu'na (UTC) dönüştürün. |
| [günOfMonth](control-flow-expression-language-functions.md#dayOfMonth) | Ayın bileşeninin gününü bir zaman damgasından döndürün. |
| [Dayofweek](control-flow-expression-language-functions.md#dayOfWeek) | Hafta bileşeninin gününü bir zaman damgasından döndürün. |
| [Dayofyear](control-flow-expression-language-functions.md#dayOfYear) | Yıl bileşeninin gününü bir zaman damgasından döndürün. |
| [formatDateTime](control-flow-expression-language-functions.md#formatDateTime) | Tarihi bir zaman damgasından döndürün. |
| [getFutureTime](control-flow-expression-language-functions.md#getFutureTime) | Geçerli zaman damgasını ve belirtilen zaman birimlerini döndürün. Ayrıca [bkz.](control-flow-expression-language-functions.md#addToTime) |
| [getPastTime](control-flow-expression-language-functions.md#getPastTime) | Geçerli zaman damgasını eksi belirtilen zaman birimlerini döndürün. Ayrıca [bkz.](control-flow-expression-language-functions.md#subtractFromTime) |
| [başlangıçOfDay](control-flow-expression-language-functions.md#startOfDay) | Günün başlangıcını bir zaman damgası için döndürün. |
| [başlangıçOfSaat](control-flow-expression-language-functions.md#startOfHour) | Saat ini saat damgası için saatin başlangıcını iade edin. |
| [başlangıçOfMonth](control-flow-expression-language-functions.md#startOfMonth) | Zaman damgası için ayın başlangıcını iade edin. |
| [çıkarmaFromTime](control-flow-expression-language-functions.md#subtractFromTime) | Zaman damgasından birkaç zaman birimi çıkarın. Ayrıca [bakınız GetPastTime](control-flow-expression-language-functions.md#getPastTime). |
| [Ticks](control-flow-expression-language-functions.md#ticks) | Belirtilen `ticks` bir zaman damgası için özellik değerini döndürün. |
| [utcŞimdi](control-flow-expression-language-functions.md#utcNow) | Geçerli zaman damgasını dize olarak döndürün. |

## <a name="function-reference"></a>Fonksiyon başvurusu

Bu bölümde, tüm kullanılabilir işlevleri alfabetik sıraya göre listelenmektedir.

<a name="add"></a>

### <a name="add"></a>add

İki sayı ekleyerek sonucu döndürün.

```
add(<summand_1>, <summand_2>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*summand_1*>, <*summand_2*> | Evet | Sonda, Float veya karışık | Eklenecek sayılar |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | -----| ----------- |
| <*sonuç toplamı*> | Sonda veya Float | Belirtilen sayıları eklemesonucu |
||||

*Örnek*

Bu örnekte belirtilen sayılar ekleniyor:

```
add(1, 1.5)
```

Ve bu sonucu döndürür:`2.5`

<a name="addDays"></a>

### <a name="adddays"></a>Adddays

Zaman damgasına birkaç gün ekleyin.

```
addDays('<timestamp>', <days>, '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | Dize | Zaman damgasını içeren dize |
| <*Gün*> | Evet | Tamsayı | Eklenecek olumlu veya negatif gün sayısı |
| <*Biçim*> | Hayır | Dize | Ya tek bir [biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ya da özel bir [biçim deseni.](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) Zaman damgası için varsayılan [biçim, ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ile uyumlu ve saat dilimi bilgilerini koruyan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-MM-ddTHH:mm:ss:fffffffK) biçimidir. |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*güncelleştirilmiş zaman damgası*> | Dize | Zaman damgası artı belirtilen gün sayısı  |
||||

*Örnek 1*

Bu örnek, belirtilen zaman damgasına 10 gün ekler:

```
addDays('2018-03-15T13:00:00Z', 10)
```

Ve bu sonucu döndürür:`"2018-03-25T00:00:0000000Z"`

*Örnek 2*

Bu örnek, belirtilen zaman damgasından beş gün çıkarır:

```
addDays('2018-03-15T00:00:00Z', -5)
```

Ve bu sonucu döndürür:`"2018-03-10T00:00:0000000Z"`

<a name="addHours"></a>

### <a name="addhours"></a>Addhours

Zaman damgasına birkaç saat ekleyin.

```
addHours('<timestamp>', <hours>, '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | Dize | Zaman damgasını içeren dize |
| <*Saat*> | Evet | Tamsayı | Eklenecek pozitif veya negatif saat sayısı |
| <*Biçim*> | Hayır | Dize | Ya tek bir [biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ya da özel bir [biçim deseni.](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) Zaman damgası için varsayılan [biçim, ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ile uyumlu ve saat dilimi bilgilerini koruyan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-MM-ddTHH:mm:ss:fffffffK) biçimidir. |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*güncelleştirilmiş zaman damgası*> | Dize | Zaman damgası artı belirtilen saat sayısı  |
||||

*Örnek 1*

Bu örnek, belirtilen zaman damgasına 10 saat ekler:

```
addHours('2018-03-15T00:00:00Z', 10)
```

Ve bu sonucu döndürür:`"2018-03-15T10:00:0000000Z"`

*Örnek 2*

Bu örnek, belirtilen zaman damgasından beş saat çıkarır:

```
addHours('2018-03-15T15:00:00Z', -5)
```

Ve bu sonucu döndürür:`"2018-03-15T10:00:0000000Z"`

<a name="addMinutes"></a>

### <a name="addminutes"></a>Addminutes

Zaman damgasına birkaç dakika ekleyin.

```
addMinutes('<timestamp>', <minutes>, '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | Dize | Zaman damgasını içeren dize |
| <*Dakika*> | Evet | Tamsayı | Eklenecek dakikaların pozitif veya negatif sayısı |
| <*Biçim*> | Hayır | Dize | Ya tek bir [biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ya da özel bir [biçim deseni.](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) Zaman damgası için varsayılan [biçim, ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ile uyumlu ve saat dilimi bilgilerini koruyan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-MM-ddTHH:mm:ss:fffffffK) biçimidir. |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*güncelleştirilmiş zaman damgası*> | Dize | Zaman damgası artı belirtilen dakika sayısı |
||||

*Örnek 1*

Bu örnek, belirtilen zaman damgasına 10 dakika ekler:

```
addMinutes('2018-03-15T00:10:00Z', 10)
```

Ve bu sonucu döndürür:`"2018-03-15T00:20:00.0000000Z"`

*Örnek 2*

Bu örnek, belirtilen zaman damgasından beş dakika çıkarır:

```
addMinutes('2018-03-15T00:20:00Z', -5)
```

Ve bu sonucu döndürür:`"2018-03-15T00:15:00.0000000Z"`

<a name="addSeconds"></a>

### <a name="addseconds"></a>Addseconds

Zaman damgasına birkaç saniye ekleyin.

```
addSeconds('<timestamp>', <seconds>, '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | Dize | Zaman damgasını içeren dize |
| <*Saniye*> | Evet | Tamsayı | Eklenecek pozitif veya negatif saniye sayısı |
| <*Biçim*> | Hayır | Dize | Ya tek bir [biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ya da özel bir [biçim deseni.](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) Zaman damgası için varsayılan [biçim, ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ile uyumlu ve saat dilimi bilgilerini koruyan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-MM-ddTHH:mm:ss:fffffffK) biçimidir. |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*güncelleştirilmiş zaman damgası*> | Dize | Zaman damgası artı belirtilen saniye sayısı  |
||||

*Örnek 1*

Bu örnek, belirtilen zaman damgasına 10 saniye ekler:

```
addSeconds('2018-03-15T00:00:00Z', 10)
```

Ve bu sonucu döndürür:`"2018-03-15T00:00:10.0000000Z"`

*Örnek 2*

Bu örnek, belirtilen zaman damgasına beş saniye çıkarır:

```
addSeconds('2018-03-15T00:00:30Z', -5)
```

Ve bu sonucu döndürür:`"2018-03-15T00:00:25.0000000Z"`

<a name="addToTime"></a>

### <a name="addtotime"></a>addToTime

Zaman damgasına birkaç zaman birimi ekleyin.
Ayrıca [bakınız FutureTime()](#getFutureTime).

```
addToTime('<timestamp>', <interval>, '<timeUnit>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | Dize | Zaman damgasını içeren dize |
| <*Aralığı*> | Evet | Tamsayı | Eklenecek belirtilen zaman birimlerinin sayısı |
| <*zaman Birimi*> | Evet | Dize | *Aralıklı*olarak kullanılacak zaman birimi : "İkinci", "Dakika", "Saat", "Gün", "Hafta", "Ay", "Yıl" |
| <*Biçim*> | Hayır | Dize | Ya tek bir [biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ya da özel bir [biçim deseni.](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) Zaman damgası için varsayılan [biçim, ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ile uyumlu ve saat dilimi bilgilerini koruyan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-MM-ddTHH:mm:ss:fffffffK) biçimidir. |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*güncelleştirilmiş zaman damgası*> | Dize | Zaman damgası artı belirtilen zaman birimi sayısı  |
||||

*Örnek 1*

Bu örnek, belirtilen zaman damgasına bir gün ekler:

```
addToTime('2018-01-01T00:00:00Z', 1, 'Day')
```

Ve bu sonucu döndürür:`"2018-01-02T00:00:00.0000000Z"`

*Örnek 2*

Bu örnek, belirtilen zaman damgasına bir gün ekler:

```
addToTime('2018-01-01T00:00:00Z', 1, 'Day', 'D')
```

Ve isteğe bağlı "D" biçimini kullanarak sonucu döndürür:`"Tuesday, January 2, 2018"`

<a name="and"></a>

### <a name="and"></a>ve

Tüm ifadelerin doğru olup olmadığını denetleyin.
Tüm ifadeler doğru olduğunda doğru döndürün veya en az bir ifade yanlış olduğunda yanlış döndürün.

```
and(<expression1>, <expression2>, ...)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*ifade1*>, <*ifade2*>, ... | Evet | Boole | Denetlenen ifadeler |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | -----| ----------- |
| true veya false | Boole | Tüm ifadeler doğru olduğunda doğru döndürün. En az bir ifade yanlış olduğunda yanlış döndürün. |
||||

*Örnek 1*

Bu örnekler, belirtilen Boolean değerlerinin hepsinin doğru olup olmadığını denetler:

```
and(true, true)
and(false, true)
and(false, false)
```

Ve bu sonuçları döndürür:

* İlk örnek: Her iki ifade `true`de doğrudur, bu nedenle döndürür.
* İkinci örnek: Bir ifade yanlıştır, bu nedenle döndürür. `false`
* Üçüncü örnek: Her iki ifade `false`de yanlış, bu nedenle döndürür.

*Örnek 2*

Bu örnekler, belirtilen ifadelerin hepsinin doğru olup olmadığını denetler:

```
and(equals(1, 1), equals(2, 2))
and(equals(1, 1), equals(1, 2))
and(equals(1, 2), equals(1, 3))
```

Ve bu sonuçları döndürür:

* İlk örnek: Her iki ifade `true`de doğrudur, bu nedenle döndürür.
* İkinci örnek: Bir ifade yanlıştır, bu nedenle döndürür. `false`
* Üçüncü örnek: Her iki ifade `false`de yanlış, bu nedenle döndürür.

<a name="array"></a>

### <a name="array"></a>array

Belirli bir girişten bir dizi döndürün.
Birden çok giriş için [createArray()](#createArray)bakın.

```
array('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize | Dizi oluşturmak için dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| [<*değer*>] | Dizi | Tek belirtilen girişi içeren bir dizi |
||||

*Örnek*

Bu örnekte "merhaba" dizesinden bir dizi oluşturulur:

```
array('hello')
```

Ve bu sonucu döndürür:`["hello"]`

<a name="base64"></a>

### <a name="base64"></a>base64

Bir dize için base64 kodlanmış sürümü döndürün.

```
base64('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize | Giriş dizesi |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*base64-string*> | Dize | Giriş dizesi için base64 kodlanmış sürüm |
||||

*Örnek*

Bu örnek, "merhaba" dizesini base64 kodlanmış bir dize dönüştürür:

```
base64('hello')
```

Ve bu sonucu döndürür:`"aGVsbG8="`

<a name="base64ToBinary"></a>

### <a name="base64tobinary"></a>base64ToBinary

Base64 kodlanmış bir dize için ikili sürümü döndürün.

```
base64ToBinary('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize | Dönüştürmek için base64 kodlanmış dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*ikili-for-base64-string*> | Dize | Base64 kodlanmış dize için ikili sürüm |
||||

*Örnek*

Bu örnek, "aGVsbG8=" base64 kodlanmış dizeyi ikili bir dize dönüştürür:

```
base64ToBinary('aGVsbG8=')
```

Ve bu sonucu döndürür:

`"0110000101000111010101100111001101100010010001110011100000111101"`

<a name="base64ToString"></a>

### <a name="base64tostring"></a>base64ToString

Base64 kodlanmış bir dize için dize sürümünü döndürün ve base64 dizesini etkili bir şekilde çözün.
[Base64() decode](#decodeBase64)yerine bu işlevi kullanın.
Her iki fonksiyon da `base64ToString()` aynı şekilde çalışsa da tercih edilir.

```
base64ToString('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize | Kod çözmek için base64 kodlanmış dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*deşifre-base64-string*> | Dize | Base64 kodlanmış bir dize için dize sürümü |
||||

*Örnek*

Bu örnek, "aGVsbG8=" base64 kodlanmış dizeyi sadece bir dize dönüştürür:

```
base64ToString('aGVsbG8=')
```

Ve bu sonucu döndürür:`"hello"`

<a name="binary"></a>

### <a name="binary"></a>ikili

Bir dize için ikili sürümü döndürün.

```
binary('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize | Dönüştürülecek dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*ikili-giriş-değer*> | Dize | Belirtilen dize için ikili sürüm |
||||

*Örnek*

Bu örnek, "merhaba" dizesini ikili dizeye dönüştürür:

```
binary('hello')
```

Ve bu sonucu döndürür:

`"0110100001100101011011000110110001101111"`

<a name="bool"></a>

### <a name="bool"></a>bool

Bir değer için Boolean sürümünü döndürün.

```
bool(<value>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Herhangi biri | Dönüştürülecek değer |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| true veya false | Boole | Belirtilen değer için Boolean sürümü |
||||

*Örnek*

Bu örnekler, belirtilen değerleri Boolean değerlerine dönüştürür:

```
bool(1)
bool(0)
```

Ve bu sonuçları döndürür:

* İlk örnek:`true`
* İkinci örnek:`false`

<a name="coalesce"></a>

### <a name="coalesce"></a>Coalesce

Bir veya daha fazla parametreden ilk null olmayan değeri döndürün.
Boş dizeleri, boş diziler ve boş nesneler null değildir.

```
coalesce(<object_1>, <object_2>, ...)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*object_1*>, <*object_2*>, ... | Evet | Herhangi, türleri karıştırabilirsiniz | Null için kontrol etmek için bir veya daha fazla öğe |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*ilk-null-öğe*> | Herhangi biri | Null olmayan ilk öğe veya değer. Tüm parametreler null ise, bu işlev null döndürür. |
||||

*Örnek*

Bu örnekler, belirtilen değerlerden ilk null olmayan değeri veya tüm değerler null olduğunda null döndürür:

```
coalesce(null, true, false)
coalesce(null, 'hello', 'world')
coalesce(null, null, null)
```

Ve bu sonuçları döndürür:

* İlk örnek:`true`
* İkinci örnek:`"hello"`
* Üçüncü örnek:`null`

<a name="concat"></a>

### <a name="concat"></a>concat

İki veya daha fazla dizebirleştirin ve birleştirilmiş dizeyi döndürün.

```
concat('<text1>', '<text2>', ...)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*metin1*>, <*text2*>, ... | Evet | Dize | Birleştirmek için en az iki dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*metin1text2...*> | Dize | Birleştirilmiş giriş dizelerinden oluşturulan dize |
||||

*Örnek*

Bu örnek, "Merhaba" ve "Dünya" dizelerini birleştirir:

```
concat('Hello', 'World')
```

Ve bu sonucu döndürür:`"HelloWorld"`

<a name="contains"></a>

### <a name="contains"></a>içerir

Koleksiyonun belirli bir öğesi olup olmadığını denetleyin.
Öğe bulunduğunda doğru döndürün veya bulunmadığında yanlış döndürün.
Bu fonksiyon büyük/küçük harf duyarlıdır.

```
contains('<collection>', '<value>')
contains([<collection>], '<value>')
```

Özellikle, bu işlev bu toplama türleri üzerinde çalışır:

* Bir *alt dize* bulmak için bir *dize*
* Bir *değeri* bulmak için bir *dizi*
* Bir *anahtar* bulmak için bir *sözlük*

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Koleksiyon*> | Evet | Dize, Dizi veya Sözlük | Denetlenen koleksiyon |
| <*Değer*> | Evet | String, Array veya Sözlük, sırasıyla | Bulunacak öğe |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| true veya false | Boole | Öğe bulunduğunda doğru döndürün. Bulunduğunda yanlış döndürün. |
||||

*Örnek 1*

Bu örnek, "dünya" alt dizesi için "merhaba dünya" dizesini denetler ve doğru döndürür:

```
contains('hello world', 'world')
```

*Örnek 2*

Bu örnek, "evren" alt dizesini "merhaba dünya" dizesini denetler ve yanlış döndürür:

```
contains('hello world', 'universe')
```

<a name="convertFromUtc"></a>

### <a name="convertfromutc"></a>convertFromUtc

Evrensel Zaman Eşgüdümlü'nden (UTC) bir zaman damgasını hedef saat dilimine dönüştürün.

```
convertFromUtc('<timestamp>', '<destinationTimeZone>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | Dize | Zaman damgasını içeren dize |
| <*hedefTimeZone*> | Evet | Dize | Hedef saat diliminin adı. Saat dilimi adları için [Microsoft Saat Dilimi Dizin Değerleri'ne](https://support.microsoft.com/help/973627/microsoft-time-zone-index-values)bakın, ancak saat dilimi adından herhangi bir noktalama işaretini kaldırmanız gerekebilir. |
| <*Biçim*> | Hayır | Dize | Ya tek bir [biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ya da özel bir [biçim deseni.](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) Zaman damgası için varsayılan [biçim, ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ile uyumlu ve saat dilimi bilgilerini koruyan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-MM-ddTHH:mm:ss:fffffffK) biçimidir. |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*dönüştürülmüş zaman damgası*> | Dize | Hedef saat dilimine dönüştürülen zaman damgası |
||||

*Örnek 1*

Bu örnek, bir zaman damgasını belirtilen saat dilimine dönüştürür:

```
convertFromUtc('2018-01-01T08:00:00.0000000Z', 'Pacific Standard Time')
```

Ve bu sonucu döndürür:`"2018-01-01T00:00:00.0000000"`

*Örnek 2*

Bu örnek, bir zaman damgasını belirtilen saat dilimine ve biçime dönüştürür:

```
convertFromUtc('2018-01-01T08:00:00.0000000Z', 'Pacific Standard Time', 'D')
```

Ve bu sonucu döndürür:`"Monday, January 1, 2018"`

<a name="convertTimeZone"></a>

### <a name="converttimezone"></a>dönüştürmeTimeZone

Kaynak saat diliminden bir zaman damgasını hedef saat dilimine dönüştürün.

```
convertTimeZone('<timestamp>', '<sourceTimeZone>', '<destinationTimeZone>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | Dize | Zaman damgasını içeren dize |
| <*Sourcetimezone*> | Evet | Dize | Kaynak saat diliminin adı. Saat dilimi adları için [Microsoft Saat Dilimi Dizin Değerleri'ne](https://support.microsoft.com/help/973627/microsoft-time-zone-index-values)bakın, ancak saat dilimi adından herhangi bir noktalama işaretini kaldırmanız gerekebilir. |
| <*hedefTimeZone*> | Evet | Dize | Hedef saat diliminin adı. Saat dilimi adları için [Microsoft Saat Dilimi Dizin Değerleri'ne](https://support.microsoft.com/help/973627/microsoft-time-zone-index-values)bakın, ancak saat dilimi adından herhangi bir noktalama işaretini kaldırmanız gerekebilir. |
| <*Biçim*> | Hayır | Dize | Ya tek bir [biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ya da özel bir [biçim deseni.](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) Zaman damgası için varsayılan [biçim, ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ile uyumlu ve saat dilimi bilgilerini koruyan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-MM-ddTHH:mm:ss:fffffffK) biçimidir. |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*dönüştürülmüş zaman damgası*> | Dize | Hedef saat dilimine dönüştürülen zaman damgası |
||||

*Örnek 1*

Bu örnek, kaynak saat dilimini hedef saat dilimine dönüştürür:

```
convertTimeZone('2018-01-01T08:00:00.0000000Z', 'UTC', 'Pacific Standard Time')
```

Ve bu sonucu döndürür:`"2018-01-01T00:00:00.0000000"`

*Örnek 2*

Bu örnek, bir saat dilimini belirtilen saat dilimine ve biçime dönüştürür:

```
convertTimeZone('2018-01-01T80:00:00.0000000Z', 'UTC', 'Pacific Standard Time', 'D')
```

Ve bu sonucu döndürür:`"Monday, January 1, 2018"`

<a name="convertToUtc"></a>

### <a name="converttoutc"></a>convertToUtc

Kaynak saat dilimindeki bir zaman damgasını Evrensel Saat Koordinasyonu'na (UTC) dönüştürün.

```
convertToUtc('<timestamp>', '<sourceTimeZone>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | Dize | Zaman damgasını içeren dize |
| <*Sourcetimezone*> | Evet | Dize | Kaynak saat diliminin adı. Saat dilimi adları için [Microsoft Saat Dilimi Dizin Değerleri'ne](https://support.microsoft.com/help/973627/microsoft-time-zone-index-values)bakın, ancak saat dilimi adından herhangi bir noktalama işaretini kaldırmanız gerekebilir. |
| <*Biçim*> | Hayır | Dize | Ya tek bir [biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ya da özel bir [biçim deseni.](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) Zaman damgası için varsayılan [biçim, ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ile uyumlu ve saat dilimi bilgilerini koruyan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-MM-ddTHH:mm:ss:fffffffK) biçimidir. |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*dönüştürülmüş zaman damgası*> | Dize | ZAMAN damgası UTC'ye dönüştürüldü |
||||

*Örnek 1*

Bu örnek, bir zaman damgasını UTC'ye dönüştürür:

```
convertToUtc('01/01/2018 00:00:00', 'Pacific Standard Time')
```

Ve bu sonucu döndürür:`"2018-01-01T08:00:00.0000000Z"`

*Örnek 2*

Bu örnek, bir zaman damgasını UTC'ye dönüştürür:

```
convertToUtc('01/01/2018 00:00:00', 'Pacific Standard Time', 'D')
```

Ve bu sonucu döndürür:`"Monday, January 1, 2018"`

<a name="createArray"></a>

### <a name="createarray"></a>createArray

Birden çok girişten bir dizi döndürün.
Tek giriş dizileri için [dizi()](#array)bakın.

```
createArray('<object1>', '<object2>', ...)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*object1*>, <*object2*>, ... | Evet | Herhangi, ama karışık değil | Diziyi oluşturmak için en az iki öğe |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| [<*object1*>, <*object2*>, ...] | Dizi | Tüm giriş öğelerinden oluşturulan dizi |
||||

*Örnek*

Bu örnek, bu girişlerden bir dizi oluşturur:

```
createArray('h', 'e', 'l', 'l', 'o')
```

Ve bu sonucu döndürür:`["h", "e", "l", "l", "o"]`

<a name="dataUri"></a>

### <a name="datauri"></a>dataUri

Bir dize için veri tekdüzen kaynak tanımlayıcısı (URI) döndürün.

```
dataUri('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize | Dönüştürülecek dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*veri-uri*> | Dize | Giriş dizesi için veri URI |
||||

*Örnek*

Bu örnek, "merhaba" dizesi için bir veri URI oluşturur:

```
dataUri('hello')
```

Ve bu sonucu döndürür:`"data:text/plain;charset=utf-8;base64,aGVsbG8="`

<a name="dataUriToBinary"></a>

### <a name="datauritobinary"></a>dataUritoBinary

Veri üniforması kaynak tanımlayıcısı (URI) için ikili sürümü döndürün.
Bu işlevi [decodeDataUri()](#decodeDataUri)yerine kullanın.
Her iki fonksiyon da `dataUriBinary()` aynı şekilde çalışsa da tercih edilir.

```
dataUriToBinary('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize | Dönüştürmek için veri URI |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*ikili-for-data-uri*> | Dize | Veri URI için ikili sürümü |
||||

*Örnek*

Bu örnek, bu veri URI için ikili bir sürüm oluşturur:

```
dataUriToBinary('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

Ve bu sonucu döndürür:

`"01100100011000010111010001100001001110100111010001100101011110000111010000101111011100000
1101100011000010110100101101110001110110110001101101000011000010111001001110011011001010111
0100001111010111010101110100011001100010110100111000001110110110001001100001011100110110010
10011011000110100001011000110000101000111010101100111001101100010010001110011100000111101"`

<a name="dataUriToString"></a>

### <a name="datauritostring"></a>dataUritostring

Veri tekdüzen kaynak tanımlayıcısı (URI) için dize sürümünü döndürün.

```
dataUriToString('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize | Dönüştürmek için veri URI |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*string-for-data-uri*> | Dize | Veri URI için dize sürümü |
||||

*Örnek*

Bu örnek, bu veri URI için bir dize oluşturur:

```
dataUriToString('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

Ve bu sonucu döndürür:`"hello"`

<a name="dayOfMonth"></a>

### <a name="dayofmonth"></a>günOfMonth

Ayın gününü bir zaman damgasından döndürün.

```
dayOfMonth('<timestamp>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | Dize | Zaman damgasını içeren dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*gün-ay*> | Tamsayı | Belirtilen zaman damgasından ayın günü |
||||

*Örnek*

Bu örnek, bu zaman damgasından ayın gününün numarasını döndürür:

```
dayOfMonth('2018-03-15T13:27:36Z')
```

Ve bu sonucu döndürür:`15`

<a name="dayOfWeek"></a>

### <a name="dayofweek"></a>Dayofweek

Haftanın gününü bir zaman damgasından döndürün.

```
dayOfWeek('<timestamp>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | Dize | Zaman damgasını içeren dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*haftanın günü*> | Tamsayı | Pazar 0, Pazartesi 1 ve benzeri belirtilen zaman damgası haftanın günü |
||||

*Örnek*

Bu örnek, bu zaman damgasından haftanın gününün numarasını döndürür:

```
dayOfWeek('2018-03-15T13:27:36Z')
```

Ve bu sonucu döndürür:`3`

<a name="dayOfYear"></a>

### <a name="dayofyear"></a>Dayofyear

Yılın gününü zaman damgasından döndürün.

```
dayOfYear('<timestamp>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | Dize | Zaman damgasını içeren dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*yıl-of-yıl*> | Tamsayı | Belirtilen zaman damgasından yılın günü |
||||

*Örnek*

Bu örnek, bu zaman damgasından yılın gün sayısını döndürür:

```
dayOfYear('2018-03-15T13:27:36Z')
```

Ve bu sonucu döndürür:`74`

<a name="decodeBase64"></a>

### <a name="decodebase64"></a>decodeBase64

Base64 kodlanmış bir dize için dize sürümünü döndürün ve base64 dizesini etkili bir şekilde çözün.
Yerine [base64ToString()](#base64ToString) kullanmayı `decodeBase64()`düşünün.
Her iki fonksiyon da `base64ToString()` aynı şekilde çalışsa da tercih edilir.

```
decodeBase64('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize | Kod çözmek için base64 kodlanmış dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*deşifre-base64-string*> | Dize | Base64 kodlanmış bir dize için dize sürümü |
||||

*Örnek*

Bu örnek, base64 kodlanmış bir dize için bir dize oluşturur:

```
decodeBase64('aGVsbG8=')
```

Ve bu sonucu döndürür:`"hello"`

<a name="decodeDataUri"></a>

### <a name="decodedatauri"></a>decodeDataUri

Veri üniforması kaynak tanımlayıcısı (URI) için ikili sürümü döndürün.
[DataUriToBinary()](#dataUriToBinary)yerine kullanmayı `decodeDataUri()`düşünün.
Her iki fonksiyon da `dataUriToBinary()` aynı şekilde çalışsa da tercih edilir.

```
decodeDataUri('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize | Çözmeiçin veri URI dizesi |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*ikili-for-data-uri*> | Dize | Veri URI dizesi için ikili sürüm |
||||

*Örnek*

Bu örnek, bu veri URI için ikili sürümünü döndürür:

```
decodeDataUri('data:text/plain;charset=utf-8;base64,aGVsbG8=')
```

Ve bu sonucu döndürür:

`"01100100011000010111010001100001001110100111010001100101011110000111010000101111011100000
1101100011000010110100101101110001110110110001101101000011000010111001001110011011001010111
0100001111010111010101110100011001100010110100111000001110110110001001100001011100110110010
10011011000110100001011000110000101000111010101100111001101100010010001110011100000111101"`

<a name="decodeUriComponent"></a>

### <a name="decodeuricomponent"></a>decodeUriComponent

Kaçış karakterlerinin yerine deşifre edilmiş sürümlerle bir dize döndürün.

```
decodeUriComponent('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize | Çözmek için kaçış karakterleri ile dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*deşifre-uri*> | Dize | Kodlanmış kaçış karakterleri ile güncelleştirilmiş dize |
||||

*Örnek*

Bu örnek, bu dizedeki kaçış karakterlerinin yerine deşifre edilmiş sürümlerle değiştirilir:

```
decodeUriComponent('http%3A%2F%2Fcontoso.com')
```

Ve bu sonucu döndürür:`"https://contoso.com"`

<a name="div"></a>

### <a name="div"></a>div

İki sayıyı bölerek gelen sonda sonucunu döndürün.
Kalan sonucu almak için [mod()](#mod)bakın.

```
div(<dividend>, <divisor>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Temettü*> | Evet | Sonda veya Float | *Bölene* bölünecek sayı |
| <*Bölen*> | Evet | Sonda veya Float | *Temettü*bölen ancak 0 olamaz sayı |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*quotient-sonuç*> | Tamsayı | İlk sayıyı ikinci sayıya bölmenin sonucu |
||||

*Örnek*

Her iki örnek de birinci sayıyı ikinci sayıya böler:

```
div(10, 5)
div(11, 5)
```

Ve bu sonucu iade edin:`2`

<a name="encodeUriComponent"></a>

### <a name="encodeuricomponent"></a>kodlamaUriComponent

URL güvenli olmayan karakterleri kaçış karakterleriyle değiştirerek, tek düzen kaynak tanımlayıcısı (URI) kodlanmış sürümü bir dize için döndürün.
[uriComponent()](#uriComponent)yerine kullanmayı `encodeUriComponent()`düşünün.
Her iki fonksiyon da `uriComponent()` aynı şekilde çalışsa da tercih edilir.

```
encodeUriComponent('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize | URI kodlu biçime dönüştürülecek dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*kodlanmış-uri*> | Dize | Kaçış karakterleri ile URI kodlanmış dize |
||||

*Örnek*

Bu örnek, bu dize için URI kodlanmış bir sürüm oluşturur:

```
encodeUriComponent('https://contoso.com')
```

Ve bu sonucu döndürür:`"http%3A%2F%2Fcontoso.com"`

<a name="empty"></a>

### <a name="empty"></a>empty

Koleksiyonun boş olup olmadığını kontrol edin.
Koleksiyon boşolduğunda doğru döndürün veya boş değilken yanlış döndürün.

```
empty('<collection>')
empty([<collection>])
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Koleksiyon*> | Evet | Dize, Dizi veya Nesne | Denetlenen koleksiyon |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| true veya false | Boole | Koleksiyon boşolduğunda doğru döndürün. Boş değilken yanlış döndürün. |
||||

*Örnek*

Bu örnekler, belirtilen koleksiyonların boş olup olmadığını denetler:

```
empty('')
empty('abc')
```

Ve bu sonuçları döndürür:

* İlk örnek: Boş bir dize `true`geçirir, böylece işlev döndürür.
* İkinci örnek: "abc" dizesini geçer, böylece işlev döndürür. `false`

<a name="endswith"></a>

### <a name="endswith"></a>endsWith

Bir dize belirli bir alt dize ile bitene denetleyin.
Alt dize bulunduğunda doğru döndürün veya bulunmadığında false döndürün.
Bu işlev büyük/küçük harf duyarlı değildir.

```
endsWith('<text>', '<searchText>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | Dize | Denetlenen dize |
| <*searchText*> | Evet | Dize | Bulmak için bitiş alt dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| true veya false  | Boole | Bitiş alt dizesi bulunduğunda doğru döndürün. Bulunduğunda yanlış döndürün. |
||||

*Örnek 1*

Bu örnek, "merhaba dünya" dizesinin "dünya" dizesiyle bitip bitmediğini denetler:

```
endsWith('hello world', 'world')
```

Ve bu sonucu döndürür:`true`

*Örnek 2*

Bu örnek, "merhaba dünya" dizesinin "evren" dizesiyle bitip bitmediğini denetler:

```
endsWith('hello world', 'universe')
```

Ve bu sonucu döndürür:`false`

<a name="equals"></a>

### <a name="equals"></a>equals

Her iki değerin, ifadenin veya nesnenin eşdeğer olup olmadığını denetleyin.
Her ikisi de eşdeğer olduğunda doğru döndürün veya eşdeğer olmadıklarında yanlış döndürün.

```
equals('<object1>', '<object2>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*object1*>, <*nesne2*> | Evet | Çeşitli | Karşılaştırmak için değerler, ifadeler veya nesneler |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| true veya false | Boole | Her ikisi de eşdeğer olduğunda doğru döndürün. Eşdeğer olmadığında false döndürün. |
||||

*Örnek*

Bu örnekler, belirtilen girişlerin eşdeğer olup olmadığını denetler.

```
equals(true, 1)
equals('abc', 'abcd')
```

Ve bu sonuçları döndürür:

* İlk örnek: Her iki değer de `true`eşdeğerdir, bu nedenle işlev döndürür.
* İkinci örnek: Her iki değer de eşdeğer `false`değildir, bu nedenle işlev döndürür.

<a name="first"></a>

### <a name="first"></a>Ilk

Bir dize veya diziden ilk öğeyi döndürün.

```
first('<collection>')
first([<collection>])
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Koleksiyon*> | Evet | Dize veya Dizi | İlk öğeyi bulabileceğiniz koleksiyon |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*ilk toplama öğesi*> | Herhangi biri | Koleksiyondaki ilk öğe |
||||

*Örnek*

Bu örnekler, bu koleksiyonlarda ilk öğeyi bulabilirsiniz:

```
first('hello')
first(createArray(0, 1, 2))
```

Ve bu sonuçları iade edin:

* İlk örnek:`"h"`
* İkinci örnek:`0`

<a name="float"></a>

### <a name="float"></a>float

Kayan nokta sayısı için dize sürümünü gerçek kayan nokta sayısına dönüştürün.

```
float('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize | Dönüştürmek için geçerli bir kayan nokta numarası olan dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*float değeri*> | Kayan | Belirtilen dize için kayan nokta numarası |
||||

*Örnek*

Bu örnek, bu kayan nokta numarası için bir dize sürümü oluşturur:

```
float('10.333')
```

Ve bu sonucu döndürür:`10.333`

<a name="formatDateTime"></a>

### <a name="formatdatetime"></a>formatDateTime

Belirtilen biçimde bir zaman damgası döndürün.

```
formatDateTime('<timestamp>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | Dize | Zaman damgasını içeren dize |
| <*Biçim*> | Hayır | Dize | Ya tek bir [biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ya da özel bir [biçim deseni.](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) Zaman damgası için varsayılan [biçim, ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ile uyumlu ve saat dilimi bilgilerini koruyan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-MM-ddTHH:mm:ss:fffffffK) biçimidir. |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*reformatted-zaman damgası*> | Dize | Belirtilen biçimde güncelleştirilmiş zaman damgası |
||||

*Örnek*

Bu örnek, bir zaman damgasını belirtilen biçime dönüştürür:

```
formatDateTime('03/15/2018 12:00:00', 'yyyy-MM-ddTHH:mm:ss')
```

Ve bu sonucu döndürür:`"2018-03-15T12:00:00"`

<a name="getFutureTime"></a>

### <a name="getfuturetime"></a>getFutureTime

Geçerli zaman damgasını ve belirtilen zaman birimlerini döndürün.

```
getFutureTime(<interval>, <timeUnit>, <format>?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Aralığı*> | Evet | Tamsayı | Eklenecek belirtilen zaman birimlerinin sayısı |
| <*zaman Birimi*> | Evet | Dize | *Aralıklı*olarak kullanılacak zaman birimi : "İkinci", "Dakika", "Saat", "Gün", "Hafta", "Ay", "Yıl" |
| <*Biçim*> | Hayır | Dize | Ya tek bir [biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ya da özel bir [biçim deseni.](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) Zaman damgası için varsayılan [biçim, ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ile uyumlu ve saat dilimi bilgilerini koruyan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-MM-ddTHH:mm:ss:fffffffK) biçimidir. |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*güncelleştirilmiş zaman damgası*> | Dize | Geçerli zaman damgası artı belirtilen zaman birimi sayısı |
||||

*Örnek 1*

Geçerli zaman damgasının "2018-03-01T00:00:00.000000Z" olduğunu varsayalım.
Bu örnek, bu zaman damgasına beş gün ekler:

```
getFutureTime(5, 'Day')
```

Ve bu sonucu döndürür:`"2018-03-06T00:00:00.0000000Z"`

*Örnek 2*

Geçerli zaman damgasının "2018-03-01T00:00:00.000000Z" olduğunu varsayalım.
Bu örnek beş gün ekler ve sonucu "D" biçimine dönüştürür:

```
getFutureTime(5, 'Day', 'D')
```

Ve bu sonucu döndürür:`"Tuesday, March 6, 2018"`

<a name="getPastTime"></a>

### <a name="getpasttime"></a>getPastTime

Geçerli zaman damgasını eksi belirtilen zaman birimlerini döndürün.

```
getPastTime(<interval>, <timeUnit>, <format>?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Aralığı*> | Evet | Tamsayı | Çıkarıiçin belirtilen zaman birimlerinin sayısı |
| <*zaman Birimi*> | Evet | Dize | *Aralıklı*olarak kullanılacak zaman birimi : "İkinci", "Dakika", "Saat", "Gün", "Hafta", "Ay", "Yıl" |
| <*Biçim*> | Hayır | Dize | Ya tek bir [biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ya da özel bir [biçim deseni.](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) Zaman damgası için varsayılan [biçim, ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ile uyumlu ve saat dilimi bilgilerini koruyan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-MM-ddTHH:mm:ss:fffffffK) biçimidir. |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*güncelleştirilmiş zaman damgası*> | Dize | Geçerli zaman damgası eksi belirtilen zaman birimi sayısı |
||||

*Örnek 1*

Geçerli zaman damgasının "2018-02-01T00:00:00.000000Z" olduğunu varsayalım.
Bu örnek, o zaman damgasından beş gün çıkarır:

```
getPastTime(5, 'Day')
```

Ve bu sonucu döndürür:`"2018-01-27T00:00:00.0000000Z"`

*Örnek 2*

Geçerli zaman damgasının "2018-02-01T00:00:00.000000Z" olduğunu varsayalım.
Bu örnek beş gün çıkarır ve sonucu "D" biçimine dönüştürür:

```
getPastTime(5, 'Day', 'D')
```

Ve bu sonucu döndürür:`"Saturday, January 27, 2018"`

<a name="greater"></a>

### <a name="greater"></a>büyüktür

İlk değerin ikinci değerden büyük olup olmadığını denetleyin.
İlk değer daha fazla olduğunda doğru döndürün veya daha az olduğunda false döndürün.

```
greater(<value>, <compareTo>)
greater('<value>', '<compareTo>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Sonda, Float veya String | İkinci değerden büyük olup olmadığını kontrol etmek için ilk değer |
| <*compareTo*> | Evet | Sırasıyla, Insalı, Float veya String | Karşılaştırma değeri |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| true veya false | Boole | İlk değer ikinci değerden büyük olduğunda doğru döndürün. İlk değer ikinci değere eşit veya daha az olduğunda yanlış döndürün. |
||||

*Örnek*

Bu örnekler, ilk değerin ikinci değerden büyük olup olmadığını denetler:

```
greater(10, 5)
greater('apple', 'banana')
```

Ve bu sonuçları iade edin:

* İlk örnek:`true`
* İkinci örnek:`false`

<a name="greaterOrEquals"></a>

### <a name="greaterorequals"></a>greaterOrEquals

İlk değerin ikinci değerden büyük mü yoksa eşit mi olduğunu denetleyin.
İlk değer büyük veya eşit olduğunda doğru döndürün veya ilk değer daha az olduğunda false döndürün.

```
greaterOrEquals(<value>, <compareTo>)
greaterOrEquals('<value>', '<compareTo>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Sonda, Float veya String | İkinci değerden büyük mü yoksa eşit mi olup olmadığını kontrol etmek için ilk değer |
| <*compareTo*> | Evet | Sırasıyla, Insalı, Float veya String | Karşılaştırma değeri |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| true veya false | Boole | İlk değer ikinci değerden büyük veya eşit olduğunda doğru döndürün. İlk değer ikinci değerden küçük olduğunda false döndürün. |
||||

*Örnek*

Bu örnekler, ilk değerin ikinci değerden büyük mü yoksa eşit mi olduğunu denetler:

```
greaterOrEquals(5, 5)
greaterOrEquals('apple', 'banana')
```

Ve bu sonuçları iade edin:

* İlk örnek:`true`
* İkinci örnek:`false`

<a name="guid"></a>

### <a name="guid"></a>Guıd

"c2ecc88d-88c8-4096-912c-d6f2e2b138ce" gibi bir dize olarak küresel olarak benzersiz bir tanımlayıcı (GUID) oluşturun:

```
guid()
```

Ayrıca, tirelerle ayrılmış 32 basamak olan varsayılan biçim olan "D" dışında GUID için farklı bir biçim belirtebilirsiniz.

```
guid('<format>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Biçim*> | Hayır | Dize | Döndürülen GUID için tek bir [biçim belirtici.](https://msdn.microsoft.com/library/97af8hh4) Varsayılan olarak, biçim "D"dir, ancak "N", "D", "B", "P" veya "X" kullanabilirsiniz. |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*GUID değeri*> | Dize | Rasgele oluşturulan GUID |
||||

*Örnek*

Bu örnek, tirelerle ayrılmış ve parantez içinde ekolarak 32 basamak olarak aynı GUID'i oluşturur:

```
guid('P')
```

Ve bu sonucu döndürür:`"(c2ecc88d-88c8-4096-912c-d6f2e2b138ce)"`

<a name="if"></a>

### <a name="if"></a>if

Bir ifadenin doğru mu yanlış mı olduğunu kontrol edin.
Sonuca bağlı olarak, belirli bir değeri döndürün.

```
if(<expression>, <valueIfTrue>, <valueIfFalse>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Ifa -de*> | Evet | Boole | Denetlenen ifade |
| <*valueIfTrue*> | Evet | Herhangi biri | İfade doğru olduğunda döndürülecek değer |
| <*valueIfFalse*> | Evet | Herhangi biri | İfade yanlış olduğunda döndürülecek değer |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*belirtilen-getiri-değer*> | Herhangi biri | İfadenin doğru mu yanlış mı olduğuna bağlı olarak dönen belirtilen değer |
||||

*Örnek*

Belirtilen ifade `"yes"` doğru döndürür, çünkü bu örnek döner.
Aksi takdirde, `"no"`örnek döndürür:

```
if(equals(1, 1), 'yes', 'no')
```

<a name="indexof"></a>

### <a name="indexof"></a>ındexof

Bir alt dize için başlangıç konumunu veya dizin değerini döndürün.
Bu işlev büyük/küçük harf duyarlı değildir ve dizinler 0 sayısıyla başlar.

```
indexOf('<text>', '<searchText>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | Dize | Bulmak için alt dize olan dize |
| <*searchText*> | Evet | Dize | Bulmak için substring |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*dizin değeri*>| Tamsayı | Belirtilen alt dize için başlangıç konumu veya dizin değeri. <p>Dize bulunamazsa, -1 sayısını döndürün. |
||||

*Örnek*

Bu örnekte, "merhaba dünya" dizesinde "dünya" alt dizesinin başlangıç dizin değeri aşağıdakileri bulur:

```
indexOf('hello world', 'world')
```

Ve bu sonucu döndürür:`6`

<a name="int"></a>

### <a name="int"></a>int

Tamsayı sürümünü bir dize için döndürün.

```
int('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize | Dönüştürülecek dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*sonuç*> | Tamsayı | Belirtilen dize için birsonrakir sürümü |
||||

*Örnek*

Bu örnek, "10" dizesi için bir tamsayı sürümü oluşturur:

```
int('10')
```

Ve bu sonucu döndürür:`10`

<a name="json"></a>

### <a name="json"></a>json

Bir dize veya XML için JavaScript Nesne Gösterimi (JSON) türü değerini veya nesnesini döndürün.

```
json('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize veya XML | Dönüştürmek için dize veya XML |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*JSON sonucu*> | JSON yerel türü veya nesnesi | Belirtilen dize veya XML için JSON yerel türü değeri veya nesnesi. Dize null ise, işlev boş bir nesne döndürür. |
||||

*Örnek 1*

Bu örnek, bu dizeyi JSON değerine dönüştürür:

```
json('[1, 2, 3]')
```

Ve bu sonucu döndürür:`[1, 2, 3]`

*Örnek 2*

Bu örnek, bu dizeyi JSON'a dönüştürür:

```
json('{"fullName": "Sophia Owen"}')
```

Ve bu sonucu döndürür:

```
{
  "fullName": "Sophia Owen"
}
```

*Örnek 3*

Bu örnek, bu XML'i JSON'a dönüştürür:

```
json(xml('<?xml version="1.0"?> <root> <person id='1'> <name>Sophia Owen</name> <occupation>Engineer</occupation> </person> </root>'))
```

Ve bu sonucu döndürür:

```json
{
   "?xml": { "@version": "1.0" },
   "root": {
      "person": [ {
         "@id": "1",
         "name": "Sophia Owen",
         "occupation": "Engineer"
      } ]
   }
}
```

<a name="intersection"></a>

### <a name="intersection"></a>Kesişim

Belirtilen koleksiyonlar arasında *yalnızca* ortak öğelere sahip bir koleksiyon döndürün.
Sonuçta görünmesi için, bir öğenin bu işleve geçirilen tüm koleksiyonlarda görünmesi gerekir.
Bir veya daha fazla öğe aynı ada sahipse, bu ada sahip son öğe sonuçta görünür.

```
intersection([<collection1>], [<collection2>], ...)
intersection('<collection1>', '<collection2>', ...)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*collection1*>, <*collection2*>, ... | Evet | Dizi veya Nesne, ancak her ikisi de | Yalnızca ortak öğeleri *istediğiniz* yerden gelen koleksiyonlar |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*ortak öğeler*> | Dizi veya Nesne, sırasıyla | Belirtilen koleksiyonlar arasında yalnızca ortak öğelere sahip bir koleksiyon |
||||

*Örnek*

Bu örnek, bu diziler arasında ortak öğeleri bulur:

```
intersection(createArray(1, 2, 3), createArray(101, 2, 1, 10), createArray(6, 8, 1, 2))
```

Ve *yalnızca* bu öğeleri içeren bir dizi döndürür:`[1, 2]`

<a name="join"></a>

### <a name="join"></a>join

Bir dizideki tüm öğeleri içeren ve her karakterin bir *delimiter*ile ayrılmış bir dize döndürün.

```
join([<collection>], '<delimiter>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Koleksiyon*> | Evet | Dizi | Öğelerin birleştirilmesi için dizi |
| <*Sınırlayıcı*> | Evet | Dize | Elde edilen dizedeki her karakter arasında görünen ayırıcı |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*char1*><*delimiter*><*char2*><*delimiter*>... | Dize | Belirtilen dizideki tüm öğelerden oluşturulan sonuç dizesi |
||||

*Örnek*

Bu örnek, delimiter olarak belirtilen karakter ile bu dizideki tüm öğelerden bir dize oluşturur:

```
join(createArray('a', 'b', 'c'), '.')
```

Ve bu sonucu döndürür:`"a.b.c"`

<a name="last"></a>

### <a name="last"></a>Son

Koleksiyondaki son öğeyi döndürün.

```
last('<collection>')
last([<collection>])
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Koleksiyon*> | Evet | Dize veya Dizi | Son öğeyi bulabileceğiniz koleksiyon |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*son toplama öğesi*> | String veya Array, sırasıyla | Koleksiyondaki son öğe |
||||

*Örnek*

Bu örnekler, bu koleksiyonlarda son öğeyi bulur:

```
last('abcd')
last(createArray(0, 1, 2, 3))
```

Ve bu sonuçları döndürür:

* İlk örnek:`"d"`
* İkinci örnek:`3`

<a name="lastindexof"></a>

### <a name="lastindexof"></a>Lastındexof

Bir alt dizenin son oluşumu için başlangıç konumunu veya dizin değerini döndürün.
Bu işlev büyük/küçük harf duyarlı değildir ve dizinler 0 sayısıyla başlar.

```
lastIndexOf('<text>', '<searchText>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | Dize | Bulmak için alt dize olan dize |
| <*searchText*> | Evet | Dize | Bulmak için substring |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*bitiş-dizin-değer*> | Tamsayı | Belirtilen alt dizenin son oluşumu için başlangıç konumu veya dizin değeri. <p>Dize bulunamazsa, -1 sayısını döndürün. |
||||

*Örnek*

Bu örnek, "merhaba dünya" dizesinde "dünya" alt dizesinin son oluşumu için başlangıç dizin değerini bulur:

```
lastIndexOf('hello world', 'world')
```

Ve bu sonucu döndürür:`6`

<a name="length"></a>

### <a name="length"></a>length

Koleksiyondaki öğe sayısını döndürün.

```
length('<collection>')
length([<collection>])
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Koleksiyon*> | Evet | Dize veya Dizi | Saymak için öğeleri ile toplama |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*uzunluk veya sayım*> | Tamsayı | Koleksiyondaki öğe sayısı |
||||

*Örnek*

Bu örnekler, bu koleksiyonlarda öğe sayısını saymak:

```
length('abcd')
length(createArray(0, 1, 2, 3))
```

Ve bu sonucu iade edin:`4`

<a name="less"></a>

### <a name="less"></a>daha az

İlk değerin ikinci değerden küçük olup olmadığını denetleyin.
İlk değer daha az olduğunda doğru döndürün veya ilk değer daha fazla olduğunda yanlış döndürün.

```
less(<value>, <compareTo>)
less('<value>', '<compareTo>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Sonda, Float veya String | İkinci değerden daha az olup olmadığını kontrol etmek için ilk değer |
| <*compareTo*> | Evet | Sırasıyla, Insalı, Float veya String | Karşılaştırma öğesi |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| true veya false | Boole | İlk değer ikinci değerden küçük olduğunda doğru döndürün. İlk değer ikinci değere eşit veya daha büyük olduğunda yanlış döndürün. |
||||

*Örnek*

Bu örnekler, ilk değerin ikinci değerden küçük olup olmadığını denetler.

```
less(5, 10)
less('banana', 'apple')
```

Ve bu sonuçları iade edin:

* İlk örnek:`true`
* İkinci örnek:`false`

<a name="lessOrEquals"></a>

### <a name="lessorequals"></a>lessOrEquals

İlk değerin ikinci değerden küçük mü yoksa eşit mi olduğunu denetleyin.
İlk değer daha az veya eşit olduğunda doğru döndürün veya ilk değer daha fazla olduğunda false döndürün.

```
lessOrEquals(<value>, <compareTo>)
lessOrEquals('<value>', '<compareTo>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Sonda, Float veya String | İkinci değerden daha az veya eşit olup olmadığını kontrol etmek için ilk değer |
| <*compareTo*> | Evet | Sırasıyla, Insalı, Float veya String | Karşılaştırma öğesi |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| true veya false  | Boole | İlk değer ikinci değerden küçük veya eşit olduğunda doğru döndürün. İlk değer ikinci değerden büyük olduğunda false döndürün. |
||||

*Örnek*

Bu örnekler, ilk değerin ikinci değerden daha az mı yoksa eşit mi olduğunu denetler.

```
lessOrEquals(10, 10)
lessOrEquals('apply', 'apple')
```

Ve bu sonuçları iade edin:

* İlk örnek:`true`
* İkinci örnek:`false`

<a name="max"></a>

### <a name="max"></a>max

Her iki uçta da dahil olan sayılarla bir listeden veya diziden en yüksek değeri döndürün.

```
max(<number1>, <number2>, ...)
max([<number1>, <number2>, ...])
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*> 1 numara,* *<numara2*>, ... | Evet | Sonda, Float veya her ikisi birden | En yüksek değeri istediğiniz sayı kümesi |
| [<*sayı1*>, <*sayı2>,* ...] | Evet | Dizi - İnteger, Float veya her ikisi birden | En yüksek değeri istediğiniz sayı dizisi |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*maksimum değer*> | Sonda veya Float | Belirtilen dizideki veya sayı kümesindeki en yüksek değer |
||||

*Örnek*

Bu örnekler, sayı kümesinden ve diziden en yüksek değeri alır:

```
max(1, 2, 3)
max(createArray(1, 2, 3))
```

Ve bu sonucu iade edin:`3`

<a name="min"></a>

### <a name="min"></a>min

Bir sayı kümesinden veya diziden en düşük değeri döndürün.

```
min(<number1>, <number2>, ...)
min([<number1>, <number2>, ...])
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*> 1 numara,* *<numara2*>, ... | Evet | Sonda, Float veya her ikisi birden | En düşük değeri istediğiniz sayı kümesi |
| [<*sayı1*>, <*sayı2>,* ...] | Evet | Dizi - İnteger, Float veya her ikisi birden | En düşük değeri istediğiniz sayı dizisi |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*min değeri*> | Sonda veya Float | Belirtilen sayı kümesindeki veya belirtilen dizideki en düşük değer |
||||

*Örnek*

Bu örnekler, sayı kümesive dizideki en düşük değeri alır:

```
min(1, 2, 3)
min(createArray(1, 2, 3))
```

Ve bu sonucu iade edin:`1`

<a name="mod"></a>

### <a name="mod"></a>mod

Kalan sayıyı iki sayıyı bölerek döndürün.
İnsteger sonucunu almak için [div()](#div)'ye bakın.

```
mod(<dividend>, <divisor>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Temettü*> | Evet | Sonda veya Float | *Bölene* bölünecek sayı |
| <*Bölen*> | Evet | Sonda veya Float | *Temettü*bölen ancak 0 olamaz sayı. |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*modulo-sonuç*> | Sonda veya Float | İlk sayının ikinci sayıya bölünmesinden kalan |
||||

*Örnek*

Bu örnek, birinci sayıyı ikinci sayıya böler:

```
mod(3, 2)
```

Ve bu sonucu iade edin:`1`

<a name="mul"></a>

### <a name="mul"></a>Mul

Ürünü iki sayıyı çarparak döndürün.

```
mul(<multiplicand1>, <multiplicand2>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*çarpma1*> | Evet | Sonda veya Float | Çarpma sayısı *çarp2* |
| <*çarpma2*> | Evet | Sonda veya Float | *Çarpanları çarpanları sayısı1* |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*ürün sonucu*> | Sonda veya Float | Birinci sayının ikinci sayıyla çarpılmasından elde edilen ürün |
||||

*Örnek*

Bu örnekler, birinci sayıyı ikinci sayıya göre birden fazla:

```
mul(1, 2)
mul(1.5, 2)
```

Ve bu sonuçları iade edin:

* İlk örnek:`2`
* İkinci örnek`3`

<a name="not"></a>

### <a name="not"></a>not

İfadenin yanlış olup olmadığını kontrol edin.
İfade yanlış olduğunda doğru döndürün veya doğru olduğunda false döndürün.

```json
not(<expression>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Ifa -de*> | Evet | Boole | Denetlenen ifade |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| true veya false | Boole | İfade yanlış olduğunda doğru döndürün. İfade doğru olduğunda yanlış döndürün. |
||||

*Örnek 1*

Bu örnekler, belirtilen ifadelerin yanlış olup olmadığını denetler:

```json
not(false)
not(true)
```

Ve bu sonuçları iade edin:

* İlk örnek: İfade yanlışolduğundan işlev `true`döndürür.
* İkinci örnek: İfade doğrudur, bu `false`nedenle işlev döndürür.

*Örnek 2*

Bu örnekler, belirtilen ifadelerin yanlış olup olmadığını denetler:

```json
not(equals(1, 2))
not(equals(1, 1))
```

Ve bu sonuçları iade edin:

* İlk örnek: İfade yanlışolduğundan işlev `true`döndürür.
* İkinci örnek: İfade doğrudur, bu `false`nedenle işlev döndürür.

<a name="or"></a>

### <a name="or"></a>or

En az bir ifadenin doğru olup olmadığını kontrol edin.
En az bir ifade doğru olduğunda doğru döndürün veya tümü yanlış olduğunda yanlış döndürün.

```
or(<expression1>, <expression2>, ...)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*ifade1*>, <*ifade2*>, ... | Evet | Boole | Denetlenen ifadeler |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| true veya false | Boole | En az bir ifade doğru olduğunda doğru döndürün. Tüm ifadeler yanlış olduğunda false döndürün. |
||||

*Örnek 1*

Bu örnekler, en az bir ifadenin doğru olup olmadığını denetler:

```json
or(true, false)
or(false, false)
```

Ve bu sonuçları iade edin:

* İlk örnek: En az bir ifade doğrudur, bu nedenle işlev döndürür. `true`
* İkinci örnek: Her iki ifade de `false`yanlışolduğundan işlev geri döner.

*Örnek 2*

Bu örnekler, en az bir ifadenin doğru olup olmadığını denetler:

```json
or(equals(1, 1), equals(1, 2))
or(equals(1, 2), equals(1, 3))
```

Ve bu sonuçları iade edin:

* İlk örnek: En az bir ifade doğrudur, bu nedenle işlev döndürür. `true`
* İkinci örnek: Her iki ifade de `false`yanlışolduğundan işlev geri döner.

<a name="rand"></a>

### <a name="rand"></a>rand

Yalnızca başlangıç sonunda dahil olan belirli bir aralıktan rasgele bir tamsayı döndürün.

```
rand(<minValue>, <maxValue>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Minvalue*> | Evet | Tamsayı | Aralıktaki en düşük tümseci |
| <*Maxvalue*> | Evet | Tamsayı | İşlevin döndürülebileceği aralıktaki en yüksek tümseci izleyen tümseci |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*rasgele sonuç*> | Tamsayı | Belirtilen aralıktan döndürülen rasgele arayıcı |
||||

*Örnek*

Bu örnek, maksimum değer hariç, belirtilen aralıktan rasgele bir tamsayı alır:

```
rand(1, 5)
```

Ve sonuç olarak bu sayılardan `1` `2`birini `3`döndürür: , , veya`4`

<a name="range"></a>

### <a name="range"></a>aralık

Belirtilen bir tamsededen başlayan bir tamsayı dizini döndürün.

```
range(<startIndex>, <count>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Startındex*> | Evet | Tamsayı | Diziyi ilk öğe olarak başlatan bir sonda değeri |
| <*Sayısı*> | Evet | Tamsayı | Dizideki tümer sayısı |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| [<*aralık lı sonuç*>] | Dizi | Belirtilen dizinden başlayan tümseleri içeren dizi |
||||

*Örnek*

Bu örnek, belirtilen dizinten başlayan ve belirtilen tamsayı sayısını içeren bir tamsayı dizisi oluşturur:

```
range(1, 4)
```

Ve bu sonucu döndürür:`[1, 2, 3, 4]`

<a name="replace"></a>

### <a name="replace"></a>Değiştirmek

Bir alt dizeyi belirtilen dizeyle değiştirin ve sonuç dizesini döndürün. Bu fonksiyon büyük/küçük harf duyarlıdır.

```
replace('<text>', '<oldText>', '<newText>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | Dize | Değiştirilecek alt dize olan dize |
| <*oldText*> | Evet | Dize | Değiştirilecek alt dize |
| <*newText*> | Evet | Dize | Değiştirme dizesi |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*güncelleştirilmiş metin*> | Dize | Alt dize yi değiştirdikten sonra güncelleştirilmiş dize <p>Alt dize bulunamazsa, özgün dizeyi döndürün. |
||||

*Örnek*

Bu örnekte "eski dize"deki "eski" alt dizeyi bulur ve "eski" yerine "yeni" yazar:

```
replace('the old string', 'old', 'new')
```

Ve bu sonucu döndürür:`"the new string"`

<a name="skip"></a>

### <a name="skip"></a>Atla

Öğeleri koleksiyonun önünden kaldırın ve diğer tüm öğeleri iade *edin.*

```
skip([<collection>], <count>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Koleksiyon*> | Evet | Dizi | Öğelerini kaldırmak istediğiniz koleksiyon |
| <*Sayısı*> | Evet | Tamsayı | Önden kaldırılacak öğe sayısı için pozitif bir tamsayı |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| [<*güncelleştirilmiş toplama*>] | Dizi | Belirtilen öğeleri kaldırdıktan sonra güncelleştirilmiş koleksiyon |
||||

*Örnek*

Bu örnek, belirtilen dizinin önünden bir öğeyi, 0 sayısını kaldırır:

```
skip(createArray(0, 1, 2, 3), 1)
```

Ve kalan öğeleri ile bu dizi döndürür:`[1,2,3]`

<a name="split"></a>

### <a name="split"></a>split

Özgün dizede belirtilen sınırlayıcı karakteri temel alarak virgülle ayrılmış alt dizeleri içeren bir dizi döndürün.

```
split('<text>', '<delimiter>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | Dize | Özgün dizede belirtilen sınırlayıcıyı temel alan alt dizeleri ayırmak için dize |
| <*Sınırlayıcı*> | Evet | Dize | Delimiter olarak kullanılacak orijinal dizedeki karakter |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| [<*substring1*>.<*substring2*>,...] | Dizi | Virgüllerle ayrılmış özgün dizeden alt dizeleri içeren bir dizi |
||||

*Örnek*

Bu örnek, delimiter olarak belirtilen karaktere dayalı olarak belirtilen dizeden alt dizeleri olan bir dizi oluşturur:

```
split('a_b_c', '_')
```

Ve sonuç olarak bu dizi döndürür:`["a","b","c"]`

<a name="startOfDay"></a>

### <a name="startofday"></a>başlangıçOfDay

Günün başlangıcını bir zaman damgası için döndürün.

```
startOfDay('<timestamp>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | Dize | Zaman damgasını içeren dize |
| <*Biçim*> | Hayır | Dize | Ya tek bir [biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ya da özel bir [biçim deseni.](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) Zaman damgası için varsayılan [biçim, ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ile uyumlu ve saat dilimi bilgilerini koruyan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-MM-ddTHH:mm:ss:fffffffK) biçimidir. |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*güncelleştirilmiş zaman damgası*> | Dize | Belirtilen zaman damgası ancak gün için sıfır saat işareti başlayarak |
||||

*Örnek*

Bu örnek, bu zaman damgası için günün başlangıcını bulur:

```
startOfDay('2018-03-15T13:30:30Z')
```

Ve bu sonucu döndürür:`"2018-03-15T00:00:00.0000000Z"`

<a name="startOfHour"></a>

### <a name="startofhour"></a>başlangıçOfSaat

Saat ini saat damgası için saatin başlangıcını iade edin.

```
startOfHour('<timestamp>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | Dize | Zaman damgasını içeren dize |
| <*Biçim*> | Hayır | Dize | Ya tek bir [biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ya da özel bir [biçim deseni.](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) Zaman damgası için varsayılan [biçim, ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ile uyumlu ve saat dilimi bilgilerini koruyan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-MM-ddTHH:mm:ss:fffffffK) biçimidir. |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*güncelleştirilmiş zaman damgası*> | Dize | Belirtilen zaman damgası ancak saat için sıfır dakika işaretinden başlayarak |
||||

*Örnek*

Bu örnek, bu zaman damgası için saatin başlangıcını bulur:

```
startOfHour('2018-03-15T13:30:30Z')
```

Ve bu sonucu döndürür:`"2018-03-15T13:00:00.0000000Z"`

<a name="startOfMonth"></a>

### <a name="startofmonth"></a>başlangıçOfMonth

Zaman damgası için ayın başlangıcını iade edin.

```
startOfMonth('<timestamp>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | Dize | Zaman damgasını içeren dize |
| <*Biçim*> | Hayır | Dize | Ya tek bir [biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ya da özel bir [biçim deseni.](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) Zaman damgası için varsayılan [biçim, ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ile uyumlu ve saat dilimi bilgilerini koruyan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-MM-ddTHH:mm:ss:fffffffK) biçimidir. |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*güncelleştirilmiş zaman damgası*> | Dize | Belirtilen zaman damgası ancak sıfır saat işareti yle ayın ilk gününde başlayan |
||||

*Örnek*

Bu örnek, bu zaman damgası için ayın başlangıcını döndürür:

```
startOfMonth('2018-03-15T13:30:30Z')
```

Ve bu sonucu döndürür:`"2018-03-01T00:00:00.0000000Z"`

<a name="startswith"></a>

### <a name="startswith"></a>startsWith

Bir dize belirli bir alt dize ile başlayıp başlamadığını denetleyin.
Alt dize bulunduğunda doğru döndürün veya bulunmadığında false döndürün.
Bu işlev büyük/küçük harf duyarlı değildir.

```
startsWith('<text>', '<searchText>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | Dize | Denetlenen dize |
| <*searchText*> | Evet | Dize | Bulmak için başlangıç dizesi |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| true veya false  | Boole | Başlangıç alt dizesi bulunduğunda doğru döndürün. Bulunduğunda yanlış döndürün. |
||||

*Örnek 1*

Bu örnek, "merhaba dünya" dizesinin "hello" alt dizesiyle başlayıp başlamadığını denetler:

```
startsWith('hello world', 'hello')
```

Ve bu sonucu döndürür:`true`

*Örnek 2*

Bu örnek, "merhaba dünya" dizesinin "selamlar" alt dizesiyle başlayıp başlamadığını denetler:

```
startsWith('hello world', 'greetings')
```

Ve bu sonucu döndürür:`false`

<a name="string"></a>

### <a name="string"></a>string

Bir değer için dize sürümünü döndürün.

```
string(<value>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Herhangi biri | Dönüştürülecek değer |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*dize değeri*> | Dize | Belirtilen değer için dize sürümü |
||||

*Örnek 1*

Bu örnek, bu sayı için dize sürümünü oluşturur:

```
string(10)
```

Ve bu sonucu döndürür:`"10"`

*Örnek 2*

Bu örnek, belirtilen JSON nesnesi için bir dize oluşturur ve çift tırnak işareti (") için bir kaçış karakteri olarak ters eğik çizgi karakteri (\\) kullanır.

```
string( { "name": "Sophie Owen" } )
```

Ve bu sonucu döndürür:`"{ \\"name\\": \\"Sophie Owen\\" }"`

<a name="sub"></a>

### <a name="sub"></a>Alt

İkinci sayıyı ilk sayıdan çıkarmanın sonucunu döndürün.

```
sub(<minuend>, <subtrahend>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*minuend*> | Evet | Sonda veya Float | *Subtrahend* çıkarmak için hangi sayı |
| <*Subtrahend*> | Evet | Sonda veya Float | *Minuend'den* çıkarılabilen sayı |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*Sonuç*> | Sonda veya Float | İkinci sayının ilk sayıdan çıkarılarak elde edilen sonuç |
||||

*Örnek*

Bu örnek, ikinci sayıyı ilk sayıdan çıkarır:

```
sub(10.3, .3)
```

Ve bu sonucu döndürür:`10`

<a name="substring"></a>

### <a name="substring"></a>Dize

Belirtilen konumdan başlayarak karakterleri bir dizeden veya dizinden döndürün.
Dizin değerleri 0 sayısıyla başlar.

```
substring('<text>', <startIndex>, <length>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | Dize | Karakterleri istediğiniz dize |
| <*Startındex*> | Evet | Tamsayı | Başlangıç pozisyonu veya dizin değeri olarak kullanmak istediğiniz 0'a eşit veya daha büyük pozitif bir sayı |
| <*Uzun -luğu*> | Evet | Tamsayı | Alt dizede istediğiniz pozitif karakter sayısı |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*substring sonucu*> | Dize | Kaynak dizedeki belirtilen dizin konumundan başlayarak, belirtilen karakter sayısına sahip bir alt diz |
||||

*Örnek*

Bu örnek, dizin değeri 6 başlayarak, belirtilen dize beş karakterli bir alt dize oluşturur:

```
substring('hello world', 6, 5)
```

Ve bu sonucu döndürür:`"world"`

<a name="subtractFromTime"></a>

### <a name="subtractfromtime"></a>çıkarmaFromTime

Zaman damgasından birkaç zaman birimi çıkarın.
Ayrıca [bakınız GetPastTime](#getPastTime).

```
subtractFromTime('<timestamp>', <interval>, '<timeUnit>', '<format>'?)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | Dize | Zaman damgasını içeren dize |
| <*Aralığı*> | Evet | Tamsayı | Çıkarıiçin belirtilen zaman birimlerinin sayısı |
| <*zaman Birimi*> | Evet | Dize | *Aralıklı*olarak kullanılacak zaman birimi : "İkinci", "Dakika", "Saat", "Gün", "Hafta", "Ay", "Yıl" |
| <*Biçim*> | Hayır | Dize | Ya tek bir [biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ya da özel bir [biçim deseni.](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) Zaman damgası için varsayılan [biçim, ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ile uyumlu ve saat dilimi bilgilerini koruyan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-MM-ddTHH:mm:ss:fffffffK) biçimidir. |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*güncelleştirilmiş zaman damgası*> | Dize | Zaman damgası eksi belirtilen zaman birimleri sayısı |
||||

*Örnek 1*

Bu örnek, bu zaman damgasından bir gün çıkarır:

```
subtractFromTime('2018-01-02T00:00:00Z', 1, 'Day')
```

Ve bu sonucu döndürür:`"2018-01-01T00:00:00:0000000Z"`

*Örnek 2*

Bu örnek, bu zaman damgasından bir gün çıkarır:

```
subtractFromTime('2018-01-02T00:00:00Z', 1, 'Day', 'D')
```

Ve bu sonucu isteğe bağlı "D" biçimini kullanarak döndürür:`"Monday, January, 1, 2018"`

<a name="take"></a>

### <a name="take"></a>almak

Öğeleri koleksiyonun önünden döndürün.

```
take('<collection>', <count>)
take([<collection>], <count>)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Koleksiyon*> | Evet | Dize veya Dizi | Öğeleri istediğiniz koleksiyon |
| <*Sayısı*> | Evet | Tamsayı | Önden istediğiniz öğe sayısı için pozitif tamsayı |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*alt kümesi*> veya [<*alt kümesi*>] | String veya Array, sırasıyla | Özgün koleksiyonun önünden alınan belirtilen öğe sayısına sahip bir dize veya dizi |
||||

*Örnek*

Bu örnekler, bu koleksiyonların önünden belirtilen öğe sayısını alır:

```
take('abcde', 3)
take(createArray(0, 1, 2, 3, 4), 3)
```

Ve bu sonuçları iade edin:

* İlk örnek:`"abc"`
* İkinci örnek:`[0, 1, 2]`

<a name="ticks"></a>

### <a name="ticks"></a>Ticks

Belirtilen `ticks` bir zaman damgası için özellik değerini döndürün.
*Kene* 100 nanosaniyelik bir aralıktır.

```
ticks('<timestamp>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Zaman damgası*> | Evet | Dize | Zaman damgası için dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*keneler-sayı*> | Tamsayı | Belirtilen zaman damgası beri kene sayısı |
||||

<a name="toLower"></a>

### <a name="tolower"></a>Tolower

Küçük harf biçiminde bir dize döndürün. Dizedeki bir karakterin küçük bir sürümü yoksa, bu karakter döndürülen dizede değişmeden kalır.

```
toLower('<text>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | Dize | Küçük harf biçiminde döndürülecek dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*küçük harf-metin*> | Dize | Küçük harf biçiminde orijinal dize |
||||

*Örnek*

Bu örnek, bu dizeyi küçük harfe dönüştürür:

```
toLower('Hello World')
```

Ve bu sonucu döndürür:`"hello world"`

<a name="toUpper"></a>

### <a name="toupper"></a>Toupper

Büyük harf biçiminde bir dize döndürün. Dizedeki bir karakterin büyük harf sürümü yoksa, bu karakter döndürülen dizede değişmeden kalır.

```
toUpper('<text>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | Dize | Büyük harf biçiminde döndürülecek dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*büyük harf-metin*> | Dize | Büyük harf biçiminde orijinal dize |
||||

*Örnek*

Bu örnek, bu dizeyi büyük harfe dönüştürür:

```
toUpper('Hello World')
```

Ve bu sonucu döndürür:`"HELLO WORLD"`

<a name="trim"></a>

### <a name="trim"></a>Döşeme

Satır aralığını ve sondaki beyaz boşluğu bir dizeden kaldırın ve güncelleştirilmiş dizeyi döndürün.

```
trim('<text>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Metin*> | Evet | Dize | Kaldırılacak öncü ve sondaki beyaz alana sahip dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*güncelleştirilmişMetin*> | Dize | Boşluk tuşuna girmeden veya beyaz boşluğu izlemeden özgün dize için güncelleştirilmiş bir sürüm |
||||

*Örnek*

Bu örnek, " Hello World " dizesinden satır aralığı ve sondaki beyaz boşluğu kaldırır:

```
trim(' Hello World  ')
```

Ve bu sonucu döndürür:`"Hello World"`

<a name="union"></a>

### <a name="union"></a>birleşim

Belirtilen koleksiyonlardan *tüm* öğeleri içeren bir koleksiyon döndürün.
Sonuç olarak görünmesi için, bu işleve geçirilen herhangi bir koleksiyonda bir öğe görünebilir. Bir veya daha fazla öğe aynı ada sahipse, bu ada sahip son öğe sonuçta görünür.

```
union('<collection1>', '<collection2>', ...)
union([<collection1>], [<collection2>], ...)
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*collection1*>, <*collection2*>, ...  | Evet | Dizi veya Nesne, ancak her ikisi de | *Tüm* öğeleri istediğiniz yerden koleksiyonlar |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*updatedCollection*> | Dizi veya Nesne, sırasıyla | Belirtilen koleksiyonlardan tüm öğeleri içeren bir koleksiyon - yineleme yok |
||||

*Örnek*

Bu örnek, bu koleksiyonlardan *tüm* öğeleri alır:

```
union(createArray(1, 2, 3), createArray(1, 2, 10, 101))
```

Ve bu sonucu döndürür:`[1, 2, 3, 10, 101]`

<a name="uriComponent"></a>

### <a name="uricomponent"></a>uriComponent

URL güvenli olmayan karakterleri kaçış karakterleriyle değiştirerek, tek düzen kaynak tanımlayıcısı (URI) kodlanmış sürümü bir dize için döndürün.
[UriComponent() kodlayerine](#encodeUriComponent)bu işlevi kullanın.
Her iki fonksiyon da `uriComponent()` aynı şekilde çalışsa da tercih edilir.

```
uriComponent('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize | URI kodlu biçime dönüştürülecek dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*kodlanmış-uri*> | Dize | Kaçış karakterleri ile URI kodlanmış dize |
||||

*Örnek*

Bu örnek, bu dize için URI kodlanmış bir sürüm oluşturur:

```
uriComponent('https://contoso.com')
```

Ve bu sonucu döndürür:`"http%3A%2F%2Fcontoso.com"`

<a name="uriComponentToBinary"></a>

### <a name="uricomponenttobinary"></a>uriComponentToBinary

Tek bir kaynak tanımlayıcısı (URI) bileşeni için ikili sürümü döndürün.

```
uriComponentToBinary('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize | Dönüştürmek için URI kodlanmış dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*ikili-for-kodlanmış-uri*> | Dize | URI kodlanmış dize için ikili sürüm. İkili içerik base64 kodlanır ve `$content`. |
||||

*Örnek*

Bu örnek, URI kodlanmış bu dize için ikili sürümü oluşturur:

```
uriComponentToBinary('http%3A%2F%2Fcontoso.com')
```

Ve bu sonucu döndürür:

`"001000100110100001110100011101000111000000100101001100
11010000010010010100110010010001100010010100110010010001
10011000110110111101101110011101000110111101110011011011
110010111001100011011011110110110100100010"`

<a name="uriComponentToString"></a>

### <a name="uricomponenttostring"></a>uriComponentToString

Uri kodlu dizeyi etkili bir şekilde çözen tek tip bir kaynak tanımlayıcısı (URI) kodlanmış dize için dize sürümünü döndürün.

```
uriComponentToString('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize | Decode için URI kodlanmış dize |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*deşifre-uri*> | Dize | URI kodlanmış dize için kodlanmış sürümü |
||||

*Örnek*

Bu örnek, URI tarafından kodlanmış bu dize için deşifre edilmiş dize sürümünü oluşturur:

```
uriComponentToString('http%3A%2F%2Fcontoso.com')
```

Ve bu sonucu döndürür:`"https://contoso.com"`

<a name="utcNow"></a>

### <a name="utcnow"></a>utcŞimdi

Geçerli zaman damgasını döndürün.

```
utcNow('<format>')
```

İsteğe bağlı olarak, <*biçimi*> parametresi ile farklı bir biçim belirtebilirsiniz.

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Biçim*> | Hayır | Dize | Ya tek bir [biçim belirticisi](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) ya da özel bir [biçim deseni.](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings) Zaman damgası için varsayılan [biçim, ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) ile uyumlu ve saat dilimi bilgilerini koruyan ["o"](https://docs.microsoft.com/dotnet/standard/base-types/standard-date-and-time-format-strings) (yyyy-MM-ddTHH:mm:ss:fffffffK) biçimidir. |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*geçerli zaman damgası*> | Dize | Geçerli tarih ve saat |
||||

*Örnek 1*

Bugün 15 Nisan 2018 saat 13:00'de olduğunu varsayalım.
Bu örnek, geçerli zaman damgasını alır:

```
utcNow()
```

Ve bu sonucu döndürür:`"2018-04-15T13:00:00.0000000Z"`

*Örnek 2*

Bugün 15 Nisan 2018 saat 13:00'de olduğunu varsayalım.
Bu örnek, isteğe bağlı "D" biçimini kullanarak geçerli zaman damgasını alır:

```
utcNow('D')
```

Ve bu sonucu döndürür:`"Sunday, April 15, 2018"`

<a name="xml"></a>

### <a name="xml"></a>xml

JSON nesnesi içeren bir dize için XML sürümünü döndürün.

```
xml('<value>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Değer*> | Evet | Dize | Dönüştürmek için JSON nesnesi ile dize <p>JSON nesnesinin yalnızca bir kök özelliği olmalıdır, bu da bir dizi olamaz. <br>Çift tırnak işareti\\(") için bir kaçış karakteri olarak ters eğik çizgi karakterini ( ) kullanın. |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*xml versiyonu*> | Nesne | Belirtilen dize veya JSON nesnesi için kodlanmış XML |
||||

*Örnek 1*

Bu örnek, bir JSON nesnesi içeren bu dize için XML sürümünü oluşturur:

`xml(json('{ \"name\": \"Sophia Owen\" }'))`

Ve bu sonucu XML döndürür:

```xml
<name>Sophia Owen</name>
```

*Örnek 2*

Bu JSON nesnesi olduğunu varsayalım:

```json
{
  "person": {
    "name": "Sophia Owen",
    "city": "Seattle"
  }
}
```

Bu örnek, bu JSON nesnesi içeren bir dize için XML oluşturur:

`xml(json('{\"person\": {\"name\": \"Sophia Owen\", \"city\": \"Seattle\"}}'))`

Ve bu sonucu XML döndürür:

```xml
<person>
  <name>Sophia Owen</name>
  <city>Seattle</city>
<person>
```

<a name="xpath"></a>

### <a name="xpath"></a>Xpath

XML'yi, XPath (XML Path Language) ifadesiyle eşleşen düğümler veya değerler için denetleyin ve eşleşen düğümleri veya değerleri döndürün. XPath ifadesi veya yalnızca "XPath", XML içeriğinde düğümleri veya işlem değerlerini seçebilmeniz için Bir XML belge yapısında gezinmenize yardımcı olur.

```
xpath('<xml>', '<xpath>')
```

| Parametre | Gerekli | Tür | Açıklama |
| --------- | -------- | ---- | ----------- |
| <*Xml*> | Evet | Herhangi biri | XPath ifade değeriyle eşleşen düğümleri veya değerleri aramak için XML dizesi |
| <*Xpath*> | Evet | Herhangi biri | Eşleşen XML düğümlerini veya değerlerini bulmak için kullanılan XPath ifadesi |
|||||

| Döndürülen değer | Tür | Açıklama |
| ------------ | ---- | ----------- |
| <*xml düğümü*> | XML | Yalnızca tek bir düğüm belirtilen XPath ifadesiyle eşleştiğinde bir XML düğümü |
| <*Değer*> | Herhangi biri | Yalnızca tek bir değer belirtilen XPath ifadesiyle eşleştiğinde XML düğümündeki değer |
| [<*xml-düğüm1*>, <*xml-düğüm2*>, ...] </br>-veya- </br>[<*değer1*>, <*değeri2*>, ...] | Dizi | XML düğümleri veya değerleri belirtilen XPath ifadesiyle eşleşen bir dizi |
||||

*Örnek 1*

Örnek 1'de aşağıdaki örnek, düğümle `<count></count>` eşleşen düğümleri bulur ve bu `sum()` düğüm değerlerini işleve ekler:

`xpath(xml(parameters('items')), 'sum(/produce/item/count)')`

Ve bu sonucu döndürür:`30`

*Örnek 2*

Bu örnekte, her iki ifadede de, xml ile ad alanı içeren belirtilen bağımsız değişkenlerde `<location></location>` düğümle eşleşen düğümler bulur. İfadeler, çift tırnak işareti\\(") için bir kaçış karakteri olarak ters eğik çizgi karakteri ( ) kullanır.

* *İfade 1*

  `xpath(xml(body('Http')), '/*[name()=\"file\"]/*[name()=\"location\"]')`

* *İfade 2*

  `xpath(xml(body('Http')), '/*[local-name()=\"file\" and namespace-uri()=\"http://contoso.com\"]/*[local-name()=\"location\"]')`

İşte argümanlar şunlardır:

* XML belge ad alanını içeren bu `xmlns="http://contoso.com"`XML:

  ```xml
  <?xml version="1.0"?> <file xmlns="http://contoso.com"> <location>Paris</location> </file>
  ```

* Ya XPath ifade burada:

  * `/*[name()=\"file\"]/*[name()=\"location\"]`

  * `/*[local-name()=\"file\" and namespace-uri()=\"http://contoso.com\"]/*[local-name()=\"location\"]`

Düğümle eşleşen sonuç düğümü `<location></location>` aşağıda veda edebilirsiniz:

```xml
<location xmlns="https://contoso.com">Paris</location>
```

*Örnek 3*

Örnek 3'te aşağıdaki örnek, `<location></location>` düğümdeki değeri bulur:

`xpath(xml(body('Http')), 'string(/*[name()=\"file\"]/*[name()=\"location\"])')`

Ve bu sonucu döndürür:`"Paris"`

## <a name="next-steps"></a>Sonraki adımlar
İfadelerde kullanabileceğiniz sistem değişkenlerinin listesi için [Bkz. Sistem değişkenleri.](control-flow-system-variables.md)
