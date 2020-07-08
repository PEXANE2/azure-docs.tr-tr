---
title: VM'ler için Azure İzleyici ile performansı grafik oluşturma
description: Performans, Windows ve Linux sistemlerinde uygulama bileşenlerini otomatik olarak bulan ve hizmetler arasındaki iletişimi eşleyen VM'ler için Azure İzleyici bir özelliğidir. Bu makalede, çeşitli senaryolarda nasıl kullanılacağına ilişkin ayrıntılar sağlanmaktadır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/31/2020
ms.openlocfilehash: 1ea6d09609d1b7b3f7ba7297a040447d1fc24756
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83684385"
---
# <a name="how-to-chart-performance-with-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici ile performansı grafik oluşturma

VM'ler için Azure İzleyici, bir sanal makinenin ne kadar iyi performans gösterdiğini belirlemenize yardımcı olmak üzere birkaç ana performans göstergelerini (KPI) hedefleyen bir performans grafikleri kümesi içerir. Grafikler, kaynak kullanımını her bir süre içinde gösterir; böylece, seçili ölçüm temelinde kaynak kullanımını görüntülemek için her bir makinenin bir perspektifine geçiş yapabilirsiniz. Performansla ilgilenirken göz önünde bulundurmanız gereken çok sayıda öğe olsa da, VM'ler için Azure İzleyici işlemci, bellek, ağ bağdaştırıcısı ve disk kullanımı ile ilgili anahtar işletim sistemi performans göstergelerini izler. Performans, sistem durumu izleme özelliğini tamamlar ve olası sistem bileşeni başarısızlığını belirten sorunları açığa çıkarır, verimlilik elde etmek için ayarlama ve iyileştirmeyi destekler veya Kapasite planlamasını destekler.  

## <a name="limitations"></a>Sınırlamalar
VM'ler için Azure İzleyici ile performans toplama sınırlamaları aşağıda verilmiştir.

