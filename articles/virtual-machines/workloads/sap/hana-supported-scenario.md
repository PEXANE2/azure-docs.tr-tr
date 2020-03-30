---
title: Azure'da SAP HANA için desteklenen senaryolar (Büyük Örnekler)| Microsoft Dokümanlar
description: Azure'da SAP HANA için desteklenen senaryolar ve mimari ayrıntıları (Büyük Örnekler)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/26/2019
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 019f462d4264d19bcc4806d91223029a95f9d819
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617176"
---
# <a name="supported-scenarios-for-hana-large-instances"></a>HANA Büyük Örnekleri için desteklenen senaryolar
Bu makalede, HANA Büyük Örnekleri (HLI) için desteklenen senaryolar ve mimari ayrıntıları açıklanmaktadır.

>[!NOTE]
>Bu makalede gerekli senaryonuz belirtilmemişse, gereksinimlerinizi değerlendirmek için Microsoft Hizmet Yönetimi ekibine başvurun.
HLI birimini ayarlamadan önce, tasarımı SAP veya hizmet uygulama ortağınızla doğrulayın.

## <a name="terms-and-definitions"></a>Terimler ve tanımlar
Bu makalede kullanılan terimleri ve tanımları anlayalım:

- **SID**: HANA sistemi için bir sistem tanımlayıcısı
- **HLI**: Hana Büyük Örnekleri
- **DR**: Olağanüstü durum kurtarma
- **Normal DR**: Yalnızca DR amaçları için özel bir kaynağa sahip bir sistem kurulumu
- **Çok Amaçlı DR**: Dr olayı için yapılandırılan bir üretim örneğinin yanı sıra üretim dışı bir ortam kullanacak şekilde yapılandırılan DR-site sistemi 
- **Single-SID**: Bir örneği yüklü bir sistem
- **Multi-SID**: Birden fazla örneği yapılandırılmış bir sistem; mcos ortamı olarak da adlandırılır
- **HSR**: SAP HANA Sistem Çoğaltma

## <a name="overview"></a>Genel Bakış
HANA Büyük Örnekler, iş gereksinimlerinizi gerçekleştirmenize yardımcı olmak için çeşitli mimarileri destekler. Aşağıdaki bölümler mimari senaryoları ve bunların yapılandırma ayrıntılarını kapsamaktadır. 

Türetilen mimari tasarım tamamen altyapı açısından dır ve HANA dağıtımı için SAP'ye veya uygulama ortaklarınıza danışmalısınız. Senaryolarınız bu makalede listelenmiyorsa, mimariyi gözden geçirmek ve sizin için bir çözüm elde etmek için Microsoft hesap ekibine başvurun.

> [!NOTE]
> Bu mimariler Özel Veri Tümleştirme (TDI) tasarımıile tamamen uyumludur ve SAP tarafından desteklenir.

Bu makalede, desteklenen her mimaride iki bileşenin ayrıntıları açıklanır:

- Ethernet
- Depolama

### <a name="ethernet"></a>Ethernet

Her sağlanan sunucu, Ethernet arabirimi kümeleriyle önceden yapılandırılmış olarak gelir. Her HLI biriminde yapılandırılan Ethernet arabirimleri dört türe ayrılır:

- **A**: İstemci erişimi için veya istemci erişimi için kullanılır.
- **B**: Düğümden düğüme iletişim için kullanılır. Bu arabirim tüm sunucularda yapılandırılır (istenen topolojiden bağımsız olarak) ancak yalnızca ölçeklendirme senaryoları için kullanılır.
- **C**: Düğümden depolama bağlantısı için kullanılır.
- **D**: STONITH kurulumu için düğüm-iSCSI cihaz bağlantısı için kullanılır. Bu arabirim yalnızca HSR kurulumu istendiğinde yapılandırılır.  

| NIC mantıksal arabirimi | SKU tipi | SUSE OS ile Ad | RHEL OS ile ad | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | TIP I | eth0.kiracı | eno1.kiracı | İstemciden HLI'ya |
| B | TIP I | eth2.kiracı | eno3.kiracı | Düğümden düğüme|
| C | TIP I | eth1.tenant | eno2.kiracı | Düğümden depolamaya |
| D | TIP I | eth4.kiracı | eno4.kiracı | TAŞIT |
| A | TIP II | vlan\<kiracıNo> | team0.tenant | İstemciden HLI'ya |
| B | TIP II | vlan\<kiracıNo+2> | takım0.kiracı+2 | Düğümden düğüme|
| C | TIP II | vlan\<kiracıNo+1> | takım0.kiracı+1 | Düğümden depolamaya |
| D | TIP II | vlan\<kiracıNo+3> | takım0.kiracı+3 | TAŞIT |

Arabirimi, HLI biriminde yapılandırılan topolojiyi temel alın. Örneğin, "B" arabirimi düğümden düğüme iletişim için ayarlanmıştır ve bu da ölçeklendirilmiş bir topolojiniz olduğunda kullanışlıdır. Bu arabirim tek düğüm, ölçeklendirme yapılandırmaları için kullanılmaz. Arabirim kullanımı hakkında daha fazla bilgi için, gerekli senaryoları gözden geçirin (bu makalenin ilerleyen saatlerinde). 

Gerekirse, ek NIC kartlarını kendi olarak tanımlayabilirsiniz. Ancak, varolan NIC'lerin yapılandırmaları *değiştirilemez.*

>[!NOTE]
>Fiziksel arabirimler veya yapıştırma ek arabirimler bulabilirsiniz. Kullanım örneğiniz için yalnızca daha önce bahsedilen arabirimleri göz önünde bulundurmalısınız. Diğerleri göz ardı edilebilir.

İki atanmış IP adresine sahip birimlerin dağılımı aşağıdaki gibi görünmelidir:

- Ethernet "A", Microsoft'a gönderdiğiniz sunucu IP havuzu adres aralığında atanmış bir IP adresine sahip olmalıdır. Bu IP adresi işletim sistemi */etc/hosts* dizininde tutulmalıdır.

- Ethernet "C"nin NFS ile iletişim için kullanılan atanmış bir IP adresi olmalıdır. Bu adresin, kiracı içinde örnek-örnek trafiğine izin vermek için *vb/hosts* dizininde tutulması *gerekmez.*

HANA Sistem Çoğaltma veya HANA ölçekli dağıtım için, iki atanmış IP adresine sahip bir bıçak yapılandırması uygun değildir. Yalnızca iki atanmış IP adresiniz varsa ve böyle bir yapılandırmadağıtmak istiyorsanız, Azure Hizmet Yönetimi'nde SAP HANA ile iletişime geçin. Üçüncü bir VLAN'da size üçüncü bir IP adresi atayabilirler. Üç NIC bağlantı noktası üzerinde üç atanmış IP adresi bulunan HANA Büyük Örnekler birimleri için aşağıdaki kullanım kuralları geçerlidir:

