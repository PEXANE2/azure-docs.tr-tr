---
title: Azure Kubernetes Hizmeti'nde (AKS) ağ ilkeleriyle güvenli bölmeler
description: Azure Kubernetes Hizmeti'nde (AKS) Kubernetes ağ ilkelerini kullanarak bölmelere girip çıkan trafiği nasıl güvenli hale erdireceklerini öğrenin
services: container-service
ms.topic: article
ms.date: 05/06/2019
ms.openlocfilehash: 37b6ebd1c8b147db0a9cead4678a0b2bb4ed234d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79473617"
---
# <a name="secure-traffic-between-pods-using-network-policies-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti'nde (AKS) ağ ilkelerini kullanarak bölmeler arasındaki trafiği güvenli hale

Kubernetes'te modern, mikro hizmet tabanlı uygulamalar çalıştırdığınızda, genellikle hangi bileşenlerin birbiriyle iletişim kurabileceğini denetlemek istersiniz. En az ayrıcalık ilkesi, bir Azure Kubernetes Hizmeti (AKS) kümesindeki bölmeler arasında trafiğin nasıl aksabildiği konusunda uygulanmalıdır. Trafiği doğrudan arka uç uygulamalara engellemek istediğinizi varsayalım. Kubernetes'teki *Ağ İlkesi* özelliği, kümedeki bölmeler arasındaki giriş ve çıkış trafiği için kuralları tanımlamanıza olanak tanır.

Bu makalede, aks bölmeleri arasındaki trafik akışını denetlemek için ağ ilkesi altyapısı yüklemek ve Kubernetes ağ ilkeleri oluşturmak nasıl gösterir. Ağ ilkesi yalnızca AKS'deki Linux tabanlı düğümler ve bölmeler için kullanılmalıdır.

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLI sürümü 2.0.61 veya daha sonra yüklenmiş ve yapılandırılan gerekir. Sürümü `az --version` bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye][install-azure-cli]bakın.

