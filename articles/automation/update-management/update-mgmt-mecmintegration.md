---
title: Azure Otomasyonu Güncelleştirme Yönetimi Windows uç noktası ile tümleştirme Configuration Manager
description: Bu makalede, yönetici istemcilerine yazılım güncelleştirmeleri dağıtmak için Güncelleştirme Yönetimi ile Microsoft uç noktası Configuration Manager nasıl yapılandırılacağı açıklanır.
services: automation
ms.subservice: update-management
ms.date: 07/28/2020
ms.topic: conceptual
ms.openlocfilehash: 4eccd53fbf3b883d6e3ba6d2c7c80ddd4ae188af
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87450480"
---
# <a name="integrate-update-management-with-windows-endpoint-configuration-manager"></a>Windows uç noktası ile Güncelleştirme Yönetimi tümleştirme Configuration Manager

Bilgisayarları, sunucuları ve mobil cihazları yönetmek için Microsoft uç nokta Configuration Manager yatırılmış müşteriler, yazılım güncelleştirme yönetimi (SUM) döngüsünün bir parçası olarak yazılım güncelleştirmelerini yönetme gücünden ve vadede yararlanır.

Windows uç nokta Configuration Manager ' de yazılım güncelleştirme dağıtımları oluşturup hazırlama ve [güncelleştirme yönetimi](update-mgmt-overview.md)kullanarak Tamamlanan güncelleştirme dağıtımlarının ayrıntılı durumunu almak yoluyla, yönetilen Windows sunucularını rapor edebilir ve güncelleştirebilirsiniz. Güncelleştirme uyumluluğu raporlaması için Windows uç nokta Configuration Manager kullanıyorsanız, ancak Windows sunucularınızla güncelleştirme dağıtımlarını yönetmek için, güvenlik güncelleştirmeleri Azure Otomasyonu Güncelleştirme Yönetimi ile yönetilirken uç nokta Configuration Manager raporlamaya devam edebilirsiniz.

>[!NOTE]
>Güncelleştirme Yönetimi, Windows Server 2008 R2 'nin güncelleştirme değerlendirmesini ve düzeltme eki uygulamayı desteklese de, bu işletim sistemini çalıştıran Configuration Manager uç nokta tarafından yönetilen istemcileri desteklemez.

## <a name="prerequisites"></a>Ön koşullar

* Otomasyon hesabınıza [Azure otomasyonu güncelleştirme yönetimi](update-mgmt-overview.md) eklemiş olmanız gerekir.
* Windows uç nokta Configuration Manager ortamınız tarafından şu anda yönetilen Windows Server 'lar, ayrıca Güncelleştirme Yönetimi etkinleştirilmiş olan Log Analytics çalışma alanına rapor etmeniz gerekir.
* Bu özellik, geçerli dal sürümü 1606 ve üzeri Configuration Manager Windows uç noktasında etkindir. Windows uç noktası Configuration Manager merkezi yönetim sitesini veya tek başına birincil siteyi Azure Izleyici günlükleri ve içeri aktarma koleksiyonlarıyla bütünleştirmek için, [Azure izleyici günlüklerine bağlan Configuration Manager](../../azure-monitor/platform/collect-sccm.md)inceleyin.  
* Windows Agents, Windows Server Update Services (WSUS) sunucusu ile iletişim kurmak veya Windows uç nokta Configuration Manager Güvenlik güncelleştirmelerini almadıklarında Microsoft Update erişimi sağlamak için yapılandırılmış olmalıdır.

Azure IaaS 'de barındırılan istemcileri mevcut Windows uç noktanızla Configuration Manager ortamı öncelikle, Azure veri merkezleri ve altyapınız arasındaki bağlantıya bağlıdır. Bu bağlantı, bu gerekli değişiklikleri desteklemek için Windows uç noktanıza Configuration Manager altyapınızda ve ilgili maliyette yapmanız gerekebilecek tüm tasarım değişikliklerini etkiler. Devam etmeden önce değerlendirmeniz gereken planlama konularını anlamak için, [Azure’da Configuration Manager - Sık Sorulan Sorular](/configmgr/core/understand/configuration-manager-on-azure#networking)’ı gözden geçirin.

