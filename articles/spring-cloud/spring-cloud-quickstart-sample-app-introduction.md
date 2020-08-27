---
title: Hızlı başlangıç-Piggy ölçümleri örnek uygulamasına giriş-Azure yay bulutu
description: Azure Spring Cloud dağıtımında kullanılan Piggy ölçümleri örnek uygulamasını açıklar.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 08/03/2020
ms.custom: devx-track-java
ms.openlocfilehash: d833c8f136a71d563ce10240f03e2c68e9131687
ms.sourcegitcommit: e69bb334ea7e81d49530ebd6c2d3a3a8fa9775c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88951897"
---
# <a name="introduction-to-piggy-metrics-sample-app"></a>Piggy ölçümleri örnek uygulamasına giriş

Bu hızlı başlangıçta, Azure Spring Cloud Service 'e bir uygulama dağıtmayı göstermek için Piggy ölçümleri adlı bir kavram kanıtı kişisel finans örneği kullanacağız. Piggy ölçümleri, mikro hizmet mimarisi modelini gösterir ve aşağıdaki bölümlerde hizmetler dökümü vurgulanacak. Hizmet bulma, yapılandırma sunucusu 'ndan Günlükler, ölçümler ve dağıtılmış izleme 'den güvenli bir şekilde kullanıma hazır Azure Spring Cloud özellikleri ile Azure 'a nasıl dağıtıldığını öğreneceksiniz.

Azure yay bulutu dağıtım örneklerini izlemek için, yalnızca kaynak kodun gerekli olduğu şekilde sağlandığı konuma ihtiyacınız vardır.

## <a name="functional-services"></a>İşlevsel hizmetler
Piggy ölçümleri üç çekirdekli mikro hizmette bulunur. Bunların hepsi, iş etki alanları etrafında düzenlenmiş bağımsız olarak dağıtılabilir uygulamalardır.

* **Hesap hizmeti (dağıtılacak)**: genel kullanıcı giriş mantığını ve doğrulamayı içerir: giriş/gider öğeleri, tasarrufları ve hesap ayarları.
* **İstatistik hizmeti (Bu hızlı başlangıçta kullanılmıyor)**: büyük istatistik parametrelerinde hesaplamalar yapar ve her hesap için zaman serisini yakalar. DataPoint temel para birimine ve zaman dilimine göre normalleştirilmiş değerler içerir. Bu veriler nakit akışı Dynamics 'in hesap ömrünü izlemek için kullanılır.
* **Bildirim hizmeti (Bu hızlı başlangıçta kullanılmıyor)**: kullanıcıya, hatırlat ve yedekleme sıklığı gibi iletişim bilgilerini ve bildirim ayarlarını depolar. Zamanlanan çalışan, diğer hizmetlerden gerekli bilgileri toplar ve abone olunan müşterilere e-posta iletileri gönderir.

## <a name="infrastructure-services"></a>Altyapı Hizmetleri
Dağıtılmış sistemlerde çekirdek hizmetlerinin çalışmasını sağlamaya yardımcı olan birkaç ortak desen vardır. Azure yay bulutu, bu desenleri uygulamak için Spring Boot uygulamaları davranışını geliştiren güçlü araçlar sağlar: 

* **Yapılandırma hizmeti (Azure Spring Cloud tarafından barındırılan)**: Azure Spring Cloud config, dağıtılmış sistemler için yatay olarak ölçeklenebilir bir merkezi yapılandırma hizmetidir. Bu, şu anda yerel depolama, git ve alt sürümü destekleyen bir takılabilir depo kullanır.
* **Hizmet bulma (Azure yay bulutu tarafından barındırılan)**: otomatik ölçeklendirme, arızalar ve yükseltmeler nedeniyle dinamik olarak atanan adreslere sahip olabilecek hizmet örnekleri için ağ konumlarının otomatik olarak algılanmasını sağlar.
* **Kimlik doğrulama hizmeti (dağıtılacak)** Yetkilendirme sorumlulukları, arka uç kaynak hizmetleri için OAuth2 belirteçleri veren ayrı bir sunucuya tamamen ayıklanır. Auth Server, kullanıcı yetkilendirmesi ve çevre içinde makineden makineye iletişim güvenliğini sağlar.
* **API Gateway (dağıtılacak)**: üç çekirdekli hizmet, istemciye BIR dış API sunar. Gerçek dünyada sistemlerde, işlevlerin sayısı sistem karmaşıklığıyla çok daha hızlı büyüyebilir. Yüzlerce hizmet, karmaşık bir Web sayfasının işlenmesiyle ilgili olabilir. API ağ geçidi, istekleri işlemek ve bunları uygun arka uç hizmetine yönlendirmek veya birden fazla arka uç hizmetini çağırmak için kullanılan ve sonuçları toplayarak, sisteme tek bir giriş noktasıdır. 

## <a name="sample-usage-of-piggy-metrics"></a>Piggy ölçümlerini örnek olarak kullanma
Tam uygulama ayrıntıları için bkz. [Piggy ölçümleri](https://github.com/Azure-Samples/piggymetrics). Örnekler, kaynak koda gereken şekilde başvurur.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure Spring Cloud Instance sağlama](spring-cloud-quickstart-provision-service-instance.md)
