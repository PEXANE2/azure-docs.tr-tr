---
title: Azure Ağ İzleyicisi'nde IP akışına giriş | Microsoft Dokümanlar
description: Bu sayfa, Network Watcher IP akışını doğrulama özelliğine genel bir bakış sağlar
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2017
ms.author: damendo
ms.openlocfilehash: 69aca5e0901a0da8aa98fe310ac220898bf650b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76845015"
---
# <a name="introduction-to-ip-flow-verify-in-azure-network-watcher"></a>Azure Ağ İzleyicisi'nde IP akışına giriş doğrulama

IP akışı, bir pakete sanal makineye izin verilip verilmediğini veya reddedilmesini doğrular. Bilgiler yön, protokol, yerel IP, uzak IP, yerel bağlantı noktası ve uzak bağlantı noktasından oluşur. Paket bir güvenlik grubu tarafından reddedilirse, paketi reddeden kuralın adı döndürülür. Herhangi bir kaynak veya hedef IP seçilebilirken, IP akışı doğrulamak yöneticilerin internetten ve şirket içi ortamdan veya şirket içi ortamdan bağlantı sorunlarını hızlı bir şekilde teşhis etmelerine yardımcı olur.

IP akışı, ağ arabirimine uygulanan alt ağ veya sanal makine NIC gibi tüm Ağ Güvenlik Gruplarının (NSG'ler) kurallarına baktığını doğrular. Trafik akışı daha sonra bu ağ arabirimine veya bu ağ arabiriminden yapılandırılan ayarlara göre doğrulanır. IP akışı doğrulama, Bir Ağ Güvenlik Grubu'ndaki bir kuralın sanal bir makineye girişi engelleyip engellemediği veya trafiği mi çıkarağını doğrulamada yararlıdır.

Ip akışını doğrulamayı çalıştırmayı planladığınız tüm bölgelerde Ağ İzleyicisi'nin bir örneğinin oluşturulması gerekir. Ağ İzleyicisi bölgesel bir hizmettir ve yalnızca aynı bölgedeki kaynaklara karşı çalıştırılabilir. NIC veya alt ağ ile ilişkili herhangi bir rota hala döndürüldeğinden, kullanılan örnek IP akışı doğrulama sonuçlarını etkilemez.

![1][1]

## <a name="next-steps"></a>Sonraki adımlar

Portal üzerinden belirli bir sanal makine için bir pakete izin verilip verilmediğini veya reddedilip reddedildiğini öğrenmek için aşağıdaki makaleyi ziyaret edin. [Ip Flow Portalı kullanarak IP Akışı Doğrula ile VM'de trafiğe izin verilip verilmediğini kontrol edin](diagnose-vm-network-traffic-filtering-problem.md)

[1]: ./media/network-watcher-ip-flow-verify-overview/figure1.png

