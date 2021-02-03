---
title: Azure Sentinel 'deki yenilikler
description: Bu makalede, son birkaç aydan Azure Sentinel 'deki yeni özellikler açıklanmaktadır.
services: sentinel
author: batamig
ms.author: bagol
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 02/02/2021
ms.openlocfilehash: 8154e1adff8d8c2bdfe7fedc9309f95e5c5880bd
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99500727"
---
# <a name="whats-new-in-azure-sentinel"></a>Azure Sentinel 'deki yenilikler

Bu makalede, Azure Sentinel için eklenen son özellikler ve Azure Sentinel 'de gelişmiş bir kullanıcı deneyimi sağlayan ilgili hizmetlerde yeni özellikler listelenir.

Sunulan önceki özellikler hakkında daha fazla bilgi için bkz. [Tech Community bloglarımız](https://techcommunity.microsoft.com/t5/azure-sentinel/bg-p/AzureSentinelBlog/label-name/What's%20New).

Belirtilen özellikler Şu anda ÖNIZLEME aşamasındadır. [Azure önizleme ek koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) , Beta, önizleme veya henüz genel kullanıma sunulmayan Azure özelliklerine uygulanan ek yasal koşulları içerir.


> [!TIP]
> Microsoft 'un, takımlarınızın uyarlayabilmesi ve kullanabileceği belirli bir arama [sorguları](https://github.com/Azure/Azure-Sentinel) dahil olmak üzere [Azure Sentinel Community](https://github.com/Azure/Azure-Sentinel)'ye yönelik Microsoft 'a yönelik sorguları, PlayBook 'ları, çalışma kitaplarını ve not defterlerini ele alan tehdit. 
>
> Ayrıca katkıda bulunabilirsiniz! [Azure Sentinel tehdit arayıcılar GitHub Community](https://github.com/Azure/Azure-Sentinel/wiki)' de bize katılarak.
> 

## <a name="january-2021"></a>Ocak 2021

- [Az. Securityınsights PowerShell modülü (Genel Önizleme)](#azsecurityinsights-powershell-module-public-preview)
- [SQL Veritabanı Bağlayıcısı](#sql-database-connector)
- [Geliştirilmiş olay açıklamaları](#improved-incident-comments)
- [Adanmış Log Analytics kümeleri](#dedicated-log-analytics-clusters)
- [Logic Apps tarafından yönetilen kimlikler](#logic-apps-managed-identities)
- [Analiz kuralı önizleme grafikleriyle geliştirilmiş kural ayarlama](#improved-rule-tuning-with-the-analytics-rule-preview-graphs-public-preview)


### <a name="azsecurityinsights-powershell-module-public-preview"></a>Az. Securityınsights PowerShell modülü (Genel Önizleme)

Azure Sentinel artık yeni [az. Securityınsights](https://www.powershellgallery.com/packages/Az.SecurityInsights/) PowerShell modülünü desteklemektedir.

**Az. Securityınsights** modülü, vs es, önem derecesi, sahip vb. değişiklik yapmak, olaylara yorum ve Etiketler eklemek ve yer işaretleri oluşturmak için olaylarla etkileşme gibi yaygın Azure Sentinel kullanım durumlarını destekler.

CI/CD işlem hattınız için [Azure Resource Manager (ARM)](/azure/azure-resource-manager/templates/) şablonları kullanmanızı öneririz ancak, **az. securityınsights** modülü dağıtım sonrası görevler için YARARLıDıR ve özel olarak SOC otomasyonu için hedeflenir.  Örneğin, SOC otomasyonunda veri bağlayıcıları yapılandırma, analiz kuralları oluşturma veya analiz kurallarına Otomasyon eylemleri ekleme adımları bulunabilir.

Kullanılabilir cmdlet 'ler, parametre açıklamaları ve örneklerin tam listesi ve açıklaması dahil olmak üzere daha fazla bilgi için, [az. Securityınsights PowerShell belgelerine](/powershell/module/az.securityinsights/)bakın.

### <a name="sql-database-connector"></a>SQL Veritabanı Bağlayıcısı

Azure Sentinel artık, veritabanlarının denetim ve tanılama günlüklerini Azure Sentinel 'e akışını ve tüm örneklerinizin etkinliklerini sürekli olarak izlemek için kullanabileceğiniz bir Azure SQL Veritabanı Bağlayıcısı sunmaktadır.

Azure SQL, Kullanıcı katılımı olmadan yükseltme, düzeltme eki uygulama ve izleme gibi birçok veritabanı yönetim işlevini işleyen, tam olarak yönetilen bir hizmet olarak platform (PaaS) veritabanı altyapısıdır.

Daha fazla bilgi için bkz. [Azure SQL veritabanı tanılamayı ve denetim günlüklerini bağlama](connect-azure-sql-logs.md).

### <a name="improved-incident-comments"></a>Geliştirilmiş olay açıklamaları

Analistlerin olaylar üzerinde işbirliği yapmak, işlemleri ve adımları el ile veya bir PlayBook 'ın parçası olarak belgelemek için olay açıklamalarını kullanır. 

Geliştirilmiş olay yorum deneyimimiz, yorumlarınızı biçimlendirmeniz ve var olan açıklamaları düzenlemenizi veya silmenizi sağlar.

Daha fazla bilgi için bkz. [Microsoft Güvenlik uyarılarından olayları otomatik olarak oluşturma](create-incidents-from-alerts.md).
### <a name="dedicated-log-analytics-clusters"></a>Adanmış Log Analytics kümeleri

Azure Sentinel artık dağıtım seçeneği olarak adanmış Log Analytics kümelerini desteklemektedir. Şunu yaparsanız adanmış bir küme dikkate almanız önerilir:

- Azure Sentinel çalışma alanınıza **günde 1 TB 'tan fazla** alma
- Azure kaydınıza **birden çok Azure Sentinel çalışma alanı var**

Adanmış kümeler, aynı kümede birden fazla çalışma alanınız olduğunda, müşteri tarafından yönetilen anahtarlar, kasa, Çift şifreleme ve daha hızlı çalışma alanı sorguları gibi özellikleri kullanmanıza olanak sağlar.

Daha fazla bilgi için bkz. [Azure izleyici günlükleri adanmış kümeler](https://docs.microsoft.com/azure/azure-monitor/log-query/logs-dedicated-clusters).

### <a name="logic-apps-managed-identities"></a>Logic Apps tarafından yönetilen kimlikler

Azure Sentinel, artık Azure Sentinel Logic Apps Bağlayıcısı için yönetilen kimlikleri destekler, böylece ek kimlikler oluşturmak yerine Azure Sentinel 'de çalışmak üzere doğrudan belirli bir PlayBook 'a izin vermenizi sağlar.

- **Yönetilen bir kimlik olmadığında** Logic Apps Bağlayıcısı, Azure Sentinel 'de çalıştırmak Için Azure Sentinel RBAC rolüne sahip ayrı bir kimlik gerektirir. Ayrı kimlik bir Azure AD kullanıcısı veya Azure AD kayıtlı uygulaması gibi bir hizmet sorumlusu olabilir.

- **Mantıksal uygulamanızda yönetilen kimlik desteğini açmak** mantıksal uygulamayı Azure AD 'ye kaydeder ve bır nesne kimliği sağlar. Azure Sentinel çalışma alanınızda bir Azure RBAC rolü ile mantıksal uygulamayı atamak için Azure Sentinel içindeki nesne KIMLIĞINI kullanın. 

Daha fazla bilgi için bkz.

- [Azure Logic Apps yönetilen kimlikle kimlik doğrulaması](/azure/logic-apps/create-managed-service-identity)
- [Azure Sentinel Logic Apps bağlayıcı belgeleri](/connectors/azuresentinel) 

### <a name="improved-rule-tuning-with-the-analytics-rule-preview-graphs-public-preview"></a>Analiz kuralı önizleme grafikleriyle geliştirilmiş kural ayarlama (Genel Önizleme)

Azure Sentinel artık analiz kurallarınızı daha iyi ayarlamanıza yardımcı olur ve bu da doğruluğu artırabilir ve paraziti azaltır.

**Kural mantığını ayarla** sekmesinde bir analiz kuralını düzenledikten sonra, sağdaki **sonuçlar benzetimi** alanını bulun. 

Azure Sentinel 'in analiz kuralınızın son 50 çalıştırmalarının benzetimini çalıştırmasını sağlamak için **geçerli verilerle test** ' i seçin. Değerlendirilen Ham olay verileri temelinde kuralın üretilebilecek Ortalama uyarı sayısını göstermek için bir grafik oluşturulur. 

Daha fazla bilgi için bkz. [kural sorgu mantığını tanımlama ve ayarları yapılandırma](tutorial-detect-threats-custom.md#define-the-rule-query-logic-and-configure-settings).

## <a name="december-2020"></a>Aralık 2020

- [80 yeni yerleşik arama sorguları](#80-new-built-in-hunting-queries)
- [Log Analytics Aracısı geliştirmeleri](#log-analytics-agent-improvements)

### <a name="80-new-built-in-hunting-queries"></a>80 yeni yerleşik arama sorguları
 
Azure Sentinel 'in yerleşik arama sorguları, geçerli algılama kapsamındaki boşlukları azaltmak için SOC analistlerini ve yeni bir yeni arama müşteri adaylarını azaltır.

Azure Sentinel için bu güncelleştirme, MITRE ATT&CK çerçevesi matrisinde kapsam sağlayan yeni bir arama sorguları içerir:

- **Koleksiyon**
- **Komut ve denetim**
- **Kimlik bilgisi erişimi**
- **Keşfini**
- **Yürütme**
- **Sızdırma**
- **Etki**
- **İlk erişim**
- **Kalıcılığı**
- **Ayrıcalık yükseltme**

Eklenen arama sorguları, ortamınızda şüpheli etkinlik bulmanıza yardımcı olmak için tasarlanmıştır. Meşru etkinlikler ve potansiyel olarak kötü amaçlı etkinlikler döndürebilirler, ancak bu kişiler, avınız tahmin etmek için yararlı olabilir. 

Bu sorguları çalıştırdıktan sonra sonuçlarla karşılaşırsanız, bunları analiz kurallarına dönüştürmek veya var olan veya yeni olaylara arama sonuçları eklemek isteyebilirsiniz.

Tüm eklenen sorgular, Azure Sentinel arama sayfası aracılığıyla kullanılabilir. Daha fazla bilgi için bkz. [Azure Sentinel ile tehditler Için Hunt](hunting.md).

### <a name="log-analytics-agent-improvements"></a>Log Analytics Aracısı geliştirmeleri

Azure Sentinel kullanıcıları aşağıdaki Log Analytics Aracısı geliştirmelerinden faydalanır:

- CentOS 8, RedHat 8 ve SUSE Linux 15 dahil **daha fazla işletim sistemi Için destek**.
- Python 2 ' ye ek olarak **Python 3 desteği**

Azure Sentinel, Windows güvenlik olayları, syslog olayları, CEF günlükleri ve daha fazlası dahil olmak üzere çalışma alanınıza olay göndermek için Log Analytics aracısını kullanır.

> [!NOTE]
> Log Analytics Aracısı bazen OMS Aracısı veya Microsoft Monitoring Agent (MMA) olarak adlandırılır. 
> 

Daha fazla bilgi için [Log Analytics belgelerine](/azure/azure-monitor/platform/log-analytics-agent) ve [Log Analytics Aracısı sürüm notlarına](https://github.com/microsoft/OMS-Agent-for-Linux/releases)bakın.
## <a name="november-2020"></a>Kasım 2020

- [Azure Sentinel 'de Logic Apps PlayBook 'larınızı izleme](#monitor-your-logic-apps-playbooks-in-azure-sentinel)
- [Microsoft 365 Defender Bağlayıcısı (Genel Önizleme)](#microsoft-365-defender-connector-public-preview)
### <a name="monitor-your-logic-apps-playbooks-in-azure-sentinel"></a>Azure Sentinel 'de Logic Apps PlayBook 'larınızı izleme

Azure Sentinel, görevleri, iş süreçlerini ve iş akışlarını zamanlamanıza, otomatikleştirmenize ve ayarlamanıza yardımcı olan bir bulut hizmeti olan [Azure günlük uygulamalarıyla](/azure/logic-apps/)tümleştirilir.

Bir olay oluşturulduğunda otomatik olarak çağrılabilen veya olaylar ile çalışırken otomatik olarak çağrılabilecek bir PlayBook olarak Azure 'da Azure mantıksal uygulaması kullanın. 

Azure Logic Apps ile eklediğiniz gibi, PlayBook 'larınızın sistem durumu, performansı ve kullanımı hakkında Öngörüler sağlamak için **PlayBook sistem durumu izleme** adlı bir [Azure çalışma kitabı](/azure/azure-monitor/platform/workbooks-overview) ekledik. 

Playbooks **sistem durumu izleme** çalışma kitabını kullanarak PlayBook 'larınızın sistem durumunu izleyin veya başarılı veya başarısız çalıştırmalar için bozukluklar olup olmadığına bakın. 

**Playbooks sistem durumu izleme** çalışma kitabı artık Azure Sentinel şablonlar galerisinde kullanılabilir:

:::image type="content" source="media/whats-new/playbook-monitoring-workbook.gif" alt-text="Örnek playbooks sistem durumu izleme çalışma kitabı":::

Daha fazla bilgi için bkz.

- [Logic Apps belgeleri](/azure/logic-apps/monitor-logic-apps-log-analytics#set-up-azure-monitor-logs)

- [Azure İzleyici belgeleri](/azure/azure-monitor/platform/activity-log#send-to-log-analytics-workspace)

### <a name="microsoft-365-defender-connector-public-preview"></a>Microsoft 365 Defender Bağlayıcısı (Genel Önizleme)
 
Azure Sentinel için Microsoft 365 Defender Bağlayıcısı, Microsoft 365 Defender 'dan Azure Sentinel 'e kadar gelişmiş arama günlüklerini (Ham olay verileri türü) akışla kullanmanıza olanak sağlar. 

[Uç nokta Için Microsoft Defender 'ın (MDADTP)](/windows/security/threat-protection/) [Microsoft 365 Defender](/microsoft-365/security/mtp/microsoft-threat-protection) güvenliği şemsiye tümleştirmesiyle, artık microsoft Defender 'ı, Microsoft 365 Defender bağlayıcısını kullanarak uç nokta gelişmiş arama olayları Için toplayabilir ve bunları Azure Sentinel çalışma alanınızdaki yeni amaç tarafından oluşturulan tablolarda doğrudan akışla aktarabilirsiniz. 

Azure Sentinel tabloları, Microsoft 365 Defender portalında kullanılan şemaya kurulmuştur ve gelişmiş arama günlüklerinin tamamına tam erişim sağlar. 

Daha fazla bilgi için bkz. [Microsoft 365 Defender 'Dan Azure Sentinel 'e veri bağlama](connect-microsoft-365-defender.md).

> [!NOTE]
> Microsoft 365 Defender daha önce Microsoft tehdit koruması veya MTP olarak biliniyordu. Uç nokta için Microsoft Defender, daha önce Microsoft Defender Gelişmiş tehdit koruması veya MDADTP olarak biliniyordu.
> 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[Yerleşik Azure Sentinel](quickstart-onboard.md)

> [!div class="nextstepaction"]
>[Uyarılara yönelik görünürlük elde etme](quickstart-get-visibility.md)
