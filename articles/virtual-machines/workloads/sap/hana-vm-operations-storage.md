---
title: SAP HANA Azure sanal makine depolama yapılandırmaları | Microsoft Dokümanlar
description: SAP HANA'nın dağıtılan VM depolama önerileri.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/10/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c4e8b544ea3daeb23b22f3864beb21ba9d3f342f
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81255626"
---
# <a name="sap-hana-azure-virtual-machine-storage-configurations"></a>SAP HANA Azure sanal makine depolama alanı yapılandırmaları

Azure, SAP HANA çalıştıran Azure VM'leri için uygun olan farklı depolama alanı türleri sağlar. SAP HANA dağıtımları listesi için düşünülebilecek **SAP HANA sertifikalı Azure depolama türleri:** 

- Azure Premium SSD  
- [Ultra disk](https://docs.microsoft.com/azure/virtual-machines/linux/disks-enable-ultra-ssd)
- [Azure NetApp Files](https://azure.microsoft.com/services/netapp/) 

Bu disk türleri hakkında bilgi [Select a disk type](https://docs.microsoft.com/azure/virtual-machines/linux/disks-types) edinmek için bkz.

Azure, Azure Standardı ve Premium Depolama'daki VHD'ler için iki dağıtım yöntemi sunar. Genel senaryo izin verirse, [Azure yönetilen disk](https://azure.microsoft.com/services/managed-disks/) dağıtımlarından yararlanın. 

IOPS ve depolama işlerindeki depolama türlerinin ve Bunların SLA'larının listesi [için yönetilen diskler için Azure belgelerini](https://azure.microsoft.com/pricing/details/managed-disks/)inceleyin.

> [!IMPORTANT]
> Seçilen Azure depolama türünden bağımsız olarak, bu depolamada kullanılan dosya sisteminin belirli işletim sistemi ve DBMS için SAP tarafından desteklenmesi gerekir. [SAP destek notu #405827](https://launchpad.support.sap.com/#/notes/405827) SAP HANA da dahil olmak üzere farklı işletim sistemleri ve veritabanları için desteklenen dosya sistemlerini listeler. Bu, SAP HANA'nın hangi görev için olursa olsun okuma ve yazma için erişebileceği tüm birimler için geçerlidir. SAP HANA için Azure'da Özellikle NFS kullanılarak, bu makalenin ilerleyen sürümlerinde belirtildiği gibi NFS sürümlerinde ek kısıtlamalar uygulanır 


Farklı depolama türleri için minimum SAP HANA onaylı koşullar şunlardır: 

- Azure Premium SSD - /hana/log, Azure [Yazma Hızlandırıcısı](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator)ile önbelleğe alınması gerekmektedir. /hana/veri hacmi Azure Yazma Hızlandırıcısı olmadan Premium SSD'ye veya Ultra diske yerleştirilebilir
- Azure Ultra disk en azından /hana/log hacmi için. /hana/veri hacmi Azure Yazma Hızlandırıcısı olmadan Premium SSD'ye veya ultra diski daha hızlı yeniden başlatma süreleri elde etmek için yerleştirilebilir
- **NFS v4.1** ciltleri Azure NetApp Dosyalarının üstünde /hana/log **ve** /hana/data için kullanılır. /hana/paylaşılan hacmi NFS v3 veya NFS v4.1 protokolünü kullanabilir. NFS v4.1 protokolü /hana/data ve/hana/log hacimleri için zorunludur.

Depolama türlerinden bazıları birleştirilebilir. Örneğin, gerekli düşük gecikme süresini elde etmek için Premium Depolama'ya /hana/veri koymak mümkündür ve /hana/log Ultra disk depolama alanına yerleştirilebilir. Ancak, /hana/data için ANF'ye dayalı bir NFS v4.1 birimi kullanıyorsanız, /hana/log için ANF'ye dayalı başka bir NFS v4.1 hacmi kullanmanız gerekir. Birimlerden biri için ANF'nin üstünde NFS ve diğer birim için Azure Premium Depolama veya Ultra disk (örneğin /hana/log) için NFS kullanılması **desteklenmez.**

Şirket içi dünyada, G/Ç alt sistemlerini ve yeteneklerini nadiren önemsemek zorunda kaldınız. Bunun nedeni, cihaz satıcısının SAP HANA için minimum depolama gereksinimlerinin karşılandıklıolduğundan emin olmasıydı. Azure altyapısını kendiniz oluştururken, sap tarafından verilen bu gereksinimlerden bazılarının farkında olmalısınız. SAP'den gerekli olan minimum iş verme özelliklerinden bazıları aşağıdakileri gerektirmeye neden olur:

- 1 MB G/Ç boyutuna sahip 250 MB/sn'nin **/hana/log'da** okuma/yazma olanağı sağlama
- 16 MB ve 64 MB G/Ç boyutları için **/hana/data** için en az 400 MB/sn okuma etkinliğini etkinleştirin
- 16 MB ve 64 MB G/Ç boyutlarında **/hana/data** için en az 250 MB/sn yazma etkinliğini etkinleştirin

Sap HANA gibi DBMS verileri bellekte tutsa da, düşük depolama gecikmesi DBMS sistemleri için kritik öneme sahip olduğu göz önüne alındığında. Depolamadaki kritik yol genellikle DBMS sistemlerinin işlem günlüğü nün etrafındadır. Ancak, kilitlenme kurtarma sonrasında kayda kaydetme noktaları yazma veya verileri bellekte yükleme gibi işlemler de kritik olabilir. Bu nedenle, Azure Premium **Diskleri'nin /hana/data** ve **/hana/log** hacimleri için yararlanması **zorunludur.** SAP tarafından istenildiği gibi **/hana/log** ve **/hana/data'nın** minimum veri elde etmek için, birden çok Azure Premium Depolama diski üzerinde MDADM veya LVM kullanarak bir RAID 0 oluşturmanız gerekir. RAID birimlerini **/hana/data** ve **/hana/log** birimleri olarak kullanın. 

**Öneri: RAID 0 için şerit boyutları olarak tavsiye kullanmaktır:**

- **/hana/data** için 256 KB
- 32 KB için **/hana/log**

> [!IMPORTANT]
> /hana/veri için şerit boyutu daha yeni Linux sürümleri ile müşteri deneyimlerine dayalı 64 KB veya 128 KB için 256 KB çağıran önceki öneriler değiştirildi. 256 KB boyutu biraz daha iyi performans sağlar

> [!NOTE]
> Azure Premium ve Standart depolama alanı bir VHD'nin üç görüntüsünü tuttuğundan, RAID birimlerini kullanarak herhangi bir artıklık düzeyini yapılandırmanız gerekmez. RAID biriminin kullanımı, yalnızca yeterli G/Ç iş hacmi sağlayan birimleri yapılandırmak içindir.

RAID'in altında çok sayıda Azure VHD biriktirmek, Bir IOPS ve depolama iş birikintisi tarafından biriktir. Bu nedenle, 3 x P30 Azure Premium Depolama diskinin üzerine bir RAID 0 koyarsanız, size IOPS'nin üç katını ve tek bir Azure Premium Depolama P30 diskinin depolama işlerinden üç kat daha fazla depolama çıktısı verecektir.

Ayrıca boyutlandırma veya VM için karar verirken genel VM G/Ç iş bilgililiğini de aklınızda bulundurun. Genel VM depolama iş liği makalede belgelenmiştir [Bellek optimize sanal makine boyutları.](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory)

## <a name="linux-io-scheduler-mode"></a>Linux I/O Zamanlayıcı modu
Linux birkaç farklı G/Ç zamanlama moduna sahiptir. Linux satıcıları ve SAP aracılığıyla ortak öneri, **mq-deadline** veya **kyber** modundan **noop** (çok sıralı olmayan) veya **(çok** sıralı) modu na kadar disk birimleri için G/Ç zamanlayıcı modunu yeniden yapılandırmaktır. Ayrıntılar [SAP Note #1984787'da](https://launchpad.support.sap.com/#/notes/1984787)başvurulmaktadır. 


## <a name="solutions-with-premium-storage-and-azure-write-accelerator-for-azure-m-series-virtual-machines"></a>Azure M Serisi sanal makineler için Premium Depolama ve Azure Yazma Hızlandırıcısı ile çözümler
Azure Yazma Hızlandırıcı, yalnızca Azure M Serisi VM'ler için kullanılabilen bir işlevdir. Adından da belirtildiği gibi, işlevselliğin amacı Azure Premium Depolama'ya karşı yazmaların Gecikme sini artırmaktır. SAP HANA için, Yazma Hızlandırıcısı sadece **/hana/log** hacmine karşı kullanılmalıdır. Bu nedenle, **/hana/veri** ve **/hana/log,** yalnızca **/hana/log** hacmini destekleyen Azure Yazma Hızlandırıcısı ile ayrı birimlerdir. 

> [!IMPORTANT]
> Azure Premium Depolama kullanırken Azure [Yazma Hızlandırıcısı](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator) veya /hana/log biriminin kullanılması zorunludur. Yazma Hızlandırıcı sadece premium Depolama ve M-Serisi ve Mv2 Serisi VM'ler için kullanılabilir.

Aşağıdaki önbelleğe alma önerileri, SAP HANA'nın Aşağıdakiler gibi listedeki G/Ç özelliklerini varsayarak dır:

- HANA veri dosyalarına karşı neredeyse hiç okunan iş yükü yoktur. Özel durumlar, HANA örneğinin yeniden başlatılmasından veya verilerin HANA'ya yüklenmesinden sonra büyük boyutlu G/Ç'lerdir. Veri dosyalarına karşı daha büyük okuma G/Ç başka bir durumda HANA veritabanı yedekleri olabilir. Sonuç olarak çoğu durumda, tüm veri dosyası hacimleri tamamen okunması gerekir, çünkü çoğunlukla önbelleğe alma çoğunlukla mantıklı değildir okuyun.
- Veri dosyalarına karşı yazma HANA savepoints ve HANA kilitlenme kurtarma dayalı patlamalar deneyimlidir. Savepoints yazma asynchronous ve herhangi bir kullanıcı hareketleri tutmayın. Kilitlenme kurtarma sırasında veri yazmak, sistemin yeniden hızlı yanıt vermesini sağlamak için performans açısından önemlidir. Ancak, çarpışma kurtarma oldukça istisnai durumlar olmalıdır
- HANA redo dosyalarından neredeyse hiç okuma yok. Özel durumlar, işlem günlüğü yedeklemeleri, kilitlenme kurtarma veya HANA örneğinin yeniden başlatma aşamasında gerçekleştirirken büyük G/Ç'lerdir.  
- SAP HANA redo günlük dosyasına karşı ana yük yazıyor. İş yükünün doğasına bağlı olarak, 4 KB kadar küçük veya diğer durumlarda 1 MB veya daha fazla G/Ç boyutunda I/Os'lere sahip olabilirsiniz. SAP HANA redo günlüğüne karşı yazma gecikmesi performans açısından önemlidir.
- Tüm yazmagüvenilir bir şekilde disküzerinde kalıcı olması gerekir

**Öneri: SAP HANA tarafından gözlemlenen bu G/Ç desenleri sonucunda, Azure Premium Depolama kullanarak farklı birimlerin önbelleğe alınması aşağıdaki gibi ayarlanmalıdır:**

- **/hana/data** - önbelleğe alma yok
- **/hana/log** - önbelleğe alma yok - M- ve Mv2 Serisi için istisna, Yazma Hızlandırıcısı okunmadan etkinleştirilmelidir. 
- **/hana/shared** - önbelleğe alma oku

### <a name="production-recommended-storage-solution"></a>Üretim tavsiye edilen depolama çözümü

> [!IMPORTANT]
> Azure M Serisi sanal makineleri için SAP HANA sertifikası yalnızca **/hana/log** hacmi için Azure Yazma Hızlandırıcısı'na aittir. Sonuç olarak, Azure M Serisi sanal makinelerdeki üretim senaryosu SAP HANA dağıtımlarının **/hana/log** hacmi için Azure Yazma Hızlandırıcısı ile yapılandırılması beklenmektedir.  

> [!NOTE]
> Üretim senaryoları için, Sap HANA tarafından SAP tarafından [IAAS için SAP belgelerinde](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html)belirli bir VM türünün desteklenip desteklenmediğini kontrol edin.

Öneriler genellikle bu makalede daha önce belirtildiği gibi SAP minimum gereksinimlerini aşıyor. Listelenen öneriler, SAP'nin boyut önerileri ile farklı VM türlerinin sağladığı maksimum depolama alanı verimi arasında bir uzlaşmadır.

**Öneri: Üretim senaryoları için önerilen yapılandırmalar aşağıdaki gibi görünür:**

| VM SKU | RAM | En çok, VM I/O<br /> Aktarım hızı | /hana/veri | /hana/log | /hana/paylaşılan | /kök hacmi | /usr/sap | hana/yedekleme |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 GİB | 500 MB/sN | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M32ls | 256 GiB | 500 MB/sN | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M64ls | 512 GiB | 1000 MB/sN | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P30 |
| M64'ler | 1000 GiB | 1000 MB/sN | 4 x P20 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 GiB | 1000 MB/sN | 3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128'ler | 2000 GİB | 2000 MB/sN |3 x P30 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 2 x P40 |
| M128ms | 3800 GiB | 2000 MB/sN | 5 x P30 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 4 x P40 |
| M208s_v2 | 2850 GİB | 1000 MB/sN | 4 x P30 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 3 x P40 |
| M208ms_v2 | 5700 GiB | 1000 MB/sN | 4 x P40 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 3 x P50 |
| M416s_v2 | 5700 GiB | 2000 MB/sN | 4 x P40 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 3 x P50 |
| M416ms_v2 | 11400 GiB | 2000 MB/sN | 8 x P40 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 4 x P50 |

Önerilen farklı birimler için depolama çıktısının çalıştırmak istediğiniz iş yükünü karşılayıp karşılamadığını denetleyin. İş yükü **/hana/data** ve **/hana/log**için daha yüksek hacim gerektiriyorsa, Azure Premium Depolama VHD'lerinin sayısını artırmanız gerekir. Listelenenden daha fazla VHD'ye sahip bir birim boyutlandırma, Azure sanal makine türü sınırları içinde IOPS ve G/Ç iş hacmini artırır.

Azure Yazma Hızlandırıcıyalnızca [Azure yönetilen disklerle](https://azure.microsoft.com/services/managed-disks/)birlikte çalışır. Bu nedenle en azından **/hana/log** hacmini oluşturan Azure Premium Depolama disklerinin yönetilen diskler olarak dağıtılması gerekir.

Azure Premium Depolama VHD'lerinin VM başına Azure Yazma Hızlandırıcısı tarafından desteklenebilecek sınırları vardır. Geçerli sınırlar şunlardır:

- Bir M128xx ve M416xx VM için 16 VHD
- Bir M64xx ve M208xx VM için 8 VHD
- Bir M32xx VM için 4 VHD

Azure Yazma Hızlandırıcısı'nın nasıl etkinleştirilecek olabileceğine ilişkin daha ayrıntılı yönergeler [Yazma Hızlandırıcı](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator)makalesinde bulunabilir.

Azure Yazma Hızlandırıcısı için ayrıntılar ve kısıtlamalar aynı belgelerde bulunabilir.

**Öneri: /hana/log hacmini oluşturan diskler için Yazma Hızlandırıcı'yı kullanmanız gerekir**


### <a name="cost-conscious-azure-storage-configuration"></a>Maliyet bilincine göre Azure Depolama yapılandırması
Aşağıdaki tablo, müşterilerin Azure VM'lerinde SAP HANA'yı barındırmak için de kullandıkları VM türlerinin yapılandırmasını gösterir. SAP HANA için tüm minimum depolama ölçütlerini karşılamayan veya SAP tarafından SAP HANA ile resmi olarak desteklenmeyen bazı VM türleri olabilir. Ama şimdiye kadar bu VMs olmayan üretim senaryoları için iyi performans gibiydi. **/hana/veri** ve **/hana/log** tek bir birimde birleştirilir. Sonuç olarak Azure Yazma Hızlandırıcısı kullanımı IOPS açısından bir sınırlama haline gelebilir.

> [!NOTE]
> SAP destekli senaryolar için, SAP tarafından SAP tarafından SAP tarafından belirli bir VM türünün desteklenip desteklenmediğini kontrol [edin.](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html)

> [!NOTE]
> Maliyet bilincine yönelik çözüm için eski önerilerden bir değişiklik, [Azure Standart HDD disklerinden](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-hdd) daha iyi performans gösteren ve daha güvenilir [Azure Standart SSD disklerine](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-ssd) geçmektir

Öneriler genellikle bu makalede daha önce belirtildiği gibi SAP minimum gereksinimlerini aşıyor. Listelenen öneriler, SAP'nin boyut önerileri ile farklı VM türlerinin sağladığı maksimum depolama alanı verimi arasında bir uzlaşmadır.

| VM SKU | RAM | En çok, VM I/O<br /> Aktarım hızı | /hana/veri ve /hana/log<br /> LVM veya MDADM ile çizgili | /hana/paylaşılan | /kök hacmi | /usr/sap | hana/yedekleme |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 112 GİB | 768 MB/sN | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E15 |
| E16v3 | 128 GiB | 384 MB/sN | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E15 |
| E32v3 | 256 GiB | 768 MB/sN | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E20 |
| E64v3 | 432 GİB | 1.200 MB/sN | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E30 |
| GS5 | 448 GİB | 2.000 MB/sN | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E30 |
| M32ts | 192 GİB | 500 MB/sN | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E20 |
| M32ls | 256 GiB | 500 MB/sN | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E20 |
| M64ls | 512 GiB | 1.000 MB/sN | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 |1 x E30 |
| M64'ler | 1.000 GiB | 1.000 MB/sN | 2 x P30 | 1 x E30 | 1 x E6 | 1 x E6 |2 x E30 |
| M64ms | 1.750 GiB | 1.000 MB/sN | 3 x P30 | 1 x E30 | 1 x E6 | 1 x E6 | 3 x E30 |
| M128'ler | 2.000 GiB | 2.000 MB/sN |3 x P30 | 1 x E30 | 1 x E10 | 1 x E6 | 2 x E40 |
| M128ms | 3.800 GiB | 2.000 MB/sN | 5 x P30 | 1 x E30 | 1 x E10 | 1 x E6 | 2 x E50 |
| M208s_v2 | 2.850 GiB | 1.000 MB/sN | 4 x P30 | 1 x E30 | 1 x E10 | 1 x E6 |  3 x E40 |
| M208ms_v2 | 5.700 GiB | 1.000 MB/sN | 4 x P40 | 1 x E30 | 1 x E10 | 1 x E6 |  4 x E40 |
| M416s_v2 | 5.700 GiB | 2.000 MB/sN | 4 x P40 | 1 x E30 | 1 x E10 | 1 x E6 |  4 x E40 |
| M416ms_v2 | 11400 GiB | 2.000 MB/sN | 8 x P40 | 1 x E30 | 1 x E10 | 1 x E6 |  4 x E50 |


3 x P20 ile küçük VM türleri için önerilen diskler [SAP TDI Depolama Teknik İnceleme](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html)göre alan önerileri ile ilgili hacimleri büyük boyutu. Ancak, SAP HANA için yeterli disk iş buzunun sağlanması için tabloda görüntülenen seçim yapılmıştır. Bellek hacminin iki katını temsil eden yedeklemeleri tutmak için boyutlandırılmış **olan /hana/yedekleme** biriminde değişikliklere ihtiyacınız varsa, ayarlamaktan çekinmeyin.   
Önerilen farklı birimler için depolama çıktısının çalıştırmak istediğiniz iş yükünü karşılayıp karşılamadığını denetleyin. İş yükü **/hana/data** ve **/hana/log**için daha yüksek hacim gerektiriyorsa, Azure Premium Depolama VHD'lerinin sayısını artırmanız gerekir. Listelenenden daha fazla VHD'ye sahip bir birim boyutlandırma, Azure sanal makine türü sınırları içinde IOPS ve G/Ç iş hacmini artırır. 

> [!NOTE]
> Yukarıdaki yapılandırmalar, Azure Premium Depolama ve Azure Standart Depolama'nın bir karışımını kullandığından [Azure sanal makine tek VM SLA'dan](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) yararlanamaz. Ancak, seçim maliyetleri optimize etmek için seçildi. VM yapılandırmasını Azure tek VM SLA ile uyumlu hale getirmek için Yukarıdaki tüm diskler için Azure Standart Depolama (Sxx) olarak listelenen Tüm Diskler için Premium Depolama'yı seçmeniz gerekir.


> [!NOTE]
> Belirtilen disk yapılandırma önerileri, SAP'nin altyapı sağlayıcılarına karşı ifade ettikleri minimum gereksinimleri hedeflemektedir. Gerçek müşteri dağıtımlarında ve iş yükü senaryolarında, bu önerilerin hala yeterli yetenek sağlamadığı durumlara karşılaşıldı. Bunlar, bir müşterinin HANA yeniden başlatmadan sonra verilerin daha hızlı yeniden yüklenmesini gerektirdiği veya yedekleme yapılandırmalarının depolama alanına daha yüksek bant genişliği gerektirdiği durumlar olabilir. 5000 IOPS'nin belirli iş yükü için yeterli olmadığı diğer durumlar **/hana/log** dahil. Bu nedenle, bu önerileri bir başlangıç noktası olarak alın ve iş yükünün gereksinimlerine göre uyum sağlayabilir.
>  

## <a name="azure-ultra-disk-storage-configuration-for-sap-hana"></a>SAP HANA için Azure Ultra disk depolama yapılandırması
Microsoft, [Azure Ultra disk](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#ultra-disk)adı verilen yeni bir Azure depolama türü kullanıma salma sürecindedir. Şimdiye kadar sunulan Azure depolama ile Ultra disk arasındaki önemli fark, disk özelliklerinin artık disk boyutuna bağlı olmamasıdır. Bir müşteri olarak Ultra disk için bu yetenekleri tanımlayabilirsiniz:

- 4 GiB ile 65.536 GiB arasında değişen bir diskin boyutu
- IOPS aralığı 100 IOPS ile 160K IOPS arasındadır (maksimum vm tiplerine de bağlıdır)
- 300 MB/sn'den 2.000 MB/sn'ye depolama girişi

Ultra disk, boyut, IOPS ve disk iş gücü aralığınızı karşılayan tek bir disk tanımlama olanağı sağlar. IOPS ve depolama iş hacmi gereksinimlerini karşılayan hacimler oluşturmak için Azure Premium Depolama'nın üstünde LVM veya MDADM gibi mantıksal birim yöneticileri kullanmak yerine. Ultra disk ve Premium Depolama arasında bir yapılandırma karışımı çalıştırabilirsiniz. Sonuç olarak, Ultra diskin kullanımını kritik performans /hana/veri ve /hana/log hacimleriyle sınırlandırabilir ve Azure Premium Depolama ile diğer birimleri kapatabilirsiniz

Ultra diskin diğer avantajları, Premium Depolama ile karşılaştırıldığında daha iyi okunabilir gecikme alanı olabilir. HANA başlangıç sürelerini ve sonraki verilerin belleğe yüklenmesini azaltmak istediğinizde daha hızlı okuma gecikmesinin avantajları olabilir. Ultra disk depolamanın avantajları HANA savepoints yazarken de hissedilebilir. /hana/data için Premium Depolama diskleri genellikle Yazma Hızlandırıcı önbelleğe alınmadığından, Premium Depolama'daki /hana/data'ya yazma gecikmesi Ultra diske kıyasla daha yüksektir. Ultra diskli savepoint yazmanın Ultra diskte daha iyi performans sergilemesi beklenebilir.

> [!IMPORTANT]
> Ultra disk henüz tüm Azure bölgelerinde mevcut değildir ve aşağıda listelenen tüm VM türlerini henüz desteklememektedir. Ultra diskin nerede kullanılabildiği ve hangi VM ailelerinin desteklendiği hakkında ayrıntılı bilgi için [Azure'da hangi disk türlerinin kullanılabildiği](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#ultra-disk)makalesine bakın.

### <a name="production-recommended-storage-solution-with-pure-ultra-disk-configuration"></a>Saf Ultra disk yapılandırması ile üretim önerilen depolama çözümü
Bu yapılandırmada , /hana/data ve /hana/log hacimlerini ayrı ayrı saklarsınız. Önerilen değerler, SAP'nin SAP HANA ve depolama yapılandırmaları için SAP [TDI Depolama Teknik Raporu'nda](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html)önerilen VM türlerini onaylamak zorunda olduğu KP'lerden türetilmiştir.

Öneriler genellikle bu makalede daha önce belirtildiği gibi SAP minimum gereksinimlerini aşıyor. Listelenen öneriler, SAP'nin boyut önerileri ile farklı VM türlerinin sağladığı maksimum depolama alanı verimi arasında bir uzlaşmadır.

| VM SKU | RAM | En çok, VM I/O<br /> Aktarım hızı | /hana/veri hacmi | /hana/veri I/O iş | /hana/veri IOPS | /hana/günlük hacmi | /hana/log I/O iş | /hana/log IOPS |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| E64s_v3 | 432 GİB | 1.200 MB/sN | 600 GB | 700 MBps | 7.500 | 512 GB | 500 MBps  | 2.000 |
| M32ts | 192 GİB | 500 MB/sN | 250 GB | 400 MBps | 7.500 | 256 GB | 250 MBps  | 2.000 |
| M32ls | 256 GiB | 500 MB/sN | 300 GB | 400 MBps | 7.500 | 256 GB | 250 MBps  | 2.000 |
| M64ls | 512 GiB | 1.000 MB/sN | 600 GB | 600 MBps | 7.500 | 512 GB | 400 MBps  | 2,500 |
| M64'ler | 1.000 GiB | 1.000 MB/sN |  1.200 GB | 600 MBps | 7.500 | 512 GB | 400 MBps  | 2,500 |
| M64ms | 1.750 GiB | 1.000 MB/sN | 2.100 GB | 600 MBps | 7.500 | 512 GB | 400 MBps  | 2,500 |
| M128'ler | 2.000 GiB | 2.000 MB/sN |2.400 GB | 1.200 MBps |9,000 | 512 GB | 800 MBps  | 3,000 | 
| M128ms | 3.800 GiB | 2.000 MB/sN | 4.800 GB | 1200 MBps |9,000 | 512 GB | 800 MBps  | 3,000 | 
| M208s_v2 | 2.850 GiB | 1.000 MB/sN | 3.500 GB | 1.000 MBps | 9,000 | 512 GB | 400 MBps  | 2,500 | 
| M208ms_v2 | 5.700 GiB | 1.000 MB/sN | 7.200 GB | 1.000 MBps | 9,000 | 512 GB | 400 MBps  | 2,500 | 
| M416s_v2 | 5.700 GiB | 2.000 MB/sN | 7.200 GB | 1.500 MBps | 9,000 | 512 GB | 800 MBps  | 3,000 | 
| M416ms_v2 | 11.400 GiB | 2.000 MB/sN | 14.400 GB | 1.500 MBps | 9,000 | 512 GB | 800 MBps  | 3,000 |   

Listelenen değerlerin bir başlangıç noktası olması amaçlanmıştır ve gerçek taleplere göre değerlendirilmesi gerekir. Azure Ultra diskinin avantajı, IOPS ve iş elde etme değerlerinin VM'yi kapatmaya veya sisteme uygulanan iş yükünü durdurmaya gerek kalmadan uyarlanabilmenizdir.   

> [!NOTE]
> Şimdiye kadar, Ultra disk depolama ile depolama anlık kullanılabilir değil. Bu, Azure Yedekleme Hizmetleri ile VM anlık görüntülerini n kullanımını engeller

### <a name="cost-conscious-storage-solution-with-pure-ultra-disk-configuration"></a>Saf Ultra disk yapılandırması ile maliyet bilinçli depolama çözümü
Bu yapılandırmada, aynı diskteki /hana/data ve /hana/log hacimleri. Önerilen değerler, SAP'nin SAP HANA ve depolama yapılandırmaları için SAP [TDI Depolama Teknik Raporu'nda](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html)önerilen VM türlerini onaylamak zorunda olduğu KP'lerden türetilmiştir. 

Öneriler genellikle bu makalede daha önce belirtildiği gibi SAP minimum gereksinimlerini aşıyor. Listelenen öneriler, SAP'nin boyut önerileri ile farklı VM türlerinin sağladığı maksimum depolama alanı verimi arasında bir uzlaşmadır.

| VM SKU | RAM | En çok, VM I/O<br /> Aktarım hızı | /hana/veri ve /log için birim | /hana/veri ve günlük I/O iş | /hana/veri ve günlük IOPS |
| --- | --- | --- | --- | --- | --- |
| E64s_v3 | 432 GİB | 1.200 MB/sN | 1.200 GB | 1.200 MBps | 9,500 | 
| M32ts | 192 GİB | 500 MB/sN | 512 GB | 400 MBps | 9,500 | 
| M32ls | 256 GiB | 500 MB/sN | 600 GB | 400 MBps | 9,500 | 
| M64ls | 512 GiB | 1.000 MB/sN | 1.100 GB | 900 MBps | 10,000 | 
| M64'ler | 1.000 GiB | 1.000 MB/sN |  1.700 GB | 900 MBps | 10,000 | 
| M64ms | 1.750 GiB | 1.000 MB/sN | 2.600 GB | 900 MBps | 10,000 | 
| M128'ler | 2.000 GiB | 2.000 MB/sN |2.900 GB | 1.800 MBps |12.000 | 
| M128ms | 3.800 GiB | 2.000 MB/sN | 5.300 GB | 1.800 MBps |12.000 |  
| M208s_v2 | 2.850 GiB | 1.000 MB/sN | 4.000 GB | 900 MBps | 10,000 |  
| M208ms_v2 | 5.700 GiB | 1.000 MB/sN | 7.700 GB | 900 MBps | 10,000 | 
| M416s_v2 | 5.700 GiB | 2.000 MB/sN | 7.700 GB | 1.800 MBps | 12.000 |  
| M416ms_v2 | 11.400 GiB | 2.000 MB/sN | 15.000 GB | 1.800 MBps | 12.000 |    

Listelenen değerlerin bir başlangıç noktası olması amaçlanmıştır ve gerçek taleplere göre değerlendirilmesi gerekir. Azure Ultra diskinin avantajı, IOPS ve iş elde etme değerlerinin VM'yi kapatmaya veya sisteme uygulanan iş yükünü durdurmaya gerek kalmadan uyarlanabilmenizdir.  

## <a name="nfs-v41-volumes-on-azure-netapp-files"></a>Azure NetApp Dosyalarında NFS v4.1 ciltleri
Azure NetApp Files, /hana/shared, /hana/data ve /hana/log birimleri için kullanılabilecek yerel NFS paylaşımları sağlar. /hana/data ve /hana/log hacimleri için ANF tabanlı NFS hisselerinin kullanılması v4.1 NFS protokolünün kullanımını gerektirir. NFS protokolü v3, /hana/data ve /hana/log hacimlerinin kullanımı için DESTEKLENMEZ. 

> [!IMPORTANT]
> Azure NetApp Dosyalarında uygulanan NFS v3 protokolü /hana/data ve /hana/log için kullanılmak üzere **desteklenmez.** NFS 4.1'in kullanımı işlevsel bir bakış açısından /hana/data ve /hana/log hacimleri için zorunludur. /hana/paylaşılan hacim için Ise NFS v3 veya NFS v4.1 protokolü işlevsel bir bakış açısıyla kullanılabilir.

### <a name="important-considerations"></a>Önemli noktalar
SAP Netweaver ve SAP HANA için Azure NetApp Dosyalarını değerlendirirken aşağıdaki önemli hususlara dikkat edin:

- Minimum kapasite havuzu 4 TiB'dir.  
- Minimum hacim boyutu 100 GiB'dir
- Azure NetApp Dosyaları ve Azure NetApp Dosyaları birimlerinin monte edildiği tüm sanal makineler aynı Azure Sanal Ağ'da veya aynı bölgedeki [eşlenen sanal ağlarda](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) olmalıdır.  
- Seçili sanal ağın Azure NetApp Dosyaları'na devredilen bir alt ağı olmalıdır.
- Azure NetApp biriminin iş hacmi, [Azure NetApp Dosyaları](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels)için Hizmet düzeyinde belgelenen ses kotası ve Hizmet düzeyinin bir fonksiyonudur. HANA Azure NetApp birimlerini boyutlandırmazken, elde edilen iş hacminin HANA sistem gereksinimlerini karşıladığından emin olun.  
- Azure NetApp Files [dışa aktarma politikası](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-configure-export-policy)sunar: izin verilen istemcileri, erişim türünü (Yalnızca Okuma&, Yalnızca Oku vb.) kontrol edebilirsiniz. 
- Azure NetApp Files özelliği henüz bölge farkında değil. Şu anda Azure NetApp Dosyaları özelliği, bir Azure bölgesindeki tüm Kullanılabilirlik bölgelerinde dağıtılmaz. Bazı Azure bölgelerindeki olası gecikme sonu etkilerine dikkat edin.  
- Düşük gecikme süreyle sanal makinelerin Azure NetApp depolamasına yakın bir yerde dağıtılması önemlidir. 
- <b>Sid</b>adm için Kullanıcı Kimliği ve `sapsys` sanal makinelerdeki Grup Kimliği, Azure NetApp Dosyaları'ndaki yapılandırmayla eşleşmelidir. 

> [!IMPORTANT]
> SAP HANA iş yükleri için düşük gecikme durumu önemlidir. Sanal makinelerin ve Azure NetApp Dosyaları birimlerinin yakın mesafede dağıtıldığından emin olmak için Microsoft temsilcinizle birlikte çalışın.  

> [!IMPORTANT]
> <b>Sid</b>adm için Kullanıcı Kimliği ile Sanal Makine `sapsys` ve Azure NetApp yapılandırması arasında Grup Kimliği arasında bir uyuşmazlık varsa, sanal makinelere monte edilen `nobody`Azure NetApp birimlerindeki dosyaların izinleri görüntülenir. Azure NetApp <b>Files'a</b> [yeni bir sisteme binerken](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxjSlHBUxkJBjmARn57skvdUQlJaV0ZBOE1PUkhOVk40WjZZQVJXRzI2RC4u) sid adm ve Grup Kimliği için `sapsys`doğru Kullanıcı Kimliğini belirttiğinden emin olun.

### <a name="sizing-for-hana-database-on-azure-netapp-files"></a>Azure NetApp Dosyalarında HANA veritabanı için boyutlandırma

Azure NetApp biriminin iş hacmi, [Azure NetApp Dosyaları](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels)için Hizmet düzeyinde belgelenen ses boyutu ve Hizmet düzeyinin bir fonksiyonudur. 

Azure'da SAP altyapısı tasarlarken, SAP'nin aşağıdakiler için minimum iş verme özelliklerine dönüşen bazı minimum depolama iş ortası gereksinimlerine dikkat etmelisiniz:

- 1 MB G/Ç boyutuna sahip 250 MB/sn'nin /hana/log'da okuma/yazma olanağı sağlama  
- 16 MB ve 64 MB G/Ç boyutları için /hana/data için en az 400 MB/sn okuma etkinliğini etkinleştirin  
- 16 MB ve 64 MB G/Ç boyutlarında /hana/data için en az 250 MB/sn yazma etkinliğini etkinleştirin  

[Azure NetApp Dosyaları birim](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels) kotasının 1 TiB'si başına iş hacmi limitleri şunlardır:
- Premium Depolama Katmanı - 64 MiB/s  
- Ultra Depolama Katmanı - 128 MiB/s  

> [!IMPORTANT]
> Tek bir NFS hacminde dağıttığınız kapasiteden bağımsız olarak, iş hacminin, sanal bir makinedeki bir tüketici tarafından yararlanıladığı 1,2-1,4 GB/sn bant genişliği aralığında plato olması beklenir. Bu, ANF teklifinin temel mimarisi ve NFS etrafındaki ilgili Linux oturumu sınırları ile ilgilidir. Azure [NetApp Dosyaları için](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-performance-benchmarks) makalede belgelenen performans ve iş hacmi numaraları, birden çok istemci VM ile paylaşılan bir NFS hacmine ve sonuç olarak birden çok oturumla gerçekleştirildi. Bu senaryo SAP'de ölçtüğük senaryodan farklıdır. Tek bir VM'den elde edilen ib'i NFS hacmine göre ölçtüğüğüz yer. ANF'de barındırılan.

Sap'ın veri ve günlük için minimum iş verme gereksinimlerini `/hana/shared`karşılamak için ve yönergelerine göre önerilen boyutlar aşağıdaki gibi görünür:

| Birim | Boyut<br /> Premium Depolama katmanı | Boyut<br /> Ultra Depolama katmanı | Desteklenen NFS protokolü |
| --- | --- | --- |
| /hana/log/ | 4 TiB | 2 TiB | v4.1 |
| /hana/veri | 6.3 TiB | 3.2 TiB | v4.1 |
| /hana/paylaşılan | 4 işçi düğümü başına maksimum (512 GB, 1xRAM) | 4 işçi düğümü başına maksimum (512 GB, 1xRAM) | v3 veya v4.1 |


> [!NOTE]
> Burada belirtilen Azure NetApp Dosyaları boyutlandırma önerileri, SAP'nin altyapı sağlayıcılarına karşı ifade ettikleri minimum gereksinimleri karşılamayı hedeflemektedir. Gerçek müşteri dağıtımlarında ve iş yükü senaryolarında bu yeterli olmayabilir. Bu önerileri başlangıç noktası olarak kullanın ve belirli iş yükünüzün gereksinimlerini temel alan uyarlayın.  

Bu nedenle, ultra disk depolama için listelenen ANF birimleri için benzer iş hacmi dağıtmayı düşünebilirsiniz. Ayrıca, ultra disk tablolarında zaten yapılan farklı VM SUS'lar için hacimler için listelenen boyutlar için boyutları da göz önünde bulundurun.

> [!TIP]
> Azure NetApp Dosyaları birimlerini, birimlere gerek kalmadan `unmount` dinamik olarak yeniden boyutlandırabilir, sanal makineleri durdurabilir veya SAP HANA'yı durdurabilirsiniz. Bu, uygulamanızın hem beklenen hem de öngörülemeyen üretim taleplerini karşılama esnekliği sağlar.

ANF'de barındırılan NFS v4.1 hacimlerini kullanarak bekleme düğümü ile SAP HANA ölçeklendirme yapılandırması nasıl dağıtılanın, [SUSE Linux Enterprise Server'daki Azure NetApp Dosyaları ile Azure VM'lerde bekleme düğümü ile SAP HANA ölçeğinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse)yayınlanır.


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz.

- [Azure sanal makineleri için SAP HANA Yüksek Kullanılabilirlik kılavuzu.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview)
