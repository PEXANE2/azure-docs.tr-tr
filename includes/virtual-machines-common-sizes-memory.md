---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 08/08/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 3d45defa9ff8e7b2e03d550b76c0e18192c58c4a
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68881612"
---
Bellek için iyileştirilmiş VM boyutları, ilişkisel veritabanı sunucuları, orta ve büyük önbellekler ve bellek içi analizler için harika olan yüksek bir bellek-CPU oranı sunar. Bu makalede, bu gruplandırmadaki her bir boyut için sanal CPU sayısı, veri diskleri ve NIC 'lerin yanı sıra depolama aktarım hızı ve ağ bant genişliği hakkında bilgi sağlanır.

* Ev3-Series, bir hiper iş yükleri için E5-2673 v4 2,3 GHz (çok Iyi) işlemciyi, çoğu genel amaçlı iş yükleri için daha iyi bir değer teklifi sağlar ve Ev3 ' yi diğer bulutların genel amaçlı VM 'Leri ile hizalı hale getiriyor.  Bellek ve ağ sınırları, hiper iş parçacığına geçme ile hizalanmak üzere bir çekirdek temelinde ayarlandığı sürece (7 GiB/vCPU 'dan 8 GiB/vCPU 'ya kadar) bellek genişletilir.  Ev3, D/dv2 ailelerinin yüksek bellek sanal makine boyutlarına göre takip edilir.

* Eav3-Series ve Easv3 serisi, en fazla 256 GB L3 önbelleği olan çok iş parçacıklı bir yapılandırmada AMD 'nin 2.35 GHz EPYıC<sup>TM</sup> 7452v işlemcisini kullanır, bu da en fazla bellek için iyileştirilmiş iş yüklerini çalıştırmaya yönelik seçenekleri artırır.  Eav3-Series ve Easv3 serisi, Ev3 & Esv3 serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

