---
title: Azure'da SAP HANA (Büyük Örnekler) koşullarını bilin | Microsoft Dokümanlar
description: Azure'daki SAP HANA (Büyük Örnekler) terimlerini bilin.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/21/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6785ae821f701121185f0064c6317c69d50191ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617060"
---
# <a name="know-the-terms"></a>Terimleri öğrenme

Mimari ve Teknik Dağıtım Kılavuzu'nda çeşitli yaygın tanımlar yaygın olarak kullanılır. Aşağıdaki terimlere ve anlamlarına dikkat edin:

- **IaaS**: Hizmet olarak altyapı.
- **PaaS**: Platform bir hizmet olarak.
- **SaaS**: Bir hizmet olarak yazılım.
- **SAP bileşeni**: ERP Merkezi Bileşeni (ECC), İş Ambarı (BW), Çözüm Yöneticisi veya Kurumsal Portal (EP) gibi tek bir SAP uygulaması. SAP bileşenleri geleneksel ABAP veya Java teknolojilerine veya İş Nesneleri gibi NetWeaver tabanlı olmayan bir uygulamaya dayalı olabilir.
- **SAP ortamı**: Geliştirme, kalite güvencesi, eğitim, olağanüstü durum kurtarma veya üretim gibi bir iş işlevini gerçekleştirmek üzere mantıksal olarak gruplanmış bir veya daha fazla SAP bileşeni.
- **SAP peyzajı**: BT ortamınızdaki tüm SAP varlıklarını ifade eder. SAP ortamı tüm üretim ve üretim dışı ortamları içerir.
- **SAP sistemi**: Örneğin, bir SAP ERP geliştirme sistemi, SAP BW test sistemi ve SAP CRM üretim sisteminin DBMS katmanı ve uygulama katmanının kombinasyonu. Azure dağıtımları, bu iki katmanı şirket içi ve Azure arasında bölmeyi desteklemez. Sap sistemi şirket içinde veya Azure'da dağıtılır. SAP ortamının farklı sistemlerini Azure'a veya şirket içinde dağıtabilirsiniz. Örneğin, SAP CRM üretim sistemini şirket içinde dağıtırken SAP CRM geliştirme ve test sistemlerini Azure'da dağıtabilirsiniz. Azure'daki SAP HANA (Büyük Örnekler) için, VM'lerde SAP sistemlerinin SAP uygulama katmanını ve ilgili SAP HANA örneğini Azure (Büyük Örnekler) damgasındaki SAP HANA'daki bir birimde barındırmanız amaçlanmıştır.
- **Büyük Örnek damgası**: SAP HANA TDI sertifikalı ve Azure içinde SAP HANA örneklerini çalıştırmak için ayrılmış bir donanım altyapı yığını.
- **Azure'da SAP HANA (Büyük Örnekler):** Farklı Azure bölgelerindeki Büyük Örnek damgalarında dağıtılan SAP HANA TDI sertifikalı donanımda HANA örneklerini çalıştırmak için Azure'daki teklifin resmi adı. İlgili TERIM *HANA Büyük Örnek,* *Azure'daki SAP HANA'nın kısaltmasıdır (Büyük Örnekler)* ve bu teknik dağıtım kılavuzunda yaygın olarak kullanılır.
- **Tesisler arası**: Sanal Tema'ların yerinde, çok siteye veya Azure ExpressRoute bağlantısına sahip bir Azure aboneliğine dağıtıldığı bir senaryoyu açıklar. Ortak Azure belgelerinde, bu tür dağıtımlar binalar arası senaryolar olarak da tanımlanır. Bağlantının nedeni, şirket içi etki alanlarını, şirket içi Azure Active Directory/OpenLDAP'ı ve şirket içi DNS'yi Azure'a genişletmektir. Şirket içi manzara, Azure aboneliklerinin Azure varlıklarına genişletilir. Bu uzantıyla, VM'ler şirket içi etki alanının bir parçası olabilir. 

   Şirket içi etki alanıkullanıcıları sunuculara erişebilir ve bu VM'lerde (DBMS hizmetleri gibi) hizmetler çalıştırabilir. Şirket içinde dağıtılan VM'ler ile Azure tarafından dağıtılan VM'ler arasında iletişim ve ad çözümlemesi mümkündür. Bu senaryo, çoğu SAP varlığının dağıtılbiçimini gösteren tipik bir durumdur. Daha fazla bilgi için Azure [VPN Ağ Geçidi'ne](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bakın ve [Azure portalını kullanarak siteden siteye bağlantı içeren bir sanal ağ oluşturun.](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- **Kiracı**: HANA Büyük Örnek damgasında dağıtılan bir müşteri *kiracıya* yalıtılır. Kiracı, diğer kiracıların ağ, depolama ve işlem katmanında yalıtılır. Farklı kiracılara atanan depolama ve bilgi işlem birimleri hana Büyük Örnek damga düzeyinde birbirlerini göremez veya birbirleriyle iletişim kuramez. Bir müşteri farklı kiracılara dağıtım yapmayı seçebilir. O zaman bile, HANA Büyük Örnek damga düzeyinde kiracılar arasında hiçbir iletişim yoktur.
- **SKU kategorisi**: HANA Büyük Örneği için aşağıdaki iki sku kategorisi sunulmaktadır:
    - **Tip I sınıfı**: S72, S72m, S96, S144, S144m, S192, S192m, S192xm, S224 ve S224m
    - **Tip II sınıfı**: S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm ve S960m
- **Damga**: HANA Büyük Örnekleri'nin Microsoft dahili dağıtım boyutunu tanımlar. HANA Büyük Örnek birimleri dağıtılmadan önce, bilgi işlem, ağ ve depolama raflarından oluşan bir HANA Büyük Örnek damgasının veri merkezi konumunda dağıtılması gerekir. Böyle bir dağıtım hana büyük örnek damgası veya Revizyon 4 (aşağıya bakın) biz **Büyük Örnek Satır** teriminin alternatif kullanmak denir
- **Revizyon**: HANA Büyük Örnek pulları için iki farklı pul revizyonu vardır. Bunlar mimari ve Azure sanal makine ana bilgisayarlarına yakınlık açısından farklılık gösterir
    - "Revision 3" (Rev 3): 2016 yılının ortasından itibaren dağıtılan orijinal tasarımdır
    - "Revizyon 4" (Rev 4): Azure sanal makine ana bilgisayarlarına daha yakın yakınlık ve Azure VM'ler ile HANA Büyük Örnek birimleri arasındaki daha düşük ağ gecikmesi sağlayan yeni bir tasarımdır 

Bulutta SAP iş yükünün nasıl dağıtılancaya kadar çeşitli ek kaynaklar mevcuttur. AZURE'da SAP HANA dağıtımı yapmayı planlıyorsanız, Azure IaaS ilkelerini ve Azure IaaS'da SAP iş yüklerinin dağıtımıkonusunda deneyimli ve bunun farkında olmanız gerekir. Devam etmeden önce daha fazla bilgi için [Azure sanal makinelerinde SAP çözümlerini kullan'a](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bakın. 

**Sonraki adımlar**
- Bkz. [HLI Sertifikası](hana-certification.md)