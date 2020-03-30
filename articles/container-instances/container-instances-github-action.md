---
title: GitHub eylemine göre kapsayıcı örneğini dağıtma
description: Kapsayıcı görüntüsünü Azure Kapsayıcı Örnekleri'ne oluşturmak, itmek ve dağıtmak için adımları otomatikleştiren bir GitHub eylemini yapılandırma
ms.topic: article
ms.date: 03/18/2020
ms.custom: ''
ms.openlocfilehash: 13397cee8197afc65b93c587ae1505e59cfdebc1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80258048"
---
# <a name="configure-a-github-action-to-create-a-container-instance"></a>Kapsayıcı örneği oluşturmak için GitHub eylemini yapılandırma

[GitHub Eylemleri,](https://help.github.com/actions/getting-started-with-github-actions/about-github-actions) kod depoladığınız ve çekme istekleri ve sorunları üzerinde işbirliği yaptığınız aynı yerde yazılım geliştirme iş akışlarınızı otomatikleştirmek için GitHub'daki bir özellik paketidir.

Bir kapsayıcının Azure Kapsayıcı Örneklerine dağıtımını otomatikleştirmek için [Azure Kapsayıcı Örnekleri](https://github.com/azure/aci-deploy) GitHub eylemini dağıt'ı kullanın. Eylem [az kapsayıcı oluşturma][az-container-create] komutu benzer bir kapsayıcı örneği için özellikleri ayarlamanızı sağlar.

Bu makalede, aşağıdaki eylemleri gerçekleştiren Bir GitHub repo'da iş akışı nasıl ayarlanıncayasınız:

* Dockerfile'den görüntü oluşturma
* Görüntüyü Azure kapsayıcı kayıt defterine itme
* Kapsayıcı görüntüsünü Azure kapsayıcı örneğine dağıtma

Bu makalede, iş akışını ayarlamak için iki yol gösterilmektedir:

* Azure Kapsayıcı Örnekleri eylemini ve diğer eylemleri kullanarak GitHub repo'sunda bir iş akışını kendiniz yapılandırın.  
* Azure `az container app up` CLI'deki [Azure'a Dağıt](https://github.com/Azure/deploy-to-azure-cli-extension) uzantısındaki komutu kullanın. Bu komut, GitHub iş akışı ve dağıtım adımlarının oluşturulmasını kolaylaştırır.

> [!IMPORTANT]
> Azure Kapsayıcı Örnekleri için GitHub eylemi şu anda önizlemededir. Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="prerequisites"></a>Ön koşullar

* **GitHub hesabı** - Zaten https://github.com hesabınız yoksa bir hesap oluşturun.
* **Azure CLI** - Azure CLI adımlarını tamamlamak için Azure Bulut Kabuğu'nu veya Azure CLI'nin yerel yüklemesini kullanabilirsiniz. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].
* **Azure kapsayıcı kayıt defteri** - Yoksa, [Azure CLI](../container-registry/container-registry-get-started-azure-cli.md), [Azure portalı](../container-registry/container-registry-get-started-portal.md)veya diğer yöntemleri kullanarak Temel katmanda bir Azure kapsayıcı kayıt defteri oluşturun. GitHub iş akışı için kullanılan dağıtım için kullanılan kaynak grubuna dikkat edin.

## <a name="set-up-repo"></a>Repo'u ayarlama

* Bu makaledeki örnekler için, aşağıdaki depoyu çatallamak için GitHub'ı kullanın:https://github.com/Azure-Samples/acr-build-helloworld-node

  Bu repo, küçük bir web uygulamasının kapsayıcı görüntüsünü oluşturmak için dockerfile ve kaynak dosyaları içerir.

  ![GitHub’daki Çatal düğmesinin (vurgulanmış) ekran görüntüsü](../container-registry/media/container-registry-tutorial-quick-build/quick-build-01-fork.png)

* Deponuz için Eylemlerin etkinleştirildiğinden emin olun. Çatallı deponuza gidin ve **Ayarlar** > **Eylemleri'ni**seçin. **Eylemler izinlerinde,** **bu depo için yerel ve üçüncü taraf Eylemleri Etkinleştir'in** seçildiğinden emin olun.

## <a name="configure-github-workflow"></a>GitHub iş akışını yapılandırma

### <a name="create-service-principal-for-azure-authentication"></a>Azure kimlik doğrulaması için hizmet ilkesi oluşturma

GitHub iş akışında, Azure CLI'ye kimlik doğrulamak için Azure kimlik bilgilerini sağlamanız gerekir. Aşağıdaki örnek, kapsayıcı kayıt defteriniz için kaynak grubuna yönelik Katılımcı rolüyle birlikte bir hizmet ilkesi oluşturur.

İlk olarak, kaynak grubunuzun kaynak kimliğini alın. Aşağıdaki [az grup gösteri][az-acr-show] komutunda grubunuzun adını değiştirin:

```azurecli
groupId=$(az group show \
  --name <resource-group-name> \
  --query id --output tsv)
```

Hizmet ilkesini oluşturmak [için az reklam sp create-for-rbac'ı][az-ad-sp-create-for-rbac] kullanın:

```azurecli
az ad sp create-for-rbac \
  --scope $groupId \
  --role Contributor \
  --sdk-auth
```

Çıkış şuna benzer olacaktır:

```json
{
  "clientId": "xxxx6ddc-xxxx-xxxx-xxx-ef78a99dxxxx",
  "clientSecret": "xxxx79dc-xxxx-xxxx-xxxx-aaaaaec5xxxx",
  "subscriptionId": "xxxx251c-xxxx-xxxx-xxxx-bf99a306xxxx",
  "tenantId": "xxxx88bf-xxxx-xxxx-xxxx-2d7cd011xxxx",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```

Daha sonraki bir adımda kullanıldığından JSON çıktısını kaydedin. Ayrıca, bir sonraki `clientId`bölümde hizmet ilkesini güncelleştirmeniz gereken ,

### <a name="update-service-principal-for-registry-authentication"></a>Kayıt defteri kimlik doğrulaması için hizmet ilkesini güncelleştirme

Konteyner kayıt defterinizde itme ve çekme izinlerine izin vermek için Azure hizmet temel kimlik bilgilerini güncelleştirin. Bu adım, GitHub iş akışının [kapsayıcı kayıt defterinizle kimlik doğrulaması](../container-registry/container-registry-auth-service-principal.md)için hizmet ilkesini kullanmasına olanak tanır. 

Konteyner kayıt defterinizin kaynak kimliğini alın. Aşağıdaki [az acr show][az-acr-show] komutunda kayıt defterinizin adını değiştirin:

```azurecli
registryId=$(az acr show \
  --name <registry-name> \
  --query id --output tsv)
```

Kayıt defterine itme ve çekme erişimi sağlayan AcrPush rolünü atamak için [az rol ataması oluşturun.][az-role-assignment-create] Hizmet müdürünüzin istemci kimliğini değiştirin:

```azurecli
az role assignment create \
  --assignee <ClientId> \
  --scope $registryId \
  --role AcrPush
```

### <a name="save-credentials-to-github-repo"></a>Kimlik bilgilerini GitHub repo'ya kaydetme

1. GitHub UI'sinde çatallı deponuza gidin ve **Ayarlar** > **Sırları'nı**seçin. 

1. Aşağıdaki sırları eklemek için **yeni bir sır ekle'yi** seçin:

|Gizli dizi  |Değer  |
|---------|---------|
|`AZURE_CREDENTIALS`     | Hizmet temel oluşturmadan tüm JSON çıkışı |
|`REGISTRY_LOGIN_SERVER`   | Kayıt defterinizin giriş sunucusu adı (hepsi küçük harf). Örnek: *myregistry.azure.cr.io*        |
|`REGISTRY_USERNAME`     |  Hizmet `clientId` temel oluşturma JSON çıktısından       |
|`REGISTRY_PASSWORD`     |  Hizmet `clientSecret` temel oluşturma JSON çıktısından |
| `RESOURCE_GROUP` | Hizmet ilkesini kapsamda kapsamda görmek için kullandığınız kaynak grubunun adı |

### <a name="create-workflow-file"></a>İş akışı dosyası oluşturma

1. GitHub UI'sinde **Eylemler** > **Yeni iş akışını**seçin.
1. **İş akışını kendiniz ayarla'yı**seçin.
1. **Yeni dosyayı edit,** örnek kodu n üzerine yazmak için aşağıdaki YAML içeriğini yapıştırın. Varsayılan dosya adını `main.yml`kabul edin veya seçtiğiniz bir dosya adı sağlayın.
1. **Commit Commit'ı**seç, isteğe bağlı olarak işlemenin kısa ve genişletilmiş açıklamalarını sağlayın ve **yeni dosyayı commit'i**seçin.

```yml
on: [push]
name: Linux_Container_Workflow

jobs:
    build-and-deploy:
        runs-on: ubuntu-latest
        steps:
        # checkout the repo
        - name: 'Checkout GitHub Action'
          uses: actions/checkout@master
          
        - name: 'Login via Azure CLI'
          uses: azure/login@v1
          with:
            creds: ${{ secrets.AZURE_CREDENTIALS }}
        
        - name: 'Build and push image'
          uses: azure/docker-login@v1
          with:
            login-server: ${{ secrets.REGISTRY_LOGIN_SERVER }}
            username: ${{ secrets.REGISTRY_USERNAME }}
            password: ${{ secrets.REGISTRY_PASSWORD }}
        - run: |
            docker build . -t ${{ secrets.REGISTRY_LOGIN_SERVER }}/sampleapp:${{ github.sha }}
            docker push ${{ secrets.REGISTRY_LOGIN_SERVER }}/sampleapp:${{ github.sha }}

        - name: 'Deploy to Azure Container Instances'
          uses: 'azure/aci-deploy@v1'
          with:
            resource-group: ${{ secrets.RESOURCE_GROUP }}
            dns-name-label: ${{ secrets.RESOURCE_GROUP }}${{ github.run_number }}
            image: ${{ secrets.REGISTRY_LOGIN_SERVER }}/sampleapp:${{ github.sha }}
            registry-login-server: ${{ secrets.REGISTRY_LOGIN_SERVER }}
            registry-username: ${{ secrets.REGISTRY_USERNAME }}
            registry-password: ${{ secrets.REGISTRY_PASSWORD }}
            name: aci-sampleapp
            location: 'west us'
```

### <a name="validate-workflow"></a>İş akışını doğrulama

İş akışı dosyasını işledikten sonra, iş akışı tetiklenir. İş akışı ilerlemesini gözden geçirmek **için, Eylemler** > **İş Akışları'na**gidin. 

![İş akışı ilerlemeyi görüntüleme](./media/container-instances-github-action/github-action-progress.png)

İş akışınızdaki her adımın durumunu ve sonuçlarını görüntüleme hakkında bilgi için [iş akışını yönetme](https://help.github.com/actions/configuring-and-managing-workflows/managing-a-workflow-run) bölümüne bakın.

İş akışı tamamlandığında, [az kapsayıcı göster][az-container-show] komutunu çalıştırarak *aci-sampleapp* adlı kapsayıcı örneği hakkında bilgi alın. Kaynak grubunuzun adını değiştirin: 

```azurecli
az container show \
  --resource-group <resource-group-name> \
  --name aci-sampleapp \
  --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
  --output table
```

Çıkış şuna benzer olacaktır:

```console
FQDN                                   ProvisioningState
---------------------------------      -------------------
aci-action01.westus.azurecontainer.io  Succeeded
```

Örnek sağlandıktan sonra, çalışan web uygulamasını görüntülemek için tarayıcınızdaki kapsayıcının FQDN'sine gidin.

![Tarayıcıda web uygulaması çalıştırma](./media/container-instances-github-action/github-action-container.png)

## <a name="use-deploy-to-azure-extension"></a>Azure uzantısına Dağıt'ı kullanma

Alternatif olarak, iş akışını yapılandırmak için Azure CLI'de [Azure'a Dağıt uzantısını](https://github.com/Azure/deploy-to-azure-cli-extension) kullanın. Uzantıdaki komut, `az container app up` Azure Kapsayıcı Örnekleri'ne dağıtmak için bir iş akışı ayarlamak için giriş parametrelerini sizden alır. 

Azure CLI tarafından oluşturulan iş [akışı, GitHub'ı kullanarak el ile oluşturabileceğiniz](#configure-github-workflow)iş akışına benzer.

### <a name="additional-prerequisite"></a>Ek ön koşul

Bu senaryoiçin [ön koşullar](#prerequisites) avesi ve [repo kurulumuna](#set-up-repo) ek olarak, Azure CLI için **Azure'a Dağıt uzantısını** yüklemeniz gerekir.

Uzantısı yüklemek için [az uzantısı eklemek][az-extension-add] komutunu çalıştırın:

```azurecli
az extension add \
  --name deploy-to-azure
```

Uzantıları bulma, yükleme ve yönetme hakkında daha fazla bilgi için azure [CLI ile uzantıları kullan'a](/cli/azure/azure-cli-extensions-overview)bakın.

### <a name="run-az-container-app-up"></a>`az container app up` öğesini çalıştırın

[Az konteyner uygulamasını çalıştırmak][az-container-app-up] için en az aşağıdakileri sağlayın:

* Azure kapsayıcı kayıt defterinizin adı, örneğin, *kayıt defteri*
* Örneğin, GitHub repo'nuzun URL'si,`https://github.com/<your-GitHub-Id>/acr-build-helloworld-node`

Örnek komut:

```azurecli
az container app up \
  --acr myregistry \
  --repository https://github.com/myID/acr-build-helloworld-node
```

### <a name="command-progress"></a>Komut ilerlemesi

* İstendiğinde, GitHub kimlik bilgilerinizi sağlayın veya kayıt defterinizde kimlik doğrulaması için *repo* ve *kullanıcı* kapsamları olan bir [GitHub kişisel erişim belirteci](https://help.github.com/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line) (PAT) sağlayın. GitHub kimlik bilgilerini sağlarsanız, komut sizin için bir PAT oluşturur.

* Komut, iş akışı için repo sırları oluşturur:

  * Azure CLI için hizmet temel kimlik bilgileri
  * Azure kapsayıcı kayıt defterine erişmek için kimlik bilgileri

* Komut iş akışı dosyasını repo'nuza işledikten sonra, iş akışı tetiklenir. 

Çıkış şuna benzer olacaktır:

```console
[...]
Checking in file github/workflows/main.yml in the Github repository myid/acr-build-helloworld-node
Creating workflow...
GitHub Action Workflow has been created - https://github.com/myid/acr-build-helloworld-node/runs/515192398
GitHub workflow completed.
Workflow succeeded
Your app is deployed at:  http://acr-build-helloworld-node.eastus.azurecontainer.io:8080/
```

### <a name="validate-workflow"></a>İş akışını doğrulama

İş akışı, GitHub repo'nuzun temel adını içeren bir Azure kapsayıcı örneği dağıtıyor, bu durumda *acr-build-helloworld düğümü.* Tarayıcınızda, çalışan web uygulamasını görüntülemek için sağlanan bağlantıya göz atabilirsiniz. Uygulamanız 8080'den başka bir bağlantı noktasında dinliyorsa, bunun yerine URL'de bunu belirtin.

GitHub UI'daki her adımın iş akışı durumunu ve sonuçlarını görüntülemek için [bkz.](https://help.github.com/actions/configuring-and-managing-workflows/managing-a-workflow-run)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kapsayıcı örneğini [az container delete][az-container-delete] komutu ile durdurun:

```azurecli
az container delete \
  --name <instance-name>
  --resource-group <resource-group-name>
```

Kaynak grubunu ve tüm kaynakları silmek için [az grubu silme][az-group-delete] komutunu çalıştırın:

```azurecli
az group delete \
  --name <resource-group-name>
```

## <a name="next-steps"></a>Sonraki adımlar

Geliştirme iş akışınızı otomatikleştirmek için daha fazla eylem için [GitHub](https://github.com/marketplace?type=actions) Marketi'ne göz atın


<!-- LINKS - external -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - internal -->

[azure-cli-install]: /cli/azure/install-azure-cli
[az-group-show]: /cli/azure/group#az-group-show
[az-group-delete]: /cli/azure/group#az-group-delete
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-container-create]: /cli/azure/container#az-container-create
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-container-show]: /cli/azure/container#az-container-show
[az-container-delete]: /cli/azure/container#az-container-delete
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-container-app-up]: /cli/azure/ext/deploy-to-azure/container/app#ext-deploy-to-azure-az-container-app-up
