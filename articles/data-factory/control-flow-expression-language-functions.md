---
title: Azure Data Factory | ifade ve işlevler | Microsoft Docs
description: Bu makalede, Data Factory varlıkları oluştururken kullanabileceğiniz ifadeler ve işlevler hakkında bilgi sağlanır.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
ms.openlocfilehash: 533d0c7461530a00e6f640ee53c0c87d336e799d
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876323"
---
# <a name="expressions-and-functions-in-azure-data-factory"></a>Azure Data Factory’deki ifadeler ve işlevler
> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-functions-variables.md)
> * [Geçerli sürüm](control-flow-expression-language-functions.md)

Bu makale, Azure Data Factory tarafından desteklenen ifadeler ve işlevlerle ilgili ayrıntıları sağlar. 

## <a name="introduction"></a>Giriş
Tanımdaki JSON değerleri, çalışma zamanında değerlendirilen bir sabit değer veya ifadeler olabilir. Örneğin:  
  
```json
"name": "value"
```

 veya  
  
```json
"name": "@pipeline().parameters.password"
```

## <a name="expressions"></a>İfadeler  
İfadeler JSON dize değerinde herhangi bir yerde görünebilir ve her zaman başka bir JSON değerine neden olabilir. JSON değeri bir ifadesiyse, ifadenin gövdesi at-Sign (\@) kaldırılarak ayıklanır. İle başlayan bir sabit değer dizesi gerekliyse, kullanılarak \@ \@ \@kaçışlı olması gerekir. Aşağıdaki örneklerde ifadelerin nasıl değerlendirildiği gösterilmektedir.  
  
|JSON değeri|Sonuç|  
|----------------|------------|  
|parametrelere|' Parameters ' karakterleri döndürülür.|  
|"parametreler [1]"|' Parameters [1] ' karakterleri döndürülür.|  
|"\@\@"|'\@' İçeren 1 karakterlik bir dize döndürüldü.|  
|" \@"|' \@' İçeren 2 karakterlik bir dize döndürüldü.|  
  
 İfadeler, ifadelerin Sarmalanan `@{ ... }` *dize ilişkilendirme* adlı bir özellik kullanılarak dizeler içinde de görünebilir. Örneğin, `"name" : "First Name: @{pipeline().parameters.firstName} Last Name: @{pipeline().parameters.lastName}"`  
  
 Dize ilişkilendirmeyi kullanarak, sonuç her zaman bir dizedir. Şu şekilde tanımladım `myNumber`: `42` `myString` `foo`  
  
|JSON değeri|Sonuç|  
|----------------|------------|  
|"\@Pipeline (). Parameters. MyString"| Bir `foo` dize olarak döndürür.|  
|"\@{Pipeline (). Parameters. MyString}"| Bir `foo` dize olarak döndürür.|  
|"\@işlem hattı (). Parameters. MyNumber"| Sayı `42` olarak döndürür.|  
|"\@{Pipeline (). Parameters. MyNumber}"| Bir `42` *dize*olarak döndürür.|  
|"Yanıt: @ {Pipeline (). Parameters. myNumber}"| Dizeyi `Answer is: 42`döndürür.|  
|"\@Concat (' yanıt: ', dize (ardışık düzen (). Parameters. MyNumber))"| Dizeyi döndürür`Answer is: 42`|  
|"Yanıt: \@ \@{Pipeline (). Parameters. MyNumber}"| Dizeyi `Answer is: @{pipeline().parameters.myNumber}`döndürür.|  
  
### <a name="examples"></a>Örnekler

#### <a name="a-dataset-with-a-parameter"></a>Parametresi olan bir veri kümesi
Aşağıdaki örnekte, BlobDataset, **Path**adlı bir parametre alır. Değeri, şu ifadeyi kullanarak **FolderPath** özelliği için bir değer ayarlamak için kullanılır: `dataset().path`. 

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

