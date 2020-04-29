---
title: Akıllı yönlendirme için Istio kullanma
titleSuffix: Azure Kubernetes Service
description: Azure Kubernetes Service (aks) kümesinde akıllı yönlendirme ve kanarya yayınları dağıtma sağlamak için ampo 'ı nasıl kullanacağınızı öğrenin
author: paulbouwer
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
zone_pivot_groups: client-operating-system
ms.openlocfilehash: 01a7764eb0a353e6842441093f70ad29c9316bbd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80668285"
---
# <a name="use-intelligent-routing-and-canary-releases-with-istio-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (aks) içinde istio ile akıllı yönlendirme ve kanarya sürümlerini kullanma

[Istio][istio-github] , bir Kubernetes kümesindeki mikro hizmetlerde anahtar bir işlevsellik kümesi sağlayan açık kaynaklı bir hizmet kafesidir. Bu özellikler trafik yönetimi, hizmet kimliği ve güvenlik, ilke zorlama ve Observability içerir. Istio hakkında daha fazla bilgi için resmi [nedir?][istio-docs-concepts] belgesine bakın.

Bu makalede, Istio 'nun trafik yönetimi işlevselliğinin nasıl kullanılacağı gösterilmektedir. Akıllı yönlendirme ve kanarya sürümlerini araştırmak için örnek bir aks oylama uygulaması kullanılır.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Uygulamayı dağıtma
> * Uygulamayı güncelleştirme
> * Uygulamanın kanarya bir sürümünü kullanıma alma
> * Dağıtımı sonlandırma

## <a name="before-you-begin"></a>Başlamadan önce

> [!NOTE]
> Bu senaryo, Istio sürümüne `1.3.2`karşı test edilmiştir.

Bu makalede açıklanan adımlarda bir AKS kümesi (RBAC etkinleştirilmiş Kubernetes `1.13` ve üzeri) oluşturdunuz ve kümeyle bir `kubectl` bağlantı oluşturmuş olduğunuz varsayılır. Ayrıca, kümenizde yüklü olan Ida gerekir.

Bu öğelerin herhangi biriyle ilgili yardıma ihtiyacınız varsa, aks [hızlı][aks-quickstart] başlangıcı ' na bakın ve [aks kılavuzuna Istio 'yu yükleyebilirsiniz][istio-install] .

## <a name="about-this-application-scenario"></a>Bu uygulama senaryosu hakkında

Örnek AKS oylama uygulaması, kullanıcılara iki oylama seçeneği (**kediler** veya **köpekler**) sağlar. Her seçenek için oy sayısını sürekli olarak sürdüren bir depolama bileşeni vardır. Ayrıca, her bir seçenek için oy saçılması etrafında ayrıntılar sağlayan bir analiz bileşeni de vardır.

Bu uygulama senaryosunda, `1.0` bir analiz bileşeninin oylama uygulaması ve sürümü `1.0` dağıtarak başlatılır. Analytics bileşeni, Oy sayısı için basit sayımlar sağlar. Oylama uygulaması ve analiz bileşeni, Reddir tarafından `1.0` desteklenen depolama bileşeni sürümü ile etkileşim kurar.

Analiz bileşenini, sayımlar ve artık `1.1`toplam ve yüzdeleri sağlayan sürüme yükseltirsiniz.

Bir kanarya sürümü aracılığıyla uygulamanın `2.0` test sürümünün bir alt kümesi. Bu yeni sürüm, bir MySQL veritabanı tarafından desteklenen bir depolama bileşeni kullanır.

Sürüm `2.0` , Kullanıcı alt kümesinde beklendiği gibi çalıştığından emin olduktan sonra tüm kullanıcılarınıza sürümü `2.0` kullanıma sunuyoruz.

## <a name="deploy-the-application"></a>Uygulamayı dağıtma

Uygulamayı Azure Kubernetes Service (AKS) kümenize dağıtarak başlayalım. Aşağıdaki diyagramda, bu bölümün sonuna kadar çalışan, tüm bileşenlerin sürümü `1.0` , istio giriş ağ geçidi aracılığıyla hizmet verilen gelen isteklerle birlikte verilmiştir:

![AKS oylama uygulama bileşenleri ve yönlendirme.](media/servicemesh/istio/scenario-routing-components-01.png)

