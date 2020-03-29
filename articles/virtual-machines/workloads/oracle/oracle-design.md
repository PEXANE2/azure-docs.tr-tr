---
title: Azure'da bir Oracle veritabanı tasarla ve uygula | Microsoft Dokümanlar
description: Azure ortamınızda bir Oracle veritabanı tasarlayın ve uygulayın.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: romitgirdhar
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/02/2018
ms.author: rogirdh
ms.openlocfilehash: c2c2d1a9affe13d485bfeef52c781ed259b53bc8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70100123"
---
# <a name="design-and-implement-an-oracle-database-in-azure"></a>Azure'da bir Oracle veritabanı tasarlama ve uygulama

## <a name="assumptions"></a>Varsayımlar

- Oracle veritabanını şirket içinde Azure'a geçirmeyi planlıyorsunuz.
- Geçirmek istediğiniz Oracle Veritabanı için [Tanılama Paketi'ne](https://docs.oracle.com/cd/E11857_01/license.111/e11987/database_management.htm) sahipsiniz
- Oracle AWR raporlarındaki çeşitli ölçümleri anlayabilirsiniz.
- Uygulama performansı ve platform kullanımı hakkında temel bir anlayışa sahipsiniz.

## <a name="goals"></a>Hedefler

- Azure'da Oracle dağıtımınızı nasıl optimize edin.
- Azure ortamında bir Oracle veritabanı için performans alanınabilirseçeneğini keşfedin.

## <a name="the-differences-between-an-on-premises-and-azure-implementation"></a>Şirket içi ve Azure uygulaması arasındaki farklar 

Şirket içi uygulamaları Azure'a geçirerken aklınızda bulundurmanız gereken bazı önemli şeyler aşağıda verebilirsiniz. 

Önemli bir fark, bir Azure uygulamasında, Sanal Bilgisayarlar, diskler ve sanal ağlar gibi kaynakların diğer istemciler arasında paylaşılsA. Buna ek olarak, kaynaklar gereksinimlere göre daraltılabilir. Azure, başarısızlığı önlemeye (MTBF) odaklanmak yerine, hatadan (MTTR) kurtulmaya daha çok odaklanır.

Aşağıdaki tabloda, şirket içi uygulama ile Oracle veritabanının Azure uygulaması arasındaki bazı farklar listelemektedir.

> 
> |  | **Şirket içi uygulama** | **Azure uygulaması** |
> | --- | --- | --- |
> | **Ağ Oluşturma** |LAN/WAN  |SDN (yazılım tanımlı ağ)|
> | **Güvenlik grubu** |IP/bağlantı noktası kısıtlama araçları |[Ağ Güvenlik Grubu (NSG)](https://azure.microsoft.com/blog/network-security-groups) |
> | **Esnek -lik** |MTBF (hatalar arasındaki ortalama süre) |MTTR (ortalama iyileşme süresi)|
> | **Planlı bakım** |Yama/yükseltmeler|[Kullanılabilirlik kümeleri](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) (Azure tarafından yönetilen yama/yükseltmeler) |
> | **Kaynak** |Ayrılmış  |Diğer istemcilerle paylaşılır|
> | **Bölgeler** |Veri merkezleri |[Bölge çiftleri](https://docs.microsoft.com/azure/virtual-machines/windows/regions#region-pairs)|
> | **Depolama** |SAN/fiziksel diskler |[Azure tarafından yönetilen depolama](https://azure.microsoft.com/pricing/details/managed-disks/?v=17.23h)|
> | **Ölçeklendirme** |Dikey ölçek |Yatay ölçeklendirme|


### <a name="requirements"></a>Gereksinimler

- Veritabanı boyutunu ve büyüme hızını belirleyin.
- Oracle AWR raporlarına veya diğer ağ izleme araçlarına göre tahmin edebilirsiniz IOPS gereksinimlerini belirleyin.

## <a name="configuration-options"></a>Yapılandırma seçenekleri

Azure ortamında performansı artırmak için ayarlayabileceğiniz dört olası alan vardır:

- Sanal makine boyutu
- Ağ iş birlağı
- Disk türleri ve yapılandırmaları
- Disk önbelleği ayarları

### <a name="generate-an-awr-report"></a>AWR raporu oluşturma

Varolan bir Oracle veritabanınız varsa ve Azure'a geçiş yapmayı planlıyorsanız, birkaç seçeneğiniz vardır. Oracle örnekleriniz için [Tanılama Paketi'ne](https://www.oracle.com/technetwork/oem/pdf/511880.pdf) sahipseniz, ölçümleri (IOPS, Mbps, GiBs vb.) almak için Oracle AWR raporunu çalıştırabilirsiniz. Ardından, topladığınız ölçümlere göre VM'yi seçin. Ya da benzer bilgiler almak için altyapı ekibinizle iletişime geçebilirsiniz.

Karşılaştırmayapabilmek için AWR raporunuzu hem düzenli hem de en yoğun iş yüklerinde çalıştırmayı düşünebilirsiniz. Bu raporlara dayanarak, VM'leri ortalama iş yükünü veya maksimum iş yükünü temel alan boyutlandırabilirsiniz.

Aşağıda, awr raporu oluşturmanın bir örneği verilmiştir (Mevcut yüklemenizde varsa, Oracle Enterprise Manager'ınızı kullanarak AWR raporlarınızı oluşturun):

```bash
$ sqlplus / as sysdba
SQL> EXEC DBMS_WORKLOAD_REPOSITORY.CREATE_SNAPSHOT;
SQL> @?/rdbms/admin/awrrpt.sql
```

### <a name="key-metrics"></a>Temel ölçümler

AWR raporundan elde edilebilen ölçümler şunlardır:

- Toplam çekirdek sayısı
- CPU saat hızı
- GB'de toplam bellek
- CPU kullanımı
- En yüksek veri aktarım hızı
- G/Ç değişim oranı (okuma/yazma)
- Redo günlük oranı (MBPs)
- Ağ iş birlağı
- Ağ gecikme hızı (düşük/yüksek)
- GB'de veritabanı boyutu
- SQL*Net üzerinden alınan baytlar istemciden/istemciye

### <a name="virtual-machine-size"></a>Sanal makine boyutu

#### <a name="1-estimate-vm-size-based-on-cpu-memory-and-io-usage-from-the-awr-report"></a>1. AWR raporundan CPU, bellek ve G/Ç kullanımına göre VM boyutunu tahmin edin

Bakabileceğiniz bir şey, sistem darboğazlarının nerede olduğunu gösteren en iyi beş zamanlanmış ön plan olayıdır.

Örneğin, aşağıdaki diyagramda, günlük dosyası eşitleme en üsttedir. LGWR günlük arabelleği redo günlük dosyasına yazmadan önce gerekli bekleme sayısını gösterir. Bu sonuçlar, daha iyi performans depolama veya diskler gerekli olduğunu göstermektedir. Buna ek olarak, diyagram da CPU (çekirdek) sayısını ve bellek miktarını gösterir.

![AWR rapor sayfasının ekran görüntüsü](./media/oracle-design/cpu_memory_info.png)

Aşağıdaki diyagram, okuma ve yazmanın toplam G/Ç'sini gösterir. Rapor sırasında 59 GB okundu ve 247,3 GB yazıldı.

![AWR rapor sayfasının ekran görüntüsü](./media/oracle-design/io_info.png)

#### <a name="2-choose-a-vm"></a>2. Bir VM seçin

AWR raporundan topladığınız bilgilere dayanarak, bir sonraki adım gereksinimlerinizi karşılayan benzer boyutta bir VM seçmektir. Bellek makalesinde kullanılabilir VM'lerin listesini en [iyi duruma getirilmiş](../../linux/sizes-memory.md)olarak bulabilirsiniz.

#### <a name="3-fine-tune-the-vm-sizing-with-a-similar-vm-series-based-on-the-acu"></a>3. ACU'ya dayalı benzer bir VM serisi ile VM boyutlandırmada ince ayar

VM'yi seçtikten sonra, VM için ACU'ya dikkat edin. Gereksinimlerinize daha uygun ACU değerine göre farklı bir VM seçebilirsiniz. Daha fazla bilgi için Azure [bilgi işlem birimine](https://docs.microsoft.com/azure/virtual-machines/windows/acu)bakın.

![ACU birimleri sayfasının ekran görüntüsü](./media/oracle-design/acu_units.png)

### <a name="network-throughput"></a>Ağ iş birlağı

Aşağıdaki diyagram, iş ve IOPS arasındaki ilişkiyi gösterir:

![Elde iş ekranı](./media/oracle-design/throughput.png)

Toplam ağ iş tirobu aşağıdaki bilgilere göre tahmin edilir:
- SQL*Net trafik
- MBps x sunucu sayısı (Oracle Data Guard gibi giden akış)
- Uygulama çoğaltma gibi diğer etkenler

![SQL*Net iş buzunekran görüntüsü](./media/oracle-design/sqlnet_info.png)

Ağ bant genişliği gereksinimlerinize bağlı olarak, aralarından seçim yapabileceğiniz çeşitli ağ geçidi türleri vardır. Bunlar temel, VpnGw ve Azure ExpressRoute'u içerir. Daha fazla bilgi için [VPN ağ geçidi fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/vpn-gateway/?v=17.23h)bakın.

**Öneriler**

- Ağ gecikmesi şirket içi dağıtıma göre daha yüksektir. Ağ turu yolculuklarını azaltmak performansı büyük ölçüde artırabilir.
- Gidiş-dönüşleri azaltmak için, aynı sanal makinede yüksek işlemlere veya "geveze" uygulamalara sahip uygulamaları birleştirin.
- Daha iyi ağ performansı için [Hızlandırılmış Ağ](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli) ile Sanal Makineler kullanın.
- Bazı Linux distrubutions için, [TRIM / UNMAP desteği](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm#trimunmap-support)etkinleştirme düşünün.
- [Oracle Enterprise Manager'ı](https://www.oracle.com/technetwork/oem/enterprise-manager/overview/index.html) ayrı bir Sanal Makineye yükleyin.
- Büyük sayfalar varsayılan olarak linux'ta etkinleştirilemez. Oracle DB'de `use_large_pages = ONLY` büyük sayfaları etkinleştirmeyi ve ayarlamayı düşünün. Bu, performansı artırmaya yardımcı olabilir. Daha fazla bilgiyi [burada](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/refrn/USE_LARGE_PAGES.html#GUID-1B0F4D27-8222-439E-A01D-E50758C88390)bulabilirsiniz.

### <a name="disk-types-and-configurations"></a>Disk türleri ve yapılandırmaları

- *Varsayılan işletim sistemi diskleri*: Bu disk türleri kalıcı veri ve önbelleğe alma sunar. Başlangıçta işletim sistemi erişimi için optimize edilseler ve işlem veya veri ambarı (analitik) iş yükleri için tasarlanmadılar.

- *Yönetilmeyen diskler*: Bu disk türleri ile, Sanal sabit disk (VHD) dosyalarınızı depolayan ve VM disklerinize karşılık gelen depolama hesaplarını yönetirsiniz. VHD dosyaları Azure depolama hesaplarında sayfa blobs olarak depolanır.

- *Yönetilen diskler*: Azure, VM diskleriniz için kullandığınız depolama hesaplarını yönetir. Disk türünü (premium veya standart) ve gereksinim duyduğunuz diskin boyutunu belirtirsiniz. Azure diski sizin için oluşturur ve yönetir.

- *Premium depolama diskleri*: Bu disk türleri üretim iş yükleri için en uygun olanlardır. Premium depolama, DS, DSv2, GS ve F serisi VM'ler gibi belirli boyut serisi VM'lere bağlanabilen VM disklerini destekler. Premium disk farklı boyutlarda dır ve 32 GB ile 4.096 GB arasında değişen diskler arasında seçim yapabilirsiniz. Her disk boyutunun kendi performans belirtimleri vardır. Uygulama gereksinimlerinize bağlı olarak, VM'nize bir veya daha fazla disk ekleyebilirsiniz.

Portaldan yeni bir yönetilen disk oluşturduğunuzda, kullanmak istediğiniz disk türü için **Hesap türünü** seçebilirsiniz. Kullanılabilir tüm disklerin açılır menüde gösterilmediğini unutmayın. Belirli bir VM boyutunu seçtikten sonra, menüde yalnızca bu VM boyutuna dayalı kullanılabilir premium depolama SUS'ları gösterilmektedir.

![Yönetilen disk sayfasının ekran görüntüsü](./media/oracle-design/premium_disk01.png)

Depolama alanınızı bir VM üzerinde yapılandırdıktan sonra, veritabanı oluşturmadan önce diskleri sınamak isteyebilirsiniz. G/Ç oranını hem gecikme sonu hem de üretim açısından bilmek, VM'lerin gecikme hedefleriyle beklenen iş buzunu destekleyip desteklemediğinizi belirlemenize yardımcı olabilir.

Oracle Orion, Sysbench ve Fio gibi uygulama yükü testi için bir dizi araç vardır.

Oracle veritabanını dağıttıktan sonra yük testini yeniden çalıştırın. Düzenli ve en yoğun iş yüklerinizi başlatın ve sonuçlar ortamınızın temelini gösterir.

Depolama boyutundan çok, Depolama Alanı'nın IOPS oranına göre boyutlandırmak daha önemli olabilir. Örneğin, gerekli IOPS 5.000 ise, ancak yalnızca 200 GB'a ihtiyacınız varsa, 200 GB'dan fazla depolama alanı yla birlikte olsa bile P30 sınıfı premium diski alabilirsiniz.

IOPS oranı AWR raporundan elde edilebilir. Redo günlüğüne göre belirlenir, fiziksel okuma ve yazma oranı.

![AWR rapor sayfasının ekran görüntüsü](./media/oracle-design/awr_report.png)

Örneğin, redo boyutu saniyede 12.200.000 bayt, 11,63 MBPs eşittir.
IOPS 12.200.000 / 2.358 = 5.174 olduğunu.

G/Ç gereksinimleri hakkında net bir resim aldıktan sonra, bu gereksinimleri karşılamak için en uygun sürücülerin bir birleşimini seçebilirsiniz.

**Öneriler**

- Veri tablosu alanı için, yönetilen depolama veya Oracle ASM kullanarak G/Ç iş yükünü bir dizi diske yayın.
- Okuma yoğun ve yazma yoğun işlemler için G/Ç blok boyutu arttıkça, daha fazla veri diski ekleyin.
- Büyük sıralı işlemler için blok boyutunu artırın.
- G/Ç'yi azaltmak için veri sıkıştırmayı kullanın (hem veri hem de dizinler için).
- Ayrı redo günlükleri, sistem ve geçici olarak ve ayrı veri disklerinde TS'i geri ala.
- Varsayılan işletim sistemi disklerine (/dev/sda) uygulama dosyaları koymayın. Bu diskler hızlı VM önyükleme süreleri için optimize edilmedi ve uygulamanız için iyi bir performans sağlamayabilir.
- Premium depolamada M-Serisi VM'leri kullanırken, redo günlükleri diskinde [Yazma Hızlandırıcısını](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator) etkinleştirin.

### <a name="disk-cache-settings"></a>Disk önbelleği ayarları

Ana bilgisayar önbelleğe alma için üç seçenek vardır:

- *ReadOnly*: Tüm istekler gelecekteki okumalar için önbelleğe alınır. Tüm yazılar doğrudan Azure Blob depolama alanına kalıcıdır.

- *ReadWrite*: Bu bir "read-ahead" algoritmasıdır. Okuma ve yazma, gelecekteki okumalar için önbelleğe alınır. Yazmayan yazma yazmalar önce yerel önbelleğe kalıcıdır. Ayrıca, ışık iş yükleri için en düşük disk gecikmesi sağlar. VM çöküyorsa, gerekli verileri kalıcı olarak işlemeyen bir uygulamayla ReadWrite önbelleğini kullanmak veri kaybına yol açabilir.

- *Yok* (devre dışı): Bu seçeneği kullanarak önbelleği atlayabilirsiniz. Tüm veriler diske aktarılır ve Azure Depolama'ya sabitlenir. Bu yöntem, yoğun gİş yükleri için en yüksek G/Ç oranını sağlar. Ayrıca "işlem maliyeti"ni de göz önünde bulundurmanız gerekir.

**Öneriler**

Elde edilen imasını en üst düzeye çıkarmak için, ana bilgisayar önbelleğe almak için **None** ile başlamanızı öneririz. Premium Depolama için, dosya sistemini **ReadOnly** veya **None** seçenekleriyle monte ettiğinizde "engelleri" devre dışı kaldırmanız gerektiğini unutmayın. UUID ile /etc/fstab dosyasını disklere güncelleştirin.

![Yönetilen disk sayfasının ekran görüntüsü](./media/oracle-design/premium_disk02.png)

- İşletim sistemi diskleri için varsayılan **Okuma/Yazma** önbelleğe alma'yı kullanın.
- SİsteM, TEMP ve UNDO için **önbelleğe** alma için Yok kullanın.
- DATA için önbelleğe alma için **Yok'u** kullanın. Ancak veritabanınız salt okunur veya okuma yoğunsa, **salt okunur** önbelleğe alma'yı kullanın.

Veri diski ayarınız kaydedildikten sonra, sürücüyü işletim sistemi düzeyinde sökmediğiniz ve değişikliği yaptıktan sonra yeniden monte etmedikçe ana bilgisayar önbelleği ayarını değiştiremezsiniz.

## <a name="security"></a>Güvenlik

Azure ortamınızı kurduktan ve yapılandırıldıktan sonra, bir sonraki adım ağınızı güvence altına almaktır. Aşağıda bazı öneriler veöneriler ve öneriler yer al

- *NSG politikası*: NSG bir alt ağ veya NIC ile tanımlanabilir. Hem güvenlik hem de uygulama güvenlik duvarları gibi şeyler için zorlama yönlendirmesi için alt ağ düzeyinde erişimi denetlemek daha kolaydır.

- *Jumpbox*: Daha güvenli erişim için yöneticiler doğrudan uygulama hizmetine veya veritabanına bağlanmamalıdır. Atlama kutusu, yönetici makinesi ve Azure kaynakları arasında ortam olarak kullanılır.
![Jumpbox topoloji sayfasının ekran görüntüsü](./media/oracle-design/jumpbox.png)

    Yönetici makinesi yalnızca atlama kutusuna IP kısıtlamalı erişim sunmalıdır. Atlama kutusu uygulama ve veritabanına erişimi olmalıdır.

- *Özel ağ* (alt ağlar): Uygulama hizmetinin ve veritabanının ayrı alt ağlarda olmasını öneririz, böylece nsg ilkesi tarafından daha iyi denetim ayarlanabilir.


## <a name="additional-reading"></a>Ek okuma

- [Oracle ASM’yi yapılandırma](configure-oracle-asm.md)
- [Oracle Data Guard’ı yapılandırma](configure-oracle-dataguard.md)
- [Oracle Golden Gate'i yapılandır](configure-oracle-golden-gate.md)
- [Oracle yedekleme ve kurtarma](oracle-backup-recovery.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Yüksek kullanılabilir VM'ler oluşturun](../../linux/create-cli-complete.md)
- [VM dağıtım Azure CLI örneklerini keşfedin](../../linux/cli-samples.md)
