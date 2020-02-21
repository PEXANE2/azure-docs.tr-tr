---
title: Held grafiklerini depola
description: Azure Container Registry içindeki depoları kullanarak Kubernetes uygulamalarınız için hele grafiklerini depolamayı öğrenin
ms.topic: article
ms.date: 01/28/2020
ms.openlocfilehash: 26588bb4dc3cf50656103b50d5d0559908a1ccb7
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77524641"
---
# <a name="push-and-pull-helm-charts-to-an-azure-container-registry"></a>Azure Container Registry 'de Held grafikleri gönderme ve çekme

Kubernetes uygulamalarını hızlıca yönetmek ve dağıtmak için, [Açık kaynaklı HELI paket yöneticisini][helm]kullanabilirsiniz. Held ile uygulama paketleri, bir [Held grafik deposunda](https://helm.sh/docs/topics/chart_repository/)toplanan ve depolanan [grafikler](https://helm.sh/docs/topics/charts/)olarak tanımlanır.

Bu makalede, bir Azure Container Registry 'de Heln 3 veya Held 2 yüklemesi kullanarak hele grafiklerini nasıl barındırabilmeniz gösterilmektedir. Bu örnekte, var olan bir Helu grafiğini genel Helpo *kalıcı* deposundan depoladığınızda. Birçok senaryoda, geliştirdiğiniz uygulamalar için kendi grafiklerinizi derleyip karşıya yüklersiniz. Kendi HELI grafiklerinizi oluşturma hakkında daha fazla bilgi için bkz. [grafik şablonu Geliştirici Kılavuzu][develop-helm-charts].

> [!IMPORTANT]
> Azure Container Registry 'de Held grafikleri için destek şu anda önizlemededir. Önizlemeler, ek [kullanım koşullarını][terms-of-use]kabul ettiğiniz koşulda size sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="helm-3-or-helm-2"></a>Helb 3 veya Held 2?

Held grafiklerini depolamak, yönetmek ve yüklemek için bir hele istemcisi ve Held CLı kullanırsınız. Held istemcisinin ana sürümleri, helm3 ve Held 2 ' dir. Helb 3 yeni bir grafik biçimini destekler ve artık Tiller sunucu tarafı bileşenini yüklemektedir. Sürüm farklılıkları hakkındaki ayrıntılar için bkz. [Sürüm SSS](https://helm.sh/docs/faq/). Daha önce helk 2 grafikleri dağıttıysanız, bkz. [Held v2 'yi v3 'e geçirme](https://helm.sh/docs/topics/v2_v3_migration/).

Her sürüme özel iş akışlarıyla Azure Container Registry, Held grafiklerini barındırmak için hele 3 veya Held 2 kullanabilirsiniz:

* [Hele 3 istemcisi](#use-the-helm-3-client) -kayıt defterinizde grafikleri [OCI yapıtları](container-registry-image-formats.md#oci-artifacts) olarak yönetmek için `helm chart` komutlarını kullanın
* [Helm 2 istemcisi](#use-the-helm-2-client) -kapsayıcı kayıt defterinizi Helm grafik deposu olarak eklemek ve yönetmek IÇIN Azure CLI 'de [az ACR Helm][az-acr-helm] komutlarını kullanın

### <a name="additional-information"></a>Ek bilgiler

* Grafikleri OCı yapıtları olarak yönetmek için yerel `helm chart` komutlarıyla Helm3 iş akışını kullanmanızı öneririz.
* Helm 3 istemcisiyle ve grafikleriyle, eski [az ACR Helm][az-acr-helm] Azure CLI komutlarını ve iş akışını kullanabilirsiniz. Ancak, `az acr helm list` gibi bazı komutlar Held 3 grafikleriyle uyumlu değildir.
* Helm 3 itibariyle, [az ACR Helm][az-acr-helm] komutları genellikle Helm 2 istemcisiyle ve grafik biçimiyle uyumluluk için desteklenir. Bu komutların gelecekte geliştirilmesi planlanmamaktadır.

## <a name="use-the-helm-3-client"></a>Helb 3 istemcisini kullanma

### <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğinizdeki **bir Azure Kapsayıcı kayıt defteri** . Gerekirse, [Azure Portal](container-registry-get-started-portal.md) veya [Azure CLI](container-registry-get-started-azure-cli.md)kullanarak bir kayıt defteri oluşturun.
- **Hele Client Version 3.0.0 veya üzeri** -geçerli sürümünüzü bulmak için `helm version` çalıştırın. Held 'yi yükleme ve yükseltme hakkında daha fazla bilgi için bkz. [Held yükleme][helm-install].
- Helk grafiğini yükleyeceğiniz **bir Kubernetes kümesi** . Gerekirse, bir [Azure Kubernetes hizmet kümesi][aks-quickstart]oluşturun. 
- **Azure CLI sürüm 2.0.71 veya üzeri** -sürümü bulmak için `az --version` çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][azure-cli-install].

### <a name="high-level-workflow"></a>Üst düzey iş akışı

**Helb 3** ile şunları yapabilirsiniz:

* Bir Azure Container Registry 'de bir veya daha fazla Held deposu oluşturabilir
* Helm3 grafiklerini bir kayıt defterinde [OCI yapıtları](container-registry-image-formats.md#oci-artifacts)olarak depolayın. Şu anda, OCı için Held 3 desteği *deneysel*olarak değerlendirilir.
* Bir kayıt defterinde Held grafiklerini göndermek, çekmek ve yönetmek için doğrudan HELI CLı 'dan `helm chart` komutlarını kullanın
* Azure CLı aracılığıyla kayıt defteriyle kimlik doğrulaması yapın ve ardından hele istemcinizi kayıt defteri URI 'SI ve kimlik bilgileriyle otomatik olarak güncelleştirir. Bu kayıt defteri bilgilerini el ile belirtmeniz gerekmez, bu nedenle kimlik bilgileri komut geçmişinde gösterilmez.
* Yerel bir depo önbelleğinden bir Kubernetes kümesine grafik yüklemek için `helm install` kullanın.

Örnekler için aşağıdaki bölümlere bakın.

### <a name="enable-oci-support"></a>OCı desteğini etkinleştir

Hele 3 istemcisinde OCı desteğini etkinleştirmek için aşağıdaki ortam değişkenini ayarlayın. Şu anda bu destek deneysel bir değer. 

```console
export HELM_EXPERIMENTAL_OCI=1
```

### <a name="pull-an-existing-helm-package"></a>Mevcut bir Held paketini çekme

`stable` Held grafik deposunu zaten eklemediyseniz, `helm repo add` komutunu çalıştırın:

```console
helm repo add stable https://kubernetes-charts.storage.googleapis.com
```

`stable` deposundan Yerel olarak bir grafik paketi çekin. Örneğin, *~/ACR-Held*gibi bir yerel dizin oluşturun ve ardından mevcut *kararlı/WordPress* grafik paketini indirin. (Bu örnekte ve bu makaledeki diğer komutlar bash kabuğu için biçimlendirilir.)

```console
mkdir ~/acr-helm && cd ~/acr-helm
helm pull stable/wordpress --untar
```

`helm pull stable/wordpress` komutu belirli bir sürüm belirtmedi, bu nedenle `wordpress` alt dizininde *en son* sürüm çekildi ve sıkıştırılmamış.

### <a name="save-chart-to-local-registry-cache"></a>Grafiği yerel kayıt defteri önbelleğine Kaydet

Dizini, Held grafik dosyalarını içeren `wordpress` alt dizinine değiştirin. Ardından, grafiğin bir kopyasını yerel olarak kaydetmek için `helm chart save` çalıştırın ve ayrıca, kayıt defterinin tam adı ve hedef depo ve etiket ile bir diğer ad oluşturun. 

Aşağıdaki örnekte, kayıt defteri adı *mycontainerregistry*, hedef depo *WordPress*ise ve hedef grafik etiketi *en son*, ancak ortamınız için değer yerine geçecek.

```console
cd wordpress
helm chart save . wordpress:latest
helm chart save . mycontainerregistry.azurecr.io/helm/wordpress:latest
```

Grafikleri yerel kayıt defteri önbelleğine kaydettiğinizden emin olmak için `helm chart list` çalıştırın. Çıkış şuna benzer olacaktır:

```console
REF                                                      NAME            VERSION DIGEST  SIZE            CREATED
wordpress:latest                                         wordpress       8.1.0   5899db0 29.1 KiB        1 day 
mycontainerregistry.azurecr.io/helm/wordpress:latest     wordpress       8.1.0   5899db0 29.1 KiB        1 day 
```

### <a name="push-chart-to-azure-container-registry"></a>Grafiği Azure Container Registry gönder

Hele grafiğini Azure Container Registry 'deki bir depoya göndermek için helm3 CLı 'da `helm chart push` komutunu çalıştırın. Yoksa, depo oluşturulur.

Önce kayıt defterinizde kimlik doğrulaması yapmak için [az ACR oturum açma][az-acr-login] Azure CLI komutunu kullanın:

```azurecli
az acr login --name mycontainerregistry
```

Grafiği tam nitelikli hedef depoya gönder:

```console
helm chart push mycontainerregistry.azurecr.io/helm/wordpress:latest
```

Başarılı bir gönderim sonrasında, çıkış şuna benzerdir:

```console
The push refers to repository [mycontainerregistry.azurecr.io/helm/wordpress]
ref:     mycontainerregistry.azurecr.io/helm/wordpress:latest
digest:  5899db028dcf96aeaabdadfa5899db025899db025899db025899db025899db02
size:    29.1 KiB
name:    wordpress
version: 8.1.0
```

### <a name="list-charts-in-the-repository"></a>Depodaki grafikleri listeleme

Azure Container Registry 'de depolanan görüntülerde olduğu gibi, grafiklerinizi barındıran depoları, grafik etiketlerini ve bildirimleri göstermek için [az ACR Repository][az-acr-repository] komutlarını kullanabilirsiniz. 

Örneğin, önceki adımda oluşturduğunuz deponun özelliklerini görmek için [az ACR Repository Show][az-acr-repository-show] ' ı çalıştırın:

```azurecli
az acr repository show \
  --name mycontainerregistry \
  --repository helm/wordpress
```

Çıkış şuna benzer olacaktır:

```console
{
  "changeableAttributes": {
    "deleteEnabled": true,
    "listEnabled": true,
    "readEnabled": true,
    "writeEnabled": true
  },
  "createdTime": "2020-01-29T16:54:30.1514833Z",
  "imageName": "helm/wordpress",
  "lastUpdateTime": "2020-01-29T16:54:30.4992247Z",
  "manifestCount": 1,
  "registry": "mycontainerregistry.azurecr.io",
  "tagCount": 1
}
```

Depoda depolanan grafiğin ayrıntılarını görmek için [az ACR Repository Show-bildirimleri][az-acr-repository-show-manifests] komutunu çalıştırın. Örneğin:

```azurecli
az acr repository show-manifests \
  --name mycontainerregistry \
  --repository helm/wordpress --detail
```

Bu örnekte kısaltılmış çıkış, `application/vnd.cncf.helm.config.v1+json``configMediaType` gösterir:

```console
[
  {
    [...]
    "configMediaType": "application/vnd.cncf.helm.config.v1+json",
    "createdTime": "2020-01-29T16:54:30.2382436Z",
    "digest": "sha256:xxxxxxxx51bc0807bfa97cb647e493ac381b96c1f18749b7388c24bbxxxxxxxxx",
    "imageSize": 29995,
    "lastUpdateTime": "2020-01-29T16:54:30.3492436Z",
    "mediaType": "application/vnd.oci.image.manifest.v1+json",
    "tags": [
      "latest"
    ]
  }
]
```

### <a name="pull-chart-to-local-cache"></a>Grafiği yerel önbelleğe çekme

Kubernetes 'e bir helk grafiği yüklemek için, grafiğin yerel önbellekte olması gerekir. Bu örnekte, önce `mycontainerregistry.azurecr.io/helm/wordpress:latest`adlı var olan yerel grafiği kaldırmak için `helm chart remove` çalıştırın:

```console
helm chart remove mycontainerregistry.azurecr.io/helm/wordpress:latest
```

Grafiği Azure Container Registry 'den yerel önbelleğinize indirmek için `helm chart pull` çalıştırın:

```console
helm chart pull mycontainerregistry.azurecr.io/helm/wordpress:latest
```

### <a name="export-helm-chart"></a>Held grafiğini dışarı aktar

Grafik ile daha fazla çalışmak için `helm chart export`kullanarak yerel bir dizine dışarı aktarın. Örneğin, çekolduğunuz grafiği `install` dizinine dışarı aktarın:

```console
helm chart export mycontainerregistry.azurecr.io/helm/wordpress:latest --destination ./install
```

Depoda, içe aktarılmış grafik bilgilerini görüntülemek için, grafiği verdiğiniz dizinde `helm inspect chart` komutunu çalıştırın.

```console
cd install
helm inspect chart wordpress
```

Sürüm numarası sağlanmamışsa, *en son* sürüm kullanılır. HELI, aşağıdaki sıkıştırılmış Çıktıda gösterildiği gibi, grafiğiniz hakkında ayrıntılı bilgiler döndürür:

```
apiVersion: v1
appVersion: 5.3.2
dependencies:
- condition: mariadb.enabled
  name: mariadb
  repository: https://kubernetes-charts.storage.googleapis.com/
  tags:
  - wordpress-database
  version: 7.x.x
description: Web publishing platform for building blogs and websites.
home: http://www.wordpress.com/
icon: https://bitnami.com/assets/stacks/wordpress/img/wordpress-stack-220x234.png
keywords:
- wordpress
- cms
- blog
- http
- web
- application
- php
maintainers:
- email: containers@bitnami.com
  name: Bitnami
name: wordpress
sources:
- https://github.com/bitnami/bitnami-docker-wordpress
version: 8.1.0
```

### <a name="install-helm-chart"></a>Held grafiğini yükler

Yerel önbelleğe çekilecek ve dışarıya verdiğiniz hele grafiğini yüklemek için `helm install` çalıştırın. Bir yayın adı belirtin veya `--generate-name` parametresini geçirin. Örneğin:

```console
helm install wordpress --generate-name
```

Yükleme devam ederken, URL 'Leri ve kimlik bilgilerini görmek için komut çıkışında bulunan yönergeleri izleyin. Helk grafiği aracılığıyla dağıtılan Kubernetes kaynaklarını görmek için `kubectl get pods` komutunu da çalıştırabilirsiniz:

```console
NAME                                    READY   STATUS    RESTARTS   AGE
wordpress-1598530621-67c77b6d86-7ldv4   1/1     Running   0          2m48s
wordpress-1598530621-mariadb-0          1/1     Running   0          2m48s
[...]
```

### <a name="delete-a-helm-chart-from-the-repository"></a>Depodaki bir hela grafiğini silme

Bir grafiği depodan silmek için [az ACR Repository Delete][az-acr-repository-delete] komutunu kullanın. Aşağıdaki komutu çalıştırın ve sorulduğunda işlemi onaylayın:

```azurecli
az acr repository delete --name mycontainerregistry --image helm/wordpress:latest
```

## <a name="use-the-helm-2-client"></a>Held 2 istemcisini kullanma

### <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğinizdeki **bir Azure Kapsayıcı kayıt defteri** . Gerekirse, [Azure Portal](container-registry-get-started-portal.md) veya [Azure CLI](container-registry-get-started-azure-cli.md)kullanarak bir kayıt defteri oluşturun.
- **Hele istemci sürümü 2.11.0 (RC sürümü değil) veya daha sonra** , geçerli sürümünüzü bulmak için `helm version` çalıştırın. Ayrıca bir Kubernetes kümesi içinde başlatılan bir Held sunucusuna (Tiller) ihtiyacınız vardır. Gerekirse, bir [Azure Kubernetes hizmet kümesi][aks-quickstart]oluşturun. Held 'yi yükleme ve yükseltme hakkında daha fazla bilgi için bkz. [Held yükleme][helm-install-v2].
- **Azure CLI sürüm 2.0.46 veya üzeri** -sürümü bulmak için `az --version` çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][azure-cli-install].

### <a name="high-level-workflow"></a>Üst düzey iş akışı

**Helb 2** ile şunları yapabilirsiniz:

* Azure Container kayıt defterinizi *tek* bir Held grafik deposu olarak yapılandırın. Azure Container Registry, havuza grafik ekleyip kaldırırken Dizin tanımını yönetir.
* Azure Container kayıt defterinizi Helm grafik deposu olarak eklemek ve grafikleri göndermek ve yönetmek için Azure CLı 'daki [az ACR Helm][az-acr-helm] komutlarını kullanın. Bu Azure CLı komutları, Helm 2 istemci komutlarını sarmalıdır.
* Grafik aramasını desteklemek için, Azure Container Kayıt defterinize grafik deposunu yerel Helmrepo dizinine ekleyin
* Azure CLı ile Azure Container Registry ile kimlik doğrulaması yapın, daha sonra hele istemcinizi kayıt defteri URI 'SI ve kimlik bilgileriyle otomatik olarak güncelleştirir. Bu kayıt defteri bilgilerini el ile belirtmeniz gerekmez, bu nedenle kimlik bilgileri komut geçmişinde gösterilmez.
* Yerel bir depo önbelleğinden bir Kubernetes kümesine grafik yüklemek için `helm install` kullanın.

Örnekler için aşağıdaki bölümlere bakın.

### <a name="add-repository-to-helm-client"></a>Hele istemcisine depo ekleme

[Az ACR Helm depo Add][az-acr-helm-repo-add] komutunu kullanarak, Azure Container Registry Helm grafik deponuzu Helm istemcisine ekleyin. Bu komut, Azure Container kayıt defteriniz için Helm istemcisi tarafından kullanılan bir kimlik doğrulama belirteci alır. Kimlik doğrulama belirteci 3 saat için geçerlidir. `docker login`benzer şekilde, Azure Container Registry Helu grafik deponuzla hele istemcinizi kimlik doğrulaması yapmak için bu komutu gelecekteki CLı oturumlarında çalıştırabilirsiniz:

```azurecli
az acr helm repo add --name mycontainerregistry
```

### <a name="add-a-chart-to-the-repository"></a>Depoya bir grafik ekleyin

İlk olarak, *~/ACR-Held*konumunda bir yerel dizin oluşturun ve ardından mevcut *kararlı/WordPress* grafiğini indirin:

```console
mkdir ~/acr-helm && cd ~/acr-helm
helm repo update
helm fetch stable/wordpress
```

İndirilen grafiği listelemek için `ls` yazın ve dosya adına dahil olan WordPress sürümünü aklınızda yapın. `helm fetch stable/wordpress` komutu belirli bir sürüm belirtmedi, bu nedenle *en son* sürüm getirildi. Aşağıdaki örnek çıktıda, WordPress grafiğinin sürümü *8.1.0*olur:

```
wordpress-8.1.0.tgz
```

Azure CLı 'de [az ACR Helm Push][az-acr-helm-push] komutunu kullanarak Azure Container Registry grafikleri Helm grafik deponuza gönderin. Önceki adımda indirilen HELI grafiğinin adını belirtin, örneğin *WordPress-8.1.0. tgz*:

```azurecli
az acr helm push --name mycontainerregistry wordpress-8.1.0.tgz
```

Birkaç dakika sonra Azure CLı, aşağıdaki örnek çıktıda gösterildiği gibi, grafiğinizin kaydedildiğini bildirir:

```
{
  "saved": true
}
```

### <a name="list-charts-in-the-repository"></a>Depodaki grafikleri listeleme

Önceki adımda karşıya yüklenen grafiği kullanmak için yerel Held depo dizininin güncellenmesi gerekir. Hele istemcisinde depoları yeniden düzenleyebilir veya depo dizinini güncelleştirmek için Azure CLı kullanabilirsiniz. Deponuza bir grafik eklediğiniz her sefer, bu adımın tamamlanması gerekir:

```azurecli
az acr helm repo add --name mycontainerregistry
```

Deponuzda depolanan ve güncelleştirilmiş Dizin yerel olarak bulunan bir grafik ile, aramak veya yüklemek için normal hele istemci komutlarını kullanabilirsiniz. Deponuzdaki tüm grafikleri görmek için, kendi Azure Container Registry adınızı sağlamak üzere `helm search` komutunu kullanın:

```console
helm search mycontainerregistry
```

Önceki adımda gönderilen WordPress grafiği, aşağıdaki örnek çıktıda gösterildiği gibi listelenir:

```
NAME                CHART VERSION   APP VERSION DESCRIPTION
helmdocs/wordpress  8.1.0           5.3.2       Web publishing platform for building blogs and websites.
```

Grafikleri, [az ACR Helm List][az-acr-helm-list]kullanarak Azure CLI ile de listeleyebilirsiniz:

```azurecli
az acr helm list --name mycontainerregistry
```

### <a name="show-information-for-a-helm-chart"></a>Hela grafiğinin bilgilerini göster

Depodaki belirli bir grafiğin bilgilerini görüntülemek için, `helm inspect` komutunu kullanabilirsiniz.

```console
helm inspect mycontainerregistry/wordpress
```

Sürüm numarası sağlanmamışsa, *en son* sürüm kullanılır. HELI, aşağıdaki sıkıştırılmış örnek çıktıda gösterildiği gibi, grafiğiniz hakkında ayrıntılı bilgiler döndürür:

```
apiVersion: v1
appVersion: 5.3.2
description: Web publishing platform for building blogs and websites.
engine: gotpl
home: http://www.wordpress.com/
icon: https://bitnami.com/assets/stacks/wordpress/img/wordpress-stack-220x234.png
keywords:
- wordpress
- cms
- blog
- http
- web
- application
- php
maintainers:
- email: containers@bitnami.com
  name: Bitnami
name: wordpress
sources:
- https://github.com/bitnami/bitnami-docker-wordpress
version: 8.1.0
[...]
```

Ayrıca, Azure CLı [az ACR Helm Show][az-acr-helm-show] komutuyla bir grafik için bilgileri gösterebilirsiniz. Yine, bir grafiğin *en son* sürümü varsayılan olarak döndürülür. Grafiğin belirli bir sürümünü listelemek için `--version` ekleyebilirsiniz, örneğin, *8.1.0*:

```azurecli
az acr helm show --name mycontainerregistry wordpress
```

### <a name="install-a-helm-chart-from-the-repository"></a>Depodan bir Held grafiği yükler

Deponuzdaki Held grafiği, Depo adı ve grafik adı belirtilerek yüklenir. WordPress grafiğini yüklemek için hele istemcisini kullanın:

```console
helm install mycontainerregistry/wordpress
```

> [!TIP]
> Azure Container Registry hele grafik deponuza gönderim yaparsanız ve daha sonra yeni bir CLı oturumunda geri dönerseniz, yerel hele istemciniz güncelleştirilmiş bir kimlik doğrulama belirtecine ihtiyaç duyuyor. Yeni bir kimlik doğrulama belirteci almak için [az ACR Helm depo Add][az-acr-helm-repo-add] komutunu kullanın.

Aşağıdaki adımlar, yüklemesi işlemi sırasında tamamlanır:

- Hela istemcisi yerel depo dizinini arar.
- Karşılık gelen grafik Azure Container Registry deposundan indirilir.
- Grafik, Kubernetes kümenizdeki Tiller kullanılarak dağıtılır.

Yükleme devam ederken, URL 'Leri ve kimlik bilgilerini görmek için komut çıkışında bulunan yönergeleri izleyin. Helk grafiği aracılığıyla dağıtılan Kubernetes kaynaklarını görmek için `kubectl get pods` komutunu da çalıştırabilirsiniz:

```
NAME                                    READY   STATUS    RESTARTS   AGE
wordpress-1598530621-67c77b6d86-7ldv4   1/1     Running   0          2m48s
wordpress-1598530621-mariadb-0          1/1     Running   0          2m48s
[...]
```

### <a name="delete-a-helm-chart-from-the-repository"></a>Depodaki bir hela grafiğini silme

Bir grafiği depodan silmek için [az ACR Helm Delete][az-acr-helm-delete] komutunu kullanın. *WordPress*gibi grafiğin adını ve silinecek sürümü (örneğin, *8.1.0*) belirtin.

```azurecli
az acr helm delete --name mycontainerregistry wordpress --version 8.1.0
```

Adlandırılmış grafiğin tüm sürümlerini silmek isterseniz, `--version` parametresini bırakın.

`helm search`çalıştırdığınızda grafik döndürülmeye devam eder. Yine, HELI istemcisi bir depodaki kullanılabilir grafiklerin listesini otomatik olarak güncelleştirmez. Helm istemci deposu dizinini güncelleştirmek için [az ACR Helm depo Add][az-acr-helm-repo-add] komutunu tekrar kullanın:

```azurecli
az acr helm repo add --name mycontainerregistry
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, genel *kararlı* depodan mevcut bir hela grafiği kullanıldı. HELI grafikleri oluşturma ve dağıtma hakkında daha fazla bilgi için bkz. [HELI grafikleri geliştirme][develop-helm-charts].

Held grafikleri kapsayıcı oluşturma sürecinin bir parçası olarak kullanılabilir. Daha fazla bilgi için bkz. [Azure Container Registry görevleri kullanma][acr-tasks].

<!-- LINKS - external -->
[helm]: https://helm.sh/
[helm-install]: https://helm.sh/docs/intro/install/
[helm-install-v2]: https://v2.helm.sh/docs/using_helm/#installing-helm
[develop-helm-charts]: https://helm.sh/docs/chart_template_guide/
[semver2]: https://semver.org/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-quickstart]: ../aks/kubernetes-walkthrough.md
[acr-bestpractices]: container-registry-best-practices.md
[az-configure]: /cli/azure/reference-index#az-configure
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-helm]: /cli/azure/acr/helm
[az-acr-repository]: /cli/azure/acr/repository
[az-acr-repository-show]: /cli/azure/acr/repository#az-acr-repository-show
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
[az-acr-repository-show-tags]: /cli/azure/acr/repository#az-acr-repository-show-tags
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests
[az-acr-helm-repo-add]: /cli/azure/acr/helm/repo#az-acr-helm-repo-add
[az-acr-helm-push]: /cli/azure/acr/helm#az-acr-helm-push
[az-acr-helm-list]: /cli/azure/acr/helm#az-acr-helm-list
[az-acr-helm-show]: /cli/azure/acr/helm#az-acr-helm-show
[az-acr-helm-delete]: /cli/azure/acr/helm#az-acr-helm-delete
[acr-tasks]: container-registry-tasks-overview.md
