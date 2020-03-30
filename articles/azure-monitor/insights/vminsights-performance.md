---
title: VM'ler için Azure Monitor ile performans grafiği nasıl çizilir?
description: Performans, Windows ve Linux sistemlerindeki uygulama bileşenlerini otomatik olarak keşfeden ve hizmetler arasındaki iletişimi eşleyen Sanal Bilgisayarlar için Azure Monitörünün bir özelliğidir. Bu makalede, çeşitli senaryolarda nasıl kullanılacağı hakkında ayrıntılı bilgi verilmektedir.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/15/2019
ms.openlocfilehash: a50ba39777e6a9d3d609e584c0c7d872f2a65f35
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80283727"
---
# <a name="how-to-chart-performance-with-azure-monitor-for-vms"></a>VM'ler için Azure Monitor ile performans grafiği nasıl çizilir?

Sanal Ayarlar için Azure Monitörü, sanal bir makinenin ne kadar iyi performans gösterdiğini belirlemenize yardımcı olmak için birkaç önemli performans göstergesini (KPI) hedefleyen bir performans grafiği kümesi içerir. Grafikler, darboğazları, anormallikleri tanımlayabilmeniz veya seçilen metrik ekime göre kaynak kullanımını görüntülemek için her makineyi listeleyen bir perspektife geçebilmeniz için belirli bir süre boyunca kaynak kullanımını gösterir. Performansla uğraşırken göz önünde bulundurulması gereken çok sayıda öğe olmakla birlikte, VM'ler için Azure Monitor işlemci, bellek, ağ bağdaştırıcısı ve disk kullanımıyla ilgili önemli işletim sistemi performans göstergelerini izler. Performans, sistem izleme özelliğini tamamlar ve olası bir sistem bileşeni arızası, verimlilik elde etmek için destek arızası ve optimizasyon veya destek kapasite planlamasını gösteren sorunların ortaya çıkarılmasına yardımcı olur.  

## <a name="multi-vm-perspective-from-azure-monitor"></a>Azure Monitor'dan Çoklu VM perspektifi

Azure Monitor'dan Performans özelliği, aboneliklerinizde veya çevrenizde çalışma grupları arasında dağıtılan tüm izlenen VM'lerin görünümünü sağlar. Azure Monitor'dan erişmek için aşağıdaki adımları gerçekleştirin. 

1. Azure portalında **Monitör'ü**seçin. 
2. **Çözümler** bölümünde **Sanal Makineler'i** seçin.
3. **Performans** sekmesini seçin.

![VM öngörüleri Performans Top N Listesi görünümü](media/vminsights-performance/vminsights-performance-aggview-01.png)

Üst **N Grafikler** sekmesinde, birden fazla Log Analytics çalışma alanınız varsa, sayfanın üst kısmındaki **Çalışma Alanı** seçicisinden çözümle birlikte etkinleştirilen çalışma alanını seçin. **Grup** seçici, bu sayfadaki ve diğer sayfalardaki grafiklerde sunulan sonuçları daha fazla filtrelemek için kullanabileceğiniz seçili çalışma alanıyla ilgili abonelikleri, kaynak gruplarını, [bilgisayar gruplarını](../platform/computer-groups.md)ve sanal makine ölçeği kümelerini döndürür. Seçiminiz yalnızca Performans özelliği için geçerlidir ve Sağlık veya Harita'ya taşımaz.  

Varsayılan olarak, grafikler son 24 saati gösterir. Zaman **Aralığı** seçicisini kullanarak, performansın geçmişte nasıl göründüğünü göstermek için 30 güne kadar olan geçmiş zaman aralıklarını sorgulayabilirsiniz.

Sayfada gösterilen beş kapasite kullanım grafiği şunlardır:

* CPU Kullanımı % - en yüksek ortalama işlemci kullanımı ile ilk beş makineleri gösterir 
* Kullanılabilir Bellek - kullanılabilir bellek en düşük ortalama miktarı ile ilk beş makineleri gösterir 
* Kullanılan Mantıksal Disk Alanı % - tüm disk hacimleri arasında kullanılan en yüksek ortalama disk alanına sahip ilk beş makineyi gösterir 
* Bayt Gönderilen Oran - gönderilen bayt ortalaması en yüksek olan ilk beş makineleri gösterir 
* Bayt Alma Oranı - alınan bayt ortalamasıen yüksek olan ilk beş makineyi gösterir 

