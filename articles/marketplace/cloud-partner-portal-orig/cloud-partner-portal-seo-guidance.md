---
title: Azure Marketplace SEO kılavuzu
description: Arama motoru optimizasyonu (SEO) en üst düzeye çıkarma konusunda rehberlik sağlar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: dsindona
ms.openlocfilehash: 761cdc2233bce3619d4c2c9ce1d7d7177d3bc407
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280159"
---
# <a name="azure-marketplace-seo-guidance"></a>Azure Marketplace SEO kılavuzu

Bu makalede, [Azure Marketi](https://azuremarketplace.microsoft.com) ve [AppSource'daki](https://appsource.microsoft.com)arama işlevleri aracılığıyla teklifinizin keşfedilebilirliğini nasıl en üst düzeye çıkarılabiliyoruz. 


## <a name="general-explanation-of-algorithm"></a>Algoritmanın genel açıklaması

Microsoft pazar yerleri, sitenin arama özelliklerine güç katif etmek için Azure Bilişsel Arama'yı kullanır. Algoritma terim frekans-ters belge frekansı[(TF-IDF)](https://en.wikipedia.org/wiki/Tf–idf)dayanmaktadır. Standart [Lucene Analyzer](https://lucene.apache.org/core/) kullanılır.

Genel olarak, tüm metin alanları, kategoriler ve endüstriler ve alaka ağırlığı dahil. Uygulamalar tarafından seyrek olarak kullanılan ancak uygulamanızda sıklıkla kullanılan özel terimler, arama ile daha yüksek bir maç puanı oluşturur. Yani "VM" gibi terimler de dahil olmak üzere çok az fayda sunacak, oysa "Azure arama" çok daha özel olacaktır.
Aşağıda göz önünde bulundurulması gereken en alakalı alanlar verilmiştir.

 
|  Alan                   | Önem | Rehber                                                                                            |
|  --------------------    | ----------                   | ---------------                                                                   |
| Teklif Adı               |  Yüksek      | Tam veya yakın arama sorgusu ile tam bir eşleşme yüksek rütbeli verecektir.                       |
| Yayımcı Adı           |  Yüksek      | Tam veya yakın arama sorgusu ile tam bir eşleşme yüksek rütbeli verecektir.                       |
| Kısa Açıklama        |  Orta    | Uygulamaların ve yayıncı adlarının adlandırılmasının neredeyse yüksek bir sıralamayı garanti edeceği göz önüne alındığında, en alakalı sı yatsı olmayabilir. Bu durumda, kısa bir açıklama önemlidir. Metni kısa ve öz tutun. Anahtar kelimeler ve beklenen arama terimleri en iyi sonuç için eklenmelidir.  Örneğin "Bu, Dynamics 365'in üzerine tamamen inşa edilmiş en iyi Perakende POS'udur" Dynamics 365 için "Retail POS (satış noktası) "den daha az etkilidir.  | 
| Uzun Açıklama         |  Düşük       | Açıklama daha derine gitmek için bir yol sunar. En etkili açıklamalar makul uzunluktadır ve anahtar kelimeler kullanılır.  Anahtar kelimeler kullanan bir noktaya değinme açıklamaları, uzun ve uzun metinden daha fazla yarar sağlar. "IoT" gibi anahtar terimlerin açıklamada bulunduğundan emin olun.  |
| Ürün Kategorileri       | Orta     |  Ürün kategorileri yayımcı seçenekleri ve Microsoft'un bir birleşimi tarafından belirlenir. Kullanıcıların uygulamaları doğru kategoride kolayca bulabilmeleri için bu kategorileri uygun şekilde seçin. |
|  |  |  |


## <a name="other-tips"></a>Diğer İpuçları

-   Arama, ağır kullanıcı etkinliği alır önerir. Uygulama adı/yayıncıyla eşleşenlere öncelik verir. Kısa açıklama, arama teriminin yayımcı/uygulama adı ile tam olarak eşleşmemesi için anahtar alan olur.
-   İndirilen belgeler arama ağırlığına dahil edilmez.
-   Uygulamalarınız gerçek satın alma ve kullanımı arama sıralamasını da etkileyecektir. Örneğin, çok daha fazla kullanıcıya sahip olan iki eşdeğer uygulama daha yüksek bir sıralama alırsınız.
