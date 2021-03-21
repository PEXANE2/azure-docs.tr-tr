---
title: Azure Red Hat OpenShift çalıştıran OpenShift 4-komut satırını kullanarak Azure Active Directory kimlik doğrulamasını yapılandırma
description: Komut satırını kullanarak OpenShift 4 çalıştıran bir Azure Red Hat OpenShift kümesi için Azure Active Directory kimlik doğrulamasını yapılandırma hakkında bilgi edinin
ms.service: azure-redhat-openshift
ms.topic: article
ms.date: 03/12/2020
author: sabbour
ms.author: asabbour
keywords: Aro, OpenShift, az Aro, Red hat, CLI
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 06f7bfea9a88627733eb9ce9166e05d05790e23a
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102213071"
---
# <a name="configure-azure-active-directory-authentication-for-an-azure-red-hat-openshift-4-cluster-cli"></a>Azure Red Hat OpenShift 4 kümesi için Azure Active Directory kimlik doğrulamasını yapılandırma (CLı)

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLı sürüm 2.6.0 veya üstünü çalıştırıyor olmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

Azure Active Directory uygulamasını yapılandırmak için kullanılacak olan kümenize özel URL 'Leri alın.

Kaynak grubu ve küme adı için değişkenleri ayarlayın.

**\<resource_group>** Kaynak grubunuzun adıyla ve **\<aro_cluster>** kümenizin adıyla değiştirin.

```azurecli-interactive
resource_group=<resource_group>
aro_cluster=<aro_cluster>
```

Kümenin OAuth geri çağırma URL 'sini oluşturun ve **Oauthcallbackurl** değişkenine depolayın. 

> [!NOTE]
> `AAD`OAuth geri çağırma URL 'sindeki bölüm, daha sonra kurulumunu yaptığınız OAuth kimlik sağlayıcısı adıyla eşleşmelidir.


```azurecli-interactive
domain=$(az aro show -g $resource_group -n $aro_cluster --query clusterProfile.domain -o tsv)
location=$(az aro show -g $resource_group -n $aro_cluster --query location -o tsv)
apiServer=$(az aro show -g $resource_group -n $aro_cluster --query apiserverProfile.url -o tsv)
webConsole=$(az aro show -g $resource_group -n $aro_cluster --query consoleProfile.url -o tsv)
```

OauthCallbackURL 'nin biçimi özel etki alanlarıyla biraz farklıdır:

* Özel bir etki alanı kullanıyorsanız, örneğin `contoso.com` . 

    ```azurecli-interactive
    oauthCallbackURL=https://oauth-openshift.apps.$domain/oauth2callback/AAD
    ```

* Özel bir etki alanı kullanmıyorsanız, `$domain` sekiz karakter alnum dizesi olur ve tarafından genişletilir `$location.aroapp.io` .

    ```azurecli-interactive
    oauthCallbackURL=https://oauth-openshift.apps.$domain.$location.aroapp.io/oauth2callback/AAD
    ```

> [!NOTE]
> `AAD`OAuth geri çağırma URL 'sindeki bölüm, daha sonra kurulumunu yaptığınız OAuth kimlik sağlayıcısı adıyla eşleşmelidir.

## <a name="create-an-azure-active-directory-application-for-authentication"></a>Kimlik doğrulaması için Azure Active Directory uygulaması oluşturma

**\<client_secret>** Uygulamanın güvenli parolasıyla değiştirin.

```azurecli-interactive
client_secret=<client_secret>
```

Azure Active Directory uygulaması oluşturun ve oluşturulan uygulama tanımlayıcısını alın.

```azurecli-interactive
app_id=$(az ad app create \
  --query appId -o tsv \
  --display-name aro-auth \
  --reply-urls $oauthCallbackURL \
  --password $client_secret)
```

Uygulamanın sahibi olan aboneliğin kiracı KIMLIĞINI alın.

```azure
tenant_id=$(az account show --query tenantId -o tsv)
```

## <a name="create-a-manifest-file-to-define-the-optional-claims-to-include-in-the-id-token"></a>KIMLIK belirtecine eklenecek isteğe bağlı talepleri tanımlamak için bir bildirim dosyası oluşturma

Uygulama geliştiricileri, kendi uygulamalarına hangi talepler gönderilmesini istediğinizi belirlemek için Azure AD uygulamalarında [isteğe bağlı talepler](../active-directory/develop/active-directory-optional-claims.md) kullanabilir.

