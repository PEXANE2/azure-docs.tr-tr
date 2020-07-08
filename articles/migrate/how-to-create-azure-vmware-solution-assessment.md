---
title: Azure geçişi sunucu değerlendirmesi ile bir AVS değerlendirmesi oluşturma | Microsoft Docs
description: Azure geçişi sunucu değerlendirmesi aracı ile bir AVS değerlendirmesi oluşturmayı açıklar
author: rashi-ms
ms.service: azure-migrate
ms.topic: article
ms.date: 06/26/2020
ms.author: rajosh
ms.openlocfilehash: 9dd6dba9b517c26277cc7f7e3c87df81b1c95648
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85568857"
---
# <a name="create-an-azure-vmware-solution-avs-assessment"></a>Azure VMware çözümü (AVS) değerlendirmesi oluşturma

Bu makalede, Azure geçişi: Sunucu değerlendirmesi ile şirket içi VMware VM 'Leri için Azure VMware çözümü (AVS) değerlendirmesi oluşturma açıklanır.

[Azure geçişi](migrate-services-overview.md) , Azure 'a geçiş yapmanıza yardımcı olur. Azure geçişi, şirket içi altyapıyı, uygulamaları ve verileri Azure 'a bulmayı, değerlendirmeyi ve geçirmeyi izlemek için merkezi bir merkez sağlar. Hub, değerlendirme ve geçiş için Azure araçları ve ayrıca üçüncü taraf bağımsız yazılım satıcısı (ISV) tekliflerini sağlar.

## <a name="before-you-start"></a>Başlamadan önce

- Bir Azure geçişi projesi [oluşturduğunuzdan](how-to-add-tool-first-time.md) emin olun.
- Zaten bir proje oluşturduysanız Azure geçişi: Sunucu değerlendirmesi [aracını eklediğinizden emin](how-to-assess.md) olun.
- Bir değerlendirme oluşturmak için, [VMware](how-to-set-up-appliance-vmware.md)Için bir Azure geçiş gereci ayarlamanız gerekir, bu da şirket içi makineleri bulur ve Azure geçişi: Sunucu değerlendirmesi ' ne meta veri ve performans verileri gönderir. [Daha fazla bilgi edinin](migrate-appliance.md).
- Ayrıca [, sunucu meta verilerini](tutorial-assess-import.md) virgülle ayrılmış değerler (CSV) biçiminde de içeri aktarabilirsiniz.


## <a name="azure-vmware-solution-avs-assessment-overview"></a>Azure VMware çözümü (AVS) değerlendirmesi genel bakış

Azure geçişi: Sunucu değerlendirmesi kullanarak oluşturabileceğiniz iki tür değerlendirme vardır.

