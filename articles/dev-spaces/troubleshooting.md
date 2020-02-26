---
title: Sorun giderme
services: azure-dev-spaces
ms.date: 09/25/2019
ms.topic: troubleshooting
description: Azure Dev Spaces etkinleştirirken ve kullanırken karşılaşılan yaygın sorunları giderme ve çözme hakkında bilgi edinin
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s '
ms.openlocfilehash: 0c6a712f1dfb4410f3eee0fbd0192c6147618f96
ms.sourcegitcommit: 163be411e7cd9c79da3a3b38ac3e0af48d551182
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77539595"
---
# <a name="azure-dev-spaces-troubleshooting"></a>Azure Dev Spaces sorunlarını giderme

Bu kılavuz, Azure geliştirme alanları kullanılırken olabilir sık karşılaşılan sorunlar hakkında bilgi içerir.

Azure Dev Spaces kullanırken bir sorununuz varsa, [Azure dev Spaces GitHub deposunda bir sorun](https://github.com/Azure/dev-spaces/issues)oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

Sorunları daha etkili bir şekilde gidermek için inceleme için daha ayrıntılı Günlükler oluşturmaya yardımcı olabilir.

Visual Studio uzantısı için `MS_VS_AZUREDEVSPACES_TOOLS_LOGGING_ENABLED` ortam değişkenini 1 olarak ayarlayın. Visual Studio ortam değişkeni için etkili olması için yeniden emin olun. Etkinleştirildikten sonra, ayrıntılı Günlükler `%TEMP%\Microsoft.VisualStudio.Azure.DevSpaces.Tools` dizininize yazılır.

CLı 'de, komut yürütme sırasında `--verbose` anahtarını kullanarak daha fazla bilgi alabilirsiniz. Ayrıca `%TEMP%\Azure Dev Spaces`daha ayrıntılı günlüklere de gidebilirsiniz. Bir Mac üzerinde, bir terminal penceresinden `echo $TMPDIR` çalıştırılarak GEÇICI dizin bulunabilir. Bir Linux bilgisayarda, GEÇICI dizin genellikle `/tmp`.

Azure Dev Spaces, tek bir örneğin veya Pod hata ayıklaması yaparken en iyi şekilde de geçerlidir. `azds.yaml` dosyası, Kubernetes 'in hizmetinize çalıştığı düğüm sayısını gösteren bir, *Replicacount*ayarı içerir. Uygulamanızı belirli bir hizmet için birden çok Pod çalıştıracak şekilde yapılandırmak üzere *replicacount* değerini değiştirirseniz, hata ayıklayıcı alfabetik olarak listelendiğinde ilk Pod 'a iliştirir. Özgün Pod geri dönüştürüldüğünde hata ayıklayıcı farklı bir pod 'a iliştirir, muhtemelen beklenmedik davranışa neden olur.

## <a name="common-issues-when-enabling-azure-dev-spaces"></a>Azure Dev Spaces etkinleştirilirken yaygın sorunlar

### <a name="error-failed-to-create-azure-dev-spaces-controller"></a>Hata "Azure Dev Spaces denetleyicisi oluşturulamadı"

Bir şeyler denetleyicisi oluşturulmasını yanlış gittiğinde şu hatayla karşılaşabilirsiniz. Geçici bir hatalarsa, onarmak için denetleyiciyi silin ve yeniden oluşturun.

Ayrıca, denetleyiciyi silmeyi de deneyebilirsiniz:

```bash
azds remove -g <resource group name> -n <cluster name>
```

Bir denetleyiciyi silmek için Azure Dev Spaces CLı kullanın. Bir denetleyiciyi Visual Studio 'dan silmek mümkün değildir. Ayrıca, Azure Cloud Shell bir denetleyiciyi silebilmek için Azure Cloud Shell Azure Dev Spaces CLı 'yı yükleyemezsiniz.

Azure Dev Spaces CLı yüklü değilse, önce aşağıdaki komutu kullanarak bu dosyayı yükleyebilir, sonra denetleyicinizi silebilirsiniz:

```cmd
az aks use-dev-spaces -g <resource group name> -n <cluster name>
```

Denetleyici yeniden CLI veya Visual Studio'dan yapılabilir. Örnekler için bkz. [.NET Core hızlı başlangıçlarla](quickstart-netcore-visualstudio.md) [Takım geliştirme](quickstart-team-development.md) veya geliştirme.

### <a name="controller-create-failing-because-of-controller-name-length"></a>Denetleyici adı uzunluğu nedeniyle denetleyici oluşturma başarısız oldu

Azure Dev Spaces denetleyicisinin adı 31 karakterden uzun olamaz. Bir AKS kümesinde dev alanlarını etkinleştirdiğinizde veya bir denetleyici oluşturduğunuzda denetleyicinin adı 31 karakteri aşarsa bir hata alırsınız. Örnek:

```console
Failed to create a Dev Spaces controller for cluster 'a-controller-name-that-is-way-too-long-aks-east-us': Azure Dev Spaces Controller name 'a-controller-name-that-is-way-too-long-aks-east-us' is invalid. Constraint(s) violated: Azure Dev Spaces Controller names can only be at most 31 characters long*
```

Bu sorunu onarmak için alternatif ada sahip bir denetleyici oluşturun. Örnek:

```cmd
azds controller create --name my-controller --target-name MyAKS --resource-group MyResourceGroup
```

### <a name="enabling-dev-spaces-failing-when-windows-node-pools-are-added-to-an-aks-cluster"></a>Bir AKS kümesine Windows düğüm havuzları eklendiğinde geliştirme alanlarını etkinleştirme başarısız oluyor

Şu anda, Azure Dev Spaces yalnızca Linux düğüm ve düğümleri üzerinde çalışmak üzere tasarlanmıştır. Windows düğüm havuzu içeren bir aks kümeniz varsa, Azure dev Spaces Pod 'nin yalnızca Linux düğümlerinde zamanlandığından emin olmanız gerekir. Bir Azure Dev Spaces Pod, bir Windows düğümünde çalışmak üzere zamanlanırsa, bu Pod başlatılmaz ve dev alanlarının etkinleştirilmesi başarısız olur.

Bu sorunu onarmak için, Linux 'ların bir Windows düğümünde çalıştırılmak üzere zamanlanmadığından emin olmak için AKS kümenize [bir Taint ekleyin](../aks/operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations) .

### <a name="error-found-no-untainted-linux-nodes-in-ready-state-on-the-cluster-there-needs-to-be-at-least-one-untainted-linux-node-in-ready-state-to-deploy-pods-in-azds-namespace"></a>Hata "kümede ayırma durumunda olmayan Linux düğümü bulunamadı. ' AZD ' ad alanında Pod 'leri dağıtmak için, uygun durumda en az bir adet bir Linux düğümü olması gerekir. "

Azure dev Spaces, aks kümenizde bir denetleyici oluşturamadı, çünkü Pod 'yi zamanlamak için serbest *bir durumda olmayan bir düğüm* bulunamadı. Azure Dev Spaces, kullanım sürelerinin toleranlarını belirtmeden zamanlamaya izin veren, *Ready* durumunda en az bir Linux düğümü gerektirir.

Bu sorunu giderecek en az bir Linux düğümünün tolerans belirtmeden zamanlamalara izin verdiğinden emin olmak için AKS kümenizde [taınt yapılandırmanızı güncelleştirin](../aks/operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations) . Ayrıca, tolerans belirtmeden düğüm zamanlamaya izin veren en az bir Linux düğümünün, *Ready* durumunda olduğundan emin olun. Düğümünüz, *hazırlık* durumuna ulaşmak uzun sürüyorsa, düğümünüz yeniden başlatmayı deneyebilirsiniz.

### <a name="error-azure-dev-spaces-cli-not-installed-properly-when-running-az-aks-use-dev-spaces"></a>`az aks use-dev-spaces` çalıştırılırken "Azure Dev Spaces CLı düzgün yüklenmemiş" hatası

CLı Azure Dev Spaces güncelleştirme, yükleme yolunu değiştirdi. Azure CLı 'nın 2.0.63 'den önceki bir sürümünü kullanıyorsanız, bu hatayı görebilirsiniz. Azure CLı sürümünüzü göstermek için `az --version`kullanın.

```bash
$ az --version
azure-cli                         2.0.60 *
...
```

2\.0.63 önce Azure CLı sürümü ile `az aks use-dev-spaces` çalıştırırken hata iletisine rağmen yükleme başarılı olur. Herhangi bir sorun olmadan `azds` kullanmaya devam edebilirsiniz.

Bu sorunu onarmak için [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) yüklemenizi 2.0.63 veya üzeri bir sürüme güncelleştirin. Bu güncelleştirme, `az aks use-dev-spaces`çalıştırılırken aldığınız hata mesajını çözmeyecektir. Alternatif olarak, geçerli Azure CLı sürümünüzü ve Azure Dev Spaces CLı 'yi kullanmaya devam edebilirsiniz.

### <a name="error-unable-to-reach-kube-apiserver"></a>Hata "Kuto-apiserver 'e ulaşılamıyor"

Azure Dev Spaces AKS kümenizin API sunucusuna bağlanamadığınızda bu hatayla karşılaşabilirsiniz. 

AKS kümesi API sunucunuza erişim kilitliyse veya AKS kümeniz için etkinleştirilmiş [API sunucusu YETKILENDIRILMIŞ IP adresi aralıklarına](../aks/api-server-authorized-ip-ranges.md) sahipseniz, [bölgeniz temelinde ek aralıklara izin vermek](https://github.com/Azure/dev-spaces/tree/master/public-ips)için kümenizi [oluşturmanız](../aks/api-server-authorized-ip-ranges.md#create-an-aks-cluster-with-api-server-authorized-ip-ranges-enabled) veya [güncelleştirmeniz](../aks/api-server-authorized-ip-ranges.md#update-a-clusters-api-server-authorized-ip-ranges) gerekir.

Kubectl komutlarını çalıştırarak API sunucusunun kullanılabilir olduğundan emin olun. API sunucusu kullanılamıyorsa, lütfen AKS desteğiyle iletişim kurun ve API sunucusu çalışırken yeniden deneyin.

## <a name="common-issues-when-preparing-your-project-for-azure-dev-spaces"></a>Projenizi Azure Dev Spaces hazırlarken yaygın sorunlar

### <a name="warning-dockerfile-could-not-be-generated-due-to-unsupported-language"></a>Uyarı "desteklenmeyen dil nedeniyle Dockerfile üretilemedi"
Azure geliştirme alanları, C# ve Node.js için yerel destek sağlar. Bu dillerden birinde yazılan kodla `azds prep` bir dizinde çalıştırdığınızda Azure Dev Spaces otomatik olarak uygun bir Dockerfile oluşturur.

Azure Dev Spaces başka dillerde yazılmış kodla kullanmaya devam edebilirsiniz, ancak `azds up` ilk kez çalıştırmadan önce Dockerfile 'ı el ile oluşturmanız gerekir.

Uygulamanız Azure Dev Spaces yerel olarak desteklenmeyen bir dilde yazılmışsa, kodunuzu çalıştıran bir kapsayıcı görüntüsü oluşturmak için uygun bir Dockerfile sağlamanız gerekir. Docker, Dockerfiles ve gereksinimlerinize uygun bir Dockerfile başvurusu yazmanıza yardımcı olabilecek bir [dockerfile başvurusu](https://docs.docker.com/engine/reference/builder/) [yazmak için en iyi yöntemlerin bir listesini](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) sağlar.

Uygun bir Dockerfile varsa, uygulamanızı Azure Dev Spaces çalıştırmak için `azds up` çalıştırırsınız.

## <a name="common-issues-when-starting-or-stopping-services-with-azure-dev-spaces"></a>Azure Dev Spaces ile hizmetleri başlatırken veya durdurulurken karşılaşılan yaygın sorunlar

### <a name="error-config-file-not-found"></a>"Yapılandırma dosyası bulunamadı:" hatası

`azds up`çalıştırılırken, bu hatayı görebilirsiniz. Hem `azds up` hem de `azds prep`, geliştirme alanınızda çalıştırmak istediğiniz projenin kök dizininden çalıştırılmalıdır.

Bu sorunu düzeltmek için:
1. Geçerli dizin hizmeti kodunuzu içeren kök klasöre değiştirin. 
1. Kod klasöründe _azds. YAML_ dosyanız yoksa Docker, Kubernetes ve Azure dev Spaces varlıkları oluşturmak için `azds prep` çalıştırın.

### <a name="timeout-at-waiting-for-container-image-build-step-with-aks-virtual-nodes"></a>"Kapsayıcı görüntüsü derlemesi bekleniyor..." saatinde zaman aşımı AKS sanal düğümleri ile adımla

Bu zaman aşımı, bir [aks sanal düğümünde](https://docs.microsoft.com/azure/aks/virtual-nodes-portal)çalışacak şekilde yapılandırılmış bir hizmeti çalıştırmak Için geliştirme alanlarını kullanmaya çalıştığınızda oluşur. Geliştirme alanları şu anda sanal düğümlerde Hizmetleri oluşturmayı veya hata ayıklamayı desteklememektedir.

`azds up` `--verbose` anahtarla çalıştırırsanız veya Visual Studio 'da ayrıntılı günlük kaydını etkinleştirirseniz, ek ayrıntılar görürsünüz:

```cmd
$ azds up --verbose

Installed chart in 2s
Waiting for container image build...
pods/mywebapi-76cf5f69bb-lgprv: Scheduled: Successfully assigned default/mywebapi-76cf5f69bb-lgprv to virtual-node-aci-linux
Streaming build container logs for service 'mywebapi' failed with: Timed out after 601.3037572 seconds trying to start build logs streaming operation. 10m 1s
Container image build failed
```

Yukarıdaki komut, hizmetin Pod öğesinin sanal düğüm *-aci-Linux*'a atandığını gösterir ve bu sanal bir düğümdür.

Bu sorunu onarmak için, hizmetin bir sanal düğümde çalışmasına izin veren tüm *Nodeselector* veya *toleranations* değerlerini kaldırmak üzere hizmetin HELI grafiğini güncelleştirin. Bu değerler genellikle grafiğin `values.yaml` dosyasında tanımlanmıştır.

Sanal düğümler özelliği etkinleştirilmiş bir AKS kümesini kullanmaya devam edebilirsiniz, geliştirme alanları aracılığıyla derlemek veya hata ayıklamak istediğiniz hizmet bir VM düğümünde çalışır. Bir VM düğümünde dev alanları olan bir hizmetin çalıştırılması varsayılan yapılandırmadır.

### <a name="error-could-not-find-a-ready-tiller-pod-when-launching-dev-spaces"></a>Dev alanları başlatılırken "Ready Tiller Pod bulunamadı" hatası

Helm istemci artık kümede çalışan Tiller pod konuşabilirsiniz değilse bu hata oluşur.

Bu sorunu onarmak için kümenizdeki aracı düğümlerini yeniden başlatın.

### <a name="error-release-azds-identifier-spacename-servicename-failed-services-servicename-already-exists-or-pull-access-denied-for-servicename-repository-does-not-exist-or-may-require-docker-login"></a>Hata "" azds-\<tanımlayıcı\>-\<spacename\>-\<ServiceName\> başarısız oldu: '\<ServiceName\>' Hizmetleri zaten var "veya" \<ServiceName\>için çekme erişimi reddedildi, depo yok veya ' Docker login' gerektirebilir.

Bu hatalar, aynı geliştirme alanı içinde dev Spaces komutlarıyla (örneğin, `azds up` ve `azds down`) çalışan doğrudan HELD komutlarını (`helm install`, `helm upgrade`veya `helm delete`) karıştırabilmeniz durumunda ortaya çıkabilir. Bu durum, dev Spaces 'ın aynı geliştirme alanında çalışan kendi Tiller örneğinizle çakışan kendi Tiller örneğine sahip olduğu için oluşur.

Aynı AKS kümesinde hem HELI komutları hem de dev Spaces komutlarının kullanılması ince, ancak her dev Spaces özellikli ad alanı birini ya da diğerini kullanmalıdır.

Örneğin, tüm uygulamanızı bir üst geliştirme alanında çalıştırmak için Held komutunu kullandığınızı varsayalım. Bu üst öğeden alt dev alanları oluşturabilir, her bir hizmeti alt geliştirme alanında çalıştırmak için geliştirme alanlarını kullanabilir ve Hizmetleri birlikte test edebilirsiniz. Değişikliklerinizi iade etmeye hazırsanız, güncelleştirilmiş kodu üst geliştirme alanına dağıtmak için Held komutunu kullanın. Ana geliştirme alanında güncelleştirilmiş hizmeti çalıştırmak için `azds up` kullanmayın. bu hizmet başlangıçta Held kullanarak çalışır.

### <a name="existing-dockerfile-not-used-to-build-a-container"></a>Mevcut Dockerfile bir kapsayıcı oluşturmak için kullanılmıyor

Azure Dev Spaces, projenizdeki belirli bir _Dockerfile_ 'ı işaret etmek için yapılandırılabilir. Kapsayıcınız derlemek istediğiniz _dockerfile_ ' ı Azure dev Spaces görünürse, hangi dockerfile 'ın kullanılacağını açıkça Azure dev Spaces söylemeniz gerekebilir. 

Bu sorunu gidermeye yönelik Azure Dev Spaces, projenizde oluşturulan _azds. YAML_ dosyasını açın. Güncelleştirme *yapılandırması: geliştirme: oluşturma: dockerfile* kullanmak Istediğiniz dockerfile 'ı işaret etmek için. Örnek:

```yaml
...
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
```

### <a name="error-unauthorized-authentication-required-when-trying-to-use-a-docker-image-from-a-private-registry"></a>Hata "yetkisiz: kimlik doğrulaması gerekiyor" özel bir kayıt defterinden Docker görüntüsünü kullanmaya çalışırken

Kimlik doğrulaması gerektiren özel bir kayıt defterinden Docker görüntüsü kullanıyorsunuz.

Bu sorunu onarmak için, geliştirici alanlarının, [ımagepullgizlilikler](https://kubernetes.io/docs/concepts/configuration/secret/#using-imagepullsecrets)kullanarak bu özel kayıt defterinden görüntü doğrulamasına ve çekmelerine izin verebilirsiniz. Imagepullgizliliklerini kullanmak için, görüntüyü kullandığınız ad alanında [bir Kubernetes gizli anahtarı oluşturun](https://kubernetes.io/docs/concepts/containers/images/#specifying-imagepullsecrets-on-a-pod) . Ardından `azds.yaml`bir ımagepullsecret olarak gizli dizi girin.

Aşağıda, `azds.yaml`ımagepullgizliliklerin belirtilmesine ilişkin bir örnek verilmiştir.

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
> `azds.yaml` ımagepullgizliliklerin ayarlanması, `values.yaml`belirtilen ımagepullgizliliklerin üzerine yazar.

### <a name="error-service-cannot-be-started"></a>"Hizmet başlatılamıyor" hatası.

Başlatmak hizmet kodunuzu başarısız olduğunda bu hatayı görebilirsiniz. Genellikle kullanıcı kodunda nedenidir. Daha fazla tanılama bilgisi edinmek için, hizmetinizi başlatırken daha ayrıntılı günlük kaydını etkinleştirin.

Komut satırından, daha ayrıntılı günlük kaydı sağlamak için `--verbose` kullanın. Ayrıca, `--output`kullanarak bir çıktı biçimi de belirtebilirsiniz. Örnek:

```cmd
azds up --verbose --output json
```

Visual Studio'da:

1. **Araçlar > seçenekler** ' i açın ve **Projeler ve çözümler**altında **Oluştur ve Çalıştır**' ı seçin.
2. **MSBuild proje derlemesi çıkış ayrıntı düzeyi** ayarını **ayrıntılı** veya **Tanılama**olarak değiştirin.

    ![Ekran Araçlar, Seçenekler iletişim kutusu](media/common/VerbositySetting.PNG)

### <a name="rerunning-a-service-after-controller-re-creation"></a>Denetleyici yeniden oluşturulduktan sonra bir hizmeti yeniden çalıştırma

Bir hizmeti kaldırdıktan sonra yeniden çalıştırmayı denerken bir *Hizmet başlatılamaz* ve bu kümeyle ilişkili Azure dev Spaces denetleyiciyi yeniden oluşturursunuz. Bu durumda, ayrıntılı çıktı aşağıdaki metni içerir:

```cmd
Installing Helm chart...
Release "azds-33d46b-default-webapp1" does not exist. Installing it now.
Error: release azds-33d46b-default-webapp1 failed: services "webapp1" already exists
Helm install failed with exit code '1': Release "azds-33d46b-default-webapp1" does not exist. Installing it now.
Error: release azds-33d46b-default-webapp1 failed: services "webapp1" already exists
```

Bu hata, dev Spaces denetleyicisinin kaldırılması bu denetleyici tarafından daha önce yüklenen hizmetleri kaldırmadığı için oluşur. Eski Hizmetleri yerinde olduğundan denetleyicisi yeniden oluşturma ve ardından yeni denetleyicisi kullanarak hizmetlerini çalıştırma denemesi başarısız olur.

Bu sorunu gidermek için `kubectl delete` komutunu kullanarak eski Hizmetleri kümeinizden el ile kaldırın ve ardından yeni hizmetleri yüklemek için geliştirme alanlarını yeniden çalıştırın.

### <a name="error-service-cannot-be-started-when-using-multi-stage-dockerfiles"></a>"Hizmet başlatılamıyor" hatası. Multi-Stage Dockerfiles kullanılırken

Bir çok aşamalı Dockerfile kullanılırken bir *hizmet başlatılamıyor* hatası alıyorsunuz. Bu durumda, ayrıntılı çıktı aşağıdaki metni içerir:

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

Bu hata, Azure Dev Spaces Şu anda çok aşamalı derlemeleri desteklemediğinden oluşur. Çok aşamalı derlemelerin önüne geçmek için Dockerfile dosyanızı yeniden yazın.

### <a name="network-traffic-is-not-forwarded-to-your-aks-cluster-when-connecting-your-development-machine"></a>Geliştirme makinenizi bağlarken ağ trafiği AKS kümenize iletilmez

[AKS kümenizi geliştirme makinenize bağlamak için Azure dev Spaces](how-to/connect.md)kullanırken, ağ trafiğinin geliştirme makineniz ve aks kümeniz arasında iletilemediği bir sorunla karşılaşabilirsiniz.

Geliştirme makinenizi AKS kümenize bağlarken, geliştirme makinenizin `hosts` dosyasını değiştirerek AKS kümeniz ile geliştirme makineniz arasında ağ trafiğini iletir Azure Dev Spaces. Azure Dev Spaces, ana bilgisayar adı olarak değiştirdiğiniz Kubernetes hizmetinin adresiyle `hosts` bir giriş oluşturur. Bu giriş, geliştirme makineniz ve AKS kümesi arasında ağ trafiğini yönlendirmek için bağlantı noktası iletme ile kullanılır. Geliştirme makinenizdeki bir hizmet, değiştirdiğiniz Kubernetes hizmetinin bağlantı noktasıyla çakışıyorsa, Azure Dev Spaces Kubernetes hizmeti için ağ trafiğini iletemez. Örneğin, *Windows BranchCache* hizmeti genellikle *0.0.0.0:80*' e bağlanır, bu da çakışmalar tüm yerel ıp 'lerde bağlantı noktası 80 ' de çakışmaya neden olur.

Bu sorunu onarmak için, değiştirmeye çalıştığınız Kubernetes hizmetinin bağlantı noktasıyla çakışan tüm hizmetleri veya süreçlerini durdurmanız gerekir. Geliştirme makinenizde hangi hizmetlerin veya işlemlerin çakışıp çakışmadığını denetlemek için *netstat*gibi araçları kullanabilirsiniz.

Örneğin, *Windows BranchCache* hizmetini durdurmak ve devre dışı bırakmak için:
* Komut isteminden `services.msc` çalıştırın.
* *BranchCache* ' e sağ tıklayın ve *Özellikler*' i seçin.
* *Durdur*' a tıklayın.
* İsteğe bağlı olarak, *Başlangıç türünü* *devre dışı*olarak ayarlayarak devre dışı bırakabilirsiniz.
* *Tamam* düğmesine tıklayın.

## <a name="common-issues-using-visual-studio-and-visual-studio-code-with-azure-dev-spaces"></a>Azure Dev Spaces ile Visual Studio ve Visual Studio Code kullanma hakkında genel sorunlar

### <a name="error-required-tools-and-configurations-are-missing"></a>"Gerekli araçlar ve Konfigürasyonlar eksik" hatası

VS Code başlatırken bu hata oluşabilir: "[Azure geliştirme alanları] araçları ve derleme ve '[Proje adı]' hata ayıklama yapılandırmaları gerekli eksik."
Hata, o azds.exe yol ortam değişkeninde değil VS Code'da görüldüğü anlamına gelir.

PATH ortam değişkeninin düzgün ayarlandığı bir komut isteminden VS Code başlatmayı deneyin.

### <a name="error-required-tools-to-build-and-debug-projectname-are-out-of-date"></a>Hata "derleme ve hata ayıklama için gerekli araçlar ' ProjectName ' güncel değil."

Azure Dev Spaces için VS Code uzantısının daha yeni bir sürümüne sahipseniz, ancak Azure Dev Spaces CLı 'nin daha eski bir sürümü olan bu hatayı Visual Studio Code görürsünüz.

Azure Dev Spaces CLı 'nın en son sürümünü indirip yüklemeyi deneyin:

* [Windows](https://aka.ms/get-azds-windows)
* [Mac](https://aka.ms/get-azds-mac)
* [Linux](https://aka.ms/get-azds-linux)

### <a name="error-failed-to-find-debugger-extension-for-typecoreclr"></a>Hata: "tür için hata ayıklayıcı uzantısı bulunamadı: CoreCLR"

Visual Studio Code hata ayıklayıcıyı çalıştırırken bu hatayı görebilirsiniz. Geliştirme makinenizde C# yüklü vs Code uzantısı olmayabilir. Uzantı C# , .NET Core (CoreCLR) için hata ayıklama desteği içerir.

Bu sorunu onarmak için [vs Code uzantısını C# ](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)' e yüklemelisiniz.

### <a name="error-configured-debug-type-coreclr-is-not-supported"></a>"Yapılandırılan hata ayıklama türü ' CoreCLR ' hatası desteklenmiyor"

Visual Studio Code hata ayıklayıcıyı çalıştırırken bu hatayı görebilirsiniz. Geliştirme makinenizde yüklü Azure Dev Spaces için VS Code uzantısına sahip olmayabilirsiniz.

Bu sorunu giderecek [Azure dev Spaces için vs Code uzantısını](get-started-netcore.md)yüklersiniz.

### <a name="error-invalid-cwd-value-src-the-system-cannot-find-the-file-specified-or-launch-program-srcpath-to-project-binary-does-not-exist"></a>Hata "geçersiz ' CWD ' değeri '/src '. Sistem belirtilen dosyayı bulamıyor." veya "Başlat: '/ src / [Proje ikili dosya yolu]' programı yok"

Visual Studio Code hata ayıklayıcıyı çalıştırırken bu hatayı görebilirsiniz. Varsayılan olarak, VS Code uzantısı kapsayıcıda proje için çalışma dizini olarak `src` kullanır. `Dockerfile` farklı bir çalışma dizini belirtmek üzere güncelleştirdiyseniz, bu hatayı görebilirsiniz.

Bu sorunu onarmak için, proje klasörünüzün `.vscode` alt dizininde bulunan `launch.json` dosyasını güncelleştirin. `configurations->cwd` yönergesini, projenizin `Dockerfile`tanımlanan `WORKDIR` aynı dizine işaret etmek üzere değiştirin. Ayrıca `configurations->program` yönergesini de güncelleştirmeniz gerekebilir.

### <a name="error-the-pipe-program-azds-exited-unexpectedly-with-code-126"></a>Hata "' azds ' Kanal programının kodu 126 ile beklenmedik şekilde çıktı."

Visual Studio Code hata ayıklayıcıyı çalıştırırken bu hatayı görebilirsiniz.

Bu sorunu onarmak için Visual Studio Code kapatıp yeniden açın. Hata ayıklayıcıyı yeniden başlatın.

### <a name="error-internal-watch-failed-watch-enospc-when-attaching-debugging-to-a-nodejs-application"></a>Bir Node. js uygulamasına hata ayıklama eklenirken "Iç izleme başarısız oldu: {ENOSPC" izleme

Bu hata, bir hata ayıklayıcı ile eklemeye çalıştığınız Node. js uygulamasıyla Pod 'ı çalıştıran düğüm, *FS. inotify. max_user_watches* değerini aşmış olduğunda oluşur. Bazı durumlarda, [varsayılan *FS. inotify. max_user_watches* değeri bir hata ayıklayıcıyı doğrudan Pod 'a eklemeyi işlemek için çok küçük olabilir](https://github.com/Azure/AKS/issues/772).

Bu soruna yönelik geçici bir geçici çözüm, kümedeki her bir düğümde *FS. inotify. max_user_watches* değerini artırmanız ve değişikliklerin etkili olması için bu düğümü yeniden başlatvermektir.

## <a name="other-common-issues"></a>Diğer yaygın sorunlar

### <a name="error-azds-is-not-recognized-as-an-internal-or-external-command-operable-program-or-batch-file"></a>"AZD" hatası iç veya dış komut, çalıştırılabilir program veya toplu iş dosyası olarak tanınmıyor

`azds.exe` yüklü değilse veya doğru yapılandırılmamışsa bu hata oluşabilir.

Bu sorunu düzeltmek için:

1. `azds.exe`için% ProgramFiles%/Microsoft SDKs\Azure\Azure dev Spaces CLı konumunu denetleyin. Yoktur, bu konuma PATH ortam değişkenine ekleyin.
2. `azds.exe` yüklü değilse, aşağıdaki komutu çalıştırın:

    ```cmd
    az aks use-dev-spaces -n <cluster-name> -g <resource-group>
    ```

### <a name="authorization-error-microsoftdevspacesregisteraction"></a>Yetkilendirme hatası "Microsoft. DevSpaces/Register/Action"

Azure Dev Spaces yönetmek için Azure aboneliğinizde *sahip* veya *katkıda bulunan* erişime ihtiyacınız vardır. Dev alanlarını yönetmeye çalışıyorsanız ve ilişkili Azure aboneliğine *sahip* veya *katkıda bulunan* erişiminiz yoksa bir yetkilendirme hatası görebilirsiniz. Örnek:

```console
The client '<User email/Id>' with object id '<Guid>' does not have authorization to perform action 'Microsoft.DevSpaces/register/action' over scope '/subscriptions/<Subscription Id>'.
```

Bu sorunu onarmak için, Azure aboneliğine *sahip* veya *katkıda* bulunan erişimi olan bir hesap kullanarak `Microsoft.DevSpaces` ad alanını el ile kaydedin:

```console
az provider register --namespace Microsoft.DevSpaces
```

### <a name="new-pods-arent-starting"></a>Yeni Pod başlamıyor

Kubernetes başlatıcısı, kümedeki *Küme Yöneticisi* rolünde RBAC izin değişiklikleri nedeniyle yeni pod için pod belirtimini uygulayamaz. Yeni Pod 'ın geçersiz bir pod özelliği olabilir, örneğin Pod ile ilişkili hizmet hesabı artık yok. Başlatıcı sorunu nedeniyle *bekleyen* bir durumda olan Pod 'leri görmek için `kubectl get pods` komutunu kullanın:

```bash
kubectl get pods --all-namespaces --include-uninitialized
```

Bu sorun, Azure Dev Spaces etkinleştirilmemiş olan ad alanları da dahil olmak üzere kümedeki *tüm ad alanlarında* yer işareti etkileyebilir.

Bu sorunu onarmak için, [dev Spaces CLI 'yı en son sürüme güncelleştirin](./how-to/upgrade-tools.md#update-the-dev-spaces-cli-extension-and-command-line-tools) ve ardından Azure dev Spaces denetleyicisinden *azds ınitializerconfiguration* ' ı silin:

```bash
az aks get-credentials --resource-group <resource group name> --name <cluster name>
kubectl delete InitializerConfiguration azds
```

*Azds ınitializerconfiguration* 'ı Azure dev Spaces denetleyicisinden kaldırdıktan sonra, *bekleyen* bir durumdaki tüm yığınları kaldırmak için `kubectl delete` kullanın. Tüm bekleyen Pod 'ler kaldırıldıktan sonra, yığınlarınızı yeniden dağıtın.

Yeni yığınların yeniden dağıtım sonrasında *bekleyen* bir durumda takılmasına devam ediyorsa, *bekleyen* bir durumdaki tüm yığınları kaldırmak için `kubectl delete` kullanın. Tüm bekleyen Pod 'ler kaldırıldıktan sonra, denetleyiciyi kümeden silin ve yeniden yükleyin:

```bash
azds remove -g <resource group name> -n <cluster name>
azds controller create --name <cluster name> -g <resource group name> -tn <cluster name>
```

Denetleyicinizin yeniden yüklenmesi sırasında, yığınlarınızı yeniden dağıtın.

### <a name="incorrect-rbac-permissions-for-calling-dev-spaces-controller-and-apis"></a>Dev Spaces denetleyicisi ve API 'Leri çağırmak için yanlış RBAC izinleri

Azure Dev Spaces denetleyicisine erişen kullanıcının AKS kümesinde admin *kubeconfig* 'i okumak için erişimi olmalıdır. Örneğin, bu izin [yerleşik Azure Kubernetes hizmet kümesi yönetici rolünde](../aks/control-kubeconfig-access.md#available-cluster-roles-permissions)bulunur. Azure Dev Spaces denetleyicisine erişen kullanıcının denetleyici için *katkıda* bulunan veya *sahip* RBAC rolüne de sahip olması gerekir. Bir kullanıcının bir AKS kümesi için izinlerini güncelleştirme hakkında daha fazla ayrıntıya [buradan](../aks/control-kubeconfig-access.md#assign-role-permissions-to-a-user-or-group)ulaşabilirsiniz.

Denetleyicinin Kullanıcı RBAC rolünü güncelleştirmek için:

1. https://portal.azure.com adresinden Azure portalında oturum açın.
1. Genellikle AKS kümeniz ile aynı olan denetleyiciyi içeren kaynak grubuna gidin.
1. *Gizli türleri göster* onay kutusunu etkinleştirin.
1. Denetleyiciye tıklayın.
1. *Access Control (IAM)* bölmesini açın.
1. *Rol atamaları* sekmesine tıklayın.
1. *Ekle* ' ye tıklayın ve *rol ataması ekleyin*.
    * *Rol*Için, *katkıda bulunan* veya *sahip*seçeneklerinden birini belirleyin.
    * *Erişim atama*Için, *Azure AD Kullanıcı, Grup veya hizmet sorumlusu*' nı seçin.
    * *Seç*için izin vermek istediğiniz kullanıcıyı arayın.
1. *Kaydet* düğmesine tıklayın.

### <a name="dns-name-resolution-fails-for-a-public-url-associated-with-a-dev-spaces-service"></a>Geliştirme alanları hizmeti ile ilişkilendirilen genel bir URL için DNS adı çözümlemesi başarısız olur

`azds prep` komutuna `--enable-ingress` anahtarını belirterek veya Visual Studio 'daki `Publicly Accessible` onay kutusunu seçerek hizmetiniz için genel bir URL uç noktası yapılandırabilirsiniz. Genel DNS adı, hizmetinizi geliştirme alanlarında çalıştırdığınızda otomatik olarak kaydedilir. Bu DNS adı kayıtlı değilse, genel URL 'ye bağlanılırken bir *sayfa görüntülenemiyor* veya Web tarayıcınızda *siteye ulaşılamıyor* hatası görürsünüz.

Bu sorunu düzeltmek için:

* Dev Spaces hizmetlerinizle ilişkili tüm URL 'Lerin durumunu kontrol edin:

  ```console
  azds list-uris
  ```

* URL *bekleme* durumundaysa, dev Spaces hala DNS kaydının tamamlanmasını bekliyor. Bazı durumlarda, kayıt tamamlanması birkaç dakika sürer. Geliştirme alanları localhost tünel DNS kaydında beklenirken kullanabileceğiniz her hizmet için de açılır.
* URL, 5 dakikadan uzun bir süre boyunca *bekleme* durumunda kalırsa, genel uç noktasını veya genel uç noktasını elde eden NGINX giriş denetleyicisi Pod 'sini oluşturan dış DNS Pod ile ilgili bir sorun olduğunu gösterebilir. Bu Pod 'yi silmek ve aks 'lerin otomatik olarak yeniden oluşturmasını sağlamak için aşağıdaki komutları kullanın:
  ```console
  kubectl delete pod -n kube-system -l app=addon-http-application-routing-external-dns
  kubectl delete pod -n kube-system -l app=addon-http-application-routing-nginx-ingress
  ```

### <a name="error-upstream-connect-error-or-disconnectreset-before-headers"></a>Hata "yukarı akış bağlantı hatası veya üst bilgilerden önce bağlantıyı kes/Sıfırla"

Hizmetinizi erişmeye çalışırken bu hatayı görebilirsiniz. Örneğin, ne zaman, hizmetin URL'sini bir tarayıcıda gidin. Bu hata kapsayıcı bağlantı noktasının kullanılamadığı anlamına gelir. Bu, aşağıdaki nedenlerden kaynaklanabilir:

* , Yine yerleşik dağıtılan ve sürecinde kapsayıcıdır. Bu sorun, `azds up` çalıştırırsanız veya hata ayıklayıcıyı başlatırsanız ve sonra başarılı bir şekilde dağıtılmadan önce kapsayıcıya erişmeyi denerseniz ortaya çıkabilir.
* Bağlantı noktası yapılandırması, _Dockerfile_, Helmchart ve bir bağlantı noktasını açan tüm sunucu kodları arasında tutarlı değildir.

Bu sorunu düzeltmek için:

1. Kapsayıcı yerleşik/dağıtılan aşamasında olan 2-3 saniye bekleyin ve hizmete tekrar erişmeyi deneyin. 
1. Bağlantı noktası yapılandırmanızı denetleyin. Belirtilen bağlantı noktası numaraları aşağıdaki varlıkların tümünde **özdeş** olmalıdır:
    * **Dockerfile:** `EXPOSE` yönergesi tarafından belirtilen.
    * **[Helb grafiği](https://docs.helm.sh):** `externalPort` ve bir hizmetin `internalPort` değerleriyle (genellikle `values.yml` dosyasında bulunur) belirtilir,
    * Uygulama kodunda açılan tüm bağlantı noktaları, örneğin Node. js: `var server = app.listen(80, function () {...}`

### <a name="the-type-or-namespace-name-mylibrary-couldnt-be-found"></a>"MyLibrary" tür veya ad alanı adı bulunamadı

Kullanmakta olduğunuz bir kitaplık projesi bulunamıyor. Geliştirme alanları ile derleme bağlamı varsayılan olarak proje/hizmet düzeyindedir.  

Bu sorunu düzeltmek için:

1. Yapı bağlamını çözüm düzeyine ayarlamak için `azds.yaml` dosyasını değiştirin.
2. `Dockerfile` ve `Dockerfile.develop` dosyalarını proje dosyalarına başvuracak şekilde değiştirin, örneğin `.csproj`, yeni derleme bağlamına doğru göreli olarak.
3. `.sln` dosyası ile aynı dizine `.dockerignore` ekleyin.
4. `.dockerignore` gerektiğinde ek girişlerle güncelleştirin.

[Burada](https://github.com/sgreenmsft/buildcontextsample)bir örnek bulabilirsiniz.

### <a name="horizontal-pod-autoscaling-not-working-in-a-dev-space"></a>Yatay Pod otomatik ölçeklendirme bir geliştirme alanında çalışmıyor

Bir hizmeti bir geliştirme alanında çalıştırdığınızda, bu hizmetin Pod 'ı, [izleme için ek kapsayıcılarla birlikte](how-dev-spaces-works.md#prepare-your-aks-cluster) eklenir ve pod içindeki tüm kapsayıcılar, yatay Pod otomatik ölçeklendirme için kaynak limitlerinin ve isteklerin ayarlanmış olması gerekir.

Bu sorunu onarmak için bir kaynak isteği uygulayın ve eklenen dev Spaces kapsayıcılarıyla sınırlayın. Pod belirtimine `azds.io/proxy-resources` ek açıklaması eklenerek eklenen kapsayıcı (devspaces-proxy) için kaynak istekleri ve sınırları uygulanabilir. Değer, proxy için kapsayıcı belirtiminin kaynaklar bölümünü temsil eden bir JSON nesnesi olarak ayarlanmalıdır.

Pod belirtimde uygulanacak bir proxy kaynakları ek açıklamasına örnek aşağıda verilmiştir.
```
azds.io/proxy-resources: "{\"Limits\": {\"cpu\": \"300m\",\"memory\": \"400Mi\"},\"Requests\": {\"cpu\": \"150m\",\"memory\": \"200Mi\"}}"
```

### <a name="enable-azure-dev-spaces-on-an-existing-namespace-with-running-pods"></a>Var olan bir ad alanı üzerinde çalışan Pod ile Azure dev Spaces etkinleştirme

Azure Dev Spaces etkinleştirmek istediğiniz yerde, çalışan bir AKS kümeniz ve ad alanınız olabilir.

AKS kümesindeki mevcut bir ad alanında Azure Dev Spaces etkinleştirmek için, `use-dev-spaces` çalıştırın ve bu ad alanındaki tüm yığınların yeniden başlatılması için `kubectl` kullanın.

```console
az aks get-credentials --resource-group MyResourceGroup --name MyAKS
az aks use-dev-spaces -g MyResourceGroup -n MyAKS --space my-namespace --yes
kubectl -n my-namespace delete pod --all
```

Yığınlarınız yeniden başlatıldıktan sonra, Azure Dev Spaces var olan ad alanınızı kullanmaya başlayabilirsiniz.

### <a name="enable-azure-dev-spaces-on-aks-cluster-with-restricted-egress-traffic-for-cluster-nodes"></a>Küme düğümlerinde kısıtlanmış çıkış trafiği ile AKS kümesinde Azure Dev Spaces etkinleştirme

Küme düğümlerinden gelen çıkış trafiğinin kısıtlandığı bir AKS kümesinde Azure Dev Spaces etkinleştirmek için aşağıdaki FQDN 'Lere izin vermeniz gerekir:

| FQDN                                    | Bağlantı noktası      | Kullanım      |
|-----------------------------------------|-----------|----------|
| cloudflare.docker.com | HTTPS: 443 | Linux alp ve diğer Azure Dev Spaces görüntülerini çekmek için |
| gcr.io | HTTP: 443 | Held/Tiller görüntülerini çekmek için|
| storage.googleapis.com | HTTP: 443 | Held/Tiller görüntülerini çekmek için|
| azds-<guid>.<location>. azds.io | HTTPS: 443 | Denetleyicinize yönelik Azure Dev Spaces arka uç hizmetleriyle iletişim kurmak için. % USERPROFILE%\.azds\settings.JSON içindeki "dataplaneFqdn" içinde tam FQDN bulunabilir|

### <a name="error-could-not-find-the-cluster-cluster-in-subscription-subscriptionid"></a>Hata "SubscriptionID\>\<abonelikte \<küme\> kümesi bulunamadı

Kubeconfig dosyanız, Azure Dev Spaces istemci tarafı araçları ile kullanmaya çalıştığınız sayıdan farklı bir kümeyi veya aboneliği hedefliyorsanız bu hatayı görebilirsiniz. Azure Dev Spaces istemci tarafı araçları, kümesini seçmek ve kümeyle iletişim kurmak için [bir veya daha fazla kubeconfig dosyası](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/) kullanan *kubectl*davranışını çoğaltır.

Bu sorunu düzeltmek için:

* Geçerli bağlamı güncelleştirmek için `az aks use-dev-spaces -g <resource group name> -n <cluster name>` kullanın. Bu komut, zaten etkinleştirilmemişse AKS kümenizdeki Azure Dev Spaces de etkinleştirilir. Alternatif olarak, geçerli bağlamı güncelleştirmek için `kubectl config use-context <cluster name>` kullanabilirsiniz.
* Hedeflediğiniz geçerli Azure aboneliğini göstermek ve bunun doğru olduğunu doğrulamak için `az account show` kullanın. `az account set`kullanarak hedeflediğiniz aboneliği değiştirebilirsiniz.