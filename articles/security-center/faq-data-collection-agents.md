---
title: Azure Güvenlik Merkezi SSS-veri toplama ve aracılar
description: Azure Güvenlik Merkezi için veri toplama, aracılar ve çalışma alanları hakkında sık sorulan sorular ve tehditleri önlemenize, algılamanıza ve yanıtlamanıza yardımcı olan bir ürün
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
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: 8317a13b9ef87679836f55627268deefa4500dce
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78372824"
---
# <a name="faq---questions-about-data-collection-agents-and-workspaces"></a>SSS-veri toplama, aracılar ve çalışma alanları hakkında sorular

Güvenlik Merkezi, güvenlik açıklarını ve tehditleri izlemek için Azure sanal makinelerinizden (VM), sanal makine ölçek kümelerinden, IaaS kapsayıcılarından ve Azure olmayan bilgisayarlardan (Şirket içi makineler dahil) veri toplar. Veriler, makineden güvenlikle ilgili çeşitli yapılandırmaları ve olay günlüklerini okuyup verileri analiz için çalışma alanınıza kopyalayan Microsoft Monitoring Agent kullanılarak toplanır.


## <a name="am-i-billed-for-azure-monitor-logs-on-the-workspaces-created-by-security-center"></a>Güvenlik Merkezi tarafından oluşturulan çalışma alanlarında Azure Izleyici günlükleri için faturalandırılırım mı?

Hayır. Güvenlik Merkezi tarafından oluşturulan ve düğüm başına Azure Izleyici günlükleri için yapılandırılmış çalışma alanları Azure Izleyici günlükleri ücretlerine tabi değildir. Güvenlik Merkezi her zaman, Güvenlik Merkezi güvenlik ilkesi ve bir çalışma alanına yüklenmiş çözümlere göre faturalandırılır:

- **Ücretsiz katman** : Güvenlik Merkezi, varsayılan çalışma alanındaki ' SecurityCenterFree ' çözümünü sunar. Ücretsiz katman için faturalandırılmaz.

- **Standart katman** : Güvenlik Merkezi, varsayılan çalışma alanındaki ' Güvenlik ' çözümüne izin vermez.

Fiyatlandırma hakkında daha fazla bilgi için bkz. [Güvenlik Merkezi fiyatlandırması](https://azure.microsoft.com/pricing/details/security-center/).

> [!NOTE]
> Güvenlik Merkezi tarafından oluşturulan çalışma alanlarının Log Analytics fiyatlandırma katmanı, güvenlik merkezi faturalandırmasını etkilemez.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]


## <a name="what-qualifies-a-vm-for-automatic-provisioning-of-the-microsoft-monitoring-agent-installation"></a>Hangi, bir VM'nin Microsoft Monitoring Agent yüklemesi otomatik sağlama için niteliği taşır?

Windows veya Linux Iaas sanal makineleri, uygun:

- Microsoft Monitoring Agent uzantısı VM'de yüklü değil.
- VM'nin çalışır durumda olduğundan.
- Windows veya Linux [Azure sanal makine Aracısı](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) yüklendi.
- VM, web uygulaması güvenlik duvarı veya yeni nesil güvenlik duvarı gibi bir gereç olarak kullanılmaz.


## <a name="can-i-delete-the-default-workspaces-created-by-security-center"></a>Ben, Güvenlik Merkezi tarafından oluşturulan varsayılan çalışma alanlarını silebilir miyim?

**Varsayılan çalışma alanını silme önerilmez.** Güvenlik Merkezi Vm'lerinizi güvenlik verileri depolamak için varsayılan çalışma alanı kullanır. Bir çalışma alanını silerseniz, Güvenlik Merkezi bu verileri ve bazı güvenlik önerilerini toplanacak belirleyemez ve uyarılar kullanılamıyor.

