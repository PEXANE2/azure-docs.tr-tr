---
title: Azure Geçir Sunucu Değerlendirmesi ile bir değerlendirme oluşturun | Microsoft Dokümanlar
description: Azure Geçir Sunucusu Değerlendirmesi aracıyla nasıl bir değerlendirme oluşturulacak açıklanır
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/15/2019
ms.author: raynew
ms.openlocfilehash: cffde2a677650387dffd19733e082ff7002ccb55
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68229108"
---
# <a name="create-an-assessment"></a>Değerlendirme oluşturma

Bu makalede, Azure Geçiş: Sunucu Değerlendirmesi ile şirket içi VMware VM'ler veya Hyper-V V M'ler için bir değerlendirme nin nasıl oluşturulacak olduğu açıklanmaktadır.

[Azure Geçiş,](migrate-services-overview.md) Azure'a geçiş yapmaya yardımcı olur. Azure Geçişi, şirket içi altyapının, uygulamaların ve verilerin Azure'a keşfini, değerlendirmesini ve geçişini izlemek için merkezi bir hub sağlar. Hub, değerlendirme ve geçiş için Azure araçlarının yanı sıra üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar. 

## <a name="before-you-start"></a>Başlamadan önce

- Bir Azure Geçiş projesi [oluşturduğunuzdan](how-to-add-tool-first-time.md) emin olun.
- Bir proje zaten oluşturduysanız, Azure Geçiş: Sunucu Değerlendirmesi aracını [eklediğinizden](how-to-assess.md) emin olun.
- Bir değerlendirme oluşturmak [için, VMware](how-to-set-up-appliance-vmware.md) veya [Hyper-V](how-to-set-up-appliance-hyper-v.md)için bir Azure Geçiş cihazı ayarlamanız gerekir. Cihaz şirket içi makineleri keşfeder ve Azure Geçiş: Sunucu Değerlendirmesi'ne meta veri ve performans verileri gönderir. [Daha fazla bilgi edinin](migrate-appliance.md).


## <a name="assessment-overview"></a>Değerlendirmeye genel bakış
Azure Geçiş'i kullanarak oluşturabileceğiniz iki tür değerlendirme vardır: Sunucu Değerlendirmesi.

**Değerlendirme** | **Şey** | **Veri**
--- | --- | ---
**Performansa dayalı** | Toplanan performans verilerine dayalı değerlendirmeler | **Önerilen VM boyutu**: CPU ve bellek kullanım verilerine dayanır.<br/><br/> **Önerilen disk türü (standart veya premium yönetilen disk)**: IOPS'ye ve şirket içi disklerin üretim emresine dayanır.
**Şirket içinde olduğu gibi** | Şirket içi boyutlandırmaya dayalı değerlendirmeler. | **Önerilen VM boyutu**: Şirket içi VM boyutuna göre<br/><br> **Önerilen disk türü**: Değerlendirme için seçtiğiniz depolama türü ayarına göre.

Değerlendirmeler hakkında [daha fazla bilgi edinin.](concepts-assessment-calculation.md)

## <a name="run-an-assessment"></a>Değerlendirme çalıştırma

Bir değerlendirmeyi aşağıdaki gibi çalıştırın:

1. Değerlendirme oluşturmak için [en iyi uygulamaları](best-practices-assessment.md) gözden geçirin.
2. **Sunucular** sekmesinde, **Azure Geçir: Sunucu Değerlendirme** döşemesinde **Değerlendir'i**tıklatın.

    ![Değerlendirme](./media/how-to-create-assessment/assess.png)

