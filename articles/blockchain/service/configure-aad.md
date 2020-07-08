---
title: Azure Active Directory erişimini Yapılandırma-Azure blok zinciri hizmeti
description: Azure blok zinciri hizmetini Azure Active Directory erişimi ile yapılandırma
ms.date: 11/22/2019
ms.topic: how-to
ms.reviewer: janders
ms.openlocfilehash: 337d01abc51d310d06aeea3427b770132be4824c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85208782"
---
# <a name="how-to-configure-azure-active-directory-access-for-azure-blockchain-service"></a>Azure blok zinciri hizmeti için Azure Active Directory erişimi yapılandırma

Bu makalede, Azure Active Directory (Azure AD) Kullanıcı, Grup veya uygulama kimliklerini kullanarak Azure blok zinciri hizmeti düğümlerine nasıl erişim ve bağlantı sağlayacağınızı öğreneceksiniz.

Azure AD, bulut tabanlı kimlik yönetimi sağlar ve Azure 'daki tüm kurumsal ve erişim uygulamaları genelinde tek bir kimlik kullanmanıza olanak sağlar. Azure blok zinciri hizmeti, Azure AD ile tümleşiktir ve KIMLIK Federasyonu, çoklu oturum açma ve çok faktörlü kimlik doğrulaması gibi avantajlar sağlar.

## <a name="prerequisites"></a>Ön koşullar

* [Azure portal kullanarak bir blok zinciri üyesi oluşturma](create-member.md)

## <a name="grant-access"></a>Erişim verme

Hem üye düzeyinde hem de düğüm düzeyinde erişim verebilirsiniz. Üye düzeyinde erişim hakları verilmesi, üyenin altındaki tüm düğümlere erişim hakkı verir.

### <a name="grant-member-level-access"></a>Üye düzeyinde erişim verme

Üye düzeyinde erişim izni vermek için.

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. **Erişim denetimi (IAM) > > rol ataması Ekle**' ye gidin.
1. **Blok zinciri üye düğümü erişimi (Önizleme)** rolünü seçin ve erişim vermek ISTEDIĞINIZ Azure AD kimlik nesnesini ekleyin. Azure AD KIMLIK nesnesi şu olabilir:

    | Azure AD nesnesi | Örnek |
    |-----------------|---------|
    | Azure AD kullanıcısı   | `kim@contoso.onmicrosoft.com` |
    | Azure AD grubu  | `sales@contoso.onmicrosoft.com` |
    | Uygulama Kimliği  | `13925ab1-4161-4534-8d18-812f5ca1ab1e` |

    ![Rol ataması ekle](./media/configure-aad/add-role-assignment.png)

1. **Kaydet**'i seçin.

### <a name="grant-node-level-access"></a>Düğüm düzeyinde erişim verme

Düğüm güvenliği ' ne giderek, erişim vermek istediğiniz düğüm adına tıklayarak düğüm düzeyinde erişim izni verebilirsiniz.

Blok zinciri üye düğümü erişimi (Önizleme) rolünü seçin ve erişim vermek istediğiniz Azure AD KIMLIK nesnesini ekleyin.

Daha fazla bilgi için bkz. [Azure blok zinciri hizmeti işlem düğümlerini yapılandırma](configure-transaction-nodes.md#azure-active-directory-access-control).

## <a name="connect-using-azure-blockchain-connector"></a>Azure blok zinciri bağlayıcısını kullanarak bağlanma

[Azure blok zinciri bağlayıcısını GitHub 'dan](https://github.com/Microsoft/azure-blockchain-connector/)indirin veya kopyalayın.

```bash
git clone https://github.com/Microsoft/azure-blockchain-connector.git
```

Kaynak kodundan bağlayıcıyı derlemek için **Benioku** dosyasındaki hızlı başlangıç bölümünü izleyin.

### <a name="connect-using-an-azure-ad-user-account"></a>Azure AD Kullanıcı hesabı kullanarak bağlanma

1. Azure AD Kullanıcı hesabı kullanarak kimlik doğrulamak için aşağıdaki komutu çalıştırın. \<myAADDirectory\>Bir Azure AD etki alanı ile değiştirin. Örneğin, `yourdomain.onmicrosoft.com`.

    ```
    connector.exe -remote <myMemberName>.blockchain.azure.com:3200 -method aadauthcode -tenant-id <myAADDirectory> 
    ```

1. Azure AD kimlik bilgileri ister.
1. Kullanıcı adınız ve parolanızla oturum açın.
1. Kimlik doğrulaması başarılı olduğunda, yerel ara sunucunuz blok zinciri düğümünüz ile bağlantı kurar. Şimdi geth istemcinizi Yerel uç noktaya ekleyebilirsiniz.

    ```bash
    geth attach http://127.0.0.1:3100
    ```

### <a name="connect-using-an-application-id"></a>Uygulama KIMLIĞI kullanarak bağlan

Birçok uygulama, Azure AD Kullanıcı hesabı yerine bir uygulama KIMLIĞI kullanarak Azure AD ile kimlik doğrular.

Bir uygulama KIMLIĞI kullanarak düğümünüz ile bağlantı kurmak için **aadauthcode** 'u **aadclient**ile değiştirin.

```
connector.exe -remote <myBlockchainEndpoint>  -method aadclient -client-id <myClientID> -client-secret "<myClientSecret>" -tenant-id <myAADDirectory>
```

| Parametre | Açıklama |
|-----------|-------------|
| Kiracı kimliği | Örneğin, Azure AD etki alanı`yourdomain.onmicrosoft.com`
| istemci kimliği | Azure AD 'de kayıtlı uygulamanın istemci KIMLIĞI
| istemci parolası | Azure AD 'de kayıtlı uygulamanın istemci gizli anahtarı

Bir uygulamayı Azure AD 'ye kaydetme hakkında daha fazla bilgi için bkz [. nasıl yapılır: Azure AD uygulaması ve kaynaklara erişebilen hizmet sorumlusu oluşturmak için portalı kullanma](../../active-directory/develop/howto-create-service-principal-portal.md)

### <a name="connect-a-mobile-device-or-text-browser"></a>Mobil cihaz veya metin tarayıcısını bağlama

Azure AD kimlik doğrulaması açılır ekran 'nin mümkün olmadığı bir mobil cihaz veya metin tabanlı tarayıcı için, Azure AD bir kerelik geçiş kodu oluşturur. Geçiş kodunu kopyalayabilir ve başka bir ortamda Azure AD kimlik doğrulaması ile devam edebilirsiniz.

Geçiş kodunu oluşturmak için **aadauthcode** değerini **aaddevice**ile değiştirin. \<myAADDirectory\>Bir Azure AD etki alanı ile değiştirin. Örneğin, `yourdomain.onmicrosoft.com`.

```
connector.exe -remote <myBlockchainEndpoint>  -method aaddevice -tenant-id <myAADDirectory>
```

## <a name="next-steps"></a>Sonraki adımlar

Azure blok zinciri hizmeti 'nde veri güvenliği hakkında daha fazla bilgi için bkz. [Azure blok zinciri hizmeti güvenliği](data-security.md).