* Mv2-Series, buluttaki herhangi bir VM 'nin en yüksek vCPU sayısını (208 vCPU 'ya kadar) ve en büyük bellek miktarını (5,7 TiB 'ye kadar) sunar. Son derece büyük veritabanları veya yüksek vCPU sayısı ve büyük miktarlarda belleğin yararlı olacağı diğer uygulamalar için idealdir.

* M serisi, yüksek bir vCPU sayısı (128 vCPU kadar) ve büyük miktarda bellek (3,8 TiB 'ye kadar) sunar. Son derece büyük veritabanları veya yüksek sanal CPU sayılarından ve büyük miktarda bellekten faydalanabilir diğer uygulamalar için de idealdir.

* Dv2 serisi, G serisi ve DSv2/GS karşılıkları, daha hızlı vCPU 'Ları, daha iyi geçici depolama performansını talep eden veya daha yüksek bellek taleplerine sahip olan uygulamalar için idealdir. Bu seçenekler birçok kurumsal sınıf uygulama için güçlü bir bileşim sunar.

* Orijinal D Serisinin üzerine geliştirilen Dv2 Serisi, daha güçlü bir CPU'ya sahiptir. Dv2 Serisi CPU, D Serisi CPU'dan yaklaşık %35 daha hızlıdır. Bu, en son nesil 2,4 GHz Intel Xeon® E5-2673 v3 2,4 GHz (Haswell) veya E5-2673 v4 2,3 GHz (çok Iyi) işlemcileri temel alır ve Intel Turbo Boost Technology 2,0 ile, ' 3,1 ye varan bir mimariye geçebilir. Dv2 Serisi, D Serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

* Azure Compute, belirli bir donanım türüyle sınırlanmış ve tek bir müşteriye ayrılmış olan sanal makine boyutları sunar.  Bu sanal makine boyutları, uyumluluk ve düzenleme gereksinimleri gibi öğeler içeren iş yükleri nedeniyle diğer müşterilerden yüksek ölçüde yalıtıma ihtiyaç duyan iş yükleri için idealdir.  Müşteriler Ayrıca, [iç içe geçmiş sanal makineler Için Azure desteği](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)'ni kullanarak bu yalıtılmış sanal makinelerin kaynaklarını daha fazla alt bölümlere ayırmak da tercih edebilir.  Yalıtılmış VM seçenekleriniz için lütfen aşağıdaki sanal makine ailelerinin tablolarına bakın.

## <a name="esv3-series"></a>Esv3 serisi

ACU 160-190 <sup>1</sup>

Premium Depolama:  Desteklenen

Premium depolama önbelleği:  Desteklenen

ESv3 serisi örnekleri, 2,3 GHz Intel XEON ® E5-2673 v4 (Broadwell) işlemciyi temel alır, Intel Turbo Boost Technology 2.0 ile 3,5 GHz hıza çıkabilir ve premium depolama kullanır. Ev3 serisi örnekleri, yoğun bellek kullanımlı kurumsal uygulamalar için idealdir.


| Size             | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS/MBps (GiB 'de önbellek boyutu) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS/MBps | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_E2s_v3 | 2      | 16          | 32             | 4              | 4000/32 (50)                                                       | 3200/48                                | 2 / 1000                                   |
| Standard_E4s_v3&nbsp;<sup>2</sup> | 4      | 32          | 64             | 8              | 8000/64 (100)                                                      | 6400/96                                | 2 / 2000                                   |
| Standard_E8s_v3&nbsp;<sup>2</sup> | 8      | 64          | 128            | 16             | 16000/128 (200)                                                    | 12800/192                              | 4 / 4000                                       |
| Standard_E16s_v3&nbsp;<sup>2</sup> | 16     | 128         | 256            | 32             | 32000/256 (400)                                                    | 25600/384                              | 8 / 8000                                       |
| Standard_E20s_v3                   | 20     | 160         | 320            | 32             | 40000/320 (400)                                                    | 32000/480                              | 8 / 10000                                       |
| Standard_E32s_v3&nbsp;<sup>2</sup> | 32     | 256         | 512            | 32             | 64000/512 (800)                                                    | 51200/768                              | 8 / 16000                             |
| Standard_E48s_v3&nbsp;<sup>2</sup> | 48     | 384         | 768            | 32             | 96000/768 (1200)                                                   | 76800/1152                             | 8 / 24000                             |
| Standard_E64s_v3&nbsp;<sup>2</sup> | 64     | 432         | 864            | 32             | 128000/1024 (1600)                                                   | 80000/1200                             | 8 / 30000                             |
| Standard_E64is_v3&nbsp;<sup>3</sup> | 64     | 432         | 864            | 32             | 128000/1024 (1600)                                                   | 80000/1200                             | 8 / 30000                             |


<sup>1</sup> ESV3 serisi VM 'nin özelliği, hiper Iş parçacığı teknolojisinin Intel®.

<sup>2</sup> kısıtlı çekirdek boyutu var.

<sup>3</sup> örnek, tek bir müşteriye adanmış donanımlar için yalıtılmıştır.

## <a name="easv3-series"></a>Easv3 serisi

Premium Depolama: Desteklenen

Premium depolama önbelleği: Desteklenen

Easv3 serisi Boyutlar, 3.35 GHz 'nin artırılmış önem derecesi elde edilebilir ve Premium Depolama alanını kullanan 2.35 GHz AMD EPIC<sup>TM</sup> 7452v işlemcisini temel alır. Easv3 serisi Boyutlar, bellek açısından yoğun kurumsal uygulamalar için idealdir.

| Size | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD): GiB |
|---|---|---|---|
| Standard_E2as_v3  | 2  | 16  | 32  |
| Standard_E4as_v3  | 4  | 32  | 64  |
| Standard_E8as_v3  | 8  | 64  | 128 |
| Standard_E16as_v3 | 16 | 128 | 256 |
| Standard_E32as_v3 | 32 | 256 | 512 |
| Standard_E48as_v3 | 48 | 384 | 768 |
| Standard_E64as_v3 | 64 | 432 | 864 |

## <a name="ev3-series"></a>Ev3 serisi 

ACU 160-190 <sup>1</sup>

Premium Depolama:  Desteklenmiyor

Premium depolama önbelleği:  Desteklenmiyor

Ev3 serisi örnekleri, 2,3 GHz Intel XEON ® E5-2673 v4 (Broadwell) işlemciyi temel alır ve Intel Turbo Boost Technology 2.0 ile 3,5 GHz hıza çıkabilir. Ev3 serisi örnekleri, yoğun bellek kullanımlı kurumsal uygulamalar için idealdir.

Veri disk depolaması, sanal makinelerden ayrı olarak faturalandırılır. Premium depolama disklerini kullanmak için ESv3 boyutlarını kullanın. ESv3 boyutları için fiyatlandırma ve faturalandırma oranları Ev3 serisi ile aynıdır. 


| Size            | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | En yüksek geçici depolama aktarım hızı: IOPS/okuma MBps/yazma MBps | Maksimum NIC/Ağ bant genişliği |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_E2_v3  | 2         | 16          | 50             | 4              | 3000/46/23                                               | 2 / 1000                 |
| Standard_E4_v3  | 4         | 32          | 100            | 8              | 6000/93/46                                               | 2 / 2000                 |
| Standard_E8_v3  | 8         | 64          | 200            | 16             | 12000/187/93                                             | 4 / 4000                     |
| Standard_E16_v3 | 16        | 128         | 400            | 32             | 24000/375/187                                            | 8 / 8000                     |
| Standard_E20_v3 | 20        | 160         | 500            | 32             | 30000/469/234                                            | 8 / 10000                     |
| Standard_E32_v3 | 32        | 256         | 800            | 32             | 48000/750/375                                            | 8 / 16000                 |
| Standard_E48_v3 | 48        | 384         | 1200            | 32             | 96000/1000/500                                            | 8 / 24000                 |
| Standard_E64_v3 | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30000           |
| Standard_E64i_v3&nbsp;<sup>2,&nbsp;3</sup> | 64        | 432         | 1600           | 32             | 96000/1000/500                                           | 8 / 30000           |

<sup>1</sup> EV3 serisi VM 'nin özelliği, hiper Iş parçacığı teknolojisinin Intel®.

<sup>2</sup> kısıtlı çekirdek boyutu var.

<sup>3</sup> örnek, tek bir müşteriye adanmış donanımlar için yalıtılmıştır.

## <a name="eav3-series"></a>Eav3 serisi

Premium Depolama: Desteklenmiyor

Premium depolama önbelleği: Desteklenmiyor

Eav3 serisi Boyutlar, 3.35 GHz 'nin artırılmış önem derecesi elde edilebilir ve Premium Depolama alanını kullanan 2.35 GHz AMD EPIC<sup>TM</sup> 7452v işlemcisini temel alır. Eav3 serisi Boyutlar, bellek açısından yoğun kurumsal uygulamalar için idealdir. Veri disk depolaması, sanal makinelerden ayrı olarak faturalandırılır. Premium Depolama disklerini kullanmak için Easv3-Series boyutlarını kullanın. Easv3 boyutları için fiyatlandırma ve faturalandırma ölçümleri Eav3 serisi ile aynıdır.

| Size | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD): GiB |
|---|---|---|---|---|---|
| Standard_E2a_v3  | 2  | 16  | 50   |
| Standard_E4a_v3  | 4  | 32  | 100  |
| Standard_E8a_v3  | 8  | 64  | 200  |
| Standard_E16a_v3 | 16 | 128 | 400  |
| Standard_E32a_v3 | 32 | 256 | 800  |
| Standard_E48a_v3 | 48 | 384 | 1200 |
| Standard_E64a_v3 | 64 | 432 | 1600 |

## <a name="mv2-series"></a>Mv2 serisi

Premium Depolama: Desteklenen

Premium depolama önbelleği: Desteklenen

Yazma Hızlandırıcısı: [Destekleniyor](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)

Mv2 serisi yüksek aktarım hızı, düşük gecikme süresi, bir hiper iş parçacıklı Intel® Xeon® Platinum 81805 2.5 GHz (ufuk Gölü) işlemcisi üzerinde çalışan, tüm çekirdek temel sıklığı 2,5 GHz ve en fazla Turbo frekansı 3,8 GHz olan, doğrudan eşlenen yerel NVMe depolama özellikleri. Tüm Mv2 serisi sanal makine boyutları, hem standart hem de Premium kalıcı diskler kullanabilir. Mv2 serisi örnekler, büyük bellek içi veritabanlarını ve iş yüklerini desteklemek için benzersiz işlem performansı sağlayan bellek için iyileştirilmiş VM boyutlarıdır ve ilişkisel veritabanı sunucuları, büyük önbellekler ve bellek içi için ideal olan yüksek bellekle CPU oranıyla desteklenir analiz. 

|Size | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS/MBps (GiB 'de önbellek boyutu) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS/MBps | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|-----------------|------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------|
| Standard_M208ms_v2<sup>1, 2</sup> | 208 | 5700 | 4096 | 64 | 80000/800 (7040) | 40000/1000 | 8 / 16000 |
| Standard_M208s_v2<sup>1, 2</sup> | 208 | 2850 | 4096 | 64 | 80000/800 (7040) | 40000/1000 | 8 / 16000 |

Mv2 serisi VM 'nin özelliği, hiper Iş parçacığı teknolojisinin Intel®  

<sup>1</sup> bu büyük VM 'ler, desteklenen bu konuk işletim sistemlerinden birini gerektirir: Windows Server 2016, Windows Server 2019, SLES 12 SP4, SLES 15.

<sup>2</sup> Mv2 serisi VM 'ler yalnızca 2. kuşak. Linux kullanıyorsanız, SUSE Linux görüntüsünü bulma ve seçme hakkında daha fazla bilgi için aşağıdaki bölüme bakın.

#### <a name="find-a-suse-image"></a>SUSE görüntüsü bulma

Azure portal uygun SUSE Linux görüntüsünü seçmek için: 

1. Azure portal **kaynak oluştur** ' u seçin. 
1. "SUSE SAP" araması yapın 
1. SLES for SAP Generation 2 görüntüleri, Kullandıkça Öde veya kendi aboneliğinizi (BYOS) olarak kullanılabilir. Arama sonuçlarında, istenen görüntü kategorisini genişletin:

    * SAP için SUSE Linux Enterprise Server (SLES)
    * SAP (BYOS) için SUSE Linux Enterprise Server (SLES)
    
1. Mv2-Series ile uyumlu SUSE görüntülerinin adı `GEN2:`ön ekine sahiptir. Aşağıdaki SUSE görüntüleri Mv2 serisi VM 'Ler için kullanılabilir:

    * GEN2 SAP uygulamaları için SUSE Linux Enterprise Server (SLES) 12 SP4
    * GEN2 SAP uygulamaları için SUSE Linux Enterprise Server (SLES) 15
    * GEN2 SAP uygulamaları için SUSE Linux Enterprise Server (SLES) 12 SP4 (BYOS)
    * GEN2 SAP uygulamaları için SUSE Linux Enterprise Server (SLES) 15 (BYOS)

#### <a name="select-a-suse-image-via-azure-cli"></a>Azure CLı aracılığıyla bir SUSE görüntüsü seçin

Mv2 serisi VM 'ler için şu anda kullanılabilir olan SLES for SAP görüntüsünün listesini görmek için aşağıdaki [`az vm image list`](https://docs.microsoft.com/cli/azure/vm/image?view=azure-cli-latest#az-vm-image-list) komutu kullanın:

```azurecli
az vm image list --output table --publisher SUSE --sku gen2 --all
```

Komut, Mv2 serisi VM 'Ler için SUSE 'ten erişilebilen Şu anda kullanılabilir 2. nesil VM 'Leri çıktı olarak verir. 

Örnek çıktı:

```
Offer          Publisher  Sku          Urn                                        Version
-------------  ---------  -----------  -----------------------------------------  ----------
SLES-SAP       SUSE       gen2-12-sp4  SUSE:SLES-SAP:gen2-12-sp4:2019.05.13       2019.05.13
SLES-SAP       SUSE       gen2-15      SUSE:SLES-SAP:gen2-15:2019.05.13           2019.05.13
SLES-SAP-BYOS  SUSE       gen2-12-sp4  SUSE:SLES-SAP-BYOS:gen2-12-sp4:2019.05.13  2019.05.13
SLES-SAP-BYOS  SUSE       gen2-15      SUSE:SLES-SAP-BYOS:gen2-15:2019.05.13      2019.05.13
```

## <a name="m-series"></a>M serisi 

ACU 160-180 <sup>1</sup>

Premium Depolama:  Desteklenen

Premium depolama önbelleği:  Desteklenen

Yazma Hızlandırıcısı:  [Destekleniyor](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)

| Size            | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS/MBps (GiB 'de önbellek boyutu) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS/MBps | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|-----------------|------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------|
| Standard_M8ms&nbsp;<sup>3</sup>    | 8  | 218,75 | 256  | 8  | 10000/100 (793)  | 5000/125 | 4 / 2000 |
| Standard_M16ms&nbsp;<sup>3</sup>   | 16 | 437,5  | 512  | 16 | 20000/200 (1587) | 10000/250 | 8 / 4000 |
| Standard_M32ts | 32 | 192    | 1024 | 32 | 40000/400 (3174) | 20000/500 | 8 / 8000 |
| Standard_M32ls | 32 | 256    | 1024 | 32 | 40000/400 (3174) | 20000/500 | 8 / 8000 |
| Standard_M32ms&nbsp;<sup>3</sup>   | 32 | 875    | 1024 | 32 | 40000/400 (3174) | 20000/500 | 8 / 8000 |
| Standard_M64s  | 64 | 1024   | 2048 | 64 | 80000/800 (6348)| 40000/1000 | 8 / 16000          |
| Standard_M64ls  | 64 | 512    | 2048 | 64 | 80000/800 (6348) | 40000/1000 | 8 / 16000 |
| Standard_M64ms&nbsp;<sup>3</sup>  | 64   | 1792 | 2048 | 64 | 80000/800 (6348)| 40000/1000 | 8 / 16000          |
| Standard_M128s&nbsp;<sup>2</sup> | 128  | 2048        | 4096  | 64 | 160000/1600 (12696) | 80000/2000                            | 8 / 30000          |
| Standard_M128ms&nbsp;<sup>2,&nbsp;3,&nbsp;4</sup> | 128  | 3892  | 4096 | 64 | 160000/1600 (12696) | 80000/2000                            | 8 / 30000          |
| Standard_M64   | 64  | 1024 | 7168  | 64 | 80000/800 (1228) | 40000/1000 | 8 / 16000 |
| Standard_M64m  | 64  | 1792 | 7168  | 64 | 80000/800 (1228) | 40000/1000 | 8 / 16000 |
| Standard_M128&nbsp;2<sup>  | 128 | 2048 | 14336 | 64 | 250000/1600 (2456) | 80000/2000 | 8 / 32000 |
| Standard_M128m&nbsp;2<sup> | 128 | 3892 | 14336 | 64 | 250000/1600 (2456) | 80000/2000 | 8 / 32000 |



<sup>1</sup> mılyon serisi VM 'nin özelliği, hiper Iş parçacığı teknolojisinin Intel®

<sup>2</sup> 64 'Den fazla vCPU, desteklenen bu konuk işletim sistemlerinden birini gerektirir: Windows Server 2016, Ubuntu 16,04 LTS, SLES 12 SP2 ve Red Hat Enterprise Linux, CentOS 7,3 veya Oracle Linux 7,3 with LIS 4.2.1.

<sup>3</sup> kısıtlı çekirdek boyutu var.

<sup>4</sup> örnek, tek bir müşteriye adanmış donanımlar için yalıtılmıştır.
<br>


## <a name="dsv2-series-11-15"></a>DSv2 serisi 11-15

ACU 210-250 <sup>1</sup>

Premium Depolama:  Desteklenen

Premium depolama önbelleği:  Desteklenen

| Size | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS/MBps (GiB 'de önbellek boyutu) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS/MBps | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11_v2&nbsp;<sup>3</sup> |2 |14 |28 |8 |8000/64 (72) |6400/96 |2 / 1500 |
| Standard_DS12_v2&nbsp;<sup>3</sup> |4 |28 |56 |16 |16000/128 (144) |12800/192 |4 / 3000 |
| Standard_DS13_v2&nbsp;<sup>3</sup> |8 |56 |112 |32 |32000/256 (288) |25600/384 |8 / 6000 |
| Standard_DS14_v2&nbsp;<sup>3</sup>|16 |112 |224 |64 |64000/512 (576) |51200/768 |8 / 12000 |
| Standard_DS15_v2&nbsp;<sup>2</sup> |20 |140 |280 |64 |80000/640 (720) |64000/960 |8/25000&nbsp;<sup>4</sup>

<sup>1</sup> DSV2 serisi VM ile olası maksimum disk aktarım hızı (IOPS veya MB/sn), eklenen disklerin sayısı, boyutu ve şeritle sınırlı olabilir.  Ayrıntılar için bkz. [yüksek performans Için tasarlama](../articles/virtual-machines/windows/premium-storage-performance.md).  
<sup>2</sup> örnek, tek bir müşteriye adanmış donanımlar için yalıtılmıştır.  
<sup>3</sup> kısıtlı çekirdek boyutu var.  
Hızlandırılmış ağ ile <sup>4</sup> 25000 Mbps. 

<br>

## <a name="dv2-series-11-15"></a>Dv2 serisi 11-15

ACU 210-250

Premium Depolama:  Desteklenmiyor

Premium depolama önbelleği:  Desteklenmiyor

| Size              | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | En yüksek geçici depolama aktarım hızı: IOPS/okuma MBps/yazma MBps | Maksimum veri diski/aktarım hızı: IOPS | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11_v2   | 2         | 14          | 100            | 6000/93/46                                           | 8/8x500                         | 2 / 1500                     |
| Standard_D12_v2   | 4         | 28          | 200            | 12000/187/93                                         | 16/16x500                         | 4 / 3000                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24000/375/187                                        | 32/32x500                       | 8 / 6000                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48000/750/375                                        | 64/64x500                       | 8 / 12000          |
| Standard_D15_v2&nbsp;<sup>1</sup> | 20        | 140         | 1000          | 60000/937/468                                        | 64/64x500                       | 8/25000&nbsp;<sup>2</sup> |

<sup>1</sup> örnek, tek bir müşteriye adanmış donanımlar için yalıtılmıştır.  
Hızlandırılmış ağ ile <sup>2</sup> 25000 Mbps. 
