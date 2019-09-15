---
title: Azure Kubernetes Service (aks) içinde istio ile akıllı yönlendirme ve kanarya yayınları
description: Azure Kubernetes Service (aks) kümesinde akıllı yönlendirme ve kanarya yayınları dağıtma sağlamak için ampo 'ı nasıl kullanacağınızı öğrenin
services: container-service
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 04/19/2019
ms.author: pabouwer
ms.openlocfilehash: 7baa2adbd615a449c73e70e1b96524fc1e18b25d
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2019
ms.locfileid: "71000168"
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
> Bu senaryo, Istio sürümüne `1.1.3`karşı test edilmiştir.

Bu makalede açıklanan adımlarda bir aks kümesi (RBAC etkinleştirilmiş Kubernetes `1.11` ve üzeri) oluşturdunuz ve kümeyle bir `kubectl` bağlantı oluşturmuş olduğunuz varsayılır. Ayrıca, kümenizde yüklü olan Ida gerekir.

Bu öğelerin herhangi biriyle ilgili yardıma ihtiyacınız varsa, aks [hızlı][aks-quickstart] başlangıcı ' na bakın ve [aks kılavuzuna Istio 'yu yükleyebilirsiniz][istio-install] .

## <a name="about-this-application-scenario"></a>Bu uygulama senaryosu hakkında

Örnek AKS oylama uygulaması, kullanıcılara iki oylama seçeneği (**kediler** veya **köpekler**) sağlar. Her seçenek için oy sayısını sürekli olarak sürdüren bir depolama bileşeni vardır. Ayrıca, her bir seçenek için oy saçılması etrafında ayrıntılar sağlayan bir analiz bileşeni de vardır.

Bu uygulama senaryosunda, `1.0` bir analiz bileşeninin oylama uygulaması ve sürümü `1.0` dağıtarak başlatılır. Analytics bileşeni, Oy sayısı için basit sayımlar sağlar. Oylama uygulaması ve analiz bileşeni, reddir tarafından `1.0` desteklenen depolama bileşeni sürümü ile etkileşim kurar.

Analiz bileşenini, sayımlar ve artık `1.1`toplam ve yüzdeleri sağlayan sürüme yükseltirsiniz.

Bir kanarya sürümü aracılığıyla uygulamanın `2.0` test sürümünün bir alt kümesi. Bu yeni sürüm, bir MySQL veritabanı tarafından desteklenen bir depolama bileşeni kullanır.

Sürüm `2.0` , Kullanıcı alt kümesinde beklendiği gibi çalıştığından emin olduktan sonra tüm kullanıcılarınıza sürümü `2.0` kullanıma sunuyoruz.

## <a name="deploy-the-application"></a>Uygulamayı dağıtma

Uygulamayı Azure Kubernetes Service (AKS) kümenize dağıtarak başlayalım. Aşağıdaki diyagramda, bu bölümün sonuna kadar çalışan, tüm bileşenlerin sürümü `1.0` , istio giriş ağ geçidi aracılığıyla hizmet verilen gelen isteklerle birlikte verilmiştir:

![AKS oylama uygulama bileşenleri ve yönlendirme.](media/istio/components-and-routing-01.png)

Bu makaleyle birlikte izlemeniz gereken yapıtlar [Azure-Samples/aks-oylama-App][github-azure-sample] GitHub deposunda bulunabilir. Yapıtları indirebilir veya depoyu şu şekilde klonlayabilirsiniz:

```console
git clone https://github.com/Azure-Samples/aks-voting-app.git
```

İndirilen/kopyalanan depolarda aşağıdaki klasöre geçin ve sonraki tüm adımları bu klasörden çalıştırın:

```console
cd scenarios/intelligent-routing-with-istio
```

İlk olarak, aşağıdaki gibi adlı `voting` örnek aks oylama uygulaması için aks kümenizde bir ad alanı oluşturun:

```azurecli
kubectl create namespace voting
```

