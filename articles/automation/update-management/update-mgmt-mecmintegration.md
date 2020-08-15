---
title: Azure Otomasyonu Güncelleştirme Yönetimi Microsoft uç noktası ile tümleştirme Configuration Manager
description: Bu makalede, yönetici istemcilerine yazılım güncelleştirmeleri dağıtmak için Güncelleştirme Yönetimi ile Microsoft uç noktası Configuration Manager nasıl yapılandırılacağı açıklanır.
services: automation
ms.subservice: update-management
ms.date: 07/28/2020
ms.topic: conceptual
ms.openlocfilehash: 09c8ef818428157c7de8c3a87bcce8a7b80e62ea
ms.sourcegitcommit: c293217e2d829b752771dab52b96529a5442a190
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2020
ms.locfileid: "88245919"
---
# <a name="integrate-update-management-with-microsoft-endpoint-configuration-manager"></a>Güncelleştirme Yönetimi Microsoft uç noktası ile tümleştirin Configuration Manager

Bilgisayarları, sunucuları ve mobil cihazları yönetmek için Microsoft uç nokta Configuration Manager yatırılmış müşteriler, yazılım güncelleştirme yönetimi (SUM) döngüsünün bir parçası olarak yazılım güncelleştirmelerini yönetme gücünden ve vadede yararlanır.

Microsoft uç nokta Configuration Manager ' de yazılım güncelleştirme dağıtımlarını oluşturup önceden hazırlama ve [güncelleştirme yönetimi](update-mgmt-overview.md)kullanarak Tamamlanan güncelleştirme dağıtımlarının ayrıntılı durumunu almak yoluyla yönetilen Windows Server raporları ve güncelleştirme yapabilirsiniz. Güncelleştirme uyumluluğu raporlaması için Microsoft uç nokta Configuration Manager kullanıyorsanız, ancak Windows sunucularınızla güncelleştirme dağıtımlarını yönetmek için, güvenlik güncelleştirmeleri Azure Otomasyonu Güncelleştirme Yönetimi ile yönetilirken Microsoft uç noktası Configuration Manager raporlamaya devam edebilirsiniz.

>[!NOTE]
>Güncelleştirme Yönetimi, Windows Server 2008 R2 'nin güncelleştirme değerlendirmesini ve düzeltme eki uygulamayı desteklese de, bu işletim sistemini çalıştıran Configuration Manager Microsoft uç noktası tarafından yönetilen istemcileri desteklemez.

## <a name="prerequisites"></a>Önkoşullar

* Otomasyon hesabınıza [Azure otomasyonu güncelleştirme yönetimi](update-mgmt-overview.md) eklemiş olmanız gerekir.
* Microsoft uç nokta Configuration Manager ortamınız tarafından şu anda yönetilen Windows sunucularının Ayrıca Güncelleştirme Yönetimi etkinleştirilmiş olan Log Analytics çalışma alanına raporlama yapması gerekir.
* Bu özellik, güncel dal sürümü 1606 ve üzeri Configuration Manager Microsoft uç noktasında etkinleştirilmiştir. Microsoft uç nokta Configuration Manager merkezi yönetim sitesini veya tek başına birincil siteyi Azure Izleyici günlükleri ve içeri aktarma koleksiyonlarıyla bütünleştirmek için, [Azure izleyici günlüklerine bağlan Configuration Manager](../../azure-monitor/platform/collect-sccm.md)inceleyin.  
* Windows aracıları, bir Windows Server Update Services (WSUS) sunucusuyla iletişim kuracak veya Microsoft uç nokta Configuration Manager güvenlik güncelleştirmeleri almadıklarında Microsoft Update erişiminizin olması gerekir.

Azure IaaS 'de barındırılan istemcileri mevcut Microsoft uç nokta Configuration Manager ortamı ile yönetme, birincil olarak Azure veri merkezleri ve altyapınız arasındaki bağlantıya bağlıdır. Bu bağlantı, Microsoft uç noktası Configuration Manager altyapınızda yapmanız gerekebilecek tüm tasarım değişikliklerini etkiler ve bu gerekli değişiklikleri desteklemek için ilgili maliyettir. Devam etmeden önce değerlendirmeniz gereken planlama konularını anlamak için, [Azure’da Configuration Manager - Sık Sorulan Sorular](/configmgr/core/understand/configuration-manager-on-azure#networking)’ı gözden geçirin.