**Değerlendirme türü** | **Ayrıntılar**
--- | --- 
**Azure VM** | Şirket içi sunucularınızı Azure sanal makinelerine geçirme değerlendirmeleri. <br/><br/> Bu değerlendirme türünü kullanarak Azure 'a geçiş için şirket içi [VMware VM](how-to-set-up-appliance-vmware.md)'lerinizi, [Hyper-V sanal](how-to-set-up-appliance-hyper-v.md)makinelerinizi ve [fiziksel sunucuları](how-to-set-up-appliance-physical.md) değerlendirebilirsiniz. [Daha fazla bilgi](concepts-assessment-calculation.md)
**Azure VMware Çözümü (AVS)** | Şirket içi sunucularınızı [Azure VMware çözümüne (AVS)](https://docs.microsoft.com/azure/azure-vmware/introduction)geçirme değerlendirmeleri. <br/><br/> Bu değerlendirme türünü kullanarak Azure VMware çözümüne (AVS) geçiş için şirket içi [VMware VM](how-to-set-up-appliance-vmware.md) 'lerinizi değerlendirebilirsiniz. [Daha fazla bilgi](concepts-azure-vmware-solution-assessment-calculation.md)

> [!NOTE]
> Azure VMware çözümü (AVS) değerlendirmesi Şu anda önizleme aşamasındadır ve yalnızca VMware VM 'Leri için oluşturulabilir.


Azure VMware çözümü (AVS) değerlendirmesi oluşturmak için kullanabileceğiniz iki tür boyutlandırma ölçütü vardır:

**Değerlendirme** | **Ayrıntılar** | **Veriler**
--- | --- | ---
**Performans tabanlı** | Şirket içi VM 'lerin toplanan performans verilerine dayanan değerlendirmeler. | **Önerilen düğüm boyutu**: değerlendirme için seçtiğiniz düğüm türü, depolama türü ve FTT ayarı Ile birlikte CPU ve bellek kullanım verilerine göre.
**Şirket içi olarak** | Şirket içi boyutlandırmayı temel alan değerlendirmeler. | **Önerilen düğüm boyutu**: değerlendirme için seçtiğiniz düğüm türü, depolama türü ve FTT ayarı ile birlikte ŞIRKET içi VM boyutuna göre.


## <a name="run-an-azure-vmware-solution-avs-assessment"></a>Azure VMware çözümü (AVS) değerlendirmesi çalıştırma

Azure VMware çözümü (AVS) değerlendirmesi aşağıdaki gibi çalıştırın:

1. Değerlendirme oluşturmak için [en iyi uygulamaları](best-practices-assessment.md) gözden geçirin.

2. **Sunucular** sekmesinde, **Azure geçişi: Sunucu değerlendirmesi** kutucuğunda **değerlendir**' e tıklayın.

    ![Değerlendirme](./media/how-to-create-assessment/assess.png)

3. **Sunucuları değerlendir**bölümünde, değerlendirme türünü "Azure VMware çözümü (AVS)" olarak seçin, bulma kaynağını seçin ve değerlendirme adını belirtin.

    ![Değerlendirme temelleri](./media/how-to-create-avs-assessment/assess-servers-avs.png)

4. Değerlendirme özelliklerini gözden geçirmek için **Tümünü görüntüle**’ye tıklayın.

    ![AVS değerlendirmesi özellikleri](./media/how-to-create-avs-assessment/avs-view-all.png)

5. **Değerlendirmek için makineleri seçmek**üzere **İleri** ' ye tıklayın. **Grup Seç veya oluştur**' da, **Yeni oluştur**' u seçin ve bir grup adı belirtin. Bir grup, değerlendirme için bir veya daha fazla VM 'yi toplar.

6. **Gruba makine ekleme**' de gruba eklenecek VM 'ler ' i seçin.

7. Değerlendirme ayrıntılarını gözden geçirmek için **İleri** ' ye ve **değerlendirme oluştur** ' a tıklayın.

8. Grubu oluşturmak için **değerlendirme oluştur** ' a tıklayın ve değerlendirmeyi çalıştırın.

    ![AVS değerlendirmesi oluşturma](./media/how-to-create-avs-assessment/avs-assessment-create.png)

9. Değerlendirme oluşturulduktan sonra **sunucuları**  >  **Azure geçişi: Sunucu değerlendirmesi**  >  **değerlendirmeleri**' nde görüntüleyin.

10. Excel dosyası olarak indirmek için **Değerlendirmeyi dışarı aktar**’a tıklayın.


## <a name="review-an-azure-vmware-solution-avs-assessment"></a>Azure VMware çözümü (AVS) değerlendirmesini gözden geçirme

Bir Azure VMware çözümü (AVS) değerlendirmesi şunları açıklar:

- **Azure VMware çözümü (AVS) hazırlığı**: Şirket Içi VM 'Lerin Azure VMware çözümüne (AVS) geçiş için uygun olup olmadığı.
- **AVS düğüm sayısı**: VM 'leri çalıştırmak için gereken, tahmini AVS düğüm sayısı.
- **AVS düğümleri genelinde kullanım**: tüm DÜĞÜMLERDE öngörülen CPU, bellek ve depolama kullanımı.
- **Aylık maliyet tahmini**: Şirket Içi VM 'leri çalıştıran tüm Azure VMware çözümü (AVS) düğümlerine yönelik tahmini aylık maliyetler.


### <a name="view-an-assessment"></a>Değerlendirme görüntüleme

1. **Geçiş hedefleri**  >   **sunucularında** **Azure geçişi: Sunucu değerlendirmesi**' nde **değerlendirmeler** ' a tıklayın.

2. **Değerlendirmede**, bir değerlendirmeye tıklayarak açın.

    ![AVS değerlendirmesi Özeti](./media/how-to-create-avs-assessment/avs-assessment-summary.png)

### <a name="review-azure-vmware-solution-avs-readiness"></a>Azure VMware Solution (AVS) hazırlığını gözden geçirme

1. **Azure hazırlığı**' nde, VM 'lerin AVS 'ye geçiş için hazır olup olmadığını doğrulayın.

2. VM durumunu gözden geçirin:
    - **AVS Için hazırlanma**: makine, Azure 'A (AVS) herhangi bir değişiklik yapılmadan olarak geçirilebilir. Tam AVS desteğiyle AVS 'de başlatılır.
    - **Koşullara göre**: VM, geçerli vSphere sürümü ile uyumluluk sorunlarına ve VM 'deki tam işlevselliği AVS 'de elde etmeden önce büyük olasılıkla VMware araçları ve veya diğer ayarları gerektirmek olabilir.
    - **AVS için hazırlanma**: VM, AVS 'de başlamaz. Örneğin, şirket içi VMware VM 'sinde CD-ROM gibi bir dış cihaz varsa, VMotion işlemi başarısız olur (VMware VMotion kullanılıyorsa).
    - **Hazır olma durumu bilinmiyor**: Azure geçişi, şirket içi ortamdan toplanan meta verilerin yetersiz olması nedeniyle makinenin hazır olduğunu saptayamadık.

3. Önerilen aracı gözden geçirin:
    - VMware **HCX veya Enterprise**: VMware makineleri Için VMware karma bulut uzantısı (HCX) çözümü, şirket içi iş yükünüzü Azure VMware çözümünüz (AVS) özel bulutuna geçirmek için önerilen geçiş aracıdır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-vmware/hybrid-cloud-extension-installation).
    - **Bilinmiyor**: bir CSV dosyası aracılığıyla içeri aktarılan makineler için varsayılan geçiş aracı bilinmez. Ancak VMware makinelerinde, VMWare karma bulut uzantısı (HCX) çözümünün kullanılması önerilir. 

