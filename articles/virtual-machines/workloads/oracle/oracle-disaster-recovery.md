---
title: Azure ortamınızda Oracle olağanüstü durum kurtarma senaryosuna genel bakış | Microsoft Dokümanlar
description: Azure ortamınızdaki Bir Oracle Database 12c veritabanı için olağanüstü durum kurtarma senaryosu
services: virtual-machines-linux
documentationcenter: virtual-machines
author: BorisB2015
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/02/2018
ms.author: borisb
ms.openlocfilehash: adf4c54d4f43f806ae985f7c1523303cf574b4c1
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687458"
---
# <a name="disaster-recovery-for-an-oracle-database-12c-database-in-an-azure-environment"></a>Azure ortamında oracle veritabanı 12c veritabanı için olağanüstü durum kurtarma

## <a name="assumptions"></a>Varsayımlar

- Oracle Data Guard tasarımı ve Azure ortamları hakkında bir anlayışa sahipsiniz.


## <a name="goals"></a>Hedefler
- Olağanüstü durum kurtarma (DR) gereksinimlerinizi karşılayan topoloji ve yapılandırmayı tasarlayın.

## <a name="scenario-1-primary-and-dr-sites-on-azure"></a>Senaryo 1: Azure'daki birincil ve DR siteleri

Müşterinin birincil sitede ayarlanmış bir Oracle veritabanı vardır. Dr sitesi farklı bir bölgededir. Müşteri, bu siteler arasında hızlı bir şekilde geri kazanım için Oracle Data Guard'ı kullanır. Birincil site de raporlama ve diğer kullanımlar için ikincil bir veritabanı vardır. 

### <a name="topology"></a>Topoloji

Azure kurulumunun bir özeti aşağıda veda edebilirsiniz:

- İki site (birincil site ve bir DR sitesi)
- İki sanal ağ
- Data Guard (birincil ve bekleme) ile iki Oracle veritabanları
- Golden Gate veya Data Guard içeren iki Oracle veritabanı (yalnızca birincil site)
- İki uygulama hizmetleri, bir birincil ve dr sitesinde bir
- Birincil sitede veritabanı ve uygulama hizmeti için kullanılan bir *kullanılabilirlik kümesi*
- Her sitede, özel ağa erişimi kısıtlayan ve yalnızca bir yönetici tarafından oturum açmaya izin veren bir atlama kutusu
- Ayrı alt ağlarda atlama kutusu, uygulama hizmeti, veritabanı ve VPN ağ geçidi
- NSG uygulama ve veritabanı alt ağları üzerinde zorunlu

![DR topoloji sayfasının ekran görüntüsü](./media/oracle-disaster-recovery/oracle_topology_01.png)

## <a name="scenario-2-primary-site-on-premises-and-dr-site-on-azure"></a>Senaryo 2: Azure'da birincil site ve şirket içi DR sitesi

Bir müşterinin şirket içi Oracle veritabanı kurulumu (birincil site) vardır. Azure'da bir DR sitesi bulunmaktadır. Oracle Data Guard bu siteler arasında hızlı kurtarma için kullanılır. Birincil site de raporlama ve diğer kullanımlar için ikincil bir veritabanı vardır. 

Bu kurulum için iki yaklaşım vardır.

### <a name="approach-1-direct-connections-between-on-premises-and-azure-requiring-open-tcp-ports-on-the-firewall"></a>Yaklaşım 1: Güvenlik duvarında açık TCP bağlantı noktaları gerektiren şirket içi ve Azure arasında doğrudan bağlantılar 

TCP bağlantı noktalarını dış dünyaya maruz kattığı için doğrudan bağlantı önermiyoruz.

#### <a name="topology"></a>Topoloji

Azure kurulumunun bir özeti aşağıda veda edinilir:

- Bir DR sitesi 
- Tek bir sanal ağ
- Data Guard (etkin) ile bir Oracle veritabanı
- DR sitesinde bir uygulama hizmeti
- Özel ağa erişimi kısıtlayan ve yalnızca bir yönetici tarafından oturum açmaya izin veren bir atlama kutusu
- Ayrı alt ağlarda atlama kutusu, uygulama hizmeti, veritabanı ve VPN ağ geçidi
- NSG uygulama ve veritabanı alt ağları üzerinde zorunlu
- Gelen TCP bağlantı noktası 1521'e (veya kullanıcı tanımlı bağlantı noktasına) izin veren bir NSG ilkesi/kuralı
- Sanal ağa erişmek için yalnızca IP adresini/adreslerini (DB veya uygulama) kısıtlamak için bir NSG ilkesi/kuralı

![DR topoloji sayfasının ekran görüntüsü](./media/oracle-disaster-recovery/oracle_topology_02.png)

### <a name="approach-2-site-to-site-vpn"></a>Yaklaşım 2: Siteden siteye VPN
Siteden siteye VPN daha iyi bir yaklaşımdır. VPN kurma hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli)

#### <a name="topology"></a>Topoloji

Azure kurulumunun bir özeti aşağıda veda edinilir:

- Bir DR sitesi 
- Tek bir sanal ağ 
- Data Guard (etkin) ile bir Oracle veritabanı
- DR sitesinde bir uygulama hizmeti
- Özel ağa erişimi kısıtlayan ve yalnızca bir yönetici tarafından oturum açmaya izin veren bir atlama kutusu
- Atlama kutusu, uygulama hizmeti, veritabanı ve VPN ağ geçidi ayrı alt ağlarda
- NSG uygulama ve veritabanı alt ağları üzerinde zorunlu
- Şirket içi ve Azure arasında siteden siteye VPN bağlantısı

![DR topoloji sayfasının ekran görüntüsü](./media/oracle-disaster-recovery/oracle_topology_03.png)

## <a name="additional-reading"></a>Ek okuma

- [Azure'da bir Oracle veritabanı tasarlama ve uygulama](oracle-design.md)
- [Oracle Data Guard’ı yapılandırma](configure-oracle-dataguard.md)
- [Oracle Golden Gate'i yapılandır](configure-oracle-golden-gate.md)
- [Oracle yedekleme ve kurtarma](oracle-backup-recovery.md)


## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Yüksek kullanılabilir VM'ler oluşturun](../../linux/create-cli-complete.md)
- [VM dağıtım Azure CLI örneklerini keşfedin](../../linux/cli-samples.md)
