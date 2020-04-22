---
title: Azure Etkin Dizin kimlik doğrulaması ile Azure Medya Hizmetleri API'na erişin | Microsoft Dokümanlar
description: Azure Medya Hizmetleri API'sine erişimin kimliğini doğrulamak için Azure Active Directory 'i (Azure AD) kullanmak için atılması gereken kavramlar ve adımlar hakkında bilgi edinin.
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
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81682040"
---
# <a name="access-the-azure-media-services-api-with-azure-ad-authentication"></a>Azure AD kimlik doğrulaması ile Azure Media Services API’sine erişme  

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Azure Medya Hizmetleri API'si yeni bir API'dir. Rest API kullanarak veya kullanılabilir istemci SDK'ları kullanarak medya kaynakları üzerinde işlemleri gerçekleştirmek için kullanabilirsiniz. Azure Media Services, Microsoft .NET için bir Media Services istemcisi SDK sunar. Medya Hizmetleri kaynaklarına ve Medya Hizmetleri API'sine erişme yetkisine ulaşmak için öncelikle kimlik doğrulaması yapılması gerekir. 

Medya [Hizmetleri, Azure Etkin Dizin (Azure AD) tabanlı kimlik doğrulamayı](../../active-directory/fundamentals/active-directory-whatis.md)destekler. Azure Media REST hizmeti, REST API isteklerini yapan kullanıcının veya uygulamanın kaynaklara erişmek için **Katılımcı** veya **Sahip** rolüne sahip olmasını gerektirir. Daha fazla bilgi için azure [portalında Rol Tabanlı Erişim Denetimi'ne başlayın'](../../role-based-access-control/overview.md)a bakın.  

Bu belge, REST veya .NET API'lerini kullanarak Medya Hizmetleri API'sine nasıl erişeceğimiz hakkında genel bir bakış sağlar.

> [!NOTE]
> Erişim Denetimi yetkisi 1 Haziran 2018 tarihinde iptal edilmiştir.

## <a name="access-control"></a>Erişim denetimi

Azure Media REST isteğinin başarılı olabilmesi için, arayan kullanıcının erişmeye çalıştığı Medya Hizmetleri hesabı için Bir Katılımcı veya Sahip rolü olması gerekir.  
Yalnızca Sahip rolüne sahip bir kullanıcı, medya kaynağına (hesap) yeni kullanıcılara veya uygulamalara erişim sağlayabilir. Katılımcı rolü yalnızca medya kaynağına erişebilir.
Yetkisiz istekler 401 durum koduyla başarısız oldu. Bu hata kodunu görürseniz, kullanıcınızın Medya Hizmetleri hesabı için atanan Katılımcı veya Sahip rolüne sahip olup olmadığını kontrol edin. Bunu Azure portalından kontrol edebilirsiniz. Medya hesabınızı arayın ve ardından **Access denetim** sekmesini tıklatın. 

![Erişim denetimi sekmesi](./media/media-services-use-aad-auth-to-access-ams-api/media-services-access-control.png)

## <a name="types-of-authentication"></a>Kimlik doğrulama türleri 
 
Azure Media Hizmetleri ile Azure AD kimlik doğrulamasını kullandığınızda, iki kimlik doğrulama seçeneğiniz vardır:

- **Kullanıcı kimlik doğrulaması.** Medya Hizmetleri kaynaklarıyla etkileşimde kalmak için uygulamayı kullanan kişinin kimliğini doğrulatın. Etkileşimli uygulama öncelikle kullanıcının kimlik bilgilerini ister. Bir örnek, kodlama işlerini veya canlı akışı izlemek için yetkili kullanıcılar tarafından kullanılan bir yönetim konsolu uygulamasıdır. 
- **Hizmet ana kimlik doğrulaması.** Bir hizmeti doğrula. Bu kimlik doğrulama yöntemini yaygın olarak kullanan uygulamalar, daemon hizmetlerini, orta katman hizmetlerini veya zamanlanmış işleri çalıştıran uygulamalardır. Örnekler web uygulamaları, işlev uygulamaları, mantık uygulamaları, API ve mikro hizmetlerdir.

### <a name="user-authentication"></a>Kullanıcı kimlik doğrulaması 

Kullanıcı kimlik doğrulama yöntemini kullanması gereken uygulamalar, yerel uygulamaları yönetme veya izlemedir: mobil uygulamalar, Windows uygulamaları ve Konsol uygulamaları. Bu tür bir çözüm, aşağıdaki senaryolardan birinde hizmetle insan etkileşimi istediğinizde yararlıdır:

