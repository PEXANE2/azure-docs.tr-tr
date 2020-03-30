---
title: Azure operasyonel güvenlik genel bakış| Microsoft Dokümanlar
description: Bu makalede, Azure işletim güvenliğine genel bir bakış sağlanmaktadır.
services: security
documentationcenter: na
author: unifycloud
manager: rkarlin
editor: tomsh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2019
ms.author: tomsh
ms.openlocfilehash: bcaf45af4c02dad22e2cc611fa7ea5a32ad3a853
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75443876"
---
# <a name="azure-operational-security-overview"></a>Azure operasyonel güvenlik genel bakış

[Azure operasyonel güvenliği,](/azure/security/fundamentals/operational-security) Microsoft Azure'daki verilerini, uygulamalarını ve diğer varlıklarını korumak için kullanıcıların kullanabileceği hizmetler, denetimler ve özellikler anlamına gelir. Microsoft'a özgü çeşitli özellikler aracılığıyla edinilen bilgileri içeren bir çerçevedir. Bu özellikler arasında Microsoft Security Development Lifecycle (SDL), Microsoft Security Response Center programı ve siber güvenlik tehdidi ortamına yönelik derin farkındalık yer almaktadır.

## <a name="azure-management-services"></a>Azure yönetim hizmetleri

Bt operasyon ekibi, bu sistemlerin kararlılığı ve güvenliği de dahil olmak üzere veri merkezi altyapısını, uygulamalarını ve verilerini yönetmekten sorumludur. Ancak, artan karmaşık BT ortamları arasında güvenlik öngörüleri kazanmak, kuruluşların genellikle birden çok güvenlik ve yönetim sisteminden gelen verileri bir araya getirmesini gerektirir.

[Microsoft Azure Monitor günlükleri,](/azure/operations-management-suite/operations-management-suite-overview) şirket içi ve bulut altyapınızı yönetmenize ve korumanıza yardımcı olan bulut tabanlı bir BT yönetimi çözümüdür. Temel işlevi Azure'da çalışan aşağıdaki hizmetler tarafından sağlanır. Azure, şirket içi ve bulut altyapınızı yönetmenize ve korumanıza yardımcı olan birden çok hizmet içerir. Her hizmet belirli bir yönetim işlevi sağlar. Farklı yönetim senaryoları elde etmek için hizmetleri birleştirebilirsiniz. 

### <a name="azure-monitor"></a>Azure İzleyici

[Azure Monitor](/azure/azure-monitor/overview) yönetilen kaynaklardan merkezi veri depolarına veri toplar. Bu veriler, API aracılığıyla sağlanan olayları, performans verilerini veya özel verileri içerebilir. Veriler toplandıktan sonra, uyarı, analiz ve dışa aktarma için kullanılabilir.

Çeşitli kaynaklardan gelen verileri birleştirebilir ve Azure hizmetlerinizdeki verileri mevcut şirket içi ortamınızla birleştirebilirsiniz. Azure Monitor günlükleri, tüm eylemlerin her türlü veri için kullanılabilmesi için verilerin toplanmasını bu veriler üzerinde gerçekleştirilen eylemden açıkça ayırır.

### <a name="automation"></a>Automation

[Azure Otomasyonu,](/azure/automation/automation-intro) bulut ve kurumsal ortamda sık sık gerçekleştirilen el kitabı, uzun süreli, hataya açık ve sık sık tekrarlanan görevleri otomatikleştirmeniz için bir yol sağlar. Zamandan tasarruf sağlar ve yönetim görevlerinin güvenilirliğini artırır. Hatta bu görevleri düzenli aralıklarla otomatik olarak gerçekleştirilecek şekilde zamanlar. Runbook'ları kullanarak işlemleri otomatikleştirebilir veya Yapılandırma yönetimini İstenilen Durum Yapılandırması'nı kullanarak otomatikleştirebilirsiniz.

### <a name="backup"></a>Backup

[Azure Yedekleme,](/azure/backup/backup-introduction-to-azure-backup) Verilerinizi Microsoft Bulutu'nda yedeklemek (veya korumak) ve geri yüklemek için kullanabileceğiniz Azure tabanlı bir hizmettir. Azure Yedekleme, mevcut şirket içi veya şirket dışı yedekleme çözümünüzü güvenilir, güvenli ve maliyete uygun bulut tabanlı bir çözümle değiştirir.