Beş grafikten herhangi birinin sağ üst köşesindeki pin simgesine tıkladığınızda, seçili grafiği en son görüntülediğiniz son Azure panosuna sabitlersiniz.  Panodan grafiği yeniden boyutlandırAbilir ve yeniden konumlandırabilirsiniz. Gösterge tablosundan grafiği seçmek sizi VM'ler için Azure Monitör'e yönlendirir ve doğru kapsamı ve görünümü yükler.  

Beş grafikten herhangi birinde pin simgesinin solunda bulunan simgeye tıkladığınızda **Top N Listesi** görünümü açılır.  Burada, bir liste görünümünde tek tek VM tarafından bu performans ölçümü için kaynak kullanımını ve hangi makinenin en yüksek eğilim de olduğunu görürsünüz.  

![Seçili performans ölçümü için En İyi N Listesi görünümü](media/vminsights-performance/vminsights-performance-topnlist-01.png)

Sanal makineyi tıklattığınızda, **Özellikler** bölmesi, işletim sistemi tarafından bildirilen sistem bilgileri, Azure VM özellikleri vb. gibi seçilen öğenin özelliklerini gösterme hakkıyla genişletilir. **Hızlı Bağlantılar** bölümünün altındaki seçeneklerden birine tıkladığınızda, sizi doğrudan seçilen VM'den bu özelliğe yönlendirirsiniz.  

![Sanal Makine Özellikleri bölmesi](./media/vminsights-performance/vminsights-properties-pane-01.png)

Ortalama veya yüzdelik ölçerlere göre filtreuygulanmış performans ölçümlerini görüntülemek için **Toplu Grafikler** sekmesine geçin.  

![VM öngörüleri Performans Toplu görünümü](./media/vminsights-performance/vminsights-performance-aggview-02.png)

Aşağıdaki kapasite kullanım çizelgeleri sağlanır:

* CPU Kullanımı % - varsayılan ortalama ve üst 95 yüzdelik gösteren 
* Kullanılabilir Bellek - varsayılan ortalamayı, en iyi 5'i ve 10'uncu yüzdelik 
* Kullanılan Mantıksal Disk Alanı % - varsayılan ortalamayı ve 95 yüzdelik 
* Bayt Gönderilen Oran - gönderilen ortalama baytları gösteren varsayılanlar 
* Bayt Alma Oranı - alınan ortalama baytları gösteren varsayılanlar

Ayrıca **yüzdelik**seçici de Avg , **Min**, **Max**, **50th**, **90 th**, ve **95 seçerek** zaman aralığında grafiklerin parçalılık değiştirebilirsiniz.

Tek tek VM tarafından kaynak kullanımını liste görünümünde görüntülemek ve hangi makinenin en yüksek kullanımla eğilim içinde olduğunu görmek için **En İyi N Listesi** sekmesini seçin.  **En İyi N Listesi** sayfası, metrik CPU Kullanımı % 95'inin en çok *CPU Utilization %* kullandığı na göre sıralanmış en iyi 20 makineyi gösterir.  **Daha Fazla Yükle'yi**seçerek daha fazla makine görebilirsiniz ve sonuçlar en iyi 500 makineyi göstermek için genişletir. 

>[!NOTE]
>Liste aynı anda 500'den fazla makine gösteremez.  
>

![En İyi N Listesi sayfa örneği](./media/vminsights-performance/vminsights-performance-topnlist-01.png)

Listedeki belirli bir sanal makinedeki sonuçları filtrelemek için, bilgisayar adını **ad** textbox'a girin.  

Farklı bir performans ölçümünden kullanımı görüntülemek istiyorsanız, **Metrik** açılır listeden **Kullanılabilir Bellek,** **Kullanılan Mantıksal Disk Alanı %**, Ağ Alınan **Byte/s**veya **Bu** metriğe kapsamlı kullanımı göstermek için liste güncelleştirmelerini seçin.  

Listeden sanal bir makine seçmek sayfanın sağ tarafındaki **Özellikler** panelini açar ve buradan **Performans ayrıntısını**seçebilirsiniz.  **Sanal Makine Ayrıntısı** sayfası açılır ve vm öngörüleri performansına doğrudan Azure VM'den erişirken benzer şekilde bu VM'ye de dahil edilir.  

## <a name="view-performance-directly-from-an-azure-vm"></a>Performansı doğrudan bir Azure VM'den görüntüleme

Doğrudan sanal bir makineden erişmek için aşağıdaki adımları gerçekleştirin.

