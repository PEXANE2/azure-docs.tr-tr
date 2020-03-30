---
title: Azure Kubernetes Hizmetinde (AKS) Istio ile akıllı yönlendirme ve kanarya yayınları
description: Bir Azure Kubernetes Hizmeti (AKS) kümesinde akıllı yönlendirme sağlamak ve kanarya sürümlerini dağıtmak için Istio'nun nasıl kullanılacağını öğrenin
author: paulbouwer
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
zone_pivot_groups: client-operating-system
ms.openlocfilehash: 4a695957c287e69ff6b40e5a01254a729eaae441
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78272997"
---
# <a name="use-intelligent-routing-and-canary-releases-with-istio-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetinde (AKS) Istio ile akıllı yönlendirme ve kanarya sürümlerini kullanın

[Istio,][istio-github] Bir Kubernetes kümesindeki mikro hizmetler de önemli bir işlevsellik kümesi sağlayan açık kaynak kodlu bir hizmet kafesidir. Bu özellikler arasında trafik yönetimi, hizmet kimliği ve güvenliği, ilke zorlama ve gözlemlenebilirlik yer almaktadır. Istio hakkında daha fazla bilgi için resmi [Istio nedir?][istio-docs-concepts]

Bu makalede, Istio'nun trafik yönetimi işlevini nasıl kullanacağınızı gösterilmektedir. Örnek bir AKS oylama uygulaması akıllı yönlendirme ve kanarya bültenleri keşfetmek için kullanılır.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Uygulamayı dağıtma
> * Uygulamayı güncelleştirme
> * Uygulamanın bir kanarya sürümü dışarı rulo
> * Lansmanı sonuçlandırın

## <a name="before-you-begin"></a>Başlamadan önce

> [!NOTE]
> Bu senaryo Istio sürümüne `1.3.2`karşı test edilmiştir.

Bu makalede ayrıntılı adımlar bir AKS kümesi (Kubernetes `1.13` ve yukarıda, RBAC etkin) oluşturduk ve küme ile bir `kubectl` bağlantı kurduk varsayalım. Ayrıca Istio'nun kümenizde yüklü olması gerekir.

