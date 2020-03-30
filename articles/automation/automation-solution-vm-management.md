---
title: Mesai dışı çözüm sırasında VM'leri Başlat/Durdur
description: Bu VM yönetimi çözümü, Azure Kaynak Yöneticisi sanal makinelerinizi bir zamanlamada başlatır ve durdurur ve Azure Monitor günlüklerini proaktif olarak izler.
services: automation
ms.subservice: process-automation
ms.date: 02/25/2020
ms.topic: conceptual
ms.openlocfilehash: cbf181b9a6d3860854c7b61cca0e6c50810cced9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79278551"
---
# <a name="startstop-vms-during-off-hours-solution-in-azure-automation"></a>Azure Otomasyonu'nda VM’leri çalışma saatleri dışında Başlatma/Durdurma çözümü

Mesai dışı çözüm başlangıcında Başlat/Durdur VM'leri, Azure sanal makinelerinizi kullanıcı tanımlı zamanlamalarda durdurur, Azure Monitor günlükleri aracılığıyla öngörüler sağlar ve [eylem gruplarını](../azure-monitor/platform/action-groups.md)kullanarak isteğe bağlı e-postalar gönderir. Çoğu senaryo için hem Azure Kaynak Yöneticisi'ni hem de klasik VM'leri destekler. Bu çözümü Klasik VM'lerle kullanmak için varsayılan olarak oluşturulmayan bir Klasik RunAs hesabına ihtiyacınız vardır. Klasik RunAs hesabı oluşturma yönergeleri [için, Klasik Run-As Hesapları'na](automation-create-standalone-account.md#classic-run-as-accounts)bakın.

> [!NOTE]
> Mesai saatleri dışında Başlat/Durdur vm'leri, çözümü dağıttığınızda Otomasyon Hesabınıza aktarılan Azure modülleriyle sınanmıştır. Çözüm şu anda Azure modülünün yeni sürümleriyle çalışmıyor. Bu yalnızca mesai dışı çözüm sırasında Başlat/Durdur VM'lerini çalıştırmak için kullandığınız Otomasyon Hesabını etkiler. [Azure PowerShell modüllerini Azure Otomasyonu'nda nasıl güncelleştirebileceğinizde](automation-update-azure-modules.md) açıklandığı gibi diğer Otomasyon Hesaplarınızda Azure modülünün daha yeni sürümlerini kullanmaya devam edebilirsiniz

Bu çözüm, VM maliyetlerini optimize etmek isteyen kullanıcılar için merkezi olmayan düşük maliyetli bir otomasyon seçeneği sağlar. Bu çözümle şunları yapabilirsiniz:

- Başlatmak ve durdurmak için VM'leri zamanlayın.
- Azure Etiketleri kullanarak (klasik VM'ler için desteklenmeyen) vm'leri artan sırada başlatmak ve durdurmak için zamanlama.
- Düşük CPU kullanımına dayalı Otomatik Durdurma VM'leri.

Geçerli çözümle ilgili sınırlamalar şunlardır:

- Bu çözüm, herhangi bir bölgedeki VM'leri yönetir, ancak yalnızca Azure Otomasyon hesabınızla aynı abonelikte kullanılabilir.
- Bu çözüm, Bir Log Analytics çalışma alanını, Azure Otomasyon hesabını ve Uyarıları destekleyen tüm bölgeleriçin Azure ve AzureGov'da kullanılabilir. AzureGov bölgeleri şu anda e-posta işlevselliğini desteklememektedir.

> [!NOTE]
> Çözümü klasik VM'ler için kullanıyorsanız, tüm VM'leriniz bulut hizmeti başına sırayla işlenir. Sanal makineler hala farklı bulut hizmetleri arasında paralel olarak işlenir. Bulut hizmeti başına 20'den fazla VM'niz varsa, üst çalışma kitabı **ScheduledStartStop_Parent** birden çok zamanlama oluşturmanızı ve zamanlama başına 20 VM belirtmenizi öneririz. Zamanlama özelliklerinde, virgülle ayrılmış bir liste olarak belirtin, **VMList** parametresinde VM adları. Aksi takdirde, bu çözüm için Otomasyon işi üç saatten fazla çalışırsa, geçici olarak boşaltılır veya [adil paylaşım](automation-runbook-execution.md#fair-share) limitine göre durdurulur.
>
> Azure Bulut Çözüm Sağlayıcısı (Azure CSP) abonelikleri yalnızca Azure Kaynak Yöneticisi modelini destekler, Programda Azure Kaynak Yöneticisi olmayan hizmetler kullanılamaz. Başlat/Durdur çözümü çalıştığında, klasik kaynakları yönetmek için cmdlets olduğu gibi hatalar alabilirsiniz. CSP hakkında daha fazla bilgi edinmek için [CSP aboneliklerinde kullanılabilir hizmetlere](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services)bakın. Bir CSP aboneliği kullanıyorsanız, [**dağıtımdan**](#variables) sonra External_EnableClassicVMs değişkenini **False** olarak değiştirmelisiniz.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu çözümün runbook'ları bir [Azure Run As hesabıyla](automation-create-runas-account.md)çalışır. Run As hesabı tercih edilen kimlik doğrulama yöntemidir, çünkü süresi dolan veya sık sık değişebilecek bir parola yerine sertifika kimlik doğrulaması kullanır.

Başlat/Durdur VM çözümü için ayrı bir Otomasyon Hesabı kullanmanızı öneririz. Bunun nedeni, Azure modül sürümlerinin sık sık yükseltilmesi ve parametrelerinin değişebileceğidir. Başlat/Durdur VM çözümü aynı cadence üzerinde yükseltilmez, bu nedenle kullandığı cmdletlerin yeni sürümleriyle çalışmayabilir. Ayrıca, modül güncellemelerini üretim Otomasyon Hesabınıza (lar) aktarmadan önce bir test Otomasyon Uyrması Hesabında test etmenizi öneririz.

### <a name="permissions-needed-to-deploy"></a>Dağıtmak için gereken izinler

Bir kullanıcının mesai saatleri dışında Başlat/Durdur VM'lerini dağıtması gereken bazı izinler vardır. Önceden oluşturulmuş bir Otomasyon Hesabı ve Log Analytics çalışma alanı kullanıyorsanız veya dağıtım sırasında yenilerini oluştururken bu izinler farklıdır. Azure Active Directory kiracınızda abonelikte Katkıda Bulunan ve Global Yöneticiyseniz, aşağıdaki izinleri yapılandırmanız gerekmez. Bu haklara sahip değilseniz veya özel bir rolü yapılandırmanız gerekiyorsa, aşağıda gerekli izinlere bakın.

#### <a name="pre-existing-automation-account-and-log-analytics-workspace"></a>Önceden varolan Otomasyon Hesabı ve Log Analytics çalışma alanı

Çalışma saatleri dışında Başlat/Durdur VM'lerini mevcut bir Otomasyon Hesabı ve Log Analytics çalışma alanına dağıtmak için, çözümü dağıtan kullanıcı **Kaynak Grubu'nda**aşağıdaki izinleri gerektirir. Roller hakkında daha fazla bilgi edinmek [için Azure kaynakları için Özel rolleri](../role-based-access-control/custom-roles.md)görün.

| İzin | Kapsam|
| --- | --- |
| Microsoft.Automation/automationAccounts/read | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/variables/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/schedules/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/runbooks/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/connections/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/certificates/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/modules/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/modules/read | Kaynak Grubu |
| Microsoft.automation/automationAccounts/jobSchedules/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/jobs/write | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/jobs/read | Kaynak Grubu |
| Microsoft.OperationsManagement/solutions/write | Kaynak Grubu |
| Microsoft.OperationalInsights/çalışma alanları/* | Kaynak Grubu |
| Microsoft.Insights/diagnosticSettings/write | Kaynak Grubu |
| Microsoft.Insights/ActionGroups/Write | Kaynak Grubu |
| Microsoft.Insights/ActionGroups/read | Kaynak Grubu |
| Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak Grubu |
| Microsoft.Resources/deployments/* | Kaynak Grubu |

#### <a name="new-automation-account-and-a-new-log-analytics-workspace"></a>Yeni Otomasyon Hesabı ve yeni Log Analytics çalışma alanı

Mesai saatleri dışında Başlat/Durdur VM'lerini yeni bir Otomasyon Hesabı ve Log Analytics çalışma alanına dağıtmak için, çözümü dağıtan kullanıcının önceki bölümde tanımlanan izinlerin yanı sıra aşağıdaki izinlere ihtiyacı vardır:

- Abonelikte yardımcı yönetici - Yalnızca Klasik VM'leri yönetecekseniz, Klasik Çalıştır Hesabı'nı oluşturmak gerekir. [Klasik RunAs Hesapları](automation-create-standalone-account.md#classic-run-as-accounts) artık varsayılan olarak oluşturulmaz.
- Azure Active [Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md) **Application Developer** rolünün bir üyesi. Hesap Olarak Çalıştır'ı yapılandırma hakkında daha fazla bilgi [için, Çalıştır'ı hesaplar olarak yapılandırmak için İzinler'e](manage-runas-account.md#permissions)bakın.
- Aboneye veya aşağıdaki izinlere katkıda bulunan kişi.

| İzin |Kapsam|
| --- | --- |
| Microsoft.Authorization/Operations/read | Abonelik|
| Microsoft.Authorization/permissions/read |Abonelik|
| Microsoft.Authorization/roleAssignments/read | Abonelik |
| Microsoft.Authorization/roleAssignments/write | Abonelik |
| Microsoft.Authorization/roleAssignments/delete | Abonelik |
| Microsoft.Automation/automationAccounts/connections/read | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/certificates/read | Kaynak Grubu |
| Microsoft.Automation/automationAccounts/write | Kaynak Grubu |
| Microsoft.OperationalInsights/çalışma alanları/yazma | Kaynak Grubu |

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Mesai saatleri dışında Başlat/Durdur VM'lerini Otomasyon hesabınıza eklemek için aşağıdaki adımları gerçekleştirin ve ardından değişkenleri çözümü özelleştirmek için yapılandırın.

1. Bir Otomasyon Hesabı'ndan, **İlgili Kaynaklar**altında **Başlat/Durdur VM'yi** seçin. Buradan daha fazla **bilgi edinin ve çözümü etkinleştirebilirsiniz.** Zaten dağıtılan bir Başlat/Durdur VM çözümünüz varsa, **çözümü Yönet'e** tıklayarak ve listede bularak bu çözümü seçebilirsiniz.

   ![Otomasyon hesabından etkinleştirme](./media/automation-solution-vm-management/enable-from-automation-account.png)

   > [!NOTE]
   > Ayrıca, **kaynak oluştur'a**tıklayarak Azure portalının herhangi bir yerinden de oluşturabilirsiniz. Market sayfasında, **Başlat** veya **Başlat/Durdur**gibi bir anahtar kelime yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Alternatif olarak, çözümün tam adından bir veya daha fazla anahtar kelime yazabilir ve ardından Enter tuşuna basabilirsiniz. Arama sonuçlarından **mesai saatleri dışında Başlat/Durdur VM'leri** seçin.

2. Seçilen çözüm için mesai dışı sayfada **Başlat/Durdur VM'lerinde** özet bilgilerini gözden geçirin ve ardından **Oluştur'u**tıklatın.

   ![Azure portalında](media/automation-solution-vm-management/azure-portal-01.png)

3. **Çözüm Ekle** sayfası görüntülenir. Otomasyon aboneliğinize aktaramadan önce çözümü yapılandırmanız istenir.

   ![VM Yönetimi Çözüm Ekle sayfası](media/automation-solution-vm-management/azure-portal-add-solution-01.png)

4. Çözüm **Ekle** sayfasında **Çalışma Alanı'nı**seçin. Otomasyon hesabının içinde olduğu aynı Azure aboneliğine bağlı bir Günlük Analizi çalışma alanı seçin. Çalışma alanınız **yoksa, Yeni Çalışma Alanı Oluştur'u**seçin. Günlük **Analitiği çalışma alanı** sayfasında aşağıdaki adımları gerçekleştirin:
   - "ContosoLAWorkspace" gibi yeni **Log Analytics çalışma alanı**için bir ad belirtin.
   - Varsayılan seçili uygun değilse, açılır listeden seçerek bağlantı kuracak bir **Abonelik** seçin.
   - **Kaynak Grubu**için yeni bir kaynak grubu oluşturabilir veya varolan bir tane seçebilirsiniz.
   - Bir **Konum** seçin.
   - Bir **Fiyatlandırma katmanı** seçin. GB **Başına (Bağımsız)** seçeneğini seçin. Azure Monitor günlükleri [fiyatlandırmayı](https://azure.microsoft.com/pricing/details/log-analytics/) güncelledi ve GB Başına katman tek seçenektir.

   > [!NOTE]
   > Çözümleri etkinleştirirken Log Analytics çalışma alanı ile Otomasyon Hesabı arasında bağlantı kurma seçeneği yalnızca belirli bölgelerde desteklenmektedir.
   >
   > Desteklenen eşleme çiftleri listesi [için, Otomasyon Hesabı ve Günlük Analizi çalışma alanı için Bölge eşleme](how-to/region-mappings.md)bölümüne bakın.

5. **Log Analytics çalışma alanı** sayfasında gerekli bilgileri sağladıktan sonra **Oluştur'u**tıklatın. Bildirimler **altında** ilerlemesini menüden izleyebilirsiniz ve bu da bittiğinde **Çözüm Ekle** sayfasına geri döner.
6. Çözüm **Ekle** sayfasında **Otomasyon hesabını**seçin. Yeni bir Log Analytics çalışma alanı oluşturuyorsanız, bu hesapla ilişkilendirilecek yeni bir Otomasyon hesabı oluşturabilir veya log analytics çalışma alanına zaten bağlı olmayan mevcut bir Otomasyon Hesabı seçebilirsiniz. Varolan bir Otomasyon Hesabı seçin veya **Bir Otomasyon Hesabı Oluştur'u**tıklatın ve Ek **Otomasyon hesabı** sayfasında aşağıdaki bilgileri sağlayın:
   - **Ad** alanına Otomasyon hesabının adını girin.

     Diğer tüm seçenekler, seçilen Log Analytics çalışma alanına göre otomatik olarak doldurulur. Bu seçenekler değiştirilemez. Bu çözüme dahil olan runbook'lar için varsayılan kimlik doğrulama yöntemi, bir Azure Farklı Çalıştır hesabıdır. **Tamam'ı**tıklattıktan sonra yapılandırma seçenekleri doğrulanır ve Otomasyon hesabı oluşturulur. Bu işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz.

7. Son olarak, **Çözüm Ekle** sayfasında **Yapılandırma'yı**seçin. **Parametreler** sayfası görüntülenir.

   ![Çözüm için parametreler sayfası](media/automation-solution-vm-management/azure-portal-add-solution-02.png)

   Burada, sizden istenir:
   - Hedef **Kaynak Grubu Adlarını**belirtin. Bu değerler, bu çözüm tarafından yönetilecek VM'ler içeren kaynak grubu adlarıdır. Virgül kullanarak birden fazla ad girebilir ve her birini ayırabilirsiniz (değerler büyük/küçük harf duyarlı değildir). Abonelikte yer alan tüm kaynak gruplarındaki sanal makineleri hedeflemek istiyorsanız, joker karakter kullanılması desteklenir. Bu değer **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames** değişkenlerde depolanır.
   - **VM Dışlama Listesini (dize) belirtin.** Bu değer, hedef kaynak grubundan bir veya daha fazla sanal makinenin adıdır. Virgül kullanarak birden fazla ad girebilir ve her birini ayırabilirsiniz (değerler büyük/küçük harf duyarlı değildir). Joker karakter kullanımı desteklenir. Bu değer **External_ExcludeVMNames** değişkeninde depolanır.
   - **Zamanlama**seçin. Zamanlamanız için bir tarih ve saat seçin. Seçtiğiniz saatten başlayarak yinelenen bir günlük zamanlama oluşturulur. Farklı bir bölge seçmek kullanılamaz. Çözümü yapılandırdıktan sonra zamanlamayı belirli saat diliminize yapılandırmak için [bkz.](#modify-the-startup-and-shutdown-schedules)
   - Bir eylem grubundan **E-posta bildirimleri** almak için **Evet** varsayılan değerini kabul edin ve geçerli bir e-posta adresi sağlayın. **Hayır'ı** seçerseniz ancak daha sonraki bir tarihte e-posta bildirimleri almak istediğinize karar verirseniz, virgülle ayrılmış geçerli e-posta adresleriyle oluşturulan [eylem grubunu](../azure-monitor/platform/action-groups.md) güncelleştirebilirsiniz. Ayrıca aşağıdaki uyarı kurallarını etkinleştirmeniz gerekir:

     - AutoStop_VM_Child
     - Scheduled_StartStop_Parent
     - Sequenced_StartStop_Parent

     > [!IMPORTANT]
     > Hedef Kaynak **Grubu Adları** için **&ast;** varsayılan değer bir . Bu, abonelikteki tüm VM'leri hedefler. Çözümün aboneliğinizdeki tüm VM'leri hedeflemesini istemiyorsanız, bu değerin zamanlamaları etkinleştirmeden önce kaynak grubu adları listesine güncelleştirilmesi gerekir.

8. Çözüm için gereken ilk ayarları yapılandırıldıktan sonra **Parametreler** sayfasını kapatmak için **Tamam'ı** tıklatın ve **Oluştur'u**seçin. Tüm ayarlar doğrulandıktan sonra, çözüm aboneliğinize dağıtılır. Bu işlemin tamamlanması birkaç saniye sürebilir ve **bildirimler** altında ilerlemesini menüden izleyebilirsiniz.

> [!NOTE]
> Bir Azure Bulut Çözüm Sağlayıcısı (Azure CSP) aboneliğiniz varsa, dağıtım tamamlandıktan sonra Otomasyon Hesabınızda **Paylaşılan Kaynaklar** altındaki **Değişkenler'e** gidin ve [**External_EnableClassicVMs**](#variables) değişkenini **False**olarak ayarlayın. Bu, çözümün Klasik VM kaynaklarını aramasını durdurur.

## <a name="scenarios"></a>Senaryolar

Çözüm üç farklı senaryo içerir. Bu senaryolar şunlardır:

### <a name="scenario-1-startstop-vms-on-a-schedule"></a>Senaryo 1: Programda VM'leri Başlat/Durdur

Bu senaryo, çözümü ilk dağıttığınızda varsayılan yapılandırmadır. Örneğin, akşam işten ayrıldığınızda abonelikteki tüm VM'leri durduracak ve sabah ofise döndüğünüzde başlatacak şekilde yapılandırabilirsiniz. Dağıtım sırasında **Zamanlanmış-BaşlangıçVM** ve **Zamanlanmış StopVM** zamanlamalarını yapılandırdığınızda, hedeflenen VM'leri başlatır ve durdurur. Bu çözümü yalnızca VM'leri durduracak şekilde yapılandırışla, [bkz.](#modify-the-startup-and-shutdown-schedules)

> [!NOTE]
> Zamanlama saat parametresini yapılandırdığınızda saat dilimi, geçerli saat diliminizdir. Ancak, Azure Otomasyonu'nda UTC formatında depolanır. Dağıtım sırasında bu işlenir gibi herhangi bir saat dilimi dönüştürme yapmak zorunda değildir.

Aşağıdaki değişkenleri External_Start_ResourceGroupNames **,** **External_Stop_ResourceGroupNames**ve **External_ExcludeVMNames.**

Bir abonelik ve kaynak grubuna karşı eylemi hedeflemeyi veya belirli bir VM listesini hedeflemeyi etkinleştirebilirsiniz, ancak her ikisini birden hedeflemeyi etkinleştirebilirsiniz.

#### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Abonelik ve kaynak grubuna yönelik eylemleri başlatma ve durdurma yı hedefleme

1. Hedef VM'leri belirtmek için **External_Stop_ResourceGroupNames** ve **External_ExcludeVMNames** değişkenleri yapılandırın.
2. **Zamanlanmış-StartVM** ve **Zamanlanmış StopVM** zamanlamalarını etkinleştirin ve güncelleştirin.
3. **Başlamak** üzere EYLEM parametresi ayarlanmış **ScheduledStartStop_Parent** runbook'u ve değişikliklerinizi önizlemek için **TRUE** olarak ayarlanmış WHATIF parametresini çalıştırın.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>VM listesine göre eylemi başlat ve durdurun

1. **Başlatmak**için ACTION parametresi ayarlanmış **ScheduledStartStop_Parent** runbook çalıştırın, *VMList* parametresinde vms virgülden ayrılmış bir listesini ekleyin ve sonra **WHATIF parametresini True**olarak ayarlayın. Değişikliklerinizi önizleyin.
1. **External_ExcludeVMNames** parametresini virgülden ayrılmış vm listesiyle (VM1, VM2, VM3) yapılandırın.
1. Bu **senaryo, External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupnames** değişkenleri onurlandırmaz. Bu senaryo için kendi Otomasyon zamanlamanızı oluşturmanız gerekir. Ayrıntılar için Azure [Otomasyonu'nda bir runbook Zamanlama'ya](../automation/automation-schedules.md)bakın.

> [!NOTE]
> **Hedef Kaynak Grubu Adları** için değer hem **External_Start_ResourceGroupNames** hem de **External_Stop_ResourceGroupNames**için değer olarak depolanır. Daha fazla ayrıntı için, bu değişkenlerin her birini farklı kaynak gruplarını hedefleyecek şekilde değiştirebilirsiniz. Start action için **External_Start_ResourceGroupNames**ve eylemi durdurmak için **External_Stop_ResourceGroupNames**kullanın. VM'ler otomatik olarak başlangıç ve durdurma zamanlamalarına eklenir.

### <a name="scenario-2-startstop-vms-in-sequence-by-using-tags"></a>Senaryo 2: Etiketleri kullanarak VMS'yi sırayla başlat/durdur

Dağıtılmış bir iş yükünü destekleyen birden çok VM'de iki veya daha fazla bileşen içeren bir ortamda, bileşenlerin sırayla başlatıldıve durdurulduğu sırayı desteklemek önemlidir. Aşağıdaki adımları gerçekleştirerek bu senaryoyu gerçekleştirebilirsiniz:

#### <a name="target-the-start-and-stop-actions-against-a-subscription-and-resource-group"></a>Abonelik ve kaynak grubuna yönelik eylemleri başlatma ve durdurma yı hedefleme

1. **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames** değişkenlerde hedeflenen VM'lere pozitif tamsayı değerine sahip bir **sequencestart** ve **sequencestop** etiketi ekleyin. Başlangıç ve durdurma eylemleri artan sırada gerçekleştirilir. VM'yi nasıl etiketleyeceklerini öğrenmek için [Azure'da Windows Sanal Makineyi Etiketle](../virtual-machines/windows/tag.md) ve [Azure'da Linux Sanal Makineyi Etiketle](../virtual-machines/linux/tag.md)'ye bakın.
1. **Sıralı-BaşlangıçVM** ve **Sıralı-StopVM** zamanlamalarını gereksinimlerinizi karşılayan tarih ve saate göre değiştirin ve zamanlamayı etkinleştirin.
1. SequencedStartStop_Parent **SequencedStartStop_Parent** runbook'u, **başlamak** üzere EYLEM parametresi ve değişikliklerinizi önizlemek için **True** olarak ayarlanmış WHATIF parametresi ile çalıştırın.
1. Eylemi önizleyin ve üretim VM'lerine karşı uygulamadan önce gerekli değişiklikleri yapın. Hazır olduğunuzda, çalışma kitabını **False**olarak ayarlanmış parametreyle el ile çalıştırın veya Otomasyon zamanlaması **Sıralı-StartVM** ve **Sıralı-StopVM'un** öngörülen zamanlamanızı izleyerek otomatik olarak çalışmasına izin verin.

#### <a name="target-the-start-and-stop-action-by-vm-list"></a>VM listesine göre eylemi başlat ve durdurun

1. **VMList** parametresine eklemeyi planladığınız VM'lere pozitif tamsayı değerine sahip bir **sequencestart** ve **sequencestop** etiketi ekleyin.
1. **Başlamak**için ACTION parametresi ayarlanmış **SequencedStartStop_Parent** runbook çalıştırın, *VMList* parametresinde vm'lerin virgülden ayrılmış bir listesini ekleyin ve ardından WHATIF parametresini **True**olarak ayarlayın. Değişikliklerinizi önizleyin.
1. **External_ExcludeVMNames** parametresini virgülden ayrılmış vm listesiyle (VM1, VM2, VM3) yapılandırın.
1. Bu **senaryo, External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupnames** değişkenleri onurlandırmaz. Bu senaryo için kendi Otomasyon zamanlamanızı oluşturmanız gerekir. Ayrıntılar için Azure [Otomasyonu'nda bir runbook Zamanlama'ya](../automation/automation-schedules.md)bakın.
1. Eylemi önizleyin ve üretim VM'lerine karşı uygulamadan önce gerekli değişiklikleri yapın. Hazır olduğunuzda, izleme ve tanılama/izleme-eylem-grupları, **False**olarak ayarlanmış parametre ile el ile çalıştırın veya Otomasyon zamanlaması **Sıralı-StartVM** ve **Sequenced-StopVM'in** öngörülen zamanlamanızı izleyerek otomatik olarak çalışmasına izin verin.

## <a name="solution-components"></a>Çözüm bileşenleri

Bu çözüm, sanal makinelerinizin başlangıç ve kapatma işlemlerini işletme gereksinimlerinize göre uyarlayabilmeniz için önceden yapılandırılmış runbook'lar, zamanlamalar ve Azure Monitor günlükleriyle tümleştirmeyi içerir.

### <a name="runbooks"></a>Runbook'lar

Aşağıdaki tabloda, bu çözüme göre Otomasyon hesabınıza dağıtılan runbook'lar listelenir. Runbook kodunda değişiklik yapmayın. Bunun yerine, yeni işlevler için kendi runbook yazın.

> [!IMPORTANT]
> Doğrudan adına eklenen "alt" ile herhangi bir runbook çalıştırmayın.

Tüm üst runbook'lar _WhatIf_ parametresini içerir. **True**olarak ayarlandığında _WhatIf,_ _WhatIf_ parametresi olmadan çalıştırıldığında runbook'un tam olarak aldığı davranışı niçin ayrıntılı olarak destekler ve doğru VM'lerin hedeflendiğini doğrular. Runbook yalnızca _WhatIf_ parametresi **False**olarak ayarlandığında tanımlanan eylemlerini gerçekleştirir.

|Runbook | Parametreler | Açıklama|
| --- | --- | ---|
|AutoStop_CreateAlert_Child | VMObject <br> AlertAction <br> WebHookURI | Üst çalışma kitabından çağrılır. Bu runbook, Otomatik Durdurma senaryosu için kaynak başına uyarılar oluşturur.|
|AutoStop_CreateAlert_Parent | VMList<br> WhatIf: Doğru veya Yanlış  | Hedeflenen abonelik veya kaynak gruplarında VM'lerde Azure uyarı kuralları oluşturur veya güncelleştirir. <br> VMList: Virgülden ayrılmış VM listesi. Örneğin, _vm1, vm2, vm3_.<br> *WhatIf* yürütmeden runbook mantığını doğrular.|
|AutoStop_Disable | yok | Otomatik Stop uyarılarını ve varsayılan zamanlamayı devre dışı bırakır.|
|AutoStop_StopVM_Child | WebHookData | Üst çalışma kitabından çağrılır. Uyarı kuralları VM'yi durdurmak için bu runbook'u çağırır.|
|Bootstrap_Main | yok | Genellikle Azure Kaynak Yöneticisi'nden erişilemeyen webhookURI gibi önyükleme yapılandırmaları ayarlamak için bir kez kullanılır. Bu runbook, başarılı dağıtım sırasında otomatik olarak kaldırılır.|
|ScheduledStartStop_Child | VMName <br> Eylem: Başlat veya Durdur <br> ResourceGroupName | Üst çalışma kitabından çağrılır. Zamanlanan durak için bir başlatma veya durdurma eylemini yürütür.|
|ScheduledStartStop_Parent | Eylem: Başlat veya Durdur <br>VMList <br> WhatIf: Doğru veya Yanlış | Bu ayar, abonelikteki tüm VM'leri etkiler. Yalnızca bu hedeflenen kaynak gruplarında yürütmek için **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames'yi** edin. **External_ExcludeVMNames** değişkenini güncelleyerek belirli VM'leri de dışlayabilirsiniz.<br> VMList: Virgülden ayrılmış VM listesi. Örneğin, _vm1, vm2, vm3_.<br> _WhatIf_ yürütmeden runbook mantığını doğrular.|
|SequencedStartStop_Parent | Eylem: Başlat veya Durdur <br> WhatIf: Doğru veya Yanlış<br>VMList| Başlat/durdurma etkinliğini sıralamak istediğiniz her VM'de **sequencestart** ve **sequencestop** adlı etiketler oluşturun. Bu etiket adları büyük/küçük harf duyarlıdır. Etiketin değeri, başlamak veya durdurmak istediğiniz sıraya karşılık gelen pozitif bir tamsayı (1, 2, 3) olmalıdır. <br> VMList: Virgülden ayrılmış VM listesi. Örneğin, _vm1, vm2, vm3_. <br> _WhatIf_ yürütmeden runbook mantığını doğrular. <br> **Not**: VM'ler Azure Otomasyon değişkenlerinde External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames ve External_ExcludeVMNames olarak tanımlanan kaynak gruplarında olmalıdır. Eylemlerin etkili olması için uygun etiketlere sahip olmaları gerekir.|

### <a name="variables"></a>Değişkenler

Aşağıdaki tabloda Otomasyon hesabınızda oluşturulan değişkenler listeleneb.r.a. Yalnızca **Harici**ile önceden belirlenmiş değişkenleri değiştirin. **Internal** ile önceden belirlenmiş değişkenleri değiştirmek istenmeyen etkilere neden olur.

|Değişken | Açıklama|
|---------|------------|
|External_AutoStop_Condition | Bir uyarıtetiklemeden önce koşulu yapılandırmak için gereken koşullu işleç. Kabul edilebilir değerler **GreaterThan**, **GreaterThanOrEqual**, **LessThan**, ve **LessThanOrEqual**.|
|External_AutoStop_Description | CPU yüzdesi eşiği aşarsa VM durdurmak için uyarı.|
|External_AutoStop_MetricName | Azure Uyarı kuralının yapılandırılması gereken performans ölçümünün adı.|
|External_AutoStop_Threshold | External_AutoStop_MetricName değişkeninde belirtilen Azure Uyarısı _External_AutoStop_MetricName_kuralıiçin eşik. Yüzde değerleri 1 ile 100 arasında değişebilir.|
|External_AutoStop_TimeAggregationOperator | Durumu değerlendirmek için seçili pencere boyutuna uygulanan zaman toplama işleci. Kabul edilebilir değerler **Ortalama,** **Minimum,** **Maksimum,** **Toplam**ve **Son**' dır.|
|External_AutoStop_TimeWindow | Azure'un bir uyarıyı tetiklemek için seçili ölçümleri analiz ettiği pencere boyutu. Bu parametre, girişin zaman alanı biçiminde kabul edilir. Olası değerler 5 dakika ile 6 saat arasındadır.|
|External_EnableClassicVMs| Klasik VM'lerin çözüm tarafından hedeflenip hedeflenmediğini belirtir. Varsayılan değer True'dur. Bu, CSP abonelikleri için False olarak ayarlanmalıdır. Klasik VM'ler [klasik Run-As Hesabı](automation-create-standalone-account.md#classic-run-as-accounts)gerektirir.|
|External_ExcludeVMNames | Boşluk olmayan bir virgül kullanarak adları ayırarak dışlanacak VM adlarını girin. Bu 140 VM ile sınırlıdır. Bu virgülle ayrılmış listeye 140'tan fazla VM eklerseniz, hariç tutulacak şekilde ayarlanan VM'ler yanlışlıkla başlatılabilir veya durdurulabilir.|
|External_Start_ResourceGroupNames | Başlangıç eylemleri için hedeflenen virgül kullanarak değerleri ayıran bir veya daha fazla kaynak grubu belirtir.|
|External_Stop_ResourceGroupNames | Durdurma eylemleri için hedeflenen virgül kullanarak değerleri ayıran bir veya daha fazla kaynak grubu belirtir.|
|Internal_AutomationAccountName | Otomasyon hesabının adını belirtir.|
|Internal_AutoSnooze_WebhookUri | AutoStop senaryosu için çağrılan Webhook URI'yi belirtir.|
|Internal_AzureSubscriptionId | Azure Abonelik Kimliğini belirtir.|
|Internal_ResourceGroupName | Otomasyon hesabı kaynak grup adını belirtir.|

Tüm senaryolarda, **AutoStop_CreateAlert_Parent,** **SequencedStartStop_Parent**ve **AutoStop_CreateAlert_Parent** **ScheduledStartStop_Parent** runbook'lar için virgülle ayrılmış vm listesi sağlama dışında, vm'leri hedeflemek için External_Start_ResourceGroupNames, **External_Stop_ResourceGroupNames**ve **External_ExcludeVMNames** değişkenleri gereklidir. Diğer bir deyişle, eylemin gerçekleşmesi için VM'lerinizin hedef kaynak gruplarında yer alması gerekir. Bu mantık, aboneliği veya kaynak grubunu hedefleyebilmeniz ve yeni oluşturulan VM'ler tarafından devralınan eylemleri oluşturabileceğiniz için Azure ilkesine benzer şekilde çalışır. Bu yaklaşım, her VM için ayrı bir zamanlama korumak zorunda önler ve ölçek başlar ve durur yönetir.

### <a name="schedules"></a>Zamanlamalar

Aşağıdaki tablo, Otomasyon hesabınızda oluşturulan varsayılan zamanlamaların her birini listeler.Bunları değiştirebilir veya kendi özel zamanlamalarınızı oluşturabilirsiniz.Varsayılan olarak, **Scheduled_StartVM** ve **Scheduled_StopVM**dışında tüm zamanlamaları devre dışı bırakılır.

Bu çakışan zamanlama eylemleri oluşturabileceğinden, tüm zamanlamaları etkinleştirmemelisiniz. En iyisi, hangi optimizasyonları gerçekleştirmek ve buna göre değiştirmek istediğinizi belirlemek. Daha fazla açıklama için genel bakış bölümündeki örnek senaryolara bakın.

|Zamanlama adı | Frequency | Açıklama|
|--- | --- | ---|
|Schedule_AutoStop_CreateAlert_Parent | 8 saatte bir | AutoStop_CreateAlert_Parent çalışma kitabını her 8 saatte bir çalıştıran bu kitap, azure otomasyon değişkenlerinde External_Start_ResourceGroupNames, External_Stop_ResourceGroupNames ve External_ExcludeVMNames VM tabanlı değerleri durdurur. Alternatif olarak, VMList parametresini kullanarak virgülle ayrılmış vm listesini belirtebilirsiniz.|
|Scheduled_StopVM | Kullanıcı tanımlı, günlük | Scheduled_Parent runbook'u belirtilen saatte her gün _Stop_ parametresi ile çalıştırın.Varlık değişkenleri tarafından tanımlanan kurallara uyarlanan tüm VM'leri otomatik olarak durdurur.İlgili zamanlama, **Zamanlanmış-StartVM**etkinleştirin.|
|Scheduled_StartVM | Kullanıcı tanımlı, günlük | Scheduled_Parent runbook'u her gün belirtilen saatte _Başlat_ parametresi ile çalıştırın. Uygun değişkenler tarafından tanımlanan kurallara uygun tüm VM'leri otomatik olarak başlatır.İlgili zamanlama, **Zamanlanmış-StopVM**etkinleştirin.|
|Sıralı-StopVM | 01:00 (UTC), her Cuma | Sequenced_Parent runbook'u her Cuma belirtilen saatte _Durdur_ parametresi ile çalıştırır.Sırayla (artan) uygun değişkenler tarafından tanımlanan **SequenceStop** etiketi yle tüm VM'leri durdurur. Etiket değerleri ve varlık değişkenleri hakkında daha fazla bilgi için RunBook'lar bölümüne bakın.İlgili zamanlama, **Sequenced-StartVM**etkinleştirin.|
|Sıralı BaşlangıçVM | 13:00 (UTC), her Pazartesi | Her Pazartesi günü belirtilen zamanda _Başlat_ parametresi ile Sequenced_Parent runbook çalıştırın. Sırayla (azalan) tüm VM'leri uygun değişkenler tarafından tanımlanan **SequenceStart** etiketiyle başlatır. Etiket değerleri ve varlık değişkenleri hakkında daha fazla bilgi için RunBook'lar bölümüne bakın. İlgili zamanlama, **Sequenced-StopVM**etkinleştirin.|

## <a name="azure-monitor-logs-records"></a>Azure Monitör kayıtları günlükleri

Otomasyon, Log Analytics çalışma alanında iki tür kayıt oluşturur: iş günlükleri ve iş akışları.

### <a name="job-logs"></a>İş günlükleri

|Özellik | Açıklama|
|----------|----------|
|Çağıran |  İşlemi başlatandır. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir.|
|Kategori | Veri türü sınıflandırması. Otomasyon için değer JobLogs olacaktır.|
|CorrelationId | GUID, runbook işinin Korelasyon Kimliği'dir.|
|JobId | GUID bu runbook iş kimliğidir.|
|operationName | Azure’da gerçekleştirilen işlem türünü belirtir. Otomasyon için değer İş'tir.|
|resourceId | Azure’daki kaynak türünü belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
|ResourceGroup | Runbook işine ait kaynak grubunun adını belirtir.|
|ResourceProvider | Dağıtıp yönetebileceğiniz kaynakları sağlayan Azure hizmetini belirtir. Otomasyon için değer, Azure Otomasyonu olacaktır.|
|ResourceType | Azure’daki kaynak türünü belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
|resultType | Runbook işinin durumudur. Olası değerler şunlardır:<br>- Başlatıldı<br>- Durduruldu<br>- Askıya alındı<br>- Başarısız oldu<br>- Başarılı oldu|
|resultDescription | Runbook iş sonucu durumunu açıklar. Olası değerler şunlardır:<br>- İş başlatıldı<br>- İş Başarısız Oldu<br>- İş Tamamlandı|
|RunbookName | Runbook’un adını belirtir.|
|SourceSystem | Gönderilen verilere ilişkin kaynak sistemi belirtir. Otomasyon için değer OpsManager'dur|
|StreamType | Olay türünü belirtir. Olası değerler şunlardır:<br>- Ayrıntılı<br>- Çıktı<br>- Hata<br>- Uyarı|
|SubscriptionId | İşin abonelik kimliğini belirtir.
|Zaman | Runbook işinin yürütüldüğü tarih ve saat.|

### <a name="job-streams"></a>İş akışları

|Özellik | Açıklama|
|----------|----------|
|Çağıran |  İşlemi başlatandır. Olası değerler, bir e-posta adresi veya zamanlanan işlere yönelik bir sistemdir.|
|Kategori | Veri türü sınıflandırması. Otomasyon için değer JobStreams olacaktır.|
|JobId | GUID bu runbook iş kimliğidir.|
|operationName | Azure’da gerçekleştirilen işlem türünü belirtir. Otomasyon için değer İş'tir.|
|ResourceGroup | Runbook işine ait kaynak grubunun adını belirtir.|
|resourceId | Azure'daki kaynak kimliğini belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
|ResourceProvider | Dağıtıp yönetebileceğiniz kaynakları sağlayan Azure hizmetini belirtir. Otomasyon için değer, Azure Otomasyonu olacaktır.|
|ResourceType | Azure’daki kaynak türünü belirtir. Otomasyon için değer, runbook ile ilişkilendirilmiş Otomasyon hesabı olacaktır.|
|resultType | Runbook işinin, olayın oluşturulduğu andaki sonucu. Olası bir değer:<br>- Devam Ediyor|
|resultDescription | Runbook’un çıktı akışını içerir.|
|RunbookName | Runbook’un adı.|
|SourceSystem | Gönderilen verilere ilişkin kaynak sistemi belirtir. Otomasyon için değer OpsManager'dur.|
|StreamType | İş akışı türü. Olası değerler şunlardır:<br>- İlerleme<br>- Çıktı<br>- Uyarı<br>- Hata<br>- Hata ayıklama<br>- Ayrıntılı|
|Zaman | Runbook işinin yürütüldüğü tarih ve saat.|

**JobLogs** veya **JobStreams**kategori kayıtlarını döndüren herhangi bir günlük araması yaptığınızda, arama tarafından döndürülen güncelleştirmeleri özetleyen bir kutucuk kümesini görüntüleyen **JobLogs** veya **JobStreams** görünümünü seçebilirsiniz.

## <a name="sample-log-searches"></a>Örnek günlük aramaları

Aşağıdaki tabloda, bu çözüm tarafından toplanan iş kayıtlarına ilişkin örnek günlük aramaları sunulmaktadır.

|Sorgu | Açıklama|
|----------|----------|
|Başarılı bir şekilde tamamlanan runbook ScheduledStartStop_Parent için iş bulma | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "ScheduledStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" )  <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|
|Başarılı bir şekilde tamamlanan runbook SequencedStartStop_Parent için iş bulma | <code>search Category == "JobLogs" <br>&#124;  where ( RunbookName_s == "SequencedStartStop_Parent" ) <br>&#124;  where ( ResultType == "Completed" ) <br>&#124;  summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h) <br>&#124;  sort by TimeGenerated desc</code>|

## <a name="viewing-the-solution"></a>Çözümü görüntüleme

Çözüme erişmek için Otomasyon Hesabınıza gidin ve **İlgili KAYNAKLAR**altında Çalışma **Alanı'nı** seçin. Günlük analizi sayfasında **GENEL**altında **Çözümler'i** seçin. **Çözümler** sayfasında, listeden **Başlat-Durdur-VM[çalışma alanı]** çözümünü seçin.

Çözümü seçmek, **Start-Stop-VM[çalışma alanı]** çözüm sayfasını görüntüler. Burada **StartStopVM** döşemesi gibi önemli ayrıntıları gözden geçirebilirsiniz. Log Analytics çalışma alanınızda olduğu gibi, bu döşeme, başlatılan ve başarıyla tamamlanan çözüm için runbook işlerinin bir sayısını ve grafik gösterimini görüntüler.

![Otomasyon Güncelleme Yönetimi çözüm sayfası](media/automation-solution-vm-management/azure-portal-vmupdate-solution-01.png)

Buradan, donut karosu tıklayarak iş kayıtlarının daha fazla analiz gerçekleştirebilirsiniz. Çözüm panosu, iş geçmişini ve önceden tanımlanmış günlük arama sorgularını gösterir. Arama sorgularınıza göre arama yapmak için günlük analitiği gelişmiş portalına geçin.

## <a name="configure-email-notifications"></a>E-posta bildirimlerini yapılandırma

Çözüm dağıtıldıktan sonra e-posta bildirimlerini değiştirmek için dağıtım sırasında oluşturulan eylem grubunu değiştirin.  

> [!NOTE]
> Azure Kamu Bulutu'ndaki abonelikler bu çözümün e-posta işlevselliğini desteklemez.

Azure portalında, İzleme -> Eylem gruplarına gidin. **StartStop_VM_Notication**başlıklı eylem grubunu seçin.

![Otomasyon Güncelleme Yönetimi çözüm sayfası](media/automation-solution-vm-management/azure-monitor.png)

**StartStop_VM_Notification** sayfasında Ayrıntılar **altında**ayrıntıları **edit'i** tıklatın. Bu, **E-posta/SMS/Push/Voice** sayfasını açar. E-posta adresini güncelleştirin ve değişikliklerinizi kaydetmek için **Tamam'ı** tıklatın.

![Otomasyon Güncelleme Yönetimi çözüm sayfası](media/automation-solution-vm-management/change-email.png)

Alternatif olarak, eylem grupları hakkında daha fazla bilgi edinmek, [eylem gruplarına](../azure-monitor/platform/action-groups.md) bakın

Çözüm sanal makineleri kapattığında gönderilen örnek bir e-posta aşağıda verilmiştir.

![Otomasyon Güncelleme Yönetimi çözüm sayfası](media/automation-solution-vm-management/email.png)

## <a name="addexclude-vms"></a><a name="add-exclude-vms"></a>VM Ekleme/Hariç Tutma

Çözüm, çözüm tarafından hedeflenecek VM'ler ekleme veya özellikle makineleri çözümden dışlama olanağı sağlar.

### <a name="add-a-vm"></a>VM ekleme

Bir VM'nin çalıştırıldığında Başlat/Durdur çözümüne dahil edildiğinden emin olmak için kullanabileceğiniz birkaç seçenek vardır.

* Çözümün üst [runbook'larının](#runbooks) her birinin bir **VMList** parametresi vardır. Durumunuz için uygun üst basamak runbook'u zamanlarken bu parametreye virgülle ayrılmış vm adlarının listesini geçirebilirsiniz ve çözüm çalıştığında bu VM'ler eklenecektir.

* Birden çok VM seçmek için, başlatmak veya durdurmak istediğiniz VM'leri içeren kaynak grubu adlarıyla **External_Start_ResourceGroupNames** ve **External_Stop_ResourceGroupNames** ayarlayın. Çözümün abonelikteki tüm `*`kaynak gruplarına karşı çalıştırılması için bu değeri de ayarlayabilirsiniz.

### <a name="exclude-a-vm"></a>VM hariç tutuma

Bir VM'yi çözümden çıkarmak **için, External_ExcludeVMNames** değişkenine ekleyebilirsiniz. Bu değişken, Başlat/Durdur çözümünden hariç tutmak için belirli VM'lerin virgülle ayrılmış bir listesidir. Bu liste 140 VM ile sınırlıdır. Bu virgülle ayrılmış listeye 140'tan fazla VM eklerseniz, hariç tutulacak şekilde ayarlanan VM'ler yanlışlıkla başlatılabilir veya durdurulabilir.

## <a name="modify-the-startup-and-shutdown-schedules"></a>Başlatma ve kapatma zamanlamalarını değiştirme

Bu çözümdeki başlangıç ve kapatma zamanlamalarını yönetmek, [Azure Otomasyonu'nda bir runbook Zamanlama'da](automation-schedules.md)belirtildiği gibi aynı adımları izler. VM'leri başlatmak ve durdurmak için ayrı bir zamanlama olması gerekir.

Çözümü, VM'leri belirli bir zamanda durduracak şekilde yapılandırılmak desteklenir. Bu senaryoda, bir **Stop** zamanlamasını ve karşılık gelen **Başlangıç** zamanlanmamış bir zamanlama oluşturursunuz. Bunu yapmanız için gerekenler:

1. **External_Stop_ResourceGroupNames** değişkeninde VM'lerin kapanması için kaynak gruplarını eklediğinizden emin olun.
2. VM'leri kapatmak istediğiniz süre için kendi zamanlamanızı oluşturun.
3. **ScheduledStartStop_Parent** runbook'a gidin ve **Zamanlama'yı**tıklatın. Bu, önceki adımda oluşturduğunuz zamanlamayı seçmenize olanak tanır.
4. Parametreler'i seçin **ve ayarları çalıştırın** ve EYLEM parametresini "Dur" olarak ayarlayın.
5. Değişikliklerinizi kaydetmek için **Tamam**’a tıklayın.

## <a name="update-the-solution"></a>Çözümü güncelleştirme

Bu çözümün önceki bir sürümünü dağıttıysanız, güncelleştirilmiş bir sürümü dağıtmadan önce bu çözümü hesabınızdan silmeniz gerekir. [Çözümü kaldırmak](#remove-the-solution) için adımları izleyin ve ardından [çözümü dağıtmak](#deploy-the-solution)için yukarıdaki adımları izleyin.

## <a name="remove-the-solution"></a>Çözümü kaldırma

Çözümü artık kullanmanıza gerek olmadığına karar verirseniz, çözümü Otomasyon hesabından silebilirsiniz. Çözümü siler, yalnızca runbook'ları kaldırır. Çözüm eklendiğinde oluşturulan zamanlamaları veya değişkenleri silmez. Diğer runbook'larla kullanmıyorsanız, el ile silmeniz gereken kıymetler.

Çözümü silmek için aşağıdaki adımları gerçekleştirin:

1. Otomasyon hesabınızdan, **İlgili kaynaklar** **altında, Bağlantılı çalışma alanını**seçin.
1. **Çalışma alanına git'i**seçin.
1. **Genel**altında, **Çözümler**seçin. 
1. **Çözümler** sayfasında, **başlat-durdur-VM[Çalışma Alanı]** çözümünü seçin. **VMManagementSolution[Workspace]** sayfasında, menüden **Sil'i**seçin.<br><br> ![VM Mgmt Çözümünü Sil](media/automation-solution-vm-management/vm-management-solution-delete.png)
1. Çözüm **Sil** penceresinde, çözümü silmek istediğinizi onaylayın.
1. Bilgiler doğrulanır ve çözüm silinir, menüden **Bildirimler** altında ilerlemesini izleyebilirsiniz. Çözüm densonra **Çözümler** sayfasına döndürülür.

Otomasyon hesabı ve Log Analytics çalışma alanı bu işlemin bir parçası olarak silinmez. Log Analytics çalışma alanını korumak istemiyorsanız, el ile silmeniz gerekir. Bu, Azure portalından gerçekleştirilebilir:

1. Azure portalında, Log **Analytics çalışma alanlarını**arayın ve seçin.
1. Günlük **Analizi çalışma alanları** sayfasında çalışma alanını seçin.
1. Çalışma alanı ayarları sayfasındaki menüden **Sil'i** seçin.

Azure Otomasyon hesap bileşenlerini korumak istemiyorsanız, her birini el ile silebilirsiniz. Çözüm tarafından oluşturulan runbook'lar, değişkenler ve zamanlamalar listesi için [Çözüm bileşenlerine](#solution-components)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Monitor günlükleriyle farklı arama sorguları oluşturma ve Otomasyon iş günlüklerini gözden geçirme hakkında daha fazla bilgi edinmek için [Azure Monitor günlüklerinde Günlük aramaları'na](../log-analytics/log-analytics-log-searches.md)bakın.
- Runbook yürütme, runbook işleri ve diğer teknik ayrıntıları izlemek hakkında daha fazla bilgi edinmek için [bkz.](automation-runbook-execution.md)
- Azure Monitor günlükleri ve veri toplama kaynakları hakkında daha fazla bilgi edinmek için [bkz.](../azure-monitor/platform/collect-azure-metrics-logs.md)
