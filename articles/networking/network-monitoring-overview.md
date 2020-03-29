---
title: Azure Monitor günlüklerinde Ağ İzleme Hakkında | Microsoft Dokümanlar
description: Bulut, şirket içi ve karma ortamlarda ağları yönetmek için NPM de dahil olmak üzere ağ izleme çözümlerine genel bakış.
services: monitoring-and-diagnostics
documentationcenter: na
author: agummadi
manager: ''
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: agummadi
ms.openlocfilehash: 2912488286745bf8d2e567d09e445b0a44dc7c39
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67672171"
---
# <a name="network-monitoring-solutions"></a>Ağ izleme çözümleri 

Azure, ağ varlıklarınızı izlemek için bir dizi çözüm sunar. Azure, ağ bağlantısını, ExpressRoute devrelerinin durumunu izlemek ve buluttaki ağ trafiğini analiz etmek için çözümlere ve yardımcı programlara sahiptir.

## <a name="network-performance-monitor-npm"></a>Ağ Performans Monitörü (NPM)

Ağ Performans İzleyicisi (NPM), her biri ağınızın durumunu, uygulamalarınızla ağ bağlantısını izlemeye yönelik ve ağınızın performansı hakkında öngörüler sağlayan bir yetenek paketidir. NPM bulut tabanlıdır ve şu lar arasındaki bağlantıyı izleyen karma bir ağ izleme çözümü sağlar:
 
* Bulut dağıtımları ve şirket içi konumlar
* Birden fazla veri merkezi ve şube
* Kritik görev çok katmanlı uygulamalar/mikro hizmetler
* Kullanıcı konumları ve web tabanlı uygulamalar (HTTP/HTTPs) 

Performans Monitörü, ExpressRoute Monitor ve Servis Bağlantısı Monitörü NPM içindeki yetenekleri izliyor ve aşağıda açıklanmıştır.

## <a name="performance-monitor"></a>Performans İzleyicisi

Performance Monitor NPM'nin bir parçasıdır ve bulut, karma ve şirket içi ortamlar için ağ izlemedir. Uzak şube ve saha ofisleri, depolama konumları, veri merkezleri ve bulutlar arasında ağ bağlantısını izleyebilirsiniz. Kullanıcılarınız şikayet etmeden önce ağ sorunlarını algılayabilirsiniz. Önemli avantajları şunlardır:

* Çeşitli alt ağlarda kaybı ve gecikmeyi izleyin ve uyarıları ayarlayın
* Ağdaki tüm yolları (gereksiz yollar dahil) izleme
* Çoğaltması zor olan geçici ve zaman içinde nokta ağ sorunlarını giderme
* Bozulmuş performansla sorumlu olan ağdaki belirli kesimi belirleme
* SNMP'ye gerek kalmadan ağın durumunu izleyin

![NPM topoloji haritası](./media/network-monitoring-overview/npm-topology-map.png) 

Daha fazla bilgi için aşağıdaki makaleleri görüntüleyin:

