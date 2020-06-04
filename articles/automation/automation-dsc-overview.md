---
title: Azure Otomasyonu durum yapılandırmasına genel bakış
description: Bu makalede, Azure Otomasyonu durum yapılandırmasına genel bakış sunulmaktadır.
keywords: PowerShell DSC, istenen durum yapılandırması, PowerShell DSC Azure
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.date: 06/03/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d2109baf077b1b4c1074cfae9edd0d2b5ef5030d
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84343012"
---
# <a name="azure-automation-state-configuration-overview"></a>Azure Otomasyonu durum yapılandırmasına genel bakış

Azure Otomasyonu durum yapılandırması, herhangi bir bulutta veya şirket içi veri merkezinde bulunan düğümler için PowerShell Istenen durum yapılandırması (DSC) [yapılandırmalarını](/powershell/scripting/dsc/configurations/configurations) yazmanızı, yönetmenizi ve derlemenize olanak tanıyan bir Azure yapılandırma yönetim hizmetidir. Hizmet ayrıca [DSC kaynaklarını](/powershell/scripting/dsc/resources/resources)içeri aktarır ve tüm bulutta, hedef düğümlere yapılandırma atar. **Yapılandırma yönetimi**altında **Durum Yapılandırması (DSC)** seçeneğini belirleyerek Azure Portal Azure Automation durum yapılandırmasına erişebilirsiniz. 

Çeşitli makineleri yönetmek için Azure Otomasyonu durum yapılandırmasını kullanabilirsiniz:

- Azure sanal makineleri
- Azure sanal makineleri (klasik)
- Şirket içinde veya Azure dışındaki bir bulutta bulunan fiziksel/sanal Windows makineleri (AWS EC2 örnekleri dahil)
- Şirket içinde, Azure 'da veya Azure dışındaki bir bulutta bulunan fiziksel/sanal Linux makineleri

Makine yapılandırmasını buluttan yönetmeye hazırsanız, Azure Otomasyonu durum yapılandırması 'nı yalnızca rapor uç noktası olarak kullanabilirsiniz. Bu özellik, konfigürasyonları DSC aracılığıyla ayarlamanıza ve Azure Otomasyonu 'nda raporlama ayrıntılarını görüntülemenize olanak sağlar.

