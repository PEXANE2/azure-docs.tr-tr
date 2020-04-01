---
title: Güvenlik Merkezi özelliklerinin emekliliği (Temmuz 2019) | Microsoft Dokümanlar
description: Bu makalede, Güvenlik Merkezi'nde 31 Temmuz 2019 tarihinde kullanımdan kaldırılan özellikler açıklanmaktadır.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: article
ms.date: 09/10/2019
ms.author: memildin
ms.openlocfilehash: fda49ae31d7598497a3128a846a3f4e12e84ffe1
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80435903"
---
# <a name="retirement-of-security-center-features-july-2019"></a>Güvenlik Merkezi'nin emekliliği özellikleri (Temmuz 2019)

> [!NOTE]
> Bu belge, Azure Güvenlik Merkezi'nden 31 Temmuz 2019'da kullanımdan kaldırılan özelliklerin listesini ayrıntılarıyla anlatır.
>
>

Temmuz 2019'a kadar olan altı ay içinde Azure Güvenlik Merkezi'nde çeşitli [iyileştirmeler](https://azure.microsoft.com/updates/?product=security-center) yaptık.
Bu gelişmiş özelliklerle, 31 Temmuz 2019'da Güvenlik Merkezi'nden bazı gereksiz özellikleri ve ilgili API'leri kaldırdık.

Bu kullanımdan kaldırılan özelliklerin çoğu Azure Güvenlik Merkezi veya Azure Günlük Analizi'ndeki diğer işlevlerle değiştirilebilir. Diğer özellikler [Azure Sentinel (önizleme)](https://azure.microsoft.com/services/azure-sentinel/)kullanılarak uygulanabilir.

Emekli Güvenlik Merkezi özellikleri şunlardır:

- [Etkinlikler panosu](#menu_events)
- [Arama menüsü girişi](#menu_search)
- [Kimlik ve erişim (önizleme) üzerinde klasik Kimlik & Erişim bağlantısını görüntüleyin](#menu_classicidentity)
- [Güvenlik uyarıları haritasındaki güvenlik olayları eşlemi düğmesi (önizleme)](#menu_securityeventsmap)
- [Özel uyarı kuralları (önizleme)](#menu_customalerts)
- [Tehdit koruması güvenlik uyarılarında araştırma düğmesi](#menu_investigate)
- [Güvenlik çözümlerinin bir alt kümesi](#menu_solutions)
- [Güvenlik ilkeleri için güvenlik yapılandırmalarını düzenleme](#menu_securityconfigurations)
- [Log Analytics çalışma alanları için güvenlik ve denetim panosu (başlangıçta OMS portalında kullanılır)](#menu_securityomsdashboard)

Bu makalede, her kullanımdan kaldırılan özellik ve değiştirme özelliklerini uygulamak için atabileceğiniz adımlar için ayrıntılı bilgiler sağlanabilir.

## <a name="events-dashboard"></a>Etkinlikler panosu<a name="menu_events"></a>

Security Center, makinelerinizden güvenlikle ilgili çeşitli yapılandırmaları ve olayları toplamak için Log Analytics aracısını kullanır. Bu olayları çalışma alanlarınızda saklar. [Etkinlikler panosu](https://docs.microsoft.com/azure/security-center/security-center-events-dashboard) bu verileri görüntülemenizi sağlar ve Log Analytics'e bir giriş noktası sağlar.

Bir çalışma alanı seçtiğinizde ortaya çıkan olaylar panosunu kullanımdan aldık:

![Etkinlikler panosu][2]

### <a name="events-dashboard---the-new-experience"></a>Etkinlikler panosu - yeni deneyim

Çalışma alanlarınızdaki önemli olayları görüntülemek için Azure Log Analytics'in yerel özelliklerini kullanmanızı öneririz.

Güvenlik Merkezi'nde özel kayda değer olaylar oluşturduysanız, bunlara erişilebilir olacaktır. Günlük Analizi'nde, **Kayıtlı Çalışma Alanı** > **Aramalarını**Seçin'e gidin. Verileriniz kaybolmaz veya değiştirilmez. Yerel önemli etkinlikler, Log Analytics'te aynı ekrandan da mevcuttur.

![Çalışma alanı aramaları kaydetti][3]

## <a name="search-menu-entry"></a>Arama menüsü girişi<a name="menu_search"></a>

Azure Güvenlik Merkezi şu anda güvenlik verilerinizi almak ve analiz etmek için Azure Monitor günlükleri aramasını kullanır. Bu ekran, Analytics'i Günlüğe Kaydetme arama sayfasına açılan bir pencere görevi görmekte ve kullanıcıların seçtikleri çalışma alanında arama sorgularını çalıştırmalarını sağlar. Daha fazla bilgi için Azure [Güvenlik Merkezi araması'na](https://docs.microsoft.com/azure/security-center/security-center-search)bakın. Bu arama pencereyi kullanımdan kaldırıldı:

![Arama sayfası][4]

### <a name="search-menu-entry---the-new-experience"></a>Arama menüsü girişi - yeni deneyim

Çalışma alanlarınızda Arama sorguları gerçekleştirmek için Azure Log Analytics yerel özelliklerini kullanmanızı öneririz. Azure Günlük Analitiği'ne gidin ve **Günlükler'i**seçin.

![Günlük Analytics günlükleri sayfası][5]

## <a name="classic-identity--access-preview"></a>Klasik Kimlik & Erişim (Önizleme)<a name="menu_classicidentity"></a>

Güvenlik Merkezi'ndeki Klasik Kimlik & Erişim deneyimi şu anda Log Analytics'te kimlik ve erişim bilgilerinin bir panosunu gösterir. Bu panoyu görüntülemek için:

1. **Klasik Kimlik & Erişimini Görüntüle'yi**seçin.

   ![Kimlik sayfası][6]

1. Kimlik **& Access panosunu**görüntüleyin.

    ![Kimlik sayfası - çalışma alanı seçimi][7]

1. Log Analytics'te **Kimlik & Erişim** panosunu açmak ve çalışma alanınızdaki kimlik ve bilgilere erişmek için bir çalışma alanı seçin.

   ![Kimlik sayfası - pano][8]

Önceki adımlarda gösterilen üç ekranı da kullanımdan çektik. Verileriniz Log Analytics güvenlik çözümünde kullanılabilir durumda kalır ve değiştirilmedi veya kaldırılmadı.

### <a name="classic-identity--access-preview---the-new-experience"></a>Klasik Kimlik & Access (Önizleme) - yeni deneyim

Log Analytics panosu, tek bir çalışma alanı hakkında öngörüler göstermiştir. Ancak, yerel Güvenlik Merkezi özellikleri, tüm aboneliklerde ve bunlarla ilişkili tüm çalışma alanlarında görünürlük sağlar. Güvenli Puanlarına göre sıralanmış önerilerle önemli olan şeylere odaklanmanızı sağlayan kullanımı kolay bir görünüme erişebilirsiniz.

Log Analytics'teki **Identity & Access** panosunun tüm özelliklerine Güvenlik Merkezi içinde Kimlik & erişimi **(Önizleme)** seçilerek ulaşılabilir.

![Kimlik sayfası - klasik deneyim emeklilik][9]

## <a name="security-events-map"></a>Güvenlik olayları haritası<a name="menu_securityeventsmap"></a>

Güvenlik Merkezi, güvenlik [tehditlerini](https://docs.microsoft.com/azure/security-center/security-center-threat-intel) belirlemenize yardımcı olacak bir güvenlik uyarıları haritası sağlar. Bu haritadaki **güvenlik olaylarına git eşlemi** düğmesi, seçilen çalışma alanında ham güvenlik olaylarını görüntülemenize olanak tanıyan bir pano açar.

**Güvenlik olayları haritasını** ve çalışma alanı başına panosuna git düğmesini kaldırdık.

![Güvenlik uyarıları haritası - düğmesi][10]

Güvenlik olayları **haritaya git düğmesini** seçtiğinizde, (şimdi kullanımdan kaldırılan) tehdit istihbarat panosunu açtı.

![Tehdit bilgileri panosu][11]

Tehdit istihbaratı panosunu görüntülemek için bir çalışma alanı seçtiğinizde, Log Analytics'te (şimdi kullanımdan kaldırılan) güvenlik uyarıları haritasını (önizleme) açtınız.

![Log Analytics'te güvenlik uyarıları haritası][12]

Varolan verileriniz Log Analytics güvenlik çözümünde kullanılabilir durumda kalır ve değiştirilmedi veya kaldırılmadı.

### <a name="security-events-map---the-new-experience"></a>Güvenlik olayları haritası - yeni deneyim

Güvenlik Merkezi' nde yerleşik uyarılar haritası işlevini kullanmanızı öneririz: **Güvenlik uyarıları haritası (Önizleme)**. Bu işlevsellik en iyi duruma getirilmiş bir deneyim sağlar ve tüm abonelikler ve ilişkili çalışma alanlarında çalışır. Ortamınız genelinde üst düzey bir görünüm sağlar ve tek bir çalışma alanına odaklanmaz.

## <a name="custom-alert-rules-preview"></a>Özel uyarı kuralları (Önizleme)<a name="menu_customalerts"></a>

Özel uyarı deneyimini 30 Haziran 2019'da, temel altyapısı emekli yedirdiği için [kullanımdan aldık.](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) Emeklilik tarihinden sonra, özel güvenlik uyarıları artık oluşturulmadı.
[Azure Sentinel'i](https://azure.microsoft.com/services/azure-sentinel/) etkinleştirmenizi ve özel uyarılarınızı burada yeniden oluşturmanızı öneririz. Alternatif olarak, Azure Monitor günlük uyarılarıyla uyarılarınızı oluşturabilirsiniz.

Azure Sentinel ile özel uyarılar oluşturmak için:

1. [Azure Sentinel'i açın](https://portal.azure.com/#create/Microsoft.ASI/preview) ve özel uyarılarınızın depolandığı çalışma alanını seçin
1. Menüden **Analytics'i** seçin
1. Azure Sentinel'de özel uyarılar oluşturma konusunda aşağıdaki [öğreticideki](https://docs.microsoft.com/azure/sentinel/tutorial-detect-threats) yönergeleri izleyin

Azure Sentinel'i kullanmak istemiyorsanız, Azure Monitor günlük uyarılarıyla uyarılarınızı oluşturabilirsiniz. Talimatlar için bkz: [Azure Monitor'da](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log)Azure Monitörü ve Günlük uyarılarını [kullanarak günlük uyarıları oluşturun, görüntüleyin ve yönetin.](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log)

![Özel uyarılar][13]

Özel uyarılar hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'nde (Önizleme) Özel Uyarı Kuralları'na](https://docs.microsoft.com/azure/security-center/security-center-custom-alert)bakın.

## <a name="security-alerts-investigation"></a>Güvenlik uyarıları soruşturma<a name="menu_investigate"></a>

Güvenlik [Merkezi'ndeki İnceleme özelliği,](https://docs.microsoft.com/azure/security-center/security-center-investigation) olası bir güvenlik olayını eşlemenize yardımcı olur. Bu özellik, bir olayın kapsamını anlamanızı ve bunun temel nedenini izlemenizi sağlar. [Azure Sentinel'de](https://azure.microsoft.com/services/azure-sentinel/)geliştirilmiş bir deneyimle değiştirilmediği için bu özelliği Güvenlik Merkezi'nden kaldırdık.

![Güvenlik olayı][14]

**Güvenlik olayı** ekranından **Araştır** düğmesini seçtiğinizde, Log Analytics'te Soruşturma Panosunu (Önizleme) açarsınız. Soruşturma Panosu'nu emekliye ayrıldık.

Varolan verileriniz Log Analytics güvenlik çözümünde kullanılabilir durumda kalır ve değiştirilmedi veya kaldırılmadı.

![Log Analytics'te araştırma panosu][15]

### <a name="investigation---the-new-experience"></a>Soruşturma - yeni deneyim

Zengin bir araştırma deneyimi için [Azure Sentinel'e](https://azure.microsoft.com/services/azure-sentinel/) geçmenizi öneririz. Azure Sentinel, kuruluşunuzun veri kaynaklarında güvenlik tehditlerini yakalamak için güçlü arama ve sorgu araçları sağlar.

## <a name="subset-of-security-solutions"></a>Güvenlik çözümlerialt kümesi<a name="menu_solutions"></a>

Güvenlik [Merkezi, Azure'da tümleşik güvenlik çözümleri](https://docs.microsoft.com/azure/security-center/security-center-partner-integration)etkinleştirebilir. Aşağıdaki iş ortağı çözümlerini Güvenlik Merkezi'nden emekli ettik. Bu çözümler Azure [Sentinel'de](https://azure.microsoft.com/services/azure-sentinel/) bir dizi ek veri kaynağıyla birlikte etkinleştirilir.

- [Yeni nesil güvenlik duvarı ve web uygulaması güvenlik duvarı çözümleri](https://docs.microsoft.com/azure/sentinel/connect-data-sources)
- [Ortak Etkinlik Biçimini (CEF) destekleyen güvenlik çözümlerinin entegrasyonu](https://docs.microsoft.com/azure/sentinel/connect-common-event-format)
- [Microsoft Advanced Threat Analytics](https://docs.microsoft.com/azure/sentinel/connect-azure-atp)
- [Azure AD Kimlik Koruması](https://docs.microsoft.com/azure/sentinel/connect-azure-ad-identity-protection)

Emekli olduktan sonra, ui veya API'den önceki listede belirtilen çözüm türlerinden herhangi birini ekleyemez veya değiştiremezsiniz. Azure Güvenlik Merkezi artık bu iş ortağı çözümlerinin yeni örneklerini keşfetmez.

Mevcut bağlı çözümleriniz varsa, Azure Sentinel'e geçmenizi öneririz.

![Güvenlik merkezleri çözümleri][16]

## <a name="edit-security-configurations-for-security-policies"></a>Güvenlik ilkeleri için güvenlik yapılandırmalarını düzenleme<a name="menu_securityconfigurations"></a>

Azure Güvenlik Merkezi, işletim sistemini güçlendirme amacıyla [150'den fazla önerilen kuraldan](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) oluşan bir küme uygulayarak güvenlik yapılandırmalarını izler. Bu kurallar güvenlik duvarları, denetim, parola ilkeleri ve daha fazlası ile ilgilidir. Bir makinenin yapılandırmasında güvenlik açığı varsa Güvenlik Merkezi bu konuyla ilgili bir güvenlik önerisi oluşturur. [Güvenlik yapılandırmasını edit ekranı,](https://docs.microsoft.com/azure/security-center/security-center-customize-os-security-config) müşterilerin Güvenlik Merkezi'ndeki varsayılan işletim sistemi güvenlik yapılandırmasını özelleştirmelerine olanak tanır.

Bu önizleme özelliğini kullanımdan aldık. Güvenlik yapılandırmalarınızı emeklilik tarihinden sonra varsayılan değerlerine sıfırlamak [için, bunu aşağıdaki yönergeleri](https://github.com/Azure/Azure-Security-Center/tree/master/Powershell%20samples/Reset%20security%20configurations%20customization)kullanarak API veya Powershell aracılığıyla yapın.

![Güvenlik yapılandırmalarını düzenleme][17]

### <a name="edit-security-configurations---the-new-experience"></a>Güvenlik yapılandırmalarını düzenleme - yeni deneyim

Güvenlik Merkezi'nin [Konuk yapılandırma aracısını](https://docs.microsoft.com/azure/governance/policy/concepts/guest-configuration)desteklemesini sağlamayı amaçlıyoruz. Böyle bir güncelleştirme, daha fazla işletim sistemi desteği ve konuk yapılandırmaları için Azure konuk içi ilkelerin entegrasyonu da dahil olmak üzere çok daha zengin bir özellik kümesine olanak tanır. Bu değişiklikler etkinleştirildikten sonra, yapılandırmaları ölçekte denetleme ve bunları otomatik olarak yeni kaynaklara uygulama olanağınız da vardır.

## <a name="security-and-audit-dashboard-for-log-analytics-workspaces"></a>Log Analytics çalışma alanları için güvenlik ve denetim panosu<a name="menu_securityomsdashboard"></a>

Güvenlik ve denetim panosu ilk olarak OMS portalında kullanılmıştır. Log Analytics'te pano, önemli güvenlik olayları ve tehditlerine, tehdit istihbaratı haritasına ve çalışma alanında kaydedilen güvenlik olaylarının kimlik ve erişim değerlendirmesine ilişkin çalışma alanı başına bir genel bakış sağlar. Gösterge panelini kaldırdık. Pano UI'da daha önce önerdiğimiz gibi, Azure Güvenlik Merkezi'ne geçmenizi tavsiye ederiz.

![Günlük Analizi güvenlik panosu][18]

### <a name="security-and-audit-dashboard---the-new-experience"></a>Güvenlik ve denetim panosu - yeni deneyim

Azure Güvenlik Merkezi'ne geçmenizi tavsiye ederiz. Birden çok abonelik ve bunlarla ilişkili çalışma alanları arasında aynı güvenlik genel görünümünüve daha zengin bir özellik kümesini sağlar.

Güvenlik Merkezi için [GitHub deposunda](https://github.com/Azure/Azure-Security-Center/tree/master/Legacy%20Log%20Analytics%20dashboards) güvenlik ve denetim panosunu dolduran orijinal Log Analytics sorgularını alabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/)hakkında daha fazla bilgi edinin.
- [Azure Sentinel](https://docs.microsoft.com/azure/sentinel)hakkında daha fazla bilgi edinin.

<!--Image references - events-->
[1]: ./media/security-center-features-retirement-july2019/asc_events_dashboard.png
[2]: ./media/security-center-features-retirement-july2019/asc_events_dashboard_inner.png
[3]: ./media/security-center-features-retirement-july2019/workspace_saved_searches.png
<!--Image references - search-->
[4]: ./media/security-center-features-retirement-july2019/asc_search.png
[5]: ./media/security-center-features-retirement-july2019/workspace_logs.png
<!--Image references - classic identity and access-->
[6]: ./media/security-center-features-retirement-july2019/asc_identity.png
[7]: ./media/security-center-features-retirement-july2019/asc_identity_workspace_selection.png
[8]: ./media/security-center-features-retirement-july2019/loganalytics_dashboard_identity.png
[9]: ./media/security-center-features-retirement-july2019/asc_identity_nobuttonhighlight.png
<!--Image references - alerts map-->
[10]: ./media/security-center-features-retirement-july2019/asc_security_alerts_map.png
[11]: ./media/security-center-features-retirement-july2019/asc_threat_intellignece_dashboard.png
[12]: ./media/security-center-features-retirement-july2019/loganalytics_security_alerts_map.png
<!--Image references - custom alerts-->
[13]: ./media/security-center-features-retirement-july2019/asc_custom_alerts.png
<!--Image references - Investigation-->
[14]: ./media/security-center-features-retirement-july2019/asc-security-incident.png
[15]: ./media/security-center-features-retirement-july2019/loganalytics_investigation_dashboard.png
<!--Image references - Solutions-->
[16]: ./media/security-center-features-retirement-july2019/asc_security_solutions.png
<!--Image references - Edit security configurations-->
[17]: ./media/security-center-features-retirement-july2019/asc_edit_security_configurations.png
<!--Image references - Security dashboard in log analytics-->
[18]: ./media/security-center-features-retirement-july2019/loganalytics_security_dashboard.png
