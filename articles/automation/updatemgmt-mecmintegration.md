---
title: Azure Otomasyonu Güncelleştirme Yönetimi Configuration Manager istemcilerle kullanma
description: Bu makale, ConfigMgr istemcilerine yazılım güncelleştirmeleri dağıtmak için Microsoft uç nokta Configuration Manager 'yi bu çözümle yapılandırmanıza yardımcı olmaya yöneliktir.
services: automation
ms.subservice: update-management
author: mgoedtel
ms.author: magoedte
ms.date: 12/11/2019
ms.topic: conceptual
ms.openlocfilehash: 2dbc33aa56c7e930596ba6806ba1dd2e128e1c82
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2020
ms.locfileid: "82780224"
---
# <a name="deploy-updates-to-microsoft-endpoint-configuration-manager-clients-with-update-management"></a>Güncelleştirmeleri Güncelleştirme Yönetimi Microsoft uç noktası Configuration Manager istemcilere dağıtma

Bilgisayarları, sunucuları ve mobil cihazları yönetmek için Microsoft uç nokta Configuration Manager yatırılmış müşteriler, yazılım güncelleştirme yönetimi (SUM) döngüsünün bir parçası olarak yazılım güncelleştirmelerini yönetme gücünden ve vadede yararlanır.

Configuration Manager ' de yazılım güncelleştirme dağıtımlarını oluşturup önceden hazırlayarak ve [güncelleştirme yönetimi](automation-update-management.md)kullanarak Tamamlanan güncelleştirme dağıtımlarının ayrıntılı durumunu alarak, yönetilen Windows sunucularını rapor edebilir ve güncelleştirebilirsiniz. Güncelleştirme uyumluluğu raporlaması için Configuration Manager, ancak Windows sunucularınız ile güncelleştirme dağıtımlarını yönetmek için kullanıyorsanız, güvenlik güncelleştirmeleri Güncelleştirme Yönetimi yönetilirken Configuration Manager raporlamaya devam edebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Otomasyon hesabınıza [güncelleştirme yönetimi](automation-update-management.md) eklemiş olmanız gerekir.
* Configuration Manager ortamınız tarafından şu anda yönetilen Windows sunucularının Ayrıca Güncelleştirme Yönetimi etkinleştirilmiş olan Log Analytics çalışma alanına raporlama yapması gerekir.
* Bu özellik geçerli Configuration Manager dalı sürümü 1606 ve üzeri sürümlerde etkinleştirilmiştir. Configuration Manager merkezi yönetim sitenizi veya bağımsız bir birincil siteyi Azure Izleyici günlükleri ve içeri aktarma koleksiyonlarıyla bütünleştirmek için, [Azure izleyici günlüklerine bağlan Configuration Manager](../azure-monitor/platform/collect-sccm.md)inceleyin.  
* Windows aracıları Configuration Manager’dan güvenlik güncelleştirmeleri almazsa Windows Server Update Services (WSUS) sunucusuyla iletişim kuracak veya Microsoft Update’e erişecek şekilde yapılandırılmış olmalıdır.

