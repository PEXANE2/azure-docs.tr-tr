---
title: Sınırlar ve sınırlar - QnA Maker
description: QnA Maker bilgi tabanı ve hizmet parçaları için meta-sınırları vardır. Test etmek ve yayınlamak için bilgi tabanınızı bu sınırlar içinde tutmak önemlidir.
ms.topic: reference
ms.date: 02/14/2020
ms.openlocfilehash: 6375a6c6efc0c7016d9947e04e9479385aa80af5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273345"
---
# <a name="qna-maker-knowledge-base-limits-and-boundaries"></a>QnA Maker bilgi tabanı sınırları ve sınırları

Aşağıda verilen QnA Maker [sınırları, Azure Bilişsel Arama fiyatlandırma katman sınırları](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity) ve [QnA Maker fiyatlandırma katmanı sınırlarının](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/)bir kombinasyonudur. Kaynak başına kaç bilgi tabanı oluşturabileceğinizi ve her bilgi tabanının ne kadar büyüyebileceğini anlamak için her iki sınır kümesini de bilmeniz gerekir.

## <a name="knowledge-bases"></a>Bilgi üsleri

Maksimum bilgi tabanı sayısı Azure [Bilişsel Arama katmanı sınırlarına](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)dayanır.

|**Azure Bilişsel Arama katmanı** | **Ücretsiz** | **Temel** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|İzin verilen maksimum yayınlanmış bilgi tabanı sayısı|2|14|49|199|199|2,999|

 Örneğin, katmanınızda izin verilen 15 dizin varsa, 14 bilgi tabanı (yayımlanmış bilgi tabanı başına 1 dizin) yayımlayabilirsiniz. On beşinci indeks, `testkb`, yazma ve test için tüm bilgi üsleri için kullanılır.

## <a name="extraction-limits"></a>Ekstraksiyon Limitleri

### <a name="file-naming-constraints"></a>Dosya adlandırma kısıtlamaları

Dosya adları aşağıdaki karakterleri içermeyebilir:

|Karakter kullanmayın|
|--|
|Tek teklif`'`|
|Çift teklif`"`|

### <a name="maximum-file-size"></a>En büyük dosya boyutu

|Biçimlendir|Maksimum dosya boyutu (MB)|
|--|--|
|`.docx`|10|
|`.pdf`|25|
|`.tsv`|10|
|`.txt`|10|
|`.xlsx`|3|

### <a name="maximum-number-of-files"></a>Maksimum dosya sayısı

Çıkarılabilen maksimum dosya sayısı ve maksimum dosya boyutu **[QnA Maker fiyatlandırma katman sınırlarınızı](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/)** temel almaktadır.

### <a name="maximum-number-of-deep-links-from-url"></a>URL'den maksimum derin bağlantı sayısı

Bir URL sayfasından Qna'ların çıkarılması için taranabilecek en fazla derin bağlantı sayısı **20'dir.**

## <a name="metadata-limits"></a>Meta Veri Sınırları

Meta veriler metin tabanlı bir anahtar olarak sunulur: `product:windows 10`değer çifti, örneğin. Bu depolanır ve küçük harfle karşılaştırılır.

### <a name="by-azure-cognitive-search-pricing-tier"></a>Azure Bilişsel Arama fiyatlandırma katmanına göre

Bilgi tabanı başına maksimum meta veri alanı **[sayısı, Azure Bilişsel Arama katmanı sınırlarınızı](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)** temel almaktadır.

|**Azure Bilişsel Arama katmanı** | **Ücretsiz** | **Temel** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|QnA Maker hizmeti başına maksimum meta veri alanları (tüm KB'lerde)|1000|100*|1000|1000|1000|1000|

### <a name="by-name-and-value"></a>Ada ve değere göre

Meta veri adı ve değeri için uzunluk ve kabul edilebilir karakterler aşağıdaki tabloda listelenmiştir.

|Öğe|İzin verilen chars|Regex desen eşleşmesi|Maksimum kar|
|--|--|--|--|
|Ad (anahtar)|Sağ -lar<br>alfanümerik (harfler ve basamaklar)<br>`_`(alt puan)<br> Boşluk içermemelidir.|`^[a-zA-Z0-9_]+$`|100|
|Değer|Hariç her şeye izin verir<br>`:`(kolon)<br>`|`(dikey boru)<br>Yalnızca bir değere izin verildi.|`^[^:|]+$`|500|
|||||

## <a name="knowledge-base-content-limits"></a>Bilgi Bankası içerik sınırları
Bilgi tabanındaki içeriğe ilişkin genel sınırlar:
* Cevap metninin uzunluğu: 25.000
* Soru metninin uzunluğu: 1.000
* Meta veri anahtar/değer metninin uzunluğu: 100
* Meta veri adı için desteklenen karakterler: Alfabeler, basamaklar ve`_`
* Meta veri değeri için desteklenen karakterler: Tüm bunlar hariç `:` ve`|`
* Dosya adı uzunluğu: 200
* Desteklenen dosya biçimleri: ".tsv", ".pdf", ".txt", ".docx", ".xlsx".
* Maksimum alternatif soru sayısı: 300
* Maksimum soru-yanıt çifti sayısı: Seçilen **[Azure Bilişsel Arama katmanına](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#document-limits)** bağlıdır. Soru ve yanıt çifti eşlemleri Azure Bilişsel Arama dizinindeki bir belgeye gelir.
* URL/HTML sayfası: 1 milyon karakter

## <a name="create-knowledge-base-call-limits"></a>Bilgi temel çağrı limitleri oluşturun:
Bunlar, her bir bilgi bankası eylemi oluşturmanın sınırlarını temsil; diğer bir şey, *KB Oluştur'u* tıklatmak veya CreateKnowledgeBase API'yi aramak.
* Yanıt başına maksimum alternatif soru sayısı: 300
* Maksimum URL sayısı: 10
* Maksimum dosya sayısı: 10

## <a name="update-knowledge-base-call-limits"></a>Bilgi temel arama sınırlarını güncelleştirme
Bunlar, her güncelleştirme eylemi için sınırları temsil; diğer bir şey, *Kaydet'i tıklatıp eğitmek* veya UpdateKnowledgeBase API'yi aramak.
* Her kaynak adın uzunluğu: 300
* En fazla eklenen veya silinen alternatif soru sayısı: 300
* En fazla eklenen veya silinen meta veri alanları sayısı: 10
* Yenilenebilen maksimum URL sayısı: 5

## <a name="next-steps"></a>Sonraki adımlar

[Hizmet fiyatlandırma katmanlarını](How-To/set-up-qnamaker-service-azure.md#upgrade-qna-maker-sku)ne zaman ve nasıl değiştireceğinizi öğrenin.
