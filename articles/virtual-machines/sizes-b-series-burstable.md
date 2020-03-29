---
title: B serisi patlanabilir - Azure Sanal Makineler
description: B serisi patlamaya uygun Azure VM boyutlarını açıklar.
services: virtual-machines
author: ayshakeen
manager: gwallace
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: ayshak
ms.openlocfilehash: 5121ef8eb3123d830233e91968c69b331d255bd0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78161089"
---
# <a name="b-series-burstable-virtual-machine-sizes"></a>B serisi patlatabilen sanal makine boyutları

B serisi VM'ler, web sunucuları, kavramların kanıtı, küçük veritabanları ve geliştirme oluşturma ortamları gibi CPU'nun tam performansına sürekli olarak ihtiyaç duymayan iş yükleri için idealdir. Bu iş yükleri genellikle burstable performans gereksinimleri vardır. B serisi, temel performansa sahip bir VM boyutu satın alma olanağı sağlar ve VM örneği, taban çizgisinden daha az ını kullandığında kredi oluşturur. VM'nin birikmiş kredisi olduğunda, uygulamanız daha yüksek CPU performansı gerektirdiğinde VM vCPU'nun %100'üne kadarını kullanarak taban çizgisinin üzerine çıkabilir.

B serisi aşağıdaki VM boyutlarında gelir:

Premium Depolama: Desteklenen

Premium Depolama önbelleğe alma: Desteklenmiyor

Canlı Geçiş: Desteklenen

Bellek Koruma Güncelleştirmeleri: Desteklenen

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | VM'nin Temel CPU Perf'i | VM'nin Maksimum CPU Perf'i | İlk Krediler | Bankaya yatırılan krediler/saat | Max Banked Krediler | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama iş baçması: IOPS/MBps | Maksimum cached disk işçıktısı: IOPS/MBps | En fazla NIC |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Standard_B1ls<sup>1</sup> | 1  | 0,5 | 4   | %5   | %100  | 30  | 3   | 72   | 2  | 200/10    | 160/10    | 2  |
| Standard_B1s              | 1  | 1   | 4   | %10  | %100  | 30  | 6   | 144  | 2  | 400/10    | 320/10    | 2  |
| Standard_B1ms             | 1  | 2   | 4   | %20  | %100  | 30  | 12  | 288  | 2  | 800/10    | 640/10    | 2  |
| Standard_B2s              | 2  | 4   | 8   | %40  | %200  | 60  | 24  | 576  | 4  | 1600/15   | 1280/15   | 3  |
| Standard_B2ms             | 2  | 8   | 16  | %60  | %200  | 60  | 36  | 864  | 4  | 2400/22.5 | 1920/22.5 | 3  |
| Standard_B4ms             | 4  | 16  | 32  | %90  | 400%  | 120 | 54  | 1296 | 8  | 3600/35   | 2880/35   | 4  |
| Standard_B8ms             | 8  | 32  | 64  | 135% | 800%  | 240 | 81  | 1944 | 16 | 4320/50   | 4320/50   | 4  |
| Standard_B12ms            | 12 | 48  | 96  | 202% | 1200% | 360 | 121 | 2909 | 16 | 6480/75   | 4320/50   | 6  |
| Standard_B16ms            | 16 | 64  | 128 | 270% | 1600% | 480 | 162 | 3888 | 32 | 8640/100  | 4320/50   | 8  |
| Standard_B20ms            | 20 | 80  | 160 | 337% | %2000 | 600 | 203 | 4860 | 32 | 10800/125 | 4320/50   | 8  |

<sup>1</sup> B1ls sadece Linux'ta desteklenir

## <a name="workload-example"></a>İş yükü örneği

Bir ofis check-in/out uygulamasını düşünün. Uygulama nın çalışma saatleri içinde CPU patlamalarına ihtiyacı vardır, ancak çalışma saatleri dışında çok fazla bilgi işlem gücü gerekmez. Bu örnekte, iş yükü verimli çalışması için 64GiB RAM ile 16vCPU sanal makine gerektirir.

Tablo saatlik trafik verilerini gösterir ve grafik bu trafiğin görsel bir temsilidir.

B16 özellikleri:

Maksimum CPU perf: 16vCPU * 100% = 1600%

Taban çizgisi: 270%

![Saatlik trafik verilerinin grafiği](./media/b-series-burstable/office-workload.png)

