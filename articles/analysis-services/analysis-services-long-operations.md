---
title: Azure Analysis Services ' de uzun süre çalışan işlemler için en iyi uygulamalar | Microsoft Docs
description: Bu makalede, uzun süre çalışan işlemler için en iyi yöntemler açıklanmaktadır.
author: minewiskan
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: owend
ms.openlocfilehash: 3f6b2194cc422a827bbc7a15c012173b3f814b52
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81428114"
---
# <a name="best-practices-for-long-running-operations"></a>Uzun süre çalışan işlemler için en iyi uygulamalar

Azure Analysis Services, *düğüm* bir sunucu kaynağının çalıştığı konak sanal makinesini temsil eder. Sunucu kaynağı farklı bir düğüme taşınırsa, uzun süre çalışan sorgular, yenileme işlemleri ve sorgu genişleme eşitlemesi gibi bazı işlemler başarısız olabilir. Bu senaryodaki yaygın hata iletileri şunları içerir:

- "Uzun süreli çalışan bir XMLA isteği bulmaya çalışırken bir hata oluştu. İstek, hizmet yükseltmesi veya sunucu yeniden başlatma tarafından kesintiye uğramış olabilir. "
- "'<guid><database>' MODELI için ' ' kimlikli iş, hiçbir güncelleştirme olmadan takıldığından dolayı yenileme isteğini iptal etme iletisi olan hizmet hatası nedeniyle iptal edildi. Bu bir iç hizmet sorunudur. Bu sorun sürekli olarak gerçekleşirse yardım almak için lütfen işi yeniden gönderin veya bir bilet dosyası yapın. "

Uzun süre çalışan işlemlerin kesintiye uğramamasının pek çok nedeni vardır. Örneğin, Azure 'daki güncelleştirmeler: 
- İşletim sistemi düzeltme ekleri 
- Güvenlik güncelleştirmeleri
- Azure Analysis Services hizmet güncelleştirmeleri
- Service Fabric güncelleştirmeler. Service Fabric, Azure Analysis Services dahil olmak üzere çok sayıda Microsoft bulut hizmeti tarafından kullanılan bir platform bileşenidir.

Hizmette gerçekleşen güncelleştirmelerin yanı sıra, Yük Dengeleme nedeniyle düğümler arasında doğal bir hizmet taşıması vardır. Düğüm hareketleri, bir bulut hizmetinin beklenen bir parçasıdır. Azure Analysis Services, düğüm hareketilerinden etkileri en aza indirmenize çalışır, ancak tamamen ortadan kaldırmak olanaksızdır. 

Düğüm hareketlerine ek olarak, oluşabilecek başka sorunlar da vardır. Örneğin, bir veri kaynağı veritabanı sistemi çevrimdışı olabilir veya ağ bağlantısı kaybolabilir. Yenileme sırasında, Bölüm 10.000.000 satıra sahiptir ve 9 milionth satırında bir hata oluşursa yenilemeyi hata durumunda yeniden başlatmanın bir yolu yoktur. Hizmetin baştan itibaren yeniden başlatılması gerekebilir. 

## <a name="refresh-rest-api"></a>REST API Yenile

Hizmet kesintileri, veri yenileme gibi uzun süre çalışan işlemler için zor olabilir. Azure Analysis Services, hizmet kesintilerinden olumsuz etkileri azaltmaya yardımcı olmak için bir REST API içerir. Daha fazla bilgi için bkz. [REST API zaman uyumsuz yenileme](analysis-services-async-refresh.md).
 
REST API yanı sıra, uzun süre çalışan yenileme işlemleri sırasında olası sorunları en aza indirmek için kullanabileceğiniz başka yaklaşımlar de vardır. Ana amaç, yenileme işlemini baştan yeniden başlatmak zorunda kalmamak ve bunun yerine, aşamalar halinde kaydedilebilen daha küçük toplu işlerle yenileme işlemi gerçekleştirmektir. 
 
REST API, bu yeniden başlatmaya izin verir, ancak bölüm oluşturma ve silmenin tam esnekliğine izin vermez. Bir senaryo karmaşık veri yönetimi işlemleri gerektiriyorsa, çözümünüz mantığındaki bazı toplu işlem formunu içermelidir. Örneğin, verileri birden çok işlemek için işlemleri kullanarak ayrı toplu işler, bir başarısızlık için baştan başlatma gerektirmeyen bir ara kontrol noktasından izin verir. 
 
## <a name="scale-out-query-replicas"></a>Genişleme sorgu çoğaltmaları

REST veya özel mantık kullanarak, toplu işler işlenirken istemci uygulama sorguları hala tutarsız veya ara sonuçlar döndürebilir. İşlem sırasında istemci uygulama sorguları tarafından döndürülen tutarlı veriler gerekliyse ve model verileri ara durumundaysa, salt okuma sorgu çoğaltmalarıyla [genişleme](analysis-services-scale-out.md) kullanabilirsiniz.

Salt okuma sorgu çoğaltmaları kullanarak, yenilemeler toplu olarak gerçekleştirilirken, istemci uygulama kullanıcıları salt okuma çoğaltmalarıyla verilerin eski anlık görüntüsünü sorgulamaya devam edebilir. Yenileme tamamlandığında, salt okuma çoğaltmalarını güncel hale getirmek için bir eşitleme işlemi gerçekleştirilebilir.


## <a name="next-steps"></a>Sonraki adımlar

[REST API ile zaman uyumsuz yenileme](analysis-services-async-refresh.md)  
[Azure Analysis Services ölçeğini genişletme](analysis-services-scale-out.md)  
[Yüksek kullanılabilirlik Analysis Services](analysis-services-bcdr.md)  
[Azure hizmetleri için yeniden deneme Kılavuzu](https://docs.microsoft.com/azure/architecture/best-practices/retry-service-specific)   

