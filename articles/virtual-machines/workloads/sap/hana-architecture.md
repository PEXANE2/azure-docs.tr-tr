---
title: AZURE'da SAP HANA Mimarisi (Büyük Örnekler) | Microsoft Dokümanlar
description: SAP HANA'nın Azure'da (Büyük Örnekler) nasıl dağıtılanabildiğini anlatan mimari.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/25/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 041da4198b0bdd040a4916008a1135aa2e2a5f7d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77614530"
---
# <a name="sap-hana-large-instances-architecture-on-azure"></a>Azure'da SAP HANA (Büyük Örnekler) mimarisi

Yüksek düzeyde, Azure'daki SAP HANA (Büyük Örnekler) çözümü, VM'lerde bulunan SAP uygulama katmanına sahiptir. Veritabanı katmanı, Azure IaaS'a bağlı aynı Azure bölgesinde Büyük Örnek damgasında bulunan SAP TDI yapılandırılmış donanımında bulunur.

> [!NOTE]
> SAP uygulama katmanını SAP DBMS katmanıyla aynı Azure bölgesinde dağıtın. Bu kural, Azure'daki SAP iş yükleri hakkında yayımlanmış bilgilerde iyi belgelenmiştir. 

Azure'daki SAP HANA'nın (Büyük Örnekler) genel mimarisi, SAP HANA veritabanı için sanallaştırılmayan, çıplak metal, yüksek performanslı bir sunucu olan SAP TDI sertifikalı bir donanım yapılandırması sağlar. Ayrıca, Azure'un SAP uygulama katmanının kaynaklarını gereksinimlerinizi karşılamak üzere ölçeklendirme olanağı ve esnekliği de sağlar.

![Azure'da SAP HANA'ya mimari genel bakış (Büyük Örnekler)](./media/hana-overview-architecture/image1-architecture.png)

Gösterilen mimari üç bölüme ayrılmıştır:

- **Sağ**: Son kullanıcıların SAP gibi LOB uygulamalarına erişebilmeleri için veri merkezlerinde farklı uygulamalar çalıştıran şirket içi altyapıyı gösterir. İdeal olarak, bu şirket içi altyapı [ExpressRoute](https://azure.microsoft.com/services/expressroute/)ile Azure'a bağlıdır.

- **Merkez**: Azure IaaS'ı ve bu durumda SAP'yi veya SAP HANA'yı DBMS sistemi olarak kullanan diğer uygulamaları barındırmak için VM'leri kullanır. VM'lerin sağladığı bellekle çalışan daha küçük HANA örnekleri, uygulama katmanlarıyla birlikte VM'lerde dağıtılır. Sanal makineler hakkında daha fazla bilgi için [Sanal makinelere](https://azure.microsoft.com/services/virtual-machines/)bakın.

   Azure ağ hizmetleri, SAP sistemlerini diğer uygulamalarla sanal ağlarda gruplandırmak için kullanılır. Bu sanal ağlar şirket içi sistemlere ve Azure'daki SAP HANA'ya (Büyük Örnekler) bağlanır.

   Azure'da çalıştırılmak üzere desteklenen SAP NetWeaver uygulamaları ve veritabanları için [Azure'daki SAP Destek Notu #1928533 - Azure'daki SAP uygulamalarına bakın: Desteklenen ürünler ve Azure VM türleri.](https://launchpad.support.sap.com/#/notes/1928533) SAP çözümlerinin Azure'da nasıl dağıtılanabildiğini belgelemek için bkz:

  -  [SAP'yi Windows sanal makinelerde kullanma](../../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [Azure sanal makinelerde SAP çözümlerini kullanma](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **Sol**: Azure Büyük Örnek damgasında SAP HANA TDI sertifikalı donanımı gösterir. HANA Büyük Örnek birimleri, şirket içi Azure'a bağlantıyla aynı teknolojiyi kullanarak Azure aboneliğinizin sanal ağlarına bağlanır. Mayıs 2019 itibariyle, HANA Büyük Örnek birimleri ile Azure VM'leri arasında ExpressRoute Ağ Geçidi'nin katılımı olmadan iletişim kurmasına olanak tanıyan bir optimizasyon sunuldu. ExpressRoute Hızlı Yol adı verilen bu optimizasyon bu mimaride (kırmızı çizgiler) görüntülenir. 

Azure Büyük Örnek damgası kendisi aşağıdaki bileşenleri birleştirir:

- **Bilgi İşlem**: Gerekli bilgi işlem yeteneğini sağlayan ve SAP HANA sertifikalı farklı nesil Intel Xeon işlemcilerine dayanan sunucular.
- **Ağ**: Bilgi işlem, depolama ve LAN bileşenlerini birbirine bağlayan birleşik yüksek hızlı ağ dokusu.
- **Depolama**: Birleşik ağ dokusu üzerinden erişilen depolama altyapısı. Sağlanan belirli depolama kapasitesi, dağıtılan Azure (Büyük Örnekler) yapılandırmasına ilişkin belirli SAP HANA'ya bağlıdır. Daha fazla depolama kapasitesi ek bir aylık maliyetle kullanılabilir.

Büyük Örnek damgasının çok kiracılı altyapısı nda, müşteriler yalıtılmış kiracı olarak dağıtılır. Kiracının dağıtımında, Azure kaydınızda bir Azure aboneliği adlarsınız. Bu Azure aboneliği, HANA Büyük Örneği'nin karşı faturalandırıldığıdır. Bu kiracıların Azure aboneliğiyle 1:1 ilişkisi vardır. Bir ağ için, farklı Azure aboneliklerine ait farklı sanal ağlardan bir Azure bölgesinde bir kiracıda dağıtılan bir HANA Büyük Örnek birimine erişmek mümkündür. Bu Azure abonelikleri aynı Azure kaydına ait olmalıdır. 

VM'lerde olduğu gibi, Azure'daki SAP HANA (Büyük Örnekler) birden çok Azure bölgesinde sunulur. Olağanüstü durum kurtarma özellikleri sunmak için, katılmayı seçebilirsiniz. Bir jeo-politik bölgede farklı Büyük Örnek pulları birbirine bağlıdır. Örneğin, ABD Batı ve ABD Doğu HANA Büyük Örnek Damgalar olağanüstü durum kurtarma çoğaltma için özel bir ağ bağlantısı üzerinden bağlanır. 

Azure Sanal Makineleri ile farklı VM türleri arasında seçim yapabileceğiniz gibi, SAP HANA'nın farklı iş yükü türlerine göre uyarlanmış hana büyük örneğinin farklı STU'ları arasından seçim yapabilirsiniz. SAP, Intel işlemci nesillerine göre değişen iş yükleri için bellekten işlemciye-soket oranları uygular. Aşağıdaki tabloda sunulan SKU türleri gösterilmektedir.

[HLI için mevcut SKUs kullanılabilir SKUs](hana-available-skus.md)bulabilirsiniz.

**Sonraki adımlar**
- [SAP HANA (Büyük Örnekler) ağ mimarisine](hana-network-architecture.md) bakın