Bu makaleyle birlikte izlemeniz gereken yapıtlar [Azure-Samples/aks-oylama-App][github-azure-sample] GitHub deposunda bulunabilir. Yapıtları indirebilir veya depoyu şu şekilde klonlayabilirsiniz:

```console
git clone https://github.com/Azure-Samples/aks-voting-app.git
```

İndirilen/kopyalanan depolarda aşağıdaki klasöre geçin ve sonraki tüm adımları bu klasörden çalıştırın:

```console
cd aks-voting-app/scenarios/intelligent-routing-with-istio
```

İlk olarak, aşağıdaki gibi adlı `voting` örnek aks oylama uygulaması için aks kümenizde bir ad alanı oluşturun:

```console
kubectl create namespace voting
```

Ad alanını ile `istio-injection=enabled`etiketleyin. Bu etiket, bu ad alanındaki tüm yığınlarınızın tüm yığınlarınıza otomatik olarak istik-proxy 'leri eklemesini sağlar.

```console
kubectl label namespace voting istio-injection=enabled
```

Şimdi AKS oylama uygulaması için bileşenleri oluşturalım. Önceki bir adımda oluşturulan `voting` ad alanında bu bileşenleri oluşturun.

```console
kubectl apply -f kubernetes/step-1-create-voting-app.yaml --namespace voting
```

Aşağıdaki örnek çıktıda oluşturulan kaynaklar gösterilmektedir:

```output
deployment.apps/voting-storage-1-0 created
service/voting-storage created
deployment.apps/voting-analytics-1-0 created
service/voting-analytics created
deployment.apps/voting-app-1-0 created
service/voting-app created
```

> [!NOTE]
> IBU, Pod ve hizmetler etrafında bazı özel gereksinimlere sahiptir. Daha fazla bilgi için bkz. [pods ve hizmet belgeleri Için Istio gereksinimleri][istio-requirements-pods-and-services].

Oluşturulan Pod 'leri görmek için [kubectl Get Pod][kubectl-get] komutunu aşağıdaki gibi kullanın:

```console
kubectl get pods -n voting --show-labels
```

Aşağıdaki örnek çıktıda, `voting-app` Pod 'un üç örneği ve hem hem de `voting-analytics` `voting-storage` sayısının tek bir örneği gösterilmektedir. Her birinin iki kapsayıcısı vardır. Bu kapsayıcılardan biri bileşen olur ve diğeri `istio-proxy`:

```output
NAME                                    READY     STATUS    RESTARTS   AGE   LABELS
voting-analytics-1-0-57c7fccb44-ng7dl   2/2       Running   0          39s   app=voting-analytics,pod-template-hash=57c7fccb44,version=1.0
voting-app-1-0-956756fd-d5w7z           2/2       Running   0          39s   app=voting-app,pod-template-hash=956756fd,version=1.0
voting-app-1-0-956756fd-f6h69           2/2       Running   0          39s   app=voting-app,pod-template-hash=956756fd,version=1.0
voting-app-1-0-956756fd-wsxvt           2/2       Running   0          39s   app=voting-app,pod-template-hash=956756fd,version=1.0
voting-storage-1-0-5d8fcc89c4-2jhms     2/2       Running   0          39s   app=voting-storage,pod-template-hash=5d8fcc89c4,version=1.0
```

Pod hakkındaki bilgileri görmek için, `voting-analytics` Pod 'ı seçmek üzere etiket seçicileri ile [kubectl açıklagöster][kubectl-describe] komutunu kullanacağız. Pod 'da bulunan iki kapsayıcının ayrıntılarını göstermek için çıktıyı filtreleyeceğiz:

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - routing scenario - show autoinjected proxy](includes/servicemesh/istio/scenario-routing-show-proxy-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - routing scenario - show autoinjected proxy](includes/servicemesh/istio/scenario-routing-show-proxy-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - routing scenario - show autoinjected proxy](includes/servicemesh/istio/scenario-routing-show-proxy-powershell.md)]

::: zone-end

Bu [ağ geçidini][istio-reference-gateway] ve [sanal hizmeti][istio-reference-virtualservice]oluşturana kadar oylama uygulamasına bağlanamazsınız. Bu Istio kaynakları, trafiği varsayılan Istio ağ geçidinden uygulamamıza yönlendirir.

> [!NOTE]
> **Ağ geçidi** , hizmet ağı tarafında gelen veya giden http ve TCP trafiğini alan bir bileşendir.
> 
> Bir **sanal hizmet** , bir veya daha fazla hedef hizmet için bir yönlendirme kuralları kümesi tanımlar.