- Ethernet "A", Microsoft'a gönderdiğiniz sunucu IP havuzu adres aralığının dışında atanmış bir IP adresine sahip olmalıdır. Bu IP adresi işletim sistemi *vb / hosts* dizininde muhafaza edilmemelidir.

- Ethernet "B" çeşitli örnekler arasındaki iletişim için *sadece etc/hosts* dizininde muhafaza edilmelidir. Bunlar, HANA'nın düğümler arası yapılandırma için kullandığı IP adresleri olarak ölçeklendirilmiş HANA yapılandırmalarında tutulması gereken IP adresleridir.

- Ethernet "C"nin NFS depolamasına iletişim için kullanılan atanmış bir IP adresi olmalıdır. Bu tür *adresler vb/hosts* dizininde tutulamaz.

- Ethernet "D" sadece Pacemaker için STONITH cihazlarına erişim için kullanılmalıdır. HANA Sistem Çoğaltma yapılandırma ve SBD tabanlı bir aygıt kullanarak işletim sisteminin otomatik arıza elde etmek istediğinizde bu arabirim gereklidir.


### <a name="storage"></a>Depolama
Depolama, istenen topolojiye göre önceden yapılandırılmıştır. Birim boyutları ve montaj noktaları sunucu sayısına, SNU sayısına ve yapılandırılan topolojiye bağlı olarak değişir. Daha fazla bilgi için, gerekli senaryoları gözden geçirin (bu makalenin ilerleyen saatlerinde). Daha fazla depolama alanına ihtiyacınız varsa, 1-TB artışlarla satın alabilirsiniz.

>[!NOTE]
>Montaj noktası /usr/sap/\<SID> /hana/paylaşılan montaj noktasına sembolik bir bağlantıdır.


## <a name="supported-scenarios"></a>Desteklenen senaryolar

Sonraki bölümlerdeki mimari diyagramlarda aşağıdaki gösterimler kullanılır:

