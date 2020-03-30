---
title: Kendi kendine barındırılan Azure API Yönetimi ağ geçidine genel bakış | Microsoft Dokümanlar
description: Kendi kendine barındırılan Azure API Yönetimi ağ geçidinin, kuruluşların karma ve çok bulutlu ortamlarda KI ApI'leri yönetmesine nasıl yardımcı olduğunu öğrenin.
services: api-management
documentationcenter: ''
author: vlvinogr
manager: gwallace
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/31/2019
ms.author: apimpm
ms.openlocfilehash: 415f0e209e607a863d715b1a66a2435603a662f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73513724"
---
# <a name="self-hosted-api-management-gateway-overview"></a>Kendi barındırılan API Yönetimi ağ geçidine genel bakış

Bu makalede, kendi kendine barındırılan ağ geçidi özelliğinin hibrit ve çok bulutlu API yönetimine nasıl olanak sağladığı, üst düzey mimarisini nasıl sunduğu ve temel yeteneklerini nasıl vurguladığı açıklanmaktadır.

> [!NOTE]
> Kendi kendine barındırılan ağ geçidi özelliği önizlemededir. Önizleme sırasında, kendi kendine barındırılan ağ geçidi ek ücret ödemeden yalnızca Geliştirici ve Premium katmanlarında kullanılabilir. Geliştirici katmanı, kendi kendine barındırılan tek bir ağ geçidi dağıtımıyla sınırlıdır.

## <a name="hybrid-and-multi-cloud-api-management"></a>Karma ve çoklu bulut API yönetimi

Kendi kendine barındırılan ağ geçidi özelliği, karma ve çok bulutlu ortamlar için API Yönetimi desteğini genişletir ve kuruluşların Azure'daki tek bir API Yönetimi hizmetinden şirket içinde ve bulutlar arasında barındırılan API'leri verimli ve güvenli bir şekilde yönetmesini sağlar.

Kendi kendine barındırılan ağ geçidi yle, müşteriler API Yönetimi ağ geçidi bileşeninin kapsayıcılaştırılmış bir sürümünü API'lerini barındırdıkları aynı ortamlara dağıtma esnekliğine sahiptir. Tüm kendi kendine barındırılan ağ geçitleri, federated edildikleri API Yönetimi hizmetinden yönetilir ve böylece müşterilere tüm iç ve dış API'lerde görünürlük ve birleşik yönetim deneyimi sağlar. Ağ geçitlerinin API'lere yakın bir yere yerleştirilmesi, müşterilerin API trafik akışlarını optimize etmesine ve güvenlik ve uyumluluk gereksinimlerini gidermesine olanak sağlar.

Her API Yönetimi hizmeti aşağıdaki temel bileşenlerden oluşur:

-   API olarak açıkta olan yönetim düzlemi, hizmeti Azure portalı, PowerShell ve diğer desteklenen mekanizmalar üzerinden yapılandırmak için kullanılır.
-   Ağ geçidi (veya veri düzlemi), API isteklerini proxyleme, ilkeler uygulama ve telemetri toplamakla yükümlüdür
-   Geliştiriciler tarafından API'leri kullanmak için keşfetmek, öğrenmek ve yerleşik olarak kullanılan geliştirici portalı

Varsayılan olarak, tüm bu bileşenler Azure'da dağıtılır ve tüm API trafiğinin (aşağıdaki resimde düz siyah oklar olarak gösterildiği) API'leri uygulayan arka uçların nerede barındırıldığına bakılmaksızın Azure'da akmasına neden olur. Bu modelin operasyonel basitliği artan gecikme, uyumluluk sorunları ve bazı durumlarda ek veri aktarım ücretleri pahasına gelir.

![Kendi kendine barındırılan ağ geçitleri olmadan API trafik akışı](media/self-hosted-gateway-overview/without-gateways.png)

Arka uç API uygulamalarıyla aynı ortamlara barındırılan ağ geçitlerini dağıtmak ve bunları API Yönetimi hizmetine eklemek, API trafiğinin gecikmeyi artıran, veri aktarım maliyetlerini optimize eden ve veri aktarım maliyetlerini optimize eden arka uç API'lerine doğrudan akmasına olanak tanır uygulamalarının nerede barındırıldığına bakılmaksızın, tek bir yönetim noktasına sahip olmanın ve kuruluş içindeki tüm API'lerin keşfinin yararlarını korurken uyumluluk.

