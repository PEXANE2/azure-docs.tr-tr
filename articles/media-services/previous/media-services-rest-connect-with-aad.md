---
title: REST ile Azure Medya Hizmetleri API'sine erişmek için Azure AD kimlik doğrulamasını kullanın | Microsoft Dokümanlar
description: REST'i kullanarak Azure Etkin Dizin kimlik doğrulaması ile Azure Media Services API'sine nasıl erişeceklerini öğrenin.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.reviewer: willzhan; johndeu
ms.openlocfilehash: a693eb374365670da3fe8c4b2bb8ce664a024217
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295436"
---
# <a name="use-azure-ad-authentication-to-access-the-media-services-api-with-rest"></a>REST ile Media Services API'sine erişmek için Azure AD kimlik doğrulamasını kullanma

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Azure Media Hizmetleri ile Azure AD kimlik doğrulamasını kullanırken, kimlik doğrulaması iki şekilde yapabilirsiniz:

- **Kullanıcı kimlik doğrulaması,** uygulamayı kullanan bir kişinin Azure Medya Hizmetleri kaynaklarıyla etkileşimde bulunan bir kişinin kimliğini doğrular. Etkileşimli uygulama öncelikle kullanıcıdan kimlik bilgileri için istekte olmalıdır. Buna örnek olarak, kodlama işlerini veya canlı akışı izlemek için yetkili kullanıcılar tarafından kullanılan bir yönetim konsolu uygulaması dır. 
- **Hizmet temel kimlik doğrulaması** bir hizmeti doğrular. Bu kimlik doğrulama yöntemini yaygın olarak kullanan uygulamalar, daemon hizmetlerini, orta katman hizmetlerini veya web uygulamaları, işlev uygulamaları, mantık uygulamaları, API'ler veya mikro hizmetler gibi zamanlanmış işleri çalıştıran uygulamalardır.

    Bu öğretici, REST ile AMS API'sine erişmek için Azure AD **hizmeti asıl** kimlik doğrulamasını nasıl kullanacağınızı gösterir. 

    > [!NOTE]
    > **Hizmet ilkesi,** Azure Medya Hizmetleri'ne bağlanan çoğu uygulama için önerilen en iyi uygulamadır. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure portalından kimlik doğrulama bilgilerini alma
> * Postacı'yı kullanarak erişim jetonuna erişin
> * Erişim belirteci kullanarak **Varlıklar** API'sini test etme


