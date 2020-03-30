---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/08/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 88b19257a6a7d335e6a928a9eaf7526fbfd5b02e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75942819"
---
## <a name="application-performance-indicators"></a>Uygulama performans göstergeleri

Bir uygulamanın iyi çalışıp çalışmadığını, bir uygulamanın kullanıcı isteğini ne kadar hızlı işlediğini, bir uygulamanın istek başına ne kadar veri işlediğini, belirli bir uygulamada kaç isteğin bir uygulama işlemesi olduğunu değerlendiririz süresi, bir kullanıcının isteğini gönderdikten sonra yanıt almak için ne kadar beklemesi gerektiğini. Bu performans göstergeleri için teknik terimler, IOPS, İş Sonu veya Bant Genişliği ve Gecikme Alanı'dır.

Bu bölümde, Premium Depolama bağlamında ortak performans göstergelerini tartışacağız. Aşağıdaki bölümde, Uygulama Gereksinimlerini Toplama, uygulamanız için bu performans göstergelerini nasıl ölçeceğinizi öğreneceksiniz. Uygulama Performansını Optimize Etmede daha sonra, bu performans göstergelerini etkileyen faktörler ve bunları optimize etmek için öneriler hakkında bilgi edineceksiniz.

## <a name="iops"></a>IOPS

IOPS veya Saniyebaşına Giriş/Çıkış İşlemleri, uygulamanızın depolama disklerine bir saniyede gönderdiği istek sayısıdır. Bir giriş/çıktı işlemi okunabilir veya yazılabilir, sıralı veya rasgele. Çevrimiçi Bir perakende web sitesi gibi online Işlem İşlemi (OLTP) uygulamaları hemen birçok eşzamanlı kullanıcı isteklerini işlemek gerekir. Kullanıcı istekleri, uygulamanın hızlı bir şekilde işlemesi gereken yoğun veritabanı hareketlerini ekler ve günceller. Bu nedenle, OLTP uygulamaları çok yüksek IOPS gerektirir. Bu tür uygulamalar milyonlarca küçük ve rasgele IO isteği işler. Böyle bir uygulamanız varsa, Uygulama altyapısını IOPS için optimize etmek için tasarlamanız gerekir. Daha sonraki bölümde, *Uygulama Performansını Optimize Etme,* yüksek IOPS almak için göz önünde bulundurmanız gereken tüm faktörleri ayrıntılı olarak tartışıyoruz.

Yüksek ölçekli VM'nize bir premium depolama diski eklediğinizde, Azure disk belirtimlerine göre size bir IOPS sayısı garantisi sağlar. Örneğin, bir P50 diski 7500 IOPS sağlar. Yüksek ölçekli her VM boyutunun sürdürebileceği belirli bir IOPS sınırı da vardır. Örneğin, Standart GS5 VM 80.000 IOPS sınırı vardır.

## <a name="throughput"></a>Aktarım hızı

Elde etme veya bant genişliği, uygulamanızın depolama disklerine belirli bir aralıkta gönderdiği veri miktarıdır. Uygulamanız büyük IO birim boyutlarıyla giriş/çıkış işlemleri gerçekleştiriyorsa, yüksek verim gerektirir. Veri ambarı uygulamaları, aynı anda büyük miktarda veriye erişen ve genellikle toplu işlemler gerçekleştiren yoğun tazyik li işlemleri düzenleme eğilimindedir. Başka bir deyişle, bu tür uygulamalar daha yüksek iş elde gerektirir. Böyle bir uygulamanız varsa, altyapısını iş için en iyi duruma getirmek üzere tasarlamanız gerekir. Bir sonraki bölümde, bunu başarmak için ayarlamanız gereken faktörleri ayrıntılı olarak tartışırız.

Yüksek ölçekli bir VM'ye premium depolama diski iliştirdiğinizde, Azure, bu disk belirtimine göre iş elde etmeyi sağlar. Örneğin, bir P50 diski saniyede 250 MB disk aktarım hızı sağlar. Yüksek ölçekli her VM boyutunun sürdürebileceği belirli bir aktarım hızı sınırı da vardır. Örneğin, Standard GS5 VM'sinin aktarım hızı üst sınırı saniyede 2.000 MB'tır.

Aşağıdaki formülde gösterildiği gibi, iş ve IOPS arasında bir ilişki vardır.

![IOPS ve iş artışı ilişkisi](../articles/virtual-machines/linux/media/premium-storage-performance/image1.png)

Bu nedenle, uygulamanızın gerektirdiği en iyi iş ve IOPS değerlerini belirlemek önemlidir. Birini optimize etmeye çalıştıkça, diğeri de etkilenir. Daha sonraki bir bölümde, *Uygulama Performansını Optimize*Etme, IOPS ve İş İsti'yi optimize etme hakkında daha fazla ayrıntı da ele alacağız.

## <a name="latency"></a>Gecikme süresi

Gecikme süresi, bir uygulamanın tek bir isteği almak, depolama disklerine göndermek ve yanıtı istemciye göndermek için gereken süredir. Bu, IOPS ve İş Verme'ye ek olarak bir uygulamanın performansının kritik bir ölçüsüdür. Premium depolama diskinin Gecikme süresi, bir istek için bilgileri almak ve uygulamanıza geri iletmek için gereken süredir. Premium Depolama tutarlı düşük gecikmeler sağlar. Premium Diskler, çoğu IO işlemi için tek basamaklı milisaniye gecikmeleri sağlamak üzere tasarlanmıştır. Premium depolama disklerinde ReadOnly ana bilgisayar önbelleğe almayı etkinleştiriseniz, çok daha düşük okuma gecikmesi elde edebilirsiniz. Disk Önbelleğe Alma'yı *uygulama performansını optimize etme*konulu daha sonraki bölümde daha ayrıntılı olarak tartışacağız.

Daha yüksek IOPS ve İş İlerletme almak için başvurunuzu optimize ettiğinizde, uygulamanızın gecikmesini etkiler. Uygulama performansını aladıktan sonra, beklenmeyen yüksek gecikme davranışından kaçınmak için her zaman uygulamanın gecikmesini değerlendirin.

Yönetilen Diskler'deki aşağıdaki denetim düzlemi işlemleri, Diskin bir Depolama konumundan diğerine hareketini içerebilir. Bu, disklerde veri miktarına bağlı olarak genellikle 24 saatten az, tamamlanması birkaç saat sürebilir verilerin arka plan kopyası ile düzenlenir. Bu süre zarfında uygulamanız normalden daha yüksek okuma gecikmesi yaşayabilir, çünkü bazı okumalar orijinal konuma yönlendirilebilir ve tamamlanması daha uzun sürebilir. Bu dönemde yazma gecikmesi üzerinde hiçbir etkisi yoktur.

- Depolama türünü güncelleştirin.
- Bir VM'den diğerine bir diski ayırın ve takın.
- VHD'den yönetilen bir disk oluşturun.
- Anlık görüntüden yönetilen bir disk oluşturun.
- Yönetilmeyen diskleri yönetilen disklere dönüştürün.

## <a name="performance-application-checklist-for-disks"></a>Diskler için Performans Uygulama Kontrol Listesi

Azure Premium Depolama'da çalışan yüksek performanslı uygulamalar tasarlamanın ilk adımı, uygulamanızın performans gereksinimlerini anlamaktır. Performans gereksinimlerini topladıktan sonra, en iyi performansı elde etmek için uygulamanızı optimize edebilirsiniz.

