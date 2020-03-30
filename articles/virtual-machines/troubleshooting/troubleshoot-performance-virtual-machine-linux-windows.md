---
title: Linux veya Windows'da Azure sanal makine performansını giderin
description: Bu makalede, izleme ve denetimdarlıkları gözlemleme yoluyla sanal makine (VM) genel performans sorun giderme açıklanır ve oluşabilecek sorunlar için olası düzeltme sağlar.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/18/2019
ms.author: v-miegge
ms.openlocfilehash: 176b0634fe2c7ee2f47162e439c4ea16bde77a8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75772627"
---
# <a name="troubleshoot-azure-virtual-machine-performance-on-linux-or-windows"></a>Linux veya Windows'da Azure sanal makine performansını giderin

Bu makalede, izleme ve denetimdarlıkları gözlemleme yoluyla sanal makine (VM) genel performans sorun giderme açıklanır ve oluşabilecek sorunlar için olası düzeltme sağlar. İzlemenin yanı sıra, IO/CPU/Bellek çevresinde en iyi uygulama önerileri ve önemli darboğazları içeren bir rapor sunabilecek Perfinsights'ı da kullanabilirsiniz. Perfinsights, Azure'da hem [Windows](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/how-to-use-perfInsights) hem de [Linux](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/how-to-use-perfinsights-linux) VM'ler için kullanılabilir.

Bu makalede, Performans darboğazları tanılamak için izleme kullanarak yürüyeceğiz.

## <a name="enabling-monitoring"></a>İzlemeyi etkinleştirme

### <a name="azure-iaas-virtual-machine-monitoring"></a>Azure IAAS sanal makine izleme

