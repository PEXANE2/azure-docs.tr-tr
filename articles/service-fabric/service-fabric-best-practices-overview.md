---
title: Azure Service Fabric uygulama ve küme en iyi uygulamaları
description: Azure Service Fabric kullanarak kümeleri, uygulamaları ve Hizmetleri yönetmeye yönelik en iyi yöntemler ve tasarım konuları.
author: peterpogorski
ms.topic: conceptual
ms.date: 06/18/2019
ms.author: pepogors
ms.openlocfilehash: 46601fd91fccb5bd0866bd999cc7643c37c6d582
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75551786"
---
# <a name="azure-service-fabric-application-and-cluster-best-practices"></a>Azure Service Fabric uygulama ve küme en iyi uygulamaları

Bu makalede, Azure Service Fabric uygulamalarını ve kümelerini yönetmeye yönelik en iyi yöntemlere bağlantılar sağlanmaktadır. Üretim ortamınızın güvenilirliğini iyileştirmek için bu uygulamaları uygulamanız önemle önerilir. Üretim çözümünüzü tasarlamaya başlamak için [Service Fabric kümesi şablonlarından](https://github.com/Azure-Samples/service-fabric-cluster-templates) birini kullanın veya var olan şablonunuzu bu uygulamaları içerecek şekilde güncelleştirin.

## <a name="security"></a>Güvenlik

* [Güvenlik için en iyi uygulamalar](service-fabric-best-practices-security.md)

## <a name="networking"></a>Ağ

* [Ağ için en iyi yöntemler](service-fabric-best-practices-networking.md)

## <a name="compute-planning-and-scaling"></a>İşlem planlama ve ölçeklendirme

* [İşlem ölçekleme için en iyi uygulamalar](service-fabric-best-practices-capacity-scaling.md)
* [İşlem kapasitesi planlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity)

## <a name="infrastructure-as-code"></a>Kod olarak altyapı

* [Kod olarak altyapı uygulamak için en iyi yöntemler](service-fabric-best-practices-infrastructure-as-code.md)

## <a name="monitoring-and-diagnostics"></a>İzleme ve tanılama

* [Küme izleme ve Tanılama için en iyi uygulamalar](service-fabric-best-practices-monitoring.md)

## <a name="application-design"></a>Uygulama tasarımı

* [Uygulama tasarımı için en iyi uygulamalar](service-fabric-best-practices-applications.md)

## <a name="checklist"></a>Denetim Listesi

Önceki bölümlerde önerilen uygulamaları uyguladıktan sonra, üretim hazırlığı denetim listesindeki en iyi uygulamaları tümleştirdiğinizden emin olun:
* [Azure Service Fabric üretim hazırlığı denetim listesi](https://docs.microsoft.com/azure/service-fabric/service-fabric-production-readiness-checklist)

## <a name="next-steps"></a>Sonraki adımlar

* VM 'lerde veya Windows Server çalıştıran bilgisayarlarda küme oluşturma: [Windows Server için Service Fabric kümesi oluşturma](service-fabric-cluster-creation-for-windows-server.md)
* VM 'lerde veya Linux çalıştıran bilgisayarlarda küme oluşturma: [Linux kümesi oluşturma](service-fabric-cluster-creation-via-portal.md)
* Sorun giderme Service Fabric: [sorun giderme kılavuzu](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides)