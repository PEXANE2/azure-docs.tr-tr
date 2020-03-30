---
author: robinsh
ms.author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: 4eb794fa35164e3f86a5e3d6f67d446321f91f0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67133624"
---
## <a name="prepare-to-authenticate-azure-resource-manager-requests"></a>Azure Kaynak Yöneticisi isteklerini doğrulamak için hazırlanın
Azure Etkin Dizin (AD) ile Azure [Kaynak Yöneticisi'ni][lnk-authenticate-arm] kullanarak kaynaklarda gerçekleştirdiğiniz tüm işlemlerin kimliğini doğrulamanız gerekir. Bunu yapılandırmanın en kolay yolu PowerShell veya Azure CLI kullanmaktır.

Devam etmeden önce [Azure PowerShell cmdlet'lerini][lnk-powershell-install] yükleyin.

Aşağıdaki adımlar, PowerShell'i kullanarak bir AD uygulaması için parola kimlik doğrulamasını nasıl ayarlayacağını gösterir. Bu komutları standart bir PowerShell oturumunda çalıştırabilirsiniz.

1. Aşağıdaki komutu kullanarak Azure aboneliğinizde oturum açın:

    ```powershell
    Connect-AzAccount
    ```

1. Birden fazla Azure aboneliğiniz varsa, Azure'da oturum açmak kimlik bilgilerinizle ilişkili tüm Azure aboneliklerine erişmenizi tanır. Kullanabileceğiniz Azure aboneliklerini listelemek için aşağıdaki komutu kullanın:

    ```powershell
    Get-AzSubscription
    ```

    IoT hub'ınızı yönetmek için komutları çalıştırmak için kullanmak istediğiniz aboneliği seçmek için aşağıdaki komutu kullanın. Önceki komutun çıkışında yer alan abonelik adını veya kimliği kullanabilirsiniz:

    ```powershell
    Select-AzSubscription `
        -SubscriptionName "{your subscription name}"
    ```

2. TenantId ve **SubscriptionId'inizi** not edin. **SubscriptionId** Bunlara daha sonra ihtiyacınız olacak.
3. Yer sahiplerinin yerini alan aşağıdaki komutu kullanarak yeni bir Azure Etkin Dizin uygulaması oluşturun:
   
   * **{Ekran adı}:** **MySampleApp** gibi uygulamanız için bir ekran adı
   * **{Giriş sayfası URL}:** http: **\//mysampleapp/home**gibi uygulamanızın ana sayfasının URL'si. Bu URL'nin gerçek bir uygulamayı işaret etmesi gerekmez.
   * **{Uygulama tanımlayıcısı}:** **Http:\//mysampleapp**gibi benzersiz bir tanımlayıcı . Bu URL'nin gerçek bir uygulamayı işaret etmesi gerekmez.
   * **{Şifre}:** Uygulamanızla kimlik doğrulaması yapmak için kullandığınız parola.
     
     ```powershell
     $SecurePassword=ConvertTo-SecureString {password} –asplaintext –force
     New-AzADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password $SecurePassword
     ```
4. Oluşturduğunuz uygulamanın **ApplicationId'ine** not edin. Buna daha sonra ihtiyacın olacak.
5. Aşağıdaki komutu kullanarak bir önceki adımdaki **ApplicationId** **ile {MyApplicationId}** yerine yeni bir hizmet sorumlusu oluşturun:
   
    ```powershell
    New-AzADServicePrincipal -ApplicationId {MyApplicationId}
    ```
6. Aşağıdaki komutu kullanarak bir rol ataması ayarlayın ve **{MyApplicationId}** ile **ApplicationId'iniz**yerine geçin.
   
    ```powershell
    New-AzRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```

Özel C# uygulamanızdan kimlik doğrulamanızı sağlayan Azure AD uygulamasını oluşturmayı tamamladınız. Bu öğreticinin ilerleyen saatlerinde aşağıdaki değerlere ihtiyacınız var:

* TenantId
* SubscriptionId
* ApplicationID
* Parola

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: /powershell/azure/install-az-ps
