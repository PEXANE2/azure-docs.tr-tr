---
title: Azure Geçir Sunucu Değerlendirmesi'nde en iyi uygulamaları değerlendirme
description: Azure Geçir Sunucu Değerlendirmesi ile değerlendirme oluşturmak için ipuçları.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: raynew
ms.openlocfilehash: de6953b6648613595bc9975b17941b3a453a6d60
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74185975"
---
# <a name="best-practices-for-creating-assessments"></a>Değerlendirme oluşturmak için en iyi uygulamalar

[Azure Geçiş,](migrate-overview.md) uygulamaları, altyapıyı ve iş yüklerini Microsoft Azure'a keşfetmenize, değerlendirmenize ve geçirmenize yardımcı olan bir araç merkezi sağlar. Hub, Azure Geçiş araçlarını ve üçüncü taraf bağımsız yazılım satıcısı (ISV) tekliflerini içerir.

Bu makalede, Azure Geçir Sunucu Değerlendirmesi aracını kullanarak değerlendirme oluştururken en iyi uygulamalar özetlenmiştir.

## <a name="about-assessments"></a>Değerlendirmeler hakkında

Azure Geçir Sunucu Değerlendirmesi ile oluşturduğunuz değerlendirmeler, verilerin anlık görüntüsütür. Azure Geçir'de iki tür değerlendirme vardır.

**Değerlendirme türü** | **Şey** | **Veri**
--- | --- | ---
**Performansa dayalı** | Toplanan performans verilerine dayalı önerilerde bulunan değerlendirmeler | VM boyutu önerisi CPU ve bellek kullanım verilerini temel alır.<br/><br/> Disk türü önerisi (standart HDD/SSD veya premium yönetilen diskler) IOPS'ye ve şirket içi disklerin iş başına dayalıdır.
**Şirket içi olduğu gibi** | Önerilerde bulunmak için performans verilerini kullanmayan değerlendirmeler. | VM boyutu önerisi şirket içi VM boyutuna dayanır<br/><br> Önerilen disk türü, değerlendirme için depolama türü ayarında seçtiklerinize bağlıdır.

### <a name="example"></a>Örnek
Örnek olarak, %20 kullanımda dört çekirdekli şirket içi VM'niz ve %10 kullanımla 8 GB bellek varsa, değerlendirmeler aşağıdaki gibi olacaktır:

- **Performansa dayalı değerlendirme**:
    - Çekirdek (4 x 0,20 = 0,8) ve bellek (8 GB x 0,10 = 0,8) kullanımına dayalı etkili çekirdekleri ve belleği tanımlar.
    - Boyutlandırma için kullanılacak değerleri elde etmek için değerlendirme özelliklerinde belirtilen konfor faktörlerini (diyelim ki 1.3x) uygular. 
    - Azure'da ~1,04 çekirdek (0,8 x 1,3) ve ~1,04 GB (0,8 x 1,3) belleği desteklenebilen en yakın VM boyutunu önerir.

- **As-is (şirket içi olarak) değerlendirmesi**:
    -  Dört çekirdekli bir VM önerir; 8 GB bellek.

## <a name="best-practices-for-creating-assessments"></a>Değerlendirme oluşturmak için en iyi uygulamalar

Azure Geçir cihazı, şirket içi ortamınızın profilini sürekli olarak profiller ve Azure'a meta veri ve performans verileri gönderir. Bir cihaz kullanılarak keşfedilen sunucuların değerlendirmeleri için aşağıdaki en iyi uygulamaları izleyin:

- **Olduğu gibi değerlendirmeler oluşturun**: Makineleriniz Azure Geçiş portalında ortaya çıktığında olduğu gibi değerlendirmeler oluşturabilirsiniz.
- **Performansa dayalı değerlendirme oluşturma**: Keşif ayarladıktan sonra, performansa dayalı bir değerlendirme çalıştırmadan önce en az bir gün beklemenizi öneririz:
    - Performans verilerinin toplanması zaman alır. En az bir gün beklemek, değerlendirmeyi çalıştırmadan önce yeterli performans veri snoktaları olmasını sağlar.
    - Performansa dayalı değerlendirmeler çalıştırırken, değerlendirme süresi için ortamınızın profilini aldığınızdan emin olun. Örneğin, performans süresi bir hafta olarak ayarlanmış bir değerlendirme oluşturursanız, tüm veri noktalarının toplanmasını sağlamak için keşfetmeye başladıktan sonra en az bir hafta beklemeniz gerekir. Eğer yapmazsan, değerlendirme beş yıldızlı bir reyting alamayacak.
- **Değerlendirmeleri yeniden hesapla**: Değerlendirmeler anlık nokta anlık görüntüler olduğundan, en son verilerle otomatik olarak güncelleştirilemez. Bir değerlendirmeyi en son verilerle güncelleştirmek için yeniden hesaplamanız gerekir.

Azure Geçiş yoluyla içe aktarılan sunucuların değerlendirmeleri için aşağıdaki en iyi uygulamaları izleyin. CSV dosyası:

