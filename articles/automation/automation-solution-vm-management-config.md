---
title: Başlat/Durdur VM çözümlerini yapılandır
description: Bu makalede, farklı kullanım örnekleri veya senaryoları desteklemek için mesai saatleri dışında Başlat/Durdur VM'leri nasıl yapılandırılabilen açıklanmaktadır.
services: automation
ms.subservice: process-automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: d3ca8d17d6637f0ab2b5a5d3d7a99ac0beaafd2e
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80550389"
---
# <a name="how-to-configure-startstop-vms-during-off-hours-solution"></a>Mesai saatleri dışında Başlat/Durdur VM'leri yapılandırma

Mesai saatleri dışında Başlat/Durdur çözümü sırasında VM'ler ile şunları yapabilirsiniz:

- [Başlatmak ve durdurmak için VM'leri zamanlayın.](#schedule)
- [Azure Etiketleri kullanarak](#tags) (klasik VM'ler için desteklenmeyen) vm'leri artan sırada başlatmak ve durdurmak için zamanlama.
- Düşük CPU kullanımına göre Otomatik durdurma [VM'leri.](#cpuutil)

Bu makalede, bu senaryoları desteklemek için çözüm başarıyla yapılandırmak için nasıl açıklanmaktadır. Çözüm için diğer yaygın yapılandırma ayarlarını nasıl gerçekleştirebileceğinizi de öğrenebilirsiniz:

* [E-posta bildirimlerini yapılandırma](#configure-email-notifications)

* [VM ekleme](#add-a-vm)

* [VM hariç tutuma](#exclude-a-vm)

* [Başlatma ve kapatma zamanlamalarını değiştirme](#modify-the-startup-and-shutdown-schedules)

## <a name="scenario-1-startstop-vms-on-a-schedule"></a><a name="schedule"></a>Senaryo 1: Programda VM'leri Başlat/Durdur

Bu senaryo, çözümü ilk dağıttığınızda varsayılan yapılandırmadır. Örneğin, akşam işten ayrıldığınızda abonelikteki tüm VM'leri durduracak ve sabah ofise döndüğünüzde başlatacak şekilde yapılandırabilirsiniz. Dağıtım sırasında **Zamanlanmış-BaşlangıçVM** ve **Zamanlanmış StopVM** zamanlamalarını yapılandırdığınızda, hedeflenen VM'leri başlatır ve durdurur. Bu çözümü yalnızca VM'leri durduracak şekilde yapılandırışla, [bkz.](#modify-the-startup-and-shutdown-schedules)

> [!NOTE]
> Zamanlama saat parametresini yapılandırdığınızda saat dilimi, geçerli saat diliminizdir. Ancak, Azure Otomasyonu'nda UTC formatında depolanır. Dağıtım sırasında bu işlenir gibi herhangi bir saat dilimi dönüştürme yapmak zorunda değildir.

Aşağıdaki değişkenleri External_Start_ResourceGroupNames **,** **External_Stop_ResourceGroupNames**ve **External_ExcludeVMNames.**

Bir abonelik ve kaynak grubuna karşı eylemi hedeflemeyi veya belirli bir VM listesini hedeflemeyi etkinleştirebilirsiniz, ancak her ikisini birden hedeflemeyi etkinleştirebilirsiniz.

### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Abonelik ve kaynak grubuna yönelik eylemleri başlatma ve durdurma yı hedefleme

1. Hedef VM'leri belirtmek için **External_Stop_ResourceGroupNames** ve **External_ExcludeVMNames** değişkenleri yapılandırın.

2. **Zamanlanmış-StartVM** ve **Zamanlanmış StopVM** zamanlamalarını etkinleştirin ve güncelleştirin.

3. **Başlamak** üzere EYLEM parametresi ayarlanmış **ScheduledStartStop_Parent** runbook'u ve değişikliklerinizi önizlemek için **TRUE** olarak ayarlanmış WHATIF parametresini çalıştırın.

### <a name="target-the-start-and-stop-action-by-vm-list"></a>VM listesine göre eylemi başlat ve durdurun

1. **Başlatmak**için ACTION parametresi ayarlanmış **ScheduledStartStop_Parent** runbook çalıştırın, *VMList* parametresinde vms virgülden ayrılmış bir listesini ekleyin ve sonra **WHATIF parametresini True**olarak ayarlayın. Değişikliklerinizi önizleyin.

2. **External_ExcludeVMNames** parametresini virgülden ayrılmış vm listesiyle (VM1, VM2, VM3) yapılandırın.

3. Bu **senaryo, External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupnames** değişkenleri onurlandırmaz. Bu senaryo için kendi Otomasyon zamanlamanızı oluşturmanız gerekir. Ayrıntılar için Azure [Otomasyonu'nda bir runbook Zamanlama'ya](../automation/automation-schedules.md)bakın.

    > [!NOTE]
    > **Hedef Kaynak Grubu Adları** için değer hem **External_Start_ResourceGroupNames** hem de **External_Stop_ResourceGroupNames**için değer olarak depolanır. Daha fazla ayrıntı için, bu değişkenlerin her birini farklı kaynak gruplarını hedefleyecek şekilde değiştirebilirsiniz. Start action için **External_Start_ResourceGroupNames**ve eylemi durdurmak için **External_Stop_ResourceGroupNames**kullanın. VM'ler otomatik olarak başlangıç ve durdurma zamanlamalarına eklenir.

## <a name="scenario-2-startstop-vms-in-sequence-by-using-tags"></a><a name="tags"></a>Senaryo 2: Etiketleri kullanarak VMS'yi sırayla başlat/durdur

Dağıtılmış bir iş yükünü destekleyen birden çok VM'de iki veya daha fazla bileşen içeren bir ortamda, bileşenlerin sırayla başlatıldıve durdurulduğu sırayı desteklemek önemlidir. Aşağıdaki adımları gerçekleştirerek bu senaryoyu gerçekleştirebilirsiniz:

### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Abonelik ve kaynak grubuna yönelik eylemleri başlatma ve durdurma yı hedefleme

1. **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames** değişkenlerde hedeflenen VM'lere pozitif tamsayı değerine sahip bir **sequencestart** ve **sequencestop** etiketi ekleyin. Başlangıç ve durdurma eylemleri artan sırada gerçekleştirilir. VM'yi nasıl etiketleyeceklerini öğrenmek için [Azure'da Windows Sanal Makineyi Etiketle](../virtual-machines/windows/tag.md) ve [Azure'da Linux Sanal Makineyi Etiketle](../virtual-machines/linux/tag.md)'ye bakın.

2. **Sıralı-BaşlangıçVM** ve **Sıralı-StopVM** zamanlamalarını gereksinimlerinizi karşılayan tarih ve saate göre değiştirin ve zamanlamayı etkinleştirin.

3. SequencedStartStop_Parent **SequencedStartStop_Parent** runbook'u, **başlamak** üzere EYLEM parametresi ve değişikliklerinizi önizlemek için **True** olarak ayarlanmış WHATIF parametresi ile çalıştırın.

4. Eylemi önizleyin ve üretim VM'lerine karşı uygulamadan önce gerekli değişiklikleri yapın. Hazır olduğunuzda, çalışma kitabını **False**olarak ayarlanmış parametreyle el ile çalıştırın veya Otomasyon zamanlaması **Sıralı-StartVM** ve **Sıralı-StopVM'un** öngörülen zamanlamanızı izleyerek otomatik olarak çalışmasına izin verin.

### <a name="target-the-start-and-stop-action-by-vm-list"></a>VM listesine göre eylemi başlat ve durdurun

1. **VMList** parametresine eklemeyi planladığınız VM'lere pozitif tamsayı değerine sahip bir **sequencestart** ve **sequencestop** etiketi ekleyin.

2. **Başlamak**için ACTION parametresi ayarlanmış **SequencedStartStop_Parent** runbook çalıştırın, *VMList* parametresinde vm'lerin virgülden ayrılmış bir listesini ekleyin ve ardından WHATIF parametresini **True**olarak ayarlayın. Değişikliklerinizi önizleyin.

3. **External_ExcludeVMNames** parametresini virgülden ayrılmış vm listesiyle (VM1, VM2, VM3) yapılandırın.

4. Bu **senaryo, External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupnames** değişkenleri onurlandırmaz. Bu senaryo için kendi Otomasyon zamanlamanızı oluşturmanız gerekir. Ayrıntılar için Azure [Otomasyonu'nda bir runbook Zamanlama'ya](../automation/automation-schedules.md)bakın.

5. Eylemi önizleyin ve üretim VM'lerine karşı uygulamadan önce gerekli değişiklikleri yapın. Hazır olduğunuzda, izleme ve tanılama/izleme-eylem-grupları, **False**olarak ayarlanmış parametre ile el ile çalıştırın veya Otomasyon zamanlaması **Sıralı-StartVM** ve **Sequenced-StopVM'in** öngörülen zamanlamanızı izleyerek otomatik olarak çalışmasına izin verin.

## <a name="scenario-3-startstop-automatically-based-on-cpu-utilization"></a><a name="cpuutil"></a>Senaryo 3: CPU kullanımına göre otomatik olarak başlat/durdur

Bu çözüm, saat sonrası gibi yoğun olmayan dönemlerde kullanılmayan VM'leri değerlendirerek ve işlemci kullanımı belirli bir yüzdeden azsa bunları otomatik olarak kapatarak, aboneliğinizde Azure Kaynak Yöneticisi ve Klasik sanal makineleri çalıştırmanın maliyetini yönetmenize yardımcı olabilir.

Varsayılan olarak, ortalama kullanım yüzde 5 veya daha az olup olmadığını görmek için yüzde CPU metrik değerlendirmek için çözüm önceden yapılandırılmıştır. Bu senaryo aşağıdaki değişkenler tarafından denetlenir ve varsayılan değerler gereksinimlerinizi karşılamazsa değiştirilebilir:

* **External_AutoStop_MetricName**

* **External_AutoStop_Threshold**

* **External_AutoStop_TimeAggregationOperator**

* **External_AutoStop_TimeWindow**

* **External_AutoStop_Frequency**

* **External_AutoStop_Severity**

Bir abonelik ve kaynak grubuna karşı eylemi etkinleştirebilir ve hedefleyebilir veya belirli bir VM listesini hedefleyebilirsiniz.

**AutoStop_CreateAlert_Parent** runbook çalıştırdığınızda, hedeflenen abonelik, kaynak grubu(lar) ve VM'ler olduğunu doğrular. VM'ler varsa, ana runbook tarafından doğrulanmış her VM için **AutoStop_CreateAlert_Child** runbook çağırır. Bu alt çalışma defteri aşağıdakileri gerçekleştirir:

* Doğrulanmış her VM için bir metrik uyarı kuralı oluşturur.

* CPU belirtilen zaman aralığı için yapılandırılmış eşiğin altına düşerse, belirli bir VM için **AutoStop_VM_Child** runbook tetikler. Bu runbook sonra VM durdurmaya çalışır.

### <a name="to-target-the-auto-stop-action-against-all-vms-in-a-subscription"></a>Abonelikteki tüm VM'lere karşı otomatik durdurma eylemini hedeflemek için

1. **External_Stop_ResourceGroupNames** değişkeninboş olduğundan veya * (joker karakter) olarak ayarlandığından emin olun.

2. [İsteğe bağlı adım] Bazı VM'leri otomatik kapatmadan hariç tutmak istiyorsanız, **External_ExcludeVMNames** değişkenine virgülle ayrılmış VM adlarının listesini ekleyebilirsiniz.

3. Aboneliğinizdeki tüm VM'ler için gerekli Stop VM metrik uyarı kurallarını oluşturmak için **Schedule_AutoStop_CreateAlert_Parent** zamanlamanın çalışmasını etkinleştirin. Bunu bir zamanlamada çalıştırmak, aboneye yeni VM'ler eklendikçe yeni metrik uyarı kuralları oluşturmanıza olanak sağlar.

### <a name="to-target-the-auto-stop-action-against-all-vms-in-a-resource-groupmultiple-resource-groups"></a>Kaynak grubundaki/çoklu kaynak gruplarındaki tüm VM'lere karşı otomatik durdurma eylemini hedeflemek için

1. **External_Stop_ResourceGroupNames** değişkenine kaynak grubu adlarının ortak bir ayrılmış listesini ekleyin.

2. İsteğe bağlı olarak, bazı VM'leri otomatik kapatmadan hariç tutmak istiyorsanız, **External_ExcludeVMNames** değişkenine virgülle ayrılmış vm adlarının listesini ekleyebilirsiniz.

3. Kaynak gruplarınızdaki tüm VM'ler için gerekli **Stop VM metrik** uyarı kurallarını oluşturmak için **Schedule_AutoStop_CreateAlert_Parent** zamanlamanın çalışmasını etkinleştirin. Bunu bir zamanlamada çalıştırmak, kaynak grubuna yeni VM'ler eklendikçe yeni metrik uyarı kuralları oluşturmanıza olanak tanır.

### <a name="to-target-the-auto-stop-action-to-a-list-of-vms"></a>Otomatik durdurma eylemini VM listesine hedeflemek için

1. **VMList** parametresine virgülle ayrılmış VM adlarının listesini ekleyerek yeni bir [Zamanlama](shared-resources/schedules.md#creating-a-schedule) oluşturun ve **AutoStop_CreateAlert_Parent** runbook'a bağlayın.

2. İsteğe bağlı olarak, bazı VM'leri otomatik kapatmadan hariç tutmak istiyorsanız, **External_ExcludeVMNames** değişkenine virgülle ayrılmış VM adları listesi ekleyebilirsiniz.

## <a name="configure-email-notifications"></a>E-posta bildirimlerini yapılandırma

Çözüm dağıtıldıktan sonra e-posta bildirimlerini değiştirmek için dağıtım sırasında oluşturulan eylem grubunu değiştirin.  

> [!NOTE]
> Azure Kamu Bulutu'ndaki abonelikler bu çözümün e-posta işlevselliğini desteklemez.

1. Azure portalında, İzleme -> Eylem gruplarına gidin. **StartStop_VM_Notication**başlıklı eylem grubunu seçin.

    ![Otomasyon Güncelleme Yönetimi çözüm sayfası](media/automation-solution-vm-management/azure-monitor.png)

2. **StartStop_VM_Notification** sayfasında Ayrıntılar **altında**ayrıntıları **edit'i** tıklatın. Bu, **E-posta/SMS/Push/Voice** sayfasını açar. E-posta adresini güncelleştirin ve değişikliklerinizi kaydetmek için **Tamam'ı** tıklatın.

    ![Otomasyon Güncelleme Yönetimi çözüm sayfası](media/automation-solution-vm-management/change-email.png)

    Alternatif olarak, eylem grupları hakkında daha fazla bilgi edinmek, [eylem gruplarına](../azure-monitor/platform/action-groups.md) bakın

Çözüm sanal makineleri kapattığında gönderilen örnek bir e-posta aşağıda verilmiştir.

![Otomasyon Güncelleme Yönetimi çözüm sayfası](media/automation-solution-vm-management/email.png)

## <a name="addexclude-vms"></a><a name="add-exclude-vms"></a>VM Ekleme/Hariç Tutma

Çözüm, çözüm tarafından hedeflenecek VM'ler ekleme veya özellikle makineleri çözümden dışlama olanağı sağlar.

### <a name="add-a-vm"></a>VM ekleme

Bir VM'nin çalıştırıldığında Başlat/Durdur çözümüne dahil edildiğinden emin olmak için kullanabileceğiniz iki seçenek vardır.

* Çözümün üst [runbook'larının](automation-solution-vm-management.md#runbooks) her birinin bir **VMList** parametresi vardır. Durumunuz için uygun üst basamak runbook'u zamanlarken bu parametreye virgülle ayrılmış vm adlarının listesini geçirebilirsiniz ve çözüm çalıştığında bu VM'ler eklenecektir.

* Birden çok VM seçmek için, başlatmak veya durdurmak istediğiniz VM'leri içeren kaynak grubu adlarıyla **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames** ayarlayın. Çözümün abonelikteki tüm `*`kaynak gruplarına karşı çalıştırılması için bu değeri de ayarlayabilirsiniz.

### <a name="exclude-a-vm"></a>VM hariç tutuma

Bir VM'yi çözümden çıkarmak **için, External_ExcludeVMNames** değişkenine ekleyebilirsiniz. Bu değişken, Başlat/Durdur çözümünden hariç tutmak için belirli VM'lerin virgülle ayrılmış bir listesidir. Bu liste 140 VM ile sınırlıdır. Bu virgülle ayrılmış listeye 140'tan fazla VM eklerseniz, hariç tutulacak şekilde ayarlanan VM'ler yanlışlıkla başlatılabilir veya durdurulabilir.

## <a name="modify-the-startup-and-shutdown-schedules"></a>Başlatma ve kapatma zamanlamalarını değiştirme

Bu çözümdeki başlangıç ve kapatma zamanlamalarını yönetmek, [Azure Otomasyonu'nda bir runbook Zamanlama'da](automation-schedules.md)belirtildiği gibi aynı adımları izler. VM'leri başlatmak ve durdurmak için ayrı bir zamanlama olması gerekir.

Çözümü, VM'leri belirli bir zamanda durduracak şekilde yapılandırılmak desteklenir. Bu senaryoda, bir **Stop** zamanlamasını ve karşılık gelen **Başlangıç** zamanlanmamış bir zamanlama oluşturursunuz. Bunu yapmanız için gerekenler:

1. **External_Stop_ResourceGroupNames** değişkeninde VM'lerin kapanması için kaynak gruplarını eklediğinizden emin olun.

2. VM'leri kapatmak istediğiniz süre için kendi zamanlamanızı oluşturun.

3. **ScheduledStartStop_Parent** runbook'a gidin ve **Zamanlama'yı**tıklatın. Bu, önceki adımda oluşturduğunuz zamanlamayı seçmenize olanak tanır.

4. Parametreler'i seçin **ve ayarları çalıştırın** ve **EYLEM** parametresini **Durdur'a**ayarlayın.

5. Değişikliklerinizi kaydetmek için **Tamam**’ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

* Mesai saatleri dışında Başlat/Durdur VM'leri nasıl sorun gidereceklerini öğrenmek için [Sorun Giderme Başlat/Durdur VM'leri'ne](troubleshoot/start-stop-vm.md)bakın.

* Başlat/Durdur VM'lerden Otomasyon runbook işlerinin durumunu çözümlemek için Azure Monitor Günlükleri'ne ve örnek günlük arama sorgularına yazılan Otomasyon kayıtlarını [gözden geçirin.](automation-solution-vm-management-logs.md)
