---
title: Azure Geçiş'te sorun giderme değerlendirmesi ve bağımlılık görselleştirmesi
description: Azure Geçiş'te sorun giderme değerlendirmesi ve bağımlılık görselleştirmesi konusunda yardım alın.
ms.service: azure-migrate
ms.topic: troubleshooting
author: musa-57
ms.manager: abhemraj
ms.author: hamusa
ms.date: 01/02/2020
ms.openlocfilehash: 205b52201edb849abab02809b58ff9dc77a32a29
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80127670"
---
# <a name="troubleshoot-assessmentdependency-visualization"></a>Değerlendirme/bağımlılık görselleştirmesi sorunlarını giderme

Bu makale, [Azure Geçiş: Sunucu Değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool)ile değerlendirme ve bağımlılık görselleştirmeile ilgili sorunları gidermenize yardımcı olur.


## <a name="assessment-readiness-issues"></a>Değerlendirmeye hazırlık sorunları

Değerlendirmeye hazırlık sorunlarını aşağıdaki gibi düzeltin:

**Sorunu** | **Düzeltme**
--- | ---
Desteklenmeyen önyükleme türü | Azure, EFI önyükleme türüyle VM'leri desteklemez. Geçiş yapmadan önce önyükleme türünü BIOS'a dönüştürmenizi öneririz. <br/><br/>Bu tür VM'lerin geçişini işlemek için Azure Geçir Sunucu Geçişi'ni kullanabilirsiniz. Geçiş sırasında VM'nin önyükleme türünü BIOS'a dönüştürür.
Koşullu destekli Windows işletim sistemi | İşletim sistemi destek bitiş tarihini geçti ve [Azure'da destek](https://aka.ms/WSosstatement)için özel destek sözleşmesine (CSA) ihtiyaç duyar. Azure'a geçiş yapmadan önce yükseltmeyi düşünün.
Desteklenmeyen Windows işletim sistemi | Azure yalnızca [seçili Windows işletim sistemi sürümlerini](https://aka.ms/WSosstatement)destekler. Azure'a geçiş yapmadan önce makineyi yükseltmeyi düşünün.
Şartlı linux işletim sistemi onayladı | Azure yalnızca [seçili Linux işletim sistemi sürümlerini](../virtual-machines/linux/endorsed-distros.md)onaylar. Azure'a geçiş yapmadan önce makineyi yükseltmeyi düşünün.
Onaylanmamış Linux Os | Makine Azure'da başlayabilir, ancak Azure işletim sistemi desteği sağlamaz. Azure'a geçiş yapmadan önce onaylanan bir [Linux sürümüne](../virtual-machines/linux/endorsed-distros.md) yükseltmeyi düşünün.
Bilinmeyen işletim sistemi | VM işletim sistemi vCenter Server "Diğer" olarak belirtilmiştir. Bu davranış, Azure Geçir'in VM'nin Azure hazır durumunu doğrulamasını engeller. Makineyi geçirmeden önce işletim sisteminin Azure tarafından [desteklendirdiğinden](https://aka.ms/azureoslist) emin olun.
Desteklenmeyen bit sürümü | 32 bit işletim sistemine sahip VM'ler Azure'da önyükleme yapabilir, ancak Azure'a geçiş yapmadan önce 64 bit'e yükseltmenizi tavsiye ettik.
Microsoft Visual Studio aboneliği gerektirir | Makine, yalnızca Visual Studio aboneliği ile desteklenen bir Windows istemci işletim sistemi çalıştırıyor.
VM gerekli depolama performansı için bulunamadı | Makine için gereken depolama performansı (saniyede giriş/çıktı işlemleri [IOPS] ve üretim) Azure VM desteğini aşıyor. Geçişten önce makinenin depolama gereksinimlerini azaltın.
VM gerekli ağ performansı için bulunamadı | Makine için gereken ağ performansı (dahil/çıkış) Azure VM desteğini aşıyor. Makinenin ağ gereksinimlerini azaltın.
VM belirtilen konumda bulunamadı | Geçişyapmadan önce farklı bir hedef konumu kullanın.
Bir veya daha fazla uygun olmayan disk | VM'ye bağlı bir veya daha fazla disk Azure gereksinimlerini karşılamaz. A<br/><br/> Azure Geçiş: Sunucu Değerlendirmesi şu anda Ultra SSD diskleri desteklemez ve diskleri birinci sınıf yönetilen diskler için disk sınırlarına göre değerlendirir (32 TB).<br/><br/> VM'ye bağlı her disk için, diskboyutunun 64 TB'< olduğundan emin olun (Ultra SSD diskler tarafından desteklenir).<br/><br/> Değilse, Azure'a geçiş yapmadan önce disk boyutunu küçültün veya Azure'da birden fazla disk kullanın ve daha yüksek depolama alanı sınırları elde etmek için [bunları bir araya](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#disk-striping) getirin. Her diskin ihtiyaç duyduğu performansın (IOPS ve iş ortası) Azure [tarafından yönetilen sanal makine diskleri](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#storage-limits)tarafından desteklendirildiğinden emin olun.
Bir veya daha fazla uygun olmayan ağ bağdaştırıcısı. | Geçişten önce kullanılmayan ağ bağdaştırıcılarını makineden çıkarın.
Disk sayısı sınırı aşıyor | Geçişten önce kullanılmayan diskleri makineden kaldırın.
Disk boyutu sınırı aşıyor | Azure Geçiş: Sunucu Değerlendirmesi şu anda Ultra SSD diskleri desteklemez ve diskleri premium disk sınırlarına (32 TB) göre değerlendirir.<br/><br/> Ancak Azure, 64 TB boyutuna sahip diskleri destekler (Ultra SSD diskler tarafından desteklenir). Diskleri geçişten önce 64 TB'nin daha azına küçültün veya Azure'da birden çok disk kullanın ve daha yüksek depolama alanı sınırları elde etmek için [bunları bir araya](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#disk-striping) getirin.
Disk belirtilen konumda kullanılamıyor | Geçiş yapmadan önce diskin hedef konumunuzda olduğundan emin olun.
Belirtilen artıklık için disk kullanılamıyor | Disk, değerlendirme ayarlarında tanımlanan artıklık depolama türünü kullanmalıdır (Varsayılan olarak LRS).
İç hata nedeniyle diske uygunluğu belirlenemedi | Grup için yeni bir değerlendirme oluşturmayı deneyin.
Gerekli çekirdekve bellek bulunamadı VM | Azure uygun bir VM türü bulamadı. Geçiş yapmadan önce şirket içi makinenin bellek ve çekirdek sayısını azaltın.
İç hata nedeniyle VM uygunluğu belirlenemedi | Grup için yeni bir değerlendirme oluşturmayı deneyin.
İç hata nedeniyle bir veya daha fazla diskiçin uygunluğu belirleyemedi | Grup için yeni bir değerlendirme oluşturmayı deneyin.
İç hata nedeniyle bir veya daha fazla ağ bağdaştırıcısı için uygunluğu belirleyemedi | Grup için yeni bir değerlendirme oluşturmayı deneyin.

## <a name="linux-vms-are-conditionally-ready"></a>Linux VM'ler "koşullu olarak hazır"

Sunucu Değerlendirmesi, Sunucu Değerlendirmesi'ndeki bilinen boşluk nedeniyle Linux VM'leri "Koşullu olarak hazır" olarak işaretler.

- Bu boşluk, şirket içi VM'lerde yüklü olan Linux OS'nin küçük sürümünü algılamasını engeller.
- Örneğin, RHEL 6.10 için şu anda Sunucu Değerlendirmesi, işletim sistemi sürümü olarak yalnızca RHEL 6'yı algılar.
-  Azure, Linux'un yalnızca belirli sürümlerini desteklediğinden, Linux VM'leri şu anda Sunucu Değerlendirmesi'nde koşullu olarak hazır olarak işaretlenir.
- Şirket içi VM'de çalışan Linux işletim sistemi azure [Linux desteğini](https://aka.ms/migrate/selfhost/azureendorseddistros)gözden geçirerek Azure'da onaylanıp onaylanmadığını belirleyebilirsiniz.
-  Onaylanan dağıtımı doğruladıktan sonra, bu uyarıyı yok sayabilirsiniz.

## <a name="azure-skus-bigger-than-on-premises"></a>Azure SK'ları şirket içinden daha büyük

Azure Geçir Sunucusu Değerlendirmesi, değerlendirme türüne bağlı olarak mevcut şirket içi tahsisattan daha fazla çekirdek ve belleğe sahip Azure VM SK'leri önerebilir:


- VM SKU önerisi değerlendirme özelliklerine bağlıdır.
- Bu, Sunucu Değerlendirmesi'nde gerçekleştirdiğiniz değerlendirme türünden etkilenir: *Performansa dayalı*veya *Şirket içinde olduğu gibi.*
- Performansa dayalı değerlendirmeler için Sunucu Değerlendirmesi, şirket içi VM'leriniz için doğru hedef VM SKU'yu belirlemek için şirket içi VM'lerin (CPU, bellek, disk ve ağ kullanımı) kullanım verilerini dikkate alır. Ayrıca etkili kullanımı belirlerken bir konfor faktörü ekler.
- Şirket içi boyutlandırma için performans verileri dikkate alınmaz ve hedef SKU şirket içi tahsise dayalı olarak önerilir.

Bunun önerileri nasıl etkileyebileceğini göstermek için bir örnek verelim:

Biz dört çekirdek ve bellek sekiz GB, 50% CPU kullanımı ve% 50 bellek kullanımı ve 1.3 belirli bir konfor faktörü ile bir şirket içi VM var.

-  Değerlendirme şirket **içi olarak**ise, dört çekirdekli ve 8 GB belleğe sahip bir Azure VM SKU önerilir.
- Değerlendirme performansa dayalı ise, etkili CPU ve bellek kullanımına dayalı (%50 4 çekirdek * 1.3 = 2.6 çekirdek ve 8 GB bellek % 50 * 1.3 = 5.3-GB bellek), dört çekirdek (en yakın desteklenen çekirdek sayısı) ve sekiz GB bellek (en yakın desteklenen çekirdek sayısı) en ucuz VM SKU bellek boyutu) önerilir.
- Değerlendirme boyutlandırma hakkında [daha fazla bilgi edinin.](concepts-assessment-calculation.md#types-of-assessments)

## <a name="azure-disk-skus-bigger-than-on-premises"></a>Azure disk SUS'ları şirket içinden daha büyük

Azure Geçir Sunucu Değerlendirmesi, değerlendirme türüne göre daha büyük bir disk önerebilir.
- Sunucu Değerlendirmesinde disk boyutlandırmaiki değerlendirme özelliğine bağlıdır: boyutlandırma ölçütleri ve depolama türü.
- Boyutlandırma ölçütleri **Performans tabanlıysa**ve depolama türü **Otomatik**, IOPS olarak ayarlanmışsa ve hedef disk türü (Standart HDD, Standart SSD veya Premium) tanımlanırken diskin üretim değerleri dikkate alınır. Daha sonra disk türünden bir disk SKU önerilir ve öneri, şirket içi diskin boyut gereksinimlerini dikkate alır.
- Boyutlandırma ölçütleri **Performans tabanlıysa**ve depolama türü **Premium**ise, Azure'da bir premium disk SKU'su, şirket içi diskin IOPS, iş ortası ve boyut gereksinimlerine göre önerilir. Boyutlandırma ölçütleri **şirket içi olarak** ve depolama türü Standart **HDD, Standart** **SSD**veya **Premium**olduğunda aynı mantık disk boyutlandırma gerçekleştirmek için kullanılır.

Örnek olarak, 32 GB belleğe sahip bir şirket içi diskiniz varsa, ancak disk için toplu okuma ve yazma IOPS 800 IOPS ise, Sunucu Değerlendirmesi bir premium disk önerir (yüksek IOPS gereksinimleri nedeniyle) ve ardından gerekli IOPS ve boyutu. Bu örnekteki en yakın eşleşme P15 (256 GB, 1100 IOPS) olabilir. Şirket içi diskin gerektirdiği boyut 32 GB olsa da, Sunucu Değerlendirmesi şirket içi diskin yüksek IOPS gereksinimi nedeniyle daha büyük bir disk önerir.

## <a name="utilized-corememory-percentage-missing"></a>Kullanılan çekirdek/bellek yüzdesi eksik

Sunucu Değerlendirmesi, Azure Geçiş cihazı ilgili şirket içi VM'ler için performans verileri toplayadığında "PercentageOfCoresUtilizedMissing" veya "PercentageOfMemoryUtilizedMissing" raporeder.

- Bu, değerlendirme süresi boyunca VM'ler kapatılırsa oluşabilir. Cihaz kapalıyken Bir VM için performans verileri toplayamaz.
- Yalnızca bellek sayaçları eksikse ve Hyper-V VM'leri değerlendirmeye çalışıyorsanız, bu VM'lerde dinamik belleğe sahip olup olmadığınızı kontrol edin. Yalnızca Hyper-V V'ler için bilinen bir sorun vardır ve bir Azure Geçiş cihazı dinamik belleği etkin olmayan VM'ler için bellek kullanım verilerini toplayamamaktadır.
- Performans sayaçlarından herhangi biri eksikse, Azure Geçir Sunucu Değerlendirmesi ayrılan çekirdeklere ve belleğe geri döner ve ilgili VM boyutu önerir.
- Tüm performans sayaçları eksikse, değerlendirme için bağlantı noktası erişim gereksinimlerinin karşılandığından emin olun. [VMware,](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#port-access) [Hyper-V](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#port-access) ve [fiziksel](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-physical#port-access) sunucu değerlendirmesi için bağlantı noktası erişim gereksinimleri hakkında daha fazla bilgi edinin.

## <a name="is-the-operating-system-license-included"></a>İşletim sistemi lisansı dahil mi?

Azure Geçir Sunucu Değerlendirmesi şu anda işletim sistemi lisans maliyetini yalnızca Windows makineleri için dikkate almaktadır. Linux makineleri için lisans maliyetleri şu anda dikkate alınmaz.

## <a name="how-does-performance-based-sizing-work"></a>Performansa dayalı boyutlandırma nasıl çalışır?

Sunucu Değerlendirmesi şirket içi makinelerin performans verilerini sürekli toplar ve bunları Azure’da VM SKU'su ile disk SKU'su önermek için kullanır. Performansa dayalı verilerin nasıl toplandığını [öğrenin.](concepts-assessment-calculation.md#calculate-sizing-performance-based)


## <a name="dependency-visualization-in-azure-government"></a>Azure Kamu'da bağımlılık görselleştirmesi

Azure Geçiş, bağımlılık görselleştirme işlevi için Hizmet Haritası'na bağlıdır. Hizmet Haritası şu anda Azure Kamu'da kullanılamadığından, bu işlevsellik Azure Kamu'da kullanılamaz.

## <a name="dependencies-dont-show-after-agent-install"></a>Bağımlılıklar aracı yükledikten sonra görünmüyor

Bağımlılık görselleştirme aracılarını şirket içi VM'lere yükledikten sonra, Azure Geçiş'in genellikle portaldaki bağımlılıkları görüntülemesi 15-30 dakika sürer. 30 dakikadan uzun süre beklediyseniz, Microsoft İzleme Aracısı'nın (MMA) Log Analytics çalışma alanına bağlanabilmesini unutmayın.

Windows VM'leri için:
1. Denetim Masası'nda MMA'yı başlatın.
2. Microsoft **İzleme Aracısı özelliklerinde** > **Azure Log Analytics (OMS),** çalışma alanı **durumunun** yeşil olduğundan emin olun.
3. Durum yeşil değilse, çalışma alanını kaldırmayı ve MMA'ya yeniden eklemeyi deneyin.

    ![MMA durumu](./media/troubleshoot-assessment/mma-properties.png)

Linux VM'leri için, MMA ve bağımlılık aracısı için yükleme komutlarının başarılı olduğundan emin olun.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

- **MMS aracısı**: Desteklenen [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems)ve [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems) işletim sistemlerini inceleyin.
- **Bağımlılık aracısı:** desteklenen [Windows ve Linux](../azure-monitor/insights/vminsights-enable-overview.md#supported-operating-systems) işletim sistemleri.

## <a name="visualize-dependencies-for--hour"></a>> saat boyunca bağımlılıkları görselleştirin

Azure Geçiş, son bir ay içinde belirli bir tarihe geri dönmenize izin vermesine rağmen, bağımlılıkları görselleştirebileceğiniz maksimum süre bir saattir.

Örneğin, bağımlılık haritasındaki süre işlevini, bağımlılıkları görüntülemek için kullanabilirsiniz, ancak bunları yalnızca bir saatlik bir süre için görüntüleyebilirsiniz.

Ancak, bağımlılık verilerini daha uzun bir süre boyunca [sorgulamak](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) için Azure Monitor günlüklerini kullanabilirsiniz.

## <a name="visualized-dependencies-for--10-machines"></a>> 10 makine için görselleştirilmiş bağımlılıklar

Azure Geçir Sunucu Değerlendirmesi'nde, 10 VM'ye kadar [olan grupların bağımlılıklarını görselleştirebilirsiniz.](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) Daha büyük gruplar için, bağımlılıkları görselleştirmek için VM'leri daha küçük gruplara bölmenizi öneririz.

## <a name="machines-show-install-agent"></a>Makineler "Montaj aracısı" gösterir

Azure'a bağlı bağımlılık görselleştirmesi etkinleştirilmiş makineleri geçirinsonra, makineler aşağıdaki davranışlar nedeniyle "Bağımlılıkları görüntüle" yerine "Aracı yükle" eylemi gösterebilir:


- Azure'a geçişten sonra, şirket içi makineler kapatılır ve Azure'da eşdeğer VM'ler döndürülmeye yüz lenir. Bu makineler farklı bir MAC adresi edinir.
- Makineler, şirket içi IP adresini tutup tutmadığınıza bağlı olarak farklı bir IP adresine de sahip olabilir.
- Hem MAC hem de IP adresleri şirket içinde farklıysa, Azure Geçiş, şirket içi makineleri herhangi bir Hizmet Haritası bağımlılık verileriyle ilişkilendiremez. Bu durumda, bağımlılıkları görüntülemek yerine aracıyı yükleme seçeneğini gösterir.
- Azure'a bir test geçişinden sonra, şirket içi makineler beklendiği gibi açık kalır. Azure'da dönen eşdeğer makineler farklı MAC adresi kazanır ve farklı IP adresleri edinebilir. Bu makinelerden giden Azure Monitor günlük trafiğini engellemediğiniz sürece, Azure Geçiş şirket içi makineleri herhangi bir Hizmet Haritası bağımlılık verileriyle ilişkilendirmez ve böylece bağımlılıkları görüntülemek yerine aracıyükleme seçeneğini gösterir.


## <a name="capture-network-traffic"></a>Ağ trafiğini yakalama

Ağ trafiği günlüklerini aşağıdaki gibi toplayın:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Geliştirici Araçlarını başlatmak için F12 tuşuna basın. Gerekirse, gezinti **ayarındaki Girişleri Temizle'yi** temizleyin.
3. **Ağ** sekmesini seçin ve ağ trafiğini yakalamaya başlayın:
   - Chrome'da, **Koru günlükünü**seçin. Kayıt otomatik olarak başlamalıdır. Kırmızı daire trafiğin yakalandığını gösterir. Kırmızı daire görünmüyorsa, başlamak için siyah daireyi seçin.
   - Microsoft Edge ve Internet Explorer'da kayıt otomatik olarak başlamalıdır. Değilse, yeşil oynat düğmesini seçin.
4. Hatayı yeniden oluşturmaya çalışın.
5. Kayıt sırasında hatayla karşılaştıktan sonra, kaydı durdurun ve kaydedilen etkinliğin bir kopyasını kaydedin:
   - Chrome'da, içerikle birlikte **HAR olarak kaydet'e**sağ tıklayın ve seçin. Bu eylem, günlükleri .har dosyası olarak sıkıştırır ve dışa aktarır.
   - Microsoft Edge veya Internet Explorer'da, **yakalanan trafiği dışa** aktar seçeneğini seçin. Bu eylem, günlüğü sıkıştırır ve dışa dışa kaydeder.
6. Uyarılar veya hataları denetlemek için **Konsol** sekmesini seçin. Konsol günlüğünü kaydetmek için:
   - Chrome'da, konsol günlüğündeki herhangi bir yere sağ tıklayın. Dışa aktarmak için **Kaydet'i**ve günlüğü zip'i seçin.
   - Microsoft Edge veya Internet Explorer'da, hataları sağ tıklatın ve **tümünü kopyala'yı**seçin.
7. Geliştirici Araçlarını kapatın.

## <a name="next-steps"></a>Sonraki adımlar

Bir değerlendirme [oluşturun](how-to-create-assessment.md) veya [özelleştirin.](how-to-modify-assessment.md)
