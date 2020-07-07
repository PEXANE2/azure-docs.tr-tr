---
title: Kullanım DıŞı Azure DC/OS kümesini izleme-ELK yığını
description: ELK (Elayoara, Logstash ve kibana) ile Azure Container Service kümesinde DC/OS kümesini izleyin.
author: sauryadas
ms.service: container-service
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 586b8d25a9f391487640e9b1f8adb3be0e4be6db
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82166181"
---
# <a name="deprecated-monitor-an-azure-container-service-cluster-with-elk"></a>Kullanım DıŞı ELK ile Azure Container Service kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bu makalede, ELK (Elam araması, Logstash, kibana) yığınının Azure Container Service bir DC/OS kümesinde nasıl dağıtılacağı gösterilmektedir. 

## <a name="prerequisites"></a>Önkoşullar
Azure Container Service tarafından yapılandırılan bir DC/OS kümesi [dağıtın](container-service-deployment.md) ve [bağlayın](../container-service-connect.md) . DC/OS panosu ve Marathon hizmetlerini [burada bulabilirsiniz](container-service-mesos-marathon-ui.md). Ayrıca, [Marathon Load Balancer](container-service-load-balancing.md)de yükler.


## <a name="elk-elasticsearch-logstash-kibana"></a>ELK (Elaun Search, Logstash, kibana)
ELK yığını, kümenizdeki günlükleri izlemek ve analiz etmek için kullanılan uçtan uca bir yığın sağlayan Elaol Search, Logstash ve kibana 'ın bir birleşimidir.

## <a name="configure-the-elk-stack-on-a-dcos-cluster"></a>Bir DC/OS kümesinde ELK yığınını yapılandırma
DC/OS `http://localhost:80/` Kullanıcı arabiriminde **Universe**adresine gıdın DC/OS Kullanıcı arabirimine bir kez erişin. DC/OS Universe ve bu belirli sırada Elayosearch, Logstash ve kibana 'ı arayın ve yükleme. **Gelişmiş yükleme** bağlantısına giderseniz yapılandırma hakkında daha fazla bilgi edinebilirsiniz.

![ELK1](./media/container-service-monitoring-elk/elk1.PNG) ![ELK2](./media/container-service-monitoring-elk/elk2.PNG) ![ELK3](./media/container-service-monitoring-elk/elk3.PNG) 

ELK kapsayıcıları ve çalışır duruma getirildikten sonra, kibana Marathon-LB aracılığıyla erişilebilmesi için etkinleştirmeniz gerekir. **Hizmetler**  >  **kibana**' e gidin ve aşağıda gösterildiği gibi **Düzenle** ' ye tıklayın.

![ELK4](./media/container-service-monitoring-elk/elk4.PNG)


**JSON moduna** geçin ve Etiketler bölümüne kaydırın.
`"HAPROXY_GROUP": "external"`Aşağıda gösterildiği gibi buraya bir girdi eklemeniz gerekir.
**Değişiklikleri dağıt**' a tıkladığınızda, Kapsayıcınız yeniden başlatılır.

![ELK5](./media/container-service-monitoring-elk/elk5.PNG)


Kibana 'ın HAPROXY panosunda bir hizmet olarak kaydedildiğini doğrulamak istiyorsanız, bağlantı noktası 9090 üzerinde HAPROXY çalışırken aracı kümesinde 9090 numaralı bağlantı noktasını açmanız gerekir.
Varsayılan olarak, DC/OS Aracısı kümesinde 80, 8080 ve 443 bağlantı noktalarını açarız.
Bir bağlantı noktası açma ve Genel değerlendirme sağlama yönergeleri [burada](container-service-enable-public-access.md)verilmiştir.

HAPROXY panosuna erişmek için, şurada Marathon-LB yönetim arabirimini açın: `http://$PUBLIC_NODE_IP_ADDRESS:9090/haproxy?stats` .
URL 'ye gittiğinizde, aşağıda gösterildiği gibi HAPROXY panosunu görmeniz gerekir ve kibana için bir hizmet girdisi görmeniz gerekir.

![ELK6](./media/container-service-monitoring-elk/elk6.PNG)


5601 numaralı bağlantı noktasına dağıtılan kibana panosuna erişmek için, bağlantı noktası 5601 ' i açmanız gerekir. [Buradaki](container-service-enable-public-access.md)yönergeleri izleyin. Daha sonra kibana panosunu şurada açın: `http://localhost:5601` .

## <a name="next-steps"></a>Sonraki adımlar

* Sistem ve uygulama günlüğü iletme ve kurulum için bkz. [ELK Ile DC/OS 'de günlük yönetimi](https://docs.mesosphere.com/1.8/administration/logging/elk/).

* Günlükleri filtrelemek için bkz. [ELK Ile günlüklere filtre uygulama](https://docs.mesosphere.com/1.8/administration/logging/filter-elk/). 

 

