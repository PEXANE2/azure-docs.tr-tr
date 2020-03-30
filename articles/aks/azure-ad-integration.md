---
title: Azure Active Directory'yi Azure Kubernetes Service ile tümleştirme
description: Azure Etkin Dizin etkin Azure Kubernetes Hizmeti (AKS) kümeleri nasıl oluşturulur?
services: container-service
ms.topic: article
ms.date: 02/02/2019
ms.openlocfilehash: 0476acadf5af3a3e2c470fe6c08ebbd355653e22
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77596598"
---
# <a name="integrate-azure-active-directory-with-azure-kubernetes-service"></a>Azure Active Directory'yi Azure Kubernetes Service ile tümleştirme

Azure Kubernetes Hizmeti (AKS), kullanıcı kimlik doğrulaması için Azure Active Directory (Azure AD) kullanacak şekilde yapılandırılabilir. Bu yapılandırmada, Azure AD kimlik doğrulama belirtecinizi kullanarak bir AKS kümesinde oturum açabilirsiniz.

Küme yöneticileri, Kubernetes rol tabanlı erişim denetimini (RBAC) kullanıcının kimliğine veya dizin grubu üyeliğine göre yapılandırabilir.

Bu makalede, nasıl açıklanmaktadır:

- AKS ve Azure AD için ön koşulları dağıtın.
- Azure AD özellikli bir küme dağıtın.
- Azure portalını kullanarak AKS kümesinde temel bir RBAC rolü oluşturun.

