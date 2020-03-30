---
title: Azure Monitör ile sürekli izleme | Microsoft Dokümanlar
description: İş akışlarınız boyunca Sürekli izlemeyi etkinleştirmek için Azure Monitörünü kullanmak için belirli adımları açıklar.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/12/2018
ms.openlocfilehash: b9ca8a703ed8a84148abd23e90114402d8806bd6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77667202"
---
# <a name="continuous-monitoring-with-azure-monitor"></a>Azure Monitor ile sürekli izleme

Sürekli izleme, DevOps ve BT operasyon yaşam döngülerinizin her aşamasında izlemeyi birleştirmek için gereken süreç ve teknolojiyi ifade eder. Geliştirmeden üretime geçtikçe uygulamanızın ve altyapınızın sağlığını, performansını ve güvenilirliğini sürekli olarak sağlamaya yardımcı olur. Sürekli izleme, kullanıcılarınıza sürekli değer sağlamak için yazılımları daha hızlı ve daha güvenilir bir şekilde geliştirmenize ve sunmanıza yardımcı olan Sürekli Tümleştirme ve Sürekli Dağıtım (CI/CD) kavramları üzerine kuruludur.

[Azure Monitor,](overview.md) Azure'da bulutta ve şirket içinde uygulamalar ve altyapı lar arasında tam yığın gözlemlenebilirlik sağlayan birleşik izleme çözümüdür. Geliştirme ve test sırasında [Visual Studio ve Visual Studio Code](https://visualstudio.microsoft.com/) ile sorunsuz çalışır ve dağıtım ve işlemler sırasında sürüm yönetimi ve iş öğesi yönetimi için Azure [DevOps](/azure/devops/user-guide/index) ile tümleşir. Hatta, mevcut BT süreçlerinizdeki sorunları ve olayları izlemenize yardımcı olmak için seçtiğiniz ITSM ve SIEM araçları yla entegre olur.

Bu makalede, iş akışlarınız boyunca sürekli izleme sağlamak için Azure Monitor'u kullanmak için belirli adımlar açıklanmaktadır. Farklı özellikleri uygulama hakkında ayrıntılı bilgi sağlayan diğer belgelere bağlantılar içerir.


## <a name="enable-monitoring-for-all-your-applications"></a>Tüm uygulamalarınız için izlemeyi etkinleştirin
Tüm ortamınızda gözlemlenebilirlik kazanmak için, tüm web uygulamaları ve hizmetleri üzerinde izleme etkinleştirmek gerekir. Bu, tüm bileşenler arasında uçtan uca hareketleri ve bağlantıları kolayca görselleştirmenize olanak sağlar.

- [Azure DevOps Projeleri,](../devops-project/overview.md) varolan kodunuz ve Git deponuzla basitleştirilmiş bir deneyim sunar veya Azure'a Sürekli Tümleştirme (CI) ve Sürekli Teslim (CD) ardışık bir altyapı oluşturmak için örnek uygulamalardan birini seçin.
- [DevOps sürüm ardışık ardışık sisteminizde sürekli izleme,](../azure-monitor/app/continuous-monitoring.md) izleme verilerine dayalı olarak dağıtımınızı bültenlemenize veya geri almanıza olanak tanır.
- [Durum İzleyicisi,](../azure-monitor/app/monitor-performance-live-website-now.md) kodunuzu değiştirmek veya yeniden dağıtmak zorunda kalmadan Windows'da Azure Uygulama Öngörüleri ile canlı bir .NET uygulaması na sahip olmanızı sağlar.
- Uygulamanızın koduna erişiminiz varsa, [.NET](../azure-monitor/learn/quick-monitor-portal.md), [Java](../azure-monitor/app/java-get-started.md), [Node.js](../azure-monitor/learn/nodejs-quick-start.md)veya [diğer programlama dilleri](../azure-monitor/app/platforms.md)için Azure Monitor Application Insights SDK'yı yükleyerek [Uygulama Öngörüleri](../azure-monitor/app/app-insights-overview.md) ile tam izlemeyi etkinleştirin. Bu, uygulamanızla ve işletmenizle alakalı özel olayları, ölçümleri veya sayfa görünümlerini belirtmenize olanak tanır.



## <a name="enable-monitoring-for-your-entire-infrastructure"></a>Tüm altyapınız için izlemeyi etkinleştirin
Uygulamalar sadece temel altyapıları kadar güvenilirdir. Tüm altyapınız boyunca izlemenin etkin olması, tam gözlemlenebilirlik elde etmenize yardımcı olur ve bir şey başarısız olduğunda olası bir temel nedeni keşfetmenizi kolaylaştırır. Azure Monitör, VM'ler, kapsayıcılar, depolama ve ağ gibi kaynaklar da dahil olmak üzere tüm karma altyapınızın sistem durumunu ve performansını izlemenize yardımcı olur.

