---
title: Dize alanlarına dil çözümleyicileri ekleme
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'da İngilizce olmayan sorgular ve dizinler için çok dilli sözlü metin çözümlemesi.
manager: nitinme
author: Yahnoosh
ms.author: jlembicz
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/10/2019
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: a97bee27b74aa211b4d4d56547726555edefa87a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79283153"
---
# <a name="add-language-analyzers-to-string-fields-in-an-azure-cognitive-search-index"></a>Azure Bilişsel Arama dizinindeki dize alanlarına dil çözümleyicileri ekleme

*Dil çözümleyicisi,* hedef dilin dilsel kurallarını kullanarak sözlü çözümleme gerçekleştiren belirli bir metin [çözümleyici](search-analyzers.md) türüdür. Her aranabilir alanın bir **çözümleyici** özelliği vardır. Dizininizde İngilizce ve Çince metin için ayrı alanlar gibi çevrilmiş dizeler içeriyorsa, bu çözümleyicilerin zengin dil selyeteneklerine erişmek için her alandaki dil çözümleyicilerini belirtebilirsiniz.  

Azure Bilişsel Arama, Lucene tarafından desteklenen 35 çözümleyiciyi ve Office ve Bing'de kullanılan özel Microsoft doğal dil işleme teknolojisi tarafından desteklenen 50 çözümleyiciyi destekler.

## <a name="comparing-analyzers"></a>Çözümleyicileri karşılaştırma

Bazı geliştiriciler Lucene daha tanıdık, basit, açık kaynak çözüm tercih edebilir. Lucene dil çözümleyicileri daha hızlıdır, ancak Microsoft çözümleyicileri lemmatizasyon, kelime decompounding (Almanca, Danca, Felemenkçe, İsveççe, Norveççe, Estonyaca, Finish, Macarca, Slovakça gibi dillerde) ve varlık gibi gelişmiş özelliklere sahiptir tanıma (URL'ler, e-postalar, tarihler, sayılar). Mümkünse, hangisinin daha uygun olduğuna karar vermek için hem Microsoft hem de Lucene çözümleyicilerinin karşılaştırmalarını çalıştırmalısınız. 

Microsoft çözümleyicileri ile dizin oluşturma, dile bağlı olarak Lucene eşdeğerlerinden ortalama 2-3 kat daha yavaştır. Ortalama boyut sorguları için arama performansı önemli ölçüde etkilenmemelidir. 

### <a name="english-analyzers"></a>İngilizce analizörler

Varsayılan çözümleyici, İngilizce için iyi çalışan standart Lucene'dir, ancak belki de Lucene'nin İngilizce çözümleyicisi veya Microsoft'un İngilizce çözümleyicisi kadar iyi değildir. 
 
+ Lucene'nin İngilizce analizörü standart analizörü genişletir. Bu iyelik kaldırır ('s) kelimelerden, Porter Stemming algoritması başına kaynaklanan uygular ve İngilizce stop kelimeleri kaldırır.  

+ Microsoft'un İngilizce çözümleyicisi, köklendirme yerine lemmatizasyon gerçekleştirir. Bu, daha alakalı arama sonuçları çok daha iyi, çekimli ve düzensiz sözcük formları işleyebilir anlamına gelir 

## <a name="configuring-analyzers"></a>Çözümleyicileri yapılandırma

Dil çözümleyicileri olduğu gibi kullanılır. Dizin tanımındaki her alan için **çözümleyici** özelliğini, dil ve dilbilgisi yığınını (Microsoft veya Lucene) belirten bir çözümleyici adı olarak ayarlayabilirsiniz. Dizin leme ve bu alan aranırken aynı çözümleyici uygulanacaktır. Örneğin, aynı dizinde yan yana bulunan İngilizce, Fransızca ve İspanyolca otel açıklamaları için ayrı alanlar alabilirsiniz.

> [!NOTE]
> Dizinleme zamanında, bir alanın sorgu zamanına göre farklı bir dil çözümleyicisi kullanmak mümkün değildir. Bu özellik [özel çözümleyiciler](index-add-custom-analyzers.md)için ayrılmıştır. Bu nedenle, **searchAnalyzer** veya **indexAnalyzer** özelliklerini bir dil çözümleyicisinin adına ayarlamaya çalışırsanız, REST API bir hata yanıtı döndürecektir. Bunun yerine **çözümleyici** özelliğini kullanmanız gerekir.

