---
title: Sınırları ve sınır - soru-cevap Oluşturucu
description: Soru-cevap Oluşturucu, bölümlerini Bilgi Bankası ve hizmet için meta-sınırlara sahiptir. Sınama ve yayımlama için bu sınırları içinde bilgi bankanızı tutmak önemlidir.
ms.topic: article
ms.date: 02/14/2020
ms.openlocfilehash: ba53513f21cfc2a4f16fe17decdf0df41570201c
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77650376"
---
# <a name="qna-maker-knowledge-base-limits-and-boundaries"></a>Soru-cevap Oluşturucu Bilgi Bankası sınırları ve sınır

Aşağıda belirtilen Soru-Cevap Oluşturma limitleri [Azure bilişsel arama fiyatlandırma katmanı sınırlarının](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity) ve [soru-cevap oluşturma fiyatlandırma katmanı sınırlarının](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/)bir birleşimidir. Kaynak başına kaç bilgi tabanı oluşturabileceğiniz ve her bilgi tabanının ne kadar büyüeceği hakkında bilgi sahibi olmak için her iki limit kümesini de bilmeniz gerekir.

## <a name="knowledge-bases"></a>Bilgi Bankası tabanları

En fazla bilgi Bankası sayısı, [Azure bilişsel arama katmanı sınırlarına](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)göre belirlenir.

|**Azure Bilişsel Arama katmanı** | **Ücretsiz** | **Temel** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|İzin verilen en fazla yayımlanan bilgi tabanı sayısı|2|14|49|199|199|2,999|

 Örneğin, katmanınızda 15 ' in izin verilen dizini varsa, 14 bilgi tabanı (yayımlanan Bilgi Bankası başına 1 Dizin) yayımlayabilirsiniz. On beşinci Dizin, `testkb`, yazma ve test için tüm bilgi tabanları için kullanılır.

## <a name="extraction-limits"></a>Ayıklama sınırları

### <a name="file-naming-constraints"></a>Dosya adlandırma kısıtlamaları

Dosya adları aşağıdaki karakterleri içeremez:

|Karakter kullanma|
|--|
|Tek tırnak `'`|
|Çift tırnak `"`|

### <a name="maximum-file-size"></a>En büyük dosya boyutu

|Biçimlendir|En büyük dosya boyutu (MB)|
|--|--|
|`.docx`|10|
|`.pdf`|25|
|`.tsv`|10|
|`.txt`|10|
|`.xlsx`|3|

### <a name="maximum-number-of-files"></a>En fazla dosya sayısı

Ayıklanabilen en fazla dosya sayısı ve en büyük dosya boyutu, **[soru-cevap oluşturma fiyatlandırma katmanı limitlerinizi](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/)** temel alır.

### <a name="maximum-number-of-deep-links-from-url"></a>URL 'den en fazla derin bağlantı sayısı

Bir URL sayfasından QnAs ayıklanmasıyla gezinilebilen en fazla derin bağlantı sayısı **20**' dir.

## <a name="metadata-limits"></a>Meta veri sınırları

Meta veriler, `product:windows 10`gibi bir metin tabanlı anahtar: değer çifti olarak sunulur. Daha küçük bir durumda depolanır ve karşılaştırılır.

### <a name="by-azure-cognitive-search-pricing-tier"></a>Azure Bilişsel Arama fiyatlandırma katmanına göre

Bilgi Bankası başına en fazla meta veri alanı sayısı, **[Azure bilişsel arama katmanı limitlerinizi](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)** temel alır.

|**Azure Bilişsel Arama katmanı** | **Ücretsiz** | **Temel** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Soru-cevap Oluşturucu hizmeti (genelinde tüm KB'leri) başına en fazla meta veri alanları|1000|100 *|1000|1000|1000|1000|

### <a name="by-name-and-value"></a>Ada ve değere göre

Meta veri adı ve değeri için uzunluk ve kabul edilebilir karakterler aşağıdaki tabloda listelenmiştir.

|Öğe|İzin verilen karakterler|Regex model eşleşmesi|En fazla karakter|
|--|--|--|--|
|Ad (anahtar)|Belirlemesine<br>alfasayısal (harfler ve rakamlar)<br>`_` (alt çizgi)<br> Boşluk içermemelidir.|`^[a-zA-Z0-9_]+$`|100|
|Değer|Hariç her şeye izin verir<br>`:` (iki nokta üst üste)<br>`|` (dikey boru)<br>Yalnızca bir değere izin verilir.|`^[^:|]+$`|500|
|||||

## <a name="knowledge-base-content-limits"></a>Bilgi Bankası içerik sınırları
Bilgi Bankası'nda içeriği genel sınırlamaları:
* Yanıt metnin uzunluğunu: 25.000
* Soru metnin uzunluğunu: 1.000
* Meta verileri anahtar/değer metnin uzunluğunu: 100
* Meta veri adı için desteklenen karakterler: Alfabeller, rakamlar ve `_`
* Meta veri değeri için desteklenen karakterler: tümü `:` ve `|` hariç
* Dosya adının uzunluğu: 200
* Desteklenen dosya biçimleri: ".tsv", ".pdf", ".txt", ".docx", ".xlsx".
* Alternatif soruların maksimum sayısı: 300
* En yüksek soru-cevap çifti sayısı: seçilen **[Azure bilişsel arama katmanına](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#document-limits)** bağlıdır. Soru ve yanıt çifti, Azure Bilişsel Arama dizininde bulunan bir belgeyle eşlenir.
* URL/HTML sayfası: 1.000.000 karakter

## <a name="create-knowledge-base-call-limits"></a>Bilgi Bankası araması sınırları oluşturun:
Bunlar her bir bilgi tabanı oluşturma eyleminin sınırlarını temsil eder; diğer bir deyişle, *BB oluştur* veya CREATEıNFO API 'sini çağırma ' ya tıklayın.
* Yanıt başına en fazla alternatif soru sayısı: 300
* URL'leri sayısı: 10
* En fazla dosya sayısı: 10

## <a name="update-knowledge-base-call-limits"></a>Bilgi Bankası araması sınırları güncelleştir
Bunlar her bir güncelleştirme eyleminin sınırlarını temsil eder; diğer bir deyişle, *kayıt ve eğitim* ' i ve sonra UPDATEıNFO API 'sini çağırır.
* Her kaynak adının uzunluğu: 300
* En fazla eklenen veya silinen diğer soru sayısı: 300
* Meta veri alanları en fazla sayısını eklendiğinde veya silindiğinde: 10
* Yenilenebilir URL'leri sayısı: 5

## <a name="next-steps"></a>Sonraki adımlar

[Hizmet fiyatlandırma katmanlarının](How-To/set-up-qnamaker-service-azure.md#upgrade-qna-maker-sku)ne zaman ve nasıl değiştirileceğini öğrenin.
