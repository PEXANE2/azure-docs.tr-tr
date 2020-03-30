---
title: Linux Azure Tanılama ile Olay Toplama
description: Azure Hizmet Kumaşı kümelerinin izlenmesi ve teşhisi için LAD'yi kullanarak olayları toplama ve toplama hakkında bilgi edinin.
author: srrengar
ms.topic: conceptual
ms.date: 2/25/2019
ms.author: srrengar
ms.openlocfilehash: fdb78498d33416ef21b2e2b0f498e7afa6a58d99
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75609970"
---
# <a name="event-aggregation-and-collection-using-linux-azure-diagnostics"></a>Linux Azure Tanılama'yı kullanarak olay toplama ve toplama
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Bir Azure Hizmet Kumaşı kümesini çalıştırırken, tüm düğümlerin günlüklerini merkezi bir konumda toplamak iyi bir fikirdir. Günlüklerin merkezi bir konumda olması, kümenizdeki sorunları veya bu kümede çalışan uygulama ve hizmetlerdeki sorunları çözümlemenize ve sorun gidermenize yardımcı olur.

Günlükleri yüklemenin ve toplamanın bir yolu, günlükleri Azure Depolama'ya yükleyen ve ayrıca günlükleri Azure Uygulama Bilgileri'ne veya Etkinlik Hub'larına gönderme seçeneğine sahip olan Linux Azure Tanılama (LAD) uzantısını kullanmaktır. Ayrıca, olayları depolama alanından okumak ve [Azure Monitor günlükleri](../log-analytics/log-analytics-service-fabric.md) veya başka bir günlük ayrıştma çözümü gibi bir analiz platformu ürününe yerleştirmek için harici bir işlem de kullanabilirsiniz.

## <a name="log-and-event-sources"></a>Günlük ve etkinlik kaynakları

### <a name="service-fabric-platform-events"></a>Servis Kumaş platformu etkinlikleri
Service Fabric, operasyonel olaylar veya çalışma zamanı etkinlikleri de dahil olmak üzere [LTTng](https://lttng.org)aracılığıyla birkaç out-of-the-box günlükleri yayar. Bu günlükler kümenin Kaynak Yöneticisi şablonunda belirttiği konumda depolanır. Depolama hesabı ayrıntılarını almak veya ayarlamak için **AzureTableWinWinFabETWQueryable** etiketini arayın ve **StoreConnectionString'i**arayın.

### <a name="application-events"></a>Uygulama etkinlikleri
 Yazılımınızı enstrümantarken sizin belirlediğiniz uygulama ve hizmetlerin kodundan yayılan olaylar. Metin tabanlı günlük dosyaları yazan herhangi bir günlük çözümlerini kullanabilirsiniz(örneğin, LTTng). Daha fazla bilgi için başvurunuzun izini sürmeyle ilgili LTTng belgelerine bakın.

[Yerel bir makine geliştirme kurulumundaki hizmetleri izleyin ve tanıkoyun.](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

## <a name="deploy-the-diagnostics-extension"></a>Tanılama uzantısını dağıtma
Günlükleri toplamanın ilk adımı, Hizmet Kumaşı kümesindeki Her VM'nin tanılama uzantısını dağıtmaktır. Tanılama uzantısı, her VM'deki günlükleri toplar ve bunları belirttiğiniz depolama hesabına yükler. 

Tanılama uzantısını küme oluşturmanın bir parçası olarak kümedeki VM'lere dağıtmak için **Tanılama'yı** **Açık**olarak ayarlayın. Kümeyi oluşturduktan sonra, portalı kullanarak bu ayarı değiştiremezsiniz, bu nedenle Kaynak Yöneticisi şablonunda uygun değişiklikleri yapmak zorunda kaldığınız için.

Bu, lad aracısını belirtilen günlük dosyalarını izlemek için yapılandırır. Dosyaya yeni bir satır eklendiğinde, belirttiğiniz depolama alanına (tabloya) gönderilen bir syslog girişi oluşturur.


## <a name="next-steps"></a>Sonraki adımlar

1. Sorun giderme sorunları sırasında hangi olayları incelemeniz gerektiğini daha ayrıntılı olarak anlamak için [LTTng belgelerine](https://lttng.org/docs) ve [LAD'yi kullanmaya](https://docs.microsoft.com/azure/virtual-machines/extensions/diagnostics-linux)bakın.
2. Ölçümleri toplamaya yardımcı olacak, kümenizde dağıtılan Kapsayıcıları izlemek ve günlüklerinizi görselleştirmek için [Log Analytics aracısını ayarlayın](service-fabric-diagnostics-event-analysis-oms.md) 
