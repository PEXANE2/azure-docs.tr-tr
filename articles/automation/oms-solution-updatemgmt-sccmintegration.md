---
title: Configuration Manager istemcileriyle Azure Güncelleştirme Yönetimi'ni kullanma
description: Bu makale, ConfigMgr istemcilerine yazılım güncelleştirmelerini dağıtmak için microsoft Endpoint Configuration Manager'ı bu çözümle yapılandırmanıza yardımcı olmak için tasarlanmıştır.
services: automation
ms.subservice: update-management
author: mgoedtel
ms.author: magoedte
ms.date: 12/11/2019
ms.topic: conceptual
ms.openlocfilehash: f0ca836e3b53c3cce755d45b50fe168073f0bbaa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76513142"
---
# <a name="deploy-updates-to-microsoft-endpoint-configuration-manager-clients-with-update-management"></a>Güncelleştirme Yönetimi ile güncelleştirmeleri Microsoft Endpoint Configuration Manager istemcilerine dağıtma

Bilgisayarları, sunucuları ve mobil cihazları yönetmek için Microsoft Endpoint Configuration Manager'a yatırım yapmış olan müşteriler, yazılım güncelleştirme yönetimi (SUM) döngüsünün bir parçası olarak yazılım güncelleştirmelerini yönetmedeki gücüne ve olgunluğuna da güvenir.

Yönetilen Windows sunucularını Configuration Manager'da yazılım güncelleştirme dağıtımları oluşturarak ve önceden evreleyerek raporlayabilir ve güncelleyebilir ve [Update Management çözümünün](automation-update-management.md)kullanımıyla tamamlanan güncelleştirme dağıtımlarının ayrıntılı durumunu alabilirsiniz. Yapılandırma Yöneticisi'ni güncelleştirme uyumluluk raporlaması için kullanıyorsanız ancak Windows sunucularınızla güncelleştirme dağıtımlarını yönetmek için değil, güvenlik güncelleştirmeleri Güncelleştirme Yönetimi çözümüyle yönetilirken Configuration Manager'a raporlamaya devam edebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Otomasyon hesabınıza [Güncelleme Yönetimi çözümünüzü](automation-update-management.md) n için eklemeniz gerekir.
* Şu anda Configuration Manager ortamınız tarafından yönetilen Windows sunucularının, Güncelleştirme Yönetimi çözümünüzü de etkinleştiren Log Analytics çalışma alanına rapor sunması gerekir.
* Bu özellik Configuration Manager geçerli şube sürümü 1606 ve daha yüksek etkinleştirilir. Configuration Manager merkezi yönetim sitenizi veya bağımsız bir birincil siteyi Azure Monitor günlükleri ve içe aktarma koleksiyonlarıyla tümleştirmek [için Configuration Manager'ı Azure Monitor günlüklerine bağlayın' ı](../azure-monitor/platform/collect-sccm.md)gözden geçirin.  
* Windows aracıları Configuration Manager’dan güvenlik güncelleştirmeleri almazsa Windows Server Update Services (WSUS) sunucusuyla iletişim kuracak veya Microsoft Update’e erişecek şekilde yapılandırılmış olmalıdır.   

