---
title: Yayımcı doğrulama sorunlarını giderme-Microsoft Identity platform | Mavisi
description: Microsoft Graph API 'Leri çağırarak Microsoft Identity platform için yayımcı doğrulaması sorunlarını nasıl giderebileceğinizi açıklar.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: troubleshooting
ms.workload: identity
ms.date: 05/08/2020
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: jesakowi
ms.openlocfilehash: c332b960caf7707953069c5252219ca6c51761a8
ms.sourcegitcommit: 3c66bfd9c36cd204c299ed43b67de0ec08a7b968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "90007562"
---
# <a name="troubleshoot-publisher-verification"></a>Yayımcı doğrulaması ile ilgili sorunları giderme
Bu işlemi tamamlayamadıysanız veya [yayımcı doğrulaması](publisher-verification-overview.md)ile ilgili beklenmeyen davranışlarla karşılaşıyorsanız, hata alıyorsanız veya beklenmeyen davranışı görüyorsanız aşağıdakileri yaparak başlatmanız gerekir: 

1. [Gereksinimleri](publisher-verification-overview.md#requirements) gözden geçirin ve tümünün karşılandıklarından emin olun.

1. [Uygulamayı yayımcı doğrulanmış olarak işaretlemek](mark-app-as-publisher-verified.md) ve tüm adımların başarıyla gerçekleştirildiğinden emin olmak için yönergeleri gözden geçirin.

1. [Yaygın sorunların](#common-issues)listesini gözden geçirin.

1. Ek bilgi toplamak ve Kullanıcı arabirimindeki sorunları gidermek için [grafik Gezginini](#making-microsoft-graph-api-calls) kullanarak isteği yeniden üretin.

## <a name="common-issues"></a>Genel Sorunlar
İşlem sırasında oluşabilecek bazı yaygın sorunlar aşağıda verilmiştir. 

- **Microsoft İş Ortağı Ağı kimliğimi (MPN ID) bilmiyorum veya hesap için birincil ilgili kişinin kim olduğunu bilmiyorum** 
    1. [MPN kayıt sayfasına](https://partner.microsoft.com/dashboard/account/v3/enrollment/joinnow/basicpartnernetwork/new) gidin
    1. Kuruluşunuzun birincil Azure AD kiracısında bir kullanıcı hesabıyla oturum açın 
    1. Bir MPN hesabı zaten mevcutsa, bu tanınacaktır ve hesaba eklenirsiniz 
    1. MPN KIMLIĞI ve birincil hesap kişisinin listeleneceği [iş ortağı profili sayfasına](https://partner.microsoft.com/pcv/accountsettings/connectedpartnerprofile) gidin

- **Azure AD Genel yöneticimin (Şirket Yöneticisi veya kiracı yöneticisi olarak da bilinir) kim olduğunu bilmiyorum, bunları nasıl bulabilirim? Uygulama Yöneticisi veya farklı bir yönetici rolü nedir?**
    1. Kuruluşunuzun birincil kiracısındaki bir kullanıcı hesabını kullanarak [Azure AD portalında](https://aad.portal.azure.com) oturum açın
    1. [Rol yönetimine](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RolesAndAdministrators) git
    1. "Genel yönetici" veya istenen yönetici rolü ' ne tıklayın
    1. Bu rolün atandığı kullanıcıların listesi görüntülenir

- **MPN hesabımın yöneticimin kim olduğunu bilmiyorum** [MPN Kullanıcı yönetimi sayfasına](https://partner.microsoft.com/pcv/users) gidin ve kullanıcıların çeşitli yönetici rollerde olduğunu görmek için kullanıcı listesine filtre uygulayın.

- **MPN KIMLIĞI 'nin geçersiz olduğunu veya erişim izni olmadığını söyleyen bir hata alıyorum.**
    1. [İş ortağı profilinize](https://partner.microsoft.com/pcv/accountsettings/connectedpartnerprofile) gidin ve şunları doğrulayın: 
        - MPN KIMLIĞI doğru. 
        - Bir hata veya "bekleyen eylemler" görüntülenir ve yasal iş profili ve Iş ortağı bilgileri altındaki doğrulama durumu "yetkili" veya "başarılı" deyin.
    1. [MPN Kiracı Yönetimi sayfasına](https://partner.microsoft.com/dashboard/account/v3/tenantmanagement) gidin ve uygulamanın kaydolmasını ve ' den bir kullanıcı hesabı ile imzalarınızın ilişkili kiracılar listesinde olduğunu doğrulayın.
    1. [MPN Kullanıcı yönetimi sayfasına](https://partner.microsoft.com/pcv/users) gidin ve oturum açmak için kullandığınız kullanıcının genel yönetici, MPN Yöneticisi veya hesaplar Yöneticisi olduğunu onaylayın.

- **Azure AD portalında oturum açarken hiçbir uygulamanın kayıtlı olmadığını görmüyorum. Kaydol?** 
    Uygulama kayıtlarınız farklı bir kullanıcı hesabı kullanılarak veya farklı bir kiracıda oluşturulmuş olabilir. Uygulama kayıtlarınızın oluşturulduğu kiracıda doğru hesapla oturum açtığınızdan emin olun.

- **Azure AD 'de bir uygulama kaydı sahibinin kim olduğunu Nasıl yaparım? mı?** 
    Uygulamanın kaydedildiği bir kiracıda oturum açıldığında, uygulama kayıtları dikey penceresine gidin, bir uygulamaya tıklayın ve sahipler ' e tıklayın.

## <a name="making-microsoft-graph-api-calls"></a>Microsoft Graph API çağrısı yapma 

Bir sorun yaşıyorsanız ancak kullanıcı arabiriminde neleri gördüğünüzü anlayacağınızı anlayamadığında, uygulama kayıt portalında gerçekleştirebileceğiniz işlemleri gerçekleştirmek için Microsoft Graph çağrılarını kullanarak daha fazla sorun giderme işlemi gerçekleştirmek yararlı olabilir.

Bu istekleri yapmanın en kolay yolu, [Graph Explorer](https://developer.microsoft.com/graph/graph-explorer)kullanmaktır. Ayrıca, [Postman](https://www.postman.com/)kullanma gibi diğer seçenekleri veya [bir Web Isteği çağırmak](/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-7)için PowerShell 'i kullanmayı da düşünebilirsiniz.  

Microsoft Graph, uygulamanızın doğrulanmış yayımcısını ayarlayıp kaldırmak ve bu işlemlerden birini gerçekleştirdikten sonra sonucu denetlemek için kullanabilirsiniz. Sonuç, uygulama kaydınızdan ve bu uygulamadan oluşturulan herhangi bir [hizmet sorumlusu](/graph/api/resources/serviceprincipal?view=graph-rest-beta) ile eşleşen [uygulama](/graph/api/resources/application?view=graph-rest-beta) nesnesi üzerinde görülebilir. Bu nesneler arasındaki ilişki hakkında daha fazla bilgi için bkz. [Azure Active Directory: uygulama ve hizmet sorumlusu nesneleri](app-objects-and-service-principals.md).  

Bazı yararlı isteklerin örnekleri aşağıda verilmiştir:  

### <a name="set-verified-publisher"></a>Doğrulanan yayımcıyı ayarla 

İstek

```
POST /applications/0cd04273-0d11-4e62-9eb3-5c3971a7cbec/setVerifiedPublisher 

{ 

    "verifiedPublisherId": "12345678" 

} 
```
 
Yanıt 
```
204 No Content 
```
> [!NOTE]
> *verifiedPublisherID* , MPN Kimliğiniz. 

### <a name="unset-verified-publisher"></a>Doğrulanan yayımcı ayarı 

İstek:  
```
POST /applications/0cd04273-0d11-4e62-9eb3-5c3971a7cbec/unsetVerifiedPublisher 
```
 
Yanıt 
```
204 No Content 
```
### <a name="get-verified-publisher-info-from-application"></a>Uygulamadan doğrulanmış Yayımcı bilgilerini al 
 
```
GET https://graph.microsoft.com/beta/applications/0cd04273-0d11-4e62-9eb3-5c3971a7cbec 

HTTP/1.1 200 OK 

{ 
    "id": "0cd04273-0d11-4e62-9eb3-5c3971a7cbec", 

    ... 

    "verifiedPublisher" : { 
        "displayName": "myexamplePublisher", 
        "verifiedPublisherId": "12345678", 
        "addedDateTime": "2019-12-10T00:00:00" 
    } 
} 
```

### <a name="get-verified-publisher-info-from-service-principal"></a>Hizmet sorumlusu 'ndan doğrulanmış Yayımcı bilgilerini al 
```
GET https://graph.microsoft.com/beta/servicePrincipals/010422a7-4d77-4f40-9335-b81ef5c23dd4 

HTTP/1.1 200 OK 

{ 
    "id": "010422a7-4d77-4f40-9335-b81ef5c22dd4", 

    ... 

    "verifiedPublisher" : { 
        "displayName": "myexamplePublisher", 
        "verifiedPublisherId": "12345678", 
        "addedDateTime": "2019-12-10T00:00:00" 
    } 
} 
```

## <a name="error-reference"></a>Hata başvurusu 

Aşağıda, Microsoft Graph ile sorun giderirken veya uygulama kayıt portalındaki işlemden ilerleyecekseniz alabileceğiniz olası hata kodlarının bir listesi verilmiştir.

### <a name="mpnaccountnotfoundornoaccess"></a>MPNAccountNotFoundOrNoAccess     

Verdiğiniz MPN KIMLIĞI <MPNID> yok veya ona erişiminiz yok. Geçerli bir MPN KIMLIĞI sağlayın ve yeniden deneyin. 

### <a name="mpnglobalaccountnotfound"></a>MPNGlobalAccountNotFound     

Verdiğiniz MPN KIMLIĞI <MPNID> geçerli değil. Geçerli bir MPN KIMLIĞI sağlayın ve yeniden deneyin. 

### <a name="mpnaccountinvalid"></a>Mpnaccountgeçersiz    

Verdiğiniz MPN KIMLIĞI <MPNID> geçerli değil. Geçerli bir MPN KIMLIĞI sağlayın ve yeniden deneyin. 

### <a name="mpnaccountnotvetted"></a>MPNAccountNotVetted  

Verdiğiniz MPN KIMLIĞI ( <MPNID> ), diting işlemini tamamlamamıştır. Iş Ortağı Merkezi 'nde bu işlemi tamamlayıp yeniden deneyin. 

### <a name="nopublisheridonassociatedmpnaccount"></a>NoPublisherIdOnAssociatedMPNAccount  

Verdiğiniz MPN KIMLIĞI <MPNID> geçerli değil. Geçerli bir MPN KIMLIĞI sağlayın ve yeniden deneyin. 

### <a name="mpniddoesnotmatchassociatedmpnaccount"></a>Mpnidyok Notmatchilişkili Mpnaccount    

Verdiğiniz MPN KIMLIĞI <MPNID> geçerli değil. Geçerli bir MPN KIMLIĞI sağlayın ve yeniden deneyin. 

### <a name="applicationnotfound"></a>ApplicationNotFound  

Hedef uygulama ( <AppId> ) bulunamıyor. Geçerli bir uygulama KIMLIĞI sağlayın ve yeniden deneyin. 

### <a name="b2ctenantnotallowed"></a>B2CTenantNotAllowed  

Bu yetenek Azure AD B2C kiracısında desteklenmez. 

### <a name="emailverifiedtenantnotallowed"></a>EmailVerifiedTenantNotAllowed    

Bu özellik, e-posta tarafından doğrulanan bir kiracıda desteklenmez. 

### <a name="nopublisherdomainonapplication"></a>NoPublisherDomainOnApplication   

Hedef uygulamanın ( <AppId> ) bir yayımcı etki alanı kümesi olması gerekir. Bir yayımcı etki alanı ayarlayıp yeniden deneyin. 

### <a name="publisherdomainisnotdnsverified"></a>Publisherdomainisnotdnsdoğrulandı  

Hedef uygulamanın yayımcı etki alanı ( <publisherDomain> ), bu kiracıda doğrulanmış bir etki alanı değil. DNS doğrulaması kullanarak bir kiracı etki alanını doğrulayın ve yeniden deneyin. 

### <a name="publisherdomainmismatch"></a>Publisherdomainuyuşmazlık  

Hedef uygulamanın yayımcı etki alanı ( <publisherDomain> ), Iş Ortağı Merkezi 'nde () e-posta doğrulaması gerçekleştirmek için kullanılan etki alanı ile eşleşmiyor <pcDomain> . Bu etki alanlarının eşleştiğinden emin olun ve yeniden deneyin. 

### <a name="notauthorizedtoverifypublisher"></a>NotAuthorizedToVerifyPublisher   

Uygulamada () doğrulanmış yayımcı özelliğini ayarlama yetkiniz yok <AppId> 

### <a name="mpnidwasnotprovided"></a>Mpnıdınotsaðlanan  

MPN KIMLIĞI istek gövdesinde sağlanmadı veya istek içerik türü "Application/JSON" değildi. 

### <a name="msanotsupported"></a>MSANotSupported  

Bu özellik Microsoft tüketici hesaplarında desteklenmez. Yalnızca Azure AD kullanıcısı tarafından Azure AD 'de kayıtlı olan uygulamalar desteklenir. 

## <a name="next-steps"></a>Sonraki adımlar

Önceki bilgilerin tümünü gözden geçirdiğinizi ve Microsoft Graph hala bir hata alıyorsanız, başarısız istekle ilgili olarak aşağıdaki bilgilerin çoğunu toplayın ve [Microsoft desteği 'ne başvurun](developer-support-help-options.md#open-a-support-request).

- Zaman damgası 
- CorrelationId 
- Oturum açmış kullanıcının ObjectID veya UserPrincipalName 
- Hedef uygulama ObjectID
- Hedef uygulamanın uygulama kimliği
- Uygulamanın kaydedildiği bir Tenantıd
- MPN Kimliği
- REST isteği yapıldı 
- Döndürülen hata kodu ve ileti 