Ad alanını ile `istio-injection=enabled`etiketleyin. Bu etiket, bu ad alanındaki tüm yığınlarınızın tüm yığınlarınıza otomatik olarak istik-proxy 'leri eklemesini sağlar.

```azurecli
kubectl label namespace voting istio-injection=enabled
```

Şimdi AKS oylama uygulaması için bileşenleri oluşturalım. Önceki bir adımda oluşturulan `voting` ad alanında bu bileşenleri oluşturun.

```azurecli
kubectl apply -f kubernetes/step-1-create-voting-app.yaml --namespace voting
```

Aşağıdaki örnek çıktıda oluşturulan kaynaklar gösterilmektedir:

```console
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

```azurecli
kubectl get pods -n voting
```

Aşağıdaki örnek çıktıda, `voting-app` Pod 'un üç örneği ve `voting-analytics` hem hem de `voting-storage` sayısının tek bir örneği gösterilmektedir. Her birinin iki kapsayıcısı vardır. Bu kapsayıcılardan biri bileşen olur ve diğeri `istio-proxy`:

```console
NAME                                    READY     STATUS    RESTARTS   AGE
voting-analytics-1-0-57c7fccb44-ng7dl   2/2       Running   0          39s
voting-app-1-0-956756fd-d5w7z           2/2       Running   0          39s
voting-app-1-0-956756fd-f6h69           2/2       Running   0          39s
voting-app-1-0-956756fd-wsxvt           2/2       Running   0          39s
voting-storage-1-0-5d8fcc89c4-2jhms     2/2       Running   0          39s
```

Pod hakkındaki bilgileri görmek için [kubectl][kubectl-describe]'i kullanın Pod 'ı kullanın. Pod adını, önceki çıkışın kendi AKS kümenizdeki Pod adı ile değiştirin:

```azurecli
kubectl describe pod voting-app-1-0-956756fd-d5w7z --namespace voting
```

`istio-proxy` Kapsayıcı, aşağıdaki örnek çıktıda gösterildiği gibi, bileşenlerinizi ve bileşenlerinden gelen ağ trafiğini yönetmek için otomatik olarak bir Ida eklenmiş.

```
[...]
Containers:
  voting-app:
    Image:         mcr.microsoft.com/aks/samples/voting/app:1.0
    ...
  istio-proxy:
    Image:         docker.io/istio/proxyv2:1.1.3