## <a name="manage-software-updates-from-microsoft-endpoint-configuration-manager"></a>Microsoft uç nokta Configuration Manager yazılım güncelleştirmelerini yönetme

Microsoft uç nokta Configuration Manager güncelleştirme dağıtımlarını yönetmeye devam edecekseniz aşağıdaki adımları gerçekleştirin. Azure Otomasyonu, Log Analytics çalışma alanınıza bağlı istemci bilgisayarlara güncelleştirmeleri uygulamak için Microsoft uç nokta Configuration Manager bağlanır. Dağıtım Microsoft uç nokta Configuration Manager tarafından yönetilmmiş gibi istemci bilgisayar önbelleğinde güncelleştirme içeriği kullanılabilir.

1. [Yazılım güncelleştirmelerini dağıtma](/configmgr/sum/deploy-use/deploy-software-updates)bölümünde açıklanan Işlemi kullanarak Microsoft uç noktası Configuration Manager hiyerarşinizdeki en üst düzey siteden bir yazılım güncelleştirme dağıtımı oluşturun. Standart bir dağıtımdan farklı şekilde yapılandırılması gereken tek ayar, dağıtım paketinin indirme davranışını denetlemeye yönelik **Yazılım güncelleştirmelerini yükleme** seçeneğidir. Bu davranış, bir sonraki adımda zamanlanmış bir güncelleştirme dağıtımı oluşturularak Güncelleştirme Yönetimi yönetilir.

2. Azure Otomasyonu 'nda **güncelleştirme yönetimi**' yi seçin. [Güncelleştirme dağıtımı oluşturma](update-mgmt-deploy-updates.md#schedule-an-update-deployment) bölümünde açıklanan adımları izleyerek yeni bir dağıtım oluşturun ve uygun Microsoft uç nokta Configuration Manager koleksiyonunu seçmek için **tür** açılan menüsünde **içeri aktarılan gruplar** ' ı seçin. Aşağıdaki önemli noktaları göz önünde bulundurun:

    a. Seçilen Microsoft uç nokta Configuration Manager cihaz koleksiyonunda bir bakım penceresi tanımlanmışsa, koleksiyonun üyeleri zamanlanan dağıtımda tanımlanan **süre** ayarı yerine bunu kabul ediyor.

    b. Hedef koleksiyonun üyelerinin Internet bağlantısı olması gerekir (doğrudan, bir proxy sunucusu veya Log Analytics ağ geçidi üzerinden).

Azure Otomasyonu aracılığıyla güncelleştirme dağıtımını tamamladıktan sonra, seçilen bilgisayar grubunun üyesi olan hedef bilgisayarlar, güncelleştirmeleri yerel istemci önbelleğinden zamanlanan saatte yükler. Dağıtımınızın sonuçlarını izlemek için [güncelleştirme dağıtım durumunu görüntüleyebilirsiniz](update-mgmt-deploy-updates.md#check-deployment-status).

## <a name="manage-software-updates-from-azure-automation"></a>Azure Otomasyonu 'ndan yazılım güncelleştirmelerini yönetme

Microsoft uç nokta Configuration Manager istemcileri olan Windows Server VM 'lerinin güncelleştirmelerini yönetmek için istemci ilkesini, Güncelleştirme Yönetimi tarafından yönetilen tüm istemciler için Yazılım Güncelleştirmesi Yönetimi özelliğini devre dışı bırakacak şekilde yapılandırmanız gerekir. Varsayılan olarak, istemci ayarları hiyerarşideki tüm cihazları hedefler. Bu ilke ayarı ve nasıl yapılandırılacağı hakkında daha fazla bilgi için [Configuration Manager istemci ayarlarını yapılandırma](/configmgr/core/clients/deploy/configure-client-settings)konusunu gözden geçirin.

Bu yapılandırma değişikliğini gerçekleştirdikten sonra, uygun Microsoft uç nokta Configuration Manager koleksiyonunu seçmek için, [güncelleştirme dağıtımı oluşturma](update-mgmt-deploy-updates.md#schedule-an-update-deployment) bölümünde açıklanan adımları izleyerek yeni bir dağıtım oluşturun ve **tür** açılan **grupları** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Tümleştirme zamanlaması ayarlamak için bkz. [güncelleştirme dağıtımı zamanlama](update-mgmt-deploy-updates.md#schedule-an-update-deployment).
