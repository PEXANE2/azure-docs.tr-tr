---
title: Kullanım DıŞı Azure DC/OS kapsayıcı uygulamasına erişimi etkinleştir
description: Azure Container Service 'de DC/OS kapsayıcılarına genel erişimi etkinleştirme.
services: container-service
author: sauryadas
manager: madhana
ms.service: container-service
ms.topic: article
ms.date: 08/26/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 3e4ba15fa1925ca40ad7760acbd14331fbdd1343
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "61457382"
---
# <a name="deprecated-enable-public-access-to-an-azure-container-service-application"></a>Kullanım DıŞı Azure Container Service uygulamasına genel erişimi etkinleştir

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

ACS [genel aracı havuzundaki](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) HERHANGI bir DC/OS kapsayıcısı, internet 'e otomatik olarak sunulur. Varsayılan olarak, **80**, **443**, **8080** bağlantı noktaları açılır ve bu bağlantı noktalarında dinleyen herhangi bir (ortak) kapsayıcıya erişilebilir. Bu makalede, Azure Container Service ' de uygulamalarınız için daha fazla bağlantı noktası açma gösterilmektedir.

## <a name="open-a-port-portal"></a>Bağlantı noktası açma (portal)
İlk olarak, istediğimiz bağlantı noktasını açmanız gerekiyor.

1. Portalda oturum açın.
2. Azure Container Service dağıttığınız kaynak grubunu bulun.
3. Aracı Yük dengeleyiciyi ( **xxxx-Agent-lb-xxxx**öğesine benzer şekilde adlandırılır) seçin.
   
    ![Azure Container Service yük dengeleyici](./media/container-service-enable-public-access/agent-load-balancer.png)
4. **Yoklamalar** ' a tıklayın ve ardından **ekleyin**.
   
    ![Azure Container Service yük dengeleyici araştırmaları](./media/container-service-enable-public-access/add-probe.png)
5. Araştırma formunu doldurun ve **Tamam**' a tıklayın.
   
   | Alan | Description |
   | --- | --- |
   | Name |Araştırmanın açıklayıcı bir adı. |
   | Bağlantı noktası |Sınanacak kapsayıcının bağlantı noktası. |
   | Yol |(HTTP modundayken) Araştırmanın göreli Web sitesi yolu. HTTPS desteklenmiyor. |
   | Interval |Saniye cinsinden yoklama denemeleri arasındaki süre miktarı. |
   | Sağlıksız durum eşiği |Kapsayıcının sağlıksız olduğunu düşünmeden önce ardışık araştırma denemelerinin sayısı. |
6. Aracı Yük dengeleyicinin özelliklerine geri dönün, **Yük Dengeleme kuralları** ' na ve ardından **Ekle**' ye tıklayın.
   
    ![Azure Container Service yük dengeleyici kuralları](./media/container-service-enable-public-access/add-balancer-rule.png)
7. Yük dengeleyici formunu doldurun ve **Tamam**' a tıklayın.
   
   | Alan | Description |
   | --- | --- |
   | Name |Yük dengeleyicinin açıklayıcı adı. |
   | Bağlantı noktası |Ortak gelen bağlantı noktası. |
   | Arka uç bağlantı noktası |Trafiğin yönlendirileceği kapsayıcının iç ortak bağlantı noktası. |
   | Arka uç havuzu |Bu havuzdaki kapsayıcılar bu yük dengeleyicinin hedefi olacak. |
   | Yoklama |**Arka uç havuzundaki** bir hedefin sağlıklı olup olmadığını belirlemede kullanılan araştırma. |
   | Oturum kalıcılığı |Bir istemciden gelen trafiğin oturum süresince nasıl işleneceğini belirler.<br><br>**Hiçbiri**: aynı istemciden gelen istekleri herhangi bir kapsayıcı tarafından işlenebilir.<br>**İstemci IP 'si**: aynı istemci IP 'sinden gelen istekleri aynı kapsayıcı tarafından işlenir.<br>**İstemci IP 'si ve Protokolü**: aynı istemci IP ve protokol birleşiminin ardışık istekleri aynı kapsayıcı tarafından işlenir. |
   | Boşta kalma zaman aşımı |(Yalnızca TCP) Dakikalar içinde, *etkin tut* iletilerine bağlı kalmadan TCP/HTTP istemcisinin açık tutulması için geçen süre. |

## <a name="add-a-security-rule-portal"></a>Güvenlik kuralı ekleme (portal)
Daha sonra, trafiği, açılan bağlantı noktasından güvenlik duvarı aracılığıyla yönlendiren bir güvenlik kuralı eklememiz gerekiyor.

1. Portalda oturum açın.
2. Azure Container Service dağıttığınız kaynak grubunu bulun.
3. **Genel** aracı ağ güvenlik grubunu seçin ( **xxxx-Agent-public-NSG-xxxx**öğesine benzer şekilde adlandırılır).
   
    ![Azure Container Service ağ güvenlik grubu](./media/container-service-enable-public-access/agent-nsg.png)
4. **Gelen güvenlik kuralları** ' nı ve ardından **Ekle**' yi seçin.
   
    ![Azure Container Service ağ güvenlik grubu kuralları](./media/container-service-enable-public-access/add-firewall-rule.png)
5. Genel bağlantı noktasına izin vermek için güvenlik duvarı kuralını doldurun ve **Tamam**' a tıklayın.
   
   | Alan | Description |
   | --- | --- |
   | Name |Güvenlik duvarı kuralının açıklayıcı bir adı. |
   | Öncelik |Kural için öncelik sıralaması. Sayı önceliği daha yüksektir. |
   | Kaynak |Gelen IP adresi aralığını bu kural tarafından izin verilen veya reddedilen şekilde kısıtla. Kısıtlama belirtmemelidir, **herhangi birini** kullanın. |
   | Hizmet |Bu güvenlik kuralının için olduğu, önceden tanımlanmış bir hizmetler kümesi seçin. Aksi takdirde kendi özel ' i oluşturmak için **özel** kullanın. |
   | Protokol |Trafiği **TCP** veya **UDP**'ye göre kısıtlayın. Kısıtlama belirtmemelidir, **herhangi birini** kullanın. |
   | Bağlantı noktası aralığı |**Hizmet** **özel**olduğunda, bu kuralın etkilediği bağlantı noktası aralığını belirtir. **80**gibi tek bir bağlantı noktası veya **1024-1500**gibi bir Aralık kullanabilirsiniz. |
   | Eylem |Ölçütlere uyan trafiğe izin verin veya reddedin. |

## <a name="next-steps"></a>Sonraki adımlar
[Ortak ve özel DC/OS aracıları](container-service-dcos-agents.md)arasındaki fark hakkında bilgi edinin.

[DC/OS Kapsayıcılarınızı yönetme](container-service-mesos-marathon-ui.md)hakkında daha fazla bilgi edinin.

