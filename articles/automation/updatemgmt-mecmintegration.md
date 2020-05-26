---
title: Azure Otomasyonu Güncelleştirme Yönetimi Windows uç noktası ile tümleştirme Configuration Manager
description: Bu makalede, yönetici istemcilerine yazılım güncelleştirmeleri dağıtmak için Güncelleştirme Yönetimi ile Microsoft uç noktası Configuration Manager nasıl yapılandırılacağı açıklanır.
services: automation
ms.subservice: update-management
author: mgoedtel
ms.author: magoedte
ms.date: 12/11/2019
ms.topic: conceptual
ms.openlocfilehash: 183189253d11638751e1f8283b202f122131b005
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83836320"
---
# <a name="integrate-update-management-with-windows-endpoint-configuration-manager"></a>Windows uç noktası ile Güncelleştirme Yönetimi tümleştirme Configuration Manager

Bilgisayarları, sunucuları ve mobil cihazları yönetmek için Microsoft uç nokta Configuration Manager yatırılmış müşteriler, yazılım güncelleştirme yönetimi (SUM) döngüsünün bir parçası olarak yazılım güncelleştirmelerini yönetme gücünden ve vadede yararlanır.

Windows uç nokta Configuration Manager ' de yazılım güncelleştirme dağıtımları oluşturup hazırlama ve [güncelleştirme yönetimi](automation-update-management.md)kullanarak Tamamlanan güncelleştirme dağıtımlarının ayrıntılı durumunu almak yoluyla, yönetilen Windows sunucularını rapor edebilir ve güncelleştirebilirsiniz. Güncelleştirme uyumluluğu raporlaması için Windows uç nokta Configuration Manager kullanıyorsanız, ancak Windows sunucularınızla güncelleştirme dağıtımlarını yönetmek için, güvenlik güncelleştirmeleri Azure Otomasyonu Güncelleştirme Yönetimi ile yönetilirken Configuration Manager 'a raporlamaya devam edebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Otomasyon hesabınıza [Azure otomasyonu güncelleştirme yönetimi](automation-update-management.md) eklemiş olmanız gerekir.
* Windows uç nokta Configuration Manager ortamınız tarafından şu anda yönetilen Windows Server 'lar, ayrıca Güncelleştirme Yönetimi etkinleştirilmiş olan Log Analytics çalışma alanına rapor etmeniz gerekir.
* Bu özellik, geçerli dal sürümü 1606 ve üzeri Configuration Manager Windows uç noktasında etkindir. Windows uç noktası Configuration Manager merkezi yönetim sitesini veya tek başına birincil siteyi Azure Izleyici günlükleri ve içeri aktarma koleksiyonlarıyla bütünleştirmek için, [Azure izleyici günlüklerine bağlan Configuration Manager](../azure-monitor/platform/collect-sccm.md)inceleyin.  
* Windows Agents, Windows Server Update Services (WSUS) sunucusu ile iletişim kurmak veya Windows uç nokta Configuration Manager Güvenlik güncelleştirmelerini almadıklarında Microsoft Update erişimi sağlamak için yapılandırılmış olmalıdır.

Azure IaaS 'de barındırılan istemcileri mevcut Windows uç noktanızla Configuration Manager ortamı öncelikle, Azure veri merkezleri ve altyapınız arasındaki bağlantıya bağlıdır. Bu bağlantı, bu gerekli değişiklikleri desteklemek için Windows uç noktanıza Configuration Manager altyapınızda ve ilgili maliyette yapmanız gerekebilecek tüm tasarım değişikliklerini etkiler. Devam etmeden önce değerlendirmeniz gereken planlama konularını anlamak için, [Azure’da Configuration Manager - Sık Sorulan Sorular](https://docs.microsoft.com/configmgr/core/understand/configuration-manager-on-azure#networking)’ı gözden geçirin.

## <a name="manage-software-updates-from-windows-endpoint-configuration-manager"></a>Windows uç nokta Configuration Manager yazılım güncelleştirmelerini yönetme

