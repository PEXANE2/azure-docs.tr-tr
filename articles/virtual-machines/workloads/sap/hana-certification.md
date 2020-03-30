---
title: AZURE'da SAP HANA Sertifikası (Büyük Örnekler) | Microsoft Dokümanlar
description: SAP HANA'nın Azure'da sertifikası (Büyük Örnekler).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2a02f0e1b05b9de8105126d1c9e4e3f79057285f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617221"
---
# <a name="certification"></a>Sertifika

NetWeaver sertifikasının yanı sıra SAP, SAP HANA için Azure IaaS gibi belirli altyapılarda SAP HANA'yı desteklemek için özel bir sertifika gerektirir.

NetWeaver'daki temel SAP Notu ve bir dereceye kadar SAP HANA sertifikası, [Azure'daki SAP Note #1928533 – Azure'daki SAP uygulamaları: Desteklenen ürünler ve Azure VM türleridir.](https://launchpad.support.sap.com/#/notes/1928533)

Azure (Büyük Örnekler) birimlerindeki SAP HANA'nın sertifika kayıtları [SAP HANA sertifikalı IaaS Platformları](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) sitesinde bulunabilir. 

SAP HANA sertifikalı IaaS Platformları sitesinde atıfta bulunulan Azure'daki SAP HANA (Büyük Örnekler) türleri, Microsoft ve SAP müşterilerine Azure'da büyük SAP Business Suite, SAP BW, S/4 HANA, BW/4HANA veya diğer SAP HANA iş yüklerini dağıtma olanağı sağlar. Çözüm SAP-HANA sertifikalı özel donanım damgası[(SAP HANA özel veri merkezi entegrasyonu – TDI)](https://scn.sap.com/docs/DOC-63140)dayanmaktadır. SAP HANA TDI tarafından yapılandırılmış bir çözüm çalıştırırsanız, tüm SAP HANA tabanlı uygulamalar (SAP HANA'daki SAP Business Suite, SAP HANA'daki SAP BW, S4/HANA ve BW4/HANA gibi) donanım altyapısı üzerinde çalışır.

VM'lerde SAP HANA'yı çalıştırmakla karşılaştırıldığında, bu çözümün bir yararı vardır. Çok daha büyük bellek hacimleri sağlar. Bu çözümü etkinleştirmek için aşağıdaki temel hususları anlamanız gerekir:

- SAP uygulama katmanı ve SAP olmayan uygulamalar, her zamanki Azure donanım damgalarında barındırılan VM'lerde çalışır.
- Müşteri şirket içi altyapı, veri merkezleri ve uygulama dağıtımları ExpressRoute (önerilir) veya sanal özel ağ (VPN) aracılığıyla bulut platformuna bağlanır. Etkin Dizin ve DNS de Azure'a genişletilir.
- HANA iş yükü için SAP HANA veritabanı örneği Azure'da (Büyük Örnekler) SAP HANA'da çalışır. Büyük Örnek damgası Azure ağına bağlanır, böylece VM'lerde çalışan yazılımlar HANA Örneği'nde çalışan HANA örneğiyle etkileşimkurabilir.
- SAP HANA'nın Azure'daki donanımı (Büyük Örnekler), SUSE Linux Enterprise Server veya Red Hat Enterprise Linux önceden yüklenmiş bir IaaS'da sağlanan özel donanımdır. Sanal makinelerde olduğu gibi, işletim sistemine daha fazla güncelleme ve bakım sizin sorumluluğunuzdadır.
- HANA'nın veya SAP HANA'nın HANA Büyük Örnek birimlerine çalıştırılması için gerekli ek bileşenlerin yüklenmesi sizin sorumluluğunuzdadır. SAP HANA'nın Azure'da devam eden tüm işlemleri ve yönetimi de sizin sorumluluğunuzdadır.
- Burada açıklanan çözümlere ek olarak, Azure aboneliğinizde SAP HANA'ya bağlanan diğer bileşenleri Azure'da (Büyük Örnekler) yükleyebilirsiniz. Örnekler, atlama sunucuları, RDP sunucuları, SAP HANA Studio, SAP BI senaryoları için SAP Veri Hizmetleri veya ağ izleme çözümleri gibi SAP HANA veritabanıyla veya doğrudan iletişimi sağlayan bileşenlerdir.
- Azure'da olduğu gibi, HANA Büyük Örnek yüksek kullanılabilirlik ve olağanüstü durum kurtarma işlevleri için destek sunar.

**Sonraki adımlar**
- [HLI için Mevcut SK'ları](hana-available-skus.md) Referans Alalım 