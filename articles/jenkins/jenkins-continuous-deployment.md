---
title: Jenkins ile GitHub'dan AKS'ye dağıtın
titleSuffix: Azure Kubernetes Service
description: Jenkins'i GitHub'dan sürekli tümleştirme (CI) ve Azure Kubernetes Hizmetine (AKS) sürekli dağıtım (CD) için ayarlama
services: container-service
author: zr-msft
ms.author: zarhoads
ms.topic: article
ms.date: 01/09/2019
ms.openlocfilehash: a93cfc77178ff7638217663c2ceda500aae4cfd7
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80668631"
---
# <a name="tutorial-deploy-from-github-to-azure-kubernetes-service-aks-with-jenkins-continuous-integration-and-deployment"></a>Öğretici: GitHub'dan Azure Kubernetes Hizmetine (AKS) Jenkins sürekli tümleştirme ve dağıtımla dağıtın

Bu öğretici, Jenkins'te sürekli tümleştirme (CI) ve sürekli dağıtım (CD) ayarlayarak GitHub'dan azure [Kubernetes Hizmeti (AKS)](/azure/aks/intro-kubernetes) kümesine örnek bir uygulama dağıtır. Bu şekilde, Uygulamanızı GitHub'a iterek güncellediğinizde, Jenkins otomatik olarak yeni bir kapsayıcı oluşturma yı çalıştırır, kapsayıcı görüntülerini Azure Konteyner Kayıt Defteri'ne (ACR) iter ve ardından uygulamanızı AKS'de çalıştırır. 

Bu öğreticide, şu görevleri tamamlayasınız:

> [!div class="checklist"]
> * Bir AKS kümesine örnek bir Azure oy uygulaması dağıtın.
> * Temel bir Jenkins projesi oluşturun.
> * Jenkins'in ACR ile etkileşimkurması için kimlik bilgilerini ayarlayın.
> * Otomatik yapılar için Jenkins build job ve GitHub webhook oluşturun.
> * GitHub kodunu temel alan AKS'deki bir uygulamayı güncelleştirmek için CI/CD ardışık hattını test edin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için şu öğelere ihtiyacınız vardır:

- Kubernetes, Git, CI/CD ve konteyner görüntülerinin temel anlayışı

- Bir [AKS kümesi](../aks/kubernetes-walkthrough.md) ve `kubectl` [AKS küme kimlik bilgileri](/cli/azure/aks#az-aks-get-credentials)ile yapılandırıldı.

- Bir [Azure Kapsayıcı Kayıt Defteri (ACR) kayıt defteri,](../container-registry/container-registry-get-started-azure-cli.md)ACR oturum açma sunucusu adı ve AKS kümesi [ACR kayıt defteri ile kimlik doğrulaması](../aks/cluster-container-registry-integration.md)için yapılandırılmıştır.

- Azure CLI sürümü 2.0.46 veya daha sonra yüklenir ve yapılandırılır. Sürümü `az --version` bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye](/cli/azure/install-azure-cli)bakın.

