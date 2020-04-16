---
title: Azure Kubernetes Service’in (AKS) hizmet sorumluları
description: Azure Kubernetes Service’te bir küme için Azure Active Directory hizmet sorumlusu oluşturma ve yönetme
services: container-service
ms.topic: conceptual
ms.date: 04/02/2020
ms.openlocfilehash: 2c792eb4dc060e3f5d7fa2d8f2176bdd51538c43
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81392734"
---
# <a name="service-principals-with-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti (AKS) ile hizmet sorumluları

Azure API'leri ile etkileşimde kalmak için BIR AKS kümesi için bir [Azure Etkin Dizin (AD) hizmet yöneticisi][aad-service-principal] veya yönetilen bir [kimlik](use-managed-identity.md)gerekir. Azure yük bakiyesi veya kapsayıcı kayıt defteri (ACR) gibi diğer Azure kaynaklarını dinamik olarak oluşturmak ve yönetmek için bir hizmet sorumlusu veya yönetilen kimlik gereklidir.

Bu makalede, AKS kümeleriniz için bir hizmet sorumlusunun nasıl oluşturulacağı ve kullanılacağı gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Azure AD hizmet sorumlusu oluşturmak için, Azure AD kiracınızla bir uygulamayı kaydetme ve uygulamanızı aboneliğinizdeki bir role atama izinlerinizin olması gerekir. Gerekli izinlere sahip değilseniz Azure AD veya abonelik yöneticinizden gerekli izinleri atamasını istemeniz veya AKS kümesiyle kullanmanız için bir hizmet sorumlusu oluşturma ön işlemlerini tamamlamanız gerekebilir.

Farklı bir Azure AD kiracısından bir hizmet sorumlusu kullanıyorsanız, kümeyi dağıttığınızda kullanılabilir izinler etrafında ek hususlar vardır. Dizin bilgilerini okumak ve yazmak için uygun izinlere sahip olmayabilirsiniz. Daha fazla bilgi için Azure [Etkin Dizini'nde varsayılan kullanıcı izinleri nelerdir?][azure-ad-permissions]

Ayrıca Azure CLI sürüm 2.0.59 veya daha sonra yüklenmiş ve yapılandırılmış gerekir. Sürümü `az --version` bulmak için çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI'yi yükle'ye][install-azure-cli]bakın.

## <a name="automatically-create-and-use-a-service-principal"></a>Otomatik olarak bir hizmet sorumlusu oluşturma ve kullanma

Azure portalda bir AKS kümesi oluşturduğunuzda veya [az aks create][az-aks-create] komutunu kullanarak, Azure otomatik olarak bir hizmet sorumlusu oluşturabilir.

Aşağıdaki Azure CLI örneğinde bir hizmet sorumlusu belirtilmemiştir. Bu senaryoda Azure CLI, AKS kümesi için bir hizmet sorumlusu oluşturur. İşlemi başarıyla tamamlamak için Azure hesabınızın gerekli hizmet sorumlusu oluşturma haklarına sahip olması gerekir.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup
```

## <a name="manually-create-a-service-principal"></a>El ile hizmet sorumlusu oluşturma

El ile hizmet sorumlusunu Azure CLI ile oluşturmak için [az ad sp create-for-rbac][az-ad-sp-create] komutunu kullanın. Aşağıdaki örnekte, `--skip-assignment` parametresi, ek varsayılan atamaların atanmasını engellemektedir:

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment --name myAKSClusterServicePrincipal
```

Çıktı aşağıdaki örneğe benzerdir. Kendi `appId` ve `password`’lerinizi not edin. Bu değerler bir sonraki bölümde AKS kümesi oluşturduğunuzda kullanılır.

```json
{
  "appId": "559513bd-0c19-4c1a-87cd-851a26afd5fc",
  "displayName": "myAKSClusterServicePrincipal",
  "name": "http://myAKSClusterServicePrincipal",
  "password": "e763725a-5eee-40e8-a466-dc88d980f415",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

## <a name="specify-a-service-principal-for-an-aks-cluster"></a>Bir AKS kümesi için hizmet sorumlusu belirtin

[az aks create][az-aks-create] komutunu kullanarak bir AKS kümesi oluşturduğunuzda var olan bir hizmet sorumlusunu kullanmak üzere, [az ad sp create-for-rbac][az-ad-sp-create] komutunun çıktısından `appId` ve `password`’i belirtmek için `--service-principal` ve `--client-secret` parametrelerini kullanın:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --service-principal <appId> \
    --client-secret <password>
```

> [!NOTE]
> Varolan bir servis müdürünü özelleştirilmiş gizli bir hizmet müdürü kullanıyorsanız, sırrın 190 bayttan fazla olmadığından emin olun.