> [!TIP]
> Önizleme sırasında ağ ilkesi özelliğini kullandıysanız, [yeni bir küme oluşturmanızı](#create-an-aks-cluster-and-enable-network-policy)öneririz.
> 
> Önizleme sırasında ağ ilkesini kullanan varolan test kümelerini kullanmaya devam etmek istiyorsanız, kümenizi en son GA sürümü için yeni bir Kubernetes sürümlerine yükseltin ve çöken ölçüm sunucusu ve Kubernetes'i düzeltmek için aşağıdaki YAML bildirimini dağıtın Pano. Bu düzeltme yalnızca Calico ağ ilkesi altyapısını kullanan kümeler için gereklidir.
>
> Güvenlik en iyi uygulama olarak, AKS kümesine nelerin dağıtılmış olduğunu anlamak için [bu YAML bildiriminin içeriğini gözden geçirin.][calico-aks-cleanup]
>
> `kubectl delete -f https://raw.githubusercontent.com/Azure/aks-engine/master/docs/topics/calico-3.3.1-cleanup-after-upgrade.yaml`

## <a name="overview-of-network-policy"></a>Ağ ilkesine genel bakış

AKS kümesindeki tüm bölmeler varsayılan olarak herhangi bir sınırlama olmaksızın trafik gönderebilir ve alabilir. Güvenliği artırmak için, trafik akışını kontrol eden kurallar tanımlayabilirsiniz. Arka uç uygulamaları genellikle yalnızca gerekli ön uç hizmetlerine maruz kalır, örneğin. Veya, veritabanı bileşenlerine yalnızca onlara bağlanan uygulama katmanları erişebilir.

Ağ İlkesi, Podlar arasındaki iletişim için erişim ilkelerini tanımlayan bir Kubernetes belirtimidir. Ağ İlkeleri'ni kullanarak, trafiği göndermek ve almak için sıralı kurallar kümesini tanımlar ve bunları bir veya daha fazla etiket seçiciyle eşleşen bir bölme koleksiyonuna uygularsınız.

Bu ağ ilkesi kuralları YAML bildirimleri olarak tanımlanır. Ağ ilkeleri, dağıtım veya hizmet de oluşturan daha geniş bir bildirimin parçası olarak eklenebilir.

### <a name="network-policy-options-in-aks"></a>AKS'de ağ ilkesi seçenekleri

Azure, ağ ilkesini uygulamanın iki yolunu sağlar. Bir AKS kümesi oluştururken bir ağ ilkesi seçeneği seçersiniz. İlke seçeneği küme oluşturulduktan sonra değiştirilemez:

* *Azure'un Azure Ağ İlkeleri*olarak adlandırılan kendi uygulaması.
* *Calico Ağ Politikaları*, [Tigera][tigera]tarafından kurulan bir açık kaynak ağı ve ağ güvenliği çözümü.

Her iki uygulama da belirtilen ilkeleri uygulamak için Linux *IPTable'ları* kullanır. İlkeler, izin verilen ve izin verilmeyen IP çiftleri kümelerine çevrilir. Bu çiftler daha sonra IPTable filtre kuralları olarak programlanır.

### <a name="differences-between-azure-and-calico-policies-and-their-capabilities"></a>Azure ve Calico ilkeleri ile yetenekleri arasındaki farklar

| Özellik                               | Azure                      | Calico                      |
|------------------------------------------|----------------------------|-----------------------------|
| Desteklenen platformlar                      | Linux                      | Linux                       |
| Desteklenen ağ seçenekleri             | Azure CNI                  | Azure CNI ve kubenet       |
| Kubernetes belirtimine uygunluk | Desteklenen tüm ilke türleri |  Desteklenen tüm ilke türleri |
| Ek özellikler                      | None                       | Global Ağ İlkesi, Global Ağ Kümesi ve Ana Bilgisayar Uçnokta'ndan oluşan genişletilmiş ilke modeli. Bu genişletilmiş özellikleri `calicoctl` yönetmek için CLI'yi kullanma hakkında daha fazla bilgi için [kalikoktl kullanıcı başvurusuna][calicoctl]bakın. |
| Destek                                  | Azure desteği ve Mühendislik ekibi tarafından desteklenir | Calico toplum desteği. Ek ücretli destek hakkında daha fazla bilgi için [Project Calico destek seçeneklerine][calico-support]bakın. |
| Günlüğe Kaydetme                                  | IPTable'larda eklenen / silinen kurallar */var/log/azure-npm.log* altında her ana bilgisayarda günlüğe kaydedilir | Daha fazla bilgi için [Calico bileşen günlüklerine][calico-logs] bakın |

## <a name="create-an-aks-cluster-and-enable-network-policy"></a>Bir AKS kümesi oluşturun ve ağ ilkesini etkinleştirin

Ağ ilkelerini iş başında görmek için, trafik akışını tanımlayan bir ilke oluşturup genişletelim:

* Pod tüm trafik inkar.
* Pod etiketlerine göre trafiğe izin verin.
* Ad alanına göre trafiğe izin verin.

İlk olarak, ağ ilkesini destekleyen bir AKS kümesi oluşturalım. 

> [!IMPORTANT]
>
> Ağ ilkesi özelliği yalnızca küme oluşturulduğunda etkinleştirilebilir. Varolan bir AKS kümesinde ağ ilkesini etkinleştiremezsiniz.

Azure Ağ İlkesi'ni kullanmak için [Azure CNI eklentisini][azure-cni] kullanmanız ve kendi sanal ağınızı ve alt ağlarınızı tanımlamanız gerekir. Gerekli alt ağ aralıklarının nasıl planlanagerektiği hakkında daha ayrıntılı bilgi için gelişmiş [ağ bağlantısını yapılandırmaya][use-advanced-networking]bakın. Calico Ağ İlkesi, aynı Azure CNI eklentisi veya Kubenet CNI eklentisi ile kullanılabilir.

Aşağıdaki örnek komut dosyası:

* Bir sanal ağ ve alt ağ oluşturur.
* AKS kümesinde kullanılmak üzere bir Azure Etkin Dizin (Azure AD) hizmet ilkesi oluşturur.
* Sanal ağdaki AKS küme hizmeti ilkesi için *Katılımcı* İzinleri atar.
* Tanımlanan sanal ağda bir AKS kümesi oluşturur ve ağ ilkesini sağlar.
    * *Azure* ağ ilkesi seçeneği kullanılır. Bunun yerine Calico'yu ağ ilkesi `--network-policy calico` seçeneği olarak kullanmak için parametreyi kullanın. Not: Calico ya da `--network-plugin azure` `--network-plugin kubenet`.

Kendi güvenli *SP_PASSWORD*sağlayın. *RESOURCE_GROUP_NAME* ve *CLUSTER_NAME* değişkenleri değiştirebilirsiniz:

```azurecli-interactive
RESOURCE_GROUP_NAME=myResourceGroup-NP
CLUSTER_NAME=myAKSCluster
LOCATION=canadaeast

# Create a resource group
az group create --name $RESOURCE_GROUP_NAME --location $LOCATION

# Create a virtual network and subnet
az network vnet create \
    --resource-group $RESOURCE_GROUP_NAME \
    --name myVnet \
    --address-prefixes 10.0.0.0/8 \
    --subnet-name myAKSSubnet \
    --subnet-prefix 10.240.0.0/16

# Create a service principal and read in the application ID
SP=$(az ad sp create-for-rbac --output json)
SP_ID=$(echo $SP | jq -r .appId)
SP_PASSWORD=$(echo $SP | jq -r .password)

# Wait 15 seconds to make sure that service principal has propagated
echo "Waiting for service principal to propagate..."
sleep 15

# Get the virtual network resource ID
VNET_ID=$(az network vnet show --resource-group $RESOURCE_GROUP_NAME --name myVnet --query id -o tsv)

# Assign the service principal Contributor permissions to the virtual network resource
az role assignment create --assignee $SP_ID --scope $VNET_ID --role Contributor

# Get the virtual network subnet resource ID
SUBNET_ID=$(az network vnet subnet show --resource-group $RESOURCE_GROUP_NAME --vnet-name myVnet --name myAKSSubnet --query id -o tsv)

# Create the AKS cluster and specify the virtual network and service principal information
# Enable network policy by using the `--network-policy` parameter
az aks create \
    --resource-group $RESOURCE_GROUP_NAME \
    --name $CLUSTER_NAME \
    --node-count 1 \
    --generate-ssh-keys \
    --network-plugin azure \
    --service-cidr 10.0.0.0/16 \
    --dns-service-ip 10.0.0.10 \
    --docker-bridge-address 172.17.0.1/16 \
    --vnet-subnet-id $SUBNET_ID \
    --service-principal $SP_ID \
    --client-secret $SP_PASSWORD \
    --network-policy azure
```

Kümenin oluşturulması birkaç dakika sürer. Küme hazır olduğunda, az `kubectl` [aks get-credentials][az-aks-get-credentials] komutunu kullanarak Kubernetes kümenize bağlanmak için yapılandırın. Bu komut kimlik bilgilerini karşıdan yükler ve Kubernetes CLI'yi bunları kullanacak şekilde yapılandırır:

```azurecli-interactive
az aks get-credentials --resource-group $RESOURCE_GROUP_NAME --name $CLUSTER_NAME
```

## <a name="deny-all-inbound-traffic-to-a-pod"></a>Bir bölmeye gelen tüm trafiği reddetme

Belirli ağ trafiğine izin vermek için kurallar tanımlamadan önce, önce tüm trafiği reddetmek için bir ağ ilkesi oluşturun. Bu ilke, yalnızca istenen trafiği beyaz listeye almaya başlamak için bir başlangıç noktası sağlar. Ağ ilkesi uygulandığında trafiğin bırakıldığını da açıkça görebilirsiniz.

Örnek uygulama ortamı ve trafik kuralları için, önce örnek bölmeleri çalıştırmak için *geliştirme* adlı bir ad alanı oluşturalım:

```console
kubectl create namespace development
kubectl label namespace/development purpose=development
```

NGINX çalıştıran bir örnek arka uç pod oluşturun. Bu arka uç bölmesi, örnek bir arka uç web tabanlı uygulamayı simüle etmek için kullanılabilir. *Geliştirme* ad alanında bu bölmeyi oluşturun ve web trafiğine hizmet etmek için bağlantı noktası *80'i* açın. Pod'u *app=webapp,role=backend* ile etiketleyin, böylece bir sonraki bölümde bir ağ ilkesiyle hedefleyebiliriz:

```console
kubectl run backend --image=nginx --labels app=webapp,role=backend --namespace development --expose --port 80 --generator=run-pod/v1
```

Varsayılan NGINX web sayfasına başarıyla erişebileceğinizi test etmek için başka bir bölme oluşturun ve bir terminal oturumu ekleyin:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Kabuk isteminde, `wget` varsayılan NGINX web sayfasına erişebileceğinizi onaylamak için kullanın:

```console
wget -qO- http://backend
```

Aşağıdaki örnek çıktı, varsayılan NGINX web sayfasının döndürüldettiğini gösterir:

```output
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Ekli terminal oturumundan çıkın. Test bölmesi otomatik olarak silinir.

```console
exit
```

### <a name="create-and-apply-a-network-policy"></a>Ağ ilkesi oluşturma ve uygulama

Artık örnek arka uç bölmesinde temel NGINX web sayfasını kullanabileceğinizi doğruladığınıziçin, tüm trafiği reddetmek için bir ağ ilkesi oluşturun. Adlandırılmış `backend-policy.yaml` bir dosya oluşturun ve aşağıdaki YAML bildirimini yapıştırın. Bu bildirim, ilkeyi *uygulama:webapp,role:backend* etiketine sahip bölmelere eklemek için bir *podSelector* kullanır, örneğin NGINX bölmeniz gibi. *Giriş*altında hiçbir kural tanımlanmaz, bu nedenle bölmeye gelen tüm trafik reddedilir:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: backend-policy
  namespace: development
spec:
  podSelector:
    matchLabels:
      app: webapp
      role: backend
  ingress: []
```

Azure [https://shell.azure.com](https://shell.azure.com) Bulut Shell'i tarayıcınızda açmak için gidin.

[Kubectl uygula][kubectl-apply] komutunu kullanarak ağ ilkesini uygulayın ve YAML manifestonuzun adını belirtin:

```console
kubectl apply -f backend-policy.yaml
```

### <a name="test-the-network-policy"></a>Ağ ilkesini test edin

Bakalım arka uç bölmesindeki NGINX web sayfasını tekrar kullanabilecek misiniz? Başka bir test bölmesi oluşturun ve bir terminal oturumu ekleyin:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Kabuk isteminde, `wget` varsayılan NGINX web sayfasına erişip erişemeyebildiğinizi görmek için kullanın. Bu kez, bir zaman kaybı değerini *2* saniyeye ayarlayın. Ağ ilkesi artık tüm gelen trafiği engeller, bu nedenle aşağıdaki örnekte gösterildiği gibi sayfa yüklenmez:

```console
wget -qO- --timeout=2 http://backend
```

```output
wget: download timed out
```

Ekli terminal oturumundan çıkın. Test bölmesi otomatik olarak silinir.

```console
exit
```

## <a name="allow-inbound-traffic-based-on-a-pod-label"></a>Pod etiketine göre gelen trafiğe izin verme

Önceki bölümde, bir arka uç NGINX pod zamanlandı ve tüm trafiği reddetmek için bir ağ ilkesi oluşturuldu. Bir ön uç bölmesi oluşturalım ve ön uç bölmelerinden gelen trafiğe izin verecek şekilde ağ ilkesini güncelleştirelim.

Etiketler *uygulaması:webapp,role:frontend* ve herhangi bir ad alanı ile bölmelerden gelen trafiğe izin vermek için ağ ilkesini güncelleştirin. Önceki arka *uç-policy.yaml* dosyasını düzenlediniz ve bildiriminizin aşağıdaki örnek gibi görünmesi için *matchLabels* giriş kurallarını ekleyin:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: backend-policy
  namespace: development
spec:
  podSelector:
    matchLabels:
      app: webapp
      role: backend
  ingress:
  - from:
    - namespaceSelector: {}
      podSelector:
        matchLabels:
          app: webapp
          role: frontend
```

> [!NOTE]
> Bu ağ ilkesi, giriş kuralı için bir *namespaceSelector* ve *podSelector* öğesi kullanır. YAML sözdizimi, giriş kurallarının katkı maddesi olması için önemlidir. Bu örnekte, her iki öğenin de uygulanacak giriş kuralı ile eşleşmesi gerekir. *1.12'den* önceki Kubernetes sürümleri bu öğeleri doğru yorumlamayabilir ve ağ trafiğini beklediğiniz gibi kısıtlayabilir. Bu davranış hakkında daha fazla bilgi için, seçicilere ve [arasından Davranış bölümüne][policy-rules]bakın.

[Kubectl uygula][kubectl-apply] komutunu kullanarak güncelleştirilmiş ağ ilkesini uygulayın ve YAML manifestonuzun adını belirtin:

```console
kubectl apply -f backend-policy.yaml
```

*App=webapp,role=frontend* olarak etiketlenmiş bir bölme zamanlayın ve terminal oturumu ekleyin:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace development --generator=run-pod/v1
```

Kabuk isteminde, `wget` varsayılan NGINX web sayfasına erişip erişemeyebildiğinizi görmek için kullanın:

```console
wget -qO- http://backend
```

Giriş kuralı etiketleri uygulaması olan bölmeleri ile trafik sağlar *çünkü: webapp,rol: frontend*, ön uç bölmesinden trafik izin verilir. Aşağıdaki örnek çıktı, döndürülen varsayılan NGINX web sayfasını gösterir:

```output
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Ekli terminal oturumundan çıkın. Bölme otomatik olarak silinir.

```console
exit
```

### <a name="test-a-pod-without-a-matching-label"></a>Eşleşen bir etiket olmadan bir bölmeyi test edin

Ağ ilkesi pods etiketli uygulamadan trafik *sağlar: webapp,rol: frontend,* ancak diğer tüm trafik inkar etmelidir. Bu etiketlerolmadan başka bir bölmenin arka uç NGINX bölmesine erişip erişemeyeceğini test edelim. Başka bir test bölmesi oluşturun ve bir terminal oturumu ekleyin:

```console
kubectl run --rm -it --image=alpine network-policy --namespace development --generator=run-pod/v1
```

Kabuk isteminde, `wget` varsayılan NGINX web sayfasına erişip erişemeyebildiğinizi görmek için kullanın. Ağ ilkesi gelen trafiği engeller, bu nedenle sayfa aşağıdaki örnekte gösterildiği gibi yüklenemez:

```console
wget -qO- --timeout=2 http://backend
```

```output
wget: download timed out
```

Ekli terminal oturumundan çıkın. Test bölmesi otomatik olarak silinir.

```console
exit
```

## <a name="allow-traffic-only-from-within-a-defined-namespace"></a>Yalnızca tanımlı bir ad alanı içinden gelen trafiğe izin verme

Önceki örneklerde, tüm trafiği reddeden bir ağ ilkesi oluşturdunuz ve ardından belirli bir etikete sahip bölmelerden gelen trafiğe izin verecek şekilde ilkeyi güncelleştirmişsiniz. Başka bir ortak gereksinim, trafiği yalnızca belirli bir ad alanı içinde sınırlamaktır. Önceki örnekler *geliştirme* ad alanında trafik için ise, *üretim*gibi başka bir ad alanından gelen trafiğin bölmelere ulaşmasını engelleyen bir ağ ilkesi oluşturun.

İlk olarak, üretim ad alanını simüle etmek için yeni bir ad alanı oluşturun:

```console
kubectl create namespace production
kubectl label namespace/production purpose=production
```

*Üretim* ad alanında *app=webapp,role=frontend*olarak etiketlenmiş bir test bölmesi zamanlayın. Terminal oturumu ekle:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace production --generator=run-pod/v1
```

Kabuk isteminde, `wget` varsayılan NGINX web sayfasına erişebileceğinizi onaylamak için kullanın:

```console
wget -qO- http://backend.development
```

Pod etiketleri ağ ilkesinde şu anda izin verilenle eşleştirdiği için, trafiğe izin verilir. Ağ ilkesi ad alanlarına değil, yalnızca pod etiketlerini bakar. Aşağıdaki örnek çıktı, döndürülen varsayılan NGINX web sayfasını gösterir:

```output
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Ekli terminal oturumundan çıkın. Test bölmesi otomatik olarak silinir.

```console
exit
```

### <a name="update-the-network-policy"></a>Ağ ilkesini güncelleştirme

Giriş kuralı *namespaceSelector* bölümünü yalnızca *geliştirme* ad alanı nın içinden gelen trafiğe izin verecek şekilde güncelleştirelim. Aşağıdaki örnekte gösterildiği gibi *arka uç-policy.yaml* bildirim dosyasını edin:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: backend-policy
  namespace: development
spec:
  podSelector:
    matchLabels:
      app: webapp
      role: backend
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          purpose: development
      podSelector:
        matchLabels:
          app: webapp
          role: frontend
```

Daha karmaşık örneklerde, *bir namespaceSelector* ve daha sonra bir *podSelector*gibi birden çok giriş kuralları tanımlayabilirsiniz.

[Kubectl uygula][kubectl-apply] komutunu kullanarak güncelleştirilmiş ağ ilkesini uygulayın ve YAML manifestonuzun adını belirtin:

```console
kubectl apply -f backend-policy.yaml
```

### <a name="test-the-updated-network-policy"></a>Güncelleştirilmiş ağ ilkesini test edin

*Üretim* ad alanında başka bir bölme zamanlayın ve bir terminal oturumu ekleyin:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace production --generator=run-pod/v1
```

Kabuk isteminde, `wget` ağ ilkesinin artık trafiği reddettiğini görmek için kullanın:

```console
wget -qO- --timeout=2 http://backend.development
```

```output
wget: download timed out
```

Test bölmesinin çıkışı:

```console
exit
```

*Üretim* ad alanından gelen trafik reddedilirken, *geliştirme* ad alanına bir test bölmesi zamanlayın ve bir terminal oturumu ekleyin:

```console
kubectl run --rm -it frontend --image=alpine --labels app=webapp,role=frontend --namespace development --generator=run-pod/v1
```

Kabuk isteminde, `wget` ağ ilkesinin trafiğe izin verdiğini görmek için kullanın:

```console
wget -qO- http://backend
```

Bölme, ağ ilkesinde izin verilenlerle eşleşen ad alanında zamanlandığı için trafiğe izin verilir. Aşağıdaki örnek çıktı, döndürülen varsayılan NGINX web sayfasını gösterir:

```output
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
[...]
```

Ekli terminal oturumundan çıkın. Test bölmesi otomatik olarak silinir.

```console
exit
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede, iki ad alanı oluşturduk ve bir ağ ilkesi uyguladık. Bu kaynakları temizlemek için [kubectl delete][kubectl-delete] komutunu kullanın ve kaynak adlarını belirtin:

```console
kubectl delete namespace production
kubectl delete namespace development
```

## <a name="next-steps"></a>Sonraki adımlar

Ağ kaynakları hakkında daha fazla bilgi için [Azure Kubernetes Hizmeti'ndeki (AKS) uygulamalar için Ağ kavramlarına][concepts-network]bakın.

İlkeler hakkında daha fazla bilgi edinmek için [Kubernetes ağ ilkelerine][kubernetes-network-policies]bakın.

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubernetes-network-policies]: https://kubernetes.io/docs/concepts/services-networking/network-policies/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[policy-rules]: https://kubernetes.io/docs/concepts/services-networking/network-policies/#behavior-of-to-and-from-selectors
[aks-github]: https://github.com/azure/aks/issues
[tigera]: https://www.tigera.io/
[calicoctl]: https://docs.projectcalico.org/v3.9/reference/calicoctl/
[calico-support]: https://www.tigera.io/tigera-products/calico/
[calico-logs]: https://docs.projectcalico.org/v3.9/maintenance/component-logs
[calico-aks-cleanup]: https://github.com/Azure/aks-engine/blob/master/docs/topics/calico-3.3.1-cleanup-after-upgrade.yaml

<!-- LINKS - internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[use-advanced-networking]: configure-advanced-networking.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[concepts-network]: concepts-network.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
