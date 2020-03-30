---
title: Azure Monitör ölçümleri ve otomatik ölçeklendirme için klasik dağıtım modeli API'lerinin emekliye ayrılması
description: Azure Hizmet Yönetimi (ASM) veya RDFE dağıtım modeli olarak da adlandırılan ölçümler ve otomatik ölçeklendirme klasik API'leri kullanımdan kaldırılıyor
ms.subservice: ''
ms.topic: conceptual
ms.date: 11/19/2018
ms.openlocfilehash: 9dfa6b278587f4ed79b1c3cd9eff1defd09ec0bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294650"
---
# <a name="azure-monitor-retirement-of-classic-deployment-model-apis-for-metrics-and-autoscale"></a>Azure Monitör ölçümleri ve otomatik ölçeklendirme için klasik dağıtım modeli API'lerinin emekliye ayrılması

Azure Monitor (eski adıyla Azure Öngörüleri) şu anda klasik VM'ler ve klasik Bulut Hizmetleri ölçümleri için otomatik ölçeklendirme ayarları oluşturma ve yönetme olanağı sunar. Klasik dağıtım modeli tabanlı API'lerin özgün kümesi, tüm bölgelerdeki tüm Azure genel ve özel bulutlarda **30 Haziran 2019'dan sonra kullanımdan kaldırılacaktır.**   

Aynı işlemler, bir yıldan uzun süredir Azure Kaynak Yöneticisi tabanlı API'ler kümesi aracılığıyla desteklenmiştir. Azure portalı, hem otomatik ölçeklendirme hem de ölçümler için yeni REST API'lerini kullanır. Bu Kaynak Yöneticisi API'lerine dayalı yeni bir SDK, PowerShell ve CLI de mevcuttur. İzleme için iş ortağı hizmetlerimiz, Azure Monitor'daki yeni Kaynak Yöneticisi tabanlı REST API'lerini tüketir.  

## <a name="who-is-not-affected"></a>Kimler etkilenmez

Azure portalı, [yeni Azure Monitor SDK,](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/)PowerShell, CLI veya Kaynak Yöneticisi şablonları üzerinden otomatik ölçeklendirme yönetiyorsanız, herhangi bir işlem yapılması gerekmez.  

Azure portalı veya çeşitli [izleme ortağı hizmetleri](../../azure-monitor/platform/partners.md)aracılığıyla ölçümler tüketiyorsanız, herhangi bir işlem yapılması gerekmez. Microsoft, yeni API'lere geçiş yapmak için izleme ortaklarıyla birlikte çalışmaktadır.

## <a name="who-is-affected"></a>Kimler etkilenir?

Aşağıdaki bileşenleri kullanıyorsanız, bu makale sizin için geçerlidir:

- **Klasik Azure Öngörüleri SDK** - [Klasik Azure Öngörüleri SDK](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring/)kullanıyorsanız, [.NET](https://github.com/azure/azure-libraries-for-net#download) veya [Java](https://github.com/azure/azure-libraries-for-java#download)için yeni Azure Monitor SDK'sını kullanmaya geçin. Azure [MonitörSI SDK NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/)indirin.

- **Klasik Otomatik Ölçeklendirme** - Özel olarak oluşturulmuş araçlarınızdan [klasik otomatik ölçek ayarları API'lerini](https://msdn.microsoft.com/library/azure/mt348562.aspx) arıyorsanız veya klasik Azure [Öngörüleri SDK'yı](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring/)kullanıyorsanız, [Kaynak Yöneticisi Azure Monitor REST API'yi](https://docs.microsoft.com/rest/api/monitor/autoscalesettings)kullanmaya geçmelisiniz.

- **Klasik Ölçümler** - Özel olarak oluşturulmuş araçlardan [klasik REST API'lerini](https://msdn.microsoft.com/library/azure/dn510374.aspx) veya klasik Azure [Öngörüleri SDK'sını](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring/) kullanarak ölçümler tüketiyorsanız, [Kaynak Yöneticisi Azure Monitor REST API'yi](https://docs.microsoft.com/rest/api/monitor/autoscalesettings)kullanmaya geçmelisiniz. 

Kod veya özel araçlarınızın klasik API'leri çağırıp çağırmadığından emin değilseniz, aşağıdakiler göz atın:

- Kodunuzda veya aracınızda başvurulan URI'yi gözden geçirin. Klasik API'ler https://management.core.windows.netURI'yi kullanır. Kaynak Yöneticisi tabanlı API'ler için yeni URI `https://management.azure.com/`kullanıyor olmalısınız.

- Makinenizdeki montaj adını karşılaştırın. Eski klasik montaj https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Monitoring/.

- Ölçümlere erişmek veya API'lere erişmek için sertifika kimlik doğrulaması kullanıyorsanız, klasik bir bitiş noktası ve kitaplık kullanıyorsunuz. Yeni Kaynak Yöneticisi API'leri, bir hizmet yöneticisi veya kullanıcı ilkesi aracılığıyla Azure Etkin Dizin kimlik doğrulaması gerektirir.

- Aşağıdaki bağlantılardan herhangi birinde belgelerde başvurulan çağrıları kullanıyorsanız, eski klasik API'leri kullanıyorsunuz.

  - [Windows.Azure.Management.Monitoring Class Kitaplığı](https://docs.microsoft.com/previous-versions/azure/dn510414(v=azure.100))

  - [İzleme (klasik) .NET](https://docs.microsoft.com/previous-versions/azure/reference/mt348562(v%3dazure.100))

  - [IMetricOperations Arayüzü](https://docs.microsoft.com/previous-versions/azure/reference/dn802395(v%3dazure.100))

## <a name="why-you-should-switch"></a>Neden geçiş yapmalısınız?

Otomatik ölçeklendirme ve ölçümler için varolan tüm yetenekler yeni API'ler aracılığıyla çalışmaya devam edecektir.  

Yeni API'lere geçiş, tüm izleme hizmetlerinizde tutarlı Rol Tabanlı Erişim Denetimi (RBAC) desteği gibi Kaynak Yöneticisi tabanlı özelliklerle birlikte gelir. Ayrıca ölçümler için ek işlevsellik kazanırsınız: 

- boyutlar için destek
- tüm hizmetler de tutarlı 1 dakikalık metrik parçalılık 
- daha iyi sorgulama
- daha yüksek veri saklama (93 günlük ölçümler vs. 30 gün) 

Genel olarak, Azure'daki diğer tüm hizmetler gibi, Kaynak Yöneticisi tabanlı Azure Monitör API'leri daha iyi performans, ölçeklenebilirlik ve güvenilirlikle birlikte gelir. 

## <a name="what-happens-if-you-do-not-migrate"></a>Göç etmezseniz ne olur?

### <a name="before-retirement"></a>Emeklilik öncesi

Azure hizmetleriniz veya iş yüklerinin doğrudan bir etkisi olmayacaktır.  

### <a name="after-retirement"></a>Emekli olduktan sonra

Daha önce listelenen klasik API'lere yapılan tüm aramalar başarısız olur ve aşağıdakilere benzer hata iletileri döndürecektir:

Otomatik ölçeklendirme için: *Bu API amortismana hazırdır. Otomatik Ölçeklendirme Ayarlarını yönetmek için Azure portalını, Azure Monitörü SDK'sını, PowerShell'i, CLI'yi veya Kaynak Yöneticisi şablonlarını kullanın.*  

Ölçümler için: *Bu API amortismana kaldırıldı. Ölçümleri sorgulamak için Azure portalını, Azure Monitor SDK'yı, PowerShell'i, CLI'yi kullanın.*

## <a name="email-notifications"></a>E-posta bildirimleri

Aşağıdaki hesap rolleri için e-posta adreslerine bir emeklilik bildirimi gönderildi: 

- Hesap ve hizmet yöneticileri
- Yardımcı Yöneticiler  

Herhangi bir sorunuz varsa, MonitorClassicAPIhelp@microsoft.combize ulaşın .  

## <a name="references"></a>Başvurular

- [Azure Monitör için yeni REST API'leri](https://docs.microsoft.com/rest/api/monitor/) 
- [Yeni Azure Monitör SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/)
