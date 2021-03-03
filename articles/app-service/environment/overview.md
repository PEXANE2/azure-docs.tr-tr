---
title: App Service Ortamı genel bakış
description: App Service Ortamı genel bakış
author: ccompy
ms.assetid: 3d37f007-d6f2-4e47-8e26-b844e47ee919
ms.topic: article
ms.date: 03/02/2021
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 23b23340550ded3642d19500270f06cfb6faf8cb
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101735106"
---
# <a name="app-service-environment-overview"></a>App Service Ortamı genel bakış 

> [!NOTE]
> Bu makale, App Service Ortamı v3 (Önizleme) ile ilgilidir
> 

Azure App Service Ortamı, App Service uygulamalarını yüksek ölçekte güvenli olarak çalıştırmak için tamamen ayrı ve özel bir ortam sağlayan, bir Azure App Service özelliğidir. Bu özellik aşağıdaki öğelerinizi barındırabilir:

- Windows web uygulamaları
- Linux web uygulamaları
- Docker kapsayıcıları
- İşlevler

App Service ortamları (ASE), şunları gerektiren uygulama iş yükleri için uygundur:

- Yüksek ölçekli.
- Yalıtım ve güvenli ağ erişimi.
- Yüksek bellek kullanımı.
- Saniye başına yüksek istek (RPS). Tek bir Azure bölgesinde veya birden çok Azure bölgesinde birden çok ASE oluşturabilirsiniz. Bu esneklik, yüksek bir RPS gereksinimi ile durum bilgisiz uygulamaların yatay olarak ölçeklendirilmesine yönelik ideal hale getirir.

ASO 'nın ana bilgisayar uygulamaları yalnızca bir müşteriden biridir ve sanal ağlarından birinde bunu yapın. Müşteriler gelen ve giden uygulama ağ trafiği üzerinde ayrıntılı denetime sahiptir. Uygulamalar VPN üzerinden şirket içi kurumsal kaynaklara yüksek hızda güvenli bağlantılar kurabilir.

ASEv3, kendi fiyatlandırma katmanı, yalıtılmış v2 ile gelir.
App Service ortamları v3 bir alt ağda uygulamalarınızın güvenliğini sağlamak için bir kapsayıcı sağlar ve kendi özel dağıtımınızı Azure App Service sağlar.
Yatay yönde ölçeklendirme için birden çok ASE kullanılabilir. ASE’ler üzerinde çalışan uygulamalara erişim, web uygulaması güvenlik duvarları (WAF) gibi yukarı akış cihazları tarafından sağlanabilir. Daha fazla bilgi için bkz. Web uygulaması güvenlik duvarı (WAF).

## <a name="usage-scenarios"></a>Kullanım senaryoları

App Service Ortamı, aşağıdakiler dahil olmak üzere birçok kullanım durumuna sahiptir:

- İç iş kolu uygulamaları
- 30 ' dan fazla ASP örneğine ihtiyacı olan uygulamalar
- İç uyumluluk veya güvenlik gereksinimlerini karşılamak için tek kiracılı sistem
- Ağ yalıtılmış uygulama barındırma
- Çok katmanlı uygulamalar

Çok kiracılı App Service uygulamaların ağdan yalıtılmış kaynaklara ulaşmasını veya ağdan yalıtılmış hale gelmesini sağlayan çeşitli ağ özellikleri vardır. Bu özellikler uygulama düzeyinde etkindir.  Bir ASE ile, uygulamalarda VNet 'te olması için ek yapılandırma yoktur. Uygulamalar, zaten VNet içinde olan bir ağ yalıtılmış ortamına dağıtılır. Ağ yalıtılmış uygulamalarının barındırıldığı en üstünde tek kiracılı bir sistem de vardır. Ao 'yu kullanan başka müşteri yok. Gerçekten de tamamen bir yalıtım öyküsünün olması gerekiyorsa, Ao 'nizi adanmış donanıma de dağıtabilirsiniz. Ağ yalıtılmış uygulama barındırma, tek kiralama ve özellik arasında 

## <a name="dedicated-environment"></a>Ayrılmış ortam
Ao, özel olarak tek bir aboneliğe ayrılmıştır ve birden çok App Service planı üzerinde 200 toplam App Service plan örneğini barındırabilirler. "Örnek" sözcüğü App Service planı yatay ölçeklendirmeyi ifade eder. Her örnek, bir çalışan rolünün eşdeğeridir. ATıCı, 200 toplam örneğe sahip olsa da, tek bir yalıtılmış v2 App Service planı 100 örnekleri tutabilir. ATıCı, her biri, 200 tek örnekli App Service planlarında veya arasındaki her şey için 100 örnek içeren iki App Service planını tutabilir.