![Kendi kendine barındırılan ağ geçitleri ile API trafik akışı](media/self-hosted-gateway-overview/with-gateways.png)

## <a name="packaging-and-features"></a>Ambalaj ve özellikler

Kendi kendine barındırılan ağ geçidi, her API Yönetimi hizmetinin bir parçası olarak Azure'a dağıtılan yönetilen ağ geçidinin kapsayıcılaştırılmış, işlevsel olarak eşdeğer bir sürümüdür. Kendi kendine barındırılan ağ geçidi, Microsoft Konteyner Kayıt Defteri'nden Linux tabanlı Docker kapsayıcısı olarak kullanılabilir. Docker, Kubernetes veya masaüstü, sunucu kümesi veya bulut altyapısında çalışan başka bir kapsayıcı düzenleme çözümüne dağıtılabilir.

> [!IMPORTANT]
> Yönetilen ağ geçidinde bulunan bazı işlevler henüz önizlemede kullanılamaz. En önemlisi: Event Hub ilkesine, Service Fabric entegrasyonuna, http/2 aşağı akışına giriş yapın. Kendi barındırılan ağ geçidinde yerleşik bir önbelleği kullanılabilir hale getirme planı yoktur.

## <a name="connectivity-to-azure"></a>Azure'a Bağlantı

Kendi kendine barındırılan ağ geçidi, 443 bağlantı noktasındaki Azure'a giden TCP/IP bağlantısı gerektirir. Her kendi kendine barındırılan ağ geçidi tek bir API Yönetimi hizmetiile ilişkilendirilmelidir ve yönetim düzlemi üzerinden yapılandırılır. Kendi kendine barındırılan ağ geçidi, Azure'a bağlantı sağlar:

-   Her dakika sinyal iletileri göndererek durumunu bildirme
-   Düzenli olarak (her 10 saniyede bir) kontrol etme ve kullanılabilir olduklarında yapılandırma güncelleştirmelerini uygulama
-   Yapılandırılmışsa, istek günlüklerini ve ölçümlerini Azure Monitor'a gönderme
-   Bunu yapmaya ayarlanmışsa, etkinlikleri Uygulama Öngörüleri'ne gönderme

Azure bağlantısı kaybolduğunda, kendi kendine barındırılan ağ geçidi yapılandırma güncelleştirmelerini alamayacak, durumunu bildiremez veya telemetri yükleyemez.

Kendi kendine barındırılan ağ geçidi "statik arıza" için tasarlanmıştır ve Azure'a geçici bağlantı kaybı yaşayabilir. Yerel yapılandırma yedeklemesi açık olsun veya açık olmadan dağıtılabilir. Eski durumda, kendi kendine barındırılan ağ geçitleri, kapsayıcıya veya bölmeye bağlı kalıcı bir birimde yapılandırmanın yedek bir kopyasını düzenli olarak kaydeder.

Yapılandırma yedeklemesi kapatıldığında ve Azure bağlantısı kesildiğinde:

-   Çalışan kendi kendine barındırılan ağ geçitleri yapılandırmanın bellek içi bir kopyasını kullanarak çalışmaya devam eder
-   Durdurulan kendi kendine barındırılan ağ geçitleri başlatılamaz

Yapılandırma yedeklemesi açıklandığında ve Azure bağlantısı kesildiğinde:

-   Çalışan kendi kendine barındırılan ağ geçitleri yapılandırmanın bellek içi bir kopyasını kullanarak çalışmaya devam eder
-   Durdurulan kendi kendine barındırılan ağ geçitleri yapılandırmanın yedek kopyasını kullanmaya başlar

Bağlantı geri yüklendiğinde, kesintiden etkilenen her kendi kendine barındırılan ağ geçidi ilişkili API Yönetimi hizmetiyle otomatik olarak yeniden bağlanır ve ağ geçidi "çevrimdışı" iken oluşan tüm yapılandırma güncelleştirmelerini karşıdan yükler.

## <a name="next-steps"></a>Sonraki adımlar

-   [Bu konuda ek arka plan için bir teknik inceleme okuyun](https://aka.ms/hybrid-and-multi-cloud-api-management)
-   [Docker'a kendi kendine barındırılan ağ geçidini dağıtın](api-management-howto-deploy-self-hosted-gateway-to-docker.md)
-   [Kubernetes'e kendi kendine barındırılan ağ geçidini dağıtın](api-management-howto-deploy-self-hosted-gateway-to-k8s.md)