[...]
```

Bu [ağ geçidini][istio-reference-gateway] ve [sanal hizmeti][istio-reference-virtualservice]oluşturana kadar oylama uygulamasına bağlanamazsınız. Bu Istio kaynakları, trafiği varsayılan Istio ağ geçidinden uygulamamıza yönlendirir.

> [!NOTE]
> **Ağ geçidi** , hizmet ağı tarafında gelen veya giden http ve TCP trafiğini alan bir bileşendir.
> 
> Bir **sanal hizmet** , bir veya daha fazla hedef hizmet için bir yönlendirme kuralları kümesi tanımlar.

Ağ geçidini ve sanal hizmet YAML 'yi dağıtmak için komutunukullanın.`kubectl apply` Bu kaynakların dağıtıldığı ad alanını belirtmeyi unutmayın.

```azurecli
kubectl apply -f istio/step-1-create-voting-app-gateway.yaml --namespace voting
```

Aşağıdaki örnek çıktı yeni ağ geçidini ve oluşturulmakta olan sanal hizmeti gösterir:

```console
virtualservice.networking.istio.io/voting-app created
gateway.networking.istio.io/voting-app-gateway created
```

Aşağıdaki komutu kullanarak, Istio giriş ağ geçidinin IP adresini alın:

```azurecli
kubectl get service istio-ingressgateway --namespace istio-system -o jsonpath='{.status.loadBalancer.ingress[0].ip}'
```

Aşağıdaki örnek çıktıda, giriş ağ geçidinin IP adresi gösterilmektedir:

```
20.188.211.19
```

Bir tarayıcı açın ve IP adresini yapıştırın. Örnek AKS oylama uygulaması görüntülenir.

![IKS oylama uygulaması, ampo 'un etkin olduğu AKS kümesinde çalışıyor.](media/istio/deploy-app-01.png)

Ekranın altındaki `1.0` bilgiler, uygulamanın sürümü `voting-app` ve `1.0` `voting-storage` sürümü (redsıs) kullandığını gösterir.

## <a name="update-the-application"></a>Uygulamayı güncelleştirme

Analiz bileşeninin yeni bir sürümünü dağıtalım. Bu yeni sürüm `1.1` , her kategori için sayıma ek olarak toplamları ve yüzdeleri görüntüler.

Aşağıdaki diyagramda, bu bölümün sonunda ne çalıştığını gösterilmektedir-bileşenimizin `1.1` `voting-analytics` yalnızca bir sürümünde `voting-app` bileşenden yönlendirilen trafik vardır. Bileşenimizin `1.0` `voting-analytics` sürümü çalışmaya devam ediyor `voting-analytics` ve hizmet tarafından başvurulduğu halde, istio proxy 'leri, giden ve giden trafiğe izin vermez.

![AKS oylama uygulama bileşenleri ve yönlendirme.](media/istio/components-and-routing-02.png)

Bileşenin`voting-analytics` sürümünü `1.1` dağıtalım. Bu bileşeni `voting` ad alanında oluştur:

```console
kubectl apply -f kubernetes/step-2-update-voting-analytics-to-1.1.yaml --namespace voting
```

Aşağıdaki örnek çıktıda oluşturulan kaynaklar gösterilmektedir:

```console
deployment.apps/voting-analytics-1-1 created
```

Örnek AKS oylama uygulamasını bir tarayıcıda yeniden açarak, önceki adımda elde edilen Istio giriş ağ geçidinin IP adresini kullanın.

Tarayıcınız aşağıda gösterilen iki görünüm arasında alternatifler vardır. Yalnızca tek etiketli seçici (`app: voting-analytics`) içeren `voting-analytics` bileşen için bir Kubernetes [hizmeti][kubernetes-service] kullandığınızdan, Kubernetes, bu seçiciyle eşleşen Pod 'ler arasında hepsini bir kez deneme için varsayılan davranışı kullanır. Bu durumda, hem sürüm `1.0` `1.1` hem de yığınlarınızın `voting-analytics` her ikisi de olur.

![AKS oylama uygulamamız üzerinde çalışan analiz bileşeninin sürüm 1,0.](media/istio/deploy-app-01.png)

![AKS oylama uygulamamız üzerinde çalışan analiz bileşeninin sürüm 1,1.](media/istio/update-app-01.png)

`voting-analytics` Bileşenin iki sürümü arasındaki geçişi aşağıdaki şekilde görselleştirebilirsiniz. Kendi Istik giriş ağ geçidinizin IP adresini kullanmayı unutmayın.

Bash 

```bash
INGRESS_IP=20.188.211.19
for i in {1..5}; do curl -si $INGRESS_IP | grep results; done
```

Powershell

```powershell
$INGRESS_IP="20.188.211.19"
(1..5) |% { (Invoke-WebRequest -Uri $INGRESS_IP).Content.Split("`n") | Select-String -Pattern "results" }
```

Aşağıdaki örnek çıktıda, sürümler arasında site anahtarları olarak döndürülen Web sitesinin ilgili bölümü gösterilmektedir:

```
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
```

### <a name="lock-down-traffic-to-version-11-of-the-application"></a>Trafiği uygulamanın 1,1 sürümüne kilitle

`1.1` Şimdi, `voting-analytics` trafiği yalnızca bileşenin sürümüne `1.0` `voting-storage` ve bileşen sürümüne kilitleyelim. Daha sonra diğer tüm bileşenler için yönlendirme kuralları tanımlarsınız.

> * Bir **sanal hizmet** , bir veya daha fazla hedef hizmet için bir yönlendirme kuralları kümesi tanımlar.
> * Bir **hedef kural** , trafik ilkelerini ve sürüme özgü ilkeleri tanımlar.
> * Bir **ilke** , iş yükleri üzerinde hangi kimlik doğrulama yöntemlerinin kabul edilebilir olduğunu tanımlar.

İçindeki sanal `voting-app` hizmet tanımını değiştirmek ve diğer bileşenlere yönelik [hedef kuralları][istio-reference-destinationrule] ve [sanal Hizmetleri][istio-reference-virtualservice] eklemek için komutunukullanın.`kubectl apply` Karşılıklı TLS ve istemci [][istio-reference-policy] sertifikaları kullanılarak Hizmetler `voting` arasındaki tüm iletişim güvenliğini sağlamak için ad alanına bir ilke ekleyeceksiniz.

* İlke `peers.mtls.mode` , `voting` ad alanındaki hizmetleriniz `STRICT` arasında karşılıklı TLS 'nin uygulanmasını sağlamak için olarak ayarlanmıştır.
* Ayrıca, `trafficPolicy.tls.mode` tüm hedef kurallarımızda olarak ' i olarak `ISTIO_MUTUAL` ayarlandık. İstio, güçlü kimlikler içeren hizmetler sağlar ve karşılıklı olarak yönettiği karşılıklı TLS ve istemci sertifikalarını kullanarak hizmetler arasındaki iletişimin güvenliğini sağlar.

```azurecli
kubectl apply -f istio/step-2-update-and-add-routing-for-all-components.yaml --namespace voting
```

Aşağıdaki örnek çıktıda yeni Ilke, hedef kuralları ve güncelleştirilmekte/oluşturulan sanal hizmetler gösterilmektedir:

```console
virtualservice.networking.istio.io/voting-app configured
policy.authentication.istio.io/default created
destinationrule.networking.istio.io/voting-app created
destinationrule.networking.istio.io/voting-analytics created
virtualservice.networking.istio.io/voting-analytics created
destinationrule.networking.istio.io/voting-storage created
virtualservice.networking.istio.io/voting-storage created
```

Aks oylama uygulamasını bir tarayıcıda yeniden açarsanız, bileşen tarafından `1.1` `voting-app` yalnızca `voting-analytics` bileşenin yeni sürümü kullanılır.

![AKS oylama uygulamamız üzerinde çalışan analiz bileşeninin sürüm 1,1.](media/istio/update-app-01.png)

Artık yalnızca bileşeninizin `1.1` `voting-analytics` sürümüne aşağıda gösterildiği gibi yönlendirildiğini görselleştirebilirsiniz. Kendi Istio giriş ağ geçidinizin IP adresini kullanmayı unutmayın:

Bash 

```bash
INGRESS_IP=20.188.211.19
for i in {1..5}; do curl -si $INGRESS_IP | grep results; done
```

Powershell

```powershell
$INGRESS_IP="20.188.211.19"
(1..5) |% { (Invoke-WebRequest -Uri $INGRESS_IP).Content.Split("`n") | Select-String -Pattern "results" }
```

Aşağıdaki örnek çıktı, döndürülen Web sitesinin ilgili bölümünü gösterir:

```
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