ASE, ön uçlar ve çalışanlardan oluşur. Ön uçlar HTTP/HTTPS sonlandırmadan ve bir ASE içindeki uygulama isteklerinin otomatik yük dengelemesinden sorumludur. ASE içindeki App Service planlarının ölçeği artırıldıkça ön uçlar otomatik olarak eklenir.

Çalışanlar, müşteri uygulamalarını barındıran rollerdir. Çalışanlar üç sabit boyutta mevcuttur:

- İki vCPU/8 GB RAM
- Dört vCPU/16 GB RAM
- Sekiz vCPU/32 GB RAM

Müşterilerin ön uçları ve çalışanları yönetmesi gerekmez. Tüm altyapı otomatik olarak kullanılır. Bir ASE’de App Service planları oluşturulduğunda veya ölçeklendirildiğinde, gerekli altyapı uygun şekilde eklenir veya kaldırılır.

Yalıtılmış v2 App Service planı örnekleri için ücretlendirilir. ASE 'de hiç App Service planınız yoksa, iki çekirdekli çalışanların tek bir örneğine sahip bir App Service planınız olmasına rağmen ücretlendirilirsiniz.

## <a name="virtual-network-support"></a>Sanal ağ desteği
Ao özelliği, Azure App Service doğrudan bir müşterinin Azure Resource Manager sanal ağına bir dağıtımdır. Bir ASE her zaman bir sanal ağın alt ağında bulunur. Uygulamalarınıza ilişkin gelen ve giden ağ iletişimini denetlemek için sanal ağların güvenlik özelliklerini kullanabilirsiniz.

Ağ Güvenlik Grupları, bir ASE’nin bulunduğu alt ağa gelen ağ iletişimini kısıtlar. WAF’ler ve ağ SaaS sağlayıcıları gibi yukarı akış cihazlarının ve hizmetlerinin arkasında uygulamaları çalıştırmak için NSG’leri kullanabilirsiniz.

Uygulamalar, iç veritabanları ve web hizmetleri gibi şirket kaynaklarına da sıklıkla erişmelidir. ASE’yi şirket içi ağınızla VPN bağlantısı olan bir sanal ağa dağıtırsanız, ASE’deki uygulamalar şirket içi kaynaklara erişebilir. Bu özellik, VPN’nin siteden siteye veya Azure ExpressRoute VPN olmasından bağımsız olarak geçerli olabilir.

## <a name="preview"></a>Önizleme
App Service Ortamı v3 genel önizlemede.  Önizleme ilerleme durumu sırasında bazı özellikler ekleniyor. ASEv3 'in geçerli sınırlamaları şunlardır:

- 50 örneklerinin ötesinde App Service planını ölçeklendirmeme
- Özel bir kayıt defterinden kapsayıcı alınamaz
- Şu anda desteklenmeyen App Service özellikleri müşteri VNet üzerinden gidemiyor
- İnternet erişimi olan bir uç noktaya sahip dış dağıtım modeli yok
- Komut satırı desteği yok (AZ CLı ve PowerShell)
- ASEv2 'den ASEv3 'e yükseltme özelliği yok
- FTP desteği yok
- Müşteri VNet 'i aracılığıyla bazı App Service özellikleri için destek yoktur. Yedekleme/geri yükleme, uygulama ayarlarındaki başvuruları Key Vault, özel bir kapsayıcı kayıt defteri kullanarak ve depolama için tanılama günlük kaydı, hizmet uç noktaları veya özel uç noktalarla çalışmaz
    
### <a name="asev3-preview-architecture"></a>ASEv3 önizleme mimarisi
ASEv3 Preview sürümünde, ATıCı gelen trafiği desteklemek için özel uç noktaları kullanır. Özel uç nokta, yük dengeleyiciler ile GA 'ye göre değişir. Önizleme aşamasında, Ao, internet 'e erişilebilen bir uç nokta için yerleşik desteğe sahip olmayacaktır. Böyle bir amaç için Application Gateway ekleyebilirsiniz. Ao 'nun iki alt ağ içinde kaynakları olması gerekir.  Gelen trafik, özel bir uç nokta üzerinden akar. Özel uç nokta, Özel uç noktalar tarafından kullanılabilen kullanılabilir bir adrese sahip olduğu sürece herhangi bir alt ağa yerleştirilebilir.  Giden alt ağ boş olmalı ve Microsoft. Web/hostingEnvironments için Temsilcili olmalıdır. ASA tarafından kullanıldığında, giden alt ağ başka bir şey için kullanılamaz.

ASEv3 ile as alt ağında gelen veya giden ağ gereksinimleri yoktur. Trafiği ağ güvenlik grupları ve yönlendirme tabloları ile denetleyebilir ve yalnızca uygulama trafiğinizi etkiler. Bu eylem geri alınamayacağından ATıCı ile ilişkili özel uç noktayı silmeyin. Atıcı için kullanılan özel uç nokta, Ao 'daki tüm uygulamalar için kullanılır. 
