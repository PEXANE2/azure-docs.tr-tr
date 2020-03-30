---
title: Azure Otomasyon Durumu Yapılandırması Genel Bakış
description: Azure Otomasyon Durumu Yapılandırmasına (DSC), terimlerine ve bilinen sorunlara Genel Bakış
keywords: powershell dsc, istenilen durum yapılandırması, powershell dsc azure
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.date: 11/06/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 46cf0d6a12ffbc836db7bd79c0f2738a94e23085
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80283200"
---
# <a name="azure-automation-state-configuration-overview"></a>Azure Otomasyon Durumu Yapılandırması Genel Bakış

Azure Otomasyon Durumu Yapılandırması, PowerShell İstenen Durum Yapılandırması (DSC) [yapılandırmalarını](/powershell/scripting/dsc/configurations/configurations)yazmanıza, yönetmenize ve derlemenize olanak tanıyan bir Azure hizmetidir. Hizmet ayrıca [DSC Kaynaklarını](/powershell/scripting/dsc/resources/resources)da içeri zabıta eder ve tümü bulutta hedef düğümlere yapılandırmalar atar.

## <a name="why-use-azure-automation-state-configuration"></a>Azure Otomasyon Durumu Yapılandırması neden kullanılır?

Azure Otomasyon Durumu Yapılandırması, Azure dışında DSC kullanmanın üzerinde çeşitli avantajlar sağlar.

### <a name="built-in-pull-server"></a>Dahili çekme sunucusu

