---
title: Azure Otomasyon Durumu Yapılandırması yapılandırmaya genel bakış
description: Bu makale, Azure Otomasyon Durumu Yapılandırması'na (DSC), terimlerine ve bilinen sorunlarına genel bir bakıştır.
keywords: powershell dsc, istenilen durum yapılandırması, powershell dsc azure
services: automation
ms.service: automation
ms.subservice: dsc
author: mgoedtel
ms.author: magoedte
ms.date: 04/15/2020
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1166f5a1d7586c54255120a656b060c93f842fd9
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81406175"
---
# <a name="state-configuration-overview"></a>Durum Yapılandırmasına genel bakış

Azure Otomasyon Durumu Yapılandırması, PowerShell İstenen Durum Yapılandırması (DSC) [yapılandırmalarını](/powershell/scripting/dsc/configurations/configurations)yazmanıza, yönetmenize ve derlemenize olanak tanıyan bir Azure hizmetidir. Hizmet ayrıca [DSC kaynaklarını](/powershell/scripting/dsc/resources/resources) da içeri zabıta eder ve tümü bulutta bulunan hedef düğümlere yapılandırmalar atar.

## <a name="why-use-azure-automation-state-configuration"></a>Azure Otomasyonu State Configuration neden kullanılmalıdır?

Azure Otomasyon Durumu Yapılandırması, Azure dışında DSC kullanmanın üzerinde çeşitli avantajlar sağlar.

### <a name="built-in-pull-server"></a>Dahili çekme sunucusu