#### <a name="a-pipeline-with-a-parameter"></a>Parametresi olan bir işlem hattı
Aşağıdaki örnekte, işlem hattı **inputPath** ve **OutputPath** parametrelerini alır. Parametreli blob veri kümesinin **yolu** , bu parametrelerin değerleri kullanılarak ayarlanır. Burada kullanılan sözdizimi: `pipeline().parameters.parametername`. 

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
#### <a name="tutorial"></a>Öğretici
Bu [öğreticide](https://azure.microsoft.com/mediahandler/files/resourcefiles/azure-data-factory-passing-parameters/Azure%20data%20Factory-Whitepaper-PassingParameters.pdf) , bir işlem hattı ve etkinlik arasında ve Etkinlikler arasında parametrelerin nasıl geçirileceğini adım adım gösterilmektedir.

  
## <a name="functions"></a>İşlevler  
 İfadeler içindeki işlevleri çağırabilirsiniz. Aşağıdaki bölümler, bir ifadede kullanılabilecek işlevlerle ilgili bilgiler sağlar.  

## <a name="string-functions"></a>Dize işlevleri  
 Aşağıdaki işlevler yalnızca dizeler için geçerlidir. Ayrıca, dizeler üzerinde bir dizi koleksiyon işlevini de kullanabilirsiniz.  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|Concat|Herhangi bir sayıda dizeyi birlikte birleştirir. Örneğin, parametre1 `foo,` ise aşağıdaki ifade `somevalue-foo-somevalue`döndürülür:`concat('somevalue-',pipeline().parameters.parameter1,'-somevalue')`<br /><br /> **Parametre numarası**: 1... *n*<br /><br /> **Ad**: Dize *n*<br /><br /> **Açıklama**: Gerekli. Tek bir dize içinde Birleştirilecek dizeler.|  
|dizeden|Dizeden bir karakter alt kümesini döndürür. Örneğin, aşağıdaki ifade:<br /><br /> `substring('somevalue-foo-somevalue',10,3)`<br /><br /> döndürdüğü<br /><br /> `foo`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. Alt dizenin alındığı dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Başlangıç dizini<br /><br /> **Açıklama**: Gerekli. Alt dizenin 1. parametrede başladığı dizin. Başlangıç dizini sıfır tabanlıdır. <br /><br /> **Parametre numarası**: 3<br /><br /> **Ad**: Uzunluk<br /><br /> **Açıklama**: Gerekli. Alt dizenin uzunluğu.|  
|değiştirin|Bir dizeyi verilen dizeyle değiştirir. Örneğin, ifadesi:<br /><br /> `replace('the old string', 'old', 'new')`<br /><br /> döndürdüğü<br /><br /> `the new string`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: dize<br /><br /> **Açıklama**: Gerekli.  Parametre 1 ' de parametre 2 ' de bulunursa, 2 parametresi için aranan ve parametre 3 ile güncellenen dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Eski dize<br /><br /> **Açıklama**: Gerekli. Parametre 1 ' de eşleşme bulunduğunda parametre 3 ile değiştirilecek dize<br /><br /> **Parametre numarası**: 3<br /><br /> **Ad**: Yeni dize<br /><br /> **Açıklama**: Gerekli. Parametre 1 ' de eşleşme bulunduğunda parametre 2 ' deki dizeyi değiştirmek için kullanılan dize.|  
|guid| Genel olarak benzersiz bir dize (aka) oluşturur. GUID). Örneğin, aşağıdaki çıktı oluşturulabilir `c2ecc88d-88c8-4096-912c-d6f2e2b138ce`:<br /><br /> `guid()`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Biçimi<br /><br /> **Açıklama**: İsteğe bağlı. [Bu GUID 'in değerinin nasıl biçimlendirileceğini](https://msdn.microsoft.com/library/97af8hh4%28v=vs.110%29.aspx)belirten tek bir Biçim belirleyicisi. Biçim parametresi "N", "D", "B", "P" veya "X" olabilir. Biçim sağlanmazsa, "D" kullanılır.|  
|toLower|Dizeyi küçük harfe dönüştürür. Örneğin, aşağıdakiler aşağıdaki gibi döndürülür `two by two is four`:`toLower('Two by Two is Four')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. Küçük harfe dönüştürülecek dize. Dizedeki bir karakterin küçük harfli bir eşdeğeri yoksa, döndürülen dizede değişmeden bulunur.|  
|toUpper|Dizeyi büyük harfe dönüştürür. Örneğin, aşağıdaki ifade şunu döndürür `TWO BY TWO IS FOUR`:`toUpper('Two by Two is Four')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. Büyük harfe dönüştürülecek dize. Dizedeki bir karakterin büyük harfli bir eşdeğeri yoksa, döndürülen dizede değişmeden bulunur.|  
|IndexOf|Bir dize insensitively dizesi içindeki bir değerin dizinini bulur. Dizin sıfır tabanlıdır. Örneğin, aşağıdaki ifade şunu döndürür `7`:`indexof('hello, world.', 'world')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. Değeri içerebilen dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. Dizininde arama yapılacak değer.|  
|LastIndexOf|Bir dize insensitively dize içinde bir değerin son dizinini bulur. Dizin sıfır tabanlıdır. Örneğin, aşağıdaki ifade şunu döndürür `3`:`lastindexof('foofoo', 'foo')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. Değeri içerebilen dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. Dizininde arama yapılacak değer.|  
|StartsWith|Dizenin insensitively bir değer durumuyla başlıyorsa denetler. Örneğin, aşağıdaki ifade şunu döndürür `true`:`startswith('hello, world', 'hello')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. Değeri içerebilen dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. Dize ile başlayabiliriz değeri.|  
|EndsWith|Dizenin insensitively bir değer durumuyla bitmediğini denetler. Örneğin, aşağıdaki ifade şunu döndürür `true`:`endswith('hello, world', 'world')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. Değeri içerebilen dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. Dizenin ile biteme değeri.|  
|split|Dizeyi ayırıcı kullanarak böler. Örneğin, aşağıdaki ifade şunu döndürür `["a", "b", "c"]`:`split('a;b;c',';')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. Bölünen dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. Ayırıcı.|  
  
  
## <a name="collection-functions"></a>Koleksiyon işlevleri  
 Bu işlevler diziler, dizeler ve bazen sözlükler gibi koleksiyonlar üzerinde çalışır.  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|içerir|Sözlük bir anahtar içeriyorsa, liste değeri içeriyorsa veya dize alt dize içeriyorsa true döndürür. Örneğin, aşağıdaki ifade döndürür`true:``contains('abacaba','aca')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Koleksiyon içinde<br /><br /> **Açıklama**: Gerekli. İçinde arama yapılacak koleksiyon.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Nesne bul<br /><br /> **Açıklama**: Gerekli. **İçindeki koleksiyon**içinde bulunacak nesne.|  
|length|Bir dizideki veya dizedeki öğelerin sayısını döndürür. Örneğin, aşağıdaki ifade şunu döndürür `3`:`length('abc')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Collection<br /><br /> **Açıklama**: Gerekli. Uzunluğunu almak için koleksiyon.|  
|boş|Nesne, dizi veya dize boşsa true döndürür. Örneğin, aşağıdaki ifade şunu döndürür `true`:<br /><br /> `empty('')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Collection<br /><br /> **Açıklama**: Gerekli. Boş olup olmadığını denetlemek için koleksiyon.|  
|imin|Kendisine geçirilen diziler veya nesneler arasında ortak öğelerle tek bir dizi veya nesne döndürür. Örneğin, bu işlev şunu döndürür `[1, 2]`:<br /><br /> `intersection([1, 2, 3], [101, 2, 1, 10],[6, 8, 1, 2])`<br /><br /> İşlevin parametreleri bir dizi nesne veya dizi kümesi olabilir (bir karışımı değil). Aynı ada sahip iki nesne varsa, bu adı taşıyan son nesne son nesnede görüntülenir.<br /><br /> **Parametre numarası**: 1... *n*<br /><br /> **Ad**: Koleksiyon *n*<br /><br /> **Açıklama**: Gerekli. Değerlendirilecek Koleksiyonlar. Bir nesne, sonuçta görünmesi için geçirilen tüm koleksiyonlar içinde olmalıdır.|  
|union|Bir dizi ya da nesne içindeki tüm öğeleri içeren tek bir dizi veya nesne döndürür. Örneğin, bu işlev döndürür`[1, 2, 3, 10, 101]:`<br /><br /> :  `union([1, 2, 3], [101, 2, 1, 10])`<br /><br /> İşlevin parametreleri bir dizi nesne veya dizi kümesi olabilir (bir karışımı değil). Son çıktıda aynı ada sahip iki nesne varsa, bu adı taşıyan son nesne son nesnede görüntülenir.<br /><br /> **Parametre numarası**: 1... *n*<br /><br /> **Ad**: Koleksiyon *n*<br /><br /> **Açıklama**: Gerekli. Değerlendirilecek Koleksiyonlar. Koleksiyonlardan herhangi birinde görüntülenen bir nesne, sonuçta görünür.|  
|adı|Geçirilen dizideki veya dizedeki ilk öğeyi döndürür. Örneğin, bu işlev şunu döndürür `0`:<br /><br /> `first([0,2,3])`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Collection<br /><br /> **Açıklama**: Gerekli. İlk nesneyi almak için koleksiyon.|  
|soyadına|Geçirilen dizideki veya dizedeki son öğeyi döndürür. Örneğin, bu işlev şunu döndürür `3`:<br /><br /> `last('0123')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Collection<br /><br /> **Açıklama**: Gerekli. Son nesneyi almak için koleksiyon.|  
|take|Geçirilen dizi veya dizeden ilk **sayı** öğelerini döndürür, örneğin, bu işlev şunu döndürür `[1, 2]`:`take([1, 2, 3, 4], 2)`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Collection<br /><br /> **Açıklama**: Gerekli. İlk **sayı** nesnesini almak için koleksiyon.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Count<br /><br /> **Açıklama**: Gerekli. **Koleksiyondan**gerçekleştirilecek nesne sayısı. Pozitif bir tamsayı olmalıdır.|  
|atla|Dizin **sayısıyla**başlayan dizideki öğeleri döndürür, örneğin bu işlev şunu döndürür `[3, 4]`:<br /><br /> `skip([1, 2 ,3 ,4], 2)`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Collection<br /><br /> **Açıklama**: Gerekli. İlk **Count** nesnesini atlanacak koleksiyon.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Count<br /><br /> **Açıklama**: Gerekli. **Koleksiyonun**önünden kaldırılacak nesne sayısı. Pozitif bir tamsayı olmalıdır.|  
  
## <a name="logical-functions"></a>Mantıksal işlevler  
 Bu işlevler, koşullar içinde yararlı olduğundan, her türlü mantığı değerlendirmek için kullanılabilirler.  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|eşittir|İki değer eşitse true değerini döndürür. Örneğin, parametre1 foo ise aşağıdaki ifade döndürülür `true`:`equals(pipeline().parameters.parameter1), 'foo')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Nesne 1<br /><br /> **Açıklama**: Gerekli. **Nesne 2**ile Karşılaştırılacak nesne.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Nesne 2<br /><br /> **Açıklama**: Gerekli. **Nesne 1**ile Karşılaştırılacak nesne.|  
