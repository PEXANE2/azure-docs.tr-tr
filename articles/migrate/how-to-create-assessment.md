---
title: Azure sunucu değerlendirmesi ile Azure VM değerlendirmesi oluşturma | Microsoft Docs
description: Azure geçişi sunucu değerlendirmesi aracı ile Azure VM değerlendirmesi oluşturmayı açıklar
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/15/2019
ms.author: raynew
ms.openlocfilehash: 534619ace09b4e11934062a591adf8d9ab6f77ad
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96500810"
---
# <a name="create-an-azure-vm-assessment"></a>Azure VM değerlendirmesi oluşturma

Bu makalede, Azure geçişi: Sunucu değerlendirmesi ile şirket içi VMware VM 'Leri veya Hyper-V VM 'Leri için Azure VM değerlendirmesi oluşturma açıklanır.

[Azure geçişi](migrate-services-overview.md) , Azure 'a geçiş yapmanıza yardımcı olur. Azure geçişi, şirket içi altyapıyı, uygulamaları ve verileri Azure 'a bulmayı, değerlendirmeyi ve geçirmeyi izlemek için merkezi bir merkez sağlar. Hub, değerlendirme ve geçiş için Azure araçları ve ayrıca üçüncü taraf bağımsız yazılım satıcısı (ISV) tekliflerini sağlar. 

## <a name="before-you-start"></a>Başlamadan önce

- Bir Azure geçişi projesi [oluşturduğunuzdan](./create-manage-projects.md) emin olun.
- Zaten bir proje oluşturduysanız Azure geçişi: Sunucu değerlendirmesi [aracını eklediğinizden emin](how-to-assess.md) olun.
- Bir değerlendirme oluşturmak için, [VMware](how-to-set-up-appliance-vmware.md) veya [Hyper-V](how-to-set-up-appliance-hyper-v.md)için bir Azure geçiş gereci ayarlamanız gerekir. Gereç, şirket içi makineleri bulur ve Azure geçişi: Sunucu değerlendirmesi ' ne meta veri ve performans verileri gönderir. [Daha fazla bilgi edinin](migrate-appliance.md).


## <a name="azure-vm-assessment-overview"></a>Azure VM değerlendirmesine genel bakış
Azure geçişi: Sunucu değerlendirmesi kullanarak bir Azure VM değerlendirmesi oluşturmak için kullanabileceğiniz iki tür boyutlandırma ölçütü vardır.

**Değerlendirme** | **Ayrıntılar** | **Veriler**
--- | --- | ---
**Performans tabanlı** | Toplanan performans verilerine dayalı değerlendirmeler | **ÖNERILEN VM boyutu**: CPU ve bellek kullanım verilerine göre.<br/><br/> **Önerilen disk türü (Standart veya Premium yönetilen disk)**: Şirket ıçı disklerin IOPS ve aktarım hızına göre.
**Şirket içi olarak** | Şirket içi boyutlandırmayı temel alan değerlendirmeler. | **ÖNERILEN VM boyutu**: ŞIRKET içi VM boyutuna göre<br/><br> **Önerilen disk türü**: değerlendirme için seçtiğiniz depolama türü ayarına göre.

Değerlendirmeler hakkında [daha fazla bilgi edinin](concepts-assessment-calculation.md) .

## <a name="run-an-assessment"></a>Değerlendirme çalıştırma

Bir değerlendirmeyi aşağıdaki gibi çalıştırın:

1. Değerlendirme oluşturmaya yönelik [en iyi yöntemleri](best-practices-assessment.md) gözden geçirin.
2. **Sunucular** sekmesinde, **Azure geçişi: Sunucu değerlendirmesi** kutucuğunda **değerlendir**' e tıklayın.

    ![Ekran görüntüsü değerlendirme araçları altında değerlendirmede Azure geçişi sunucularını gösterir.](./media/how-to-create-assessment/assess.png)

3. **Sunucuları değerlendir** bölümünde, değerlendirme türünü "Azure VM" olarak seçin, bulma kaynağını seçin ve değerlendirme adını belirtin.

    ![Değerlendirmeyle ilgili temel bilgiler](./media/how-to-create-assessment/assess-servers-azurevm.png)

4. Değerlendirme özelliklerini gözden geçirmek için **Tümünü görüntüle**’ye tıklayın.

    ![Değerlendirme özellikleri](./media/how-to-create-assessment//view-all.png)

5. **Değerlendirilecek makineleri seçin**'in yanındaki **İleri**'yi seçin. **Grup oluşturun veya seçin** alanında **Yeni oluştur**'u seçip bir grup adı belirtin. Grup, değerlendirme için bir veya daha fazla VM'yi bir araya getirir.
6. **Gruba makine ekleme**' de gruba eklenecek VM 'ler ' i seçin.
7. Değerlendirme ayrıntılarını gözden geçirmek için **Gözden geçir ve değerlendirmeyi oluştur**'un yanındaki **İleri**'yi seçin.
8. Grubu oluşturmak için **değerlendirme oluştur** ' a tıklayın ve değerlendirmeyi çalıştırın.

    ![Değerlendirme oluşturma](./media/how-to-create-assessment//assessment-create.png)

9. Değerlendirme oluşturulduktan sonra **Sunucular** > **Azure Geçişi: Sunucu Değerlendirmesi** > **Değerlendirmeler** sayfasından görüntüleyin.
10. Excel dosyası olarak indirmek için **Değerlendirmeyi dışarı aktar**’a tıklayın.



## <a name="review-an-azure-vm-assessment"></a>Azure VM değerlendirmesini gözden geçirme

Azure VM değerlendirmesi şunları ifade eder:

- **Azure hazırlığı**: VM 'lerin Azure 'a geçiş için uygun olup olmadığı.
- **Aylık maliyet tahmini**: VM 'leri Azure 'da çalıştırmaya yönelik tahmini aylık işlem ve depolama maliyetleri.
- **Aylık depolama maliyeti tahmini**: Geçiş sonrasında disk depolaması için tahmini maliyetler.

### <a name="view-an-azure-vm-assessment"></a>Azure VM değerlendirmesi görüntüleme

1. **Geçiş hedefleri**  >   **sunucularında** **Azure geçişi: Sunucu değerlendirmesi**' nde **değerlendirmeler** ' a tıklayın.
2. **Değerlendirmede**, bir değerlendirmeye tıklayarak açın.

    ![Değerlendirme özeti](./media/how-to-create-assessment/assessment-summary.png)

### <a name="review-azure-readiness"></a>Azure için hazır olmayı gözden geçirme

1. **Azure 'a hazırlık** bölümünde, VM 'lerin Azure 'a geçiş için hazır olup olmadığını doğrulayın.
2. VM durumunu gözden geçirin:
    - **Azure için hazır**: Azure Geçişi, değerlendirme kapsamındaki VM'ler için VM boyutu önerisi ve maliyet tahmini sunar.
    - **Koşullara hazırlanma**: sorunları ve önerilen düzeltmeyi gösterir.
    - **Azure için hazırlanma**: sorunları ve önerilen düzeltmeyi gösterir.
    - **Hazır olma durumu bilinmiyor**: veri kullanılabilirliği sorunları nedeniyle Azure geçişi hazırlığı değerlendiremez kullanılır.

3. **Azure hazırlık** durumuna tıklayın. VM hazırlığı ayrıntılarını görüntüleyebilir ve işlem, depolama ve ağ ayarları dahil olmak üzere VM ayrıntılarını görmek için ayrıntıya gidebilirsiniz.



### <a name="review-cost-details"></a>Maliyet ayrıntılarını gözden geçirme

Bu görünümde, Azure’da çalışan sanal makinelerin tahmini işlem ve depolama maliyeti görüntülenir.

1. Aylık işlem ve depolama maliyetlerini gözden geçirebilirsiniz. Ücretler, değerlendirilen gruptaki tüm VM 'Ler için toplanır.

    - Maliyet tahminleri, tek bir makine için boyut önerileri ile disklerini ve özelliklerini temel alır.
    - İşlem ve depolama için aylık tahmini maliyetler gösterilir.
    - Maliyet tahmini, şirket içi VM 'Leri IaaS VM 'Leri olarak çalıştırmaya yöneliktir. Azure geçişi sunucu değerlendirmesi PaaS veya SaaS maliyetlerini göz önünde bulundurmaz.

2. Aylık depolama maliyeti tahminlerini gözden geçirebilirsiniz. Bu görünüm, farklı türlerdeki depolama disklerinin üzerine bölünen, değerlendirilen grup için toplanan depolama maliyetlerini gösterir.
3. Belirli VM 'Lerin ayrıntılarını görmek için ayrıntıya gidebilirsiniz.


### <a name="review-confidence-rating"></a>Güvenilirlik derecelendirmesini gözden geçirme

Performans tabanlı değerlendirmeler çalıştırdığınızda, değerlendirmeye bir güvenilirlik derecelendirmesi atanır.

![Güvenilirlik derecelendirmesi](./media/how-to-create-assessment/confidence-rating.png)

- 1-yıldız (en düşük) ile 5 yıldız (en yüksek) arasında bir derecelendirme verilir.
- Güvenilirlik derecelendirmesi, değerlendirme tarafından belirtilen boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur.
- Güvenilirlik derecelendirmesi, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliğine bağlıdır.

Bir değerlendirme için güven derecelendirmeleri aşağıdaki gibidir.

**Veri noktası kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
--- | ---
%0-%20 | 1 Yıldız
%21-%40 | 2 Yıldız
%41-%60 | 3 Yıldız
%61-%80 | 4 Yıldız
%81-%100 | 5 Yıldız




## <a name="next-steps"></a>Sonraki adımlar

- Yüksek güvenilirlikli gruplar oluşturmak için [bağımlılık eşlemeyi](how-to-create-group-machine-dependencies.md) nasıl kullanacağınızı öğrenin.
- Değerlendirmelerin nasıl hesaplandığı hakkında [daha fazla bilgi](concepts-assessment-calculation.md) edinin.