Sorgularınızda hangi dile özgü alana karşı arama yapılacağını belirtmek için **arama Alanları** sorgu parametresini kullanım. [Arama Belgelerinde](https://docs.microsoft.com/rest/api/searchservice/search-documents)çözümleyici özelliğini içeren sorgu örneklerini gözden geçirebilirsiniz. 

Dizin özellikleri hakkında daha fazla bilgi için Azure [Bilişsel Arama REST API &#40;&#41;dizin oluştur'a ](https://docs.microsoft.com/rest/api/searchservice/create-index)bakın. Azure Bilişsel Arama'da analiz hakkında daha fazla bilgi için Azure [Bilişsel Arama'da Çözümleyiciler'e](https://docs.microsoft.com/azure/search/search-analyzers)bakın.

<a name="language-analyzer-list"></a>

## <a name="language-analyzer-list"></a>Dil çözümleyici listesi 
 Aşağıda Lucene ve Microsoft çözümleyici adları ile birlikte desteklenen dillerin listesi verilmiştir.  

|Dil|Microsoft Analyzer Adı|Lucene Analizörü Adı|  
|--------------|-----------------------------|--------------------------|  
|Arapça|ar.microsoft|ar.lucene|  
|Ermenice||hy.lucene|  
|Bangla|bn.microsoft||  
|Baskça||eu.lucene|  
|Bulgarca|bg.microsoft|bg.lucene|  
|Katalanca|ca.microsoft|ca.lucene|  
|Basitleştirilmiş Çince|zh-Hans.microsoft|zh-Hans.lucene|  
|Geleneksel Çince|zh-Hant.microsoft|zh-Hant.lucene|  
|Hırvatça|hr.microsoft||  
|Çekçe|cs.microsoft|cs.lucene|  
|Danca|da.microsoft|da.lucene|  
|Felemenkçe|nl.microsoft|nl.lucene|  
|Türkçe|tr.microsoft|en.lucene|  
|Estonya Dili|et.microsoft||  
|Fince|fi.microsoft|fi.lucene|  
|Fransızca|fr.microsoft|fr.lucene|  
|Galiçya Dili||gl.lucene|  
|Almanca|de.microsoft|de.lucene|  
|Yunanca|el.microsoft|el.lucene|  
|Gucerat dili|gu.microsoft||  
|İbranice|he.microsoft||  
|Hintçe|merhaba.microsoft|merhaba.lucene|  
|Macarca|hu.microsoft|hu.lucene|  
|İzlandaca|is.microsoft||  
|Endonezya dili (Bahasa)|id.microsoft|id.lucene|  
|İrlandaca||ga.lucene|  
|İtalyanca|it.microsoft|it.lucene|  
|Japonca|ja.microsoft|ja.lucene|  
|Kannada dili|kn.microsoft||  
|Korece|ko.microsoft|ko.lucene|  
|Letonca|lv.microsoft|lv.lucene|  
|Litvanca|lt.microsoft||  
|Malayalam dili|ml.microsoft||  
|Malay (Latince)|ms.microsoft||  
|Marathi|mr.microsoft||  
|Norveççe|nb.microsoft|no.lucene|  
|Farsça||fa.lucene|  
|Lehçe|pl.microsoft|pl.lucene|  
|Portekizce (Brezilya)|pt-Br.microsoft|pt-Br.lucene|  
|Portekizce (Portekiz)|pt-Pt.microsoft|pt-Pt.lucene|  
|Pencap dili|pa.microsoft||  
|Rumence|ro.microsoft|ro.lucene|  
|Rusça|ru.microsoft|ru.lucene|  
|Sırpça (Kiril)|sr-kirillic.microsoft||  
|Sırpça (Latin)|sr-latin.microsoft||  
|Slovakça|sk.microsoft||  
|Slovence|sl.microsoft||  
|İspanyolca|es.microsoft|es.lucene|  
|İsveççe|sv.microsoft|sv.lucene|  
|Tamil dili|ta.microsoft||  
|Telugu dili|te.microsoft||  
|Tayca|th.microsoft|th.lucene|  
|Türkçe|tr.microsoft|tr.lucene|  
|Ukraynaca|uk.microsoft||  
|Urduca|sizin.microsoft||  
|Vietnamca|vi.microsoft||  

 **Lucene** ile açıklamalı isimleri ile tüm analizörler [Apache Lucene dil analizörleri](https://lucene.apache.org/core/6_6_1/core/overview-summary.html )tarafından desteklenmektedir.

## <a name="see-also"></a>Ayrıca bkz.  

+ [Azure Bilişsel Arama REST API &#40;Dizin oluşturma&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index)  

+ [AnalyzerName Sınıfı](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername)  

