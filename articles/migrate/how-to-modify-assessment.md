---
title: Azure sunucu değerlendirmesi geçişi için değerlendirmeleri özelleştirin | Microsoft Docs
description: Azure geçişi sunucu değerlendirmesi ile oluşturulan değerlendirmelerin nasıl özelleştirileceğini açıklar
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/15/2019
ms.author: raynew
ms.openlocfilehash: bcc6f41d7cc08764266ffb6705d1b8937d355199
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86109731"
---
# <a name="customize-an-assessment"></a>Bir değerlendirmeyi özelleştirme

Bu makalede, Azure geçişi sunucu değerlendirmesi tarafından oluşturulan değerlendirmelerin nasıl özelleştirileceği açıklanır.

[Azure geçişi](migrate-services-overview.md) , şirket içi uygulamalarınızı, iş yüklerinizi ve özel/genel bulut VM 'lerini Azure 'a bulmayı, değerlendirmeyi ve geçirmeyi izlemek için bir merkezi Merkez sağlar. Hub, değerlendirme ve geçiş için Azure geçiş araçları ve ayrıca üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar.

Azure 'a geçiş için hazırlık bölümünde şirket içi VMware VM 'leri ve Hyper-V VM 'Leri için değerlendirmeler oluşturmak üzere Azure geçişi sunucu değerlendirmesi aracını kullanabilirsiniz. Sunucu değerlendirmesi Aracı, Azure IaaS sanal makinelerine ve Azure VMware çözümüne (AVS) geçiş için şirket içi sunucular değerlendirir. 

## <a name="about-assessments"></a>Değerlendirmeler hakkında

Sunucu değerlendirmesi ile oluşturduğunuz değerlendirmeler, verilerin bir zaman noktası anlık görüntüsüdür. Azure geçişi: Sunucu değerlendirmesi kullanarak oluşturabileceğiniz iki tür değerlendirme vardır.

**Değerlendirme türü** | **Ayrıntılar**
--- | --- 
**Azure VM** | Şirket içi sunucularınızı Azure sanal makinelerine geçirme değerlendirmeleri. <br/><br/> Bu değerlendirme türünü kullanarak Azure 'a geçiş için şirket içi [VMware VM](how-to-set-up-appliance-vmware.md)'lerinizi, [Hyper-V sanal](how-to-set-up-appliance-hyper-v.md)makinelerinizi ve [fiziksel sunucuları](how-to-set-up-appliance-physical.md) değerlendirebilirsiniz. (concepts-assessment-calculation.md)
**Azure VMware Çözümü (AVS)** | Şirket içi sunucularınızı [Azure VMware çözümüne (AVS)](../azure-vmware/introduction.md)geçirme değerlendirmeleri. <br/><br/> Bu değerlendirme türünü kullanarak Azure VMware çözümüne (AVS) geçiş için şirket içi [VMware VM](how-to-set-up-appliance-vmware.md) 'lerinizi değerlendirebilirsiniz. [Daha fazla bilgi](concepts-azure-vmware-solution-assessment-calculation.md)

Sunucu değerlendirmesinde bir Azure VM değerlendirmesi iki boyutlandırma ölçütü seçeneği sağlar:

**Boyutlandırma ölçütü** | **Ayrıntılar** | **Veriler**
--- | --- | ---
**Performans tabanlı** | Toplanan performans verilerine dayalı öneriler oluşturan değerlendirmeler | **Azure VM değerlendirmesi**: VM boyutu ÖNERISI, CPU ve bellek kullanımı verilerini temel alır.<br/><br/> Disk türü önerisi (Standart HDD/SSD veya Premium yönetilen diskler), şirket içi disklerin ıOPS ve aktarım hızını temel alır.<br/><br/> **Azure VMware çözümü (AVS) değerlendirmesi**: AVS düğümleri ÖNERISI, CPU ve bellek kullanımı verilerini temel alır.
**Şirket içi olarak** | Öneriler oluşturmak için performans verilerini kullanmayan değerlendirmeler. | **Azure VM değerlendirmesi**: VM boyutu önerisi, ŞIRKET içi VM boyutunu temel alır<br/><br> Önerilen disk türü, değerlendirme için depolama türü ayarında neleri seçdiklerinize bağlıdır.<br/><br/> **Azure VMware çözümü (AVS) değerlendirmesi**: AVS düğümleri önerisi, ŞIRKET içi VM boyutunu temel alır.


