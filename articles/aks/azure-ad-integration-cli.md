---
title: Azure Active Directory'yi Azure Kubernetes Service ile tümleştirme
description: Oluşturmak için Azure CLI'yi nasıl kullanacağınızı ve Azure Etkin Dizin etkin Azure Kubernetes Hizmeti (AKS) kümesini öğrenin
services: container-service
ms.topic: article
ms.date: 04/16/2019
ms.openlocfilehash: d17ae12beecf9d83ef6d688af799787c5ccf322b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253058"
---
# <a name="integrate-azure-active-directory-with-azure-kubernetes-service-using-the-azure-cli"></a>Azure CLI'yi kullanarak Azure Etkin Dizini Azure Kubernetes Hizmetiile tümleştirin

Azure Kubernetes Hizmeti (AKS), kullanıcı kimlik doğrulaması için Azure Active Directory (AD) kullanacak şekilde yapılandırılabilir. Bu yapılandırmada, Azure AD kimlik doğrulama belirteci kullanarak bir AKS kümesine giriş yapabilirsiniz. Küme operatörleri ayrıca Kubernetes rol tabanlı erişim denetimini (RBAC) kullanıcının kimliğine veya dizin grubu üyeliğine göre yapılandırabilir.

Bu makalede, gerekli Azure AD bileşenlerini nasıl oluşturabileceğinizi, ardından Azure AD özellikli bir küme dağıtmayı ve AKS kümesinde temel bir RBAC rolünü nasıl oluşturabileceğinizi gösterir. [Azure portalını kullanarak da bu adımları][azure-ad-portal]tamamlayabilirsiniz.

Bu makalede kullanılan örnek komut dosyasının tamamı için Azure [CLI örneklerine bakın - Azure AD ile AKS tümleştirmesi.][complete-script]

Aşağıdaki sınırlamalar geçerlidir:

- Azure AD yalnızca yeni, RBAC özellikli bir küme oluşturduğunuzda etkinleştirilebilir. Varolan bir AKS kümesinde Azure AD'yi etkinleştiremezsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLI sürümü 2.0.61 veya daha sonra yüklenmiş ve yapılandırılan gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][install-azure-cli].

