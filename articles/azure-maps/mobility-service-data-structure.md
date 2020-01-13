---
title: Azure haritalar 'da Mobility hizmeti veri yapıları | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure Maps Mobility Hizmetleri aracılığıyla döndürülen ortak alanlar ve veri yapıları hakkında bilgi edineceksiniz.
author: walsehgal
ms.author: v-musehg
ms.date: 06/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 213910ee2439fa958b9f1d4926883eb8e066ba41
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2020
ms.locfileid: "75910716"
---
# <a name="data-structures-in-azure-maps-mobility-service"></a>Azure Maps Mobility hizmetindeki veri yapıları

Bu makalede, [Azure Maps Mobility hizmetinde](https://aka.ms/AzureMapsMobilityService) Metro alanı kavramı ve hizmetler aracılığıyla, genel aktarım duraklarına ve çizgilere yönelik sorgulanırken bazı ortak alanlar tanıtılmaktadır. Mobility hizmeti API 'Leri ile çalışmaya başlamadan önce bu makaleyi kullanmanızı öneririz. Aşağıda bu ortak alanları tartıştık.

## <a name="metro-area"></a>Metro alanı

Mobility hizmeti verileri, desteklenen Metro alanlarında bölünür. Metro alanlarında şehir sınırları uygulanmaz, bir metro alanı birden çok şehir içerebilir; Örneğin, daha seyrek doldurulmuş şehir ve onun çevresindeki şehir; bir ülke/bölge de bir metro alanı olabilir. 

`metroID`, desteklenen aktarım türlerini ve aktarım kurumları ve etkin uyarılar gibi Metro alanı için ek ayrıntıları istemek üzere [Metro alanı bilgilerini al API](https://aka.ms/AzureMapsMobilityMetroAreaInfo) 'sini çağırmak için kullanılabilen bir metro alanı kimliğidir. Desteklenen Metro alanını ve metroIDs istemek için Azure haritalar 'ın Metro API 'sini kullanmasını sağlayabilirsiniz. Metro alanı kimlikleri değişikliğe tabidir.

**metroID:** 522 **ad:** Seattle-Tacoma-Bellevue

![Seattle-Metro alanı](./media/mobility-service-data-structure/seattle-metro.png)

## <a name="stop-ids"></a>Kimliği durdur

Aktarım duraklarına iki tür kimlik, [genel transit akış belirtimi (GFTS)](https://gtfs.org/) kimliği (stopkey olarak adlandırılır) ve Azure HARITALAR durdurma kimliği (stopid olarak adlandırılır) tarafından başvurulabilir. Zamana göre durdurulduğunda, bu KIMLIK daha kararlı olduğundan ve fiziksel durun varolduğu sürece değişmeyecek olduğundan, Azure haritalar durdurma KIMLIĞINI kullanmanız önerilir. GTFS durdurma KIMLIĞI daha sık güncelleştirilir. Örneğin, GTFS sağlayıcısının bunu değiştirmesi veya yeni bir GTFS sürümü yayınlanması durumunda, fiziksel durun hiçbir değişikliği olmamasına karşın.

Başlamak için, yakın transit isteklerini [Al](https://aka.ms/AzureMapsMobilityNearbyTransit)' ı kullanarak yakında geçiş durduruluyor.

## <a name="line-groups-and-lines"></a>Satır grupları ve satırlar

Mobility hizmeti, [Gtfs](https://gtfs.org/) rotalarından ve gezileri veri modelinden devralınan değişikliklerle daha iyi uğraşmak için satırlar ve satır grupları için bir paralel veri modeli kullanır.


### <a name="line-groups"></a>Satır grupları

Bir satır grubu, aynı grubun mantıksal bir parçası olan tüm satırları gruplandıran bir varlıktır. Genellikle bir satır grubu iki satır, biri A 'dan B 'ye, diğeri de aynı genel taşıma ajanına ait olan ve aynı satır numarasına sahip olacak şekilde B 'den A 'ya gidiyor. Ancak, bir satır grubunun iki satırı veya yalnızca tek bir satırı içerdiği durumlar olabilir.


### <a name="lines"></a>Satırlar

Yukarıda açıklandığı gibi, her satır grubu bir dizi satırdan oluşur. Genellikle her satır bir yönü açıklar ve her satır grubu iki satırdan oluşur. Ancak, bir satır grubunu daha fazla satır oluşturan durumlar vardır. Örneğin, bazen belirli bir semte gezinerek bir satır vardır ve bazen aynı satır numarası altındaki her iki durumda da çalıştırılır ve bir satır g rubu tek bir satırdan oluşur, örneğin tek yönlü dairesel bir çizgi.

Başlamak için, [geçiş satırı al API 'sini](https://aka.ms/AzureMapsMobilityTransitLine) ve daha sonra satırlarda detaya gitmeyi kullanarak satır grupları isteyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Mobility hizmetini kullanarak geçiş verileri isteme hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Geçiş verileri isteme](how-to-request-transit-data.md)

Mobility hizmetini kullanarak gerçek zamanlı veriler isteme hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Gerçek zamanlı veriler isteme](how-to-request-real-time-data.md)

Azure haritalar Mobility hizmeti API 'SI belgelerini inceleyin

> [!div class="nextstepaction"]
> [Mobility hizmeti API 'SI belgeleri](https://aka.ms/AzureMapsMobilityService)
