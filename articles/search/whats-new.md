---
title: Hizmette yenilikler
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama Azure Search hizmet yeniden adlandırma özelliği de dahil olmak üzere yeni ve geliştirilmiş özelliklerin duyuruları.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: e81eaf232e3234ac4de0cfb7412e23709f0c0b99
ms.sourcegitcommit: 3f8017692169bd75483eefa96c225d45cd497f06
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73549106"
---
# <a name="whats-new-in-azure-cognitive-search"></a>Azure Bilişsel Arama yenilikleri

Hizmette nelerin yeni olduğunu öğrenin. Hizmetle güncel kalmasını sağlamak için bu sayfaya yer işareti ekleyin.

<a name="new-service-name"></a>

## <a name="new-service-name-for-azure-search"></a>Azure Search için yeni hizmet adı

Azure Search, çekirdek işlemlerde bilişsel yeteneklerin ve AI işlemenin genişletilmiş kullanımını yansıtmak için artık **Azure bilişsel arama** olarak yeniden adlandırıldı. Bilişsel yetenekler yeni yetenekler Ekken, AI kullanmak kesinlikle isteğe bağlıdır. Bulutta oluşturduğunuz ve yönettiğiniz bir dizinde özel, heterojen, metin tabanlı içeriğe göre zengin, tam metin arama çözümleri oluşturmak için Azure Bilişsel Arama 'yi AI olmadan kullanmaya devam edebilirsiniz. 

API sürümleri, NuGet paketleri, ad alanları ve uç noktalar değiştirilmez. Mevcut arama çözümlerinizi hizmet adı değişikliğinden etkilenmez.

## <a name="feature-announcements"></a>Özellik Duyuruları

4 Kasım 2019-Menite Konferansı

+ Artık önizlemede olan [artımlı dizin oluşturma](cognitive-search-incremental-indexing-conceptual.md), yalnızca bir zenginleştirme ardışık düzeninde değişiklik yaparken kesinlikle gerekli olan adımları işleyebilir veya yeniden işleyebilirsiniz. Bu özellikle, daha önce analiz ettiğiniz görüntü içeriğiniz varsa yararlıdır. Maliyetli çözümlemenin çıktısı depolanır ve ek dizin oluşturma veya zenginleştirme için temel olarak kullanılır.

<!-- 
+ Custom Entity Lookup is a cognitive skill used during indexing that allows you to provide a list of custom entities (such as part numbers, diseases, or names of locations you care about) that should be found within the text. It supports fuzzy matching, case-insensitive matching, and entity synonyms. -->

+ [Belge ayıklama](cognitive-search-skill-document-extraction.md) , bir dosyanın içeriğini bir beceri içinden ayıklamanızı sağlayan dizin oluşturma sırasında kullanılan bilişsel bir yetenişdir. Daha önce belge çözme yalnızca beceri yürütmeden önce oluşmuştur. Bu beceriye ek olarak, bu işlemi beceri yürütme içinde de gerçekleştirebilirsiniz.

+ [Metin çevirisi](cognitive-search-skill-text-translation.md) , dizinleme sırasında metni değerlendiren ve her kayıt için belirtilen hedef dile çevrilen metni döndüren bilişsel bir yetenküldür.

+ [Power BI şablonlar](https://github.com/Azure-Samples/cognitive-search-templates/blob/master/README.md) , görselleştirmelerinizi ve Power BI masaüstündeki bir bilgi deposunda zenginleştirme içeriği analizini başlatabilir. Bu şablon, [veri Içeri aktarma Sihirbazı](knowledge-store-create-portal.md)aracılığıyla oluşturulan Azure Tablo projeksiyonlarını için tasarlanmıştır.

## <a name="service-updates"></a>Hizmet güncelleştirmeleri

Azure Bilişsel Arama için [hizmet güncelleştirme duyuruları](https://azure.microsoft.com/updates/?product=search&status=all) Azure Web sitesinde bulunabilir.