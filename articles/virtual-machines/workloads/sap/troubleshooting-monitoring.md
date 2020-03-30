---
title: SAP HANA'nın Azure'da İzlenmesi (Büyük Örnekler) | Microsoft Dokümanlar
description: SAP HANA'yı Bir Azure'da (Büyük Örnekler) izleyin.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 50a6b4f15a7de02533e3bb51e5659f7b4c078b40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617302"
---
# <a name="how-to-monitor-sap-hana-large-instances-on-azure"></a>Azure'da SAP HANA (büyük örnekler) nasıl izlenir?

Azure'daki SAP HANA (Büyük Örnekler) diğer IaaS dağıtımından farklı değildir — işletim sistemi ve uygulamanın ne yaptığını ve uygulamaların aşağıdaki kaynakları nasıl tükettiğinizi izlemeniz gerekir:

- CPU
- Bellek
- Ağ bant genişliği
- Disk alanı

Azure Sanal Makineler ile, yukarıda belirtilen kaynak sınıflarının yeterli olup olmadığını veya tükenip tükenmediğini anlamanız gerekir. Burada farklı sınıfların her biri hakkında daha fazla ayrıntı:

**CPU kaynak tüketimi:** SAP'nin HANA'ya karşı belirli iş yükü için tanımladığı oran, bellekte depolanan veriler üzerinden çalışmak için yeterli CPU kaynağı nın bulunması gerektiğinden emin olmak için uygulanır. Bununla birlikte, HANA'nın eksik dizinler veya benzer sorunlar nedeniyle sorguları yürüten birçok CPU tükettiği durumlar olabilir. Bu, HANA büyük örnek biriminin CPU kaynak tüketimini ve belirli HANA hizmetleri tarafından tüketilen CPU kaynaklarını izlemeniz gerektiği anlamına gelir.

**Bellek tüketimi:** Hana içinden ve hana dışından birim üzerinde izlemek önemlidir. HANA içinde, SAP'nin gerekli boyutlandırma yönergeleri içinde kalmak için verilerin HANA tahsis edilmiş belleği nasıl tükettiğini izleyin. Ayrıca, hana olmayan ek yüklü yazılımın çok fazla bellek tüketmediğinden ve bu nedenle bellek için HANA ile rekabet etmediğinden emin olmak için bellek tüketimini Büyük Örnek düzeyinde izlemek istiyorsunuz.

**Ağ bant genişliği:** Azure VNet ağ geçidi, Azure VNet'e taşınan verilerin bant genişliğiyle sınırlıdır, bu nedenle seçtiğiniz Azure ağ geçidi SKU'nun sınırlarına ne kadar yakın olduğunuzu anlamak için VNet içindeki tüm Azure VM'leri tarafından alınan verileri izlemek yararlı olur. HANA Büyük Örnek biriminde, gelen ve giden ağ trafiğini de izlemek ve zaman içinde işlenen birimleri izlemek mantıklıdır.

**Disk alanı:** Disk alanı tüketimi genellikle zaman içinde artar. En sık karşılaşılan nedenler şunlardır: veri hacmi artışları, işlem günlüğü yedeklemelerinin yürütülmesi, izleme dosyalarının depolanması ve depolama anlık görüntülerinin gerçekleştirilmesi. Bu nedenle, disk alanı kullanımını izlemek ve HANA Büyük Örnek birimiyle ilişkili disk alanını yönetmek önemlidir.

HANA Büyük Örnekleri'nin **Tip II SK'leri** için sunucu önceden yüklenmiş sistem tanılama araçlarıyla birlikte gelir. Sistem sistem durumu denetimini gerçekleştirmek için bu tanılama araçlarını kullanabilirsiniz. /var/log/health_check adresindeki sistem durumu denetimi günlüğü dosyasını oluşturmak için aşağıdaki komutu çalıştırın.
```
/opt/sgi/health_check/microsoft_tdi.sh
```
Bir sorunu gidermek için Microsoft Destek ekibiyle birlikte çalıştığınızda, bu tanılama araçlarını kullanarak günlük dosyalarını sağlamanız da istenebilir. Aşağıdaki komutu kullanarak dosyayı kapatabilirsiniz.
```
tar  -czvf health_check_logs.tar.gz /var/log/health_check
```

**Sonraki adımlar**

- [Azure'da SAP HANA'yı (büyük örnekler) nasıl izleyebilirsiniz'e](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-monitor-troubleshoot)bakın.