Windows uç nokta Configuration Manager güncelleştirme dağıtımlarını yönetmeye devam edecekseniz aşağıdaki adımları gerçekleştirin. Azure Otomasyonu, Log Analytics çalışma alanınıza bağlı istemci bilgisayarlara güncelleştirmeleri uygulamak için Windows uç noktası Configuration Manager bağlanır. Dağıtım Windows uç nokta Configuration Manager tarafından yönetilmmiş gibi, istemci bilgisayar önbelleğinde güncelleştirme içeriği kullanılabilir.

1. [Yazılım güncelleştirmelerini dağıtma](https://docs.microsoft.com/configmgr/sum/deploy-use/deploy-software-updates)bölümünde açıklanan Işlemi kullanarak Windows uç nokta Configuration Manager hiyerarşinizdeki en üst düzey siteden yazılım güncelleştirme dağıtımı oluşturun. Standart bir dağıtımdan farklı şekilde yapılandırılması gereken tek ayar, dağıtım paketinin indirme davranışını denetlemeye yönelik **Yazılım güncelleştirmelerini yükleme** seçeneğidir. Bu davranış, bir sonraki adımda zamanlanmış bir güncelleştirme dağıtımı oluşturularak Güncelleştirme Yönetimi yönetilir.

1. Azure Otomasyonu 'nda **güncelleştirme yönetimi**' yi seçin. [Güncelleştirme dağıtımı oluşturma](automation-tutorial-update-management.md#schedule-an-update-deployment) bölümünde açıklanan adımları izleyerek yeni bir dağıtım oluşturun ve uygun Windows uç noktası Configuration Manager koleksiyonunu seçmek için **tür** açılan menüsünde **içeri aktarılan gruplar** ' ı seçin. Aşağıdaki önemli noktaları göz önünde bulundurun: a. Seçilen Windows uç noktası Configuration Manager cihaz koleksiyonunda bir bakım penceresi tanımlanmışsa, koleksiyonun üyeleri zamanlanan dağıtımda tanımlanan **süre** ayarı yerine bunu kabul ediyor.
    b. Hedef koleksiyonun üyelerinin Internet bağlantısı olması gerekir (doğrudan, bir proxy sunucusu veya Log Analytics ağ geçidi üzerinden).

Azure Otomasyonu aracılığıyla güncelleştirme dağıtımını tamamladıktan sonra, seçilen bilgisayar grubunun üyesi olan hedef bilgisayarlar, güncelleştirmeleri yerel istemci önbelleğinden zamanlanan saatte yükler. Dağıtımınızın sonuçlarını izlemek için [güncelleştirme dağıtım durumunu görüntüleyebilirsiniz](automation-tutorial-update-management.md#view-results-of-an-update-deployment).

## <a name="manage-software-updates-from-azure-automation"></a>Azure Otomasyonu 'ndan yazılım güncelleştirmelerini yönetme

Windows uç nokta Configuration Manager istemcileri olan Windows Server VM 'lerinin güncelleştirmelerini yönetmek için istemci ilkesini, Güncelleştirme Yönetimi tarafından yönetilen tüm istemciler için Yazılım Güncelleştirmesi Yönetimi özelliğini devre dışı bırakacak şekilde yapılandırmanız gerekir. Varsayılan olarak, istemci ayarları hiyerarşideki tüm cihazları hedefler. Bu ilke ayarı ve nasıl yapılandırılacağı hakkında daha fazla bilgi için [Configuration Manager istemci ayarlarını yapılandırma](https://docs.microsoft.com/configmgr/core/clients/deploy/configure-client-settings)konusunu gözden geçirin.

Bu yapılandırma değişikliğini gerçekleştirdikten sonra, uygun Windows uç noktası Configuration Manager koleksiyonunu seçmek için, [güncelleştirme dağıtımı oluşturma](automation-tutorial-update-management.md#schedule-an-update-deployment) bölümünde açıklanan adımları izleyerek yeni bir dağıtım oluşturun ve **tür** açılır penceresinde **içeri aktarılan gruplar** ' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Tümleştirme zamanlaması ayarlamak için bkz. [güncelleştirme dağıtımı zamanlama](automation-tutorial-update-management.md#schedule-an-update-deployment).