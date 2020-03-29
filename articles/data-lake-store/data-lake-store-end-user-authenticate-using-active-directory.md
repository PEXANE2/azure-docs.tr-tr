---
title: 'Son kullanıcı kimlik doğrulaması: Azure Active Directory ile Azure Veri Gölü Depolama Gen1 | Microsoft Dokümanlar'
description: Azure Etkin Dizini'ni kullanarak Azure Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması nasıl elde edilenöğrenin
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: 4c2b774c304e46f9fc68f3beaf64218e614ecad1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66234067"
---
# <a name="end-user-authentication-with-azure-data-lake-storage-gen1-using-azure-active-directory"></a>Azure Etkin Dizini kullanarak Azure Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması
> [!div class="op_single_selector"]
> * [Son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-using-active-directory.md)
> * [Hizmetten hizmete kimlik doğrulaması](data-lake-store-service-to-service-authenticate-using-active-directory.md)
> 
> 

Azure Veri Gölü Depolama Gen1 kimlik doğrulaması için Azure Etkin Dizini kullanır. Veri Gölü Depolama Gen1 veya Azure Veri Gölü Analitiği ile çalışan bir uygulama yazmadan önce, Azure Active Directory (Azure AD) ile uygulamanızın kimliğini nasıl doğruladığını belirlemeniz gerekir. Mevcut iki ana seçenek şunlardır:

* Son kullanıcı kimlik doğrulaması (bu makale)
* Servisten hizmete kimlik doğrulaması (yukarıdaki açılır yerden bu seçeneği seçin)

Bu seçeneklerin her ikisi de uygulamanızın Veri Gölü Depolama Gen1 veya Azure Veri Gölü Analizi'ne yapılan her isteğe iliştirilen bir OAuth 2.0 belirteci yle birlikte sunulmasıyla sonuçlanır.

Bu makalede, **son kullanıcı kimlik doğrulaması için**bir Azure AD yerel uygulamasının nasıl oluşturulacağı anlatılıyor. Hizmete kimlik doğrulama için Azure AD uygulama yapılandırması ile ilgili talimatlar için, [Azure Etkin Dizini'ni kullanarak Veri Gölü Depolama Gen1 ile hizmete hizmet kimlik doğrulaması'na](data-lake-store-authenticate-using-active-directory.md)bakın.

## <a name="prerequisites"></a>Ön koşullar
* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

