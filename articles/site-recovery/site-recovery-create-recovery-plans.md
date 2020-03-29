---
title: Azure Site Kurtarma'da kurtarma planları oluşturma/özelleştirme
description: Azure Site Kurtarma hizmetini kullanarak olağanüstü durum kurtarma için kurtarma planlarını nasıl oluşturup özelleştireceklerini öğrenin.
ms.topic: how-to
ms.date: 01/23/2020
ms.openlocfilehash: 6540317324a9f0d9bccc046ecf95824d4128bd09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76705845"
---
# <a name="create-and-customize-recovery-plans"></a>Kurtarma planları oluşturma ve özelleştirme

Bu makalede, [Azure Site Kurtarma'da](site-recovery-overview.md)başarısız olmak için bir kurtarma planının nasıl oluşturulup özelleştirilen açıklanmaktadır. Başlamadan önce kurtarma planları hakkında [daha fazla bilgi edinin.](recovery-plan-overview.md)

## <a name="create-a-recovery-plan"></a>Kurtarma planı oluşturma

1. Kurtarma Hizmetleri kasasında, **Kurtarma Planları (Site Kurtarma)** > **+Kurtarma Planı'nı**seçin.
2. **Kurtarma planı oluştur'da,** plan için bir ad belirtin.
3. Plandaki makineleri temel alan bir kaynak ve hedef seçin ve dağıtım modeli için **Kaynak Yöneticisi'ni** seçin. Kaynak konumda, hata ve kurtarma için etkin makineler olmalıdır. 

    **Başarısızlık** | **Kaynak** | **Hedef** 
   --- | --- | ---
   Azure-Azure arası | Azure bölgesini seçin | Azure bölgesini seçin
   VMware'den Azure'a | Yapılandırma sunucusunu seçin | Azure'u Seçin
   Azure'a fiziksel makineler | Yapılandırma sunucusunu seçin | Azure'u Seçin   
   Hyper-V’den Azure’a | Hyper-V site adını seçin | Azure'u Seçin
   Hyper-V (VMM tarafından yönetilir) ile Azure  | VMM sunucusunu seçin | Azure'u Seçin
  
    Şunlara dikkat edin:
    -  Yalnızca kaynak konumdan Azure'a başarısız olmak için bir kurtarma planı kullanabilirsiniz. Azure'dan geri dönüş için bir kurtarma planı kullanamazsınız.
    - Kaynak konumda, hata ve kurtarma için etkin makineler olmalıdır. 
    - Kurtarma planı aynı kaynak ve hedefe sahip makineleri içerebilir. 
    - VMM tarafından yönetilen VMware VM'leri ve Hyper-V VM'leri aynı plana ekleyebilirsiniz.
    - VMware VM'ler ve fiziksel sunucular aynı planda olabilir.

4. **Öğeleri seçin sanal makinelerde,** plana eklemek istediğiniz makineleri (veya çoğaltma grubunu) seçin. Ardından **Tamam**'a tıklayın.
    - Makineler plana varsayılan grup (Grup 1) eklenir. Başarısız olduktan sonra, bu gruptaki tüm makineler aynı anda başlar.
    - Yalnızca belirttiğiniz kaynak ve hedef konumlarda makinelerin olduğunu seçebilirsiniz. 
5. Planı oluşturmak için **Tamam'ı** tıklatın.

## <a name="add-a-group-to-a-plan"></a>Plana grup ekleme

Grup bazında farklı davranışlar belirtebilmeniz için ek gruplar oluşturur ve farklı gruplara makineler eklersiniz. Örneğin, bir gruptaki makinelerin başarısız olduktan sonra ne zaman başlayacağını belirtebilir veya grup başına özelleştirilmiş eylemler belirtebilirsiniz.

1. **Kurtarma**Planları'nda, **Özelleştir'>** planı sağ tıklatın. Varsayılan olarak, bir plan oluşturduktan sonra ona eklediğiniz tüm makineler varsayılan Grup 1'de bulunur.
2. **+Grup'u**tıklatın. Varsayılan olarak, yeni bir grup eklendiği sırada numaralandırılır. En fazla yedi gruba sahip olabilirsiniz.
3. Yeni gruba taşımak istediğiniz makineyi seçin, **Grubu Değiştir'i**tıklatın ve ardından yeni grubu seçin. Alternatif olarak, **Korumalı öğe**> grup adını sağ tıklatın ve gruba makineler ekleyin. Bir makine veya çoğaltma grubu kurtarma planındaki yalnızca bir gruba ait olabilir.


## <a name="add-a-script-or-manual-action"></a>Komut dosyası veya el ile eylem ekleme

Bir komut dosyası veya el ile eylem ekleyerek bir kurtarma planı özelleştirebilirsiniz. Şunlara dikkat edin:

- Azure'a çoğalıyorsanız, Azure otomasyon runbook'larını kurtarma planınıza entegre edebilirsiniz. [Daha fazla bilgi edinin](site-recovery-runbook-automation.md).
- System Center VMM tarafından yönetilen Hyper-V VM'leri çoğaltırsanız, şirket içi VMM sunucusunda bir komut dosyası oluşturabilir ve kurtarma planına ekleyebilirsiniz.
- Bir komut dosyası eklediğinizde, grup için yeni bir eylem kümesi ekler. Örneğin, Grup 1 için bir ön adım kümesi Grup 1 adı ile *oluşturulur: ön adımlar.* Tüm ön adımlar bu kümenin içinde listelenir. Birincil siteye yalnızca bir VMM sunucunuz varsa komut dosyası ekleyebilirsiniz.
- El ile eylem eklerseniz, kurtarma planı çalıştığında, el ile eylemi eklediğiniz noktada durur. Bir iletişim kutusu, el ile eylemin tamamlandığını belirtmenizi ister.
- VMM sunucusunda bir komut dosyası oluşturmak için [bu makaledeki](hyper-v-vmm-recovery-script.md)yönergeleri izleyin.
- Komut dosyaları ikincil siteye başarısız lık sırasında ve ikincil siteden birincil siteye geri dönüş sırasında uygulanabilir. Destek çoğaltma senaryonuza bağlıdır:
    
    **Senaryo** | **Başarısızlık** | **Failback**
    --- | --- | --- 
    Azure-Azure arası  | Runbook | Runbook
    Vmware’den Azure’a | Runbook | NA 
    VMM ile Azure'a Hyper-V | Runbook | Betik
    Azure'da Hyper-V sitesi | Runbook | NA
    VMM ikincil VMM için | Betik | Betik

1. Kurtarma planında, eylemin eklenmesi gereken adımı tıklatın ve eylemin ne zaman gerçekleşmesi gerektiğini belirtin:
    1. İşlemin, gruptaki makineler başarısız olduktan sonra başlatılmadan önce gerçekleşmesini istiyorsanız, **ön eylem ekle'yi**seçin.
    1. Eylemin, gruptaki makineler başarısız olduktan sonra başladıktan sonra gerçekleşmesini istiyorsanız, **eylem sonrası ekle'yi**seçin. Eylemin konumunu taşımak için Yukarı **Taşıy** veya **Aşağı Taşı** düğmelerini seçin.
2. **Insert eyleminde,** **Komut Dosyası** veya El **Ile eylem'i**seçin.
3. El ile eylem eklemek istiyorsanız, aşağıdakileri yapın:
    1. Eylem için bir ad yazın ve eylem yönergeleri yazın. Başarısız olan kişi bu yönergeleri görür.
    1. Tüm hata türleri için el ile eylem eklemek isteyip istemediğinizi belirtin (Test, Failover, Planlı başarısız (ilgiliyse)). Ardından **Tamam**'a tıklayın.
4. Bir komut dosyası eklemek istiyorsanız, aşağıdakileri yapın:
    1. VMM komut dosyası ekliyorsanız, **VMM komut dosyasına Failover'ı**seçin ve **Komut Dosyası Yolu'nda** paylaşıma göreli yolu yazın. Örneğin, paylaşım VMMServerName \\ \<>\MSSCVMMLibrary\RPScripts adresinde bulunuyorsa, yolu belirtin: \RPScripts\RPScript.PS1.
    1. Bir Azure otomasyon çalışması kitabı ekliyorsanız, runbook'un bulunduğu **Azure Otomasyon Hesabı'nı** belirtin ve uygun **Azure Runbook**Script'ini seçin.
5. Komut dosyasının beklendiği gibi çalıştığından emin olmak için kurtarma planının bir test başarısızlığını çalıştırın.

## <a name="watch-a-video"></a>Nasıl yapılacağını görmek için

Kurtarma planının nasıl geliştirilebildiğini gösteren bir video izleyin.


> [!VIDEO https://www.youtube.com/embed/1KUVdtvGqw8]

## <a name="next-steps"></a>Sonraki adımlar

[Failovers çalıştırma](site-recovery-failover.md)hakkında daha fazla bilgi edinin.  

    