## <a name="how-is-an-assessment-done"></a>Değerlendirme nasıl yapılır?

Azure geçişi sunucu değerlendirmesi sırasında yapılan bir değerlendirme üç aşamaya sahiptir. Değerlendirme, bir uygunluk analizi, ardından boyutlandırma ve son olarak aylık maliyet tahmini ile başlar. Bir makine, bir öncekini geçerse daha sonraki bir aşamaya geçer. Örneğin, bir makine Azure uygunluk denetiminde başarısız olursa, Azure için uygun değil olarak işaretlenir ve boyutlandırma ve maliyetlendirme yapılmaz. [Daha fazla bilgi edinin.](./concepts-assessment-calculation.md)

## <a name="whats-in-an-azure-vm-assessment"></a>Azure VM değerlendirmesinde neler var?

**Özellik** | **Ayrıntılar**
--- | ---
**Hedef konum** | Geçişi yapmak istediğiniz Azure konumu.<br/> Sunucu değerlendirmesi Şu anda şu hedef bölgeleri destekliyor: Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Kanada Orta, Kanada Doğu, Orta Hindistan, Orta ABD, Çin Doğu, Çin Kuzey, Doğu Asya, Doğu ABD, Doğu ABD2, Almanya Orta, Almanya kuzeydoğu, Japonya Doğu, Japonya Batı, Kore Orta, Kore Güney, Orta Kuzey ABD, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, Güney Hindistan, UK Güney, UK Batı, US Gov Arizona , US Gov Teksas, US Gov Virginia, Orta Batı ABD, Batı Avrupa, Batı Hindistan, Batı ABD ve Batı ABD2.
**Depolama türü** | Azure 'da, ' ye taşımak istediğiniz disk türünü belirtmek için bu özelliği kullanabilirsiniz.<br/><br/> Şirket içi boyutlandırma için, hedef depolama türünü Premium yönetilen diskler, Standart SSD yönetilen diskler veya Standart HDD tarafından yönetilen diskler olarak belirtebilirsiniz. Performans tabanlı boyutlandırma için, hedef disk türünü otomatik, Premium yönetilen diskler, Standart HDD yönetilen diskler veya Standart SSD tarafından yönetilen diskler olarak belirtebilirsiniz.<br/><br/> Depolama türünü otomatik olarak belirttiğinizde, disk önerisi, disklerin performans verilerine (ıOPS ve aktarım hızı) göre yapılır. Depolama türünü Premium/standart olarak belirtirseniz, değerlendirme, seçilen depolama türü içinde bir disk SKU 'SU önerir. % 99,9 tek örnekli bir VM SLA 'Sı elde etmek istiyorsanız, depolama türünü Premium ile yönetilen diskler olarak belirtmek isteyebilirsiniz. Bu, değerlendirmede tüm disklerin Premium tarafından yönetilen diskler olarak önerilmesini sağlar. Azure
**Ayrılmış örnekler (RI)** | Bu özellik, Azure 'da [ayrılmış örneklere](https://azure.microsoft.com/pricing/reserved-vm-instances/) sahip olup olmadığını belirtmenize yardımcı olur, değerlendirmede maliyet tahminleri daha sonra RI indirimlerine göre yapılır. Ayrılmış örnekler şu anda yalnızca Azure geçişi 'nde Kullandıkça Öde teklifi için desteklenmektedir.
**Boyutlandırma ölçütü** | Azure için VM 'Leri doğru şekilde boyutlandırma için kullanılacak ölçüt. Performans geçmişini dikkate almadan, *performans tabanlı* boyutlandırma yapabilir veya VM 'leri *Şirket içinde olacak şekilde boyut olarak*ayarlayabilirsiniz.
**Performans geçmişi** | Makinelerin performans verilerini değerlendirmek için göz önünde bulundurulması gereken süre. Bu özellik yalnızca boyutlandırma ölçütü *performans tabanlı*olduğunda geçerlidir.
**Yüzdebirlik kullanımı** | Doğru boyutlandırma için dikkate alınacak performans örnek kümesinin yüzdebirlik değeri. Bu özellik yalnızca boyutlandırma *performans tabanlı*olduğunda geçerlidir.
**VM serisi** |     Uygun boyutlandırma için değerlendirmek istediğiniz bir VM serisini belirtebilirsiniz. Örneğin, Azure 'da bir serisi VM 'lere geçirmeyi planlamadığınız bir üretim ortamınız varsa, bir serisi listeden veya seriden hariç tutabilir ve sağ boyutlandırma yalnızca seçili seriler içinde yapılır.
**Konfor katsayısı** | Azure geçişi sunucu değerlendirmesi, değerlendirme sırasında bir arabellek (rahatetken) kabul eder. Bu tampon, VM’lerin makine kullanım verilerinin (CPU, bellek, disk ve ağ) üzerine uygulanır. Konfor katsayısı; sezona özgü kullanım, kısa performans geçmişi ve gelecek kullanımlarda oluşabilecek artışlar gibi konuları hesaba katar.<br/><br/> Örneğin, %20 kullanıma sahip 10 çekirdekli bir VM normalde 2 çekirdekli VM ile sonuçlanır. Ancak, 2.0x konfor katsayısı ile sonuç 4 çekirdekli VM olur.
**Teklif** | Kaydolduğunuz [Azure teklifi](https://azure.microsoft.com/support/legal/offer-details/). Azure Geçişi, buna göre bir maliyet tahmini oluşturur.
**Para birimi** | Fatura para birimi.
**İndirim (%)** | Azure teklifinin yanı sıra aldığınız, aboneliğe özgü indirim.<br/> Varsayılan ayar, %0’dır.
**VM çalışma süresi** | VM 'leriniz Azure 'da 7/24 çalışmaya devam etmeyeceklerse, çalıştırılacağı süreyi (aylık gün sayısı ve gün başına saat sayısı), maliyet tahminleri de uygun şekilde yapılacak şekilde belirtebilirsiniz.<br/> Varsayılan değer ayda 31 gün ve günde 24 saat olur.
**Azure Hibrit Avantajı** | Yazılım Güvencesi olup olmadığını ve [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/)için uygun olup olmadığını belirtin. Evet olarak ayarlanırsa, Windows sanal makineleri için Windows olmayan Azure fiyatları dikkate alınır. Varsayılan değer Evet’tir.

## <a name="whats-in-an-azure-vmware-solution-avs-assessment"></a>Azure VMware Çözüm (AVS) değerlendirmesi nelerdir?

Sunucu değerlendirmesinde bir AVS değerlendirmesi aşağıda verilmiştir:


| **Özellik** | **Ayrıntılar** |
| - | - |
| **Hedef konum** | Geçirmek istediğiniz AVS özel bulut konumunu belirtir.<br/><br/> Sunucu değerlendirmesinde AVS değerlendirmesi Şu anda şu hedef bölgeleri destekliyor: Doğu ABD, Batı Avrupa Batı ABD. |
| **Depolama türü** | AVS 'de kullanılacak depolama altyapısını belirtir.<br/><br/> AVS değerlendirmelerinin yalnızca varsayılan depolama türü olarak vSAN 'ı desteklediğini unutmayın. |
**Ayrılmış örnekler (RIS)** | Bu özellik, AVS 'de ayrılmış örnekler belirtmenize yardımcı olur. Bu, AVS düğümlerinde Şu anda desteklenmemektedir. |
**Düğüm türü** | Şirket içi VM 'Leri eşlemek için kullanılan [AVS düğüm türünü](../azure-vmware/concepts-private-clouds-clusters.md) belirtir. Varsayılan düğüm türünün AV36 olduğunu unutmayın. <br/><br/> Azure geçişi, sanal makinelerin AVS 'ye geçirilmesi için gerekli sayıda düğüm önermenizi önerir. |
**FTT ayarı, RAID düzeyi** | Tolerans ve RAID birleşimlerine yönelik geçerli hatayı belirtir. Şirket içi VM disk gereksinimiyle birlikte bulunan seçili FTT seçeneği, AVS 'de gereken toplam vSAN depolama alanını tespit eder. |
**Boyutlandırma ölçütü** | AVS için sanal makineleri _doğru olarak boyutlandıralmak_ üzere kullanılacak ölçütleri ayarlar. Performans geçmişini dikkate almadan _performans tabanlı_ boyutlandırmayı veya _Şirket içi olarak_ tercih edebilirsiniz. |
**Performans geçmişi** | Makinelerin performans verilerini değerlendirmek için göz önünde bulundurulması gereken süreyi ayarlar. Bu özellik yalnızca boyutlandırma ölçütü _performans tabanlı_olduğunda geçerlidir. |
**Yüzdebirlik kullanımı** | Sağ boyutlandırmanın kabul edileceği performans örneği kümesinin yüzdebirlik değerini belirtir. Bu özellik yalnızca boyutlandırma performans tabanlı olduğunda geçerlidir.|
**Konfor katsayısı** | Azure geçişi sunucu değerlendirmesi, değerlendirme sırasında bir arabellek (rahatetken) kabul eder. Bu tampon, VM’lerin makine kullanım verilerinin (CPU, bellek, disk ve ağ) üzerine uygulanır. Konfor katsayısı; sezona özgü kullanım, kısa performans geçmişi ve gelecek kullanımlarda oluşabilecek artışlar gibi konuları hesaba katar.<br/><br/> Örneğin, %20 kullanıma sahip 10 çekirdekli bir VM normalde 2 çekirdekli VM ile sonuçlanır. Ancak, 2.0x konfor katsayısı ile sonuç 4 çekirdekli VM olur. |
**Teklif** | Kayıtlı olduğunuz [Azure teklifini](https://azure.microsoft.com/support/legal/offer-details/) görüntüler. Azure Geçişi, buna göre bir maliyet tahmini oluşturur.|
**Para birimi** | Hesabınız için faturalandırma para birimini gösterir. |
**İndirim (%)** | Azure teklifinin üzerine aldığınız aboneliğe özgü tüm indirimi listeler. Varsayılan ayar, %0’dır. |
**Azure Hibrit Avantajı** | Yazılım Güvencesi olup olmadığını ve [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/)uygun olduğunu belirtir. Düğüm tabanlı fiyat nedeniyle Azure VMware Çözüm fiyatlandırması üzerinde hiçbir etkisi olmamasına karşın, müşteriler Azure hibrit avantajlarını kullanarak AVS 'de şirket içi işletim sistemi lisanslarını (Microsoft tabanlı) uygulayabilirler. Diğer yazılım işletim sistemi satıcılarının, RHEL gibi kendi lisans şartlarını sağlaması gerekecektir. |
**vCPU fazla abonelik** | AVS düğümündeki 1 fiziksel çekirdeğe bağlı sanal çekirdek sayısının oranını belirtir. Hesaplamalardaki varsayılan değer 4 vCPU: AVS 'de 1 fiziksel çekirdek olur. <br/><br/> API kullanıcıları bu değeri bir tamsayı olarak ayarlayabilir. VCPU fazla aboneliği > 4:1, performans düşüşüne neden olabilir ancak Web sunucusu türü iş yükleri için kullanılabilir. |

## <a name="edit-assessment-properties"></a>Değerlendirme özelliklerini Düzenle

Değerlendirme oluşturduktan sonra değerlendirme özelliklerini düzenlemek için aşağıdakileri yapın:

1. Azure geçişi projesinde **sunucular**' a tıklayın.
2. **Azure geçişi: Sunucu değerlendirmesi**' nde değerlendirme sayısı ' na tıklayın.
3. **Değerlendirme**bölümünde ilgili değerlendirmeye > **Özellikleri Düzenle**' ye tıklayın.
5. Yukarıdaki tablolara uygun olarak değerlendirme özelliklerini özelleştirin.
6. Değerlendirmeyi güncelleştirmek için **Kaydet** ' e tıklayın.


Değerlendirme oluştururken değerlendirme özelliklerini de düzenleyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

- Azure VM değerlendirmelerinin nasıl hesaplandığı hakkında [daha fazla bilgi edinin](concepts-assessment-calculation.md) .
- AVS değerlendirmelerinin nasıl hesaplandığı hakkında [daha fazla bilgi edinin](concepts-azure-vmware-solution-assessment-calculation.md) .
