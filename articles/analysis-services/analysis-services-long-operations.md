---
title: Azure Analiz Hizmetleri'nde uzun süreli işlemler için en iyi uygulamalar | Microsoft Dokümanlar
description: Bu makalede, uzun süren işlemler için en iyi uygulamalar açıklanmaktadır.
author: minewiskan
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: owend
ms.openlocfilehash: 3f6b2194cc422a827bbc7a15c012173b3f814b52
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81428114"
---
# <a name="best-practices-for-long-running-operations"></a>Uzun süren operasyonlar için en iyi uygulamalar

Azure Çözümleme Hizmetleri'nde *düğüm,* sunucu kaynağının çalıştığı ana bilgisayar sanal makinesini temsil eder. Uzun süren sorgular, yenileme işlemleri ve sorgu ölçeklendirme eşitleme gibi bazı işlemler, sunucu kaynağı farklı bir düğüme taşınırsa başarısız olabilir. Bu senaryodaki yaygın hata iletileri şunlardır:

- "Uzun süredir devam eden bir XMLA isteğini bulmaya çalışırken bir hata oluştu. İstek, hizmet yükseltmesi veya sunucu yeniden başlatma sı tarafından kesintiye uğramış olabilir."
- "Model için<guid>ID '<database>ile İş ' ' ' hizmet hatası (hareketsizlik) iletisi ile iptal edildi 'Herhangi bir güncelleştirme olmadan sıkışmış çünkü yenileme isteği iptal. Bu bir dahili hizmet sorunudur. Lütfen bu sorun tekrar tekrar olursa, lütfen işi yeniden gönderin veya yardım almak için bir bilet gönderin."

Uzun süren operasyonların kesintiye uğraması için birçok neden vardır. Örneğin, Azure'daki güncelleştirmeler: 
- İşletim Sistemi yamaları 
- Güvenlik güncelleştirmeleri
- Azure Analiz Hizmetleri hizmet güncelleştirmeleri
- Servis Kumaş güncellemeleri. Service Fabric, Azure Analiz Hizmetleri de dahil olmak üzere bir dizi Microsoft bulut hizmeti tarafından kullanılan bir platform bileşenidir.

Hizmette meydana gelen güncelleştirmelerin yanı sıra, yük dengelemesi nedeniyle düğümler arasında doğal bir hizmet hareketi vardır. Düğüm hareketleri bulut hizmetinin beklenen bir parçasıdır. Azure Analiz Hizmetleri düğüm hareketlerinden kaynaklanan etkileri en aza indirmeye çalışır, ancak bunları tamamen ortadan kaldırmak imkansızdır. 

Düğüm hareketlerine ek olarak, oluşabilecek başka hatalar da vardır. Örneğin, bir veri kaynağı veritabanı sistemi çevrimdışı olabilir veya ağ bağlantısı kaybolur. Yenileme sırasında bir bölüm 10 milyon satıra sahipse ve 9 milyonuncu satırda bir hata oluşuyorsa, hata noktasında yenilemeyi yeniden başlatmanın bir yolu yoktur. Hizmet baştan başlamak zorundadır. 

## <a name="refresh-rest-api"></a>REST API'yi yenile

Hizmet kesintileri, veri yenileme gibi uzun süren işlemler için zor olabilir. Azure Çözümleme Hizmetleri, hizmet kesintilerinden kaynaklanan olumsuz etkileri azaltmaya yardımcı olmak için bir REST API içerir. Daha fazla bilgi için [REST API ile Asynchronous yenile'ye](analysis-services-async-refresh.md)bakın.
 
REST API'sinin yanı sıra, uzun süren yenileme işlemleri sırasında olası sorunları en aza indirmek için kullanabileceğiniz başka yaklaşımlar da vardır. Temel amaç, yenileme işlemini baştan başlatmak zorunda kalmamak ve bunun yerine aşamalar halinde işlenebilecek daha küçük gruplar halinde yenilemeler gerçekleştirmektir. 
 
REST API bu tür yeniden başlatma ya da bölüm oluşturma ve silme tam esneklik için izin vermez. Bir senaryo karmaşık veri yönetimi işlemleri gerektiriyorsa, çözümünüzün mantığına bir tür toplu iş ekleme içermelidir. Örneğin, verileri birden çok, ayrı toplu iş halinde işlemek için hareketleri kullanarak, başlangıçtan itibaren yeniden başlatılmasını değil, bunun yerine bir ara denetim noktasından yeniden başlatmayı gerektirmemesine izin verir. 
 
## <a name="scale-out-query-replicas"></a>Ölçeklendirme sorgu yinelemeleri

İster REST ister özel mantık kullanarak, istemci uygulama sorguları toplu işlenirken yine de tutarsız veya ara sonuçlar döndürebilir. İşleme olurken istemci uygulama sorguları tarafından döndürülen tutarlı veriler gerekiyorsa ve model verileri bir ara durumdaysa, salt okunur sorgu yinelemeleriyle [ölçeklendirmek](analysis-services-scale-out.md) kullanabilirsiniz.

Salt okunur sorgu yinelemelerini kullanarak, yenilemeler toplu olarak gerçekleştirilirken, istemci uygulama kullanıcıları yalnızca okunan yinelemelerde eski veri anlık görüntüsünü sorgulamaya devam edebilir. Yenilemeler tamamlandıktan sonra, salt okunur yinelemeleri güncel getirmek için bir Eşitleme işlemi gerçekleştirilebilir.


## <a name="next-steps"></a>Sonraki adımlar

[REST API ile zaman uyumsuz yenileme](analysis-services-async-refresh.md)  
[Azure Analysis Services ölçeğini genişletme](analysis-services-scale-out.md)  
[Analiz Hizmetleri yüksek kullanılabilirlik](analysis-services-bcdr.md)  
[Azure hizmetleri için kılavuzu yeniden deneyin](https://docs.microsoft.com/azure/architecture/best-practices/retry-service-specific)   