Azure portalı kullanarak bir AKS kümesi dağıtırsanız, **Kubernetes kümesi oluştur** iletişim kutusunun *Kimlik doğrulaması* sayfasında **Hizmet sorumlusu yapılandır**’ı seçin. **Var olanı kullan**’ı seçin ve aşağıdaki değerleri belirtin:

- **Hizmet sorumlusu istemci kimliği**, *appId*’nizdir
- **Hizmet sorumlusu istemci parolası**, *parola* değeridir

![Azure Vote’a göz atma görüntüsü](media/kubernetes-service-principal/portal-configure-service-principal.png)

## <a name="delegate-access-to-other-azure-resources"></a>Diğer Azure kaynaklarına erişimi temsilciye devretme

AKS kümesinin hizmet ilkesi diğer kaynaklara erişmek için kullanılabilir. Örneğin, AKS kümenizi mevcut bir Azure sanal ağ alt ağına dağıtmak veya Azure Kapsayıcı Kayıt Defteri'ne (ACR) bağlanmak istiyorsanız, bu kaynaklara erişimi hizmet ilkesine devretmeniz gerekir.

İzinleri devretmek için az rol atamakomutunu kullanarak bir [rol ataması oluşturun.][az-role-assignment-create] Kaynak grubu `appId` veya sanal ağ kaynağı gibi belirli bir kapsama atama. Bir rol, aşağıdaki örnekte gösterildiği gibi, hizmet sorumlusunun kaynak üzerinde hangi izinlere sahip olduğunu tanımlar:

```azurecli
az role assignment create --assignee <appId> --scope <resourceScope> --role Contributor
```

Bir `--scope` kaynak için */subscriptions/\<\>guid /resourceGroups/myResourceGroup* veya */subscriptions/ guid\<\>/resourceGroups/myResourceGroup/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet* gibi tam bir kaynak kimliği olması gerekir

Aşağıdaki bölümlerde yapmanız gereken ortak delegasyonlar ayrıntılı.

### <a name="azure-container-registry"></a>Azure Container Kayıt Defteri

Azure Kapsayıcı Kayıt Defteri'ni (ACR) kapsayıcı görüntü deponuz olarak kullanıyorsanız, AKS kümenizin görüntüleri okuması ve çekmesi için hizmet sorumlusuna izin vermeniz gerekir. Şu anda, önerilen yapılandırma bir kayıt defteri ile tümleştirmek ve hizmet sorumlusu için uygun rolü atamak için [az aks oluşturma][az-aks-create] veya [az aks güncelleştirme][az-aks-update] komutunu kullanmaktır. Ayrıntılı adımlar için Azure [Kubernetes Hizmetinden Azure Kapsayıcı Kayıt Defteri ile Kimlik Doğrulaması'na][aks-to-acr]bakın.

### <a name="networking"></a>Ağ

Sanal ağ ve alt ağ veya genel IP adreslerinin başka bir kaynak grubunda olduğu gelişmiş ağ kullanımını kullanabilirsiniz. Aşağıdaki rol izinlerinden birini atayın:

- Özel bir [rol][rbac-custom-role] oluşturun ve aşağıdaki rol izinlerini tanımlayın:
  - *Microsoft.Network/virtualNetworks/subnets/join/action*
  - *Microsoft.Network/virtualNetworks/subnets/read*
  - *Microsoft.Network/virtualNetworks/subnets/write*
  - *Microsoft.Network/publicIPAddresses/join/action*
  - *Microsoft.Network/publicIPAddresses/read*
  - *Microsoft.Network/publicIPAddresses/write*