Tarayıcınızda [https://shell.azure.com](https://shell.azure.com) Cloud Shell'i açmak için gidin.

Tutarlılık ve bu makaledeki komutların çalışmasına yardımcı olmak için istediğiniz AKS küme adı için bir değişken oluşturun. Aşağıdaki örnekte *myakscluster*adı kullanır:

```console
aksname="myakscluster"
```

## <a name="azure-ad-authentication-overview"></a>Azure AD kimlik doğrulamaya genel bakış

Azure AD kimlik doğrulaması OpenID Connect ile AKS kümelerine sağlanır. OpenID Connect, OAuth 2.0 protokolünün üzerine kurulmuş bir kimlik katmanıdır. OpenID Connect hakkında daha fazla bilgi için [Open ID connect belgelerine][open-id-connect]bakın.

Webhook Belirteç Kimlik Doğrulaması, Kubernetes kümesinin içinden kimlik doğrulama belirteçlerini doğrulamak için kullanılır. Webhook belirteç kimlik doğrulaması, AKS kümesinin bir parçası olarak yapılandırılır ve yönetilir. Webhook belirteç kimlik doğrulaması hakkında daha fazla bilgi için [webhook kimlik doğrulama belgelerine][kubernetes-webhook]bakın.

> [!NOTE]
> AKS kimlik doğrulaması için Azure AD yapılandırılırken, iki Azure AD uygulaması yapılandırılır. Bu işlem bir Azure kiracı yöneticisi tarafından tamamlanmalıdır.

## <a name="create-azure-ad-server-component"></a>Azure AD sunucu bileşeni oluşturma

AKS ile tümleştirmek için, kimlik istekleri için bir bitiş noktası görevi gören bir Azure REKLAM uygulaması oluşturur ve kullanırsınız. İhtiyacınız olan ilk Azure REKLAM uygulaması, bir kullanıcı için Azure AD grup üyeliğine geçer.

[Az reklam uygulaması oluşturma][az-ad-app-create] komutunu kullanarak sunucu uygulaması bileşenini oluşturun, ardından az reklam uygulaması [güncelleme][az-ad-app-update] komutunu kullanarak grup üyelik taleplerini güncelleyin. Aşağıdaki örnek, [başlamadan önce](#before-you-begin) bölümünde tanımlanan *aksname* değişkenini kullanır ve bir değişken oluşturur

```azurecli-interactive
# Create the Azure AD application
serverApplicationId=$(az ad app create \
    --display-name "${aksname}Server" \
    --identifier-uris "https://${aksname}Server" \
    --query appId -o tsv)

# Update the application group memebership claims
az ad app update --id $serverApplicationId --set groupMembershipClaims=All
```

Şimdi [az reklam sp oluşturma][az-ad-sp-create] komutunu kullanarak sunucu uygulaması için bir hizmet ilkesi oluşturun. Bu hizmet ilkesi, Azure platformunda kimliğini doğrulamak için kullanılır. Ardından, [az reklam sp kimlik bilgilerini kullanarak][az-ad-sp-credential-reset] hizmet temel sırrını alın ve aşağıdaki adımlardan birinde kullanılmak üzere *serverApplicationSecret* adlı değişkene atayın:

```azurecli-interactive
# Create a service principal for the Azure AD application
az ad sp create --id $serverApplicationId

# Get the service principal secret
serverApplicationSecret=$(az ad sp credential reset \
    --name $serverApplicationId \
    --credential-description "AKSPassword" \
    --query password -o tsv)
```

Azure AD'nin aşağıdaki eylemleri gerçekleştirmek için izinlere ihtiyacı vardır:

* Dizin verilerini oku
* Oturum açma ve kullanıcı profilini okuma

[Az reklam uygulaması eklenti][az-ad-app-permission-add] komutunu kullanarak bu izinleri atayın:

```azurecli-interactive
az ad app permission add \
    --id $serverApplicationId \
    --api 00000003-0000-0000-c000-000000000000 \
    --api-permissions e1fe6dd8-ba31-4d61-89e7-88639da4683d=Scope 06da0dbc-49e2-44d2-8312-53f166ab848a=Scope 7ab1d382-f21e-4acd-a863-ba3e13f7da61=Role
```

Son olarak, [az reklam uygulaması izni verme][az-ad-app-permission-grant] komutunu kullanarak sunucu uygulaması için önceki adımda atanan izinleri ver. Cari hesap kiracı yöneticisi değilse bu adım başarısız olur. Ayrıca, [az reklam uygulaması izni yönetici onayı][az-ad-app-permission-admin-consent]kullanarak yönetim onayı gerektirebilecek bilgileri istemek için Azure AD uygulaması için izin eklemeniz gerekir:

```azurecli-interactive
az ad app permission grant --id $serverApplicationId --api 00000003-0000-0000-c000-000000000000
az ad app permission admin-consent --id  $serverApplicationId
```

## <a name="create-azure-ad-client-component"></a>Azure AD istemci bileşeni oluşturma

İkinci Azure AD uygulaması, bir kullanıcı Kubernetes CLI ( ile`kubectl`AKS kümesine oturum açtığınızda kullanılır). Bu istemci uygulaması kullanıcıdan kimlik doğrulama isteğini alır ve kimlik bilgilerini ve izinlerini doğrular. Az reklam uygulaması oluşturma komutunu kullanarak istemci bileşeni için Azure AD [uygulamasını oluşturun:][az-ad-app-create]

```azurecli-interactive
clientApplicationId=$(az ad app create \
    --display-name "${aksname}Client" \
    --native-app \
    --reply-urls "https://${aksname}Client" \
    --query appId -o tsv)
```

[Az reklam sp oluşturma][az-ad-sp-create] komutunu kullanarak istemci uygulaması için bir hizmet anası oluşturun:

```azurecli-interactive
az ad sp create --id $clientApplicationId
```

[Az reklam uygulaması göster][az-ad-app-show] komutunu kullanarak iki uygulama bileşeni arasında kimlik doğrulama akışına izin vermek için sunucu uygulamasının oAuth2 kimliğini alın. Bu oAuth2 kimliği bir sonraki adımda kullanılır.

```azurecli-interactive
oAuthPermissionId=$(az ad app show --id $serverApplicationId --query "oauth2Permissions[0].id" -o tsv)
```

Az reklam uygulaması izni komutunu kullanarak oAuth2 iletişim akışını kullanmak için istemci uygulaması ve sunucu uygulama bileşenleri için [izinleri ekleyin.][az-ad-app-permission-add] Ardından, az [reklam uygulaması izni hibe][az-ad-app-permission-grant] komutunu kullanarak sunucu uygulaması yla iletişim için istemci uygulaması için izin ler verir:

```azurecli-interactive
az ad app permission add --id $clientApplicationId --api $serverApplicationId --api-permissions ${oAuthPermissionId}=Scope
az ad app permission grant --id $clientApplicationId --api $serverApplicationId
```

## <a name="deploy-the-cluster"></a>Kümeyi dağıtma

Oluşturulan iki Azure AD uygulamasıyla artık AKS kümesini oluşturun. İlk olarak, az grubu oluşturma komutunu kullanarak bir kaynak grubu [oluşturun.][az-group-create] Aşağıdaki örnek, *EastUS* bölgesinde kaynak grubunu oluşturur:

Küme için bir kaynak grubu oluşturun:

```azurecli-interactive
az group create --name myResourceGroup --location EastUS
```

[Az hesap göster][az-account-show] komutunu kullanarak Azure aboneliğinizin kiracı kimliğini alın. Daha sonra [az aks komutunu][az-aks-create] kullanarak AKS kümesini oluşturun. AKS kümesini oluşturma komutu sunucu ve istemci uygulama kimliklerini, sunucu uygulama hizmeti nin temel sırrını ve kiracı kimliğinizi sağlar:

```azurecli-interactive
tenantId=$(az account show --query tenantId -o tsv)

az aks create \
    --resource-group myResourceGroup \
    --name $aksname \
    --node-count 1 \
    --generate-ssh-keys \
    --aad-server-app-id $serverApplicationId \
    --aad-server-app-secret $serverApplicationSecret \
    --aad-client-app-id $clientApplicationId \
    --aad-tenant-id $tenantId
```

Son olarak, [az aks get-credentials][az-aks-get-credentials] komutunu kullanarak küme yönetici kimlik bilgilerini alın. Aşağıdaki adımlardan birinde, Azure AD kimlik doğrulama akışını iş başında görmek için normal *kullanıcı* kümesi kimlik bilgilerini alırsınız.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name $aksname --admin
```

## <a name="create-rbac-binding"></a>RBAC bağlama oluşturma

Bir Azure Active Directory hesabıAKS kümesiyle kullanılabilmeden önce bir rol bağlama veya küme rol bağlama oluşturulması gerekir. *Roller,* verme izinlerini tanımlar ve *bağlamalar* bunları istenen kullanıcılara uygular. Bu atamalar belirli bir ad alanına veya tüm kümeye uygulanabilir. Daha fazla bilgi için [RBAC yetkilendirmesi kullanma'ya][rbac-authorization]bakın.

[Az reklam imzalı kullanıcı içi göster][az-ad-signed-in-user-show] komutunu kullanarak oturum açmış olan kullanıcının kullanıcı ana adını (UPN) alın. Bu kullanıcı hesabı, bir sonraki adımda Azure AD tümleştirmesi için etkinleştirilir.

```azurecli-interactive
az ad signed-in-user show --query userPrincipalName -o tsv
```

> [!IMPORTANT]
> RBAC bağlayıcısını hibe ettiğiniz kullanıcı aynı Azure AD kiracısındaysa, *userPrincipalName'e*dayalı izinler atayın. Kullanıcı farklı bir Azure AD kiracıysa, bunun yerine *objectId* özelliğini sorgula ve kullanın.

Adlandırılmış `basic-azure-ad-binding.yaml` bir YAML bildirimi oluşturun ve aşağıdaki içeriği yapıştırın. Son satırda, *userPrincipalName_or_objectId* önceki komuttan UPN veya nesne kimliği çıkışıyla değiştirin:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: contoso-cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: userPrincipalName_or_objectId
```

[Kubectl uygula][kubectl-apply] komutunu kullanarak ClusterRoleBinding'i oluşturun ve YAML manifestonuzun dosya adını belirtin:

```console
kubectl apply -f basic-azure-ad-binding.yaml
```

## <a name="access-cluster-with-azure-ad"></a>Azure AD ile erişim kümesi

Şimdi AKS kümesi için Azure AD kimlik doğrulamasının tümleştirmesini test edelim. Düzenli `kubectl` kullanıcı kimlik bilgilerini kullanacak config bağlamını ayarlayın. Bu bağlam, tüm kimlik doğrulama isteklerini Azure AD aracılığıyla geri geçirir.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name $aksname --overwrite-existing
```

Şimdi tüm ad alanlarında bölmeleri görüntülemek için [kubectl get pods][kubectl-get] komutunu kullanın:

```console
kubectl get pods --all-namespaces
```

Bir web tarayıcısı kullanarak Azure AD kimlik bilgilerini kullanarak kimlik doğrulaması yapmak için bir oturum açma istemi alırsınız. Komut, başarılı bir şekilde kimlik `kubectl` doğrulaması yaptıktan sonra, aşağıdaki örnek çıktıda gösterildiği gibi AKS kümesindeki bölmeleri görüntüler:

```console
kubectl get pods --all-namespaces
```

```output
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code BYMK7UXVD to authenticate.

NAMESPACE     NAME                                    READY   STATUS    RESTARTS   AGE
kube-system   coredns-754f947b4-2v75r                 1/1     Running   0          23h
kube-system   coredns-754f947b4-tghwh                 1/1     Running   0          23h
kube-system   coredns-autoscaler-6fcdb7d64-4wkvp      1/1     Running   0          23h
kube-system   heapster-5fb7488d97-t5wzk               2/2     Running   0          23h
kube-system   kube-proxy-2nd5m                        1/1     Running   0          23h
kube-system   kube-svc-redirect-swp9r                 2/2     Running   0          23h
kube-system   kubernetes-dashboard-847bb4ddc6-trt7m   1/1     Running   0          23h
kube-system   metrics-server-7b97f9cd9-btxzz          1/1     Running   0          23h
kube-system   tunnelfront-6ff887cffb-xkfmq            1/1     Running   0          23h
```

Alınan `kubectl` kimlik doğrulama belirteci önbelleğe alınır. Yalnızca belirteç süresi dolduğunda veya Kubernetes config dosyası yeniden oluşturulduğunda oturum açmak için yeniden istenirsiniz.

Aşağıdaki örnek çıktıda olduğu gibi bir web tarayıcısını başarıyla kullandıktan sonra bir yetkilendirme hatası iletisi görürseniz, aşağıdaki olası sorunları kontrol edin:

```output
error: You must be logged in to the server (Unauthorized)
```

* Kullanıcı hesabının aynı Azure AD kiracısında olup olmamasına bağlı olarak uygun nesne kimliğini veya UPN'yi tanımladınız.
* Kullanıcı 200'den fazla grubun üyesi değildir.
* Sunucu için uygulama kaydında tanımlanan gizli, kullanılarak yapılandırılan değerle eşleşir`--aad-server-app-secret`

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede gösterilen komutları içeren tam komut dosyası [için, AKS örnekleri repo'sundaki Azure AD tümleştirme komutdosyasına][complete-script]bakın.

Küme kaynaklarına erişimi denetlemek için Azure AD kullanıcılarını ve gruplarını kullanmak için, [AKS'de rol tabanlı erişim denetimi ve Azure AD kimliklerini kullanarak küme kaynaklarına erişimi denetleme'ye][azure-ad-rbac]bakın.

Kubernetes kümelerinin nasıl güvenli hale sağaltılması hakkında daha fazla bilgi için [AKS için Erişim ve kimlik seçeneklerine bakın.][rbac-authorization]

Kimlik ve kaynak denetimi ile ilgili en iyi uygulamalar [için AKS'de kimlik doğrulama ve yetkilendirme için en iyi uygulamalara][operator-best-practices-identity]bakın.

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[complete-script]: https://github.com/Azure-Samples/azure-cli-samples/tree/master/aks/azure-ad-integration/azure-ad-integration.sh

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[open-id-connect]:../active-directory/develop/v2-protocols-oidc.md
[az-ad-user-show]: /cli/azure/ad/user#az-ad-user-show
[az-ad-app-create]: /cli/azure/ad/app#az-ad-app-create
[az-ad-app-update]: /cli/azure/ad/app#az-ad-app-update
[az-ad-sp-create]: /cli/azure/ad/sp#az-ad-sp-create
[az-ad-app-permission-add]: /cli/azure/ad/app/permission#az-ad-app-permission-add
[az-ad-app-permission-grant]: /cli/azure/ad/app/permission#az-ad-app-permission-grant
[az-ad-app-permission-admin-consent]: /cli/azure/ad/app/permission#az-ad-app-permission-admin-consent
[az-ad-app-show]: /cli/azure/ad/app#az-ad-app-show
[az-group-create]: /cli/azure/group#az-group-create
[az-account-show]: /cli/azure/account#az-account-show
[az-ad-signed-in-user-show]: /cli/azure/ad/signed-in-user#az-ad-signed-in-user-show
[azure-ad-portal]: azure-ad-integration.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-ad-sp-credential-reset]: /cli/azure/ad/sp/credential#az-ad-sp-credential-reset
[rbac-authorization]: concepts-identity.md#role-based-access-controls-rbac
[operator-best-practices-identity]: operator-best-practices-identity.md
[azure-ad-rbac]: azure-ad-rbac.md