| Senaryo | Zaman | CPU kullanımı (%) | Birikmiş krediler<sup>1</sup> | Mevcut krediler |
| --- | --- | --- | --- | --- |
| B16ms Dağıtım | Dağıtım | Dağıtım  | 480 (İlk Kredi) | 480 |
| Trafik yok | 0:00 | 0 | 162 | 642 |
| Trafik yok | 1:00 | 0 | 162 | 804 |
| Trafik yok | 2:00 | 0 | 162 | 966 |
| Trafik yok | 3:00 | 0 | 162 | 1128 |
| Trafik yok | 4:00 | 0 | 162 | 1290 |
| Trafik yok | 5:00 | 0 | 162 | 1452 |
| Düşük Trafik | 6:00 | 270 | 0 | 1452 |
| Çalışanlar ofise gelir (uygulama %80 vCPU'ya ihtiyaç duyar) | 7:00 | 1280 | -606 | 846 |
| Çalışanlar ofise gelmeye devam ediyor (uygulamanın %80 vCPU'ya ihtiyacı var) | 8:00 | 1280 | -606 | 240 |
| Düşük Trafik | 9:00 | 270 | 0 | 240 |
| Düşük Trafik | 10:00 | 100 | 102 | 342 |
| Düşük Trafik | 11:00 | 50 | 132 | 474 |
| Düşük Trafik | 12:00 | 100 | 102 | 576 |
| Düşük Trafik | 13:00 | 100 | 102 | 678 |
| Düşük Trafik | 14:00 | 50 | 132 | 810 |
| Düşük Trafik | 15:00 | 100 | 102 | 912 |
| Düşük Trafik | 16:00 | 100 | 102 | 1014 |
| Çalışanlar check-out yapıyor (uygulamanın %100 vCPU'ya ihtiyacı var) | 17:00 | 1600 | -798 | 216 |
| Düşük Trafik | 18:00 | 270 | 0 | 216 |
| Düşük Trafik | 19:00 | 270 | 0 | 216 |
| Düşük Trafik | 20:00 | 50 | 132 | 348 |
| Düşük Trafik | 21:00 | 50 | 132 | 480 |
| Trafik yok | 22:00 | 0 | 162 | 642 |
| Trafik yok | 23:00 | 0 | 162 | 804 |

<sup>1</sup> Bir saat içinde biriken/kullanılan `((Base CPU perf of VM - CPU Usage) / 100) * 60 minutes`krediler: .  

16 vCPUs ve 64 GiB bellek saatlik oranı olan bir D16s_v3 için saat ücreti 0,936 $ (aylık 673,92 $) ve B16ms için 16 vCPUs ve 64 GiB bellek oranı 0,794 $ saat başına (aylık 547,86 $) olduğunu. <b>Bu da %15 tasarruf sağlar!</b>

## <a name="q--a"></a>Soru-Cevap

### <a name="q-how-do-you-get-135-baseline-performance-from-a-vm"></a>S: VM'den %135 temel performansı nasıl elde edeyim?

**C**: %135'lik pay, VM boyutunu oluşturan 8 vCPU arasında paylaşılır. Örneğin, uygulamanız toplu işleme üzerinde çalışan 8 çekirdeğin 4'ünü kullanıyorsa ve bu 4 vCPU'nun her biri %30 kullanımla çalışıyorsa, toplam VM CPU performansı miktarı %120'ye eşit olacaktır.  Yani VM'niz, temel performansınızdan elde edilen %15 delta'ya göre kredi süresini oluşturur.  Ama aynı zamanda aynı VM tüm 8 vCPU%100 bu VM% 800 Max CPU performansı vererek kullanabilirsiniz kredi var demektir.


### <a name="q-how-can-i-monitor-my-credit-balance-and-consumption"></a>S: Kredi bakiyemi ve tüketimimi nasıl izleyebilirim?

**C**: Önümüzdeki haftalarda 2 yeni ölçüm sunacağız, **Kredi** ölçümü VM'nizin kaç kredi bankaya yatırdığını görüntülemenize olanak sağlayacak ve **ConsumedCredit** ölçümü VM'nizin bankadan kaç CPU kredisi tükettiğini gösterecektir.    Bu ölçümleri portaldaki ölçümler bölmesinden veya Azure Monitör API'leri aracılığıyla programlı olarak görüntüleyebilirsiniz.

Azure'un metrik verilerine nasıl erişilisürüğe ilişkin daha fazla bilgi için Microsoft [Azure'daki ölçümlere Genel Bakış](../azure-monitor/platform/data-platform.md)bölümüne bakın.

### <a name="q-how-are-credits-accumulated"></a>S: Krediler nasıl birikir?

**C**: VM birikim ve tüketim oranları, tam olarak temel performans düzeyinde çalışan bir VM'nin net bir birikim veya patlama kredisi tüketimiolmayacak şekilde ayarlanır.  Bir VM, temel performans düzeyinin altında çalışırken kredilerde net bir artış alacaktır ve VM CPU'yu temel performans seviyesinden daha fazla kullandığında kredilerde net bir düşüş olacaktır.

**Örnek**: Küçük zaman ve katılım veritabanı uygulamam için B1ms boyutunu kullanarak bir VM dağıtıyorum. Bu boyut, uygulamamın bir vCPU'nun %20'sine kadar ını temel olarak kullanmasına izin verir, bu da dakikada 0,2 kredi veya banka olarak kullanılır.

Başvurum, çalışanlarımın iş günü başında ve sonunda, 07:00-09:00 ve 04:00 -18:00 saatleri arasında yoğundur. Günün diğer 20 saat boyunca, benim uygulama genellikle boşta, sadece vCPU% 10 kullanarak. Yoğun olmayan saatler için, dakikada 0,2 kredi kazanıyorum ama sadece dakikada 0,l kredi tüketiyorum, bu yüzden VM'im saatte 0,1 x 60 = 6 kredi yatıracak.  Zirveden indiğim 20 saat boyunca 120 kredi yatıracağım.  

Yoğun saatlerde benim uygulama ortalamaları 60% vCPU kullanımı, hala dakikada 0,2 kredi kazanmak ama dakikada 0,6 kredi tüketmek, 0,4 kredi bir dakika veya 0,4 x 60 = saatte 24 kredi net maliyet için. Ben pik kullanım günde 4 saat var, bu yüzden benim pik kullanım için 4 x 24 = 96 kredi maliyeti.

Eğer kazandığım 120 krediyi zirve dışında alırsam ve yoğun zamanlar için kullandığım 96 krediyi çıkarırsam, diğer faaliyet patlamaları için kullanabileceğim günde 24 kredi daha yatırıyorum.

### <a name="q-how-can-i-calculate-credits-accumulated-and-used"></a>S: Birikmiş ve kullanılan kredileri nasıl hesaplayabilirim?

**C**: Aşağıdaki formülü kullanabilirsiniz:

(VM'nin Temel CPU perf'i - CPU Kullanımı) / 100 = Krediler banka veya dakika başına kullanım

örneğin, yukarıdaki örnekte taban çizginiz %20'dir ve CPU'nun %10'unu kullanırsanız biriktiğiniz cpu (20%-10%/100 = 0,1 kredi) dakikada.

### <a name="q-does-the-b-series-support-premium-storage-data-disks"></a>S: B Serisi Premium Depolama veri disklerini destekliyor mu?

**C**: Evet, tüm B-Serisi boyutları Premium Depolama veri disklerini destekler.

### <a name="q-why-is-my-remaining-credit-set-to-0-after-a-redeploy-or-a-stopstart"></a>S: Kalan kredim neden yeniden dağıtıldıktan veya bir stop/start'tan sonra 0 olarak ayarlandı?

**C** : Bir VM "REDPLOYED" olduğunda ve VM başka bir düğüme geçtiğinde, birikmiş kredi kaybolur. VM durdurulur/başlatılırsa, ancak aynı düğümüzerinde kalırsa, VM birikmiş krediyi korur. VM bir düğüm üzerinde taze başladığında, bir başlangıç kredi alır, Standard_B8ms için 240 dakika.

### <a name="q-what-happens-if-i-deploy-an-unsupported-os-image-on-b1ls"></a>S: B1ls üzerinde desteklenmeyen bir işletim sistemi görüntüsü dağılasam ne olur?

**C** : B1ls yalnızca Linux görüntülerini destekler ve başka bir işletim sistemi görüntüsü dağıtursanız en iyi müşteri deneyimini elde eyinemeyebilirsiniz.

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaç](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure bilgi [işlem birimlerinin (ACU)](acu.md) Azure SK'leri genelinde bilgi işlem performansını karşılaştırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin.