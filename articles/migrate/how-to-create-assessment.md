---
title: Azure sunucu değerlendirmesi ile Azure VM değerlendirmesi oluşturma | Microsoft Docs
description: Azure geçişi sunucu değerlendirmesi aracı ile Azure VM değerlendirmesi oluşturmayı açıklar
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: how-to
ms.date: 07/15/2019
ms.openlocfilehash: 178bdca78c6f011c607de8e1f5d5eabcdbaab7d4
ms.sourcegitcommit: ca215fa220b924f19f56513fc810c8c728dff420
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2021
ms.locfileid: "98567699"
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

1. **Windows ve Linux sunucuları**> **sunucular** sayfasında, **sunucuları değerlendir ve geçir**' e tıklayın.

   ![Sunucuları değerlendir ve geçir düğmesinin konumu](./media/tutorial-assess-vmware-azure-vm/assess.png)

2. **Azure geçişi: Sunucu değerlendirmesi**' nde **değerlendir**' e tıklayın.

    ![Değerlendirme düğmesinin konumu](./media/tutorial-assess-vmware-azure-vm/assess-servers.png)

3. **Sunucu**  >  **değerlendirmesi türünü** değerlendir bölümünde **Azure VM**' yi seçin.
4. **Bulma kaynağında**:

    - Gereci kullanarak makineler keşfetiyorsanız, **Azure geçişi gereci ' ndan bulunan makineler**' i seçin.
    - İçeri aktarılan bir CSV dosyası kullanan makineler tespit ederseniz, **Içeri aktarılan makineler**' i seçin. 
    
1. Değerlendirme özelliklerini gözden geçirmek için **Düzenle** ' ye tıklayın.

    :::image type="content" source="./media/tutorial-assess-vmware-azure-vm/assessment-name.png" alt-text="Değerlendirme özelliklerini gözden geçirmek için Düzenle düğmesinin konumu":::

1. **Değerlendirme özellikleri**  >  **hedef özellikleri**:
    - **Hedef konum**' da, geçirmek istediğiniz Azure bölgesini belirtin.
        - Boyut ve maliyet önerileri belirttiğiniz konuma göre hesaplanır. Hedef konumu varsayılan olarak değiştirdiğinizde, **ayrılmış örnekler** ve **VM Serisi** belirtmeniz istenir.
        - Azure Kamu 'da, [Bu bölgelerdeki](migrate-support-matrix.md#supported-geographies-azure-government) değerlendirmeleri hedefleyebilirsiniz
    - **Depolama türü**' nde,
        - Değerlendirmede performans tabanlı verileri kullanmak istiyorsanız, disk ıOPS ve aktarım hızı temelinde depolama türü önermek için Azure geçişi için **Otomatik** ' i seçin.
        - Alternatif olarak, geçiş yaparken VM için kullanmak istediğiniz depolama türünü seçin.
    - **Ayrılmış örnekler**' de, GEÇIŞ yaparken VM için ayrılmış örnekler kullanmak isteyip istemediğinizi belirtin.
        - Ayrılmış bir örnek kullanmayı seçerseniz, '**indirim (%)** veya **VM çalışma süresi** belirtemezsiniz. 
        - [Daha fazla bilgi edinin](https://aka.ms/azurereservedinstances).
 1. **VM boyutu**:
     - **Boyutlandırma ölçütündeki** değerlendirmeyi makine yapılandırma verileri/meta verileri veya performans tabanlı veriler üzerinde temel almak istiyorsanız seçin. Performans verileri kullanıyorsanız:
        - **Performans geçmişi**' nde, değerlendirmeye dayandırmak istediğiniz veri süresini belirtin
        - **Yüzdelik kullanım**' de, performans örneği için kullanmak istediğiniz yüzdebirlik değerini belirtin. 
    - **VM Serisi**' nde, göz önünde bulundurmanız ISTEDIĞINIZ Azure VM serisini belirtin.
        - Performans tabanlı değerlendirme kullanıyorsanız, Azure geçişi sizin için bir değer önerir.
        - Gerektiğinde ince ayar ayarları. Örneğin, Azure 'da bir serisi VM gerektiren bir üretim ortamınız yoksa, seri listesinden bir serisi dışarıda bırakabilirsiniz.
    - **Rahatlık faktörüyle**, değerlendirme sırasında kullanmak istediğiniz arabelleği belirtin. Bu hesaplar, dönemsel kullanım, kısa performans geçmişi ve gelecekteki kullanımlarda olası artışlar gibi sorunlara yöneliktir. Örneğin, iki bir rahatlık faktörü kullanıyorsanız:
    
        **Bileşen** | **Etkin kullanım** | **Rakip çarpanı ekleme (2,0)**
        --- | --- | ---
        Çekirdekler | 2  | 4
        Bellek | 8 GB | 16 GB
   
1. **Fiyatlandırma**:
    - **Teklif** bölümünde, kaydolduysanız [Azure teklifini](https://azure.microsoft.com/support/legal/offer-details/) belirtin. Sunucu değerlendirmesi, bu teklifin maliyetini tahmin eder.
    - **Para birimi**' nde, hesabınız için faturalandırma para birimini seçin.
    - **İndirim (%)** bölümünde, Azure teklifinin üzerine aldığınız aboneliğe özgü indirimleri ekleyin. Varsayılan ayar, %0’dır.
    - **VM çalışma süresi**' nde, VM 'lerin çalışacağı süreyi (gün başına aylık gün/saat) belirtin.
        - Bu, sürekli olarak çalışmayan Azure VM 'Leri için yararlıdır.
        - Maliyet tahminleri belirtilen süreye göre belirlenir.
        - Varsayılan değer ayda 31 gün/günde 24 saattir.
    - **EA aboneliğinde**, maliyet tahmini için bir kurumsal anlaşma (EA) abonelik indirimi hesaba alıp almayacağını belirtin. 
    - **Azure hibrit avantajı**' de, zaten bir Windows Server lisansınız olup olmadığını belirtin. Bunu yaptıysanız ve Windows Server aboneliklerinin etkin yazılım güvencesi kapsamında yer alındıklarında, lisansları Azure 'a aktardığınızda [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/) uygulayabilirsiniz.

1. Değişiklik yaparsanız **Kaydet** ' e tıklayın.

    ![Değerlendirme özellikleri](./media/tutorial-assess-vmware-azure-vm/assessment-properties.png)

1. **Sunucuları değerlendir** > **İleri**' ye tıklayın.

1. Değerlendirme **adını değerlendirmek için makineleri seçin**  >   > değerlendirme için bir ad belirtin. 

1. > **Grup Seç veya oluştur** bölümünde **Yeni oluştur** ' u seçin ve bir grup adı belirtin. 

    :::image type="content" source="./media/tutorial-assess-vmware-azure-vm/assess-group.png" alt-text="Bir gruba VM ekleme":::

1. Gereç ' ı seçin ve gruba eklemek istediğiniz VM 'Leri seçin. Ardından **İleri**'ye tıklayın.


1. **İnceleme ve değerlendirme oluştur**' da, değerlendirme ayrıntılarını gözden geçirin ve grubu oluşturmak ve değerlendirmeyi çalıştırmak Için değerlendirme **Oluştur** ' a tıklayın.

1. Değerlendirme oluşturulduktan sonra **Sunucular** > **Azure Geçişi: Sunucu Değerlendirmesi** > **Değerlendirmeler** sayfasından görüntüleyin.

1. Excel dosyası olarak indirmek için **Değerlendirmeyi dışarı aktar**’a tıklayın.
    > [!NOTE]
    > Performans tabanlı değerlendirmeler için, değerlendirme oluşturmadan önce bulmayı başlattıktan sonra en az bir gün beklemeniz önerilir. Bu, daha yüksek güvenilirliğe sahip performans verilerinin toplanması için zaman sağlar. İdeal olarak, bulmayı başlattıktan sonra, belirttiğiniz performans süresini (gün/hafta/ay) yüksek güvenilirlikli bir derecelendirme için bekleyin.


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