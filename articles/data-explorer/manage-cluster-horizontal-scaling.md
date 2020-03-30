---
title: Azure Veri Gezgini'ndeki talebi eşleştirmek için küme yatay ölçekleme (ölçeklendirme) yönetme
description: Bu makalede, değişen talebe göre bir Azure Veri Gezgini kümesinde ölçeklendirme ve ölçeklendirme adımları açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 12/09/2019
ms.openlocfilehash: ff7420619cffc2287ab8ff6332df605d56329549
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77664142"
---
# <a name="manage-cluster-horizontal-scaling-scale-out-in-azure-data-explorer-to-accommodate-changing-demand"></a>Değişen talebi karşılamak için Azure Veri Gezgini'nde küme yatay ölçekleme (ölçekleme) yönetme

Bir kümeyi uygun şekilde boyutlandırmak, Azure Veri Gezgini'nin performansı için çok önemlidir. Statik küme boyutu, ikisi de ideal olmayan az kullanıma veya aşırı kullanıma yol açabilir. Bir kümedeki talep mutlak doğrulukla tahmin edilemedığından, değişen taleple kapasite ve CPU kaynakları ekleyerek ve kaldırarak bir kümeyi *ölçeklendirmek* daha iyidir. 

Azure Veri Gezgini kümesini ölçekleme için iki iş akışı vardır: 
* Yatay ölçekleme, aynı zamanda ölçekleme olarak adlandırılır.
* [Dikey ölçekleme,](manage-cluster-vertical-scaling.md)yukarı ve aşağı ölçekleme olarak da adlandırılır.
Bu makalede, yatay ölçekleme iş akışı açıklanmaktadır.

## <a name="configure-horizontal-scaling"></a>Yatay ölçekleme yapılandırma

Yatay ölçeklendirme kullanarak, önceden tanımlanmış kurallara ve zamanlamalara göre örnek sayısını otomatik olarak ölçeklendirebilirsiniz. Kümenizin otomatik ölçek ayarlarını belirtmek için:

1. Azure portalında Azure Veri Gezgini küme kaynağınıza gidin. **Ayarlar**altında, **Ölçeklendir'i**seçin. 

2. Ölçek **çıkış** penceresinde, istediğiniz otomatik ölçek yöntemini seçin: **El ile ölçek,** **Optimize edilmiş otomatik ölçek,** veya **Özel otomatik ölçek.**

### <a name="manual-scale"></a>Manuel ölçek

El ile ölçek küme oluşturma sırasında varsayılan ayardır. Küme otomatik olarak değişmeyen statik bir kapasiteye sahiptir. **Instance count** çubuğunu kullanarak statik kapasiteyi seçersiniz. Kümenin ölçekleme başka bir değişiklik yapana kadar bu ayarda kalır.

   ![Manuel ölçek yöntemi](media/manage-cluster-horizontal-scaling/manual-scale-method.png)

### <a name="optimized-autoscale-preview"></a>Optimize edilmiş otomatik ölçek (önizleme)

Optimize edilmiş otomatik ölçeklendirme önerilen otomatik ölçekyöntemidir. Bu yöntem küme performansını ve maliyetlerini en iyi duruma getirin. Küme düşük kullanım durumuna yaklaşırsa, ölçeklendirilir. Bu eylem maliyetleri düşürür, ancak performans düzeyini korur. Küme aşırı kullanım durumuna yaklaşırsa, en iyi performansı korumak için ölçeklendirilir. Optimize edilmiş otomatik ölçeği yapılandırmak için:

1. **Optimize edilmiş otomatik ölçek'i**seçin. 

1. En az örnek sayısı ve en büyük örnek sayısı seçin. Küme otomatik ölçekleme, yüke bağlı olarak bu iki sayı arasında aralıklar.

1. **Kaydet'i**seçin.

   ![Optimize edilmiş otomatik ölçeklendirme yöntemi](media/manage-cluster-horizontal-scaling/optimized-autoscale-method.png)

Optimize edilmiş otomatik ölçek çalışmaya başlar. Eylemleri artık kümenin Azure etkinlik günlüğünde görülebilir.

#### <a name="logic-of-optimized-autoscale"></a>Optimize edilmiş otomatik ölçek mantığı 

**Ölçeği genişletme**

Kümeniz aşırı kullanım durumuna yaklaştığında, en iyi performansı korumak için ölçeklendirin. Ölçeklendirme, şu anda ortaya çıkacaktır:
* Küme örneklerinin sayısı, kullanıcı tarafından tanımlanan en fazla örnek sayısının altındadır.
* Önbellek kullanımı bir saatten fazla yüksektir.
* CPU bir saatten fazla yüksektir.
* Yutma kullanımı bir saatten fazla yüksektir.

