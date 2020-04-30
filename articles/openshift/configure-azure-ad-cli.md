---
title: Azure Red Hat OpenShift çalıştıran OpenShift 4-komut satırını kullanarak Azure Active Directory kimlik doğrulamasını yapılandırma
description: Komut satırını kullanarak OpenShift 4 çalıştıran bir Azure Red Hat OpenShift kümesi için Azure Active Directory kimlik doğrulamasını yapılandırma hakkında bilgi edinin
ms.service: container-service
ms.topic: article
ms.date: 03/12/2020
author: sabbour
ms.author: asabbour
keywords: Aro, OpenShift, az Aro, Red hat, CLI
ms.custom: mvc
ms.openlocfilehash: 45da3034891e5a82fb8423adb6bcd5e867f9d4e2
ms.sourcegitcommit: 67bddb15f90fb7e845ca739d16ad568cbc368c06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82205007"
---
# <a name="configure-azure-active-directory-authentication-for-an-azure-red-hat-openshift-4-cluster-cli"></a>Azure Red Hat OpenShift 4 kümesi için Azure Active Directory kimlik doğrulamasını yapılandırma (CLı)

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLı sürüm 2.0.75 veya üstünü çalıştırıyor olmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

Azure Active Directory uygulamasını yapılandırmak için kullanılacak olan kümenize özel URL 'Leri alın.

Kümenin OAuth geri çağırma URL 'sini oluşturun ve **Oauthcallbackurl**değişkenine depolayın. Kaynak grubunuzun adı ve **Aro-Cluster** ' i kümenizin **adıyla değiştirdiğinizden emin** olun.

> [!NOTE]
> OAuth `AAD` geri çağırma URL 'sindeki bölüm, daha sonra kurulumunu yaptığınız OAuth kimlik sağlayıcısı adıyla eşleşmelidir.

```azurecli-interactive
domain=$(az aro show -g aro-rg -n aro-cluster --query clusterProfile.domain -o tsv)
location=$(az aro show -g aro-rg -n aro-cluster --query location -o tsv)
apiServer=$(az aro show -g aro-rg -n aro-cluster --query apiserverProfile.url -o tsv)
webConsole=$(az aro show -g aro-rg -n aro-cluster --query consoleProfile.url -o tsv)
oauthCallbackURL=https://oauth-openshift.apps.$domain.$location.aroapp.io/oauth2callback/AAD
```

## <a name="create-an-azure-active-directory-application-for-authentication"></a>Kimlik doğrulaması için Azure Active Directory uygulaması oluşturma

Azure Active Directory uygulaması oluşturun ve oluşturulan uygulama tanımlayıcısını alın. ** \<ClientSecret>** , güvenli bir parolayla değiştirin.

```azurecli-interactive
az ad app create \
  --query appId -o tsv \
  --display-name aro-auth \
  --reply-urls $oauthCallbackURL \
  --password '<ClientSecret>'
```

Buna benzer bir şey geri almalısınız. Daha sonraki adımlarda ihtiyacınız olan **AppID** olduğundan bunu unutmayın.

```output
6a4cb4b2-f102-4125-b5f5-9ad6689f7224
```

Uygulamanın sahibi olan aboneliğin kiracı KIMLIĞINI alın.

```azure
az account show --query tenantId -o tsv
```

Buna benzer bir şey geri almalısınız. Daha sonraki adımlarda ihtiyacınız olan **Tenantıd** olduğundan bunu unutmayın.

```output
72f999sx-8sk1-8snc-js82-2d7cj902db47
```

## <a name="create-a-manifest-file-to-define-the-optional-claims-to-include-in-the-id-token"></a>KIMLIK belirtecine eklenecek isteğe bağlı talepleri tanımlamak için bir bildirim dosyası oluşturma

Uygulama geliştiricileri, kendi uygulamalarına hangi talepler gönderilmesini istediğinizi belirlemek için Azure AD uygulamalarında [isteğe bağlı talepler](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims) kullanabilir.

İsteğe bağlı talepleri şu şekilde kullanabilirsiniz:

- Uygulamanızın belirteçlerine dahil etmek için ek talepler ' i seçin.
- Azure AD 'nin belirteçlerde döndürdüğü belirli taleplerin davranışını değiştirin.
- Uygulamanız için özel talepler ekleyin ve erişin.

OpenShift 'i `email` talebi kullanacak ve `upn` Azure Active Directory tarafından döndürülen kimlik belirtecinin parçası `upn` olarak ekleyerek tercih edilen Kullanıcı adını ayarlamak için yapılandıracağız.

Azure Active Directory uygulamasını yapılandırmak için bir **manifest. JSON** dosyası oluşturun.

```bash
cat > manifest.json<< EOF
[{
  "name": "upn",
  "source": null,
  "essential": false,
  "additionalProperties": []
},
{
"name": "email",
  "source": null,
  "essential": false,
  "additionalProperties": []
}]
EOF
```

## <a name="update-the-azure-active-directory-applications-optionalclaims-with-a-manifest"></a>Azure Active Directory uygulamasının Optionaltaleplerini bir bildirimle güncelleştirme