Önceki bölümde, ortak performans göstergeleri, IOPS, İş Sonu ve Gecikme Durumu'nu açıkladık. İstenilen kullanıcı deneyimini sunmak için bu performans göstergelerinden hangisinin uygulamanız için kritik öneme sahip olduğunu belirlemeniz gerekir. Örneğin, yüksek IOPS en önemli OLTP uygulamaları saniyede milyonlarca işlem işleme. Oysa, yüksek İş İlerletme, veri ambarı uygulamaları için saniyede büyük miktarda veri işleme sayılsa için önemlidir. Son derece düşük Gecikme süresi, canlı video akış lı web siteleri gibi gerçek zamanlı uygulamalar için çok önemlidir.

Ardından, uygulamanızın ömrü boyunca maksimum performans gereksinimlerini ölçün. Başlangıç olarak aşağıdaki örnek denetim listesini kullanın. Normal, en yüksek ve mesai dışı iş yükü dönemlerinde maksimum performans gereksinimlerini kaydedin. Tüm iş yükleri düzeyleri için gereksinimleri tanımlayarak, uygulamanızın genel performans gereksinimini belirleyebilirsiniz. Örneğin, bir e-ticaret web sitesinin normal iş yükü, bir yıl içinde çoğu gün hizmet verdiği işlemler olacaktır. Web sitesinin en yüksek iş yükü, tatil sezonunda veya özel satış etkinliklerinde hizmet verdiği işlemler olacaktır. En yüksek iş yükü genellikle sınırlı bir süre için yaşanır, ancak uygulamanızın normal çalışmasının iki veya daha fazla katını ölçeklendirmesini gerektirebilir. Yüzde 50, yüzde 90 ve yüzde 99'luk gereksinimleri öğrenin. Bu, performans gereksinimlerindeki aykırı ları filtrelemenize yardımcı olur ve çabalarınızı doğru değerleri optimize etme üzerine odaklayabilirsiniz.

## <a name="application-performance-requirements-checklist"></a>Uygulama performans gereksinimleri kontrol listesi

| **Performans gereksinimleri** | **50 Yüzdelik** | **90 Yüzdelik** | **99 Yüzdelik** |
| --- | --- | --- | --- |
| En çok, Saniyedeki işlemler | | | |
| % İşlemleri okuyun | | | |
| % Yazma işlemleri | | | |
| % Rastgele işlemler | | | |
| % Ardışık işlemler | | | |
| IO istek boyutu | | | |
| Ortalama Iş Throughput | | | |
| En çok, Aktarım hızı | | | |
| Dk. Gecikme süresi | | | |
| Ortalama Gecikme | | | |
| En çok, CPU | | | |
| Ortalama CPU | | | |
| En çok, Bellek | | | |
| Ortalama Bellek | | | |
| Sıra Derinliği | | | |

> [!NOTE]
> Bu sayıları, uygulamanızın gelecekteki beklenen büyümesine göre ölçeklemeyi düşünmelisiniz. Büyümeyi önceden planlamak iyi bir fikirdir, çünkü daha sonra performansı artırmak için altyapıyı değiştirmek daha zor olabilir.

Varolan bir uygulamanız varsa ve Premium Depolama'ya geçmek istiyorsanız, öncelikle varolan uygulama için yukarıdaki denetim listesini oluşturun. Ardından, Premium Depolama'da uygulamanızın bir prototipini oluşturun ve uygulamayı bu belgenin sonraki bir bölümünde *Uygulama Performansını Optimize Etmede* açıklanan yönergelere göre tasarlayın. Sonraki makalede, performans ölçümlerini toplamak için kullanabileceğiniz araçlar açıklanmaktadır.

### <a name="counters-to-measure-application-performance-requirements"></a>Uygulama performans gereksinimlerini ölçmek için sayaçlar

Uygulamanızın performans gereksinimlerini ölçmenin en iyi yolu, sunucunun işletim sistemi tarafından sağlanan performans izleme araçlarını kullanmaktır. Windows için PerfMon'u ve Linux için iostat'ı kullanabilirsiniz. Bu araçlar, yukarıdaki bölümde açıklanan her ölçüye karşılık gelen sayaçları yakalar. Uygulamanız normal, en yüksek ve mesai dışı iş yüklerini çalıştırırken bu sayaçların değerlerini yakalamanız gerekir.

PerfMon sayaçları, sunucunuzun işlemci, bellek ve her mantıksal diski ve fiziksel diski için kullanılabilir. VM'li birinci sınıf depolama diskleri kullandığınızda, fiziksel disk sayaçları her premium depolama diski içindir ve mantıksal disk sayaçları premium depolama disklerinde oluşturulan her birim içindir. Uygulama iş yükünüzü barındıran disklerin değerlerini yakalamanız gerekir. Mantıksal ve fiziksel diskler arasında bire bir eşleme varsa, fiziksel disk sayaçlarına başvurabilirsiniz; aksi takdirde mantıksal disk sayaçları bakın. Linux'ta, iostat komutu bir CPU ve disk kullanım raporu oluşturur. Disk kullanım raporu, fiziksel aygıt veya bölüm başına istatistikler sağlar. Verilerini ve ayrı disklerde günlüklerini içeren bir veritabanı sunucunuz varsa, bu verileri her iki disk için de toplayın. Aşağıdaki tabloda diskler, işlemciler ve bellek sayaçları açıklanmaktadır:

| Sayaç | Açıklama | Perfmon | Iostat |
| --- | --- | --- | --- |
| **Saniyede IOPS veya İşlemler** |Depolama diskine saniyede verilen G/Ç isteklerinin sayısı. |Disk Okuma/sn <br> Disk Yazma/sn |Tps <br> r/s <br> w/s |
| **Disk Okur ve Yazar** |Diskte gerçekleştirilen Okuma ve Yazma işlemlerinin %'si. |% Disk Okuma Süresi <br> % Disk Yazma Süresi |r/s <br> w/s |
| **Aktarım hızı** |Saniyede diskten okunan veya diske yazılan veri miktarı. |Disk Okuma Bayt/sn <br> Disk Bayt/sn Yazma |kB_read/s <br> kB_wrtn/s |
| **Gecikme süresi** |Bir disk IO isteğini tamamlamak için toplam süre. |Ortalama Disk sn/Oku <br> Ortalama disk sn/Yaz |bekleme <br> svctm |
| **IO boyutu** |G/Ç'nin boyutu depolama disklerine sorun isteklerini sağlar. |Ortalama Disk Baytları/Okuma <br> Ortalama Disk Bayt/Yazma |avgrq-sz |
| **Sıra Derinliği** |Depolama diskinden okunmayı veya depodiskine yazılmayı bekleyen bekleyen G/Ç isteklerinin sayısı. |Geçerli Disk Sıra Uzunluğu |avgqu-sz |
| **Max. Bellek** |Uygulamayı sorunsuz çalıştırmak için gereken bellek miktarı |Kullanımda Taahhüt Edilen Baytlar |vmstat kullanın |
| **Max. Cpu** |Uygulamayı sorunsuz çalıştırmak için gereken CPU miktarı |% İşlemci süresi |%util |

[iostat](https://linux.die.net/man/1/iostat) ve [PerfMon](https://msdn.microsoft.com/library/aa645516.aspx)hakkında daha fazla bilgi edinin.



## <a name="optimize-application-performance"></a>Uygulama performansını optimize edin

Premium Depolama'da çalışan bir uygulamanın performansını etkileyen başlıca faktörler IO isteklerinin doğası, VM boyutu, Disk boyutu, disk sayısı, disk önbelleğe alma, çok iş parçacığı ve sıra derinliğidir. Bu faktörlerden bazılarını sistem tarafından sağlanan düğümlerle kontrol edebilirsiniz. Çoğu uygulama size IO boyutunu ve Sıra Derinliğini doğrudan değiştirme seçeneği vermeyebilir. Örneğin, SQL Server kullanıyorsanız, IO boyutunu ve sıra derinliğini seçemezsiniz. SQL Server en iyi performansı elde etmek için en iyi IO boyutunu ve sıra derinliği değerlerini seçer. Performans gereksinimlerini karşılamak için uygun kaynakları sağlamak için her iki etken türünün de uygulama performansınız üzerindeki etkilerini anlamak önemlidir.

Bu bölüm boyunca, uygulama performansınızı en iyi duruma getirmek için ne kadar ihtiyacınız olduğunu belirlemek için oluşturduğunuz uygulama gereksinimleri denetim listesine bakın. Buna dayanarak, bu bölümden hangi faktörleri ayarlamanız gerekeceğini belirleyebileceksiniz. Her faktörün uygulama performansınız üzerindeki etkilerine tanık olmak için, karşılaştırma araçlarını uygulama kurulumunuzda çalıştırın. Windows ve Linux VM'lerde ortak kıyaslama araçlarını çalıştırma adımları için, sonunda bağlantılı Kıyaslama makalesine bakın.

### <a name="optimize-iops-throughput-and-latency-at-a-glance"></a>Bir bakışta IOPS'yi, iş akışını ve gecikmeyi optimize edin

Aşağıdaki tabloda performans faktörleri ve IOPS, iş sonu ve gecikme en iyi duruma getirmek için gerekli adımları özetler. Bu özeti izleyen bölümlerher faktörü çok daha derinlemesine açıklar.

VM boyutları ve IOPS, iş sonu ve her VM türü için kullanılabilir gecikme hakkında daha fazla bilgi için [Linux VM boyutları](../articles/virtual-machines/linux/sizes.md) veya Windows [VM boyutlarına](../articles/virtual-machines/windows/sizes.md)bakın.

| &nbsp; | **IOPS** | **Aktarım hızı** | **Gecikme süresi** |
| --- | --- | --- | --- |
| **Örnek Senaryo** |Kurumsal OLTP uygulaması saniyede çok yüksek işlemler gerektiren. |Kurumsal Veri depolama uygulaması büyük miktarda veri işleme. |Çevrimiçi oyun gibi kullanıcı isteklerine anında yanıt gerektiren gerçek zamanlı uygulamaların yakınında. |
| Performans faktörleri | &nbsp; | &nbsp; | &nbsp; |
| **IO boyutu** |Daha küçük IO boyutu daha yüksek IOPS verir. |Daha yüksek Verim elde etmek için daha büyük IO boyutu. | &nbsp;|
| **VM boyutu** |Uygulama gereksiniminizden daha büyük IOPS sunan bir VM boyutu kullanın. |Uygulama gereksiniminizden daha büyük iş fazlası olan bir VM boyutu kullanın. |Uygulama gereksiniminizden daha büyük ölçek sınırları sunan bir VM boyutu kullanın. |
| **Disk boyutu** |IOPS'yi uygulama gereksiniminizden daha büyük sunan bir disk boyutu kullanın. |İş Ortası sınırı uygulama gereksiniminizden daha büyük bir disk boyutu kullanın. |Uygulama gereksiniminizden daha büyük ölçek sınırları sunan bir disk boyutu kullanın. |
| **VM ve Disk Ölçeği Sınırları** |Seçilen VM boyutunun IOPS sınırı, bağlı depolama diskleri tarafından yönlendirilen toplam IOPS'den daha büyük olmalıdır. |Seçilen VM boyutunun iş verme sınırı, bağlı premium depolama diskleri tarafından yönlendirilen toplam İş İbzül'den daha büyük olmalıdır. |Seçilen VM boyutunun ölçek sınırları, ekli premium depolama disklerinin toplam ölçek sınırlarından daha büyük olmalıdır. |
| **Disk Önbelleğe Alma** |Read ağır işlemleri ile premium depolama disklerinde ReadOnly Önbelleğini etkinleştirin ve daha yüksek Okuma IOPS'u elde edin. | &nbsp; |Çok düşük Okuma gecikmeleri elde etmek için Hazır ağır işlemleri ile premium depolama disklerinde ReadOnly Önbelleğini etkinleştirin. |
| **Disk Şeritleme** |Birden çok disk kullanın ve daha yüksek Bir IOPS ve İş Verme sınırı elde etmek için onları bir araya getirin. VM başına birleştirilmiş sınır, bağlı premium disklerin birleşik sınırlarından daha yüksek olmalıdır. | &nbsp; | &nbsp; |
| **Şerit Boyutu** |OLTP uygulamalarında görülen rastgele küçük IO deseni için daha küçük şerit boyutu. Örneğin, SQL Server OLTP uygulaması için 64 KB şerit boyutunu kullanın. |Veri Ambarı uygulamalarında görülen sıralı büyük IO deseni için daha büyük şerit boyutu. Örneğin, SQL Server Data ambarı uygulaması için 256 KB şerit boyutu kullanın. | &nbsp; |
| **Çoklu iş parçacığı kullanımı** |Daha yüksek IOPS ve İş Elde Etme işlemine yol açacak daha fazla sayıda talebi Premium Depolama'ya itmek için çoklu iş parçacığı kullanın. Örneğin, SQL Server'da, SQL Server'a daha fazla CPU ayırmak için yüksek bir MAXDOP değeri ayarlayın. | &nbsp; | &nbsp; |
| **Sıra Derinliği** |Daha büyük Kuyruk Derinliği daha yüksek IOPS verir. |Daha Büyük Sıra Derinliği daha yüksek Verim verir. |Daha küçük Sıra Derinliği daha düşük gecikmeler verir. |

## <a name="nature-of-io-requests"></a>IO isteklerinin doğası

IO isteği, uygulamanızın gerçekleştireceği bir giriş/çıkış işlemi birimidir. Küçük veya büyük, rasgele veya sıralı, okuma veya yazma iIO isteklerinin niteliğini belirlemek, uygulamanızın performans gereksinimlerini belirlemenize yardımcı olur. IO isteklerinin doğasını anlamak, uygulama altyapınızı tasarlarken doğru kararlar almak önemlidir. Mümkün olan en iyi performansı elde etmek için IOs eşit olarak dağıtılmalıdır.

IO boyutu en önemli faktörlerden biridir. IO boyutu, uygulamanız tarafından oluşturulan giriş/çıkış işlemi isteğinin boyutudur. IO boyutu, özellikle uygulamanın elde edilebildiği IOPS ve Bant Genişliği üzerinde performans üzerinde önemli bir etkiye sahiptir. Aşağıdaki formül, IOPS, IO boyutu ve Bant Genişliği/İş-İş-İş  
    ![](media/premium-storage-performance/image1.png)

Bazı uygulamalar IO boyutlarını değiştirmenize izin verirken, bazı uygulamalar değiştirmez. Örneğin, SQL Server en iyi IO boyutunu kendisi belirler ve kullanıcılara bunu değiştirmek için herhangi bir düğüm sağlamaz. Öte yandan Oracle, veritabanının G/Ç istek boyutunu yapılandırabileceğiniz [DB\_BLOCK\_SIZE](https://docs.oracle.com/cd/B19306_01/server.102/b14211/iodesign.htm#i28815) adlı bir parametre sağlar.

IO boyutunu değiştirmenize izin vermeyen bir uygulama kullanıyorsanız, uygulamanızla en alakalı performans KPI'sını optimize etmek için bu makaledeki yönergeleri kullanın. Örneğin,

* Bir OLTP uygulaması milyonlarca küçük ve rasgele IO isteği oluşturur. Bu tür IO isteklerini işlemek için, uygulama altyapınızı daha yüksek IOPS almak için tasarlamanız gerekir.  
* Veri depolama uygulaması büyük ve sıralı IO istekleri oluşturur. Bu tür IO isteklerini işlemek için, daha yüksek Bant genişliği veya İş İş İlerletmek için uygulama altyapınızı tasarlamanız gerekir.

IO boyutunu değiştirmenize olanak tanıyan bir uygulama kullanıyorsanız, diğer performans yönergelerine ek olarak IO boyutu için bu başparmak kuralını kullanın,

* Daha yüksek IOPS almak için daha küçük IO boyutu. Örneğin, bir OLTP uygulaması için 8 KB.  
* Daha yüksek Bant genişliği/İş Verme elde etmek için daha büyük IO boyutu. Örneğin, bir veri ambarı uygulaması için 1024 KB.

Uygulamanız için IOPS ve İş İsp/Bant Genişliğini nasıl hesaplayabildiğinize ilişkin bir örnek aşağıda verilmiştir. P30 diskkullanarak bir uygulama düşünün. Bir P30 diskin elde edebileceği maksimum IOPS ve Throughput/Bant genişliği sırasıyla saniyede 5000 IOPS ve 200 MB'dır. Şimdi, uygulamanız P30 diskten maksimum IOPS gerektiriyorsa ve 8 KB gibi daha küçük bir IO boyutu kullanıyorsanız, elde edeceğiniz Bant genişliği saniyede 40 MB'dır. Ancak, uygulamanız P30 diskten maksimum ThroughPut/Bant genişliği gerektiriyorsa ve 1024 KB gibi daha büyük bir IO boyutu kullanıyorsanız, ortaya çıkan IOPS daha az, 200 IOPS olacaktır. Bu nedenle, Uygulamanızın IOPS ve İş-İş/Bant Genişliği gereksinimini karşılar şekilde IO boyutunu ayarlayın. Aşağıdaki tablo, bir P30 disk için farklı IO boyutlarını ve bunların karşılık gelen IOPS ve İş İbılasını özetler.

| Uygulama Gereksinimi | G/Ç boyutu | IOPS | İş-İş- Bant Genişliği |
| --- | --- | --- | --- |
| Maks. IOPS |8 KB |5.000 |Saniyede 40 MB |
| Max Throughput |1024 KB |200 |Saniyede 200 MB |
| Maksimum Throughput + yüksek IOPS |64 KB |3,200 |Saniyede 200 MB |
| Max IOPS + yüksek Throughput |32 KB |5.000 |Saniyede 160 MB |

IOPS ve Bant genişliğini tek bir premium depolama diskinin maksimum değerinden daha yüksek almak için, birlikte çizgili birden çok premium disk kullanın. Örneğin, 10.000 IOPS'nin birleşik IOPS'si veya saniyede 400 MB'lık birleşik Bir İş Verme İşlemi elde etmek için iki P30 diski şerit altına alın. Bir sonraki bölümde açıklandığı gibi, birleştirilmiş disk IOPS ve İş Verme destekleyen bir VM boyutu kullanmanız gerekir.

> [!NOTE]
> IOPS veya Throughput diğer de artar arttıkça, disk veya VM iş üretimi veya IOPS sınırları isabet olmadığından emin olun.

IO boyutunun uygulama performansı üzerindeki etkilerine tanık olmak için, VM ve disklerinizde kıyaslama araçları çalıştırabilirsiniz. Etkisini görmek için birden çok test çalışması oluşturun ve her çalıştırma için farklı IO boyutu kullanın. Daha fazla bilgi için, sonunda bağlantılı Kıyaslama makalesine bakın.

## <a name="high-scale-vm-sizes"></a>Yüksek ölçekli VM boyutları

Bir uygulama tasarlamaya başladığınızda, yapmanız gereken ilk şeylerden biri, uygulamanızı barındıracak bir VM seçmektir. Premium Depolama, daha yüksek bilgi işlem gücü ve yüksek yerel disk G/Ç performansı gerektiren uygulamaları çalıştırabilen Yüksek Ölçekli VM boyutlarıyla birlikte gelir. Bu VM'ler daha hızlı işlemciler, daha yüksek bellek-çekirdek oranı ve yerel disk için Katı Hal Sürücüsü (SSD) sağlar. Premium Depolamayı destekleyen yüksek ölçekli VM'lere örnek olarak DS ve GS serisi VM'ler verilebilir.

Yüksek Ölçekli VM'ler farklı sayıda CPU çekirdeği, bellek, işletim sistemi ve geçici disk boyutuyla farklı boyutlarda kullanılabilir. Her VM boyutunda, VM'ye ekebileceğiniz maksimum veri diski sayısı da vardır. Bu nedenle, seçilen VM boyutu uygulamanız için ne kadar işlem, bellek ve depolama kapasitesinin kullanılabilir olduğunu etkiler. İşlem ve Depolama maliyetini de etkiler. Örneğin, aşağıda bir DS serisi ve GS serisinin en büyük VM boyutunun özellikleri ve rilmiştir:

| VM boyutu | CPU çekirdekleri | Bellek | VM disk boyutları | En çok, veri diskleri | Önbellek boyutu | IOPS | Bant genişliği Önbellek IO sınırları |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS14 |16 |112 GB |İşletim Sistemi = 1023 GB <br> Yerel SSD = 224 GB |32 |576 GB |50.000 IOPS <br> Saniyede 512 MB |4.000 IOPS ve saniyede 33 MB |
| Standard_GS5 |32 |448 GB |İşletim Sistemi = 1023 GB <br> Yerel SSD = 896 GB |64 |4224 GB |80.000 IOPS <br> Saniyede 2.000 MB |Saniyede 5.000 IOPS ve 50 MB |

Kullanılabilir tüm Azure VM boyutlarının tam listesini görüntülemek için [Windows VM boyutlarına](../articles/virtual-machines/windows/sizes.md) veya Linux [VM boyutlarına](../articles/virtual-machines/linux/sizes.md)bakın. İstediğiniz uygulama performansı gereksinimlerini karşılayıp ölçeklendirebilen bir VM boyutu seçin. Buna ek olarak, VM boyutları seçerken önemli hususları göz önünde bulundurun.

*Ölçek Sınırları*  
VM ve disk başına maksimum IOPS sınırları birbirinden farklıdır ve birbirinden bağımsızdır. Uygulamanın IOPS'yi VM sınırları içinde ve ona bağlı premium diskler içinde kullandığından emin olun. Aksi takdirde, uygulama performansı azaltma yaşayacaktır.

Örnek olarak, bir uygulama gereksiniminin en fazla 4.000 IOPS olduğunu varsayalım. Bunu başarmak için, DS1 VM'de bir P30 diski sağlamanız gerekir. P30 disk5.000 IOPS'ye kadar teslim edebilir. Ancak, DS1 VM 3.200 IOPS ile sınırlıdır. Sonuç olarak, uygulama performansı VM limiti ile 3.200 IOPS'de sınırlandırılacak ve performans düşürülecektir. Bu durumu önlemek için, her ikisi de uygulama gereksinimlerini karşılayacak bir VM ve disk boyutu seçin.

*İşletme Maliyeti*  
Çoğu durumda, Premium Depolama'yı kullanmanızın toplam kullanım maliyetinin Standart Depolama'yı kullanmaktan daha düşük olması mümkündür.

Örneğin, 16.000 IOPS gerektiren bir uygulama düşünün. Bu performansı elde etmek için,\_32 standart depolama 1 TB disk kullanarak maksimum 16.000 IOPS verebilecek bir Standart D14 Azure IaaS VM'ye ihtiyacınız olacaktır. Her 1-TB standart depolama diski maksimum 500 IOPS elde edebilirsiniz. Bu VM'nin aylık tahmini maliyeti 1.570 ABD doları olacaktır. 32 standart depolama disklerinin aylık maliyeti 1.638 TL olacaktır. Tahmini toplam aylık maliyet 3.208 $ olacaktır.

Ancak, Premium Depolama'da aynı uygulamayı barındırıyorsanız, daha küçük bir VM boyutuna ve daha az premium depolama diskine ihtiyacınız olur ve böylece toplam maliyeti düşürür. Standart\_DS13 VM dört P30 disk kullanarak 16.000 IOPS gereksinimini karşılayabilir. DS13 VM 25.600 maksimum IOPS ve her P30 disk 5.000 maksimum IOPS vardır. Genel olarak, bu yapılandırma 5.000 x 4 = 20.000 IOPS elde edebilirsiniz. Bu VM'nin aylık tahmini maliyeti 1.003 TL olacaktır. Dört P30 premium depolama disklerinin aylık maliyeti 544,34 $ olacaktır. Tahmini toplam aylık maliyet 1.544 $ olacaktır.

Aşağıdaki tablo, Standart ve Premium Depolama için bu senaryonun maliyet dökümünü özetleyilmektedir.

| &nbsp; | **Standart** | **Premium** |
| --- | --- | --- |
| **Aylık VM maliyeti** |$1,570.58 (Standart\_D14) |$1,003.66 (Standart\_DS13) |
| **Disklerin aylık maliyeti** |$1,638.40 (32 x 1-TB diskler) |$544.34 (4 x P30 diskler) |
| **Aylık Toplam Maliyet** |3.208,98 $ |1.544,34 $ |

*Linux Distros*  

Azure Premium Depolama ile, Windows ve Linux çalıştıran VM'ler için aynı performans düzeyine sahip olursunuz. Biz Linux distros birçok tatlar destek ve [burada](../articles/virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)tam listesini görebilirsiniz. Farklı dağıtımların farklı iş yükleri için daha uygun olduğunu unutmayın. İş yükünüzün üzerinde çalışmaya devam eden dağıtıma bağlı olarak farklı performans düzeyleri görürsünüz. Uygulamanızla Linux dağıtımını test edin ve en iyi çalışanı seçin.

Premium Depolama ile Linux çalıştırırken, yüksek performans sağlamak için gerekli sürücüler le ilgili en son güncellemeleri kontrol edin.

## <a name="premium-storage-disk-sizes"></a>Üstün depolama disk boyutları

Azure Premium Depolama, ihtiyaçlarınıza en uygun olanı seçebilmeniz için çeşitli boyutlar sunar. Her disk boyutunun IOPS, bant genişliği ve depolama için farklı bir ölçek sınırı vardır. Uygulama gereksinimlerine ve yüksek ölçekli VM boyutuna bağlı olarak doğru Premium Depolama Diskboyutunu seçin. Aşağıdaki tabloda disklerin boyutları ve yetenekleri gösterilmektedir. P4, P6, P15, P60, P70 ve P80 boyutları şu anda yalnızca Yönetilen Diskler için desteklenir.

[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

Seçtiğiniz disk kaç seçilen disk boyutuna bağlıdır. Uygulama gereksiniminizi karşılamak için tek bir P50 disk veya birden çok P10 disk kullanabilirsiniz. Seçim yaparken aşağıda listelenen hususları göz önünde bulundurun.

*Ölçek Sınırları (IOPS ve İş İbadet)*  
Her Premium disk boyutunun IOPS ve İş İbazalt sınırları, VM ölçek sınırlarından farklıdır ve bağımsızdır. Disklerden gelen toplam IOPS ve Throughput'un seçilen VM boyutunun ölçek sınırları içinde olduğundan emin olun.

Örneğin, bir uygulama gereksinimi maksimum 250 MB/sn Verim ise ve tek bir P30 diske sahip bir DS4 VM kullanıyorsanız. DS4 VM 256 MB/sn'ye kadar Throughput verebilir. Ancak, tek bir P30 disk200 MB/sn İş İş Parçacığı sınırına sahiptir. Sonuç olarak, uygulama disk sınırı nedeniyle 200 MB/sn olarak sınırlandırılacaktır. Bu sınırı aşmak için VM'ye birden fazla veri diski verin veya disklerinizi P40 veya P50'ye yeniden boyutlandırın.

> [!NOTE]
> Önbellek tarafından sunulan okumalar disk IOPS ve İş Verme'ye dahil edilmez, bu nedenle disk sınırlarına tabi değildir. Önbellek, VM başına ayrı IOPS ve İş İş İbzül sınırına sahiptir.
>
> Örneğin, başlangıçta okuma ve yazma sırasıyla 60MB/sn ve 40MB/sn vardır. Zaman içinde, önbellek ısınır ve önbellekten okumadaha fazla hizmet vermektedir. Daha sonra diskten daha yüksek yazı Misli ile alabilirsiniz.

*Disk sayısı*  
Uygulama gereksinimlerini değerlendirerek gereksinim duyacağınız disk sayısını belirleyin. Her VM boyutu, VM'ye ekebileceğiniz disk sayısında da bir sınıra sahiptir. Genellikle, bu çekirdek sayısının iki katıdır. Seçtiğiniz VM boyutunun gerekli disk sayısını desteklediğinden emin olun.

Premium Depolama disklerinin Standart Depolama disklerine kıyasla daha yüksek performans özelliklerine sahip olduğunu unutmayın. Bu nedenle, Standart Depolama'yı Kullanarak Uygulamanızı Azure IaaS VM'den Premium Depolama'ya geçiriyorsanız, uygulamanız için aynı veya daha yüksek performansı elde etmek için büyük olasılıkla daha az premium diske ihtiyacınız olacaktır.

## <a name="disk-caching"></a>Disk önbelleği

Azure Premium Depolama'dan yararlanan yüksek ölçekli VM'ler, BlobCache adı verilen çok katmanlı bir önbelleğe alma teknolojisine sahiptir. BlobCache önbelleğe alma için Sanal Makine RAM ve yerel SSD bir arada kullanır. Bu önbellek, Premium Depolama kalıcı diskleri ve VM yerel diskleri için kullanılabilir. Varsayılan olarak, bu önbellek ayarı, İşletim Sistemi diskleri için Oku/Yaz ve Premium Depolama'da barındırılan veri diskleri için ReadOnly olarak ayarlanır. Premium Depolama disklerinde disk önbelleğe alma özelliği etkinleştirildiğinde, yüksek ölçekli VM'ler temel disk performansını aşan son derece yüksek performans düzeyleri elde edebilir.

> [!WARNING]
> Disk Önbelleğe Alma diskler 4 TiB ve daha büyük için desteklenmez. VM'nize birden fazla disk bağlıysa, 4 TiB'den küçük her disk önbelleğe alma desteği verecektir.
>
> Bir Azure diskinin önbellek ayarını değiştirmek diski ayırıp yeniden ekler. İşletim sistemi diskiyse, VM yeniden başlatılır. Disk önbellek ayarını değiştirmeden önce bu kesintiden etkilenebilecek tüm uygulamaları/hizmetleri durdurun.

BlobCache'nin nasıl çalıştığı hakkında daha fazla bilgi edinmek için, Inside [Azure Premium Storage](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) blog gönderisine bakın.

Sağ disk kümesinde önbelleği etkinleştirmek önemlidir. Premium diskte disk önbelleğe alınmasını etkinleştirip etkinleştirmemeniz, diskin iş yükünün işleme sayılacağına bağlıdır. Aşağıdaki tabloda işletim sistemi ve veri diskleri için varsayılan önbellek ayarları gösterilmektedir.

| **Disk türü** | **Varsayılan önbellek ayarı** |
| --- | --- |
| İşletim sistemi diski |ReadWrite |
| Veri diski |ReadOnly |

Aşağıda veri diskleri için önerilen disk önbelleği ayarları,

| **Disk önbelleğe alma ayarı** | **bu ayarı ne zaman kullanacağına ilişkin öneri** |
| --- | --- |
| None |Yalnızca yazma ve yazma ağır diskleri için ana bilgisayar önbelleğini Yok olarak yapılandırın. |
| ReadOnly |Yalnızca okuma ve okuma-yazma diskleri için ana bilgisayar önbelleğini ReadOnly olarak yapılandırın. |
| ReadWrite |Ana bilgisayar önbelleğini ReadWrite olarak yapılandırın, ancak uygulamanız önbelleğe alınmış verileri gerektiğinde kalıcı disklere yazmayı düzgün bir şekilde işlerse. |

*Readonly*  
Premium Depolama veri disklerinde ReadOnly önbelleğe alma nızı yapılandırarak, düşük Okuma gecikmesi elde edebilir ve uygulamanız için çok yüksek Okuma IOPS ve İş Kaynağı elde edebilirsiniz. Bunun iki nedeni var.

1. VM belleğinde ve yerel SSD'de bulunan önbellekten gerçekleştirilen okumalar, Azure blob depolama alanı üzerindeki veri diskinden okumalardan çok daha hızlıdır.  
1. Premium Depolama, önbellekten disk IOPS ve İş İlerletme'ye doğru sunulan Okumaları saymaz. Bu nedenle, uygulamanız daha yüksek toplam IOPS ve ThroughPut elde etmek mümkün.

*Readwrite*  
Varsayılan olarak, işletim sistemi diskleri ReadWrite önbelleğe alma etkin. Yakın zamanda veri disklerinde ReadWrite önbelleğe alma desteği de ekledik. ReadWrite önbelleğe alma kullanıyorsanız, önbellekten kalıcı disklere veri yazmak için uygun bir yolunuz olmalıdır. Örneğin, SQL Server önbelleğe alınmış verileri kalıcı depolama disklerine tek başına yazmayı işler. VM çöküyorsa, gerekli verileri kalıcı olarak işlemeyen bir uygulamayla ReadWrite önbelleğini kullanmak veri kaybına yol açabilir.

*Yok*  
Şu **anda, Yok** yalnızca veri disklerinde desteklenir. İşletim sistemi disklerinde desteklenmez. Bir işletim sistemi diskinde **None** ayarlarsanız, bunu içten olarak geçersiz kılar ve **ReadOnly**olarak ayarlar.

Örnek olarak, aşağıdakileri yaparak Premium Depolama'da çalışan SQL Server'a bu yönergeleri uygulayabilirsiniz,

1. Veri dosyalarını barındıran birinci sınıf depolama disklerinde "ReadOnly" önbelleğini yapılandırın.  
   a.  Önbellekten hızlı okumalar, veri sayfaları doğrudan veri disklerinden göre önbellekten çok daha hızlı alındığı için SQL Server sorgu süresini düşürür.  
   b.  Önbellekten okumaların sunulması, premium veri disklerinden ek Iş Aktarış olduğu anlamına gelir. SQL Server, yedekleme/geri yükleme, toplu iş yükleri ve dizin yeniden oluşturma gibi daha fazla veri sayfası ve diğer işlemleri almak için bu ek İş Metot'u kullanabilir.  
1. Günlük dosyalarını barındıran premium depolama disklerinde "Yok" önbelleğini yapılandırın.  
   a.  Günlük dosyaları öncelikle yazma ağır işlemleri var. Bu nedenle, ReadOnly önbelleğinden yararlanamaz.

## <a name="optimize-performance-on-linux-vms"></a>Linux VM'lerde performansı optimize edin

**ReadOnly** veya **None**olarak ayarlanmış önbelleği olan tüm premium SSD'ler veya ultra diskler için, dosya sistemini monte ederken "engelleri" devre dışı bırakmalısınız. Bu senaryoda engellere gerek yoktur, çünkü premium depolama disklerine yazmabu önbellek ayarları için dayanıklıdır. Yazma isteği başarıyla tamamlandığında, veriler kalıcı depoya yazılır. "Engelleri" devre dışı kaldırmak için aşağıdaki yöntemlerden birini kullanın. Dosya sisteminiz için birini seçin:
  
* **ReiserFS**için, engelleri devre dışı `barrier=none` kullanabilirsiniz, montaj seçeneğini kullanın. (Engelleri etkinleştirmek `barrier=flush`için kullanın.)
* **Ext3/ext4**için, engelleri devre dışı `barrier=0` kullanabilirsiniz, montaj seçeneğini kullanın. (Engelleri etkinleştirmek `barrier=1`için kullanın.)
* **XFS**için, engelleri devre dışı `nobarrier` kullanabilirsiniz, montaj seçeneğini kullanın. (Engelleri etkinleştirmek `barrier`için kullanın.)
* **Önbelleği ReadWrite**olarak ayarlanmış birinci sınıf depolama diskleri için yazma dayanıklılığı için engelleri etkinleştirin.
* VM'yi yeniden başlattıktan sonra ses etiketlerinin devam edeabilmesi için, disklere yapılan evrensel benzersiz tanımlayıcı (UUID) başvurularını güncellemeniz gerekir. Daha fazla bilgi için [bkz.](../articles/virtual-machines/linux/add-disk.md)

Aşağıdaki Linux dağıtımları premium SSD'ler için doğrulanmıştır. Premium SSD'lerle daha iyi performans ve kararlılık için, VM'lerinizi bu sürümlerden birine veya daha yeni sürümlere yükseltmenizi öneririz. 

Bazı sürümler, Azure için en son Linux Tümleştirme Hizmetleri (LIS), v4.0'ı gerektirir. Bir dağıtımı indirmek ve yüklemek için aşağıdaki tabloda listelenen bağlantıyı izleyin. Doğrulamayı tamamlarken listeye resim ekliyoruz. Doğrulamalarımız, performansın her görüntü için değiştiğini gösterir. Performans iş yükü özelliklerine ve görüntü ayarlarınıza bağlıdır. Farklı görüntüler, farklı iş yükleri için ayarlanır.

| Dağıtım | Sürüm | Desteklenen çekirdek | Ayrıntılar |
| --- | --- | --- | --- |
| Ubuntu | 12.04 veya daha yeni| 3.2.0-75.110+ | &nbsp; |
| Ubuntu | 14.04 veya daha yeni| 3.13.0-44.73+  | &nbsp; |
| Debian | 7.x, 8.x veya daha yeni| 3.16.7-ckt4-1+ | &nbsp; |
| SUSE | SLES 12 veya daha yeni| 3.12.36-38.1+ | &nbsp; |
| SUSE | SLES 11 SP4 veya daha yeni| 3.0.101-0.63.1+ | &nbsp; |
| CoreOS | 584.0.0+ veya daha yeni| 3.18.4+ | &nbsp; |
| CentOS | 6.5, 6.6, 6.7, 7.0 veya daha yeni| &nbsp; | [LIS4 gerekli](https://www.microsoft.com/download/details.aspx?id=55106) <br> *Sonraki bölümdeki nota bakın* |
| CentOS | 7.1+ veya daha yeni| 3.10.0-229.1.2.el7+ | [LIS4 tavsiye edilir](https://www.microsoft.com/download/details.aspx?id=55106) <br> *Sonraki bölümdeki nota bakın* |
| Red Hat Enterprise Linux (RHEL) | 6.8+, 7.2+veya daha yeni | &nbsp; | &nbsp; |
| Oracle | 6.0+, 7.2+veya daha yeni | &nbsp; | UEK4 veya RHCK |
| Oracle | 7.0-7.1 veya daha yeni | &nbsp; | UEK4 veya RHCK w/[LIS4](https://www.microsoft.com/download/details.aspx?id=55106) |
| Oracle | 6.4-6.7 veya daha yeni | &nbsp; | UEK4 veya RHCK w/[LIS4](https://www.microsoft.com/download/details.aspx?id=55106) |

### <a name="lis-drivers-for-openlogic-centos"></a>OpenLogic CentOS için LIS sürücüleri

OpenLogic CentOS VM'leri çalıştırıyorsanız, en son sürücüleri yüklemek için aşağıdaki komutu çalıştırın:

```
sudo yum remove hypervkvpd  ## (Might return an error if not installed. That's OK.)
sudo yum install microsoft-hyper-v
sudo reboot
```

Bazı durumlarda yukarıdaki komut çekirdeği de yükseltecektir. Çekirdek güncelleştirmesi gerekiyorsa, microsoft-hyper-v paketini tam olarak yüklemek için yeniden başlatıldıktan sonra yukarıdaki komutları yeniden çalıştırmanız gerekebilir.


## <a name="disk-striping"></a>Disk şeritleme

Yüksek ölçekli bir VM birkaç premium depolama kalıcı diskleri ile bağlı olduğunda, diskler iOS' larını, bant genişliklerini ve depolama kapasitelerini toplamak için birlikte şeritlenebilir.

Windows'da, diskleri birlikte şeritlemek için Depolama Alanları'nı kullanabilirsiniz. Havuzdaki her disk için bir sütun yapılandırmanız gerekir. Aksi takdirde, diskler arasında trafiğin düzensiz dağılımı nedeniyle çizgili birimin genel performansı beklenenden düşük olabilir.

Önemli: Server Manager UI'yi kullanarak, çizgili bir birim için toplam sütun sayısını 8'e ayarlayabilirsiniz. Sekizden fazla disk takarken, ses düzeyini oluşturmak için PowerShell'i kullanın. PowerShell'i kullanarak, sütun sayısını disk sayısına eşit olarak ayarlayabilirsiniz. Örneğin, tek bir şerit kümesinde 16 disk varsa; *New-VirtualDisk* PowerShell cmdlet'in *NumberOfColumns* parametresinde 16 sütun belirtin.

Linux'ta, diskleri birlikte şeritlemek için MDADM yardımcı programını kullanın. Linux'ta diskleri şeritleme ile ilgili ayrıntılı adımlar [için, Linux'ta Yazılım RAID'i Yapılandır'a](../articles/virtual-machines/linux/configure-raid.md)bakın.

*Şerit Boyutu*  
Disk şeritlemede önemli bir yapılandırma şerit boyutudur. Şerit boyutu veya blok boyutu, uygulamanın çizgili bir birimde ele alabileceğiniz en küçük veri yığınıdır. Yapılandırdığınız şerit boyutu, uygulamanın türüne ve istek desenine bağlıdır. Yanlış şerit boyutunu seçerseniz, IO yanlış hizalanmasına neden olabilir ve bu da uygulamanızın performansında bozulmaya yol açabilir.

Örneğin, uygulamanız tarafından oluşturulan bir IO isteği disk şerit boyutundan daha büyükse, depolama sistemi bunu birden fazla diskte şerit birim sınırları boyunca yazar. Bu verilere erişim zamanı geldiğinde, isteği tamamlamak için birden fazla şerit birimi arasında aramak gerekir. Bu davranışın kümülatif etkisi önemli performans düşüşüne yol açabilir. Öte yandan, IO istek boyutu şerit boyutundan daha küçükse ve doğada rasgeleyse, IO istekleri aynı diskte bir soruna neden olabilir ve sonuçta IO performansını düşürebilir.

Uygulamanızın çalıştıran iş yükü türüne bağlı olarak, uygun bir şerit boyutu seçin. Rasgele küçük IO istekleri için daha küçük bir şerit boyutu kullanın. Büyük sıralı IO istekleri için ise daha büyük bir şerit boyutu kullanın. Premium Depolama'da çalıştıracağınız uygulama için şerit boyutu önerilerini öğrenin. SQL Server için, OLTP iş yükleri için 64 KB şerit boyutunu ve veri depolama iş yükleri için 256 KB'yi yapılandırın. Daha fazla bilgi edinmek [için Azure VM'lerde SQL Server için en iyi performans uygulamalarını](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md#disks-guidance) görün.

> [!NOTE]
> Bir DS serisi VM'de en fazla 32 premium depolama diskini ve GS serisi VM'de 64 premium depolama diskini bir araya getirebilirsiniz.

## <a name="multi-threading"></a>Çoklu iş parçacığı

Azure, Premium Depolama platformlarını büyük ölçüde paralel olacak şekilde tasarlamıştır. Bu nedenle, çok iş parçacığı uygulaması tek iş parçacığı uygulama çok daha yüksek performans elde eder. Çok iş parçacığı uygulaması görevlerini birden çok iş parçacığına böler ve VM ve disk kaynaklarını maksimuma kadar kullanarak yürütme verimliliğini artırır.

Örneğin, uygulamanız iki iş parçacığı kullanarak tek bir çekirdekVM üzerinde çalışıyorsa, CPU verimlilik elde etmek için iki iş parçacığı arasında geçiş yapabilir. Bir iş parçacığı nın tamamlanması için bir disk IO'su beklerken, CPU diğer iş parçacığına geçebilir. Bu şekilde, iki iş parçacığı tek bir iş parçacığı daha fazla gerçekleştirebilirsiniz. VM'de birden fazla çekirdek varsa, her çekirdek görevleri paralel olarak yürütebildiği için çalışma süresini daha da azaltır.

Hazır bir uygulamanın tek iş parçacığı veya çoklu iş parçacığı uygulama şeklini değiştiremeyebilirsiniz. Örneğin, SQL Server çok IŞLEMCIli ve çok çekirdekli işleme yeteneğine sahiptir. Ancak, SQL Server bir sorguişlemek için bir veya daha fazla iş parçacığı kaldıraç hangi koşullar altında karar verir. Sorguları çalıştırabilir ve çoklu iş parçacığı kullanarak dizinler oluşturabilir. Kullanıcıya dönmeden önce büyük tabloları birleştirmeyi ve verileri sıralamayı içeren bir sorgu için, SQL Server büyük olasılıkla birden çok iş parçacığı kullanır. Ancak, kullanıcı SQL Server'ın tek bir iş parçacığı mı yoksa birden çok iş parçacığı kullanarak sorgu kullanıp kullanmadığını denetleyemez.

Bir uygulamanın bu çok iş parçacığı veya paralel işleme etkilemek için değiştirebilirsiniz yapılandırma ayarları vardır. Örneğin, SQL Server durumunda paralellik yapılandırmasının en yüksek derecesidir. MAXDOP adı verilen bu ayar, PARALEL İşlem yaparken SQL Server'ın kullanabileceği maksimum işlemci sayısını yapılandırmanızı sağlar. MaxDOP'u tek tek sorgular veya dizin işlemleri için yapılandırabilirsiniz. Bu, performans açısından kritik bir uygulama için sisteminizin kaynaklarını dengelemek istediğinizde yararlıdır.

Örneğin, SQL Server'ı kullanan uygulamanızın aynı anda büyük bir sorgu ve dizin işlemi yürüttüğünüzün aynı zamanda olduğunu varsayın. Dizin çalışmasının büyük sorguya göre daha performant olmasını istediğinizi varsayalım. Böyle bir durumda, dizin işleminin MAXDOP değerini sorguiçin MAXDOP değerinden daha yüksek olarak ayarlayabilirsiniz. Bu şekilde, SQL Server büyük sorgu için adamak işlemci sayısı ile karşılaştırıldığında dizin işlemi için kaldıraç olabilir işlemcilerin daha fazla sayıda vardır. Unutmayın, SQL Server'ın her işlem için kullanacağı iş parçacığı sayısını denetlemezsiniz. Çoklu iş parçacığı için ayrılan maksimum işlemci sayısını kontrol edebilirsiniz.

SQL Server'da [Paralellik Dereceleri](https://technet.microsoft.com/library/ms188611.aspx) hakkında daha fazla bilgi edinin. Performansı optimize etmek için uygulamanızda ve yapılandırmalarında çoklu iş parçacığı etkileyen bu ayarları öğrenin.

## <a name="queue-depth"></a>Sıra derinliği

Sıra derinliği veya sıra uzunluğu veya sıra boyutu, sistemde bekleyen IO isteklerinin sayısıdır. Sıra derinliğinin değeri, uygulamanızın kaç IO işlemi düzenlenebildiğini ve depolama disklerinin hangisini işleteceğini belirler. Bu makalede ele aldığımız üç uygulama performans göstergesini de etkiler., IOPS, iş gücü ve gecikme süresi.

Sıra Derinliği ve çoklu iş parçacığı yakından ilişkilidir. Sıra Derinliği değeri, uygulama tarafından ne kadar çoklu iş parçacığı elde edilebildiğini gösterir. Sıra Derinliği büyükse, uygulama aynı anda daha fazla işlem yürütebilir, başka bir deyişle, daha çok iş parçacığı. Sıra Derinliği küçükse, uygulama çok iş parçacığı olsa bile, eşzamanlı yürütme için sıraya dizilmiş yeterli istek olmaz.

Genellikle, raf dışı uygulamalar sıra derinliğini değiştirmenize izin vermez, çünkü yanlış ayarlanırsa yarardan çok zarar verir. Uygulamalar, en iyi performansı elde etmek için sıra derinliğinin doğru değerini ayarlar. Ancak, uygulamanızla ilgili performans sorunlarını gidermeniz için bu kavramı anlamanız önemlidir. Ayrıca sisteminizde kıyaslama araçları çalıştırarak sıra derinliğinin etkilerini de gözlemleyebilirsiniz.

Bazı uygulamalar, Sıra Derinliğini etkileyecek ayarlar sağlar. Örneğin, SQL Server'daki MAXDOP (maksimum paralellik derecesi) ayarı önceki bölümde açıklanmıştır. MAXDOP, SQL Server'ın Sıra Derinliği değerini doğrudan değiştirmese de, Sıra Derinliği ni ve çoklu iş parçacığı nı etkilemenin bir yoludur.

*Yüksek sıra derinliği*  
Yüksek sıra derinliği diskte daha fazla işlemi sıralar. Disk, sıradaki bir sonraki isteği önceden bilir. Sonuç olarak, disk işlemleri önceden zamanlayabilir ve bunları en uygun sırada işleyebilir. Uygulama diske daha fazla istek gönderdiğinden, disk daha fazla paralel IOs işleyebilir. Sonuçta, uygulama daha yüksek IOPS elde etmek mümkün olacak. Uygulama daha fazla istek işlediğinden, uygulamanın toplam İşme Mesuliyeni de artar.

Genellikle, bir uygulama ekli disk başına 8-16 + olağanüstü IOs ile maksimum Throughput elde edebilirsiniz. Bir sıra derinliği bir ise, uygulama sisteme yeterli IOs iterek değildir ve belirli bir dönemde daha az miktarda işlenir. Başka bir deyişle, daha az Throughput.

Örneğin, SQL Server'da, bir sorguiçin MAXDOP değerini "4" olarak ayarlamak, SQL Server'a sorguyu yürütmek için en fazla dört çekirdek kullanabileceğini bildirir. SQL Server, en iyi sıra derinliği değerinin ve sorgu yürütmesinin çekirdek sayısını belirler.

*Optimum sıra derinliği*  
Çok yüksek sıra derinliği değeri de dezavantajları vardır. Sıra derinliği değeri çok yüksekse, uygulama çok yüksek IOPS sürmeye çalışır. Uygulamayeterli sağlanan IOPS ile kalıcı diskler yoksa, bu uygulama gecikmelerini olumsuz etkileyebilir. Aşağıdaki formül IOPS, gecikme ve sıra derinliği arasındaki ilişkiyi gösterir.  
    ![](media/premium-storage-performance/image6.png)

Kuyruk Derinliğini herhangi bir yüksek değere değil, gecikme gevelmelerini etkilemeden uygulama için yeterli IOPS sağlayabilen en uygun değere yapılandırmanız gerekir. Örneğin, uygulama gecikme süresinin 1 milisaniye olması gerekiyorsa, 5.000 IOPS elde etmek için gereken Sıra Derinliği, QD = 5000 x 0,001 = 5'tir.

*Çizgili Hacim için Sıra Derinliği*  
Çizgili bir birim için, her diskin tek tek en yüksek sıra derinliğine sahip olacak kadar yüksek bir sıra derinliğini koruyun. Örneğin, 2 sıra derinliği iten ve şeritte dört disk olan bir uygulama düşünün. İki IO isteği iki diske gider ve kalan iki disk boşta kalır. Bu nedenle, tüm disklerin meşgul olabileceği sıra derinliğini yapılandırın. Aşağıdaki formül, çizgili birimlerin sıra derinliğini nasıl belirleyirgösterin.  
    ![](media/premium-storage-performance/image7.png)

## <a name="throttling"></a>Azaltma

Azure Premium Depolama hükümleri, seçtiğiniz VM boyutlarına ve disk boyutlarına bağlı olarak belirtilen sayıda IOPS ve İş Mesuliyonu belirtir. Uygulamanız IOPS veya İş İlerletiyi VM veya diskin işleyebileceği bu sınırların üzerinde artırmaya çalıştığında, Premium Depolama bunu daraltacaktır. Bu, uygulamanızda bozulmuş performans şeklinde tezahür eder. Bu, daha yüksek gecikme, daha düşük İş Sonu veya daha düşük IOPS anlamına gelebilir. Premium Depolama azaltmazsa, kaynaklarının elde edebileceği miktarı aşarak uygulamanız tamamen başarısız olabilir. Bu nedenle, azaltma nedeniyle performans sorunları önlemek için, her zaman uygulamanız için yeterli kaynakları sağlamak. Yukarıdaki VM boyutları ve Disk boyutları bölümlerinde neler konuştuğumuzu göz önünde bulundurun. Karşılaştırma, uygulamanızı barındırmak için hangi kaynaklara ihtiyacınız olacağını belirlemenin en iyi yoludur.

## <a name="next-steps"></a>Sonraki adımlar
