---
title: Azure Güvenlik Merkezi'nde veri toplamayı | Microsoft Docs
description: Bu makalede, bir Log Analytics aracısının nasıl yükleneceği ve toplanan verilerin depolandığı bir Log Analytics çalışma alanının nasıl ayarlanacağı açıklanır.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 09/10/2019
ms.author: memildin
ms.openlocfilehash: 71c30e0a86f67a2e2826859032144aa491c0cee1
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77597040"
---
# <a name="data-collection-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde veri toplamayı
Güvenlik Merkezi, Azure sanal makinelerinizden (VM), sanal makine ölçek kümelerinden, IaaS kapsayıcılarından ve Azure olmayan (Şirket içi) bilgisayarların yanı sıra güvenlik açıklarını ve tehditleri izlemek için veri toplar. Veriler, makineden güvenlikle ilgili çeşitli yapılandırma ve olay günlüklerini okuyan ve analiz için verileri çalışma alanınıza kopyalayan Log Analytics Aracı kullanılarak toplanır. Bu tür verilerin örnekleri şunlardır: işletim sistemi türü ve sürümü, işletim sistemi günlükleri (Windows olay günlükleri) çalışan işlemler, makine adı, IP adresleri ve kullanıcı oturum. Log Analytics Aracısı Ayrıca kilitlenme bilgi döküm dosyalarını çalışma alanınıza kopyalar.

Eksik güncelleştirmelere görünürlük sağlamak için veri toplama gerekir, yanlış yapılandırılmış işletim sistemi güvenlik ayarları, uç nokta koruma durumu ve sistem durumu ve tehdit algılamaları. 

Bu makalede, bir Log Analytics aracısının nasıl yükleneceği ve toplanan verilerin depolandığı bir Log Analytics çalışma alanının nasıl ayarlanacağı açıklanır. Her iki işlem, veri toplamayı etkinleştirmek için gereklidir. 

