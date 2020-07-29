---
title: Azure Izleyici 'de Öngörüler 'e genel bakış | Microsoft Docs
description: Öngörüler, belirli uygulamalar ve hizmetler için Azure Izleyici 'de özelleştirilmiş bir izleme deneyimi sağlar. Bu makalede, şu anda kullanılabilir olan her bir öngörüden kısa bir açıklama sunulmaktadır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/22/2019
ms.openlocfilehash: e3eefb97c85b3865abbf116b9912dc45f6db8ce8
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87326877"
---
# <a name="overview-of-insights-in-azure-monitor"></a>Azure Izleyici 'de Öngörüler 'e genel bakış
Öngörüler, belirli uygulamalar ve hizmetler için özelleştirilmiş bir izleme deneyimi sağlar. Bunlar, verileri [Azure izleyici veri platformunda](../platform/data-platform.md) depolar ve analiz ve uyarı Için diğer Azure izleyici özelliklerinden yararlanır, ancak Azure Portal ek veri toplayabilir ve benzersiz bir kullanıcı deneyimi sağlayabilir. Azure portal Azure Izleyici menüsünün **Öngörüler** bölümünden Öngörüler 'e erişin.


Aşağıdaki bölümlerde, Azure Izleyici 'de Şu anda kullanılabilir olan Öngörüler hakkında kısa bir açıklama sağlanmaktadır. Ayrıntılar için ayrıntılı belgelere bakın.

## <a name="application-insights"></a>Application Insights
Application Insights, farklı platformlardaki web geliştiricilerine yönelik kapsamlı bir Uygulama Performans Yönetimi (APM) hizmetidir. Canlı web uygulamanızı izlemek için kullanabilirsiniz. .NET, Node.js ve Java EE, şirket içi, hibrit veya herhangi bir genel bulut dahil olmak üzere çok çeşitli platformlarda uygulamalar için geçerlidir. Ayrıca, DevOps süreciyle tümleştirilir ve çeşitli geliştirme araçlarına bağlantı noktaları vardır.

Bkz. [Application Insights nedir?](../app/app-insights-overview.md).

![Application Insights](media/insights-overview/app-insights.png)

## <a name="azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici
Kapsayıcılar için Azure Izleyici, Azure Kubernetes Service (AKS) üzerinde barındırılan Azure Container Instances veya yönetilen Kubernetes kümelerine dağıtılan kapsayıcı iş yüklerinin performansını izler. Kapsayıcılarınızın izlenmesi, özellikle de bir üretim kümesini birden çok uygulamayla birlikte çalışırken kritik öneme sahiptir.

Bkz. [kapsayıcılara yönelik Azure izleyici 'ye genel bakış](./container-insights-overview.md).

![Kapsayıcılar için Azure İzleyici](media/insights-overview/container-insights.png)

## <a name="azure-monitor-for-resource-groups-preview"></a>Kaynak grupları için Azure Izleyici (Önizleme)
Kaynak grupları için Azure Izleyici, tek tek kaynaklarınızın karşılaştığı tüm sorunları önceliklendirmenize ve tanılamanıza yardımcı olur ve kaynak grubunun bir bütün olarak sistem durumu ve performansına göre bağlamı sunar.

Bkz. [Azure izleyici (Önizleme) ile kaynak gruplarını izleme](./resource-group-insights.md).

![Kaynak grupları için Azure Izleyici](media/insights-overview/resource-group-insights.png)

## <a name="azure-monitor-for-vms-preview"></a>VM'ler için Azure İzleyici (Önizleme)
VM'ler için Azure İzleyici, Azure sanal makinelerinizi (VM) ve büyük ölçekli sanal makine ölçek kümelerinizi izler. İşlemlerini ve diğer kaynaklarla dış işlemlere olan bağımlılıklarını izleyerek Windows ve Linux VM'lerinizin performansını ve sistem durumunu analiz eder.

Bkz. [VM'ler için Azure izleyici nedir?](vminsights-overview.md)

![VM'ler için Azure İzleyici](media/insights-overview/vm-insights.png)

## <a name="azure-monitor-for-networks-preview"></a>Ağlar için Azure Izleyici (Önizleme)
[Ağlar Için Azure izleyici](network-insights-overview.md) , tüm ağ kaynağınızın sistem durumu ve ölçümlerinin kapsamlı bir görünümünü sağlar. Gelişmiş arama özelliği, yalnızca Web sitenizin adını arayarak Web sitenizi barındıran kaynağı tanımlama gibi senaryoları etkinleştirerek Kaynak bağımlılıklarını belirlemenize yardımcı olur.

![Ağlar için Azure İzleyici](media/insights-overview/network-insights.png)

## <a name="next-steps"></a>Sonraki adımlar
* Öngörüler tarafından [Azure izleyici veri platformu](../platform/data-platform.md) yararlanılabilir hakkında daha fazla bilgi edinin.
* [Azure izleyici tarafından kullanılan farklı veri kaynakları](../platform/data-sources.md) ve her bir öngörüde toplanan farklı veri türleri hakkında bilgi edinin.

