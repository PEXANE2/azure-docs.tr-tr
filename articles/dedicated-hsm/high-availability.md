---
title: Yüksek kullanılabilirlik-Azure ayrılmış HSM | Microsoft Docs
description: Azure adanmış HSM yüksek kullanılabilirlik örneği ve temel konular
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: mbaldwin
ms.openlocfilehash: 536ef62acad900090924598edfa45450b2a8c951
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "70882256"
---
# <a name="azure-dedicated-hsm-high-availability"></a>Azure adanmış HSM yüksek kullanılabilirlik

Azure adanmış HSM, Microsoft 'un yüksek oranda kullanılabilir veri merkezleri tarafından sabitlenmiştir. Ancak, yüksek oranda kullanılabilir veri merkezi, yerelleştirilmiş arızalara ve çok büyük koşullarda bölgesel düzey hatalara karşı savunmasız kalır. Microsoft, birden çok cihazın tek bir rafı paylaşan cihazlara neden olmamasını sağlamak için bir bölgedeki farklı veri merkezlerinde HSM cihazları dağıtır. Bu HSM 'ler, Gemalto HA Group özelliğini kullanarak bir bölgedeki veri merkezlerinde bu HSM 'leri eşleştirerek daha fazla yüksek kullanılabilirlik elde edilebilir. Ayrıca, bir olağanüstü durum kurtarma durumunda bölgesel yük devretmeyi ele almak için cihazları bölgeler arasında eşleştirmek mümkündür. Bu çok katmanlı yüksek kullanılabilirlik yapılandırmasıyla, uygulamaların çalışmasını sağlamak için herhangi bir cihaz hatası otomatik olarak karşılanır. Tüm veri merkezleri aynı zamanda site üzerinde yedek cihazlara ve bileşenlere sahiptir, bu nedenle başarısız olan tüm cihazlar zamanında değiştirilebilir.

## <a name="high-availability-example"></a>Yüksek kullanılabilirlik örneği

Yazılım düzeyinde yüksek kullanılabilirlik için HSM cihazlarının nasıl yapılandırılacağı hakkında bilgiler, ' Gemalto Luna Network HSM Yönetim Kılavuzu ' nda bulunur. Bu belge, [Gemalto HSM sayfasında](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/)bulunur.

Aşağıdaki diyagramda, yüksek oranda kullanılabilir bir mimari gösterilmektedir. Bölgede birden çok cihazı ve ayrı bir bölgede eşleştirilmiş birden çok cihazı kullanır. Bu mimari en az dört HSM cihaz ve sanal ağ bileşeni kullanır.

![Yüksek kullanılabilirlik diyagramı](media/high-availability/high-availability.png)

## <a name="next-steps"></a>Sonraki adımlar

Yüksek kullanılabilirlik ve güvenlik gibi hizmetin temel kavramlarının cihaz sağlama ve uygulama tasarımı ya da dağıtımdan önce iyi anlaşıldığından emin olmanız önerilir.
Daha fazla kavram düzeyi konuları:

* [Dağıtım mimarisi](deployment-architecture.md)
* [Fiziksel güvenlik](physical-security.md)
* [Ağ](networking.md)
* [Desteklenebilirlik](supportability.md)
* [İzleme](monitoring.md)

Yüksek kullanılabilirlik için HSM cihazlarını yapılandırma hakkında ayrıntılı bilgi için, lütfen yönetici kılavuzlarıyla ilgili olarak Gemalto müşteri destek portalına başvurun ve Bölüm 6 ' yı inceleyin.