> [!IMPORTANT]
> Medya Hizmetleri şu anda Azure Erişim Denetimi hizmetleri kimlik doğrulama modelini destekler. Ancak, Access Control kimlik doğrulaması 1 Haziran 2018'de amortismana kalınacaktır. Azure AD kimlik doğrulaması modeline mümkün olan en kısa sürede geçiş yapmanız önerilir.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) bir hesap oluşturun.
- [Azure portalını kullanarak bir Azure Medya Hizmetleri hesabı oluşturun.](media-services-portal-create-account.md)
- Azure [AD kimlik doğrulamagenel bakış makalesiyle Erişim Azure Medya Hizmetleri API'sini](media-services-use-aad-auth-to-access-ams-api.md) gözden geçirin.
- Bu makalede gösterilen REST API'lerini çalıştırmak için [Postacı](https://www.getpostman.com/) REST istemcisini yükleyin. 

    Bu eğitimde, **postacı** kullanıyoruz ama herhangi bir REST aracı uygun olacaktır. Diğer seçenekler şunlardır: REST eklentili **Visual Studio Code** veya **Telerik Fiddler**. 

## <a name="get-the-authentication-information-from-the-azure-portal"></a>Azure portalından kimlik doğrulama bilgilerini alma

### <a name="overview"></a>Genel Bakış

Medya Hizmetleri API'sine erişmek için aşağıdaki veri noktalarını toplamanız gerekir.

|Ayar|Örnek|Açıklama|
|---|-------|-----|
|Azure Active Directory kiracı etki alanı|microsoft.onmicrosoft.com|Güvenli Belirteç Hizmeti (STS) uç noktası olarak Azure <https://login.microsoftonline.com/{your-ad-tenant-name.onmicrosoft.com}/oauth2/token>AD aşağıdaki biçim kullanılarak oluşturulur: . Azure AD, kaynaklara (erişim belirteci) erişmek için bir JWT sorunu çıkarır.|
|REST API uç noktası|<https://amshelloworld.restv2.westus.media.azure.net/api/>|Bu, başvurunuzdaki tüm Medya Hizmetleri REST API çağrılarının yapıldığı noktadır.|
|İstemci Kimliği (Başvuru Kimliği)|f7fbbb29-a02d-4d91-bbc6-59a2579259d2|Azure AD uygulaması (istemci) kimliği. Erişim jetonunu almak için istemci kimliği gereklidir. |
|İstemci Gizli Anahtarı|+mUERiNzVMoJGggD6aV1etzFGa1n6KeSlLjIq+Dbim0=|Azure AD uygulama anahtarları (istemci sırrı). Erişim jetonunu almak için istemci sırrı gereklidir.|

### <a name="get-aad-auth-info-from-the-azure-portal"></a>Azure portalından AAD auth bilgilerini alın

Bilgi almak için aşağıdaki adımları izleyin:

1. [Azure portalına](https://portal.azure.com)giriş yapın.
2. AMS örneğinize gidin.
3. **API erişimini**seçin.
4. Hizmet **sorumlusuyla Azure Medya Hizmetleri API'sine Bağlan'ı**tıklatın.

    ![API erişimi](./media/connect-with-rest/connect-with-rest01.png)

5. Varolan bir **Azure AD uygulamasını** seçin veya yeni bir uygulama oluşturun (aşağıda gösterilmiştir).

    > [!NOTE]
    > Azure Media REST isteğinin başarılı olabilmesi için, arayan kullanıcının erişmeye çalıştığı Medya Hizmetleri hesabı için Bir **Katılımcı** veya **Sahip** rolü olması gerekir. "Uzak sunucu bir hata döndürür: (401) Yetkisiz" yazan bir özel durum alırsanız, [bkz.](media-services-use-aad-auth-to-access-ams-api.md#access-control)

    Yeni bir AD uygulaması oluşturmanız gerekiyorsa, aşağıdaki adımları izleyin:
    
   1. **Yeni Oluştur**tuşuna basın.
   2. Bir ad girin.
   3. **Yeniden Oluştur Yeni'ye** basın.
   4. **Kaydet**’e basın.

      ![API erişimi](./media/connect-with-rest/new-app.png)

      Yeni uygulama sayfada beliriyor.

6. **İstemci kimliğini** (Başvuru Kimliği) alın.
    
   1. Uygulamayı seçin.
   2. **Sağdaki** pencereden Müşteri Kimliğini alın. 

      ![API erişimi](./media/connect-with-rest/existing-client-id.png)

7. Uygulamanın **Anahtarını** (istemci sırrı) alın. 

   1. Uygulamayı **Yönet** düğmesini tıklatın (Müşteri Kimliği bilgisinin **Uygulama Kimliği**altında olduğuna dikkat edin). 
   2. **Tuşlara**basın.
    
       ![API erişimi](./media/connect-with-rest/manage-app.png)
   3. **DESCRIPTION** ve **EXPIRES'i** doldurarak ve **Kaydet**tuşuna basarak uygulama anahtarını (istemci sırrı) oluşturun.
    
       **Kaydet** düğmesine basıldıktan sonra tuş değeri görüntülenir. Bıçaktan çıkmadan önce anahtar değerini kopyalayın.

   ![API erişimi](./media/connect-with-rest/connect-with-rest03.png)

Ad bağlantı parametreleri için değerler ekleyebilir, web.config veya app.config dosyanıza daha sonra kodunuzda kullanmak için.

> [!IMPORTANT]
> **İstemci anahtarı** önemli bir sırdır ve anahtar kasasında düzgün bir şekilde emniyete alınmalıdır veya üretimde şifrelenmelidir.

## <a name="get-the-access-token-using-postman"></a>Postacı'yı kullanarak erişim jetonuna erişin

Bu bölümde, Bir JWT Taşıyıcı Belirteci (erişim belirteci) döndüren bir REST API yürütmek için **Postacı** nasıl kullanılacağını gösterir. Herhangi bir Medya Hizmetleri REST API'yi aramak için çağrılara "Yetkilendirme" üstbilgisini eklemeniz ve her çağrıya "Taşıyıcı *your_access_token"* değerini eklemeniz gerekir (bu öğreticinin bir sonraki bölümünde gösterildiği gibi). 

1. Açık **Postacı**.
2. **POST**'u seçin.
3. Aşağıdaki biçimi kullanarak kiracı adınızı içeren URL'yi girin: kiracı adı **.onmicrosoft.com** ile sona ermeli ve URL **oauth2/token**ile bitmelidir: 

    `https://login.microsoftonline.com/{your-aad-tenant-name.onmicrosoft.com}/oauth2/token`

4. **Üstbilgi** sekmesini seçin.
5. "Anahtar/Değer" veri ızgarasını kullanarak **Üstbilgiler** bilgilerini girin. 

    ![Veri Kılavuzu](./media/connect-with-rest/headers-data-grid.png)

    Alternatif olarak, Postacı penceresinin sağındaki **Toplu Edit** bağlantısını tıklatın ve aşağıdaki kodu yapıştırın.

        Content-Type:application/x-www-form-urlencoded
        Keep-Alive:true

6. **Vücut** sekmesine basın.
7. "Anahtar/Değer" veri ızgarasını kullanarak gövde bilgilerini girin (istemci kimliğini ve gizli değerleri değiştirin). 

    ![Veri Kılavuzu](./media/connect-with-rest/data-grid.png)

    Alternatif olarak, Postacı penceresinin **sağındaki Toplu Edit'i** tıklatın ve aşağıdaki gövdeyi yapıştırın (istemci kimliğini ve gizli değerleri değiştirin):

        grant_type:client_credentials
        client_id:{Your Client ID that you got from your Azure AD Application}
        client_secret:{Your client secret that you got from your Azure AD Application's Keys}
        resource:https://rest.media.azure.net

8. **Gönder**’e basın.

    ![belirteç olsun](./media/connect-with-rest/connect-with-rest04.png)

Döndürülen yanıt, ams API'lerine erişmek için kullanmanız gereken **erişim belirteci** içerir.

## <a name="test-the-assets-api-using-the-access-token"></a>Erişim belirteci kullanarak **Varlıklar** API'sini test etme

Bu bölümde **Postacı**kullanarak **Varlıklar** API'sine nasıl erişilir gösterilmektedir.

1. Açık **Postacı**.
2. **GET**'i seçin.
3. REST API bitiş noktasını yapıştırın (örneğin,https://amshelloworld.restv2.westus.media.azure.net/api/Assets)
4. **Yetkilendirme** sekmesini seçin. 
5. **Taşıyıcı Belirteci'ni**seçin.
6. Önceki bölümde oluşturulan belirteci yapıştırın.

    ![belirteç olsun](./media/connect-with-rest/connect-with-rest05.png)

    > [!NOTE]
    > Postacı UX bir Mac ve PC arasında farklı olabilir. Mac sürümünde **Kimlik Doğrulama** bölümünde "Taşıyıcı Belirteci" seçeneği yoksa, **Yetki üstbilgisini** Mac istemcisine el ile eklemeniz gerekir.

   ![Auth üstbilgi](./media/connect-with-rest/auth-header.png)

7. **Üstbilgi**seçin.
5. Postacı penceresinin sağdaki **Toplu Edit** bağlantısını tıklatın.
6. Aşağıdaki üstbilgi yapıştırın:

        x-ms-version:2.19
        Accept:application/json
        Content-Type:application/json
        DataServiceVersion:3.0
        MaxDataServiceVersion:3.0

7. **Gönder**’e basın.

İade edilen yanıt, hesabınızdaki varlıkları içerir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Medya Hizmetleri Erişimi için Azure AD Kimlik Doğrulaması'nda bu örnek kodu [deneyin: Her ikisi de REST API ile](https://github.com/willzhan/WAMSRESTSoln)
* [.NET ile dosya yükleme](media-services-dotnet-upload-files.md)