- [Docker](https://docs.docker.com/install/) geliştirme sisteminize yüklendi

- Bir GitHub hesabı, [GitHub kişisel erişim belirteci](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)ve Git istemcisi geliştirme sisteminizde yüklü

- Jenkins dağıtmak için bu örnek komut dosyası yolu yerine kendi Jenkins örneğini sağlarsanız, Jenkins örneğinizin [Docker'ın yüklü, yapılandırılmış ve](https://docs.docker.com/install/) [kubectl'e](https://kubernetes.io/docs/tasks/tools/install-kubectl/)ihtiyacı vardır.

## <a name="prepare-your-app"></a>Uygulamanızı hazırlayın

Bu makalede, bir veya daha fazla bölmede barındırılan bir web arabirimi ve geçici veri depolama için Redis'i barındıran ikinci bir bölme içeren örnek bir Azure oy uygulaması kullanıyorsunuz. Jenkins ve AKS'yi otomatik dağıtımlar için tümleştirmeden önce, azure oy uygulamasını önce el ile hazırlayın ve AKS kümenize dağıtın. Bu el ile dağıtım, uygulamanın bir sürümüdür ve uygulamayı iş başında görmenizi sağlar.

> [!NOTE]
> Örnek Azure oy uygulaması, Linux düğümünde çalışacak şekilde zamanlanan bir Linux bölmesi kullanır. Bu makalede özetlenen akış, Windows Server düğümünde zamanlanan bir Windows Server bölmesi için de çalışır.

Çatal örnek uygulama için aşağıdaki GitHub deposu [https://github.com/Azure-Samples/azure-voting-app-redis](https://github.com/Azure-Samples/azure-voting-app-redis)- . Depo için GitHub hesabınızda çatal oluşturmak üzere sağ üst köşedeki **Fork** (Çatal Oluştur) düğmesini seçin.

Çatalı geliştirme sisteminize klonlayın. Bu repoyu klonlarken çatalınızın URL'sini kullandığınızdan emin olun:

```console
git clone https://github.com/<your-github-account>/azure-voting-app-redis.git
```

Klonlanmış çatalınızın dizinini değiştirin:

```console
cd azure-voting-app-redis
```

Örnek uygulama için gerekli kapsayıcı görüntüleri oluşturmak için *docker-compose.yaml* dosyasını `docker-compose`kullanın:

```console
docker-compose up -d
```

Gerekli temel görüntüler çekilir ve uygulama kapları oluşturulur. Daha sonra oluşturulan görüntüyü görmek için [docker images](https://docs.docker.com/engine/reference/commandline/images/) komutunu kullanabilirsiniz. Üç görüntü indirilir veya oluşturulur. `azure-vote-front` görüntüsü uygulamayı içerir ve temel olarak `nginx-flask` görüntüsünü kullanır. Resim `redis` bir Redis örneğini başlatmak için kullanılır:

```
$ docker images

REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

*Azure oy ön* kapsayıcı görüntüsünü ACR'ye itmeden [önce, az acr listesi](/cli/azure/acr#az-acr-list) komutuyla ACR giriş sunucunuzu alın. Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda ki bir kayıt defterinin ACR giriş sunucusu adresini alır:

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Görüntüyü ACR giriş sunucusu adı ve sürüm numarası ile etiketlemek `v1`için docker [etiketi](https://docs.docker.com/engine/reference/commandline/tag/) komutunu kullanın. Önceki adımda elde edilen kendi `<acrLoginServer>` adınızı sağlayın:

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Son olarak, *azure oy ön* görüntüsünü ACR kayıt defterinize taşıyın. Yine, `<acrLoginServer>` kendi ACR kayıt defterinin giriş sunucusu adı `myacrregistry.azurecr.io`ile değiştirin:

```console
docker push <acrLoginServer>/azure-vote-front:v1
```

## <a name="deploy-the-sample-application-to-aks"></a>Örnek uygulamayı AKS'ye dağıtma

Örnek uygulamayı AKS kümenize dağıtmak için, Azure oy deposu reposunun kökündeki Kubernetes bildirim dosyasını kullanabilirsiniz. *Azure-vote-all-in-one-redis.yaml* manifest dosyasını bir düzenleyici `vi`ile açın. `microsoft` yerine ACR oturum açma sunucunuzun adını yazın. Bu değer, bildirim dosyasının **47.**

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Ardından, uygulamayı AKS kümenize dağıtmak için [kubectl uygula](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply) komutunu kullanın:

```console
kubectl apply -f azure-vote-all-in-one-redis.yaml
```

Uygulamayı internete maruz bırakmak için bir Kubernetes yük dengeleyici hizmeti oluşturulur. Bu işlem birkaç dakika sürebilir. Yük dengeleyici dağıtımının ilerlemesini izlemek için, bağımsız değişkenle birlikte `--watch` [kubectl get service](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get) komutunu kullanın. *EXTERNAL-IP* adresi *pending* durumundan *IP address* değerine değiştiğinde kubectl izleme işlemini durdurmak için `Control + C` komutunu kullanın.

```console
$ kubectl get service azure-vote-front --watch

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.215.27   <pending>     80:30747/TCP   22s
azure-vote-front   LoadBalancer   10.0.215.27   40.117.57.239   80:30747/TCP   2m
```

Uygulamayı iş başında görmek için, hizmetin dış IP adresine bir web tarayıcısı açın. Azure oy uygulaması aşağıdaki örnekte gösterildiği gibi görüntülenir:

![AKS'de çalışan Azure örnek oy uygulaması](media/jenkins-continuous-deployment/azure-vote.png)

## <a name="deploy-jenkins-to-an-azure-vm"></a>Jenkins'i Azure VM'ye dağıtma

Jenkins'i bu makalede kullanılmak üzere hızla dağıtmak için, bir Azure sanal makinesini dağıtmak, ağ erişimini yapılandırmak ve Jenkins'in temel yüklemesini tamamlamak için aşağıdaki komut dosyasını kullanabilirsiniz. Jenkins ve AKS kümesi arasındaki kimlik doğrulaması için komut dosyası, Geliştirme sisteminizden Jenkins sistemine Kubernetes yapılandırma dosyanızı kopyalar.

> [!WARNING]
> Bu örnek komut dosyası, azure VM'de çalışan bir Jenkins ortamını hızlı bir şekilde sağlamak için demo amaçlıdır. VM'yi yapılandırmak ve gerekli kimlik bilgilerini görüntülemek için Azure özel komut dosyası uzantısını kullanır. *~/.kube/config* Jenkins VM kopyalanır.

Komut dosyasını indirmek ve çalıştırmak için aşağıdaki komutları çalıştırın. Çalıştırmadan önce herhangi bir komut dosyasının [https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/jenkins-tutorial/deploy-jenkins-vm.sh](https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/jenkins-tutorial/deploy-jenkins-vm.sh)içeriğini gözden geçirmelisiniz .

```console
curl https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/jenkins-tutorial/deploy-jenkins-vm.sh > azure-jenkins.sh
sh azure-jenkins.sh
```

VM'yi oluşturmak ve Docker ve Jenkins için gerekli bileşenleri dağıtmak birkaç dakika sürer. Komut dosyası tamamlandığında, jenkins sunucusu için bir adres ve aşağıdaki örnek çıktıda gösterildiği gibi pano kilidini açmak için bir anahtar çıkar:

```
Open a browser to http://40.115.43.83:8080
Enter the following to Unlock Jenkins:
667e24bba78f4de6b51d330ad89ec6c6
```

Görüntülenen URL'ye bir web tarayıcısı açın ve kilit açma tuşunu girin. Jenkins yapılandırmasını tamamlamak için ekrandaki istemleri izleyin:

- **Önerilen eklentileri yükle'yi** seçin
- İlk yönetici kullanıcıyı oluşturun. *Azureuser*gibi bir kullanıcı adı girin, ardından kendi güvenli parolanızı girin. Son olarak, bir tam ad ve e-posta adresi yazın.
- **Kaydet ve Bitir**’i seçin
- Jenkins hazır olduktan sonra **Jenkins kullanmaya başla**’yı seçin
    - Jenkins kullanmaya başladığınızda web tarayıcınız boş bir sayfa görüntülerse, Jenkins hizmetini yeniden başlatın. Hizmeti yeniden başlatmak için, Jenkins örneğinizin genel IP adresine SSH ve yazın. `sudo service jenkins restart` Hizmet yeniden başlatıldıktan sonra web tarayıcınızı yenileyin.
- Yükleme işleminde oluşturduğunuz kullanıcı adı ve parolayla Jenkins'te oturum açın.

## <a name="create-a-jenkins-environment-variable"></a>Jenkins ortamı değişkeni oluşturma

ACR giriş sunucusu adını tutmak için Jenkins ortam değişkeni kullanılır. Bu değişken Jenkins yapı işi sırasında başvurulan. Bu ortam değişkenini oluşturmak için aşağıdaki adımları tamamlayın:

- Jenkins portalının sol tarafında, Jenkins > Yapılandırma**Sistemini** **Yönet'i**seçin
- **Global Özellikler** **altında, Çevre değişkenlerini**seçin. ACR giriş sunucunuzun adını `ACR_LOGINSERVER` ve değerini içeren bir değişken ekleyin.

    ![Jenkins ortam değişkenleri](media/jenkins-continuous-deployment/env-variables.png)

- Tamamlandığında, Jenkins yapılandırma sayfasının altındaki **Kaydet'i** tıklatın.

## <a name="create-a-jenkins-credential-for-acr"></a>ACR için Jenkins kimlik bilgisi oluşturma

Jenkins'in güncelleştirilmiş kapsayıcı görüntülerini oluşturmasına ve ardından ACR'ye itmesine izin vermek için ACR için kimlik bilgilerini belirtmeniz gerekir. Bu kimlik doğrulaması Azure Etkin Dizin hizmet ilkelerini kullanabilir. Ön koşullarda, AKS kümenizin hizmet ilkesini *ACR* kayıt defterinize Reader izinleri ile yapınızlandırıldınız. Bu izinler, AKS kümesinin ACR kayıt defterinden görüntü *çekmesine* olanak sağlar. CI/CD işlemi sırasında Jenkins, uygulama güncelleştirmelerine dayalı yeni kapsayıcı görüntüleri oluşturur ve bu görüntüleri ACR kayıt defterine *itmesi* gerekir. Rollerin ve izinlerin ayrılması için, şimdi Jenkins için ACR kayıt defterinize *Katkıda Bulunan* izinleri olan bir hizmet ilkesi ni yapılandırır.

### <a name="create-a-service-principal-for-jenkins-to-use-acr"></a>Jenkins'in ACR kullanması için bir hizmet ilkesi oluşturma

İlk olarak, [az reklam sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) komutunu kullanarak bir hizmet sorumlusu oluşturun:

```azurecli
$ az ad sp create-for-rbac --skip-assignment

{
  "appId": "626dd8ea-042d-4043-a8df-4ef56273670f",
  "displayName": "azure-cli-2018-09-28-22-19-34",
  "name": "http://azure-cli-2018-09-28-22-19-34",
  "password": "1ceb4df3-c567-4fb6-955e-f95ac9460297",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

Çıktınızda gösterilen *appId* ve *parolayı* not alın. Bu değerler Jenkins kimlik bilgisi kaynağı yapılandırmak için aşağıdaki adımları kullanılır.

[Az acr show](/cli/azure/acr#az-acr-show) komutunu kullanarak ACR kayıt defterinizin kaynak kimliğini alın ve değişken olarak saklayın. Kaynak grup adınızı ve ACR adınızı sağlayın:

```azurecli
ACR_ID=$(az acr show --resource-group myResourceGroup --name <acrLoginServer> --query "id" --output tsv)
```

Şimdi ACR kayıt için hizmet temel *Katılımcı* haklarını atamak için bir rol atama oluşturun. Aşağıdaki örnekte, hizmet ilkesini oluşturmak için çıktıda gösterilen önceki komutu kendi *appId'inizi* sağlayın:

```azurecli
az role assignment create --assignee 626dd8ea-042d-4043-a8df-4ef56273670f --role Contributor --scope $ACR_ID
```

### <a name="create-a-credential-resource-in-jenkins-for-the-acr-service-principal"></a>ACR hizmet ilkesi için Jenkins'te bir kimlik bilgisi kaynağı oluşturma

Azure'da oluşturulan rol atamasıyla, artık ACR kimlik bilgilerinizi Jenkins kimlik bilgisi nesnesinde depolayın. Bu kimlik bilgileri Jenkins yapı işi sırasında başvurulan.

Jenkins portalının sol tarafında, **Kimlik Bilgileri** > **Jenkins** > **Global kimlik bilgilerini (sınırsız)** > **Kimlik Bilgileri Ekle'yi** tıklatın

Kimlik bilgisi türünün **parolalı kullanıcı adı** olduğundan emin olun ve aşağıdaki öğeleri girin:

- **Kullanıcı adı** - ACR kayıt defteriile kimlik doğrulaması için oluşturulan hizmet sorumlusunun *appId'i.*
- **Parola** - ACR kayıt defteriile kimlik doğrulama için oluşturulan servis sorumlusunun *şifresi.*
- **ID** - *Acr kimlik bilgileri* gibi kimlik bilgisi

Tamamlandığında, kimlik bilgileri formu aşağıdaki örnek gibi görünür:

![Hizmet temel bilgileriyle jenkins kimlik bilgisi nesnesi oluşturma](media/jenkins-continuous-deployment/acr-credentials.png)

**Tamam'ı** tıklatın ve Jenkins portalına dönün.

## <a name="create-a-jenkins-project"></a>Jenkins projesi oluşturma

Jenkins portalınızın ana sayfasından sol taraftaki **Yeni öğeyi** seçin:

1. İş adı olarak *azure oy* girin. **Serbest stil projesini**seçin, ardından **Tamam'ı** seçin
1. **Genel** bölümde, **GitHub projesini** seçin ve *https:\//github.com/\<your-github-account\>/azure-voting-app-redis* gibi çatallı repo URL'nizi girin
1. Kaynak **kod yönetimi** bölümünde, **Git**'i seçin , çatallı repo *.git* URL'nizi *girin, https:\//github.com/\<your-github-account\>/azure-voting-app-redis.git*

1. Yapı **Tetikleyicileri** bölümünün **altında, GITscm yoklaması için GitHub kanca tetikleyicisini** seçin
1. **Yapı Ortamı**altında, **gizli metinleri veya dosyaları kullan'ı** seçin
1. **Ciltler**altında, Kullanıcı Adı ve parola **ekle** > **'yi (ayrılmış)** seçin
   - Kullanıcı `ACR_ID` **Adı Değişkeni**ve `ACR_PASSWORD` **Parola Değişkeni** için girin

     ![Jenkins ciltler](media/jenkins-continuous-deployment/bindings.png)

1. **Yürüt türde** bir **Yapı Adımı** eklemeyi seçin ve aşağıdaki metni kullanın. Bu komut dosyası yeni bir kapsayıcı görüntüsü oluşturur ve ACR kayıt defterinize iter.

    ```bash
    # Build new image and push to ACR.
    WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
    docker build -t $WEB_IMAGE_NAME ./azure-vote
    docker login ${ACR_LOGINSERVER} -u ${ACR_ID} -p ${ACR_PASSWORD}
    docker push $WEB_IMAGE_NAME
    ```

1. **Yürütme kabuğunun** başka bir **Yapı Adımı** ekleyin ve aşağıdaki metni kullanın. Bu komut dosyası, AKS'deki uygulama dağıtımını ACR'nin yeni kapsayıcı görüntüsüyle güncelleştirir.

    ```bash
    # Update kubernetes deployment with new image.
    WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
    kubectl set image deployment/azure-vote-front azure-vote-front=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
    ```

1. Tamamlandıktan sonra **Kaydet'i**tıklatın.

## <a name="test-the-jenkins-build"></a>Jenkins inşasını test edin

GitHub taahhütlerine göre işi otomatikleştirmeden önce, önce Jenkins yapısını el ile test edin. Bu el kitabı oluşturma, işin doğru şekilde yapılandırıldığını, uygun Kubernetes kimlik doğrulama dosyasının yerinde olduğunu ve ACR ile kimlik doğrulamanın çalıştığını doğrular.

Projenin sol menüsünde **Şimdi Oluştur'u**seçin.

![Jenkins test yapısı](media/jenkins-continuous-deployment/test-build.png)

Docker görüntü katmanları Jenkins sunucusuna çekilir gibi ilk yapı bir veya iki dakika sürer. Sonraki yapılar, önbelleğe alınmış görüntü katmanlarını yapı sürelerini iyileştirmek için kullanabilir.

Oluşturma işlemi sırasında, GitHub deposu Jenkins build sunucusuna klonlanır. Yeni bir kapsayıcı görüntüsü oluşturulur ve ACR kayıt defterine itilir. Son olarak, AKS kümesinde çalışan Azure oy uygulaması yeni görüntüyü kullanmak üzere güncelleştirilir. Uygulama kodunda herhangi bir değişiklik yapılmadığından, örnek uygulamayı bir web tarayıcısında görüntüleseniz uygulama değiştirilmez.

Yapı işi tamamlandıktan sonra, yapı geçmişi altında **#1 oluştur'a** tıklayın. **Konsol** Çıktısı'nı seçin ve yapı işleminden çıktıyı görüntüleyin. Son satır başarılı bir yapı yı göstermelidir.

## <a name="create-a-github-webhook"></a>GitHub webhook oluşturma

Başarılı bir manuel yapı tamamlandı, şimdi Jenkins inşa içine GitHub entegre. Bir webhook Bir kod işleme GitHub yapılır her zaman Jenkins inşa iş çalıştırmak için kullanılabilir. GitHub webhook'u oluşturmak için aşağıdaki adımları tamamlayın:

1. Bir web tarayıcısında çatallı GitHub deponuza göz atın.
1. **Ayarlar'ı**seçin, ardından sol **taraftaki Webhooks'ları** seçin.
1. **Webhook ekle'yi**seçin. *Payload URL için,* `http://<publicIp:8080>/github-webhook/`jenkins `<publicIp>` sunucusunun IP adresi nerede girin. İzleyen /'u eklediğinden emin olun. İçerik türü ve *itme* olaylarını tetiklemek için diğer varsayılanları bırakın.
1. **Webhook Ekle'yi**seçin.

    ![Jenkins için bir GitHub webhook oluşturma](media/jenkins-continuous-deployment/webhook.png)

## <a name="test-the-complete-cicd-pipeline"></a>Tam CI/CD ardışık hattını test edin

Şimdi tüm CI /CD ardışık hattını test edebilirsiniz. Bir kodu GitHub'a itdiğinizde aşağıdaki adımlar gerçekleşir:

1. GitHub webhook Jenkins ulaşır.
1. Jenkins yapı işini başlatır ve GitHub'dan en son kodu çeker.
1. Docker yapısı güncelleştirilmiş kodu kullanmaya başlar ve yeni kapsayıcı görüntüsü en son yapı numarasıyla etiketlenir.
1. Bu yeni kapsayıcı görüntüsü Azure Kapsayıcı Kayıt Defteri'ne itilir.
1. Uygulamanız Azure Konteyner Kayıt Defteri kayıt defterindeki en son kapsayıcı görüntüsüyle Azure Kubernetes Hizmeti güncelleştirmelerine dağıtılır.

Geliştirme makinenizde, klonlanmış uygulamayı bir kod düzenleyicisi ile açın. */azure-vote/azure-vote* dizininde **config_file.cfg**adlı dosyayı açın. Bu dosyadaki oy değerlerini, aşağıdaki örnekte gösterildiği gibi, kedi ve köpekler dışında bir şeyle güncelleştirin:

```
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Güncelleştirildiğinde, dosyayı kaydedin, değişiklikleri gerçekleştirin ve bunları GitHub deposunun çatalına itin. GitHub webhook Jenkins yeni bir yapı iş tetikler. Jenkins web panosunda, yapı işlemini izleyin. En son kodu çekmek, güncelleştirilmiş görüntüyü oluşturmak ve itmek ve güncelleştirilmiş uygulamayı AKS'de dağıtmak birkaç saniye sürer.

Yapı tamamlandıktan sonra, örnek Azure oy uygulamasının web tarayıcınızı yenileyin. Değişiklikleriniz aşağıdaki örnekte gösterildiği gibi görüntülenir:

![Jenkins yapı işi tarafından güncellenen AKS'deki Azure oylamasına örnek](media/jenkins-continuous-deployment/azure-vote-updated.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure'da Jenkins](/azure/jenkins)