- Yapılandırma olmadan Azure kaynaklarınızın çoğundan [platform ölçümleri, etkinlik günlükleri ve tanılama günlüklerini](platform/data-sources.md) otomatik olarak alırsınız.
- VM'ler için [Azure Monitor](insights/vminsights-overview.md)ile VM'ler için daha derin izleme yi etkinleştirin.
-  [Kapsayıcılar](insights/container-insights-overview.md)için Azure Monitor ile AKS kümeleri için daha derin izleme yi etkinleştirin.
- Ortamınızdaki farklı uygulamalar ve hizmetler için [izleme çözümleri](insights/solutions-inventory.md) ekleyin.


[Kod olarak altyapı,](/azure/devops/learn/what-is-infrastructure-as-code) DevOps ekiplerinin kaynak kodu için kullandığı sürümle aynı sürümü kullanarak açıklayıcı bir modeldeki altyapının yönetimidir. Ortamınıza güvenilirlik ve ölçeklenebilirlik ekler ve uygulamalarınızı yönetmek için kullanılan benzer işlemlerden yararlanmanızı sağlar.

-  Büyük bir kaynak kümesi üzerinde uyarıları izlemeyi etkinleştirmek ve yapılandırmak için [Kaynak Yöneticisi şablonlarını](platform/template-workspace-configuration.md) kullanın.
- Kaynaklarınız üzerinde farklı kurallar uygulamak için [Azure İlkesi'ni](../governance/policy/overview.md) kullanın. Bu, bu kaynakların kurumsal standartlarınıza ve hizmet düzeyi anlaşmalarınızla uyumlu kalmasını sağlar. 


##  <a name="combine-resources-in-azure-resource-groups"></a>Azure Kaynak Gruplarında kaynakları birleştirme
Azure'daki tipik bir uygulama bugün, Bulut Hizmetleri, AKS kümeleri veya Hizmet Kumaşı'nda barındırılan VM'ler ve Uygulama Hizmetleri veya mikro hizmetler gibi birden çok kaynağı içerir. Bu uygulamalar sık sık Olay Hub'ları, Depolama, SQL ve Hizmet Veri Meskeni gibi bağımlılıkları kullanır.

- Farklı uygulamalarınızı oluşturan tüm kaynaklarınızda tam görünürlük elde etmek için Azure Kaynak Gruplarındaki kaynakları birleştirin. [Kaynak Grupları için Azure Monitörü,](../azure-monitor/insights/resource-group-insights.md) tüm tam yığın uygulamanızın sistem durumunu ve performansını izlemek için basit bir yol sağlar ve herhangi bir araştırma veya hata ayıklama için ilgili bileşenlere sondaj yapılmasını sağlar.

## <a name="ensure-quality-through-continuous-deployment"></a>Sürekli Dağıtım ile kaliteyi sağlayın
Sürekli Tümleştirme / Sürekli Dağıtım, otomatik test sonuçlarına bağlı olarak uygulamanızda kod değişikliklerini otomatik olarak entegre etmenizi ve dağıtmanızı sağlar. Dağıtım işlemini kolaylaştırır ve üretime geçmeden önce değişikliklerin kalitesini sağlar.


- Sürekli Dağıtım uygulamak ve CI/CD testlerinize dayalı olarak kod commit'den üretime tüm işleminizi otomatikleştirmek için [Azure Ardışık Hatları'nı](/azure/devops/pipelines) kullanın.
- İzlemeyi dağıtım öncesi veya dağıtım sonrası nize entegre etmek için [Kalite Kapıları'nı](/azure/devops/pipelines/release/approvals/gates) kullanın. Bu, uygulamalarınız dev'den üretime geçerken ve altyapı ortamındaveya ölçekteki herhangi bir farklılık KP'lerinizi olumsuz etkilemediğinden, önemli sistem/performans ölçümlerini (KP'ler) karşıladığınızdan emin olur.
- Dev, Test, Kanarya ve Prod gibi farklı dağıtım ortamlarınız arasında [ayrı izleme örnekleri saklayın.](../azure-monitor/app/separate-resources.md) Bu, toplanan verilerin ilişkili uygulamalar ve altyapı arasında alakalı olmasını sağlar. Verileri ortamlar arasında ilişkilendirmeniz gerekiyorsa, [Metrics Explorer'da çok kaynak lı grafikler](../azure-monitor/platform/metrics-charts.md) kullanabilir veya Azure [Monitor'da çapraz kaynak sorguları](log-query/cross-workspace-query.md)oluşturabilirsiniz.