Azure IaaS içinde barındırılan istemcileri mevcut Configuration Manager ortamınızla nasıl yönettiğiniz birincil olarak Azure veri merkezleri ile altyapınız arasında mevcut olan bağlantıya bağlıdır. Bu bağlantı, Configuration Manager altyapısında yapmanız gereken her türlü tasarım değişikliğini ve bu değişiklikleri desteklemeyle ilgili maliyetleri etkiler. Devam etmeden önce değerlendirmeniz gereken planlama konularını anlamak için, [Azure’da Configuration Manager - Sık Sorulan Sorular](https://docs.microsoft.com/configmgr/core/understand/configuration-manager-on-azure#networking)’ı gözden geçirin.

## <a name="configuration"></a>Yapılandırma

### <a name="manage-software-updates-from-configuration-manager"></a>Configuration Manager’dan yazılım güncelleştirmelerini yönetme

Güncelleştirme dağıtımlarını Configuration Manager’dan yönetmeye devam edecekseniz aşağıdaki adımları gerçekleştirin. Azure Otomasyonu, Log Analytics çalışma alanınıza bağlı istemci bilgisayarlara güncelleştirmeleri uygulamak için Configuration Manager bağlanır. Güncelleştirme içeriği, dağıtım Configuration Manager’dan yönetiliyormuş gibi istemci bilgisayar önbelleğinden alınabilir.

1. [Yazılım güncelleştirmelerini dağıtma](https://docs.microsoft.com/configmgr/sum/deploy-use/deploy-software-updates)bölümünde açıklanan işlemi kullanarak Configuration Manager hiyerarşinizdeki en üst düzey siteden bir yazılım güncelleştirme dağıtımı oluşturun. Standart bir dağıtımdan farklı şekilde yapılandırılması gereken tek ayar, dağıtım paketinin indirme davranışını denetlemeye yönelik **Yazılım güncelleştirmelerini yükleme** seçeneğidir. Bu davranış, bir sonraki adımda zamanlanmış bir güncelleştirme dağıtımı oluşturularak Güncelleştirme Yönetimi yönetilir.

1. Azure Otomasyonu 'nda **güncelleştirme yönetimi**' yi seçin. [Güncelleştirme dağıtımı oluşturma](automation-tutorial-update-management.md#schedule-an-update-deployment) bölümünde açıklanan adımları izleyerek yeni bir dağıtım oluşturun ve uygun Configuration Manager koleksiyonunu seçmek için **tür** açılan menüsünde **içeri aktarılan gruplar** ' ı seçin. Aşağıdaki önemli noktaları göz önünde bulundurun: a. Seçilen Configuration Manager cihaz koleksiyonunda bir bakım penceresi tanımlanmışsa, koleksiyonun üyeleri zamanlanan dağıtımda tanımlanan **süre** ayarı yerine bunu kabul ediyor.
    b. Hedef koleksiyonun üyelerinin Internet bağlantısı olması gerekir (doğrudan, bir proxy sunucusu veya Log Analytics ağ geçidi üzerinden).

Azure Otomasyonu aracılığıyla güncelleştirme dağıtımını tamamladıktan sonra, seçilen bilgisayar grubunun üyesi olan hedef bilgisayarlar, güncelleştirmeleri yerel istemci önbelleğinden zamanlanan saatte yükler. Dağıtımınızın sonuçlarını izlemek için [güncelleştirme dağıtım durumunu görüntüleyebilirsiniz](automation-tutorial-update-management.md#view-results-of-an-update-deployment).

### <a name="manage-software-updates-from-azure-automation"></a>Azure Otomasyonu 'ndan yazılım güncelleştirmelerini yönetme

Configuration Manager istemcileri olan Windows Server VM 'lerinin güncelleştirmelerini yönetmek için istemci ilkesini, Güncelleştirme Yönetimi tarafından yönetilen tüm istemciler için Yazılım Güncelleştirmesi Yönetimi özelliğini devre dışı bırakacak şekilde yapılandırmanız gerekir. Varsayılan olarak, istemci ayarları hiyerarşideki tüm cihazları hedefler. Bu ilke ayarı ve nasıl yapılandırılacağı hakkında daha fazla bilgi için [Configuration Manager istemci ayarlarını yapılandırma](https://docs.microsoft.com/configmgr/core/clients/deploy/configure-client-settings)konusunu gözden geçirin.

Bu yapılandırma değişikliğini gerçekleştirdikten sonra, [bir güncelleştirme dağıtımı oluşturma](automation-tutorial-update-management.md#schedule-an-update-deployment) bölümünde açıklanan adımları izleyerek yeni bir dağıtım oluşturursunuz ve uygun Configuration Manager koleksiyonunu seçmek için **tür** açılır penceresinde **içeri aktarılan gruplar** ' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Güncelleştirme dağıtımı oluşturma](automation-tutorial-update-management.md#schedule-an-update-deployment)bölümünde açıklanan adımları izleyerek yeni bir dağıtım oluşturun.