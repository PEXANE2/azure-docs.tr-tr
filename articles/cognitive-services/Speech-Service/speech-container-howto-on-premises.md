---
title: Kubernetes ve Helm ile Konuşma hizmet kapları kullanma
titleSuffix: Azure Cognitive Services
description: Kubernetes ve Helm'i kullanarak konuşmadan metne ve metinden konuşmaya kapsayıcı görüntüleri tanımlayarak bir Kubernetes paketi oluşturacağız. Bu paket, şirket içinde bir Kubernetes kümesine dağıtılacaktır.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 3c183f6d0e2d80ed497654448a726a1562bd046c
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80874373"
---
# <a name="use-speech-service-containers-with-kubernetes-and-helm"></a>Kubernetes ve Helm ile Konuşma hizmet kapları kullanma

Konuşma kapsayıcılarınızı şirket içinde yönetmek için bir seçenek kubernetes ve Helm kullanmaktır. Kubernetes ve Helm'i kullanarak konuşmadan metne ve metinden konuşmaya kapsayıcı görüntüleri tanımlayarak bir Kubernetes paketi oluşturacağız. Bu paket, şirket içinde bir Kubernetes kümesine dağıtılacaktır. Son olarak, dağıtılan hizmetleri ve çeşitli yapılandırma seçeneklerini nasıl test edebileceğimizi araştıracağız. Kubernetes orkestrasyonu olmadan Docker kapsayıcıları çalıştırma hakkında daha fazla bilgi için Konuşma [hizmet kaplarını yükleyin ve çalıştırın.](speech-container-howto.md)

## <a name="prerequisites"></a>Ön koşullar

Konuşma kapsayıcılarını şirket içinde kullanmadan önce aşağıdaki ön koşullar:

| Gerekli | Amaç |
|----------|---------|
| Azure Hesabı | Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz][free-azure-account] bir hesap oluşturun. |
| Konteyner Kayıt Defteri erişimi | Kubernetes'in docker görüntülerini kümeye çekmesi için konteyner kayıt defterine erişmesi gerekir. |
| Kubernetes CLI | [Kubernetes CLI,][kubernetes-cli] konteyner kayıt defterinden paylaşılan kimlik bilgilerini yönetmek için gereklidir. Kubernetes de Helm, Kubernetes paket yöneticisi önce gereklidir. |
| Dümen CLI | Bir dümen grafiği (konteyner paketi tanımı) yüklemek için kullanılan [Helm CLI][helm-install]yükleyin. |
|Konuşma kaynağı |Bu kapsayıcıları kullanabilmek için şunları yapmalısınız:<br><br>İlişkili faturalandırma anahtarını ve faturalandırma bitiş noktası URI'yi almak için Bir _Konuşma_ Azure kaynağı. Her iki değer de Azure portalının **Konuşma** Genel Bakışı ve Anahtarlar sayfalarında mevcuttur ve kapsayıcıyı başlatmak için gereklidir.<br><br>**{API_KEY}**: kaynak anahtarı<br><br>**{ENDPOINT_URI}**: uç nokta URI örneği:`https://westus.api.cognitive.microsoft.com/sts/v1.0`|

## <a name="the-recommended-host-computer-configuration"></a>Önerilen ana bilgisayar yapılandırması

[Konuşma hizmeti kapsayıcısı ana bilgisayar][speech-container-host-computer] ayrıntılarını başvuru olarak inceleyin. Bu *dümen grafiği,* kullanıcının belirlediği kaç decodes (eşzamanlı istek) temel alınca CPU ve bellek gereksinimlerini otomatik olarak hesaplar. Ayrıca, ses/metin girişi için optimizasyonların .olarak `enabled`yapılandırılıp yapılandırılmasına bağlı olarak ayarlanır. Dümen grafiği varsayılan olarak, iki eşzamanlı istek ve optimizasyon devre dışı bırakma.

| Hizmet | CPU / Konteyner | Bellek / Konteyner |
|--|--|--|
| **Konuşmadan Metne** | bir kod çözücü en az 1.150 milicores gerektirir. `optimizedForAudioFile` Etkinise, 1.950 milicores gereklidir. (varsayılan: iki kod çözücü) | Gerekli: 2 GB<br>Sınırlı: 4 GB |
| **Metinden Konuşmaya** | bir eşzamanlı istek en az 500 milicores gerektirir. Eğer `optimizeForTurboMode` etkinse, o zaman 1.000 milicores gereklidir. (varsayılan: iki eşzamanlı istek) | Gerekli: 1 GB<br> Sınırlı: 2 GB |

