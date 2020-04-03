---
title: Azure Kubernetes Hizmeti'nde (AKS) statik IP adresine sahip bir HTTP giriş denetleyicisi oluşturma
description: Azure Kubernetes Hizmeti (AKS) kümesinde statik genel IP adresine sahip bir NGINX giriş denetleyicisini nasıl yükleyip yapılandırabilirsiniz öğrenin.
services: container-service
ms.topic: article
ms.date: 05/24/2019
ms.openlocfilehash: 3e79bbe76a751097acd5c9d3c42dbd4020b6866b
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80617284"
---
# <a name="create-an-ingress-controller-with-a-static-public-ip-address-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti'nde (AKS) statik genel IP adresine sahip bir giriş denetleyicisi oluşturma

Giriş denetleyicisi, Kubernetes hizmetleri için ters proxy, yapılandırılabilir trafik yönlendirmesi ve TLS sonlandırma sağlayan bir yazılım dır. Kubernetes giriş kaynakları, tek tek Kubernetes hizmetleri için giriş kurallarını ve rotalarını yapılandırmak için kullanılır. Giriş denetleyicisi ve giriş kuralları kullanılarak, trafiği Bir Kubernetes kümesindeki birden çok hizmete yönlendirmek için tek bir IP adresi kullanılabilir.

