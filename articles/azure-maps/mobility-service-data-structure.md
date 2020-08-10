---
title: Azure haritalar 'da Mobility hizmeti veri yapıları | Microsoft Azure haritaları
description: Azure haritalar Mobility hizmetlerinde verilerin Metro alanlarında nasıl düzenlendiğini anlayın. Bkz. genel transit hakkında bilgileri depolayan alanları ve çizgileri.
author: anastasia-ms
ms.author: v-stharr
ms.date: 06/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 02d89226bd2df4bfe5d11897199c50c702e7bc1c
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88033232"
---
# <a name="data-structures-in-azure-maps-mobility-service"></a>Azure Maps Mobility hizmetindeki veri yapıları

Bu makalede, [Azure Maps Mobility hizmeti](https://aka.ms/AzureMapsMobilityService)'Nde Metro alanı kavramı tanıtılmaktadır. Bu hizmet, genel aktarım duraklarına ve çizgilere yönelik olarak sorgulandığında döndürülen bazı yaygın alanları tartıştık. Mobility hizmeti API 'Leri ile geliştirmeye başlamadan önce bu makaleyi okumanızı öneririz.

## <a name="metro-area"></a>Metro alanı

Mobility hizmeti verileri, desteklenen Metro alanlarıyla gruplandırılır. Metro alanlarında şehir sınırları izdeğildir. Bir metro alanı birden çok şehir, daha seyrek doldurulmuş şehir ve çevreleyen şehirler içerebilir. Aslında bir ülke/bölge bir metro alanı olabilir. 

, `metroID` [Metro alanı BILGILERINI al API](https://aka.ms/AzureMapsMobilityMetroAreaInfo)'sini çağırmak için kullanılabilen BIR Metro alanı kimliğidir. Aktarım türlerini, transit kuruluşlarını, etkin uyarıları ve seçilen Metro için ek ayrıntıları istemek üzere Azure Maps ' "Metro 'yı al" API 'sini kullanın. Ayrıca, desteklenen Metro alanını ve metroIDs isteyebilirsiniz. Metro alanı kimlikleri değişikliğe tabidir.

**metroID:** 522 **ad:** Seattle-Tacoma-Bellevue

![Seattle-Metro alanı](./media/mobility-service-data-structure/seattle-metro.png)

## <a name="stop-ids"></a>Kimliği durdur

Aktarım duraklarına iki tür kimlik, [genel transit akış belirtimi (GFTS)](http://gtfs.org/) kimliği ve Azure HARITALAR durdurma kimliği tarafından başvurulabilir. GFTS ID, stopKey olarak adlandırılır ve Azure Maps durdurma KIMLIĞI, stopID olarak adlandırılır. Aktarım için sık sık yanıt durdurulduğunda, Azure haritalar durdurma KIMLIĞINI kullanmanız önerilir. stopID daha kararlı ve büyük ihtimalle fiziksel durun var olduğu sürece aynı kalabiliyor. GTFS durdurma KIMLIĞI daha sık güncelleştirilir. Örneğin, gtfs durdurma KIMLIĞI, GTFS sağlayıcı isteği başına ya da yeni bir GTFS sürümü yayınlandığında güncelleştirilebilen olabilir. Fiziksel durda değişikliğe sahip olmasa da, GTFS durdurma KIMLIĞI değişebilir.

Başlamak için yakında ulaşım [API 'Sini al](https://aka.ms/AzureMapsMobilityNearbyTransit)' ı kullanarak yakındaki geçiş işlemini izleyebilirsiniz.

## <a name="line-groups-and-lines"></a>Satır grupları ve satırlar

Mobility hizmeti, satırlar ve satır grupları için bir paralel veri modeli kullanır. Bu model, [Gtfs](http://gtfs.org/) rotalarından ve gezme verilerinden devralınan değişikliklerle daha iyi uğraşmak için kullanılır.


### <a name="line-groups"></a>Satır grupları

Bir satır grubu, aynı grubun mantıksal bir parçası olan tüm satırları gruplandıran bir varlıktır. Genellikle, bir satır grubu iki satır içerir, biri A 'dan B 'ye ve diğeri B 'den A 'ya döndürülüyor. Her iki satır da aynı ortak taşıma ajanına ait ve aynı satır numarasına sahip olur. Ancak, bir satır grubunun iki satırı veya yalnızca tek bir satırı içerdiği durumlar olabilir.


### <a name="lines"></a>Satırlar

Yukarıda açıklandığı gibi, her satır grubu bir dizi satırdan oluşur. Her satır grubu iki satırdan oluşur ve her satır bir yönü açıklar.  Ancak, daha fazla satır bir satır grubu oluşturan durumlar vardır. Örneğin, bazen belirli bir komşuları içinde Gezinmeyen ve bazen olmayan bir çizgi vardır. Her iki durumda da, aynı satır numarası altında çalışır. Ayrıca, bir satır grubu tek bir satırdan oluşabilir. Tek yönlü bir dairesel çizgi, tek satırlık bir daire grubudur.

Başlamak için, [geçiş satırı al API](https://aka.ms/AzureMapsMobilityTransitLine)'sini kullanarak satır grupları isteyebilirsiniz.


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
