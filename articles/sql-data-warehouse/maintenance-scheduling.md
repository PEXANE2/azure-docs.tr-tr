---
title: Azure bakım zamanlamaları (Önizleme) | Microsoft Docs
description: Bakım zamanlaması, müşterilerin Azure SQL veri ambarı hizmeti 'nin yeni özellikleri, yükseltmeleri ve düzeltme eklerini almak için kullandığı gerekli zamanlanmış bakım olaylarını planlayabilmesini sağlar.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: design
ms.date: 07/16/2019
ms.author: anvang
ms.reviewer: jrasnick
ms.openlocfilehash: 3875106e8c6301c95bc8d0fbce6a1c0400d07f78
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68278119"
---
# <a name="use-maintenance-schedules-to-manage-service-updates-and-maintenance"></a>Bakım zamanlamalarını kullanarak hizmet güncelleştirmelerini ve bakımını yönetme

Bakım zamanlamaları artık tüm Azure SQL veri ambarı bölgelerinde kullanılabilir. Bu özellik, hizmet durumu planlı bakım bildirimlerini, Kaynak Durumu denetim Izleyicisini ve Azure SQL veri ambarı bakım zamanlaması hizmetini tümleştirir.

Yeni özellikler, yükseltmeler ve düzeltme ekleri almak için uygun olduğunda bir zaman penceresi seçmek için bakım zamanlamasını kullanırsınız. Birincil ve ikincil bakım penceresini yedi günlük bir süre içinde seçersiniz. Bir örnek, Cumartesi 22:00 ' nin Pazar 01:00 ' nin birincil bir penceresidir ve Çarşamba 19:00 ' nin ikinci bir penceresinde 22:00. Birincil bakım pencereniz sırasında SQL Data Warehouse bakım gerçekleştiremediği takdirde, bakım, ikincil bakım pencereniz sırasında yeniden denenecek. Hizmet bakımı hem birincil hem de Ikincil pencereler sırasında gerçekleşebilir. Tüm bakım işlemlerinin hızlı bir şekilde tamamlanmasını sağlamak için, DW400 (c) ve daha düşük veri ambarı katmanları, belirli bir bakım penceresi dışında bakım işlemini tamamlayabilir.

Yeni oluşturulan tüm Azure SQL veri ambarı örneklerinin, dağıtım sırasında uygulanan sistem tanımlı bir bakım zamanlaması olacaktır. Zamanlama, dağıtım tamamlandıktan hemen sonra düzenlenebilir.

Her bakım penceresi üç ile sekiz saat arasında olabilir. Bakım, pencerenin içinde herhangi bir zamanda gerçekleşebilir. Bakım başladığında, tüm etkin oturumlar iptal edilir ve teslim edilmemiş işlemler geri alınacaktır. Hizmet, veri ambarınıza yeni kod dağıttığı için bağlantıda birden çok kısa kayıp beklemeniz gerekir. Veri Ambarınızda bakım tamamlandıktan hemen sonra bilgilendirilirsiniz

Bu özelliği kullanmak için, ayrı gün aralıklarında bir birincil ve ikincil pencere belirlemeniz gerekir. Tüm bakım işlemleri, zamanlanmış bakım pencereleri içinde bitmelidir. Önceki bildirim olmadan belirtilen bakım pencerelerinin dışında hiçbir bakım gerçekleşmeyecektir. Zamanlanan Bakım sırasında veri ambarınız duraklatıldıysa, sürdürme işlemi sırasında güncelleştirilir.  

## <a name="alerts-and-monitoring"></a>Uyarılar ve izleme

Hizmet durumu bildirimleri ve Kaynak Durumu Check Monitor ile tümleştirme, müşterilerin yaklaşan bakım etkinliklerinden haberdar olmasına olanak sağlar. Yeni otomasyon, Azure Izleyici 'den yararlanır. Yaklaşan bakım olayları hakkında nasıl bilgilendirilmek istediğinize karar verebilirsiniz. Ayrıca, hangi otomatikleştirilmiş akışların kapalı kalma süresini yönetmenize ve işlemlerinizin etkisini en aza indirmenize yardımcı olmaya karar verin

24 saatlik bir ön bildirim, DW400c ve alt katmanların geçerli özel durumu ile tüm bakım olaylarının önüne gelir. Örnek kapalı kalma süresini en aza indirmek için, veri ambarınızın seçtiğiniz bakım süresinden önce uzun süre çalışan işlemlere sahip olmadığından emin olun.

> [!NOTE]
> Zaman açısından kritik güncelleştirme dağıtımı için gerekli olan olayda gelişmiş bildirim süreleri önemli ölçüde azaltılabilir.

Bakımın gerçekleştireceğiz, ancak SQL veri ambarı bu süre içinde bakım gerçekleştiremediği halde bir iptal bildirimi alacaksınız. Bakım daha sonra zamanlanan bir sonraki bakım döneminde sürdürülecek.

Tüm etkin bakım olayları, **hizmet durumu-planlı bakım** bölümünde görünür. Hizmet durumu geçmişi geçmiş olayların tam kaydını içerir. Etkin bir olay sırasında Azure hizmet durumu denetimi Portal panosu aracılığıyla Bakımı izleyebilirsiniz.

### <a name="maintenance-schedule-availability"></a>Bakım zamanlaması kullanılabilirliği

Bakım zamanlaması seçtiğiniz bölgede mevcut olmasa bile, bakım zamanlamanızı dilediğiniz zaman görüntüleyebilir ve düzenleyebilirsiniz. Bölgede bakım zamanlaması kullanılabilir hale geldiğinde, belirlenen zamanlama hemen veri Ambarınızda etkin olur.

## <a name="next-steps"></a>Sonraki adımlar

- Bakım zamanlamasını görüntüleme hakkında [daha fazla bilgi edinin](viewing-maintenance-schedule.md) .
- Bakım zamanlamasını değiştirme hakkında [daha fazla bilgi edinin](changing-maintenance-schedule.md) .
- Azure Izleyici 'yi kullanarak uyarı oluşturma, görüntüleme ve yönetme hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) .
- Günlük uyarı kuralları için Web kancası eylemleri hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) .
- [Daha fazla bilgi](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) Eylem grupları oluşturma ve yönetme.
- Azure hizmet durumu hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-health/service-health-overview) .
