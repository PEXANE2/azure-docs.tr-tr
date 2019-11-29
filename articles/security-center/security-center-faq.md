---
title: Azure Güvenlik Merkezi hakkında sık sorulan sorular (SSS) | Microsoft Docs
description: Bu SSS, tehditleri önlemenize, algılamanıza ve yanıtlamanıza yardımcı olan Azure Güvenlik Merkezi hakkındaki sorulara yanıt verir.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2019
ms.author: memildin
ms.openlocfilehash: 896db06204188c4347fbdced0b1bb3f216f56ef9
ms.sourcegitcommit: b5d59c6710046cf105236a6bb88954033bd9111b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2019
ms.locfileid: "74558674"
---
# <a name="azure-security-center-frequently-asked-questions-faq"></a>Azure Güvenlik Merkezi - Sık sorulan sorular (SSS)
Bu SSS, Azure Güvenlik Merkezi hakkında, artırılmış görünürlük ile tehditleri önlemenize, algılamanıza ve bu tehditlere yanıt vermenize ve Microsoft Azure kaynaklarınızın güvenliği üzerinde denetim sağlamanıza yardımcı olan bir hizmettir.

> [!NOTE]
> Güvenlik Merkezi, veri toplamak ve depolamak için Microsoft Monitoring Agent kullanır. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi platform geçişi](security-center-platform-migration.md).
>
>

## <a name="general-questions"></a>Genel sorular
### <a name="what-is-azure-security-center"></a>Azure Güvenlik Merkezi nedir?
Azure Güvenlik Merkezi, artırılmış görünürlük aracılığıyla tehditleri engellemenize, algılamanıza ve yanıtlamanıza, ayrıca Azure kaynaklarınızın güvenliğini denetlemenize yardımcı olur. Aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

