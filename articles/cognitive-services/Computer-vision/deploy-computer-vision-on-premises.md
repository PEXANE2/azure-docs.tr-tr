---
title: Kubernetes ve Helm ile Bilgisayarlı Vizyon konteynerini kullanma
titleSuffix: Azure Cognitive Services
description: Computer Vision kapsayıcısını bir Azure Kapsayıcı Örneği'ne dağıtın ve bir web tarayıcısında test edin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 9aac374de5af748eafbe4c22e5fc89f64e483c2a
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80877991"
---
# <a name="use-computer-vision-container-with-kubernetes-and-helm"></a>Kubernetes ve Helm ile Bilgisayarlı Vizyon konteynerini kullanma

Bilgisayar Lı Vizyon kapsayıcılarınızı şirket içinde yönetmek için seçeneklerden biri Kubernetes ve Helm kullanmaktır. Bir Computer Vision konteyner görüntüsünü tanımlamak için Kubernetes ve Helm'i kullanarak bir Kubernetes paketi oluşturacağız. Bu paket, şirket içinde bir Kubernetes kümesine dağıtılacaktır. Son olarak, dağıtılan hizmetlerin nasıl test edilebildiğini araştıracağız. Kubernetes orkestrasyonu olmadan Docker kapsayıcıları çalıştırma hakkında daha fazla bilgi için Computer [Vision kapsayıcılarını yükleyin ve çalıştırın.](computer-vision-how-to-install-containers.md)

## <a name="prerequisites"></a>Ön koşullar

Bilgisayar Lı Vizyon kapsayıcılarını şirket içinde kullanmadan önce aşağıdaki ön koşullar:

| Gerekli | Amaç |
|----------|---------|
| Azure Hesabı | Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz][free-azure-account] bir hesap oluşturun. |
| Kubernetes CLI | [Kubernetes CLI,][kubernetes-cli] konteyner kayıt defterinden paylaşılan kimlik bilgilerini yönetmek için gereklidir. Kubernetes de Helm, Kubernetes paket yöneticisi önce gereklidir. |
| Dümen CLI | Bir dümen grafiği (konteyner paketi tanımı) yüklemek için kullanılan [Helm CLI][helm-install]yükleyin. |
| Bilgisayarlı Vizyon kaynağı |Kapsayıcıyı kullanabilmek için aşağıdakilere sahip olmalısınız:<br><br>Bir Azure **Computer Vision** kaynağı ve ilişkili API bitiş noktası URI anahtarı. Her iki değer de kaynak için Genel Bakış ve Anahtarlar sayfalarında kullanılabilir ve kapsayıcıyı başlatmak için gereklidir.<br><br>**{API_KEY}**: **Keys** sayfasındaki mevcut iki kaynak anahtarından biri<br><br>**{ENDPOINT_URI}**: **Genel Bakış** sayfasında sağlanan bitiş noktası|

[!INCLUDE [Gathering required parameters](../containers/includes/container-gathering-required-parameters.md)]

### <a name="the-host-computer"></a>Ana bilgisayar

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Konteyner gereksinimleri ve öneriler

[!INCLUDE [Container requirements and recommendations](includes/container-requirements-and-recommendations.md)]

## <a name="connect-to-the-kubernetes-cluster"></a>Kubernetes kümesine bağlanın

Ana bilgisayarın kullanılabilir bir Kubernetes kümesine sahip olması beklenir. Bir Kubernetes kümesini ana bilgisayara nasıl dağıtılayabilirsiniz kavramsal bir anlayış için [bir Kubernetes kümesini dağıtma](../../aks/tutorial-kubernetes-deploy-cluster.md) yla ilgili şu öğreticiye bakın.

### <a name="sharing-docker-credentials-with-the-kubernetes-cluster"></a>Docker kimlik bilgilerini Kubernetes kümesiyle paylaşma

Kubernetes kümesinin `docker pull` `containerpreview.azurecr.io` konteyner kayıt defterinden yapılandırılan görüntü(ler)e izin vermek için docker kimlik bilgilerini kümeye aktarmanız gerekir. Konteyner [`kubectl create`][kubectl-create] kayıt defteri erişim ön koşulundan sağlanan kimlik bilgilerine dayalı bir *docker-registry gizli* oluşturmak için aşağıdaki komutu çalıştırın.