Azure Yedekleme, uygun bilgisayarda veya sunucuda veya bulutta indirip dağıttığınız bileşenler sunar. Dağıtacağınız bileşen veya aracı, korumak istediğiniz nesnelere göre değişiklik gösterir. Tüm Azure Yedekleme bileşenleri (verileri şirket içinde veya bulutta koruyor olun) verileri Azure'daki bir Azure Kurtarma Hizmetleri kasasına yedeklemek için kullanılabilir.

Daha fazla bilgi için [Azure Yedekleme bileşenleri tablosuna](/azure/backup/backup-overview#what-can-i-back-up)bakın.

### <a name="site-recovery"></a>Site Recovery

[Azure Site Kurtarma,](https://azure.microsoft.com/documentation/services/site-recovery) şirket içi sanal ve fiziksel makinelerin azure'a veya ikincil bir siteye çoğaltılmasını düzenleyerek iş sürekliliği sağlar. Birincil siteniz kullanılamıyorsa, kullanıcıların çalışmaya devam edebilmeleri için ikincil konuma geçemezsiniz. Sistemler çalışma düzenine geri döndüğünde başarısız oluyorsunuz. Daha akıllı ve etkili tehdit algılama gerçekleştirmek için Azure Güvenlik Merkezi'ni kullanın.

## <a name="azure-active-directory"></a>Azure Active Directory

[Azure Etkin Dizin (Azure AD)](/azure/active-directory/active-directory-enable-sso-scenario) kapsamlı bir kimlik hizmetidir:

-   Bulut hizmeti olarak kimlik ve erişim yönetimini (IAM) etkinleştirirken.
-   Merkezi erişim yönetimi, tek oturum açma (SSO) ve raporlama sağlar.
-   Salesforce, Google Apps, Box ve Concur dahil olmak üzere Azure Marketi'ndeki [binlerce uygulama](https://azure.microsoft.com/marketplace/active-directory/) için tümleşik erişim yönetimini destekler.

Azure AD, bunlar da dahil olmak üzere tam bir [kimlik yönetimi özelliği](/azure/security/security-identity-management-overview#security-monitoring-alerts-and-machine-learning-based-reports)paketi de içerir:

- [Çok faktörlü kimlik doğrulama](/azure/multi-factor-authentication/multi-factor-authentication)
- [Self servis şifre yönetimi](https://azure.microsoft.com/resources/videos/self-service-password-reset-azure-ad/)
- [Self servis grup yönetimi](/azure/active-directory/active-directory-passwords-update-your-own-password)
- [Ayrıcalıklı hesap yönetimi](/azure/active-directory/active-directory-privileged-identity-management-configure)
- [Rol tabanlı erişim denetimi](/azure/role-based-access-control/overview)
- [Uygulama kullanım izleme](/azure/active-directory/connect-health/active-directory-aadconnect-health)
- [Zengin denetim](/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Güvenlik izleme ve uyarı](/azure/operations-management-suite/oms-security-responding-alerts)

Azure Active Directory ile iş ortaklarınız ve müşterileriniz (işletme veya tüketici) için yayımladırdığınız tüm uygulamalar aynı kimliğe ve erişim yönetimi yeteneklerine sahiptir. Bu, işletme maliyetlerinizi önemli ölçüde azaltmanızı sağlar.

## <a name="azure-security-center"></a>Azure Güvenlik Merkezi

[Azure Güvenlik Merkezi,](/azure/security-center/security-center-intro) Azure kaynaklarınızın güvenliğini artırarak (ve denetimle) tehditleri önlemenize, algılamanıza ve yanıt vermeye yardımcı olur. Abonelikleriniz genelinde tümleşik güvenlik izleme ve ilke yönetimi sağlar. Aksi takdirde fark edilmeden gidebilecek tehditleri algılamaya yardımcı olur ve geniş bir güvenlik çözümleri ekosistemi ile çalışır.

Sanal makinenizin güvenlik ayarlarında görünürlük sağlayarak ve tehditlere karşı izleme yaparak [Azure'daki sanal makine (VM) verilerini koruyun.](/azure/security-center/security-center-linux-virtual-machine) Güvenlik Merkezi, sanal makinelerinizi şu açılardan izleyebilir:

- Önerilen yapılandırma kurallarıyla işletim sistemi güvenlik ayarları.
- Sistem güvenliği ve eksik olan kritik güncelleştirmeler.
- Endpoint koruma önerileri.
- Disk şifreleme doğrulaması.
- Ağ tabanlı saldırılar.

Güvenlik [Merkezi, Role Tabanlı Erişim Denetimi (RBAC)](/azure/role-based-access-control/role-assignments-portal)kullanır. RBAC, [Azure'daki](../../role-based-access-control/built-in-roles.md) kullanıcılara, gruplara ve hizmetlere atanabilecek yerleşik roller sağlar.

Güvenlik Merkezi, güvenlik sorunlarını ve güvenlik açıklarını tanımlamak için kaynaklarınızın yapılandırmasını değerlendirir. Güvenlik Merkezi'nde, kaynakla ilgili bilgileri yalnızca kaynağın ait olduğu abonelik veya kaynak grubu için sahip, katkıda bulunan veya okuyucu rolünü aldığınızda görürsünüz.

>[!Note]
>Güvenlik Merkezi'ndeki roller ve izin verilen eylemler hakkında daha fazla bilgi edinmek için [Azure Güvenlik Merkezi'ndeki İzinler'e](/azure/security-center/security-center-permissions)bakın.

Güvenlik Merkezi, Microsoft İzleme Aracısını kullanır. Bu, Azure Monitor hizmetinin kullandığı aracıdır. Bu aracıdan toplanan veriler, VM'nin coğrafi konumunu da hesaba katarak Azure aboneliğinizle ilişkili mevcut bir Log Analytics [çalışma alanında](/azure/log-analytics/log-analytics-manage-access) veya yeni bir çalışma alanında depolanır.

## <a name="azure-monitor"></a>Azure İzleyici

Bulut uygulamanızdaki performans sorunları işletmenizi etkileyebilir. Birbirine bağlı birden çok bileşen ve sık sürümlerde, herhangi bir zamanda bozulmalar meydana gelebilir. Bir uygulama geliştiriyorsanız, kullanıcılarınız genellikle testte bulamadığınız sorunları keşfeder. Bu sorunları hemen bilmeniz gerekir ve sorunları tanılama ve düzeltme araçları olmalıdır.

[Azure Monitor,](/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) Azure'da çalışan hizmetleri izlemek için temel araçtır. Bir hizmetin üretim yapısı ve çevre ortamı hakkında altyapı düzeyinde veri sağlar. Uygulamalarınızı tümü Azure'da yönetiyor ve kaynakları ölçeklendirmeye mi yoksa küçültmeye mi karar veriyorsanız, Başlamak için gereken yer Azure Monitor'dur.

Uygulamanız hakkında derin bilgiler edinmek için izleme verilerini de kullanabilirsiniz. Bu bilgi, uygulama performansını veya bakımı geliştirmenize veya aksi takdirde el ile müdahale gerektiren eylemleri otomatikleştirmenize yardımcı olabilir.

Azure Monitor aşağıdaki bileşenleri içerir.

### <a name="azure-activity-log"></a>Azure Etkinlik Günlüğü

[Azure Etkinlik Günlüğü,](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen işlemlerhakkında bilgi sağlar. Abonelikleriniz için denetim düzlemi olaylarını bildirdiği için daha önce "Denetim Günlüğü" veya "Operasyon Günlüğü" olarak biliniyordu.

### <a name="azure-diagnostic-logs"></a>Azure tanılama günlükleri

[Azure tanı günlükleri](/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) bir kaynak tarafından yayılır ve bu kaynağın çalışması hakkında zengin ve sık veri sağlar. Bu günlüklerin içeriği kaynak türüne göre değişir.

Windows olay sistem günlükleri VM'ler için tanılama günlükleri bir kategoridir. Blob, tablo ve sıra günlükleri depolama hesapları için tanıgünlükleri kategorileridir.

Tanılama günlükleri [Etkinlik Günlüğünden](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)farklıdır. Etkinlik günlüğü, aboneliğinizdeki kaynaklarüzerinde gerçekleştirilen işlemlerhakkında bilgi sağlar. Tanılama günlükleri, kaynağınızın kendisinin gerçekleştirdiği işlemlerhakkında bilgi sağlar.

### <a name="metrics"></a>Ölçümler

Azure Monitor, Azure'daki iş yüklerinizin performansı ve sistem durumu hakkında görünürlük sağlayan telemetri sağlar. Azure telemetri verilerinin en önemli türü, çoğu Azure kaynağı tarafından yayılan [ölçümlerdir](/azure/monitoring-and-diagnostics/monitoring-overview-metrics) (performans sayaçları olarak da adlandırılır). Azure Monitor, izleme ve sorun giderme için bu ölçümleri yapılandırmak ve tüketmek için çeşitli yollar sağlar.

### <a name="azure-diagnostics"></a>Azure Tanılama

Azure Tanılama, dağıtılan bir uygulamada tanılama verilerinin toplanmasını sağlar. Tanılama uzantısını çeşitli kaynaklardan kullanabilirsiniz. Şu anda desteklenen [Azure bulut hizmeti rolleri,](/azure/vs-azure-tools-configure-roles-for-cloud-service)Microsoft Windows çalıştıran [Azure sanal makineleri](/azure/vs-azure-tools-configure-roles-for-cloud-service) ve Azure [Hizmet Kumaşı.](/azure/monitoring-and-diagnostics/azure-diagnostics)

## <a name="azure-network-watcher"></a>Azure Ağ İzleyicisi

Müşteriler, sanal ağlar, Azure ExpressRoute, Azure Application Gateway ve yük dengeleyicileri gibi tek tek ağ kaynaklarını düzenleyerek ve oluşturarak Azure'da uçtan uca bir ağ oluşturur. İzleme, ağ kaynaklarının her birinde kullanılabilir.

Uçlardan uca ağ, karmaşık yapılandırmalara ve kaynaklar arasındaki etkileşimlere sahip olabilir. Sonuç, [Azure Ağ İzleyicisi](/azure/network-watcher/network-watcher-monitoring-overview)aracılığıyla senaryo tabanlı izleme gerektiren karmaşık senaryolardır.

Ağ İzleyicisi, Azure ağınızın izlenmesini ve tanılamayı kolaylaştırır. Ağ İzleyicisi'ndeki tanılama ve görselleştirme araçlarını şu şekilde kullanabilirsiniz:

- Bir Azure sanal makinesinde uzaktan paket yakalamaları alın.
- Akış günlüklerini kullanarak ağ trafiğiniz hakkında bilgi edinin.
- Azure VPN Ağ Geçidi'ni ve bağlantılarını tanın.

Ağ İzleyicisi şu anda aşağıdaki özelliklere sahiptir:

- [Topoloji](/azure/network-watcher/network-watcher-topology-overview): Kaynak grubundaki ağ kaynakları arasındaki çeşitli bağlantıların ve ara bağlantıların görünümünü sağlar.
- [Değişken paket yakalama](/azure/network-watcher/network-watcher-packet-capture-overview): Sanal makinenin içinde ve dışında paket verilerini yakalar. Zaman ve boyut sınırlamaları ayarlama gibi gelişmiş filtreleme seçenekleriyle ince ayarlı denetimler çok yönlülük getirir. Paket verileri bir blob deposunda veya .cap biçiminde yerel diskte depolanabilir.
- [IP akışı doğrulama](/azure/network-watcher/network-watcher-ip-flow-verify-overview): Akış bilgileri (hedef IP, kaynak IP, hedef bağlantı noktası, kaynak bağlantı noktası ve protokol) için 5-tuple paket parametrelerine göre bir pakete izin verilip verilmediğini veya reddedilip reddedilip reddedildiğini denetler. Bir güvenlik grubu paketi reddederse, paketi reddeden kural ve grup döndürülür.
- [Sonraki atlama](/azure/network-watcher/network-watcher-next-hop-overview): Azure ağ dokusunda yönlendirilen paketler için bir sonraki atlamayı belirler, böylece yanlış yapılandırılmış kullanıcı tanımlı yolları tanılayabilirsiniz.
- [Güvenlik grubu görünümü](/azure/network-watcher/network-watcher-security-group-view-overview): VM'de uygulanan etkili ve uygulamalı güvenlik kurallarını alır.
- [Ağ güvenlik grupları için NSG akış günlükleri](/azure/network-watcher/network-watcher-nsg-flow-logging-overview): Gruptaki güvenlik kuralları tarafından izin verilen veya reddedilen trafikle ilgili günlükleri yakalamanızı sağlar. Akış 5-tuple bilgi ile tanımlanır: kaynak IP, hedef IP, kaynak bağlantı noktası, hedef bağlantı noktası ve protokol.
- [Sanal ağ ağ geçidi ve bağlantı sorun giderme](/azure/network-watcher/network-watcher-troubleshoot-manage-rest): Sanal ağ ağ ağ geçitlerini ve bağlantılarını giderme olanağı sağlar.
- [Ağ abonelik sınırları](/azure/network-watcher/network-watcher-monitoring-overview): Ağ kaynağı kullanımını sınırlara göre görüntülemenizi sağlar.
- [Tanılama günlükleri](/azure/network-watcher/network-watcher-monitoring-overview): Kaynak grubundaki ağ kaynakları için tanılama günlüklerini etkinleştirmek veya devre dışı kalmak için tek bir bölme sağlar.

Daha fazla bilgi için [bkz.](/azure/network-watcher/network-watcher-create)

## <a name="cloud-service-provider-access-transparency"></a>Bulut Hizmeti Sağlayıcısı Erişim Saydamlığı

[Microsoft Azure için Müşteri Kilit Kutusu,](customer-lockbox-overview.md) bir Microsoft Destek Mühendisinin bir sorunu çözmek için verilerinize erişmeye ihtiyaç duyabileceği nadir durumlarda size açık denetim sağlayan Azure portalına entegre edilmiş bir hizmettir.
Microsoft Destek Mühendisi'nin bu sorunu gidermek için yüksek izinler gerektirdiği hata ayıklama uzaktan erişim sorunu gibi çok az örnek vardır. Bu gibi durumlarda, Microsoft mühendisleri, hizmetle sınırlı, zamana bağlı yetkilendirme sağlayan tam zamanında erişim hizmeti kullanır.  
Microsoft her zaman erişim için müşteri onayı almış olsa da, Customer Lockbox artık Azure Portalı'ndan gelen bu tür istekleri gözden geçirme ve onaylama veya reddetme olanağı sağlar. Microsoft destek mühendislerine, siz isteği onaylayana kadar erişim izni verilmez.

## <a name="standardized-and-compliant-deployments"></a>Standartlaştırılmış ve Uyumlu Dağıtımlar

[Azure Planları,](/azure/governance/blueprints/overview) bulut mimarlarının ve merkezi bilgi teknolojisi gruplarının, kuruluşun standartlarını, desenlerini ve gereksinimlerini uygulayan ve bu standartlara uyan tekrarlanabilir bir Azure kaynağı kümesini tanımlamasına olanak tanır.  
Bu, DevOps ekiplerinin hızla yeni ortamlar oluşturmalarını ve ayakta durmalarını ve bunları kuruluş uyumluluğunu koruyan altyapıyla inşa ettiklerine güvenmelerini mümkün kılar.
Planlar, çeşitli kaynak şablonlarının ve aşağıdakiler gibi diğer yapıların dağıtımını düzenlemek için açıklayıcı bir yol sağlar:

- Rol Atamaları
- İlke Atamaları
- Azure Resource Manager şablonları
- Kaynak Grupları

## <a name="devops"></a>DevOps

[Geliştirici İşlemleri (DevOps)](https://www.visualstudio.com/learn/what-is-devops/) uygulama geliştirmeden önce, ekipler bir yazılım programı için iş gereksinimlerini toplamak ve kod yazmaktan sorumluydu. Daha sonra ayrı bir QA ekibi programı yalıtılmış bir geliştirme ortamında test etti. Gereksinimler karşılandıysa, QA ekibi dağıtılan işlemler için kod yayımladı. Dağıtım ekipleri ağ ve veritabanı gibi gruplara daha da bölündü. Her zaman bir yazılım programı bağımsız bir ekibe "duvardan atıldığında" darboğazlar ekledi.

DevOps, ekiplerin daha güvenli, daha yüksek kaliteli çözümleri daha hızlı ve daha ucuza sunmalarını sağlar. Müşteriler yazılım ve hizmetleri tüketirken dinamik ve güvenilir bir deneyim beklerler. Takımlar yazılım güncelleştirmelerini hızla yinelemeli ve güncelleştirmelerin etkisini ölçmelidir. Sorunları gidermek veya daha fazla değer sağlamak için yeni geliştirme yinelemeleriyle hızlı bir şekilde yanıt vermeleri gerekir.  

Microsoft Azure gibi bulut platformları geleneksel darboğazları ortadan kaldırmış ve altyapının emtia haline alınmasına yardımcı olur. Yazılım, her işte iş sonuçlarının temel farkını ve etkeni olarak hüküm sürer. Hiçbir kuruluş, geliştirici veya BT çalışanı DevOps hareketinden kaçınamaz veya kaçınmalıdır.

Olgun DevOps uygulayıcıları aşağıdaki uygulamalardan birkaçını benimserler. Bu uygulamalar, insanların iş senaryolarına dayalı stratejiler oluşturmalarını [içerir.](https://www.visualstudio.com/learn/what-is-devops-culture/) Takım, çeşitli uygulamaların otomatikleştirilmesine yardımcı olabilir.

- [Çevik planlama ve proje yönetimi](https://www.visualstudio.com/learn/what-is-agile/) teknikleri, işi sprint'lere dönüştürmek ve yalıtmak, ekip kapasitesini yönetmek ve ekiplerin değişen iş gereksinimlerine hızla uyum sağlamasına yardımcı olmak için kullanılır.
- [Sürüm denetimi, genellikle Git ile,](https://www.visualstudio.com/learn/what-is-git/)dünyanın herhangi bir yerinde bulunan ekiplerin kaynak paylaşmak ve serbest bırakma boru hattı otomatikleştirmek için yazılım geliştirme araçları ile entegre sağlar.
- [Sürekli tümleştirme,](https://www.visualstudio.com/learn/what-is-continuous-integration/) kodların sürekli birleştirilmesini ve test edilmesine yol açar ve bu da hataların erken bulunmasına yol açar.  Diğer avantajlar arasında birleştirme sorunlarıyla mücadele de daha az zaman harcanmış ve geliştirme ekipleri için hızlı geri bildirim yer almaktadır.
- Yazılım çözümlerinin üretim ve test ortamlarına [sürekli olarak sunulması,](https://www.visualstudio.com/learn/what-is-continuous-delivery/) kuruluşların hataları hızla düzeltmesine ve sürekli değişen iş gereksinimlerine yanıt vermesine yardımcı olur.
- Uygulama durumu için üretim ortamları ve müşteri kullanımı da dahil olmak üzere çalışan uygulamaların [izlenmesi,](https://www.visualstudio.com/learn/what-is-monitoring/) kuruluşların bir hipotez oluşturmasına ve stratejileri hızla doğrulamalarına veya çürütmesine yardımcı olur.  Zengin veriler yakalanır ve çeşitli günlük biçimlerinde depolanır.
- [Kod Olarak Altyapı (IaC),](https://www.visualstudio.com/learn/what-is-infrastructure-as-code/) güvenli ve kararlı uygulama barındırma platformları sunmaya yardımcı olmak için ağların ve sanal makinelerin oluşturulması ve yıkılmasının otomasyonu ve doğrulanmasını sağlayan bir uygulamadır.
- [Microservices](https://www.visualstudio.com/learn/what-are-microservices/) mimarisi, iş kullanım kılıflarını küçük yeniden kullanılabilir hizmetlere ayırmak için kullanılır.  Bu mimari ölçeklenebilirlik ve verimlilik sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Güvenlik ve Denetim çözümü hakkında bilgi edinmek için aşağıdaki makalelere bakın:

- [Güvenlik ve uyumluluk](https://azure.microsoft.com/overview/trusted-cloud/)
- [Azure Güvenlik Merkezi](/azure/security-center/security-center-intro)
- [Azure İzleyici](/azure/azure-monitor/overview)