Ağ geçidini `kubectl apply` ve sanal hizmet YAML 'yi dağıtmak için komutunu kullanın. Bu kaynakların dağıtıldığı ad alanını belirtmeyi unutmayın.

```console
kubectl apply -f istio/step-1-create-voting-app-gateway.yaml --namespace voting
```

Aşağıdaki örnek çıktı yeni ağ geçidini ve oluşturulmakta olan sanal hizmeti gösterir:

```output
virtualservice.networking.istio.io/voting-app created
gateway.networking.istio.io/voting-app-gateway created
```

Aşağıdaki komutu kullanarak, Istio giriş ağ geçidinin IP adresini alın:

```output
kubectl get service istio-ingressgateway --namespace istio-system -o jsonpath='{.status.loadBalancer.ingress[0].ip}'
```

Aşağıdaki örnek çıktıda, giriş ağ geçidinin IP adresi gösterilmektedir:

```output
20.188.211.19
```

Bir tarayıcı açın ve IP adresini yapıştırın. Örnek AKS oylama uygulaması görüntülenir.

![IKS oylama uygulaması, ampo 'un etkin olduğu AKS kümesinde çalışıyor.](media/servicemesh/istio/scenario-routing-deploy-app-01.png)

Ekranın altındaki bilgiler `1.0` , uygulamanın sürümü `voting-app` ve `1.0` sürümü `voting-storage` (redsıs) kullandığını gösterir.

## <a name="update-the-application"></a>Uygulamayı güncelleştirme

Analiz bileşeninin yeni bir sürümünü dağıtalım. Bu yeni sürüm `1.1` , her kategori için sayıma ek olarak toplamları ve yüzdeleri görüntüler.

Aşağıdaki diyagramda, bu bölümün sonunda ne çalıştığını gösterilmektedir-bileşenimizin `1.1` `voting-analytics` yalnızca bir sürümünde `voting-app` bileşenden yönlendirilen trafik vardır. Bileşenimizin `1.0` `voting-analytics` sürümü çalışmaya devam ediyor ve `voting-analytics` hizmet tarafından başvurulduğu halde, istio proxy 'leri, giden ve giden trafiğe izin vermez.

![AKS oylama uygulama bileşenleri ve yönlendirme.](media/servicemesh/istio/scenario-routing-components-02.png)

`voting-analytics` Bileşenin sürümünü `1.1` dağıtalım. Bu bileşeni `voting` ad alanında oluştur:

```console
kubectl apply -f kubernetes/step-2-update-voting-analytics-to-1.1.yaml --namespace voting
```

Aşağıdaki örnek çıktıda oluşturulan kaynaklar gösterilmektedir:

```output
deployment.apps/voting-analytics-1-1 created
```

Örnek AKS oylama uygulamasını bir tarayıcıda yeniden açarak, önceki adımda elde edilen Istio giriş ağ geçidinin IP adresini kullanın.

Tarayıcınız aşağıda gösterilen iki görünüm arasında alternatifler vardır. Yalnızca tek etiketli seçici ( `voting-analytics` `app: voting-analytics`) içeren bileşen için bir Kubernetes [hizmeti][kubernetes-service] kullandığınızdan, Kubernetes, bu seçiciyle eşleşen Pod 'ler arasında hepsini bir kez deneme için varsayılan davranışı kullanır. Bu durumda, hem sürüm `1.0` `1.1` hem de yığınlarınızın `voting-analytics` her ikisi de olur.

![AKS oylama uygulamamız üzerinde çalışan analiz bileşeninin sürüm 1,0.](media/servicemesh/istio/scenario-routing-deploy-app-01.png)

![AKS oylama uygulamamız üzerinde çalışan analiz bileşeninin sürüm 1,1.](media/servicemesh/istio/scenario-routing-update-app-01.png)

`voting-analytics` Bileşenin iki sürümü arasındaki geçişi aşağıdaki şekilde görselleştirebilirsiniz. Kendi Istik giriş ağ geçidinizin IP adresini kullanmayı unutmayın.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - routing scenario - loop through results](includes/servicemesh/istio/scenario-routing-loop-results-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - routing scenario - loop through results](includes/servicemesh/istio/scenario-routing-loop-results-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - routing scenario - loop through results](includes/servicemesh/istio/scenario-routing-loop-results-powershell.md)]

::: zone-end