Seçtiğiniz komut satırı arabirimi, aşağıdaki komutu çalıştırın. 'yi `<username>` `<password>`ve `<email-address>` kapsayıcı kayıt defteri kimlik bilgilerini değiştirdiğinden emin olun.

```console
kubectl create secret docker-registry containerpreview \
    --docker-server=containerpreview.azurecr.io \
    --docker-username=<username> \
    --docker-password=<password> \
    --docker-email=<email-address>
```

> [!NOTE]
> Kapsayıcı kayıt defterine `containerpreview.azurecr.io` zaten erişiminiz varsa, bunun yerine genel bayrağı kullanarak bir Kubernetes sırrı oluşturabilirsiniz. Docker yapılandırmanız JSON'a karşı yürüten aşağıdaki komutu düşünün.
> ```console
>  kubectl create secret generic containerpreview \
>      --from-file=.dockerconfigjson=~/.docker/config.json \
>      --type=kubernetes.io/dockerconfigjson
> ```

Aşağıdaki çıktı, gizli başarıyla oluşturulduğunda konsola yazdırılır.

```console
secret "containerpreview" created
```

Sırrın oluşturulduğunu doğrulamak için bayrakla `secrets` birlikte çalıştırın. [`kubectl get`][kubectl-get]

```console
kubectl get secrets
```

Yapılandırılan `kubectl get secrets` tüm sırların baskılarını yürütme.

```console
NAME                  TYPE                                  DATA      AGE
containerpreview      kubernetes.io/dockerconfigjson        1         30s
```

## <a name="configure-helm-chart-values-for-deployment"></a>Dağıtım için Miğfer grafiği değerlerini yapılandırma

*Read*adlı bir klasör oluşturarak başlayın, ardından aşağıdaki YAML içeriğini *Chart.yml*adlı yeni bir dosyaya yapıştırın.

```yaml
apiVersion: v1
name: read
version: 1.0.0
description: A Helm chart to deploy the microsoft/cognitive-services-read to a Kubernetes cluster
```

Miğfer grafiği varsayılan değerlerini yapılandırmak için aşağıdaki YAML'yi `values.yaml`kopyalayıp yapıştırın. `# {ENDPOINT_URI}` Yorumları ve `# {API_KEY}` yorumları kendi değerlerinizle değiştirin.

```yaml
# These settings are deployment specific and users can provide customizations

read:
  enabled: true
  image:
    name: cognitive-services-read
    registry: containerpreview.azurecr.io/
    repository: microsoft/cognitive-services-read
    tag: latest
    pullSecret: containerpreview # Or an existing secret
    args:
      eula: accept
      billing: # {ENDPOINT_URI}
      apikey: # {API_KEY}
```

> [!IMPORTANT]
> `billing` Ve `apikey` değerleri sağlanmazise, hizmetler 15 dakika sonra sona erer. Aynı şekilde, hizmetler kullanılamadığı için doğrulama başarısız olur.

*Okuma* dizininin altında bir *şablonlar* klasörü oluşturun. Aşağıdaki YAML'yi kopyalayıp yapıştırın. `deployment.yaml` Dosya `deployment.yaml` Bir Helm şablonu olarak hizmet verecektir.

> Şablonlar, Kubernetes'in anlayabileceği YAML biçimli kaynak açıklamaları olan bildirim dosyaları oluşturur. [- Dümen Grafiği Şablon Kılavuzu][chart-template-guide]

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: read
spec:
  template:
    metadata:
      labels:
        app: read-app
    spec:
      containers:
      - name: {{.Values.read.image.name}}
        image: {{.Values.read.image.registry}}{{.Values.read.image.repository}}
        ports:
        - containerPort: 5000
        env:
        - name: EULA
          value: {{.Values.read.image.args.eula}}
        - name: billing
          value: {{.Values.read.image.args.billing}}
        - name: apikey
          value: {{.Values.read.image.args.apikey}}
      imagePullSecrets:
      - name: {{.Values.read.image.pullSecret}}

--- 
apiVersion: v1
kind: Service
metadata:
  name: read
spec:
  type: LoadBalancer
  ports:
  - port: 5000
  selector:
    app: read-app
