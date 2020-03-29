---
title: (AmortismanA Uğradı) Azure DC/İşletim sistemi kümesini izleme - ELK yığını
description: ELK (Elasticsearch, Logstash ve Kibana) ile Azure Kapsayıcı Hizmeti kümesinde bir DC/OS kümesini izleyin.
author: sauryadas
ms.service: container-service
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 3d34ebe22344be8acc6ec3cc974071639293e2b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76277769"
---
# <a name="deprecated-monitor-an-azure-container-service-cluster-with-elk"></a>(AmortismanA Uğradı) ELK ile Bir Azure Kapsayıcı Hizmeti kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bu makalede, ElK (Elasticsearch, Logstash, Kibana) yığınının Azure Kapsayıcı Hizmeti'ndeki bir DC/OS kümesinde nasıl dağıtılancayakadardırıştırılabiliyoruz. 

## <a name="prerequisites"></a>Ön koşullar
Azure Kapsayıcı Hizmeti tarafından yapılandırılan bir DC/OS kümesini [dağıtın](container-service-deployment.md) ve [bağlayın.](../container-service-connect.md) DC/OS panosunu ve Maraton hizmetlerini [buradan](container-service-mesos-marathon-ui.md)keşfedin. Ayrıca [Maraton Yük Dengeleyici](container-service-load-balancing.md)yükleyin.


## <a name="elk-elasticsearch-logstash-kibana"></a>ELK (Elasticsearch, Logstash, Kibana)
ELK yığını, cluster'ınızdaki günlükleri izlemek ve analiz etmek için kullanılabilecek uçtan uca yığın sağlayan Elasticsearch, Logstash ve Kibana'nın bir leşimidir.

## <a name="configure-the-elk-stack-on-a-dcos-cluster"></a>ELK yığınını DC/OS kümesinde yapılandırın
DC/OS UI'nize [http://localhost:80/](http://localhost:80/) Bir Kez DC/OS UI üzerinden **erişin Universe'e**gidin. Dc/OS Universe'den elasticsearch, Logstash ve Kibana'yı arama ve yükleme yi ve bu özel sırada. **Gelişmiş Yükleme** bağlantısına giderseniz yapılandırma hakkında daha fazla bilgi edinebilirsiniz.

![ELK1](./media/container-service-monitoring-elk/elk1.PNG) ![ELK2](./media/container-service-monitoring-elk/elk2.PNG) ![ELK3](./media/container-service-monitoring-elk/elk3.PNG) 

ELK konteynerleri çalışmaya başladıktan ve çalışmaya başladıktan sonra, Kibana'ya Marathon-LB üzerinden erişilmesini sağlamanız gerekir. **Hizmetler** > **kibana**gidin ve aşağıda gösterildiği gibi **Edit'i** tıklatın.

![ELK4](./media/container-service-monitoring-elk/elk4.PNG)


**JSON moduna** geçin ve etiketler bölümüne gidin.
Aşağıda gösterildiği gibi `"HAPROXY_GROUP": "external"` burada bir giriş eklemeniz gerekir.
**Değişiklikleri Dağıt'ı**tıklattığınızda, kapsayıcınız yeniden başlar.

![ELK5](./media/container-service-monitoring-elk/elk5.PNG)


Kibana'nın HAPROXY panosunda hizmet olarak kayıtlı olduğunu doğrulamak istiyorsanız, HAPROXY port 9090'da çalıştığından aracı kümesinde 9090 bağlantı noktasını açmanız gerekir.
Varsayılan olarak, DC/OS aracı kümesinde 80, 8080 ve 443 bağlantı noktalarını açıyoruz.
Bir liman açmak ve kamu değerlendirmek sağlamak için talimatlar [burada](container-service-enable-public-access.md)sağlanmaktadır.

HAPROXY panosuna erişmek için Marathon-LB yönetici `http://$PUBLIC_NODE_IP_ADDRESS:9090/haproxy?stats`arabirimini şu anda açın: .
URL'ye gittikten sonra, aşağıda gösterildiği gibi HAPROXY panosunu görmeniz ve Kibana için bir hizmet girişi görmeniz gerekir.

![ELK6](./media/container-service-monitoring-elk/elk6.PNG)


Bağlantı noktası 5601'de dağıtılan Kibana panosuna erişmek için 5601 portu açmanız gerekir. Talimatları [buradan](container-service-enable-public-access.md)takip edin. Sonra Kibana panosunu `http://localhost:5601`açın: .

## <a name="next-steps"></a>Sonraki adımlar

* Sistem ve uygulama günlüğü yönlendirme ve kurulumu için, [ELK ile DC/OS'deki Log Management 'a](https://docs.mesosphere.com/1.8/administration/logging/elk/)bakın.

* Günlükleri filtrelemek için [ELK ile Filtreleme Günlükleri'ne](https://docs.mesosphere.com/1.8/administration/logging/filter-elk/)bakın. 

 