İsteğe bağlı talepleri şu şekilde kullanabilirsiniz:

- Uygulamanızın belirteçlerine dahil etmek için ek talepler ' i seçin.
- Azure AD 'nin belirteçlerde döndürdüğü belirli taleplerin davranışını değiştirin.
- Uygulamanız için özel talepler ekleyin ve erişin.

OpenShift 'i `email` talebi kullanacak ve `upn` `upn` Azure ACTIVE DIRECTORY tarafından döndürülen kimlik belirtecinin parçası olarak ekleyerek tercih edilen Kullanıcı adını ayarlamak için yapılandıracağız.

Azure Active Directory uygulamasını yapılandırmak için dosyada **manifest.js** oluşturun.

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

```azurecli-interactive
az ad app update \
  --set optionalClaims.idToken=@manifest.json \
  --id $app_id
```

## <a name="update-the-azure-active-directory-application-scope-permissions"></a>Uygulama kapsamı izinlerini Azure Active Directory güncelleştirme

Azure Active Directory Kullanıcı bilgilerini okuyabilmeniz için, uygun kapsamları tanımlamanız gerekir.

Oturum açmayı etkinleştirmek ve kullanıcı profilini okumak için **Azure Active Directory Graph. User. Read** kapsamı için izin ekleyin.

```azurecli-interactive
az ad app permission add \
 --api 00000002-0000-0000-c000-000000000000 \
 --api-permissions 311a71cc-e848-46a1-bdf8-97ff7156d8e6=Scope \
 --id $app_id
```

> [!NOTE]
> Bu Azure Active Directory için genel yönetici olarak kimlik doğrulaması yapmadığınız sürece onay vermek üzere iletiyi güvenle yoksayabilirsiniz. Standart etki alanı kullanıcılarına, AAD kimlik bilgilerini kullanarak kümede ilk kez oturum açtıklarında izin vermesi istenir.

## <a name="assign-users-and-groups-to-the-cluster-optional"></a>Kümeye Kullanıcı ve Grup atama (isteğe bağlı)

Bir Azure Active Directory (Azure AD) kiracısında kayıtlı uygulamalar, varsayılan olarak, başarıyla kimlik doğrulayan kiracının tüm kullanıcıları tarafından kullanılabilir. Azure AD, kiracı yöneticilerinin ve geliştiricilerin bir uygulamayı Kiracıdaki belirli bir kullanıcı veya güvenlik grubu kümesiyle kısıtlamasına olanak sağlar.

[Uygulamaya kullanıcılar ve gruplar atamak](../active-directory/develop/howto-restrict-your-app-to-a-set-of-users.md#app-registration)için Azure Active Directory belgelerindeki yönergeleri izleyin.

## <a name="configure-openshift-openid-authentication"></a>OpenShift OpenID kimlik doğrulamasını yapılandırma

`kubeadmin`Kimlik bilgilerini alın. Kullanıcı parolasını bulmak için aşağıdaki komutu çalıştırın `kubeadmin` .

```azurecli-interactive
kubeadmin_password=$(az aro list-credentials \
  --name $aro_cluster \
  --resource-group $resource_group \
  --query kubeadminPassword --output tsv)
```

Aşağıdaki komutu kullanarak OpenShift kümesinin API sunucusunda oturum açın. 

```azurecli-interactive
oc login $apiServer -u kubeadmin -p $kubeadmin_password
```

Azure Active Directory uygulama gizli anahtarını depolamak için bir OpenShift gizli dizisi oluşturun.

```azurecli-interactive
oc create secret generic openid-client-secret-azuread \
  --namespace openshift-config \
  --from-literal=clientSecret=$client_secret
```

Azure Active Directory karşı OpenShift OpenID kimlik doğrulamasını yapılandırmak için bir **OIDC. YAML** dosyası oluşturun. 

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
      clientID: $app_id
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
      issuer: https://login.microsoftonline.com/$tenant_id
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

Artık OpenShift web konsolunun oturumunu kapatıp tekrar oturum açmayı denerseniz **AAD** ile oturum açma için yeni bir seçenek sunulur. Birkaç dakika beklemeniz gerekebilir.

![Azure Active Directory seçeneği ile oturum açma ekranı](media/aro4-login-2.png)
