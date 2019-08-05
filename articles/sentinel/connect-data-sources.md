---
title: Veri kaynakları Azure Sentinel önizlemesine bağlanır mi? | Microsoft Docs
description: Veri kaynaklarını Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: a3b63cfa-b5fe-4aff-b105-b22b424c418a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/04/2019
ms.author: rkarlin
ms.openlocfilehash: 4928657aa9052b50faf1f326cc09797c5aaf69bb
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780509"
---
# <a name="connect-data-sources"></a>Veri kaynaklarını bağlama

> [!IMPORTANT]
> Azure Sentinel Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



Azure Sentinel 'e kadar, önce veri kaynaklarınıza bağlanmanız gerekir. Azure Sentinel, Microsoft çözümleri için çok sayıda bağlayıcı sunar ve Microsoft tehdit koruması çözümleri dahil gerçek zamanlı tümleştirme ve Office 365, Azure AD, Azure ATP ve dahil olmak üzere Microsoft 365 kaynakları sağlar. Microsoft Cloud App Security ve daha fazlası. Ayrıca, Microsoft dışı çözümler için daha geniş güvenlik ekosistemine yerleşik bağlayıcılar vardır. Ayrıca, veri kaynaklarınızı Azure Sentinel 'e bağlamak için ortak olay biçimi, syslog veya REST API de kullanabilirsiniz.  

1. Menüsünde, **veri bağlayıcıları**' nı seçin. Bu sayfa, Azure Sentinel 'in sağladığı bağlayıcıların tam listesini ve bunların durumlarını görmenizi sağlar. Bağlanmak istediğiniz bağlayıcıyı seçin ve **bağlayıcı sayfasını aç**' ı seçin. 

   ![Veri toplayıcılar](./media/collect-data/collect-data-page.png)

1. Belirli bağlayıcı sayfasında, tüm önkoşulları karşıladığınızdan emin olun ve verileri Azure Sentinel 'e bağlamak için yönergeleri izleyin. Günlüklerin Azure Sentinel ile eşitlenmeye başlaması biraz zaman alabilir. Bağlandıktan sonra, **alınan veri** grafiğindeki verilerin özetini ve veri türlerinin bağlantı durumunu görürsünüz.

   ![Toplayıcıların bağlantısını yapın](./media/collect-data/opened-connector-page.png)
  
1. Belirli veri türü için Azure Sentinel 'in sağladığı kullanıma hazır içeriğin bir listesini almak için **sonraki adımlar** sekmesine tıklayın.

   ![Veri toplayıcılar](./media/collect-data/data-insights.png)
 

## <a name="data-connection-methods"></a>Veri bağlantısı yöntemleri

Aşağıdaki veri bağlantısı yöntemleri Azure Sentinel tarafından desteklenir:

- **Microsoft Hizmetleri**:<br> Microsoft Hizmetleri yerel olarak bağlanır ve kullanıma hazır tümleştirme için Azure Foundation 'ı kullanarak, aşağıdaki çözümler birkaç tıklamayla bağlanabilir:
    - [Office 365](connect-office-365.md)
    - [Azure AD denetim günlükleri ve oturum açmalar](connect-azure-active-directory.md)
    - [Azure etkinliği](connect-azure-activity.md)
    - [Azure AD Kimlik Koruması](connect-azure-ad-Identity-protection.md)
    - [Azure Güvenlik Merkezi](connect-azure-security-center.md)
    - [Azure Information Protection](connect-azure-information-protection.md)
    - [Azure Gelişmiş tehdit koruması](connect-azure-atp.md)
    - [Cloud App Security](connect-cloud-app-security.md)
    - [Windows güvenlik olayları](connect-windows-security-events.md) 
    - [Windows Güvenlik Duvarı](connect-windows-firewall.md)

- **API aracılığıyla dış çözümler**: Bazı veri kaynakları, bağlı veri kaynağı tarafından sağlanmış API 'Ler kullanılarak bağlanır. Genellikle, çoğu güvenlik teknolojisi, olay günlüklerinin alınabileceği bir API kümesi sağlar. API 'Ler Azure Sentinel 'e bağlanır ve belirli veri türlerini toplayıp Azure Log Analytics 'e gönderir. API aracılığıyla bağlı olan gereçler şunları içerir:
    - [Barracuda](connect-barracuda.md)
    - ['E](connect-symantec.md)
- **Aracı aracılığıyla dış çözümler**: Azure Sentinel, bir aracı aracılığıyla Syslog protokolünü kullanarak gerçek zamanlı günlük akışı gerçekleştirebilen diğer tüm veri kaynaklarına bağlanabilir. <br>Çoğu gereçleri, günlüğü kapsayan olay iletilerini ve günlük bilgilerini içeren olay iletilerini göndermek için Syslog protokolünü kullanır. Günlüklerin biçimi değişir, ancak çoğu gereç ortak olay biçimi (CEF) standardını destekler. <br>Microsoft Monitoring Agent temel alan Azure Sentinel Aracısı, CEF biçimli günlükleri Log Analytics tarafından kullanılabilecek bir biçime dönüştürür. Gereç türüne bağlı olarak, aracı doğrudan gereç üzerine ya da adanmış bir Linux sunucusuna yüklenir. Linux Aracısı, syslog arka plan programından UDP üzerinden olay alır, ancak bir Linux makinenin yüksek hacimli bir Syslog olayları toplaması bekleniyorsa, bunlar Syslog arka plan programından aracıya ve Log Analytics için Buradan TCP üzerinden gönderilir.
    - Güvenlik duvarları, proxy 'ler ve uç noktalar:
        - [F5](connect-f5.md)
        - [Check Point](connect-checkpoint.md)
        - [Cisco ASA](connect-cisco.md)
        - [Fortinet](connect-fortinet.md)
        - [Palo Alto](connect-paloalto.md)
        - [Diğer CEF cihazları](connect-common-event-format.md)
        - [Diğer Syslog cihazları](connect-syslog.md)
    - DLP çözümleri
    - [Tehdit bilgileri sağlayıcıları](connect-threat-intelligence.md)
    - DNS [makineleri](connect-dns.md) -aracı doğrudan DNS makinesine yüklendi
    - Linux sunucuları
    - Diğer bulutlar
    
## Aracı bağlantısı seçenekleri<a name="agent-options"></a>

Dış gerecinizi Azure Sentinel 'e bağlamak için aracının, Gereç ve Azure Sentinel arasındaki iletişimi desteklemesi için adanmış bir makineye (VM veya şirket içi) dağıtılması gerekir. Aracıyı otomatik olarak veya el ile dağıtabilirsiniz. Otomatik dağıtım yalnızca adanmış makineniz Azure 'da oluşturmakta olduğunuz yeni bir VM ise kullanılabilir. 


![Azure 'da CEF](./media/connect-cef/cef-syslog-azure.png)

Alternatif olarak, aracıyı mevcut bir Azure VM 'de, başka bir buluttaki VM 'de veya şirket içi bir makinede el ile dağıtabilirsiniz.

![Şirket içi CEF](./media/connect-cef/cef-syslog-onprem.png)


## <a name="next-steps"></a>Sonraki adımlar

- Azure Sentinel ile çalışmaya başlamak için Microsoft Azure aboneliğine sahip olmanız gerekir. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
- [Verilerinizi Azure Sentinel 'e](quickstart-onboard.md) [ekleme ve verilerinize ilişkin görünürlük alma ve olası tehditler](quickstart-get-visibility.md)hakkında bilgi edinin.
