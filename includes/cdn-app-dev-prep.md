---
author: zhangmanling
ms.service: azure-cdn
ms.topic: include
ms.date: 11/21/2018
ms.author: mazha
ms.openlocfilehash: 41f2d4540f665137d34d262546cdc1a2edfbae3a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77608729"
---
## <a name="prerequisites"></a>Ön koşullar
CDN yönetim kodunu yazmadan önce, kodun Azure Kaynak Yöneticisi ile etkileşimkurmasını sağlamak için bazı hazırlıklar yapmanız gerekir. Bu hazırlığı yapmak için şunları yapmanız gerekir:

* Bu öğreticide oluşturulan CDN profilini içerecek bir kaynak grubu oluşturma
* Uygulama için kimlik doğrulaması sağlamak için Azure Active Directory'yi yapılandırın
* Yalnızca Azure AD kiracınızdan gelen yetkili kullanıcıların CDN profiliyle etkileşim kurabilmesi için kaynak grubuna izinler uygulayın

### <a name="creating-the-resource-group"></a>Kaynak grubunu oluşturma
1. [Azure Portalı'nda](https://portal.azure.com)oturum açın.
2. **Kaynak Oluştur'u**tıklatın.
3. Kaynak **grubu** ve Kaynak grubu bölmesinde arama yapın, **Oluştur'u**tıklatın.

    ![Yeni bir kaynak grubu oluşturma](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)
3. Kaynak grubunuzun adını *CdnConsoleTutorial.*  Aboneliğinizi seçin ve size yakın bir konum seçin.  İsterseniz, kaynak grubunu portaldaki **panoya** sabitlemek için pano onay kutusuna Pin'i tıklatabilirsiniz.  Sabitleme daha sonra bulmayı kolaylaştırır.  Seçimlerinizi yaptıktan sonra **Oluştur'u**tıklatın.

    ![Kaynak grubunu adlandırma](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)
4. Kaynak grubu oluşturulduktan sonra, panonuza sabitlemediyseniz, **Gözat'ı,** ardından Kaynak **Grupları'nı**tıklatarak bulabilirsiniz.  Açmak için kaynak grubunu tıklatın.  **Abonelik Kimliğinizi**not edin. Daha sonra lazım.

    ![Kaynak grubunu adlandırma](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>Azure AD uygulamasını oluşturma ve izinleri uygulama
Azure Active Directory ile uygulama kimlik doğrulaması için iki yaklaşım vardır: Tek tek kullanıcılar veya bir hizmet yöneticisi. Hizmet sorumlusu, Windows'taki bir hizmet hesabına benzer.  CDN profilleriyle etkileşimde kalmak için belirli bir kullanıcıya izin vermek yerine, izinler hizmet ilkesine verilir.  Hizmet ilkeleri genellikle otomatik, etkileşimli olmayan işlemler için kullanılır.  Bu öğretici etkileşimli bir konsol uygulaması yazıyor olsa da, hizmet temel yaklaşımına odaklanacağız.

Hizmet ilkesi oluşturmak, Azure Etkin Dizin uygulaması oluşturmak da dahil olmak üzere birkaç adımdan oluşur.  Oluşturmak için, bu öğretici [takip](../articles/active-directory/develop/howto-create-service-principal-portal.md)edeceğiz.

> [!IMPORTANT]
> [Bağlantılı öğreticideki](../articles/active-directory/develop/howto-create-service-principal-portal.md)tüm adımları izlediğinden emin olun.  Tam olarak açıklandığı gibi tamamlamak *önemlidir.*  **Kiracı kimliğinizi,** kiracı **etki alanı adınızı** (özel bir etki alanı belirtmediğiniz sürece genellikle *.onmicrosoft.com* etki alanı), **istemci kimliğinizi**ve **istemci kimlik doğrulama anahtarınızı**not aldığınızdan emin olun, çünkü bu bilgilere daha sonra ihtiyacımız var.  Bu kimlik bilgileri, hizmetleri asıl olarak işlemleri yürütmek için herkes tarafından kullanılabilir **gibi, istemci kimliği** ve **istemci kimlik doğrulama anahtarı**korumak için dikkatli olun.
>
> Çok kiracılı uygulamayı Yapılandırma adlı adıma girdiğinizde, **Hayır'ı**seçin.
>
> Adımı aldığınızda [uygulamayı bir role atayın,](../articles/active-directory/develop/howto-create-service-principal-portal.md#assign-a-role-to-the-application)daha önce oluşturulan kaynak grubunu kullanın, *CdnConsoleTutorial*, ancak **Reader** rolü yerine **CDN Profil Oluşturici** rolünü atayın.  Uygulamayı kaynak grubunuzdaki **CDN Profil Katılımcısı** rolünü atadıktan sonra bu öğreticiye geri dönün. 
>
>

Hizmet yöneticinizi oluşturduktan ve **CDN Profil Katılımcısı** rolünü atadıktan sonra, kaynak grubunuz için **Kullanıcılar** bıçağı aşağıdaki resme benzer olmalıdır.

![Kullanıcılar bıçak](./media/cdn-app-dev-prep/cdn-service-principal-include.png)

### <a name="interactive-user-authentication"></a>Etkileşimli kullanıcı kimlik doğrulaması
Bir hizmet sorumlusu yerine etkileşimli bireysel kullanıcı kimlik doğrulaması yapmak istiyorsanız, işlem bir hizmet sorumlusu için benzerdir.  Aslında, aynı yordamı takip etmek gerekir, ancak birkaç küçük değişiklikler yapmak.

> [!IMPORTANT]
> Yalnızca bir hizmet ilkesi yerine tek tek kullanıcı kimlik doğrulaması kullanmayı tercih ediyorsanız aşağıdaki sonraki adımları izleyin.
>
>

1. Uygulamanızı oluştururken, **Web Uygulaması**yerine **Yerel uygulamayı**seçin.

    ![Yerel uygulama](./media/cdn-app-dev-prep/cdn-native-application-include.png)
2. Bir sonraki sayfada, bir yeniden **yönlendirme URI**için istenir.  URI doğrulanmaz, ama ne girdiğinizi unutmayın. Daha sonra ihtiyacın olacak.
3. **İstemci kimlik doğrulama anahtarı**oluşturmaya gerek yoktur.
4. **CDN Profil Katılımcısı** rolüne bir hizmet müdürü atamak yerine, tek tek kullanıcıları veya grupları atayacağız.  Bu örnekte, *CDN Demo Kullanıcısını CDN* **Profil Katılımcısı** rolüne atadiğimı görebilirsiniz.  

    ![Bireysel kullanıcı erişimi](./media/cdn-app-dev-prep/cdn-aad-user-include.png)
