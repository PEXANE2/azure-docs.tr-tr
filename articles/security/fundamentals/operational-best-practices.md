---
title: Azure varlıklarınız için güvenlik en iyi uygulamaları
titleSuffix: Azure security
description: Bu makalede, Azure'daki verilerinizi, uygulamalarınızı ve diğer varlıklarınızı korumak için bir dizi operasyonel en iyi uygulama sağlanmaktadır.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: tomsh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/06/2019
ms.author: terrylan
ms.openlocfilehash: 72d7a2dd112e5e7a5105ff977e3917ccdfd7b53e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77500300"
---
# <a name="azure-operational-security-best-practices"></a>Azure Operasyonel Güvenlik en iyi uygulamaları
Bu makalede, Azure'daki verilerinizi, uygulamalarınızı ve diğer varlıklarınızı korumak için bir dizi operasyonel en iyi uygulama sağlanmaktadır.

En iyi uygulamalar fikir birliğine dayanır ve geçerli Azure platformu özellikleri ve özellik kümeleriyle çalışır. Görüşler ve teknolojiler zaman içinde değişir ve bu makale, bu değişiklikleri yansıtacak şekilde düzenli olarak güncelleştirilir.

## <a name="define-and-deploy-strong-operational-security-practices"></a>Güçlü operasyonel güvenlik uygulamalarını tanımlayın ve dağıtın
Azure operasyonel güvenliği, Azure'daki verilerini, uygulamalarını ve diğer varlıklarını korumak için kullanıcıların kullanabileceği hizmetler, denetimler ve özellikler anlamına gelir. Azure operasyonel güvenliği, [Güvenlik Geliştirme Yaşam Döngüsü (SDL),](https://www.microsoft.com/sdl)Microsoft Güvenlik Yanıt [Merkezi](https://www.microsoft.com/msrc?rtc=1) programı ve siber güvenlik tehdidi ortamına ilişkin derin farkındalık da dahil olmak üzere Microsoft'a özgü özellikler sayesinde elde edilen bilgileri içeren bir çerçeve üzerine kuruludur.

## <a name="manage-and-monitor-user-passwords"></a>Kullanıcı parolalarını yönetme ve izleme
Aşağıdaki tabloda kullanıcı parolalarını yönetmeyle ilgili en iyi uygulamalar listeleneb::