> [!NOTE]
> Azure Otomasyonu durum yapılandırması ile Azure VM 'Leri yönetmek, yüklü Azure VM Istenen durum yapılandırma uzantısı sürümü 2,70 ' den büyükse ek ücret alınmaz. Daha fazla bilgi için bkz. [**Automation fiyatlandırma sayfası**](https://azure.microsoft.com/pricing/details/automation/).

## <a name="why-use-azure-automation-state-configuration"></a>Azure Otomasyonu durum yapılandırması neden kullanılmalıdır?

Azure Otomasyonu durum yapılandırması, Azure dışında DSC 'nin kullanımıyla çeşitli avantajlar sağlar. Bu hizmet, merkezi bir güvenli konumdan binlerce makine genelinde hızla ve kolayca ölçeklenebilirlik sunar. Makineleri kolayca etkinleştirebilir, bildirime dayalı yapılandırmalara atayabilir ve her makinenin, belirttiğiniz istenen durumla uyumluluğunu gösteren raporları görüntüleyebilirsiniz.

Azure Otomasyonu durum yapılandırma hizmeti, Azure Otomasyonu runbook 'larının PowerShell komut dosyasına ne kadar olduğunu DSC 'ye göre yapılır. Diğer bir deyişle, Azure Otomasyonu 'nun PowerShell betiklerini yönetmenize yardımcı olduğu şekilde, DSC yapılandırmalarının yönetilmesine de yardımcı olur. 

### <a name="built-in-pull-server"></a>Yerleşik çekme sunucusu

Azure Otomasyonu durum yapılandırması, [Windows ÖZELLIĞI DSC hizmeti](/powershell/scripting/dsc/pull-server/pullserver)'ne benzer bir DSC çekme sunucusu sağlar. Hedef düğümler otomatik olarak yapılandırma alabilir, istenen duruma uyum sağlayabilir ve uyumluluklarını rapor edebilir. Azure Otomasyonu 'ndaki yerleşik çekme sunucusu, kendi çekme sunucunuzu ayarlama ve sürdürme gereksinimini ortadan kaldırır. Azure Otomasyonu, sanal veya fiziksel Windows veya Linux makinelerini bulutta veya şirket içinde hedefleyebilir.

### <a name="management-of-all-your-dsc-artifacts"></a>Tüm DSC yapıtlarınızın yönetimi

Azure Otomasyonu durum yapılandırması, PowerShell komut dosyası oluşturma özelliği için [Istenen durum yapılandırması](/powershell/scripting/dsc/overview/overview) için aynı yönetim katmanını getirir. Azure portal veya PowerShell 'den, tüm DSC yapılandırmalarının, kaynaklarınızın ve hedef düğümlerinizi yönetebilirsiniz.

![Azure Otomasyonu sayfasının ekran görüntüsü](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-of-reporting-data-into-azure-monitor-logs"></a>Raporlama verilerini Azure Izleyici günlüklerine aktarma

Azure Otomasyonu durum yapılandırması ile yönetilen düğümler, yerleşik çekme sunucusuna ayrıntılı raporlama durum verileri gönderir. Azure Otomasyonu durum yapılandırması 'nı, bu verileri Log Analytics çalışma alanınıza gönderecek şekilde yapılandırabilirsiniz. Bkz. [Azure Otomasyonu durum yapılandırması raporlama verilerini Azure izleyici günlüklerine iletme](automation-dsc-diagnostics.md).

## <a name="prerequisites-for-using-azure-automation-state-configuration"></a>Azure Otomasyonu durum yapılandırması 'nı kullanma önkoşulları

Azure Otomasyonu durum yapılandırması 'nı kullanırken bu bölümdeki gereksinimleri göz önünde bulundurun.

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

Azure 'da çalışan tüm Windows düğümleri için, makineler etkinleştirildiğinde [WMF 5,1](https://docs.microsoft.com/powershell/scripting/wmf/setup/install-configure) yüklenir. Windows Server 2012 ve Windows 7 çalıştıran düğümler için [WinRM](https://docs.microsoft.com/powershell/scripting/dsc/troubleshooting/troubleshooting#winrm-dependency) etkinleştirilmiştir.

Azure 'da çalışan tüm Linux düğümleri için, makineler etkinleştirildiğinde [Linux Için POWERSHELL DSC](https://github.com/Microsoft/PowerShell-DSC-for-Linux) yüklenir.

### <a name="configuration-of-private-networks"></a><a name="network-planning"></a>Özel ağların yapılandırması

Düğümleriniz özel bir ağda bulunuyorsa, aşağıdaki bağlantı noktası ve URL 'Ler gereklidir. Bu kaynaklar yönetilen düğüm için ağ bağlantısı sağlar ve DSC 'nin Azure Otomasyonu ile iletişim kurmasına izin verir.

* Bağlantı noktası: giden internet erişimi için yalnızca TCP 443 gerekir
* Genel URL: ***. Azure-Automation.net**
* US Gov Virginia genel URL 'SI: ***. Azure-Automation.us**
* Aracı hizmeti: **https:// \<workspaceId\> . Agentsvc.Azure-Automation.net**

[Waitfor * kaynakları](https://docs.microsoft.com/powershell/scripting/dsc/reference/resources/windows/waitForAllResource)gibi düğümler arasında ILETIŞIM kuran DSC kaynakları kullanıyorsanız, düğümler arasında trafiğe de izin vermeniz gerekir. Bu ağ gereksinimlerini anlamak için her DSC kaynağına yönelik belgelere bakın.

TLS 1,2 için istemci gereksinimlerini anlamak üzere bkz. [Azure Otomasyonu Için tls 1,2 zorlaması](automation-managing-data.md#tls-12-enforcement-for-azure-automation).

#### <a name="proxy-support"></a>Proxy desteği

DSC aracısına yönelik ara sunucu desteği Windows sürüm 1809 ve sonrasında kullanılabilir. Bu seçenek, `ProxyURL` `ProxyCredential` düğümleri kaydetmek için kullanılan [metaconfiguration betiğinin](automation-dsc-onboarding.md#generate-dsc-metaconfigurations) ve için değerleri ayarlanarak etkinleştirilir. 

>[!NOTE]
>Azure Otomasyonu durum yapılandırması, önceki Windows sürümleri için DSC proxy desteği sağlamaz.

Linux düğümleri için DSC Aracısı ara sunucuyu destekler ve `http_proxy` URL 'yi belirlemede bu değişkeni kullanır. Ara sunucu desteği hakkında daha fazla bilgi edinmek için bkz. [DSC metaconfigurations oluşturma](automation-dsc-onboarding.md#generate-dsc-metaconfigurations).

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

- Başlamak için bkz. [Azure Otomasyonu durum yapılandırmasını kullanmaya başlama](automation-dsc-getting-started.md).
- Düğümleri nasıl etkinleştireceğinizi öğrenmek için bkz. [Azure Otomasyonu durum yapılandırmasını etkinleştirme](automation-dsc-onboarding.md).
- Hedef düğümlere atayabilmeniz için DSC yapılandırmalarını derleme hakkında bilgi edinmek için bkz. [Azure Otomasyonu durum YAPıLANDıRMASıNDA DSC yapılandırmalarını derleme](automation-dsc-compile.md).
- Azure Otomasyonu durum yapılandırması 'nı sürekli bir dağıtım ardışık düzeninde kullanmaya ilişkin bir örnek görmek için bkz. [Chocolatey ile sürekli dağıtımı ayarlama](automation-dsc-cd-chocolatey.md).
- Fiyatlandırma bilgileri için bkz. [Azure Otomasyonu durum yapılandırması fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/).
- PowerShell cmdlet başvurusu için bkz. [az. Automation](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
).