- **Olduğu gibi değerlendirmeler oluşturun**: Makineleriniz Azure Geçiş portalında ortaya çıktığında olduğu gibi değerlendirmeler oluşturabilirsiniz.
- **Performansa dayalı değerlendirme oluşturma**: Bu, özellikle şirket içinde aşırı hizmete sahip sunucu kapasiteniz varsa, daha iyi bir maliyet tahmini elde etmeye yardımcı olur. Ancak, performansa dayalı değerlendirmenin doğruluğu, sunucular için sizin belirlediğiniz performans verilerine bağlıdır. 
- **Değerlendirmeleri yeniden hesapla**: Değerlendirmeler anlık nokta anlık görüntüler olduğundan, en son verilerle otomatik olarak güncelleştirilemez. Bir değerlendirmeyi en son alınan verilerle güncelleştirmek için yeniden hesaplamanız gerekir.

## <a name="best-practices-for-confidence-ratings"></a>Güven derecelendirmeleri için en iyi uygulamalar

Performansa dayalı değerlendirmeleri çalıştırdığınızda, değerlendirmeye 1 yıldızlı (en düşük) ile 5 yıldızlı (en yüksek) arasında bir güven derecelendirmesi verilir. Güven derecelendirmelerini etkili bir şekilde kullanmak için:
- Azure Geçir Sunucu Değerlendirmesi, VM CPU/Bellek için kullanım verilerine ihtiyaç duyar.
- Şirket içi VM'ye bağlı her disk için, IOPS/iş verme verilerinin okunması/yazılması gerekir.
- VM'ye bağlı her ağ bağdaştırıcısı için, ağ içeri/çıkış verilerine ihtiyaç duyar.

Seçili süre için kullanılabilir veri noktalarının yüzdeye bağlı olarak, bir değerlendirme için güven derecelendirmesi aşağıdaki tabloda özetlendiği gibi sağlanır.

   **Veri noktası kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
   --- | ---
   %0-%20 | 1 Yıldız
   %21-%40 | 2 Yıldız
   %41-%60 | 3 Yıldız
   %61-%80 | 4 Yıldız
   %81-%100 | 5 Yıldız


## <a name="common-assessment-issues"></a>Sık değerlendirme sorunları

Değerlendirmeleri etkileyen bazı yaygın çevre sorunlarını şu şekilde ele alalım.

###  <a name="out-of-sync-assessments"></a>Senkronize olmayan değerlendirmeler

Bir değerlendirme oluşturduktan sonra bir gruptan makine ekler veya kaldırırsanız, oluşturduğunuz değerlendirme **eşitlenmemiş**olarak işaretlenir. Grup değişikliklerini yansıtacak şekilde değerlendirmeyi yeniden çalıştırın **(Yeniden Hesapla).**

### <a name="outdated-assessments"></a>Güncel olmayan değerlendirmeler

Değerlendirilen bir grupta bulunan VM'lerde şirket içi değişiklikler varsa, değerlendirme **eski olarak**işaretlenir. Değişiklikleri yansıtmak için değerlendirmeyi yeniden çalıştırın.

### <a name="low-confidence-rating"></a>Düşük güven derecesi

Bir değerlendirme, bir dizi nedenden dolayı tüm veri noktalarına sahip olmayabilir:

- Değerlendirmeyi oluşturduğunuz süre boyunca ortamınızın profilini oluşturmadınız. Örneğin, performans süresi bir hafta olarak ayarlanmış *performans tabanlı* bir değerlendirme oluşturuyorsanız, toplanan tüm veri noktaları için keşfi başlattıktan sonra en az bir hafta beklemeniz gerekir. En son geçerli güven derecelendirmesini görmek için Her zaman **Yeniden Hesapla'yı** tıklatabilirsiniz. Güven derecelendirmesi yalnızca *performansa dayalı* bir değerlendirme oluşturduğunuzda uygulanabilir.

- Değerlendirmenin hesaplandığı dönem boyunca birkaç sanal makine kapatılmıştır. Bazı VM'ler belirli bir süre boyunca kapatıldıysa Sunucu Değerlendirmesi o süreye ait performans verilerini toplayamaz.

- Sunucu Değerlendirmesi'nde bulma işlemi başlatıldıktan sonra birkaç VM oluşturulmuştur. Örneğin, son bir ayın performans geçmişi için değerlendirme oluşturuyorsanız, ancak yalnızca bir hafta önce ortamda birkaç sanal makine oluşturulduysa. Bu durumda, sürenin tamamında yeni VM'lerin performans verileri sağlanmaz ve güvenilirlik derecesi düşük olabilir.


## <a name="next-steps"></a>Sonraki adımlar

- Değerlendirmelerin nasıl hesaplandığı hakkında [bilgi edinin.](concepts-assessment-calculation.md)
- Bir değerlendirmeyi [nasıl](how-to-modify-assessment.md) özelleştirileştirin öğrenin.