## <a name="connect-to-the-kubernetes-cluster"></a>Kubernetes kümesine bağlanın

Ana bilgisayarın kullanılabilir bir Kubernetes kümesine sahip olması beklenir. Bir Kubernetes kümesini ana bilgisayara nasıl dağıtılayabilirsiniz kavramsal bir anlayış için [bir Kubernetes kümesini dağıtma](../../aks/tutorial-kubernetes-deploy-cluster.md) yla ilgili şu öğreticiye bakın.

### <a name="sharing-docker-credentials-with-the-kubernetes-cluster"></a>Docker kimlik bilgilerini Kubernetes kümesiyle paylaşma

Kubernetes kümesinin `docker pull` `containerpreview.azurecr.io` konteyner kayıt defterinden yapılandırılan görüntü(ler)e izin vermek için docker kimlik bilgilerini kümeye aktarmanız gerekir. Konteyner [`kubectl create`][kubectl-create] kayıt defteri erişim ön koşulundan sağlanan kimlik bilgilerine dayalı bir *docker-registry gizli* oluşturmak için aşağıdaki komutu çalıştırın.

Seçtiğiniz komut satırı arabirimi, aşağıdaki komutu çalıştırın. 'yi `<username>` `<password>`ve `<email-address>` kapsayıcı kayıt defteri kimlik bilgilerini değiştirdiğinden emin olun.

```console
kubectl create secret docker-registry mcr \
    --docker-server=containerpreview.azurecr.io \
    --docker-username=<username> \
    --docker-password=<password> \
    --docker-email=<email-address>
```

> [!NOTE]
> Kapsayıcı kayıt defterine `containerpreview.azurecr.io` zaten erişiminiz varsa, bunun yerine genel bayrağı kullanarak bir Kubernetes sırrı oluşturabilirsiniz. Docker yapılandırmanız JSON'a karşı yürüten aşağıdaki komutu düşünün.
> ```console
>  kubectl create secret generic mcr \
>      --from-file=.dockerconfigjson=~/.docker/config.json \
>      --type=kubernetes.io/dockerconfigjson
> ```

Aşağıdaki çıktı, gizli başarıyla oluşturulduğunda konsola yazdırılır.

```console
secret "mcr" created
```

Sırrın oluşturulduğunu doğrulamak için bayrakla `secrets` birlikte çalıştırın. [`kubectl get`][kubectl-get]

```console
kubectl get secrets
```

Yapılandırılan `kubectl get secrets` tüm sırların baskılarını yürütme.

```console
NAME    TYPE                              DATA    AGE
mcr     kubernetes.io/dockerconfigjson    1       30s
```

## <a name="configure-helm-chart-values-for-deployment"></a>Dağıtım için Miğfer grafiği değerlerini yapılandırma

