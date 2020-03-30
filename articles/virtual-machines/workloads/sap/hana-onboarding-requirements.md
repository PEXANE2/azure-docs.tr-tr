---
title: Azure'da SAP HANA için onboarding gereksinimleri (Büyük Örnekler) | Microsoft Dokümanlar
description: Azure'da SAP HANA için onboarding gereksinimleri (Büyük Örnekler).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/31/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 141a75a75a214ff4a6f136df7570d6e81f7f4e82
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617012"
---
# <a name="onboarding-requirements"></a>Ekleme gereksinimleri

Bu liste, Azure'da (Büyük Örnekler) SAP HANA'yı çalıştırmak için gereksinimleri bir araya getirin.

**Microsoft Azure**

- Azure'da SAP HANA'ya bağlanabilen bir Azure aboneliği (Büyük Örnekler).
- Microsoft Premier destek sözleşmesi. Azure'da SAP'yi çalıştırmayla ilgili belirli bilgiler için [Microsoft Azure'daki SAP Destek Notu #2015553 - SAP: Ön koşulları destekleyin.](https://launchpad.support.sap.com/#/notes/2015553) 384 ve daha fazla CPU'ya sahip HANA Büyük Örnek birimlerini kullanıyorsanız, Premier destek sözleşmesini Azure Hızlı Yanıt'ı da içerecek şekilde genişletmeniz gerekir.
- SAP ile boyutlandırma egzersizi yaptıktan sonra ihtiyacınız olan HANA Büyük Örnek SUK'ların farkındalığı.

**Ağ bağlantısı**

- Şirket içi ile Azure arasında ExpressRoute: Şirket içi veri merkezinizi Azure'a bağlamak için ISS'nizden en az 1 Gbps bağlantı siparişi verin. HANA Büyük Örnek birimleri ile Azure arasındaki bağlantı ExpressRoute teknolojisini de kullanıyor. HANA Büyük Örnek birimleri ile Azure arasındaki bu ExpressRoute bağlantısı, bu özel ExpressRoute devresi için tüm veri girişi ve çıkış ücretleri de dahil olmak üzere HANA Büyük Örnek birimlerinin fiyatına dahildir. Bu nedenle, müşteri olarak, şirket içi ve Azure arasındaki ExpressRoute bağlantınızın ötesinde ek maliyetlerle karşılaşamazsınız.

**İşletim sistemi**

- SAP Uygulamaları için SUSE Linux Enterprise Server 12 lisansları.

   > [!NOTE] 
   > Microsoft tarafından teslim edilen işletim sistemi SUSE'ye kayıtlı değildir. Abonelik Yönetimi Aracı örneğine bağlı değildir.

- SUSE Linux Abonelik Yönetimi Aracı, Azure'da bir VM'de dağıtılır. Bu araç, Azure'daki SAP HANA'nın (Büyük Örnekler) SUSE tarafından kaydedilmesi ve sırasıyla güncelleştirilebilme olanağı sağlar. (HANA Büyük Örnek veri merkezi içinde internet erişimi yoktur.) 
- Red Hat Enterprise Linux 6.7 veya SAP HANA için 7.x lisansları.

   > [!NOTE]
   > Microsoft tarafından teslim edilen işletim sistemi Red Hat'e kayıtlı değildir. Red Hat Abonelik Yöneticisi örneğine bağlı değildir.

- Red Hat Subscription Manager, Azure'da bir VM'de dağıtıldı. Red Hat Subscription Manager, Azure'daki SAP HANA'nın (Büyük Örnekler) Kaydedilmesi ve sırasıyla Red Hat tarafından güncelleştirilebilme olanağı sağlar. (Azure Büyük Örnek damgası üzerinde dağıtılan kiracının içinden doğrudan internet erişimi yoktur.)
- SAP, Linux sağlayıcınızla da bir destek sözleşmesi ne kadar çok sahip olduğunuzu gerektirir. Bu gereksinim, HANA Büyük Örnek çözümü veya Azure'da Linux çalıştırdığınız gerçeği yle kaldırılmaz. Bazı Linux Azure galeri görüntülerinin aksine, hizmet ücreti HANA Large Instance'ın çözüm teklifine dahil *değildir.* Linux dağıtıcısı ile yapılan destek sözleşmeleri ile ilgili SAP gerekliliklerini yerine getirmek sizin sorumluluğunuzdadır. 
   - SUSE Linux için SAP Note #1984787 destek sözleşmelerinin gerekliliklerini araştırın [- SUSE Linux Enterprise Server 12: Kurulum notları](https://launchpad.support.sap.com/#/notes/1984787) ve SAP Not #1056161 - SAP uygulamaları için [SUSE öncelikli desteği.](https://launchpad.support.sap.com/#/notes/1056161)
   - Red Hat Linux için, HANA Büyük Örnek işletim sistemlerinde destek ve hizmet güncelleştirmeleri içeren doğru abonelik düzeylerine sahip olmanız gerekir. Red Hat SAP çözümü için Red Hat Enterprise Linux aboneliği önerir. Bkz. https://access.redhat.com/solutions/3082481. 

Farklı Linux sürümlerine sahip farklı SAP HANA sürümlerinin destek matrisi için [SAP Note #2235581](https://launchpad.support.sap.com/#/notes/2235581)bakın.

İşletim sisteminin uyumluluk matrisi ve HLI firmware/sürücü sürümleri [için HLI için OS Yükseltme'ye](os-upgrade-hana-large-instance.md)bakın.


> [!IMPORTANT] 
> Tip II üniteleri için bu noktada yalnızca SLES 12 SP2 OS sürümü desteklenir. 


**Database**

- SAP HANA (platform veya kurumsal sürüm) için lisanslar ve yazılım yükleme bileşenleri.

**Uygulamalar**

- SAP HANA'ya ve ilgili SAP destek sözleşmelerine bağlanan tüm SAP uygulamaları için lisanslar ve yazılım yükleme bileşenleri.
- Azure (Büyük Örnekler) ortamlarında ve ilgili destek sözleşmelerinde SAP HANA ile kullanılan SAP dışı uygulamalar için lisanslar ve yazılım yükleme bileşenleri.

**Beceri**

- Azure IaaS ve bileşenleri yle ilgili deneyim ve bilgi birikimi.
- Azure'da SAP iş yükünü nasıl dağıtılanın ve bunları öğrenin.
- SAP HANA kurulum sertifikalı kişisel.
- SAP HANA çevresinde yüksek kullanılabilirlik ve felaket kurtarma tasarlamak için SAP mimar becerileri.

**SAP**

- Beklenti, sap müşterisi ve SAP ile bir destek sözleşmeniz olmasıdır.
- Özellikle HANA Büyük Örnek SK'lerin Tip II sınıfı nın uygulamaları için SAP HANA sürümleri ve büyük boyutlu ölçekli donanım üzerindeki nihai yapılandırmalar hakkında SAP'ye danışın.

**Sonraki adımlar**
- [Azure'da SAP HANA (Büyük Örnekler) mimarisine](hana-architecture.md) bakın