Aşağıdaki örnek çıktıda, sürümler arasında site anahtarları olarak döndürülen Web sitesinin ilgili bölümü gösterilmektedir:

```output
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
```

### <a name="lock-down-traffic-to-version-11-of-the-application"></a>Trafiği uygulamanın 1,1 sürümüne kilitle

`1.1` Şimdi, `voting-analytics` trafiği yalnızca bileşenin sürümüne ve `1.0` `voting-storage` bileşen sürümüne kilitleyelim. Daha sonra diğer tüm bileşenler için yönlendirme kuralları tanımlarsınız.

> * Bir **sanal hizmet** , bir veya daha fazla hedef hizmet için bir yönlendirme kuralları kümesi tanımlar.
> * Bir **hedef kural** , trafik ilkelerini ve sürüme özgü ilkeleri tanımlar.
> * Bir **ilke** , iş yükleri üzerinde hangi kimlik doğrulama yöntemlerinin kabul edilebilir olduğunu tanımlar.

İçindeki sanal `kubectl apply` hizmet tanımını değiştirmek `voting-app` ve diğer bileşenlere yönelik [hedef kuralları][istio-reference-destinationrule] ve [sanal Hizmetleri][istio-reference-virtualservice] eklemek için komutunu kullanın. Karşılıklı TLS ve istemci [Policy][istio-reference-policy] sertifikaları kullanılarak Hizmetler `voting` arasındaki tüm iletişim güvenliğini sağlamak Için ad alanına bir ilke ekleyeceksiniz.

* Ilke, `voting` ad `peers.mtls.mode` ALANıNDAKI hizmetleriniz `STRICT` arasında karşılıklı TLS 'nin uygulanmasını sağlamak için olarak ayarlanmıştır.
* Ayrıca, `trafficPolicy.tls.mode` tüm hedef kurallarımızda olarak ' i olarak `ISTIO_MUTUAL` ayarlandık. İstio, güçlü kimlikler içeren hizmetler sağlar ve karşılıklı olarak yönettiği karşılıklı TLS ve istemci sertifikalarını kullanarak hizmetler arasındaki iletişimin güvenliğini sağlar.

```console
kubectl apply -f istio/step-2-update-and-add-routing-for-all-components.yaml --namespace voting
```

Aşağıdaki örnek çıktıda yeni Ilke, hedef kuralları ve güncelleştirilmekte/oluşturulan sanal hizmetler gösterilmektedir:

```output
virtualservice.networking.istio.io/voting-app configured
policy.authentication.istio.io/default created
destinationrule.networking.istio.io/voting-app created
destinationrule.networking.istio.io/voting-analytics created
virtualservice.networking.istio.io/voting-analytics created
destinationrule.networking.istio.io/voting-storage created
virtualservice.networking.istio.io/voting-storage created
```

AKS oylama uygulamasını bir tarayıcıda yeniden açarsanız, bileşen tarafından `1.1` `voting-analytics` `voting-app` yalnızca bileşenin yeni sürümü kullanılır.

![AKS oylama uygulamamız üzerinde çalışan analiz bileşeninin sürüm 1,1.](media/servicemesh/istio/scenario-routing-update-app-01.png)

Artık yalnızca bileşeninizin `1.1` `voting-analytics` sürümüne aşağıda gösterildiği gibi yönlendirildiğini görselleştirebilirsiniz. Kendi Istio giriş ağ geçidinizin IP adresini kullanmayı unutmayın:

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - routing scenario - loop through results](includes/servicemesh/istio/scenario-routing-loop-results-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - routing scenario - loop through results](includes/servicemesh/istio/scenario-routing-loop-results-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - routing scenario - loop through results](includes/servicemesh/istio/scenario-routing-loop-results-powershell.md)]

::: zone-end

Aşağıdaki örnek çıktı, döndürülen Web sitesinin ilgili bölümünü gösterir:

```output
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
```

Şimdi de, tüm hizmetlerimiz arasındaki iletişimleri güvenli hale getirmek için IBU ' ın karşılıklı TLS kullandığını doğrulayalim. Bu işlem için, `istioctl` istemci ikilisinde aşağıdaki formu alan [AuthN TLS-Check][istioctl-authn-tls-check] komutunu kullanacağız.

```console
istioctl authn tls-check <pod-name[.namespace]> [<service>]
```