[![Mimari diyagramlar tablosu](media/hana-supported-scenario/Legends.png)](media/hana-supported-scenario/Legends.png#lightbox)

Desteklenen senaryolar şunlardır:

* Bir SID ile tek düğüm
* Tek düğüm MCOS
* DR ile tek düğüm (normal)
* DR ile tek düğüm (çok amaçlı)
* STONITH ile HSR
* DR ile HSR (normal/çok amaçlı) 
* Ana bilgisayar otomatik arıza (1+1) 
* Bekleme ile ölçeklendirme
* Bekleme olmadan ölçeklendirme
* DR ile ölçeklendirme

## <a name="single-node-with-one-sid"></a>Bir SID ile tek düğüm

Bu topoloji, bir SID ile ölçeklendirme yapılandırmasında ki tek düğümü destekler.

### <a name="architecture-diagram"></a>Mimari diyagramı  

![Bir SID ile tek düğüm](media/hana-supported-scenario/Single-node-with-one-SID.png)

### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri önceden yapılandırılmıştır:

| NIC mantıksal arabirimi | SKU tipi | SUSE OS ile Ad | RHEL OS ile ad | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | TIP I | eth0.kiracı | eno1.kiracı | İstemciden HLI'ya |
| B | TIP I | eth2.kiracı | eno3.kiracı | Yapılandırılan ancak kullanımda olmayan |
| C | TIP I | eth1.tenant | eno2.kiracı | Düğümden depolamaya |
| D | TIP I | eth4.kiracı | eno4.kiracı | Yapılandırılan ancak kullanımda olmayan |
| A | TIP II | vlan\<kiracıNo> | team0.tenant | İstemciden HLI'ya |
| B | TIP II | vlan\<kiracıNo+2> | takım0.kiracı+2 | Yapılandırılan ancak kullanımda olmayan |
| C | TIP II | vlan\<kiracıNo+1> | takım0.kiracı+1 | Düğümden depolamaya |
| D | TIP II | vlan\<kiracıNo+3> | takım0.kiracı+3 | Yapılandırılan ancak kullanımda olmayan |

### <a name="storage"></a>Depolama
Aşağıdaki montaj noktaları önceden yapılandırılmıştır:

| Montaj noktası | Kullanım örneği | 
| --- | --- |
|/hana/paylaşılan/SID | HANA kurulumu | 
|/hana/veri/SID/mnt00001 | Veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | Günlük dosyaları yükleme | 
|/hana/logbackups/SID | Redo günlükleri |

### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/sap/SID , /hana/shared/SID için sembolik bir bağlantıdır.

## <a name="single-node-mcos"></a>Tek düğüm MCOS

Bu topoloji, birden çok SIT'li ölçeklenmiş yapılandırmadaki bir düğümü destekler.

### <a name="architecture-diagram"></a>Mimari diyagramı  

![Tek düğüm MCOS](media/hana-supported-scenario/single-node-mcos.png)

### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri önceden yapılandırılmıştır:

| NIC mantıksal arabirimi | SKU tipi | SUSE OS ile Ad | RHEL OS ile ad | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | TIP I | eth0.kiracı | eno1.kiracı | İstemciden HLI'ya |
| B | TIP I | eth2.kiracı | eno3.kiracı | Yapılandırılan ancak kullanımda olmayan |
| C | TIP I | eth1.tenant | eno2.kiracı | Düğümden depolamaya |
| D | TIP I | eth4.kiracı | eno4.kiracı | Yapılandırılan ancak kullanımda olmayan |
| A | TIP II | vlan\<kiracıNo> | team0.tenant | İstemciden HLI'ya |
| B | TIP II | vlan\<kiracıNo+2> | takım0.kiracı+2 | Yapılandırılan ancak kullanımda olmayan |
| C | TIP II | vlan\<kiracıNo+1> | takım0.kiracı+1 | Düğümden depolamaya |
| D | TIP II | vlan\<kiracıNo+3> | takım0.kiracı+3 | Yapılandırılan ancak kullanımda olmayan |

### <a name="storage"></a>Depolama
Aşağıdaki montaj noktaları önceden yapılandırılmıştır:

| Montaj noktası | Kullanım örneği | 
| --- | --- |
|/hana/paylaşılan/SID1 | SID1 için HANA kurulumu | 
|/hana/veri/SID1/mnt00001 | SID1 için veri dosyaları yükleme | 
|/hana/log/SID1/mnt00001 | SID1 için günlük dosyaları yükleme | 
|/hana/logbackups/SID1 | SID1 için Redo günlükleri |
|/hana/paylaşılan/SID2 | SID2 için HANA kurulumu | 
|/hana/veri/SID2/mnt00001 | SID2 için veri dosyaları yükleme | 
|/hana/log/SID2/mnt00001 | SID2 için günlük dosyaları yükleme | 
|/hana/logbackups/SID2 | SID2 için Redo günlükleri |

### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/sap/SID , /hana/shared/SID için sembolik bir bağlantıdır.
- Birim boyutu dağılımı bellekteki veritabanı boyutuna dayanır. Bellekteki veritabanı boyutlarının hangilerinin çok SID ortamında destekleniyi öğrenmek için [bkz.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)

## <a name="single-node-with-dr-using-storage-replication"></a>Depolama çoğaltma kullanarak DR ile tek düğüm
 
Bu topoloji, bir veya birden çok SID ile ölçeklenmiş yapılandırmada bir düğümü destekler ve birincil SID için DR sitesine depolama tabanlı çoğaltma sağlar. Diyagramda, birincil sitede yalnızca tek bir SID sistemi gösterilmiştir, ancak MCOS sistemleri de desteklenir.

### <a name="architecture-diagram"></a>Mimari diyagramı  

![Depolama çoğaltma kullanarak DR ile tek düğüm](media/hana-supported-scenario/Single-node-with-dr.png)

### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri önceden yapılandırılmıştır:

| NIC mantıksal arabirimi | SKU tipi | SUSE OS ile Ad | RHEL OS ile ad | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | TIP I | eth0.kiracı | eno1.kiracı | İstemciden HLI'ya |
| B | TIP I | eth2.kiracı | eno3.kiracı | Yapılandırılan ancak kullanımda olmayan |
| C | TIP I | eth1.tenant | eno2.kiracı | Düğümden depolamaya |
| D | TIP I | eth4.kiracı | eno4.kiracı | Yapılandırılan ancak kullanımda olmayan |
| A | TIP II | vlan\<kiracıNo> | team0.tenant | İstemciden HLI'ya |
| B | TIP II | vlan\<kiracıNo+2> | takım0.kiracı+2 | Yapılandırılan ancak kullanımda olmayan |
| C | TIP II | vlan\<kiracıNo+1> | takım0.kiracı+1 | Düğümden depolamaya |
| D | TIP II | vlan\<kiracıNo+3> | takım0.kiracı+3 | Yapılandırılan ancak kullanımda olmayan |

### <a name="storage"></a>Depolama
Aşağıdaki montaj noktaları önceden yapılandırılmıştır:

| Montaj noktası | Kullanım örneği | 
| --- | --- |
|/hana/paylaşılan/SID | SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | SID için günlük dosyaları yükleme | 
|/hana/logbackups/SID | SID için Redo günlükleri |


### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/sap/SID , /hana/shared/SID için sembolik bir bağlantıdır.
- MCOS için: Birim boyutu dağılımı bellekteki veritabanı boyutuna dayanır. Bellekteki veritabanı boyutlarının hangilerinin çok SID ortamında destekleniyi öğrenmek için [bkz.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- DR yerinde: Dr HLI ünitesindeki hana örnek kurulum için hacimler ve montaj noktaları yapılandırılır (HANA kurulumu için gerekli olarak işaretlenmiştir). 
- DR sitesinde: Veriler, günlük yedeklemeleri ve paylaşılan birimler ("Depolama Çoğaltma" olarak işaretlenir) üretim sitesinden anlık görüntü yoluyla çoğaltılır. Bu birimler yalnızca başarısız sırasında monte edilir. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery)
- *SKU Tip I sınıfının* önyükleme hacmi DR düğümüne kopyalanır.


## <a name="single-node-with-dr-multipurpose-using-storage-replication"></a>Depolama çoğaltma kullanarak DR (çok amaçlı) ile tek düğüm
 
Bu topoloji, bir veya birden çok SID ile ölçeklenmiş yapılandırmada bir düğümü destekler ve birincil SID için DR sitesine depolama tabanlı çoğaltma sağlar. Diyagramda, birincil sitede yalnızca tek bir SID sistemi gösterilmiştir, ancak multi-SID (MCOS) sistemleri de desteklenir. DR yerinde, üretim işlemleri birincil siteden çalışırken Qa örneği için HLI birimi kullanılır. DR failover (veya failover testi) sırasında, DR yerindeki QA örneği aşağı alınır.

### <a name="architecture-diagram"></a>Mimari diyagramı  

![Depolama çoğaltma kullanarak DR (çok amaçlı) ile tek düğüm](media/hana-supported-scenario/single-node-with-dr-multipurpose.png)

### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri önceden yapılandırılmıştır:

| NIC mantıksal arabirimi | SKU tipi | SUSE OS ile Ad | RHEL OS ile ad | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | TIP I | eth0.kiracı | eno1.kiracı | İstemciden HLI'ya |
| B | TIP I | eth2.kiracı | eno3.kiracı | Yapılandırılan ancak kullanımda olmayan |
| C | TIP I | eth1.tenant | eno2.kiracı | Düğümden depolamaya |
| D | TIP I | eth4.kiracı | eno4.kiracı | Yapılandırılan ancak kullanımda olmayan |
| A | TIP II | vlan\<kiracıNo> | team0.tenant | İstemciden HLI'ya |
| B | TIP II | vlan\<kiracıNo+2> | takım0.kiracı+2 | Yapılandırılan ancak kullanımda olmayan |
| C | TIP II | vlan\<kiracıNo+1> | takım0.kiracı+1 | Düğümden depolamaya |
| D | TIP II | vlan\<kiracıNo+3> | takım0.kiracı+3 | Yapılandırılan ancak kullanımda olmayan |

### <a name="storage"></a>Depolama
Aşağıdaki montaj noktaları önceden yapılandırılmıştır:

| Montaj noktası | Kullanım örneği | 
| --- | --- |
|**Birincil sitede**|
|/hana/paylaşılan/SID | ÜRETIM SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | Üretim SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | Üretim SID için günlük dosyaları yükleme | 
|/hana/logbackups/SID | Üretim SID için Redo günlükleri |
|**DR sitesinde**|
|/hana/paylaşılan/SID | ÜRETIM SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | Üretim SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | Üretim SID için günlük dosyaları yükleme | 
|/hana/paylaşılan/QA-SID | QA SID için HANA kurulumu | 
|/hana/veri/QA-SID/mnt00001 | QA SID için veri dosyaları yükleme | 
|/hana/log/QA-SID/mnt00001 | QA SID için günlük dosyaları yükleme |
|/hana/logbackups/QA-SID | QA SID için redo günlükleri |

### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/sap/SID , /hana/shared/SID için sembolik bir bağlantıdır.
- MCOS için: Birim boyutu dağılımı bellekteki veritabanı boyutuna dayanır. Bellekteki veritabanı boyutlarının hangilerinin çok SID ortamında destekleniyi öğrenmek için [bkz.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- DR yerinde: Dr HLI ünitesindeki hana örnek kurulum için hacimler ve montaj noktaları yapılandırılır (HANA kurulumu için gerekli olarak işaretlenmiştir). 
- DR sitesinde: Veriler, günlük yedeklemeleri ve paylaşılan birimler ("Depolama Çoğaltma" olarak işaretlenir) üretim sitesinden anlık görüntü yoluyla çoğaltılır. Bu birimler yalnızca başarısız sırasında monte edilir. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery) 
- DR sitesinde: QA için veri, günlük yedeklemeleri, günlük ve paylaşılan birimler ("QA örnek yüklemesi" olarak işaretlenmiştir) QA örnek yüklemesi için yapılandırılır.
- *SKU Tip I sınıfının* önyükleme hacmi DR düğümüne kopyalanır.

## <a name="hsr-with-stonith-for-high-availability"></a>Yüksek kullanılabilirlik için STONITH ile HSR
 
Bu topoloji HANA Sistem Çoğaltma yapılandırması için iki düğüm destekler. Bu yapılandırma yalnızca düğümdeki tek HANA örnekleri için desteklenir. Bu, MCOS senaryolarının *desteklenmediğini* anlamına gelir.

> [!NOTE]
> Aralık 2019 itibariyle bu mimari yalnızca SUSE işletim sistemi için desteklenir.


### <a name="architecture-diagram"></a>Mimari diyagramı  

![Yüksek kullanılabilirlik için STONITH ile HSR](media/hana-supported-scenario/HSR-with-STONITH.png)



### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri önceden yapılandırılmıştır:

| NIC mantıksal arabirimi | SKU tipi | SUSE OS ile Ad | RHEL OS ile ad | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | TIP I | eth0.kiracı | eno1.kiracı | İstemciden HLI'ya |
| B | TIP I | eth2.kiracı | eno3.kiracı | Yapılandırılan ancak kullanımda olmayan |
| C | TIP I | eth1.tenant | eno2.kiracı | Düğümden depolamaya |
| D | TIP I | eth4.kiracı | eno4.kiracı | STONITH için kullanılır |
| A | TIP II | vlan\<kiracıNo> | team0.tenant | İstemciden HLI'ya |
| B | TIP II | vlan\<kiracıNo+2> | takım0.kiracı+2 | Yapılandırılan ancak kullanımda olmayan |
| C | TIP II | vlan\<kiracıNo+1> | takım0.kiracı+1 | Düğümden depolamaya |
| D | TIP II | vlan\<kiracıNo+3> | takım0.kiracı+3 | STONITH için kullanılır |

### <a name="storage"></a>Depolama
Aşağıdaki montaj noktaları önceden yapılandırılmıştır:

| Montaj noktası | Kullanım örneği | 
| --- | --- |
|**Birincil düğümde**|
|/hana/paylaşılan/SID | ÜRETIM SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | Üretim SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | Üretim SID için günlük dosyaları yükleme | 
|/hana/logbackups/SID | Üretim SID için Redo günlükleri |
|**İkincil düğümde**|
|/hana/paylaşılan/SID | İkincil SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | İkincil SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | İkincil SID için günlük dosyaları yükleme | 
|/hana/logbackups/SID | İkincil SID için redo günlükleri |

### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/sap/SID , /hana/shared/SID için sembolik bir bağlantıdır.
- MCOS için: Birim boyutu dağılımı bellekteki veritabanı boyutuna dayanır. Bellekteki veritabanı boyutlarının hangilerinin çok SID ortamında destekleniyi öğrenmek için [bkz.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- STONITH: STONITH kurulumu için bir SBD yapılandırılır. Ancak, STONITH kullanımı isteğe bağlıdır.


## <a name="high-availability-with-hsr-and-dr-with-storage-replication"></a>Depolama çoğaltma ile HSR ve DR ile yüksek kullanılabilirlik
 
Bu topoloji HANA Sistem Çoğaltma yapılandırması için iki düğüm destekler. Hem normal hem de çok amaçlı DR desteklenir. Bu yapılandırmalar yalnızca düğümüzerindeki tek HANA örnekleri için desteklenir. Bu, MCOS senaryolarının bu yapılandırmalarla *desteklenmediğini* anlamına gelir.

Diyagramda, üretim işlemleri birincil siteden çalışırken HLI biriminin QA örneği için kullanıldığı DR yerinde çok amaçlı bir senaryo gösterilmiştir. DR failover (veya failover testi) sırasında, DR yerindeki QA örneği aşağı alınır. 

### <a name="architecture-diagram"></a>Mimari diyagramı  

![Depolama çoğaltma ile HSR ve DR ile yüksek kullanılabilirlik](media/hana-supported-scenario/HSR-with-DR.png)

### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri önceden yapılandırılmıştır:

| NIC mantıksal arabirimi | SKU tipi | SUSE OS ile Ad | RHEL OS ile ad | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | TIP I | eth0.kiracı | eno1.kiracı | İstemciden HLI'ya |
| B | TIP I | eth2.kiracı | eno3.kiracı | Yapılandırılan ancak kullanımda olmayan |
| C | TIP I | eth1.tenant | eno2.kiracı | Düğümden depolamaya |
| D | TIP I | eth4.kiracı | eno4.kiracı | STONITH için kullanılır |
| A | TIP II | vlan\<kiracıNo> | team0.tenant | İstemciden HLI'ya |
| B | TIP II | vlan\<kiracıNo+2> | takım0.kiracı+2 | Yapılandırılan ancak kullanımda olmayan |
| C | TIP II | vlan\<kiracıNo+1> | takım0.kiracı+1 | Düğümden depolamaya |
| D | TIP II | vlan\<kiracıNo+3> | takım0.kiracı+3 | STONITH için kullanılır |

### <a name="storage"></a>Depolama
Aşağıdaki montaj noktaları önceden yapılandırılmıştır:

| Montaj noktası | Kullanım örneği | 
| --- | --- |
|**Birincil sitedeki birincil düğümde**|
|/hana/paylaşılan/SID | ÜRETIM SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | Üretim SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | Üretim SID için günlük dosyaları yükleme | 
|/hana/logbackups/SID | Üretim SID için Redo günlükleri |
|**Birincil sitedeki ikincil düğümde**|
|/hana/paylaşılan/SID | İkincil SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | İkincil SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | İkincil SID için günlük dosyaları yükleme | 
|/hana/logbackups/SID | İkincil SID için redo günlükleri |
|**DR sitesinde**|
|/hana/paylaşılan/SID | ÜRETIM SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | Üretim SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | Üretim SID için günlük dosyaları yükleme | 
|/hana/paylaşılan/QA-SID | QA SID için HANA kurulumu | 
|/hana/veri/QA-SID/mnt00001 | QA SID için veri dosyaları yükleme | 
|/hana/log/QA-SID/mnt00001 | QA SID için günlük dosyaları yükleme |
|/hana/logbackups/QA-SID | QA SID için redo günlükleri |

### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/sap/SID , /hana/shared/SID için sembolik bir bağlantıdır.
- MCOS için: Birim boyutu dağılımı bellekteki veritabanı boyutuna dayanır. Bellekteki veritabanı boyutlarının hangilerinin çok SID ortamında destekleniyi öğrenmek için [bkz.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- STONITH: STONITH kurulumu için bir SBD yapılandırılır. Ancak, STONITH kullanımı isteğe bağlıdır.
- DR sitesinde: Birincil ve ikincil düğüm çoğaltma için *iki depolama hacmi kümesi gereklidir.*
- DR yerinde: Dr HLI ünitesindeki hana örnek kurulum için hacimler ve montaj noktaları yapılandırılır (HANA kurulumu için gerekli olarak işaretlenmiştir). 
- DR sitesinde: Veriler, günlük yedeklemeleri ve paylaşılan birimler ("Depolama Çoğaltma" olarak işaretlenir) üretim sitesinden anlık görüntü yoluyla çoğaltılır. Bu birimler yalnızca başarısız sırasında monte edilir. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery) 
- DR sitesinde: QA için veri, günlük yedeklemeleri, günlük ve paylaşılan birimler ("QA örnek yüklemesi" olarak işaretlenmiştir) QA örnek yüklemesi için yapılandırılır.
- *SKU Tip I sınıfının* önyükleme hacmi DR düğümüne kopyalanır.


## <a name="host-auto-failover-11"></a>Ana bilgisayar otomatik arıza (1+1)
 
Bu topoloji, ana bilgisayar otomatik hata yapılandırmasında iki düğümü destekler. Bir ana/işçi rolü olan bir düğüm ve bekleme olarak başka bir düğüm vardır. *SAP bu senaryoyu yalnızca S/4 HANA için destekler.* Daha fazla bilgi için [OSS note 2408419 - SAP S/4HANA - Multi-Node Desteği'ne](https://launchpad.support.sap.com/#/notes/2408419)bakın.



### <a name="architecture-diagram"></a>Mimari diyagramı  

![Ana bilgisayar otomatik arıza (1+1)](media/hana-supported-scenario/scaleup-with-standby.png)

### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri önceden yapılandırılmıştır:

| NIC mantıksal arabirimi | SKU tipi | SUSE OS ile Ad | RHEL OS ile ad | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | TIP I | eth0.kiracı | eno1.kiracı | İstemciden HLI'ya |
| B | TIP I | eth2.kiracı | eno3.kiracı | Düğümden düğüme iletişim |
| C | TIP I | eth1.tenant | eno2.kiracı | Düğümden depolamaya |
| D | TIP I | eth4.kiracı | eno4.kiracı | Yapılandırılan ancak kullanımda olmayan |
| A | TIP II | vlan\<kiracıNo> | team0.tenant | İstemciden HLI'ya |
| B | TIP II | vlan\<kiracıNo+2> | takım0.kiracı+2 | Düğümden düğüme iletişim |
| C | TIP II | vlan\<kiracıNo+1> | takım0.kiracı+1 | Düğümden depolamaya |
| D | TIP II | vlan\<kiracıNo+3> | takım0.kiracı+3 | Yapılandırılan ancak kullanımda olmayan |

### <a name="storage"></a>Depolama
Aşağıdaki montaj noktaları önceden yapılandırılmıştır:

| Montaj noktası | Kullanım örneği | 
| --- | --- |
|**Ana ve bekleme düğümlerinde**|
|/hana/paylaşılan | ÜRETIM SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | Üretim SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | Üretim SID için günlük dosyaları yükleme | 
|/hana/logbackups/SID | Üretim SID için Redo günlükleri |



### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/sap/SID , /hana/shared/SID için sembolik bir bağlantıdır.
- Bekleme de: Bekleme ünitesindeki HANA örnek kurulumu için hacimler ve montaj noktaları yapılandırılır ("HANA kurulumu için gerekli" olarak işaretlenir).
 

## <a name="scale-out-with-standby"></a>Bekleme ile ölçeklendirme
 
Bu topoloji, ölçeklendirme yapılandırmasında birden çok düğümü destekler. Ana rolü olan bir düğüm, işçi rolü olan bir veya daha fazla düğüm ve bekleme olarak bir veya daha fazla düğüm vardır. Ancak, zaman içinde herhangi bir noktada yalnızca bir ana düğüm olabilir.


### <a name="architecture-diagram"></a>Mimari diyagramı  

![Bekleme ile ölçeklendirme](media/hana-supported-scenario/scaleout-nm-standby.png)

### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri önceden yapılandırılmıştır:

| NIC mantıksal arabirimi | SKU tipi | SUSE OS ile Ad | RHEL OS ile ad | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | TIP I | eth0.kiracı | eno1.kiracı | İstemciden HLI'ya |
| B | TIP I | eth2.kiracı | eno3.kiracı | Düğümden düğüme iletişim |
| C | TIP I | eth1.tenant | eno2.kiracı | Düğümden depolamaya |
| D | TIP I | eth4.kiracı | eno4.kiracı | Yapılandırılan ancak kullanımda olmayan |
| A | TIP II | vlan\<kiracıNo> | team0.tenant | İstemciden HLI'ya |
| B | TIP II | vlan\<kiracıNo+2> | takım0.kiracı+2 | Düğümden düğüme iletişim |
| C | TIP II | vlan\<kiracıNo+1> | takım0.kiracı+1 | Düğümden depolamaya |
| D | TIP II | vlan\<kiracıNo+3> | takım0.kiracı+3 | Yapılandırılan ancak kullanımda olmayan |

### <a name="storage"></a>Depolama
Aşağıdaki montaj noktaları önceden yapılandırılmıştır:

| Montaj noktası | Kullanım örneği | 
| --- | --- |
|**Ana, işçi ve bekleme düğümlerinde**|
|/hana/paylaşılan | ÜRETIM SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | Üretim SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | Üretim SID için günlük dosyaları yükleme | 
|/hana/logbackups/SID | Üretim SID için Redo günlükleri |


## <a name="scale-out-without-standby"></a>Bekleme olmadan ölçeklendirme
 
Bu topoloji, ölçeklendirme yapılandırmasında birden çok düğümü destekler. Ana rolü olan bir düğüm ve işçi rolü olan bir veya daha fazla düğüm vardır. Ancak, zaman içinde herhangi bir noktada yalnızca bir ana düğüm olabilir.


### <a name="architecture-diagram"></a>Mimari diyagramı  

![Bekleme olmadan ölçeklendirme](media/hana-supported-scenario/scaleout-nm.png)


### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri önceden yapılandırılmıştır:

| NIC mantıksal arabirimi | SKU tipi | SUSE OS ile Ad | RHEL OS ile ad | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | TIP I | eth0.kiracı | eno1.kiracı | İstemciden HLI'ya |
| B | TIP I | eth2.kiracı | eno3.kiracı | Düğümden düğüme iletişim |
| C | TIP I | eth1.tenant | eno2.kiracı | Düğümden depolamaya |
| D | TIP I | eth4.kiracı | eno4.kiracı | Yapılandırılan ancak kullanımda olmayan |
| A | TIP II | vlan\<kiracıNo> | team0.tenant | İstemciden HLI'ya |
| B | TIP II | vlan\<kiracıNo+2> | takım0.kiracı+2 | Düğümden düğüme iletişim |
| C | TIP II | vlan\<kiracıNo+1> | takım0.kiracı+1 | Düğümden depolamaya |
| D | TIP II | vlan\<kiracıNo+3> | takım0.kiracı+3 | Yapılandırılan ancak kullanımda olmayan |

### <a name="storage"></a>Depolama
Aşağıdaki montaj noktaları önceden yapılandırılmıştır:

| Montaj noktası | Kullanım örneği | 
| --- | --- |
|**Ana ve işçi düğümleri üzerinde**|
|/hana/paylaşılan | ÜRETIM SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | Üretim SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | Üretim SID için günlük dosyaları yükleme | 
|/hana/logbackups/SID | Üretim SID için Redo günlükleri |


### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/sap/SID , /hana/shared/SID için sembolik bir bağlantıdır.

## <a name="scale-out-with-dr-using-storage-replication"></a>Depolama çoğaltma kullanarak DR ile ölçeklendirme
 
Bu topoloji, dr ile bir ölçek-out birden fazla düğüm leri destekler. Hem normal hem de çok amaçlı DR desteklenir. Diyagramda, yalnızca tek amaçlı DR tasvir edilir. Bu topolojiyi bekleme düğümü ile veya bekleme düğümü olmadan talep edebilirsiniz.


### <a name="architecture-diagram"></a>Mimari diyagramı  

![Depolama çoğaltma kullanarak DR ile ölçeklendirme](media/hana-supported-scenario/scaleout-with-dr.png)


### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri önceden yapılandırılmıştır:

| NIC mantıksal arabirimi | SKU tipi | SUSE OS ile Ad | RHEL OS ile ad | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | TIP I | eth0.kiracı | eno1.kiracı | İstemciden HLI'ya |
| B | TIP I | eth2.kiracı | eno3.kiracı | Düğümden düğüme iletişim |
| C | TIP I | eth1.tenant | eno2.kiracı | Düğümden depolamaya |
| D | TIP I | eth4.kiracı | eno4.kiracı | Yapılandırılan ancak kullanımda olmayan |
| A | TIP II | vlan\<kiracıNo> | team0.tenant | İstemciden HLI'ya |
| B | TIP II | vlan\<kiracıNo+2> | takım0.kiracı+2 | Düğümden düğüme iletişim |
| C | TIP II | vlan\<kiracıNo+1> | takım0.kiracı+1 | Düğümden depolamaya |
| D | TIP II | vlan\<kiracıNo+3> | takım0.kiracı+3 | Yapılandırılan ancak kullanımda olmayan |

### <a name="storage"></a>Depolama
Aşağıdaki montaj noktaları önceden yapılandırılmıştır:

| Montaj noktası | Kullanım örneği | 
| --- | --- |
|**Birincil düğümde**|
|/hana/paylaşılan | ÜRETIM SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | Üretim SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | Üretim SID için günlük dosyaları yükleme | 
|/hana/logbackups/SID | Üretim SID için Redo günlükleri |
|**DR düğümünde**|
|/hana/paylaşılan | ÜRETIM SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | Üretim SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | Üretim SID için günlük dosyaları yükleme | 


### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/sap/SID , /hana/shared/SID için sembolik bir bağlantıdır.
-  DR yerinde: Dr HLI ünitesindeki hana örnek kurulum için hacimler ve montaj noktaları yapılandırılır (HANA kurulumu için gerekli olarak işaretlenmiştir). 
- DR sitesinde: Veriler, günlük yedeklemeleri ve paylaşılan birimler ("Depolama Çoğaltma" olarak işaretlenir) üretim sitesinden anlık görüntü yoluyla çoğaltılır. Bu birimler yalnızca başarısız sırasında monte edilir. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery) 
- *SKU Tip I sınıfının* önyükleme hacmi DR düğümüne kopyalanır.


## <a name="single-node-with-dr-using-hsr"></a>HSR kullanarak DR ile tek düğüm
 
Bu topoloji, bir SID ile ölçeklendirme yapılandırmasında bir düğümü destekler, birincil SID için DR sitesine HANA Sistem Çoğaltma ile. Diyagramda, birincil sitede yalnızca tek bir SID sistemi gösterilmiştir, ancak multi-SID (MCOS) sistemleri de desteklenir.

### <a name="architecture-diagram"></a>Mimari diyagramı  

![HSR kullanarak DR ile tek düğüm](media/hana-supported-scenario/single-node-hsr-dr-111.png)

### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri önceden yapılandırılmıştır:

| NIC mantıksal arabirimi | SKU tipi | SUSE OS ile Ad | RHEL OS ile ad | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | TIP I | eth0.kiracı | eno1.kiracı | İstemciden HLI/HSR'ye |
| B | TIP I | eth2.kiracı | eno3.kiracı | Yapılandırılan ancak kullanımda olmayan |
| C | TIP I | eth1.tenant | eno2.kiracı | Düğümden depolamaya |
| D | TIP I | eth4.kiracı | eno4.kiracı | Yapılandırılan ancak kullanımda olmayan |
| A | TIP II | vlan\<kiracıNo> | team0.tenant | İstemciden HLI/HSR'ye |
| B | TIP II | vlan\<kiracıNo+2> | takım0.kiracı+2 | Yapılandırılan ancak kullanımda olmayan |
| C | TIP II | vlan\<kiracıNo+1> | takım0.kiracı+1 | Düğümden depolamaya |
| D | TIP II | vlan\<kiracıNo+3> | takım0.kiracı+3 | Yapılandırılan ancak kullanımda olmayan |

### <a name="storage"></a>Depolama
Aşağıdaki montaj noktaları hem HLI birimlerinde (Birincil ve DR) önceden yapılandırılmıştır:

| Montaj noktası | Kullanım örneği | 
| --- | --- |
|/hana/paylaşılan/SID | SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | SID için günlük dosyaları yükleme | 
|/hana/logbackups/SID | SID için Redo günlükleri |


### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/sap/SID , /hana/shared/SID için sembolik bir bağlantıdır.
- MCOS için: Birim boyutu dağılımı bellekteki veritabanı boyutuna dayanır. Bellekteki veritabanı boyutlarının hangilerinin çok SID ortamında destekleniyi öğrenmek için [bkz.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- Birincil düğüm HANA Sistem Çoğaltma kullanarak DR düğümü ile eşitler. 
- [Global Reach,](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach) bölgesel ağlarınız arasında özel bir ağ yapmak için ExpressRoute devrelerini birbirine bağlamak için kullanılır.



## <a name="single-node-hsr-to-dr-cost-optimized"></a>Dr tek düğüm HSR (maliyet optimize) 
 
 Bu topoloji, bir SID ile ölçeklendirme yapılandırmasında bir düğümü destekler, birincil SID için DR sitesine HANA Sistem Çoğaltma ile. Diyagramda, birincil sitede yalnızca tek bir SID sistemi gösterilmiştir, ancak multi-SID (MCOS) sistemleri de desteklenir. DR sitesinde, üretim işlemleri birincil siteden çalışırken QA örneği için bir HLI birimi kullanılır. DR failover (veya failover testi) sırasında, DR yerindeki QA örneği aşağı alınır.

### <a name="architecture-diagram"></a>Mimari diyagramı  

![Dr tek düğüm HSR (maliyet optimize)](media/hana-supported-scenario/single-node-hsr-dr-cost-optimized-121.png)

### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri önceden yapılandırılmıştır:

| NIC mantıksal arabirimi | SKU tipi | SUSE OS ile Ad | RHEL OS ile ad | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | TIP I | eth0.kiracı | eno1.kiracı | İstemciden HLI/HSR'ye |
| B | TIP I | eth2.kiracı | eno3.kiracı | Yapılandırılan ancak kullanımda olmayan |
| C | TIP I | eth1.tenant | eno2.kiracı | Düğümden depolamaya |
| D | TIP I | eth4.kiracı | eno4.kiracı | Yapılandırılan ancak kullanımda olmayan |
| A | TIP II | vlan\<kiracıNo> | team0.tenant | İstemciden HLI/HSR'ye |
| B | TIP II | vlan\<kiracıNo+2> | takım0.kiracı+2 | Yapılandırılan ancak kullanımda olmayan |
| C | TIP II | vlan\<kiracıNo+1> | takım0.kiracı+1 | Düğümden depolamaya |
| D | TIP II | vlan\<kiracıNo+3> | takım0.kiracı+3 | Yapılandırılan ancak kullanımda olmayan |

### <a name="storage"></a>Depolama
Aşağıdaki montaj noktaları önceden yapılandırılmıştır:

| Montaj noktası | Kullanım örneği | 
| --- | --- |
|**Birincil sitede**|
|/hana/paylaşılan/SID | ÜRETIM SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | Üretim SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | Üretim SID için günlük dosyaları yükleme | 
|/hana/logbackups/SID | Üretim SID için Redo günlükleri |
|**DR sitesinde**|
|/hana/paylaşılan/SID | ÜRETIM SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | Üretim SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | Üretim SID için günlük dosyaları yükleme | 
|/hana/logbackups/SID | Üretim SID için Redo günlükleri |
|/hana/paylaşılan/QA-SID | QA SID için HANA kurulumu | 
|/hana/veri/QA-SID/mnt00001 | QA SID için veri dosyaları yükleme | 
|/hana/log/QA-SID/mnt00001 | QA SID için günlük dosyaları yükleme |
|/hana/logbackups/QA-SID | QA SID için redo günlükleri |

### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/sap/SID , /hana/shared/SID için sembolik bir bağlantıdır.
- MCOS için: Birim boyutu dağılımı bellekteki veritabanı boyutuna dayanır. Bellekteki veritabanı boyutlarının hangilerinin çok SID ortamında destekleniyi öğrenmek için [bkz.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture)
- DR yerinde: Hacimler ve montaj noktaları DR HLI ünitesinde üretim HANA örnek kurulumu için yapılandırılır (DR yerinde PROD Örneği olarak işaretlenmiştir). 
- DR sitesinde: QA için veri, günlük yedeklemeleri, günlük ve paylaşılan birimler ("QA örnek yüklemesi" olarak işaretlenmiştir) QA örnek yüklemesi için yapılandırılır.
- Birincil düğüm HANA Sistem Çoğaltma kullanarak DR düğümü ile eşitler. 
- [Global Reach,](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach) bölgesel ağlarınız arasında özel bir ağ yapmak için ExpressRoute devrelerini birbirine bağlamak için kullanılır.

## <a name="high-availability-and-disaster-recovery-with-hsr"></a>HSR ile yüksek kullanılabilirlik ve olağanüstü durum kurtarma 
 
 Bu topoloji, yerel bölgelerin yüksek kullanılabilirliği için HANA Sistem Çoğaltma yapılandırması için iki düğüm destekler. DR için, DR bölgesindeki üçüncü düğüm HSR (async modu) kullanarak birincil siteyle eşitler. 

### <a name="architecture-diagram"></a>Mimari diyagramı  

![HSR ile yüksek kullanılabilirlik ve olağanüstü durum kurtarma](media/hana-supported-scenario/hana-system-replication-dr-131.png)

### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri önceden yapılandırılmıştır:

| NIC mantıksal arabirimi | SKU tipi | SUSE OS ile Ad | RHEL OS ile ad | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | TIP I | eth0.kiracı | eno1.kiracı | İstemciden HLI/HSR'ye |
| B | TIP I | eth2.kiracı | eno3.kiracı | Yapılandırılan ancak kullanımda olmayan |
| C | TIP I | eth1.tenant | eno2.kiracı | Düğümden depolamaya |
| D | TIP I | eth4.kiracı | eno4.kiracı | Yapılandırılan ancak kullanımda olmayan |
| A | TIP II | vlan\<kiracıNo> | team0.tenant | İstemciden HLI/HSR'ye |
| B | TIP II | vlan\<kiracıNo+2> | takım0.kiracı+2 | Yapılandırılan ancak kullanımda olmayan |
| C | TIP II | vlan\<kiracıNo+1> | takım0.kiracı+1 | Düğümden depolamaya |
| D | TIP II | vlan\<kiracıNo+3> | takım0.kiracı+3 | Yapılandırılan ancak kullanımda olmayan |

### <a name="storage"></a>Depolama
Aşağıdaki montaj noktaları önceden yapılandırılmıştır:

| Montaj noktası | Kullanım örneği | 
| --- | --- |
|**Birincil sitede**|
|/hana/paylaşılan/SID | ÜRETIM SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | Üretim SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | Üretim SID için günlük dosyaları yükleme | 
|/hana/logbackups/SID | Üretim SID için Redo günlükleri |
|**DR sitesinde**|
|/hana/paylaşılan/SID | ÜRETIM SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | Üretim SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | Üretim SID için günlük dosyaları yükleme | 
|/hana/logbackups/SID | Üretim SID için Redo günlükleri |


### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/sap/SID , /hana/shared/SID için sembolik bir bağlantıdır.
- DR yerinde: Hacimler ve montaj noktaları DR HLI ünitesindeki hana örnek kurulumu için yapılandırılır ("PROD DR örneği" olarak işaretlenir). 
- Birincil site düğümü HANA Sistem Çoğaltma kullanarak DR düğümü ile eşitler. 
- [Global Reach,](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach) bölgesel ağlarınız arasında özel bir ağ yapmak için ExpressRoute devrelerini birbirine bağlamak için kullanılır.

## <a name="high-availability-and-disaster-recovery-with-hsr-cost-optimized"></a>HSR ile yüksek kullanılabilirlik ve olağanüstü durum kurtarma (maliyet optimize edilmiş)
 
 Bu topoloji, yerel bölgelerin yüksek kullanılabilirliği için HANA Sistem Çoğaltma yapılandırması için iki düğümü destekler. DR için, DR bölgesindeki üçüncü düğüm HSR (async modu) kullanarak birincil siteyle eşitlenirken, başka bir örnek (örneğin, QA) dr düğümünden zaten tükeniyor. 

### <a name="architecture-diagram"></a>Mimari diyagramı  

![HSR ile yüksek kullanılabilirlik ve olağanüstü durum kurtarma (maliyet optimize edilmiş)](media/hana-supported-scenario/hana-system-replication-dr-cost-optimized-141.png)

### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri önceden yapılandırılmıştır:

| NIC mantıksal arabirimi | SKU tipi | SUSE OS ile Ad | RHEL OS ile ad | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | TIP I | eth0.kiracı | eno1.kiracı | İstemciden HLI/HSR'ye |
| B | TIP I | eth2.kiracı | eno3.kiracı | Yapılandırılan ancak kullanımda olmayan |
| C | TIP I | eth1.tenant | eno2.kiracı | Düğümden depolamaya |
| D | TIP I | eth4.kiracı | eno4.kiracı | Yapılandırılan ancak kullanımda olmayan |
| A | TIP II | vlan\<kiracıNo> | team0.tenant | İstemciden HLI/HSR'ye |
| B | TIP II | vlan\<kiracıNo+2> | takım0.kiracı+2 | Yapılandırılan ancak kullanımda olmayan |
| C | TIP II | vlan\<kiracıNo+1> | takım0.kiracı+1 | Düğümden depolamaya |
| D | TIP II | vlan\<kiracıNo+3> | takım0.kiracı+3 | Yapılandırılan ancak kullanımda olmayan |

### <a name="storage"></a>Depolama
Aşağıdaki montaj noktaları önceden yapılandırılmıştır:

| Montaj noktası | Kullanım örneği | 
| --- | --- |
|**Birincil sitede**|
|/hana/paylaşılan/SID | ÜRETIM SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | Üretim SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | Üretim SID için günlük dosyaları yükleme | 
|/hana/logbackups/SID | Üretim SID için Redo günlükleri |
|**DR sitesinde**|
|/hana/paylaşılan/SID | ÜRETIM SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | Üretim SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | Üretim SID için günlük dosyaları yükleme | 
|/hana/logbackups/SID | Üretim SID için Redo günlükleri |
|/hana/paylaşılan/QA-SID | QA SID için HANA kurulumu | 
|/hana/veri/QA-SID/mnt00001 | QA SID için veri dosyaları yükleme | 
|/hana/log/QA-SID/mnt00001 | QA SID için günlük dosyaları yükleme |
|/hana/logbackups/QA-SID | QA SID için redo günlükleri |

### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/sap/SID , /hana/shared/SID için sembolik bir bağlantıdır.
- DR yerinde: Hacimler ve montaj noktaları DR HLI ünitesindeki hana örnek kurulumu için yapılandırılır ("PROD DR örneği" olarak işaretlenir). 
- DR sitesinde: QA için veri, günlük yedeklemeleri, günlük ve paylaşılan birimler ("QA örnek yüklemesi" olarak işaretlenmiştir) QA örnek yüklemesi için yapılandırılır.
- Birincil site düğümü HANA Sistem Çoğaltma kullanarak DR düğümü ile eşitler. 
- [Global Reach,](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach) bölgesel ağlarınız arasında özel bir ağ yapmak için ExpressRoute devrelerini birbirine bağlamak için kullanılır.

## <a name="scale-out-with-dr-using-hsr"></a>HSR kullanarak DR ile ölçeklendirme
 
Bu topoloji, dr ile bir ölçek-out birden fazla düğüm leri destekler. Bu topolojiyi bekleme düğümü ile veya bekleme düğümü olmadan talep edebilirsiniz. Birincil site düğümü HANA Sistem Çoğaltma (async modu) kullanarak DR site düğümü ile eşitler.


### <a name="architecture-diagram"></a>Mimari diyagramı  

[![HSR kullanarak DR ile ölçeklendirme](media/hana-supported-scenario/scale-out-dr-hsr-151.png)](media/hana-supported-scenario/scale-out-dr-hsr-151.png#lightbox)


### <a name="ethernet"></a>Ethernet
Aşağıdaki ağ arabirimleri önceden yapılandırılmıştır:

| NIC mantıksal arabirimi | SKU tipi | SUSE OS ile Ad | RHEL OS ile ad | Kullanım örneği|
| --- | --- | --- | --- | --- |
| A | TIP I | eth0.kiracı | eno1.kiracı | İstemciden HLI/HSR'ye |
| B | TIP I | eth2.kiracı | eno3.kiracı | Düğümden düğüme iletişim |
| C | TIP I | eth1.tenant | eno2.kiracı | Düğümden depolamaya |
| D | TIP I | eth4.kiracı | eno4.kiracı | Yapılandırılan ancak kullanımda olmayan |
| A | TIP II | vlan\<kiracıNo> | team0.tenant | İstemciden HLI/HSR'ye |
| B | TIP II | vlan\<kiracıNo+2> | takım0.kiracı+2 | Düğümden düğüme iletişim |
| C | TIP II | vlan\<kiracıNo+1> | takım0.kiracı+1 | Düğümden depolamaya |
| D | TIP II | vlan\<kiracıNo+3> | takım0.kiracı+3 | Yapılandırılan ancak kullanımda olmayan |

### <a name="storage"></a>Depolama
Aşağıdaki montaj noktaları önceden yapılandırılmıştır:

| Montaj noktası | Kullanım örneği | 
| --- | --- |
|**Birincil düğümde**|
|/hana/paylaşılan | ÜRETIM SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | Üretim SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | Üretim SID için günlük dosyaları yükleme | 
|/hana/logbackups/SID | Üretim SID için Redo günlükleri |
|**DR düğümünde**|
|/hana/paylaşılan | ÜRETIM SID için HANA kurulumu | 
|/hana/veri/SID/mnt00001 | Üretim SID için veri dosyaları yükleme | 
|/hana/log/SID/mnt00001 | Üretim SID için günlük dosyaları yükleme | 
|/hana/logbackups/SID | Üretim SID için Redo günlükleri |


### <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular
- /usr/sap/SID , /hana/shared/SID için sembolik bir bağlantıdır.
- DR yerinde: Hacimler ve montaj noktaları DR HLI ünitesinde üretim HANA örnek kurulumu için yapılandırılmıştır. 
- Birincil site düğümü HANA Sistem Çoğaltma kullanarak DR düğümü ile eşitler. 
- [Global Reach,](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach) bölgesel ağlarınız arasında özel bir ağ yapmak için ExpressRoute devrelerini birbirine bağlamak için kullanılır.


## <a name="next-steps"></a>Sonraki adımlar
- HANA Büyük Örnekleri için [altyapı ve bağlantı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-infrastructure-connectivity)
- HANA Büyük Örnekleri için [yüksek kullanılabilirlik ve olağanüstü durum kurtarma](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery)