Azure Otomasyon Durumu Yapılandırması, Windows Özelliği [DSC Hizmeti'ne](/powershell/scripting/dsc/pull-server/pullserver)benzer bir DSC çekme sunucusu sağlar. Hedef düğümler yapılandırmaları otomatik olarak alabilir, istenilen duruma uygun olabilir ve uyumluluklarını raporlayabilir. Azure Automation'daki yerleşik çekme sunucusu, kendi çekme sunucunuzu kurma ve koruma gereksinimini ortadan kaldırır. Azure Otomasyonu, bulutta veya şirket içinde sanal veya fiziksel Windows veya Linux makinelerini hedefleyebilir.

### <a name="management-of-all-your-dsc-artifacts"></a>Tüm DSC yapılarınızın yönetimi

Azure Otomasyon Durumu Yapılandırması, PowerShell komut dosyası için sunduğu [powershell istenilen durum yapılandırmasına](/powershell/scripting/dsc/overview/overview) aynı yönetim katmanını getirir. Azure portalından veya PowerShell'den tüm DSC yapılandırmalarınızı, kaynaklarınızı ve hedef düğümlerinizi yönetebilirsiniz.

![Azure Otomasyonu sayfasının ekran görüntüsü](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-of-reporting-data-into-azure-monitor-logs"></a>Verileri Azure Monitor günlüklerine bildirme

Azure Otomasyon Durumu Yapılandırması ile yönetilen düğümler, yerleşik çekme sunucusuna ayrıntılı raporlama durumu verileri gönderir. Azure Otomasyon Durumu Yapılandırmasını, bu verileri Log Analytics çalışma alanınıza gönderecek şekilde yapılandırabilirsiniz. Azure [Denetim günlüklerine veri raporlama verilerini İleri Azure Otomasyon Durumu Yapılandırmasına](automation-dsc-diagnostics.md)bakın.

## <a name="prerequisites-for-using-azure-automation-state-configuration"></a>Azure Otomasyon Durumu Yapılandırması'nı kullanmak için ön koşullar

DSC için Azure Otomasyon Durum Yapılandırması'nı kullanırken lütfen aşağıdaki gereksinimleri göz önünde bulundurun.

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
>[Microsoft Hyper-V Server](/windows-server/virtualization/hyper-v/hyper-v-server-2016) bağımsız ürün SKU DSC bir uygulama içermez. Bu nedenle PowerShell DSC veya Azure Otomasyon Durumu Yapılandırması tarafından yönetilemez.

Linux çalıştıran düğümler için, DSC Linux uzantısı Desteklenen [Linux Dağıtımları](https://github.com/Azure/azure-linux-extensions/tree/master/DSC#4-supported-linux-distributions)altında listelenen tüm Linux dağıtımlarını destekler.

### <a name="dsc-requirements"></a>DSC gereksinimleri

Azure'da çalışan tüm Windows düğümleri [için, WMF 5.1](https://docs.microsoft.com/powershell/scripting/wmf/setup/install-configure) onboarding sırasında yüklenir. Windows Server 2012 ve Windows 7 çalıştıran düğümler için [WinRM](https://docs.microsoft.com/powershell/scripting/dsc/troubleshooting/troubleshooting#winrm-dependency) etkindir.

Azure'da çalışan tüm Linux düğümleri için, Linux [için PowerShell DSC](https://github.com/Microsoft/PowerShell-DSC-for-Linux) uçağa binme sırasında yüklenir.

### <a name="configuration-of-private-networks"></a><a name="network-planning"></a>Özel ağların yapılandırması

Düğümleriniz özel bir ağda bulunuyorsa, aşağıdaki bağlantı noktası ve URL'ler gereklidir. Bu kaynaklar yönetilen düğüm için ağ bağlantısı sağlar ve DSC'nin Azure Otomasyonu ile iletişim kurmasına olanak tanır.

* Bağlantı noktası: Sadece TCP 443 giden internet erişimi için gerekli
* Genel URL: ***.azure-automation.net**
* ABD Gov Virginia Küresel URL: ***.azure-automation.us**
* Aracı hizmeti: **https://\<\>çalışma alanıId .agentsvc.azure-automation.net**

[WaitFor* kaynakları](https://docs.microsoft.com/powershell/scripting/dsc/reference/resources/windows/waitForAllResource)gibi düğümler arasında iletişim sağlayan DSC kaynaklarını kullanıyorsanız, düğümler arasındaki trafiğe de izin vermeniz gerekir. Bu ağ gereksinimlerini anlamak için her DSC kaynağının belgelerine bakın.

#### <a name="proxy-support"></a>Proxy desteği

DSC aracısı için proxy desteği Windows sürüm 1809 ve daha sonra kullanılabilir. Bu seçenek, düğümleri kaydetmek `ProxyURL` `ProxyCredential` için kullanılan [metayapılandırma komut dosyasıiçin](automation-dsc-onboarding.md#generating-dsc-metaconfigurations) ve bu değere ayarlayarak etkinleştirilir.

>[!NOTE]
>Azure Otomasyon Durumu Yapılandırması, Windows'un önceki sürümleri için DSC proxy desteği sağlamaz.

Linux düğümleri için, DSC aracısı proxy'yi destekler ve URL'yi belirlemek için değişkeni `http_proxy` kullanır.

#### <a name="azure-automation-state-configuration-network-ranges-and-namespace"></a>Azure Automation State Yapılandırma ağ aralıkları ve ad alanı

Özel durumları tanımlarken aşağıda listelenen adreslerin kullanılması önerilir. IP adresleri için [Microsoft Azure Veri Merkezi IP Aralıklarını](https://www.microsoft.com/download/details.aspx?id=41653)indirebilirsiniz. Bu dosya haftalık olarak güncellenir ve şu anda dağıtılan aralıklara ve IP aralıklarında yapılacak değişikliklere sahiptir.

Belirli bir bölge için tanımlanmış bir Otomasyon hesabınız varsa, iletişimi o bölgesel veri merkeziyle sınırlandırabilirsiniz. Aşağıdaki tablo her bölge için DNS kaydısağlar:

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

Bölge adları yerine bölge IP adresleri listesi için Microsoft İndirme Merkezi'nden [Azure Veri Merkezi IP adresi](https://www.microsoft.com/download/details.aspx?id=41653) XML dosyasını indirin.

> [!NOTE]
> Azure Veri Merkezi IP adresi XML dosyası, Microsoft Azure veri merkezlerinde kullanılan IP adres aralıklarını listeler. Dosya, işlem, SQL ve depolama aralıklarını içerir.
>
>Güncelleştirilmiş bir dosya haftalık olarak deftere nakledilir. Dosya, şu anda dağıtılan aralıkları ve IP aralıklarında yapılacak değişiklikleri yansıtır. Dosyada görünen yeni aralıklar veri merkezlerinde en az bir hafta boyunca kullanılmaz. Yeni XML dosyasını her hafta indirmek iyi bir fikirdir. Ardından Azure'da çalışan hizmetleri doğru tanımlamak için sitenizi güncelleştirin. 

Azure ExpressRoute kullanıcıları, bu dosyanın her ayın ilk haftasında Azure alanının Kenarlık Ağ Geçidi Protokolü (BGP) reklamını güncelleştirmek için kullanıldığını belirtmelidir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Otomasyon Durumu Yapılandırmasında DSC kullanmaya başlamak için [bkz.](automation-dsc-getting-started.md)
- Düğümlere nasıl bindirilir öğrenince Azure [Otomasyon Durumu Yapılandırması tarafından yönetilen Yerleşik makinelere](automation-dsc-onboarding.md)bakın.
- DSC yapılandırmalarını hedef düğümlerine atayabilmeniz için derleme hakkında bilgi edinmek için [bkz.](automation-dsc-compile.md)
- PowerShell cmdlet başvurusu için Azure [Otomasyon Durumu Yapılandırma cmdlets'e](/powershell/module/azurerm.automation/#automation)bakın.
- Fiyatlandırma bilgileri için Azure [Otomasyon Durumu Yapılandırma fiyatlandırması'na](https://azure.microsoft.com/pricing/details/automation/)bakın.
- Sürekli dağıtım ardışık bir şekilde Azure Otomasyon Durumu Yapılandırmasını kullanma örneğini görmek için, [Azure Otomasyon Durumu Yapılandırması ve Chocolatey kullanarak Sürekli dağıtım](automation-dsc-cd-chocolatey.md)bölümüne bakın.