Bu komut kümesi, bir ad alanında olan ve bir etiket kümesiyle eşleşen tüm yığınlardan belirtilen hizmetlere erişim hakkında bilgi sağlar:

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - routing scenario - verify mtls](includes/servicemesh/istio/scenario-routing-verify-mtls-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - routing scenario - verify mtls](includes/servicemesh/istio/scenario-routing-verify-mtls-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - routing scenario - verify mtls](includes/servicemesh/istio/scenario-routing-verify-mtls-powershell.md)]

::: zone-end

Aşağıdaki örnek çıktıda, yukarıdaki sorgularımızın her biri için karşılıklı TLS 'nin zorunlu olduğu gösterilmektedir. Çıktı Ayrıca, karşılıklı TLS 'yi zorlayan Ilke ve hedef kurallarını da gösterir:

```output
# mTLS configuration between istio ingress pods and the voting-app service
HOST:PORT                                    STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-app.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-app/voting

# mTLS configuration between each of the voting-app pods and the voting-analytics service
HOST:PORT                                          STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-analytics.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-analytics/voting
HOST:PORT                                          STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-analytics.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-analytics/voting
HOST:PORT                                          STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-analytics.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-analytics/voting

# mTLS configuration between each of the voting-app pods and the voting-storage service
HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting
HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting
HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting

# mTLS configuration between each of the voting-analytics version 1.1 pods and the voting-storage service
HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting
```

## <a name="roll-out-a-canary-release-of-the-application"></a>Uygulamanın kanarya bir sürümünü kullanıma alma

Şimdi `voting-app`, `voting-analytics`, ve `voting-storage` bileşenlerinin yeni bir `2.0` sürümünü dağıtalım. Yeni `voting-storage` bileşen redin yerine MySQL kullanır ve `voting-app` ve `voting-analytics` bileşenleri, bu yeni `voting-storage` bileşeni kullanmasına izin verecek şekilde güncelleştirilir.

`voting-app` Bileşen artık Özellik bayrağı işlevini desteklemektedir. Bu özellik bayrağı, bir kullanıcı alt kümesi için istio 'nun kanarya yayın yeteneğini test etmenize olanak tanır.

Aşağıdaki diyagramda, bu bölümün sonunda ne çalıştırdıklarınız gösterilmektedir.

* `1.0` `1.1` `voting-analytics` Bileşenin sürümü, bileşen sürümü ve `1.0` `voting-storage` bileşen sürümü birbirleriyle iletişim kurabilir. `voting-app`
* `2.0` `2.0` `voting-analytics` Bileşenin sürümü, bileşen sürümü ve `2.0` `voting-storage` bileşen sürümü birbirleriyle iletişim kurabilir. `voting-app`
* `voting-app` Bileşenin sürümüne `2.0` yalnızca belirli bir özellik bayrağı ayarlanmış olan kullanıcılar erişebilir. Bu değişiklik, tanımlama bilgisi aracılığıyla bir özellik bayrağı kullanılarak yönetilir.

![AKS oylama uygulama bileşenleri ve yönlendirme.](media/servicemesh/istio/scenario-routing-components-03.png)

İlk olarak, bu yeni bileşenler için istio hedef kurallarını ve sanal hizmetlerini karşılamak olarak güncelleştirin. Bu güncelleştirmeler, trafiği yeni bileşenlere doğru bir şekilde yönlendirmemenizi ve kullanıcıların beklenmeyen bir erişim almaz olmasını sağlar:

```console
kubectl apply -f istio/step-3-add-routing-for-2.0-components.yaml --namespace voting
```

Aşağıdaki örnek çıktıda, güncelleştirilmekte olan hedef kurallar ve sanal hizmetler gösterilmektedir:

```output
destinationrule.networking.istio.io/voting-app configured
virtualservice.networking.istio.io/voting-app configured
destinationrule.networking.istio.io/voting-analytics configured
virtualservice.networking.istio.io/voting-analytics configured
destinationrule.networking.istio.io/voting-storage configured
virtualservice.networking.istio.io/voting-storage configured
```

Ardından, yeni sürüm `2.0` bileşenleri Için Kubernetes nesnelerini ekleyelim. Ayrıca, `voting-storage` hizmeti MySQL için `3306` bağlantı noktasını içerecek şekilde de güncelleştirebilirsiniz:

```console
kubectl apply -f kubernetes/step-3-update-voting-app-with-new-storage.yaml --namespace voting
```

