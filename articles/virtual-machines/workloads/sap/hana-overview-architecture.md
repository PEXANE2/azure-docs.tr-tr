---
title: AZURE'da SAP HANA'ya Genel Bakış (Büyük Örnekler) | Microsoft Dokümanlar
description: SAP HANA'nın Azure'da (Büyük Örnekler) nasıl dağıtılanabildiğini genel olarak öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/12/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 39fcf5d0fe2273c4debd3ae5ebe5fd1190ddc959
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616961"
---
#  <a name="what-is-sap-hana-on-azure-large-instances"></a>Azure üzerinde SAP HANA (Büyük Örnekler) nedir?

Azure'daki SAP HANA (Büyük Örnekler) Azure için benzersiz bir çözümdür. Azure, SAP HANA'yı dağıtmak ve çalıştırmak için sanal makineler sağlamanın yanı sıra, size adanmış çıplak metal sunucularda SAP HANA'yı çalıştırma ve dağıtma olanağı sunar. Azure'da SAP HANA (Büyük Örnekler) çözümü, size atanan paylaşılmayan ana bilgisayar/sunucu çıplak metal donanımı üzerine inşa edilir. Sunucu donanımı, bilgi işlem/sunucu, ağ ve depolama altyapısı içeren daha büyük pullara gömülüdür. Bir kombinasyon olarak, HANA özel veri merkezi entegrasyonu (TDI) sertifikalı. Azure'daki SAP HANA (Büyük Örnekler) farklı sunucu SUS'ları veya boyutları sunar. Birimler 36 Intel CPU çekirdeğine ve 768 GB belleğe sahip olabilir ve 480 Intel CPU çekirdeğine ve 24 TB'a kadar belleğe sahip birimlere kadar çıkabilir.

Altyapı damgası içindeki müşteri yalıtımı kiracılarda gerçekleştirilir ve aşağıdakiler gibi görünür:

- **Ağ :** Müşterilerin altyapı yığını içindeki yalıtımı, atanan kiracı başına sanal ağlar aracılığıyla. Kiracı tek bir müşteriye atanır. Bir müşterinin birden çok kiracısı olabilir. Kiracıların ağ yalıtımı, kiracılar aynı müşteriye ait olsa bile, altyapı damgası düzeyinde kiracılar arasında ağ iletişimini yasaklar.
- **Depolama bileşenleri**: Depolama hacimleri atanmış depolama sanal makineleri aracılığıyla yalıtım. Depolama birimleri yalnızca tek bir depolama sanal makinesine atanabilir. Bir depolama sanal makine sap HANA TDI sertifikalı altyapı yığınında tek bir kiracı ya da yalnızca atanır. Sonuç olarak, bir depolama sanal makineatanan depolama birimleri yalnızca belirli ve ilgili kiracı erişilebilir. Bunlar, dağıtılan farklı kiracılar arasında görünmez.
- **Sunucu veya ana bilgisayar**: Sunucu veya ana bilgisayar birimi müşteriler veya kiracılar arasında paylaşılmaz. Bir müşteriye dağıtılan sunucu veya ana bilgisayar, tek bir kiracıya atanan atomik bir çıplak metal bilgi işlem birimidir. Ana bilgisayar veya sunucuyu başka bir müşteriyle paylaşmanıza neden olabilecek *donanım* bölümleme veya yumuşak bölümleme kullanılmaz. Belirli bir kiracının depolama sanal makinesine atanan depolama birimleri böyle bir sunucuya monte edilir. Kiracı, yalnızca farklı SK'lerden oluşan bir sunucu birimine sahip olabilir.
- Azure (Büyük Örnekler) altyapı damgası üzerindeki BIR SAP HANA'da, ağ, depolama ve bilgi işlem düzeyindeki kiracı kavramları aracılığıyla birçok farklı kiracı dağıtılır ve birbirlerine karşı izole edilir. 


Bu çıplak metal sunucu birimleri yalnızca SAP HANA'yı çalıştırmak için desteklenir. SAP uygulama katmanı veya iş yükü orta gereç katmanı sanal makinelerde çalışır. Azure (Büyük Örnekler) birimlerinde SAP HANA'yı çalıştıran altyapı damgaları Azure ağ hizmetleri omurgalarına bağlanır. Bu şekilde, Azure (Büyük Örnekler) ünitelerindeki SAP HANA ile sanal makineler arasında düşük gecikmeli bağlantı sağlanır.

Temmuz 2019 itibariyle, HANA Büyük Örnek pullarının iki farklı revizyonu ve dağıtımların yeri arasında ayrım yapıyoruz:

- "Revizyon 3" (Rev 3): Temmuz 2019'dan önce müşterinin dağıtılaması için yapılmış pullar
- "Revizyon 4" (Rev 4): Azure VM ana bilgisayarlarına yakın bir mesafede dağıtılan ve şu ana kadar Azure bölgelerinde yayımlanan yeni damga tasarımı:
    -  Batı ABD 2 
    -  Doğu ABD 
    -  Batı Avrupa
    -  Kuzey Avrupa


Bu belge, Azure'daki SAP HANA'yı (Büyük Örnekler) kapsayan birkaç belgeden biridir. Bu belge, çözümün sağladığı temel mimari, sorumluluklar ve hizmetleri tanımaktadır. Çözümün üst düzey yetenekleri de tartışılmıştır. Ağ ve bağlantı gibi diğer birçok alanda, diğer dört belge ayrıntıları ve ayrıntılı bilgileri kapsar. SAP HANA'nın Azure'daki (Büyük Örnekler) dokümantasyonu, SAP NetWeaver yüklemesinin veya SAP NetWeaver'ın VM'lerde dağıtımlarının yönlerini kapsamaz. Azure'daki SAP NetWeaver, aynı Azure belge kapsayıcısında bulunan ayrı belgelerle kaplıdır. 


HANA Büyük Örnek kılavuzunun farklı belgeleri aşağıdaki alanları kapsamaktadır:

- [AZURE'da SAP HANA (Büyük Örnekler) genel bakışı ve mimarisi](hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Azure'da SAP HANA (Büyük Örnekler) altyapısı ve bağlantısı](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [SAP HANA'yı (Büyük Örnekler) Azure'a yükleme ve yapılandırma](hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Azure'da SAP HANA (Büyük Örnekler) yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Azure'da SAP HANA (Büyük Örnekler) sorun giderme ve izleme](troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [STONITH kullanılarak SUSE'da yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/ha-setup-with-stonith)
- [Revizyon 3 pulların Tip II SK'leri için IŞLETIM sistemi yedekleme ve geri yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/os-backup-type-ii-skus)

**Sonraki adımlar**
- Bkz. [Terimleri Tanıyın](hana-know-terms.md)