---
title: Azure Red Hat OpenShift çalıştıran OpenShift 4-Azure portal ve OpenShift web konsolunu kullanarak Azure Active Directory kimlik doğrulamasını yapılandırma
description: Azure portal ve OpenShift web konsolunu kullanarak OpenShift 4 çalıştıran bir Azure Red Hat OpenShift kümesi için Azure Active Directory kimlik doğrulamasını yapılandırma hakkında bilgi edinin
ms.service: container-service
ms.topic: article
ms.date: 03/12/2020
author: sabbour
ms.author: asabbour
keywords: Aro, OpenShift, az Aro, Red hat, CLI
ms.custom: mvc
ms.openlocfilehash: 2cb54c202af04996080cda970b3d327145f0e72b
ms.sourcegitcommit: 4feb198becb7a6ff9e6b42be9185e07539022f17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89469890"
---
# <a name="configure-azure-active-directory-authentication-for-an-azure-red-hat-openshift-4-cluster-portal"></a>Azure Red Hat OpenShift 4 kümesi için Azure Active Directory kimlik doğrulamasını yapılandırma (portal)

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğreticide, Azure CLı sürüm 2.6.0 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="before-you-begin"></a>Başlamadan önce

Kümenin **OAuth geri çağırma URL 'sini** oluşturun ve bunu unutmayın. Kaynak grubunuzun adı ve **Aro-Cluster** ' i kümenizin **adıyla değiştirdiğinizden emin** olun.

> [!NOTE]
> `AAD`OAuth geri çağırma URL 'sindeki bölüm, daha sonra kurulumunu yaptığınız OAuth kimlik sağlayıcısı adıyla eşleşmelidir.

```azurecli-interactive
domain=$(az aro show -g aro-rg -n aro-cluster --query clusterProfile.domain -o tsv)
location=$(az aro show -g aro-rg -n aro-cluster --query location -o tsv)
echo "OAuth callback URL: https://oauth-openshift.apps.$domain.$location.aroapp.io/oauth2callback/AAD"
```

## <a name="create-an-azure-active-directory-application-for-authentication"></a>Kimlik doğrulaması için Azure Active Directory uygulaması oluşturma

Azure portal oturum açın ve [uygulama kayıtları dikey penceresine](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)gidin ve yeni **kayıt** ' a tıklayarak yeni bir uygulama oluşturun.

Uygulama için bir ad sağlayın (örneğin, **Aro-azuread-auth**) ve daha önce aldığınız OAuth geri arama URL 'si değerini kullanarak **yeniden yönlendirme URI** 'sini girin.

![Yeni uygulama kaydı](media/aro4-ad-registerapp.png)

**Sertifikalar & gizlilikler** ' a gidin ve **yeni istemci parolası** ' na tıklayın ve ayrıntıları girin. Daha sonraki bir aşamada kullanacağınız için anahtar değerini unutmayın. Yeniden alamazsınız.

![Gizli anahtar oluşturma](media/aro4-ad-clientsecret.png)

**Genel Bakış ' a** gidin ve **uygulamanın (istemci) kimliğini** ve **Dizin (kiracı) kimliğini**unutmayın. Daha sonraki bir aşamada ihtiyacınız olacak.

![Uygulama (istemci) ve dizin (kiracı) kimliklerini alma](media/aro4-ad-ids.png)

## <a name="configure-optional-claims"></a>İsteğe bağlı talepleri yapılandırma

Uygulama geliştiricileri, kendi uygulamalarına hangi talepler gönderilmesini istediğinizi belirlemek için Azure AD uygulamalarında [isteğe bağlı talepler](../active-directory/develop/active-directory-optional-claims.md) kullanabilir.

İsteğe bağlı talepleri şu şekilde kullanabilirsiniz:

* Uygulamanızın belirteçlerine dahil etmek için ek talepler ' i seçin.
* Azure AD 'nin belirteçlerde döndürdüğü belirli taleplerin davranışını değiştirin.
* Uygulamanız için özel talepler ekleyin ve erişin.