## <a name="create-actionable-alerts-with-actions"></a>Eylemlerle eyleme geçirilebilir uyarılar oluşturma
İzlemenin kritik bir yönü, yöneticilere güncel ve öngörülen sorunları proaktif olarak bildirmektir. 

- Öngörülebilir hata durumlarını belirlemek için günlükleri ve ölçümleri temel alan [Azure Monitor'da uyarılar](../azure-monitor/platform/alerts-overview.md) oluşturun. Tüm uyarıları eyleme geçirilebilir hale getirme amacınız olmalıdır, bu da gerçek kritik koşulları temsil eder ve yanlış pozitifleri azaltmaya çalışırlar. Kendi statik eşiklerinizi tanımlamak yerine metrik verilerdeki taban çizgilerini otomatik olarak hesaplamak için [Dinamik Eşikleri](platform/alerts-dynamic-thresholds.md) kullanın. 
- Yöneticilerinize bildirimde bulunmanın en etkili araçlarını kullanmak için uyarılar için eylemler tanımlayın. [Bildirim için](platform/action-groups.md#create-an-action-group-by-using-the-azure-portal) kullanılabilir eylemler SMS, e-postalar, anında iletme bildirimleri veya sesli aramalardır.
- [ITSM aracınıza](platform/itsmc-overview.md) veya diğer uyarı yönetim sistemlerine [webhooks](platform/activity-log-alerts-webhook.md)aracılığıyla bağlanmak için daha gelişmiş eylemler kullanın.
- [Azure Otomasyon runbook'ları](../automation/automation-webhooks.md) veya web hooks kullanarak bir uyarıdan başlatılabilen [Logic Apps](/connectors/custom-connectors/create-webhook-trigger) ile uyarılarda da tanımlanan durumları düzeltin. 
- Toplanan ölçümlere göre bilgi işlem kaynaklarınızı dinamik olarak artırmak ve azaltmak için [otomatik ölçekleme](../azure-monitor/learn/tutorial-autoscale-performance-schedule.md) kullanın.

## <a name="prepare-dashboards-and-workbooks"></a>Panolar ve çalışma kitapları hazırlama
Geliştirme ve işlemlerinizin aynı telemetri ve araçlara erişmesini sağlamak, tüm ortamınızdaki desenleri görüntülemelerine ve Algılama Süresinizi (MTTD) ve Geri Yükleme Süresi (MTTR) en aza indirmelerine olanak tanır.

- Kuruluşunuzdaki farklı roller için ortak ölçümlere ve günlüklere dayalı [özel panolar](../azure-monitor/learn/tutorial-app-dashboards.md) hazırlayın. Panolar tüm Azure kaynaklarından gelen verileri birleştirebilir.
- Geliştirme ve operasyonlar arasında bilgi paylaşımını sağlamak için [Çalışma Kitapları](../azure-monitor/app/usage-workbooks.md) hazırlayın. Bunlar, metrik grafikler ve günlük sorguları içeren dinamik raporlar olarak veya hatta müşteri desteğine veya işlemlerinde temel sorunları ele almaya yardımcı olan geliştiriciler tarafından hazırlanan sorun giderme kılavuzları olarak hazırlanabilir.

## <a name="continuously-optimize"></a>Sürekli olarak optimize etmek
 İzleme, KP'lerinizi ve kullanıcı davranış ölçümlerinizi sürekli olarak izlemenizi ve yinelemeleri planlayarak bunları optimize etmeye çabalamanızı öneren popüler Build-Measure-Learn felsefesinin temel yönlerinden biridir. Azure Monitor, işletmenizle ilgili ölçümler ve günlükler toplamanıza ve gerektiğinde bir sonraki dağıtıma yeni veri noktaları eklemenize yardımcı olur.

- [Son kullanıcı davranışını ve etkileşimini izlemek](../azure-monitor/learn/tutorial-users.md)için Uygulama Öngörüleri'ndeki araçları kullanın.
- Önemli KPM'lere gitmek için hangi alanlara odaklanabileceğinize öncelik vermek için [Etki Analizi'ni](../azure-monitor/app/usage-impact.md) kullanın.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Monitor'un](overview.md)fark bileşenleri hakkında bilgi edinin.
- Sürüm ardışık sisteminize [sürekli izleme ekleyin.](../azure-monitor/app/continuous-monitoring.md)
