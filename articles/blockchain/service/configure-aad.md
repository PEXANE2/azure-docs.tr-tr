---
title: Azure Active Directory erişimini yapılandırma - Azure Blockchain Hizmeti
description: Azure Etkin Dizin erişimi yle Azure Blockchain Hizmeti nasıl yapılandırılabilirsin?
ms.date: 11/22/2019
ms.topic: article
ms.reviewer: janders
ms.openlocfilehash: 682ab282036fcd592e66942d08a84cdce46d8915
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74455854"
---
# <a name="how-to-configure-azure-active-directory-access-for-azure-blockchain-service"></a>Azure Blockchain Hizmeti için Azure Active Directory erişimi nasıl yapılandırılabilen

Bu makalede, Azure Active Directory (Azure AD) kullanıcı, grup veya uygulama adlarını kullanarak Azure Blockchain Hizmet düğümlerine nasıl erişim ve bağlanma izni verilip bağlanabileceğinizi öğreneceksiniz.

Azure AD bulut tabanlı kimlik yönetimi sağlar ve tüm kuruluş genelinde tek bir kimlik kullanmanıza ve Azure'daki uygulamalara erişmenize olanak tanır. Azure Blockchain Hizmeti Azure AD ile entegre edilmiştir ve kimlik federasyonu, tek oturum açma ve çok faktörlü kimlik doğrulama gibi avantajlar sunar.

## <a name="prerequisites"></a>Ön koşullar

* [Azure portalını kullanarak blockchain üyesi oluşturma](create-member.md)

## <a name="grant-access"></a>Erişim verme

Hem üye düzeyinde hem de düğüm düzeyinde erişim izni verebilirsiniz. Üye düzeyinde erişim hakkı verilmesi de üye altındaki tüm düğümlere erişim sağlayacaktır.

### <a name="grant-member-level-access"></a>Üye düzeyinde erişim izni

Üye düzeyinde erişim izni vermek için.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Access **denetimine (IAM) gidin > rol ataması > ekle.**
1. **Blockchain Üye Düğümü Erişimi (Önizleme)** rolünü seçin ve erişim izni vermek istediğiniz Azure AD Kimliği nesnesini ekleyin. Azure AD KIMLIĞI nesnesi şu olabilir:

    | Azure AD nesnesi | Örnek |
    |-----------------|---------|
    | Azure AD kullanıcısı   | `kim@contoso.onmicrosoft.com` |
    | Azure REKLAM grubu  | `sales@contoso.onmicrosoft.com` |
    | Uygulama Kimliği  | `13925ab1-4161-4534-8d18-812f5ca1ab1e` |

    ![Rol ataması ekle](./media/configure-aad/add-role-assignment.png)

1. **Kaydet'i**seçin.

### <a name="grant-node-level-access"></a>Düğüm düzeyi erişimini hibe

Düğüm güvenliğine gidip erişim vermek istediğiniz düğüm adını tıklatarak düğüm düzeyinde erişim sağlayabilirsiniz.

Blockchain Üye Düğümü Erişimi (Önizleme) rolünü seçin ve erişim izni vermek istediğiniz Azure AD Kimliği nesnesini ekleyin.

Daha fazla bilgi için bkz. [Azure Blockchain Hizmeti işlem düğümlerini yapılandır.](configure-transaction-nodes.md#azure-active-directory-access-control)

## <a name="connect-using-azure-blockchain-connector"></a>Azure Blockchain Konektörü kullanarak bağlanma

[Azure Blockchain Bağlayıcısını GitHub'dan](https://github.com/Microsoft/azure-blockchain-connector/)indirin veya klonlayın.

```bash
git clone https://github.com/Microsoft/azure-blockchain-connector.git
```

Kaynak kodundan bağlayıcıyı oluşturmak için **okuma me'deki** hızlı başlangıç bölümünü izleyin.

### <a name="connect-using-an-azure-ad-user-account"></a>Azure AD kullanıcı hesabı kullanarak bağlanma

1. Azure AD kullanıcı hesabı kullanarak kimlik doğrulaması yapmak için aşağıdaki komutu çalıştırın. myAADDirectory'yi azure AD etki alanıyla değiştirin. \<\> Örneğin, `yourdomain.onmicrosoft.com`.

    ```
    connector.exe -remote <myMemberName>.blockchain.azure.com:3200 -method aadauthcode -tenant-id <myAADDirectory> 
    ```

1. Azure AD kimlik bilgileri için istemleri.
1. Kullanıcı adınız ve şifrenizle oturum açın.
1. Başarılı bir kimlik doğrulaması üzerine, yerel proxy'niz blockchain düğümünüze bağlanır. Artık Geth istemcinizi yerel bitiş noktasına ekleyebilirsiniz.

    ```bash
    geth attach http://127.0.0.1:3100
    ```

### <a name="connect-using-an-application-id"></a>Uygulama kimliğini kullanarak bağlanma

Birçok uygulama, Azure AD kullanıcı hesabı yerine bir uygulama kimliği kullanarak Azure AD ile kimlik doğrulaması sağlar.

Bir uygulama kimliği kullanarak düğümünüze bağlanmak için **aadauthcode'u** **aadclient ile değiştirin.**

```
connector.exe -remote <myBlockchainEndpoint>  -method aadclient -client-id <myClientID> -client-secret "<myClientSecret>" -tenant-id <myAADDirectory>
```

| Parametre | Açıklama |
|-----------|-------------|
| kiracı kimliği | Azure AD etki alanı, Örneğin,`yourdomain.onmicrosoft.com`
| istemci kimliği | Azure AD'de kayıtlı uygulamanın istemci kimliği
| istemci-gizli | Azure AD'de kayıtlı uygulamanın istemci sırrı

Azure AD'de bir uygulamanın nasıl kaydedilebildiği hakkında daha fazla bilgi için [bkz: Kaynaklara erişebilen bir Azure REKLAM uygulaması ve hizmet ilkesi oluşturmak için portalı kullanın](../../active-directory/develop/howto-create-service-principal-portal.md)

### <a name="connect-a-mobile-device-or-text-browser"></a>Mobil cihaz veya metin tarayıcısı bağlama

Azure AD kimlik doğrulama açılır ekranının mümkün olmadığı bir mobil aygıt veya metin tabanlı tarayıcı için Azure AD tek seferlik bir parola oluşturur. Parolayı kopyalayabilir ve Azure AD kimlik doğrulaması yla başka bir ortamda devam edebilirsiniz.

Parolayı oluşturmak için **aadauthcode'u** **aaddevice**ile değiştirin. myAADDirectory'yi azure AD etki alanıyla değiştirin. \<\> Örneğin, `yourdomain.onmicrosoft.com`.

```
connector.exe -remote <myBlockchainEndpoint>  -method aaddevice -tenant-id <myAADDirectory>
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Blockchain Hizmeti'nde veri güvenliği hakkında daha fazla bilgi için [Azure Blockchain Service güvenliği'ne](data-security.md)bakın.