Bash

```bash
# mTLS configuration between each of the istio ingress pods and the voting-app service
kubectl get pod -n istio-system -l app=istio-ingressgateway | grep Running | cut -d ' ' -f1 | xargs -n1 -I{} istioctl authn tls-check {}.istio-system voting-app.voting.svc.cluster.local

# mTLS configuration between each of the voting-app pods and the voting-analytics service
kubectl get pod -n voting -l app=voting-app | grep Running | cut -d ' ' -f1 | xargs -n1 -I{} istioctl authn tls-check {}.voting voting-analytics.voting.svc.cluster.local

# mTLS configuration between each of the voting-app pods and the voting-storage service
kubectl get pod -n voting -l app=voting-app | grep Running | cut -d ' ' -f1 | xargs -n1 -I{} istioctl authn tls-check {}.voting voting-storage.voting.svc.cluster.local

# mTLS configuration between each of the voting-analytics version 1.1 pods and the voting-storage service
kubectl get pod -n voting -l app=voting-analytics,version=1.1 | grep Running | cut -d ' ' -f1 | xargs -n1 -I{} istioctl authn tls-check {}.voting voting-storage.voting.svc.cluster.local
```

Powershell

```powershell
# mTLS configuration between each of the istio ingress pods and the voting-app service
(kubectl get pod -n istio-system -l app=istio-ingressgateway | Select-String -Pattern "Running").Line |% { $_.Split()[0] |% { istioctl authn tls-check $($_ + ".istio-system") voting-app.voting.svc.cluster.local } }

# mTLS configuration between each of the voting-app pods and the voting-analytics service
(kubectl get pod -n voting -l app=voting-app | Select-String -Pattern "Running").Line |% { $_.Split()[0] |% { istioctl authn tls-check $($_ + ".voting") voting-analytics.voting.svc.cluster.local } }

# mTLS configuration between each of the voting-app pods and the voting-storage service
(kubectl get pod -n voting -l app=voting-app | Select-String -Pattern "Running").Line |% { $_.Split()[0] |% { istioctl authn tls-check $($_ + ".voting") voting-storage.voting.svc.cluster.local } }

# mTLS configuration between each of the voting-analytics version 1.1 pods and the voting-storage service
(kubectl get pod -n voting -l app=voting-analytics,version=1.1 | Select-String -Pattern "Running").Line |% { $_.Split()[0] |% { istioctl authn tls-check $($_ + ".voting") voting-storage.voting.svc.cluster.local } }
```

