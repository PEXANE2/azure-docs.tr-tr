---
title: Azure Haritalar'da Mobilite Hizmeti veri yapıları| Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Mobilite Hizmetleri aracılığıyla döndürülen ortak alanlar ve veri yapıları hakkında bilgi edineceksiniz.
author: philmea
ms.author: philmea
ms.date: 06/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 30696c5dcb3353ea468aa78dbc107dae4d292edb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334438"
---
# <a name="data-structures-in-azure-maps-mobility-service"></a>Azure Haritalar Mobilite Hizmeti'ndeki veri yapıları

Bu makalede, [Azure Haritalar Mobilite Hizmeti'nde](https://aka.ms/AzureMapsMobilityService)Metro Alanı kavramı tanıtılıyor. Bu hizmet toplu taşıma durakları ve satırları için sorgulandığında döndürülen ortak alanlardan bazılarını tartışıyoruz. Mobilite Hizmeti API'leri ile geliştirmeden önce bu makaleyi okumanızı öneririz.

## <a name="metro-area"></a>Metro alanı

Mobilite Hizmeti verileri desteklenen metro alanlarına göre gruplandırılır. Metro alanları şehir sınırlarını takip etmez. Bir metro alanı birden fazla şehir, yoğun nüfuslu şehir ve çevre şehirler içerebilir. Aslında, bir ülke / bölge bir metro alanı olabilir. 

Metro `metroID` [Alanı Bilgi API alın](https://aka.ms/AzureMapsMobilityMetroAreaInfo)aramak için kullanılabilecek bir metro alanının kimliğidir. Transit türleri, transit acenteleri, etkin uyarılar ve seçilen metro için ek ayrıntılar istemek için Azure Haritalar'ın "Metro Al" API'sini kullanın. Ayrıca desteklenen metro alanları ve metroIDs talep edebilirsiniz. Metro alanı tbm'leri değişebilir.

**metroID:** 522 **Adı:** Seattle-Tacoma-Bellevue

![Seattle-metro alanı](./media/mobility-service-data-structure/seattle-metro.png)

## <a name="stop-ids"></a>Durdur

Geçiş durakları, genel [geçiş akışı belirtimi (GFTS)](https://gtfs.org/) kimliği ve Azure Haritaları durdurma kimliği olmak üzere iki tür kimlikle anılabilir. GFTS Kimliği stopKey olarak adlandırılır ve Azure Haritalar stop ID stopID olarak adlandırılır. Geçiş duraklarına sık sık atıfta bulunduğınızda, Azure Haritalar durdurma kimliğini kullanmanız için teşvik edilmektedir. stopID daha kararlıdır ve fiziksel duruş olduğu sürece aynı kalma olasılığı yüksektir. GTFS stop ID daha sık güncelleştirilir. Örneğin, GTFS stop ID, GTFS sağlayıcı isteğine göre veya yeni bir GTFS sürümü yayımlandığında güncellenebilir. Fiziksel duruşta değişiklik olmamasına rağmen, GTFS stop ID değişebilir.

Başlamak için, [Yakındaki Transit API'sini Al'ı](https://aka.ms/AzureMapsMobilityNearbyTransit)kullanarak yakındaki transit durakları isteyebilirsiniz.

## <a name="line-groups-and-lines"></a>Çizgi Grupları ve Çizgiler

Mobilite Hizmeti, Satırlar ve Satır Grupları için paralel bir veri modeli kullanır. Bu model, [GTFS](https://gtfs.org/) rotalarından ve geziler verilerinden devralınan değişikliklerle daha iyi başa çıkmak için kullanılır.


### <a name="line-groups"></a>Çizgi Grupları

Çizgi Grubu, mantıksal olarak aynı grubun parçası olan tüm satırları bir araya getiren bir varlıktır. Genellikle, bir çizgi grubu biri A noktasından B noktasına, diğeri ise B noktasından A noktasına dönen iki satır içerir. Her iki hat da aynı Toplu Taşıma dairesine ait olacak ve aynı hat numarasına sahip olacak. Ancak, bir satır grubunun içinde ikiden fazla satır veya yalnızca tek bir satır olduğu durumlar olabilir.


### <a name="lines"></a>Satırlar

Yukarıda da belirtildiği gibi, her satır grubu bir dizi satırdan oluşur. Her satır grubu iki satırdan oluşur ve her satır bir yön açıklar.  Ancak, daha fazla satır bir satır grubu oluşturan durumlar vardır. Örneğin, bazen belirli bir mahalleden geçen ve bazen olmayan bir çizgi vardır. Her iki durumda da, aynı satır numarası altında çalışır. Ayrıca bir çizgi grubu tek bir satırdan oluşabilir. Tek yönlü dairesel bir çizgi, tek satırlı bir ling grubudur.

Başlamak [için, Geçiş Hattı API'sini Al'ı](https://aka.ms/AzureMapsMobilityTransitLine)kullanarak satır grupları isteyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Mobilite Hizmetini kullanarak transit verilerini nasıl isteyeceğizi öğrenin:

> [!div class="nextstepaction"]
> [Aktarım verileri nasıl istenir?](how-to-request-transit-data.md)

Mobilite Hizmeti'ni kullanarak gerçek zamanlı verileri nasıl isteyeceğizi öğrenin:

> [!div class="nextstepaction"]
> [Gerçek zamanlı veri isteme](how-to-request-real-time-data.md)

Azure Haritalar Mobilite Hizmeti API belgelerini keşfedin

> [!div class="nextstepaction"]
> [Mobilite Hizmeti API belgeleri](https://aka.ms/AzureMapsMobilityService)