Microsoft tarafından sunulan herkese açık tüm dümen grafikleri için [Microsoft Helm Hub'ı][ms-helm-hub] ziyaret edin. Microsoft Helm Hub'ından **Bilişsel Hizmetler Konuşma Şirket İçi Grafiği'ni**bulacaksınız. **Bilişsel Hizmetler Konuşma On-Premises** yükleyeceğimiz grafik, ancak öncelikle `config-values.yaml` açık yapılandırmaları içeren bir dosya oluşturmamız gerekir. Microsoft deposunu Helm örneğimize ekleyerek başlayalım.

```console
helm repo add microsoft https://microsoft.github.io/charts/repo
```

Ardından, Helm grafik değerlerimizi yapılandıracağız. Aşağıdaki YAML'yi kopyalayıp yapıştırın. `config-values.yaml` **Bilişsel Hizmetler Konuşma On-Premises Helm Chart**özelleştirme hakkında daha fazla bilgi için, [dümen grafikleri özelleştirmek](#customize-helm-charts)bakın. `# {ENDPOINT_URI}` Yorumları ve `# {API_KEY}` yorumları kendi değerlerinizle değiştirin.

```yaml
# These settings are deployment specific and users can provide customizations

# speech-to-text configurations
speechToText:
  enabled: true
  numberOfConcurrentRequest: 3
  optimizeForAudioFile: true
  image:
    registry: containerpreview.azurecr.io
    repository: microsoft/cognitive-services-speech-to-text
    tag: latest
    pullSecrets:
      - mcr # Or an existing secret
    args:
      eula: accept
      billing: # {ENDPOINT_URI}
      apikey: # {API_KEY}

# text-to-speech configurations
textToSpeech:
  enabled: true
  numberOfConcurrentRequest: 3
  optimizeForTurboMode: true
  image:
    registry: containerpreview.azurecr.io
    repository: microsoft/cognitive-services-text-to-speech
    tag: latest
    pullSecrets:
      - mcr # Or an existing secret
    args:
      eula: accept
      billing: # {ENDPOINT_URI}
      apikey: # {API_KEY}
```

> [!IMPORTANT]
> `billing` Ve `apikey` değerleri sağlanmazise, hizmetler 15 dakika sonra sona erer. Aynı şekilde, hizmetler kullanılamadığı için doğrulama başarısız olur.

### <a name="the-kubernetes-package-helm-chart"></a>Kubernetes paketi (Dümen grafiği)

*Miğfer grafiği,* konteyner kayıt defterinden hangi docker `containerpreview.azurecr.io` görüntüsünün(ler) çekilecek yapılandırmasını içerir.

> [Miğfer grafiği,][helm-charts] ilgili bir Kubernetes kaynaklarını açıklayan bir dosya topluluğudur. Tek bir grafik basit bir şey dağıtmak için kullanılabilir, bir memcached pod gibi, ya da karmaşık bir şey, HTTP sunucuları, veritabanları, önbellekleri ile tam bir web uygulaması yığını gibi, ve benzeri.

Sağlanan *Helm grafikleri,* konuşma hizmetinin docker görüntülerini, hem metinden konuşmaya hem de `containerpreview.azurecr.io` kapsayıcı kayıt defterinden konuşma-metin hizmetleri çeker.

## <a name="install-the-helm-chart-on-the-kubernetes-cluster"></a>Kubernetes kümesine Miğfer grafiğini yükleme

*Dümen grafiğini* yüklemek için komutu [`helm install`][helm-install-cmd] yürütmemiz gerekir, uygun yol ve dosya adı bağımsız değişkenini `<config-values.yaml>` değiştirmemiz gerekir. Aşağıda `microsoft/cognitive-services-speech-onpremise` başvurulan Helm [grafiği, Microsoft Helm Hub'da][ms-helm-hub-speech-chart]mevcuttur.

```console
helm install onprem-speech microsoft/cognitive-services-speech-onpremise \
    --version 0.1.1 \
    --values <config-values.yaml> 
```

Başarılı bir yükleme yürütmesinden görmeyi bekleyebilecekleriniz bir örnek çıktı aşağıda verilmiştir:

```console
NAME:   onprem-speech
LAST DEPLOYED: Tue Jul  2 12:51:42 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                             READY  STATUS             RESTARTS  AGE
speech-to-text-7664f5f465-87w2d  0/1    Pending            0         0s
speech-to-text-7664f5f465-klbr8  0/1    ContainerCreating  0         0s
text-to-speech-56f8fb685b-4jtzh  0/1    ContainerCreating  0         0s
text-to-speech-56f8fb685b-frwxf  0/1    Pending            0         0s

==> v1/Service
NAME            TYPE          CLUSTER-IP    EXTERNAL-IP  PORT(S)       AGE
speech-to-text  LoadBalancer  10.0.252.106  <pending>    80:31811/TCP  1s
text-to-speech  LoadBalancer  10.0.125.187  <pending>    80:31247/TCP  0s

==> v1beta1/PodDisruptionBudget
NAME                                MIN AVAILABLE  MAX UNAVAILABLE  ALLOWED DISRUPTIONS  AGE
speech-to-text-poddisruptionbudget  N/A            20%              0                    1s
text-to-speech-poddisruptionbudget  N/A            20%              0                    1s

==> v1beta2/Deployment
NAME            READY  UP-TO-DATE  AVAILABLE  AGE
speech-to-text  0/2    2           0          0s
text-to-speech  0/2    2           0          0s

==> v2beta2/HorizontalPodAutoscaler
NAME                       REFERENCE                  TARGETS        MINPODS  MAXPODS  REPLICAS  AGE
speech-to-text-autoscaler  Deployment/speech-to-text  <unknown>/50%  2        10       0         0s
text-to-speech-autoscaler  Deployment/text-to-speech  <unknown>/50%  2        10       0         0s


NOTES:
cognitive-services-speech-onpremise has been installed!
Release is named onprem-speech
```

Kubernetes dağıtımının tamamlanması birkaç dakika sürebilir. Hem bölmelerin hem de hizmetlerin düzgün şekilde dağıtılmış ve kullanılabilir olduğunu doğrulamak için aşağıdaki komutu uygulayın:

```console
kubectl get all
```

Aşağıdaki çıktıya benzer bir şey görmeyi beklemelisiniz:

```console
NAME                                  READY     STATUS    RESTARTS   AGE
pod/speech-to-text-7664f5f465-87w2d   1/1       Running   0          34m
pod/speech-to-text-7664f5f465-klbr8   1/1       Running   0          34m
pod/text-to-speech-56f8fb685b-4jtzh   1/1       Running   0          34m
pod/text-to-speech-56f8fb685b-frwxf   1/1       Running   0          34m

NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)        AGE
service/kubernetes       ClusterIP      10.0.0.1       <none>           443/TCP        3h
service/speech-to-text   LoadBalancer   10.0.252.106   52.162.123.151   80:31811/TCP   34m
service/text-to-speech   LoadBalancer   10.0.125.187   65.52.233.162    80:31247/TCP   34m

NAME                             DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/speech-to-text   2         2         2            2           34m
deployment.apps/text-to-speech   2         2         2            2           34m

NAME                                        DESIRED   CURRENT   READY     AGE
replicaset.apps/speech-to-text-7664f5f465   2         2         2         34m
replicaset.apps/text-to-speech-56f8fb685b   2         2         2         34m

NAME                                                            REFERENCE                   TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
horizontalpodautoscaler.autoscaling/speech-to-text-autoscaler   Deployment/speech-to-text   1%/50%    2         10        2          34m
horizontalpodautoscaler.autoscaling/text-to-speech-autoscaler   Deployment/text-to-speech   0%/50%    2         10        2          34m
```

### <a name="verify-helm-deployment-with-helm-tests"></a>Miğfer dağıtımını Miğfer testleri ile doğrula

Yüklü Miğfer grafikleri, doğrulama için kolaylık sağlayan *Helm testlerini*tanımlar. Bu testler hizmete hazırlık durumunu doğrular. Hem **konuşma-metin** hem de **metinden konuşmaya** hizmetleri doğrulamak için [Miğfer test][helm-test] komutunu uygulayacağız.

```console
helm test onprem-speech
```

> [!IMPORTANT]
> Pod durumu yoksa `Running` veya dağıtım `AVAILABLE` sütunun altında listelenmemişse, bu testler başarısız olur. Bu tamamlamak için on dakikadan fazla sürebilir gibi sabırlı olun.

Bu testler çeşitli durum sonuçları çıktı:

```console
RUNNING: speech-to-text-readiness-test
PASSED: speech-to-text-readiness-test
RUNNING: text-to-speech-readiness-test
PASSED: text-to-speech-readiness-test
```

*Dümen testlerini*yürütmeye alternatif olarak, *External IP* `kubectl get all` harici IP adreslerini ve ilgili bağlantı noktalarını komuttan toplayabilirsiniz. IP ve bağlantı noktasını kullanarak, bir `http://<external-ip>:<port>:/swagger/index.html` web tarayıcısı açın ve API swagger sayfasını (lar) görüntülemek için gidin.

## <a name="customize-helm-charts"></a>Miğfer grafiklerini özelleştir

Miğfer grafikleri hiyerarşiktir. Hiyerarşik grafik devralma için izin verir olmak, aynı zamanda daha özel ayarları devralınan kuralları geçersiz kılma özellik kavramına hitap eder.

[!INCLUDE [Speech umbrella-helm-chart-config](includes/speech-umbrella-helm-chart-config.md)]

[!INCLUDE [Speech-to-Text Helm Chart Config](includes/speech-to-text-chart-config.md)]

[!INCLUDE [Text-to-Speech Helm Chart Config](includes/text-to-speech-chart-config.md)]

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
[helm-install]: https://helm.sh/docs/intro/install/
[helm-install-cmd]: https://helm.sh/docs/intro/using_helm/#helm-install-installing-a-package
[tiller-install]: https://helm.sh/docs/install/#installing-tiller
[helm-charts]: https://helm.sh/docs/topics/charts/
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[helm-test]: https://v2.helm.sh/docs/helm/#helm-test
[ms-helm-hub]: https://hub.helm.sh/charts/microsoft
[ms-helm-hub-speech-chart]: https://hub.helm.sh/charts/microsoft/cognitive-services-speech-onpremise

<!-- LINKS - internal -->
[speech-container-host-computer]: speech-container-howto.md#the-host-computer
[installing-helm-apps-in-aks]: ../../aks/kubernetes-helm.md
[cog-svcs-containers]: ../cognitive-services-container-support.md