**En iyi uygulama**: Bulutta uygun düzeyde parola koruma düzeyine sahip olduğundan emin olun.   
**Ayrıntı**: Microsoft kimlik platformlarının (Azure Active Directory, Active Directory ve Microsoft hesabı) kullanıcıları tarafından desteklenen [Microsoft Parola Kılavuzu'ndaki](https://www.microsoft.com/research/publication/password-guidance/)kılavuzu izleyin.

**En iyi yöntem**: Kullanıcı hesaplarınızla ilgili şüpheli eylemleri izleyin.   
**Ayrıntı**: Azure AD güvenlik raporlarını kullanarak [risk altındaki ve](/azure/active-directory/reports-monitoring/concept-user-at-risk) riskli oturum açma [ları](../../active-directory/reports-monitoring/concept-risk-events.md) için izleyin.

**En iyi yöntem**: Yüksek riskli parolaları otomatik olarak algılar ve düzeltin.   
**Ayrıntı**: [Azure AD Kimlik Koruması,](/azure/active-directory/identity-protection/overview) Azure AD Premium P2 sürümünün aşağıdakileri yapmanızı sağlayan bir özelliğidir:

- Kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılama
- Kuruluşunuzun kimlikleri ile ilgili algılanan şüpheli eylemlere otomatik yanıtları yapılandırma
- Şüpheli olayları araştırın ve bunları çözmek için uygun eylemleri gerçekleştirin

## <a name="receive-incident-notifications-from-microsoft"></a>Microsoft'tan olay bildirimleri alma
Güvenlik işlemleri ekibinizin Microsoft'tan Azure olay bildirimleri aldığından emin olun. Olay bildirimi, güvenlik ekibinizin Azure kaynaklarını tehlikeye attığınızı bilmesini sağlar, böylece olası güvenlik risklerine hızlı bir şekilde yanıt verebilir ve düzeltebilir.

Azure kayıt portalında, yönetici iletişim bilgilerinin güvenlik işlemlerini bildiren ayrıntıları içerdiğinden emin olabilirsiniz. İletişim bilgileri bir e-posta adresi ve telefon numarasıdır.

## <a name="organize-azure-subscriptions-into-management-groups"></a>Azure aboneliklerini yönetim gruplarına düzenleme
Kuruluşunuzun çok sayıda aboneliği varsa, söz konusu aboneliklerde erişimi, ilkeleri ve uyumluluğu yönetmek için verimli bir yönteme ihtiyacınız olabilir. [Azure yönetim grupları,](/azure/governance/management-groups/create) aboneliklerin üzerinde bir kapsam düzeyi sağlar. Abonelikleri yönetim grupları adı verilen kapsayıcılar halinde düzenler ve yönetim koşullarınızı yönetim gruplarına uygularsınız. Bir yönetim grubu içindeki aboneliklerin tümü otomatik olarak yönetim grubuna uygulanmış olan koşulları devralır.

Yönetim grupları nın ve aboneliklerin esnek bir yapısını bir dizine dönüştürebilirsiniz. Her dizin kök yönetim grubu olarak adlandırılan tek bir üst düzey yönetim grubu verilir. Diğer tüm yönetim grupları ve abonelikler hiyerarşide en üstte yer alan bu kök yönetim grubunun altındadır. Kök yönetim grubu, genel politikaların ve RBAC atamalarının dizin düzeyinde uygulanmasına olanak tanır.

Yönetim gruplarını kullanmak için en iyi uygulamalar şunlardır:

**En iyi uygulama**: Yeni aboneliklerin eklenerek ilkeler ve izinler gibi yönetim öğelerini uyguladığından emin olun.   
**Ayrıntı**: Tüm Azure varlıkları için geçerli olan kuruluş genelindeki güvenlik öğelerini atamak için kök yönetim grubunu kullanın. İlkeler ve izinler öğelere örnektir.

**En iyi uygulama**: Her segmentte denetim ve ilke tutarlılığı için bir nokta sağlamak için yönetim gruplarının üst düzeylerini segmentasyon stratejisiyle hizala.   
**Detay**: Kök yönetim grubu altındaki her kesim için tek bir yönetim grubu oluşturun. Kök altında başka yönetim grupları oluşturmayın.

**En iyi uygulama**: Hem işlemleri hem de güvenliği engelleyen karışıklığı önlemek için yönetim grubu derinliğini sınırlandırın.   
**Ayrıntı**: Hiyerarşinizi kök de dahil olmak üzere üç düzeyle sınırlandırın.

**En iyi uygulama**: Kök yönetim grubuyla tüm kuruluşa hangi öğelerin uygulanacağı dikkatle seçin.   
**Ayrıntı**: Kök yönetim grubu öğelerinin her kaynağa açık bir şekilde uygulanmasını ve düşük etkiye sahip olmalarını sağlayın.

İyi adaylar şunlardır:

- Net bir iş etkisi olan mevzuat gereklilikleri (örneğin, veri egemenliği ile ilgili kısıtlamalar)
- Denetim etkisi olan ilke veya dikkatle gözden geçirilmiş RBAC izin atamaları gibi operasyonlar üzerinde neredeyse sıfıra yakın potansiyel olumsuz etkisi olan gereksinimler

**En iyi uygulama**: Kök yönetim grubundaki tüm kurumsal değişiklikleri uygulamadan önce (ilke, RBAC modeli vb.) dikkatle planlayın ve test edin.   
**Ayrıntı**: Kök yönetim grubundaki değişiklikler Azure'daki her kaynağı etkileyebilir. Kuruluş genelinde tutarlılık sağlamak için güçlü bir yol sağlarken, hatalar veya yanlış kullanım üretim işlemlerini olumsuz etkileyebilir. Kök yönetim grubundaki tüm değişiklikleri bir test laboratuvarında veya üretim pilotunda test edin.

## <a name="streamline-environment-creation-with-blueprints"></a>Planlarla ortam oluşturmayı kolaylaştırın
[Azure Planları hizmeti,](/azure/governance/blueprints/overview) bulut mimarlarının ve merkezi bilgi teknolojisi gruplarının, kuruluşun standartlarını, desenlerini ve gereksinimlerini uygulayan ve bu standartlara uyan tekrarlanabilir bir Azure kaynağı kümesini tanımlamasına olanak tanır. Azure Blueprints, geliştirme ekiplerinin bir dizi yerleşik bileşen ve bu ortamları kuruluş uyumluluğu içinde oluşturduklarına duyulan güvenle hızla yeni ortamlar oluşturmalarını ve ayakta durmalarını mümkün kılar.

## <a name="monitor-storage-services-for-unexpected-changes-in-behavior"></a>Beklenmeyen davranış değişiklikleri için depolama hizmetlerini izleme
Bulut ortamında barındırılan dağıtılmış bir uygulamada tanılama ve sorun giderme sorunları geleneksel ortamlardaolduğundan daha karmaşık olabilir. Uygulamalar PaaS veya IaaS altyapısında, şirket içinde, mobil cihazda veya bu ortamların bir birleşiminde dağıtılabilir. Uygulamanızın ağ trafiği genel ve özel ağlararasında geçiş yapabilir ve uygulamanız birden çok depolama teknolojisi kullanabilir.

Uygulamanızın beklenmeyen davranış değişiklikleri (daha yavaş yanıt süreleri gibi) için kullandığı depolama hizmetlerini sürekli olarak izlemeniz gerekir. Daha ayrıntılı veri toplamak ve bir sorunu derinlemesine çözümlemek için günlüğe kaydetmeyi kullanın. Hem izlemeden hem de günlüğe kaydetmeden elde ettiğiniz tanılama bilgileri, uygulamanızın karşılaştığı sorunun temel nedenini belirlemenize yardımcı olur. Daha sonra sorunu giderebilir ve düzeltmek için uygun adımları belirleyebilirsiniz.

[Azure Depolama Analizi](../../storage/common/storage-analytics.md) günlüğe kaydetme yapar ve bir Azure depolama hesabı için metrik verileri sağlar. Bu verileri istekleri izlemek, kullanım eğilimlerini çözümlemek ve depolama hesabınızla ilgili sorunları tanılamak için kullanmanızı öneririz.

## <a name="prevent-detect-and-respond-to-threats"></a>Tehditleri önleme, algılama ve yanıt verme
[Azure Güvenlik Merkezi,](../../security-center/security-center-intro.md) Azure kaynaklarınızın güvenliğinde daha fazla görünürlük sağlayarak (ve üzerinde denetim sağlayarak) tehditleri önlemenize, algılamanıza ve yanıt vermeye yardımcı olur. Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, aksi takdirde fark edilmeyecek tehditleri algılamaya yardımcı olur ve çeşitli güvenlik çözümleriyle çalışır.

Ücretsiz Güvenlik Merkezi katmanı, yalnızca Azure kaynaklarınız için sınırlı güvenlik sağlar. Standart katman, bu yetenekleri şirket içi ve diğer bulutlara genişletir. Güvenlik Merkezi Standardı, güvenlik açıklarını bulmanıza ve düzeltmenize, kötü amaçlı etkinlikleri engellemek için erişim ve uygulama denetimleri uygulamanıza, analitik ve istihbarat kullanarak tehditleri algılamanıza ve saldırı altındayken hızlı bir şekilde yanıt vermenize yardımcı olur. Güvenlik Merkezi Standart katmanını ilk 60 gün boyunca hiçbir ücret ödemeden deneyebilirsiniz. [Azure aboneliğinizi Güvenlik Merkezi Standardı'na yükseltmenizi](../../security-center/security-center-get-started.md)öneririz.

Tüm Azure kaynaklarınızın güvenlik durumunu merkezi bir görünüme sahip olmak için Güvenlik Merkezi'ni kullanın. Bir bakışta, uygun güvenlik denetimlerinin yerinde olduğunu ve doğru şekilde yapılandırıldığından doğrulayın ve dikkat edilmesi gereken kaynakları hızla tanımlayın.

Güvenlik Merkezi ayrıca kapsamlı Uç Nokta Algılama ve Yanıt (EDR) özellikleri sağlayan [Microsoft Defender Gelişmiş Tehdit Koruması (ATP)](../../security-center/security-center-wdatp.md)ile de entegre olur. Microsoft Defender ATP tümleştirmesi ile anormallikleri fark edebilirsiniz. Ayrıca, Güvenlik Merkezi tarafından izlenen sunucu uç noktalarına yapılan gelişmiş saldırıları algılayabilir ve yanıtlayabilirsiniz.

Hemen hemen tüm kurumsal kuruluşlar, çeşitli sinyal toplama aygıtlarından gelen günlük bilgilerini birleştirerek ortaya çıkan tehditleri belirlemeye yardımcı olacak bir güvenlik bilgileri ve olay yönetimi (SIEM) sistemine sahiptir. Günlükler daha sonra, tüm günlük toplama ve analiz çözümlerinde kaçınılmaz olan gürültüden "ilginç" olan ın ne olduğunu belirlemeye yardımcı olmak için bir veri analiz sistemi tarafından analiz edilir.

[Azure Sentinel](/azure/sentinel/overview) ölçeklenebilir, buluta özgü, güvenlik bilgileri ve olay yönetimi (SIEM) ve güvenlik orkestrasyonu otomatik yanıt (SOAR) çözümüdür. Azure Sentinel, uyarı algılama, tehdit görünürlüğü, proaktif avcılık ve otomatik tehdit yanıtı yoluyla akıllı güvenlik analitiği ve tehdit istihbaratı sağlar.

Tehditleri önlemek, algılamak ve bunlara yanıt vermek için en iyi uygulamalar şunlardır:

**En iyi uygulama**: Bulut tabanlı bir SIEM kullanarak SIEM çözümünüzün hızını ve ölçeklenebilirliğini artırın.   
**Ayrıntı**: [Azure Sentinel'in](/azure/sentinel/overview) özelliklerini ve özelliklerini inceleyin ve bunları şirket içinde kullanmakta olduğunuz özelliklerle karşılaştırın. Kuruluşunuzun SIEM gereksinimlerini karşılıyorsa Azure Sentinel'i benimsemeyi düşünün.

**En iyi uygulama**: Araştırmaya öncelik verebilmek için en ciddi güvenlik açıklarını bulun.   
**Ayrıntı**: Azure Güvenlik Merkezi'nde yerleşik Azure ilkeleri ve girişimlerinden kaynaklanan önerileri görmek için [Azure güvenli puanınızı](../../security-center/security-center-secure-score.md) gözden geçirin. Bu öneriler, güvenlik güncelleştirmeleri, uç nokta koruması, şifreleme, güvenlik yapılandırmaları, eksik WAF, internete bağlı VM'ler ve daha birçok riskin giderilen en önemli risklerin giderilene yardımcı olur.

Internet Güvenliği Merkezi (BDT) denetimlerini temel alan güvenli puan, kuruluşunuzun Azure güvenliğini dış kaynaklarla karşılaştırmanıza olanak tanır. Dış doğrulama, ekibinizin güvenlik stratejisini doğrulamaya ve zenginleştirmenize yardımcı olur.

**En iyi uygulama**: Olası güvenlik sorunlarını keşfetmek ve önceliklendirmek için makinelerin, ağların, depolama ve veri hizmetlerinin ve uygulamaların güvenlik duruşunu izleyin.  
**Ayrıntı**: Güvenlik Merkezi'nde en yüksek öncelikli öğelerle başlayan [güvenlik önerilerini](../../security-center/security-center-recommendations.md) izleyin.

**En iyi uygulama**: Güvenlik Merkezi uyarılarını güvenlik bilgilerinize ve olay yönetimi (SIEM) çözümüne entegre edin.   
**Ayrıntı**: SIEM'li kuruluşların çoğu, bir analist yanıtı gerektiren güvenlik uyarıları için merkezi bir takas merkezi olarak kullanır. Güvenlik Merkezi tarafından üretilen işlenmiş etkinlikler, Azure Monitor üzerinden kullanılabilen günlüklerden biri olan Azure Etkinlik Günlüğü'nde yayınlanır. Azure Monitor, izleme verilerinizden herhangi birini bir SIEM aracına yönlendirmek için birleştirilmiş bir ardışık kaynak sağlar. Bkz. Güvenlik Çözümlerini Güvenlik Merkezi'nde talimatlar için [tümleştir.](../../security-center/security-center-partner-integration.md#exporting-data-to-a-siem) Azure Sentinel [kullanıyorsanız,](../../sentinel/connect-azure-security-center.md)bkz.

**En iyi yöntem**: Azure günlüklerini SIEM'inizle tümleştirin.   
**Ayrıntı**: [Veri toplamak ve dışa aktarmak için Azure Monitor'u](/azure/azure-monitor/overview#integrate-and-export-data)kullanın. Bu uygulama, güvenlik olayı soruşturmasını etkinleştirmek için önemlidir ve çevrimiçi günlük tutma sınırlıdır. Azure Sentinel kullanıyorsanız, [veri kaynaklarını bağlayın'a](../../sentinel/connect-data-sources.md)bakın.

**En iyi uygulama**: Endpoint Detection and Response (EDR) yeteneklerini saldırı araştırmanıza entegre ederek araştırma ve avlama süreçlerinizi hızlandırın ve yanlış pozitifleri azaltın.   
**Ayrıntı**: Güvenlik Merkezi güvenlik politikanız aracılığıyla [Microsoft Defender ATP tümleştirmesini etkinleştirin.](../../security-center/security-center-wdatp.md#enable-microsoft-defender-atp-integration) Tehdit avı ve olay yanıtı için Azure Sentinel'i kullanmayı düşünün.

## <a name="monitor-end-to-end-scenario-based-network-monitoring"></a>Uçuça senaryo tabanlı ağ izlemeyi izleme
Müşteriler, sanal ağ, ExpressRoute, Application Gateway ve yük dengeleyicileri gibi ağ kaynaklarını birleştirerek Azure'da uça bir ağ oluşturur. İzleme, ağ kaynaklarının her birinde kullanılabilir.

[Azure Ağ İzleyicisi](../../network-watcher/network-watcher-monitoring-overview.md) bölgesel bir hizmettir. Azure'da ve Azure'dan ağ senaryosu düzeyindeki koşulları izlemek ve tanılamak için tanılama ve görselleştirme araçlarını kullanın.

Aşağıda ağ izleme ve kullanılabilir araçlar için en iyi uygulamalar ve redelenir.

**En iyi uygulama**: Paket yakalama ile uzak ağ izlemeyi otomatikleştirin.  
**Ayrıntı**: Ağ İzleyicisi'ni kullanarak VM'lerinize giriş yapmadan ağ sorunlarını izleyin ve tanıkoyun. Uyarıları ayarlayarak [paket yakalamayı](../../network-watcher/network-watcher-alert-triggered-packet-capture.md) tetikleyebilir ve paket düzeyinde gerçek zamanlı performans bilgilerine erişin. Bir sorun gördüğünüzde, daha iyi tanılar için ayrıntılı olarak araştırabilirsiniz.

**En iyi uygulama**: Akış günlüklerini kullanarak ağ trafiğinize ilişkin bilgi edinin.  
**Ayrıntı**: [Ağ güvenliği grubu akış günlüklerini](../../network-watcher/network-watcher-nsg-flow-logging-overview.md)kullanarak ağ trafiği desenlerinizi daha iyi anlayın. Akış günlüklerinde yer alan bilgiler, ağ güvenlik profilinizin uyumluluğu, denetlenmesi ve izlenmesi için veri toplamanıza yardımcı olur.

**En iyi yöntem**: VPN bağlantı sorunlarını tanılayın.  
**Ayrıntı**: [En yaygın VPN Ağ Geçidi ve bağlantı sorunlarınızı tanılamak](../../network-watcher/network-watcher-diagnose-on-premises-connectivity.md)için Ağ İzleyicisini kullanın. Sorunu tanımlamakla kalmıyor, aynı zamanda daha fazla araştırma yapmak için ayrıntılı günlükleri de kullanabilirsiniz.

## <a name="secure-deployment-by-using-proven-devops-tools"></a>Kanıtlanmış DevOps araçlarını kullanarak güvenli dağıtım
İşletmenizin ve ekiplerinizin üretken ve verimli olmasını sağlamak için aşağıdaki DevOps en iyi uygulamalarını kullanın.

**En iyi uygulama**: Hizmetlerin oluşturulmasını ve dağıtılmasını otomatikleştirin.  
**Ayrıntı**: [Kod olarak altyapı,](https://docs.microsoft.com/azure/devops/learn/what-is-infrastructure-as-code) BT profesyonellerinin modüler altyapının günlük yapı ve yönetimini kaldırmasına yardımcı olan bir dizi teknik ve uygulamadır. BT profesyonellerinin modern sunucu ortamlarını yazılım geliştiricilerin uygulama kodunu oluşturma ve koruma gibi bir şekilde oluşturmalarına ve sürdürmelerine olanak tanır.

Açıklayıcı bir şablon kullanarak uygulamalarınızı sağlamak için [Azure Kaynak Yöneticisi'ni](https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/) kullanabilirsiniz. Tek bir şablonda birden çok hizmeti bağımlılıklarıyla birlikte dağıtabilirsiniz. Uygulamanızı uygulama yaşam döngüsünün her aşamasında tekrar tekrar dağıtmak için aynı şablonu kullanırsınız.

**En iyi uygulama**: Azure web uygulamalarını veya bulut hizmetlerini otomatik olarak oluşturun ve dağıtın.  
**Ayrıntı**: Azure DevOps Projelerinizi Azure web uygulamalarını veya bulut hizmetlerini [otomatik olarak oluşturacak ve dağıtacak](https://docs.microsoft.com/azure/devops/pipelines/index?azure-devops) şekilde yapılandırabilirsiniz. Azure DevOps, her kod iadeden sonra Azure'da bir yapı yaptıktan sonra ikilileri otomatik olarak dağır. Paket oluşturma işlemi Visual Studio'daki Paket komutuna eşdeğerdir ve yayımlama adımları Visual Studio'daki Yayımla komutuna eşittir.

**En iyi uygulama**: Sürüm yönetimini otomatikleştirin.  
**Ayrıntı**: [Azure Pipelines,](https://docs.microsoft.com/azure/devops/pipelines/index?azure-devops) çok aşamalı dağıtımı otomatikleştirmek ve sürüm işlemini yönetmek için bir çözümdür. Hızlı, kolay ve sık sık serbest bırakmak için yönetilen sürekli dağıtım ardışık hatları oluşturun. Azure Ardışık İşlemleri ile sürüm işleminizi otomatikleştirebilir ve önceden tanımlanmış onay iş akışlarına sahip olabilirsiniz. Şirket içinde ve buluta dağıtın, gerektiğinde genişletin ve özelleştirin.

**En iyi uygulama**: Uygulamanızı başlatmadan veya güncellemeleri üretime dağıtmadan önce uygulamanızın performansını kontrol edin.  
**Ayrıntı**: Bulut tabanlı [yük testlerini](/azure/devops/test/load-test/overview#alternatives) şu şekilde çalıştırın:

- Uygulamanızda performans sorunlarını bulun.
- Dağıtım kalitesini artırın.
- Uygulamanızın her zaman kullanılabilir olduğundan emin olun.
- Uygulamanızın bir sonraki lansman veya pazarlama kampanyanız için trafiği kaldıradığından emin olun.

[Apache JMeter](https://jmeter.apache.org/) güçlü bir topluluk desteği ile ücretsiz, popüler bir açık kaynak aracıdır.

**En iyi uygulama**: Uygulama performansını izleyin.  
**Ayrıntı**: [Azure Application Insights,](../../azure-monitor/app/app-insights-overview.md) web geliştiricileri için birden çok platformda genişletilebilir bir uygulama performans yönetimi (APM) hizmetidir. Canlı web uygulamanızı izlemek için Uygulama Öngörüleri'ni kullanın. Performans anormalliklerini otomatik olarak algılar. Sorunları tanılamanıza ve kullanıcıların uygulamanızla gerçekte ne yaptığını anlamanıza yardımcı olacak analiz araçlarını içerir. Performansı ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak amacıyla tasarlanmıştır.

## <a name="mitigate-and-protect-against-ddos"></a>DDoS'a karşı azletmek ve korumak
Dağıtılmış hizmet reddi (DDoS), uygulama kaynaklarını tüketmeye çalışan bir saldırı türüdür. Amaç, uygulamanın kullanılabilirliğini ve yasal istekleri işleme yeteneğini etkilemektir. Bu saldırılar boyut ve etki olarak daha sofistike ve daha büyük hale gelmektedir. Onlar internet üzerinden kamuya ulaşılabilir herhangi bir uç noktada hedeflenebilir.

DDoS esnekliği için tasarım ve bina, çeşitli hata modları için planlama ve tasarım gerektirir. Aşağıda Azure'da DDoS'a dayanıklı hizmetler oluşturmak için en iyi uygulamalar veremetleri yer almaktadır.

**En iyi uygulama**: Tasarım ve uygulamadan dağıtım ve işlemlere kadar, bir uygulamanın tüm kullanım döngüsü boyunca güvenliğin bir öncelik olduğundan emin olun. Uygulamalar, çok fazla kaynak kullanmasına izin veren ve hizmet kesintisine neden olan nispeten düşük sayıda istek içeren hatalara sahip olabilir.  
**Ayrıntı**: Microsoft Azure'da çalışan bir hizmetin korunmasına yardımcı olmak için, uygulama mimarinizi iyi anlamalı ve [yazılım kalitesinin beş ayağına](https://docs.microsoft.com/azure/architecture/guide/pillars)odaklanmalısınız. Tipik trafik hacimlerini, uygulama ve diğer uygulamalar arasındaki bağlantı modelini ve genel internete maruz kalan hizmet bitiş noktalarını bilmeniz gerekir.

Bir uygulamanın, uygulamanın kendisini hedefleyen bir hizmet reddini işleyecek kadar esnek olmasını sağlamak en önemlidir. Güvenlik ve gizlilik, Güvenlik Geliştirme Yaşam [Döngüsü (SDL)](https://www.microsoft.com/sdl)ile başlayan Azure platformunda yerleşiktir. SDL, her geliştirme aşamasında güvenliğe hitap eder ve Azure'un güvenliği daha da güvenli hale getirmek için sürekli olarak güncelleştirilmesini sağlar.

**En iyi uygulama**: Özellikle ddos saldırısı durumunda, yükseltilmiş bir yük talebini karşılamak için uygulamalarınızı [yatay olarak ölçeklendirecek](https://docs.microsoft.com/azure/architecture/guide/design-principles/scale-out) şekilde tasarla. Uygulamanız bir hizmetin tek bir örneğine bağlıysa, tek bir hata noktası oluşturur. Birden çok örnek sağlama, sisteminizi daha esnek ve daha ölçeklenebilir hale getirir.  
**Ayrıntı**: [Azure Uygulama Hizmeti](/azure/app-service/app-service-value-prop-what-is)için, birden çok örnek sunan bir Uygulama Hizmeti [planı](../../app-service/overview-hosting-plans.md) seçin.

Azure Bulut Hizmetleri için, rollerinizi [birden çok örnek](../../cloud-services/cloud-services-choose-me.md)kullanacak şekilde yapılandırın.

[Azure Sanal Makineler](/azure/virtual-machines/windows/overview)için, VM mimarinizin birden fazla VM içerdiğinden ve her VM'nin bir [kullanılabilirlik kümesine](/azure/virtual-machines/virtual-machines-windows-manage-availability)dahil olduğundan emin olun. Otomatik ölçeklendirme özellikleri için sanal makine ölçeği kümeleri kullanmanızı öneririz.

**En iyi uygulama**: Bir uygulamada güvenlik savunmasını katmanlama başarılı bir saldırı olasılığını azaltır. Azure platformunun yerleşik özelliklerini kullanarak uygulamalarınız için güvenli tasarımlar uygulayın.  
**Detay**: Uygulamanın büyüklüğü (yüzey alanı) ile saldırı riski artar. Açıkta kalan IP adres alanını kapatmak için beyaz liste kullanarak yüzey alanını ve yük dengeleyicilerinde[(Azure Yük Dengeleyicisi](/azure/load-balancer/load-balancer-get-started-internet-portal) ve [Azure Uygulama Ağ Geçidi)](/azure/application-gateway/application-gateway-create-probe-portal)gerekli olmayan dinleme bağlantı noktalarını kullanarak yüzey alanını azaltabilirsiniz.

[Ağ güvenlik grupları,](../../virtual-network/security-overview.md) saldırı yüzeyini azaltmanın başka bir yoludur. Bir uygulamayapısının doğal bir uzantısı olarak, güvenlik kuralları oluşturmak ve ağ güvenliğini yapılandırmak için karmaşıklığı en aza indirmek için [hizmet etiketleri](../../virtual-network/security-overview.md#service-tags) ve [uygulama güvenlik grupları](../../virtual-network/security-overview.md#application-security-groups) kullanabilirsiniz.

Azure hizmetlerini mümkün olduğunca [sanal ağda](../../virtual-network/virtual-networks-overview.md) dağıtmanız gerekir. Bu uygulama, hizmet kaynaklarının özel IP adresleri üzerinden iletişim kurmasına olanak tanır. Sanal ağdaki Azure hizmet trafiği, varsayılan olarak genel IP adreslerini kaynak IP adresleri olarak kullanır.

[Hizmet bitiş noktalarını](../../virtual-network/virtual-network-service-endpoints-overview.md) kullanmak, Azure hizmetine sanal ağdan erişirken kaynak IP adresleri olarak sanal ağ özel adreslerini kullanmak için servis trafiğini değiştirir.

Azure'daki kaynaklarıyla birlikte müşterilerin şirket içi kaynaklarının saldırıya uğradığını sık sık görüyoruz. Şirket içi bir ortamı Azure'a bağlıyorsanız, şirket içi kaynakların genel internete maruz kalmasını en aza indirin.

Azure'da ağ saldırılarına karşı koruma sağlayan iki DDoS [hizmet teklifi](../../virtual-network/ddos-protection-overview.md) vardır:

- Temel koruma, ek ücret ödemeden varsayılan olarak Azure'a entegre edilir. Küresel olarak dağıtılan Azure ağının ölçeği ve kapasitesi, her zaman çevrimiçi trafik izleme ve gerçek zamanlı azaltma yoluyla yaygın ağ katmanı saldırılarına karşı savunma sağlar. Basic kullanıcı yapılandırması veya uygulama değişikliği gerektirmez ve Azure DNS gibi PaaS hizmetleri de dahil olmak üzere tüm Azure hizmetlerinin korunmasına yardımcı olur.
- Standart koruma, ağ saldırılarına karşı gelişmiş DDoS azaltma özellikleri sağlar. Belirli Azure kaynaklarınızı korumak için otomatik olarak ayarlanır. Sanal ağların oluşturulması sırasında güvenliği sağlamak kolaydır. Ayrıca oluşturulduktan sonra yapılabilir ve hiçbir uygulama veya kaynak değişikliği gerektirir.

## <a name="enable-azure-policy"></a>Azure İlkesini Etkinleştir
[Azure İlkesi,](/azure/governance/policy/overview) Azure'da ilkeler oluşturmak, atamak ve yönetmek için kullandığınız bir hizmettir. Bu ilkeler, kaynaklarınız üzerindeki kuralları ve etkileri uygular, böylece bu kaynaklar şirket standartlarınızla ve hizmet düzeyi anlaşmalarınızla uyumlu kalır. Azure İlkesi, kaynaklarınızın atanan ilkelerle uyumlu olup olmadığını değerlendirerek bu ihtiyacı karşılar.

Kuruluşunuzun yazılı ilkesini izlemek ve uygulamak için Azure İlkesi'ni etkinleştirin. Bu, karma bulut iş yükleri nizde güvenlik ilkelerini merkezi olarak yöneterek şirketinize veya düzenleyici güvenlik gereksinimlerine uygunluğu sağlar. Uyumluluğu uygulamak için ilkeleri nasıl [oluşturacağınızı ve yönetacağınızı](../../governance/policy/tutorials/create-and-manage.md)öğrenin. İlkenin öğelerine genel bakış için [Azure İlkesi tanım yapısına](../../governance/policy/concepts/definition-structure.md) bakın.

Azure İlkesini benimsedikten sonra izlenemeniz gereken bazı güvenlik en iyi uygulamaları şunlardır:

**En iyi yöntem**: İlke çeşitli etki türlerini destekler. Azure [İlkesi tanım yapısında](../../governance/policy/concepts/definition-structure.md#policy-rule)bunlar hakkında bilgi edinebilirsiniz. İş operasyonları **inkar** etkisi ve **düzeltici** etkiden olumsuz etkilenebilir, bu nedenle politikadan kaynaklanan olumsuz etki riskini sınırlamak için **denetim** etkisiyle başlayın.   
**Ayrıntı**: [Denetim modunda ilke dağıtımlarını başlatın](../../governance/policy/concepts/definition-structure.md#policy-rule) ve daha sonra **reddetmek** veya düzeltmek için ilerleme **kaydedin.** Reddetmeye veya **düzeltmeye**geçmeden önce denetim efektinin sonuçlarını **test edin** ve gözden geçirin.

Daha fazla bilgi için [bkz.](../../governance/policy/tutorials/create-and-manage.md)

**En iyi uygulama**: Politika ihlallerinin izlenmesinden ve doğru düzeltme eyleminin hızlı bir şekilde gerçekleştirilmesini sağlamaktan sorumlu rolleri belirleyin.   
**Ayrıntı**: Atanan rol izleme uyumluluğunu [Azure portalı](../../governance/policy/how-to/get-compliance-data.md#portal) üzerinden veya [komut satırı](../../governance/policy/how-to/get-compliance-data.md#command-line)üzerinden alın.

**En iyi uygulama**: Azure İlkesi, kuruluşun yazılı ilkelerinin teknik bir temsilidir. Karışıklığı azaltmak ve tutarlılığı artırmak için tüm Azure ilkelerini kuruluş ilkeleriyle eşle.   
**Ayrıntı**: Azure [ilke açıklamasındaki](../../governance/policy/concepts/definition-structure.md#display-name-and-description) kuruluş ilkesine veya Azure ilkesi [girişimi](../../governance/policy/concepts/definition-structure.md#initiatives) açıklamasına bir başvuru ekleyerek kuruluşunuzun belgelerinde veya Azure ilkesinin kendisinde belge eşleme.

## <a name="monitor-azure-ad-risk-reports"></a>Azure AD risk raporlarını izleme
Güvenlik ihlallerinin büyük çoğunluğu, saldırganların bir kullanıcının kimliğini çalarak bir ortama erişmelerinde gerçekleşir. Tehlikeye edilmiş kimlikleri keşfetmek kolay bir iş değildir. Azure AD, kullanıcı hesaplarınızla ilgili şüpheli eylemleri algılamak için uyarlanabilir makine öğrenme algoritmaları ve buluşsal algoritmalar kullanır. Algılanan her şüpheli [eylem, risk algılama](../../active-directory/reports-monitoring/concept-risk-events.md)adı verilen bir kayıtta depolanır. Risk algılamaları Azure AD güvenlik raporlarına kaydedilir. Daha fazla bilgi için, [risk güvenliği raporundaki kullanıcılar](../../active-directory/reports-monitoring/concept-user-at-risk.md) ve riskli oturum açma güvenlik [raporu](../../active-directory/reports-monitoring/concept-risky-sign-ins.md)hakkında bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar
Azure'u kullanarak bulut çözümlerinizi tasarlarken, dağıtırken ve yönetirken kullanabileceğiniz daha fazla güvenlik en iyi uygulamaları için Azure güvenlik en iyi uygulamaları ve [desenleri](best-practices-and-patterns.md) görün.

Azure güvenliği ve ilgili Microsoft hizmetleri hakkında daha genel bilgi sağlamak için aşağıdaki kaynaklar kullanılabilir:
* [Azure Güvenlik Ekibi Blogu](https://blogs.msdn.microsoft.com/azuresecurity/) - Azure Güvenlik'teki en son bilgiler için
* [Microsoft Güvenlik Yanıt Merkezi](https://technet.microsoft.com/library/dn440717.aspx) - Azure ile ilgili sorunlar da dahil olmak üzere Microsoft güvenlik açıklarının rapor edilebildiği veya e-posta yoluylasecure@microsoft.com
