---
title: Bing öngörülerine örnekler - Bing Görsel Arama
titleSuffix: Azure Cognitive Services
description: Bu makalede, Bing Görsel Arama'nın Bing.com'da görüntü öngörülerini nasıl kullanabileceği ve görüntü olarak görüntüleyebileceğine ilişkin örnekler yer alıyor.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: scottwhi
ms.openlocfilehash: df66dbeebb04209921ff91c4b99a14580f026718
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74111644"
---
# <a name="examples-of-bing-insights-usage"></a>Bing öngörüleri kullanımına örnekler

Bu makalede, Bing'in Bing.com'da görüntü öngörülerini nasıl kullanabileceğine ve görüntüleyebileceğine ilişkin örnekler yer alıyor.

## <a name="pagesincluding-insight-example"></a>Sayfaİçbilgi örneği dahil

Aşağıdaki ilk web sayfasına bir bağlantı görüntüler ve kullanıcının görüntüyü içeren diğer web sayfalarının listesini genişletmesine ve daraltmasını sağlar:

![Dahil olmak üzere genişletilmiş sayfalar](./media/pages-including.PNG)

## <a name="shoppingsources-insight-example"></a>ShoppingSources içgörü örneği

Aşağıda, Bing'in resimde görülen ürünler için alışveriş kaynaklarını nasıl görüntüleyebileceği gösterilmektedir:

![Alışveriş kaynakları](./media/shopping-sources.PNG)

## <a name="visualsearch-insight-example"></a>VisualSearch içgörü örneği

Aşağıda Bing'in görsel olarak benzer görüntüleri nasıl görüntüleyebileceği gösterilmektedir (örnekteki **İlgili resimlere** bakın):

![Görsel olarak benzer görüntüler](./media/similar-images.PNG)

## <a name="recipes-insight-example"></a>Yemek tarifleri içgörü örneği

Aşağıda Bing'in resimde gösterilen yiyeceklerin tariflerini nasıl gösterebileceği gösterilmektedir. Örnek, kullanıcıya mevcut tarifler olduğunu bildirir:

![Yemek tarifleri ve sayfalar dahil](./media/recipes-pages-including.PNG)

 Ve kullanıcı listeyi genişletir zaman tarifleri için bağlantı sağlar:

![Dahil olmak üzere genişletilmiş yemek tarifi sayfaları](./media/expanded-recipes-pages-including.PNG)

## <a name="relatedsearches-insight-example"></a>RelatedAramalar içgörü örneği

Aşağıda Bing'in başkaları tarafından yapılan resimlerin ilgili aramalarını nasıl görüntüleyebileceği gösterilmektedir. Kullanıcı resmi tıklarsa, kullanıcı ilgili sorgu için Bing.com/images arama sonuçları sayfasına götürülür.

![Resimler için ilgili aramalar](./media/bordered-related-searches.PNG)

## <a name="entity-insight-example"></a>Varlık içgörüsü örneği

Aşağıda Bing'in resimde gösterilen varlık (kişi, yer veya şey) hakkındaki bilgileri nasıl görüntüleyebileceği gösterilmektedir. Kullanıcı varlık bağlantısını tıklarsa, kullanıcı varlığın Bing.com arama sonuçları sayfasına alınır:

![Resimde gösterilen varlık](./media/entity.PNG)

## <a name="displaying-other-insights-that-the-user-might-explore"></a>Kullanıcının keşfedebileceği diğer öngörüleri görüntüleme

Aşağıda Bing'in kullanıcının keşfedebileceği görüntü yle ilgili diğer bilgileri nasıl görüntüleyebileceği gösterilmektedir.

![Resim le ilgili diğer bilgileri keşfedin](./media/apple-pie-more-tags.PNG)

## <a name="bounding-boxes-and-hot-spots"></a>Sınırlayıcı kutular ve sıcak noktalar

Varsayılan olmayan etiketler, etiketin uygulandığı görüntüdeki ilgi alanını tanımlayan sınırlayıcı kutuyu içerir. Sınırlayıcı kutu görüntünün tamamını tanımlamazsa, görüntüüzerinde sıcak bir nokta oluşturmak için sınırlayıcı kutuyu kullanın. Kullanıcı, sıcak noktanın (veya dikdörtgenin) altında bulunan içerikle ilgili bilgi almak için sıcak noktayı tıklatabilir. Örneğin, görüntü yüksek moda bir görüntüyse, sonuçlar görüntüde gösterilen çanta, takı, eşarp vb. aksesuarlar için etiketler (ve sınırlayıcı kutular) içerebilir. Aşağıdaki örnek, resimde gösterilen güneş gözlüğü için sıcak nokta dikdörtgeni gösterir:

![Sınırlama kutusu ve sıcak nokta](./media/click-to-search.PNG)

## <a name="next-steps"></a>Sonraki adımlar

İlk isteğinizle başlamak için hızlı başlangıçlara bakın: [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [düğümü.js](quickstarts/nodejs.md) | [Python](quickstarts/python.md)





