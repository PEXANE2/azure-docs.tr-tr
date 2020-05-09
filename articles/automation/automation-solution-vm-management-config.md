---
title: Azure Otomasyonu yapılandırma VM'leri çalışma saatleri dışında başlat/durdur çözümü
description: Bu makalede, farklı kullanım durumlarını veya senaryolarını desteklemek üzere VM'leri çalışma saatleri dışında başlat/durdur çözümünün nasıl yapılandırılacağı açıklanır.
services: automation
ms.subservice: process-automation
ms.date: 04/01/2020
ms.topic: conceptual
ms.openlocfilehash: 4cceb0d5ada82de73bc74c0ed408f8eb988ea8ec
ms.sourcegitcommit: 602e6db62069d568a91981a1117244ffd757f1c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82864275"
---
# <a name="how-to-configure-startstop-vms-during-off-hours-solution"></a>VM'leri çalışma saatleri dışında başlat/durdur çözümü nasıl yapılandırılır

**VM'leri çalışma saatleri dışında Başlat/Durdur** çözümü ile şunları yapabilirsiniz:

- [VM 'leri başlatıp durdurulacak şekilde zamanlayın](#schedule).
- [Azure etiketlerini kullanarak](#tags) (klasik VM 'lerde desteklenmez), sanal makinelerin artan sırada başlamasını ve durdurulmasını zamanlayın.
- VM 'Leri [düşük CPU kullanımına](#cpuutil)göre otomatik olarak durdurun.

Bu makalede, çözümü bu senaryoları destekleyecek şekilde nasıl başarıyla yapılandıracağınız açıklanır. Çözüm için diğer yaygın yapılandırma ayarlarının nasıl gerçekleştirileceğini de öğrenebilirsiniz, örneğin:

* [E-posta bildirimlerini yapılandırma](#configure-email-notifications)

* [VM ekleme](#add-a-vm)

* [VM hariç tutuma](#exclude-a-vm)

* [Başlatma ve kapatılma zamanlamalarını değiştirme](#modify-the-startup-and-shutdown-schedules)

## <a name="scenario-1-startstop-vms-on-a-schedule"></a><a name="schedule"></a>Senaryo 1: VM 'Leri zamanlamaya göre Başlat/Durdur

Bu senaryo, çözümü ilk kez dağıttığınızda varsayılan yapılandırmadır. Örneğin, bu uygulamayı bir abonelik genelinde tüm VM 'Leri durduracak şekilde yapılandırabilir ve ardından ofisiniz geri döndüğünüzde bu aboneliği sabah içinde başlatabilirsiniz. Dağıtım sırasında **Zamanlanmış-startvm** ve **Zamanlanmış-stopvm** zamanlamalarını yapılandırdığınızda, hedeflenen VM 'leri başlatıp durdurur. Bu çözümü yalnızca VM 'Leri durduracak şekilde yapılandırmak için, bkz. özel bir zamanlamayı yapılandırmayı öğrenmek için [Başlangıç ve kapalı zamanlamaları değiştirme](#modify-the-startup-and-shutdown-schedules) .

> [!NOTE]
> Zamanlama saati parametresini yapılandırırken saat dilimi geçerli saat diliminiz. Ancak, Azure Otomasyonu 'nda UTC biçiminde depolanır. Dağıtım sırasında işlendiği için herhangi bir zaman dilimi dönüştürmesi yapmanız gerekmez.

Şu değişkenleri yapılandırarak hangi VM 'Lerin kapsamda olduğunu denetlersiniz: **External_Start_ResourceGroupNames**, **External_Stop_ResourceGroupNames**ve **External_ExcludeVMNames**.

Eylemi bir abonelik ve kaynak grubuna karşı hedefleyerek veya VM 'lerin belirli bir listesini hedefleyerek, her ikisini birden değil de etkinleştirebilirsiniz.

### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Başlat ve durdur eylemlerini bir aboneliğe ve kaynak grubuna göre hedefleyin

1. Ve `External_ExcludeVMNames` değişkenlerini `External_Stop_ResourceGroupNames` , hedef VM 'leri belirtecek şekilde yapılandırın.

2. **Zamanlanmış-startvm** ve **Zamanlanmış-stopvm** zamanlamalarını etkinleştirin ve güncelleştirin.

3. Değişikliklerinizi önizlemek için **eylem** parametresi alanı **Start** ve **whatIf** parametre alanı true olarak ayarlanmış **ScheduledStartStop_Parent** runbook 'u çalıştırın.

### <a name="target-the-start-and-stop-action-by-vm-list"></a>VM listesine göre Başlat ve Durdur eylemini hedefleme

1. **ScheduledStartStop_Parent** runbook 'U **başlatma** **eylemi** ile çalıştırın, **vmlist** parametre alanına VM 'Lerin virgülle ayrılmış bir listesini ekleyin ve ardından **whatIf** parametre alanını true olarak ayarlayın. Değişikliklerinizi önizleyin.

2. Değişkeni, `External_ExcludeVMNames` virgülle ayrılmış VM 'ler listesiyle yapılandırın (VM1, VM2, VM3).

3. Bu senaryo `External_Start_ResourceGroupNames` ve `External_Stop_ResourceGroupnames` değişkenlerini dikkate almaz. Bu senaryo için kendi Otomasyon zamanlamanızı oluşturmanız gerekir. Ayrıntılar için bkz. [Azure Otomasyonu 'nda runbook 'U zamanlama](../automation/automation-schedules.md).

    > [!NOTE]
    > **Hedef ResourceGroup adları** için değeri hem hem de `External_Start_ResourceGroupNames` için değer olarak depolanır. `External_Stop_ResourceGroupNames` Daha fazla ayrıntı düzeyi için, bu değişkenlerin her birini farklı kaynak gruplarını hedefleyecek şekilde değiştirebilirsiniz. Başlat eylemi için, kullanın `External_Start_ResourceGroupNames`ve Durdur eylemini `External_Stop_ResourceGroupNames` kullanın. VM 'Ler başlatma ve durdurma zamanlamalarına otomatik olarak eklenir.

## <a name="scenario-2-startstop-vms-in-sequence-by-using-tags"></a><a name="tags"></a>Senaryo 2: etiketleri kullanarak sırayla VM 'leri başlatma/durdurma

Dağıtılmış bir iş yükünü destekleyen birden çok VM üzerinde iki veya daha fazla bileşen içeren bir ortamda, bileşenlerin başlatıldığı ve sırasıyla durdurulduğu sırayı destekleme. Aşağıdaki adımları gerçekleştirerek bu senaryoyu gerçekleştirebilirsiniz:

### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Başlat ve durdur eylemlerini bir aboneliğe ve kaynak grubuna göre hedefleyin

1. `External_Start_ResourceGroupNames` Ve `External_Stop_ResourceGroupNames` değişkenlerini `sequencestart` hedefleyen VM `sequencestop` 'lere pozitif bir tamsayı değeri olan bir ve etiketi ekleyin. Başlat ve Durdur eylemleri artan sırada gerçekleştirilir. Bir VM 'nin nasıl etiketleyeceğinizi öğrenmek için bkz. Azure ['Da Windows sanal makinesini etiketleme](../virtual-machines/windows/tag.md) ve [Azure 'Da Linux sanal makinesini etiketleme](../virtual-machines/linux/tag.md).

2. **Sıralı-startvm** ve **sıralı-stopvm** zamanlamalarını gereksinimlerinize uygun tarih ve saate göre değiştirin ve zamanlamayı etkinleştirin.

3. Değişikliklerinizi önizlemek için **Start** ve **whatIf** olarak ayarlanan **ACTION** **SequencedStartStop_Parent** runbook 'unu doğru olarak ayarlayın.

4. Üretim VM 'lerine göre uygulamadan önce eylemin önizlemesini görüntüleyin ve gerekli değişiklikleri yapın. Hazırlandığınızda, parametresini **false**olarak ayarlanmış şekilde el ile yürütün veya Otomasyon zamanlamasın `Sequenced-StartVM` ve `Sequenced-StopVM` otomatik olarak belirtilen zamanlamanızı izleyerek çalıştırın.

### <a name="target-the-start-and-stop-action-by-vm-list"></a>VM listesine göre Başlat ve Durdur eylemini hedefleme

1. Parametreye eklemeyi `sequencestart` planladığınız VM `sequencestop` 'lere pozitif tamsayı değeri olan bir ve bir etiketi ekleyin. `VMList`

2. **SequencedStartStop_Parent** runbook 'U **başlatma** **eylemi** ile çalıştırın, **vmlist** parametre alanında VM 'Lerin virgülle ayrılmış bir listesini ekleyin ve ardından **whatIf** değerini true olarak ayarlayın. Değişikliklerinizi önizleyin.

3. Değişkeni, `External_ExcludeVMNames` virgülle ayrılmış VM 'ler listesiyle yapılandırın (VM1, VM2, VM3).

4. Bu senaryo `External_Start_ResourceGroupNames` ve `External_Stop_ResourceGroupnames` değişkenlerini dikkate almaz. Bu senaryo için kendi Otomasyon zamanlamanızı oluşturmanız gerekir. Ayrıntılar için bkz. [Azure Otomasyonu 'nda runbook 'U zamanlama](../automation/automation-schedules.md).

5. Üretim VM 'lerine göre uygulamadan önce eylemin önizlemesini görüntüleyin ve gerekli değişiklikleri yapın. Hazırlandığınızda, **izleme-ve-Tanılama/izleme-eylem-groupsrunbook** ' u, parametresini **false**olarak ayarlanmış şekilde el ile yürütün. Alternatif olarak, Otomasyon zamanlamalarına `Sequenced-StartVM` izin `Sequenced-StopVM` verin ve önceden belirlenmiş zamanlamanızı izleyerek otomatik olarak çalıştırın.

## <a name="scenario-3-startstop-automatically-based-on-cpu-utilization"></a><a name="cpuutil"></a>Senaryo 3: CPU kullanımına göre otomatik olarak Başlat/Durdur

Bu çözüm, saat sonu gibi yoğun olmayan dönemler sırasında kullanılmayan VM 'Leri değerlendirerek ve işlemci kullanımı belirtilen yüzdeden küçük olduğunda otomatik olarak kapatmak için aboneliğinizdeki Azure Resource Manager ve klasik sanal makinelerin çalışma maliyetini yönetmenize yardımcı olabilir.

Varsayılan olarak, çözüm, ortalama kullanımın yüzde 5 veya daha az olup olmadığını görmek için CPU Ölçümü yüzdesini değerlendirmek üzere önceden yapılandırılmıştır. Bu senaryo aşağıdaki değişkenlerle denetlenir ve varsayılan değerler gereksinimlerinizi karşılamıyorsa değiştirilebilir:

* `External_AutoStop_MetricName`
* `External_AutoStop_Threshold`
* `External_AutoStop_TimeAggregationOperator`
* `External_AutoStop_TimeWindow`
* `External_AutoStop_Frequency`
* `External_AutoStop_Severity`

Eylemi bir abonelik ve kaynak grubuna karşı etkinleştirebilir ve hedefleyebilir ya da belirli bir VM listesini hedefleyebilirsiniz.

**AutoStop_CreateAlert_Parent** runbook 'unu çalıştırdığınızda, hedeflenen aboneliğin, kaynak grubunun ve VM 'lerin var olduğunu doğrular. VM 'Ler mevcutsa, runbook, her doğrulanmış VM için üst runbook tarafından **AutoStop_CreateAlert_Child** runbook 'u çağırır. Bu alt runbook aşağıdakileri gerçekleştirir:

* Doğrulanan her VM için bir ölçüm uyarı kuralı oluşturur.

* CPU, belirtilen zaman aralığı boyunca yapılandırılan eşiğin altına düşerse belirli bir sanal makine için **AutoStop_VM_Child** runbook 'unu tetikler. Bu runbook daha sonra VM 'yi durdurmaya çalışır.

### <a name="to-target-the-auto-stop-action-against-all-vms-in-a-subscription"></a>Otomatik durdurma eylemini bir abonelikteki tüm VM 'Lere göre hedeflemek için

1. `External_Stop_ResourceGroupNames` Değişkenin boş veya * (joker karakter) olarak ayarlandığından emin olun.

2. [İsteğe bağlı adım] Bazı sanal makineleri otomatik kapatmadan dışlamak istiyorsanız, `External_ExcludeVMNames` DEĞIŞKENINE bir VM adları virgülle ayrılmış listesi ekleyebilirsiniz.

3. Aboneliğinizdeki `Schedule_AutoStop_CreateAlert_Parent` tüm VM 'ler için gereken sanal makine ölçümü uyarı kurallarını oluşturmak üzere zamanlamayı çalıştırmaya izin vermeyi etkinleştirin. Bu tür bir zamanlamayı çalıştırmak, aboneliğe yeni VM 'Ler eklendikçe yeni ölçüm uyarı kuralları oluşturmanıza olanak sağlar.

### <a name="to-target-the-auto-stop-action-against-all-vms-in-a-resource-group-or-multiple-resource-groups"></a>Otomatik durdurma eylemini bir kaynak grubundaki veya birden çok kaynak grubundaki tüm VM 'Lere göre hedeflemek için

1. Değişkene, `External_Stop_ResourceGroupNames` kaynak grubu adlarının virgülle ayrılmış bir listesini ekleyin.

2. Sanal makinelerin bazılarını otomatik kapatmadan dışlamak istiyorsanız, `External_ExcludeVMNames` değişkene ayrılmış bir VM adları listesi ekleyebilirsiniz.

3. Kaynak gruplarınızın tüm VM 'Leri için gereken sanal makine ölçümü uyarı kurallarını oluşturmak üzere **Schedule_AutoStop_CreateAlert_Parent** zamanlamasını çalıştırmaya izin vermeyi etkinleştirin. Bu işlemi bir zamanlamaya göre çalıştırmak, yeni VM 'Ler kaynak gruplarına eklendikçe yeni ölçüm uyarı kuralları oluşturmanıza olanak sağlar.

### <a name="to-target-the-autostop-action-to-a-list-of-vms"></a>Bir VM listesine oto durdurma eylemini hedeflemek için

1. Yeni bir [zamanlama](shared-resources/schedules.md#create-a-schedule) oluşturun ve bu `VMList` parametreyi **AutoStop_CreateAlert_Parent** runbook 'una bağlayın, bu da parametreye virgülle ayrılmış bir VM adları listesi ekler.

2. İsteğe bağlı olarak, bazı sanal makineleri otomatik kapatmadan dışlamak istiyorsanız, `External_ExcludeVMNames` değişkenine bir sanal makine adları virgülle ayrılmış listesini ekleyebilirsiniz.

## <a name="configure-email-notifications"></a>E-posta bildirimlerini yapılandırma

Çözüm dağıtıldıktan sonra e-posta bildirimlerini değiştirmek için, dağıtım sırasında oluşturulan eylem grubunu değiştirin.  

> [!NOTE]
> Azure Kamu bulutundaki abonelikler, bu çözümün e-posta işlevlerini desteklemez.

1. Azure portal **izleyici**' ye ve ardından **eylem grupları**' na gidin. **StartStop_VM_Notication**adlı eylem grubunu seçin.

    ![Otomasyon Güncelleştirme Yönetimi çözüm sayfası](media/automation-solution-vm-management/azure-monitor.png)

2. **StartStop_VM_Notification** **sayfasında Ayrıntılar ' ın**altındaki **Ayrıntıları Düzenle** ' ye tıklayın. Bu, **e-posta/SMS/Push/ses** sayfasını açar. E-posta adresini güncelleştirip **Tamam** ' a tıklayarak değişikliklerinizi kaydedin.

    ![Otomasyon Güncelleştirme Yönetimi çözüm sayfası](media/automation-solution-vm-management/change-email.png)

    Alternatif olarak, eylem grubu hakkında daha fazla bilgi edinmek için eylem grubuna ek eylemler ekleyebilirsiniz, eylem [grupları](../azure-monitor/platform/action-groups.md) konusuna bakın

Aşağıda, çözüm sanal makineleri kapattığında gönderilen örnek bir e-posta verilmiştir.

![Otomasyon Güncelleştirme Yönetimi çözüm sayfası](media/automation-solution-vm-management/email.png)

## <a name="addexclude-vms"></a><a name="add-exclude-vms"></a>VM 'Leri ekle/çıkar

Çözüm, çözüm tarafından hedeflenen VM 'Leri ekleme veya makineleri çözümden özel olarak hariç tutma yeteneği sağlar.

### <a name="add-a-vm"></a>VM ekleme

Çalıştığında, bir VM 'nin çalıştırıldığında Başlat/Durdur çözümüne eklendiğinden emin olmak için kullanabileceğiniz iki seçenek vardır.

* Çözümün üst [runbook](automation-solution-vm-management.md#runbooks) 'larının her biri bir `VMList` parametreye sahiptir. Durumunuza uygun üst runbook 'u zamanlarken ve bu VM 'Ler çözüm çalışırken dahil edilse bu parametreye, virgülle ayrılmış bir VM adları listesi geçirebilirsiniz.

* Birden çok VM seçmek için, `External_Start_ResourceGroupNames` başlatmak `External_Stop_ResourceGroupNames` veya durdurmak istediğiniz VM 'leri içeren kaynak grubu adlarını ayarlayın. Ayrıca, çözümün abonelikteki tüm kaynak grupları üzerinde çalışmasını sağlamak `*` için değişkenlerini değerine ayarlayabilirsiniz.

### <a name="exclude-a-vm"></a>VM hariç tutuma

Bir VM 'yi çözümden dışlamak için, `External_ExcludeVMNames` değişkenine ekleyebilirsiniz. Bu değişken, Başlat/Durdur çözümünün dışında tutulacak belirli VM 'lerin virgülle ayrılmış listesidir. Bu liste 140 VM 'Lerle sınırlıdır. Bu virgülle ayrılmış listeye 140 'den fazla VM eklerseniz, hariç tutulacak şekilde ayarlanan VM 'Ler istenmeden başlatılabilir veya durdurulabilir.

## <a name="modify-the-startup-and-shutdown-schedules"></a>Başlatma ve kapatılma zamanlamalarını değiştirme

Bu çözümdeki başlangıç ve kapalı zamanlamaların yönetilmesi, [Azure Otomasyonu 'nda runbook zamanlamaya](automation-schedules.md)göre belirtilen adımların aynısını izler. VM 'Leri başlatmak ve durdurmak için ayrı bir zamanlama olması gerekir.

Yalnızca belirli bir zamanda VM 'Leri durdurmak için çözümü yapılandırma desteklenir. Bu senaryoda, yalnızca bir **Dur** zamanlaması oluşturmanız ve buna karşılık gelen bir **Başlangıç** zamanlaması yapmanız gerekir. Bunu yapmanız için gerekenler:

1. `External_Stop_ResourceGroupNames` Değişkende kapatılacak VM 'ler için kaynak grupları eklendiğinden emin olun.

2. VM 'Leri kapatmak istediğiniz zaman için kendi zamanlamanızı oluşturun.

3. **ScheduledStartStop_Parent** runbook 'a gidin ve **zamanla**' ya tıklayın. Bu, önceki adımda oluşturduğunuz zamanlamayı seçmenizi sağlar.

4. **Parametreler ' i seçin ve ayarları çalıştırın** ve **eylem** alanını **Durdur**olarak ayarlayın.

5. Değişikliklerinizi kaydetmek için **Tamam**’ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

* VM'leri çalışma saatleri dışında başlat/durdur sorunlarını giderme hakkında bilgi edinmek için bkz. [VM 'Leri başlatma/durdurma sorunlarını giderme](troubleshoot/start-stop-vm.md).

* Başlat/Durdur VM 'lerinden Otomasyon Runbook işlerinin durumunu çözümlemek için Azure Izleyici günlüklerine yazılan Otomasyon kayıtlarını ve örnek günlük arama sorgularını [gözden geçirin](automation-solution-vm-management-logs.md) .
