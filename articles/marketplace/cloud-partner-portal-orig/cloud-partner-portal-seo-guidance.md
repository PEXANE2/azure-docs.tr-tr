---
title: Azure Market SEO Kılavuzu
description: Arama motoru iyileştirmesini (SEO) en üst düzeye çıkarma hakkında rehberlik sağlar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: dsindona
ms.openlocfilehash: 761cdc2233bce3619d4c2c9ce1d7d7177d3bc407
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80280159"
---
# <a name="azure-marketplace-seo-guidance"></a>Azure Market SEO Kılavuzu

Bu makalede, [Azure Marketi](https://azuremarketplace.microsoft.com) ve [appsource](https://appsource.microsoft.com)'ta arama işlevselliği aracılığıyla teklifinizin bulunabilirliği nasıl en üst düzeye çıkarabileceğiniz açıklanır. 


## <a name="general-explanation-of-algorithm"></a>Algoritmanın genel açıklaması

Microsoft marketler, sitenin arama yeteneklerini güçetmek için Azure Bilişsel Arama kullanır. Algoritma, dönem sıklığı – ters belge sıklığı ([tf-ıDF](https://en.wikipedia.org/wiki/Tf–idf)) temel alır. Standart [Lucene Çözümleyicisi](https://lucene.apache.org/core/) kullanılır.

Genel olarak, tüm metin alanları, Kategoriler ve endüstriler ve ilgiye ait ağırğa dahil edilmiştir. Uygulamalar tarafından nadiren kullanılan ancak uygulamanızda sık kullanılan özel terimler arama ile daha yüksek bir eşleşme puanı oluşturacaktır. "Sanal makine" gibi şartlar da dahil olmak çok yararlı olacaktır, "Azure Search" çok daha özelleştirilmiş olur.
Dikkate alınması gereken en ilgili alanlar aşağıda verilmiştir.

 
|  Alan                   | Önem | Rehber                                                                                            |
|  --------------------    | ----------                   | ---------------                                                                   |
| Teklif Adı               |  Yüksek      | Arama sorgusuyla tam olarak tam eşleşme veya kapatma yüksek sıralama sağlar.                       |
| Yayımcı Adı           |  Yüksek      | Arama sorgusuyla tam olarak tam eşleşme veya kapatma yüksek sıralama sağlar.                       |
| Kısa Açıklama        |  Orta    | Uygulamalar ve yayımcı adları için verilen adlandırma, neredeyse yüksek bir derecelendirme garantisi sağlayacak ve bu, en ilgili olmayabilir. Bu durumda, kısa bir açıklama kritik öneme sahiptir. Metni kısa ve noktaya tutun. En iyi sonuç için anahtar sözcükler ve beklenen arama terimleri eklenmelidir.  Örneğin, "Dynamics 365 ' in en üstünde eksiksiz olarak oluşturulan en iyi Retail POS, Dynamics 365 için" Retail POS (satış noktası) "daha az etkilidir.  | 
| Uzun açıklama         |  Düşük       | Açıklama, daha ayrıntılı bir şekilde gitmeniz için bir yol sunar. En etkili açıklamalar makul uzunluktadır ve anahtar sözcüklerdir.  Anahtar sözcüklerin kullanıldığı noktadan noktaya açıklamalar uzun, uzun metin avantajına sahip olur. "IoT" gibi anahtar terimlerin açıklamasında bulunduğundan emin olun.  |
| Ürün kategorileri       | Orta     |  Ürün kategorileri, yayımcı seçimlerinin ve Microsoft 'un birleşimiyle belirlenir. Kullanıcıların uygulamaları doğru kategoride kolayca bulabilmesi için bu kategorileri uygun şekilde seçin. |
|  |  |  |


## <a name="other-tips"></a>Diğer Ipuçları

-   Arama, ağır Kullanıcı etkinliği alır. Uygulama adı/yayımcısı ile eşleşmeleri önceliklendirir. Kısa açıklama, arama teriminin yayımcı/uygulama adıyla tam bir eşleşme olmaması durumunda için anahtar alanı olur.
-   İndirileceği belgeler, arama ağırlıklı olarak dahil edilmez.
-   Uygulamalarınızın gerçek alımı ve kullanımı, arama derecelendirmesini da etkiler. Örneğin, bir büyük ölçüde daha fazla kullanıcının daha yüksek bir derecelendirme aldığı iki eşdeğer uygulama.