4. **AVS hazırlığı** durumuna tıklayın. VM hazırlığı ayrıntılarını görüntüleyebilir ve işlem, depolama ve ağ ayarları dahil olmak üzere VM ayrıntılarını görmek için ayrıntıya gidebilirsiniz.



### <a name="review-cost-details"></a>Maliyet ayrıntılarını gözden geçirin

Bu görünüm, Azure VMware çözümünde (AVS) çalışan VM 'Lerin tahmini maliyetini gösterir.

1. Aylık toplam maliyetleri gözden geçirin. Ücretler, değerlendirilen gruptaki tüm VM 'Ler için toplanır. 

    - Maliyet tahminleri, toplam tüm VM 'lerin kaynak gereksinimlerini dikkate alarak gereken AVS düğüm sayısına bağlıdır.
    - Azure VMware çözümü (AVS) fiyatlandırması düğüm başına olduğunda, toplam maliyet işlem maliyeti ve depolama maliyeti dağıtımına sahip değildir.
    - Maliyet tahmini, AVS 'de şirket içi VM 'Leri çalıştırmak içindir. Azure geçişi sunucu değerlendirmesi PaaS veya SaaS maliyetlerini göz önünde bulundurmaz.
    
2. Aylık depolama maliyeti tahminlerini gözden geçirebilirsiniz. Bu görünüm, farklı türlerdeki depolama disklerinin üzerine bölünen, değerlendirilen grup için toplanan depolama maliyetlerini gösterir.

3. Belirli VM 'Lerin ayrıntılarını görmek için ayrıntıya gidebilirsiniz.


### <a name="review-confidence-rating"></a>Güvenilirlik derecelendirmesini gözden geçirme

Performans tabanlı değerlendirmeler çalıştırdığınızda, değerlendirmeye bir güvenilirlik derecelendirmesi atanır.

![Güvenilirlik derecelendirmesi](./media/how-to-create-assessment/confidence-rating.png)

- 1-yıldız (en düşük) ile 5 yıldız (en yüksek) arasında bir derecelendirme verilir.
- Güvenilirlik derecelendirmesi, değerlendirme tarafından belirtilen boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur.
- Güvenilirlik derecelendirmesi, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliğine bağlıdır.
- Performans tabanlı boyutlandırma için sunucu değerlendirmesinde AVS değerlendirmelerinde CPU ve VM belleği için kullanım verileri gerekir. Aşağıdaki performans verileri toplanır ancak AVS değerlendirmelerine yönelik boyutlandırma önerilerinde kullanılmaz:
  - SANAL makineye bağlı her diske yönelik disk ıOPS ve aktarım hızı verileri.
  - Bir VM 'ye bağlı her bir ağ bağdaştırıcısı için performans tabanlı boyutlandırmayı işlemek üzere ağ g/ç.

Bir değerlendirme için güven derecelendirmeleri aşağıdaki gibidir.

**Veri noktası kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
--- | ---
%0-%20 | 1 Yıldız
%21-%40 | 2 Yıldız
%41-%60 | 3 Yıldız
%61-%80 | 4 Yıldız
%81-%100 | 5 Yıldız

Performans verileri hakkında [daha fazla bilgi edinin](concepts-azure-vmware-solution-assessment-calculation.md) 


## <a name="next-steps"></a>Sonraki adımlar

- Yüksek güvenilirlikli gruplar oluşturmak için [bağımlılık eşlemeyi](how-to-create-group-machine-dependencies.md) nasıl kullanacağınızı öğrenin.
- AVS değerlendirmelerinin nasıl hesaplandığı hakkında [daha fazla bilgi edinin](concepts-azure-vmware-solution-assessment-calculation.md) .