Aşağıdaki örnek çıktıda, yukarıdaki sorgularımızın her biri için karşılıklı TLS 'nin zorunlu olduğu gösterilmektedir. Çıktı Ayrıca, karşılıklı TLS 'yi zorlayan Ilke ve hedef kurallarını da gösterir:

```console
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

`2.0` Şimdi `voting-app`, ,ve`voting-storage` bileşenlerinin yeni bir sürümünü dağıtalım. `voting-analytics` Yeni `voting-storage` bileşen redin yerine MySQL kullanır `voting-app` ve ve `voting-analytics` bileşenleri, bu yeni `voting-storage` bileşeni kullanmasına izin verecek şekilde güncelleştirilir.

`voting-app` Bileşen artık Özellik bayrağı işlevini desteklemektedir. Bu özellik bayrağı, bir kullanıcı alt kümesi için istio 'nun kanarya yayın yeteneğini test etmenize olanak tanır.

Aşağıdaki diyagramda, bu bölümün sonunda ne çalıştırdıklarınız gösterilmektedir.

* `1.0` Bileşeninsürümü`1.1` , Bileşensürümü`1.0` ve Bileşensürümübirbirleriyleiletişimkurabilir`voting-storage`. `voting-app` `voting-analytics`
* `2.0` Bileşeninsürümü`2.0` , Bileşensürümü`2.0` ve Bileşensürümübirbirleriyleiletişimkurabilir`voting-storage`. `voting-app` `voting-analytics`
* Bileşenin sürümüne `2.0` yalnızca belirli bir özellik bayrağı ayarlanmış olan kullanıcılar erişebilir. `voting-app` Bu değişiklik, tanımlama bilgisi aracılığıyla bir özellik bayrağı kullanılarak yönetilir.

![AKS oylama uygulama bileşenleri ve yönlendirme.](media/istio/components-and-routing-03.png)

İlk olarak, bu yeni bileşenler için istio hedef kurallarını ve sanal hizmetlerini karşılamak olarak güncelleştirin. Bu güncelleştirmeler, trafiği yeni bileşenlere doğru bir şekilde yönlendirmemenizi ve kullanıcıların beklenmeyen bir erişim almaz olmasını sağlar:

```azurecli
kubectl apply -f istio/step-3-add-routing-for-2.0-components.yaml --namespace voting
```

Aşağıdaki örnek çıktıda, güncelleştirilmekte olan hedef kurallar ve sanal hizmetler gösterilmektedir:

```console
destinationrule.networking.istio.io/voting-app configured
virtualservice.networking.istio.io/voting-app configured
destinationrule.networking.istio.io/voting-analytics configured
virtualservice.networking.istio.io/voting-analytics configured
destinationrule.networking.istio.io/voting-storage configured
virtualservice.networking.istio.io/voting-storage configured
```

Ardından, yeni sürüm `2.0` bileşenleri için Kubernetes nesnelerini ekleyelim. Ayrıca, `voting-storage` hizmeti MySQL için `3306` bağlantı noktasını içerecek şekilde de güncelleştirebilirsiniz:

```azurecli
kubectl apply -f kubernetes/step-3-update-voting-app-with-new-storage.yaml --namespace voting
```

Aşağıdaki örnek çıktıda, Kubernetes nesnelerinin başarıyla güncelleştirildiği veya oluşturulduğu gösterilmektedir:

```console
service/voting-storage configured
secret/voting-storage-secret created
deployment.apps/voting-storage-2-0 created
persistentvolumeclaim/mysql-pv-claim created
deployment.apps/voting-analytics-2-0 created
deployment.apps/voting-app-2-0 created
```

Tüm sürüm `2.0` boşlarını çalıştırmaya kadar bekleyin. `voting` Ad alanındaki tüm Pod 'leri görüntülemek için [kubectl Get Pod][kubectl-get] komutunu kullanın:

```azurecli
kubectl get pods --namespace voting
```

Artık oylama uygulamasının sürümü `1.0` ve sürümü `2.0` (Canary) arasında geçiş yapabiliyor olmanız gerekir. Ekranın alt kısmındaki Özellik bayrağı değiştirme bir tanımlama bilgisi ayarlar. Bu tanımlama bilgisi, `voting-app` sanal hizmet tarafından kullanıcıları yeni sürüme `2.0`yönlendirmek için kullanılır.

![AKS oylama uygulaması-Özellik bayrağının 1,0 sürümü ayarlanmamış.](media/istio/canary-release-01.png)

![AKS oylama uygulaması-Özellik bayrağının 2,0 sürümü ayarlanır.](media/istio/canary-release-02.png)

Oy sayıları, uygulamanın sürümleri arasında farklılık görüntüler. Bu fark, iki farklı depolama arka ucu kullandığınızı vurgular.

## <a name="finalize-the-rollout"></a>Dağıtımı sonlandırma

Kanarya sürümünü başarıyla test edildikten sonra, `voting-app` sanal hizmeti tüm trafiği `voting-app` bileşen sürümüne `2.0` yönlendirecek şekilde güncelleştirin. Tüm kullanıcılar, özellik bayrağının `2.0` ayarlanmış olup olmamasına bakılmaksızın uygulamanın sürümünü görür:

![AKS oylama uygulama bileşenleri ve yönlendirme.](media/istio/components-and-routing-04.png)

Artık etkin olmasını istemediğiniz bileşenlerin sürümlerini kaldırmak için tüm hedef kurallarını güncelleştirin. Ardından, bu sürümlere başvurmayı durdurmak için tüm sanal Hizmetleri güncelleştirin.

Artık bileşenlerin eski sürümlerinden hiçbirine hiç trafik olmadığından, bu bileşenlere yönelik tüm dağıtımları güvenle silebilirsiniz.

![AKS oylama uygulama bileşenleri ve yönlendirme.](media/istio/components-and-routing-05.png)

Artık AKS oylama uygulamasının yeni bir sürümünü başarıyla tamamladınız.

## <a name="clean-up"></a>Temizleme 

Bu senaryoda kullandığımız aks oylama uygulamasını, `voting` ad alanını aşağıdaki şekilde silerek aks kümenizdeki bir şekilde kaldırabilirsiniz:

```azurecli
kubectl delete namespace voting
```

Aşağıdaki örnek çıktı, AKS oylama uygulamasının tüm bileşenlerinin AKS kümenizdeki kaldırıldığını gösterir.

```console
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
[istio-install]: ./istio-install.md
