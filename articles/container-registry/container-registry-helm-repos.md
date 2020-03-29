---
title: Mağaza Miğferi grafikleri
description: Azure Konteyner Kayıt Defteri'ndeki depoları kullanarak Kubernetes uygulamalarınız için Miğfer grafiklerini nasıl depolayacağız öğrenin
ms.topic: article
ms.date: 03/20/2020
ms.openlocfilehash: 6304486ac493e235ed74f26ab4be4f843ef52513
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131475"
---
# <a name="push-and-pull-helm-charts-to-an-azure-container-registry"></a>Miğfer grafiklerini Azure kapsayıcı kayıt defterine itme ve çekme

Kubernetes için uygulamaları hızlı bir şekilde yönetmek ve dağıtmak için [açık kaynak lı Helm paket yöneticisini][helm]kullanabilirsiniz. Miğfer ile uygulama paketleri, Helm [grafik deposunda](https://helm.sh/docs/topics/chart_repository/)toplanan ve depolanan [grafikler](https://helm.sh/docs/topics/charts/)olarak tanımlanır.

Bu makalede, Miğfer 3 veya Miğfer 2 yüklemesini kullanarak Miğfer grafiklerini bir Azure kapsayıcı kayıt defterinde nasıl barındırabileceğinizi gösterir. Birçok senaryoda, geliştirdiğiniz uygulamalar için kendi grafiklerinizi oluşturur ve yüklersiniz. Kendi Miğfer grafiklerinizi nasıl oluşturabildiğiniz hakkında daha fazla bilgi için [Grafik Şablonu Geliştirici Kılavuzu'na][develop-helm-charts]bakın. Ayrıca, varolan bir Miğfer grafiğini başka bir Miğfer reposundan da depolayabilirsiniz.

> [!IMPORTANT]
> Azure Kapsayıcı Kayıt Defteri'ndeki Miğfer grafikleri için destek şu anda önizlemede. Önizlemeler, ek [kullanım koşullarını][terms-of-use]kabul etmeniz koşuluyla kullanımınıza sunulmuştur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="helm-3-or-helm-2"></a>Mi, Miğfer 3 mü, Miğfer 2 mi?

Miğfer grafiklerini depolamak, yönetmek ve yüklemek için bir Helm istemcisi ve Helm CLI kullanırsınız. Helm istemcisinin önemli sürümleri Helm 3 ve Helm 2'dir. Helm 3 yeni bir grafik biçimini destekler ve artık Tiller sunucu tarafı bileşenini yüklemez. Sürüm farklılıkları hakkında ayrıntılı bilgi için [SSS sürümüne](https://helm.sh/docs/faq/)bakın. Daha önce Helm 2 grafiklerini dağıttıysanız, [bkz.](https://helm.sh/docs/topics/v2_v3_migration/)

Her sürüme özgü iş akışlarıyla Birlikte Azure Kapsayıcı Kayıt Defteri'nde Miğfer grafiklerini barındırmak için Helm 3 veya Helm 2'yi kullanabilirsiniz:

* [Helm 3 istemcisi](#use-the-helm-3-client) - Kayıt defterinizdeki grafikleri OCI yapıları olarak yönetmek için Helm CLI'deki `helm chart` [komutları](container-registry-image-formats.md#oci-artifacts) kullanın
* [Helm 2 istemcisi](#use-the-helm-2-client) - konteyner kayıt defterinizi Miğfer grafiği deposu olarak eklemek ve yönetmek için Azure CLI'deki [az acr dümen][az-acr-helm] komutlarını kullanın

### <a name="additional-information"></a>Ek bilgiler

* Çoğu senaryoda, grafikleri OCI yapıları olarak `helm chart` yönetmek için yerel komutlarla birlikte Miğfer 3 iş akışını kullanmanızı öneririz.
* Helm 3 istemcisi ve grafikleri ile eski [az acr dümenAzure][az-acr-helm] CLI komutlarını ve iş akışını kullanabilirsiniz. Ancak, helm 3 `az acr helm list` grafikleri ile uyumlu değildir gibi bazı komutlar.
* Helm 3 itibariyle, [az acr dümen][az-acr-helm] komutları esas olarak Helm 2 istemcisi ve grafik biçimi ile uyumluluk için desteklenir. Bu komutların gelecekteki gelişimi şu anda planlanmamakta.

## <a name="use-the-helm-3-client"></a>Helm 3 istemcisini kullanma

### <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğinizde **bir Azure kapsayıcı kayıt defteri.** Gerekirse, [Azure portalını](container-registry-get-started-portal.md) veya [Azure CLI'yi](container-registry-get-started-azure-cli.md)kullanarak bir kayıt defteri oluşturun.
- **Helm istemci sürümü 3.1.0** `helm version` veya sonraki - Geçerli sürümünüzü bulmak için çalıştırın. Helm'in nasıl yüklenir ve yükseltilir hakkında daha fazla bilgi için Yükleme [Miğferi'ne][helm-install]bakın.
- Bir Miğfer grafiği yükleyeceğiniz bir **Kubernetes kümesi.** Gerekirse, bir [Azure Kubernetes Hizmet kümesi][aks-quickstart]oluşturun. 
- **Azure CLI sürüm 2.0.71** `az --version` veya sonrası - Sürümü bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

### <a name="high-level-workflow"></a>Üst düzey iş akışı

**Helm 3** ile:

* Azure kapsayıcı kayıt defterinde bir veya daha fazla Miğfer deposu oluşturabilir
* [OCI yapıları](container-registry-image-formats.md#oci-artifacts)olarak bir kayıt defterinde Helm 3 grafikleri polar. Şu anda, OCI için Helm 3 desteği *deneysel*.
* Komutu kullanarak kayıt defteriniz `helm registry login` ile kimlik doğrulaması.
* Kayıt `helm chart` defterindeki Miğfer grafiklerini itmek, çekmek ve yönetmek için Helm CLI'deki komutları kullanın
* Yerel `helm install` bir depo önbelleğinden grafikleri Bir Kubernetes kümesine yüklemek için kullanın.

Örnekler için aşağıdaki bölümlere bakın.

### <a name="enable-oci-support"></a>OCI desteğini etkinleştirme

Helm 3 istemcisinde OCI desteğini etkinleştirmek için aşağıdaki ortam değişkenini ayarlayın. Şu anda, bu destek deneysel. 

```console
export HELM_EXPERIMENTAL_OCI=1
```

### <a name="create-a-sample-chart"></a>Örnek grafik oluşturma

Aşağıdaki komutları kullanarak bir test grafiği oluşturun:

```console
mkdir helmtest

cd helmtest
helm create hello-world
```

Temel bir örnek olarak, dizin klasörünü `templates` değiştirin ve ilk olarak buradaki içeriği silin:

```console
rm -rf *
```

Klasörde, `templates` aşağıdaki `configmap.yaml` içerikleri içeren bir dosya oluşturun:

```yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: hello-world-configmap
data:
  myvalue: "Hello World"
```

Bu örneği oluşturma ve çalıştırma hakkında daha fazla şey için Miğfer Dokümanlarında [Başlarken'e](https://helm.sh/docs/chart_template_guide/getting_started/) bakın.

### <a name="save-chart-to-local-registry-cache"></a>Grafiği yerel kayıt defteri önbelleğine kaydetme

Dizini `hello-world` alt dizini olarak değiştirin. Ardından, `helm chart save` grafiğin bir kopyasını yerel olarak kaydetmek için çalıştırın ve ayrıca kayıt defterinin (tüm küçük harf) tam nitelikli adı ve hedef deposu ve etiketiiçeren bir takma ad oluşturun. 

Aşağıdaki örnekte, kayıt defteri adı *mycontainerregistry*, hedef repo *hello-world*, ve hedef grafik etiketi *v1*, ancak ortamıiçin yedek değerler:

```console
cd ..
helm chart save . hello-world:v1
helm chart save . mycontainerregistry.azurecr.io/helm/hello-world:v1
```

Yerel `helm chart list` kayıt defteri önbelleğindeki grafikleri kaydettiğinizi doğrulamak için çalıştırın. Çıkış şuna benzer olacaktır:

```console
REF                                                      NAME            VERSION DIGEST  SIZE            CREATED
hello-world:v1                                           hello-world       0.1.0   5899db0 3.2 KiB        2 minutes 
mycontainerregistry.azurecr.io/helm/hello-world:v1       hello-world       0.1.0   5899db0 3.2 KiB        2 minutes
```

### <a name="authenticate-with-the-registry"></a>Kayıt defteriyle kimlik doğrulaması

Senaryonuza `helm registry login` uygun kimlik bilgilerini kullanarak [kayıt defteriyle kimlik doğrulaması](container-registry-authentication.md) yapmak için Helm 3 CLI'deki komutu çalıştırın.

Örneğin, kayıt defterine [çekme ve itme izinleri](container-registry-auth-service-principal.md#create-a-service-principal) (AcrPush rolü) içeren bir Azure Etkin Dizin hizmet ilkesi oluşturun. Daha sonra hizmet temel `helm registry login`kimlik bilgilerini . Aşağıdaki örnekte bir ortam değişkeni kullanarak parola sağlar:

```console
echo $spPassword | helm registry login mycontainerregistry.azurecr.io \
  --username <service-principal-id> \
  --password-stdin
```

### <a name="push-chart-to-azure-container-registry"></a>Grafiği Azure Kapsayıcı Kayıt Defteri'ne itme

Grafiği `helm chart push` tam nitelikli hedef deposuna itmek için Helm 3 CLI'deki komutu çalıştırın:

```console
helm chart push mycontainerregistry.azurecr.io/helm/hello-world:v1
```

Başarılı bir itme den sonra, çıktı benzer:

```output
The push refers to repository [mycontainerregistry.azurecr.io/helm/hello-world]
ref:     mycontainerregistry.azurecr.io/helm/hello-world:v1
digest:  5899db028dcf96aeaabdadfa5899db025899db025899db025899db025899db02
size:    3.2 KiB
name:    hello-world
version: 0.1.0
```

### <a name="list-charts-in-the-repository"></a>Depodaki liste listeleri

Azure kapsayıcı kayıt defterinde depolanan görüntülerde olduğu gibi, grafiklerinizi barındıran depoları ve grafik etiketlerini ve bildirimlerini göstermek için [az acr deposu][az-acr-repository] komutlarını kullanabilirsiniz. 

Örneğin, önceki adımda oluşturduğunuz repo özelliklerini görmek için [az acr deposu gösterisini][az-acr-repository-show] çalıştırın:

```azurecli
az acr repository show \
  --name mycontainerregistry \
  --repository helm/hello-world
```

Çıkış şuna benzer olacaktır:

```output
{
  "changeableAttributes": {
    "deleteEnabled": true,
    "listEnabled": true,
    "readEnabled": true,
    "writeEnabled": true
  },
  "createdTime": "2020-03-20T18:11:37.6701689Z",
  "imageName": "helm/hello-world",
  "lastUpdateTime": "2020-03-20T18:11:37.7637082Z",
  "manifestCount": 1,
  "registry": "mycontainerregistry.azurecr.io",
  "tagCount": 1
}
```

Depoda depolanan grafiğin ayrıntılarını görmek için [az acr deposu show-manifests][az-acr-repository-show-manifests] komutunu çalıştırın. Örnek:

```azurecli
az acr repository show-manifests \
  --name mycontainerregistry \
  --repository helm/hello-world --detail
```

Bu örnekte kısaltılmış çıktı, `configMediaType` `application/vnd.cncf.helm.config.v1+json`bir gösterir:

```output
[
  {
    [...]
    "configMediaType": "application/vnd.cncf.helm.config.v1+json",
    "createdTime": "2020-03-20T18:11:37.7167893Z",
    "digest": "sha256:0c03b71c225c3ddff53660258ea16ca7412b53b1f6811bf769d8c85a1f0663ee",
    "imageSize": 3301,
    "lastUpdateTime": "2020-03-20T18:11:37.7167893Z",
    "mediaType": "application/vnd.oci.image.manifest.v1+json",
    "tags": [
      "v1"
    ]
```

### <a name="pull-chart-to-local-cache"></a>Grafiği yerel önbelleğe çekme

Kubernetes'e Bir Miğfer grafiği yüklemek için grafiğin yerel önbellekte olması gerekir. Bu örnekte, `helm chart remove` aşağıdakiadlı `mycontainerregistry.azurecr.io/helm/hello-world:v1`varolan yerel grafiği kaldırmak için ilk çalıştırma:

```console
helm chart remove mycontainerregistry.azurecr.io/helm/hello-world:v1
```

Grafiği `helm chart pull` Azure kapsayıcı kayıt defterinden yerel önbelleğinize indirmek için çalıştırın:

```console
helm chart pull mycontainerregistry.azurecr.io/helm/hello-world:v1
```

### <a name="export-helm-chart"></a>Dışa Aktarma Dümeni grafiği

Grafikle daha fazla çalışmak için, kullanarak yerel `helm chart export`bir dizine dışa aktarın. Örneğin, çektiğiniz grafiği dizine `install` dışa aktarın:

```console
helm chart export mycontainerregistry.azurecr.io/helm/hello-world:v1 \
  --destination ./install
```

Repo'da dışa aktarılan grafiğin bilgilerini `helm show chart` görüntülemek için, grafiği dışa aktardığınız dizinde komutu çalıştırın.

```console
cd install
helm show chart hello-world
```

Helm, aşağıdaki örnek çıktıda gösterildiği gibi grafiğinizin en son sürümü hakkında ayrıntılı bilgi verir:

```output
apiVersion: v2
appVersion: 1.16.0
description: A Helm chart for Kubernetes
name: hello-world
type: application
version: 0.1.0    
```

### <a name="install-helm-chart"></a>Miğfer grafiğini yükle

Yerel `helm install` önbelleğe çektiğiniz ve dışa aktarılan Miğfer grafiğini yüklemek için çalıştırın. *Myhelmtest*gibi bir sürüm adı belirtin veya parametreyi geçirin. `--generate-name` Örnek:

```console
helm install myhelmtest ./hello-world
```

Başarılı grafik yüklemeden sonra çıktı benzer:

```console
NAME: myhelmtest
LAST DEPLOYED: Fri Mar 20 14:14:42 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

Yüklemeyi doğrulamak için `helm get manifest` komutu çalıştırın. Komut, şablon dosyanızdaki `configmap.yaml` YAML verilerini döndürür.

Kümenizdeki grafik yayınını kaldırmak için çalıştırın: `helm uninstall`

```console
helm uninstall myhelmtest
```

### <a name="delete-a-helm-chart-from-the-repository"></a>Depodan Bir Miğfer grafiğini silme

Depodaki grafiği silmek için [az acr deposu silme][az-acr-repository-delete] komutunu kullanın. Aşağıdaki komutu çalıştırın ve istendiğinde işlemi onaylayın:

```azurecli
az acr repository delete --name mycontainerregistry --image helm/hello-world:v1
```

## <a name="use-the-helm-2-client"></a>Helm 2 istemcisini kullanma

### <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğinizde **bir Azure kapsayıcı kayıt defteri.** Gerekirse, [Azure portalını](container-registry-get-started-portal.md) veya [Azure CLI'yi](container-registry-get-started-azure-cli.md)kullanarak bir kayıt defteri oluşturun.
- **Helm istemci sürümü 2.11.0 (RC sürümü değil) veya daha sonra** - Geçerli sürümünü bulmak için çalıştırın. `helm version` Ayrıca bir Kubernetes kümesi içinde başharfe bir Helm sunucusu (Tiller) gerekir. Gerekirse, bir [Azure Kubernetes Hizmet kümesi][aks-quickstart]oluşturun. Helm'in nasıl yüklenir ve yükseltilir hakkında daha fazla bilgi için Yükleme [Miğferi'ne][helm-install-v2]bakın.
- **Azure CLI sürüm 2.0.46** `az --version` veya sonrası - Sürümü bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

### <a name="high-level-workflow"></a>Üst düzey iş akışı

**Helm 2** ile:

* Azure kapsayıcı kayıt defterinizi *tek* bir Miğfer grafiği deposu olarak yapılandırın. Azure Kapsayıcı Kayıt Defteri, grafikleri depoya ekler ve kaldırırken dizin tanımını yönetir.
* Azure kapsayıcı kayıt defterinizde azure kapsayıcı kayıt defterinizde kimlik doğrulaması yapın ve bu da Miğfer istemcinizi kayıt şirketi URI ve kimlik bilgileriyle otomatik olarak güncelleyin. Kimlik bilgileri komut geçmişinde açıkolmaması için bu kayıt defteri bilgilerini el ile belirtmeniz gerekmez.
* Azure CLI'deki [az acr komutlarını][az-acr-helm] kullanarak Azure kapsayıcı kayıt defterinizi Miğfer grafiği deposu olarak ekleyin ve grafikleri itin ve yönetin. Bu Azure CLI komutları Helm 2 istemci komutlarını kaydırın.
* Azure kapsayıcı kayıt defterinizdeki grafik deposunu yerel Miğfer repo dizininize ekleyerek grafik aramasını destekleyin.
* Yerel `helm install` bir depo önbelleğinden grafikleri Bir Kubernetes kümesine yüklemek için kullanın.

Örnekler için aşağıdaki bölümlere bakın.

### <a name="add-repository-to-helm-client"></a>Helm istemcisine depo ekleme

[Az acr dümen repo ekleme][az-acr-helm-repo-add] komutunu kullanarak Miğfer istemcinize Azure Konteyner Kayıt Defteri Dümeni grafiği nizi ekleyin. Bu komut, Helm istemcisi tarafından kullanılan Azure kapsayıcı kayıt defteriniz için bir kimlik doğrulama belirteci alır. Kimlik doğrulama belirteci 3 saat boyunca geçerlidir. Benzer `docker login`şekilde, Bu komutu gelecekteki CLI oturumlarında, Miğfer istemcinizi Azure Kapsayıcı Kayıt Defteri Miğferi grafik deponuzla doğrulamak için çalıştırabilirsiniz:

```azurecli
az acr helm repo add --name mycontainerregistry
```

### <a name="add-a-sample-chart-to-the-repository"></a>Depoya örnek grafik ekleme

İlk olarak, *~/acr-helm'de*yerel bir dizin oluşturun, ardından varolan *kararlı/wordpress* grafiğini indirin:

```console
mkdir ~/acr-helm && cd ~/acr-helm
helm repo update
helm fetch stable/wordpress
```

İndirilen grafiği listelemek için yazın `ls` ve dosya adına dahil edilen Wordpress sürümünü not edin. Komut `helm fetch stable/wordpress` belirli bir sürümü belirtmedi, bu nedenle *en son* sürüm getirilmiştir. Aşağıdaki örnek çıktı, Wordpress grafik sürüm *8.1.0:*

```output
wordpress-8.1.0.tgz
```

Azure CLI'deki [az acr komutunu][az-acr-helm-push] kullanarak grafiği Azure Konteyner Kayıt Defteri'ndeki Miğfer grafiği deponuza taşıyın. *Wordpress-8.1.0.tgz*gibi önceki adımda indirilen Helm grafiğinizin adını belirtin:

```azurecli
az acr helm push --name mycontainerregistry wordpress-8.1.0.tgz
```

Birkaç dakika sonra Azure CLI, aşağıdaki örnek çıktıda gösterildiği gibi grafiğinizin kayda kaydolduğunu bildirir:

```output
{
  "saved": true
}
```

### <a name="list-charts-in-the-repository"></a>Depodaki liste listeleri

Önceki adımda yüklenen grafiği kullanmak için yerel Miğfer deposu dizininin güncellenmesi gerekir. Miğfer istemcisindeki depoları yeniden dizine ekleyebilir veya depo dizini güncelleştirmek için Azure CLI'yi kullanabilirsiniz. Deponuza her grafik eklediğinizde, bu adımın tamamlanması gerekir:

```azurecli
az acr helm repo add --name mycontainerregistry
```

Deponuzda depolanan bir grafik ve yerel olarak kullanılabilen güncelleştirilmiş dizinle, aramak veya yüklemek için normal Helm istemci komutlarını kullanabilirsiniz. Deponuzdaki tüm grafikleri görmek için kendi `helm search` Azure Kapsayıcı Kayıt Defteri adınızı sağlayarak komutu kullanın:

```console
helm search mycontainerregistry
```

Önceki adımda itilen Wordpress grafiği, aşağıdaki örnek çıktıda gösterildiği gibi listelenir:

```output
NAME                  CHART VERSION    APP VERSION    DESCRIPTION
helmdocs/wordpress    8.1.0           5.3.2          Web publishing platform for building blogs and websites.
```

Ayrıca [az acr dümen listesini][az-acr-helm-list]kullanarak, Azure CLI ile grafikleri listeleyebilirsiniz:

```azurecli
az acr helm list --name mycontainerregistry
```

### <a name="show-information-for-a-helm-chart"></a>Miğfer grafiği için bilgileri göster

Repo'daki belirli bir grafiğin bilgilerini görüntülemek `helm inspect` için komutu kullanabilirsiniz.

```console
helm inspect mycontainerregistry/wordpress
```

Sürüm numarası sağlanmadığında, *en son* sürüm kullanılır. Helm, aşağıdaki yoğunlaştırılmış örnek çıktıda gösterildiği gibi grafiğinizle ilgili ayrıntılı bilgileri verir:

```output
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

Azure CLI [az acr helm show][az-acr-helm-show] komutuyla bir grafiğin bilgilerini de gösterebilirsiniz. Yine, grafiğin *en son* sürümü varsayılan olarak döndürülür. Bir `--version` grafiğin *8.1.0*gibi belirli bir sürümünü listeleyebilirsiniz:

```azurecli
az acr helm show --name mycontainerregistry wordpress
```

### <a name="install-a-helm-chart-from-the-repository"></a>Depodan Bir Miğfer grafiği yükleme

Deponuzdaki Miğfer grafiği, depo adı ve grafik adı belirtilerek yüklenir. Wordpress grafiğini yüklemek için Helm istemcisini kullanın:

```console
helm install mycontainerregistry/wordpress
```

> [!TIP]
> Azure Kapsayıcı Kayıt Defteri Dümeni grafiği deponuza basar ve daha sonra yeni bir CLI oturumunda geri dönerseniz, yerel Helm istemcinizin güncelleştirilmiş bir kimlik doğrulama belirteci gerekir. Yeni bir kimlik doğrulama belirteci elde etmek için [az acr dümen repo ekleme][az-acr-helm-repo-add] komutunu kullanın.

Yükleme işlemi sırasında aşağıdaki adımlar tamamlanır:

- Helm istemcisi yerel depo dizini arar.
- İlgili grafik Azure Kapsayıcı Kayıt Defteri deposundan indirilir.
- Grafik, Kubernetes kümenizdeki Çapa Makinesi kullanılarak dağıtılır.

Yükleme ilerledikçe, WorPress URL'lerini ve kimlik bilgilerini görmek için komut çıkışındaki yönergeleri izleyin. Helm grafiğinde `kubectl get pods` dağıtılan Kubernetes kaynaklarını görmek için komutu da çalıştırabilirsiniz:

```output
NAME                                    READY   STATUS    RESTARTS   AGE
wordpress-1598530621-67c77b6d86-7ldv4   1/1     Running   0          2m48s
wordpress-1598530621-mariadb-0          1/1     Running   0          2m48s
[...]
```

### <a name="delete-a-helm-chart-from-the-repository"></a>Depodan Bir Miğfer grafiğini silme

Depodaki grafiği silmek için [az acr helm delete][az-acr-helm-delete] komutunu kullanın. *Wordpress*gibi grafiğin adını ve *8.1.0*gibi silmek için sürümü belirtin.

```azurecli
az acr helm delete --name mycontainerregistry wordpress --version 8.1.0
```

Adlandırılmış grafiğin tüm sürümlerini silmek isterseniz, parametreyi `--version` dışarıda bırakın.

Grafik çalıştırdığınızda `helm search`döndürülmeye devam ediyor. Yine, Helm istemcisi bir depodaki kullanılabilir grafiklerin listesini otomatik olarak güncelleştirmez. Helm istemcirepo dizini güncelleştirmek [için, az acr dümen repo yeniden komutu ekleyin][az-acr-helm-repo-add] kullanın:

```azurecli
az acr helm repo add --name mycontainerregistry
```

## <a name="next-steps"></a>Sonraki adımlar

* Miğfer grafiklerini oluşturma ve dağıtma hakkında daha fazla bilgi için [Miğfer grafiklerini geliştirme'ye][develop-helm-charts]bakın.
* [Azure Kubernetes Hizmetinde (AKS)](../aks/kubernetes-helm.md)Helm ile uygulama yükleme hakkında daha fazla bilgi edinin.
* Miğfer grafikleri kapsayıcı oluşturma işleminin bir parçası olarak kullanılabilir. Daha fazla bilgi için [bkz.][acr-tasks]

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
