---
title: Held grafiklerini depola
description: Azure Container Registry içindeki depoları kullanarak Kubernetes uygulamalarınız için hele grafiklerini depolamayı öğrenin
ms.topic: article
ms.date: 03/20/2020
ms.openlocfilehash: 3f1a68258b758380a66b63e3c3137f1d460d288c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81399385"
---
# <a name="push-and-pull-helm-charts-to-an-azure-container-registry"></a>Azure Container Registry 'de Held grafikleri gönderme ve çekme

Kubernetes uygulamalarını hızlıca yönetmek ve dağıtmak için, [Açık kaynaklı HELI paket yöneticisini][helm]kullanabilirsiniz. Held ile uygulama paketleri, bir [Held grafik deposunda](https://helm.sh/docs/topics/chart_repository/)toplanan ve depolanan [grafikler](https://helm.sh/docs/topics/charts/)olarak tanımlanır.

Bu makalede, bir Azure Container Registry 'de hele 3 veya Held 2 yüklemesi kullanılarak HELI grafik depolarının nasıl barındırılacağını gösterilmektedir. Birçok senaryoda, geliştirdiğiniz uygulamalar için kendi grafiklerinizi derleyip karşıya yüklersiniz. Kendi HELI grafiklerinizi oluşturma hakkında daha fazla bilgi için bkz. [grafik şablonu Geliştirici Kılavuzu][develop-helm-charts]. Ayrıca, başka bir Held deposundan var olan bir Helu grafiğini de saklayabilirsiniz.

> [!IMPORTANT]
> Azure Container Registry 'de Held grafikleri için destek şu anda önizlemededir. Önizlemeler, ek [kullanım koşullarını][terms-of-use]kabul ettiğiniz koşulda size sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="helm-3-or-helm-2"></a>Helb 3 veya Held 2?

Held grafiklerini depolamak, yönetmek ve yüklemek için bir hele istemcisi ve Held CLı kullanırsınız. Held istemcisinin ana sürümleri, helm3 ve Held 2 ' dir. Helb 3 yeni bir grafik biçimini destekler ve artık Tiller sunucu tarafı bileşenini yüklemektedir. Sürüm farklılıkları hakkındaki ayrıntılar için bkz. [Sürüm SSS](https://helm.sh/docs/faq/). Daha önce helk 2 grafikleri dağıttıysanız, bkz. [Held v2 'yi v3 'e geçirme](https://helm.sh/docs/topics/v2_v3_migration/).

Her sürüme özel iş akışlarıyla Azure Container Registry, Held grafiklerini barındırmak için hele 3 veya Held 2 kullanabilirsiniz:

* [Heln 3 istemcisi](#use-the-helm-3-client) -kayıt `helm chart` defterinizde grafikleri [OCI yapıtları](container-registry-image-formats.md#oci-artifacts) olarak yönetmek için Held CLI içindeki komutları kullanın
* [Helm 2 istemcisi](#use-the-helm-2-client) -kapsayıcı kayıt defterinizi Helm grafik deposu olarak eklemek ve yönetmek IÇIN Azure CLI 'de [az ACR Helm][az-acr-helm] komutlarını kullanın

### <a name="additional-information"></a>Ek bilgiler

* Çoğu senaryoda, grafikleri OCı yapıtları olarak yönetmek için yerel `helm chart` komutlarla hele 3 iş akışını kullanmanızı öneririz.
* Helm 3 istemcisiyle ve grafikleriyle, eski [az ACR Helm][az-acr-helm] Azure CLI komutlarını ve iş akışını kullanabilirsiniz. Ancak, gibi bazı komutlar Held 3 grafikleriyle uyumlu `az acr helm list` değildir.
* Helm 3 itibariyle, [az ACR Helm][az-acr-helm] komutları genellikle Helm 2 istemcisiyle ve grafik biçimiyle uyumluluk için desteklenir. Bu komutların gelecekte geliştirilmesi planlanmamaktadır.

## <a name="use-the-helm-3-client"></a>Helb 3 istemcisini kullanma

### <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğinizdeki **bir Azure Kapsayıcı kayıt defteri** . Gerekirse, [Azure Portal](container-registry-get-started-portal.md) veya [Azure CLI](container-registry-get-started-azure-cli.md)kullanarak bir kayıt defteri oluşturun.
- **Hele Client Version 3.1.0 veya üzeri** -geçerli `helm version` sürümünüzü bulmak için çalıştırın. Held 'yi yükleme ve yükseltme hakkında daha fazla bilgi için bkz. [Held yükleme][helm-install].
- Helk grafiğini yükleyeceğiniz **bir Kubernetes kümesi** . Gerekirse, bir [Azure Kubernetes hizmet kümesi][aks-quickstart]oluşturun. 
- Sürümü bulmak için **Azure CLI sürüm 2.0.71 veya üzeri** -çalıştırın `az --version` . Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

### <a name="high-level-workflow"></a>Üst düzey iş akışı

**Helb 3** ile şunları yapabilirsiniz:

* Bir Azure Container Registry 'de bir veya daha fazla Held deposu oluşturabilir
* Helm3 grafiklerini bir kayıt defterinde [OCI yapıtları](container-registry-image-formats.md#oci-artifacts)olarak depolayın. Şu anda, OCı için Held 3 desteği *deneysel*.
* `helm registry login` Komutunu kullanarak kayıt defterinizde kimlik doğrulaması yapın.
* Bir `helm chart` kayıt defterinde Held grafiklerini göndermek, çekmek ve yönetmek Için HELı CLI içindeki komutları kullanın
* Yerel `helm install` bir depo önbelleğinden bir Kubernetes kümesine grafik yüklemek için kullanın.

Örnekler için aşağıdaki bölümlere bakın.

### <a name="enable-oci-support"></a>OCı desteğini etkinleştir

Hele 3 istemcisinde OCı desteğini etkinleştirmek için aşağıdaki ortam değişkenini ayarlayın. Şu anda bu destek deneysel bir değer. 

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

Temel bir örnek olarak, dizini `templates` klasörü olarak değiştirin ve ilk olarak içeriğini silin:

```console
cd hello-world/templates
rm -rf *
```

`templates` Klasöründe, aşağıdaki içerikle adlı `configmap.yaml` bir dosya oluşturun:

```console
cat <<EOF > configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: hello-world-configmap
data:
  myvalue: "Hello World"
EOF
```

Bu örneği oluşturma ve çalıştırma hakkında daha fazla bilgi için, bkz. HELI docs 'ta [çalışmaya](https://helm.sh/docs/chart_template_guide/getting_started/) başlama.

### <a name="save-chart-to-local-registry-cache"></a>Grafiği yerel kayıt defteri önbelleğine Kaydet

Dizini `hello-world` alt dizine değiştirin. Ardından, grafiğin `helm chart save` bir kopyasını yerel olarak kaydetmek ve ayrıca, kayıt defterinin tam adı (tümü küçük) ve hedef depo ve etiket ile bir diğer ad oluşturmak için öğesini çalıştırın. 

Aşağıdaki örnekte, kayıt defteri adı *mycontainerregistry*, hedef depo ise *Merhaba-Dünya*ve hedef grafik etiketi *v1*, ancak ortamınız için de değer yerine kullanılır:

```console
cd ..
helm chart save . hello-world:v1
helm chart save . mycontainerregistry.azurecr.io/helm/hello-world:v1
```

Grafikleri `helm chart list` yerel kayıt defteri önbelleğine kaydettiğinizden emin olmak için ' i çalıştırın. Çıkış şuna benzer olacaktır:

```console
REF                                                      NAME            VERSION DIGEST  SIZE            CREATED
hello-world:v1                                           hello-world       0.1.0   5899db0 3.2 KiB        2 minutes 
mycontainerregistry.azurecr.io/helm/hello-world:v1       hello-world       0.1.0   5899db0 3.2 KiB        2 minutes
```

### <a name="authenticate-with-the-registry"></a>Kayıt defteriyle kimlik doğrulama

Senaryolarınız `helm registry login` için uygun kimlik bilgilerini kullanarak [kayıt defteriyle kimlik doğrulaması](container-registry-authentication.md) yapmak IÇIN helb 3 CLI içinde komutunu çalıştırın.

Örneğin, kayıt defterine [çekme ve itme izinleri](container-registry-auth-service-principal.md#create-a-service-principal) (acrpush rolü) ile bir Azure Active Directory hizmet sorumlusu oluşturun. Ardından hizmet sorumlusu kimlik bilgilerini sağlayın `helm registry login`. Aşağıdaki örnek, bir ortam değişkeni kullanarak parolayı sağlar:

```console
echo $spPassword | helm registry login mycontainerregistry.azurecr.io \
  --username <service-principal-id> \
  --password-stdin
```

### <a name="push-chart-to-azure-container-registry"></a>Grafiği Azure Container Registry gönder

Grafiği tam `helm chart push` nitelikli hedef depoya göndermek için helm3 CLI içinde komutunu çalıştırın:

```console
helm chart push mycontainerregistry.azurecr.io/helm/hello-world:v1
```

Başarılı bir gönderim sonrasında, çıkış şuna benzerdir:

```output
The push refers to repository [mycontainerregistry.azurecr.io/helm/hello-world]
ref:     mycontainerregistry.azurecr.io/helm/hello-world:v1
digest:  5899db028dcf96aeaabdadfa5899db025899db025899db025899db025899db02
size:    3.2 KiB
name:    hello-world
version: 0.1.0
```

### <a name="list-charts-in-the-repository"></a>Depodaki grafikleri listeleme

Azure Container Registry 'de depolanan görüntülerde olduğu gibi, grafiklerinizi barındıran depoları, grafik etiketlerini ve bildirimleri göstermek için [az ACR Repository][az-acr-repository] komutlarını kullanabilirsiniz. 

Örneğin, önceki adımda oluşturduğunuz deponun özelliklerini görmek için [az ACR Repository Show][az-acr-repository-show] ' ı çalıştırın:

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

Depoda depolanan grafiğin ayrıntılarını görmek için [az ACR Repository Show-bildirimleri][az-acr-repository-show-manifests] komutunu çalıştırın. Örneğin:

```azurecli
az acr repository show-manifests \
  --name mycontainerregistry \
  --repository helm/hello-world --detail
```

Bu örnekte kısaltılmış çıkış, ' a `configMediaType` şunu gösterir: `application/vnd.cncf.helm.config.v1+json`

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

Kubernetes 'e bir helk grafiği yüklemek için, grafiğin yerel önbellekte olması gerekir. Bu örnekte, önce aşağıdaki adlı `helm chart remove` `mycontainerregistry.azurecr.io/helm/hello-world:v1`var olan yerel grafiği kaldırmak için öğesini çalıştırın:

```console
helm chart remove mycontainerregistry.azurecr.io/helm/hello-world:v1
```

Grafiği `helm chart pull` Azure Container Registry 'den yerel önbelleğinize indirmek için çalıştırın:

```console
helm chart pull mycontainerregistry.azurecr.io/helm/hello-world:v1
```

### <a name="export-helm-chart"></a>Held grafiğini dışarı aktar

Grafikle daha fazla çalışmak için, kullanarak `helm chart export`yerel bir dizine dışarı aktarın. Örneğin, `install` dizine çekilecek grafiği dışarı aktarın:

```console
helm chart export mycontainerregistry.azurecr.io/helm/hello-world:v1 \
  --destination ./install
```

Depoda, içe aktarılmış grafik bilgilerini görüntülemek için, grafiği verdiğiniz dizinde `helm show chart` komutunu çalıştırın.

```console
cd install
helm show chart hello-world
```

HELI, aşağıdaki örnek çıktıda gösterildiği gibi, grafiğinizin en son sürümü hakkında ayrıntılı bilgiler döndürür:

```output
apiVersion: v2
appVersion: 1.16.0
description: A Helm chart for Kubernetes
name: hello-world
type: application
version: 0.1.0    
```

### <a name="install-helm-chart"></a>Held grafiğini yükler

Yerel `helm install` önbelleğe ve dışarıya verdiğiniz Held grafiğini yüklemek için öğesini çalıştırın. *Myhelmtest*gibi bir yayın adı belirtin veya `--generate-name` parametreyi geçirin. Örneğin:

```console
helm install myhelmtest ./hello-world
```

Başarılı grafik yüklemesinden sonra oluşan çıkış şuna benzerdir:

```console
NAME: myhelmtest
LAST DEPLOYED: Fri Mar 20 14:14:42 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

Yüklemeyi doğrulamak için `helm get manifest` komutunu çalıştırın. Komut, `configmap.yaml` şablon dosyanızdaki YAML verilerini döndürür.

Kümenizdeki `helm uninstall` grafik sürümünü kaldırmak için öğesini çalıştırın:

```console
helm uninstall myhelmtest
```

### <a name="delete-a-helm-chart-from-the-repository"></a>Depodaki bir hela grafiğini silme

Bir grafiği depodan silmek için [az ACR Repository Delete][az-acr-repository-delete] komutunu kullanın. Aşağıdaki komutu çalıştırın ve sorulduğunda işlemi onaylayın:

```azurecli
az acr repository delete --name mycontainerregistry --image helm/hello-world:v1
```

## <a name="use-the-helm-2-client"></a>Held 2 istemcisini kullanma

### <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğinizdeki **bir Azure Kapsayıcı kayıt defteri** . Gerekirse, [Azure Portal](container-registry-get-started-portal.md) veya [Azure CLI](container-registry-get-started-azure-cli.md)kullanarak bir kayıt defteri oluşturun.
- **Hele istemci sürümü 2.11.0 (RC sürümü değil) veya daha sonra** geçerli sürümünüzü `helm version` bulmak için ' i çalıştırın. Ayrıca bir Kubernetes kümesi içinde başlatılan bir Held sunucusuna (Tiller) ihtiyacınız vardır. Gerekirse, bir [Azure Kubernetes hizmet kümesi][aks-quickstart]oluşturun. Held 'yi yükleme ve yükseltme hakkında daha fazla bilgi için bkz. [Held yükleme][helm-install-v2].
- Sürümü bulmak için **Azure CLI sürüm 2.0.46 veya üzeri** -çalıştırın `az --version` . Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

### <a name="high-level-workflow"></a>Üst düzey iş akışı

**Helb 2** ile şunları yapabilirsiniz:

* Azure Container kayıt defterinizi *tek* bir Held grafik deposu olarak yapılandırın. Azure Container Registry, havuza grafik ekleyip kaldırırken Dizin tanımını yönetir.
* Azure CLı ile Azure Container Registry ile kimlik doğrulaması yapın, daha sonra hele istemcinizi kayıt defteri URI 'SI ve kimlik bilgileriyle otomatik olarak güncelleştirir. Bu kayıt defteri bilgilerini el ile belirtmeniz gerekmez, bu nedenle kimlik bilgileri komut geçmişinde gösterilmez.
* Azure Container kayıt defterinizi Helm grafik deposu olarak eklemek ve grafikleri göndermek ve yönetmek için Azure CLı 'daki [az ACR Helm][az-acr-helm] komutlarını kullanın. Bu Azure CLı komutları, Helm 2 istemci komutlarını sarmalıdır.
* Grafik aramasını destekleyen, Azure Container Kayıt defterinize grafik deposunu yerel Helmrepo dizinine ekleyin.
* Yerel `helm install` bir depo önbelleğinden bir Kubernetes kümesine grafik yüklemek için kullanın.

Örnekler için aşağıdaki bölümlere bakın.

### <a name="add-repository-to-helm-client"></a>Hele istemcisine depo ekleme

[Az ACR Helm depo Add][az-acr-helm-repo-add] komutunu kullanarak, Azure Container Registry Helm grafik deponuzu Helm istemcisine ekleyin. Bu komut, Azure Container kayıt defteriniz için Helm istemcisi tarafından kullanılan bir kimlik doğrulama belirteci alır. Kimlik doğrulama belirteci 3 saat için geçerlidir. Benzer şekilde `docker login`, Azure Container Registry Held grafik deponuzu ile helmistemcinizin kimliğini doğrulamak için bu komutu gelecekteki CLI oturumlarında çalıştırabilirsiniz:

```azurecli
az acr helm repo add --name mycontainerregistry
```

### <a name="add-a-sample-chart-to-the-repository"></a>Depoya örnek grafik ekleme

İlk olarak, *~/ACR-Held*konumunda bir yerel dizin oluşturun ve ardından mevcut *kararlı/WordPress* grafiğini indirin:

```console
mkdir ~/acr-helm && cd ~/acr-helm
helm repo update
helm fetch stable/wordpress
```

İndirilen `ls` grafiği listelemek için yazın ve dosya adına dahil olan WordPress sürümünü aklınızda yazın. `helm fetch stable/wordpress` Komut belirli bir sürüm belirtmedi, bu nedenle *en son* sürüm getirildi. Aşağıdaki örnek çıktıda, WordPress grafiğinin sürümü *8.1.0*olur:

```output
wordpress-8.1.0.tgz
```

Azure CLı 'de [az ACR Helm Push][az-acr-helm-push] komutunu kullanarak Azure Container Registry grafikleri Helm grafik deponuza gönderin. Önceki adımda indirilen HELI grafiğinin adını belirtin, örneğin *WordPress-8.1.0. tgz*:

```azurecli
az acr helm push --name mycontainerregistry wordpress-8.1.0.tgz
```

Birkaç dakika sonra Azure CLı, aşağıdaki örnek çıktıda gösterildiği gibi, grafiğinizin kaydedildiğini bildirir:

```output
{
  "saved": true
}
```

### <a name="list-charts-in-the-repository"></a>Depodaki grafikleri listeleme

Önceki adımda karşıya yüklenen grafiği kullanmak için yerel Held depo dizininin güncellenmesi gerekir. Hele istemcisinde depoları yeniden düzenleyebilir veya depo dizinini güncelleştirmek için Azure CLı kullanabilirsiniz. Deponuza bir grafik eklediğiniz her sefer, bu adımın tamamlanması gerekir:

```azurecli
az acr helm repo add --name mycontainerregistry
```

Deponuzda depolanan ve güncelleştirilmiş Dizin yerel olarak bulunan bir grafik ile, aramak veya yüklemek için normal hele istemci komutlarını kullanabilirsiniz. Deponuzdaki tüm grafikleri görmek için, kendi Azure Container Registry adınızı sağlayan `helm search` komutunu kullanın:

```console
helm search mycontainerregistry
```

Önceki adımda gönderilen WordPress grafiği, aşağıdaki örnek çıktıda gösterildiği gibi listelenir:

```output
NAME                  CHART VERSION    APP VERSION    DESCRIPTION
helmdocs/wordpress    8.1.0           5.3.2          Web publishing platform for building blogs and websites.
```

Grafikleri, [az ACR Helm List][az-acr-helm-list]kullanarak Azure CLI ile de listeleyebilirsiniz:

```azurecli
az acr helm list --name mycontainerregistry
```

### <a name="show-information-for-a-helm-chart"></a>Hela grafiğinin bilgilerini göster

Depodaki belirli bir grafiğin bilgilerini görüntülemek için `helm inspect` komutunu kullanabilirsiniz.

```console
helm inspect mycontainerregistry/wordpress
```

Sürüm numarası sağlanmamışsa, *en son* sürüm kullanılır. HELI, aşağıdaki sıkıştırılmış örnek çıktıda gösterildiği gibi, grafiğiniz hakkında ayrıntılı bilgiler döndürür:

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

Ayrıca, Azure CLı [az ACR Helm Show][az-acr-helm-show] komutuyla bir grafik için bilgileri gösterebilirsiniz. Yine, bir grafiğin *en son* sürümü varsayılan olarak döndürülür. Grafiğin belirli bir `--version` sürümünü listeye ekleyebilirsiniz, örneğin, *8.1.0*:

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

Yükleme devam ederken, URL 'Leri ve kimlik bilgilerini görmek için komut çıkışında bulunan yönergeleri izleyin. Helk grafiği aracılığıyla dağıtılan `kubectl get pods` Kubernetes kaynaklarını görmek için komutunu da çalıştırabilirsiniz:

```output
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

Adlandırılmış grafiğin tüm sürümlerini silmek istiyorsanız, `--version` parametresini bırakın.

, Çalıştırdığınızda `helm search`grafik döndürülür. Yine, HELI istemcisi bir depodaki kullanılabilir grafiklerin listesini otomatik olarak güncelleştirmez. Helm istemci deposu dizinini güncelleştirmek için [az ACR Helm depo Add][az-acr-helm-repo-add] komutunu tekrar kullanın:

```azurecli
az acr helm repo add --name mycontainerregistry
```

## <a name="next-steps"></a>Sonraki adımlar

* HELI grafikleri oluşturma ve dağıtma hakkında daha fazla bilgi için bkz. [HELI grafikleri geliştirme][develop-helm-charts].
* [Azure Kubernetes Service 'te (AKS)](../aks/kubernetes-helm.md)Held ile uygulama yükleme hakkında daha fazla bilgi edinin.
* Held grafikleri kapsayıcı oluşturma sürecinin bir parçası olarak kullanılabilir. Daha fazla bilgi için bkz. [Azure Container Registry görevleri kullanma][acr-tasks].

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