** \<AppID>** , daha önce aldığınız kimlikle değiştirin.

```azurecli-interactive
az ad app update \
  --set optionalClaims.idToken=@manifest.json \
  --id <AppId>
```

## <a name="update-the-azure-active-directory-application-scope-permissions"></a>Uygulama kapsamı izinlerini Azure Active Directory güncelleştirme

Azure Active Directory Kullanıcı bilgilerini okuyabilmeniz için, uygun kapsamları tanımlamanız gerekir.

** \<AppID>** , daha önce aldığınız kimlikle değiştirin.

Oturum açmayı etkinleştirmek ve kullanıcı profilini okumak için **Azure Active Directory Graph. User. Read** kapsamı için izin ekleyin.

```azurecli-interactive
az ad app permission add \
 --api 00000002-0000-0000-c000-000000000000 \
 --api-permissions 311a71cc-e848-46a1-bdf8-97ff7156d8e6=Scope \
 --id <AppId>
```

> [!NOTE]
> Bu Azure Active Directory için genel yönetici olarak kimlik doğrulaması yapmadığınız sürece, kendi hesabınızda oturum açtıktan sonra bunu yapmanız istenecek olduğundan, izin vermek için iletiyi yoksayabilirsiniz.

## <a name="assign-users-and-groups-to-the-cluster-optional"></a>Kümeye Kullanıcı ve Grup atama (isteğe bağlı)

Bir Azure Active Directory (Azure AD) kiracısında kayıtlı uygulamalar, varsayılan olarak, başarıyla kimlik doğrulayan kiracının tüm kullanıcıları tarafından kullanılabilir. Azure AD, kiracı yöneticilerinin ve geliştiricilerin bir uygulamayı Kiracıdaki belirli bir kullanıcı veya güvenlik grubu kümesiyle kısıtlamasına olanak sağlar.

[Uygulamaya kullanıcılar ve gruplar atamak](https://docs.microsoft.com/azure/active-directory/develop/howto-restrict-your-app-to-a-set-of-users#app-registration)için Azure Active Directory belgelerindeki yönergeleri izleyin.

## <a name="configure-openshift-openid-authentication"></a>OpenShift OpenID kimlik doğrulamasını yapılandırma

`kubeadmin` Kimlik bilgilerini alın. `kubeadmin` Kullanıcı parolasını bulmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az aro list-credentials \
  --name aro-cluster \
  --resource-group aro-rg
```

Aşağıdaki örnek çıktı, parolasının içinde `kubeadminPassword`olacağını gösterir.

```json
{
  "kubeadminPassword": "<generated password>",
  "kubeadminUsername": "kubeadmin"
}
```

Aşağıdaki komutu kullanarak OpenShift kümesinin API sunucusunda oturum açın. `$apiServer` Değişken [daha önce]()ayarlandı. ** \<Kubeadmin Password>** , aldığınız parolayla değiştirin.

```azurecli-interactive
oc login $apiServer -u kubeadmin -p <kubeadmin password>
```

Azure Active Directory uygulama gizli anahtarını depolamak için bir OpenShift gizli dizisi oluşturun ve ** \<ClientSecret>** daha önce aldığınız gizli anahtar ile değiştirin.

```azurecli-interactive
oc create secret generic openid-client-secret-azuread \
  --namespace openshift-config \
  --from-literal=clientSecret=<ClientSecret>
```    

Azure Active Directory karşı OpenShift OpenID kimlik doğrulamasını yapılandırmak için bir **OIDC. YAML** dosyası oluşturun. ** \<AppID>** ve ** \<tenantıd>** daha önce aldığınız değerlerle değiştirin.

```bash
cat > oidc.yaml<< EOF
apiVersion: config.openshift.io/v1
kind: OAuth
metadata:
  name: cluster
spec:
  identityProviders:
  - name: AAD
    mappingMethod: claim
    type: OpenID
    openID:
      clientID: <AppId>
      clientSecret: 
        name: openid-client-secret-azuread
      extraScopes: 
      - email
      - profile
      extraAuthorizeParameters: 
        include_granted_scopes: "true"
      claims:
        preferredUsername: 
        - email
        - upn
        name: 
        - name
        email: 
        - email
      issuer: https://login.microsoftonline.com/<TenantId>
EOF
```

Yapılandırmayı kümeye uygulayın.

```azurecli-interactive
oc apply -f oidc.yaml
```

Aşağıdakine benzer bir yanıt geri alacaksınız.

```output
oauth.config.openshift.io/cluster configured
```

## <a name="verify-login-through-azure-active-directory"></a>Azure Active Directory aracılığıyla oturum açmayı doğrula

Artık OpenShift web konsolunun oturumunu kapatıp tekrar oturum açmayı denerseniz **AAD**ile oturum açma için yeni bir seçenek sunulur. Birkaç dakika beklemeniz gerekebilir.

![Azure Active Directory seçeneği ile oturum açma ekranı](media/aro4-login-2.png)
