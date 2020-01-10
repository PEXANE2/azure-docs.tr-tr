---
title: Azure Güncelleştirme Yönetimi Configuration Manager istemcilerle kullanma
description: Bu makale, ConfigMgr istemcilerine yazılım güncelleştirmeleri dağıtmak için Microsoft uç nokta Configuration Manager 'yi bu çözümle yapılandırmanıza yardımcı olmaya yöneliktir.
services: automation
ms.subservice: update-management
ms.date: 03/19/2018
ms.topic: conceptual
ms.openlocfilehash: 9df401ec9c6d11bfef5d1d60833c855029f8ca01
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75769957"
---
# <a name="deploy-updates-to-microsoft-endpoint-configuration-manager-clients-with-update-management"></a>Güncelleştirmeleri Güncelleştirme Yönetimi Microsoft uç noktası Configuration Manager istemcilere dağıtma

Bilgisayarları, sunucuları ve mobil cihazları yönetmek için Microsoft uç nokta Configuration Manager yatırılmış müşteriler, yazılım güncelleştirme yönetimi (SUM) döngüsünün bir parçası olarak yazılım güncelleştirmelerini yönetme gücünden ve vadede yararlanır.

Rapor ve oluşturma ve önceden yazılım güncelleştirme dağıtımlarını Configuration Manager'ı hazırlama tarafından yönetilen Windows sunucularını güncelleştirme ve kullanarak tamamlanmış güncelleştirme dağıtımlarının ayrıntılı durumunu alın [güncelleştirme yönetimi çözümü](automation-update-management.md). Güncelleştirme uyumluluğu raporlaması için Configuration Manager, ancak Windows sunucularınızla güncelleştirme dağıtımlarını yönetmek için kullanıyorsanız, güvenlik güncelleştirmeleri Güncelleştirme Yönetimi çözümüyle yönetilirken Configuration Manager raporlamaya devam edebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Olmalıdır [güncelleştirme yönetimi çözümünü](automation-update-management.md) Otomasyon hesabınıza eklendi.
* Configuration Manager ortamınız tarafından şu anda yönetilen Windows sunucularının Ayrıca Güncelleştirme Yönetimi çözümü etkinleştirilmiş olan Log Analytics çalışma alanına raporlama yapması gerekir.
* Bu özellik geçerli Configuration Manager dalı sürümü 1606 ve üzeri sürümlerde etkinleştirilmiştir. Configuration Manager merkezi yönetim sitenizi veya bağımsız bir birincil siteyi Azure Izleyici günlükleri ve içeri aktarma koleksiyonlarıyla bütünleştirmek için, [Azure izleyici günlüklerine bağlan Configuration Manager](../azure-monitor/platform/collect-sccm.md)inceleyin.  
* Windows aracıları Configuration Manager’dan güvenlik güncelleştirmeleri almazsa Windows Server Update Services (WSUS) sunucusuyla iletişim kuracak veya Microsoft Update’e erişecek şekilde yapılandırılmış olmalıdır.   

Azure IaaS içinde barındırılan istemcileri mevcut Configuration Manager ortamınızla nasıl yönettiğiniz birincil olarak Azure veri merkezleri ile altyapınız arasında mevcut olan bağlantıya bağlıdır. Bu bağlantı, Configuration Manager altyapısında yapmanız gereken her türlü tasarım değişikliğini ve bu değişiklikleri desteklemeyle ilgili maliyetleri etkiler. Devam etmeden önce değerlendirmeniz gereken planlama konularını anlamak için, [Azure’da Configuration Manager - Sık Sorulan Sorular](/sccm/core/understand/configuration-manager-on-azure#networking)’ı gözden geçirin.

## <a name="configuration"></a>Yapılandırma

### <a name="manage-software-updates-from-configuration-manager"></a>Configuration Manager’dan yazılım güncelleştirmelerini yönetme 

Güncelleştirme dağıtımlarını Configuration Manager’dan yönetmeye devam edecekseniz aşağıdaki adımları gerçekleştirin. Azure Otomasyonu, Log Analytics çalışma alanınıza bağlı istemci bilgisayarlara güncelleştirmeleri uygulamak için Configuration Manager'için bağlanır. Güncelleştirme içeriği, dağıtım Configuration Manager’dan yönetiliyormuş gibi istemci bilgisayar önbelleğinden alınabilir.

1. Configuration Manager hiyerarşinizde en üst düzeydeki siteden bir yazılım güncelleştirme dağıtımı oluşturmak için [yazılım güncelleştirme işlemini dağıtma](/sccm/sum/deploy-use/deploy-software-updates) bölümünde açıklanan işlemi kullanın. Standart bir dağıtımdan farklı şekilde yapılandırılması gereken tek ayar, dağıtım paketinin indirme davranışını denetlemeye yönelik **Yazılım güncelleştirmelerini yükleme** seçeneğidir. Bu davranış, sonraki adımda zamanlanmış bir güncelleştirme dağıtımı oluşturarak güncelleştirme yönetimi çözümü tarafından yönetilir.

1. Azure Otomasyonu'nda seçin **güncelleştirme yönetimi**. İçinde açıklanan adımları izleyerek yeni bir dağıtımını oluşturun [güncelleştirme dağıtımı oluşturma](automation-tutorial-update-management.md#schedule-an-update-deployment) seçip **içe gruplar** üzerinde **türü** uygun seçmek için açılır Configuration Manager koleksiyonu. Aşağıdaki önemli noktalara dikkat edin: bir. Seçili Configuration Manager cihaz koleksiyonunda bir bakım penceresi tanımlanmışsa, koleksiyonun üyeleri yerine bunu dikkate **süresi** zamanlanmış dağıtımda tanımlanan ayarı.
    b. Hedef koleksiyonun üyeleri (doğrudan Log Analytics ağ geçidi veya Ara sunucu aracılığıyla) Internet bağlantısı olması gerekir.

Azure Otomasyonu ile güncelleştirme dağıtımını tamamladıktan sonra seçili bilgisayar grubunun üyesi olan hedef bilgisayarlar zamanlanan saatte güncelleştirmeleri kendi yerel istemci önbelleğine yükler. Dağıtımınızın sonuçlarını izlemek için [güncelleştirme dağıtım durumunu görüntüleyebilirsiniz](automation-tutorial-update-management.md#view-results-of-an-update-deployment).

### <a name="manage-software-updates-from-azure-automation"></a>Azure Otomasyonu yazılım güncelleştirmelerini yönetme

Configuration Manager istemcisi olan Windows Server VM’lerinin güncelleştirmelerini yönetmek için, istemci ilkesini bu çözüm tarafından yönetilen tüm istemcilere ait Yazılım Güncelleştirme Yönetimi özelliğini devre dışı bırakacak şekilde yapılandırmanız gerekir. Varsayılan olarak, istemci ayarları hiyerarşideki tüm cihazları hedefler. Bu ilke ayarı ve nasıl yapılandırılacağı hakkında daha fazla bilgi için [System Center Configuration Manager'da istemci ayarlarını yapılandırma](/sccm/core/clients/deploy/configure-client-settings) makalesini inceleyin.

Bu yapılandırma değişikliğini uyguladıktan sonra açıklanan adımları izleyerek yeni bir dağıtım oluşturma [güncelleştirme dağıtımı oluşturma](automation-tutorial-update-management.md#schedule-an-update-deployment) seçip **içe gruplar** üzerinde **türü** uygun Configuration Manager koleksiyonunu seçmek için açılır.

## <a name="next-steps"></a>Sonraki adımlar

