---
title: Azure Güvenlik Merkezi SSS - veri toplama ve aracılar
description: Tehditleri önlemenize, algılamanıza ve yanıt vermeye yardımcı olan Azure Güvenlik Merkezi için veri toplama, aracılar ve çalışma alanları hakkında sık sorulan sorular
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
ms.openlocfilehash: 0dbad1a94479430426dae47df7ca3a3ecd9dc980
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80436191"
---
# <a name="faq---questions-about-data-collection-agents-and-workspaces"></a>SSS - Veri toplama, aracılar ve çalışma alanları ile ilgili sorular

Güvenlik Merkezi, güvenlik açıklarını ve tehditlerini izlemek için Azure sanal makinelerinizden (VM'ler), Sanal makine ölçek kümeleri, IaaS kapsayıcıları ve Azure olmayan bilgisayarlardan (şirket içi makineler dahil) veri toplar. Veriler, makineden çeşitli güvenlikle ilgili yapılandırmaları ve olay günlüklerini okuyan ve verileri analiz için çalışma alanınıza kopyalayan Log Analytics aracısı kullanılarak toplanır.


## <a name="am-i-billed-for-azure-monitor-logs-on-the-workspaces-created-by-security-center"></a>Güvenlik Merkezi tarafından oluşturulan çalışma alanlarındaki Azure Monitör günlükleri için faturalandırıldım mı?

Hayır. Güvenlik Merkezi tarafından oluşturulan çalışma alanları, düğüm faturalama başına Azure Monitor günlükleri için yapılandırılırken, Azure Monitor günlük ücretlerine tabi değildir. Güvenlik Merkezi faturalandırması her zaman Güvenlik Merkezi güvenlik politikanıza ve bir çalışma alanına yüklenen çözümleri temel adatır:

- **Ücretsiz katman** – Security Center varsayılan çalışma alanında 'SecurityCenterFree' çözümsağlar. Ücretsiz katman için faturalandırılmazsınız.

- **Standart katman** – Güvenlik Merkezi varsayılan çalışma alanında 'Güvenlik' çözüme sahiptir.

Fiyatlandırma hakkında daha fazla bilgi için [Güvenlik Merkezi fiyatlandırması'na](https://azure.microsoft.com/pricing/details/security-center/)bakın.

> [!NOTE]
> Güvenlik Merkezi tarafından oluşturulan çalışma alanlarının günlük analizi fiyatlandırma katmanı Güvenlik Merkezi faturalandırmasını etkilemez.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]


## <a name="what-qualifies-a-vm-for-automatic-provisioning-of-the-log-analytics-agent-installation"></a>Log Analytics aracısı yüklemesinin otomatik olarak sağlanması için VM'yi nitelendiren nedir?

Windows veya Linux IaaS VM'ler şunları göz etmek durumunda geçerli

- Log Analytics aracı uzantısı şu anda VM'de yüklü değil.
- VM çalışıyor.
- Windows veya Linux [Azure Sanal Makine Aracısı](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) yüklenir.
- VM, web uygulama güvenlik duvarı veya yeni nesil güvenlik duvarı gibi bir cihaz olarak kullanılmaz.


## <a name="can-i-delete-the-default-workspaces-created-by-security-center"></a>Güvenlik Merkezi tarafından oluşturulan varsayılan çalışma alanlarını silebilir miyim?

**Varsayılan çalışma alanının silmesi önerilmez.** Güvenlik Merkezi, Sanal M'lerinizdeki güvenlik verilerini depolamak için varsayılan çalışma alanlarını kullanır. Bir çalışma alanını silerseniz, Güvenlik Merkezi bu verileri toplayamaz ve bazı güvenlik önerileri ve uyarıları kullanılamaz.

Kurtarmak için, silinen çalışma alanına bağlı VM'lerde Log Analytics aracısını kaldırın. Güvenlik Merkezi aracıyı yeniden yükler ve yeni varsayılan çalışma alanları oluşturur.

## <a name="how-can-i-use-my-existing-log-analytics-workspace"></a>Varolan Log Analytics çalışma alanımı nasıl kullanabilirim?

Security Center tarafından toplanan verileri depolamak için varolan bir Log Analytics çalışma alanını seçebilirsiniz. Mevcut Log Analytics çalışma alanınızı kullanmak için:

- Çalışma alanı, seçtiğiniz Azure aboneliğiyle ilişkilendirilmelidir.
- En azından çalışma alanına erişmek için izinleri okumuş olmalısınız.

Varolan bir Log Analytics çalışma alanını seçmek için:

1. **Güvenlik ilkesi altında – Veri Toplama**, başka bir çalışma alanı **kullan'ı**seçin.

    ![Başka bir çalışma alanı kullanma][4]

1. Açılan menüden, toplanan verileri depolamak için bir çalışma alanı seçin.

    > [!NOTE]
    > Aşağı çekme menüsünde yalnızca Azure aboneliğinize erişebildiğiniz ve bu abonelikte bulunan çalışma alanları gösterilir.

1. **Kaydet'i**seçin. İzlenen VM'leri yeniden yapılandırmak isteyip istineceğiniz sorulur.

    - Yeni çalışma alanı ayarlarının yalnızca **yeni VM'lere uygulanmasını**istiyorsanız **Hayır'ı** seçin. Yeni çalışma alanı ayarları yalnızca yeni aracı yüklemeleri için geçerlidir; Log Analytics aracısını yüklememiş yeni keşfedilen VM'ler.
    - Yeni çalışma alanı ayarlarının tüm **VM'lere uygulanmasını**istiyorsanız **Evet'i** seçin. Ayrıca, güvenlik merkezi oluşturulan çalışma alanına bağlı her VM, yeni hedef çalışma alanına yeniden bağlanır.

    > [!NOTE]
    > **Evet'i**seçerseniz, tüm VM'ler yeni hedef çalışma alanına yeniden bağlanana kadar Güvenlik Merkezi tarafından oluşturulan çalışma alanlarını silmeyin. Çalışma alanı çok erken silinirse bu işlem başarısız olur.

    - İşlemi iptal etmek için **İptal**et'i seçin.

## <a name="what-if-the-log-analytics-agent-was-already-installed-as-an-extension-on-the-vm"></a>Log Analytics aracısı VM'de uzantı olarak zaten yüklüyse ne olur?<a name="mmaextensioninstalled"></a>

İzleme Aracısı uzantı olarak yüklendiğinde, uzantı yapılandırması raporlamanın yalnızca tek bir çalışma alanına yapılmasına olanak tanır. Güvenlik Merkezi, kullanıcı çalışma alanlarına varolan bağlantıları geçersiz kılmaz. Güvenlik Merkezi, "Security" veya "SecurityCenterFree" çözümyüklemesi koşuluyla, zaten bağlı olan bir çalışma alanında bir VM'den gelen güvenlik verilerini depolar. Güvenlik Merkezi, bu işlemde uzantı sürümünü en son sürüme yükseltebilir.

Daha fazla bilgi için, [önceden varolan bir aracı yükleme durumlarında Otomatik sağlama](security-center-enable-data-collection.md#preexisting)bölümüne bakın.



## <a name="what-if-a-log-analytics-agent-is-directly-installed-on-the-machine-but-not-as-an-extension-direct-agent"></a>Log Analytics aracısı doğrudan makineye yüklenirken uzantı (Direct Agent) olarak yüklenmezse ne olur?<a name="directagentinstalled"></a>

Log Analytics aracısı doğrudan VM'ye yüklenirse (Azure uzantısı olarak değil), Güvenlik Merkezi Log Analytics aracısı uzantısını yükler ve Log Analytics aracısını en son sürüme yükseltebilir.

Yüklü aracı, zaten yapılandırılmış çalışma alanına(lar) rapor vermeye devam eder ve ek olarak Güvenlik Merkezi'nde yapılandırılan çalışma alanına rapor verecektir (Çoklu homing Windows makinelerinde desteklenir).

Yapılandırılan çalışma alanı bir kullanıcı çalışma alanıysa (Güvenlik Merkezi'nin varsayılan çalışma alanı değilse), Güvenlik Merkezi'nin bu çalışma alanına rapor veren VM'lerden ve bilgisayarlardan olayları işlemeye başlaması için "Security/"SecurityCenterFree" çözümlerini yüklemeniz gerekir.

Linux makineleri için, Agent multi-homing henüz desteklenmez - dolayısıyla, mevcut bir aracı yüklemesi algılanırsa, otomatik sağlama oluşmaz ve makinenin yapılandırması değiştirilmez.

Mevcut bir aracının algılanacağı 17 Mart 2019'dan önce Güvenlik Merkezi'ne giden aboneliklerde mevcut makineler için Log Analytics aracı sıyüklenmez ve makine etkilenmez. Bu makineler için, bu makinelerdeki aracı yükleme sorunlarını çözmek için "Makinelerinizdeki izleme aracısı sağlık sorunlarını çöz" önerisine bakın

Daha fazla bilgi için, bir sonraki bölüme bakın [Bir Sistem Merkezi Operasyon Yöneticisi veya OMS doğrudan aracısı zaten VM](#scomomsinstalled) yüklü yse ne olur?

## <a name="what-if-a-system-center-operations-manager-agent-is-already-installed-on-my-vm"></a>VM'me zaten bir Sistem Merkezi Operasyon Yöneticisi aracısı yüklenmişse ne olur?<a name="scomomsinstalled"></a>

Güvenlik merkezi, Log Analytics aracıuzantısını mevcut System Center Operations Manager aracısına yan yana yükler. Varolan aracı, Sistem Merkezi Operasyon Yöneticisi sunucusuna normal olarak rapor vermeye devam eder. Operations Manager aracısı ve Log Analytics aracısının, bu işlem sırasında en son sürüme güncellenecek olan ortak çalışma zamanı kitaplıklarını paylaştığını unutmayın. Not - Operations Manager aracısının 2012 sürümü yüklenirse, otomatik sağlamayı açmayın (Operations Manager sunucusu da sürüm 2012 olduğunda yönetilebilirlik yetenekleri kaybedilebilir).


## <a name="what-is-the-impact-of-removing-these-extensions"></a>Bu uzantıları kaldırmanın etkisi nedir?

Microsoft İzleme Uzantısı'nı kaldırırsanız, Güvenlik Merkezi VM'den güvenlik verileri toplamıyor ve bazı güvenlik önerileri ve uyarıları kullanılamıyor. 24 saat içinde, Güvenlik Merkezi VM uzantısı eksik olduğunu belirler ve uzantısı yeniden yükler.


## <a name="how-do-i-stop-the-automatic-agent-installation-and-workspace-creation"></a>Otomatik aracı yüklemesini ve çalışma alanı oluşturmayı nasıl durduracağım?

Güvenlik ilkesinde abonelikleriniz için otomatik sağlamayı kapatabilirsiniz, ancak bu önerilmez. Otomatik sağlamanın kapatılıyor, Güvenlik Merkezi önerilerini ve uyarılarını sınırlar. Otomatik sağlamayı devre dışı kalmak için:

1. Aboneliğiniz Standart katman için yapılandırıldıysa, bu aboneliğin güvenlik ilkesini açın ve **Ücretsiz** katmanı seçin.

   ![Fiyatlandırma katmanı][1]

1. Ardından, **Güvenlik ilkesini** - Veri toplama sayfasında **Kapat'ı** seçerek otomatik sağlamayı kapatın.
   ![Veri toplama][2]


## <a name="should-i-opt-out-of-the-automatic-agent-installation-and-workspace-creation"></a>Otomatik aracı yüklemesini ve çalışma alanı oluşturmayı devre dışı bırakmalı mıyım?

> [!NOTE]
> Bölümleri gözden geçirdiğinizden emin olun Devre dışı [recommended steps when opting out](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning) [bırakmanın etkileri nelerdir?](#what-are-the-implications-of-opting-out-of-automatic-provisioning)

Aşağıdakiler sizin için geçerliyse, otomatik sağlamayı devre dışı bırakmak isteyebilirsiniz:

- Güvenlik Merkezi tarafından otomatik aracı yükleme tüm abonelik için geçerlidir. VM'lerin bir alt kümesine otomatik yükleme uygulayamazsınız. Log Analytics aracısıyla yüklenemeyen kritik VM'ler varsa, otomatik sağlamayı devre dışı bırakmanız gerekir.
- Log Analytics aracısının yüklenmesi aracının sürümünü güncelleştirir. Bu, doğrudan bir aracı ve Sistem Merkezi Operasyon Yöneticisi aracısı için geçerlidir (ikincisinde, Operasyon Yöneticisi ve Log Analytics aracısı işlemsırasında güncellenecek olan ortak çalışma zamanı kitaplıklarını paylaşır). Yüklü Operations Manager aracısı sürüm 2012 ise ve yükseltilirse, Operations Manager sunucusu da sürüm 2012 olduğunda yönetilebilirlik yetenekleri kaybedilebilir. Yüklü Operations Manager aracısı sürüm 2012 ise otomatik sağlama yı devre dışı bırakmayı düşünün.
- Aboneliğin dışında özel bir çalışma alanınız (merkezi leştirilmiş bir çalışma alanı) varsa, otomatik sağlamayı devre dışı bırakmanız gerekir. Log Analytics aracısı uzantısını el ile yükleyebilir ve Güvenlik Merkezi bağlantıyı geçersiz kılmadan çalışma alanınızı bağlayabilirsiniz.
- Abonelik başına birden çok çalışma alanı oluşturulmasını önlemek istiyorsanız ve abonelik içinde kendi özel çalışma alanınız varsa, iki seçeneğiniz vardır:

   1. Otomatik sağlamayı devre dışı kullanabilirsiniz. Geçişten sonra, varsayılan çalışma alanı ayarlarını mevcut [Log Analytics çalışma alanımı nasıl kullanabilirim?'da](#how-can-i-use-my-existing-log-analytics-workspace) açıklandığı şekilde ayarlayın?

   1. Veya geçişin tamamlanmasına, Log Analytics aracısının VM'lere ve oluşturulan çalışma alanına bağlı VM'lere yüklenmesine izin verebilirsiniz. Ardından, varsayılan çalışma alanı ayarını, zaten yüklü olan aracıları yeniden yapılandırmayı seçerek kendi özel çalışma alanınızı seçin. Daha fazla bilgi için [bkz.](#how-can-i-use-my-existing-log-analytics-workspace)


## <a name="what-are-the-implications-of-opting-out-of-automatic-provisioning"></a>Otomatik sağlamadan vazgeçmenin sonuçları nelerdir?

Geçiş tamamlandığında, Güvenlik Merkezi VM'den güvenlik verileri toplamıyor ve bazı güvenlik önerileri ve uyarıları kullanılamıyor. Devre dışı kalırsanız, Log Analytics aracısını el ile yükleyin. [Devre dışı kalırken önerilen adımlara](#what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning)bakın.


## <a name="what-are-the-recommended-steps-when-opting-out-of-automatic-provisioning"></a>Otomatik sağlamayı devre dışı bırakmak için önerilen adımlar nelerdir?

Güvenlik Merkezi'nin Sanal M'lerinizden güvenlik verileri toplayabilmesi ve öneriler ve uyarılar sunabilmesi için Log Analytics aracısı uzantısını el ile yükleyin. Yükleme kılavuzu için Windows VM veya [Linux VM için aracı yüklemesi](../virtual-machines/extensions/oms-linux.md) için aracı yüklemesi'ne bakın. [agent installation for Windows VM](../virtual-machines/extensions/oms-windows.md)

Aracıyı varolan herhangi bir özel çalışma alanına veya Güvenlik Merkezi oluşturulan çalışma alanına bağlayabilirsiniz. Özel bir çalışma alanında 'Güvenlik' veya 'SecurityCenterFree' çözümleri etkin değilse, bir çözüm uygulamanız gerekir. Başvurmak için özel çalışma alanını veya aboneliği seçin ve **Güvenlik ilkesi – Fiyatlandırma katmanı** sayfası üzerinden bir fiyatlandırma katmanı uygulayın.

   ![Fiyatlandırma katmanı][1]

Güvenlik Merkezi, seçili fiyatlandırma katmanını temel alan çalışma alanında doğru çözümü etkinleştirecektir.


## <a name="how-do-i-remove-oms-extensions-installed-by-security-center"></a>Güvenlik Merkezi tarafından yüklenen OMS uzantılarını nasıl kaldırırım?<a name="remove-oms"></a>

Log Analytics aracısını el ile kaldırabilirsiniz. Güvenlik Merkezi önerilerini ve uyarılarını sınırlamadıkça bu önerilmez.

> [!NOTE]
> Veri toplama etkinleştirilmişse, Güvenlik Merkezi aracıyı kaldırdıktan sonra yeniden yükler.  Aracıyı el ile kaldırmadan önce veri toplamayı devre dışı kaldırmanız gerekir. Bkz. Otomatik aracı yüklemesini ve çalışma alanı oluşturmayı nasıl durduracağım? veri toplama devre dışı bırakma talimatları için.

Aracıyı el ile kaldırmak için:

1.    **Portalda, Log Analytics'i**açın.

1.    Günlük Analitiği sayfasında bir çalışma alanı seçin:

1.    İzlemek istemediğiniz VM'leri seçin ve **Bağlantıyı Kesme'yi**seçin.

   ![Aracıyı kaldırma][3]

> [!NOTE]
> Bir Linux VM'de zaten uzantılı olmayan bir OMS aracısı varsa, uzantıyı kaldırmak aracıyı da kaldırır ve yeniden yüklemeniz gerekir.


## <a name="how-do-i-disable-data-collection"></a>Veri toplamayı nasıl devre dışı atarım?

Otomatik sağlama varsayılan olarak kapalıdır. Güvenlik ilkesindeki bu ayarı kapatarak istediğiniz zaman kaynaklardan otomatik sağlamayı devre dışı bırakabilirsiniz. Sistem güncelleştirmeleri, işletim sistemi güvenlik açıkları ve uç nokta koruması hakkında güvenlik uyarıları ve öneriler almak için otomatik sağlama önerilir.

Veri toplamayı devre dışı katmak için [Azure portalında oturum açın](https://portal.azure.com), **Gözat' ı**seçin, Güvenlik **Merkezi'ni**seçin ve **politikayı seçin.** Otomatik sağlamayı hangi abonelik için devre dışı bırakmak istediğinizi belirtin. Bir abonelik Güvenlik ilkesi seçtiğinizde **- Veri toplama** açılır. **Otomatik sağlama**altında, **Kapalı**seçin.


## <a name="how-do-i-enable-data-collection"></a>Veri toplamayı nasıl etkinleştirebilirim?

Güvenlik ilkesinde Azure aboneliğiniz için veri toplamayı etkinleştirebilirsiniz. Veri toplamayı etkinleştirmek için. [Azure portalında oturum açın,](https://portal.azure.com) **Gözat'ı**seçin, **Güvenlik Merkezi'ni**seçin ve **Güvenlik ilkesini**seçin. Otomatik sağlamayı etkinleştirmek istediğiniz aboneliği seçin. Bir abonelik Güvenlik ilkesi seçtiğinizde **- Veri toplama** açılır. **Otomatik sağlama**altında, **On'u**seçin.


## <a name="what-happens-when-data-collection-is-enabled"></a>Veri toplama etkinleştirildiğinde ne olur?

Otomatik sağlama etkinleştirildiğinde, Güvenlik Merkezi Log Analytics aracısını desteklenen tüm Azure VM'lerinde ve oluşturulan yeni leri etkinleştirirken. Otomatik sağlama önerilir, ancak manuel aracı yükleme de mevcuttur. [Log Analytics aracı uzantısını nasıl yükleyin](../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)öğrenin. 

Aracı, işlem oluşturma olayı 4688'i ve 4688 olayının içindeki *CommandLine* alanını etkinleştiriyor. VM'de oluşturulan yeni süreçler EventLog tarafından kaydedilir ve Güvenlik Merkezi'nin algılama hizmetleri tarafından izlenir. Her yeni işlem için kaydedilen ayrıntılar hakkında daha fazla bilgi [için, 4688'deki açıklama alanlarına](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4688#fields)bakın. Aracı ayrıca VM'de oluşturulan 4688 olayı toplar ve bunları arama da saklar.

Aracı ayrıca [Uyarlanabilir Uygulama Denetimleri](security-center-adaptive-application.md)için veri toplama sağlar, Güvenlik Merkezi tüm uygulamalara izin vermek için Denetim modunda yerel bir AppLocker ilkesi yapılandırır. Bu ilke, AppLocker'ın daha sonra Güvenlik Merkezi tarafından toplanan ve yararlanılan olaylar oluşturmasına neden olur. Bu ilkenin, zaten yapılandırılmış bir AppLocker ilkesi nin bulunduğu makinelerde yapılandırılmadığını unutmayın. 

Güvenlik Merkezi VM'de şüpheli bir etkinlik algıladığında, [güvenlik iletişim bilgileri](security-center-provide-security-contact-details.md) sağlanmışsa müşterie e-posta ile bildirilir. Güvenlik Merkezi'nin güvenlik uyarıları panosunda da bir uyarı görülebilir.


## <a name="will-security-center-work-using-an-oms-gateway"></a>Güvenlik Merkezi Bir OMS ağ geçidi kullanarak çalışır mı?

Evet. Azure Güvenlik Merkezi, Log Analytics aracısını kullanarak Azure VM'lerden ve sunuculardan veri toplamak için Azure Monitor'dan yararlanır.
Verileri toplamak için her VM ve sunucunun HTTPS kullanarak Internet'e bağlanması gerekir. Bağlantı, proxy kullanarak veya [OMS Ağ Geçidi](../azure-monitor/platform/gateway.md)üzerinden doğrudan olabilir.


## <a name="does-the-monitoring-agent-impact-the-performance-of-my-servers"></a>İzleme Aracısı sunucularımın performansını etkiler mi?

Aracı, nominal miktarda sistem kaynağı tüketir ve performans üzerinde çok az etkisi olmalıdır. Performans etkisi ve aracı ve uzantı hakkında daha fazla bilgi için [planlama ve operasyon kılavuzuna](security-center-planning-and-operations-guide.md#data-collection-and-storage)bakın.


## <a name="where-is-my-data-stored"></a>Verilerim nerede depolanır?

Bu aracıdan toplanan veriler, aboneliğinizle ilişkili varolan bir Log Analytics çalışma alanında veya yeni bir çalışma alanında depolanır. Daha fazla bilgi için [Bkz. Veri Güvenliği.](security-center-data-security.md)


<!--Image references-->
[1]: ./media/security-center-platform-migration-faq/pricing-tier.png
[2]: ./media/security-center-platform-migration-faq/data-collection.png
[3]: ./media/security-center-platform-migration-faq/remove-the-agent.png
[4]: ./media/security-center-platform-migration-faq/use-another-workspace.png