|less|İlk bağımsız değişken ikinciden küçükse true döndürür. Değer yalnızca Integer, float veya String türünde olabilir. Örneğin, aşağıdaki ifade şunu döndürür `true`:`less(10,100)`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Nesne 1<br /><br /> **Açıklama**: Gerekli. **Nesne 2**' den daha az olup olmadığını Denetlenecek nesne.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Nesne 2<br /><br /> **Açıklama**: Gerekli. **Nesne 1**' den büyük olup olmadığını Denetlenecek nesne.|  
|Lessotalals|İlk bağımsız değişken ikinciden küçükse true döndürür. Değer yalnızca Integer, float veya String türünde olabilir. Örneğin, aşağıdaki ifade şunu döndürür `true`:`lessOrEquals(10,10)`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Nesne 1<br /><br /> **Açıklama**: Gerekli. **Nesne 2**' ye eşit veya ondan daha küçükse Denetlenecek nesne.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Nesne 2<br /><br /> **Açıklama**: Gerekli. **Nesne 1**' den büyük veya ona eşit olup olmadığını Denetlenecek nesne.|  
|ilerisi|İlk bağımsız değişken ikinciden büyükse true değerini döndürür. Değer yalnızca Integer, float veya String türünde olabilir. Örneğin, aşağıdaki ifade şunu döndürür `false`:`greater(10,10)`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Nesne 1<br /><br /> **Açıklama**: Gerekli. **Nesne 2**' den büyükse Denetlenecek nesne.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Nesne 2<br /><br /> **Açıklama**: Gerekli. **Nesne 1**' den daha az olup olmadığını kontrol etmek için nesne.|  
|greaterOrEquals|İlk bağımsız değişken ikinciden büyükse ya da eşitse true değerini döndürür. Değer yalnızca Integer, float veya String türünde olabilir. Örneğin, aşağıdaki ifade şunu döndürür `false`:`greaterOrEquals(10,100)`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Nesne 1<br /><br /> **Açıklama**: Gerekli. **Nesne 2**' den büyük veya ona eşit olup olmadığını Denetlenecek nesne.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Nesne 2<br /><br /> **Açıklama**: Gerekli. **Nesne 1**' den küçük veya ona eşit olup olmadığını Denetlenecek nesne.|  
|and|Parametrelerin her ikisi de true olduğunda true döndürür. Her iki bağımsız değişkenin de Boole olması gerekir. Aşağıdakiler aşağıdaki gibi `false`döndürülür:`and(greater(1,10),equals(0,0))`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Boole 1<br /><br /> **Açıklama**: Gerekli. Olması `true`gereken ilk bağımsız değişken.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Boole 2<br /><br /> **Açıklama**: Gerekli. İkinci bağımsız değişken `true`olmalıdır.|  
|veya|Parametrelerden biri true olduğunda true döndürür. Her iki bağımsız değişkenin de Boole olması gerekir. Aşağıdakiler aşağıdaki gibi `true`döndürülür:`or(greater(1,10),equals(0,0))`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Boole 1<br /><br /> **Açıklama**: Gerekli. Olabilecek ilk bağımsız değişken `true`.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Boole 2<br /><br /> **Açıklama**: Gerekli. İkinci bağımsız değişken olabilir `true`.|  
|başlatılmadı|Parametresi ise true döndürür `false`. Her iki bağımsız değişkenin de Boole olması gerekir. Aşağıdakiler aşağıdaki gibi `true`döndürülür:`not(contains('200 Success','Fail'))`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Boole değeri<br /><br /> **Açıklama**: Parametresi ise true döndürür `false`. Her iki bağımsız değişkenin de Boole olması gerekir. Aşağıdakiler aşağıdaki gibi `true`döndürülür:`not(contains('200 Success','Fail'))`|  
|if|Girilen ifadenin `true` veya `false`ile sonuçlanmasına göre belirtilen bir değeri döndürür.  Örneğin, aşağıdakiler aşağıdaki gibi döndürülür `"yes"`:`if(equals(1, 1), 'yes', 'no')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: İfade<br /><br /> **Açıklama**: Gerekli. İfadenin hangi değerin döndürüleceğini belirleyen bir Boolean değer.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Doğru<br /><br /> **Açıklama**: Gerekli. İfade ise döndürülecek değer `true`.<br /><br /> **Parametre numarası**: 3<br /><br /> **Ad**: False<br /><br /> **Açıklama**: Gerekli. İfade ise döndürülecek değer `false`.|  
  
## <a name="conversion-functions"></a>Dönüşüm işlevleri  
 Bu işlevler, dildeki yerel türlerin her biri arasında dönüştürme yapmak için kullanılır:  
  
-   dize  
  
-   integer  
  
-   float  
  
-   boolean  
  
-   dizi  
  
-   sözlüğü  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|int|Parametreyi bir tamsayıya dönüştürün. Örneğin, aşağıdaki ifade bir dize yerine 100 olarak bir sayı döndürür:`int('100')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Value<br /><br /> **Açıklama**: Gerekli. Tamsayıya dönüştürülen değer.|  
|dize|Parametreyi bir dizeye Dönüştür. Örneğin, aşağıdaki ifade şunu döndürür `'10'`:  `string(10)`Ayrıca, bir nesneyi dizeye dönüştürebilirsiniz, örneğin, **foo** parametresi bir özelliği `bar : baz`olan bir nesne ise, aşağıdakiler döndürülür `{"bar" : "baz"}``string(pipeline().parameters.foo)`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Value<br /><br /> **Açıklama**: Gerekli. Bir dizeye dönüştürülen değer.|  
|json|Parametreyi bir JSON türü değerine dönüştürür. Dizenin tersidir (). Örneğin, aşağıdaki ifade bir dize yerine `[1,2,3]` bir dizi olarak döner:<br /><br /> `json('[1,2,3]')`<br /><br /> Benzer şekilde, bir dizeyi nesnesine dönüştürebilirsiniz. Örneğin, şunu `json('{"bar" : "baz"}')` döndürür:<br /><br /> `{ "bar" : "baz" }`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. Yerel bir tür değerine dönüştürülen dize.<br /><br /> JSON işlevi xml girişini de destekler. Örneğin, öğesinin parametre değeri:<br /><br /> `<?xml version="1.0"?> <root>   <person id='1'>     <name>Alan</name>     <occupation>Engineer</occupation>   </person> </root>`<br /><br /> Şu JSON 'a dönüştürüldü:<br /><br /> `{ "?xml": { "@version": "1.0" },   "root": {     "person": [     {       "@id": "1",       "name": "Alan",       "occupation": "Engineer"     }   ]   } }`|  
|float|Parametre bağımsız değişkenini bir kayan noktalı sayıya dönüştürün. Örneğin, aşağıdaki ifade şunu döndürür `10.333`:`float('10.333')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Value<br /><br /> **Açıklama**: Gerekli. Kayan noktalı sayıya dönüştürülen değer.|  
|bool|Parametreyi Boole değerine dönüştürür. Örneğin, aşağıdaki ifade şunu döndürür `false`:`bool(0)`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Value<br /><br /> **Açıklama**: Gerekli. Boole değerine dönüştürülen değer.|  
|coalesce|Geçirilen bağımsız değişkenlerde ilk null olmayan nesneyi döndürür. Not: boş bir dize null değil. Örneğin, 1 ve 2. parametreler tanımlanmamışsa, şunu döndürür `fallback`:`coalesce(pipeline().parameters.parameter1', pipeline().parameters.parameter2 ,'fallback')`<br /><br /> **Parametre numarası**: 1... *n*<br /><br /> **Ad**: Nesne*n*<br /><br /> **Açıklama**: Gerekli. Denetlenecek `null`nesneler.|  
|biçiminde|Giriş dizesinin Base64 gösterimini döndürür. Örneğin, aşağıdaki ifade şunu döndürür `c29tZSBzdHJpbmc=`:`base64('some string')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Dize 1<br /><br /> **Açıklama**: Gerekli. Base64 gösterimine Kodlanacak dize.|  
|base64ToBinary|Base64 ile kodlanmış bir dizenin ikili gösterimini döndürür. Örneğin, aşağıdaki ifade bazı bir dizenin ikili gösterimini döndürür: `base64ToBinary('c29tZSBzdHJpbmc=')`.<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. Base64 kodlamalı dize.|  
|base64ToString|Based64 kodlamalı bir dizenin dize gösterimini döndürür. Örneğin, aşağıdaki ifade bir dize döndürür: `base64ToString('c29tZSBzdHJpbmc=')`.<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. Base64 kodlamalı dize.|  
|Binary|Bir değerin ikili gösterimini döndürür.  Örneğin, aşağıdaki ifade bir dizenin ikili gösterimini döndürür:`binary(‘some string’).`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Value<br /><br /> **Açıklama**: Gerekli. İkiliye dönüştürülmüş değer.|  
|dataUriToBinary|Bir veri URI 'sinin ikili gösterimini döndürür. Örneğin, aşağıdaki ifade bir dizenin ikili gösterimini döndürür:`dataUriToBinary('data:;base64,c29tZSBzdHJpbmc=')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. İkili gösterimine dönüştürülecek veri URI 'SI.|  
|dataUriToString|Bir veri URI 'sinin dize gösterimini döndürür. Örneğin, aşağıdaki ifade bir dize döndürür:`dataUriToString('data:;base64,c29tZSBzdHJpbmc=')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Dize<br /><br />**Açıklama**: Gerekli. Dize gösterimine dönüştürülecek veri URI 'SI.|  
|dataUri|Bir değerin veri URI 'sini döndürür. Örneğin, aşağıdaki ifade verileri döndürür:`text/plain;charset=utf8;base64,c29tZSBzdHJpbmc=: dataUri('some string')`<br /><br /> **Parametre numarası**: 1.<br /><br />**Ad**: Value<br /><br />**Açıklama**: Gerekli. Veri URI 'sine dönüştürülecek değer.|  
|decodeBase64|Giriş based64 dizesinin dize gösterimini döndürür. Örneğin, aşağıdaki ifade şunu döndürür `some string`:`decodeBase64('c29tZSBzdHJpbmc=')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Giriş based64 dizesinin dize gösterimini döndürür.|  
|encodeURIComponent|URL-geçirilen dizeyi çıkar. Örneğin, aşağıdaki ifade şunu döndürür `You+Are%3ACool%2FAwesome`:`encodeUriComponent('You Are:Cool/Awesome')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. URL 'den çıkmak için dize-güvenli olmayan karakterler.|  
|decodeURIComponent|URL kaldır-geçilen dizeyi çıkar. Örneğin, aşağıdaki ifade şunu döndürür `You Are:Cool/Awesome`:`encodeUriComponent('You+Are%3ACool%2FAwesome')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. URL kodunun çözülmesi için dize-güvenli olmayan karakterler.|  
|decodeDataUri|Giriş verisi URI dizesinin ikili gösterimini döndürür. Örneğin, aşağıdaki ifade öğesinin `some string`ikili gösterimini döndürür:`decodeDataUri('data:;base64,c29tZSBzdHJpbmc=')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Dize<br /><br /> **Açıklama**: Gerekli. Bir ikili gösterimde çözülecek dataURI.|  
|URIComponent|Bir değerin URI kodlamalı gösterimini döndürür. Örneğin, aşağıdaki ifade döndürür`You+Are%3ACool%2FAwesome: uriComponent('You Are:Cool/Awesome ')`<br /><br /> Parametre ayrıntıları: Sayısından 1, ad: Dize, Açıklama: Gerekli. URI kodlamalı olan dize.|  
|Urıonenttobinary|URI kodlamalı bir dizenin ikili gösterimini döndürür. Örneğin, aşağıdaki ifade öğesinin `You Are:Cool/Awesome`ikili gösterimini döndürür:`uriComponentToBinary('You+Are%3ACool%2FAwesome')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Dize<br /><br />**Açıklama**: Gerekli. URI kodlamalı dize.|  
|Urıonenttostring|URI kodlamalı bir dizenin dize gösterimini döndürür. Örneğin, aşağıdaki ifade şunu döndürür `You Are:Cool/Awesome`:`uriComponentToString('You+Are%3ACool%2FAwesome')`<br /><br /> **Parametre numarası**: 1.<br /><br />**Ad**: Dize<br /><br />**Açıklama**: Gerekli. URI kodlamalı dize.|  
|xml|Değerin bir XML gösterimini döndürür. Örneğin, aşağıdaki ifade tarafından `'\<name>Alan\</name>'`temsil edilen bir XML içeriği döndürür:. `xml('\<name>Alan\</name>')` XML işlevi JSON nesne girişini de destekler. Örneğin, parametresi `{ "abc": "xyz" }` bir XML içeriğine dönüştürülür`\<abc>xyz\</abc>`<br /><br /> **Parametre numarası**: 1.<br /><br />**Ad**: Value<br /><br />**Açıklama**: Gerekli. XML 'e dönüştürülecek değer.|  
|XPath|XPath ifadesinin değerlendirdiği bir değerin XPath ifadesiyle eşleşen bir XML düğümleri dizisi döndürür.<br /><br />  **Örnek 1**<br /><br /> ' P1 ' parametresinin değerinin aşağıdaki XML 'in bir dize temsili olduğunu varsayın:<br /><br /> `<?xml version="1.0"?> <lab>   <robot>     <parts>5</parts>     <name>R1</name>   </robot>   <robot>     <parts>8</parts>     <name>R2</name>   </robot> </lab>`<br /><br /> 1. Bu kod:`xpath(xml(pipeline().parameters.p1), '/lab/robot/name')`<br /><br /> döndürür<br /><br /> `[ <name>R1</name>, <name>R2</name> ]`<br /><br /> leri<br /><br /> 2. Bu kod:`xpath(xml(pipeline().parameters.p1, ' sum(/lab/robot/parts)')`<br /><br /> döndürür<br /><br /> `13`<br /><br /> <br /><br /> **Örnek 2**<br /><br /> Aşağıdaki XML içeriği veriliyor:<br /><br /> `<?xml version="1.0"?> <File xmlns="http://foo.com">   <Location>bar</Location> </File>`<br /><br /> 1.  Bu kod:`@xpath(xml(body('Http')), '/*[name()=\"File\"]/*[name()=\"Location\"]')`<br /><br /> veya<br /><br /> 2. Bu kod:`@xpath(xml(body('Http')), '/*[local-name()=\"File\" and namespace-uri()=\"http://foo.com\"]/*[local-name()=\"Location\" and namespace-uri()=\"\"]')`<br /><br /> döndürdüğü<br /><br /> `<Location xmlns="http://foo.com">bar</Location>`<br /><br /> and<br /><br /> 3. Bu kod:`@xpath(xml(body('Http')), 'string(/*[name()=\"File\"]/*[name()=\"Location\"])')`<br /><br /> döndürdüğü<br /><br /> ``bar``<br /><br /> **Parametre numarası**: 1.<br /><br />**Ad**: Xml<br /><br />**Açıklama**: Gerekli. XPath ifadesinin değerlendirileceği XML.<br /><br /> **Parametre numarası**: 2<br /><br />**Ad**: XPath<br /><br />**Açıklama**: Gerekli. Değerlendirilecek XPath ifadesi.|  
|array|Parametreyi bir diziye dönüştürür.  Örneğin, aşağıdaki ifade şunu döndürür `["abc"]`:`array('abc')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Value<br /><br /> **Açıklama**: Gerekli. Bir diziye dönüştürülen değer.|
|createArray|Parametrelerden bir dizi oluşturur.  Örneğin, aşağıdaki ifade şunu döndürür `["a", "c"]`:`createArray('a', 'c')`<br /><br /> **Parametre numarası**: 1... No<br /><br /> **Ad**: Herhangi bir n<br /><br /> **Açıklama**: Gerekli. Bir dizide birleştirilecek değerler.|

## <a name="math-functions"></a>Matematiksel işlevler  
 Bu işlevler, iki tür numara için kullanılabilir: **tamsayılar** ve **float**.  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|ekle|İki sayının eklemenin sonucunu döndürür. Örneğin, bu işlev şunu döndürür `20.333`:`add(10,10.333)`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Summve 1<br /><br /> **Açıklama**: Gerekli. **Summand 2 '** ye eklenecek sayı.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Summve 2<br /><br /> **Açıklama**: Gerekli. **Summand 1 '** e eklenecek sayı.|  
|sub|İki sayının çıkarma sonucunu döndürür. Örneğin, bu işlev şunu döndürür: `-0.333`<br /><br /> `sub(10,10.333)`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Eksilen<br /><br /> **Açıklama**: Gerekli. **Çıkarılan** kaldırılan sayı.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Çıkarılan<br /><br /> **Açıklama**: Gerekli. **Minuend**'ten kaldırılacak sayı.|  
|MUL|İki sayının çarpma sonucu sonucunu döndürür. Örneğin, aşağıdakiler aşağıdaki gibi döndürülür `103.33`:<br /><br /> `mul(10,10.333)`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Çoğullıve 1<br /><br /> **Açıklama**: Gerekli. **Çoğullıve 2 '** nin çarpılacağı sayı.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Çoğullıve 2<br /><br /> **Açıklama**: Gerekli. **Çoğullıve 1 ' i** çarpılacak sayı.|  
|DIV|İki sayının bölümünün sonucunu döndürür. Örneğin, aşağıdakiler aşağıdaki gibi döndürülür `1.0333`:<br /><br /> `div(10.333,10)`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Eni<br /><br /> **Açıklama**: Gerekli. **Bölen**tarafından bölünecek sayı.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Lene<br /><br /> **Açıklama**: Gerekli. **Bölünen** bölüneceği sayı.|  
|alma|İki sayının (modül) bölümünün arkasına kalanı döndürür. Örneğin, aşağıdaki ifade şunu döndürür `2`:<br /><br /> `mod(10,4)`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Eni<br /><br /> **Açıklama**: Gerekli. **Bölen**tarafından bölünecek sayı.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Lene<br /><br /> **Açıklama**: Gerekli. **Bölünen** bölüneceği sayı. Bölme işleminden sonra geri kalanı alınır.|  
|dk|Bu işlevi çağırmak için iki farklı desen vardır: `min([0,1,2])`Burada en az bir dizi alır. Bu ifade döndürür `0`. Alternatif olarak, bu işlev virgülle ayrılmış bir değer listesi alabilir:  `min(0,1,2)`Bu işlev Ayrıca 0 değerini döndürür. Tüm değerlerin sayı olması gerekir, bu nedenle parametre yalnızca içinde numaralara sahip olması gereken bir dizidir.<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Koleksiyon veya değer<br /><br /> **Açıklama**: Gerekli. En küçük değeri bulmak için bir değer dizisi ya da bir küme ilk değeri olabilir.<br /><br /> **Parametre numarası**: 2... *n*<br /><br /> **Ad**: Değer *n*<br /><br /> **Açıklama**: İsteğe bağlı. İlk parametre bir değer ise, ek değerler geçirebilirsiniz ve geçirilen tüm değerlerin en az biri döndürülür.|  
|en çok|Bu işlevi çağırmak için iki farklı desen vardır:`max([0,1,2])`<br /><br /> Burada en fazla bir dizi alır. Bu ifade döndürür `2`. Alternatif olarak, bu işlev virgülle ayrılmış bir değer listesi alabilir:  `max(0,1,2)`Bu işlev 2 değerini döndürür. Tüm değerlerin sayı olması gerekir, bu nedenle parametre yalnızca içinde numaralara sahip olması gereken bir dizidir.<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Koleksiyon veya değer<br /><br /> **Açıklama**: Gerekli. En büyük değeri bulmak için bir değer dizisi ya da bir küme ilk değeri olabilir.<br /><br /> **Parametre numarası**: 2... *n*<br /><br /> **Ad**: Değer *n*<br /><br /> **Açıklama**: İsteğe bağlı. İlk parametre bir değer ise, ek değerler geçirebilirsiniz ve geçirilen tüm değerlerin en fazla değeri döndürülür.|  
|aralığı| Belirli bir sayıdan başlayan tamsayılar dizisi oluşturur ve döndürülen dizinin uzunluğunu tanımlarsınız. Örneğin, bu işlev şunu döndürür `[3,4,5,6]`:<br /><br /> `range(3,4)`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Başlangıç dizini<br /><br /> **Açıklama**: Gerekli. Dizideki ilk tamsayıdır.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Count<br /><br /> **Açıklama**: Gerekli. Dizide bulunan tamsayılar sayısı.|  
|ran| Belirtilen Aralık içinde (her iki uçta da) rastgele bir tamsayı üretir. Örneğin, bu şu şekilde olabilir `42`:<br /><br /> `rand(-1000,1000)`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Minimum<br /><br /> **Açıklama**: Gerekli. Döndürülebilecek en küçük tamsayı.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Maksimum<br /><br /> **Açıklama**: Gerekli. Döndürülebilecek en yüksek tamsayı.|  
  
## <a name="date-functions"></a>Tarih işlevleri  
  
|İşlev adı|Açıklama|  
|-------------------|-----------------|  
|UtcNow|Geçerli zaman damgasını bir dize olarak döndürür. Örneğin `2015-03-15T13:27:36Z`:<br /><br /> `utcnow()`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Biçimi<br /><br /> **Açıklama**: İsteğe bağlı. Tek bir [biçim belirticisi karakteri](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya bu zaman damgasının değerinin nasıl biçimlendirileceğini gösteren bir [özel biçim deseninin](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) olması. Biçim sağlanmazsa ISO 8601 biçimi ("o") kullanılır.|  
|AddSeconds|Geçirilen dize zaman damgasına tamsayı cinsinden saniye sayısı ekler. Saniye sayısı pozitif veya negatif olabilir. Bir biçim belirticisi sağlanmamışsa, sonuç ISO 8601 biçiminde ("o") varsayılan olarak bir dizedir. Örneğin `2015-03-15T13:27:00Z`:<br /><br /> `addseconds('2015-03-15T13:27:36Z', -36)`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Timestamp<br /><br /> **Açıklama**: Gerekli. Saati içeren bir dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Saniye<br /><br /> **Açıklama**: Gerekli. Eklenecek saniye sayısı. Saniye çıkarmak için negatif olabilir.<br /><br /> **Parametre numarası**: 3<br /><br /> **Ad**: Biçimi<br /><br /> **Açıklama**: İsteğe bağlı. Tek bir [biçim belirticisi karakteri](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya bu zaman damgasının değerinin nasıl biçimlendirileceğini gösteren bir [özel biçim deseninin](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) olması. Biçim sağlanmazsa ISO 8601 biçimi ("o") kullanılır.|  
|AddMinutes|Geçirilen bir dize zaman damgasına tamsayı cinsinden bir sayı ekler. Dakika sayısı pozitif veya negatif olabilir. Bir biçim belirticisi sağlanmamışsa, sonuç ISO 8601 biçiminde ("o") varsayılan olarak bir dizedir. Örneğin `2015-03-15T14:00:36Z`:<br /><br /> `addminutes('2015-03-15T13:27:36Z', 33)`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Timestamp<br /><br /> **Açıklama**: Gerekli. Saati içeren bir dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Dakika<br /><br /> **Açıklama**: Gerekli. Eklenecek dakika sayısı. Dakika çıkarmak için negatif olabilir.<br /><br /> **Parametre numarası**: 3<br /><br /> **Ad**: Biçimi<br /><br /> **Açıklama**: İsteğe bağlı. Tek bir [biçim belirticisi karakteri](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya bu zaman damgasının değerinin nasıl biçimlendirileceğini gösteren bir [özel biçim deseninin](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) olması. Biçim sağlanmazsa ISO 8601 biçimi ("o") kullanılır.|  
|AddHours|Geçirilen bir dize zaman damgasına tamsayı cinsinden bir saat ekler. Saat sayısı pozitif veya negatif olabilir. Bir biçim belirticisi sağlanmamışsa, sonuç ISO 8601 biçiminde ("o") varsayılan olarak bir dizedir. Örneğin `2015-03-16T01:27:36Z`:<br /><br /> `addhours('2015-03-15T13:27:36Z', 12)`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Timestamp<br /><br /> **Açıklama**: Gerekli. Saati içeren bir dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Saat<br /><br /> **Açıklama**: Gerekli. Eklenecek saat sayısı. Saatleri çıkarmak için negatif olabilir.<br /><br /> **Parametre numarası**: 3<br /><br /> **Ad**: Biçimi<br /><br /> **Açıklama**: İsteğe bağlı. Tek bir [biçim belirticisi karakteri](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya bu zaman damgasının değerinin nasıl biçimlendirileceğini gösteren bir [özel biçim deseninin](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) olması. Biçim sağlanmazsa ISO 8601 biçimi ("o") kullanılır.|  
|AddDays|Geçirilen bir dize zaman damgasına bir tamsayı gün sayısı ekler. Gün sayısı pozitif veya negatif olabilir. Bir biçim belirticisi sağlanmamışsa, sonuç ISO 8601 biçiminde ("o") varsayılan olarak bir dizedir. Örneğin `2015-02-23T13:27:36Z`:<br /><br /> `adddays('2015-03-15T13:27:36Z', -20)`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Timestamp<br /><br /> **Açıklama**: Gerekli. Saati içeren bir dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Gün<br /><br /> **Açıklama**: Gerekli. Eklenecek gün sayısı. Gün çıkarmak için negatif olabilir.<br /><br /> **Parametre numarası**: 3<br /><br /> **Ad**: Biçimi<br /><br /> **Açıklama**: İsteğe bağlı. Tek bir [biçim belirticisi karakteri](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya bu zaman damgasının değerinin nasıl biçimlendirileceğini gösteren bir [özel biçim deseninin](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) olması. Biçim sağlanmazsa ISO 8601 biçimi ("o") kullanılır.|  
|formatDateTime|Tarih biçiminde bir dize döndürür. Bir biçim belirticisi sağlanmamışsa, sonuç ISO 8601 biçiminde ("o") varsayılan olarak bir dizedir. Örneğin `2015-02-23T13:27:36Z`:<br /><br /> `formatDateTime('2015-03-15T13:27:36Z', 'o')`<br /><br /> **Parametre numarası**: 1.<br /><br /> **Ad**: Date<br /><br /> **Açıklama**: Gerekli. Tarihi içeren bir dize.<br /><br /> **Parametre numarası**: 2<br /><br /> **Ad**: Biçimi<br /><br /> **Açıklama**: İsteğe bağlı. Tek bir [biçim belirticisi karakteri](https://msdn.microsoft.com/library/az4se3k1%28v=vs.110%29.aspx) veya bu zaman damgasının değerinin nasıl biçimlendirileceğini gösteren bir [özel biçim deseninin](https://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx) olması. Biçim sağlanmazsa ISO 8601 biçimi ("o") kullanılır.|  

## <a name="next-steps"></a>Sonraki adımlar
İfadelerde kullanabileceğiniz sistem değişkenlerinin listesi için bkz. [Sistem değişkenleri](control-flow-system-variables.md).
