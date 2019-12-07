---
title: Yeni Özellik Duyuruları
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama Azure Search hizmet yeniden adlandırma özelliği de dahil olmak üzere yeni ve geliştirilmiş özelliklerin duyuruları.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: b1df328f151a4085ec0aadd1b880048f81483a51
ms.sourcegitcommit: 375b70d5f12fffbe7b6422512de445bad380fe1e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74901316"
---
# <a name="whats-new-in-azure-cognitive-search"></a>Azure Bilişsel Arama yenilikleri

Hizmette nelerin yeni olduğunu öğrenin. Hizmette güncel kalmasını sağlamak için bu sayfaya yer işareti ekleyin.

<a name="new-service-name"></a>

## <a name="new-service-name-for-azure-search"></a>Azure Search için yeni hizmet adı

Azure Search, çekirdek işlemlerde bilişsel yeteneklerin ve AI işlemenin genişletilmiş kullanımını yansıtmak için artık **Azure bilişsel arama** olarak yeniden adlandırıldı. Bilişsel yetenekler yeni yetenekler Ekken, AI kullanmak kesinlikle isteğe bağlıdır. Bulutta oluşturduğunuz ve yönettiğiniz bir dizinde özel, heterojen, metin tabanlı içeriğe göre zengin, tam metin arama çözümleri oluşturmak için Azure Bilişsel Arama 'yi AI olmadan kullanmaya devam edebilirsiniz. 

API sürümleri, NuGet paketleri, ad alanları ve uç noktalar değiştirilmez. Mevcut arama çözümlerinizi hizmet adı değişikliğinden etkilenmez.

## <a name="feature-announcements"></a>Özellik Duyuruları

### <a name="december-2019"></a>Aralık 2019

+ [Uygulama oluşturma (Önizleme)](search-create-app-portal.md) , portalda INDIRILEBILIR bir HTML dosyası üreten yeni bir sihirbazdır. Dosya, arama hizmetinizde bir dizine bağlı olan, işlemsel bir "localhost" stili Web uygulaması işleyen katıştırılmış komut dosyası ile birlikte gelir. Sayfalar sihirbazda yapılandırılabilir ve bir arama çubuğu, sonuç alanı, kenar çubuğu gezintisi ve typeahead sorgu desteği bulunabilir. İş akışını veya görünümü genişletmek veya özelleştirmek için HTML 'yi çevrimdışı değiştirebilirsiniz.

### <a name="november-2019---ignite-conference"></a>Kasım 2019-Menite Konferansı

+ [Artımlı dizin oluşturma (Önizleme)](cognitive-search-incremental-indexing-conceptual.md) , bir zenginleştirme ardışık düzeninde değişiklik yaparken hangi adımların yeniden işleyeceğini seçmenizi ve seçmenizi sağlar. Daha önce analiz ettiğiniz görüntü içeriğiniz varsa, artımlı dizin oluşturma yararlı olur. Maliyetli çözümlemenin çıktısı depolanır ve ek dizin oluşturma veya zenginleştirme için temel olarak kullanılır.

<!-- 
+ Custom Entity Lookup is a cognitive skill used during indexing that allows you to provide a list of custom entities (such as part numbers, diseases, or names of locations you care about) that should be found within the text. It supports fuzzy matching, case-insensitive matching, and entity synonyms. -->

+ [Belge ayıklama (Önizleme)](cognitive-search-skill-document-extraction.md) , dizin oluşturma sırasında kullanılan bilişsel bir beceriye sahiptir ve bir dosyanın içeriğini bir beceri içinden ayıklamanızı sağlar. Daha önce belge çözme yalnızca beceri yürütmeden önce oluşmuştur. Bu beceriye ek olarak, bu işlemi beceri yürütme içinde de gerçekleştirebilirsiniz.

+ [Metin çevirisi (Önizleme)](cognitive-search-skill-text-translation.md) , dizinleme sırasında metin değerlendiren ve her kayıt için belirtilen hedef dile çevrilen metni döndüren bilişsel bir yetenküldür.

+ [Power BI şablonlar](https://github.com/Azure-Samples/cognitive-search-templates/blob/master/README.md) , görselleştirmelerinizi ve Power BI masaüstündeki bir bilgi deposunda zenginleştirme içeriği analizini başlatabilir. Bu şablon, [veri Içeri aktarma Sihirbazı](knowledge-store-create-portal.md)aracılığıyla oluşturulan Azure Tablo projeksiyonlarını için tasarlanmıştır.

+ [Azure Data Lake Storage 2. (Önizleme)](search-howto-index-azure-data-lake-storage.md), [Cosmos DB GREMLIN API (önizleme)](search-howto-index-cosmosdb.md)ve [Cosmos DB Cassandra API (Önizleme)](search-howto-index-cosmosdb.md) Dizin oluşturucularda artık desteklenmektedir. [Bu formu](https://aka.ms/azure-cognitive-search/indexer-preview)kullanarak kaydolabilirsiniz. Önizleme programına kabul edildikten sonra bir onay e-postası alacaksınız.

### <a name="july-2019"></a>Temmuz 2019

+ [Azure Kamu Bulutu](../azure-government/documentation-government-services-webandmobile.md#azure-cognitive-search)'nda genel olarak kullanılabilir.

## <a name="service-updates"></a>Hizmet güncellemeleri

Azure Bilişsel Arama için [hizmet güncelleştirme duyuruları](https://azure.microsoft.com/updates/?product=search&status=all) Azure Web sitesinde bulunabilir.