1. Azure portalında **Sanal Makineler'i**seçin. 
2. Listeden bir VM seçin ve **İzleme** bölümünde **Öngörüler'i**seçin.  
3. **Performans** sekmesini seçin. 

Bu sayfa yalnızca performans kullanım grafiklerini değil, aynı zamanda keşfedilen her mantıksal disk için, kapasitesini, kullanımını ve her ölçüye göre toplam ortalamayı gösteren bir tabloyu da içerir.  

Aşağıdaki kapasite kullanım çizelgeleri sağlanır:

* CPU Kullanımı % - varsayılan ortalama ve üst 95 yüzdelik gösteren 
* Kullanılabilir Bellek - varsayılan ortalamayı, en iyi 5'i ve 10'uncu yüzdelik 
* Kullanılan Mantıksal Disk Alanı % - varsayılan ortalamayı ve 95 yüzdelik 
* Mantıksal Disk IOPS - varsayılan ortalama ve 95 yüzdelik gösteren
* Mantıksal Disk MB/s - varsayılan ortalamayı ve 95 yüzdelik
* Max Mantıksal Disk Kullanılan % - varsayılan ortalama ve 95 yüzdelik gösteren
* Bayt Gönderilen Oran - gönderilen ortalama baytları gösteren varsayılanlar 
* Bayt Alma Oranı - alınan ortalama baytları gösteren varsayılanlar

Grafiklerden herhangi birinin sağ üst köşesindeki pin simgesine tıkladığınızda, seçili grafiği görüntülediğiniz son Azure panosuna sabitler. Panodan grafiği yeniden boyutlandırAbilir ve yeniden konumlandırabilirsiniz. Gösterge tablosundan grafiğin seçilmesi sizi VM'ler için Azure Monitör'e yönlendirir ve VM'nin performans ayrıntı görünümünü yükler.  

![VM öngörüleri Performans doğrudan VM görünümünden](./media/vminsights-performance/vminsights-performance-directvm-01.png)

## <a name="view-performance-directly-from-an-azure-virtual-machine-scale-set"></a>Performansı doğrudan Azure sanal makine ölçeği kümesinden görüntüleme

Azure sanal makine ölçeği kümesinden doğrudan erişmek için aşağıdaki adımları gerçekleştirin.

1. Azure portalında **Sanal makine ölçek kümelerini**seçin.
2. Listeden bir VM seçin ve **İzleme** bölümünde **Performans** sekmesini görüntülemek için **Öngörüler'i** seçin.

Bu sayfa, seçili ölçek kümesine göre kapsamlı Azure Monitor performans görünümünü yükler. Bu, izlenen ölçümler kümesi nde ayarlanan ölçekte En İyi N Örnekleri'ni görmenizi, ölçek kümesi genelinde toplam performansı görüntülemenizi ve ölçek kümesinde tek tek örnekler arasında seçili ölçümlerin eğilimlerini görmenizi sağlar. Liste görünümünden bir örnek seçmek, haritasını yüklemenize veya bu örnek için ayrıntılı bir performans görünümüne girmenize olanak tanır.

Grafiklerden herhangi birinin sağ üst köşesindeki pin simgesine tıkladığınızda, seçili grafiği görüntülediğiniz son Azure panosuna sabitler. Panodan grafiği yeniden boyutlandırAbilir ve yeniden konumlandırabilirsiniz. Gösterge tablosundan grafiğin seçilmesi sizi VM'ler için Azure Monitör'e yönlendirir ve VM'nin performans ayrıntı görünümünü yükler.  

![VM öngörüleri Doğrudan sanal makine ölçeği set görünümünden performans](./media/vminsights-performance/vminsights-performance-directvmss-01.png)

>[!NOTE]
>Ayrıca, ölçek kümeniz için Örnekler görünümünden belirli bir örnek için ayrıntılı bir performans görünümüne de erişebilirsiniz. **Ayarlar** bölümündeki **Örneklere** gidin ve ardından **İstatistikler'i**seçin.



## <a name="next-steps"></a>Sonraki adımlar

- Performansı ve ağ ölçümlerini daha fazla analiz etmek için VM'ler için Azure Monitor'a dahil olan [Çalışma Kitaplarını](vminsights-workbooks.md) nasıl kullanacağınızı öğrenin.  

- Keşfedilen uygulama bağımlılıkları hakkında bilgi edinmek [için VM Haritası için Azure Monitörünü Görüntüle'ye](vminsights-maps.md)bakın.