* [Azure Monitör günlüklerinde Ağ Performans İzleme Çözümü yapılandırma](../azure-monitor/insights/network-performance-monitor.md) 
* [Servis taleplerini kullanma](https://blogs.technet.microsoft.com/msoms/2016/08/30/monitor-on-premises-cloud-iaas-and-hybrid-networks-using-oms-network-performance-monitor/)
* Ürün Güncellemeleri:
  * [Şubat 2017](https://blogs.technet.microsoft.com/msoms/2017/02/27/oms-network-performance-monitor-is-now-generally-available/)
  * [Ağustos 2017](https://blogs.technet.microsoft.com/msoms/2017/08/14/improvements-to-oms-network-performance-monitor/)

## <a name="expressroute-monitor"></a>ExpressRoute Monitör

ExpressRoute için NPM, Azure Özel ve Microsoft bakan bağlantıları için kapsamlı ExpressRoute izleme hizmeti sunar. ExpressRoute üzerinden şubeleriniz ve Azure arasındaki E2E bağlantısını ve performansını izleyebilirsiniz. Önemli yetenekleri şunlardır:

* Aboneliğinizle ilişkili ER devrelerinin otomatik algılaması
* Ağ topolojisinin şirket içinde bulut uygulamalarınıza algılanması
* Kapasite planlaması, bant genişliği kullanım analizi
* Hem birincil hem de ikincil yollarda izleme ve uyarı
* Office 365, Dynamics 365, ... gibi Azure hizmetlerine bağlantı izleme ExpressRoute üzerinden
* VNets bağlantısının bozulmasını algılama

![Bölgeler arası trafiği gösteren coğrafi harita](./media/network-monitoring-overview/expressroute-topology-map.png) 

Daha fazla bilgi için aşağıdaki makalelere bakın:

* [ExpressRoute için Ağ Performansı İzleyicisi’ni Yapılandırma](../expressroute/how-to-npm.md)
* [Blog yazısı](https://aka.ms/NPMExRmonitorGA)

## <a name="service-connectivity-monitor"></a>Hizmet Bağlantısı İzleyicisi

Service Connectivity izleme ile artık uygulamaların ulaşılabilirliğini test edebilir ve şirket içi, operatör ağları ve bulut/özel veri merkezlerinde performans darboğazlarını algılayabilirsiniz.

* Uygulamalara uçlardan uca ağ bağlantısını izleme
* Uygulama teslimini ağ performansıyla ilişkilendirin, kullanıcı ile uygulama arasındaki yol boyunca bozulmanın kesin konumunu algılayın
* Uygulama erişimini dünya çapında birden fazla kullanıcı noktasından test etme
* İş ve SaaS uygulamalarınız için ağ gecikmesi ve paket kaybını belirleme
* Ağdaki kötü uygulama performansına neden olabilecek etkin noktaları belirleme
* Microsoft Office 365, Dynamics 365, Skype for Business ve diğer Microsoft hizmetleri için yerleşik testleri kullanarak Office 365 uygulamalarına erişebilme özelliğini izleme

Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Hizmet Bitiş Noktalarını izlemek için Ağ Performans Monitörünü yapılandırın](../azure-monitor/insights/network-performance-monitor-service-connectivity.md#configuration)
* [Blog yazısı](https://aka.ms/svcendptmonitor)

## <a name="traffic-analytics"></a>Trafik Analizi
Traffic Analytics, bulut ağlarınızdaki kullanıcı ve uygulama etkinliklerinde görünürlük sağlayan bulut tabanlı bir çözümdür. NSG Akış günlükleri, şu anlama ilişkin öngörüler sağlamak için analiz edilir:

* Azure ve Internet, genel bulut bölgeleri, VNET'ler ve alt ağlar arasında ağlarınızda trafik akışı
* Algılayıcılara veya özel akış kolektörü cihazlarına gerek kalmadan ağınızdaki uygulamalar ve protokoller
* En çok konuşanlar, geveze uygulamalar, buluttaki VM konuşmaları, trafik noktaları
* VNET'ler arasındaki trafiğin kaynakları ve varış noktaları, kritik iş hizmetleri ve uygulamalar arasındaki ilişkiler
* Güvenlik – kötü niyetli trafik, Internet'e açık bağlantı noktaları, Internet erişimine çalışan uygulamalar veya VM'ler...
* Kapasite kullanımı - VPN ağ geçitlerinin ve diğer hizmetlerin kullanım eğilimlerini izleyerek aşırı sağlama veya yetersiz kullanım sorunlarını ortadan kaldırmanıza yardımcı olur

Traffic Analytics, kuruluşunuzun ağ etkinliğini denetlemenize, uygulamaları ve verileri korumanıza, iş yükü performansını optimize ekolte getirmenize ve uyumlu kalmanıza yardımcı olan işlem yapılabilir bilgilerle donatılmasına yardımcı olur.

![Bölgeler arası trafiği gösteren coğrafi harita](../network-watcher/media/traffic-analytics/geo-map-view-showcasing-traffic-distribution-to-countries-and-continents.png) 

İlgili linkler:
* [Blog yazısı](https://aka.ms/trafficanalytics), [Belgeler](https://aka.ms/trafficanalyticsdocs), [SSS](https://docs.microsoft.com/azure/network-watcher/traffic-analytics-faq)

## <a name="dns-analytics"></a>DNS Analizi
DNS Yöneticileri için tasarlanan bu çözüm, güvenlik, işlemler ve performansla ilgili öngörüler sağlamak için DNS günlüklerini toplar, çözümler ve ilişkilendirir.  Bazı yetenekler şunlardır:

* Kötü amaçlı etki alanlarını çözmeye çalışan istemcilerin tanımlanması
* Eski kaynak kayıtlarının tanımlanması
* Sık sorgulanmış alan adları ve konuşkan DNS istemcilerine görünürlük
* DNS sunucularında istek yüküne görünürlük
* Dinamik DNS kayıt hatalarının izlenmesi

![DNS Analitik Panosu](./media/network-monitoring-overview/dns-analytics-overview.png) 

İlgili linkler:
* [Blog yazısı](https://blogs.technet.microsoft.com/msoms/2017/04/19/introducing-oms-dns-analytics/), [Dokümantasyon](https://docs.microsoft.com/azure/log-analytics/log-analytics-dns)

## <a name="miscellaneous"></a>Çeşitli

* [Yeni Fiyatlandırma](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor-pricing-faq)