### <a name="how-do-i-get-azure-security-center"></a>Azure Güvenlik Merkezi 'Ni Nasıl yaparım? mi?
Azure Güvenlik Merkezi, Microsoft Azure aboneliğiniz ile etkinleştirilir ve [Azure Portal](https://azure.microsoft.com/features/azure-portal/)erişilir. ([Portalda oturum açın](https://portal.azure.com), **Araştır**' ı seçin ve **Güvenlik Merkezi**' ne gidin).  

## <a name="billing"></a>Faturalandırma
### <a name="how-does-billing-work-for-azure-security-center"></a>Azure Güvenlik Merkezi için Faturalandırma nasıl çalışır?
Güvenlik Merkezi iki katmanda sunulur:

**Ücretsiz katman** , Azure kaynaklarınızın güvenlik durumu, temel güvenlik ilkesi, güvenlik önerileri ve iş ortaklarının güvenlik ürünleri ve hizmetleriyle tümleştirilmesine ilişkin görünürlük sağlar.

**Standart katman** , tehdit bilgileri, davranış analizi, anomali algılama, güvenlik olayları ve tehdit attributıon raporları dahil olmak üzere Gelişmiş tehdit algılama özellikleri ekler. Standart katman ücretsiz deneme sürümü başlatabilirsiniz. Yükseltmek için güvenlik ilkesinde [fiyatlandırma katmanı](https://docs.microsoft.com/azure/security-center/security-center-pricing) ' nı seçin. Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

### <a name="how-can-i-track-who-in-my-organization-performed-pricing-tier-changes-in-azure-security-center"></a>Kuruluşumun Azure Güvenlik Merkezi 'nde fiyatlandırma katmanı değişikliklerini gerçekleştirdiği kişileri nasıl izleyebilirim?
Azure aboneliklerinde fiyatlandırma katmanını değiştirme izinlerine sahip birden çok yönetici olabilir. Hangi kullanıcının fiyatlandırma katmanı değişikliğini gerçekleştirmediğini öğrenmek için Azure etkinlik günlüğünü kullanın. Daha fazla bilgi için [buraya](https://techcommunity.microsoft.com/t5/Security-Identity/Tracking-Changes-in-the-Pricing-Tier-for-Azure-Security-Center/td-p/390832)bakın.

## <a name="permissions"></a>İzinler
Azure Güvenlik Merkezi, Azure'daki kullanıcılara, gruplara ve hizmetlere atanabilen [yerleşik roller](../role-based-access-control/built-in-roles.md) sağlayan [Rol Tabanlı Erişim Denetimi'ni (RBAC)](../role-based-access-control/role-assignments-portal.md) kullanır.

Güvenlik Merkezi, güvenlik sorunlarını ve güvenlik açıklarını belirlemek için kaynaklarınızın yapılandırmasını değerlendirir. Güvenlik Merkezi 'nde, yalnızca bir kaynağın ait olduğu abonelik veya kaynak grubu için sahip, katkıda bulunan veya okuyucu rolüne atadığınız zaman kaynakla ilgili bilgileri görürsünüz.

Güvenlik Merkezi 'nde roller ve izin verilen eylemler hakkında daha fazla bilgi edinmek için bkz. [Azure Güvenlik Merkezi 'Ndeki izinler](security-center-permissions.md) .

## <a name="data-collection-agents-and-workspaces"></a>Veri toplama, aracılar ve çalışma alanları
Güvenlik Merkezi, Azure sanal makinelerinizden (VM), sanal makine ölçek kümelerinden, IaaS kapsayıcılarından ve Azure olmayan bilgisayarlardan (Şirket içi dahil), güvenlik açıklarını ve tehditleri izlemek için veri toplar. Veriler, makineden güvenlikle ilgili çeşitli yapılandırmaları ve olay günlüklerini okuyup verileri analiz için çalışma alanınıza kopyalayan Microsoft Monitoring Agent kullanılarak toplanır.

### <a name="am-i-billed-for-azure-monitor-logs-on-the-workspaces-created-by-security-center"></a>Güvenlik Merkezi tarafından oluşturulan çalışma alanlarında Azure Izleyici günlükleri için faturalandırılırım mı?
Hayır. Güvenlik Merkezi tarafından oluşturulan ve düğüm başına Azure Izleyici günlükleri için yapılandırılmış çalışma alanları Azure Izleyici günlükleri ücretlerine tabi değildir. Güvenlik Merkezi 'nin faturalandırması her zaman Güvenlik Merkezi güvenlik ilkenize ve bir çalışma alanına yüklenmiş çözümlere göre belirlenir:

- **Ücretsiz katman** : Güvenlik Merkezi, varsayılan çalışma alanındaki ' SecurityCenterFree ' çözümünü sunar. Ücretsiz katman için faturalandırılmaz.
- **Standart katman** : Güvenlik Merkezi, varsayılan çalışma alanındaki ' Güvenlik ' çözümüne izin vermez.

Fiyatlandırma hakkında daha fazla bilgi için bkz. [Güvenlik Merkezi fiyatlandırması](https://azure.microsoft.com/pricing/details/security-center/).

> [!NOTE]
> Güvenlik Merkezi tarafından oluşturulan çalışma alanlarının Log Analytics fiyatlandırma katmanı, güvenlik merkezi faturalandırmasını etkilemez.
>
>

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

### <a name="what-qualifies-a-vm-for-automatic-provisioning-of-the-microsoft-monitoring-agent-installation"></a>Microsoft Monitoring Agent yüklemesinin otomatik olarak sağlanması için bir VM nitelendirir mi?
Windows veya Linux IaaS VM 'Leri şu durumlarda niteler:

- Microsoft Monitoring Agent uzantısı Şu anda VM 'de yüklü değil.
- VM çalışıyor durumunda.
- Windows veya Linux [Azure sanal makine Aracısı](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) yüklendi.
- VM, Web uygulaması güvenlik duvarı veya yeni nesil güvenlik duvarı gibi bir gereç olarak kullanılmaz.

### <a name="can-i-delete-the-default-workspaces-created-by-security-center"></a>Güvenlik Merkezi tarafından oluşturulan varsayılan çalışma alanlarını silebilir miyim?
**Varsayılan çalışma alanını silme önerilmez.** Güvenlik Merkezi, sanal makinelerinizden güvenlik verilerini depolamak için varsayılan çalışma alanlarını kullanır.  Bir çalışma alanını silerseniz, güvenlik merkezi bu verileri toplayamıyor ve bazı güvenlik önerileri ve uyarılar kullanılamıyor.

Kurtarmak için, silinen çalışma alanına bağlı olan VM 'lerdeki Microsoft Monitoring Agent kaldırın. Güvenlik Merkezi aracıyı yeniden yükler ve yeni varsayılan çalışma alanları oluşturur.

### <a name="how-can-i-use-my-existing-log-analytics-workspace"></a>Mevcut Log Analytics çalışma alanım 'ı nasıl kullanabilirim?

Güvenlik Merkezi tarafından toplanan verileri depolamak için mevcut bir Log Analytics çalışma alanı seçebilirsiniz. Mevcut Log Analytics çalışma alanınızı kullanmak için:

- Çalışma alanının seçtiğiniz Azure aboneliğiniz ile ilişkilendirilmesi gerekir.
- En azından, çalışma alanına erişmek için okuma izninizin olması gerekir.

Mevcut bir Log Analytics çalışma alanını seçmek için:

1. **Güvenlik ilkesi – veri koleksiyonu**altında **başka bir çalışma alanı kullan**' ı seçin.

   ![Başka bir çalışma alanı kullan][5]

2. Açılan menüden, toplanan verileri depolamak için bir çalışma alanı seçin.

   > [!NOTE]
   > Aşağı açılan menüde, yalnızca erişiminiz olan ve Azure aboneliğinizde bulunan çalışma alanları gösterilir.
   >
   >

3. **Kaydet**’i seçin.
4. **Kaydet**' i seçtikten sonra, Izlenen VM 'leri yeniden yapılandırmak isteyip istemediğiniz sorulur.

   - Yeni çalışma alanı ayarlarının **yalnızca yeni VM 'lere uygulanmasını**istiyorsanız **Hayır** ' ı seçin. Yeni çalışma alanı ayarları yalnızca yeni aracı yüklemeleri için geçerlidir; Microsoft Monitoring Agent yüklü olmayan yeni bulunan VM 'Ler.
   - Yeni çalışma alanı ayarlarının **Tüm VM 'lere uygulanmasını**istiyorsanız **Evet** ' i seçin. Ayrıca, bir güvenlik merkezi tarafından oluşturulan çalışma alanına bağlı her VM yeni hedef çalışma alanına yeniden bağlanır.

   > [!NOTE]
   > Evet ' i seçerseniz, tüm VM 'Ler yeni hedef çalışma alanına yeniden bağlanana kadar Güvenlik Merkezi tarafından oluşturulan çalışma alanlarını silmemelidir. Çalışma alanı çok erken silinirse bu işlem başarısız olur.
   >
   >

   - İşlemi iptal etmek için **Iptal 'i** seçin.

### Microsoft Monitoring Agent VM 'de zaten bir uzantı olarak yüklüyse ne olacak?<a name="mmaextensioninstalled"></a>
Izleme Aracısı bir uzantı olarak yüklendiğinde, uzantı yapılandırması raporlamaya yalnızca tek bir çalışma alanına izin verir. Güvenlik Merkezi, mevcut kullanıcı çalışma alanları bağlantılarını geçersiz kılmaz. Güvenlik Merkezi, "güvenlik" veya "SecurityCenterFree" çözümünün yüklenmiş olması şartıyla, zaten bağlı olan bir çalışma alanındaki bir VM 'den güvenlik verilerini depolar. Güvenlik Merkezi bu işlemdeki en son sürüme uzantı sürümünü yükseltebilir.

Daha fazla bilgi için bkz. [önceden var olan aracı yüklemesi durumlarında otomatik sağlama](security-center-enable-data-collection.md#preexisting).


### Bir Microsoft Monitoring Agent, bir uzantı (doğrudan aracı) olarak değil makineye doğrudan yüklenmişse ne olur?<a name="directagentinstalled"></a>
Microsoft Monitoring Agent doğrudan VM 'ye (Azure uzantısı olarak değil) yüklenirse, Güvenlik Merkezi Microsoft Monitoring Agent uzantısını yükler ve Microsoft Monitoring Agent 'ı en son sürüme yükseltemez.
Yüklü aracı zaten yapılandırılmış çalışma alanına (ler) raporlama yapmaya devam edecektir ve ayrıca, güvenlik merkezi 'nde yapılandırılan çalışma alanına rapor eder (Windows makinelerde çoklu barındırma desteklenir).
Yapılandırılmış çalışma alanı bir kullanıcı çalışma alanı ise (Güvenlik Merkezi 'nin varsayılan çalışma alanı değil), bu çalışma alanına raporlama yapan VM 'Ler ve bilgisayarlardan gelen olayları işlemeye başlamak için Güvenlik Merkezi 'nin "Security/" SecurityCenterFree "çözümünü yüklemeniz gerekir.

Linux makineler için, aracı çoklu barındırma henüz desteklenmiyor. bu nedenle, mevcut bir aracı yüklemesi algılanırsa, otomatik sağlama gerçekleşmez ve makinenin yapılandırması değiştirilmez.

Aboneliklerdeki mevcut makineler için eklendi Mart 17 2019 ' den önce güvenlik merkezi 'ne, var olan bir aracı algılandığında Microsoft Monitoring Agent uzantısı yüklenmez ve makine etkilenmeyecektir. Bu makineler için, bu makinelerdeki aracı yükleme sorunlarını gidermek için "makinelerinizdeki izleme Aracısı sistem durumu sorunlarını çözme" önerisine bakın

 Daha fazla bilgi için, [VM 'imde System Center Operations Manager veya OMS doğrudan Aracısı zaten yüklüyse](#scomomsinstalled) , sonraki bölüme bakın.

### VM 'imde bir System Center Operations Manager Aracısı zaten yüklüyse ne olur?<a name="scomomsinstalled"></a>
Güvenlik Merkezi, Microsoft Monitoring Agent uzantısını mevcut System Center Operations Manager aracısına yan yana yükleyecek. Mevcut Aracı, System Center Operations Manager sunucusuna normal olarak rapor etmeye devam edecektir. Operations Manager Aracısı ve Microsoft Monitoring Agent, bu işlem sırasında en son sürüme güncellenecek ortak çalışma zamanı kitaplıklarını paylaştığından emin olun. Not-Operations Manager aracısının 2012 sürümü yüklüyse otomatik sağlamayı etkinleştirmeyin (Operations Manager sunucusu da sürüm 2012 olduğunda yönetilebilirlik özellikleri kaybolabilir).

### <a name="what-is-the-impact-of-removing-these-extensions"></a>Bu uzantıları kaldırmanın etkisi nedir?
Microsoft Monitoring uzantısını kaldırırsanız, güvenlik merkezi VM 'den güvenlik verileri toplayamaz ve bazı güvenlik önerileri ve uyarılar kullanılamaz. Güvenlik Merkezi, 24 saat içinde, VM 'nin uzantının eksik olduğunu belirler ve uzantıyı yeniden yükler.

### <a name="how-do-i-stop-the-automatic-agent-installation-and-workspace-creation"></a>Nasıl yaparım? otomatik aracı yüklemesi ve çalışma alanı oluşturma durdurulsun mu?
Güvenlik ilkesinde abonelikleriniz için otomatik sağlamayı devre dışı bırakabilirsiniz, ancak bu önerilmez. Otomatik sağlama devre dışı bırakmak, güvenlik merkezi önerilerini ve uyarılarını kısıtlar. Otomatik sağlamayı devre dışı bırakmak için:

1. Aboneliğiniz standart katman için yapılandırılmışsa, bu abonelik için güvenlik ilkesini açın ve **ücretsiz** katmanı seçin.

   ![Fiyatlandırma katmanı][1]

2. Sonra, **güvenlik ilkesi – veri toplama** sayfasında **kapalı** ' yı seçerek otomatik sağlamayı devre dışı bırakın.
   ![Veri toplama][2]

### <a name="should-i-opt-out-of-the-automatic-agent-installation-and-workspace-creation"></a>Otomatik aracı yüklemesi ve çalışma alanı oluşturmayı kabul etmem gerekir mi?

> [!NOTE]
> Bölümleri yeniden [kullanıma alma etkileri nelerdir?](#what-are-the-implications-of-opting-out-of-automatic-provisioning) ve otomatik sağlamayı devre dışı bırakmak isterseniz, [Önerilen adımları](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning) gözden geçirdiğinizden emin olun.
>
>

Aşağıdakiler geçerliyse otomatik sağlamayı devre dışı bırakmak isteyebilirsiniz:

- Güvenlik Merkezi tarafından otomatik aracı yüklemesi tüm abonelik için geçerlidir. Otomatik yüklemeyi VM 'lerin bir alt kümesine uygulayamazsınız. Microsoft Monitoring Agent ile yüklenemeden kritik VM 'Ler varsa, otomatik sağlamayı devre dışı geçirmeniz gerekir.
- Microsoft Monitoring Agent (MMA) uzantısının yüklenmesi aracının sürümünü güncelleştirir. Bu, doğrudan aracı ve bir System Center Operations Manager Aracısı için geçerlidir (ikinci içinde, Operations Manager ve MMA paylaşma ortak çalışma zamanı kitaplıklarını, işlem içinde güncelleştirilecektir). Yüklü Operations Manager Aracısı sürüm 2012 ise ve yükseltildiyse, Operations Manager sunucusu da sürüm 2012 olduğunda yönetilebilirlik özellikleri kaybolabilir. Yüklü Operations Manager Aracısı sürüm 2012 ise otomatik sağlamayı tercih edin.
- Abonelik dışında (Merkezi çalışma alanı) özel bir çalışma alanınız varsa, otomatik sağlamayı devre dışı geçirmeniz gerekir. Microsoft Monitoring Agent uzantısını el ile yükleyebilir ve bağlantıyı geçersiz kılarak Güvenlik Merkezi 'Ni kullanmadan çalışma alanınıza bağlayabilirsiniz.
- Abonelik başına birden çok çalışma alanı oluşturulmasını önlemek istiyorsanız ve abonelik içinde kendi özel çalışma alanınız varsa iki seçeneğiniz vardır:

   1. Otomatik sağlamayı devre dışı bırakabilirsiniz. Geçişten sonra, [var olan Log Analytics çalışma alanım 'ı nasıl kullanabilirim](#how-can-i-use-my-existing-log-analytics-workspace) bölümünde açıklandığı gibi varsayılan çalışma alanı ayarlarını ayarlayın.
   2. Ya da geçişin tamamlanmasına, VM 'Lere yüklenecek Microsoft Monitoring Agent ve oluşturulan çalışma alanına bağlı VM 'Lere izin verebilirsiniz. Daha sonra, zaten yüklü olan aracıları yeniden yapılandırmak için, varsayılan çalışma alanınızı ' de kabul edilmesiyle olarak ayarlayarak kendi özel çalışma alanınızı seçin. Daha fazla bilgi için bkz. [var olan Log Analytics çalışma alanım 'ı nasıl kullanabilirim?](#how-can-i-use-my-existing-log-analytics-workspace)

### <a name="what-are-the-implications-of-opting-out-of-automatic-provisioning"></a>Otomatik sağlamayı geri alma etkileri nelerdir?
Geçiş tamamlandığında, güvenlik merkezi VM 'den güvenlik verileri toplayabilir ve bazı güvenlik önerileri ve uyarılar kullanılamaz. Devre dışı bırakırsanız Microsoft Monitoring Agent el ile yükleyebilirsiniz. [Çalışırken önerilen adımlara](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning)bakın.

### <a name="what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning"></a>Otomatik sağlamayı geri yüklerken önerilen adımlar nelerdir?

Güvenlik Merkezi 'nin sanal makinelerinizden güvenlik verilerini toplayabilmesi ve öneriler ve uyarılar sağlaması için Microsoft Monitoring Agent uzantısını el ile yükleyebilirsiniz. Yükleme ile ilgili yönergeler için bkz. [WINDOWS VM için aracı yükleme](../virtual-machines/extensions/oms-windows.md) veya [Linux sanal makinesi için aracı yükleme](../virtual-machines/extensions/oms-linux.md) .

Aracıyı var olan herhangi bir özel çalışma alanına veya Güvenlik Merkezi tarafından oluşturulan çalışma alanına bağlayabilirsiniz. Özel bir çalışma alanında ' Security ' veya ' SecurityCenterFree ' çözümleri etkinleştirilmemişse, bir çözüm uygulamanız gerekir. Uygulamak için, özel çalışma alanı veya aboneliği seçin ve **güvenlik ilkesi – fiyatlandırma katmanı** sayfası aracılığıyla bir fiyatlandırma katmanı uygulayın.

   ![Fiyatlandırma katmanı][1]

Güvenlik Merkezi, seçili fiyatlandırma katmanına göre çalışma alanında doğru çözümü etkinleştirir.

### Güvenlik Merkezi tarafından yüklenen OMS uzantıları Nasıl yaparım? kaldırılsın mı?<a name="remove-oms"></a>
Microsoft Monitoring Agent el ile kaldırabilirsiniz. Bu, güvenlik merkezi önerilerini ve uyarılarını sınırlandırdığından önerilmez.

> [!NOTE]
> Veri toplama etkinleştirilirse Güvenlik Merkezi, aracıyı kaldırdıktan sonra yeniden yükler.  Aracıyı el ile kaldırmadan önce veri toplamayı devre dışı bırakmanız gerekir. Otomatik aracı yüklemesini ve çalışma alanı oluşturmayı durdurmak Nasıl yaparım? bakın. veri toplamayı devre dışı bırakma yönergeleri için.
>
>

Aracıyı el ile kaldırmak için:

1.  Portalda **Log Analytics**açın.
2.  Log Analytics sayfasında bir çalışma alanı seçin:
3.  İzlemek istemediğiniz VM 'Leri seçin ve **bağlantıyı kes**' i seçin.

   ![Aracıyı Kaldırma][3]

> [!NOTE]
> Bir Linux VM 'sinde zaten uzantı olmayan OMS Aracısı varsa, uzantının kaldırılması aracıyı de kaldırır ve müşterinin yeniden yüklenmesi gerekir.
>
>
### <a name="how-do-i-disable-data-collection"></a>Veri toplamayı devre dışı Nasıl yaparım?.
Otomatik sağlama varsayılan olarak kapalıdır. Bu ayarı güvenlik ilkesinde devre dışı bırakarak, her zaman kaynaklardan otomatik sağlamayı devre dışı bırakabilirsiniz. Sistem güncelleştirmeleri, işletim sistemi güvenlik açıkları ve uç nokta koruması hakkında güvenlik uyarıları ve öneriler almak için otomatik sağlama kesinlikle önerilir.

Veri toplamayı devre dışı bırakmak için [Azure Portal oturum açın](https://portal.azure.com), **Araştır**' ı seçin, **Güvenlik Merkezi**' ni seçin ve **ilke Seç**' i seçin. Otomatik sağlamayı hangi abonelik için devre dışı bırakmak istediğinizi belirtin. Bir abonelik **güvenlik ilkesi seçtiğinizde, veri toplama** açılır. **Otomatik sağlama**altında **kapalı**' yı seçin.

### <a name="how-do-i-enable-data-collection"></a>Veri toplamayı etkinleştirmek Nasıl yaparım? mı?
Azure aboneliğiniz için veri toplamayı güvenlik ilkesinde etkinleştirebilirsiniz. Veri toplamayı etkinleştirmek için. [Azure Portal oturum açın](https://portal.azure.com), **Araştır**' ı seçin, **Güvenlik Merkezi**' ni seçin ve **güvenlik ilkesi**' ni seçin. Otomatik sağlamayı etkinleştirmek istediğiniz aboneliği seçin. Bir abonelik **güvenlik ilkesi seçtiğinizde, veri toplama** açılır. **Otomatik sağlama**altında **Açık**' ı seçin.

### <a name="what-happens-when-data-collection-is-enabled"></a>Veri toplama etkinleştirildiğinde ne olur?
Otomatik sağlama etkinleştirildiğinde Güvenlik Merkezi, desteklenen tüm Azure VM 'lerde ve oluşturulan tüm yeni makinelerde Microsoft Monitoring Agent sağlar. Otomatik sağlama önerilir, ancak el ile aracı yüklemesi de kullanılabilir. [Microsoft Monitoring Agent uzantısını yüklemeyi öğrenin](../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension). 

Aracı, işlem oluşturma olayı 4688 ve olay 4688 ' nin içindeki *commandLine* alanı ' na izin vermez. VM üzerinde oluşturulan yeni süreçler, olay günlüğü tarafından kaydedilir ve Güvenlik Merkezi 'nin algılama hizmetleri tarafından izlenir. Her yeni işlem için kaydedilen ayrıntılar hakkında daha fazla bilgi için bkz. [4688 içindeki açıklama alanları](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4688#fields). Aracı Ayrıca, VM 'de oluşturulan 4688 olayını toplar ve bunları arama halinde depolar.

Aracı, [Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md)için veri toplamayı da sağlar, Güvenlik Merkezi, tüm uygulamalara izin vermek için denetim modunda yerel bir AppLocker ilkesi yapılandırır. Bu ilke, AppLocker 'ın Güvenlik Merkezi tarafından toplanan ve yararlanılabilir olayları oluşturmasına neden olur. Bu ilkenin zaten yapılandırılmış bir AppLocker ilkesinin bulunduğu makinelerde yapılandırılmaması gerektiğini unutmayın. 

Güvenlik Merkezi, VM 'de şüpheli etkinlik algıladığında, [güvenlik iletişim bilgileri](security-center-provide-security-contact-details.md) sağlanmışsa müşteri e-posta ile bildirilir. Güvenlik Merkezi 'nin güvenlik uyarıları panosunda bir uyarı da görünür.

### <a name="will-security-center-work-using-an-oms-gateway"></a>Güvenlik Merkezi, bir OMS ağ geçidi kullanarak çalışacak mı?
Evet. Azure Güvenlik Merkezi, Microsoft Monitoring Agent kullanarak Azure sanal makinelerinden ve sunuculardan veri toplamak için Azure Izleyicisini kullanır.
Verileri toplamak için, her VM ve sunucunun HTTPS kullanarak Internet 'e bağlanması gerekir. Bağlantı, bir proxy kullanılarak veya [OMS ağ geçidi](../azure-monitor/platform/gateway.md)aracılığıyla doğrudan olabilir.

### <a name="does-the-monitoring-agent-impact-the-performance-of-my-servers"></a>Izleme Aracısı sunucularım performansını etkiler mi?
Aracı nominal miktarda sistem kaynağı tüketir ve performansı çok az etkiler. Performans etkisi ve aracı ve uzantı hakkında daha fazla bilgi için bkz. [planlama ve işlemler Kılavuzu](security-center-planning-and-operations-guide.md#data-collection-and-storage).

### <a name="where-is-my-data-stored"></a>Verilerim nerede depolanır?
Bu aracıdan toplanan veriler, aboneliğiniz veya yeni bir çalışma alanıyla ilişkili mevcut bir Log Analytics çalışma alanında depolanır. Daha fazla bilgi için bkz. [veri güvenliği](security-center-data-security.md).

## Mevcut Azure Izleyici günlükleri müşterileri<a name="existingloganalyticscust"></a>

### <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>Güvenlik Merkezi, VM 'Ler ve çalışma alanları arasındaki mevcut bağlantıları geçersiz kılar mi?
Bir VM zaten bir Azure uzantısı olarak Microsoft Monitoring Agent yüklüyse, güvenlik merkezi mevcut çalışma alanı bağlantısını geçersiz kılmaz. Bunun yerine, güvenlik merkezi mevcut çalışma alanını kullanır. "Güvenlik" veya "SecurityCenterFree" çözümünün, raporlama yaptığı çalışma alanına yüklenmiş olması için VM korunacaktır. 

Zaten mevcut değilse, veri toplama ekranında seçilen çalışma alanına bir güvenlik merkezi çözümü yüklenir ve bu çözüm yalnızca ilgili VM 'lere uygulanır. Bir çözüm eklediğinizde, varsayılan olarak, Log Analytics çalışma alanınıza bağlı tüm Windows ve Linux aracılarına otomatik olarak dağıtılır. [Çözüm hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md) , çözümlerinizi bir kapsam uygulamanıza olanak tanır.

Microsoft Monitoring Agent doğrudan VM 'ye (Azure uzantısı olarak değil) yüklenirse, Güvenlik Merkezi Microsoft Monitoring Agent yüklemez ve güvenlik izleme sınırlı olur.

### <a name="does-security-center-install-solutions-on-my-existing-log-analytics-workspaces-what-are-the-billing-implications"></a>Güvenlik Merkezi, mevcut Log Analytics çalışma alanlarıma çözüm yüklesin mi? Faturalandırma etkileri nelerdir?
Güvenlik Merkezi, bir sanal makinenin oluşturduğunuz bir çalışma alanına zaten bağlı olduğunu belirlediğinde, Güvenlik Merkezi, fiyatlandırma katmanınıza göre bu çalışma alanındaki çözümleri sağlar. Çözümler, [çözüm hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md)aracılığıyla yalnızca Ilgili Azure sanal makinelerine uygulanır, böylece faturalandırma aynı kalır.

- **Ücretsiz katman** : Güvenlik Merkezi, çalışma alanına ' SecurityCenterFree ' çözümünü yüklüyor. Ücretsiz katman için faturalandırılmaz.
- **Standart katman** : Güvenlik Merkezi, çalışma alanına ' Güvenlik ' çözümünü yüklüyor.

   ![Varsayılan çalışma alanındaki çözümler][4]

### <a name="i-already-have-workspaces-in-my-environment-can-i-use-them-to-collect-security-data"></a>Ortammda çalışma alanım zaten var, bunları güvenlik verilerini toplamak için kullanabilir miyim?
Bir VM zaten bir Azure uzantısı olarak Microsoft Monitoring Agent yüklüyse, güvenlik merkezi var olan bağlı çalışma alanını kullanır. Zaten mevcut değilse, bir güvenlik merkezi çözümü çalışma alanına yüklenir ve çözüm [hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md)aracılığıyla yalnızca ilgili sanal makinelere uygulanır.

Güvenlik Merkezi VM 'Lere Microsoft Monitoring Agent yüklediğinde, Güvenlik Merkezi tarafından oluşturulan varsayılan çalışma alanlarını kullanır.

### <a name="i-already-have-security-solution-on-my-workspaces-what-are-the-billing-implications"></a>Çalışma alanlarım üzerinde zaten güvenlik çözümüdür. Faturalandırma etkileri nelerdir?
Güvenlik & denetim çözümü, Azure VM 'Leri için Güvenlik Merkezi Standart katmanı özelliklerini etkinleştirmek üzere kullanılır. Güvenlik & denetim çözümü bir çalışma alanında zaten yüklüyse, güvenlik merkezi mevcut çözümü kullanır. Faturalandırma sırasında değişiklik yapılmaz.

## <a name="using-azure-security-center"></a>Azure Güvenlik Merkezi 'ni kullanma
### <a name="what-is-a-security-policy"></a>Güvenlik ilkesi nedir?
Bir güvenlik ilkesi, belirtilen abonelikteki kaynaklar için önerilen denetim kümesini tanımlar. Azure Güvenlik Merkezi 'nde, şirketinizin güvenlik gereksinimlerine ve uygulama türüne ve her abonelikteki verilerin duyarlılığına göre Azure abonelikleriniz için ilkeler tanımlarsınız.

Azure Güvenlik Merkezi 'nde etkinleştirilen güvenlik ilkeleri, güvenlik önerileri ve izleme. Güvenlik ilkeleri hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi 'Nde güvenlik sistem durumu izleme](security-center-monitoring.md).

### <a name="who-can-modify-a-security-policy"></a>Güvenlik ilkesini kim değiştirebilir?
Bir güvenlik ilkesini değiştirmek için, bir güvenlik yöneticisi veya bu aboneliğin sahibi veya katılımcısı olmanız gerekir.

Güvenlik ilkesi yapılandırma hakkında bilgi edinmek için bkz. [Azure Güvenlik Merkezi 'nde güvenlik Ilkelerini ayarlama](tutorial-security-policy.md).

### <a name="what-is-a-security-recommendation"></a>Güvenlik önerisi nedir?
Azure Güvenlik Merkezi, Azure kaynaklarınızın güvenlik durumunu analiz eder. Olası güvenlik açıkları tanımlandığında, öneriler oluşturulur. Öneriler, gerekli denetimi yapılandırma sürecinde size yol gösterir. Örnekler şunlardır:

* Kötü amaçlı yazılımdan koruma sağlama kötü amaçlı yazılımları tanımanıza ve kaldırmanıza yardımcı olur
* Sanal makinelere trafiği denetlemek için [ağ güvenlik grupları](../virtual-network/security-overview.md) ve kuralları
* Web uygulamalarınızı hedefleyen saldırılara karşı savunmasına yardımcı olmak için bir Web uygulaması güvenlik duvarı sağlama
* Eksik sistem güncelleştirmelerini dağıtma
* Önerilen taban çizgileriyle eşleşmeyen işletim sistemi yapılandırmalarını ele alma

Yalnızca güvenlik Ilkelerinde etkinleştirilen öneriler burada gösterilmiştir.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Azure kaynaklarım 'ın geçerli güvenlik durumunu nasıl görebilirim?
**Güvenlik Merkezi 'Ne genel bakış** sayfası, ortamınızın Işlem, ağ, depolama & verileri ve uygulamalar tarafından ayrılmış genel güvenlik duruşunu gösterir. Her kaynak türünün, olası güvenlik açıklarına ilişkin tanımlanmış olup olmadığını gösteren bir göstergesi vardır. Her kutucuğa tıklamak, Güvenlik Merkezi tarafından tanımlanan güvenlik sorunlarının bir listesini, aboneliğinizdeki kaynakların envanterini gösterir.

### <a name="what-triggers-a-security-alert"></a>Güvenlik uyarısı ne tetikler?
Azure Güvenlik Merkezi, Azure kaynaklarınızdan, ağdan ve kötü amaçlı yazılımdan koruma ve güvenlik duvarları gibi iş ortağı çözümlerinden günlük verilerini otomatik olarak toplar, çözümler ve kullanır. Tehditler algılandığında bir güvenlik uyarısı oluşturulur. Örneklere şunların algılanması dahildir:

* Bilinen kötü amaçlı IP adresleri ile iletişim kuran tehlikeye girmiş sanal makineler
* Windows hata bildirimi kullanılarak algılanan gelişmiş kötü amaçlı yazılım
* Sanal makinelere karşı deneme yanılma saldırıları
* Kötü amaçlı yazılımdan koruma veya Web uygulaması güvenlik duvarları gibi tümleşik iş ortağı güvenlik çözümlerinin güvenlik uyarıları

### Neden güvenli puanlar değerlerinin değiştirilmesi? <a name="secure-score-faq"></a>
Şubat 2019 itibariyle Güvenlik Merkezi, önem derecesini daha iyi uydurmak için birkaç önerilerin Puanını ayarladı. Bu ayarlamanın sonucunda, genel olarak güvenli puan değerlerinde değişiklikler olabilir.  Güvenli Puanlama hakkında daha fazla bilgi için bkz. [güvenli Puanlama hesaplama](security-center-secure-score.md).

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Microsoft Güvenlik Yanıt Merkezi 'nin Azure Güvenlik Merkezi 'ne göre algılanan ve uyarı oluşan tehditler arasındaki fark nedir?
Microsoft Güvenlik Yanıt Merkezi (MSRC), Azure ağ ve altyapısının güvenlik izlemesini seçin ve üçüncü taraflardan tehdit zekasını ve uygunsuz kullanım şikayetini alır. MSRC müşteri verilerine izinsiz veya yetkisiz bir taraf tarafından erişildiğine veya müşterinin Azure 'un kullanımı, kabul edilebilir kullanım koşullarına uymadığında, bir güvenlik olay Yöneticisi müşteriyi bilgilendirir. Bildirim genellikle Azure Güvenlik Merkezi 'nde belirtilen güvenlik kişilerine veya bir güvenlik kişisi belirtilmemişse Azure abonelik sahibine bir e-posta göndererek oluşur.

Güvenlik Merkezi, müşterinin Azure ortamını sürekli olarak izleyen ve analiz uygulayan ve çok sayıda kötü amaçlı olabilecek etkinlikleri otomatik olarak algılayan bir Azure hizmetidir. Bu algılamalar Güvenlik Merkezi panosunda güvenlik uyarıları olarak ortaya çıkmış.

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Azure Güvenlik Merkezi tarafından hangi Azure kaynakları izleniyor?
Azure Güvenlik Merkezi aşağıdaki Azure kaynaklarını izler:

* Sanal makineler (VM 'Ler) ( [Cloud Services](../cloud-services/cloud-services-choose-me.md)dahil)
* Sanal makine ölçek kümeleri
* Azure Sanal Ağları
* Azure SQL hizmeti
* Azure Storage hesabı
* Azure Web Apps ( [App Service ortamı](../app-service/environment/intro.md))
* VM 'lerde ve App Service Ortamı bir Web uygulaması güvenlik duvarı gibi Azure aboneliğiniz ile tümleştirilmiş iş ortağı çözümleri

Ayrıca, Azure dışı (Şirket içi) bilgisayarlar da Azure Güvenlik Merkezi tarafından izlenebilir (hem [Windows bilgisayarları](./quick-onboard-windows-computer.md) hem de [Linux bilgisayarları](./quick-onboard-linux-computer.md) desteklenir)

## <a name="virtual-machines"></a>Virtual Machines (Sanal Makineler)
### <a name="what-types-of-virtual-machines-are-supported"></a>Hangi tür sanal makineler destekleniyor?
İzleme ve öneriler, hem klasik hem de [Kaynak Yöneticisi dağıtım modelleri](../azure-classic-rm.md)kullanılarak oluşturulan sanal makineler (VM) için kullanılabilir.

Desteklenen platformların listesi için bkz. [Azure Güvenlik Merkezi 'Nde desteklenen platformlar](security-center-os-coverage.md) .

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Azure Güvenlik Merkezi, Azure VM 'imde çalışan kötü amaçlı yazılımdan koruma çözümünü neden tanımıyor?
Azure Güvenlik Merkezi 'nin, Azure uzantıları aracılığıyla yüklenen kötü amaçlı yazılımdan bir görünürlüğü vardır. Örneğin, Güvenlik Merkezi, belirttiğiniz bir görüntüde önceden yüklenmiş olan kötü amaçlı yazılımdan koruma veya kendi işlemlerinizi (örneğin, yapılandırma yönetimi sistemleri) kullanarak sanal makinelerinize yükleiyor.

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Neden Sanal makinem için "tarama verilerini eksik" iletisini alıyorum?
Bu ileti, VM için tarama verisi olmadığında görüntülenir. Veri Toplama, Azure Güvenlik Merkezi'nde etkinleştirildikten sonra tarama verilerinin toplanması zaman (bir saatten az) alabilir. Tarama verileri ilk kez alındıktan sonra hiç tarama verisi veya güncel tarama verisi olmadığından bu iletiyle karşılaşabilirsiniz. Durdurulmuş VM'lerle ilgili değerler taramaya eklenmez. Bu ileti, tarama verileri yakın zamanda doldurulmamışsa de görünebilir (varsayılan değeri 30 gün olan Windows aracısının bekletme ilkesine uygun olarak).

### <a name="how-often-does-security-center-scan-for-operating-system-vulnerabilities-system-updates-and-endpoint-protection-issues"></a>Güvenlik Merkezi, işletim sistemi güvenlik açıkları, sistem güncelleştirmeleri ve uç nokta koruma sorunlarını ne sıklıkta tarar?
Güvenlik açıkları, güncelleştirmeler ve sorunlar için Güvenlik Merkezi taramalarının gecikme süreleri aşağıda verilmiştir:

- İşletim sistemi güvenlik yapılandırması – veriler 48 saat içinde güncelleştirilir
- Sistem güncelleştirmeleri – veriler 24 saat içinde güncelleştirilir
- Endpoint Protection sorunları – veriler 8 saat içinde güncelleştirilir

Güvenlik Merkezi genellikle her saat yeni verileri tarar ve önerileri buna göre yeniler. 

> [!NOTE]
> Güvenlik Merkezi, veri toplamak ve depolamak için Microsoft Monitoring Agent kullanır. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi platform geçişi](security-center-platform-migration.md).
>
>

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>"VM Aracısı eksik mı?" iletisini neden alıyorum?
Veri toplamayı etkinleştirmek için VM 'Lere sanal makine aracısının yüklü olması gerekir. VM Aracısı, Azure Marketi’nden dağıtılan VM’ler için varsayılan olarak yüklüdür. VM aracısının diğer VM 'Lere nasıl yükleneceği hakkında bilgi için bkz. Blog Post [VM Aracısı ve uzantıları](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).


<!--Image references-->
[1]: ./media/security-center-platform-migration-faq/pricing-tier.png
[2]: ./media/security-center-platform-migration-faq/data-collection.png
[3]: ./media/security-center-platform-migration-faq/remove-the-agent.png
[4]: ./media/security-center-platform-migration-faq/solutions.png
[5]: ./media/security-center-platform-migration-faq/use-another-workspace.png