- Kodlama işleriniz için pano izleme.
- Canlı akışlarınız için izleme panosu.
- Medya Hizmetleri hesabında ki kaynakları yönetmek için masaüstü veya mobil kullanıcılar için yönetim uygulaması.

> [!NOTE]
> Bu kimlik doğrulama yöntemi tüketiciye yönelik uygulamalar için kullanılmamalıdır. 

Yerel bir uygulamanın önce Azure AD'den bir erişim jetonu edinmesi ve ardından Media Services REST API'ye HTTP isteklerinde kullanmanız gerekir. İstek üstbilgisine erişim belirteci ekleyin. 

Aşağıdaki diyagram tipik bir etkileşimli uygulama kimlik doğrulama akışını gösterir: 

![Yerel uygulamalar diyagramı](./media/media-services-use-aad-auth-to-access-ams-api/media-services-native-aad-app1.png)

Önceki diyagramda, sayılar isteklerin akışını kronolojik sırada temsil eder.

> [!NOTE]
> Kullanıcı kimlik doğrulama yöntemini kullandığınızda, tüm uygulamalar aynı (varsayılan) yerel uygulama istemci kimliğini ve yerel uygulama yeniden yönlendirmeURI'yi paylaşır. 

1. Kimlik bilgileri için bir kullanıcı ister.
2. Aşağıdaki parametrelere sahip bir Azure AD erişim belirteci isteyin:  

   * Azure AD kiracı bitiş noktası.

       Kiracı bilgileri Azure portalından alınabilir. İmlecinizi sağ üst köşedeki oturum açmış kullanıcının adının üzerine yerleştirin.
   * Medya Hizmetleri kaynak URI. 

       Bu URI, aynı Azure ortamında bulunan Medya Hizmetleri hesapları için de\/aynıdır (örneğin, https: /rest.media.azure.net).

   * Medya Hizmetleri (yerel) uygulama istemci kimliği.
   * Medya Hizmetleri (yerel) uygulaması URI yönlendirme.
   * REST Medya Hizmetleri için Kaynak URI.
        
       URI, REST API bitiş noktasını temsil https://test03.restv2.westus.media.azure.net/api/)eder (örneğin, .

     Bu parametreler için değer elde etmek için, kullanıcı kimlik doğrulama seçeneğini kullanarak [Azure AD kimlik doğrulama ayarlarına erişmek için Azure portalını](media-services-portal-get-started-with-aad.md) kullanın'a bakın.

3. Azure AD erişim belirteci istemciye gönderilir.
4. İstemci, Azure AD erişim belirteciyle Azure Media REST API'sine bir istek gönderir.
5. İstemci, Medya Hizmetleri'nden gelen verileri geri alır.

Medya Hizmetleri .NET istemciSI SDK'yı kullanarak REST istekleriyle iletişim kurmak için Azure AD kimlik doğrulamasını nasıl kullanacağıhakkında bilgi için [bkz.](media-services-dotnet-get-started-with-aad.md) 

Medya Hizmetleri .NET istemciSi SDK'yı kullanmıyorsanız, adım 2'de açıklanan parametreleri kullanarak bir Azure AD erişim belirteci isteği oluşturmanız gerekir. Daha fazla bilgi için Azure [AD belirteci almak için Azure AD Kimlik Doğrulama Kitaplığını nasıl kullanacağınız](../../active-directory/azuread-dev/active-directory-authentication-libraries.md)abakın.

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Bu kimlik doğrulama yöntemini yaygın olarak kullanan uygulamalar, orta katman hizmetleri ve zamanlanmış işler çalıştıran uygulamalardır: web uygulamaları, işlev uygulamaları, mantık uygulamaları, API'ler ve mikro hizmetler. Bu kimlik doğrulama yöntemi, kaynakları yönetmek için bir hizmet hesabı kullanmak isteyebileceğin etkileşimli uygulamalar için de uygundur.

Tüketici senaryoları oluşturmak için hizmet temel kimlik doğrulama yöntemini kullandığınızda, kimlik doğrulama genellikle orta katmanda (bazı API'ler aracılığıyla) işlenir ve doğrudan bir mobil veya masaüstü uygulamasında değil. 

Bu yöntemi kullanmak için, kendi kiracısında bir Azure AD uygulaması ve hizmet ilkesi oluşturun. Uygulamayı oluşturduktan sonra, uygulamanın Medya Hizmetleri hesabına Katkıda Bulunan veya Sahip rolüne erişim izni verin. Bunu Azure portalında, Azure CLI'yi kullanarak veya PowerShell komut dosyasıyla yapabilirsiniz. Ayrıca varolan bir Azure AD uygulamasını da kullanabilirsiniz. Azure REKLAM uygulamanızı ve hizmet müdürünüzü [Azure portalında](media-services-portal-get-started-with-aad.md)kaydedebilir ve yönetebilirsiniz. Bunu [Azure CLI](media-services-use-aad-auth-to-access-ams-api.md) veya [PowerShell'i](media-services-powershell-create-and-configure-aad-app.md)kullanarak da yapabilirsiniz. 

