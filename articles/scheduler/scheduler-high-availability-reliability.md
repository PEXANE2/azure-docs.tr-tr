---
title: Yüksek kullanılabilirlik ve güvenilirlik
description: Azure Scheduler 'da yüksek kullanılabilirlik ve güvenilirlik hakkında bilgi edinin
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: article
ms.date: 08/16/2016
ms.openlocfilehash: 20c2054e168a9b17d9b4ab159cfefbf607ab6d11
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78898555"
---
# <a name="high-availability-and-reliability-for-azure-scheduler"></a>Azure Scheduler için yüksek kullanılabilirlik ve güvenilirlik

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) , [devre dışı bırakılmakta](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date)olan Azure Scheduler 'ı değiştiriyor. Zamanlayıcı 'da ayarladığınız işlerle çalışmaya devam etmek için lütfen en kısa sürede [Azure Logic Apps geçirin](../scheduler/migrate-from-scheduler-to-logic-apps.md) . 
>
> Zamanlayıcı artık Azure portal kullanılamıyor, ancak iş ve iş koleksiyonlarınızı yönetebilmeniz için [REST API](/rest/api/scheduler) ve [Azure Scheduler PowerShell cmdlet 'leri](scheduler-powershell-reference.md) Şu anda kullanılabilir durumda kalır.

Azure Zamanlayıcı, işleriniz için hem [yüksek kullanılabilirlik](https://docs.microsoft.com/azure/architecture/framework/#resiliency) hem de güvenilirlik sağlar. Daha fazla bilgi için bkz. [Scheduler Için SLA](https://azure.microsoft.com/support/legal/sla/scheduler).

## <a name="high-availability"></a>Yüksek kullanılabilirlik

Azure Scheduler, [yüksek oranda kullanılabilir] ve coğrafi olarak yedekli hizmet dağıtımı ve coğrafi bölgesel iş çoğaltmasını kullanır.

### <a name="geo-redundant-service-deployment"></a>Coğrafi olarak yedekli hizmet dağıtımı

Azure Scheduler, [bugün Azure tarafından desteklenen neredeyse her coğrafi bölgede](https://azure.microsoft.com/global-infrastructure/regions/#services)kullanılabilir. Bu nedenle, barındırılan bir bölgedeki bir Azure veri merkezi kullanılamaz hale gelirse, hizmetin yük devretme özellikleri Zamanlayıcı 'Yı başka bir veri merkezinde kullanıma sunmak için Azure Zamanlayıcı 'yı kullanmaya devam edebilirsiniz.

### <a name="geo-regional-job-replication"></a>Coğrafi bölgesel iş çoğaltması

Azure Scheduler 'da kendi işleriniz Azure bölgeleri arasında çoğaltılır. Bu nedenle, bir bölgede kesinti varsa, Azure Scheduler üzerinde devreder ve işinizin eşleştirilmiş coğrafi bölgedeki başka bir veri merkezinde çalıştığından emin olur.

Örneğin, Orta Güney ABD bir iş oluşturursanız, Azure Scheduler bu işi Orta Kuzey ABD otomatik olarak çoğaltır. Orta Güney ABD bir hata oluşursa, Azure Scheduler işi Orta Kuzey ABD çalıştırır. 

![Coğrafi bölgesel iş çoğaltması](./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png)

Azure Zamanlayıcı Ayrıca, Azure 'da bir hata olması durumunda verilerinizin aynı ancak daha geniş coğrafi bölge içinde kalmasını de sağlar. Bu nedenle, yalnızca yüksek kullanılabilirlik istediğinizde işlerinizi yinelememeniz gerekmez. Azure Scheduler, işleriniz için otomatik olarak yüksek kullanılabilirlik sağlar.

## <a name="reliability"></a>Güvenilirlik

Azure Scheduler kendi yüksek kullanılabilirlik garantisi sağlar, ancak kullanıcı tarafından oluşturulan işlere farklı bir yaklaşım alır. Örneğin, işinizin kullanılamayan bir HTTP uç noktasını çağırdığını varsayın. Azure Zamanlayıcı yine de başarısız olan sorunları işlemek için alternatif yollar sunarak işinizi başarıyla çalıştırmaya çalışır: 

* Yeniden deneme ilkeleri ayarlayın.
* Alternatif uç noktaları ayarlayın.

<a name="retry-policies"></a>

### <a name="retry-policies"></a>Yeniden deneme ilkeleri

Azure Zamanlayıcı, yeniden deneme ilkeleri ayarlamanıza olanak sağlar. Bir iş başarısız olursa, Zamanlayıcı varsayılan olarak 30 saniyelik aralıklarda işi dört kez yeniden dener. Bu yeniden deneme ilkesini 30 saniyelik aralıklarla 10 kez veya günlük aralıklarda iki kez daha az agresif hale getirebilirsiniz.

Örneğin, bir HTTP uç noktası çağıran haftalık bir iş oluşturduğunuzu varsayalım. İşiniz çalıştırıldığında HTTP uç noktası birkaç saat boyunca kullanılamaz hale gelirse, işin yeniden çalışması için başka bir hafta beklemek istemeyebilirsiniz, bu durum varsayılan yeniden deneme ilkesi bu durumda çalışmayacaktır. Bu nedenle, yeniden denemeler olması için standart yeniden deneme ilkesini değiştirmek isteyebilirsiniz, örneğin her 30 saniye yerine her üç saatte bir. 

Yeniden deneme ilkesi ayarlamayı öğrenmek için bkz. [retrypolicy](scheduler-concepts-terms.md#retrypolicy).

### <a name="alternate-endpoints"></a>Alternatif uç noktalar

Azure Scheduler işiniz erişilemeyen bir uç nokta çağırırsa, yeniden deneme ilkesi yapıldıktan sonra bile Zamanlayıcı, bu hataları işleyebilen alternatif bir uç noktaya geri döner. Bu nedenle, bu uç noktayı ayarlarsanız Zamanlayıcı bu uç noktayı çağırır ve bu da, bir başarısızlık durumunda kendi işlerinizi yüksek oranda kullanılabilir hale getirir.

Örneğin, bu diyagramda, New York 'da bir Web hizmetini çağırırken Scheduler 'ın yeniden deneme ilkesini nasıl izlediği gösterilmektedir. Yeniden denemeler başarısız olursa Zamanlayıcı, alternatif bir uç nokta olup olmadığını denetler. Uç nokta varsa Zamanlayıcı, istekleri alternatif uç noktaya göndermeye başlar. Aynı yeniden deneme ilkesi, hem özgün eylem hem de alternatif eylem için geçerlidir.

![Yeniden deneme ilkesiyle Zamanlayıcı davranışı ve alternatif uç nokta](./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png)

Alternatif eylem için eylem türü, özgün eylemden farklı olabilir. Örneğin, özgün eylem bir HTTP uç noktası çağırıyor olsa da, diğer eylem bir depolama kuyruğu, Service Bus kuyruğu veya Service Bus konu eylemi kullanarak hataları günlüğe alabilir.

Alternatif bir uç nokta ayarlamayı öğrenmek için bkz. [ErrorAction](scheduler-concepts-terms.md#error-action).

## <a name="next-steps"></a>Sonraki adımlar

* [Kavramlar, terminoloji ve varlık hiyerarşisi](scheduler-concepts-terms.md)
* [Azure Scheduler REST API başvurusu](/rest/api/scheduler)
* [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)
* [Sınırlar, kotalar, varsayılan değerler ve hata kodları](scheduler-limits-defaults-errors.md)
