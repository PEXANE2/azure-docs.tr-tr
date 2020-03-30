---
title: Azure Monitor çok kiracılı bir uygulamayla günlükler
description: Çok kiracılı Azure SQL Veritabanı SaaS uygulamasıyla Azure Monitor günlüklerini ayarlama ve kullanma
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: billgib
ms.date: 01/25/2019
ms.openlocfilehash: 7429a9d5e9a803f0e9a6f900c5d81e77e7477a48
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79214498"
---
# <a name="set-up-and-use-azure-monitor-logs-with-a-multitenant-sql-database-saas-app"></a>Çok kiracılı bir SQL Veritabanı SaaS uygulamasıyla Azure Monitor günlüklerini ayarlama ve kullanma

Bu eğitimde, elastik havuzları ve veritabanlarını izlemek için [Azure Monitor günlüklerini](/azure/log-analytics/log-analytics-overview) ayarlar ve kullanırsınız. Bu öğretici Performans [izleme ve yönetim öğretici](saas-dbpertenant-performance-monitoring.md)üzerine inşa edin. Azure portalında sağlanan izleme ve uyarıyı artırmak için Azure Monitor günlüklerinin nasıl kullanılacağını gösterir. Azure Monitor günlükleri binlerce esnek havuzun ve yüz binlerce veritabanının izlenmesini destekler. Azure Monitor günlükleri, farklı uygulamaların ve Azure hizmetlerinin izleme lerini birden çok Azure aboneliğine entegre edebilen tek bir izleme çözümü sağlar.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure Monitor günlüklerini yükleyin ve yapılandırır.
> * Havuzları ve veritabanlarını izlemek için Azure Monitor günlüklerini kullanın.

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip Tickets SaaS veritabanı başına kiracı uygulaması dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için [Wingtip Tickets SaaS veritabanı başına kiracı uygulamasını dağıt'a](saas-dbpertenant-get-started-deploy.md)bakın ve keşfedin.
* Azure PowerShell’in yüklendiğinden. Daha fazla bilgi için bkz. [Azure PowerShell kullanmaya başlayın](https://docs.microsoft.com/powershell/azure/get-started-azureps).

SaaS senaryoları ve desenleri ve bunların izleme çözümündeki gereksinimleri nasıl etkilediğini tartışmak için [Performans izleme ve yönetim öğreticisine](saas-dbpertenant-performance-monitoring.md) bakın.

## <a name="monitor-and-manage-database-and-elastic-pool-performance-with-azure-monitor-logs"></a>Azure Monitor günlükleriyle veritabanını ve esnek havuz performansını izleme ve yönetme

Azure SQL Veritabanı için, Azure portalındaki veritabanlarında ve havuzlarda izleme ve uyarı kullanılabilir. Bu yerleşik izleme ve uyarı kullanışlıdır, ancak kaynağa da özeldir. Bu, büyük yüklemeleri izlemek veya kaynaklar ve abonelikler arasında birleşik bir görünüm sağlamak için daha az uygun olduğu anlamına gelir.

Yüksek hacimli senaryolar için, izleme ve uyarı için Azure Monitor günlüklerini kullanabilirsiniz. Azure Monitor, bir çalışma alanında toplanmış günlükler üzerinden birçok hizmetten analitik sağlayan ayrı bir Azure hizmetidir. Azure Monitor günlükleri, operasyonel veri analizine izin veren yerleşik bir sorgu dili ve veri görselleştirme araçları sağlar. SQL Analytics çözümü, önceden tanımlanmış birkaç elastik havuz ve veritabanı izleme ve uyarı görünümleri ve sorguları sağlar. Azure Monitor günlükleri ayrıca özel bir görünüm tasarımcısı da sağlar.

OMS çalışma alanları artık Log Analytics çalışma alanları olarak adlandırılır. Azure portalında Log Analytics çalışma alanları ve analiz çözümleri açılır. Azure portalı yeni erişim noktasıdır, ancak bazı alanlarda Kiİşlemler Yönetimi Paketi portalının arkasında ne olabilir.

### <a name="create-performance-diagnostic-data-by-simulating-a-workload-on-your-tenants"></a>Kiracılarınızdaki iş yükünü simüle ederek performans tanılama verileri oluşturun 

1. PowerShell ISE, açık *... WingtipTicketsSaaS-MultiTenantDb-master\\Öğrenme\\Modülleri Performans\\İzleme ve Yönetim Demo-PerformanceMonitoringAndManagement.ps1 . \\* Bu öğretici sırasında yük oluşturma senaryolarından birkaçını çalıştırmak isteyebileceğinden bu komut dosyasını açık tutun.
1. Bunu daha önce yapmadıysanız, izleme bağlamını daha ilginç hale getirmek için bir yığın kiracı sağlayabilir. Bu işlem birkaç dakika sürer.

   a. Set **$DemoScenario = 1**, Madde kiracı bir _toplu_.

   b. Komut dosyasını çalıştırmak ve ek 17 kiracı dağıtmak için F5 tuşuna basın.

1. Şimdi tüm kiracı üzerinde simüle yük çalıştırmak için yük jeneratörü başlatın.

    a. Set **$DemoScenario = 2**, Normal yoğunluk yükü _(yaklaşık 30 DTU) oluşturun._

    b. Komut dosyasını çalıştırmak için F5 tuşuna basın.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Wingtip Tickets SaaS veritabanı-kiracı başına uygulama komut dosyalarını edinin

Wingtip Biletleri SaaS çok kiracılı veritabanı komut dosyaları ve uygulama kaynak kodu [WingtipTicketsSaaS-DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub repo mevcuttur. Wingtip Tickets PowerShell komut dosyalarını indirmek ve engelini kaldırmak için [genel kılavuza](saas-tenancy-wingtip-app-guidance-tips.md)bakın.

## <a name="install-and-configure-log-analytics-workspace-and-the-azure-sql-analytics-solution"></a>Log Analytics çalışma alanını ve Azure SQL Analytics çözümlerini yükleme ve yapılandırma

Azure Monitor, yapılandırılması gereken ayrı bir hizmettir. Azure Monitor günlükleri, günlük verileri, telemetri ve ölçümleri bir Log Analytics çalışma alanında toplar. Azure'daki diğer kaynaklar gibi, Bir Günlük Analizi çalışma alanı oluşturulmalıdır. Çalışma alanının, izlediği uygulamalarla aynı kaynak grubunda oluşturulması gerekmez. Bunu yapmak sık sık olsa en mantıklı. Wingtip Tickets uygulaması için, uygulamayla birlikte çalışma alanının silindiğinden emin olmak için tek bir kaynak grubu kullanın.

1. PowerShell ISE, açık *... WingtipTicketsSaaS-MultiTenantDb-master\\Öğrenme\\Modülleri Performans\\İzleme ve\\Yönetim Günlüğü Analytics Demo-LogAnalytics.ps1 . \\*
1. Komut dosyasını çalıştırmak için F5 tuşuna basın.

Artık Azure portalında Azure Monitor günlüklerini açabilirsiniz. Log Analytics çalışma alanında telemetri toplamak ve görünür hale getirmek birkaç dakika sürer. Tanılama verilerini toplayarak sistemden ne kadar uzun süre ayrılırsanız, deneyim o kadar ilginç olur. 

## <a name="use-log-analytics-workspace-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Havuzları ve veritabanlarını izlemek için Log Analytics çalışma alanını ve SQL Analytics çözümünü kullanın


Bu alıştırmada, veritabanları ve havuzlar için toplanan telemetriye bakmak için Azure portalındaki Log Analytics çalışma alanını açın.

1. [Azure portalına](https://portal.azure.com)göz atın. Log Analytics çalışma alanını açmak için **tüm hizmetleri** seçin. Ardından Log Analytics'i arayın.

   ![Günlük Analizini Aç çalışma alanı](media/saas-dbpertenant-log-analytics/log-analytics-open.png)

1. _&lt;wtploganalytics- kullanıcı&gt;_ adlı çalışma alanını seçin.

1. Azure portalında günlük analizi çözümünü açmak için **Genel Bakış'ı** seçin.

   ![Genel Bakış](media/saas-dbpertenant-log-analytics/click-overview.png)

    > [!IMPORTANT]
    > Çözümün etkin olması birkaç dakika sürebilir. 

1. Açmak için **Azure SQL Analytics** döşemesini seçin.

    ![Genel bakış döşemesi](media/saas-dbpertenant-log-analytics/overview.png)

1. Çözümdeki görünümler, alt kısmında kendi iç kaydırma çubuğuyla yan lara doğru kaydırılır. Gerekirse sayfayı yenileyin.

1. Özet sayfasını keşfetmek için, ayrıntılı bir gezgin açmak için kutucukları veya tek tek veritabanlarını seçin.

    ![Günlük analiz panosu](media/saas-dbpertenant-log-analytics/log-analytics-overview.png)

1. Zaman aralığını değiştirmek için filtre ayarını değiştirin. Bu öğretici için **Son 1 saat'i**seçin.

    ![Zaman filtresi](media/saas-dbpertenant-log-analytics/log-analytics-time-filter.png)

1. Sorgu kullanımını ve bu veritabanının ölçümlerini keşfetmek için tek bir veritabanı seçin.

    ![Veritabanı analitiği](media/saas-dbpertenant-log-analytics/log-analytics-database.png)

1. Kullanım ölçümlerini görmek için analitik sayfasını sağa kaydırın.
 
     ![Veritabanı ölçümleri](media/saas-dbpertenant-log-analytics/log-analytics-database-metrics.png)

1. Analiz sayfasını sola kaydırın ve **Kaynak Bilgileri** listesindeki sunucu döşemesini seçin.  

    ![Kaynak Bilgileri listesi](media/saas-dbpertenant-log-analytics/log-analytics-resource-info.png)

    Sunucudaki havuzları ve veritabanlarını gösteren bir sayfa açılır.

    ![Havuzlu ve veritabanları olan sunucu](media/saas-dbpertenant-log-analytics/log-analytics-server.png)

1. Bir havuz seçin. Açılan havuz sayfasında, havuz ölçümlerini görmek için sağa kaydırın. 

    ![Havuz ölçümleri](media/saas-dbpertenant-log-analytics/log-analytics-pool-metrics.png)


1. Log Analytics çalışma alanında, çalışma alanını açmak için **OMS Portal'ı** seçin.

    ![Log Analytics çalışma alanı](media/saas-dbpertenant-log-analytics/log-analytics-workspace-oms-portal.png)

Log Analytics çalışma alanında, günlük ve metrik verileri daha fazla keşfedebilirsiniz. 

Azure Monitor günlüklerinde izleme ve uyarı, Azure portalındaki her kaynakta tanımlanan uyarının aksine, çalışma alanındaki veriler üzerindeki sorgulara dayanır. Uyarıları sorgulara dayandırarak, veritabanı başına bir tane tanımlamak yerine tüm veritabanlarına bakan tek bir uyarı tanımlayabilirsiniz. Sorgular yalnızca çalışma alanında bulunan verilerle sınırlıdır.

Sorgu yapmak ve uyarıları ayarlamak için Azure Monitor günlüklerini nasıl kullanacağınız hakkında daha fazla bilgi için Azure [Monitor günlüklerinde uyarı kurallarıyla Çalışma](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts-creating)'ya bakın.

Azure Monitor, çalışma alanındaki veri hacmine bağlı olarak SQL Veritabanı ücretleri için günlükler. Bu eğitimde, günde 500 MB ile sınırlı ücretsiz bir çalışma alanı oluşturdunuz. Bu sınıra ulaşıldıktan sonra, veriler artık çalışma alanına eklenmez.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Monitor günlüklerini yükleyin ve yapılandırır.
> * Havuzları ve veritabanlarını izlemek için Azure Monitor günlüklerini kullanın.

Tenant [analitik öğreticisini](saas-dbpertenant-log-analytics.md)deneyin.

## <a name="additional-resources"></a>Ek kaynaklar

* [İlk Wingtip Tickets SaaS veritabanı-kiracı başına uygulama dağıtımı üzerine inşa ek öğreticiler](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure Monitör günlükleri](../azure-monitor/insights/azure-sql.md)