OpenShift 'i `email` talebi kullanacak ve `upn` `upn` Azure ACTIVE DIRECTORY tarafından döndürülen kimlik belirtecinin parçası olarak ekleyerek tercih edilen Kullanıcı adını ayarlamak için yapılandıracağız.

**Belirteç yapılandırması (Önizleme)** bölümüne gidin ve **Isteğe bağlı talep Ekle**' ye tıklayın. **Kimlik** ' i seçin ve ardından **e-posta** ve **UPN** taleplerini denetleyin.

![Gizli anahtar oluşturma](media/aro4-ad-tokens.png)

## <a name="assign-users-and-groups-to-the-cluster-optional"></a>Kümeye Kullanıcı ve Grup atama (isteğe bağlı)

Bir Azure Active Directory (Azure AD) kiracısında kayıtlı uygulamalar, varsayılan olarak, başarıyla kimlik doğrulayan kiracının tüm kullanıcıları tarafından kullanılabilir. Azure AD, kiracı yöneticilerinin ve geliştiricilerin bir uygulamayı Kiracıdaki belirli bir kullanıcı veya güvenlik grubu kümesiyle kısıtlamasına olanak sağlar.

[Uygulamaya kullanıcılar ve gruplar atamak](../active-directory/develop/howto-restrict-your-app-to-a-set-of-users.md#app-registration)için Azure Active Directory belgelerindeki yönergeleri izleyin.

## <a name="configure-openshift-openid-authentication"></a>OpenShift OpenID kimlik doğrulamasını yapılandırma

`kubeadmin`Kimlik bilgilerini alın. Kullanıcı parolasını bulmak için aşağıdaki komutu çalıştırın `kubeadmin` .

```azurecli-interactive
az aro list-credentials \
  --name aro-cluster \
  --resource-group aro-rg
```

Aşağıdaki örnek çıktı, parolasının içinde olacağını gösterir `kubeadminPassword` .

```json
{
  "kubeadminPassword": "<generated password>",
  "kubeadminUsername": "kubeadmin"
}
```

Aşağıdaki komutu çalıştırarak küme konsolu URL 'sini bulabilirsiniz, şöyle görünür `https://console-openshift-console.apps.<random>.<region>.aroapp.io/`

```azurecli-interactive
 az aro show \
    --name aro-cluster \
    --resource-group aro-rg \
    --query "consoleProfile.url" -o tsv
```

Konsol URL 'sini bir tarayıcıda başlatın ve kimlik bilgilerini kullanarak oturum açın `kubeadmin` .

**Yönetim**' e gidin, **küme ayarları**' na tıklayın ve ardından **genel yapılandırma** sekmesini seçin. **OAuth**Seç ' e gidin.

Aşağı kaydırarak **kimlik sağlayıcıları** altında **Ekle** ' yi seçin ve **OpenID Connect**' i seçin.
![Kimlik sağlayıcıları açılan listesinden OpenID Connect ' i seçin](media/aro4-oauth-idpdrop.png)

Adı **AAD**olarak, **ISTEMCI kimliğini** **uygulama kimliği** ve **istemci parolası**olarak girin. **Veren URL 'si** şöyle biçimlendirilir: `https://login.microsoftonline.com/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` . Yer tutucusunu, daha önce aldığınız kiracı KIMLIĞIYLE değiştirin.

![OAuth ayrıntılarını doldur](media/aro4-oauth-idp-1.png)

Aşağı kaydırarak **talepler** bölümüne gidin ve **tercih edilen Kullanıcı adını** **UPN** talebinin değerini kullanacak şekilde güncelleştirin.

![Talep ayrıntılarını doldur](media/aro4-oauth-idp-2.png)

## <a name="verify-login-through-azure-active-directory"></a>Azure Active Directory aracılığıyla oturum açmayı doğrula

Artık OpenShift web konsolunun oturumunu kapatıp yeniden oturum açmayı denerseniz **AAD**ile oturum açma için yeni bir seçenek sunulur. Birkaç dakika beklemeniz gerekebilir.

![Azure Active Directory seçeneği ile oturum açma ekranı](media/aro4-login-2.png)