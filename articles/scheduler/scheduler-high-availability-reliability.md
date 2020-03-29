---
title: Yüksek kullanılabilirlik ve güvenilirlik
description: Azure Zamanlayıcısı'nda yüksek kullanılabilirlik ve güvenilirlik hakkında bilgi edinin
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: article
ms.date: 08/16/2016
ms.openlocfilehash: 20c2054e168a9b17d9b4ab159cfefbf607ab6d11
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898555"
---
# <a name="high-availability-and-reliability-for-azure-scheduler"></a>Azure Zamanlayıcısı için yüksek kullanılabilirlik ve güvenilirlik

> [!IMPORTANT]
> [Azure Logic Apps,](../logic-apps/logic-apps-overview.md) [kullanımdan kaldırılan](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date)Azure Zamanlayıcısı'nın yerini alıyor. Zamanlayıcı'da ayarladığınız işlerle çalışmaya devam etmek için lütfen mümkün olan en kısa sürede [Azure Logic Apps'a geçirin.](../scheduler/migrate-from-scheduler-to-logic-apps.md) 
>
> Zamanlayıcı artık Azure portalında kullanılamıyor, ancak işlerinizi ve iş koleksiyonlarınızı yönetebilmeniz için [ŞU anda REST API](/rest/api/scheduler) ve [Azure Scheduler PowerShell cmdlets'i](scheduler-powershell-reference.md) kullanılabilir durumda kalır.

Azure Zamanlayıcısı, işleriniz için hem [yüksek kullanılabilirlik](https://docs.microsoft.com/azure/architecture/framework/#resiliency) hem de güvenilirlik sağlar. Daha fazla bilgi için [Zamanlayıcı için SLA'ya](https://azure.microsoft.com/support/legal/sla/scheduler)bakın.

## <a name="high-availability"></a>Yüksek kullanılabilirlik

Azure Zamanlayıcısı [son derece kullanılabilir] ve hem coğrafi yedekli hizmet dağıtımı hem de coğrafi bölge iş çoğaltma kullanır.

### <a name="geo-redundant-service-deployment"></a>Coğrafi yedekli hizmet dağıtımı

Azure [Zamanlayıcısı, bugün Azure tarafından desteklenen](https://azure.microsoft.com/global-infrastructure/regions/#services)hemen hemen her coğrafi bölgede kullanılabilir. Bu nedenle, barındırılan bir bölgedeki Azure veri merkezi kullanılamıyorsa, hizmetin başarısız yetenekleri Zamanlayıcı'yı başka bir veri merkezinden kullanılabilir hale getirebileceğinden Azure Zamanlayıcısı'nı kullanmaya devam edebilirsiniz.

### <a name="geo-regional-job-replication"></a>Coğrafi-bölgesel iş çoğaltma

Azure Zamanlayıcısı'ndaki kendi işleriniz Azure bölgelerinde çoğaltılır. Bu nedenle, bir bölgede kesinti varsa, Azure Zamanlayıcısı başarısız olur ve işinizin eşleştirilmiş coğrafi bölgedeki başka bir veri merkezinden çalıştığından emin olur.

Örneğin, Güney Orta ABD'de bir iş oluşturursanız, Azure Scheduler bu işi Kuzey Orta ABD'de otomatik olarak çoğaltır. Güney Orta ABD'de bir hata olursa, Azure Zamanlayıcısı işi Kuzey Orta ABD'de çalıştırın. 

![Coğrafi-bölgesel iş çoğaltma](./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png)

Azure Zamanlayıcısı, Azure'da bir hata olması durumunda verilerinizin aynı ancak daha geniş bir coğrafi bölgede kalmasını da sağlar. Yani, sadece yüksek kullanılabilirlik istediğinizde işinizi çoğaltmak zorunda değilsiniz. Azure Scheduler otomatik olarak işleriniz için yüksek kullanılabilirlik sağlar.

## <a name="reliability"></a>Güvenilirlik

Azure Zamanlayıcısı kendi yüksek kullanılabilirliğini garanti eder, ancak kullanıcı tarafından oluşturulan işlere farklı bir yaklaşım benimser. Örneğin, işinizin kullanılamayan bir HTTP bitiş noktasını çağrıştırdığını varsayalım. Azure Zamanlayıcısı, hataları işlemek için alternatif yollar sunarak işinizi başarıyla çalıştırmaya çalışır: 

* Yeniden deneme ilkeleri ayarlayın.
* Alternatif uç noktaları ayarlayın.

<a name="retry-policies"></a>

### <a name="retry-policies"></a>Yeniden deneme ilkeleri

Azure Scheduler yeniden deneme ilkeleri ayarlamanızı sağlar. Bir iş başarısız olursa, varsayılan olarak Zamanlayıcı işi 30 saniyelik aralıklarla dört kez daha yeniden dener. Bu yeniden deneme ilkesini, 30 saniyelik aralıklarla 10 kez veya günlük aralıklarla iki kez gibi daha az agresif gibi daha agresif hale getirebilirsiniz.

Örneğin, HTTP bitiş noktası çağıran haftalık bir iş oluşturduğunuzu varsayalım. HTTP bitiş noktası işiniz çalıştığında birkaç saat için kullanılamaz hale gelirse, varsayılan yeniden deneme ilkesi bu durumda çalışmaz, çünkü olur, iş yeniden çalışması için başka bir hafta beklemek istemeyebilirsiniz. Bu nedenle, standart yeniden deneme ilkesini değiştirmek isteyebilirsiniz, böylece yeniden denemeler her 30 saniyede bir değil, her üç saatte bir gerçekleşir. 

Yeniden deneme ilkesini nasıl ayarlayatılabildiğini öğrenmek için [retryPolicy'ye](scheduler-concepts-terms.md#retrypolicy)bakın.

### <a name="alternate-endpoints"></a>Alternatif uç noktalar

Azure Zamanlayıcısı işiniz, yeniden deneme ilkesini izleyerek bile erişilemeyen bir bitiş noktası çağırırsa, Zamanlayıcı bu tür hataları işleyebilecek alternatif bir bitiş noktasına geri döner. Bu nedenle, bu bitiş noktasını ayarlarsanız, Zamanlayıcı bu bitiş noktasını çağırır ve bu da hatalar olduğunda kendi işlerinizi yüksek oranda kullanılabilir hale getirir.

Örneğin, bu diyagram, Zamanlayıcı'nın New York'taki bir web hizmetini ararken yeniden deneme ilkesini nasıl izlediğini gösterir. Yeniden denemeler başarısız olursa, Zamanlayıcı alternatif bir bitiş noktası için denetler. Bitiş noktası varsa, Zamanlayıcı istekleri alternatif bitiş noktasına göndermeye başlar. Aynı yeniden deneme ilkesi hem özgün eylem hem de alternatif eylem için geçerlidir.

![Yeniden deneme ilkesi ve alternatif bitiş noktası ile zamanlayıcı davranışı](./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png)

Alternatif eylem için eylem türü özgün eylemden farklı olabilir. Örneğin, özgün eylem bir HTTP bitiş noktası çağırsa da, alternatif eylem bir Depolama sırası, Hizmet Veri Servisi sırası veya Hizmet Veri Servisi konu eylemi kullanarak hataları günlüğe kaydedebilir.

Alternatif bir bitiş noktası nın nasıl ayarlan yapılacağını öğrenmek için [hata Eylemi'ne](scheduler-concepts-terms.md#error-action)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Kavramlar, terminoloji ve varlık hiyerarşisi](scheduler-concepts-terms.md)
* [Azure Scheduler REST API başvurusu](/rest/api/scheduler)
* [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)
* [Sınırlar, kotalar, varsayılan değerler ve hata kodları](scheduler-limits-defaults-errors.md)