Bu öğelerden herhangi biriyle ilgili yardıma ihtiyacınız varsa, AKS kılavuzunda [AKS hızlı başlat][aks-quickstart] ın ve [Istio'yu yükleyin.][istio-install]

## <a name="about-this-application-scenario"></a>Bu uygulama senaryosu hakkında

Örnek AKS oylama uygulaması kullanıcılara iki oylama seçeneği **(Kediler** veya **Köpekler)** sağlar. Her seçenek için oy sayısını kalıcı bir depolama bileşeni vardır. Ayrıca, her seçenek için kullanılan oylar hakkında ayrıntılı bilgi sağlayan bir analiz bileşeni de vardır.

Bu uygulama senaryosunda, oylama uygulamasının sürümünü `1.0` ve `1.0` analiz bileşeninin sürümünü dağıtarak başlarsınız. Analiz bileşeni oy sayısı için basit sayımlar sağlar. Oylama uygulaması ve analiz bileşeni, `1.0` Redis tarafından desteklenen depolama bileşeninin sürümüyle etkileşime girerek.

Analitik bileşenini, sayımlar `1.1`ve şimdi toplamlar ve yüzdeler sağlayan sürüme yükseltirsiniz.

Kullanıcıların bir alt kümesi `2.0` bir kanarya sürümü ile uygulamanın sürümünü test. Bu yeni sürüm, MySQL veritabanı tarafından desteklenen bir depolama bileşeni kullanır.

Sürümün `2.0` kullanıcı alt kümenizde beklendiği gibi çalıştığından emin olduğunuzda, `2.0` sürümü tüm kullanıcılarınıza dağıtabilirsiniz.

## <a name="deploy-the-application"></a>Uygulamayı dağıtma

Uygulamayı Azure Kubernetes Hizmeti (AKS) kümenize dağıtarak başlayalım. Aşağıdaki diyagram, bu bölümün sonunda neyin çalıştığını `1.0` gösterir - Gelen isteklere sahip tüm bileşenlerin Sürümü Istio giriş ağ geçidi üzerinden servis edilir:

![AKS Oylama uygulaması bileşenleri ve yönlendirme.](media/servicemesh/istio/scenario-routing-components-01.png)

Bu makaleyle birlikte izlemeniz gereken yapılar [Azure Örnekleri/aks oylama uygulaması][github-azure-sample] GitHub repo'sunda kullanılabilir. Yapıları indirebilir veya repo'yu aşağıdaki gibi kopyalayabilirsiniz:

```console
git clone https://github.com/Azure-Samples/aks-voting-app.git
```

İndirilen / klonlanmış repo'da aşağıdaki klasöre değiştirin ve bu klasörden sonraki tüm adımları çalıştırın:

```console
cd aks-voting-app/scenarios/intelligent-routing-with-istio
```

İlk olarak, aşağıdaki gibi adlı `voting` örnek AKS oylama uygulaması için AKS kümenizde bir ad alanı oluşturun:

```console
kubectl create namespace voting
```

Ad alanını ' `istio-injection=enabled`ile etiketle. Bu etiket, Istio'ya istio-proxies'i bu ad alanında tüm bölmelerinize otomatik olarak yan araba olarak enjekte etmesini bildirir.

```console
kubectl label namespace voting istio-injection=enabled
```

Şimdi AKS Oylama uygulaması için bileşenleri oluşturalım. Bu bileşenleri önceki `voting` adımda oluşturulan ad alanında oluşturun.

```console
kubectl apply -f kubernetes/step-1-create-voting-app.yaml --namespace voting
```

Aşağıdaki örnek çıktı, oluşturulan kaynakları gösterir:

```output
deployment.apps/voting-storage-1-0 created
service/voting-storage created
deployment.apps/voting-analytics-1-0 created
service/voting-analytics created
deployment.apps/voting-app-1-0 created
service/voting-app created
```

> [!NOTE]
> Istio pods ve hizmetler etrafında bazı özel gereksinimleri vardır. Daha fazla bilgi [için, Podlar ve Hizmetler belgeleri için Istio Gereksinimleri'ne][istio-requirements-pods-and-services]bakın.

Oluşturulan bölmeleri görmek için [kubectl get pods][kubectl-get] komutunu aşağıdaki gibi kullanın:

```console
kubectl get pods -n voting --show-labels
```

Aşağıdaki örnek çıktı, bölmenin `voting-app` üç örneği ve hem bölmenin `voting-analytics` `voting-storage` hem de bölmenin tek bir örneğini gösterir. Kapsüllerin her birinde iki konteyner var. Bu kapsayıcılardan biri bileşen, diğeri `istio-proxy`ise:

```output
NAME                                    READY     STATUS    RESTARTS   AGE   LABELS
voting-analytics-1-0-57c7fccb44-ng7dl   2/2       Running   0          39s   app=voting-analytics,pod-template-hash=57c7fccb44,version=1.0
voting-app-1-0-956756fd-d5w7z           2/2       Running   0          39s   app=voting-app,pod-template-hash=956756fd,version=1.0
voting-app-1-0-956756fd-f6h69           2/2       Running   0          39s   app=voting-app,pod-template-hash=956756fd,version=1.0
voting-app-1-0-956756fd-wsxvt           2/2       Running   0          39s   app=voting-app,pod-template-hash=956756fd,version=1.0
voting-storage-1-0-5d8fcc89c4-2jhms     2/2       Running   0          39s   app=voting-storage,pod-template-hash=5d8fcc89c4,version=1.0
```

Pod hakkındaki bilgileri görmek için, pod'u seçmek için etiket seçicileri olan `voting-analytics` [kubectl describe pod][kubectl-describe] komutunu kullanırız. Bölmede bulunan iki kapsayıcının ayrıntılarını göstermek için çıktıyı filtreleriz:

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - routing scenario - show autoinjected proxy](includes/servicemesh/istio/scenario-routing-show-proxy-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - routing scenario - show autoinjected proxy](includes/servicemesh/istio/scenario-routing-show-proxy-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - routing scenario - show autoinjected proxy](includes/servicemesh/istio/scenario-routing-show-proxy-powershell.md)]