Azure Otomasyon Durumu Yapılandırması, Windows Özelliği [DSC Hizmeti'ne](/powershell/scripting/dsc/pull-server/pullserver)benzer bir DSC çekme sunucusu sağlar. Hedef düğümler yapılandırmaları otomatik olarak alabilir, istenilen duruma uygun olabilir ve uyumluluklarını raporlayabilir. Azure Automation'daki yerleşik çekme sunucusu, kendi çekme sunucunuzu kurma ve koruma gereksinimini ortadan kaldırır. Azure Otomasyonu, bulutta veya şirket içinde sanal veya fiziksel Windows veya Linux makinelerini hedefleyebilir.

### <a name="manage-all-your-dsc-artifacts"></a>Tüm DSC yapılarınızı yönetin

Azure Otomasyon Durumu Yapılandırması, PowerShell komut dosyası için sunduğu [powershell istenilen durum yapılandırmasına](/powershell/scripting/dsc/overview/overview) aynı yönetim katmanını getirir. Azure portalından veya PowerShell'den tüm DSC yapılandırmalarınızı, kaynaklarınızı ve hedef düğümlerinizi yönetebilirsiniz.

![Azure Otomasyonu sayfasının ekran görüntüsü](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-azure-monitor-logs"></a>Raporlama verilerini Azure Monitör Günlüklerine aktarın

Azure Otomasyon Durumu Yapılandırması ile yönetilen düğümler, yerleşik çekme sunucusuna ayrıntılı raporlama durumu verileri gönderir. Azure Otomasyon Durumu Yapılandırmasını, bu verileri Log Analytics çalışma alanınıza gönderecek şekilde yapılandırabilirsiniz. Daha fazla bilgi için bkz: Azure Denetim Günlüğü verilerini [İleri Azure Otomasyon Durumu Yapılandırması.'na](automation-dsc-diagnostics.md)bildirin.

## <a name="prerequisites"></a>Ön koşullar

DSC için Azure Otomasyon Durumu Yapılandırmasını kullandığınızda aşağıdaki gereksinimleri göz önünde bulundurun.

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
>Microsoft [Hyper-V Server](/windows-server/virtualization/hyper-v/hyper-v-server-2016) bağımsız ürün SKU DSC uygulamasını içermediğinden, PowerShell DSC veya Azure Otomasyon Durumu Yapılandırması tarafından yönetilemez.

Linux çalıştıran düğümler için, DSC Linux uzantısı Desteklenen [Linux Dağıtımları](https://github.com/Azure/azure-linux-extensions/tree/master/DSC#4-supported-linux-distributions)altında listelenen tüm Linux dağıtımlarını destekler.

### <a name="dsc-requirements"></a>DSC gereksinimleri

Azure'da çalışan tüm Windows düğümleri [için, Windows Management Framework 5.1](https://docs.microsoft.com/powershell/scripting/wmf/setup/install-configure) onboarding sırasında yüklenir. Windows Server 2012 ve Windows 7 çalıştıran düğümler için [WinRM](https://docs.microsoft.com/powershell/scripting/dsc/troubleshooting/troubleshooting#winrm-dependency) etkindir.

Azure'da çalışan tüm Linux düğümleri için, Linux [için PowerShell DSC](https://github.com/Microsoft/PowerShell-DSC-for-Linux) uçağa binme sırasında yüklenir.

### <a name="configuration-of-private-networks"></a><a name="network-planning"></a>Özel ağların yapılandırması

Düğümleriniz özel bir ağda bulunuyorsa, aşağıdaki bağlantı noktası ve URL'ler gereklidir. Bu kaynaklar yönetilen düğüm için ağ bağlantısı sağlar ve DSC'nin Azure Otomasyonu ile iletişim kurmasına olanak tanır.

* Bağlantı noktası: Sadece TCP 443 giden internet erişimi için gerekli
* Genel URL: ***.azure-automation.net**
* ABD Gov Virginia Küresel URL: ***.azure-automation.us**
* Aracı hizmeti: **https://\<\>çalışma alanıId .agentsvc.azure-automation.net**

[WaitFor* kaynakları](https://docs.microsoft.com/powershell/scripting/dsc/reference/resources/windows/waitForAllResource)gibi düğümler arasında iletişim sağlayan DSC kaynaklarını kullanıyorsanız, düğümler arasında iletişime de izin vermeniz gerekir. Bu ağ gereksinimlerini anlamak için her DSC kaynağının belgelerine bakın.

#### <a name="proxy-support"></a>Proxy desteği

DSC aracısı için proxy desteği Windows sürüm 1809 ve daha sonra kullanılabilir. Düğümleri kaydetmek için `ProxyURL` kullanılan `ProxyCredential` [metayapılandırma komut dosyasıiçin](automation-dsc-onboarding.md#generate-dsc-metaconfigurations) ve bu değere ayarlayarak bu seçeneği etkinleştirin.

>[!NOTE]
>Azure Otomasyon Durumu Yapılandırması, Windows'un önceki sürümleri için DSC proxy desteği sağlamaz.

Linux düğümleri için, DSC aracısı bir proxy `http_proxy` sunucusunu destekler ve URL'yi belirtmek için değişkeni kullanır.

#### <a name="azure-automation-state-configuration-network-ranges-and-namespace"></a>Azure Automation State Yapılandırma ağ aralıkları ve ad alanı

Özel durumları tanımlarken, aşağıdaki tabloda listelenen IP adreslerini kullanmanızı öneririz. IP adresleri için [Microsoft Azure Veri Merkezi IP Aralıkları](https://www.microsoft.com/download/details.aspx?id=41653) XML dosyasını Microsoft İndirme Merkezi'nden indirebilirsiniz. Bu dosya, şu anda dağıtılan aralıkları ve IP aralıklarında yapılacak değişiklikleri içerir. Haftalık olarak güncellenir.

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

> [!NOTE]
> Azure Veri Merkezi IP adresi XML dosyası, Microsoft Azure veri merkezlerinde kullanılan IP adres aralıklarını listeler. Dosya, işlem, SQL ve depolama aralıklarını içerir.
>
>Güncelleştirilmiş bir dosya haftalık olarak deftere nakledilir. Dosya, şu anda dağıtılan aralıkları ve IP aralıklarında yapılacak değişiklikleri yansıtır. Dosyada görünen yeni aralıklar veri merkezlerinde en az bir hafta boyunca kullanılmaz. Her hafta yeni bir XML dosyası indirmek iyi bir fikirdir. Ardından, Azure'da çalışan hizmetleri doğru tanımlamak için sitenizi güncelleştirebilirsiniz. 

Bir Azure ExpressRoute kullanıcısıysanız, bu dosyanın her ayın ilk haftasında Azure alanının Kenarlık Ağ Geçidi Protokolü (BGP) reklamını güncelleştirmek için kullanıldığını unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Otomasyon Durumu Yapılandırmasında DSC kullanmaya başlamak için [bkz.](automation-dsc-getting-started.md)
- Düğümlere nasıl bindirilir öğrenince Azure [Otomasyon Durumu Yapılandırması tarafından yönetilen yerleşik makinelere](automation-dsc-onboarding.md)bakın.
- DSC yapılandırmalarını hedef düğümlerine atayabileceğiniz şekilde derleme hakkında bilgi edinmek için Azure [Otomasyon Durumu Yapılandırmasında Derleme yapılandırmalarına](automation-dsc-compile.md)bakın.
- PowerShell cmdlet referansı için [Az.Automation'a](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation)bakın.
- Fiyatlandırma bilgileri için Azure [Otomasyon Durumu Yapılandırma fiyatlandırması'na](https://azure.microsoft.com/pricing/details/automation/)bakın.
- Sürekli dağıtım ardışık bir şekilde Azure Otomasyon Durumu Yapılandırmasını kullanma örneği için, [Azure Otomasyon Durumu Yapılandırması ve Chocolatey'i kullanarak sanal makinelere sürekli dağıtım](automation-dsc-cd-chocolatey.md)bölümüne bakın.
