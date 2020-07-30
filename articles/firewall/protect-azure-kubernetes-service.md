---
title: Azure Kubernetes hizmeti (AKS) dağıtımlarını korumak için Azure Güvenlik duvarını kullanma
description: Azure Kubernetes hizmeti (AKS) dağıtımlarını korumak için Azure Güvenlik Duvarı 'nı nasıl kullanacağınızı öğrenin
author: vhorne
ms.service: firewall
services: firewall
ms.topic: how-to
ms.date: 07/29/2020
ms.author: victorh
ms.openlocfilehash: 602671f1052de2d9446f32946271cea2f9995044
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87412958"
---
# <a name="use-azure-firewall-to-protect-azure-kubernetes-service-aks-deployments"></a>Azure Kubernetes hizmeti (AKS) dağıtımlarını korumak için Azure Güvenlik duvarını kullanma

Azure Kubernetes hizmeti (AKS), Azure 'da yönetilen bir Kubernetes kümesi sunar. Bu sorumluluğun çoğunu Azure 'a devrederek, Kubernetes 'in yönetilmesine yönelik karmaşıklık ve operasyonel ek yükü azaltır. AKS, sistem durumu izleme ve bakım gibi kritik görevleri işler ve kolaylaştırılmaz idare sayesinde kurumsal düzeyde ve güvenli bir küme sunar.

Kubernetes, sanal makinelerin kümelerini düzenler ve bu sanal makinelerde, her kapsayıcının kaynak gereksinimlerine bağlı olarak bu sanal makinelerde çalıştırılacak kapsayıcıları zamanlar. Kapsayıcılar, Kubernetes için temel işlemsel birim olan ve bu yığınların ölçeği istediğiniz duruma kadar olan Pod 'ler halinde gruplandırılır.

Yönetim ve operasyonel amaçlar için, bir AKS kümesindeki düğümlerin belirli bağlantı noktalarına ve tam etki alanı adlarına (FQDN) erişmesi gerekir. Bu eylemler, API sunucusuyla iletişim kurmak veya temel Kubernetes küme bileşenlerini ve düğüm güvenliği güncelleştirmelerini indirmek ve yüklemek olabilir. Azure Güvenlik Duvarı, ortamınızı kilitlemenize ve giden trafiği filtrelemenize yardımcı olabilir.

Azure Güvenlik Duvarı 'nı kullanarak Azure Kubernetes kümeniz için ek koruma sağlamak üzere bu makaledeki yönergeleri izleyin.

## <a name="prerequisites"></a>Ön koşullar

- Çalışan uygulamayla dağıtılan bir Azure Kubernetes kümesi.

   Daha fazla bilgi için bkz. [öğretici: Azure Kubernetes Service (aks) kümesi dağıtma](../aks/tutorial-kubernetes-deploy-cluster.md) ve [öğretici: Azure Kubernetes SERVICE (aks) içinde uygulamaları çalıştırma](../aks/tutorial-kubernetes-deploy-application.md).


## <a name="securing-aks"></a>AKS güvenliğini sağlama

Azure Güvenlik Duvarı, yapılandırmayı basitleştirmek için bir AKS FQDN etiketi sağlar. Giden AKS platform trafiğine izin vermek için aşağıdaki adımları kullanın:

- Giden trafiği kısıtlamak ve tüm giden trafiği yönlendirmek üzere Kullanıcı tanımlı yol (UDR) oluşturmak için Azure Güvenlik Duvarı 'nı kullandığınızda, gelen trafiğe doğru şekilde izin vermek için güvenlik duvarında uygun bir DNAT kuralı oluşturduğunuzdan emin olun. 

   Azure Güvenlik Duvarı 'nı bir UDR ile kullanmak, asimetrik yönlendirme nedeniyle gelen kurulumu keser. Bu sorun, AKS alt ağının güvenlik duvarının özel IP adresine giden bir varsayılan yolu varsa, ancak bir genel yük dengeleyici kullanıyorsanız oluşur. Örneğin, *LoadBalancer*türünde gelen veya Kubernetes hizmeti.

   Bu durumda, gelen yük dengeleyici trafiği genel IP adresi aracılığıyla alınır, ancak döndürülen yol güvenlik duvarının özel IP adresinden geçer. Güvenlik duvarı durum bilgisi olduğundan, güvenlik duvarı kurulu bir oturumun farkında olmadığından döndürülen paketi bırakır. Azure Güvenlik duvarını giriş veya hizmet yük dengeleyicinizle tümleştirmeyi öğrenmek için bkz. Azure [güvenlik duvarını azure standart Load Balancer tümleştirme](integrate-lb.md).
- Bir uygulama kuralı koleksiyonu oluşturun ve *AzureKubernetesService* FQDN etiketini etkinleştirmek için bir kural ekleyin. Kaynak IP adresi aralığı konak havuzunun sanal ağı, protokol https ve hedef ise AzureKubernetesService.
- AKS kümesi için aşağıdaki giden bağlantı noktaları/ağ kuralları gereklidir:

   - TCP bağlantı noktası 443
   - API sunucusuyla iletişim kurmayı gerektiren bir uygulamanız varsa TCP [*ıpaddrofyourapiserver*]: 443 gereklidir. Bu değişiklik küme oluşturulduktan sonra ayarlanabilir.
   - TCP bağlantı noktası 9000 ve tünel ön pod için, API sunucusundaki tünel sonuyla iletişim kurmak üzere UDP bağlantı noktası 1194.

      Daha belirgin olması için bkz. **. HCP. <location> . *aşağıdaki tabloda azmk8s.io ve adresler.
   - Ağ zaman Protokolü (NTP) zaman eşitleme (Linux düğümleri) için UDP bağlantı noktası 123.
   - API sunucusuna doğrudan erişiyorsanız, DNS için UDP bağlantı noktası 53 de gereklidir.

   Daha fazla bilgi için bkz. [Azure Kubernetes Service (AKS) içindeki küme düğümleri Için denetim çıkış trafiği](../aks/limit-egress-traffic.md).
- AzureMonitor ve Storage hizmeti etiketlerini yapılandırın. Azure Izleyici, Log Analytics verilerini alır.

   Ayrıca çalışma alanı URL 'nizin tek tek de izin verebilirsiniz: `<worksapceguid>.ods.opinsights.azure.com` ve `<worksapceguid>.oms.opinsights.azure.com` . Bunu aşağıdaki yollarla ele alabilirsiniz:

    - Konak havuzunuzdaki alt ağınızdan ve için https erişimine izin verin `*. ods.opinsights.azure.com` `*.oms. opinsights.azure.com` . Bu joker karakter FQDN 'leri gereken erişimi etkinleştirir, ancak daha az kısıtlayıcıdır.
    - Gerekli FQDN 'leri tam olarak listelemek için aşağıdaki Log Analytics sorgusunu kullanın ve ardından Güvenlik Duvarı uygulama kurallarınızı açık olarak bunlara izin verin:
   ```
   AzureDiagnostics 
   | where Category == "AzureFirewallApplicationRule" 
   | search "Allow" 
   | search "*. ods.opinsights.azure.com" or "*.oms. opinsights.azure.com"
   | parse msg_s with Protocol " request from " SourceIP ":" SourcePort:int " to " FQDN ":" * 
   | project TimeGenerated,Protocol,FQDN 
   ```


## <a name="next-steps"></a>Sonraki adımlar

- Azure Kubernetes hizmeti hakkında daha fazla bilgi için bkz. [Azure Kubernetes hizmeti (AKS) Için Kubernetes temel kavramları](../aks/concepts-clusters-workloads.md).