::: zone-end

Istio [Ağ Geçidi][istio-reference-gateway] ve [Sanal Hizmet'i][istio-reference-virtualservice]oluşturmadan oylama uygulamasına bağlanamazsınız. Bu Istio kaynakları trafiği varsayılan Istio giriş ağ geçidinden uygulamamıza yönlendirir.

> [!NOTE]
> **Ağ Geçidi,** gelen veya giden HTTP ve TCP trafiğini alan servis kafesinin kenarındaki bir bileşendir.
> 
> **Sanal Hizmet,** bir veya daha fazla hedef hizmet için bir yönlendirme kuralları kümesi tanımlar.

Ağ `kubectl apply` Geçidi ve Sanal Hizmet yaml dağıtmak için komutu kullanın. Bu kaynakların dağıtılan ad alanını belirtmeyi unutmayın.

```console
kubectl apply -f istio/step-1-create-voting-app-gateway.yaml --namespace voting
```

Aşağıdaki örnek çıktı, oluşturulan yeni Ağ Geçidi ve Sanal Hizmet'i gösterir:

```output
virtualservice.networking.istio.io/voting-app created
gateway.networking.istio.io/voting-app-gateway created
```

Aşağıdaki komutu kullanarak Istio Ingress Ağ Geçidi'nin IP adresini edinin:

```output
kubectl get service istio-ingressgateway --namespace istio-system -o jsonpath='{.status.loadBalancer.ingress[0].ip}'
```

Aşağıdaki örnek çıktı, Giriş Ağ Geçidi'nin IP adresini gösterir:

```output
20.188.211.19
```

Bir tarayıcı açın ve IP adresine yapıştırın. Örnek AKS oylama uygulaması görüntülenir.

![Istio cluster'Da çalışan AKS Oylama uygulaması AKS kümesini etkinleştirildi.](media/servicemesh/istio/scenario-routing-deploy-app-01.png)

Ekranın alt kısmındaki bilgiler, `1.0` uygulamanın `voting-app` `1.0` `voting-storage` (Redis) sürümünü ve sürümünü kullandığını gösterir.

## <a name="update-the-application"></a>Uygulamayı güncelleştirme

Analiz bileşeninin yeni bir sürümünü dağıtalım. Bu yeni `1.1` sürüm, her kategori için sayıma ek olarak toplamları ve yüzdeleri görüntüler.

Aşağıdaki diyagram, bu bölümün sonunda ne çalışır durumda `1.1` olacağını `voting-analytics` gösterir - bileşenimizin `voting-app` yalnızca sürümü bileşenden yönlendirilen trafiktir. Bileşenimizin `1.0` `voting-analytics` sürümü devam etse ve `voting-analytics` hizmet tarafından başvurulsa da, Istio vekilleri trafiğe izin vermez ve ondan.

![AKS Oylama uygulaması bileşenleri ve yönlendirme.](media/servicemesh/istio/scenario-routing-components-02.png)

Bileşenin `1.1` `voting-analytics` sürümünü dağıtalım. Bu bileşeni `voting` ad alanında oluşturun:

```console
kubectl apply -f kubernetes/step-2-update-voting-analytics-to-1.1.yaml --namespace voting
```

Aşağıdaki örnek çıktı, oluşturulan kaynakları gösterir:

```output
deployment.apps/voting-analytics-1-1 created
```

Önceki adımda elde edilen Istio Ingress Gateway IP adresini kullanarak örnek AKS oylama uygulamasını bir tarayıcıda yeniden açın.

Tarayıcınız aşağıda gösterilen iki görünüm arasında geçiş yapar. Bileşen için yalnızca tek bir etiket seçici () ile bir`app: voting-analytics`Kubernetes Hizmeti kullandığınızdan, Kubernetes bu seçiciyle eşleşen bölmeler arasında yuvarlanma varsayılan davranışını kullanır. [Service][kubernetes-service] `voting-analytics` Bu durumda, hem sürümü `1.0` `1.1` hem `voting-analytics` de pods olduğunu.

![AKS Oylama uygulamamızda çalışan analiz bileşeninin Sürüm 1.0.0.0.'ı.](media/servicemesh/istio/scenario-routing-deploy-app-01.png)

![AKS Oylama uygulamamızda çalışan analiz bileşeninin Sürüm 1.1.](media/servicemesh/istio/scenario-routing-update-app-01.png)

`voting-analytics` Bileşenin iki sürümü arasındaki geçişi aşağıdaki gibi görselleştirebilirsiniz. Kendi Istio Ingress Ağ Geçidinizin IP adresini kullanmayı unutmayın.

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - routing scenario - loop through results](includes/servicemesh/istio/scenario-routing-loop-results-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - routing scenario - loop through results](includes/servicemesh/istio/scenario-routing-loop-results-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - routing scenario - loop through results](includes/servicemesh/istio/scenario-routing-loop-results-powershell.md)]

::: zone-end

Aşağıdaki örnek çıktı, site sürümler arasında geçiş yaptıkça döndürülen web sitesinin ilgili bölümünü gösterir:

```output
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
```

### <a name="lock-down-traffic-to-version-11-of-the-application"></a>Trafiğin uygulamanın 1.1 sürümüne kilitleme

Şimdi `1.1` trafiği `voting-analytics` yalnızca bileşenin sürümüne ve `1.0` `voting-storage` bileşenin sürümüne kilitleyelim. Daha sonra tüm diğer bileşenler için yönlendirme kuralları tanımlarsınız.

> * **Sanal Hizmet,** bir veya daha fazla hedef hizmet için bir yönlendirme kuralları kümesi tanımlar.
> * **Hedef Kuralı** trafik ilkelerini ve sürüm özel ilkeleri tanımlar.
> * **İlke,** iş yükünde hangi kimlik doğrulama yöntemlerinin kabul edilebilmiş olabileceğini tanımlar.

`kubectl apply` Sanal `voting-app` Hizmet tanımını değiştirmek ve diğer bileşenler için Hedef [Kuralları][istio-reference-destinationrule] ve Sanal [Hizmetler][istio-reference-virtualservice] eklemek için komutu kullanın. Hizmetler arasındaki tüm iletişimin `voting` karşılıklı TLS ve istemci sertifikaları kullanılarak güvence altına alınmıştır sağlamak için ad alanına bir [İlke][istio-reference-policy] eklersiniz.

* Politika, `peers.mtls.mode` hizmetleriniz `STRICT` arasında `voting` ad alanı içinde karşılıklı TLS uygulanmasını sağlamak için ayarlanmıştır.
* Ayrıca tüm `trafficPolicy.tls.mode` Hedef `ISTIO_MUTUAL` Kurallarımızda da yerimizi alıyoruz. Istio, güçlü kimliklere sahip hizmetler sunar ve karşılıklı TLS ve Istio'nun şeffaf bir şekilde yönettiği istemci sertifikalarını kullanarak hizmetler arasındaki iletişimi sağlar.

```console
kubectl apply -f istio/step-2-update-and-add-routing-for-all-components.yaml --namespace voting
```

Aşağıdaki örnek çıktı, güncelleştirilen/oluşturulan yeni İlke, Hedef Kuralları ve Sanal Hizmetler'i gösterir:

```output
virtualservice.networking.istio.io/voting-app configured
policy.authentication.istio.io/default created
destinationrule.networking.istio.io/voting-app created
destinationrule.networking.istio.io/voting-analytics created
virtualservice.networking.istio.io/voting-analytics created
destinationrule.networking.istio.io/voting-storage created
virtualservice.networking.istio.io/voting-storage created
```

AKS Oylama uygulamasını bir tarayıcıda yeniden açarsanız, `1.1` `voting-analytics` `voting-app` bileşenin yalnızca yeni sürümü bileşen tarafından kullanılır.

![AKS Oylama uygulamamızda çalışan analiz bileşeninin Sürüm 1.1.](media/servicemesh/istio/scenario-routing-update-app-01.png)

Artık yalnızca bileşeninizin sürümüne `1.1` aşağıdaki gibi `voting-analytics` yönlendirildiğinize göre görüntüleyebilirsiniz. Kendi Istio Ingress Ağ Geçidinizin IP adresini kullanmayı unutmayın:

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - routing scenario - loop through results](includes/servicemesh/istio/scenario-routing-loop-results-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - routing scenario - loop through results](includes/servicemesh/istio/scenario-routing-loop-results-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - routing scenario - loop through results](includes/servicemesh/istio/scenario-routing-loop-results-powershell.md)]