- Red Hat Linux (RHEL) 6 çalıştıran sanal makinelerde **kullanılabilir bellek** yok. Bu ölçüm, [çekirdek sürümü 3,14](http://www.man7.org/linux/man-pages/man1/free.1.html)' de tanıtılan **memavailable** öğesinden hesaplanır.
- Ölçümler yalnızca EXT dosya sistemi ailesi (EXT2, EXT3, EXT4) kullanan Linux sanal makinelerinde veri diskleri için kullanılabilir.

## <a name="multi-vm-perspective-from-azure-monitor"></a>Azure Izleyici 'den çoklu VM perspektifi

Azure Izleyici 'den performans özelliği, aboneliklerinizde veya ortamınızda çalışma gruplarında dağıtılan tüm izlenen VM 'lerin bir görünümünü sağlar. Azure Izleyici 'den erişmek için aşağıdaki adımları uygulayın. 

1. Azure portal, **İzle**' yi seçin. 
2. **Çözümler** bölümünde **sanal makineler** ' i seçin.
3. **Performans** sekmesini seçin.

![VM Insights performansı üst N liste görünümü](media/vminsights-performance/vminsights-performance-aggview-01.png)

**Ilk N grafik** sekmesinde, birden fazla Log Analytics çalışma alanınız varsa, sayfanın üst kısmındaki **çalışma alanı** seçicisindeki çözümle birlikte etkin çalışma alanını seçin. **Grup** Seçicisi, bu sayfadaki grafiklerde ve diğer sayfalarda sunulan sonuçları daha fazla filtrelemek için kullanabileceğiniz seçili çalışma alanıyla ilgili abonelikler, kaynak grupları, [bilgisayar grupları](../platform/computer-groups.md)ve sanal makine ölçek kümelerini döndürür. Seçiminiz yalnızca performans özelliği için geçerlidir ve sistem durumu veya eşleme üzerinde kalmaz.  

Varsayılan olarak, grafikler son 24 saati gösterir. **Timerange** seçiciyi kullanarak, performansın geçmişte nasıl arandığı hakkında en fazla 30 gün geçmiş zaman aralıklarını sorgulama yapabilirsiniz.

Sayfada gösterilen beş kapasite kullanımı grafiği şunlardır:

* CPU kullanımı%-en yüksek ortalama işlemci kullanımına sahip ilk beş makineyi gösterir 
* Kullanılabilir bellek-en düşük ortalama kullanılabilir bellek miktarına sahip ilk beş makineyi gösterir 
* Kullanılan mantıksal disk alanı%-tüm disk birimlerinde en yüksek ortalama disk alanı% kullanılan beş makineyi gösterir 
* Gönderilen bayt oranı-en yüksek ortalama bayt gönderilen beş makineyi gösterir 
* Bayt alma hızı-en yüksek ortalama bayt alınan beş makineyi gösterir 

Beş grafiğin sağ üst köşesindeki sabitleme simgesine tıkladığınızda seçili grafik, son görüntülediğiniz son Azure panosuna sabitedilir.  Panodan grafiği yeniden boyutlandırabilir ve yeniden konumlandırabilirsiniz. Panodan grafik seçilmesi, sizi VM'ler için Azure İzleyici ve doğru kapsamı ve görünümü yüklemek için yönlendirir.  

Beş grafikten birindeki sabitleme simgesinin solunda bulunan simgeye tıkladığınızda, **üstteki N liste** görünümü açılır.  Burada, bu performans ölçüsünün kaynak kullanımını bir liste görünümünde tek bir VM tarafından ve hangi makinenin en yüksek düzeyde son olduğunu görürsünüz.  

![Seçili performans ölçümü için ilk N liste görünümü](media/vminsights-performance/vminsights-performance-topnlist-01.png)

Sanal makineye tıkladığınızda, **Özellikler** bölmesi, seçilen öğenin özelliklerini (örneğin, işletim sistemi tarafından bildirilen sistem bilgileri, Azure VM 'nin özellikleri vb.) göstermek için sağda genişletilir. **Hızlı bağlantılar** bölümünün altındaki seçeneklerden birine tıkladığınızda, sizi doğrudan seçilen VM 'deki bu özelliğe yönlendirebilirsiniz.  

![Sanal makine özellikleri bölmesi](./media/vminsights-performance/vminsights-properties-pane-01.png)

Ortalama veya yüzdebirlik değeri ölçülere göre filtrelenen performans ölçümlerini görüntülemek için **toplanmış grafikler** sekmesine geçin.  

![VM öngörüleri performansı toplam görünümü](./media/vminsights-performance/vminsights-performance-aggview-02.png)

Aşağıdaki kapasite kullanım grafikleri verilmiştir:

* CPU kullanımı%-ortalama ve ilk 95. yüzdebirlik değerlerini gösteren varsayılanlar 
* Kullanılabilir bellek-ortalama, en çok 5 ve 10 yüzdebirlik değerlerini gösteren varsayılanlar 
* Kullanılan mantıksal disk alanı%-ortalama ve 95. yüzdebirlik değerlerini gösterir 
* Gönderilen bayt sayısı-varsayılan değer gönderilen ortalama bayt 
* Bayt alma oranı-alınan ortalama bayt sayısını gösteren varsayılanlar

Ayrıca, yüzdebirlik seçicideki **AVG**, **Min**, **Max**, **50. Yüzdeliğini**, **90**TH ve **95** ' i seçerek zaman aralığındaki grafiklerin ayrıntı düzeyini de değiştirebilirsiniz.

Bir liste görünümünde tek bir VM tarafından kaynak kullanımını görüntülemek ve en yüksek kullanım ile hangi makinenin hangi makineye eğilmekte olduğunu görmek için, **Ilk N liste** sekmesini seçin.  **Ilk N liste** sayfası, ölçüm *CPU kullanımı%* için en çok yüzde 95 ' lik olan en fazla 80 ' e sıralanan ilk 20 makineyi gösterir.  **Daha fazla yükle**' yi seçerek daha fazla makine görebilirsiniz ve sonuçlar üst 500 makineleri gösterecek şekilde genişletilir. 

>[!NOTE]
>Listede aynı anda 500 ' den fazla makine gösterilemez.  
>

![İlk N liste sayfası örneği](./media/vminsights-performance/vminsights-performance-topnlist-01.png)

Listedeki belirli bir sanal makinede bulunan sonuçlara filtre uygulamak için **ada göre ara** metin kutusuna bilgisayar adını girin.  

Farklı bir performans ölçümünün kullanımını görüntülemeyi tercih ediyorsanız, **ölçüm** açılan listesinden **kullanılabilir bellek**, **kullanılan mantıksal disk alanı%**, **alınan ağ bayt/sn**veya **ağ tarafından gönderilen bayt/sn** ve bu ölçüm için kapsamlı kullanımı göstermek için liste güncelleştirmelerini seçin.  

Listeden bir sanal makine seçmek, sayfanın sağ tarafındaki **Özellikler** panelini açar ve buradan **performans ayrıntısı**' nı seçebilirsiniz.  **Sanal makine ayrıntısı** sayfası açılır ve bu VM KAPSAMıNDA, VM Öngörüler performansına doğrudan Azure VM 'den erişilirken deneyim de buna benzer.  

## <a name="view-performance-directly-from-an-azure-vm"></a>Performansı doğrudan bir Azure VM 'den görüntüleme

Doğrudan bir sanal makineden erişmek için aşağıdaki adımları gerçekleştirin.

1. Azure portal **sanal makineler**' i seçin. 
2. Listeden bir sanal makine seçin ve **izleme** bölümünde **Öngörüler**' i seçin.  
3. **Performans** sekmesini seçin. 

Bu sayfa yalnızca performans kullanım grafiklerini içermez, aynı zamanda bulunan her bir mantıksal disk için bir tablo, kapasitesi, kullanımı ve her bir ölçünün toplam ortalaması dahil değildir.  

Aşağıdaki kapasite kullanım grafikleri verilmiştir:

* CPU kullanımı%-ortalama ve ilk 95. yüzdebirlik değerlerini gösteren varsayılanlar 
* Kullanılabilir bellek-ortalama, en çok 5 ve 10 yüzdebirlik değerlerini gösteren varsayılanlar 
* Kullanılan mantıksal disk alanı%-ortalama ve 95. yüzdebirlik değerlerini gösterir 
* Mantıksal disk ıOPS-ortalama ve 95. yüzdebirlik değerlerini gösteren varsayılanlar
* Mantıksal disk MB/s-ortalama ve 95. yüzdebirlik değerlerini gösteren varsayılanlar
* En büyük mantıksal disk kullanıldı%-ortalama ve 95. yüzdebirlik değerlerini gösteren varsayılanlar
* Gönderilen bayt sayısı-varsayılan değer gönderilen ortalama bayt 
* Bayt alma oranı-alınan ortalama bayt sayısını gösteren varsayılanlar

Grafiklerin sağ üst köşesindeki sabitleme simgesine tıkladığınızda seçili grafik, görüntülediğiniz son Azure panosuna sabitindedir. Panodan grafiği yeniden boyutlandırabilir ve yeniden konumlandırabilirsiniz. Panodaki grafiğin seçilmesi, sanal makine için performans ayrıntısı görünümünü VM'ler için Azure İzleyici ve yükler.  

![VM Insights performansını doğrudan VM görünümünden](./media/vminsights-performance/vminsights-performance-directvm-01.png)

## <a name="view-performance-directly-from-an-azure-virtual-machine-scale-set"></a>Performansı doğrudan bir Azure sanal makine ölçek kümesinden görüntüleme

Doğrudan bir Azure sanal makine ölçek kümesinden erişmek için aşağıdaki adımları gerçekleştirin.

1. Azure portal, **Sanal Makine Ölçek Kümeleri**' ni seçin.
2. Listeden bir sanal makine seçin ve **izleme** bölümünde, **performans** sekmesini görüntülemek için **Öngörüler** ' i seçin.

Bu sayfa, seçilen ölçek kümesine kapsamındaki Azure Izleyici performans görünümünü yükler. Bu, ölçek kümesinde izlenen ölçümler kümesi genelinde Ilk N örneği görmenizi, ölçek kümesi genelinde toplam performansı görüntülemenizi ve ölçek kümesinin tek tek örnekleri genelinde seçili ölçümler için eğilimleri görmenizi sağlar. Liste görünümünden bir örnek seçmek, bu örneğin eşlemesini yüklemenize veya bu örnek için ayrıntılı bir performans görünümüne gitmenizi sağlar.

Grafiklerin sağ üst köşesindeki sabitleme simgesine tıkladığınızda seçili grafik, görüntülediğiniz son Azure panosuna sabitindedir. Panodan grafiği yeniden boyutlandırabilir ve yeniden konumlandırabilirsiniz. Panodaki grafiğin seçilmesi, sanal makine için performans ayrıntısı görünümünü VM'ler için Azure İzleyici ve yükler.  

![VM öngörüleri performansını doğrudan sanal makine ölçek kümesi görünümünden](./media/vminsights-performance/vminsights-performance-directvmss-01.png)

>[!NOTE]
>Ayrıca, ölçek kümesi için örnek görünümünden belirli bir örnek için ayrıntılı bir performans görünümüne erişebilirsiniz. **Ayarlar** bölümünün altındaki **örneklere** gidin ve ardından **Öngörüler**' i seçin.



## <a name="next-steps"></a>Sonraki adımlar

- Performans ve ağ ölçümlerini daha fazla analiz etmek için VM'ler için Azure İzleyici eklenen [çalışma kitaplarını](vminsights-workbooks.md) nasıl kullanacağınızı öğrenin.  

- Bulunan uygulama bağımlılıkları hakkında bilgi edinmek için bkz. [VM'ler için Azure izleyici haritasını görüntüleme](vminsights-maps.md).
