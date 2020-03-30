---
title: Sorun giderme
services: azure-dev-spaces
ms.date: 09/25/2019
ms.topic: troubleshooting
description: Azure Geliştirme Alanları'nı etkinleştirirken ve kullanırken sık karşılaşılan sorunları nasıl gidereceğinizi ve nasıl çözeceğinizi öğrenin
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler, Miğfer, servis kafesi, servis örgü yönlendirme, kubectl, k8s '
ms.openlocfilehash: c12dfd385962d8dd7de8239a0d4ecd46746499c0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80239776"
---
# <a name="azure-dev-spaces-troubleshooting"></a>Azure Dev Spaces sorun giderme

Bu kılavuz, Azure Geliştirme Alanları'nı kullanırken sık karşılaşılabilecek sorunlar hakkında bilgi içerir.

Azure Dev Spaces kullanırken bir sorun varsa, [Azure Dev Spaces GitHub deposunda](https://github.com/Azure/dev-spaces/issues)bir sorun oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

Sorunları daha etkili bir şekilde gidermek için, gözden geçirilen daha ayrıntılı günlükler oluşturulmasına yardımcı olabilir.

Visual Studio uzantısı için `MS_VS_AZUREDEVSPACES_TOOLS_LOGGING_ENABLED` ortam değişkenini 1 olarak ayarlayın. Ortam değişkeninin etkili olması için Visual Studio'yu yeniden başlattığınızdan emin olun. Etkinleştirildikten sonra, ayrıntılı günlükler dizininize `%TEMP%\Microsoft.VisualStudio.Azure.DevSpaces.Tools` yazılır.

CLI'de, `--verbose` anahtarı kullanarak komut yürütme sırasında daha fazla bilgi çıktınız. Ayrıca daha ayrıntılı günlükleri göz `%TEMP%\Azure Dev Spaces`atabilirsiniz. Mac'te *TEMP* dizini terminal penceresinden `echo $TMPDIR` çalıştırılarak bulunabilir. Bir Linux bilgisayarında, *TEMP* dizini genellikle `/tmp`. Ayrıca, [Azure CLI yapılandırma dosyanızda](/cli/azure/azure-cli-configuration?view=azure-cli-latest#cli-configuration-values-and-environment-variables)günlüğe kaydetmenin etkin olduğunu doğrulayın.

Azure Dev Spaces, tek bir örneği veya bölmeyi hata ayıklarken de en iyi şekilde çalışır. Dosya, `azds.yaml` Kubernetes'in hizmetiniz için çalıştırdığı bölme sayısını gösteren bir ayar, *çoğaltma Sayısı*içerir. Uygulamanızı belirli bir hizmet için birden çok bölme çalışacak şekilde yapılandırmak üzere *çoğaltma Sayısı'nı* değiştirirseniz, hata ayıklayıcı alfabetik olarak listelendiğinde ilk bölmeye bağlanır. Hata ayıklama, özgün bölme geri dönüşümleri olduğunda farklı bir bölmeye bağlanır ve bu da beklenmeyen davranışlara neden olur.

## <a name="common-issues-when-enabling-azure-dev-spaces"></a>Azure Geliştirme Alanlarını etkinleştirirken sık karşılaşılan sorunlar

### <a name="error-failed-to-create-azure-dev-spaces-controller"></a>Hata "Azure Dev Spaces denetleyicisi oluşturmak için başarısız oldu"

Denetleyicinin oluşturulmasında bir sorun olduğunda bu hatayı görebilirsiniz. Geçici bir hataysa, düzeltmek için denetleyiciyi silin ve yeniden oluşturun.

Denetleyiciyi silmeyi de deneyebilirsiniz:

```bash
azds remove -g <resource group name> -n <cluster name>
```

Bir denetleyiciyi silmek için Azure Dev Spaces CLI'yi kullanın. Visual Studio'dan bir denetleyiciyi silmek mümkün değildir. Azure Bulut Bulut Uybiti'nde bir denetleyiciyi silememek için Azure Dev Spaces CLI'yi Azure Bulut Kabuğu'na da yükleyemezsiniz.

Azure Dev Spaces CLI yüklü değilseniz, önce aşağıdaki komutu kullanarak yükleyebilirsiniz, ardından kumandanızı silebilirsiniz:

```azurecli
az aks use-dev-spaces -g <resource group name> -n <cluster name>
```

Denetleyiciyi yeniden oluşturmak CLI veya Visual Studio'dan yapılabilir. Örnekler için [.NET Core](quickstart-netcore-visualstudio.md) hızlı başlangıçlarıyla Takım [geliştirme](quickstart-team-development.md) veya Geliştirme'ye bakın.

### <a name="controller-create-failing-because-of-controller-name-length"></a>Denetleyici ad uzunluğu nedeniyle başarısız oluşturma

Azure Dev Spaces denetleyicisi adı 31 karakterden uzun olamaz. Bir AKS kümesinde Dev Spaces'i etkinleştirdiğinizde veya bir denetleyici oluşturduğunuzda denetleyicinizin adı 31 karakteri aşarsa, bir hata alırsınız. Örnek:

```console
Failed to create a Dev Spaces controller for cluster 'a-controller-name-that-is-way-too-long-aks-east-us': Azure Dev Spaces Controller name 'a-controller-name-that-is-way-too-long-aks-east-us' is invalid. Constraint(s) violated: Azure Dev Spaces Controller names can only be at most 31 characters long*
```

Bu sorunu gidermek için alternatif bir ada sahip bir denetleyici oluşturun. Örnek:

```cmd
azds controller create --name my-controller --target-name MyAKS --resource-group MyResourceGroup
```

### <a name="enabling-dev-spaces-failing-when-windows-node-pools-are-added-to-an-aks-cluster"></a>Windows düğüm havuzları bir AKS kümesine eklendiğinde Dev Spaces'in başarısız olmasını etkinleştirme

Şu anda Azure Dev Spaces yalnızca Linux bölmelerinde ve düğümlerinde çalışacak şekilde tasarlanmıştır. Windows düğüm havuzu olan bir AKS kümeniz varsa, Azure Dev Spaces bölmelerinin yalnızca Linux düğümlerinde zamanlandığından emin olmalısınız. Bir Azure Dev Spaces bölmesi Windows düğümünde çalışacak şekilde zamanlanırsa, bu bölme başlatılamaz ve Dev Spaces'i etkinleştirmek başarısız olur.

Bu sorunu gidermek için, Linux bölmelerinin Windows düğümünde çalışacak şekilde zamanlanmadığından emin olmak için AKS kümenize [bir leke ekleyin.](../aks/operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations)

### <a name="error-found-no-untainted-linux-nodes-in-ready-state-on-the-cluster-there-needs-to-be-at-least-one-untainted-linux-node-in-ready-state-to-deploy-pods-in-azds-namespace"></a>Hata "Kümede Ready durumunda bozulmamış Linux düğümleri bulunamadı. 'Azds' ad alanında bölme dağıtmak için Ready durumunda en az bir tane bozulmamış Linux düğümü olması gerekir."

Azure Dev Spaces, bölmeleri zamanlamak için *Hazır* durumunda lekelenmemiş bir düğüm bulamadığı için AKS kümenizde bir denetleyici oluşturamadı. Azure Dev Spaces, hazır *durumunda,* not eretmeleri belirtmeden bölmeleri zamanlamaya olanak tanıyan en az bir Linux düğümü gerektirir.

Bu sorunu gidermek için, aks [kümenizdeki kusurlu yapılandırmanızı güncelleştirerek](../aks/operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations) en az bir Linux düğümünin, tolere girmeden bölmeleri zamanlamaya izin verdiğinden emin olun. Ayrıca, noterasyonları belirtmeden bölmeleri zamanlamaya izin veren en az bir Linux düğümünün *Hazır* durumunda olduğundan emin olun. Düğümünüzün *Hazır* durumuna ulaşması uzun sürüyorsa düğümünüze yeniden başlatmayı deneyebilirsiniz.

### <a name="error-azure-dev-spaces-cli-not-installed-properly-when-running-az-aks-use-dev-spaces"></a>Az aks kullanım-dev-boşluklarını çalıştırırken "Azure Dev Spaces CLI düzgün yüklenmedi" hatası

Azure Dev Spaces CLI'ye yapılan bir güncelleştirme yükleme yolunu değiştirdi. Azure CLI sürümünü 2.0.63'ten önce kullanıyorsanız, bu hatayı görebilirsiniz. Azure CLI sürümünüzü görüntülemek için `az --version`.

```azurecli
az --version
```

```output
azure-cli                         2.0.60 *
...
```

2.0.63'ten önce Azure CLI sürümüyle çalışırken `az aks use-dev-spaces` hata iletisi rağmen, yükleme başarılı olur. Herhangi bir sorun `azds` olmadan kullanmaya devam edebilirsiniz.

Bu sorunu gidermek için Azure [CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) yüklemenizi 2.0.63 veya sonraki olarak güncelleştirin. Bu güncelleştirme, çalışırken `az aks use-dev-spaces`aldığınız hata iletisini çözecektir. Alternatif olarak, geçerli Azure CLI ve Azure Dev Spaces CLI sürümünüzü kullanmaya devam edebilirsiniz.

### <a name="error-unable-to-reach-kube-apiserver"></a>Hata "Kube-apiserver'a ulaşılamıyor"

Azure Dev Spaces AKS kümenizin API sunucusuna bağlanamayınca bu hatayı görebilirsiniz. 

AKS küme API sunucunuza erişim kilitlenmişse veya AKS kümeniz için etkin [leştirilmiş API sunucusu yetkili IP adresi aralıkları](../aks/api-server-authorized-ip-ranges.md) varsa, [bölgenize dayalı ek aralıklara izin](https://github.com/Azure/dev-spaces/tree/master/public-ips)vermek için kümenizi [oluşturmanız](../aks/api-server-authorized-ip-ranges.md#create-an-aks-cluster-with-api-server-authorized-ip-ranges-enabled) veya [güncelleştirmeniz](../aks/api-server-authorized-ip-ranges.md#update-a-clusters-api-server-authorized-ip-ranges) gerekir.

API sunucusunun kubectl komutlarını çalıştırarak kullanılabilir olduğundan emin olun. API sunucusu kullanılamıyorsa, lütfen AKS desteğine başvurun ve API sunucusu çalışırken yeniden deneyin.

## <a name="common-issues-when-preparing-your-project-for-azure-dev-spaces"></a>Projenizi Azure Geliştirme Alanları'na hazırlarken sık karşılaşılan sorunlar

### <a name="warning-dockerfile-could-not-be-generated-due-to-unsupported-language"></a>Uyarı "Dockerfile desteklenmeyen dil nedeniyle oluşturulamadı"
Azure Dev Spaces, C# ve Node.js için yerel destek sağlar. Bu dillerden birinde kod yazıldığı bir dizinde çalıştırdığınızda, `azds prep` Azure Dev Spaces sizin için otomatik olarak uygun bir Dockerfile oluşturur.

Azure Dev Spaces'i başka dillerde yazılmış kodlarla kullanmaya devam edebilirsiniz, `azds up` ancak ilk kez çalışmadan önce Dockerfile'ı el ile oluşturmanız gerekir.

Uygulamanız Azure Dev Spaces'in doğal olarak desteklemediği bir dilde yazılmışsa, kodunuzu çalıştıran bir kapsayıcı görüntüsü oluşturmak için uygun bir Dockerfile sağlamanız gerekir. Docker, [Dockerfiles yazmak için en iyi uygulamaların](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) bir listesini ve ihtiyaçlarınıza uygun bir Dockerfile yazmanıza yardımcı olabilecek bir [Dockerfile başvurusu](https://docs.docker.com/engine/reference/builder/) sağlar.

Uygun bir Dockerfile'ı yerleştirdikten `azds up` sonra, uygulamanızı Azure Geliştirme Alanları'nda çalıştırmak için çalışırsınız.

## <a name="common-issues-when-starting-or-stopping-services-with-azure-dev-spaces"></a>Azure Geliştirme Alanları ile hizmet başlatırken veya durdururken sık karşılaşılan sorunlar

### <a name="error-config-file-not-found"></a>Hata "Config dosyası bulunamadı:"

Çalışırken, `azds up`bu hatayı görebilirsiniz. Her `azds up` `azds prep` ikisi de ve geliştirme alanınızda çalıştırmak istediğiniz projenin kök dizininden çalıştırılmalıdır.

Bu sorunu düzeltmek için:
1. Geçerli dizininizi hizmet kodunuzu içeren kök klasörle değiştirin. 
1. Kod klasöründe _azds.yaml_ dosyanız yoksa Docker, `azds prep` Kubernetes ve Azure Dev Spaces varlıklarını oluşturmak için çalıştırın.

### <a name="timeout-at-waiting-for-container-image-build-step-with-aks-virtual-nodes"></a>Zaman aramı "Kapsayıcı görüntü oluşturmayı bekliyorum..." AKS sanal düğümleri ile adım

Bu zaman kaçamaz, [AKS sanal düğümünde](https://docs.microsoft.com/azure/aks/virtual-nodes-portal)çalışacak şekilde yapılandırılan bir hizmeti çalıştırmak için Dev Spaces'i kullanmaya çalıştığınızda oluşur. Dev Spaces şu anda sanal düğümlerde hizmet oluşturmayı veya hata ayıklamayı desteklemiyor.

`--verbose` Anahtarla çalışırsanız `azds up` veya Visual Studio'da ayrıntılı günlüğe kaydetmeyi etkinleştirinizse, ek ayrıntı görürsünüz:

```cmd
azds up --verbose

Installed chart in 2s
Waiting for container image build...
pods/mywebapi-76cf5f69bb-lgprv: Scheduled: Successfully assigned default/mywebapi-76cf5f69bb-lgprv to virtual-node-aci-linux
Streaming build container logs for service 'mywebapi' failed with: Timed out after 601.3037572 seconds trying to start build logs streaming operation. 10m 1s
Container image build failed
```

Yukarıdaki komut, hizmetin bölmesinin sanal bir düğüm olan *sanal düğüm-aci-linux'a*atandığını gösterir.

Bu sorunu gidermek için, hizmetin sanal bir düğüm üzerinde çalışmasına izin veren *düğüm veya* *tolere* değerlerini kaldırmak için hizmetin Miğfer grafiğini güncelleştirin. Bu değerler genellikle grafiğin `values.yaml` dosyasında tanımlanır.

Dev Spaces üzerinden oluşturmak veya hata ayıklamak istediğiniz hizmet VM düğümünde çalışıyorsa, sanal düğüm özelliği etkinleştirilmiş bir AKS kümesini kullanmaya devam edebilirsiniz. VM düğümünde Dev Spaces ile hizmet çalıştırmak varsayılan yapılandırmadır.

### <a name="error-could-not-find-a-ready-tiller-pod-when-launching-dev-spaces"></a>Hata Dev Spaces başlatırken "hazır bir çapa makinesi pod bulamadı"

Bu hata, Miğfer istemcisi kümede çalışan Tiller bölmesi ile artık konuşamazsa oluşur.

Bu sorunu gidermek için kümenizdeki aracı düğümlerini yeniden başlatın.

### <a name="error-release-azds-identifier-spacename-servicename-failed-services-servicename-already-exists-or-pull-access-denied-for-servicename-repository-does-not-exist-or-may-require-docker-login"></a>Hata "sürüm azds-\<tanımlayıcı\>-\<spacename\>-\<servicename\> başarısız\<oldu:\>hizmetler ' servicename ' zaten \<var" veya "Hizmet adı\>için reddedilen erişimi çekin , depo yok veya 'docker giriş' gerektirebilir"

Bu hatalar, aynı dev alanı içinde Dev `helm install`Spaces `helm upgrade`komutları (gibi `helm delete` `azds up` ve `azds down`) ile çalışan doğrudan Miğfer komutları (, , veya ) karıştırırsanız oluşabilir. Bunlar, Dev Spaces'in kendi Çapa Makinesi örneğine sahip olması nedeniyle oluşur ve bu da aynı dev alanda çalışan kendi Tiller örneğinizle çakışıyor.

Hem Miğfer komutlarını hem de Dev Spaces komutlarını aynı AKS kümesine karşı kullanmak iyidir, ancak dev spaces etkinleştirilmiş her ad alanı birini veya diğerini kullanmalıdır.

Örneğin, tüm uygulamanızı bir üst dev alanında çalıştırmak için bir Miğfer komutu kullandığınızı varsayalım. Bu üst öğeden alt öğe geliştirme alanları oluşturabilir, dev boşlukları kullanarak tek tek hizmetleri alt geliştirme alanları içinde çalıştırabilir ve hizmetleri birlikte test edebilirsiniz. Değişikliklerinizi iade etmeye hazır olduğunuzda, güncelleştirilmiş kodu üst geliştirme alanına dağıtmak için bir Miğfer komutu kullanın. Güncelleştirilmiş hizmeti `azds up` üst geliştirme alanında çalıştırmak için kullanmayın, çünkü başlangıçta Miğfer kullanılarak çalıştırılen hizmetle çakışacaktır.

### <a name="existing-dockerfile-not-used-to-build-a-container"></a>Varolan Dockerfile bir kapsayıcı oluşturmak için kullanılmaz

Azure Geliştirme Alanları, projenizdeki belirli bir _Dockerfile'ı_ işaret etmek üzere yapılandırılabilir. Azure Dev Spaces kapsayıcılarınızı oluşturmayı beklediğiniz _Dockerfile'ı_ kullanmıyor gibi görünüyorsa, Azure Dev Spaces'e hangi Dockerfile'ı kullanacağınızı açıkça söylemeniz gerekebilir. 

Bu sorunu gidermek için, Azure Dev Spaces'in projenizde oluşturduğu _azds.yaml_ dosyasını açın. *Yapılandırmaları güncelleştirin: geliştirin: inşa:* kullanmak istediğiniz Dockerfile'ı işaret etmek için dockerfile. Örnek:

```yaml
...
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
```

### <a name="error-unauthorized-authentication-required-when-trying-to-use-a-docker-image-from-a-private-registry"></a>Özel bir kayıt defterinden Docker görüntüsünü kullanmaya çalışırken "yetkisiz: kimlik doğrulama gerekli" hatası

Kimlik doğrulaması gerektiren özel bir kayıt defterinden docker görüntüsü kullanıyorsunuz.

Bu sorunu gidermek için, Dev Spaces'in [görüntüPullSecrets'ı](https://kubernetes.io/docs/concepts/configuration/secret/#using-imagepullsecrets)kullanarak bu özel kayıt defterinden görüntüleri doğrulaması ve çekmesine izin verebilirsiniz. ImagePullSecrets'ı kullanmak için, görüntüyü kullandığınız ad alanında [bir Kubernetes sırrı oluşturun.](https://kubernetes.io/docs/concepts/containers/images/#specifying-imagepullsecrets-on-a-pod) Sonra bir görüntü PullSecret olarak `azds.yaml`gizli sağlamak .

Aşağıda belirterek bir görüntü örneğidirPullSecrets içinde `azds.yaml`.

```yaml
kind: helm-release
apiVersion: 1.1
build:
  context: $BUILD_CONTEXT$
  dockerfile: Dockerfile
install:
  chart: $CHART_DIR$
  values:
  - values.dev.yaml?
  - secrets.dev.yaml?
  set:
    # Optional, specify an array of imagePullSecrets. These secrets must be manually created in the namespace.
    # This will override the imagePullSecrets array in values.yaml file.
    # If the dockerfile specifies any private registry, the imagePullSecret for the registry must be added here.
    # ref: https://kubernetes.io/docs/concepts/containers/images/#specifying-imagepullsecrets-on-a-pod
    #
    # This uses credentials from secret "myRegistryKeySecretName".
    imagePullSecrets:
      - name: myRegistryKeySecretName
```

> [!IMPORTANT]
> Ayarı görüntüPullSecrets `azds.yaml` içinde belirtilen görüntüPullSecrets `values.yaml`geçersiz kılacak .

### <a name="error-service-cannot-be-started"></a>Hata "Hizmet başlatılamaz."

Hizmet kodunuz başlatıldığında bu hatayı görebilirsiniz. Nedeni genellikle kullanıcı kodundadır. Daha fazla tanılama bilgisi almak için, hizmetinizi başlatırken daha ayrıntılı günlüğe kaydetmeyi etkinleştirin.

Komut satırından, daha `--verbose` ayrıntılı günlüğe kaydetmeyi etkinleştirmek için kullanın. Ayrıca bir çıktı biçimi `--output`kullanarak belirtebilirsiniz. Örnek:

```cmd
azds up --verbose --output json
```

Görsel Stüdyo'da:

1. Açık **Araçlar > Seçenekleri** ve Projeler ve **Çözümler**altında, Oluştur **ve Çalıştır'ı**seçin.
2. **MSBuild projesi nin** ayarlarını **ayrıntılı** veya **tanılama**ile değiştirin.

    ![Araç Seçenekleri iletişim kutusunun ekran görüntüsü](media/common/VerbositySetting.PNG)

### <a name="rerunning-a-service-after-controller-re-creation"></a>Denetleyici yeniden oluşturulduktan sonra bir hizmeti yeniden çalıştırma

Bu kümeyle ilişkili Azure Dev Spaces denetleyicisini kaldırdıktan ve sonra yeniden oluşturduktan sonra bir hizmeti yeniden çalıştırmaya çalışırken bir *Hizmet başlatılamaz.* Bu durumda, ayrıntılı çıktı aşağıdaki metni içerir:

```output
Installing Helm chart...
Release "azds-33d46b-default-webapp1" does not exist. Installing it now.
Error: release azds-33d46b-default-webapp1 failed: services "webapp1" already exists
Helm install failed with exit code '1': Release "azds-33d46b-default-webapp1" does not exist. Installing it now.
Error: release azds-33d46b-default-webapp1 failed: services "webapp1" already exists
```

Dev Spaces denetleyicisini kaldırmanın, bu denetleyici tarafından daha önce yüklenmiş hizmetleri kaldırmaması nedeniyle bu hata oluşur. Denetleyiciyi yeniden oluşturma ve eski hizmetler hala yerinde olduğundan, yeni denetleyiciyi kullanarak hizmetleri çalıştırmayı denemek başarısız olur.

Bu sorunu gidermek için, eski hizmetleri kümenizden el ile kaldırmak için `kubectl delete` komutu kullanın ve ardından yeni hizmetleri yüklemek için Dev Spaces'i yeniden çalıştırın.

### <a name="error-service-cannot-be-started-when-using-multi-stage-dockerfiles"></a>Hata "Hizmet başlatılamaz." çok aşamalı Dockerfiles kullanırken

Çok aşamalı Dockerfile kullanırken bir Hizmet hatası *başlatılamaz.* Bu durumda, ayrıntılı çıktı aşağıdaki metni içerir:

```cmd
$ azds up -v
Using dev space 'default' with target 'AksClusterName'
Synchronizing files...6s
Installing Helm chart...2s
Waiting for container image build...10s
Building container image...
Step 1/12 : FROM [imagename:tag] AS base
Error parsing reference: "[imagename:tag] AS base" is not a valid repository/tag: invalid reference format
Failed to build container image.
Service cannot be started.
```

Azure Dev Spaces şu anda çok aşamalı yapıları desteklemediği için bu hata oluşur. Çok aşamalı yapılardan kaçınmak için Dockerfile'ınızı yeniden yazın.

### <a name="network-traffic-is-not-forwarded-to-your-aks-cluster-when-connecting-your-development-machine"></a>Geliştirme makinenizi bağlarken ağ trafiği AKS kümenize iletilir

[AKS kümenizi geliştirme makinenize bağlamak için Azure Dev Spaces'i](how-to/connect.md)kullanırken, geliştirme makineniz ile AKS kümeniz arasında ağ trafiğinin iletülmediğini zedelediğiniz bir sorunla karşılaşabilirsiniz.

Geliştirme makinenizi AKS kümenize bağlarken, Azure Dev Spaces, geliştirme makinenizin `hosts` dosyasını değiştirerek AKS kümeniz ile geliştirme makineniz arasındaki ağ trafiğini iletir. Azure Dev Spaces, ana `hosts` bilgisayar adı olarak değiştirdiğiniz Kubernetes hizmetinin adresiyle birlikte bir giriş oluşturur. Bu giriş, geliştirme makineniz ve AKS kümesi arasındaki doğrudan ağ trafiğine bağlantı noktası yönlendirmesiyle birlikte kullanılır. Geliştirme makinenizdeki bir hizmet, değiştirmekte olduğunuz Kubernetes hizmetinin bağlantı noktasıyla çakışıyorsa, Azure Dev Spaces Kubernetes hizmeti için ağ trafiğini iletemez. Örneğin, *Windows BranchCache* hizmeti genellikle *0.0.0.0:80'e*bağlıdır ve bu da çakışmaları tüm yerel IP'ler üzerinde bağlantı noktası 80 için çakışmaya neden olur.

Bu sorunu gidermek için, değiştirmeye çalıştığınız Kubernetes hizmetinin bağlantı noktasıyla çakışan hizmetleri veya işlemleri durdurmanız gerekir. Geliştirme makinenizdeki hangi hizmetlerin veya işlemlerin çakıştığınızı denetlemek için *netstat*gibi araçları kullanabilirsiniz.

Örneğin, *Windows BranchCache* hizmetini durdurmak ve devre dışı bırakmak için:
* Komut `services.msc` isteminden çalıştırın.
* *BranchCache'ye* sağ tıklayın ve *Özellikler'i*seçin.
* *Durdur'u*tıklatın.
* İsteğe bağlı olarak, *Başlangıç türünü* Devre *Dışı Bırakılmış*olarak ayarlayarak devre dışı kullanabilirsiniz.
* *Tamam*'a tıklayın.

## <a name="common-issues-using-visual-studio-and-visual-studio-code-with-azure-dev-spaces"></a>Azure Dev Spaces ile Visual Studio ve Visual Studio Code kullanarak sık karşılaşılan sorunlar

### <a name="error-required-tools-and-configurations-are-missing"></a>Hata "Gerekli araçlar ve yapılandırmalar eksik"

Bu hata VS Kodu başlatılırken oluşabilir: "[Azure Dev Spaces] '[proje adı]' oluşturmak ve hata ayıklamak için gerekli araçlar ve yapılandırmalar eksik."
Hata, azds.exe'nin VS Kodu'nda görüldüğü gibi PATH ortamı değişkeninde olmadığı anlamına gelir.

PATH ortamı değişkeninin düzgün ayarlandığı bir komut isteminden VS Kodu başlatmayı deneyin.

### <a name="error-required-tools-to-build-and-debug-projectname-are-out-of-date"></a>Hata "'projeadı' oluşturmak ve hata ayıklamak için gerekli araçlar güncel değil."

Azure Dev Spaces için VS Kodu uzantısının daha yeni bir sürümüne, ancak Azure Dev Spaces CLI'nin eski bir sürümüne sahipseniz, bu hatayı Visual Studio Code'da görürsünüz.

Azure Dev Spaces CLI'nin en son sürümünü indirmeyi ve yüklemeyi deneyin:

* [Windows](https://aka.ms/get-azds-windows)
* [Mac](https://aka.ms/get-azds-mac)
* [Linux](https://aka.ms/get-azds-linux)

### <a name="error-failed-to-find-debugger-extension-for-typecoreclr"></a>Hata: "Türü için hata ayıklama uzantısı bulmak için başarısız oldu:coreclr"

Visual Studio Code hata ayıklayıcısını çalıştırırken bu hatayı görebilirsiniz. Geliştirme makinenizde C# için VS Kodu uzantısı yüklü olmayabilir. C# uzantısı .NET Core (CoreCLR) için hata ayıklama desteği içerir.

Bu sorunu gidermek [için C# için VS Kodu uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)yükleyin.

### <a name="error-configured-debug-type-coreclr-is-not-supported"></a>Hata "Yapılandırılmış hata ayıklama türü 'coreclr' desteklenmez"

Visual Studio Code hata ayıklayıcısını çalıştırırken bu hatayı görebilirsiniz. Geliştirme makinenizde Azure Geliştirme Alanları için VS Kodu uzantısı yüklü olmayabilir.

Bu sorunu gidermek [için Azure Geliştirme Alanları için VS Kodu uzantısını](get-started-netcore.md)yükleyin.

### <a name="error-invalid-cwd-value-src-the-system-cannot-find-the-file-specified-or-launch-program-srcpath-to-project-binary-does-not-exist"></a>Hata "Geçersiz 'cwd' değeri '/src'. Sistem belirtilen dosyayı bulamıyor." veya "başlatın: program '/src/[proje ikilisine giden yol]' diye bir şey yok"

Visual Studio Code hata ayıklayıcısını çalıştırırken bu hatayı görebilirsiniz. Varsayılan olarak, VS Kodu `src` uzantısı kapsayıcıdaki proje için çalışma dizini olarak kullanır. Farklı bir çalışma `Dockerfile` dizini belirtmek için güncellediyseniz, bu hatayı görebilirsiniz.

Bu sorunu gidermek için, proje `.vscode` klasörünüzün alt dizininin altındaki dosyayı `launch.json` güncelleştirin. Yönergeyi, `configurations->cwd` projenizde `WORKDIR` tanımlanan dizini işaret etmek için `Dockerfile`değiştirin. Yönergeyi de güncelleştirmeniz `configurations->program` gerekebilir.

### <a name="error-the-pipe-program-azds-exited-unexpectedly-with-code-126"></a>Hata "Boru programı 'azds' kod 126 ile beklenmedik bir şekilde çıktı."

Visual Studio Code hata ayıklayıcısını çalıştırırken bu hatayı görebilirsiniz.

Bu sorunu gidermek için Visual Studio Code'u kapatın ve yeniden açın. Hata ayıklamayı yeniden başlatın.

### <a name="error-internal-watch-failed-watch-enospc-when-attaching-debugging-to-a-nodejs-application"></a>Hata "Dahili saat başarısız oldu: Düğüm.js uygulamasına hata ayıklama takıldığında ENOSPC izle"

Bu hata, hata ayıklama ile eklemeye çalıştığınız Düğüm.js uygulaması ile bölmeyi çalıştıran düğüm *fs.inotify.max_user_watches* değerini aştığında oluşur. Bazı durumlarda, [ *fs.inotify.max_user_watches* varsayılan değeri bir bölmeye doğrudan hata ayıklama takmayı işlemek için çok küçük olabilir.](https://github.com/Azure/AKS/issues/772)

Bu sorun için geçici bir geçici çözüm kümedeki her düğümde *fs.inotify.max_user_watches* değerini artırmak ve değişikliklerin etkili olması için düğümü yeniden başlatmaktır.

## <a name="other-common-issues"></a>Diğer yaygın sorunlar

### <a name="error-azds-is-not-recognized-as-an-internal-or-external-command-operable-program-or-batch-file"></a>Hata "azds" bir iç veya dış komut, operable programı veya toplu dosya olarak tanınmıyor

Bu hata, `azds.exe` doğru yüklenmez veya yapılandırılmamışsa gerçekleşebilir.

Bu sorunu düzeltmek için:

1. Konumu kontrol edin %ProgramFiles%/Microsoft SDK'ları\Azure\Azure `azds.exe`Dev Spaces CLI için. Varsa, bu konumu PATH ortamı değişkenine ekleyin.
2. Yüklenmiyorsa, `azds.exe` aşağıdaki komutu çalıştırın:

    ```azurecli
    az aks use-dev-spaces -n <cluster-name> -g <resource-group>
    ```

### <a name="authorization-error-microsoftdevspacesregisteraction"></a>Yetkilendirme hatası "Microsoft.DevSpaces/register/action"

Azure Dev Alanlarını yönetmek için Azure aboneliğinizde *Sahip veya* *Katılımcı* erişimine ihtiyacınız vardır. Dev Spaces'i yönetmeye çalışıyorsanız ve ilişkili Azure aboneliğine *Sahip* veya *Katılımcı* erişiminiz yoksa, bir yetkilendirme hatası görebilirsiniz. Örnek:

```output
The client '<User email/Id>' with object id '<Guid>' does not have authorization to perform action 'Microsoft.DevSpaces/register/action' over scope '/subscriptions/<Subscription Id>'.
```

Bu sorunu gidermek için, *Azure* aboneliğine Sahip Veya *Katılımcı* erişimi `Microsoft.DevSpaces` olan bir hesabı kullanarak ad alanını el ile kaydedin:

```azurecli
az provider register --namespace Microsoft.DevSpaces
```

### <a name="new-pods-arent-starting"></a>Yeni bölmeler çalışmıyor

Kubernetes başharfi, kümedeki *küme yöneticisi* rolündeki RBAC izin değişiklikleri nedeniyle PodSpec'i yeni bölmeler için uygulayamaz. Yeni bölmede geçersiz bir PodSpec da olabilir, örneğin podla ilişkili hizmet hesabı artık yok. Baş harfi sorunu nedeniyle *Bekleyen* durumda olan bölmeleri görmek için `kubectl get pods` aşağıdaki komutu kullanın:

```bash
kubectl get pods --all-namespaces --include-uninitialized
```

Bu sorun, Azure Dev Spaces'in etkin olmadığı ad alanları da dahil olmak üzere kümedeki *tüm ad alanlarındaki* bölmeleri etkileyebilir.

Bu sorunu gidermek için [Dev Spaces CLI'yi en son sürüme güncelleştirin](./how-to/upgrade-tools.md#update-the-dev-spaces-cli-extension-and-command-line-tools) ve ardından Azure Dev Spaces denetleyicisinden *azds InitializerConfiguration'ı* silin:

```azurecli
az aks get-credentials --resource-group <resource group name> --name <cluster name>
```

```bash
kubectl delete InitializerConfiguration azds
```

*Azds InitializerConfiguration'ı* Azure Dev Spaces denetleyicisinden `kubectl delete` kaldırdıktan sonra, *Beklemedeki* durumdaki tüm bölmeleri kaldırmak için kullanın. Bekleyen tüm bölmeler kaldırıldıktan sonra, bölmelerinizi yeniden dağıtın.

Yeniden dağıtımdan sonra bekleyen *bir* durumda yeni bölmeler `kubectl delete` hala sıkışmışsa, *Bekleyen* durumdaki tüm kapsülleri kaldırmak için kullanın. Bekleyen tüm bölmeler kaldırıldıktan sonra denetleyiciyi kümeden silin ve yeniden yükleyin:

```bash
azds remove -g <resource group name> -n <cluster name>
azds controller create --name <cluster name> -g <resource group name> -tn <cluster name>
```

Denetleyiciniz yeniden yüklendikten sonra bölmelerinizi yeniden dağıtın.

### <a name="incorrect-rbac-permissions-for-calling-dev-spaces-controller-and-apis"></a>Dev Spaces denetleyicisi ve API'leri aramak için yanlış RBAC izinleri

Azure Dev Spaces denetleyicisine erişen kullanıcının AKS kümesindeki yönetici *kubeconfig'i* okuyabilmesi gerekir. Örneğin, bu izin yerleşik [Azure Kubernetes Hizmet Kümesi Yönetici Rolü'nde](../aks/control-kubeconfig-access.md#available-cluster-roles-permissions)kullanılabilir. Azure Geliştirme Alanları denetleyicisi'ne erişen kullanıcının denetleyici için *Katılımcı* veya *Sahip* RBAC rolüne de sahip olması gerekir. Bir AKS kümesi için kullanıcıizinlerini güncelleştirme hakkında daha fazla bilgi [burada](../aks/control-kubeconfig-access.md#assign-role-permissions-to-a-user-or-group)bulabilirsiniz.

Denetleyici için kullanıcının RBAC rolünü güncelleştirmek için:

1. https://portal.azure.com adresinden Azure portalında oturum açın.
1. Genellikle AKS kümenizle aynı olan denetleyiciyi içeren Kaynak Grubuna gidin.
1. Gizli *türleri göster* onay kutusunu etkinleştirin.
1. Kumandaya tıklayın.
1. Erişim *Denetimi (IAM)* bölmesini açın.
1. *Rol Atamaları* sekmesini tıklatın.
1. *Ekle'yi* tıklatın sonra *rol ataması ekle' yi*tıklatın.
    * *Rol* *için, Katkıda Bulunan* veya *Sahip'i*seçin.
    * *Erişim atamak için* *Azure AD kullanıcısı, grubu veya hizmet sorumlusunu*seçin.
    * *Select*için, izin vermek istediğiniz kullanıcıyı arayın.
1. *Kaydet*'e tıklayın.

### <a name="dns-name-resolution-fails-for-a-public-url-associated-with-a-dev-spaces-service"></a>DNS ad çözümlemesi, Dev Spaces hizmetiyle ilişkili genel bir URL için başarısız olur

Komuta `--enable-ingress` geçişi belirterek veya Visual Studio'daki `Publicly Accessible` onay kutusunu seçerek hizmetiniz için genel bir URL bitiş noktası yapılandırabilirsiniz. `azds prep` Dev Spaces'te hizmetinizi çalıştırdığınızda herkese açık DNS adı otomatik olarak kaydedilir. Bu DNS adı kayıtlı değilse, genel URL'ye bağlanırken bir *Sayfanın görüntülenemediğini* veya Site'ye web tarayıcınızda hata *erişilemediğini* görürsünüz.

Bu sorunu düzeltmek için:

* Dev Spaces hizmetlerinizle ilişkili tüm URL'lerin durumunu denetleyin:

  ```console
  azds list-uris
  ```

* *Bekleyen* durumda bir URL varsa, Dev Spaces hala DNS kaydının tamamlanmasını bekliyor. Bazen, kaydın tamamlanması birkaç dakika sürer. Dev Spaces ayrıca, DNS kaydını beklerken kullanabileceğiniz her hizmet için bir localhost tüneli de açar.
* Bir URL *Bekleme* durumunda 5 dakikadan fazla kalırsa, genel bitiş noktasını oluşturan dış DNS bölmesiyle ilgili bir sorun veya ortak bitiş noktasını alan nginx giriş denetleyici bölmesiyle ilgili bir sorun gösterebilir. Bu bölmeleri silmek ve AKS'nin bunları otomatik olarak yeniden oluşturmasına izin vermek için aşağıdaki komutları kullanın:
  ```console
  kubectl delete pod -n kube-system -l app=addon-http-application-routing-external-dns
  kubectl delete pod -n kube-system -l app=addon-http-application-routing-nginx-ingress
  ```

### <a name="error-upstream-connect-error-or-disconnectreset-before-headers"></a>Hata "upstream bağlantı hatası veya üstbilgi önce kesme / sıfırlama"

Hizmetinize erişmeye çalışırken bu hatayı görebilirsiniz. Örneğin, bir tarayıcıda hizmetin URL'sine gittiğinizde. Bu hata, kapsayıcı bağlantı noktasının kullanılamadığı anlamına gelir. Bu aşağıdaki nedenlerle olabilir:

* Kapsayıcı hala inşa edilme ve dağıtılma sürecindedir. Hata ayıklayıcıyı çalıştırır `azds up` veya çalıştırırsanız ve sonra kapsayıcıyı başarıyla dağıtmadan önce erişmeye çalışırsanız bu sorun ortaya çıkabilir.
* Bağlantı noktası yapılandırması _Dockerfile,_ Helm Chart ve bir bağlantı noktası açılır herhangi bir sunucu kodu arasında tutarlı değildir.

Bu sorunu düzeltmek için:

1. Kapsayıcı oluşturulma/dağıtılma sürecindeyse, 2-3 saniye bekleyebilir ve hizmete yeniden erişmeye çalışabilirsiniz. 
1. Aşağıdaki varlıklarda bağlantı noktası yapılandırmanızı denetleyin:
    * **[Miğfer grafiği](https://docs.helm.sh):** Komutla sayılsın `service.port` `deployment.containerPort` `azds prep` ve values.yaml tarafından belirtilir.
    * Uygulama kodunda açılan tüm bağlantı noktaları, örneğin Düğüm.js'de:`var server = app.listen(80, function () {...}`

### <a name="the-type-or-namespace-name-mylibrary-couldnt-be-found"></a>"MyLibrary" türü veya ad alanı adı bulunamadı

Kullandığınız bir kitaplık projesi bulunamıyor. Dev Spaces ile yapı bağlamı varsayılan olarak proje/hizmet düzeyindedir.  

Bu sorunu düzeltmek için:

1. Yapı `azds.yaml` bağlamını çözüm düzeyine ayarlamak için dosyayı değiştirin.
2. Proje `Dockerfile` dosyalarına başvurmak için dosyaları ve `Dockerfile.develop` `.csproj`dosyaları değiştirin, örneğin , yeni yapı bağlamına göre doğru şekilde.
3. Dosyayla `.dockerignore` aynı dizinde a ekleyin. `.sln`
4. Gerektiğinde `.dockerignore` ek girişleri ile güncelleştirin.

[Burada](https://github.com/sgreenmsft/buildcontextsample)bir örnek bulabilirsiniz.

### <a name="horizontal-pod-autoscaling-not-working-in-a-dev-space"></a>Dev alanda çalışmayan yatay bölme otomatik

Bir hizmeti geliştirme alanında çalıştırdığınızda, bu hizmetin bölmesine [enstrümantasyon için ek kapsayıcılar enjekte edilir](how-dev-spaces-works-cluster-setup.md#prepare-your-aks-cluster) ve bir bölmedeki tüm kapsayıcıların yatay pod otomatik ölçeklendirme için ayarlanmış kaynak sınırları ve istekleri olması gerekir.

Bu sorunu gidermek için bir kaynak isteği uygulayın ve enjekte edilen Dev Spaces kapsayıcılarına sınır uygulayın. Kaynak istekleri ve sınırları, bölme spektrumunuza ek açıklama eklenerek `azds.io/proxy-resources` enjekte edilen kapsayıcı (devspaces-proxy) için uygulanabilir. Değer, proxy için kapsayıcı spec'in kaynaklar bölümünü temsil eden bir JSON nesnesine ayarlanmalıdır.

Aşağıda, pod spec'inize uygulanacak proxy kaynakları ek açıklamasına bir örnek verilmiştir.
```
azds.io/proxy-resources: "{\"Limits\": {\"cpu\": \"300m\",\"memory\": \"400Mi\"},\"Requests\": {\"cpu\": \"150m\",\"memory\": \"200Mi\"}}"
```

### <a name="enable-azure-dev-spaces-on-an-existing-namespace-with-running-pods"></a>Çalışan bölmelerle varolan bir ad alanında Azure Dev Alanları'nı etkinleştirme

Azure Dev Spaces'i etkinleştirmek istediğiniz çalışan bölmeleri içeren varolan bir AKS kümeniz ve ad alanınız olabilir.

AkS kümesindeki varolan bir ad alanında Azure `use-dev-spaces` Dev `kubectl` Spaces'i etkinleştirmek için, bu ad alanındaki tüm bölmeleri yeniden başlatmak için çalıştırın ve kullanın.

```azurecli
az aks get-credentials --resource-group MyResourceGroup --name MyAKS
az aks use-dev-spaces -g MyResourceGroup -n MyAKS --space my-namespace --yes
```

```console
kubectl -n my-namespace delete pod --all
```

Bölmeleriniz yeniden başlatıldıktan sonra, varolan ad alanınızı Azure Dev Alanları ile kullanmaya başlayabilirsiniz.

### <a name="enable-azure-dev-spaces-on-aks-cluster-with-restricted-egress-traffic-for-cluster-nodes"></a>Küme düğümleri için sınırlı çıkış trafiğiyle AKS kümesinde Azure Dev Boşlukları etkinleştirme

Küme düğümlerinden çıkış trafiğinin kısıtlandığı bir AKS kümesinde Azure Dev Spaces'i etkinleştirmek için aşağıdaki FQDN'lere izin vermeniz gerekir:

| FQDN                                    | Bağlantı noktası      | Kullanım      |
|-----------------------------------------|-----------|----------|
| cloudflare.docker.com | HTTPS:443 SAYıLı | Linux Alpine ve diğer Azure Dev Spaces görüntülerini çekmek için |
| gcr.io | HTTP:443 SAYıLı | Dümen/çapa makinesi görüntülerini çekmek için|
| storage.googleapis.com | HTTP:443 SAYıLı | Dümen/çapa makinesi görüntülerini çekmek için|
| azds-<guid>. <location>.azds.io | HTTPS:443 SAYıLı | Denetleyiciniz için Azure Dev Spaces arka uç hizmetleriyle iletişim kurmak için. Tam FQDN bulunabilir "dataplaneFqdn" içinde %USERPROFILE%\.azds\settings.json|

### <a name="error-could-not-find-the-cluster-cluster-in-subscription-subscriptionid"></a>Hata "Abonelik aboneliğinde \<\> \<küme kümesini\>bulamadımId"

Kubeconfig dosyanız Azure Dev Spaces istemci tarafı aracıyla kullanmaya çalıştığınızdan farklı bir küme veya aboneliği hedefliyorsa bu hatayı görebilirsiniz. Azure Dev Spaces istemci tarafı aracı, kümeyi seçmek ve onunla iletişim kurmak için [bir veya daha fazla kubeconfig dosyası](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/) kullanan *kubectl*davranışını çoğaltır.

Bu sorunu düzeltmek için:

* Geçerli `az aks use-dev-spaces -g <resource group name> -n <cluster name>` bağlamı güncelleştirmek için kullanın. Bu komut, zaten etkin değilse AKS kümenizde Azure Dev Spaces'e de olanak tanır. Alternatif olarak, geçerli `kubectl config use-context <cluster name>` bağlamı güncelleştirmek için kullanabilirsiniz.
* Hedeflediğiniz geçerli Azure aboneliğini göstermek ve bunun doğru olduğunu doğrulamak için kullanın. `az account show` Hedeflediğiniz aboneliği kullanarak `az account set`değiştirebilirsiniz.

### <a name="error-using-dev-spaces-after-rotating-aks-certificates"></a>AKS sertifikalarını döndürdükten sonra Dev Spaces kullanma hatası

[AKS kümenizdeki sertifikaları döndürdükten](../aks/certificate-rotation.md)sonra, bazı `azds space list` `azds up` işlemler gibi ve başarısız olur. Ayrıca, kümenizdeki sertifikaları döndürdükten sonra Azure Dev Spaces denetleyicinizdeki sertifikaları yenilemeniz gerekir.

Bu sorunu gidermek için, *kubeconfig* sonra `az aks get-credentials` `azds controller refresh-credentials` komutu çalıştırarak güncelleştirilmiş sertifikaları olduğundan emin olun. Örnek:

```azurecli
az aks get-credentials -g <resource group name> -n <cluster name>
```

```console
azds controller refresh-credentials -g <resource group name> -n <cluster name>
```