* Abonelik kimliğiniz. Azure portalından alabilirsiniz. Örneğin, Veri Gölü Depolama Gen1 hesap bıçağından edinilebilir.
  
    ![Abonelik kimliği alma](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* Azure AD etki alanı adınız. Azure portalının sağ üst köşesinde fareyi gezdirerek geri alabilirsiniz. Aşağıdaki ekran görüntüsünden alan adı **contoso.onmicrosoft.com**ve parantez içindeki GUID kiracı kimliğidir. 
  
    ![AAD etki alanı alın](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

* Azure kiracı kimliğiniz. Kiracı kimliğini niçin alacağına ilişkin talimatlar için [bkz.](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in)

## <a name="end-user-authentication"></a>Son kullanıcı kimlik doğrulaması
Bu kimlik doğrulama mekanizması, bir son kullanıcının Azure AD aracılığıyla uygulamanızda oturum açmasını istiyorsanız önerilen yaklaşımdır. Daha sonra uygulamanız, oturum açan son kullanıcıyla aynı erişim düzeyine sahip Azure kaynaklarına erişebilir. Son kullanıcınızın uygulamanızın erişimi koruyabilmesi için kimlik bilgilerini düzenli aralıklarla sağlaması gerekir.

Son kullanıcı oturum açmanın sonucu, uygulamanız için bir erişim belirteci ve yenileme belirteci verilmesidir. Erişim belirteci, Data Lake Storage Gen1 veya Data Lake Analytics'e yapılan her isteğe eklenir ve varsayılan olarak bir saat boyunca geçerlidir. Yenileme belirteci, yeni bir erişim belirteci elde etmek için kullanılabilir ve varsayılan olarak iki haftaya kadar geçerlidir. Son kullanıcı oturum açma için iki farklı yaklaşım kullanabilirsiniz.

### <a name="using-the-oauth-20-pop-up"></a>OAuth 2.0 açılır penceresini kullanma
Başvurunuz, son kullanıcının kimlik bilgilerini girebileceği bir OAuth 2.0 yetkilendirme açılır penceresini tetikleyebilir. Bu açılır pencere, gerekirse Azure AD İki faktörlü Kimlik Doğrulama (2FA) işlemiyle de çalışır. 

> [!NOTE]
> Bu yöntem, Python veya Java için Azure AD Kimlik Doğrulama Kitaplığı'nda (ADAL) henüz desteklenmez.
> 
> 

### <a name="directly-passing-in-user-credentials"></a>Kullanıcı kimlik bilgilerinde doğrudan geçiş
Uygulamanız doğrudan Azure AD kullanıcı kimlik bilgilerini sağlayabilir. Bu yöntem yalnızca kuruluş kimliği kullanıcı hesaplarıile çalışır; veya @outlook.com . @live.com Ayrıca, bu yöntem Azure AD İki faktörlü Kimlik Doğrulaması (2FA) gerektiren kullanıcı hesaplarıyla uyumlu değildir.

### <a name="what-do-i-need-for-this-approach"></a>Bu yaklaşım için neye ihtiyacım var?
* Azure AD etki alanı adı. Bu gereksinim zaten bu makalenin ön koşulunda listelenir.
* Azure AD kiracı kimliği. Bu gereksinim zaten bu makalenin ön koşulunda listelenir.
* Azure AD **yerel uygulaması**
* Azure AD yerel uygulaması için başvuru kimliği
* URI'yi Azure AD yerel uygulaması için yeniden yönlendirme
* Yetki verilmiş izinleri ayarlayın


## <a name="step-1-create-an-active-directory-native-application"></a>Adım 1: Etkin Dizin yerel uygulama oluşturma

Azure Etkin Dizini'ni kullanarak Data Lake Storage Gen1 ile son kullanıcı kimlik doğrulaması için bir Azure AD yerel uygulaması oluşturun ve yapılandırır. Yönergeler için [bkz.](../active-directory/develop/howto-create-service-principal-portal.md)

Bağlantıdaki yönergeleri takip ederken, aşağıdaki ekran görüntüsünde gösterildiği gibi uygulama türü için **Yerel'i** seçtiğinizden emin olun:

![Web uygulaması oluşturma](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-create-native-app.png "Yerel uygulama oluşturma")

## <a name="step-2-get-application-id-and-redirect-uri"></a>Adım 2: Uygulama kimliği alın ve URI'yi yeniden yönlendirin

Bkz. Uygulama kimliğini almak için [uygulama kimliğini alın.](../active-directory/develop/howto-create-service-principal-portal.md#get-values-for-signing-in)

URI yönlendirmesini almak için aşağıdaki adımları yapın.

1. Azure portalından **Azure Active Directory'yi**seçin , **Uygulama kayıtlarını**tıklatın ve ardından oluşturduğunuz Azure AD yerel uygulamasını bulun ve tıklatın.

2. Uygulama için **Ayarlar** bıçağından, **Yönlendirme URI'lerini**tıklatın.

    ![URI'yi Yeniden Yönlendirme](./media/data-lake-store-end-user-authenticate-using-active-directory/azure-active-directory-redirect-uri.png)

3. Görüntülenen değeri kopyalayın.


## <a name="step-3-set-permissions"></a>Adım 3: İzinleri ayarlama

1. Azure portalından **Azure Active Directory'yi**seçin , **Uygulama kayıtlarını**tıklatın ve ardından oluşturduğunuz Azure AD yerel uygulamasını bulun ve tıklatın.

2. Uygulama için **Ayarlar** çubuğundan **Gerekli İzinler'i**tıklatın ve sonra **Ekle'yi**tıklatın.

    ![istemci kimliği](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

3. **API Erişim Ekle** çubuğunda **API Seç'i**tıklatın, Azure Veri **Gölü'nü**tıklatın ve sonra **Seç'i**tıklatın.

    ![istemci kimliği](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)
 
4.  **API Erişim Ekle** çubuğunda, **İzinleri Seç'i**tıklatın, **Veri Gölü Deposu'na tam erişim**sağlamak için onay kutusunu seçin ve ardından **Seç'i**tıklatın.

    ![istemci kimliği](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-3.png)

    **Bitti**’ye tıklayın.

5. **Windows Azure Hizmet Yönetimi API'si** için izin vermek için son iki adımı da yineleyin.
   
## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir Azure AD yerel uygulaması oluşturdunuz ve .NET SDK, Java SDK, REST API, vb. kullanarak yazarolduğunuz istemci uygulamalarınızda ihtiyacınız olan bilgileri topladınız. Şimdi, önce Veri Gölü Depolama Gen1 ile kimlik doğrulaması yapmak ve ardından mağazadaki diğer işlemleri gerçekleştirmek için Azure AD web uygulamasının nasıl kullanılacağı hakkında aşağıdaki makalelere geçebilirsiniz.

* [Java SDK kullanarak Data Lake Storage Gen1 ile son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-java-sdk.md)
* [.NET SDK kullanarak Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-net-sdk.md)
* [Python kullanarak Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-python.md)
* [REST API kullanarak Veri Gölü Depolama Gen1 ile son kullanıcı kimlik doğrulaması](data-lake-store-end-user-authenticate-rest-api.md)

