---
title: Azure geçişi 'nde değerlendirme ve bağımlılık görselleştirmesine sorun giderme
description: Azure geçişi 'nde değerlendirme ve bağımlılık görselleştirmesiyle ilgili yardım alın.
ms.service: azure-migrate
ms.topic: troubleshooting
author: musa-57
ms.manager: abhemraj
ms.author: hamusa
ms.date: 01/02/2020
ms.openlocfilehash: 18158c867ba7a3307585eab0f950d15a6a12aa7c
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84342638"
---
# <a name="troubleshoot-assessmentdependency-visualization"></a>Değerlendirme/bağımlılık görselleştirmesi sorunlarını giderme

Bu makale, [Azure geçişi: Sunucu değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool)ile değerlendirme ve bağımlılık görselleştirmesiyle ilgili sorunları gidermenize yardımcı olur.


## <a name="assessment-readiness-issues"></a>Değerlendirme hazırlığı sorunları

Değerlendirme hazırlık sorunlarını aşağıdaki şekilde giderin:

**Konuda** | **Düzeltme**
--- | ---
Desteklenmeyen önyükleme türü | Azure, EFı önyükleme türü olan VM 'Leri desteklemez. Geçiş çalıştırmadan önce önyükleme türünü BIOS 'a dönüştürmeniz önerilir. <br/><br/>Bu sanal makinelerin geçişini yönetmek için Azure geçişi sunucu geçişini kullanabilirsiniz. Geçiş sırasında VM 'nin önyükleme türünü BIOS 'a dönüştürür.
Koşullu olarak desteklenen Windows işletim sistemi | İşletim sistemi destek son tarihini geçti ve [Azure 'da destek](https://aka.ms/WSosstatement)için özel bir destek SÖZLEŞMESINE (CSA) ihtiyaç duyuyor. Azure 'a geçiş yapmadan önce yükseltmeyi göz önünde bulundurun.
Desteklenmeyen Windows işletim sistemi | Azure yalnızca [Seçili Windows işletim sistemi sürümlerini](https://aka.ms/WSosstatement)destekler. Azure 'a geçiş yapmadan önce makineyi yükseltmeyi düşünün.
Koşullu olarak onaylama Linux işletim sistemi | Azure yalnızca [Seçili Linux işletim sistemleri sürümlerini](../virtual-machines/linux/endorsed-distros.md)onaylar. Azure 'a geçiş yapmadan önce makineyi yükseltmeyi düşünün.
Hazırlanmamış Linux işletim sistemi | Makine Azure 'da başlayabilir, ancak Azure işletim sistemi desteği sağlamaz. Azure 'a geçiş yapmadan önce, [onaylı bir Linux sürümüne](../virtual-machines/linux/endorsed-distros.md) yükseltmeyi düşünün.
Bilinmeyen işletim sistemi | VM 'nin işletim sistemi vCenter Server içinde "Other" olarak belirtilmiştir. Bu davranış, Azure geçişi 'nin VM 'nin Azure 'un hazır olduğunu doğrulamasını engeller. Makineyi geçirmeden önce işletim sisteminin Azure tarafından [desteklendiğinden](https://aka.ms/azureoslist) emin olun.
Desteklenmeyen bit sürümü | 32 bitlik bir işletim sistemi olan VM 'Ler Azure 'da önbaşlatılabilir, ancak Azure 'a geçmeden önce 64 bit 'e yükseltmeniz önerilir.
Microsoft Visual Studio aboneliği gerektirir | Makine, yalnızca bir Visual Studio aboneliği aracılığıyla desteklenen bir Windows istemci işletim sistemi çalıştırıyor.
Gerekli depolama performansı için VM bulunamadı | Makine için gereken depolama performansı (saniye başına giriş/çıkış işlemi [ıOPS] ve üretilen iş), Azure VM desteğini aşıyor. Geçişten önce makinenin depolama gereksinimlerini azaltın.
Gerekli ağ performansı için VM bulunamadı | Makine için gereken ağ performansı (ın/out) Azure VM desteğini aşıyor. Makinenin ağ gereksinimlerini azaltın.
Belirtilen konumda VM bulunamadı | Geçişten önce farklı bir hedef konum kullanın.
Bir veya daha fazla uygun olmayan disk | VM 'ye bağlı bir veya daha fazla disk, Azure gereksinimlerini karşılamıyor. A<br/><br/> Azure geçişi: Sunucu değerlendirmesi Şu anda Ultra SSD diskleri desteklememektedir ve Premium yönetilen diskler (32 TB) için disk sınırlarına göre diskleri değerlendirir.<br/><br/> SANAL makineye bağlı her disk için, diskin boyutunun < 64 TB (Ultra SSD diskler tarafından desteklenir) olduğundan emin olun.<br/><br/> Değilse, Azure 'a geçmeden önce disk boyutunu azaltın veya Azure 'da birden çok disk kullanın ve daha yüksek depolama sınırları almak için [bunları birlikte toplayın](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#disk-striping) . Her disk için gereken performansın (ıOPS ve aktarım hızı) Azure [yönetilen sanal makine diskleri](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#storage-limits)tarafından desteklendiğinden emin olun.
Bir veya daha fazla uygun olmayan ağ bağdaştırıcısı. | Kullanılmayan ağ bağdaştırıcılarını geçişten önce makineden kaldırın.
Disk sayısı sınırı aşıyor | Kullanılmayan diskleri geçişten önce makineden kaldırın.
Disk boyutu sınırı aşıyor | Azure geçişi: Sunucu değerlendirmesi Şu anda Ultra SSD diskleri desteklemez ve değerlendirir disk sınırlarına (32 TB) dayalı diskleri destekler.<br/><br/> Ancak Azure, 64 TB 'a kadar olan diskleri destekler (Ultra SSD diskleri tarafından desteklenir). Geçiş işleminden önce diskleri 64 TB 'den daha az bir değere küçültün veya Azure 'da birden çok disk kullanın ve daha yüksek depolama sınırları almak için [bunları birlikte toplayın](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#disk-striping) .
Belirtilen konumda disk yok | Geçirmeden önce diskin hedef konumunda olduğundan emin olun.
Belirtilen artıklık için disk kullanılamıyor | Disk, değerlendirme ayarlarında tanımlanan artıklık depolama türünü kullanmalıdır (varsayılan olarak LRS).
Bir iç hata nedeniyle disk uygunluğu belirlenemedi | Grup için yeni bir değerlendirme oluşturmayı deneyin.
Gerekli çekirdek ve bellek içeren VM bulunamadı | Azure uygun bir VM türü bulamadı. Geçirmeden önce şirket içi makinenin bellek ve çekirdek sayısını azaltın.
Bir iç hata nedeniyle VM uygunluğu belirlenemedi | Grup için yeni bir değerlendirme oluşturmayı deneyin.
Bir iç hata nedeniyle bir veya daha fazla disk için uygunluğu belirlenemedi | Grup için yeni bir değerlendirme oluşturmayı deneyin.
Bir iç hata nedeniyle bir veya daha fazla ağ bağdaştırıcısı için uygunluğu belirlenemedi | Grup için yeni bir değerlendirme oluşturmayı deneyin.

## <a name="linux-vms-are-conditionally-ready"></a>Linux VM 'Leri "koşullu olarak hazırlanıyor"

VMware ve Hyper-V VM 'lerinde sunucu değerlendirmesi, sunucu değerlendirmesinde bilinen bir boşluk nedeniyle Linux sanal makinelerini "koşullu olarak hazırlanıyor" olarak işaretler. 

- Bu Aralık, BT 'nin şirket içi VM 'lerde yüklü Linux IŞLETIM sisteminin ikincil sürümünü algılamasını önler.
- Örneğin, RHEL 6,10 için şu anda sunucu değerlendirmesi, işletim sistemi sürümü olarak yalnızca RHEL 6 ' ı algılar. Bunun nedeni, Hyper-V ana bilgisayarının vCenter Server ar, Linux VM işletim sistemleri için çekirdek sürümü sağlamasunmamalıdır.
-  Azure, Linux 'un yalnızca belirli sürümlerini sağladığından, Linux VM 'Leri Şu anda sunucu değerlendirmesi için koşullu olarak kullanılabilir olarak işaretlendi.
- [Azure Linux desteğini](https://aka.ms/migrate/selfhost/azureendorseddistros)inceleyerek, ŞIRKET içi VM 'de çalışan Linux Işletim sisteminin Azure 'da olup olmadığını belirleyebilirsiniz.
-  Doğrulanan dağıtımı doğruladıktan sonra bu uyarıyı yoksayabilirsiniz.

Bu boşluk, VMware VM 'lerinde [uygulama bulma](https://docs.microsoft.com/azure/migrate/how-to-discover-applications) etkinleştirilerek çözülebilir. Sunucu değerlendirmesi, belirtilen konuk kimlik bilgilerini kullanarak VM 'den algılanan işletim sistemini kullanır. Bu işletim sistemi verileri, hem Windows hem de Linux VM 'lerinde doğru IŞLETIM sistemi bilgilerini tanımlar.


## <a name="azure-skus-bigger-than-on-premises"></a>Azure SKU 'Ları Şirket içinden daha büyük

Azure geçişi sunucu değerlendirmesi, Azure VM SKU 'Larını, değerlendirme türüne göre geçerli şirket içi ayırmadan daha fazla çekirdek ve bellek ile önerebilir:


- VM SKU 'SU önerisi, değerlendirme özelliklerine bağlıdır.
- Bu, sunucu değerlendirmesi: *performans tabanlı*veya *Şirket içi olarak*gerçekleştirdiğiniz değerlendirme türünden etkilenir.
- Performans tabanlı değerlendirmelere yönelik sunucu değerlendirmesi, şirket içi sanal makinelerinize yönelik doğru hedef VM SKU 'sunu belirlemede şirket içi VM 'lerin (CPU, bellek, disk ve ağ kullanımı) kullanım verilerini dikkate alır. Ayrıca, etkin kullanımı belirlerken bir rahatlık faktörü ekler.
- Şirket içi boyutlandırma için performans verileri göz önünde bulundurulmaz ve hedef SKU, şirket içi ayırmaya göre önerilir.

Bunun önerileri nasıl etkileyebileceğini göstermek için bir örnek alalım:

Dört çekirdekli ve sekiz GB bellek içeren, %50 CPU kullanımı ve %50 bellek kullanımı ve belirli bir rahatlık 1,3 faktörü içeren bir şirket içi VM sunuyoruz.

-  Değerlendirme **Şirket içinde**ise, dört çekirdekli ve 8 GB bellek Içeren BIR Azure VM SKU 'su önerilir.
- Değerlendirme, etkin CPU ve bellek kullanımı 50 (4 çekirdek * 1,3 = 2,6 çekirdekler 50 ve 8 GB bellek * 1,3 = 5,3-GB bellek) temel alınarak, dört çekirdekli sanal makine SKU 'SU (en yakın desteklenen çekirdek sayısı) ve sekiz GB bellek (en yakın desteklenen bellek boyutu) önerilir.
- Değerlendirme boyutlandırma hakkında [daha fazla bilgi edinin](concepts-assessment-calculation.md#types-of-assessments) .

## <a name="azure-disk-skus-bigger-than-on-premises"></a>Azure disk SKU 'Ları Şirket içinden daha büyük

Azure geçişi sunucu değerlendirmesi, değerlendirme türüne göre daha büyük bir disk önerebilir.
- Sunucu değerlendirmesinde disk boyutlandırma iki değerlendirme özelliklerine bağlıdır: boyutlandırma ölçütleri ve depolama türü.
- Boyutlandırma ölçütü **performans tabanlıdır**ve depolama türü **Otomatik**, IOPS ve aktarım hızı değerleri, hedef disk türü (Standart HDD, standart SSD veya Premium) tanımlanırken kabul edilir. Daha sonra disk türünden bir disk SKU 'SU önerilir ve öneri, şirket içi diskin boyut gereksinimlerini dikkate alır.
- Boyutlandırma ölçütü **performans tabanlıdır**ve depolama türü **Premium**ise, şirket içi diskin IOPS, verimlilik ve Boyut gereksinimlerine bağlı olarak Azure 'DA BIR Premium disk SKU 'su önerilir. Aynı mantık, boyutlandırma ölçütleri **Şirket içinde** olduğunda ve depolama türü **Standart HDD**, **Standart SSD**veya **Premium**olduğunda disk boyutlandırmayı gerçekleştirmek için kullanılır.

Örnek olarak, 32 GB bellek içeren bir şirket içi diskiniz varsa, ancak diskin toplanmış okuma ve yazma ıOPS değeri 800 ıOPS ise, sunucu değerlendirmesi bir Premium disk önerir (daha yüksek ıOPS gereksinimleri nedeniyle) ve ardından gerekli ıOPS ve boyutu destekleyebilen bir disk SKU 'SU önerir. Bu örnekteki en yakın eşleşme P15 (256 GB, 1100 IOPS) olabilir. Şirket içi diskin gerektirdiği boyut 32 GB olsa da, şirket içi diskin yüksek ıOPS gereksinimi nedeniyle sunucu değerlendirmesi daha büyük bir disk önerir.

## <a name="utilized-corememory-percentage-missing"></a>Kullanılan çekirdek/bellek yüzdesi eksik

Sunucu değerlendirmesi, "PercentageOfCoresUtilizedMissing" veya "PercentageOfMemoryUtilizedMissing" raporlarını Azure geçiş gereci ilgili şirket içi VM 'Ler için performans verilerini toplayamayacak şekilde bildirir.

- Bu durum, değerlendirme süresi boyunca VM 'Ler kapatılmışsa ortaya çıkabilir. Gereç, devre dışı bırakıldığında bir VM için performans verilerini toplayamıyor.
- Yalnızca bellek sayaçları eksikse ve Hyper-V VM 'lerini değerlendirmeye çalışıyorsanız, bu VM 'lerde etkin bir dinamik bellek olup olmadığını kontrol edin. Yalnızca Hyper-V VM 'Leri için bir Azure geçişi gerecinin, dinamik belleği etkin olmayan VM 'Ler için bellek kullanım verilerini toplayamediği bilinen bir sorun vardır.
- Performans sayaçlarından herhangi biri eksikse, Azure geçişi sunucu değerlendirmesi ayrılmış çekirdeğe ve belleğe geri döner ve buna karşılık gelen VM boyutunu önerir.
- Tüm performans sayaçları eksikse, değerlendirme için bağlantı noktası erişim gereksinimlerinin karşılandığından emin olun. [VMware](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#port-access), [Hyper-V](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#port-access) ve [fiziksel](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-physical#port-access) sunucu değerlendirmesi için bağlantı noktası erişim gereksinimleri hakkında daha fazla bilgi edinin.

## <a name="is-the-operating-system-license-included"></a>İşletim sistemi lisansı dahil mi?

Azure geçişi sunucu değerlendirmesi Şu anda yalnızca Windows makineler için işletim sistemi lisans maliyetini dikkate alır. Linux makineleri için lisans maliyetleri Şu anda dikkate alınmamaktadır.

## <a name="how-does-performance-based-sizing-work"></a>Performans tabanlı boyutlandırma nasıl çalışır?

Sunucu Değerlendirmesi şirket içi makinelerin performans verilerini sürekli toplar ve bunları Azure’da VM SKU'su ile disk SKU'su önermek için kullanır. Performans tabanlı verilerin nasıl toplandığını [öğrenin](concepts-assessment-calculation.md#calculate-sizing-performance-based) .

## <a name="why-is-my-assessment-showing-a-warning-that-it-was-created-with-an-invalid-combintion-of-reserved-instances-vm-uptime-and-discount-"></a>Değerlendirmem neden, geçersiz bir combintion ayrılmış örnek, VM çalışma süresi ve Indirim (%) ile oluşturulduğunu belirten bir uyarı gösteriyor?
' Ayrılmış örnekler ' seçtiğinizde, ' Discount (%) ' ve ' VM çalışma süresi ' özellikleri geçerli değil. Değerlendirmenizi bu özelliklerin geçersiz bir birleşimiyle oluşturulduğundan, Düzenle ve yeniden hesapla düğmeleri devre dışı bırakılır. Lütfen yeni bir değerlendirme oluşturun. [Daha fazla bilgi edinin](https://go.microsoft.com/fwlink/?linkid=2131554).

## <a name="dependency-visualization-in-azure-government"></a>Azure Kamu 'da bağımlılık görselleştirmesi

Azure geçişi, bağımlılık görselleştirme işlevselliği için Hizmet Eşlemesi bağımlıdır. Hizmet Eşlemesi Şu anda Azure Kamu 'da kullanılamadığından, bu işlev Azure Kamu 'da kullanılamaz.

## <a name="dependencies-dont-show-after-agent-install"></a>Bağımlılıklar, aracı yüklendikten sonra gösterilmez

Şirket içi VM 'Lere bağımlılık görselleştirme aracılarını yükledikten sonra, Azure geçişi genellikle portalda bağımlılıkları göstermek için 15-30 dakika sürer. 30 dakikadan uzun süre beklemişseniz, Microsoft Monitoring Agent (MMA) Log Analytics çalışma alanına bağlanabildiğinizden emin olun.

Windows VM 'Leri için:
1. Denetim Masası 'nda MMA 'yı başlatın.
2. **Microsoft Monitoring Agent properties**  >  **Azure Log Analytics (OMS)** Microsoft Monitoring Agent özelliklerinde, çalışma alanının **durumunun** yeşil olduğundan emin olun.
3. Durum yeşil değilse, çalışma alanını kaldırmayı ve onu yeniden eklemeyi deneyin.

    ![MMA durumu](./media/troubleshoot-assessment/mma-properties.png)

Linux sanal makineleri için, MMA ve bağımlılık aracısının yükleme komutlarının başarılı olduğundan emin olun.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

- **MMS Aracısı**: desteklenen [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems)ve [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems) işletim sistemlerini gözden geçirin.
- **Bağımlılık Aracısı**: desteklenen [Windows ve Linux](../azure-monitor/insights/vminsights-enable-overview.md#supported-operating-systems) işletim sistemleri.

## <a name="visualize-dependencies-for--hour"></a>> saat bağımlılıklarını görselleştirin

Azure geçişi, son ayın belirli bir tarihine geri dönmenize izin verse de, bağımlılıkları görselleştirebileceğiniz en uzun süre bir saattir.

Örneğin, dün bağımlılıklarını görüntülemek için bağımlılık eşlemesindeki süre süresi işlevini kullanabilirsiniz, ancak bunları yalnızca bir saatlik dönem için görüntüleyebilirsiniz.

Ancak, Azure Izleyici günlüklerini daha uzun bir süre boyunca [bağımlılık verilerini sorgulamak](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) için kullanabilirsiniz.

## <a name="visualized-dependencies-for--10-machines"></a>> 10 makineler için görselleştirilen bağımlılıklar

Azure geçişi sunucu değerlendirmesi ' nde, en fazla 10 VM içeren [gruplar için bağımlılıkları görselleştirebilirsiniz](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) . Daha büyük gruplar için, bağımlılıkları görselleştirmek üzere VM 'Leri daha küçük gruplara bölmeniz önerilir.

## <a name="machines-show-install-agent"></a>Makineler "aracıyı yüklemeyi" gösterir

Bağımlılık görselleştirmesi etkinleştirilmiş makineleri Azure 'a geçirdikten sonra makineler, aşağıdaki davranış nedeniyle "bağımlılıkları görüntüle" yerine "aracıyı yüklemek" eylemini gösterebilir:


- Azure 'a geçiş yaptıktan sonra şirket içi makineler kapalıdır ve eşdeğer VM 'Ler Azure 'da kapalıdır. Bu makineler farklı bir MAC adresi edintir.
- Makineler, şirket içi IP adresini elde etmeksizin, aynı zamanda farklı bir IP adresine sahip olabilir.
- MAC ve IP adreslerinin her ikisi de Şirket içinden farklıysa Azure geçişi, şirket içi makineleri Hizmet Eşlemesi bağımlılık verileriyle ilişkilendirmez. Bu durumda, bağımlılıkları görüntülemek yerine aracıyı yüklemek için seçeneği görüntülenir.
- Azure 'a bir test geçişten sonra şirket içi makineler beklendiği gibi açık kalır. Azure 'da bulunan eşdeğer makineler, farklı bir MAC adresi elde edebilir ve farklı IP adresleri alabilir. Bu makinelerden giden Azure Izleyici günlük trafiğini engeletmediğiniz takdirde Azure geçişi, şirket içi makineleri Hizmet Eşlemesi bağımlılık verileriyle ilişkilendirmez ve bu nedenle, bağımlılıkları görüntülemek yerine aracıları yüklemek için seçeneği gösterir.


## <a name="capture-network-traffic"></a>Ağ trafiğini yakala

Ağ trafiği günlüklerini aşağıda gösterildiği gibi toplayın:

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Geliştirici Araçları başlamak için F12 tuşuna basın. Gerekirse, **Gezinti ayarında girişleri temizle** ' yi temizleyin.
3. **Ağ** sekmesini seçin ve ağ trafiğini yakalamaya başlayın:
   - Chrome 'da **günlüğü koru**' yı seçin. Kayıt otomatik olarak başlamalıdır. Kırmızı bir daire trafiğin yakalandığını gösterir. Kırmızı daire görünmezse başlamak için siyah daireyi seçin.
   - Microsoft Edge ve Internet Explorer 'da kayıt otomatik olarak başlamalıdır. Aksi takdirde, yeşil oynatma düğmesini seçin.
4. Hatayı yeniden oluşturmaya çalışın.
5. Kayıt sırasında hata ile karşılaşduktan sonra kaydı durdurun ve kayıtlı etkinliğin bir kopyasını kaydedin:
   - Chrome 'da, sağ tıklayın ve **IÇERIKLE har olarak kaydet**' i seçin. Bu eylem, günlükleri bir. har dosyası olarak sıkıştırır ve dışarı aktarır.
   - Microsoft Edge veya Internet Explorer 'da **yakalanan trafiği dışarı aktar** seçeneğini belirleyin. Bu eylem günlüğü sıkıştırır ve dışarı aktarır.
6. Herhangi bir uyarı veya hata olup olmadığını denetlemek için **konsol** sekmesini seçin. Konsol günlüğünü kaydetmek için:
   - Chrome 'da konsol günlüğünde herhangi bir yere sağ tıklayın. Günlüğe **Kaydet**, dışarı aktarmak için ve günlüğü posta ' yı seçin.
   - Microsoft Edge veya Internet Explorer 'da hatalara sağ tıklayıp **Tümünü Kopyala**' yı seçin.
7. Geliştirici Araçları kapatın.


## <a name="where-is-the-operating-system-data-in-my-assessment-discovered-from"></a>Değerlendirmem içindeki işletim sistemi verileri nerede bulundu?

- VMware VM 'Leri için varsayılan olarak, vCenter tarafından belirtilen işletim sistemi verileri vardır. 
   - VMware Linux VM 'Leri için, uygulama bulma etkinse, işletim sistemi ayrıntıları Konuk VM 'den alınır. Değerlendirmede hangi IŞLETIM sistemi ayrıntılarının olduğunu denetlemek için, bulunan sunucular görünümüne ve fareyi "Işletim sistemi" sütunundaki değerin üzerine gidin. Açılan metinde, gördüğünüz işletim sistemi verilerinin vCenter Server 'dan mı yoksa VM kimlik bilgileri kullanılarak Konuk VM 'den mi toplandığını görebileceksiniz. 
   - Windows VM 'Leri için, işletim sistemi ayrıntıları her zaman vCenter Server getirilir.
- Hyper-V VM 'Leri için, işletim sistemi verileri Hyper-V konağından toplanır
- Fiziksel sunucular için, sunucusundan getirilir.

## <a name="next-steps"></a>Sonraki adımlar

Bir değerlendirme [oluşturun](how-to-create-assessment.md) veya [özelleştirin](how-to-modify-assessment.md) .
