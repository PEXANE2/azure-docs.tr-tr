---
title: Kubernetes 'e şirket içinde barındırılan bir ağ geçidi dağıtma | Microsoft Docs
description: Azure API Management şirket içinde barındırılan bir ağ geçidi bileşenini Kubernetes 'e dağıtmayı öğrenin
services: api-management
author: vladvino
manager: gwallace
ms.service: api-management
ms.workload: mobile
ms.topic: article
ms.author: apimpm
ms.date: 04/23/2020
ms.openlocfilehash: abcda4ea4b14f058325318661daa574494268780
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87056387"
---
# <a name="deploy-a-self-hosted-gateway-to-kubernetes"></a>Şirket içinde barındırılan ağ geçidini Kubernetes'e dağıtma

Bu makalede, Azure API Management şirket içinde barındırılan ağ geçidi bileşenini bir Kubernetes kümesine dağıtma adımları açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

- Şu hızlı başlangıcı tamamlayın: [Azure API Management örneği oluşturma](get-started-create-service-instance.md).
- Bir Kubernetes kümesi oluşturun.
   > [!TIP]
   > [Tek düğümlü kümeler](https://kubernetes.io/docs/setup/#learning-environment) geliştirme ve değerlendirme amaçlarıyla iyi çalışır. Şirket içinde veya üretim iş yükleri için bulutta [Kubernetes sertifikalı](https://kubernetes.io/partners/#conformance) çok düğümlü kümeler kullanın.
- [API Management Örneğinizde kendi kendine barındırılan bir ağ geçidi kaynağı sağlayın](api-management-howto-provision-self-hosted-gateway.md).

## <a name="deploy-to-kubernetes"></a>Kubernetes’e dağıtma

1. **Dağıtım ve altyapı**altında **ağ geçitleri** ' ni seçin.
2. Dağıtmak istediğiniz şirket içinde barındırılan ağ geçidi kaynağını seçin.
3. **Dağıtım**' ı seçin.
4. Varsayılan **süre sonu** ve **gizli anahtar** değerlerine bağlı olarak, **belirteç** metin kutusundaki bir erişim belirteci sizin için otomatik olarak oluşturulmuştur. Gerekirse, yeni bir belirteç oluşturmak için ya da her iki denetim içindeki değerleri seçin.
5. **Dağıtım betikleri**altında **Kubernetes** sekmesini seçin.
6. ** \<gateway-name\> . Yıml** dosyası bağlantısını seçin ve YAML dosyasını indirin.
7. Komutları panoya kaydetmek için **Dağıt** metin kutusunun sağ alt köşesindeki **Kopyala** simgesini seçin `kubectl` .
8. Komutları Terminal (veya komut) penceresine yapıştırın. İlk komut, 4. adımda oluşturulan erişim belirtecini içeren bir Kubernetes gizli anahtarı oluşturur. İkinci komut, adım 6 ' da indirilen yapılandırma dosyasını Kubernetes kümesine uygular ve dosyanın geçerli dizinde olmasını bekler.
9. [Varsayılan ad](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/) alanında gerekli Kubernetes nesnelerini oluşturmak ve Microsoft Container Registry indirilen [kapsayıcı görüntüsünden](https://aka.ms/apim/sputnik/dhub) şirket içinde barındırılan ağ geçidi Pod 'yi başlatmak için komutları çalıştırın.
10. Dağıtımın başarılı olup olmadığını denetlemek için aşağıdaki komutu çalıştırın. Tüm nesnelerin oluşturulması ve pod 'nin başlatılması için biraz zaman alabilir.
    ```console
    kubectl get deployments
    NAME             READY   UP-TO-DATE   AVAILABLE   AGE
    <gateway-name>   1/1     1            1           18s
    ```
11. Hizmetin başarıyla oluşturulup oluşturulmadıysa emin olmak için aşağıdaki komutu çalıştırın. Hizmet IP 'Leri ve bağlantı noktalarınızın farklı olacağını unutmayın.
    ```console
    kubectl get services
    NAME             TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
    <gateway-name>   LoadBalancer   10.99.236.168   <pending>     80:31620/TCP,443:30456/TCP   9m1s
    ```
12. Azure portal geri dönüp **genel bakış**' ı seçin.
13. **Durum** değerinin yeşil bir onay işareti gösterdiğini ve ardından, YAML dosyasında belirtilen çoğaltmaların sayısıyla eşleşen bir düğüm sayısı olduğunu onaylayın. Bu durum, dağıtılan şirket içinde barındırılan ağ geçidi 'lerin API Management hizmetiyle başarıyla iletişim kurduğu ve normal bir "sinyalle" sahip olduğu anlamına gelir.

    ![Ağ Geçidi durumu](media/how-to-deploy-self-hosted-gateway-kubernetes/status.png)

> [!TIP]
> Birden <code>kubectl logs deployment/<gateway-name></code> fazla varsa, rastgele seçilmiş bir pod 'dan günlükleri görüntülemek için komutunu çalıştırın.
> <code>kubectl logs -h</code>Belirli bir pod veya kapsayıcı için günlükleri görüntüleme gibi bir dizi komut seçeneği için çalıştırın.

## <a name="production-deployment-considerations"></a>Üretim dağıtımı konuları

### <a name="access-token"></a>Erişim belirteci
Geçerli bir erişim belirteci olmadan, şirket içinde barındırılan bir ağ geçidi, ilişkili API Management hizmetinin uç noktasından yapılandırma verilerine erişemez ve bu verileri indiremez. Erişim belirteci en fazla 30 gün geçerli olabilir. Yeniden oluşturulması gerekir ve küme, süresi dolmadan önce el ile veya Otomasyon yoluyla, yeni bir belirteçle yapılandırılmalıdır. 

Belirteç yenilemeyi otomatikleştirdiğiniz zaman, yeni bir belirteç oluşturmak için [Bu YÖNETIM API işlemini](/rest/api/apimanagement/2019-12-01/gateway/generatetoken) kullanın. Kubernetes gizliliklerini yönetme hakkında bilgi için bkz. [Kubernetes Web sitesi](https://kubernetes.io/docs/concepts/configuration/secret).

### <a name="namespace"></a>Ad Alanı
Kubernetes [ad alanları](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/) , birden fazla takım, proje veya uygulama arasında tek bir kümeyi bölmek için yardım sağlar. Ad alanları, kaynaklar ve adlar için bir kapsam sağlar. Bunlar, bir kaynak kotası ve erişim denetimi ilkeleriyle ilişkilendirilebilir.

Azure portal, **varsayılan** ad alanında Şirket içinde barındırılan ağ geçidi kaynakları oluşturmak için komutlar sağlar. Bu ad alanı otomatik olarak oluşturulur, her kümede bulunur ve silinemez.
Şirket içinde barındırılan bir ağ geçidini üretimde ayrı bir ad alanına [oluşturup dağıtmaya](https://kubernetesbyexample.com/ns/) göz önünde bulundurun.

### <a name="number-of-replicas"></a>Çoğaltma sayısı
Üretime uygun en az çoğaltma sayısı ikidir.

Varsayılan olarak, şirket içinde barındırılan bir ağ geçidi, bir **Rollingupdate** dağıtım [stratejisi](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#strategy)ile dağıtılır. Varsayılan değerleri gözden geçirin ve özellikle yüksek bir çoğaltma sayısı kullanıyorsanız, [Maxunavailable](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#max-unavailable) ve [maxdalgalanma](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#max-surge) alanlarını açıkça ayarlamayı düşünün.

### <a name="container-resources"></a>Kapsayıcı kaynakları
Varsayılan olarak, Azure portal belirtilen YAML dosyası kapsayıcı kaynak isteklerini belirtmez.

Kapsayıcı başına CPU ve bellek kaynakları ve belirli bir iş yükünü desteklemek için gereken kopyaların sayısını güvenilir bir şekilde tahmin etmek ve önermek mümkün değildir. Birçok etken oynatma sırasında, örneğin:

- Kümenin üzerinde çalıştığı belirli donanımlar.
- Varlık ve sanallaştırma türü.
- Eş zamanlı istemci bağlantılarının sayısı ve oranı.
- İstek hızı.
- Yapılandırılmış ilkelerin türü ve sayısı.
- Yük boyutu ve yüklerin arabelleğe alınıp alınmayacağı.
- Arka uç hizmeti gecikmesi.

Kaynak isteklerini, başlangıç noktası olarak iki çekirdeğe ve 2 GiB 'ye ayarlamayı öneririz. Sonuçları temel alarak bir yük testi gerçekleştirin ve ölçeği yukarı/dışarı veya aşağı doğru yapın.

### <a name="container-image-tag"></a>Kapsayıcı görüntüsü etiketi
Azure portal belirtilen YAML dosyası **en son** etiketi kullanır. Bu etiket, her zaman şirket içinde barındırılan ağ geçidi kapsayıcı görüntüsünün en son sürümüne başvurur.

Daha yeni bir sürüme yanlışlıkla yükseltmemek için üretimde belirli bir sürüm etiketi kullanmayı düşünün.

[Kullanılabilir etiketlerin tam listesini indirebilirsiniz](https://mcr.microsoft.com/v2/azure-api-management/gateway/tags/list).

### <a name="dns-policy"></a>DNS ilkesi
DNS ad çözümlemesi, şirket içinde barındırılan bir ağ geçidinin Azure 'daki bağımlılıklara bağlanma ve arka uç hizmetlerine yönelik API çağrıları dağıtma olanlarında kritik bir rol oynar.

Azure portal belirtilen YAML dosyası varsayılan [Clusterfirst](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#pod-s-dns-policy) ilkesini uygular. Bu ilke, düğüm tarafından devralınan yukarı akış DNS sunucusuna iletilmek üzere küme DNS tarafından çözümlenemeyen ad çözümleme isteklerinin oluşmasına neden olur.

Kubernetes 'de ad çözümleme hakkında bilgi edinmek için bkz. [Kubernetes Web sitesi](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service). [DNS ilkesi](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#pod-s-dns-policy) veya [DNS yapılandırmasını](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#pod-s-dns-config) , kuruluma uygun şekilde özelleştirmeyi düşünün.

### <a name="custom-domain-names-and-ssl-certificates"></a>Özel etki alanı adları ve SSL sertifikaları

API Management uç noktaları için özel etki alanı adları kullanıyorsanız, özellikle yönetim uç noktası için özel bir etki alanı adı kullanırsanız, `config.service.endpoint` varsayılan etki alanı adını özel etki alanı adıyla değiştirmek için ** \<gateway-name\> . YAML** dosyasındaki değerini güncelleştirmeniz gerekebilir. Yönetim uç noktasına, Kubernetes kümesindeki şirket içinde barındırılan ağ geçidinin Pod değerinden erişilebildiğinden emin olun.

Bu senaryoda, yönetim uç noktası tarafından kullanılan SSL sertifikası iyi bilinen bir CA sertifikası tarafından imzalanmadıysa, CA sertifikasına şirket içinde barındırılan ağ geçidinin Pod 'u tarafından güvenildiğinden emin olmanız gerekir.

### <a name="configuration-backup"></a>Yapılandırma yedeklemesi
Geçici bir Azure bağlantı kesintisi olması halinde şirket içinde barındırılan ağ geçidi davranışı hakkında bilgi edinmek için, bkz. [Şirket içinde barındırılan ağ geçidine genel bakış](self-hosted-gateway-overview.md#connectivity-to-azure).

Şirket içinde barındırılan ağ geçidi kapsayıcısı için yerel bir depolama birimi yapılandırarak, en son indirilen yapılandırmanın yedek bir kopyasını kalıcı hale getirebilirler. Bağlantı kapalıysa, depolama birimi yeniden başlatma sonrasında yedek kopyayı kullanabilir. Birim bağlama yolu olmalıdır <code>/apim/config</code> . [GitHub](https://github.com/Azure/api-management-self-hosted-gateway/blob/master/examples/self-hosted-gateway-with-configuration-backup.yaml)üzerinde bir örnek görüntüleyin.
Kubernetes 'te depolama hakkında bilgi edinmek için bkz. [Kubernetes Web sitesi](https://kubernetes.io/docs/concepts/storage/volumes/).

### <a name="local-logs-and-metrics"></a>Yerel Günlükler ve ölçümler
Şirket içinde barındırılan ağ geçidi, [Azure izleyici](api-management-howto-use-azure-monitor.md) 'ye ve [azure Application Insights](api-management-howto-app-insights.md) ilişkili API Management hizmetindeki yapılandırma ayarlarına göre telemetri gönderir.
[Azure bağlantısı](self-hosted-gateway-overview.md#connectivity-to-azure) geçici olarak kaybedildiyse Azure 'a telemetri akışı kesilir ve kesinti süresi boyunca veriler kaybolur.
API trafiğini gözlemleyebilme ve Azure bağlantı kesintileri sırasında telemetri kaybını önleme özelliğini güvence altına almak için [Yerel izleme ayarlamayı](how-to-configure-local-metrics-logs.md) göz önünde bulundurun.

## <a name="next-steps"></a>Sonraki adımlar

* Şirket içinde barındırılan ağ geçidi hakkında daha fazla bilgi edinmek için bkz. [Şirket içinde barındırılan ağ geçidine genel bakış](self-hosted-gateway-overview.md).