Kurtarmak için Microsoft Monitoring Agent'ı silinmiş çalışma alanına bağlı sanal makineleri kaldırın. Güvenlik Merkezi, aracı yeniden yükler ve yeni varsayılan çalışma alanı oluşturur.

## <a name="how-can-i-use-my-existing-log-analytics-workspace"></a>Mevcut bir Log Analytics çalışma Alanım nasıl kullanabilirim?

Güvenlik Merkezi tarafından toplanan verileri depolamak için mevcut bir Log Analytics çalışma alanını seçebilirsiniz. Mevcut bir Log Analytics çalışma alanınızı kullanmak için:

- Çalışma alanı, seçili Azure aboneliğiniz ile ilişkilendirilmesi gerekir.
- En azından, çalışma alanına erişmek için Okuma izinlerine sahip olmalıdır.

Mevcut bir Log Analytics çalışma alanı seçmek için:

1. **Güvenlik ilkesi – veri koleksiyonu**altında **başka bir çalışma alanı kullan**' ı seçin.

    ![Başka bir çalışma alanı kullan][4]

1. Aşağı açılır menüden, toplanan verileri depolamak için bir çalışma alanı seçin.

    > [!NOTE]
    > Açılır menü, erişimi ve Azure aboneliğinizde olan çalışma alanları gösterilir.

1. **Kaydet**’i seçin. İzlenen VM 'Leri yeniden yapılandırmak isteyip istemediğiniz sorulur.

    - Yeni çalışma alanı ayarlarının **yalnızca yeni VM 'lere uygulanmasını**istiyorsanız **Hayır** ' ı seçin. Yeni çalışma alanı ayarları, yalnızca yeni aracı yüklemelerini için geçerlidir; Microsoft Monitoring Agent yüklüyse olmayan yeni bulunmuş VM'ler.
    - Yeni çalışma alanı ayarlarının **Tüm VM 'lere uygulanmasını**istiyorsanız **Evet** ' i seçin. Ayrıca, çalışma alanı oluşturulduğunda bir güvenlik Merkezi'ne bağlı her bir VM yeni hedef çalışma alanına bağlanır.

    > [!NOTE]
    > **Evet**' i seçerseniz, tüm VM 'ler yeni hedef çalışma alanına yeniden bağlanana kadar Güvenlik Merkezi tarafından oluşturulan çalışma alanlarını silmeyin. Bir çalışma alanı çok erken silinirse bu işlem başarısız olur.

    - İşlemi iptal etmek için **Iptal 'i**seçin.

## Microsoft Monitoring Agent VM 'de zaten bir uzantı olarak yüklüyse ne olacak?<a name="mmaextensioninstalled"></a>

Izleme Aracısı bir uzantı olarak yüklendiğinde, uzantı yapılandırması raporlamaya yalnızca tek bir çalışma alanına izin verir. Güvenlik Merkezi, kullanıcı çalışma alanları için varolan bağlantılar kılmaz. Güvenlik Merkezi, "güvenlik" veya "SecurityCenterFree" çözümünün yüklenmiş olması şartıyla, zaten bağlı olan bir çalışma alanındaki bir VM 'den güvenlik verilerini depolar. Güvenlik Merkezi bu işlemdeki en son sürüme uzantı sürümünü yükseltebilir.

