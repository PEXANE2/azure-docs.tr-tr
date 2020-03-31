---
title: Azure güvenlik günlüğü ve denetimi | Microsoft Dokümanlar
description: Azure'da bulunan günlükler ve elde edebilirsiniz güvenlik istatistikleri hakkında bilgi edinin.
services: security
documentationcenter: na
author: TerryLanfear
manager: rkarlin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2019
ms.author: terrylan
ms.openlocfilehash: bd0f42507e22559690e2682a391c53b9c090aa6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75750797"
---
# <a name="azure-security-logging-and-auditing"></a>Azure güvenlik günlüğü ve denetimi

Azure, güvenlik ilkelerive mekanizmalarınızdaki boşlukları belirlemenize yardımcı olmak için çok çeşitli yapılandırılabilir güvenlik denetimi ve günlüğe kaydetme seçenekleri sunar. Bu makalede, Azure'da barındırılan hizmetlerden güvenlik günlükleri oluşturma, toplama ve çözümleme açıklanmaktadır.

> [!Note]
> Bu makaledeki bazı öneriler veri, ağ veya bilgi işlem kaynağı kullanımının artmasına ve lisans ınızın veya abonelik maliyetlerinin artmasına neden olabilir.

## <a name="types-of-logs-in-azure"></a>Azure'daki günlük türleri

Bulut uygulamaları birçok hareketli parçayla karmaşıktır. Günlük verileri uygulamalarınız hakkında öngörüler sağlayabilir ve size yardımcı olabilir:

- Geçmiş sorunları giderme veya olası sorunları önleme
- Uygulama performansını veya bakımı geliştirin
- Aksi takdirde el ile müdahale gerektiren eylemleri otomatikleştirin

Azure günlükleri aşağıdaki türlere ayrılır:
* **Denetim/yönetim günlükleri** Azure Kaynak Yöneticisi OLUŞTURMA, GÜNCELLEME ve DELETE işlemleri hakkında bilgi sağlar. Daha fazla bilgi için [Azure etkinlik günlüklerine](../../azure-monitor/platform/platform-logs-overview.md)bakın.

* **Veri düzlemi günlükleri,** Azure kaynak kullanımının bir parçası olarak ortaya çıkan olaylar hakkında bilgi sağlar. Bu tür günlüklere örnek olarak, sanal makinedeki (VM) Windows olay sistemi, güvenlik ve uygulama günlükleri ve Azure Monitor aracılığıyla yapılandırılan [tanılama günlükleri](../../azure-monitor/platform/platform-logs-overview.md) verilebilir.

* **İşlenen olaylar,** sizin adınıza işlenen analiz edilen olaylar/uyarılar hakkında bilgi sağlar. Bu türe örnek olarak, [Azure Güvenlik Merkezi'nin](../../security-center/security-center-intro.md) aboneliğinizi işlediği ve analiz ettiği ve kısa güvenlik uyarıları sağladığı Azure Güvenlik [Merkezi uyarıları](../../security-center/security-center-managing-and-responding-alerts.md) verilebilir.

Aşağıdaki tablo, Azure'da bulunan en önemli günlük türlerini listeler:

| Günlük kategorisi | Günlük türü | Kullanım | Tümleştirme |
| ------------ | -------- | ------ | ----------- |
|[Etkinlik günlükleri](../../azure-monitor/platform/platform-logs-overview.md)|Azure Kaynak Yöneticisi kaynaklarında denetim düzlemi olayları|  Aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar.|    Dinlenme API'si, [Azure Monitör](../../azure-monitor/platform/platform-logs-overview.md)|
|[Azure Kaynak günlükleri](../../azure-monitor/platform/platform-logs-overview.md)|Azure Kaynak Yöneticisi kaynaklarının abonelikte çalışması yla ilgili sık kullanılan veriler|   Kaynağınızın kendisinin gerçekleştirdiği işlemler hakkında bilgi sağlar.| Azure İzleyici|
|[Azure Active Directory raporlama](../../active-directory/reports-monitoring/overview-reports.md)|Günlükler ve raporlar | Kullanıcı oturum açma etkinliklerini ve kullanıcılar ve grup yönetimi hakkındaki sistem etkinlik bilgilerini raporlar.|[Grafik API](../../active-directory/develop/active-directory-graph-api-quickstart.md)|
|[Sanal makineler ve bulut hizmetleri](../../azure-monitor/learn/quick-collect-azurevm.md)|Windows Olay Günlüğü hizmeti ve Linux Syslog|  Sanal makinelerdeki sistem verilerini ve günlük verilerini yakalar ve bu verileri seçtiğiniz bir depolama hesabına aktarın.|   Azure Monitör'de Windows (Windows Azure Diagnostics [[WAD](../../monitoring-and-diagnostics/azure-diagnostics.md)] depolama alanı kullanma) ve Linux|
|[Azure Depolama Analitiği](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)|Depolama günlüğü, bir depolama hesabı için metrik verileri sağlar|İzleme istekleri hakkında bilgi sağlar, kullanım eğilimlerini analiz eder ve depolama hesabınızla ilgili sorunları tanılar.|   REST API veya [istemci kitaplığı](https://msdn.microsoft.com/library/azure/mt347887.aspx)|
|[Ağ güvenlik grubu (NSG) akış günlükleri](../../network-watcher/network-watcher-nsg-flow-logging-overview.md)|JSON biçimi, kural başına olarak giden ve gelen akışları gösterir|Ağ Güvenlik Grubu aracılığıyla giriş ve çıkış IP trafiği hakkındaki bilgileri görüntüler.|[Azure Ağ İzleyicisi](../../network-watcher/network-watcher-monitoring-overview.md)|
|[Uygulama öngörüsü](../../azure-monitor/app/app-insights-overview.md)|Günlükler, özel durumlar ve özel tanılama|  Birden çok platformda web geliştiricileri için bir uygulama performans izleme (APM) hizmeti sağlar.| REST API, [Güç BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)|
|[İşlem verileri / güvenlik uyarıları](../../security-center/security-center-intro.md)|  Azure Güvenlik Merkezi uyarıları, Azure Monitor günlükleri uyarıları|    Güvenlik bilgileri ve uyarılar sağlar.|  REST API'ler, JSON|

## <a name="log-integration-with-on-premises-siem-systems"></a>Şirket içi SIEM sistemleriyle günlük entegrasyonu
[Güvenlik Merkezi uyarılarını tümleştirme,](../../security-center/security-center-export-data-to-siem.md) Güvenlik Merkezi uyarılarını, Azure tanılama günlükleri tarafından toplanan sanal makine güvenlik olaylarını ve Azure Monitör günlükleriniz veya SIEM çözümünüzle Azure denetim günlüklerini nasıl eşitlemeyi tartışır.

## <a name="next-steps"></a>Sonraki adımlar

- [Denetim ve günlüğe kaydetme](management-monitoring-overview.md): Görünürlüğü koruyarak ve zamanında güvenlik uyarılarına hızlı bir şekilde yanıt vererek verileri koruyun.

- [Azure'da güvenlik günlüğü ve denetim günlüğü koleksiyonu](https://azure.microsoft.com/resources/videos/security-logging-and-audit-log-collection/): Azure örneklerinizin doğru güvenlik ve denetim günlüklerini topladığından emin olmak için bu ayarları uygulayın.

- [Bir site koleksiyonu için denetim ayarlarını yapılandırın](https://support.office.com/article/Configure-audit-settings-for-a-site-collection-A9920C97-38C0-44F2-8BCB-4CF1E2AE22D2?ui=&rs=&ad=US): Bir site koleksiyonu yöneticisiyseniz, tek tek kullanıcıların eylemlerinin geçmişini ve belirli bir tarih aralığında gerçekleştirilen eylemlerin geçmişini alın.

- [Office 365 Güvenlik & Uyumluluk Merkezi'ndeki denetim günlüğünde arama yapın](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=&rs=&ad=US): Office 365 Güvenlik & Uyumluluk Merkezi'ni kullanarak Office 365 kuruluşunuzdaki birleşik denetim günlüğünü ve kullanıcı ve yönetici etkinliğini görüntüleyin.