Azure IaaS içinde barındırılan istemcileri mevcut Configuration Manager ortamınızla nasıl yönettiğiniz birincil olarak Azure veri merkezleri ile altyapınız arasında mevcut olan bağlantıya bağlıdır. Bu bağlantı, Configuration Manager altyapısında yapmanız gereken her türlü tasarım değişikliğini ve bu değişiklikleri desteklemeyle ilgili maliyetleri etkiler. Devam etmeden önce değerlendirmeniz gereken planlama konularını anlamak için, [Azure’da Configuration Manager - Sık Sorulan Sorular](https://docs.microsoft.com/configmgr/core/understand/configuration-manager-on-azure#networking)’ı gözden geçirin.

## <a name="configuration"></a>Yapılandırma

### <a name="manage-software-updates-from-configuration-manager"></a>Configuration Manager’dan yazılım güncelleştirmelerini yönetme 

Güncelleştirme dağıtımlarını Configuration Manager’dan yönetmeye devam edecekseniz aşağıdaki adımları gerçekleştirin. Azure Otomasyonu, Log Analytics çalışma alanınıza bağlı istemci bilgisayarlara güncelleştirmeleri uygulamak için Configuration Manager'a bağlanır. Güncelleştirme içeriği, dağıtım Configuration Manager’dan yönetiliyormuş gibi istemci bilgisayar önbelleğinden alınabilir.

1. Yazılım güncelleştirmelerini dağıt'ta açıklanan işlemi kullanarak Configuration Manager hiyerarşinizdeki üst düzey siteden bir [yazılım güncelleştirmesi](https://docs.microsoft.com/configmgr/sum/deploy-use/deploy-software-updates)dağıtımı oluşturun. Standart bir dağıtımdan farklı şekilde yapılandırılması gereken tek ayar, dağıtım paketinin indirme davranışını denetlemeye yönelik **Yazılım güncelleştirmelerini yükleme** seçeneğidir. Bu davranış, bir sonraki adımda zamanlanmış bir güncelleştirme dağıtımı oluşturarak Güncelleştirme Yönetimi çözümü tarafından yönetilir.

1. Azure Otomasyonu'nda **Yönetim'i Güncelleştir'i**seçin. [Güncelleştirme Dağıtımı Oluşturma'da](automation-tutorial-update-management.md#schedule-an-update-deployment) açıklanan adımları izleyerek yeni bir dağıtım oluşturun ve uygun Configuration Manager koleksiyonunu seçmek için **Tür** açılır bölümünde **İçe Aktarılan grupları** seçin. Aşağıdaki önemli noktaları unutmayın: a. Seçili Configuration Manager aygıt koleksiyonunda bir bakım penceresi tanımlanırsa, zamanlanan dağıtımda tanımlanan **Süre** ayarı yerine koleksiyon üyeleri bunu onurlandırır.
    b. Hedef koleksiyonun üyelerinin Internet'e bağlantısı olmalıdır (doğrudan, bir proxy sunucusu aracılığıyla veya Log Analytics ağ geçidi aracılığıyla).

Azure Otomasyonu üzerinden güncelleştirme dağıtımını tamamladıktan sonra, seçili bilgisayar grubunun üyesi olan hedef bilgisayarlar güncelleştirmeleri yerel istemci önbelleğinden zamanlanan zamanda yükler. Dağıtımınızın sonuçlarını izlemek için [güncelleştirme dağıtım durumunu görüntüleyebilirsiniz](automation-tutorial-update-management.md#view-results-of-an-update-deployment).

### <a name="manage-software-updates-from-azure-automation"></a>Azure Otomasyonu'ndan yazılım güncelleştirmelerini yönetme

Configuration Manager istemcisi olan Windows Server VM’lerinin güncelleştirmelerini yönetmek için, istemci ilkesini bu çözüm tarafından yönetilen tüm istemcilere ait Yazılım Güncelleştirme Yönetimi özelliğini devre dışı bırakacak şekilde yapılandırmanız gerekir. Varsayılan olarak, istemci ayarları hiyerarşideki tüm cihazları hedefler. Bu ilke ayarı ve nasıl yapılandırılabilen ler hakkında daha fazla bilgi için [Configuration Manager'da istemci ayarlarını nasıl yapılandırılatır'](https://docs.microsoft.com/configmgr/core/clients/deploy/configure-client-settings)ı gözden geçirin.

Bu yapılandırma değişikliğini gerçekleştirdikten sonra, [Güncelleştirme Dağıtımı Oluşturma'da](automation-tutorial-update-management.md#schedule-an-update-deployment) açıklanan adımları izleyerek yeni bir dağıtım oluşturur ve uygun Configuration Manager koleksiyonunu seçmek için **Tür** açılır bölümünde **İçe Aktarılan grupları** seçersiniz.

## <a name="next-steps"></a>Sonraki adımlar