> [!NOTE]
> Ölçeklendirme mantığı şu anda yutma kullanım ölçüsü dikkate almaz. Bu metrik kullanım örneğiniz için önemliyse, [özel otomatik ölçek](#custom-autoscale)kullanın.

**Ölçeği daraltma**

Kümeniz düşük kullanım durumuna yaklaştığında, maliyetleri düşürmek ancak performansı korumak için ölçeklendirin. Kümede ölçeklendirmenin güvenli olduğunu doğrulamak için birden çok ölçüm kullanılır. Aşağıdaki kurallar ölçek lendirmeden önce 6 saat boyunca saatlik olarak değerlendirilir:
* Örnek sayısı 2'nin üzerinde ve tanımlanan en az örnek sayısının üzerindedir.
* Kaynakların aşırı yüklenmediğinden emin olmak için, ölçeklendirme gerçekleştirilmeden önce aşağıdaki ölçümlerin doğrulanmış olması gerekir: 
    * Önbellek kullanımı yüksek değil
    * CPU ortalamanın altında 
    * Yutma kullanımı ortalamanın altındadır 
    * Akış kullanımı (akış varsa) yüksek değil
    * Canlı tutmak olaylar tanımlanmış bir minimumun üzerindedir, düzgün işlenir ve zamanında.
    * Sorgu azaltma yok 
    * Başarısız sorgu sayısı tanımlı minimumun altındadır.

> [!NOTE]
> Mantık ölçeği şu anda optimize edilmiş ölçek uygulanmadan önce 7 günlük bir değerlendirme gerektirir. Bu değerlendirme her 24 saatte bir gerçekleşir. Hızlı bir değişiklik gerekiyorsa, [el ile ölçek](#manual-scale)kullanın.

### <a name="custom-autoscale"></a>Özel otomatik ölçeklendirme

Özel otomatik ölçek kullanarak, kümenizi belirlediğiniz ölçümlere göre dinamik olarak ölçeklendirebilirsiniz. Aşağıdaki grafik, özel otomatik ölçeklendirmeyi yapılandırmak için akışı ve adımları gösterir. Daha fazla ayrıntı grafiği izleyin.

1. Otomatik **Ölçekayar adı** kutusuna, *Ölçeklendirme: önbellek kullanımı*gibi bir ad girin. 

   ![Ölçek kuralı](media/manage-cluster-horizontal-scaling/custom-autoscale-method.png)

2. **Ölçek modu için,** **bir metrik tabanlı Ölçek**seçin. Bu mod dinamik ölçekleme sağlar. Ayrıca belirli **bir örnek sayısıiçin Ölçek'i**de seçebilirsiniz.

3. Seçin **+ Kural ekle**.

4. Sağdaki **Ölçek kuralı** bölümünde, her ayar için değerleri girin.

    **Ölçütler**

    | Ayar | Açıklama ve değer |
    | --- | --- |
    | **Toplam süre** | **Ortalama**gibi bir toplama ölçütünü seçin. |
    | **Ölçüm adı** | **Önbellek Kullanımı**gibi ölçek işleminin temel alolmasını istediğiniz ölçüt'ün seçilmesini seçin. |
    | **Zaman dilimi istatistiği** | **Ortalama,** **Minimum,** **Maksimum**ve **Toplam**arasında seçim yapın. |
    | **Işleç** | 'den büyük veya **eşit**gibi uygun seçeneği seçin |
    | **Eşik** | Uygun bir değer seçin. Örneğin, önbellek kullanımı için yüzde 80 iyi bir başlangıç noktasıdır. |
    | **Süre (dakika)** | Ölçümleri hesaplarken sistemin geriye bakması için uygun bir zaman miktarı seçin. Varsayılan 10 dakika ile başlayın. |
    |  |  |

    **Eylem**

    | Ayar | Açıklama ve değer |
    | --- | --- |
    | **İşlem** | Ölçeklendirmek veya ölçeklendirmek için uygun seçeneği seçin. |
    | **Örnek sayısı** | Metrik bir koşul karşılandığında eklemek veya kaldırmak istediğiniz düğüm veya örnek sayısını seçin. |
    | **Soğuma (dakika)** | Ölçek işlemleri arasında beklemek için uygun bir zaman aralığı seçin. Varsayılan beş dakika ile başlayın. |
    |  |  |

5. **Ekle'yi**seçin.

6. **Soldaki Örnek sınırları** bölümünde, her ayar için değerleri girin.

    | Ayar | Açıklama ve değer |
    | --- | --- |
    | **Minimum** | Kümenizin kullanımdan bağımsız olarak aşağıda ölçeklendirmeyeceği örneklerin sayısı. |
    | **Maksimum** | Kümenizin kullanımdan bağımsız olarak, yukarıda ölçeklendirmeyeceği örneklerin sayısı. |
    | **Varsayılan** | Varsayılan örnek sayısı. Kaynak ölçümlerini okumada sorun varsa bu ayar kullanılır. |
    |  |  |

7. **Kaydet'i**seçin.

Azure Veri Gezgini kümeniz için yatay ölçekleme yapılandırıldınız. Dikey ölçekleme için başka bir kural ekleyin. Küme ölçekleme sorunları yla ilgili yardıma ihtiyacınız varsa, Azure portalında [bir destek isteği açın.](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini performansını, sistem durumunu ve kullanımını ölçümlerle izleme](using-metrics.md)
* Kümenin uygun boyutlandırması için [küme dikey ölçeklemesini yönetin.](manage-cluster-vertical-scaling.md)