Aşağıdaki örnek çıktıda, Kubernetes nesnelerinin başarıyla güncelleştirildiği veya oluşturulduğu gösterilmektedir:

```output
service/voting-storage configured
secret/voting-storage-secret created
deployment.apps/voting-storage-2-0 created
persistentvolumeclaim/mysql-pv-claim created
deployment.apps/voting-analytics-2-0 created
deployment.apps/voting-app-2-0 created
```

Tüm sürüm `2.0` boşlarını çalıştırmaya kadar bekleyin. Ad alanındaki tüm düğüm üzerindeki değişiklikleri izlemek için `-w` izleme anahtarıyla [kubectl Get Pod][kubectl-get] komutunu kullanın: `voting`

```console
kubectl get pods --namespace voting -w
```

Artık oylama uygulamasının sürümü `1.0` ve sürümü `2.0` (Canary) arasında geçiş yapabiliyor olmanız gerekir. Ekranın alt kısmındaki Özellik bayrağı değiştirme bir tanımlama bilgisi ayarlar. Bu tanımlama bilgisi, `voting-app` sanal hizmet tarafından kullanıcıları yeni sürüme `2.0`yönlendirmek için kullanılır.

![AKS oylama uygulaması-Özellik bayrağının 1,0 sürümü ayarlanmamış.](media/servicemesh/istio/scenario-routing-canary-release-01.png)

![AKS oylama uygulaması-Özellik bayrağının 2,0 sürümü ayarlanır.](media/servicemesh/istio/scenario-routing-canary-release-02.png)

Oy sayıları, uygulamanın sürümleri arasında farklılık görüntüler. Bu fark, iki farklı depolama arka ucu kullandığınızı vurgular.

## <a name="finalize-the-rollout"></a>Dağıtımı sonlandırma

Kanarya sürümünü başarıyla test edildikten sonra, `voting-app` sanal hizmeti tüm trafiği `2.0` `voting-app` bileşen sürümüne yönlendirecek şekilde güncelleştirin. Tüm kullanıcılar, özellik bayrağının `2.0` ayarlanmış olup olmamasına bakılmaksızın uygulamanın sürümünü görür:

![AKS oylama uygulama bileşenleri ve yönlendirme.](media/servicemesh/istio/scenario-routing-components-04.png)

Artık etkin olmasını istemediğiniz bileşenlerin sürümlerini kaldırmak için tüm hedef kurallarını güncelleştirin. Ardından, bu sürümlere başvurmayı durdurmak için tüm sanal Hizmetleri güncelleştirin.

Artık bileşenlerin eski sürümlerinden hiçbirine hiç trafik olmadığından, bu bileşenlere yönelik tüm dağıtımları güvenle silebilirsiniz.

![AKS oylama uygulama bileşenleri ve yönlendirme.](media/servicemesh/istio/scenario-routing-components-05.png)

Artık AKS oylama uygulamasının yeni bir sürümünü başarıyla tamamladınız.

## <a name="clean-up"></a>Temizleme 

Bu senaryoda kullandığımız AKS oylama uygulamasını, `voting` ad alanını aşağıdaki şekilde silerek aks kümenizdeki bir şekilde kaldırabilirsiniz:

```console
kubectl delete namespace voting
```

Aşağıdaki örnek çıktı, AKS oylama uygulamasının tüm bileşenlerinin AKS kümenizdeki kaldırıldığını gösterir.

```output
namespace "voting" deleted
```

## <a name="next-steps"></a>Sonraki adımlar

[Istio BookInfo uygulama örneğini][istio-bookinfo-example]kullanarak ek senaryolar keşfedebilirsiniz.

<!-- LINKS - external -->
[github-azure-sample]: https://github.com/Azure-Samples/aks-voting-app
[istio-github]: https://github.com/istio/istio

[istio]: https://istio.io
[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-requirements-pods-and-services]: https://istio.io/docs/setup/kubernetes/prepare/requirements/
[istio-reference-gateway]: https://istio.io/docs/reference/config/networking/v1alpha3/gateway/
[istio-reference-policy]: https://istio.io/docs/reference/config/istio.authentication.v1alpha1/#Policy
[istio-reference-virtualservice]: https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/
[istio-reference-destinationrule]: https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/
[istioctl-authn-tls-check]: https://istio.io/docs/reference/commands/istioctl/#istioctl-authn-tls-check

[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-install]: ./servicemesh-istio-install.md
