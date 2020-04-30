---
title: Kubernetes 'e şirket içinde barındırılan ağ geçidi dağıtma | Microsoft Docs
description: Azure API Management şirket içinde barındırılan ağ geçidi bileşenini Kubernetes 'e dağıtmayı öğrenin
services: api-management
author: vladvino
manager: gwallace
ms.service: api-management
ms.workload: mobile
ms.topic: article
ms.author: apimpm
ms.date: 04/23/2020
ms.openlocfilehash: 7802614540bd5e553fbf1d7a0884ffa2f7433c37
ms.sourcegitcommit: 67bddb15f90fb7e845ca739d16ad568cbc368c06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82205111"
---
# <a name="deploy-self-hosted-gateway-to-kubernetes"></a>Kubernetes 'e şirket içinde barındırılan ağ geçidi dağıtma

Bu makalede, Azure API Management şirket içinde barındırılan ağ geçidi bileşenini bir Kubernetes kümesine dağıtma adımları açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

- Şu hızlı başlangıcı doldurun: [Azure API Management örneği oluşturma](get-started-create-service-instance.md)
- Bir Kubernetes kümesi oluşturun.
> [!TIP]
> [Tek düğümlü kümeler](https://kubernetes.io/docs/setup/#learning-environment) geliştirme ve değerlendirme amaçlarıyla iyi çalışır. Şirket içinde veya üretim iş yükleri için bulutta [Kubernetes sertifikalı](https://kubernetes.io/partners/#conformance) çok düğümlü kümeler kullanın.
- [API Management Örneğinizde kendi kendine barındırılan bir ağ geçidi kaynağı sağlayın](api-management-howto-provision-self-hosted-gateway.md).

## <a name="deploy-to-kubernetes"></a>Kubernetes’e dağıtma

1. **Dağıtım ve altyapı**altında **ağ geçitleri** ' ni seçin.
2. Dağıtmayı düşündüğünüz şirket içinde barındırılan ağ geçidi kaynağını seçin.
3. **Dağıtım**' ı seçin.
4. **Belirteç** metin kutusundaki bir erişim belirtecinin, varsayılan **süre sonu** ve **gizli anahtar** değerleri kullanılarak sizin için otomatik olarak oluşturulduğunu unutmayın. Gerekirse, yeni bir belirteç oluşturmak için, ya da her iki denetimin içindeki istenen değerleri seçin.
5. **Dağıtım betikleri**altında **Kubernetes** sekmesini seçin.
6. **<ağ geçidi-adı>. yıml** dosya bağlantısına tıklayın ve YAML dosyasını indirin.
7. Komutları panoya kaydetmek için **Dağıt** metin kutusunun sağ alt köşesinde bulunan Kopyala simgesini seçin. **copy** `kubectl`
8. Komutları Terminal (veya komut) penceresine yapıştırın. İlk komut 4. adımda oluşturulan erişim belirtecini içeren bir Kubernetes gizli anahtarı oluşturur. İkinci komut, adım 6 ' da indirilen yapılandırma dosyasını Kubernetes kümesine uygular ve dosyanın geçerli dizinde mevcut olmasını bekler.
9. [Varsayılan ad](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/) alanında gerekli Kubernetes nesnelerini oluşturmak için komutları yürütün ve Microsoft Container Registry indirilen [kapsayıcı görüntüsünden](https://aka.ms/apim/sputnik/dhub) kendi kendine barındırılan ağ geçidi Pod 'ları başlatın.
10. Dağıtımın başarılı olup olmadığını denetlemek için normalin altında gösterilen komutu yürütün. Tüm nesnelerin oluşturulması ve pod (ler) in başlatılması için biraz zaman alabilir.
```console
kubectl get deployments
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
<gateway-name>   1/1     1            1           18s
```
11. Hizmetin başarıyla oluşturulup oluşturulmadıysa denetlemek için, gösterilen komutu yürütün. Hizmet IP 'Leri ve bağlantı noktalarınızın farklı olacağını unutmayın.
```console
kubectl get services
NAME             TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
<gateway-name>   LoadBalancer   10.99.236.168   <pending>     80:31620/TCP,443:30456/TCP   9m1s
```
12. Azure portal geri dönüp **genel bakış**' ı seçin.
13. **Status** Bir yeşil onay işareti simgesi ve ardından, YAML dosyasında belirtilen çoğaltma sayısıyla eşleşen düğüm sayısı, dağıtılmış şirket içinde barındırılan ağ geçidi ayırmalarının API Management hizmetiyle başarıyla iletişim kurduğunu ve normal bir "sinyalle" olduğunu onaylar.

![Ağ Geçidi durumu](media/how-to-deploy-self-hosted-gateway-kubernetes/status.png)

> [!TIP]
> Birden <code>kubectl logs deployment/<gateway-name></code> fazla varsa, rastgele seçilmiş bir pod 'dan günlükleri görüntülemek için komutunu yürütün.
> Tüm <code>kubectl logs -h</code> komut seçenekleri için yürütün, örneğin belirli bir pod veya kapsayıcı için günlükleri görüntüleme.

## <a name="production-deployment-considerations"></a>Üretim dağıtımı konuları

### <a name="access-token"></a>Erişim belirteci
Geçerli bir erişim belirteci barındırılmayan şirket içinde barındırılan ağ geçidi, ilişkili API Management hizmetinin yapılandırma verileri uç noktasındaki yapılandırmaya erişemez ve bu sunucudan indiremiyor. Erişim belirteci en fazla 30 gün geçerli olabilir. Yeniden oluşturulmalı ve küme, süresi dolmadan önce el ile veya Otomasyon aracılığıyla yapılandırılmış olmalıdır. Belirteç yenilemeyi otomatikleştirmede, yeni bir belirteç üretilmesi için bu yönetim API 'SI [işlemini](https://docs.microsoft.com/rest/api/apimanagement/2019-12-01/gateway/generatetoken) kullanın. Kubernetes gizli dizilerini yönetme hakkında bilgi için bu [bağlantıyı](https://kubernetes.io/docs/concepts/configuration/secret) izleyin.

### <a name="namespace"></a>Ad Alanı
Kubernetes [ad alanları](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/) , birden fazla takım, proje veya uygulama arasında tek bir kümeyi bölmek için yardım sağlar. Ad alanları, kaynaklar ve adlar için bir kapsam sağlar ve bir kaynak kotası ve erişim denetimi ilkeleriyle ilişkilendirilebilir.
Azure portal belirtilen komutlar, otomatik olarak oluşturulan, her kümede bulunan ve silinebilen **varsayılan** ad alanında Şirket içinde barındırılan ağ geçidi kaynakları oluşturur.
Şirket içinde barındırılan ağ geçidini üretimde ayrı bir ad alanı içinde [oluşturup dağıtmaya](https://kubernetesbyexample.com/ns/) göz önünde bulundurun.

### <a name="number-of-replicas"></a>Çoğaltma sayısı
Üretimde uygun en az sayıda çoğaltma iki.

Varsayılan olarak, şirket içinde barındırılan ağ geçidi bir **Rollingupdate** dağıtım [stratejisi](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#strategy)ile dağıtılır. Varsayılan değerleri gözden geçirin ve özellikle yüksek bir çoğaltma sayısı kullanılırken, [**Maxunavailable**](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#max-unavailable) ve [**maxdalgalanma**](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#max-surge) alanlarını açıkça ayarlamayı düşünün.

### <a name="container-resources"></a>Kapsayıcı kaynakları
Varsayılan olarak, Azure portal belirtilen YAML dosyası kapsayıcı kaynak isteklerini belirtmez.

Her zaman, yürütme sırasında birçok etken nedeniyle, belirli bir iş yükünü desteklemek için gereken çoğaltma sayısını ve, örneğin:

- Kümenin üzerinde çalıştığı belirli donanımlar
- Varlık ve sanallaştırma türü
- Eş zamanlı istemci bağlantılarının sayısı ve oranı
- İstek hızı
- Yapılandırılmış ilkelerin türü ve sayısı
- Yük boyutu ve yükleri arabelleğe alındı veya akışla
- Arka uç hizmeti gecikmesi

Başlangıç noktası olarak 2 çekirdeğe ve 2 GiB 'a kaynak isteği ayarlamayı, yük testi gerçekleştirmeyi ve sonuçları temel alarak büyütmeyi/dışarı veya aşağı/dışarı/dışarı/dışarı/dışarı ölçeklendirmeyi öneririz.

### <a name="container-image-tag"></a>Kapsayıcı görüntüsü etiketi
Azure portal belirtilen YAML dosyası, her zaman otomatik olarak barındırılan ağ geçidi kapsayıcı görüntüsünün en son sürümüne başvuran **en son** etiketi kullanır.

Daha yeni bir sürüme yanlışlıkla yükseltmemek için üretimde belirli bir sürüm etiketi kullanmayı düşünün.

Kullanılabilir etiketlerin tam listesini görmek için bu [bağlantıyı](https://mcr.microsoft.com/v2/azure-api-management/gateway/tags/list) izleyin.

### <a name="dns-policy"></a>DNS ilkesi
DNS ad çözümlemesi, şirket içinde barındırılan ağ geçitlerinde, Azure 'daki bağımlılıklara bağlanma ve arka uç hizmetlerine yönelik API çağrıları gönderme olanlarında kritik bir rol oynar.

Azure portal belirtilen YAML dosyası, ad çözümleme isteklerinin, düğümden devralınan yukarı akış DNS sunucusuna iletilmek üzere küme DNS tarafından çözümlenmemesi için varsayılan [**Clusterfirst**](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#pod-s-dns-policy) ilkesini uygular.

Kubernetes 'de ad çözümleme hakkında bilgi edinmek için bu [bağlantıyı](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service) Izleyin ve [DNS Ilkesi](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#pod-s-dns-policy) veya D[NS yapılandırmasını](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#pod-s-dns-config) kuruluma uygun şekilde özelleştirmeyi düşünün.

### <a name="configuration-backup"></a>Yapılandırma yedeklemesi
Geçici bir Azure bağlantı kesintisi olması halinde şirket içinde barındırılan ağ geçidi davranışı hakkında bilgi edinmek için bu [bağlantıyı](self-hosted-gateway-overview.md#connectivity-to-azure) izleyin.
Şirket içinde barındırılan ağ geçidi kapsayıcısı için yerel depolama birimini yapılandırın; bu nedenle, en son indirilen yapılandırmanın yedek bir kopyasını kalıcı hale getirebileceğinden ve bağlantı kapalıysa, yeniden başlatma sırasında kullanın. Birim bağlama yolu olmalıdır <code>/apim/config</code>. [Buraya](https://github.com/Azure/api-management-self-hosted-gateway/blob/master/examples/self-hosted-gateway-with-configuration-backup.yaml)bir örnek bakın.
Kubernetes 'te depolama hakkında bilgi edinmek için bu [bağlantıyı](https://kubernetes.io/docs/concepts/storage/volumes/)izleyin.

### <a name="local-logs-and-metrics"></a>Yerel Günlükler ve ölçümler
Şirket içinde barındırılan ağ geçidi, [Azure izleyici](api-management-howto-use-azure-monitor.md) 'ye telemetri gönderir ve ilişkili API Management hizmetinde yapılandırma ayarları başına [Azure Application Insights](api-management-howto-app-insights.md) .
[Azure bağlantısı](self-hosted-gateway-overview.md#connectivity-to-azure) geçici olarak kaybedildiyse Azure 'a telemetri akışı kesilir ve kesinti süresi boyunca veriler kaybolur.
API trafiğini gözlemleyebilme ve Azure bağlantı kesintileri sırasında telemetri kaybını önlemeye olanak sağlamak için [Yerel izleme ayarlamayı](how-to-configure-local-metrics-logs.md) düşünün.

## <a name="next-steps"></a>Sonraki adımlar

* Şirket içinde barındırılan ağ geçidi hakkında daha fazla bilgi edinmek için bkz. [Azure API Management Self-barındırılan ağ geçidine genel bakış](self-hosted-gateway-overview.md)