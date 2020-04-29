---
title: Azure Otomasyonu durum yapılandırmasına genel bakış
description: Azure Otomasyonu durum yapılandırması (DSC), koşullarına ve bilinen sorunlara genel bakış
keywords: PowerShell DSC, istenen durum yapılandırması, PowerShell DSC Azure
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.date: 11/06/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: afceb11180662416aa4953b8b58ef03ffaa70eec
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81406175"
---
# <a name="state-configuration-overview"></a>Durum Yapılandırmasına genel bakış

Azure Otomasyonu durum yapılandırması, PowerShell Istenen durum yapılandırması (DSC) [yapılandırmalarını](/powershell/scripting/dsc/configurations/configurations)yazmanıza, yönetmenize ve derlemenize olanak tanıyan bir Azure hizmetidir. Hizmet ayrıca [DSC kaynaklarını](/powershell/scripting/dsc/resources/resources)içeri aktarır ve tüm bulutta, hedef düğümlere yapılandırma atar.

## <a name="why-use-azure-automation-state-configuration"></a>Azure Otomasyonu durum yapılandırması neden kullanılmalıdır?

Azure Otomasyonu durum yapılandırması, Azure dışında DSC kullanmaya kıyasla çeşitli avantajlar sağlar.

### <a name="built-in-pull-server"></a>Yerleşik çekme sunucusu

Azure Otomasyonu durum yapılandırması, [Windows ÖZELLIĞI DSC hizmeti](/powershell/scripting/dsc/pull-server/pullserver)'ne benzer bir DSC çekme sunucusu sağlar. Hedef düğümler otomatik olarak yapılandırma alabilir, istenen duruma uyum sağlayabilir ve uyumluluklarını rapor edebilir. Azure Otomasyonu 'ndaki yerleşik çekme sunucusu, kendi çekme sunucunuzu ayarlama ve sürdürme gereksinimini ortadan kaldırır. Azure Otomasyonu, sanal veya fiziksel Windows veya Linux makinelerini bulutta veya şirket içinde hedefleyebilir.

### <a name="management-of-all-your-dsc-artifacts"></a>Tüm DSC yapıtlarınızın yönetimi

Azure Otomasyonu durum yapılandırması, PowerShell komut dosyası oluşturma özelliği için [Istenen durum yapılandırması](/powershell/scripting/dsc/overview/overview) için aynı yönetim katmanını getirir. Azure portal veya PowerShell 'den, tüm DSC yapılandırmalarının, kaynaklarınızın ve hedef düğümlerinizi yönetebilirsiniz.