Bu makalede, Bir Azure Kubernetes Hizmeti (AKS) kümesinde [NGINX giriş denetleyicisini][nginx-ingress] nasıl dağıtabileceğinizgösterilmektedir. Giriş denetleyicisi statik bir genel IP adresi ile yapılandırılır. [Sertifika yöneticisi][cert-manager] projesi, [Let's Encrypt][lets-encrypt] sertifikalarını otomatik olarak oluşturmak ve yapılandırmak için kullanılır. Son olarak, aks kümesinde her biri tek bir IP adresi üzerinden erişilebilen iki uygulama çalıştırılır.

Aşağıdakileri de yapabilirsiniz:

- [Harici ağ bağlantısı na sahip temel bir giriş denetleyicisi oluşturma][aks-ingress-basic]
- [HTTP uygulama yönlendirme eklentisini etkinleştirme][aks-http-app-routing]
- [Kendi TLS sertifikalarınızı kullanan bir giriş denetleyicisi oluşturun][aks-ingress-own-tls]
- [Dinamik bir genel IP adresine sahip TLS sertifikaları otomatik olarak oluşturmak için Let's Encrypt'i kullanan bir giriş denetleyicisi oluşturun][aks-ingress-tls]

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, varolan bir AKS kümesi var sayıyor. AKS kümesine ihtiyacınız varsa, [Azure CLI'yi veya][aks-quickstart-cli] [Azure portalını kullanarak][aks-quickstart-portal]AKS hızlı başlat'ına bakın.

Bu makalede, NGINX giriş denetleyicisini, cert-manager'ı ve örnek bir web uygulamasını yüklemek için Helm kullanır. Helm'in en son sürümünden kullandığınızdan emin olun. Yükseltme yönergeleri için [Helm yükleme dokümanlarına][helm-install]bakın. Helm'i yapılandırma ve kullanma hakkında daha fazla bilgi için Azure [Kubernetes Hizmetinde (AKS) Helm ile Yükleme uygulamaları'na][use-helm]bakın.

Bu makalede, Azure CLI sürümünü 2.0.64 veya sonraki sürümde çalıştırdığınızda da vardır. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Giriş denetleyicisi oluşturma

Varsayılan olarak, yeni bir genel IP adresi ataması ile bir NGINX giriş denetleyicisi oluşturulur. Bu genel IP adresi yalnızca giriş denetleyicisinin ömrü için statiktir ve denetleyici silinip yeniden oluşturulursa kaybolur. Yaygın bir yapılandırma gereksinimi NGINX giriş denetleyicisi varolan statik genel IP adresi sağlamaktır. Giriş denetleyicisi silinirse statik genel IP adresi kalır. Bu yaklaşım, uygulamalarınızın yaşam döngüsü boyunca varolan DNS kayıtlarını ve ağ yapılandırmalarını tutarlı bir şekilde kullanmanıza olanak tanır.

Statik bir genel IP adresi oluşturmanız gerekiyorsa, önce AKS kümesinin kaynak grup adını [az aks göster][az-aks-show] komutuyla alın:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv
```

Ardından, [az ağı public-ip oluşturma][az-network-public-ip-create] komutunu kullanarak *statik* ayırma yöntemiyle ortak bir IP adresi oluşturun. Aşağıdaki örnek, önceki adımda elde edilen AKS küme kaynak grubunda *myAKSPublicIP* adlı genel bir IP adresi oluşturur:

```azurecli-interactive
az network public-ip create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP --sku Standard --allocation-method static --query publicIp.ipAddress -o tsv
```

Şimdi Helm ile *nginx-ingress* grafik dağıtmak. Daha fazla yedeklilik sağlamak için `--set controller.replicaCount` parametresiyle iki NGINX giriş denetleyicisi çoğaltması dağıtılır. Giriş denetleyicisinin yinelemelerini çalıştıran kopyalardan tam olarak yararlanmak için AKS kümenizde birden fazla düğüm olduğundan emin olun.

Giriş denetleyicisi, giriş denetleyici hizmetine tahsis edilecek yük dengeleyicisinin statik IP adresinin ve kamu IP adresi kaynağına uygulanan DNS ad etiketinin haberdar olması için Miğfer sürümüne iki ek parametre geçirmeniz gerekir. HTTPS sertifikalarının doğru çalışması için, giriş denetleyicisi IP adresi için bir FQDN yapılandırmak için bir DNS ad etiketi kullanılır.

1. Parametreyi `--set controller.service.loadBalancerIP` ekleyin. Önceki adımda oluşturulan kendi genel IP adresinizi belirtin.
1. Parametreyi `--set controller.service.annotations."service\.beta\.kubernetes\.io/azure-dns-label-name"` ekleyin. Önceki adımda oluşturulan genel IP adresine uygulanacak bir DNS ad etiketi belirtin.

Ayrıca giriş denetleyicisinin bir Linux düğümü üzerinde zamanlanması gerekir. Windows Server düğümleri (şu anda AKS önizlemede) giriş denetleyicisini çalıştırmamalıdır. Kubernetes zamanlayıcısına NGINX giriş denetleyicisini Linux tabanlı bir düğümde çalıştırmasını söylemek için `--set nodeSelector` parametresi kullanılarak bir düğüm seçici belirtilir.

> [!TIP]
> Aşağıdaki örnek, *giriş temel*adlı giriş kaynakları için bir Kubernetes ad alanı oluşturur. Gerektiğinde kendi ortamınız için bir ad alanı belirtin. AKS kümeniz RBAC etkin değilse, Helm komutlarına ekleyin. `--set rbac.create=false`

> [!TIP]
> Kümenizdeki kapsayıcılara gelen istekler için istemci kaynağı IP `--set controller.service.externalTrafficPolicy=Local` [korumasını][client-source-ip] etkinleştirmek istiyorsanız, Helm install komutuna ekleyin. İstemci kaynağı IP, *X-Forwarded-For*altında istek üstbilgisinde depolanır. İstemci kaynağı IP koruma etkin leştirilmiş bir giriş denetleyicisi kullanırken, SSL geçişi çalışmaz.

Giriş denetleyicinizin **IP adresi** ve FQDN öneki için kullanmak istediğiniz benzersiz bir **ad** ile aşağıdaki komut dosyasını güncelleştirin:

```console
# Create a namespace for your ingress resources
kubectl create namespace ingress-basic

# Use Helm to deploy an NGINX ingress controller
helm install nginx-ingress stable/nginx-ingress \
    --namespace ingress-basic \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set controller.service.loadBalancerIP="40.121.63.72"
    --set controller.service.annotations."service\.beta\.kubernetes\.io/azure-dns-label-name"="demo-aks-ingress"
```

NGINX giriş denetleyicisi için Kubernetes yük dengeleyici hizmeti oluşturulduğunda, aşağıdaki örnek çıktıda gösterildiği gibi statik IP adresiniz atanır:

```
$ kubectl get service -l app=nginx-ingress --namespace ingress-basic

NAME                                        TYPE           CLUSTER-IP    EXTERNAL-IP    PORT(S)                      AGE
nginx-ingress-controller                    LoadBalancer   10.0.232.56   40.121.63.72   80:31978/TCP,443:32037/TCP   3m
nginx-ingress-default-backend               ClusterIP      10.0.95.248   <none>         80/TCP                       3m
```

Henüz giriş kuralları oluşturulmadı, bu nedenle ortak IP adresine göz atarsanız NGINX giriş denetleyicisinin varsayılan 404 sayfası görüntülenir. Giriş kuralları aşağıdaki adımlarda yapılandırılır.

Genel IP adresindeki FQDN sorgulayarak DNS ad etiketinin uygulandığını doğrulayabilirsiniz:

```azurecli-interactive
#!/bin/bash
az network public-ip list --resource-group MC_myResourceGroup_myAKSCluster_eastus --query $("[?name=='myAKSPublicIP'].[dnsSettings.fqdn]") -o tsv
```

Giriş denetleyicisi artık IP adresi veya FQDN üzerinden erişilebilir.

## <a name="install-cert-manager"></a>cert-manager denetleyicisini yükleme

NGINX giriş denetleyicisi, TLS sonlandırmayı destekler. HTTPS için birçok farklı sertifika alma ve yapılandırma yöntemi vardır. Bu makalede, otomatik [Lets Encrypt][lets-encrypt] sertifika oluşturma ve yönetim işlevselliği sağlayan [cert-manager,][cert-manager]kullanarak gösterir.

> [!NOTE]
> Bu makalede, `staging` Let's Encrypt için ortam kullanır. Üretim dağıtımlarında, `letsencrypt-prod` kaynak `https://acme-v02.api.letsencrypt.org/directory` tanımlarında ve Miğfer grafiği yüklerken kullanın.

Cert yöneticisi denetleyicisini RBAC etkin leştirilmiş bir kümeye `helm install` yüklemek için aşağıdaki komutu kullanın:

```console
# Install the CustomResourceDefinition resources separately
kubectl apply --validate=false -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.13/deploy/manifests/00-crds.yaml

# Label the cert-manager namespace to disable resource validation
kubectl label namespace ingress-basic cert-manager.io/disable-validation=true

# Add the Jetstack Helm repository
helm repo add jetstack https://charts.jetstack.io

# Update your local Helm chart repository cache
helm repo update

# Install the cert-manager Helm chart
helm install \
  cert-manager \
  --namespace ingress-basic \
  --version v0.13.0 \
  jetstack/cert-manager
```

Sertifika yöneticisi yapılandırması hakkında daha fazla bilgi [için, sertifika yöneticisi projesine][cert-manager]bakın.

## <a name="create-a-ca-cluster-issuer"></a>CA küme veren oluşturma

Sertifikalar verilmeden önce, sertifika yöneticisi nin bir [İhraççı][cert-manager-issuer] veya [ClusterIssuer][cert-manager-cluster-issuer] kaynağı na ihtiyacı vardır. Bu Kubernetes kaynakları işlevsellik `Issuer` olarak aynıdır, ancak tek `ClusterIssuer` bir ad alanında çalışır ve tüm ad alanlarında çalışır. Daha fazla bilgi için [sertifika yöneticisi veren][cert-manager-issuer] belgelerine bakın.

Aşağıdaki örnek bildirimi kullanarak `cluster-issuer.yaml`bir küme veren oluşturun. E-posta adresini kuruluşunuzdan geçerli bir adresle güncelleştirin:

```yaml
apiVersion: cert-manager.io/v1alpha2
kind: ClusterIssuer
metadata:
  name: letsencrypt-staging
spec:
  acme:
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    email: user@contoso.com
    privateKeySecretRef:
      name: letsencrypt-staging
    solvers:
    - http01:
        ingress:
          class: nginx
```

İhraççıyı oluşturmak `kubectl apply -f cluster-issuer.yaml` için komutu kullanın.

```
$ kubectl apply -f cluster-issuer.yaml --namespace ingress-basic

clusterissuer.cert-manager.io/letsencrypt-staging created
```

## <a name="run-demo-applications"></a>Demo uygulamaları çalıştırma

Giriş denetleyicisi ve sertifika yönetimi çözümü yapılandırıldı. Şimdi AKS kümenizde iki demo uygulaması çalıştıralım. Bu örnekte, Helm basit bir 'Merhaba dünya' uygulamasının iki örneğini dağıtmak için kullanılır.

Örnek Miğfer grafiklerini yüklemeden önce, Azure örnek lerini Miğfer ortamınıza aşağıdaki gibi ekleyin:

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Bir Miğfer grafiğinden aşağıdaki komutla ilk demo uygulamasını oluşturun:

```console
helm install aks-helloworld azure-samples/aks-helloworld --namespace ingress-basic
```

Şimdi demo uygulamasının ikinci bir örneğini yükleyin. İkinci örnek için, iki uygulamanın görsel olarak farklı olması için yeni bir başlık belirtirsiniz. Ayrıca benzersiz bir hizmet adı belirtirsiniz:

```console
helm install aks-helloworld-2 azure-samples/aks-helloworld \
    --namespace ingress-basic \
    --set title="AKS Ingress Demo" \
    --set serviceName="ingress-demo"
```

## <a name="create-an-ingress-route"></a>Giriş rotası oluşturma

Her iki uygulama şimdi Kubernetes kümeüzerinde çalışıyor, ancak tür `ClusterIP`bir hizmet ile yapılandırılır. Bu nedenle, uygulamalara internetten erişilemez. Bunları herkese açık hale getirmek için bir Kubernetes giriş kaynağı oluşturun. Giriş kaynağı, trafiği iki uygulamadan birine yönlendiren kuralları yapılandırır.

Aşağıdaki örnekte, adrese `https://demo-aks-ingress.eastus.cloudapp.azure.com/` giden trafik adlı `aks-helloworld`hizmete yönlendirilir. Adrese `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` giden trafik `ingress-demo` servise yönlendirilir. Ana *bilgisayarları* ve *ana bilgisayarları* önceki adımda oluşturduğunuz DNS adına güncelleştirin.

Adlandırılmış `hello-world-ingress.yaml` bir dosya oluşturun ve aşağıdaki örnekYAML'de kopyalayın.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    cert-manager.io/cluster-issuer: letsencrypt-staging
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  tls:
  - hosts:
    - demo-aks-ingress.eastus.cloudapp.azure.com
    secretName: tls-secret
  rules:
  - host: demo-aks-ingress.eastus.cloudapp.azure.com
    http:
      paths:
      - backend:
          serviceName: aks-helloworld
          servicePort: 80
        path: /(.*)
      - backend:
          serviceName: ingress-demo
          servicePort: 80
        path: /hello-world-two(/|$)(.*)
```

`kubectl apply -f hello-world-ingress.yaml --namespace ingress-basic` Komutu kullanarak giriş kaynağını oluşturun.

```
$ kubectl apply -f hello-world-ingress.yaml --namespace ingress-basic

ingress.extensions/hello-world-ingress created
```

## <a name="create-a-certificate-object"></a>Sertifika nesnesi oluşturma

Ardından, bir sertifika kaynağı oluşturulmalıdır. Sertifika kaynağı istenen X.509 sertifikasını tanımlar. Daha fazla bilgi için [sertifika yöneticisi sertifikalarına][cert-manager-certificates]bakın.

Cert-manager büyük olasılıkla v0.2.2'den beri cert yöneticisiyle otomatik olarak dağıtılan ingress-shim'i kullanarak sizin için otomatik olarak bir sertifika nesnesi oluşturmuştur. Daha fazla bilgi için [giriş-şim belgelerine][ingress-shim]bakın.

Sertifikanın başarıyla oluşturulduğunu doğrulamak için `kubectl describe certificate tls-secret --namespace ingress-basic` komutu kullanın.

Sertifika verildiyse, aşağıdakilere benzer çıktı görürsünüz:
```
Type    Reason          Age   From          Message
----    ------          ----  ----          -------
  Normal  CreateOrder     11m   cert-manager  Created new ACME order, attempting validation...
  Normal  DomainVerified  10m   cert-manager  Domain "demo-aks-ingress.eastus.cloudapp.azure.com" verified with "http-01" validation
  Normal  IssueCert       10m   cert-manager  Issuing certificate...
  Normal  CertObtained    10m   cert-manager  Obtained certificate from ACME server
  Normal  CertIssued      10m   cert-manager  Certificate issued successfully
```

Ek bir sertifika kaynağı oluşturmanız gerekiyorsa, bunu aşağıdaki örnek bildirimle yapabilirsiniz. *DNSAdlarını* ve *etki alanlarını* önceki adımda oluşturduğunuz DNS adına güncelleştirin. Yalnızca dahili giriş denetleyicisi kullanıyorsanız, hizmetinizin dahili DNS adını belirtin.

```yaml
apiVersion: cert-manager.io/v1alpha2
kind: Certificate
metadata:
  name: tls-secret
  namespace: ingress-basic
spec:
  secretName: tls-secret
  dnsNames:
  - demo-aks-ingress.eastus.cloudapp.azure.com
  acme:
    config:
    - http01:
        ingressClass: nginx
      domains:
      - demo-aks-ingress.eastus.cloudapp.azure.com
  issuerRef:
    name: letsencrypt-staging
    kind: ClusterIssuer
```

Sertifika kaynağını oluşturmak için `kubectl apply -f certificates.yaml` komutu kullanın.

```
$ kubectl apply -f certificates.yaml

certificate.cert-manager.io/tls-secret created
```

## <a name="test-the-ingress-configuration"></a>Giriş yapılandırmasını test edin

Kubernetes giriş denetleyicinizin FQDN'sine bir web *https://demo-aks-ingress.eastus.cloudapp.azure.com*tarayıcısı açın, örneğin.

Bu örneklerde `letsencrypt-staging`kullanıldığı gibi, verilen SSL sertifikası tarayıcı tarafından güvenilmeyen. Başvurunuza devam etmek için uyarı istemini kabul edin. Sertifika bilgileri, bu *Sahte LE Ara X1* sertifikasının Let's Encrypt tarafından verildiğini gösterir. Bu sahte `cert-manager` sertifika, isteği doğru şekilde işledi ve sağlayıcıdan bir sertifika aldığını gösterir:

![Evreleme sertifikasını şifreleyelim](media/ingress/staging-certificate.png)

Let's Encrypt yerine kullanmak `prod` için `staging`Şifreleyelim'i değiştirdiğinizde, aşağıdaki örnekte gösterildiği gibi Let's Encrypt tarafından verilen güvenilir bir sertifika kullanılır:

![Sertifikayı Şifreleyelim](media/ingress/certificate.png)

Demo uygulaması web tarayıcısında gösterilir:

![Uygulama örneği bir](media/ingress/app-one.png)

Şimdi FQDN için */ hello-world-iki* yol *https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two*ekleyin, gibi . Özel başlığı ile ikinci demo uygulaması gösterilir:

![Uygulama örneği iki](media/ingress/app-two.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede, giriş bileşenleri, sertifikalar ve örnek uygulamalar yüklemek için Helm kullanılmıştır. Bir Miğfer grafiği dağıttığınızda, bir dizi Kubernetes kaynağı oluşturulur. Bu kaynaklar bölmeleri, dağıtımları ve hizmetleri içerir. Bu kaynakları temizlemek için, örnek ad alanının tamamını veya tek tek kaynakları silebilirsiniz.

### <a name="delete-the-sample-namespace-and-all-resources"></a>Örnek ad alanını ve tüm kaynakları silme

Örnek ad alanının tamamını silmek için komutu `kubectl delete` kullanın ve ad alanı adınızı belirtin. Ad alanındaki tüm kaynaklar silinir.

```console
kubectl delete namespace ingress-basic
```

Daha sonra, AKS merhaba dünya uygulaması için Helm repo kaldırın:

```console
helm repo remove azure-samples
```

### <a name="delete-resources-individually"></a>Kaynakları tek tek silme

Alternatif olarak, daha ayrıntılı bir yaklaşım oluşturulan tek tek kaynakları silmektir. İlk olarak, sertifika kaynaklarını kaldırın:

```console
kubectl delete -f certificates.yaml
kubectl delete -f cluster-issuer.yaml
```

Şimdi Helm bültenlerini `helm list` komutla listele. Aşağıdaki örnek çıktıda gösterildiği gibi *nginx-ingress*, *cert-manager*ve *aks-helloworld*adlı grafiklere bakın:

```
$ helm list --all-namespaces

NAME                    NAMESPACE       REVISION        UPDATED                        STATUS          CHART                   APP VERSION
aks-helloworld          ingress-basic   1               2020-01-11 15:02:21.51172346   deployed        aks-helloworld-0.1.0
aks-helloworld-2        ingress-basic   1               2020-01-11 15:03:10.533465598  deployed        aks-helloworld-0.1.0
nginx-ingress           ingress-basic   1               2020-01-11 14:51:03.454165006  deployed        nginx-ingress-1.28.2    0.26.2
cert-manager            ingress-basic    1               2020-01-06 21:19:03.866212286  deployed        cert-manager-v0.13.0    v0.13.0
```

`helm uninstall` Komutla sürümleri silin. Aşağıdaki örnek, NGINX giriş dağıtımını, sertifika yöneticisini ve iki örnek AKS merhaba dünya uygulamalarını siler.

```
$ helm uninstall aks-helloworld aks-helloworld-2 nginx-ingress cert-manager -n ingress-basic

release "aks-helloworld" deleted
release "aks-helloworld-2" deleted
release "nginx-ingress" deleted
release "cert-manager" deleted
```

Ardından, AKS merhaba dünya uygulaması için Helm repo kaldırın:

```console
helm repo remove azure-samples
```

Ad alanını silin. Komutu `kubectl delete` kullanın ve ad alanı adınızı belirtin:

```console
kubectl delete namespace ingress-basic
```

Son olarak, giriş denetleyicisi için oluşturulan statik genel IP adresini kaldırın. Bu makalenin ilk adımında elde edilen *MC_* küme kaynak grubu adınızı sağlayın( *örneğin, MC_myResourceGroup_myAKSCluster_eastus:*

```azurecli-interactive
az network public-ip delete --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede AKS bazı dış bileşenleri dahil. Bu bileşenler hakkında daha fazla bilgi edinmek için aşağıdaki proje sayfalarına bakın:

- [Dümen CLI][helm-cli]
- [NGINX giriş denetleyicisi][nginx-ingress]
- [sertifika yöneticisi][cert-manager]

Aşağıdakileri de yapabilirsiniz:

- [Harici ağ bağlantısı na sahip temel bir giriş denetleyicisi oluşturma][aks-ingress-basic]
- [HTTP uygulama yönlendirme eklentisini etkinleştirme][aks-http-app-routing]
- [Dahili, özel ağ ve IP adresi kullanan bir giriş denetleyicisi oluşturma][aks-ingress-internal]
- [Kendi TLS sertifikalarınızı kullanan bir giriş denetleyicisi oluşturun][aks-ingress-own-tls]
- [Dinamik bir genel IP'ye sahip bir giriş denetleyicisi oluşturun ve TLS sertifikalarını otomatik olarak oluşturmak için Şifreleyelim'i yapılandırın][aks-ingress-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm
[cert-manager]: https://github.com/jetstack/cert-manager
[cert-manager-certificates]: https://cert-manager.readthedocs.io/en/latest/reference/certificates.html
[cert-manager-cluster-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/clusterissuers.html
[cert-manager-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/issuers.html
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm
[ingress-shim]: https://docs.cert-manager.io/en/latest/tasks/issuing-certificates/ingress-shim.html

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-own-tls]: ingress-own-tls.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[client-source-ip]: concepts-network.md#ingress-controllers
[install-azure-cli]: /cli/azure/install-azure-cli
