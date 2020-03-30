---
title: Azure Hizmet Kumaş uygulaması ve küme en iyi uygulamaları
description: Azure Hizmet Kumaşı'nı kullanarak kümeleri, uygulamaları ve hizmetleri yönetmek için en iyi uygulamalar ve tasarım konuları.
author: peterpogorski
ms.topic: conceptual
ms.date: 06/18/2019
ms.author: pepogors
ms.openlocfilehash: 46601fd91fccb5bd0866bd999cc7643c37c6d582
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75551786"
---
# <a name="azure-service-fabric-application-and-cluster-best-practices"></a>Azure Hizmet Kumaş uygulaması ve küme en iyi uygulamaları

Bu makalede, Azure Hizmet Kumaşı uygulamalarını ve kümelerini yönetmek için en iyi uygulamalara bağlantılar sağlanmaktadır. Üretim ortamınızın güvenilirliğini optimize etmek için bu uygulamaları uygulamanızı şiddetle öneririz. Üretim çözümünüzü tasarlamaya başlamak için [Service Fabric küme şablonlarından](https://github.com/Azure-Samples/service-fabric-cluster-templates) birini kullanın veya bu uygulamaları birleştirmek için varolan şablonunuzu güncelleştirin.

## <a name="security"></a>Güvenlik

* [Güvenlik için en iyi uygulamalar](service-fabric-best-practices-security.md)

## <a name="networking"></a>Ağ Oluşturma

* [Ağ için en iyi uygulamalar](service-fabric-best-practices-networking.md)

## <a name="compute-planning-and-scaling"></a>İşlem planlama ve ölçeklendirme

* [Hesaplama ölçekleme için en iyi uygulamalar](service-fabric-best-practices-capacity-scaling.md)
* [İşlem kapasitesi planlaması](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity)

## <a name="infrastructure-as-code"></a>Kod olarak altyapı

* [Altyapıyı kod olarak uygulamak için en iyi uygulamalar](service-fabric-best-practices-infrastructure-as-code.md)

## <a name="monitoring-and-diagnostics"></a>İzleme ve tanılama

* [Küme izleme ve tanılama için en iyi uygulamalar](service-fabric-best-practices-monitoring.md)

## <a name="application-design"></a>Uygulama tasarımı

* [Uygulama tasarımı için en iyi uygulamalar](service-fabric-best-practices-applications.md)

## <a name="checklist"></a>Denetim Listesi

Önceki bölümlerde önerilen uygulamaları uyguladıktan sonra, üretime hazırlık denetim listesindeki en iyi uygulamaları entegre ettiğinizden emin olun:
* [Azure Hizmet Kumaş ı üretime hazırlık kontrol listesi](https://docs.microsoft.com/azure/service-fabric/service-fabric-production-readiness-checklist)

## <a name="next-steps"></a>Sonraki adımlar

* Windows Server çalıştıran VM'lerde veya bilgisayarlarda küme [oluşturma: Windows Server için Hizmet Kumaşı küme oluşturma](service-fabric-cluster-creation-for-windows-server.md)
* Linux çalıştıran VM'lerde veya bilgisayarlarda küme oluşturma: [Linux kümesi oluşturma](service-fabric-cluster-creation-via-portal.md)
* Sorun Giderme Hizmet Kumaşı: [Sorun giderme kılavuzları](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides)