---
title: Azure Uygulama Öngörüleri Telemetri Veri Modeli - Telemetri Bağlamı | Microsoft Dokümanlar
description: Uygulama Öngörüleri telemetri bağlam veri modeli
ms.topic: conceptual
ms.date: 05/15/2017
ms.reviewer: sergkanz
ms.openlocfilehash: 8a2e3296643b13a54c4fceb11f044a2808cf2877
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671872"
---
# <a name="telemetry-context-application-insights-data-model"></a>Telemetri bağlamı: Application Insights veri modeli

Her telemetri öğesi güçlü bir şekilde yazılan bağlam alanları olabilir. Her alan belirli bir izleme senaryosu sağlar. Özel veya uygulamaya özgü bağlamsal bilgileri depolamak için özel özellikler koleksiyonunu kullanın.


## <a name="application-version"></a>Uygulama sürümü

Uygulama bağlamında alandaki bilgiler her zaman telemetriyi gönderen uygulama yla ilgilidir. Uygulama sürümü, uygulama davranışındaki eğilim değişikliklerini ve dağıtımlarla olan korelasyonunu çözümlemek için kullanılır.

Maksimum uzunluk: 1024


## <a name="client-ip-address"></a>İstemci IP adresi

İstemci aygıtının IP adresi. IPv4 ve IPv6 desteklenir. Bir hizmetten telemetri gönderildiğinde, konum bağlamı hizmette işlemi başlatan kullanıcıyla ilgilidir. Application Insights, coğrafi konum bilgilerini istemci IP'den ayıklar ve sonra da küçülttü. Bu nedenle istemci IP tek başına son kullanıcı tanımlanabilir bilgi olarak kullanılamaz. 

Maksimum uzunluk: 46


## <a name="device-type"></a>Cihaz Türü

Başlangıçta bu alan, uygulamanın son kullanıcının kullandığı aygıtın türünü belirtmek için kullanılmıştır. Bugün öncelikle cihaz türü 'Tarayıcı' ile javascript telemetri ayırt etmek için kullanılan cihaz türü 'PC' ile sunucu tarafı telemetri.

Maksimum uzunluk: 64


## <a name="operation-id"></a>İşlem kimliği

Kök işleminin benzersiz bir tanımlayıcısı. Bu tanımlayıcı, telemetriyi birden çok bileşen arasında gruplandırmaya olanak tanır. Ayrıntılar için [telemetri korelasyonuna](../../azure-monitor/app/correlation.md) bakın. İşlem kimliği bir istek veya sayfa görünümü tarafından oluşturulur. Diğer tüm telemetriler bu alanı içeren istek veya sayfa görünümü değerine ayarlar. 

Maksimum uzunluk: 128


## <a name="parent-operation-id"></a>Üst işlem kimliği

Telemetri öğesinin yakın ebeveyninin benzersiz tanımlayıcısı. Ayrıntılar için [telemetri korelasyonuna](../../azure-monitor/app/correlation.md) bakın.

Maksimum uzunluk: 128


## <a name="operation-name"></a>İşlem adı

İşlemin adı (grubu). İşlem adı, bir istek veya sayfa görünümü tarafından oluşturulur. Diğer tüm telemetri öğeleri bu alanı içeren istek veya sayfa görünümü değerine ayarlar. İşlem adı, bir operasyon grubu için tüm telemetri öğelerini bulmak için kullanılır (örneğin,'GET Home/Index'). Bu bağlam özelliği , "bu sayfada atılan tipik özel durumlar nelerdir" gibi soruları yanıtlamak için kullanılır.

Maksimum uzunluk: 1024


## <a name="synthetic-source-of-the-operation"></a>İşlemin sentetik kaynağı

Sentetik kaynağın adı. Uygulamadan bazı telemetri sentetik trafik temsil edebilir. Web sitesini, site kullanılabilirlik testlerini veya Application Insights SDK'nın kendisi gibi tanı kitaplıklarından gelen izleri dizine ekleyen web tarayıcısı olabilir.

Maksimum uzunluk: 1024


## <a name="session-id"></a>Oturum kimliği

Oturum Kimliği - kullanıcının uygulamayla etkileşimi örneği. Oturum bağlamı alanlarındaki bilgiler her zaman son kullanıcı yla ilgilidir. Bir hizmetten telemetri gönderildiğinde, oturum bağlamı hizmette işlemi başlatan kullanıcıyla ilgilidir.

Maksimum uzunluk: 64


## <a name="anonymous-user-id"></a>Anonim kullanıcı kimliği

Anonim kullanıcı kimliği. Uygulamanın son kullanıcısını temsil eder. Bir hizmetten telemetri gönderildiğinde, kullanıcı bağlamı hizmette işlemi başlatan kullanıcıyla ilgilidir.

[Örnekleme](../../azure-monitor/app/sampling.md) toplanan telemetri miktarını en aza indirmek için tekniklerden biridir. Örnekleme algoritması, ilişkili tüm telemetrinin içinde veya dışında örnekleme yapmaya çalışır. Anonim kullanıcı kimliği örnekleme puanı üretimi için kullanılır. Yani anonim kullanıcı kimliği rasgele yeterli değer olmalıdır. 

Kullanıcı adını depolamak için anonim kullanıcı kimliği kullanmak alanın kötüye kullanımıdır. Kimlik doğrulaması kullanıcı kimliğini kullanın.

Maksimum uzunluk: 128


## <a name="authenticated-user-id"></a>Kimlik doğrulaması kullanıcı kimliği

Kimlik doğrulaması kullanıcı kimliği. Anonim kullanıcı kimliğinin tersi, bu alan kullanıcıyı dost bir ada sahip temsil eder. Kişisel bilgileri çoğu SDK tarafından varsayılan olarak toplanmadığından.

Maksimum uzunluk: 1024


## <a name="account-id"></a>Hesap kimliği

Çok kiracılı uygulamalarda bu, kullanıcının hareket ettiği hesap kimliği veya adıdır. Örnekler, Azure portalı nın abonelik kimliği veya bir blog platformu için blog adı olabilir.

Maksimum uzunluk: 1024


## <a name="cloud-role"></a>Bulut rolü

Uygulamanın bir parçası olduğu rolün adı. Azure'daki rol adı yla doğrudan eşler. Ayrıca, tek bir uygulamanın parçası olan mikro hizmetleri ayırt etmek için de kullanılabilir.

Maksimum uzunluk: 256


## <a name="cloud-role-instance"></a>Bulut rolü örneği

Uygulamanın çalıştığı örneğin adı. Şirket içi bilgisayar adı, Azure için örnek adı.

Maksimum uzunluk: 256


## <a name="internal-sdk-version"></a>Dahili: SDK sürümü

SDK versiyonu. Bilgi için [bu makaleye](https://github.com/microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/SDK-VERSIONS.md) bakın.

Maksimum uzunluk: 64


## <a name="internal-node-name"></a>İç: Düğüm adı

Bu alan, faturalandırma amacıyla kullanılan düğüm adını temsil eder. Düğümlerin standart algılamasını geçersiz kılmak için kullanın.

Maksimum uzunluk: 256


## <a name="next-steps"></a>Sonraki adımlar

- [Telemetriyi nasıl genişletip filtreleyeceklerini](../../azure-monitor/app/api-filtering-sampling.md)öğrenin.
- Uygulama Öngörüleri türleri ve veri modeli için [veri modeline](data-model.md) bakın.
- Standart bağlam özellikleri toplama [yapılandırması](../../azure-monitor/app/configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet)göz atın.