::: zone-end

Aşağıdaki örnek çıktı, döndürülen web sitesinin ilgili bölümünü gösterir:

```output
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
```

Şimdi Istio'nun her bir hizmet arasında iletişimi sağlamak için karşılıklı TLS kullandığını doğrulayalım. Bunun için `istioctl` aşağıdaki formu alan istemci ikilisindeki [authn tls-check][istioctl-authn-tls-check] komutunu kullanacağız.

```console
istioctl authn tls-check <pod-name[.namespace]> [<service>]
```

Bu komut kümesi, ad alanında bulunan ve bir etiket kümesiyle eşleşen tüm bölmelerden belirtilen hizmetlere erişim hakkında bilgi sağlar:

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Bash - routing scenario - verify mtls](includes/servicemesh/istio/scenario-routing-verify-mtls-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [Bash - routing scenario - verify mtls](includes/servicemesh/istio/scenario-routing-verify-mtls-bash.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [PowerShell - routing scenario - verify mtls](includes/servicemesh/istio/scenario-routing-verify-mtls-powershell.md)]

::: zone-end

Aşağıdaki örnek çıktı, yukarıdaki sorgularımızın her biri için karşılıklı TLS'nin zorlanmış olduğunu göstermektedir. Çıktı, karşılıklı TLS'yi zorlayan İlke ve Hedef Kuralları'nı da gösterir:

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

## <a name="roll-out-a-canary-release-of-the-application"></a>Uygulamanın bir kanarya sürümü dışarı rulo

`2.0` Şimdi `voting-app`, ve `voting-analytics` `voting-storage` bileşenlerinin yeni bir sürümünü dağıtalım. Yeni `voting-storage` bileşen Redis yerine MySQL kullanır `voting-app` `voting-analytics` ve bileşenler bu yeni `voting-storage` bileşeni kullanmalarına izin verecek şekilde güncelleştirilir.

Bileşen `voting-app` artık özellik bayrağı işlevselliğini destekler. Bu özellik bayrağı, Istio'nun kanarya serbest bırakma yeteneğini bir kullanıcı alt kümesi için test etmenizi sağlar.

Aşağıdaki diyagram, bu bölümün sonunda ne çalışıyor olacak gösterir.

* `voting-app` Bileşenin sürümü, `1.0` `1.1` `voting-analytics` bileşenin sürümü `1.0` ve `voting-storage` bileşenin sürümü birbiriyle iletişim kurabiliyor.
* `voting-app` Bileşenin sürümü, `2.0` `2.0` `voting-analytics` bileşenin sürümü `2.0` ve `voting-storage` bileşenin sürümü birbiriyle iletişim kurabiliyor.
* Bileşenin `2.0` `voting-app` sürümüne yalnızca belirli bir özellik bayrağı kümesi olan kullanıcılar erişebilir. Bu değişiklik, çerez aracılığıyla bir özellik bayrağı kullanılarak yönetilir.

![AKS Oylama uygulaması bileşenleri ve yönlendirme.](media/servicemesh/istio/scenario-routing-components-03.png)

İlk olarak, bu yeni bileşenleriçin hizmet vermek için Istio Hedef Kuralları ve Sanal Hizmetler'i güncelleştirin. Bu güncelleştirmeler, trafiği yeni bileşenlere yanlış yönlendirmemenizi ve kullanıcıların beklenmeyen erişim almamasını sağlar:

```console
kubectl apply -f istio/step-3-add-routing-for-2.0-components.yaml --namespace voting
```

Aşağıdaki örnek çıktı, Hedef Kuralları'nın ve Sanal Hizmetlerin güncelleştirildiğini gösterir:

```output
destinationrule.networking.istio.io/voting-app configured
virtualservice.networking.istio.io/voting-app configured
destinationrule.networking.istio.io/voting-analytics configured
virtualservice.networking.istio.io/voting-analytics configured
destinationrule.networking.istio.io/voting-storage configured
virtualservice.networking.istio.io/voting-storage configured
```

Ardından, yeni sürüm `2.0` bileşenleri için Kubernetes nesnelerini ekleyelim. Ayrıca `voting-storage` MySQL için `3306` bağlantı noktası içerecek şekilde hizmeti güncelleştirin:

```console
kubectl apply -f kubernetes/step-3-update-voting-app-with-new-storage.yaml --namespace voting
```

Aşağıdaki örnek çıktı, Kubernetes nesnelerinin başarıyla güncelleştirilen veya oluşturulduğunu gösterir:

```output
service/voting-storage configured
secret/voting-storage-secret created
deployment.apps/voting-storage-2-0 created
persistentvolumeclaim/mysql-pv-claim created
deployment.apps/voting-analytics-2-0 created
deployment.apps/voting-app-2-0 created
```

Tüm sürüm `2.0` bölmeleri çalışana kadar bekleyin. Ad alanındaki tüm bölmelerde değişiklik `-w` izlemek için saat anahtarı ile `voting` [kubectl get pods][kubectl-get] komutunu kullanın:

```console
kubectl get pods --namespace voting -w
```

Artık oylama uygulamasının sürümü ve `1.0` sürümü `2.0` (kanarya) arasında geçiş yapabilmeniz gerekir. Ekranın alt kısmında özellik bayrağı geçiş bir çerez ayarlar. Bu `voting-app` çerez, kullanıcıları yeni sürüme `2.0`yönlendirmek için Sanal Hizmet tarafından kullanılır.

![AKS Oylama uygulamasının Sürüm 1.0 - özellik bayrağı AYARLANMAZ.](media/servicemesh/istio/scenario-routing-canary-release-01.png)

![AKS Oylama uygulamasının Sürüm 2.0 - özellik bayrağı ayarlanır.](media/servicemesh/istio/scenario-routing-canary-release-02.png)

Oy sayımları uygulamanın sürümleri arasında farklıdır. Bu fark, iki farklı depolama arka uçları kullandığınızı vurgular.

## <a name="finalize-the-rollout"></a>Lansmanı sonuçlandırın

Kanarya sürümünü başarıyla test ettikten sonra, `voting-app` tüm trafiği `voting-app` bileşenin `2.0` sürümüne yönlendirmek için Sanal Hizmet'i güncelleştirin. Tüm kullanıcılar, `2.0` özellik bayrağının ayarlanıp ayarlanmadığına bakılmaksızın uygulamanın sürümünü görür:

![AKS Oylama uygulaması bileşenleri ve yönlendirme.](media/servicemesh/istio/scenario-routing-components-04.png)

Artık etkin olmasını istemediğiniz bileşenlerin sürümlerini kaldırmak için tüm Hedef Kuralları'nı güncelleştirin. Ardından, bu sürümlere başvurmayı durdurmak için tüm Sanal Hizmetler'i güncelleştirin.

Bileşenlerin eski sürümlerinden herhangi birinde artık trafik olmadığından, artık bu bileşenlerin tüm dağıtımlarını güvenle silebilirsiniz.

![AKS Oylama uygulaması bileşenleri ve yönlendirme.](media/servicemesh/istio/scenario-routing-components-05.png)

Artık AKS Oylama Uygulaması'nın yeni bir sürümünü başarıyla kullanıma çıkardınız.

## <a name="clean-up"></a>Temizleme 

Bu senaryoda kullandığımız AKS oylama `voting` uygulamasını, ad alanını aşağıdaki gibi silerek AKS kümenizden kaldırabilirsiniz:

```console
kubectl delete namespace voting
```

Aşağıdaki örnek çıktı, AKS oylama uygulamasının tüm bileşenlerinin AKS kümenizden kaldırıldığını gösterir.

```output
namespace "voting" deleted
```

## <a name="next-steps"></a>Sonraki adımlar

[Istio Bookinfo Application örneğini][istio-bookinfo-example]kullanarak ek senaryoları keşfedebilirsiniz.

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
