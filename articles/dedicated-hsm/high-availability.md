---
title: Yüksek kullanılabilirlik-Azure ayrılmış HSM | Microsoft Docs
description: Azure adanmış HSM yüksek kullanılabilirliğe yönelik temel konular hakkında bilgi edinin. Bu makale bir örnek içerir.
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/15/2021
ms.author: mbaldwin
ms.openlocfilehash: 85aa9d355b2440535ea13c8c15e95500bac22352
ms.sourcegitcommit: ca215fa220b924f19f56513fc810c8c728dff420
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2021
ms.locfileid: "98567263"
---
# <a name="azure-dedicated-hsm-high-availability"></a>Azure adanmış HSM yüksek kullanılabilirlik

Azure adanmış HSM, Microsoft 'un yüksek oranda kullanılabilir veri merkezleri tarafından sabitlenmiştir. Ancak, yüksek oranda kullanılabilir veri merkezi, yerelleştirilmiş arızalara ve çok büyük koşullarda bölgesel düzey hatalara karşı savunmasız kalır. Microsoft, birden çok cihazın tek bir rafı paylaşan cihazlara neden olmamasını sağlamak için bir bölgedeki farklı veri merkezlerinde HSM cihazları dağıtır. Bu HSM 'ler, Thales HA Grup özelliği kullanılarak bir bölgedeki veri merkezleri arasında eşlenilerek daha fazla yüksek kullanılabilirlik elde edilebilir. Ayrıca, bir olağanüstü durum kurtarma durumunda bölgesel yük devretmeyi ele almak için cihazları bölgeler arasında eşleştirmek mümkündür. Bu çok katmanlı yüksek kullanılabilirlik yapılandırmasıyla, uygulamaların çalışmasını sağlamak için herhangi bir cihaz hatası otomatik olarak karşılanır. Tüm veri merkezleri aynı zamanda site üzerinde yedek cihazlara ve bileşenlere sahiptir, bu nedenle başarısız olan tüm cihazlar zamanında değiştirilebilir.

## <a name="high-availability-example"></a>Yüksek kullanılabilirlik örneği

Yazılım düzeyinde yüksek kullanılabilirlik için HSM cihazlarının nasıl yapılandırılacağı hakkında bilgi ' Thales Luna 7 HSM Yönetim Kılavuzu ' nda bulunur. Bu belge  [Thales HSM sayfasında](https://thalesdocs.com/gphsm/Content/luna/network/luna_network_releases.htm)bulunur.

Aşağıdaki diyagramda, yüksek oranda kullanılabilir bir mimari gösterilmektedir. Bölgede birden çok cihazı ve ayrı bir bölgede eşleştirilmiş birden çok cihazı kullanır. Bu mimari en az dört HSM cihaz ve sanal ağ bileşeni kullanır.

![Yüksek kullanılabilirlik diyagramı](media/high-availability/high-availability.png)

## <a name="next-steps"></a>Sonraki adımlar

Yüksek kullanılabilirlik ve güvenlik gibi hizmetin temel kavramlarının cihaz sağlama ve uygulama tasarımı ya da dağıtımdan önce iyi anlaşıldığından emin olmanız önerilir.
Daha fazla kavram düzeyi konuları:

* [Dağıtım mimarisi](deployment-architecture.md)
* [Fiziksel Güvenlik](physical-security.md)
* [Ağ](networking.md)
* [Desteklenebilirlik](supportability.md)
* [İzleme](monitoring.md)

Yüksek kullanılabilirlik için HSM cihazlarını yapılandırma hakkında ayrıntılı bilgi için, lütfen yönetici kılavuzlarına yönelik Thales müşteri destek portalına başvurun ve Bölüm 6 ' yı inceleyin.