> [!NOTE]
> - Veri toplama yalnızca Işlem kaynakları (VM 'Ler, sanal makine ölçek kümeleri, IaaS kapsayıcıları ve Azure dışı bilgisayarlar) için gereklidir. Aracıları sağlama yoksa bile Azure Güvenlik Merkezi'nden yararlanabilir; Ancak, güvenlik kısıtlı olarak erişebilir ve yukarıda listelenen özellikleri desteklenmez.  
> - Desteklenen platformların listesi için bkz. [Azure Güvenlik Merkezi 'Nde desteklenen platformlar](security-center-os-coverage.md).
> - Yeni veya mevcut bir çalışma alanı kullanmanıza bakılmaksızın verileri Log Analytics depolama, veri depolama için ek ücretler gerektirebilir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

## Log Analytics aracısının otomatik sağlamasını etkinleştir<a name="auto-provision-mma"></a>

Makinelerden verileri toplamak için Log Analytics aracısının yüklü olması gerekir. Aracının yüklenmesi otomatik olarak yapılabilir (önerilir) veya aracıyı el ile yükleyebilirsiniz.  

>[!NOTE]
> Otomatik sağlama varsayılan olarak kapalıdır. Güvenlik Merkezi 'ni otomatik sağlamayı varsayılan olarak yükleyecek şekilde ayarlamak için, **seçeneğini Açık**olarak ayarlayın.
>

Otomatik sağlama açık olduğunda, Güvenlik Merkezi, desteklenen tüm Azure VM 'lerde ve oluşturulan tüm yeni makinelerde Log Analytics aracısını sağlar. Otomatik sağlama önemle tavsiye edilir ancak el ile aracı yüklemelerini da kullanılabilir. [Log Analytics Aracısı uzantısını yüklemeyi öğrenin](#manual-agent).



Log Analytics aracısının otomatik sağlamasını etkinleştirmek için:
1. Güvenlik Merkezi ana menüsünde, **fiyatlandırma & ayarları**' nı seçin.
2. Geçerli aboneliğe tıklayın

   ![Abonelik seçme][7]

3. **Veri toplamayı**seçin.
4. Otomatik **sağlama altında,** otomatik sağlamayı etkinleştirmek için **Açık** ' ı seçin.
5. **Kaydet**’i seçin.

   ![Otomatik sağlamayı etkinleştirme][1]

>[!NOTE]
> - Önceden var olan bir yüklemeyi sağlama hakkında yönergeler için, bkz. [Otomatik sağlama, önceden var olan aracı yüklemesi durumlarında](#preexisting).
> - El ile sağlama hakkında yönergeler için bkz. [Log Analytics Agent uzantısını El Ile yüklemeye](#manual-agent).
> - Otomatik sağlamayı kapatma hakkında yönergeler için bkz. [otomatik sağlamayı devre dışı](#offprovisioning)bırakma.
> - PowerShell kullanarak güvenlik merkezi 'ni ekleme hakkında yönergeler için bkz. [PowerShell kullanarak Azure Güvenlik Merkezi 'Ni otomatik hale](security-center-powershell-onboarding.md)getirme.
>

## <a name="workspace-configuration"></a>Çalışma alanı yapılandırması
Güvenlik Merkezi tarafından toplanan veriler, Log Analytics çalışma alanlarında depolanır. Azure Güvenlik Merkezi tarafından oluşturulan çalışma alanlarında veya kendi oluşturduğunuz mevcut bir çalışma depolanan vm'lerden toplanan verileri seçebilirsiniz. 

Çalışma alanı yapılandırması, abonelik başına ayarlanır ve çok abonelik aynı çalışma alanını kullanabilir.

### <a name="using-a-workspace-created-by-security-center"></a>Güvenlik Merkezi tarafından oluşturulan bir çalışma alanını kullanma

Güvenlik Merkezi otomatik olarak veri depolamak için varsayılan çalışma alanı oluşturabilirsiniz. 

Güvenlik Merkezi tarafından oluşturulan bir çalışma alanı seçmek için:

1. **Varsayılan çalışma alanı yapılandırması**altında, Güvenlik Merkezi tarafından oluşturulan çalışma alanlarını kullan ' ı seçin.
   fiyatlandırma katmanını seçin ![][10] 

1. **Kaydet**’e tıklayın.<br>
    Güvenlik Merkezi, coğrafi konum içinde yeni bir kaynak grubu ve varsayılan çalışma alanı oluşturur ve aracıyı bu çalışma alanına bağlar. Çalışma alanını ve kaynak grubu için adlandırma kuralı aşağıdaki gibidir:<br>
   **Çalışma alanı: DefaultWorkspace-[abonelik-KIMLIĞI]-[coğrafi]<br> kaynak grubu: DefaultResourceGroup-[coğrafi]**

   Bir abonelik birden çok geolocations Vm'leri içeriyorsa, Güvenlik Merkezi birden çok çalışma alanı oluşturur. Birden çok çalışma alanı, veri gizlilik kuralları korumak için oluşturulur.
1. Güvenlik Merkezi bir Güvenlik Merkezi çözüm çalışma alanının fiyatlandırma katmanını ayarlamak için abonelik başına otomatik olarak etkinleştirir. 

> [!NOTE]
> Fiyatlandırma katmanı Güvenlik Merkezi tarafından oluşturulan çalışma alanları, Log Analytics, Güvenlik Merkezi faturalandırma etkilemez. Güvenlik Merkezi faturaları her zaman bir çalışma alanına yüklenmiş olan Güvenlik Merkezi ilkelerine ve çözümlerine göre belirlenir. Güvenlik Merkezi, Ücretsiz katmanında varsayılan çalışma alanında *SecurityCenterFree* çözümünü etkinleştirir. Güvenlik Merkezi, Standart katmanda varsayılan çalışma alanında *Security* çözümünü etkinleştirir.
> Log Analytics veri depolama, veri depolama için ek ücretler gerektirebilir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

Mevcut Log Analytics Hesapları hakkında daha fazla bilgi için bkz. [var olan Log Analytics müşterileri](./faq-azure-monitor-logs.md).

### <a name="using-an-existing-workspace"></a>Mevcut bir çalışma alanını kullanma

Zaten mevcut bir Log Analytics çalışma alanınız varsa aynı çalışma alanını kullanmak isteyebilirsiniz.

Mevcut bir Log Analytics çalışma alanınızı kullanmak için olmalıdır okuma ve yazma izinleri için çalışma alanı.

> [!NOTE]
> Mevcut çalışma alanınızda etkin çözüm, kendisine bağlı Azure vm'lerine uygulanır. Ücretli çözümler için bu ek ücretlere neden olabilir. İçin veri gizlilik konuları, seçili çalışma alanının sağ coğrafi bölgede olduğundan emin olun.
> Verileri Log Analytics 'te depolamak, veri depolama için ek ücretler gerektirebilir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

Mevcut bir Log Analytics çalışma alanı seçmek için:

1. **Varsayılan çalışma alanı yapılandırması**altında **başka bir çalışma alanı kullan**' ı seçin.

   ![Mevcut bir çalışma alanı seçin][2]

2. Aşağı açılır menüden, toplanan verileri depolamak için bir çalışma alanı seçin.

   > [!NOTE]
   > Tüm aboneliklerinizi tüm çalışma alanları açılır menü, kullanılabilir. Daha fazla bilgi için bkz. [çapraz abonelik çalışma alanı seçimi](security-center-enable-data-collection.md#cross-subscription-workspace-selection) . Çalışma alanına erişim izni olması gerekir.
   >
   >

3. **Kaydet**’i seçin.
4. **Kaydet**' i seçtikten sonra, daha önce varsayılan bir çalışma alanına bağlı olan Izlenen VM 'leri yeniden yapılandırmak isteyip istemediğiniz sorulur.

   - Yeni çalışma alanı ayarlarının yalnızca yeni VM 'Lere uygulanmasını istiyorsanız **Hayır** ' ı seçin. Yeni çalışma alanı ayarları yalnızca yeni aracı yüklemeleri için geçerlidir; Log Analytics Aracısı yüklü olmayan yeni bulunan VM 'Ler.
   - Yeni çalışma alanı ayarlarının tüm VM 'Lere uygulanmasını istiyorsanız **Evet** ' i seçin. Ayrıca, çalışma alanı oluşturulduğunda bir güvenlik Merkezi'ne bağlı her bir VM yeni hedef çalışma alanına bağlanır.

   > [!NOTE]
   > Evet'i seçerseniz, tüm sanal makineler yeni hedef çalışma alanına bağlandı kadar Güvenlik Merkezi tarafından oluşturulan çalışma alanlarını silmemelisiniz. Bir çalışma alanı çok erken silinirse bu işlem başarısız olur.
   >
   >

   - İşlemi iptal etmek için **Iptal 'i** seçin.

     ![Mevcut bir çalışma alanı seçin][3]

5. Log Analytics aracısını ayarlamayı düşündüğünüz istenen çalışma alanı için fiyatlandırma katmanını seçin. <br>Mevcut bir çalışma alanını kullanmak için çalışma alanı için fiyatlandırma katmanını ayarlayın. Zaten mevcut değilse bu çalışma alanında bir Güvenlik Merkezi çözümü yükler.

    a.  Güvenlik Merkezi ana menüsünde, **fiyatlandırma & ayarları**' nı seçin.
     
    b.  Aracıyı bağlamayı planladığınız istediğiniz çalışma alanını seçin.
        ![çalışma alanı][7] seçin. Fiyatlandırma katmanını ayarlayın.
        fiyatlandırma katmanını seçin ![][9]
   
   >[!NOTE]
   >Çalışma alanında zaten bir **güvenlik** veya **securitycenterücretsiz** çözümü etkinse, fiyatlandırma otomatik olarak ayarlanır. 

## <a name="cross-subscription-workspace-selection"></a>Çapraz abonelik çalışma alanı seçimi
Tüm aboneliklerinizdeki tüm çalışma alanları, verilerinizin depolanacağı bir çalışma alanı seçtiğinizde kullanılabilir. Abonelikler arası çalışma alanı seçme özelliği sayesinde farklı aboneliklerde çalışan sanal makinelerden veri toplayabilir ve bunları istediğiniz çalışma alanında depolayabilirsiniz. Bu seçim, kuruluşunuzda merkezi bir çalışma alanı kullandığınızda ve bunu güvenlik veri koleksiyonu için de kullanmak istediğinizde kullanışlıdır. Çalışma alanlarını yönetme hakkında daha fazla bilgi için bkz. [çalışma alanı erişimini yönetme](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access).


## <a name="data-collection-tier"></a>Veri koleksiyonu katmanı
Azure Güvenlik Merkezi'nde seçtiğiniz veri koleksiyonu katmanı yalnızca güvenlik olaylarının Log Analytics çalışma alanınızda depolanma durumunu etkiler. Log Analytics Aracısı yine de Azure Güvenlik Merkezi 'nin tehdit algılamaları için gereken güvenlik olaylarını toplayıp analiz eder ve Log Analytics çalışma alanınızda (varsa) hangi güvenlik olaylarının depolanmasını istediğinizi seçin. Güvenlik olaylarını çalışma alanınızda depolamayı seçerek çalışma alanınızda bu olaylarla ilgili araştırma, arama ve denetim işlemi gerçekleştirebilirsiniz. 
> [!NOTE]
> Verileri Log Analytics 'te depolamak, veri depolama için ek ücretler gerektirebilir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).
> 
> Çalışma alanınızda depolanmış Abonelikleriniz ve çalışma alanlarından olayların dört kümesi için ilke filtreleme hakkı seçebilirsiniz: 

- **Hiçbiri** – güvenlik olay depolamayı devre dışı bırakın. Bu varsayılan ayardır.
- **Minimum** : olay birimini en aza indirmek isteyen müşteriler için daha küçük bir olay kümesi.
- **Ortak** – bu, çoğu müşteriyi karşılayan ve bunlara tam denetim izi veren bir olay kümesidir.
- **Tüm olaylar – tüm** olayların depolandığından emin olmak isteyen müşteriler için.


> [!NOTE]
> Bu güvenlik olayları kümeleri yalnızca Güvenlik Merkezi'nin standart katmanında kullanılabilir. Güvenlik Merkezi’nin fiyatlandırma katmanları hakkında daha fazla bilgi almak için bkz. [Fiyatlandırma](security-center-pricing.md).
Bu ayarlar, tipik senaryoları için tasarlanmıştır. Uygulamadan önce gereksinimlerinize hangisinin uyduğunu değerlendirmek üzere emin olun.
>
>

**Ortak** ve **En düşük** olay kümelerine ait olan olayları öğrenmek için, her bir olayın filtrelenmemiş sıklığı ve kullanımları hakkında bilgi edinmek üzere müşteriler ve sektör standartları ile çalıştık. Bu işlem aşağıdaki yönergeleri kullandık:

- **En az** -bu küme, yalnızca başarılı bir ihlal ve çok düşük bir birimi olan önemli olayları gösterebilen olayları kapsadığından emin olun. Örneğin, bu küme kullanıcı başarılı ve başarısız oturum açma bilgilerini (olay kimlikleri 4624, 4625) içerir, ancak bu, denetim için önemli olan oturum açma bilgilerini içermez ancak algılama için anlamlı ve görece yüksek hacimdir. Bu veri hacmini çoğunu, oturum açma olayları ve işlem oluşturma olayı (olay kimliği 4688) olur.
- **Ortak** -bu küme içinde tam bir kullanıcı denetim izi sağlayın. Örneğin, bu küme hem Kullanıcı oturum açmaları hem de Kullanıcı imzası içerir (olay KIMLIĞI 4634). Güvenlik grubu değişikliklerini, anahtar etki alanı denetleyicisi Kerberos işlemleri ve sektör kuruluşlar tarafından önerilen diğer olaylar gibi eylemleri denetimi ekliyoruz.

Tüm olayları üzerine miktarının azaltılmasını ve belirli olay filtre için olduğunu seçin için ana motivasyon olarak ortak çok düşük bir birime sahip olayları dahil edilmişti.

Güvenlik ve App Locker olay kimlikleri her küme için tam bir dökümü aşağıda verilmiştir:

| Veri katmanı | Toplanan olay göstergeleri |
| --- | --- |
| En Az | 1102,4624,4625,4657,4663,4688,4700,4702,4719,4720,4722,4723,4724,4727,4728,4732,4735,4737,4739,4740,4754,4755, |
| | 4756,4767,4799,4825,4946,4948,4956,5024,5033,8001,8002,8003,8004,8005,8006,8007,8222 |
| Common | 1, 299, 300, 324, 340, 403, 404, 410, 411, 412, 413, 431, 500, 501, 1100, 1102, 1107, 1108, 4608, 4610, 4611, 4614, 4622, |
| |  4624,4625,4634,4647,4648,4649,4657,4661,4662,4663,4665,4666,4667,4688,4670,4672,4673,4674,4675,4689,4697, |
| | 4700,4702,4704,4705,4716,4717,4718,4719,4720,4722,4723,4724,4725,4726,4727,4728,4729,4733,4732,4735,4737, |
| | 4738,4739,4740,4742,4744,4745,4746,4750,4751,4752,4754,4755,4756,4757,4760,4761,4762,4764,4767,4768,4771, |
| | 4774,4778,4779,4781,4793,4797,4798,4799,4800,4801,4802,4803,4825,4826,4870,4886,4887,4888,4893,4898,4902, |
| | 4904,4905,4907,4931,4932,4933,4946,4948,4956,4985,5024,5033,5059,5136,5137,5140,5145,5632,6144,6145,6272, |
| | 6273,6278,6416,6423,6424,8001,8002,8003,8004,8005,8006,8007,8222,26401,30004 |

> [!NOTE]
> - Grup ilkesi nesnesi (GPO) kullanıyorsanız, denetim ilkeleri Işlem oluşturma olayı 4688 ' nı ve olay 4688 ' nin içindeki *commandLine* alanını etkinleştirmeniz önerilir. Işlem oluşturma olayı 4688 hakkında daha fazla bilgi için Güvenlik Merkezi 'nin [SSS](faq-data-collection-agents.md#what-happens-when-data-collection-is-enabled)bölümüne bakın. Bu denetim ilkeleri hakkında daha fazla bilgi için bkz. [Denetim Ilkesi önerileri](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations).
> -  [Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md)için veri toplamayı etkinleştirmek üzere, Güvenlik Merkezi, tüm uygulamalara izin vermek için denetim modunda yerel bir AppLocker ilkesi yapılandırır. Bu, daha sonra toplanan ve Güvenlik Merkezi tarafından kullanılabilir olaylar oluşturmak AppLocker neden olur. Bu ilke, üzerinde zaten var. yapılandırılmış bir AppLocker İlkesi tüm makinelerde yapılandırılmaz dikkat edin önemlidir. 
> - Windows Filtre Platformu [olay kimliği 5156](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=5156)' i toplamak Için, [Denetim filtreleme platformu bağlantısını](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-connection) etkinleştirmeniz gerekir (auditpol/set/subcategory: "Platform bağlantısı filtreleniyor"/Success: etkinleştir)
>

Filtreleme ilkenizi seçmek için:
1. **Veri toplama** sayfasında, **güvenlik olayları**altında filtreleme ilkenizi seçin.
2. **Kaydet**’i seçin.

   ![Filtreleme ilkesi seçin][5]

### Önceden var olan bir aracı yüklemesi durumlarında otomatik sağlama<a name="preexisting"></a> 

Otomatik sağlama çalışır durumda olduğunda zaten bir aracı veya uzantı yüklü aşağıdaki kullanım örneklerini belirtin. 

- Log Analytics Aracısı makineye yüklendi, ancak uzantı olarak değil (doğrudan aracı)<br>
Log Analytics Aracısı doğrudan VM 'ye (Azure uzantısı olarak değil) yüklenirse, güvenlik merkezi Log Analytics aracı uzantısını yükler ve Log Analytics aracısını en son sürüme yükseltecektir.
Yüklü aracı zaten yapılandırılmış çalışma alanına (ler) rapor etmeye devam eder ve ek olarak güvenlik merkezi 'nde yapılandırılan çalışma alanına rapor eder (Windows makinelerde çoklu barındırma desteklenir).
Yapılandırılmış çalışma alanı bir kullanıcı çalışma alanı ise (Güvenlik Merkezi 'nin varsayılan çalışma alanı değil), bu çalışma alanına raporlama yapan VM 'Ler ve bilgisayarlardan gelen olayları işlemeye başlamak için Güvenlik Merkezi 'nin "güvenlik/" securityFree "çözümünü yüklemeniz gerekir.<br>
<br>
Linux makineler için, aracı çoklu barındırma henüz desteklenmiyor. bu nedenle, mevcut bir aracı yüklemesi algılanırsa, otomatik sağlama gerçekleşmez ve makinenin yapılandırması değiştirilmez.
<br>
Aboneliklerdeki mevcut makineler için eklendi Güvenlik Merkezi 'ne 2019-03-17 önce, var olan bir aracı algılandığında Log Analytics Aracısı uzantısı yüklenmez ve makine etkilenmeyecektir. Bu makineler için, bu makinelerdeki aracı yükleme sorunlarını gidermek için "makinelerinizdeki izleme Aracısı sistem durumu sorunlarını çözün" önerisine bakın.

  
- System Center Operations Manager Aracısı makineye yüklendi<br>
Güvenlik Merkezi, Log Analytics aracı uzantısını mevcut Operations Manager yan yana yükleyecek. Mevcut Operations Manager Aracısı normal olarak Operations Manager sunucusuna rapor etmeye devam edecektir. Operations Manager Aracısı ve Log Analytics aracısının, bu işlem sırasında en son sürüme güncellenecek ortak çalışma zamanı kitaplıklarını paylaştığından emin olun.
Not-Operations Manager Agent 2012 sürümü **yüklüyse, otomatik** sağlamayı açık duruma getirin.<br>

- Önceden var olan VM uzantısı yok<br>
    - Izleme Aracısı bir uzantı olarak yüklendiğinde, uzantı yapılandırması raporlamaya yalnızca tek bir çalışma alanına izin verir. Güvenlik Merkezi, kullanıcı çalışma alanları için varolan bağlantılar kılmaz. Güvenlik Merkezi, "güvenlik" veya "securityFree" çözümünün yüklenmiş olması şartıyla, zaten bağlı olan çalışma alanındaki VM 'den güvenlik verilerini depolar. Güvenlik Merkezi bu işlemdeki en son sürüme uzantı sürümünü yükseltebilir.  
    - Var olan uzantının hangi çalışma alanına veri gönderdiğini görmek için, [Azure Güvenlik Merkezi ile bağlantıyı doğrulamak](https://blogs.technet.microsoft.com/yuridiogenes/2017/10/13/validating-connectivity-with-azure-security-center/)üzere testi çalıştırın. Alternatif olarak, Log Analytics çalışma alanlarını açabilir, bir çalışma alanı seçebilir, sanal makineyi seçebilir ve Log Analytics Aracı bağlantısına bakabilirsiniz. 
    - Log Analytics aracısının istemci iş istasyonlarında yüklü olduğu bir ortamınız varsa ve var olan bir Log Analytics çalışma alanına raporlama yaptıysanız, işletim sisteminizin desteklendiğinden emin olmak için [Azure Güvenlik Merkezi tarafından desteklenen işletim sistemlerinin](security-center-os-coverage.md) listesini gözden geçirin. Daha fazla bilgi için bkz. [var olan Log Analytics müşterileri](./faq-azure-monitor-logs.md).
 
### Otomatik sağlamayı devre dışı bırakma<a name="offprovisioning"></a>
Otomatik kaynaklardan herhangi bir zamanda bu güvenlik ilkesi ayarı devre dışı bırakarak sağlamayı kapatın kapatabilirsiniz. 


1. Güvenlik Merkezi ana menüsüne geri dönün ve Güvenlik İlkesi'ni seçin.
2. Otomatik sağlamayı devre dışı bırakmak istediğiniz aboneliğin satırındaki **Ayarları Düzenle** ' ye tıklayın.
3. **Güvenlik ilkesi – veri toplama** dikey penceresinde, **Otomatik sağlama** altında **kapalı**' yı seçin.
4. **Kaydet**’i seçin.

   ![Otomatik sağlamayı devre dışı bırak][6]

Otomatik sağlama (Kapalı) devre dışı bırakıldığında, varsayılan çalışma alanı yapılandırma bölümü görüntülenmez.

Otomatik sağlama sonra kapalı geçiş yaparsanız daha önce şirket şöyleydi:
-   Aracıları yeni Vm'lere sağlanmadı.
-   Güvenlik Merkezi, varsayılan çalışma alanından veri toplamayı durdurur.
 
> [!NOTE]
>  Otomatik sağlamayı devre dışı bırakmak, aracının sağlandığı Azure VM 'lerinden Log Analytics aracısını kaldırmaz. OMS uzantısını kaldırma hakkında bilgi için, bkz. [Güvenlik Merkezi tarafından yüklenen OMS uzantılarını kaldırma nasıl yaparım?](faq-data-collection-agents.md#remove-oms).
>
    
## El ile aracı sağlama<a name="manual-agent"></a>
 
Log Analytics aracısını el ile yüklemek için birkaç yol vardır. El ile yükleme sırasında otomatik sağlamayı devre dışı bırakıldığından emin olun.

### <a name="operations-management-suite-vm-extension-deployment"></a>Operations Management Suite VM uzantısı dağıtımı 

Güvenlik Merkezi 'nin sanal makinelerinizden güvenlik verilerini toplayabilmesi ve öneriler ve uyarılar sağlaması için Log Analytics aracısını el ile yükleyebilirsiniz.
1. Otomatik sağlama – Kapalı'i seçin.
2. Log Analytics aracısını ayarlamak istediğiniz çalışma alanı için bir çalışma alanı oluşturun ve fiyatlandırma katmanını ayarlayın:

   a.  Güvenlik Merkezi ana menüsünde **güvenlik ilkesi**' ni seçin.
     
   b.  Aracısını bağlamak istediğiniz çalışma alanını seçin. Çalışma alanı aynı abonelikte Güvenlik Merkezi'nde kullanın ve çalışma alanı okuma/yazma izinlerine sahip olduğundan emin olun.
       ![çalışma alanı][8] seçin
3. Fiyatlandırma katmanını ayarlayın.
   fiyatlandırma katmanını seçin ![][9] 
   >[!NOTE]
   >Çalışma alanında zaten bir **güvenlik** veya **securitycenterücretsiz** çözümü etkinse, fiyatlandırma otomatik olarak ayarlanır. 
   > 

4. Resource Manager şablonu kullanılarak yeni Vm'lere aracılarda dağıtmak istiyorsanız, OMS sanal makine uzantısını yükleyin:

   a.  [Windows için OMS sanal makine uzantısını yükler](../virtual-machines/extensions/oms-windows.md)
    
   b.  [Linux için OMS sanal makine uzantısını yükler](../virtual-machines/extensions/oms-linux.md)
5. Mevcut VM 'lerde uzantıları dağıtmak için [Azure sanal makineleri hakkında veri toplama](../azure-monitor/learn/quick-collect-azurevm.md)bölümündeki yönergeleri izleyin.

   > [!NOTE]
   > **Olay ve performans verilerini topla** bölümü isteğe bağlıdır.
   >
6. Uzantıyı dağıtmak için PowerShell kullanmak için aşağıdaki PowerShell örneği kullanın:
   
   [!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
   
   1. **Log Analytics** gidin ve **Gelişmiş ayarlar**' a tıklayın.
    
      ![Log Analytics'i ayarlama][11]

   2. Değerleri çalışma alanı **kimliği** ve **birincil anahtar**dışına kopyalayın.
  
      ![Değerleri kopyalayın][12]

   3. Genel yapılandırma ve özel yapılandırma aşağıdaki değerlerle doldurun:
     
           $PublicConf = @{
               "workspaceId"= "<WorkspaceID value>"
           }
 
           $PrivateConf = @{
               "workspaceKey"= "<Primary key value>"
           }

      - Bir Windows VM'de yüklerken:
        
            Set-AzVMExtension -ResourceGroupName $vm.ResourceGroupName -VMName $vm.Name -Name "MicrosoftMonitoringAgent" -Publisher "Microsoft.EnterpriseCloud.Monitoring" -ExtensionType "MicrosoftMonitoringAgent" -TypeHandlerVersion '1.0' -Location $vm.Location -settings $PublicConf -ProtectedSettingString $PrivateConf -ForceRerun True 
    
      - Bir Linux VM'de yüklerken:
        
            Set-AzVMExtension -ResourceGroupName $vm1.ResourceGroupName -VMName $vm1.Name -Name "OmsAgentForLinux" -Publisher "Microsoft.EnterpriseCloud.Monitoring" -ExtensionType "OmsAgentForLinux" -TypeHandlerVersion '1.0' -Location $vm.Location -Settingstring $PublicConf -ProtectedSettingString $PrivateConf -ForceRerun True`

> [!NOTE]
> PowerShell kullanarak güvenlik merkezi 'ni ekleme hakkında yönergeler için bkz. [PowerShell kullanarak Azure Güvenlik Merkezi 'Ni otomatik hale](security-center-powershell-onboarding.md)getirme.

## <a name="troubleshooting"></a>Sorun giderme

-   Otomatik sağlama yükleme sorunlarını belirlemek için bkz. [Aracı sistem durumu sorunlarını izleme](security-center-troubleshooting-guide.md#mon-agent).

-  İzleme Aracısı ağ gereksinimlerini belirlemek için bkz. [İzleme Aracısı ağ gereksinimlerini giderme](security-center-troubleshooting-guide.md#mon-network-req).
-   El ile ekleme sorunlarını belirlemek için bkz. [Operations Management Suite ekleme sorunlarını giderme](https://support.microsoft.com/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues).

- Izlenmeyen VM 'Leri ve bilgisayar sorunlarını belirlemek için:

    Microsoft Monitoring Agent uzantısını makine çalışmıyorsa bir VM'yi veya bilgisayarı Güvenlik Merkezi tarafından izlenmez. Bir makinede yerel bir aracı zaten yüklü olabilir, örneğin OMS doğrudan Aracısı veya System Center Operations Manager Aracısı. Bu aracıları makinelerle olarak tanımlandığında bu aracılar Güvenlik Merkezi'nde tam olarak desteklenmediğinden izlenmeyen. Güvenlik Merkezi’nin tüm özelliklerinden tam olarak faydalanmak için, Microsoft Monitoring Agent uzantısı gereklidir.

    Güvenlik Merkezi 'nin otomatik sağlama için başlatılan VM 'Leri ve bilgisayarları başarılı bir şekilde izleyememesinin nedenleri hakkında daha fazla bilgi için bkz. [Aracı sistem durumu sorunlarını izleme](security-center-troubleshooting-guide.md#mon-agent).


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede gösterilen, nasıl veri toplama ve otomatik sağlama Güvenlik Merkezi çalışır. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

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