Daha fazla bilgi için bkz. [önceden var olan aracı yüklemesi durumlarında otomatik sağlama](security-center-enable-data-collection.md#preexisting).



## Bir Microsoft Monitoring Agent doğrudan makinede yüklüyse (doğrudan aracı).<a name="directagentinstalled"></a>

Microsoft Monitoring Agent doğrudan VM 'ye (Azure uzantısı olarak değil) yüklenirse, Güvenlik Merkezi Microsoft Monitoring Agent uzantısını yükler ve Microsoft Monitoring Agent 'ı en son sürüme yükseltemez.

Yüklü aracı zaten yapılandırılmış çalışma alanına (ler) raporlama yapmaya devam edecektir ve ayrıca, güvenlik merkezi 'nde yapılandırılan çalışma alanına rapor eder (Windows makinelerde çoklu barındırma desteklenir).

Yapılandırılmış çalışma alanı bir kullanıcı çalışma alanı ise (Güvenlik Merkezi 'nin varsayılan çalışma alanı değil), bu çalışma alanına raporlama yapan VM 'Ler ve bilgisayarlardan gelen olayları işlemeye başlamak için Güvenlik Merkezi 'nin "Security/" SecurityCenterFree "çözümünü yüklemeniz gerekir.

Linux makineler için, aracı çoklu barındırma henüz desteklenmiyor. bu nedenle, mevcut bir aracı yüklemesi algılanırsa, otomatik sağlama gerçekleşmez ve makinenin yapılandırması değiştirilmez.

Aboneliklerdeki mevcut makineler için eklendi Mart 17 2019 ' den önce güvenlik merkezi 'ne, var olan bir aracı algılandığında Microsoft Monitoring Agent uzantısı yüklenmez ve makine etkilenmeyecektir. Bu makineler için, bu makinelerdeki aracı yükleme sorunlarını gidermek için "makinelerinizdeki izleme Aracısı sistem durumu sorunlarını çözme" önerisine bakın

Daha fazla bilgi için, [VM 'imde System Center Operations Manager veya OMS doğrudan Aracısı zaten yüklüyse](#scomomsinstalled) , sonraki bölüme bakın.

## System Center Operations Manager Aracısı VM 'imde zaten yüklüyse ne olur?<a name="scomomsinstalled"></a>

Güvenlik Merkezi, Microsoft Monitoring Agent uzantısını mevcut System Center Operations Manager aracısına yan yana yükleyecek. Mevcut Aracı, System Center Operations Manager sunucusuna normal olarak rapor etmeye devam edecektir. Operations Manager Aracısı ve Microsoft Monitoring Agent, bu işlem sırasında en son sürüme güncellenecek ortak çalışma zamanı kitaplıklarını paylaştığından emin olun. Not-Operations Manager aracısının 2012 sürümü yüklüyse otomatik sağlamayı etkinleştirmeyin (Operations Manager sunucusu da sürüm 2012 olduğunda yönetilebilirlik özellikleri kaybolabilir).


## <a name="what-is-the-impact-of-removing-these-extensions"></a>Bu Uzantılar'ı kaldırmanın etkisi nedir?

Microsoft Monitoring uzantısı kaldırırsanız, Güvenlik Merkezi sanal makine ve bazı güvenlik önerilerini güvenlik verilerini toplamak mümkün değildir ve uyarılar kullanılamıyor. 24 saat içinde Güvenlik Merkezi, VM uzantısı eksik ve uzantıyı yükler belirler.


## <a name="how-do-i-stop-the-automatic-agent-installation-and-workspace-creation"></a>Otomatik aracı yükleme ve çalışma alanı oluşturma nasıl durdururum?

Otomatik aboneliklerinizde güvenlik ilkesinde sağlamayı kapatın kapatabilirsiniz, ancak bu önerilmemektedir. Otomatik sağlama sınırları Güvenlik Merkezi önerilerini ve Uyarıları kapatma. Otomatik sağlamayı devre dışı bırakmak için:

1. Aboneliğiniz standart katman için yapılandırılmışsa, bu abonelik için güvenlik ilkesini açın ve **ücretsiz** katmanı seçin.

   ![Fiyatlandırma katmanı][1]

1. Sonra, **güvenlik ilkesi – veri toplama** sayfasında **kapalı** ' yı seçerek otomatik sağlamayı devre dışı bırakın.
   ![Veri toplama][2]


## <a name="should-i-opt-out-of-the-automatic-agent-installation-and-workspace-creation"></a>Çalışma alanı oluşturma ve otomatik aracı yüklemesi dışında iyileştirilmiş?

> [!NOTE]
> Bölümleri yeniden [kullanıma alma etkileri nelerdir?](#what-are-the-implications-of-opting-out-of-automatic-provisioning) ve otomatik sağlamayı devre dışı bırakmak isterseniz, [Önerilen adımları](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning) gözden geçirdiğinizden emin olun.

Aşağıdakiler sizin için geçerliyse, otomatik sağlama dışında bırakmak isteyebilirsiniz:

- Güvenlik Merkezi tarafından otomatik aracı yüklemesi, tüm abonelik için geçerlidir. Otomatik Yükleme VM'lerin bir alt kümesine uygulanamıyor. Microsoft İzleme Aracısı ile yüklenmiş kritik VM'lerin varsa, otomatik sağlama dışında iyileştirilmiş.
- Microsoft Monitoring Agent (MMA) uzantısının yüklenmesi aracının sürümünü güncelleştirir. Bu, doğrudan aracı ve bir System Center Operations Manager Aracısı için geçerlidir (ikinci içinde, Operations Manager ve MMA paylaşma ortak çalışma zamanı kitaplıklarını, işlem içinde güncelleştirilecektir). Yüklü Operations Manager Aracısı sürüm 2012 ise ve yükseltildiyse, Operations Manager sunucusu da sürüm 2012 olduğunda yönetilebilirlik özellikleri kaybolabilir. Yüklü Operations Manager Aracısı sürüm 2012 ise otomatik sağlamayı tercih edin.
- Abonelik dışında (Merkezi çalışma alanı) özel bir çalışma alanınız varsa, otomatik sağlamayı devre dışı geçirmeniz gerekir. El ile Microsoft Monitoring Agent uzantısını yükleyin ve bağlantıyı geçersiz kılma olmadan Güvenlik Merkezi çalışma alanınızı bağlayın.
- Abonelik başına birden çok çalışma alanı oluşturulmasını önlemek istediğiniz ve kendi özel bir çalışma alanı aboneliği içinde varsa, iki seçeneğiniz vardır:

   1. Otomatik sağlama dışında tercih edebilirsiniz. Geçişten sonra, [var olan Log Analytics çalışma alanım 'ı nasıl kullanabilirim](#how-can-i-use-my-existing-log-analytics-workspace) bölümünde açıklandığı gibi varsayılan çalışma alanı ayarlarını ayarlayın.

   1. Veya, geçiş, sanal makinelere yüklenecek Microsoft Monitoring Agent'ı tamamlamak izin verebilir ve Vm'leri oluşturulan çalışma alanına bağlı. Ardından, önceden yüklenmiş aracıları harcamamak için seçim ile varsayılan çalışma alanı ayarını ayarlayarak kendi özel çalışma alanı seçin. Daha fazla bilgi için bkz. [var olan Log Analytics çalışma alanım 'ı nasıl kullanabilirim?](#how-can-i-use-my-existing-log-analytics-workspace)


## <a name="what-are-the-implications-of-opting-out-of-automatic-provisioning"></a>Otomatik sağlama dışında edilmesiyle etkileri nelerdir?

Geçiş tamamlandığında, güvenlik merkezi VM 'den güvenlik verileri toplayabilir ve bazı güvenlik önerileri ve uyarılar kullanılamaz. Devre dışı bırakırsanız Microsoft Monitoring Agent el ile yükleyebilirsiniz. [Çalışırken önerilen adımlara](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning)bakın.


## <a name="what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning"></a>Otomatik sağlama dışında kabul edilirse, önerilen adımlar nelerdir?

Güvenlik Merkezi 'nin sanal makinelerinizden güvenlik verilerini toplayabilmesi ve öneriler ve uyarılar sağlaması için Microsoft Monitoring Agent uzantısını el ile yükleyebilirsiniz. Yükleme ile ilgili yönergeler için bkz. [WINDOWS VM için aracı yükleme](../virtual-machines/extensions/oms-windows.md) veya [Linux sanal makinesi için aracı yükleme](../virtual-machines/extensions/oms-linux.md) .

Aracı mevcut özel çalışma alanına bağlanabilir veya Güvenlik Merkezi çalışma alanı oluşturulur. Özel bir çalışma alanı etkin 'Güvenlik' veya 'SecurityCenterFree' çözümü yoksa, bir çözüm uygulamak gerekir. Uygulamak için, özel çalışma alanı veya aboneliği seçin ve **güvenlik ilkesi – fiyatlandırma katmanı** sayfası aracılığıyla bir fiyatlandırma katmanı uygulayın.

   ![Fiyatlandırma katmanı][1]

Güvenlik Merkezi seçili fiyatlandırma katmanını temel alan çalışma alanında doğru çözümün olanak sağlar.


## Güvenlik Merkezi tarafından yüklenen OMS uzantıları Nasıl yaparım? kaldırılsın mı?<a name="remove-oms"></a>

Microsoft Monitoring Agent el ile kaldırabilirsiniz. Güvenlik Merkezi önerilerini ve Uyarıları sınırlar bu önerilmez.

> [!NOTE]
> Veri toplama etkinleştirilirse Güvenlik Merkezi kaldırdıktan sonra aracıyı yeniden yükler.  El ile aracı kaldırmadan önce veri toplamayı devre dışı bırakmak gerekir. Otomatik aracı yüklemesini ve çalışma alanı oluşturmayı durdurmak Nasıl yaparım? bakın. veri toplamayı devre dışı bırakma yönergeleri için.

Aracıyı el ile kaldırmak için:

1.  Portalda **Log Analytics**açın.

1.  Log Analytics sayfasında bir çalışma alanı seçin:

1.  İzlemek istemediğiniz VM 'Leri seçin ve **bağlantıyı kes**' i seçin.

   ![Aracıyı kaldır][3]

> [!NOTE]
> Bir Linux VM 'sinde zaten uzantı olmayan OMS Aracısı varsa, uzantının kaldırılması aracıyı de kaldırır ve yeniden yüklemeniz gerekir.


## <a name="how-do-i-disable-data-collection"></a>Veri toplama nasıl devre dışı bırakabilirim?

Otomatik sağlama varsayılan olarak kapalıdır. Otomatik kaynaklardan herhangi bir zamanda bu güvenlik ilkesi ayarı devre dışı bırakarak sağlama devre dışı bırakabilirsiniz. Sistem güncelleştirmeleri, işletim sistemi güvenlik açıkları ve uç nokta koruması hakkında güvenlik uyarıları ve öneriler almak için otomatik sağlama kesinlikle önerilir.

Veri toplamayı devre dışı bırakmak için [Azure Portal oturum açın](https://portal.azure.com), **Araştır**' ı seçin, **Güvenlik Merkezi**' ni seçin ve **ilke Seç**' i seçin. Otomatik sağlamayı hangi abonelik için devre dışı bırakmak istediğinizi belirtin. Bir abonelik **güvenlik ilkesi seçtiğinizde, veri toplama** açılır. **Otomatik sağlama**altında **kapalı**' yı seçin.


## <a name="how-do-i-enable-data-collection"></a>Veri toplama hizmetini nasıl etkinleştirebilirim?

Azure aboneliğinizi güvenlik ilkesinde veri toplamayı etkinleştirebilirsiniz. Veri toplamayı etkinleştirmek için. [Azure Portal oturum açın](https://portal.azure.com), **Araştır**' ı seçin, **Güvenlik Merkezi**' ni seçin ve **güvenlik ilkesi**' ni seçin. Otomatik sağlamayı etkinleştirmek istediğiniz aboneliği seçin. Bir abonelik **güvenlik ilkesi seçtiğinizde, veri toplama** açılır. **Otomatik sağlama**altında **Açık**' ı seçin.


## <a name="what-happens-when-data-collection-is-enabled"></a>Veri toplama etkinleştirilirse ne olur?

Otomatik sağlama etkinleştirildiğinde Güvenlik Merkezi Microsoft Monitoring Agent'ı tüm Azure Vm'lere ve oluşturulan tüm yeni vm'lere desteklenen hazırlar. Otomatik sağlama önerilir, ancak el ile aracı yüklemesi de kullanılabilir. [Microsoft Monitoring Agent uzantısını yüklemeyi öğrenin](../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension). 

Aracı, işlem oluşturma olayı 4688 ve olay 4688 ' nin içindeki *commandLine* alanı ' na izin vermez. VM üzerinde oluşturulan yeni süreçler olay günlüğü tarafından kaydedilir ve Güvenlik Merkezi'nin algılama hizmetleri tarafından izlenen. Her yeni işlem için kaydedilen ayrıntılar hakkında daha fazla bilgi için bkz. [4688 içindeki açıklama alanları](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4688#fields). Aracı ayrıca VM üzerinde oluşturulan 4688 olayları toplar ve bunları Search'te depolar.

Aracı, [Uyarlamalı uygulama denetimleri](security-center-adaptive-application.md)için veri toplamayı da sağlar, Güvenlik Merkezi, tüm uygulamalara izin vermek için denetim modunda yerel bir AppLocker ilkesi yapılandırır. Bu ilke, AppLocker 'ın Güvenlik Merkezi tarafından toplanan ve yararlanılabilir olayları oluşturmasına neden olur. Bu ilke, üzerinde zaten var. yapılandırılmış bir AppLocker İlkesi tüm makinelerde yapılandırılmaz dikkat edin önemlidir. 

Güvenlik Merkezi, VM 'de şüpheli etkinlik algıladığında, [güvenlik iletişim bilgileri](security-center-provide-security-contact-details.md) sağlanmışsa müşteri e-posta ile bildirilir. Bir uyarı da Güvenlik Merkezi'nin güvenlik uyarıları panosunda görünür.


## <a name="will-security-center-work-using-an-oms-gateway"></a>Güvenlik Merkezi, bir OMS ağ geçidi kullanarak çalışacak mı?

Evet. Azure Güvenlik Merkezi, Microsoft Monitoring Agent kullanarak Azure sanal makinelerinden ve sunuculardan veri toplamak için Azure Izleyicisini kullanır.
Verileri toplamak için, her VM ve sunucunun HTTPS kullanarak Internet 'e bağlanması gerekir. Bağlantı, bir proxy kullanılarak veya [OMS ağ geçidi](../azure-monitor/platform/gateway.md)aracılığıyla doğrudan olabilir.


## <a name="does-the-monitoring-agent-impact-the-performance-of-my-servers"></a>İzleme Aracısı Sunucularım performansı etkiler mi?

Aracı, sistem kaynaklarının nominal bir miktarını kullanıyor ve performans üzerinde çok az bir etkiye sahip değildir. Performans etkisi ve aracı ve uzantı hakkında daha fazla bilgi için bkz. [planlama ve işlemler Kılavuzu](security-center-planning-and-operations-guide.md#data-collection-and-storage).


## <a name="where-is-my-data-stored"></a>Verilerim nerede depolanır?

Bu Aracıdan toplanan veriler, aboneliğinizle ilişkili mevcut bir Log Analytics çalışma alanı veya yeni bir çalışma alanı içinde depolanır. Daha fazla bilgi için bkz. [veri güvenliği](security-center-data-security.md).


<!--Image references-->
[1]: ./media/security-center-platform-migration-faq/pricing-tier.png
[2]: ./media/security-center-platform-migration-faq/data-collection.png
[3]: ./media/security-center-platform-migration-faq/remove-the-agent.png
[4]: ./media/security-center-platform-migration-faq/use-another-workspace.png