Azure [CLI'yi][azure-ad-cli]kullanarak da bu adımları tamamlayabilirsiniz.

> [!NOTE]
> Azure AD yalnızca RBAC özellikli yeni bir küme oluşturduğunuzda etkinleştirilebilir. Varolan bir AKS kümesinde Azure AD'yi etkinleştiremezsiniz.

## <a name="authentication-details"></a>Kimlik doğrulama ayrıntıları

Azure AD kimlik doğrulaması, OpenID Connect'e sahip AKS kümelerine sağlanır. OpenID Connect, OAuth 2.0 protokolünün üzerine kurulmuş bir kimlik katmanıdır.

OpenID Connect hakkında daha fazla bilgi için [OpenID Connect ve Azure AD'yi kullanarak web uygulamalarına erişim yetkisi][open-id-connect]ne rendele' ye bakın.

Bir Kubernetes kümesinin içinde, webhook belirteç kimlik doğrulaması kimlik doğrulama belirteçleri için kullanılır. Webhook belirteç kimlik doğrulaması, AKS kümesinin bir parçası olarak yapılandırılır ve yönetilir.

Webhook belirteç kimlik doğrulaması hakkında daha fazla bilgi için Kubernetes Documentation'deki [Webhook Belirteç Kimlik Doğrulama][kubernetes-webhook] bölümüne bakın.

Bir AKS kümesi için Azure AD kimlik doğrulaması sağlamak için iki Azure AD uygulaması oluşturulur. İlk uygulama, kullanıcı kimlik doğrulaması sağlayan bir sunucu bileşenidir. İkinci uygulama, kimlik doğrulaması için CLI tarafından istendiğinde kullanılan bir istemci bileşenidir. Bu istemci uygulaması istemci tarafından sağlanan kimlik bilgilerinin gerçek kimlik doğrulaması için sunucu uygulamasını kullanır.

> [!NOTE]
> AKS kimlik doğrulaması için Azure AD'yi yapılandırdığınızda, iki Azure AD uygulaması yapılandırılır. Her uygulama için izinleri devretme adımları bir Azure kiracı yöneticisi tarafından tamamlanmalıdır.

## <a name="create-the-server-application"></a>Sunucu uygulamasını oluşturma

İlk Azure REKLAM uygulaması, bir kullanıcının Azure AD grup üyeliğini almak için uygulanır. Bu uygulamayı Azure portalında oluşturmak için:

1. **Azure Active Directory** > **App kayıtlarını** > seçin**Yeni kayıt**.

    a. Uygulamaya *AKSAzureADServer*gibi bir ad verin.

    b. **Desteklenen hesap türleri için,** yalnızca bu kuruluş **dizinindeki Hesapları**seçin.
    
    c. URI'yi Yönlendirme türü için **Web'i** seçin ve uri biçimli herhangi bir değer *https://aksazureadserver*girin.

    d. Bittiğinde **Kaydol'u** seçin.

2. **Manifest'i**seçin ve ardından **groupMembershipClaims'i edin:** **Tüm**olarak değer. Güncelleştirmeleri bitirdikten sonra **Kaydet'i**seçin.

    ![Grup üyeliğini herkese güncelleştirin](media/aad-integration/edit-manifest.png)

3. Azure AD uygulamasının sol **bölmesinde, sertifikalar & sırları**seçin.

    a. + **Yeni istemci sırrını**seçin.

    b. *AKS Azure AD sunucusu*gibi önemli bir açıklama ekleyin. Bir son kullanma süresi seçin ve sonra **Ekle'yi**seçin.

    c. Yalnızca şu anda görüntülenen anahtar değerine dikkat edin. Azure AD özellikli bir AKS kümesi dağıttığınızda, bu değere sunucu uygulaması gizli adı verilir.

4. Azure AD uygulamasının sol bölmesinde **API izinlerini**seçin ve ardından **+ İzin Ekle'yi**seçin.

    a. **Microsoft API'leri**altında **Microsoft Graph'ı**seçin.

    b. **Temsilcili izinleri**seçin ve ardından **Dizin > Dizin.Read.All (Dizin verilerini oku)** yanındaki onay kutusunu seçin.

    c. **Kullanıcı > User.Read (Oturum aç ve oku kullanıcı profili)** için varsayılan olarak devredilen bir izin yoksa, yanındaki onay kutusunu seçin.

    d. **Uygulama izinlerini**seçin ve ardından **Dizin > Dizin.Read.All (Dizin verilerini oku)** yanındaki onay kutusunu seçin.

    ![Grafik izinlerini ayarlama](media/aad-integration/graph-permissions.png)

    e. Güncelleştirmeleri kaydetmek için **İzin Ekle'yi** seçin.

    f. **Grant onayı**altında, Grant **admin onayı**seçin. Bu düğme kullanılamaz kullanılan cari hesap kiracı yönetici olarak listelenmez.

    İzinler başarıyla verildiğinde, aşağıdaki bildirim portalda görüntülenir:

   ![Verilen başarılı izinlerin bildirimi](media/aad-integration/permissions-granted.png)

5. Azure AD uygulamasının sol bölmesinde **API'yi göster'i**ve ardından **kapsam ekle '** yi seçin.
    
    a. Kapsam **adı,** **Yönetici onayı görüntü adı**ve ardından *AKSAzureADServer*gibi bir **Yönetici onayı açıklaması** girin.

    b. **Durum'un** **Etkin**olarak ayarlandıklarına emin olun.

    ![Sunucu uygulamasını diğer hizmetlerde kullanılmak üzere API olarak gösterin](media/aad-integration/expose-api.png)

    c. **Kapsam Ekle'yi**seçin.

6. Uygulamaya geri dön **Genel Bakış** sayfasına ve **Uygulama (istemci) kimliğine**dikkat edin. Azure AD özellikli bir AKS kümesi dağıttığınızda, bu değere sunucu uygulama kimliği denir.

    ![Uygulama kimliği alın](media/aad-integration/application-id.png)

## <a name="create-the-client-application"></a>İstemci uygulamasını oluşturma

İkinci Azure REKLAM uygulaması, Kubernetes CLI (kubectl) ile oturum açtığınızda kullanılır.

1. **Azure Active Directory** > **App kayıtlarını** > seçin**Yeni kayıt**.

    a. Uygulamaya *AKSAzureADClient*gibi bir ad verin.

    b. **Desteklenen hesap türleri için,** yalnızca bu kuruluş **dizinindeki Hesapları**seçin.

    c. URI'yi Yönlendirme türü için **Web'i** seçin ve uri *https://aksazureadclient*biçimli herhangi bir değer girin.

    >[!NOTE]
    >Kapsayıcılar için Azure Monitor'u desteklemek için RBAC özellikli yeni bir küme oluşturuyorsanız, **web** uygulama türleri olarak bu listeye aşağıdaki iki ek yönlendirme URL'sini ekleyin. İlk temel URL değeri, `https://afd.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` ikinci temel URL `https://monitoring.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html`değeri ise.
    >
    >Bu özelliği Azure China'da kullanıyorsanız, ilk temel `https://afd.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` URL değeri ve ikinci `https://monitoring.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html`temel URL değeri olmalıdır.
    >
    >Daha fazla bilgi için, kapsayıcılar için Azure Monitor için [Canlı Veri (önizleme) özelliğini nasıl ayarları](../azure-monitor/insights/container-insights-livedata-setup.md) ve [Yapıl artırın AD tümleşik kimlik doğrulama](../azure-monitor/insights/container-insights-livedata-setup.md#configure-ad-integrated-authentication) sı altında kimlik doğrulamayı yapılandırma adımlarına bakın.

    d. Bittiğinde **Kaydol'u** seçin.

2. Azure AD uygulamasının sol bölmesinde **API izinlerini**seçin ve ardından **+ İzin Ekle'yi**seçin.

    a. **API'lerim'i**seçin ve ardından *AKSAzureADServer*gibi önceki adımda oluşturulan Azure AD sunucu uygulamanızı seçin.

    b. **Temsilci leştirilmiş izinleri**seçin ve ardından Azure AD sunucu uygulamanızın yanındaki onay kutusunu seçin.

    ![Uygulama izinlerini yapılandırma](media/aad-integration/select-api.png)

    c. **İzin Ekle'yi**seçin.

    d. **Grant onayı**altında, Grant **admin onayı**seçin. Cari hesap kiracı yönetici değilse, bu düğme kullanılamaz. İzinler verildiğinde, aşağıdaki bildirim portalda görüntülenir:

    ![Verilen başarılı izinlerin bildirimi](media/aad-integration/permissions-granted.png)

3. Azure AD uygulamasının sol bölmesinde **Kimlik Doğrulama'yı**seçin.

    - **Varsayılan istemci türü**altında, **istemciyi ortak istemci olarak ele**almak için **Evet'i** seçin.

5. Azure AD uygulamasının sol bölmesinde uygulama kimliğine dikkat edin. Azure AD özellikli bir AKS kümesi dağıttığınızda, bu değere istemci uygulama kimliği denir.

   ![Uygulama kimliğini alma](media/aad-integration/application-id-client.png)

## <a name="get-the-tenant-id"></a>Kiracı kimliğini alma

Ardından, Azure kiracınızın kimliğini alın. Bu değer, AKS kümesini oluştururken kullanılır.

Azure portalından **Azure Etkin Dizin** > **Özellikleri'ni** seçin ve **Dizin Kimliği'ne dikkat edin.** Azure AD özellikli bir AKS kümesi oluşturduğunuzda, bu değere kiracı kimliği denir.

![Azure kiracı kimliğini alma](media/aad-integration/tenant-id.png)

## <a name="deploy-the-aks-cluster"></a>AKS kümesini dağıtma

AKS kümesi için bir kaynak grubu oluşturmak için [az grubu oluşturma][az-group-create] komutunu kullanın.

```azurecli
az group create --name myResourceGroup --location eastus
```

AKS kümesini dağıtmak için [az aks oluşturma][az-aks-create] komutunu kullanın. Ardından, aşağıdaki örnek komuttaki değerleri değiştirin. Sunucu uygulaması kimliği, uygulama sırrı, istemci uygulaması kimliği ve kiracı kimliği için Azure AD uygulamalarını oluşturduğunuzda toplanan değerleri kullanın.

```azurecli
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --generate-ssh-keys \
  --aad-server-app-id b1536b67-29ab-4b63-b60f-9444d0c15df1 \
  --aad-server-app-secret wHYomLe2i1mHR2B3/d4sFrooHwADZccKwfoQwK2QHg= \
  --aad-client-app-id 8aaf8bd5-1bdd-4822-99ad-02bfaa63eea7 \
  --aad-tenant-id 72f988bf-0000-0000-0000-2d7cd011db47
```

AKS kümesinin oluşturulması birkaç dakika sürer.

## <a name="create-an-rbac-binding"></a>RBAC bağlama oluşturma

> [!NOTE]
> Küme rol bağlama adı büyük/küçük harf duyarlıdır.

AKS kümesine sahip bir Azure Active Directory hesabını kullanmadan önce rol bağlama veya küme rol bağlama oluşturmanız gerekir. Roller, verme izinlerini tanımlar ve bağlamalar bunları istenen kullanıcılara uygular. Bu atamalar belirli bir ad alanına veya tüm kümeye uygulanabilir. Daha fazla bilgi için [RBAC yetkilendirmesi kullanma'ya][rbac-authorization]bakın.

İlk olarak, yönetici erişimi ile kümede `--admin` oturum açma bağımsız değişkeni ile [az aks get-credentials][az-aks-get-credentials] komutunu kullanın.

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster --admin
```

Ardından, AKS kümesine erişim izni vermek istediğiniz bir Azure REKLAM hesabı için ClusterRoleBinding'i oluşturun. Aşağıdaki örnek, hesaba kümedeki tüm ad alanlarına tam erişim sağlar:

- RBAC bağlayıcısını hibe ettiğiniz kullanıcı aynı Azure AD kiracısındaysa, kullanıcı ana adına (UPN) göre izinler atayın. ClusterRoleBinding için YAML bildirimini oluşturmak için adıma geçin.

- Kullanıcı farklı bir Azure AD kiracıysa, bunun yerine **objectId** özelliğini sorgula ve kullanın. Gerekirse, [az reklam kullanıcı göster][az-ad-user-show] komutunu kullanarak gerekli kullanıcı hesabının objectId'ini alın. Gerekli hesabın kullanıcı ana adı (UPN) sağlayın:

    ```azurecli-interactive
    az ad user show --upn-or-object-id user@contoso.com --query objectId -o tsv
    ```

*Rbac-aad-user.yaml*gibi bir dosya oluşturun ve ardından aşağıdaki içeriği yapıştırın. Son satırda, **userPrincipalName_or_objectId** UPN veya nesne kimliğiyle değiştirin. Seçim, kullanıcının aynı Azure AD kiracısı olup olmadığına bağlıdır.

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

Aşağıdaki örnekte gösterildiği gibi [kubectl uygula][kubectl-apply] komutunu kullanarak bağlama uygulayın:

```console
kubectl apply -f rbac-aad-user.yaml
```

Bir Azure REKLAM grubunun tüm üyeleri için rol bağlama da oluşturulabilir. Azure REKLAM grupları, aşağıdaki örnekte gösterildiği gibi grup nesnekimliği kullanılarak belirtilir.

*Rbac-aad-group.yaml*gibi bir dosya oluşturun ve ardından aşağıdaki içeriği yapıştırın. Grup nesnesi kimliğini Azure AD kiracınızdan biriyle güncelleştirin:

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
   kind: Group
   name: "894656e1-39f8-4bfe-b16a-510f61af6f41"
```

Aşağıdaki örnekte gösterildiği gibi [kubectl uygula][kubectl-apply] komutunu kullanarak bağlama uygulayın:

```console
kubectl apply -f rbac-aad-group.yaml
```

RBAC ile bir Kubernetes kümesini güvence altına alma hakkında daha fazla bilgi için Bkz. [RBAC Yetkilendirmekullanma.][rbac-authorization]

## <a name="access-the-cluster-with-azure-ad"></a>Azure AD ile kümeye erişin

[Az aks get-credentials][az-aks-get-credentials] komutunu kullanarak yönetici olmayan kullanıcının bağlamını çekin.

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Komutu `kubectl` çalıştırdıktan sonra Azure'u kullanarak kimliğinizin doğrulanması istenir. Aşağıdaki örnekte gösterildiği gibi, işlemi tamamlamak için ekrandaki yönergeleri izleyin:

```console
$ kubectl get nodes

To sign in, use a web browser to open https://microsoft.com/devicelogin. Next, enter the code BUJHWDGNL to authenticate.

NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-79590246-0   Ready     agent     1h        v1.13.5
aks-nodepool1-79590246-1   Ready     agent     1h        v1.13.5
aks-nodepool1-79590246-2   Ready     agent     1h        v1.13.5
```

İşlem tamamlandığında, kimlik doğrulama belirteci önbelleğe aledilir. Yalnızca belirteç sona erdiğinde veya Kubernetes config dosyası yeniden oluşturulduğunda yeniden oturum açmanız istenir.

Başarılı bir şekilde oturum açınca bir yetkilendirme hatası iletisi görürseniz, aşağıdaki ölçütleri denetleyin:

```console
error: You must be logged in to the server (Unauthorized)
```


- Kullanıcı hesabının aynı Azure AD kiracısında olup olmamasına bağlı olarak uygun nesne kimliğini veya UPN'yi tanımladınız.
- Kullanıcı 200'den fazla grubun üyesi değildir.
- Sunucu için uygulama kaydında tanımlanan gizli kullanılarak `--aad-server-app-secret`yapılandırılan değerle eşleşir.

## <a name="next-steps"></a>Sonraki adımlar

Küme kaynaklarına erişimi denetlemek için Azure AD kullanıcılarını ve gruplarını kullanmak için, [AKS'de rol tabanlı erişim denetimi ve Azure AD kimliklerini kullanarak küme kaynaklarına erişimi denetleme'ye][azure-ad-rbac]bakın.

Kubernetes kümelerinin nasıl güvenli hale sağlanın sağlanıp sağlanabildiğini öğrenmek [için AKS için Erişim ve kimlik seçeneklerine][rbac-authorization]bakın.

Kimlik ve kaynak denetimi hakkında daha fazla bilgi edinmek [için AKS'de kimlik doğrulama ve yetkilendirme için en iyi uygulamalara][operator-best-practices-identity]bakın.

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[open-id-connect]:../active-directory/develop/v2-protocols-oidc.md
[az-ad-user-show]: /cli/azure/ad/user#az-ad-user-show
[rbac-authorization]: concepts-identity.md#role-based-access-controls-rbac
[operator-best-practices-identity]: operator-best-practices-identity.md
[azure-ad-rbac]: azure-ad-rbac.md
[azure-ad-cli]: azure-ad-integration-cli.md