## <a name="manage-software-updates-from-windows-endpoint-configuration-manager"></a>Windows uç nokta Configuration Manager yazılım güncelleştirmelerini yönetme

Windows uç nokta Configuration Manager güncelleştirme dağıtımlarını yönetmeye devam edecekseniz aşağıdaki adımları gerçekleştirin. Azure Otomasyonu, Log Analytics çalışma alanınıza bağlı istemci bilgisayarlara güncelleştirmeleri uygulamak için Windows uç noktası Configuration Manager bağlanır. Dağıtım Windows uç nokta Configuration Manager tarafından yönetilmmiş gibi, istemci bilgisayar önbelleğinde güncelleştirme içeriği kullanılabilir.

1. [Yazılım güncelleştirmelerini dağıtma](/configmgr/sum/deploy-use/deploy-software-updates)bölümünde açıklanan Işlemi kullanarak Windows uç nokta Configuration Manager hiyerarşinizdeki en üst düzey siteden yazılım güncelleştirme dağıtımı oluşturun. Standart bir dağıtımdan farklı şekilde yapılandırılması gereken tek ayar, dağıtım paketinin indirme davranışını denetlemeye yönelik **Yazılım güncelleştirmelerini yükleme** seçeneğidir. Bu davranış, bir sonraki adımda zamanlanmış bir güncelleştirme dağıtımı oluşturularak Güncelleştirme Yönetimi yönetilir.

2. Azure Otomasyonu 'nda **güncelleştirme yönetimi**' yi seçin. [Güncelleştirme dağıtımı oluşturma](update-mgmt-deploy-updates.md#schedule-an-update-deployment) bölümünde açıklanan adımları izleyerek yeni bir dağıtım oluşturun ve uygun Windows uç noktası Configuration Manager koleksiyonunu seçmek için **tür** açılan menüsünde **içeri aktarılan gruplar** ' ı seçin. Aşağıdaki önemli noktaları göz önünde bulundurun:

    a. Seçilen Windows uç noktası Configuration Manager cihaz koleksiyonunda bir bakım penceresi tanımlanmışsa, koleksiyonun üyeleri zamanlanan dağıtımda tanımlanan **süre** ayarı yerine bunu kabul ediyor.

    b. Hedef koleksiyonun üyelerinin Internet bağlantısı olması gerekir (doğrudan, bir proxy sunucusu veya Log Analytics ağ geçidi üzerinden).

Azure Otomasyonu aracılığıyla güncelleştirme dağıtımını tamamladıktan sonra, seçilen bilgisayar grubunun üyesi olan hedef bilgisayarlar, güncelleştirmeleri yerel istemci önbelleğinden zamanlanan saatte yükler. Dağıtımınızın sonuçlarını izlemek için [güncelleştirme dağıtım durumunu görüntüleyebilirsiniz](update-mgmt-deploy-updates.md#check-deployment-status).

## <a name="manage-software-updates-from-azure-automation"></a>Azure Otomasyonu 'ndan yazılım güncelleştirmelerini yönetme

Windows uç nokta Configuration Manager istemcileri olan Windows Server VM 'lerinin güncelleştirmelerini yönetmek için istemci ilkesini, Güncelleştirme Yönetimi tarafından yönetilen tüm istemciler için Yazılım Güncelleştirmesi Yönetimi özelliğini devre dışı bırakacak şekilde yapılandırmanız gerekir. Varsayılan olarak, istemci ayarları hiyerarşideki tüm cihazları hedefler. Bu ilke ayarı ve nasıl yapılandırılacağı hakkında daha fazla bilgi için [Configuration Manager istemci ayarlarını yapılandırma](/configmgr/core/clients/deploy/configure-client-settings)konusunu gözden geçirin.

Bu yapılandırma değişikliğini gerçekleştirdikten sonra, uygun Windows uç noktası Configuration Manager koleksiyonunu seçmek için, [güncelleştirme dağıtımı oluşturma](update-mgmt-deploy-updates.md#schedule-an-update-deployment) bölümünde açıklanan adımları izleyerek yeni bir dağıtım oluşturun ve **tür** açılır penceresinde **içeri aktarılan gruplar** ' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Tümleştirme zamanlaması ayarlamak için bkz. [güncelleştirme dağıtımı zamanlama](update-mgmt-deploy-updates.md#schedule-an-update-deployment).