![Azure Otomasyonu sayfasının ekran görüntüsü](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-of-reporting-data-into-azure-monitor-logs"></a>Raporlama verilerini Azure Izleyici günlüklerine aktarma

Azure Otomasyonu durum yapılandırması ile yönetilen düğümler, yerleşik çekme sunucusuna ayrıntılı raporlama durum verileri gönderir. Azure Otomasyonu durum yapılandırması 'nı, bu verileri Log Analytics çalışma alanınıza gönderecek şekilde yapılandırabilirsiniz. Bkz. [Azure Otomasyonu durum yapılandırması raporlama verilerini Azure izleyici günlüklerine iletme](automation-dsc-diagnostics.md).

## <a name="prerequisites-for-using-azure-automation-state-configuration"></a>Azure Otomasyonu durum yapılandırması 'nı kullanma önkoşulları

DSC için Azure Otomasyonu durum yapılandırması 'nı kullanırken lütfen aşağıdaki gereksinimleri göz önünde bulundurun.

### <a name="operating-system-requirements"></a>İşletim sistemi gereksinimleri

Windows çalıştıran düğümler için aşağıdaki sürümler desteklenir:

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012R2
- Windows Server 2012
- Windows Server 2008 R2 SP1
- Windows 10
- Windows 8.1
- Windows 7

>[!NOTE]
>[Microsoft Hyper-V Server](/windows-server/virtualization/hyper-v/hyper-v-server-2016) tek başına Ürün SKU 'SU bir DSC uygulamasını içermez. Bu nedenle, PowerShell DSC veya Azure Otomasyonu durum yapılandırması tarafından yönetilemez.

Linux çalıştıran düğümler için DSC Linux uzantısı, [desteklenen Linux dağıtımları](https://github.com/Azure/azure-linux-extensions/tree/master/DSC#4-supported-linux-distributions)altında listelenen tüm Linux dağıtımlarını destekler.

### <a name="dsc-requirements"></a>DSC gereksinimleri

Azure 'da çalışan tüm Windows düğümleri için [WMF 5,1](https://docs.microsoft.com/powershell/scripting/wmf/setup/install-configure) ekleme sırasında yüklenir. Windows Server 2012 ve Windows 7 çalıştıran düğümler için [WinRM](https://docs.microsoft.com/powershell/scripting/dsc/troubleshooting/troubleshooting#winrm-dependency) etkinleştirilmiştir.

Azure 'da çalışan tüm Linux düğümleri için ekleme sırasında [Linux Için POWERSHELL DSC](https://github.com/Microsoft/PowerShell-DSC-for-Linux) yüklenir.

### <a name="configuration-of-private-networks"></a><a name="network-planning"></a>Özel ağların yapılandırması

Düğümleriniz özel bir ağda bulunuyorsa, aşağıdaki bağlantı noktası ve URL 'Ler gereklidir. Bu kaynaklar yönetilen düğüm için ağ bağlantısı sağlar ve DSC 'nin Azure Otomasyonu ile iletişim kurmasına izin verir.

* Bağlantı noktası: giden internet erişimi için yalnızca TCP 443 gerekir
* Genel URL: ***. Azure-Automation.net**
* US Gov Virginia genel URL 'SI: ***. Azure-Automation.us**
* Aracı hizmeti: **https://\<workspace ID\>. Agentsvc.Azure-Automation.net**

[Waitfor * kaynakları](https://docs.microsoft.com/powershell/scripting/dsc/reference/resources/windows/waitForAllResource)gibi düğümler arasında ILETIŞIM kuran DSC kaynakları kullanıyorsanız, düğümler arasında trafiğe de izin vermeniz gerekir. Bu ağ gereksinimlerini anlamak için her DSC kaynağına yönelik belgelere bakın.

#### <a name="proxy-support"></a>Proxy desteği

DSC aracısına yönelik ara sunucu desteği Windows sürüm 1809 ve sonrasında kullanılabilir. Bu seçenek, düğümleri kaydetmek için kullanılan `ProxyURL` [metaconfiguration betiğinde](automation-dsc-onboarding.md#generating-dsc-metaconfigurations) ve `ProxyCredential` için değerleri ayarlanarak etkinleştirilir.

>[!NOTE]
>Azure Otomasyonu durum yapılandırması, önceki Windows sürümleri için DSC proxy desteği sağlamaz.

Linux düğümleri için DSC Aracısı ara sunucuyu destekler ve URL 'YI belirlemede `http_proxy` bu değişkeni kullanır.

#### <a name="azure-automation-state-configuration-network-ranges-and-namespace"></a>Azure Otomasyonu durum yapılandırması ağ aralıkları ve ad alanı

Özel durumları tanımlarken aşağıda listelenen adreslerin kullanılması önerilir. IP adresleri için [Microsoft Azure veri MERKEZI IP aralıklarını](https://www.microsoft.com/download/details.aspx?id=41653)indirebilirsiniz. Bu dosya haftalık olarak güncelleştirilir ve şu anda dağıtılmış aralıklar ve IP aralıklarında yaklaşan değişiklikler vardır.

Belirli bir bölge için tanımlanan bir Otomasyon hesabınız varsa, bu bölgesel veri merkezi ile iletişimi kısıtlayabilirsiniz. Aşağıdaki tabloda her bölge için DNS kaydı verilmiştir:

| **Bölge** | **DNS kaydı** |
| --- | --- |
| Orta Batı ABD | wcus-jobruntimedata-prod-su1.azure-automation.net</br>wcus-agentservice-prod-1.azure-automation.net |
| Orta Güney ABD |scus-jobruntimedata-prod-su1.azure-automation.net</br>scus-agentservice-prod-1.azure-automation.net |
| Doğu ABD    | eus-jobruntimedata-prod-su1.azure-automation.net</br>eus-agentservice-prod-1.azure-automation.net |
| Doğu ABD 2 |eus2-jobruntimedata-prod-su1.azure-automation.net</br>eus2-agentservice-prod-1.azure-automation.net |
| Orta Kanada |cc-jobruntimedata-prod-su1.azure-automation.net</br>cc-agentservice-prod-1.azure-automation.net |
| Batı Avrupa |we-jobruntimedata-prod-su1.azure-automation.net</br>we-agentservice-prod-1.azure-automation.net |
| Kuzey Avrupa |ne-jobruntimedata-prod-su1.azure-automation.net</br>ne-agentservice-prod-1.azure-automation.net |
| Güneydoğu Asya |sea-jobruntimedata-prod-su1.azure-automation.net</br>sea-agentservice-prod-1.azure-automation.net|
| Orta Hindistan |cid-jobruntimedata-prod-su1.azure-automation.net</br>cid-agentservice-prod-1.azure-automation.net |
| Doğu Japonya |jpe-jobruntimedata-prod-su1.azure-automation.net</br>jpe-agentservice-prod-1.azure-automation.net |
| Avustralya Güneydoğu |ase-jobruntimedata-prod-su1.azure-automation.net</br>ase-agentservice-prod-1.azure-automation.net |
| Güney Birleşik Krallık | uks-jobruntimedata-prod-su1.azure-automation.net</br>uks-agentservice-prod-1.azure-automation.net |
| US Gov Virginia | usge-jobruntimedata-prod-su1.azure-automation.us<br>usge-agentservice-prod-1.azure-automation.us |

Bölge adları yerine bölge IP adresleri listesi için, Microsoft Indirme Merkezi ' nden [Azure veri MERKEZI IP adresi](https://www.microsoft.com/download/details.aspx?id=41653) XML dosyasını indirin.

> [!NOTE]
> Azure veri merkezi IP adresi XML dosyası Microsoft Azure veri merkezlerinde kullanılan IP adresi aralıklarını listeler. Dosya işlem, SQL ve depolama aralıklarını içerir.
>
>Güncelleştirilmiş bir dosya haftalık olarak gönderilir. Dosya, şu anda dağıtılmış aralıkları ve IP aralıklarında yaklaşan değişiklikleri yansıtır. Dosyada görüntülenen yeni aralıklar, en az bir hafta için veri merkezlerinde kullanılmıyor. Her hafta yeni XML dosyasını indirmek iyi bir fikirdir. Ardından sitenizi Azure 'da çalışan hizmetleri doğru şekilde belirlemek için güncelleştirin. 

Azure ExpressRoute kullanıcıları bu dosyanın, her ayın ilk haftasında Azure Space Sınır Ağ Geçidi Protokolü (BGP) tanıtımını güncelleştirmek için kullanıldığını unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Otomasyonu durum yapılandırmasında DSC kullanmaya başlamak için bkz. [Azure Automation durum yapılandırması ile çalışmaya](automation-dsc-getting-started.md)başlama.
- Düğümlerin nasıl ekleneceğini öğrenmek için bkz. [Azure Otomasyonu durum yapılandırmasına göre yönetim için makineleri ekleme](automation-dsc-onboarding.md).
- Hedef düğümlere atayabilmeniz için DSC yapılandırmalarını derleme hakkında bilgi edinmek için bkz. [Azure Otomasyonu durum yapılandırmasında yapılandırmaları derleme](automation-dsc-compile.md).
- PowerShell cmdlet başvurusu için bkz. [az. Automation](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
).
- Fiyatlandırma bilgileri için bkz. [Azure Otomasyonu durum yapılandırması fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/).
- Azure Otomasyonu durum yapılandırması 'nı sürekli bir dağıtım ardışık düzeninde kullanmaya ilişkin bir örnek görmek için bkz. [Azure Otomasyonu durum yapılandırması ve Chocolatey kullanarak sürekli dağıtım](automation-dsc-cd-chocolatey.md).