Konuk VM'yi izlemek için, belirli üst düzey kaynak koşulları konusunda sizi uyaran Azure VM İzleme'yi kullanın. VM tanılama özelliğinin etkin olup olmadığını kontrol etmek için [Azure Kaynak günlüklerine genel bakış'a](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-resource-logs)bakın. Aşağıdakileri görürseniz, büyük olasılıkla tanılama etkin değildir:

![İzleme etkinleştirildi](media/troubleshoot-performance-virtual-machine-linux-windows/1-virtual-machines-monitoring-not-enabled.png)
 
### <a name="enable-vm-diagnostics-through-microsoft-azure-portal"></a>Microsoft Azure portalı aracılığıyla VM tanılamayı etkinleştirme

VM tanılamayı etkinleştirmek için:

1. VM'ye git
2. **Tanılama Ayarları'nı** tıklatın
3. Depolama hesabını seçin ve **konuk düzeyinde izlemeyi etkinleştir'i**tıklatın.

   ![Ayarlar'ı tıklatın, ardından Tanılama](media/troubleshoot-performance-virtual-machine-linux-windows/2-virtual-machines-diagnostics.png)

**Tanılama Ayarları**altında **Aracı** sekmesinden Tanılama kurulumu için kullanılan depolama hesabını kontrol edebilirsiniz.

![Depolama hesabını kontrol edin](media/troubleshoot-performance-virtual-machine-linux-windows/3-check-storage-account.png)

### <a name="enable-storage-account-diagnostics-through-azure-portal"></a>Azure portalı üzerinden depolama hesabı tanılamayı etkinleştirme

Azure'da bir Sanal Makine için IO performansını analiz etmeyi planladığımız da depolama çok önemli bir katmandır. Depolamayla ilgili ölçümler için tanılamayı ek bir adım olarak etkinleştirmemiz gerekir. Yalnızca depolamayla ilgili sayaçları analiz etmek istiyorsak, bu da etkinleştirilebilir.

1. VM'nizi seçerek VM'nizin hangi depolama hesabını (veya hesaplarını) kullandığını belirleyin. **Ayarlar'ı**tıklatın ve **ardından Diskler'i**tıklatın:

   ![Ayarlar'ı tıklatın, ardından Diskler](media/troubleshoot-performance-virtual-machine-linux-windows/4-storage-disks-disks-selection.png)

2. Portalda, VM için depolama hesabına (veya hesaplara) gidin ve aşağıdaki adımları gözden geçirin:

   1. Yukarıdaki adımla bulduğunuz Depolama hesabı için genel bakışı tıklatın.
   2. Varsayılan ölçümler gösterilir. 

    ![Varsayılan ölçümler](media/troubleshoot-performance-virtual-machine-linux-windows/5-default-metrics.png)

3. Ölçümleri yapılandırmak ve eklemek için daha fazla seçenek içeren başka bir bıçak gösteren ölçümlerden herhangi birini tıklatın.

   ![Metrik ekleme](media/troubleshoot-performance-virtual-machine-linux-windows/6-add-metrics.png)

Bu seçenekleri yapılandırmak için:

1.  **Ölçümler**’i seçin.
2.  **Kaynak** 'u (depolama hesabı) seçin.
3.  Ad **Alanını** seçin
4.  **Metrik'i**seçin.
5.  **Toplama** türünü seçin
6.  Bu görünümü panoya sabitleyebilirsiniz.

## <a name="observing-bottlenecks"></a>Darboğazları gözlemleme

Gerekli ölçümler için ilk kurulum işlemini atlattıktan ve VM ve ilgili Depolama hesabı için tanılamayı etkinleştirdikten sonra, analiz aşamasına geçebiliriz.

### <a name="accessing-the-monitoring"></a>İzlemeye erişim

Araştırmak istediğiniz Azure VM'yi seçin ve **İzleme'yi**seçin.

![İzleme'yi seçin](media/troubleshoot-performance-virtual-machine-linux-windows/7-select-monitoring.png)
 
### <a name="timelines-of-observation"></a>Gözlem zaman çizelgeleri

Kaynak sorunları olup olmadığını belirlemek için verilerinizi gözden geçirin. Makinenizin iyi çalıştığını, ancak performansın son zamanlarda bozulduğunu fark ettiyseniz, rapor edilen işlem sırasında ve sonrasında performans metrik verilerini kapsayan bir zaman aralığını gözden geçirin.

### <a name="check-for-cpu-bottleneck"></a>CPU darboğaz olup olup yok

![CPU Darboğaz kontrol edin](media/troubleshoot-performance-virtual-machine-linux-windows/8-cpu-bottleneck-time-range.png)

1. Grafiği edin.
2. Zaman aralığını ayarlayın.
3. Daha sonra sayaç eklemeniz gerekir: CPU Yüzde konuk işletim sistemi
4. Kaydedin.

### <a name="cpu-observe-trends"></a>CPU eğilimleri gözlemlemek

Performans sorunlarına bakarken, eğilimlerin farkında olun ve sizi etkileyip etkilemediklerini anlayın. Sonraki bölümlerde, eğilimleri göstermek için portaldaki İzleme grafiklerini kullanacağız. Bunlar, aynı zaman diliminde çapraz başvuru farkı kaynak davranışları için de yararlı olabilir. Grafikleri özelleştirmek için [Azure Monitör veri platformünü](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform)tıklatın.

Spiking – Spiking zamanlanan bir görev/bilinen olayla ilgili olabilir. Görevi tanımlayabilirseniz, görevin gerekli performans düzeyinde çalışıp çalışmadığını belirleyin. Performans kabul edilebilirse, kaynakları artırmanız gerekmeyebilir.

Spike up ve Sabit – Genellikle yeni bir iş yükünü gösterir. Tanınan bir iş yükü değilse, vm'deki izlemeyi etkinleştirerek hangi işlemin (veya işlemlerin) davranışa neden olduğunu öğrenin. İşlem kabul edildikten sonra, artan tüketimin verimsiz koddan mı yoksa normal tüketimden mi kaynaklandığını belirleyin. Normal tüketim varsa, işlemin gerekli performans düzeyinde çalışıp çalışmadığına karar verin.

Sabit – VM'nizin her zaman bu düzeyde çalışıp çalışmadığını veya tanılama etkinleştirildiğinden beri yalnızca bu düzeyde çalışıp çalışmadığını belirleyin. Bu nedenle, soruna neden olan işlemi (veya işlemleri) tanımlayın ve bu kaynaktan daha fazlasını eklemeyi düşünün.

Giderek Artıyor – Tüketimdeki sürekli artış genellikle verimsiz kod veya daha fazla kullanıcı iş yükü üstlenen bir işlemdir.

### <a name="high-cpu-utilization-remediation"></a>Yüksek CPU kullanımı düzeltmesi

Uygulamanız veya işleminiz doğru performans düzeyinde çalışmıyorsa ve %95 + CPU kullanım sabiti görüyorsanız, aşağıdaki görevlerden birini gerçekleştirebilirsiniz:

* Hemen rahatlama için - VM'nin boyutunu daha fazla çekirdekli bir boyuta yükseltin
* Sorunu anlayın – uygulamayı/süreci bulun ve sorun giderin.

VM'yi artırdıysanız ve CPU hala %95 çalışıyorsa, bu ayarın daha iyi performans mı yoksa daha yüksek uygulama çıktısı mı sunduğunu kabul edilebilir bir düzeye taşıyın. Değilse, tek tek uygulama\işlemi sorun giderin.

CPU tüketimini hangi işlemin yönlendiren bir işlem olduğunu analiz etmek için [Windows](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/how-to-use-perfInsights) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/how-to-use-perfinsights-linux) için Perfinsights'ı kullanabilirsiniz. 

## <a name="check-for-memory-bottleneck"></a>Bellek darboğazı nı kontrol edin

Ölçümleri görüntülemek için:

1. Bölüm ekleyin.
2. Bir Kiremit ekleyin.
3. Galeriyi açın.
4. Bellek Kullanımı'nı seçin ve sürükleyin. Döşeme sabitlendiğinde, sağ tıklatın ve **6x4**seçin.

### <a name="memory-observe-trends"></a>Bellek eğilimleri gözlemlemek

Bellek Kullanımı, VM ile ne kadar bellek tüketildiğini gösterir. Eğilimi ve sorunları gördüğünüz zamana göre eşlenip haritalanmadığını anlayın. Her zaman 100 MB'dan fazla kullanılabilir belleğe sahip olmalısınız.

Spike ve Constant/Constant Steady Consumption - İlişkisel veritabanı motorları gibi bazı uygulamalar büyük miktarda bellek ayırdığı için yüksek bellek kullanımı kötü performansın nedeni olmayabilir ve bu kullanım önemli olmayabilir. Ancak, birden çok bellek aç uygulamaları varsa, kırpma ve diske sayfalama / takas neden bellek çekişme düşük performans görebilirsiniz. Bu kötü performans genellikle uygulama performansı etkisinin fark edilir bir nedenidir.

Sürekli Artan Tüketim - Olası bir uygulama 'ısınma', bu tüketim veritabanı motorları arasında yaygındır başlangıç. Bununla birlikte, bir uygulamadaki bellek sızıntısının bir işareti de olabilir. Uygulamayı tanımlayın ve davranışın beklenip beklenmediğini anlayın.

Sayfa veya Takas Dosyası Kullanımı – Windows Sayfalama dosyasını kullanıp\) kullanmadığınızı kontrol edin `/dev/sdb`(D: veya Linux Swap dosyasında bulunan ( üzerinde bulunan) yoğun olarak kullanılıyor. Bu dosyalar dışında bu birimlerde hiçbir şey yoksa, bu disklerde yüksek Okuma/Yazma'yı denetleyin. Bu sorun, düşük bellek koşullarının göstergesidir.

### <a name="high-memory-utilization-remediation"></a>Yüksek bellek kullanımı düzeltmesi

Yüksek bellek kullanımını çözmek için aşağıdaki görevlerden herhangi birini gerçekleştirin:

* Hemen rahatlama veya Sayfa veya Takas Dosya Kullanımı için - VM boyutunu daha fazla bellekle bire yükseltin, ardından izleyin.
* Sorunu anlayın – yüksek tüketen bellek uygulamalarını tanımlamak için uygulamaları/süreci bulun ve sorun giderin.
* Uygulamayı biliyorsanız, bellek ayırmanın kapatılmış olup olmadığını görün.

Daha büyük bir VM'ye yükselttikten sonra, %100'e kadar sabit bir artış olduğunu fark ederseniz, uygulamayı/işlemi ve sorun giderme yi tanımlayın.

Bellek tüketimini yönlendiren işlemin ne olduğunu analiz etmek için [Windows](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/how-to-use-perfInsights) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/how-to-use-perfinsights-linux) için Perfinsights'ı kullanabilirsiniz. 

## <a name="check-for-disk-bottleneck"></a>Disk darboğaz olup olup yok

VM için depolama alt sistemini kontrol etmek için, VM Diagnostics'teki sayaçları ve depolama hesabı tanılamalarını kullanarak Azure VM düzeyindetanılamayı denetleyin.

VM'ye özgü sorun giderme de, [Windows](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/how-to-use-perfInsights) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/how-to-use-perfinsights-linux)için Perfinsights'ı kullanabilirsiniz , bu da IO'ları hangi sürecin yönlendiren bir süreç olduğunu analiz etmeye yardımcı olabilir. 

Bölge Yedekli ve Premium Depolama Hesapları için sayaçlarımız olmadığını unutmayın. Bu sayaçlarla ilgili sorunlar için bir destek durumu yükseltin.

### <a name="viewing-storage-account-diagnostics-in-monitoring"></a>İzlemede depolama hesabı tanılamalarını görüntüleme

Aşağıdaki öğeler üzerinde çalışmak için portaldaki VM'nin depolama hesabına gidin:

![İzlemede Depolama Hesabı Tanılamalarını Görüntüleme](media/troubleshoot-performance-virtual-machine-linux-windows/9-virtual-machine-storage-account.png)

1. İzleme Grafiğini edin.
2. Zaman aralığını ayarlayın.
3. Aşağıdaki adımlarda açıklanan sayaçları ekleyin.
4. Değişiklikleri kaydedin.

### <a name="disk-observe-trends-standard-storage-only"></a>Disk gözlem eğilimleri (yalnızca standart depolama)

Depolama yla ilgili sorunları belirlemek için, Depolama Hesabı Tanılama ve VM Tanılama'daki performans ölçümlerine bakın.

Aşağıdaki her denetim için, sorunlar sorunun zaman aralığında oluştuğunda önemli eğilimleri arayın.

#### <a name="check-azure-storage-availability--add-the-storage-account-metric-availability"></a>Azure depolama kullanılabilirliğini kontrol et – Depolama hesabı ölçümü ekleme: kullanılabilirlik

Kullanılabilirlikte bir düşüş görürseniz, platformla ilgili bir sorun olabilir, [Azure Durumunu](https://azure.microsoft.com/status/)kontrol edin. Burada herhangi bir sorun gösterilmezse, yeni bir destek isteği kaldırın.

#### <a name="check-for-azure-storage-timeout---add-the-storage-account-metrics"></a>Azure depolama zaman dilimini kontrol edin - Depolama hesabı ölçümlerini ekleyin:

* istemcitimeouthatası
* ServerTimeOutHatası
* AverageE2ELatency
* AverageServerLatency
* TotalRequests

*TimeOutError ölçümlerinde yer alan değerler, bir IO işleminin çok uzun sürdüğünü ve zaman sızdığını gösterir. Sonraki adımlar üzerinde çalışmak, olası nedenleri belirlemenize yardımcı olur.

TimeOutErrors aynı anda AverageServerLatency artar bir platform sorunu olabilir. Bu durumda yeni bir destek isteği getirin.

AverageE2ELatency istemci gecikmesi temsil eder. Uygulama tarafından IOPS'nin nasıl gerçekleştirilmekte olduğunu doğrulayın. Bir artış veya sürekli yüksek TotalRequests metrik arayın. Bu metrik IOPS'yi temsil eder. Depolama hesabının veya tek VHD'nin sınırlarını vurmaya başlıyorsanız, gecikme süreci azaltmayla ilgili olabilir.

#### <a name="check-for-azure-storage-throttling---add-the-storage-account-metrics-throttlingerror"></a>Azure depolama azaltmayı denetleme - Depolama hesabı ölçümlerini ekleme: ThrottlingError

Azaltma değerleri, depolama hesabı düzeyinde kısıtlandığınızı, yani hesabın IOPS sınırına vurduğunuzu gösterir. Metrik **TotalRequests'ü**işaretleyerek IOPs eşiğine uyup uymayacağınızı belirleyebilirsiniz.

Her VHD'nin 500 IOPS veya 60 MBit sınırı olduğunu, ancak depolama hesabı başına 20000 IOPS'un kümülatif sınırına bağlı olduğunu unutmayın.

Bu metrik ile, hangi blob azaltma neden olduğunu ve hangi etkilenir söyleyemem. Ancak, depolama hesabının IOPS veya Giriş/Çıkış sınırlarına da basmış oluyorsunuz.

IOPS sınırına uyup olmadığınızı belirlemek için Depolama Hesabı tanısına gidin ve 20 bin TotalRequests'e yaklaşıp yaklaşmadığınızı görmek için Toplam İstekleri kontrol edin. Desenin değişikliğini, sınırı ilk kez görüp görmediğiniz veya bu sınırın belirli bir zamanda gerçekleşip gerçekleşmediğini tanımlayın.

Standart depolama altında yeni disk teklifleri ile, IOPS ve İş İbiş sınırları farklı olabilir, ancak Standart Depolama hesabının kümülatif sınırı 20000 IOPS(Premium depolama hesap veya Disk düzeyinde farklı sınırlara sahiptir). Farklı standart depolama diski teklifleri ve her disk sınırı hakkında daha fazla bilgi edinin:

* [Windows'daki VM diskler için ölçeklenebilirlik ve performans hedefleri.](https://docs.microsoft.com/azure/virtual-machines/windows/disk-scalability-targets)

#### <a name="references"></a>Başvurular

* [Premium sayfa blob depolama hesapları için ölçeklenebilirlik ve performans hedefleri](../../storage/blobs/scalability-targets-premium-page-blobs.md)

Depolama hesabının bant genişliği Depolama Hesabı Ölçümleri: TotalIngress ve TotalEgress ile ölçülür. Artıklık türüne ve bölgelere bağlı olarak bant genişliği için farklı eşiklere sahipsiniz.

* [Standart depolama hesapları için ölçeklenebilirlik ve performans hedefleri](../../storage/common/scalability-targets-standard-account.md)

Depolama hesabı artıklığı türü ve bölgesi için Toplam Giriş ve Çıkış limitlerine karşı TotalIngress ve TotalEgress'i kontrol edin.

VM'ye bağlı VHD'lerin ThroughPut Limitlerini kontrol edin. VM Ölçümleri Disk Okuma ve Yazma ekleyin.

Standart depolama altındaki yeni disk teklifleri farklı IOPS ve üretim limitlerine sahiptir (IOPS VHD başına açıklanmaz). Disk Okuma ve Yazma'yı kullanarak VM düzeyinde VHD(ler) kombine iş hamuru MB'sinin sınırlarına uyup olmadığınızı görmek için verilere bakın ve ardından VM depolama yapılandırmanızı tek VHD sınırlarını ölçeklendirmek için optimize edin. Farklı standart depolama diski teklifleri ve her disk sınırı hakkında daha fazla bilgi edinin:

* [Windows'daki VM diskler için ölçeklenebilirlik ve performans hedefleri.](https://docs.microsoft.com/azure/virtual-machines/windows/disk-scalability-targets)

### <a name="high-disk-utilizationlatency-remediation"></a>Yüksek disk kullanımı/gecikme düzeltmesi

Geçmiş VHD Limitlerini ölçeklendirmek için İstemcinin Gecikme süresini azaltın ve VM IO'yı Optimize Edin

* [Azure'da Windows için IO'yı en iyi duruma alma](https://azure.microsoft.com/documentation/articles/virtual-machines-sql-server-performance-best-practices/)

* [Azure'da Linux için IO'nun optimizasyonu](https://blogs.msdn.microsoft.com/igorpag/2014/10/23/azure-storage-secrets-and-linux-io-optimizations/)

#### <a name="reduce-throttling"></a>Azaltmayı azaltın

Depolama hesaplarının üst sınırlarına isabet ediyorsanız, VHD'leri depolama hesapları arasında yeniden dengeleyin. Azure [Depolama Ölçeklenebilirliği ve Performans Hedefleri'ne](https://azure.microsoft.com/documentation/articles/storage-scalability-targets/)bakın.

### <a name="increase-throughput-and-reduce-latency"></a>İş iliğini artırın ve gecikme süresini azaltın

Gecikmeye duyarlı bir uygulamanız varsa ve yüksek iş ortası gerektiriyorsa, DS ve GS serisi VM'yi kullanarak VHD'lerinizi Azure Premium depolama alanına geçirin.

Bu makaleler belirli senaryoları tartışır:

* [Azure Premium Depolamaya Geçiş](https://azure.microsoft.com/documentation/articles/storage-migration-to-premium-storage/)

* [SQL Server ile Azure Premium Depolama'yı kullanma](https://azure.microsoft.com/documentation/articles/virtual-machines-sql-server-use-premium-storage/)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarına başvurun.

Alternatif olarak, bir Azure destek olayı dosyala. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.
