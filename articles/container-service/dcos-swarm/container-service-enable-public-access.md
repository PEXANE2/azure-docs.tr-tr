---
title: (AmortismanA Uğradı) Azure DC/OS kapsayıcı uygulamasına erişimi etkinleştirme
description: Azure Kapsayıcı Hizmeti'nde DC/OS kapsayıcılarına genel erişimi etkinleştirme.
services: container-service
author: sauryadas
manager: madhana
ms.service: container-service
ms.topic: article
ms.date: 08/26/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 3e4ba15fa1925ca40ad7760acbd14331fbdd1343
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61457382"
---
# <a name="deprecated-enable-public-access-to-an-azure-container-service-application"></a>(AmortismanA Uğradı) Azure Kapsayıcı Hizmeti uygulamasına genel erişimi etkinleştirme

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

ACS [public agent havuzundaki](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) herhangi bir DC/OS konteyneri otomatik olarak internete maruz kalır. Varsayılan olarak, **80**, **443**, **8080** bağlantı noktaları açılır ve bu bağlantı noktalarını dinleyen herhangi bir (ortak) kapsayıcıya erişilebilir. Bu makalede, Azure Kapsayıcı Hizmeti'ndeki uygulamalarınız için daha fazla bağlantı noktası nın nasıl açılacağını gösterilmektedir.

## <a name="open-a-port-portal"></a>Bağlantı noktası açma (portal)
Önce istediğimiz limanı açmalıyız.

1. Portalda oturum açın.
2. Azure Kapsayıcı Hizmeti'ni dağıttığınız kaynak grubunu bulun.
3. Aracı yük bakiyesini seçin **(XXXX-agent-lb-XXXX'e**benzer olarak adlandırılır).
   
    ![Azure konteyner hizmeti yük dengeleyicisi](./media/container-service-enable-public-access/agent-load-balancer.png)
4. **Probları** tıklatın ve sonra **ekle.**
   
    ![Azure konteyner hizmeti yük dengeleyici probları](./media/container-service-enable-public-access/add-probe.png)
5. Sonda formunu doldurun ve **Tamam'ı**tıklatın.
   
   | Alan | Açıklama |
   | --- | --- |
   | Adı |Sondanın açıklayıcı bir adı. |
   | Bağlantı noktası |Test etmek için kapsayıcının bağlantı noktası. |
   | Yol |(HTTP modundayken) Sonda için göreceli web sitesi yolu. HTTPS desteklenmez. |
   | Interval |Sonda denemeleri arasındaki süre saniyecinsinden. |
   | Sağlıksız durum eşiği |Kapsayıcı sağlıksız düşünmeden önce ardışık sonda girişimleri sayısı. |
6. Aracı yük dengeleyicisinin özelliklerine geri dön, **Yük dengeleme kurallarını** tıklatın ve sonra **ekle.**
   
    ![Azure kapsayıcı hizmeti yük dengeleyici kuralları](./media/container-service-enable-public-access/add-balancer-rule.png)
7. Yük dengeleyici formunu doldurun ve **Tamam'ı**tıklatın.
   
   | Alan | Açıklama |
   | --- | --- |
   | Adı |Yük dengeleyicisinin açıklayıcı adı. |
   | Bağlantı noktası |Halka açık liman. |
   | Arka uç bağlantı noktası |Trafiği yönlendirmek için kapsayıcının iç-ortak noktası. |
   | Arka uç havuzu |Bu havuzdaki konteynerler bu yük dengeleyicisinin hedefi olacaktır. |
   | Yoklama |Sonda, **Arka uç havuzundaki** bir hedefin sağlıklı olup olmadığını belirlemek için kullanılır. |
   | Oturum kalıcılığı |Oturum süresince istemciden gelen trafiğin nasıl işleneceğini belirler.<br><br>**Yok**: Aynı istemciden art arda gelen istekler herhangi bir kapsayıcı tarafından işlenebilir.<br>**İstemci IP**: Aynı istemci IP'den art arda gelen istekler aynı kapsayıcı tarafından işlenir.<br>**İstemci IP ve protokolü**: Aynı istemci IP ve protokol kombinasyonundan art arda gelen istekler aynı kapsayıcı tarafından işlenir. |
   | Boşta zaman ası |(Yalnızca TCP) Dakikalar içinde, bir TCP/HTTP istemcisini *canlı tutma* iletilerine güvenmeden açık tutma zamanı. |

## <a name="add-a-security-rule-portal"></a>Güvenlik kuralı ekleme (portal)
Ardından, açılan bağlantı noktasımızdan trafiği güvenlik duvarından gönderen bir güvenlik kuralı eklememiz gerekir.

1. Portalda oturum açın.
2. Azure Kapsayıcı Hizmeti'ni dağıttığınız kaynak grubunu bulun.
3. **Ortak** aracı ağı güvenlik grubunu seçin **(XXXX-agent-public-nsg-XXXX'e**benzer adlandırılmış).
   
    ![Azure kapsayıcı servis ağı güvenlik grubu](./media/container-service-enable-public-access/agent-nsg.png)
4. **Gelen güvenlik kurallarını** seçin ve sonra **ekleyin.**
   
    ![Azure kapsayıcı servis ağı güvenlik grubu kuralları](./media/container-service-enable-public-access/add-firewall-rule.png)
5. Ortak bağlantı noktasınıza izin vermek için güvenlik duvarı kuralını doldurun ve **Tamam'ı**tıklatın.
   
   | Alan | Açıklama |
   | --- | --- |
   | Adı |Güvenlik duvarı kuralının açıklayıcı adı. |
   | Öncelik |Kural için öncelik sıralaması. Sayı ne kadar düşükse öncelik de o kadar yüksektir. |
   | Kaynak |Bu kural tarafından izin verilmesine veya reddedilmesine izin verilmesi için gelen IP adresi aralığını kısıtlayın. Bir kısıtlama belirtmemek için **Herhangi'yi** kullanın. |
   | Hizmet |Bu güvenlik kuralının önceden tanımlanmış hizmetler kümesini seçin. Aksi takdirde kendi oluşturmak için **Özel** kullanın. |
   | Protokol |**TCP** veya **UDP'ye**göre trafiği kısıtlayın. Bir kısıtlama belirtmemek için **Herhangi'yi** kullanın. |
   | Bağlantı noktası aralığı |**Hizmet** **Özel**olduğunda, bu kuralın etkilediği bağlantı noktaları aralığını belirtir. **80**gibi tek bir bağlantı noktası veya **1024-1500**gibi bir aralık kullanabilirsiniz. |
   | Eylem |Ölçütleri karşılayan trafiğe izin verin veya reddedin. |

## <a name="next-steps"></a>Sonraki adımlar
[Genel ve özel DC/OS aracıları](container-service-dcos-agents.md)arasındaki fark hakkında bilgi edinin.

[DC/OS kapsayıcılarınızı yönetme](container-service-mesos-marathon-ui.md)hakkında daha fazla bilgi edinin.

