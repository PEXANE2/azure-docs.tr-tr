---
title: Bir SaaS uygulamasını kaydedin | Azure Marketi
description: Azure portalını kullanarak bir SaaS uygulamasının nasıl kaydedilebildiğini açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: dsindona
ms.openlocfilehash: 9c20fe34e108de95a34aabea56390e8a6f0d858f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275722"
---
# <a name="register-a-saas-application"></a>SaaS uygulaması kaydetme

Bu makalede, Microsoft [Azure portalını](https://portal.azure.com/)kullanarak bir SaaS uygulamasının nasıl kaydolunca kaydedilen açıklanmaktadır.  Başarılı bir kayıttan sonra, SaaS Karşılama API'lerine erişmek için kullanabileceğiniz bir Azure Etkin Dizin (Azure AD) güvenlik belirteci alırsınız.  Azure AD hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios)


## <a name="service-to-service-authentication-flow"></a>Servise hizmet kimlik doğrulama akışı

Aşağıdaki diyagram, yeni bir müşterinin abonelik akışını ve bu API'ler kullanıldığında şunları gösterir:

![SaaS API akışı sunar](./media/saas-offer-publish-api-flow-v1.png)

Azure, SaaS hizmetinin son kullanıcılarına maruz kattığı kimlik doğrulamasına herhangi bir kısıtlama getirmez. Ancak, SaaS Karşılama API'leri ile kimlik doğrulama, genellikle SaaS uygulamasını Azure portalı üzerinden kaydederek elde edilen bir Azure AD güvenlik belirteciyle gerçekleştirilir. 


## <a name="register-an-azure-ad-secured-app"></a>Azure AD tarafından güvenli bir uygulama kaydetme

Azure AD'nin özelliklerini kullanmak isteyen her uygulama önce bir Azure AD kiracısı olarak kaydedilmelidir. Bu kayıt işlemi, Azure AD'ye uygulamanızın bulunduğu URL, kullanıcının kimliği doğrulandıktan sonra yanıt gönderilecek URL, uygulamayı tanımlayan URI gibi ayrıntıları vermeyi içerir.  Azure portalını kullanarak yeni bir uygulama kaydetmek için aşağıdaki adımları gerçekleştirin:

1.  [Azure portalında](https://portal.azure.com/)oturum açın.
2.  Hesabınız birden fazla erişim sağlıyorsa, sağ üst köşedeki hesabınızı tıklatın ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
3.  Sol daki gezinti bölmesinde Azure **Active Directory** hizmetini tıklatın, **Uygulama kayıtlarını**tıklatın ve **Yeni uygulama kaydı'nı**tıklatın.

    ![SaaS AD Uygulama Kayıtları](./media/saas-offer-app-registration-v1.png)

4.  Oluştur sayfasında, başvurunuzun\'kayıt bilgilerini girin:
    -   **Ad**: Anlamlı bir uygulama adı girin
    -   **Uygulama türü**: 
        - Bir cihaza yerel olarak yüklenen [istemci uygulamaları](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#client-application) için **Yerel**'i seçin. Bu ayar OAuth ortak [yerel istemcileri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#native-client) için kullanılır.
        - Güvenli bir sunucuya yüklenen [istemci uygulamaları](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#client-application) ve [kaynak/API uygulamaları](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#resource-server) için Web uygulaması / **API'yi** seçin. Bu ayar, OAuth gizli [web istemcileri](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#web-client) ve ortak [kullanıcı aracısı tabanlı istemciler](https://docs.microsoft.com/azure/active-directory/develop/active-directory-dev-glossary#user-agent-based-client)için kullanılır.
        Aynı uygulama gerek bir istemciyi, gerekse kaynağı/API'yi sunabilir.
    -   **Oturum Açma URL'si**: Web uygulaması/API uygulamaları için uygulamanızın temel URL'sini sağlayın. Örneğin, **http://localhost:31544** yerel makinenizde çalışan bir web uygulamasının URL'si olabilir. Kullanıcılar daha sonra bir web istemcisi uygulamasında oturum açabilmek için bu URL'yi kullanır.
    -   **URI'yi Yeniden Yönlendirme**: Yerel uygulamalar için, belirteç yanıtlarını döndürmek için Azure AD tarafından kullanılan URI'yi sağlayın. Örneğin, **http://MyFirstAADApp**uygulamanıza özgü bir değer girin.

        ![SaaS AD Uygulama Kayıtları](./media/saas-offer-app-registration-v1-2.png)

        Web uygulamaları veya yerel uygulamalar için belirli örnekler için, [Azure AD Geliştiricileri Kılavuzu'nun](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) *Başlangıç Al* bölümünde bulunan hızlı başlangıç kılavuzlu kurulumlara göz atın.

5.  Tamamladığınızda **Oluştur**’a tıklayın. Azure AD, başvurunuz için benzersiz bir *Uygulama Kimliği* atar\'ve başvurunuzun ana kayıt sayfasına tekrar alınırsınız.\' Web uygulaması ya da yerel uygulama olmasına bağlı olarak uygulamanıza ek özellikler eklemek için değişik seçenekler sunulur.

>[!Note]
>Varsayılan olarak, yeni kaydedilmiş uygulama yalnızca aynı kiracıdan kullanıcıların uygulamanızda oturum açmasına izin verecek şekilde yapılandırılır.


## <a name="using-the-azure-ad-security-token"></a>Azure AD güvenlik belirteci kullanma

Uygulamanızı kaydettikten sonra, programlı bir şekilde Azure AD güvenlik belirteci isteyebilirsiniz.  Yayımcının bu belirteci kullanması ve çözümlemek için bir istekte bulunması beklenir.  Çeşitli Karşılama API'leri kullanırken, kullanıcı Azure'dan SaaS web sitesine yönlendirildiğinde belirteç sorgu parametresi URL'de yer almaktadır.  Bu belirteç yalnızca bir saat için geçerlidir.  Ayrıca, kullanmadan önce tarayıcıdan gelen belirteç değerini URL'de çözmeniz gerekir.

Bu belirteçler hakkında daha fazla bilgi için [Azure Etkin Dizin erişim belirteçleri'ne](https://docs.microsoft.com/azure/active-directory/develop/access-tokens)bakın.


### <a name="get-a-token-based-on-the-azure-ad-app"></a>Azure AD uygulamasına dayalı bir belirteç alın

HTTP Yöntemi

`POST`

*İstek URL'si*

**https://login.microsoftonline.com/*{tenantId}*/oauth2/token**

*URI parametresi*

|  **Parametre adı**  | **Gerekli**  | **Açıklama**                               |
|  ------------------  | ------------- | --------------------------------------------- |
| tenantId             | True          | Kayıtlı AAD başvurusunun kiracı kimliği   |
|  |  |  |


*İstek üst bilgisi*

|  **Üst bilgi adı**  | **Gerekli** |  **Açıklama**                                   |
|  --------------   | ------------ |  ------------------------------------------------- |
|  İçerik Türü     | True         | İstekle ilişkili içerik türü. Varsayılan değer: `application/x-www-form-urlencoded`.  |
|  |  |  |


*İstek gövdesi*

| **Özellik adı**   | **Gerekli** |  **Açıklama**                                                          |
| -----------------   | -----------  | ------------------------------------------------------------------------- |
|  Grant_type         | True         | Hibe türü. Varsayılan değer: `client_credentials`.                    |
|  Client_id          | True         |  Azure AD uygulamasıyla ilişkili istemci/uygulama tanımlayıcısı.                  |
|  client_secret      | True         |  Azure AD uygulamasıyla ilişkili parola.                               |
|  Kaynak           | True         |  Belirteç istenen hedef kaynak. Varsayılan değer: `62d94f6c-d599-489b-a797-3e10e42fbe22`. |
|  |  |  |


*Yanıt*

|  **Adı**  | **Tür**       |  **Açıklama**    |
| ---------- | -------------  | ------------------- |
| 200 TAMAM    | TokenResponse  | İstek başarılı oldu   |
|  |  |  |

*TokenResponse*

Örnek yanıt belirteci:

``` json
  {
      "token_type": "Bearer",
      "expires_in": "3600",
      "ext_expires_in": "0",
      "expires_on": "15251…",
      "not_before": "15251…",
      "resource": "62d94f6c-d599-489b-a797-3e10e42fbe22",
      "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayIsImtpZCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayJ9…"
  }               
```


## <a name="next-steps"></a>Sonraki adımlar

Azure AD ile güvenli uygulamanız artık [SaaS Karşılama API Sürüm 2'yi](./pc-saas-fulfillment-api-v2.md)kullanabilir.