![Orta katman uygulamalar](./media/media-services-use-aad-auth-to-access-ams-api/media-services-principal-service-aad-app1.png)

Azure AD uygulamanızı oluşturduktan sonra, aşağıdaki ayarlar için değerler alırsınız. Kimlik doğrulaması için bu değerlere ihtiyacınız vardır:

- İstemci Kimliği 
- Gizli anahtar 

Bir önceki şekilde, sayılar isteklerin akışını kronolojik sırada temsil eleştirir:
    
1. Orta katmandaki bir uygulama (web API veya web uygulaması), aşağıdaki parametrelere sahip bir Azure AD erişim belirteci ister:  

   * Azure AD kiracı bitiş noktası.

       Kiracı bilgileri Azure portalından alınabilir. İmlecinizi sağ üst köşedeki oturum açmış kullanıcının adının üzerine yerleştirin.
   * Medya Hizmetleri kaynak URI. 

       Bu URI, aynı Azure ortamında bulunan Medya Hizmetleri hesapları için de geçerlidir (örneğin, https:\//rest.media.azure.net).

   * REST Medya Hizmetleri için Kaynak URI.

       URI, REST API bitiş noktasını temsil https://test03.restv2.westus.media.azure.net/api/)eder (örneğin, .

   * Azure AD uygulama değerleri: istemci kimliği ve istemci sırrı.
    
     Bu parametreler için değer elde etmek için, hizmet temel kimlik doğrulama seçeneğini kullanarak [Azure AD kimlik doğrulama ayarlarına erişmek için Azure portalını](media-services-portal-get-started-with-aad.md) kullanın'a bakın.

2. Azure AD erişim belirteci orta katmana gönderilir.
4. Orta katman, Azure AD belirteciyle birlikte Azure Media REST API'sine istek gönderir.
5. Orta katman, Medya Hizmetleri'nden gelen verileri geri alır.

Medya Hizmetleri .NET istemciSI SDK'yı kullanarak REST istekleriyle iletişim kurmak için Azure AD kimlik doğrulamasını nasıl kullanacağımız hakkında daha fazla bilgi için [bkz.](media-services-dotnet-get-started-with-aad.md) 

Medya Hizmetleri .NET istemciSi SDK'yı kullanmıyorsanız, adım 1'de açıklanan parametreleri kullanarak el ile bir Azure AD belirteç isteği oluşturmanız gerekir. Daha fazla bilgi için Azure [AD belirteci almak için Azure AD Kimlik Doğrulama Kitaplığını nasıl kullanacağınız](../../active-directory/azuread-dev/active-directory-authentication-libraries.md)abakın.

## <a name="troubleshooting"></a>Sorun giderme

Özel Durum: "Uzak sunucu bir hata döndürülür: (401) Yetkisiz."

Çözüm: Media Services REST isteğinin başarılı olabilmesi için, arayan kullanıcının erişmeye çalıştığı Medya Hizmetleri hesabında Bir Katılımcı veya Sahip rolü olması gerekir. Daha fazla bilgi için [Erişim denetimi](media-services-use-aad-auth-to-access-ams-api.md#access-control) bölümüne bakın.

## <a name="resources"></a>Kaynaklar

Aşağıdaki makaleler, Azure AD kimlik doğrulama kavramlarına genel bakışlardır: 

- [Azure AD tarafından ele verilen kimlik doğrulama senaryoları](../../active-directory/develop/authentication-scenarios.md)
- [Azure AD'de bir uygulama ekleme, güncelleştirme veya kaldırma](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md)
- [PowerShell'i kullanarak Rol Tabanlı Erişim Denetimini yapılandırın ve yönetin](../../role-based-access-control/role-assignments-powershell.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Medya Hizmetleri API'sini kullanmak için Azure AD kimlik doğrulaması'na erişmek için](media-services-portal-get-started-with-aad.md)Azure portalını kullanın.
* [.NET ile Azure Medya Hizmetleri API'sine erişmek](media-services-dotnet-get-started-with-aad.md)için Azure AD kimlik doğrulamasını kullanın.

