---
title: Sınırları ve sınır - soru-cevap Oluşturucu
titleSuffix: Azure Cognitive Services
description: Soru-cevap Oluşturucu, bölümlerini Bilgi Bankası ve hizmet için meta-sınırlara sahiptir. Sınama ve yayımlama için bu sınırları içinde bilgi bankanızı tutmak önemlidir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 08/30/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: c7b0dc39d2da403383f245b9ff3227734c58cbbe
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70193492"
---
# <a name="qna-maker-knowledge-base-limits-and-boundaries"></a>Soru-cevap Oluşturucu Bilgi Bankası sınırları ve sınır

Aşağıda belirtilen Soru-Cevap Oluşturma limitleri [Azure Search fiyatlandırma katmanı sınırlarının](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity) ve [soru-cevap oluşturma fiyatlandırma katmanı sınırlarının](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/)bir birleşimidir. Kaynak başına kaç bilgi tabanı oluşturabileceğiniz ve her bilgi tabanının ne kadar büyüeceği hakkında bilgi sahibi olmak için her iki limit kümesini de bilmeniz gerekir.

## <a name="knowledge-bases"></a>Bilgi Bankası tabanları

En fazla bilgi tabanı sayısı [Azure Search katmanı sınırlarına](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)dayanır.

|**Azure arama katmanı** | **Ücretsiz** | **Temel** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|İzin verilen en fazla yayımlanan bilgi tabanı sayısı|2|14|49|199|199|2,999|

 Örneğin, katmanınızda 15 ' in izin verilen dizini varsa, 14 bilgi tabanı (yayımlanan Bilgi Bankası başına 1 Dizin) yayımlayabilirsiniz. On beşinci Dizin `testkb`, yazma ve test için tüm bilgi tabanları için kullanılır. 

## <a name="extraction-limits"></a>Ayıklama sınırları

### <a name="maximum-number-of-files"></a>En fazla dosya sayısı

Ayıklanabilen en fazla dosya sayısı ve en büyük dosya boyutu, **[soru-cevap oluşturma fiyatlandırma katmanı limitlerinizi](https://azure.microsoft.com/pricing/details/cognitive-services/qna-maker/)** temel alır.

### <a name="maximum-number-of-deep-links-from-url"></a>URL 'den en fazla derin bağlantı sayısı

Bir URL sayfasından QnAs ayıklanmasıyla gezinilebilen en fazla derin bağlantı sayısı **20**' dir.

## <a name="metadata-limits"></a>Meta veri sınırları

### <a name="by-azure-search-pricing-tier"></a>Azure Search fiyatlandırma katmanına göre

Bilgi Bankası başına en fazla meta veri alanı sayısı **[Azure Search katmanı limitlerinizi](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity)** temel alır.

|**Azure arama katmanı** | **Ücretsiz** | **Temel** |**S1** | **S2**| **S3** |**S3 HD**|
|---|---|---|---|---|---|----|
|Soru-cevap Oluşturucu hizmeti (genelinde tüm KB'leri) başına en fazla meta veri alanları|1000|100 *|1000|1000|1000|1000|

### <a name="by-name-and-value"></a>Ada ve değere göre

Meta veri adı ve değeri için uzunluk ve kabul edilebilir karakterler aşağıdaki tabloda listelenmiştir.

|Öğe|İzin verilen karakterler|Regex model eşleşmesi|En fazla karakter|
|--|--|--|--|
|Name|Belirlemesine<br>alfasayısal (harfler ve rakamlar)<br>`_`adında|`^[a-zA-Z0-9_]+$`|100|
|Value|Hariç her şeye izin verir<br>`:`üste<br>`|`(dikey boru)|`^[^:|]+$`|500|
|||||

## <a name="knowledge-base-content-limits"></a>Bilgi Bankası içerik sınırları
Bilgi Bankası'nda içeriği genel sınırlamaları:
* Yanıt metninin uzunluğu: 25,000
* Soru metninin uzunluğu: 1000
* Meta veri anahtarı/değer metninin uzunluğu: 100
* Meta veri adı için desteklenen karakterler: Harfler, rakamlar ve`_`  
* Meta veri değeri için desteklenen karakterler: Ve dışında `:` tümü`|` 
* Dosya adı uzunluğu: 200
* Desteklenen dosya biçimleri: ".tsv", ".pdf", ".txt", ".docx", ".xlsx".
* Alternatif soruların maksimum sayısı: 300
* En yüksek soru-yanıt çifti sayısı: Seçilen **[Azure Search katmanına](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#document-limits)** bağlıdır. Soru ve yanıt çifti Azure Search dizindeki bir belgeyle eşlenir. 
* URL/HTML sayfası: 1.000.000 karakter

## <a name="create-knowledge-base-call-limits"></a>Bilgi Bankası araması sınırları oluşturun:
Bu temsil Bilgi Bankası eylem her sınırları oluşturmak; diğer bir deyişle, tıklayıp *oluşturma KB* veya CreateKnowledgeBase API'ye çağrı yapma.
* Yanıt başına en fazla alternatif soru sayısı: 300
* En fazla URL sayısı: 10
* En fazla dosya sayısı: 10

## <a name="update-knowledge-base-call-limits"></a>Bilgi Bankası araması sınırları güncelleştir
Bunlar, her güncelleştirme eylemi sınırlarını temsil eder; diğer bir deyişle, tıklayıp *kaydedin ve eğitme* veya UpdateKnowledgeBase API'ye çağrı yapma.
* Her kaynak adının uzunluğu: 300
* Eklenen veya silinen diğer soruların sayısı üst sınırı: 300
* Eklenen veya silinen en fazla meta veri alanı sayısı: 10
* Yenilenemeyen URL sayısı üst sınırı: 5

## <a name="next-steps"></a>Sonraki adımlar

[Hizmet fiyatlandırma katmanlarının](How-To/set-up-qnamaker-service-azure.md#upgrade-qna-maker)ne zaman ve nasıl değiştirileceğini öğrenin.
