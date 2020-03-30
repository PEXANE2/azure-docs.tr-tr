---
title: Azure Monitöründe Öngörülere Genel Bakış | Microsoft Dokümanlar
description: Öngörüler, azure monitörde belirli uygulamalar ve hizmetler için özelleştirilmiş bir izleme deneyimi sağlar. Bu makalede, şu anda kullanılabilir olan öngörülerin her birinin kısa bir açıklamasını sağlar.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/22/2019
ms.openlocfilehash: 15ea7698c9e90fa8b0dfa20f71b552a2b0e9c7d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77657257"
---
# <a name="overview-of-insights-in-azure-monitor"></a>Azure Monitöründe Öngörülere Genel Bakış
Öngörüler, belirli uygulamalar ve hizmetler için özelleştirilmiş bir izleme deneyimi sağlar. Verileri [Azure Monitor veri platformunda](../platform/data-platform.md) depolar lar ve analiz ve uyarı için diğer Azure Monitor özelliklerinden yararlanırlar, ancak ek veri toplayabilir ve Azure portalında benzersiz bir kullanıcı deneyimi sağlayabilirler. Azure portalındaki Azure Monitörü menüsünün **Öngörüler** bölümünden öngörülere erişin.

Aşağıdaki bölümler, Şu anda Azure Monitor'da kullanılabilen öngörülerin kısa bir açıklamasını sağlar. Her biri hakkında ayrıntılı bilgi için ayrıntılı belgelere bakın.

## <a name="application-insights"></a>Application Insights
Application Insights, farklı platformlardaki web geliştiricilerine yönelik kapsamlı bir Uygulama Performans Yönetimi (APM) hizmetidir. Canlı web uygulamanızı izlemek için kullanabilirsiniz. .NET, Node.js ve Java EE dahil olmak üzere çok çeşitli platformlarda, şirket içi, karma veya herhangi bir genel bulut barındırılan uygulamalar için çalışır. Ayrıca DevOps işleminizle entegre olur ve çeşitli geliştirme araçlarına bağlantı noktaları na sahiptir.

Bkz. [Uygulama Öngörüleri Nedir?](../app/app-insights-overview.md).

![Application Insights](media/insights-overview/app-insights.png)

## <a name="azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici
Kapsayıcılar için Azure Monitörü, Azure Kapsayıcı Örnekleri'ne veya Azure Kubernetes Hizmetinde (AKS) barındırılan yönetilen Kubernetes kümelerine dağıtılan kapsayıcı iş yüklerinin performansını izler. Kapsayıcılarınızı izlemek, özellikle birden çok uygulamayla ölçekte bir üretim kümesi çalıştırıyorsanız çok önemlidir.

[Kapsayıcılara genel bakış için Azure Monitörü'ne](../insights/container-insights-overview.md)bakın.

![Kapsayıcılar için Azure İzleyici](media/insights-overview/container-insights.png)

## <a name="azure-monitor-for-resource-groups-preview"></a>Kaynak Grupları için Azure Monitörü (önizleme)
Kaynak grupları için Azure Monitor, tek tek kaynaklarınızın karşılaştığı sorunları üçlemenize ve tanılamaya yardımcı olurken, kaynak grubunun bir bütün olarak sistem durumu ve performansı yla ilgili bağlam sunar.

Bkz. [Azure Monitor ile kaynak gruplarını izleyin (önizleme)](../insights/resource-group-insights.md).

![Kaynak Grupları için Azure Monitörü](media/insights-overview/resource-group-insights.png)

## <a name="azure-monitor-for-vms-preview"></a>VM'ler için Azure Monitör (önizleme)
VM'ler için Azure Monitor, Azure sanal makinelerinizi (VM) ve sanal makine ölçek kümelerini ölçekte izler. İşlemlerini ve diğer kaynaklarla dış işlemlere olan bağımlılıklarını izleyerek Windows ve Linux VM'lerinizin performansını ve sistem durumunu analiz eder.

[VM'ler için Azure Monitör nedir?](vminsights-overview.md)

![VM'ler için Azure İzleyici](media/insights-overview/vm-insights.png)

## <a name="azure-monitor-for-networks-preview"></a>Ağlar için Azure Monitörü (önizleme)
[Ağlar için Azure Monitor,](network-insights-overview.md) tüm ağ kaynağınız için sistem durumu ve ölçümlerhakkında kapsamlı bir görünüm sağlar. Gelişmiş arama özelliği, yalnızca web sitenizin adını arayarak web sitenizi barındıran kaynağı tanımlama gibi senaryoları etkinleştirerek kaynak bağımlılıklarını belirlemenize yardımcı olur.

![Ağlar için Azure İzleyici](media/insights-overview/network-insights.png)

## <a name="next-steps"></a>Sonraki adımlar
* Öngörüler tarafından yararlanılanan [Azure Monitor veri platformu](../platform/data-platform.md) hakkında daha fazla bilgi edinin.
* Azure Monitor [tarafından kullanılan](../platform/data-sources.md) farklı veri kaynakları ve her bir öngörü tarafından toplanan farklı veri türleri hakkında bilgi edinin.
