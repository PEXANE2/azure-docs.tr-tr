---
title: Azure Active Directory kimlik doğrulamasıyla Azure Media Services API 'sine erişme | Microsoft Docs
description: Azure Media Services API 'sine erişim için kimlik doğrulaması yapmak üzere Azure Active Directory (Azure AD) kullanmak için gereken kavramlar ve adımlar hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: 8e1aeaf105ce371e965b433ac78e2b257f4bc18b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81682040"
---
# <a name="access-the-azure-media-services-api-with-azure-ad-authentication"></a>Azure AD kimlik doğrulaması ile Azure Media Services API’sine erişme  

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>[V3 Media Services](https://docs.microsoft.com/azure/media-services/latest/)en son sürüme göz atın. Ayrıca bkz. [v2 'den v3 'e geçiş kılavuzu](../latest/migrate-from-v2-to-v3.md)

Azure Media Services API 'si, yeniden bir API 'dir. Bunu, REST API kullanarak veya kullanılabilir istemci SDK 'larını kullanarak medya kaynakları üzerinde işlem gerçekleştirmek için kullanabilirsiniz. Azure Media Services Microsoft .NET için Media Services istemci SDK 'Sı sunmaktadır. Media Services kaynaklarına ve Media Services API 'sine erişme yetkisi sağlamak için, önce kimliğiniz doğrulanmalıdır. 

Media Services, [Azure Active Directory (Azure AD) tabanlı kimlik doğrulamasını](../../active-directory/fundamentals/active-directory-whatis.md)destekler. Azure Media REST hizmeti, REST API isteklerini yapan kullanıcı veya uygulamanın kaynaklara erişmek için **katkıda** bulunan veya **sahip** rolü olmasını gerektirir. Daha fazla bilgi için, bkz. [Azure Portal rol tabanlı Access Control kullanmaya başlama](../../role-based-access-control/overview.md).  

Bu belge, REST veya .NET API 'Lerini kullanarak Media Services API 'sine erişme hakkında genel bakış sunar.

> [!NOTE]
> Access Control yetkilendirmesi 1 Haziran 2018 ' de kullanımdan kaldırılmıştır.

## <a name="access-control"></a>Erişim denetimi

Azure Medya REST isteğinin başarılı olması için, çağıran kullanıcının erişmeye çalıştığı Media Services hesap için katkıda bulunan veya sahip rolü olmalıdır.  
Yalnızca sahip rolüne sahip bir Kullanıcı, yeni kullanıcılara veya uygulamalara medya kaynağı (hesap) erişimi verebilir. Katkıda bulunan rolü yalnızca medya kaynağına erişebilir.
Durum kodu 401 ile yetkisiz istekler başarısız olur. Bu hata kodunu görürseniz, Kullanıcı Media Services hesabı için kullanıcının katkıda bulunan veya sahip rolüne sahip olup olmadığını denetleyin. Azure portal bunu kontrol edebilirsiniz. Medya hesabınızı arayın ve ardından **erişim denetimi** sekmesine tıklayın. 

![Erişim denetimi sekmesi](./media/media-services-use-aad-auth-to-access-ams-api/media-services-access-control.png)

## <a name="types-of-authentication"></a>Kimlik doğrulama türleri 
 
Azure Media Services ile Azure AD kimlik doğrulaması kullandığınızda, iki kimlik doğrulama seçeneğiniz vardır:

- **Kullanıcı kimlik doğrulaması**. Media Services kaynaklarla etkileşim kurmak için uygulamayı kullanan bir kişinin kimliğini doğrulayın. Etkileşimli uygulama öncelikle kullanıcıdan kullanıcının kimlik bilgilerini istemelidir. Bu örnek, yetkili kullanıcılar tarafından kodlama işlerini veya canlı akışı izlemek için kullanılan bir yönetim konsolu uygulamasıdır. 
- **Hizmet sorumlusu kimlik doğrulaması**. Bir hizmetin kimliğini doğrulayın. Bu kimlik doğrulama yöntemini yaygın olarak kullanan uygulamalar, Daemon Hizmetleri, orta katman hizmetleri veya zamanlanmış işleri çalıştıran uygulamalardır. Örnek olarak Web Apps, işlev uygulamaları, Logic Apps, API ve mikro hizmetlerdir.

### <a name="user-authentication"></a>Kullanıcı kimlik doğrulaması 

Kullanıcı kimlik doğrulaması yöntemini kullanması gereken uygulamalar, yerel uygulamalar, Windows Uygulamaları ve konsol uygulamaları için yönetim veya izleme. Bu tür bir çözüm, aşağıdaki senaryolardan birinde hizmetle insan etkileşimi istediğinizde yararlıdır:

- Kodlama işleriniz için panoyu izleme.
- Canlı akışlarınız için panoyu izleme.
- Bir Media Services hesabındaki kaynakları yönetmek için masaüstü veya mobil kullanıcıların yönetim uygulaması.

> [!NOTE]
> Bu kimlik doğrulama yöntemi, tüketiciye yönelik uygulamalar için kullanılmamalıdır. 

Yerel bir uygulama, önce Azure AD 'den bir erişim belirteci almalıdır, ardından Media Services REST API HTTP istekleri yaptığınızda bunu kullanır. Erişim belirtecini istek üstbilgisine ekleyin. 

Aşağıdaki diyagramda tipik bir etkileşimli uygulama kimlik doğrulama akışı gösterilmektedir: 

![Yerel uygulamalar diyagramı](./media/media-services-use-aad-auth-to-access-ams-api/media-services-native-aad-app1.png)

Yukarıdaki diyagramda, sayılar isteklerin kronolojik sırada akışını temsil eder.

> [!NOTE]
> Kullanıcı kimlik doğrulama yöntemini kullandığınızda tüm uygulamalar aynı (varsayılan) yerel uygulama istemci KIMLIĞINI ve yerel uygulama yeniden yönlendirme URI 'sini paylaşır. 

1. Kullanıcıdan kimlik bilgilerini ister.
2. Şu parametrelerle bir Azure AD erişim belirteci isteyin:  

   * Azure AD kiracı uç noktası.

       Kiracı bilgileri Azure portal elde edilebilir. İmlecinizi sağ üst köşedeki oturum açmış kullanıcının adının üzerine getirin.
   * Kaynak URI 'sini Media Services. 

       Bu URI aynı Azure ortamındaki Media Services hesapları için aynıdır (örneğin, https:\//Rest.Media.Azure.net).

   * Media Services (yerel) uygulama istemci KIMLIĞI.
   * Media Services (yerel) uygulama yeniden yönlendirme URI 'SI.
   * REST Media Services için kaynak URI 'SI.
        
       URI REST API uç noktasını temsil eder (örneğin, https://test03.restv2.westus.media.azure.net/api/).

     Bu parametrelerin değerlerini almak için, bkz. Kullanıcı kimlik doğrulama seçeneğini kullanarak [Azure AD kimlik doğrulaması ayarlarına erişmek için Azure Portal kullanma](media-services-portal-get-started-with-aad.md) .

3. Azure AD erişim belirteci istemciye gönderilir.
4. İstemci Azure AD erişim belirteci ile Azure Media REST API bir istek gönderir.
5. İstemci Media Services verileri geri alır.

Media Services .NET istemci SDK 'sını kullanarak REST istekleriyle iletişim kurmak için Azure AD kimlik doğrulamasını kullanma hakkında daha fazla bilgi için bkz. [.NET ile MEDIA SERVICES API 'sine erişmek Için Azure AD kimlik doğrulamasını kullanma](media-services-dotnet-get-started-with-aad.md). 

.NET istemci SDK Media Services kullanmıyorsanız, adım 2 ' de açıklanan parametreleri kullanarak el ile bir Azure AD erişim belirteci isteği oluşturmanız gerekir. Daha fazla bilgi için bkz. Azure AD [kimlik doğrulama kitaplığı 'nı kullanarak Azure AD belirtecini alma](../../active-directory/azuread-dev/active-directory-authentication-libraries.md).

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Bu kimlik doğrulama yöntemini yaygın olarak kullanan uygulamalar, orta katman hizmetleri ve zamanlanan işleri çalıştıran uygulamalardır: Web Apps, işlev uygulamaları, Logic Apps, API 'Ler ve mikro hizmetler. Bu kimlik doğrulama yöntemi, kaynakları yönetmek için bir hizmet hesabı kullanmak isteyebileceğiniz etkileşimli uygulamalar için de uygundur.

Tüketici senaryoları oluşturmak için hizmet sorumlusu kimlik doğrulama yöntemini kullandığınızda, kimlik doğrulama genellikle orta katmanda (bazı API 'leri aracılığıyla) işlenir ve doğrudan bir mobil veya masaüstü uygulamasında değildir. 

Bu yöntemi kullanmak için kendi kiracısında bir Azure AD uygulaması ve hizmet sorumlusu oluşturun. Uygulamayı oluşturduktan sonra, uygulamaya katkıda bulunan veya sahip rolü Media Services hesaba erişim izni verin. Bunu, Azure CLı kullanarak veya bir PowerShell betiği ile Azure portal yapabilirsiniz. Ayrıca mevcut bir Azure AD uygulamasını da kullanabilirsiniz. Azure AD uygulamanızı ve hizmet sorumlunuzu [Azure Portal](media-services-portal-get-started-with-aad.md)kaydedebilir ve yönetebilirsiniz. Bunu [Azure CLI](media-services-use-aad-auth-to-access-ams-api.md) veya [PowerShell](media-services-powershell-create-and-configure-aad-app.md)kullanarak da yapabilirsiniz. 

![Orta katman uygulamalar](./media/media-services-use-aad-auth-to-access-ams-api/media-services-principal-service-aad-app1.png)

Azure AD uygulamanızı oluşturduktan sonra aşağıdaki ayarların değerlerini alırsınız. Kimlik doğrulaması için bu değerlere ihtiyacınız vardır:

- İstemci Kimliği 
- Gizli anahtar 

Yukarıdaki şekilde, sayılar isteklerin akışını kronolojik sırada temsil eder:
    
1. Bir orta katman uygulaması (Web API 'SI veya Web uygulaması) aşağıdaki parametrelere sahip bir Azure AD erişim belirteci ister:  

   * Azure AD kiracı uç noktası.

       Kiracı bilgileri Azure portal elde edilebilir. İmlecinizi sağ üst köşedeki oturum açmış kullanıcının adının üzerine getirin.
   * Kaynak URI 'sini Media Services. 

       Bu URI aynı Azure ortamında bulunan Media Services hesapları için aynıdır (örneğin, https:\//Rest.Media.Azure.net).

   * REST Media Services için kaynak URI 'SI.

       URI REST API uç noktasını temsil eder (örneğin, https://test03.restv2.westus.media.azure.net/api/).

   * Azure AD uygulama değerleri: istemci KIMLIĞI ve istemci parolası.
    
     Bu parametrelerin değerlerini almak için bkz. hizmet sorumlusu kimlik doğrulaması seçeneğini kullanarak [Azure AD kimlik doğrulaması ayarlarına erişmek için Azure Portal kullanma](media-services-portal-get-started-with-aad.md) .

2. Azure AD erişim belirteci Orta katmana gönderilir.
4. Orta katman Azure AD belirteci ile Azure Media REST API isteği gönderir.
5. Orta katman Media Services verileri geri alır.

Media Services .NET istemci SDK 'sını kullanarak REST istekleriyle iletişim kurmak için Azure AD kimlik doğrulamasını kullanma hakkında daha fazla bilgi için bkz. [.NET ile Azure MEDIA SERVICES API 'sine erişmek Için Azure AD kimlik doğrulamasını kullanma](media-services-dotnet-get-started-with-aad.md). 

Media Services .NET istemci SDK 'sını kullanmıyorsanız, adım 1 ' de açıklanan parametreleri kullanarak el ile bir Azure AD belirteci isteği oluşturmanız gerekir. Daha fazla bilgi için bkz. Azure AD [kimlik doğrulama kitaplığı 'nı kullanarak Azure AD belirtecini alma](../../active-directory/azuread-dev/active-directory-authentication-libraries.md).

## <a name="troubleshooting"></a>Sorun giderme

Özel durum: "uzak sunucu bir hata döndürdü: (401) yetkilendirilmemiş."

Çözüm: Media Services REST isteğinin başarılı olması Için, çağıran kullanıcının erişmeye çalıştığı Media Services hesapta katkıda bulunan veya sahip rolü olması gerekir. Daha fazla bilgi için [erişim denetimi](media-services-use-aad-auth-to-access-ams-api.md#access-control) bölümüne bakın.

## <a name="resources"></a>Kaynaklar

Aşağıdaki makaleler, Azure AD kimlik doğrulama kavramları hakkında genel bakışlardır: 

- [Azure AD tarafından belirtilen kimlik doğrulama senaryoları](../../active-directory/develop/authentication-scenarios.md)
- [Azure AD 'de uygulama ekleme, güncelleştirme veya kaldırma](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md)
- [PowerShell kullanarak rol tabanlı Access Control yapılandırma ve yönetme](../../role-based-access-control/role-assignments-powershell.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Media Services API 'sini kullanmak Için Azure AD kimlik doğrulamasına erişmek](media-services-portal-get-started-with-aad.md)üzere Azure Portal kullanın.
* [.NET ile Azure MEDIA SERVICES API 'sine erişmek](media-services-dotnet-get-started-with-aad.md)IÇIN Azure AD kimlik doğrulamasını kullanın.