```

Şablon, bir yük dengeleyici hizmeti ve Read için kapsayıcınızın/resminizin dağıtımını belirtir.

### <a name="the-kubernetes-package-helm-chart"></a>Kubernetes paketi (Dümen grafiği)

*Miğfer grafiği,* konteyner kayıt defterinden hangi docker `containerpreview.azurecr.io` görüntüsünün(ler) çekilecek yapılandırmasını içerir.

> [Miğfer grafiği,][helm-charts] ilgili bir Kubernetes kaynaklarını açıklayan bir dosya topluluğudur. Tek bir grafik basit bir şey dağıtmak için kullanılabilir, bir memcached pod gibi, ya da karmaşık bir şey, HTTP sunucuları, veritabanları, önbellekleri ile tam bir web uygulaması yığını gibi, ve benzeri.

Sağlanan *Helm grafikleri,* Bilgisayar Görme Hizmeti'nin docker görüntülerini `containerpreview.azurecr.io` ve ilgili hizmeti konteyner kayıt defterinden çeker.

## <a name="install-the-helm-chart-on-the-kubernetes-cluster"></a>Kubernetes kümesine Miğfer grafiğini yükleme

*Dümen grafiğini*yüklemek için komutu [`helm install`][helm-install-cmd] yürütmemiz gerekir. Klasörün üstündeki dizinden yükleme komutunu `read` çalıştırdığından emin olun.

```console
helm install read ./read
```

Başarılı bir yükleme yürütmesinden görmeyi bekleyebilecekleriniz bir örnek çıktı aşağıda verilmiştir:

```console
NAME: read
LAST DEPLOYED: Thu Sep 04 13:24:06 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                    READY  STATUS             RESTARTS  AGE
read-57cb76bcf7-45sdh   0/1    ContainerCreating  0         0s

==> v1/Service
NAME     TYPE          CLUSTER-IP    EXTERNAL-IP  PORT(S)         AGE
read     LoadBalancer  10.110.44.86  localhost    5000:31301/TCP  0s

==> v1beta1/Deployment
NAME    READY  UP-TO-DATE  AVAILABLE  AGE
read    0/1    1           0          0s
```

Kubernetes dağıtımının tamamlanması birkaç dakika sürebilir. Hem bölmelerin hem de hizmetlerin düzgün şekilde dağıtılmış ve kullanılabilir olduğunu doğrulamak için aşağıdaki komutu uygulayın:

```console
kubectl get all
```

Aşağıdaki çıktıya benzer bir şey görmeyi beklemelisiniz:

```console
kubectl get all
NAME                        READY   STATUS    RESTARTS   AGE
pod/read-57cb76bcf7-45sdh   1/1     Running   0          17s

NAME                   TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/kubernetes     ClusterIP      10.96.0.1      <none>        443/TCP          45h
service/read           LoadBalancer   10.110.44.86   localhost     5000:31301/TCP   17s

NAME                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/read   1/1     1            1           17s

NAME                              DESIRED   CURRENT   READY   AGE
replicaset.apps/read-57cb76bcf7   1         1         1       17s
```
<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure Kubernetes Hizmetinde (AKS) Helm ile uygulama yükleme hakkında daha fazla bilgi [için burayı ziyaret edin.][installing-helm-apps-in-aks]

> [!div class="nextstepaction"]
> [Bilişsel Hizmetler Konteynerler][cog-svcs-containers]

<!-- LINKS - external -->
[free-azure-account]: https://azure.microsoft.com/free
[git-download]: https://git-scm.com/downloads
[azure-cli]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest
[docker-engine]: https://www.docker.com/products/docker-engine
[kubernetes-cli]: https://kubernetes.io/docs/tasks/tools/install-kubectl
[helm-install]: https://helm.sh/docs/using_helm/#installing-helm
[helm-install-cmd]: https://helm.sh/docs/intro/using_helm/#helm-install-installing-a-package
[helm-charts]: https://helm.sh/docs/topics/charts/
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[helm-test]: https://helm.sh/docs/helm/#helm-test
[chart-template-guide]: https://helm.sh/docs/chart_template_guide

<!-- LINKS - internal -->
[vision-container-host-computer]: computer-vision-how-to-install-containers.md#the-host-computer
[installing-helm-apps-in-aks]: ../../aks/kubernetes-helm.md
[cog-svcs-containers]: ../cognitive-services-container-support.md
