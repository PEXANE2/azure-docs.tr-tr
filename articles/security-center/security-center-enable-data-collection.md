---
title: Azure Güvenlik Merkezinde Veri Toplama | Microsoft Dokümanlar
description: Bu makalede, bir Log Analytics Aracısı nasıl yüklenir ve toplanan verileri depolayacak bir Log Analytics çalışma alanı ayarlanır.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 09/10/2019
ms.author: memildin
ms.openlocfilehash: 5236fd89ed99ca14bb7fc24e40ef79e12e5177d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79245505"
---
# <a name="data-collection-in-azure-security-center"></a>Azure Güvenlik Merkezinde veri toplama
Güvenlik Merkezi, güvenlik açıklarını ve tehditlerini izlemek için Azure sanal makinelerinizden (VM'ler), sanal makine ölçek kümeleri, IaaS kapsayıcıları ve Azure olmayan (şirket içi dahil) bilgisayarlardan veri toplar. Veriler, makineden çeşitli güvenlikle ilgili yapılandırmaları ve olay günlüklerini okuyan ve verileri analiz için çalışma alanınıza kopyalayan Log Analytics Agent kullanılarak toplanır. Bu verilere örnek olarak işletim sistemi türü ve sürümü, işletim sistemi günlükleri (Windows olay günlükleri), çalışan işlemler, makine adı, IP adresleri ve kullanıcı oturum açma. Log Analytics Aracısı ayrıca kilitlenme döküm dosyalarını çalışma alanınıza kopyalar.

Veri toplama eksik güncelleştirmeler, yanlış yapılandırılmış işletim sistemi güvenlik ayarları, uç nokta koruma durumu ve sistem ve tehdit koruması görünürlük sağlamak için gereklidir. 

Bu makalede, bir Log Analytics Aracısı nasıl yüklenir ve toplanan verileri depolayacak bir Log Analytics çalışma alanı ayarlanır. Veri toplamayı etkinleştirmek için her iki işlem de gereklidir. 

> [!NOTE]
> - Veri toplama yalnızca Bilgi İşlem kaynakları (VM'ler, sanal makine ölçek kümeleri, IaaS kapsayıcıları ve Azure olmayan bilgisayarlar) için gereklidir. Aracı sağlamasanız bile Azure Güvenlik Merkezi'nden yararlanabilirsiniz; ancak, sınırlı güvenlik olacak ve yukarıda listelenen yetenekleri desteklenmez.  
> - Desteklenen platformlar listesi için [Azure Güvenlik Merkezi'ndeki Desteklenen platformlara](security-center-os-coverage.md)bakın.
> - Yeni veya varolan bir çalışma alanı kullanın, Günlük Analizi'nde veri depolamak, veri depolama için ek ücrete tabi olabilir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="enable-automatic-provisioning-of-the-log-analytics-agent"></a>Log Analytics Aracısı'nın otomatik olarak sağlanmasını etkinleştirin<a name="auto-provision-mma"></a>

Makinelerden veri toplamak için Log Analytics Agent yüklü olmalıdır. Aracının kurulumu otomatik olarak yapılabilir (önerilir) veya aracıyı el ile yükleyebilirsiniz.  

>[!NOTE]
> Otomatik sağlama varsayılan olarak kapalıdır. Güvenlik Merkezi'ni varsayılan olarak otomatik sağlama yı yüklemek üzere ayarlamak için, bunu **A'ya**ayarlayın.
>

Otomatik sağlama A)'da olduğunda, Güvenlik Merkezi Log Analytics Aracısını desteklenen tüm Azure VM'lerinde ve oluşturulan yeni leri karşılar. Otomatik sağlama şiddetle önerilir, ancak manuel aracı yükleme de mevcuttur. [Log Analytics Agent uzantısını nasıl yükleyin](#manual-agent)öğrenin.



Log Analytics Aracısı'nın otomatik olarak sağlanmasını etkinleştirmek için:
1. Güvenlik Merkezi ana menüsü **altında, Fiyatlandırma & ayarlarını**seçin.
2. İlgili abonelik için tıklayın

   ![Abonelik seçme][7]

3. **Veri Toplama'yı**seçin.
4. **Otomatik Sağlama**altında, otomatik sağlama yı etkinleştirmek için **On'u** seçin.
5. **Kaydet'i**seçin.

   ![Otomatik sağlamayı etkinleştirme][1]

>[!NOTE]
> - Önceden varolan bir yüklemenin nasıl sağlanacak ile ilgili talimatlar için, [önceden varolan bir aracı yüklemesi durumunda Otomatik sağlama](#preexisting)konusuna bakın.
> - Manuel sağlama yla ilgili talimatlar için [bkz.](#manual-agent)
> - Otomatik sağlamayı kapatma yönergeleri [için](#offprovisioning)bkz.
> - PowerShell'i kullanarak Güvenlik Merkezi'ne nasıl bininilir yönergeleri için [PowerShell'i kullanarak Azure Güvenlik Merkezi'ne binmeyi ana](security-center-powershell-onboarding.md)görün.
>

## <a name="workspace-configuration"></a>Çalışma alanı yapılandırması
Güvenlik Merkezi tarafından toplanan veriler, Log Analytics çalışma alanlarında depolanır. Güvenlik Merkezi tarafından oluşturulan çalışma alanlarında veya oluşturduğunuz mevcut bir çalışma alanında Azure Sanal Taşıtlarından toplanan verilerin depolanmasını seçebilirsiniz. 

Çalışma alanı yapılandırması abonelik başına ayarlanır ve birçok abonelik aynı çalışma alanını kullanabilir.

### <a name="using-a-workspace-created-by-security-center"></a>Güvenlik Merkezi tarafından oluşturulan çalışma alanını kullanma

Güvenlik merkezi, verileri depolamak için otomatik olarak varsayılan bir çalışma alanı oluşturabilir. 

Güvenlik Merkezi tarafından oluşturulan bir çalışma alanı seçmek için:

1. **Varsayılan çalışma alanı yapılandırması**altında, Güvenlik merkezi tarafından oluşturulan çalışma alanı(lar)ı kullanın'ı seçin.
   ![Fiyatlandırma katmanını seçin][10] 

1. **Kaydet**'e tıklayın.<br>
    Güvenlik Merkezi, bu coğrafi konumda yeni bir kaynak grubu ve varsayılan çalışma alanı oluşturur ve aracıyı bu çalışma alanına bağlar. Çalışma alanı ve kaynak grubu için adlandırma kuralı:<br>
   **Çalışma Alanı: DefaultWorkspace-[abonelik-ID]-[geo]<br> Kaynak Grubu: VarsayılanKaynak Grubu-[geo]**

   Bir abonelik birden çok coğrafi konumdan VM içeriyorsa, Güvenlik Merkezi birden çok çalışma alanı oluşturur. Veri gizliliği kurallarını korumak için birden çok çalışma alanı oluşturulur.
1. Güvenlik Merkezi, abonelik için belirlenen fiyatlandırma katmanı başına çalışma alanında bir Güvenlik Merkezi çözüme otomatik olarak olanak sağlar. 

> [!NOTE]
> Güvenlik Merkezi tarafından oluşturulan çalışma alanlarının Log Analytics fiyatlandırma katmanı Güvenlik Merkezi faturalandırmasını etkilemez. Güvenlik Merkezi faturaları her zaman bir çalışma alanına yüklenmiş olan Güvenlik Merkezi ilkelerine ve çözümlerine göre belirlenir. Güvenlik Merkezi, Ücretsiz katmanında varsayılan çalışma alanında *SecurityCenterFree* çözümünü etkinleştirir. Güvenlik Merkezi, Standart katmanda varsayılan çalışma alanında *Security* çözümünü etkinleştirir.
> Günlük Analizi'nde veri depolamak, veri depolama için ek ücrete tabi olabilir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

Varolan günlük analizi hesapları hakkında daha fazla bilgi için, [Varolan günlük analizi müşterileri](./faq-azure-monitor-logs.md)bölümüne bakın.

### <a name="using-an-existing-workspace"></a>Varolan bir çalışma alanını kullanma

Zaten varolan bir Log Analytics çalışma alanınız varsa, aynı çalışma alanını kullanmak isteyebilirsiniz.

Varolan Log Analytics çalışma alanınızı kullanmak için çalışma alanında izinleri okumuş ve yazmış olmalısınız.

> [!NOTE]
> Varolan çalışma alanında etkinleştirilen çözümler, ona bağlı Azure VM'lerine uygulanır. Ücretli çözümler için bu ek ücrete neden olabilir. Veri gizliliği yle ilgili hususlar için, seçtiğiniz çalışma alanının doğru coğrafi bölgede olduğundan emin olun.
> Günlük analitiğinde veri depolamak, veri depolama için ek ücrete tabi olabilir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

Varolan bir Log Analytics çalışma alanını seçmek için:

1. **Varsayılan çalışma alanı yapılandırması**altında, başka bir çalışma alanı **kullan'ı**seçin.

   ![Varolan çalışma alanını seçin][2]

2. Açılan menüden, toplanan verileri depolamak için bir çalışma alanı seçin.

   > [!NOTE]
   > Aşağı çekme menüsünde, tüm aboneliklerinizdeki tüm çalışma alanları kullanılabilir. Daha fazla bilgi için [abonelikler arası çalışma alanı seçimine](security-center-enable-data-collection.md#cross-subscription-workspace-selection) bakın. Çalışma alanına erişmek için izniniz olmalıdır.
   >
   >

3. **Kaydet'i**seçin.
4. **Kaydet'i**seçtikten sonra, daha önce varsayılan çalışma alanına bağlı olan izlenen VM'leri yeniden yapılandırmak isteyip istineceğiniz sorulur.

   - Yeni çalışma alanı ayarlarının yalnızca yeni VM'lere uygulanmasını istiyorsanız **Hayır'ı** seçin. Yeni çalışma alanı ayarları yalnızca yeni aracı yüklemeleri için geçerlidir; Log Analytics Agent yüklü olmayan yeni keşfedilen VM'ler.
   - Yeni çalışma alanı ayarlarının tüm VM'lere uygulanmasını istiyorsanız **Evet'i** seçin. Ayrıca, güvenlik merkezi oluşturulan çalışma alanına bağlı her VM, yeni hedef çalışma alanına yeniden bağlanır.

   > [!NOTE]
   > Evet'i seçerseniz, tüm VM'ler yeni hedef çalışma alanına yeniden bağlanana kadar Güvenlik Merkezi tarafından oluşturulan çalışma alanı(lar)'ı silmemelisiniz. Çalışma alanı çok erken silinirse bu işlem başarısız olur.
   >
   >

   - İşlemi iptal etmek için **İptal'i** seçin.

     ![Varolan çalışma alanını seçin][3]

5. Log Analytics Aracısı'nı ayarlamak istediğiniz çalışma alanı için fiyatlandırma katmanını seçin. <br>Varolan bir çalışma alanını kullanmak için, çalışma alanı için fiyatlandırma katmanını ayarlayın. Bu, çalışma alanına bir güvenlik merkezi çözümü yükler, eğer bir çözüm zaten mevcut değilse.

    a.  Güvenlik Merkezi ana **menüsünde, Fiyatlandırma & ayarlarını**seçin.
     
    b.  Aracıyı bağlamak istediğiniz istediğiniz Çalışma Alanını seçin.
        ![Çalışma alanı][7] c'yi seçin. Fiyatlandırma katmanını ayarlayın.
        ![Fiyatlandırma katmanını seçin][9]
   
   >[!NOTE]
   >Çalışma alanında zaten bir **Güvenlik** veya **SecurityCenterFree** çözümü etkinleştirilmişse, fiyatlandırma otomatik olarak ayarlanır. 

## <a name="cross-subscription-workspace-selection"></a>Çapraz abonelik çalışma alanı seçimi
Verilerinizi depolamak için bir çalışma alanı seçtiğinizde, tüm aboneliklerinizdeki tüm çalışma alanları kullanılabilir. Abonelikler arası çalışma alanı seçme özelliği sayesinde farklı aboneliklerde çalışan sanal makinelerden veri toplayabilir ve bunları istediğiniz çalışma alanında depolayabilirsiniz. Bu seçim, kuruluşunuzda merkezi bir çalışma alanı kullandığınızda ve bunu güvenlik veri koleksiyonu için de kullanmak istediğinizde kullanışlıdır. Çalışma alanlarını yönetme hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access)


## <a name="data-collection-tier"></a>Veri toplama katmanı
Azure Güvenlik Merkezi'nde seçtiğiniz veri koleksiyonu katmanı yalnızca güvenlik olaylarının Log Analytics çalışma alanınızda depolanma durumunu etkiler. Log Analytics aracısı, Log Analytics çalışma alanınızda (varsa) hangi güvenlik olaylarını depolamayı seçtiğinize bakılmaksızın Azure Güvenlik Merkezi'nin tehdit koruması için gereken güvenlik olaylarını toplamaya ve analiz etmeye devam edecektir. Güvenlik olaylarını çalışma alanınızda depolamayı seçerek çalışma alanınızda bu olaylarla ilgili araştırma, arama ve denetim işlemi gerçekleştirebilirsiniz. 
> [!NOTE]
> Günlük analitiğinde veri depolamak, veri depolama için ek ücrete tabi olabilir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).
> 
> Abonelikleriniz ve çalışma alanlarınız için çalışma alanınızda depolanacak dört olay kümesiarasından doğru filtreleme ilkesini seçebilirsiniz: 

- **Yok** – Güvenlik olayı depolamaalanını devre dışı kılabilir. Bu varsayılan ayardır.
- **Minimal** – Olay hacmini en aza indirmek isteyen müşteriler için daha küçük bir etkinlik kümesi.
- **Ortak** – Bu, çoğu müşteriyi tatmin eden ve onlara tam bir denetim izi sağlayan bir dizi etkinliktir.
- **Tüm etkinlikler** - Tüm etkinliklerin depolandırılıolduğundan emin olmak isteyen müşteriler için.


> [!NOTE]
> Bu güvenlik olayları kümeleri yalnızca Güvenlik Merkezi'nin Standart katmanında kullanılabilir. Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
Bu kümeler tipik senaryoları ele almak üzere tasarlanmıştır. Uygulamadan önce hangibirinin ihtiyaçlarınıza uyduğunu değerlendirdiğinden emin olun.
>
>

**Ortak** ve **Minimal** etkinlik kümelerine ait olacak olayları belirlemek için, her etkinliğin filtrelenmemiş sıklığını ve bunların kullanımını öğrenmek için müşteriler ve endüstri standartlarıyla birlikte çalıştık. Bu süreçte aşağıdaki yönergeleri kullandık:

- **Minimal** - Bu kümenin yalnızca başarılı bir ihlali ve çok düşük ses düzeyine sahip önemli olayları kapsadığından emin olun. Örneğin, bu küme kullanıcı başarılı ve başarısız giriş (olay iAh4624, 4625) içerir, ancak denetim için önemli ama algılama için anlamlı değildir ve nispeten yüksek hacimli oturum açma içermez. Bu kümenin veri hacminin çoğu oturum açma olayları ve işlem oluşturma olayıdır (olay kimliği 4688).
- **Ortak** - Bu kümede tam bir kullanıcı denetim izi sağlayın. Örneğin, bu set hem kullanıcı girişlerini hem de kullanıcı oturum larını (olay kimliği 4634) içerir. Güvenlik grubu değişiklikleri, önemli etki alanı denetleyicisi Kerberos işlemleri ve sektör kuruluşları tarafından önerilen diğer etkinlikler gibi denetim eylemlerini içeririz.

Çok düşük hacimli olaylar, tüm olaylar yerine seçmek için ana motivasyon olarak belirli olayları filtrelemek için değil, ses düzeyini azaltmak için ortak kümeye dahil edildi.

Burada her set için Güvenlik ve App Locker olay dislerinin tam bir dökümü:

| Veri katmanı | Toplanan olay göstergeleri |
| --- | --- |
| En az | 1102,4624,4625,4657,4663,4688,4700,4702,4719,4720,4722,4723,4724,4727,4728,4732,4735,4737,4739,4740,4754,4755, |
| | 4756,4767,4799,4825,4946,4948,4956,5024,5033,8001,8002,8003,8004,8005,8006,8007,8222 |
| Common | 1,299,300,324,340,403,404,410,411,412,413,431,500,501,1100,1102,1107,1108,4608,4610,4611,4614,4622, |
| |  4624,4625,4634,4647,4648,4649,4657,4661,4662,4663,4665,4666,4667,4688,4670,4672,4673,4674,4675,4689,4697, |
| | 4700,4702,4704,4705,4716,4717,4718,4719,4720,4722,4723,4724,4725,4726,4727,4728,4729,4733,4732,4735,4737, |
| | 4738,4739,4740,4742,4744,4745,4746,4750,4751,4752,4754,4755,4756,4757,4760,4761,4762,4764,4767,4768,4771, |
| | 4774,4778,4779,4781,4793,4797,4798,4799,4800,4801,4802,4803,4825,4826,4870,4886,4887,4888,4893,4898,4902, |
| | 4904,4905,4907,4931,4932,4933,4946,4948,4956,4985,5024,5033,5059,5136,5137,5140,5145,5632,6144,6145,6272, |
| | 6273,6278,6416,6423,6424,8001,8002,8003,8004,8005,8006,8007,8222,26401,30004 |

> [!NOTE]
> - Grup İlkesi Nesnesi (GPO) kullanıyorsanız, denetim ilkeleri Süreç Oluşturma Olayı 4688 ve olay 4688 içindeki *CommandLine* alanını etkinleştirmeniz önerilir. İşlem Oluşturma Etkinliği 4688 hakkında daha fazla bilgi için Güvenlik Merkezi'nin [SSS](faq-data-collection-agents.md#what-happens-when-data-collection-is-enabled)bölümüne bakın. Bu denetim ilkeleri hakkında daha fazla bilgi için Denetim [İlkesi Önerileri'ne](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations)bakın.
> -  [Uyarlanabilir Uygulama Denetimleri](security-center-adaptive-application.md)için veri toplamayı etkinleştirmek için, Güvenlik Merkezi tüm uygulamalara izin verecek şekilde Denetim modunda yerel bir AppLocker ilkesini yapılandırır. Bu, AppLocker'ın daha sonra Güvenlik Merkezi tarafından toplanan ve yararlanılan olaylar oluşturmasına neden olur. Bu ilkenin, zaten yapılandırılmış bir AppLocker ilkesi nin bulunduğu makinelerde yapılandırılmadığını unutmayın. 
> - Windows Filtreleme Platformu [Olay Kimliği 5156'yı](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=5156)toplamak için [Denetim Filtreleme Platformu Bağlantısı](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-connection) (Auditpol /set /alt kategori:"Filtreleme Platformu Bağlantısı" /Başarı:Etkinleştir)
>

Filtreleme ilkenizi seçmek için:
1. Veri **Toplama** sayfasında, **Güvenlik Olayları**altında filtreleme ilkenizi seçin.
2. **Kaydet'i**seçin.

   ![Filtreleme ilkesini seçin][5]

### <a name="automatic-provisioning-in-cases-of-a-pre-existing-agent-installation"></a>Önceden varolan bir aracı yüklemesi durumunda otomatik sağlama<a name="preexisting"></a> 

Aşağıdaki kullanım örnekleri, zaten bir aracı veya uzantı yüklü durumda otomatik hükmün nasıl çalıştığını belirtir. 

- Log Analytics Agent makineye yüklenir, ancak uzantı olarak değil (Doğrudan aracı)<br>
Log Analytics Agent doğrudan VM'ye yüklenirse (Azure uzantısı olarak değil), Güvenlik Merkezi Log Analytics Agent uzantısını yükler ve Log Analytics Agent'ı en son sürüme yükseltebilir.
Yüklü aracı, önceden yapılandırılmış çalışma alanına (lar) rapor vermeye devam eder ve ayrıca Güvenlik Merkezi'nde yapılandırılan çalışma alanına rapor verecektir (Çoklu homing Windows makinelerinde desteklenir).
Yapılandırılan çalışma alanı bir kullanıcı çalışma alanıysa (Güvenlik Merkezi'nin varsayılan çalışma alanı değilse), Güvenlik Merkezi'nin bu çalışma alanına rapor veren VM'lerden ve bilgisayarlardan olayları işlemeye başlaması için "security/"securityFree" çözümlerini yüklemeniz gerekir.<br>
<br>
Linux makineleri için, Agent multi-homing henüz desteklenmez - dolayısıyla, mevcut bir aracı yüklemesi algılanırsa, otomatik sağlama oluşmaz ve makinenin yapılandırması değiştirilmez.
<br>
2019-03-17'den önce Güvenlik Merkezi'ne giden aboneliklerde mevcut makinelerde varolan makinelerde, varolan bir temsilci nin algılanacağı zaman, Log Analytics Agent uzantısı yüklenmez ve makine etkilenmez. Bu makineler için, bu makinelerdeki aracı yükleme sorunlarını çözmek için "Makinelerinizdeki izleme aracısı sağlık sorunlarını giderin" önerisine bakın.

  
- Sistem Merkezi Operasyon Yöneticisi aracısı makineye yüklenir<br>
Güvenlik merkezi, Log Analytics Agent uzantısını varolan Operasyon Yöneticisi'ne yan yana yükler. Varolan Operations Manager aracısı, Normalde Operations Manager sunucusuna rapor vermeye devam edecektir. Operations Manager aracısı ve Log Analytics Aracısı'nın, bu işlem sırasında en son sürüme güncellenecek olan ortak çalışma zamanı kitaplıklarını paylaştığını unutmayın.
Not - Operations Manager aracısı sürümü 2012 yüklüyse, otomatik sağlama açma **yı açmayın.**<br>

- Önceden varolan bir VM uzantısı mevcuttur<br>
    - İzleme Aracısı uzantı olarak yüklendiğinde, uzantı yapılandırması raporlamanın yalnızca tek bir çalışma alanına yapılmasına olanak tanır. Güvenlik Merkezi, kullanıcı çalışma alanlarına varolan bağlantıları geçersiz kılmaz. Güvenlik Merkezi, "güvenlik" veya "securityFree" çözümyüklemesi koşuluyla, vm'deki güvenlik verilerini zaten bağlı olan çalışma alanında depolayacaktır. Güvenlik Merkezi, bu işlemde uzantı sürümünü en son sürüme yükseltebilir.  
    - Varolan uzantınhangi çalışma alanına veri gönderdiğini görmek için, [Azure Güvenlik Merkezi ile bağlantıyı doğrulamak](https://blogs.technet.microsoft.com/yuridiogenes/2017/10/13/validating-connectivity-with-azure-security-center/)için testi çalıştırın. Alternatif olarak, Log Analytics çalışma alanlarını açabilir, bir çalışma alanı seçebilir, VM'yi seçebilir ve Log Analytics aracısı bağlantısına bakabilirsiniz. 
    - Log Analytics aracısının istemci iş istasyonlarına yüklendiği ve varolan bir Log Analytics çalışma alanına raporlama yaptığı bir ortamınız varsa, işletim sisteminizin desteklendiğinden emin olmak için [Azure Güvenlik Merkezi tarafından desteklenen işletim sistemleri](security-center-os-coverage.md) listesini inceleyin. Daha fazla bilgi için bkz: [Varolan günlük analizi müşterileri.](./faq-azure-monitor-logs.md)
 
### <a name="turn-off-automatic-provisioning"></a>Otomatik sağlamayı kapatma<a name="offprovisioning"></a>
Güvenlik ilkesindeki bu ayarı kapatarak istediğiniz zaman kaynaklardan otomatik sağlamayı kapatabilirsiniz. 


1. Güvenlik Merkezi ana menüsüne dönün ve Güvenlik ilkesini seçin.
2. Otomatik sağlamayı devre dışı kaltın istediğiniz abonelik satırındaki **ayarları düzelt'i** tıklatın.
3. Güvenlik **ilkesinde – Veri Toplama** bıçağı, **Otomatik sağlama** altında select **Off**.
4. **Kaydet'i**seçin.

   ![Otomatik sağlamayı devre dışı][6]

Otomatik sağlama devre dışı bırakıldığında (kapalı), varsayılan çalışma alanı yapılandırma bölümü görüntülenmez.

Daha önce açıldıktan sonra otomatik hükmü kapatırsanız:
-   Aracılar yeni VM'lerde sağlanmaz.
-   Güvenlik Merkezi varsayılan çalışma alanından veri toplamayı durdurur.
 
> [!NOTE]
>  Otomatik sağlamanın devre dışı bırakılması, log analytics aracısını aracının sağlandığı Azure VM'lerinden kaldırmaz. OMS uzantısını kaldırma hakkında daha fazla bilgi için, [Güvenlik Merkezi tarafından yüklenen OMS uzantılarını nasıl kaldıracağıma](faq-data-collection-agents.md#remove-oms)bakın.
>
    
## <a name="manual-agent-provisioning"></a>Manuel aracı sağlama<a name="manual-agent"></a>
 
Log Analytics Aracısını el ile yüklemenin birkaç yolu vardır. El ile yüklerken, otomatik sağlamayı devre dışı aldığınızdan emin olun.

### <a name="operations-management-suite-vm-extension-deployment"></a>Operasyon Yönetimi Suite VM uzantı dağıtımı 

Güvenlik Merkezi'nin SANAL'larınızdan güvenlik verileri toplayabilmesi ve öneriler ve uyarılar sunabilmesi için Log Analytics Aracısını el ile yükleyebilirsiniz.
1. Otomatik sağlama -KAPALI) seçeneğini belirleyin.
2. Bir çalışma alanı oluşturun ve Log Analytics Aracısı'nı ayarlamayı istediğiniz çalışma alanı için fiyatlandırma katmanını ayarlayın:

   a.  Güvenlik Merkezi ana menüsünde **Güvenlik ilkesini**seçin.
     
   b.  Aracıyı bağlamak istediğiniz Çalışma Alanı'nı seçin. Çalışma alanının Güvenlik Merkezi'nde kullandığınız abonelikte olduğundan ve çalışma alanında okuma/yazma izinleri olduğundan emin olun.
       ![Çalışma alanını seçme][8]
3. Fiyatlandırma katmanını ayarlayın.
   ![Fiyatlandırma katmanını seçin][9] 
   >[!NOTE]
   >Çalışma alanında zaten bir **Güvenlik** veya **SecurityCenterFree** çözümü etkinleştirilmişse, fiyatlandırma otomatik olarak ayarlanır. 
   > 

4. Kaynak Yöneticisi şablonu kullanarak aracıları yeni VM'lere dağıtmak istiyorsanız, OMS sanal makine uzantısını yükleyin:

   a.  [Windows için OMS sanal makine uzantısını yükleme](../virtual-machines/extensions/oms-windows.md)
    
   b.  [Linux için OMS sanal makine uzantısını yükleyin](../virtual-machines/extensions/oms-linux.md)
5. Uzantıları varolan SANAL Makinelere dağıtmak için Azure [Sanal Makineleri hakkında veri topla'daki](../azure-monitor/learn/quick-collect-azurevm.md)yönergeleri izleyin.

   > [!NOTE]
   > Olay **ve performans verilerini topla** bölümü isteğe bağlıdır.
   >
6. Uzantıyı dağıtmak için PowerShell'i kullanmak için aşağıdaki PowerShell örneğini kullanın:
   
   [!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
   
   1. Log **Analytics'e** gidin ve **Gelişmiş ayarlara**tıklayın.
    
      ![Günlük analitiğini ayarlama][11]

   2. **Değerleri WorkspaceID** ve **Birincil anahtardan**kopyalayın.
  
      ![Değerleri kopyalama][12]

   3. Kamu config ve özel config bu değerlerle doldurmak:
     
           $PublicConf = @{
               "workspaceId"= "<WorkspaceID value>"
           }
 
           $PrivateConf = @{
               "workspaceKey"= "<Primary key value>"
           }

      - Windows VM'ye yüklerken:
        
            Set-AzVMExtension -ResourceGroupName $vm.ResourceGroupName -VMName $vm.Name -Name "MicrosoftMonitoringAgent" -Publisher "Microsoft.EnterpriseCloud.Monitoring" -ExtensionType "MicrosoftMonitoringAgent" -TypeHandlerVersion '1.0' -Location $vm.Location -settings $PublicConf -ProtectedSettingString $PrivateConf -ForceRerun True 
    
      - Linux VM'ye yüklerken:
        
            Set-AzVMExtension -ResourceGroupName $vm1.ResourceGroupName -VMName $vm1.Name -Name "OmsAgentForLinux" -Publisher "Microsoft.EnterpriseCloud.Monitoring" -ExtensionType "OmsAgentForLinux" -TypeHandlerVersion '1.0' -Location $vm.Location -Settingstring $PublicConf -ProtectedSettingString $PrivateConf -ForceRerun True`

> [!NOTE]
> PowerShell'i kullanarak Güvenlik Merkezi'ne nasıl bininilir yönergeleri için [PowerShell'i kullanarak Azure Güvenlik Merkezi'ne binmeyi ana](security-center-powershell-onboarding.md)görün.

## <a name="troubleshooting"></a>Sorun giderme

-   Otomatik sağlama yükleme sorunlarını belirlemek için [bkz.](security-center-troubleshooting-guide.md#mon-agent)

-  İzleme aracısı ağ gereksinimlerini belirlemek için Sorun [Giderme izleme aracısı ağ gereksinimlerine](security-center-troubleshooting-guide.md#mon-network-req)bakın.
-   El ile onboarding sorunlarını belirlemek [için, Operations Management Suite onboarding sorunlarının giderilmesi](https://support.microsoft.com/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues)ne kadar önemli olduğunu öğrenin.

- İzlenmeyen VM'leri ve bilgisayar sorunlarını tanımlamak için:

    Makine Microsoft İzleme Aracısı uzantısını çalıştırmıyorsa, bir VM veya bilgisayar Güvenlik Merkezi tarafından izlenmez. Bir makine, OMS doğrudan aracısı veya Sistem Merkezi Operasyon Yöneticisi aracısı gibi yerel bir aracızaten yüklü olabilir. Bu aracılar Güvenlik Merkezi'nde tam olarak desteklenmedığından, bu aracılara sahip makineler izlenmeden tanımlanır. Güvenlik Merkezi’nin tüm özelliklerinden tam olarak faydalanmak için, Microsoft Monitoring Agent uzantısı gereklidir.

    Güvenlik Merkezi'nin otomatik sağlama için başlatılması gereken VM'leri ve bilgisayarları başarıyla izleyememesinin nedenleri hakkında daha fazla bilgi [için](security-center-troubleshooting-guide.md#mon-agent)bkz.


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Güvenlik Merkezi'nde veri toplama ve otomatik sağlamanın nasıl çalıştığını göstermiştir. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi ile ilgili SSS](faq-general.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.



<!--Image references-->
[1]: ./media/security-center-enable-data-collection/enable-automatic-provisioning.png
[2]: ./media/security-center-enable-data-collection/use-another-workspace.png
[3]: ./media/security-center-enable-data-collection/reconfigure-monitored-vm.png
[5]: ./media/security-center-enable-data-collection/data-collection-tiers.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png
[7]: ./media/security-center-enable-data-collection/select-subscription.png
[8]: ./media/security-center-enable-data-collection/manual-provision.png
[9]: ./media/security-center-enable-data-collection/pricing-tier.png
[10]: ./media/security-center-enable-data-collection/workspace-selection.png
[11]: ./media/security-center-enable-data-collection/log-analytics.png
[12]: ./media/security-center-enable-data-collection/log-analytics2.png
