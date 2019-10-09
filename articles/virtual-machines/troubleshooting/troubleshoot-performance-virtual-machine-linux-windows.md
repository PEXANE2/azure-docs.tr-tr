---
title: Linux veya Windows çalıştıran Azure sanal makinesi için genel performans sorunlarını giderme
description: Bu makalede, performans sorunlarını izleme ve gözlemleyerek sanal makine (VM) genel performans sorunlarını giderme ve oluşabilecek sorunlar için olası düzeltme sağlar.
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
ms.openlocfilehash: fc8cc4834997033203376cd33670cc907e2911e7
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72170289"
---
# <a name="generic-performance-troubleshooting-for-azure-virtual-machine-running-linux-or-windows"></a>Linux veya Windows çalıştıran Azure sanal makinesi için genel performans sorunlarını giderme

Bu makalede, performans sorunlarını izleme ve gözlemleyerek sanal makine (VM) genel performans sorunlarını giderme ve oluşabilecek sorunlar için olası düzeltme sağlar.

## <a name="enabling-monitoring"></a>İzlemeyi etkinleştirme

### <a name="azure-iaas-virtual-machine-monitoring"></a>Azure ıAAS sanal makine izleme

Konuk VM 'yi izlemek için, Azure VM Izlemeyi kullanın ve bu, bazı üst düzey kaynak koşullarına göre sizi uyarır. VM tanılamaları 'nın etkin olup olmadığını denetlemek için bkz. [Azure Kaynak günlüklerine genel bakış](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-overview#collecting-resource-logs). Aşağıdakileri görürseniz, büyük olasılıkla tanılamayı etkinleştirmezsiniz:

![İzleme etkin değil](media/troubleshoot-performance-virtual-machine-linux-windows/1-virtual-machines-monitoring-not-enabled.png)
 
### <a name="enable-vm-diagnostics-through-microsoft-azure-portal"></a>Microsoft Azure portal aracılığıyla VM tanılamayı etkinleştirme

VM tanılamayı etkinleştirmek için VM 'ye gidin, **Ayarlar**' a ve ardından **Tanılama**' ya tıklayın.

![Ayarlar ' a ve ardından Tanılama ' ya tıklayın](media/troubleshoot-performance-virtual-machine-linux-windows/2-virtual-machines-diagnostics.png)
 
### <a name="enable-storage-account-diagnostics-through-azure-portal"></a>Azure portal aracılığıyla depolama hesabı tanılamayı etkinleştirme

İlk olarak, VM 'yi seçerek sanal makinenizin hangi depolama hesabını (veya hesaplarını) kullandığını belirlersiniz. **Ayarlar**' a ve ardından **diskler**' e tıklayın:

![Ayarlar ' a ve ardından diskler ' e tıklayın](media/troubleshoot-performance-virtual-machine-linux-windows/3-storage-disks-disks-selection.png)

Portalda, VM için depolama hesabına (veya hesaplara) gidin ve aşağıdaki adımlarla çalışın:

![Blob ölçümlerini seçin](media/troubleshoot-performance-virtual-machine-linux-windows/4-select-blob-metrics.png)
 
1. **Tüm ayarlar**' ı seçin.
2. Tanılamayı açın.
3. **BLOB* ölçümleri*' ni seçin ve bekletme 'yi **30** güne ayarlayın.
4. Değişiklikleri kaydedin.

## <a name="observing-bottlenecks"></a>Performans sorunlarını gözlemleme

### <a name="accessing-the-monitoring"></a>İzlemeye erişme

Araştırmak istediğiniz Azure VM 'yi seçin ve **izleme**' yi seçin.

![Izlemeyi Seç](media/troubleshoot-performance-virtual-machine-linux-windows/5-observe-monitoring.png)
 
### <a name="timelines-of-observation"></a>Gözlem zaman çizelgeleri

Kaynak sorunları olup olmadığını belirlemek için verilerinizi gözden geçirin. Makinenizin sorunsuz çalıştığını, ancak performansın yakın zamanda düştüğü bildirildiyse, bildirilen değiştirilmeden önce performans ölçümü verilerini kapsayan zaman aralığını gözden geçirin, bu süre sırasında ve sonrasında.

### <a name="check-for-cpu-bottleneck"></a>CPU performans sorunlarını denetle

![CPU performans sorunlarını denetle](media/troubleshoot-performance-virtual-machine-linux-windows/6-cpu-bottleneck-time-range.png)

1. Grafiği düzenleyin.
2. Zaman aralığını ayarlayın.
3. Daha sonra sayaca eklemeniz gerekir: CPU yüzdesi Konuk işletim sistemi
4. Kaydet.

### <a name="cpu-observe-trends"></a>CPU gözleme eğilimleri

Performans sorunlarına baktığınızda eğilimleri göz önünde bulundurun ve sizi etkileyebileceğini anlayın. Sonraki bölümlerde, eğilimleri göstermek için portaldaki Izleme grafiklerini kullanacağız. Aynı zamanda aynı dönemdeki fark kaynağı davranışları arasında çapraz başvuru için de kullanışlı olabilir. Grafikleri özelleştirmek için [Azure izleyici veri platformu](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform)' na tıklayın.

Spıking – Spıking, zamanlanmış bir görevle/bilinen bir olayla ilişkili olabilir. Görevi tanımlayabiliyorsanız, görevin gerekli performans düzeyinde çalışıp çalışmadığını saptayın. Performans kabul edilebilir ise, kaynakları artırmanız gerekebilir.

Ani artış ve sabit – genellikle yeni bir iş yükü gösterir. Tanınan bir iş yükü değilse, hangi işlemin (veya işlemlerin) davranışa neden olduğunu bulmak için VM 'de izlemeyi etkinleştirin. İşlem tanındıktan sonra, artan tüketimin verimsiz kod veya normal tüketim nedeniyle kaynaklanıp kaynaklanmadığını saptayın. Normal tüketim ise işlemin gerekli performans düzeyinde çalışıp çalışmadığını belirleyin.

Sabit – sanal makinenizin bu düzeyde her zaman çalıştırılıp çalıştırılmadığını veya yalnızca bu düzeyde, tanılamaların etkinleştirildiğinden beri çalışıp çalışmadığını saptayın. Bu durumda, soruna neden olan işlemi (veya işlemleri) ve bu kaynağı eklemeyi göz önünde bulundurun.

Artmasıyla artıyor – tüketimde sabit artış, genellikle verimsiz bir koddur veya daha fazla Kullanıcı iş yükünü alan bir işlemdir.

### <a name="high-cpu-utilization-remediation"></a>Yüksek CPU kullanımı düzeltmesi

Uygulamanız veya işleminiz doğru performans düzeyinde çalışmıyorsa ve% 95 + CPU kullanım sabiti görüyorsanız, aşağıdaki görevlerden birini gerçekleştirebilirsiniz:

* Anında yardım için sanal makinenin boyutunu daha fazla çekirdeğe sahip bir boyuta büyütün
* Sorunu anlayın – uygulama/işlem bulun ve uygun şekilde sorun giderin.

VM 'yi artırdıysanız ve CPU hala% 95 çalıştırıyorsa, bu ayarın kabul edilebilir bir düzeyde daha iyi performans veya daha yüksek uygulama performansı sunmakta olup olmadığını saptayın. Aksi takdirde, tek bir application\processdosyasında sorun giderin.

## <a name="check-for-memory-bottleneck"></a>Bellek sorunu olup olmadığını denetleyin

Ölçümleri görüntülemek için:

1. Bölüm ekleyin.
2. Kutucuk ekleyin.
3. Galeriyi açın.
4. Bellek kullanımını seçin ve sürükleyin. Kutucuk yerleştirildiğinde sağ tıklayıp **6x4**' ü seçin.

### <a name="memory-observe-trends"></a>Bellek gözlemleme eğilimleri

Bellek kullanımı, VM ile ne kadar bellek harcanmakta olduğunu gösterir. Eğilimi ve sorun gördüğünüz zamana eşlenip eşlenmediğini anlayın. Her zaman 100 MB 'tan fazla kullanılabilir belleğe sahip olmanız gerekir.

Ani ve sabit/sabit sürekli tüketim-ilişkisel veritabanı motorları gibi bazı uygulamalar büyük miktarda bellek ayırmakta ve bu kullanım önemli olmayabilir. Ancak, çok fazla bellek kullanan uygulamalar varsa, diske yönelik kırpma ve sayfalama/değiştirme sorunlarına neden olan bellek çekişmesinin yetersiz performansını görebilirsiniz. Bu düşük performans genellikle uygulama performansı etkilerine ilişkin belirgin bir nedendir.

Artmasıyla artan tüketim: olası bir uygulama olan ' ısınmış ', bu tüketim, veritabanı motorları arasında ortaktır. Bununla birlikte, bir uygulamadaki Bellek sızıntısının bir işareti de olabilir. Uygulamayı tanımlayıp davranışın beklenip beklenmediğini anlayın.

Sayfa veya takas dosyası kullanımı – Windows disk belleği dosyasını kullanıp kullanmayacağınızı denetleyin (D: \) veya Linux takas dosyasında (`/dev/sdb` ' de bulunur), yoğun olarak kullanılır. Bu birimler üzerinde bu dosyalar haricinde hiçbir şey yoksa, bu disklerde yüksek okuma/yazma Işlemleri olup olmadığını kontrol edin. Bu sorun, düşük bellek koşullarına göre belirlenir.

### <a name="high-memory-utilization-remediation"></a>Yüksek bellek kullanımı düzeltmesi

Yüksek bellek kullanımını çözümlemek için aşağıdaki görevlerden herhangi birini gerçekleştirin:

* Anında yardım veya sayfa ya da takas dosyası kullanımı için-VM boyutunu daha fazla bellekle artırın ve ardından izleyin.
* Sorunu anlama – uygulamaları/işlemleri bulun ve yüksek tüketen bellek uygulamalarını tanımlamaya yönelik sorunları giderin.
* Uygulamayı biliyorsanız, bellek ayırmasının erişilebilir olup olmadığını görün.

Daha büyük bir VM 'ye yükseltme yaptıktan sonra, hala% 100 ' e kadar sabit bir artış olduğunu fark edersiniz, uygulama/işlem ve sorun giderme işlemlerini belirlemeniz gerekir.

## <a name="check-for-disk-bottleneck"></a>Disk sorunu olup olmadığını denetleyin

VM 'nin depolama alt sistemini denetlemek için, VM tanılama 'daki sayaçları ve ayrıca depolama hesabı tanılamayı kullanarak Azure VM düzeyinde tanılamayı denetleyin.

Bölgesel olarak yedekli ve Premium Depolama hesapları için sayaçların olmadığı unutulmamalıdır. Bu sayaçlarla ilgili sorunlar için bir destek talebi yükseltin.

### <a name="viewing-storage-account-diagnostics-in-monitoring"></a>İzleme sırasında depolama hesabı tanılamayı görüntüleme

Aşağıdaki öğeler üzerinde çalışmak için portaldaki VM için depolama hesabına gidin:

![Izleme sırasında depolama hesabı tanılamayı görüntüleme](media/troubleshoot-performance-virtual-machine-linux-windows/7-virtual-machine-storage-account.png)

1. Izleme grafiğini düzenleyin.
2. Zaman aralığını ayarlayın.
3. Aşağıdaki adımlarda açıklanan sayaçları ekleyin.
4. Değişiklikleri kaydedin.

### <a name="disk-observe-trends-standard-storage-only"></a>Disk gözlemleme eğilimleri (yalnızca standart depolama)

Depolama sorunlarını belirlemek için depolama hesabı tanılama ve VM tanılamasındaki performans ölçümlerine bakın.

Aşağıdaki her bir denetim için, sorun zaman aralığı içinde sorunlar oluştuğunda önemli eğilimleri arayın.

#### <a name="check-azure-storage-availability--add-the-storage-account-metric-availability"></a>Azure depolama kullanılabilirliğini denetleme – depolama hesabı ölçümünü ekleyin: kullanılabilirlik

Kullanılabilirliği olan bir bırakma görürseniz, platformda bir sorun olabilir, [Azure durumunu](https://azure.microsoft.com/status/)kontrol edin. Burada hiçbir sorun gösterilmezse yeni bir destek isteği yükseltin.

#### <a name="check-for-azure-storage-timeout---add-the-storage-account-metrics"></a>Azure depolama zaman aşımını denetle-depolama hesabı ölçümlerini ekleyin:

* ClientTimeOutError
* ServerTimeOutError
* AverageE2ELatency
* Averageserverlatency gösteriyor
* Toplam Istek sayısı

\* TimeOutError ölçümlerinde bulunan değerler bir GÇ işleminin çok uzun sürdüğünü ve zaman aşımına uğradığını gösterir. Sonraki adımlarla çalışmak olası nedenleri belirlemenize yardımcı olur.

AverageServerLatency, TimeOutErrors bir platform sorunu olabilir. Bu durumda yeni bir destek isteği oluştur.

AverageE2ELatency istemci gecikmesini temsil eder. IOPS 'nin uygulama tarafından nasıl gerçekleştirildiğinden emin olun. Artış veya sürekli yüksek TotalRequests ölçümünü arayın. Bu ölçüm ıOPS 'yi temsil eder. Depolama hesabının veya tek VHD 'nin sınırlarına ulaşırsanız, gecikme azaltma ile ilgili olabilir.

#### <a name="check-for-azure-storage-throttling---add-the-storage-account-metrics-throttlingerror"></a>Azure depolama alanı azaltmayı denetle-depolama hesabı ölçümlerini ekleme: Kısıtıngerror

Azaltma değerleri, depolama hesabı düzeyinde sınırlandırıldığınızı, yani hesabın ıOPS sınırına ulaşmadığınızı gösterir. Ölçüm **Totalrequests**' i denetleyerek IOPS eşiğine ulaşıp vurmadığını belirleyebilirsiniz.

Her VHD 'nin 500 ıOPS veya 60 MBits sınırının olduğunu, ancak depolama hesabı başına 20000 ıOPS 'nin birikimli sınırına bağlandığını unutmayın.

Bu ölçüm sayesinde, hangi Blobun bu kısma neden olduğunu ve bundan etkilendiğine bildiremezsiniz. Ancak, depolama hesabının ıOPS veya giriş/çıkış limitlerine vuruyoruz.

IOPS sınırına ulaşıp ulaşmayacağını belirlemek için, depolama hesabı tanılamalarına gidin ve TotalRequests ' i denetleyerek 20000 TotalRequests 'e yaklaşarak göz atın. Bir değişikliği ilk kez görüyor musunuz, ya da bu sınırın belirli bir zamanda olup olmadığı gibi, düzende bir değişiklik yapın.

#### <a name="references"></a>Başvur

* [Sanal makine diskleri için ölçeklenebilirlik hedefleri](https://azure.microsoft.com/documentation/articles/storage-scalability-targets/#scalability-targets-for-virtual-machine-disks)

Depolama hesabının bant genişliği depolama hesabı ölçümleri ile ölçülür: TotalIngress ve TotalEgress. Yedeklilik ve bölge türlerine bağlı olarak bant genişliği için farklı eşiklere sahipsiniz.

* [Bloblar, kuyruklar, tablolar ve dosyalar için ölçeklenebilirlik hedefleri](https://azure.microsoft.com/documentation/articles/storage-scalability-targets/#scalability-targets-for-blobs-queues-tables-and-files)

Depolama hesabı yedeklilik türü ve bölgesi için giriş ve çıkış sınırlarına karşı TotalIngress ve TotalEgress ' i kontrol edin.

VM 'ye bağlı VHD 'lerin verimlilik sınırlarını denetleyin. Okuma ve yazma VM ölçümleri diskini ekleyin.

Her VHD, en fazla 60 MB/sn destekleyebilir (ıOPS, VHD başına gösterilmez). Disk okuma ve yazma kullanarak VM düzeyinde VHD 'ler için Birleşik üretilen iş MB/sn 'nin sınırlarına göz atın ve ardından sanal makine depolama yapılandırmanızı, son VHD limitlerini ölçeklendirmek için iyileştirin.

### <a name="high-disk-utilizationlatency-remediation"></a>Yüksek disk kullanımı/gecikme süresi düzeltmesi

Istemci gecikmesini azaltın ve son VHD sınırlarını ölçeklendirmek için VM GÇ 'yi Iyileştirin

* [Azure 'da Windows için GÇ 'yi iyileştirme](https://azure.microsoft.com/documentation/articles/virtual-machines-sql-server-performance-best-practices/)

* [Azure 'da Linux için GÇ 'yi iyileştirme](https://blogs.msdn.microsoft.com/igorpag/2014/10/23/azure-storage-secrets-and-linux-io-optimizations/)

#### <a name="reduce-throttling"></a>Azaltmayı azalt

Depolama hesaplarının üst sınırlarına vurma, VHD 'leri depolama hesapları arasında yeniden dengeleyin. [Azure depolama ölçeklenebilirlik ve performans hedefleri '](https://azure.microsoft.com/documentation/articles/storage-scalability-targets/)ne bakın.

### <a name="increase-throughput-and-reduce-latency"></a>Aktarım hızını artırma ve gecikme süresini azaltma

Gecikme süresine duyarlı bir uygulamanız varsa ve yüksek verimlilik gerekliyse, DS ve GS serisi VM 'yi kullanarak VHD 'nizi Azure Premium depolamaya geçirin.

Bu makaleler, belirli senaryoları tartışır:

* [Azure Premium Depolama 'ya geçiş](https://azure.microsoft.com/documentation/articles/storage-migration-to-premium-storage/)

* [SQL Server ile Azure Premium depolama kullanma](https://azure.microsoft.com/documentation/articles/virtual-machines-sql-server-use-premium-storage/)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişim kurun.

Alternatif olarak, bir Azure destek olayı dosyası. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.