2. **Değerlendir sunucularında,** değerlendirme için bir ad belirtin.
3. Değerlendirme özelliklerini gözden geçirmek için **Tümünü görüntüle**’ye tıklayın.

    ![Değerlendirme özellikleri](./media/how-to-create-assessment//view-all.png)

3. **Bir grubu seç veya oluştur'da**Yeni **Oluştur'u**seçin ve bir grup adı belirtin. Bir grup değerlendirme için bir veya daha fazla VM'yi bir araya toplar.
4. **Gruba makine ekle'de,** gruba eklemek için VM'leri seçin.
5. Grubu oluşturmak ve değerlendirmeyi çalıştırmak için **Değerlendirme Oluştur'u** tıklatın.

    ![Değerlendirme oluşturma](./media/how-to-create-assessment//assessment-create.png)

6. Değerlendirme oluşturulduktan sonra, **Sunucular** > **Azure Geçiş: Sunucu Değerlendirme** > **Değerlendirmeleri'nde**görüntüleyin.
7. Excel dosyası olarak indirmek için **Değerlendirmeyi dışarı aktar**’a tıklayın.



## <a name="review-an-assessment"></a>Değerlendirmeyi gözden geçirme

Bir değerlendirme açıklar:

- **Azure hazırlığı**: VM'lerin Azure'a geçiş için uygun olup olmadığı.
- **Aylık maliyet tahmini**: Azure'da VM'leri çalıştırmak için tahmini aylık işlem ve depolama maliyetleri.
- **Aylık depolama maliyeti tahmini**: Geçişten sonra disk depolama için tahmini maliyetler.

### <a name="view-an-assessment"></a>Bir değerlendirmeyi görüntüleme

1. **Geçiş hedefleri** >  **Sunucularında,** **Azure Geçişinde Değerlendirmeler: Sunucu Değerlendirmesi'ni**tıklatın. **Assessments**
2. **Değerlendirmeler'de,** açmak için bir değerlendirmeye tıklayın.

    ![Değerlendirme özeti](./media/how-to-create-assessment/assessment-summary.png)

### <a name="review-azure-readiness"></a>Azure hazırlık durumunu gözden geçirin

1. **Azure'a hazır durumda,** VM'lerin Azure'a geçişiçin hazır olup olmadığını doğrulayın.
2. VM durumunu gözden geçirin:
    - **Azure için Hazır**: Azure Geçirim, değerlendirmede VM boyutu ve VM'ler için maliyet tahminleri önerir.
    - **Koşullara hazır**: Sorunları ve önerilen düzeltmeyi gösterir.
    - **Azure için hazır değil**: Sorunları ve önerilen düzeltmeyi gösterir.
    - **Hazırlık bilinmiyor**: Veri kullanılabilirliği sorunları nedeniyle Azure Geçiş hazır durumunu değerlendiremediğinde kullanılır.

2. **Azure hazırlık** durumunu tıklatın. VM hazırlık ayrıntılarını görüntüleyebilir ve bilgi işlem, depolama ve ağ ayarları da dahil olmak üzere VM ayrıntılarını görmek için ayrıntıya inebilirsiniz.



### <a name="review-cost-details"></a>Maliyet ayrıntılarını gözden geçirin

Bu görünüm, Azure'da VM'leri çalıştırmanın tahmini işlem ve depolama maliyetini gösterir.

1. Aylık işlem ve depolama maliyetlerini gözden geçirin. Maliyetler, değerlendirilen gruptaki tüm VM'ler için toplanır.

    - Maliyet tahminleri, bir makinenin boyut önerilerini ve disklerini ve özelliklerini temel alınr.
    - Hesaplama ve depolama için tahmini aylık maliyetler gösterilir.
    - Maliyet tahmini, şirket içi VM'leri IaaS VM olarak çalıştırmak içindir. Azure Geçir Sunucu Değerlendirmesi, PaaS veya SaaS maliyetlerini dikkate almaz.

2. Aylık depolama maliyeti tahminlerini gözden geçirebilirsiniz. Bu görünüm, değerlendirilen grup için farklı depolama diskleri türlerine bölünmüş toplu depolama maliyetlerini gösterir.
3. Belirli VM'ler için ayrıntıları görmek için ayrıntıya inebilirsiniz.


### <a name="review-confidence-rating"></a>Güvenilirlik derecelendirmesini gözden geçirme

Performansa dayalı değerlendirmeler çalıştırdığınızda, değerlendirmeye bir güven derecelendirmesi atanır.

![Güvenilirlik derecelendirmesi](./media/how-to-create-assessment/confidence-rating.png)

- 1-star (en düşük) ile 5 yıldızlı (en yüksek) arasında bir derecelendirme verilir.
- Güven derecelendirmesi, değerlendirme tarafından sağlanan boyut önerilerinin güvenilirliğini tahmin edinmesine yardımcı olur.
- Güven derecelendirmesi, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliğine dayanır.

Bir değerlendirme için güven derecelendirmeleri aşağıdaki gibidir.

**Veri noktası kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
--- | ---
%0-%20 | 1 Yıldız
%21-%40 | 2 Yıldız
%41-%60 | 3 Yıldız
%61-%80 | 4 Yıldız
%81-%100 | 5 Yıldız




## <a name="next-steps"></a>Sonraki adımlar

- Yüksek güven grupları oluşturmak için [bağımlılık eşlemeyi](how-to-create-group-machine-dependencies.md) nasıl kullanacağınızı öğrenin.
- Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.