- Veya, sanal ağ içindeki alt ağdaki [Ağ Katılımcısı'nın][rbac-network-contributor] yerleşik rolünü atayın

### <a name="storage"></a>Depolama

Başka bir kaynak grubundaki varolan Disk kaynaklarına erişmeniz gerekebilir. Aşağıdaki rol izinlerinden birini atayın:

- Özel bir [rol][rbac-custom-role] oluşturun ve aşağıdaki rol izinlerini tanımlayın:
  - *Microsoft.Compute/disks/read*
  - *Microsoft.Compute/disks/write*
- Veya, kaynak grubundaki [Depolama Hesabı Katılımcısı][rbac-storage-contributor] yerleşik rolünü atama

### <a name="azure-container-instances"></a>Azure Container Instances

AKS ile tümleştirmek için Sanal Kubelet kullanıyorsanız ve AKS kümesinden ayrı kaynak grubunda Azure Kapsayıcı Örnekleri (ACI) çalıştırmayı seçerseniz, AKS hizmet sorumlusuna ACI kaynak grubunda *Katılımcı* izinleri verilmelidir.

## <a name="additional-considerations"></a>Diğer konular

AKS ve Azure AD hizmet sorumlularını kullanılırken aşağıdaki noktalara dikkat edin.

- Kubernetes için hizmet sorumlusu, küme yapılandırmasının bir parçasıdır. Ancak, kümeyi dağıtmak için kimlik kullanmayın.
- Varsayılan olarak, hizmet temel kimlik bilgileri bir yıl boyunca geçerlidir. Hizmet temel kimlik bilgilerini istediğiniz zaman [güncelleyebilir veya döndürebilirsiniz.][update-credentials]
- Her hizmet sorumlusunun bir Azure AD uygulamasıyla ilişkilendirilmiş olması gerekir. Bir Kubernetes kümesinin hizmet sorumlusu, geçerli bir Azure AD uygulama *https://www.contoso.org/example*adıyla ilişkilendirilebilir (örneğin: ). Uygulama URL'sinin gerçek bir uç nokta olması gerekmez.
- Hizmet sorumlusu **İstemci kimliğini** belirttiğinizde `appId` değerini kullanın.
- Kubernetes kümesindeki aracı düğümü VM'lerde, hizmet temel kimlik bilgileri dosyada depolanır`/etc/kubernetes/azure.json`
- Hizmet sorumlusunu otomatik olarak oluşturmak için [az aks create][az-aks-create] komutunu kullandığınızda, hizmet sorumlusu kimlik bilgileri komutun çalıştırıldığı makinede `~/.azure/aksServicePrincipal.json` dosyasına yazılır.
- Ek AKS CLI komutlarında özel olarak bir hizmet ilkesini geçirmiyorsanız, bulunan `~/.azure/aksServicePrincipal.json` varsayılan hizmet ilkesi kullanılır.  
- Ayrıca isteğe bağlı olarak aksServicePrincipal.json dosyasını kaldırabilirsiniz ve AKS yeni bir hizmet ilkesi oluşturur.
- [az aks create][az-aks-create] komutu tarafından oluşturulan bir AKS kümesini sildiğinizde, otomatik olarak oluşturulan hizmet sorumlusu silinmez.
    - Hizmet üstatlarını silmek için, küme *hizmetinizle sorgulayınPrincipalProfile.clientId* ve [az reklam uygulaması ile sil.][az-ad-app-delete] Aşağıdaki kaynak grubunu ve küme adlarını kendi değerlerinizle değiştirin:

        ```azurecli
        az ad sp delete --id $(az aks show -g myResourceGroup -n myAKSCluster --query servicePrincipalProfile.clientId -o tsv)
        ```

## <a name="troubleshoot"></a>Sorun giderme

Bir AKS kümesinin hizmet temel kimlik bilgileri Azure CLI tarafından önbelleğe alınır. Bu kimlik bilgilerinin süresi dolmuşsa, AKS kümelerini dağıtan hatalarla karşılaşırsınız. [Az aks oluştururken][az-aks-create] aşağıdaki hata iletisi önbelleğe alınan hizmet temel kimlik bilgileri ile ilgili bir sorun gösterebilir:

```console
Operation failed with status: 'Bad Request'.
Details: The credentials in ServicePrincipalProfile were invalid. Please see https://aka.ms/aks-sp-help for more details.
(Details: adal: Refresh request failed. Status Code = '401'.
```

Aşağıdaki komutu kullanarak kimlik bilgileri dosyasının yaşını denetleyin:

```console
ls -la $HOME/.azure/aksServicePrincipal.json
```

Hizmet temel kimlik bilgileri için varsayılan son kullanma süresi bir yıldır. *aksServicePrincipal.json* dosyanız bir yıldan eskiyse, dosyayı silin ve bir AKS kümesini yeniden dağıtmayı deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Active Directory hizmet ilkeleri hakkında daha fazla bilgi için [Bkz. Uygulama ve hizmet temel nesneleri.][service-principal]

Kimlik bilgilerini nasıl güncelleştirişti hakkında bilgi için [AKS'deki bir hizmet sorumlusunun kimlik bilgilerini güncelleştirme veya döndürme][update-credentials]bilgisine bakın.

<!-- LINKS - internal -->
[aad-service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[acr-intro]: ../container-registry/container-registry-intro.md
[az-ad-sp-create]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[azure-load-balancer-overview]: ../load-balancer/load-balancer-overview.md
[install-azure-cli]: /cli/azure/install-azure-cli
[service-principal]:../active-directory/develop/app-objects-and-service-principals.md
[user-defined-routes]: ../load-balancer/load-balancer-overview.md
[az-ad-app-list]: /cli/azure/ad/app#az-ad-app-list
[az-ad-app-delete]: /cli/azure/ad/app#az-ad-app-delete
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-update]: /cli/azure/aks#az-aks-update
[rbac-network-contributor]: ../role-based-access-control/built-in-roles.md#network-contributor
[rbac-custom-role]: ../role-based-access-control/custom-roles.md
[rbac-storage-contributor]: ../role-based-access-control/built-in-roles.md#storage-account-contributor
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[aks-to-acr]: cluster-container-registry-integration.md
[update-credentials]: update-credentials.md
[azure-ad-permissions]: ../active-directory/fundamentals/users-default-permissions.md
