---
title: Azure portal kullanarak Azure AD kimlik doğrulaması ile çalışmaya başlama | Microsoft Docs
description: Azure Media Services API 'sini kullanmak için Azure Active Directory (Azure AD) kimlik doğrulamasına erişmek üzere Azure portal nasıl kullanacağınızı öğrenin.
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
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 4428187c985c1004c88f2ac20b0e5811803cce2a
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77162778"
---
# <a name="get-started-with-azure-ad-authentication-by-using-the-azure-portal"></a>Azure portal kullanarak Azure AD kimlik doğrulaması ile çalışmaya başlama

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz. [v2 'den v3 'e geçiş kılavuzu](../latest/migrate-from-v2-to-v3.md)

Azure Media Services API 'sine erişmek için Azure Active Directory (Azure AD) kimlik doğrulamasına erişmek üzere Azure portal nasıl kullanacağınızı öğrenin.

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure hesabı. Hesabınız yoksa [Azure Ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/)ile başlayın. 
- Bir Media Services hesabı. Daha fazla bilgi için [Azure Portal kullanarak Azure Media Services hesabı oluşturma](media-services-portal-create-account.md)konusuna bakın.
- [Azure AD kimlik doğrulamasına genel bakış Ile erişim Azure MEDIA SERVICES API](media-services-use-aad-auth-to-access-ams-api.md)'sini gözden geçirdiğinizden emin olun. 

Azure Media Services ile Azure AD kimlik doğrulaması kullandığınızda, iki kimlik doğrulama seçeneğiniz vardır:

- **Kullanıcı kimlik doğrulaması**. Media Services kaynaklarla etkileşim kurmak için uygulamayı kullanan bir kişinin kimliğini doğrulayın. Etkileşimli uygulama öncelikle kullanıcıdan kimlik bilgilerini istemelidir. Bu örnek, yetkili kullanıcılar tarafından kodlama işlerini veya canlı akışı izlemek için kullanılan bir yönetim konsolu uygulamasıdır. 
- **Hizmet sorumlusu kimlik doğrulaması**. Bir hizmetin kimliğini doğrulayın. Bu kimlik doğrulama yöntemini yaygın olarak kullanan uygulamalar, Daemon Hizmetleri, orta katman hizmetler veya zamanlanan işleri çalıştıran uygulamalardır: Web Apps, işlev uygulamaları, Logic Apps, API 'Ler veya mikro hizmet.

> [!IMPORTANT]
> Şu anda Media Services Azure Access Control hizmeti kimlik doğrulama modelini destekler. Ancak, Access Control yetkilendirmesi 1 Haziran 2018 ' de kullanım dışı olacaktır. Azure AD kimlik doğrulaması modeline mümkün olan en kısa sürede geçiş yapmanız önerilir.

## <a name="select-the-authentication-method"></a>Kimlik doğrulama yöntemini seçin

1. [Azure portal](https://portal.azure.com/)Media Services hesabınızı seçin.
2. Media Services API 'sine nasıl bağlanılacağını seçin.

    ![Bağlantı yöntemi seçin sayfası](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started01.png)

## <a name="user-authentication"></a>Kullanıcı kimlik doğrulaması

Kullanıcı kimlik doğrulaması seçeneğini kullanarak Media Services API 'sine bağlanmak için, istemci uygulamanın aşağıdaki parametrelere sahip bir Azure AD belirteci istemesi gerekir:  

* Azure AD kiracı uç noktası
* Media Services kaynak URI 'SI
* Media Services (yerel) uygulama istemci KIMLIĞI 
* Media Services (yerel) uygulama yeniden yönlendirme URI 'SI 
* REST Media Services için kaynak URI 'SI

Bu parametrelerin değerlerini **Kullanıcı kimlik doğrulaması ile Media Services API 'si ile** alabilirsiniz. 

![Kullanıcı kimlik doğrulama sayfasıyla Bağlan](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started02.png)

Media Services Microsoft .NET SDK 'sını kullanarak Media Services API 'sine bağlanıyorsanız, gerekli değerler SDK 'nin bir parçası olarak kullanılabilir. Daha fazla bilgi için bkz. [.NET ile Azure MEDIA SERVICES API 'sine erişmek Için Azure AD kimlik doğrulamasını kullanma](media-services-dotnet-get-started-with-aad.md).

Media Services .NET istemci SDK 'sını kullanmıyorsanız, daha önce tartışılan parametreleri kullanarak el ile bir Azure AD belirteç isteği oluşturmanız gerekir. Daha fazla bilgi için bkz. Azure AD [kimlik doğrulama kitaplığı 'nı kullanarak Azure AD belirtecini alma](../../active-directory/azuread-dev/active-directory-authentication-libraries.md).

## <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Hizmet sorumlusu seçeneğini kullanarak Media Services API 'sine bağlanmak için, orta katman uygulamanızın (Web API 'SI veya Web uygulaması) aşağıdaki parametrelere sahip bir Azure AD belirteci istemesi gerekir:  

* Azure AD kiracı uç noktası
* Media Services kaynak URI 'SI 
* REST Media Services için kaynak URI 'SI
* Azure AD uygulama değerleri: **ISTEMCI kimliği** ve **istemci parolası**

Bu parametrelerin değerlerini **hizmet sorumlusu ile MEDIA SERVICES API 'Sine Bağlan** sayfasından alabilirsiniz. Yeni bir Azure AD uygulaması oluşturmak veya var olanı seçmek için bu sayfayı kullanın. Azure AD uygulamasını seçtikten sonra, istemci KIMLIĞINI (uygulama KIMLIĞI) alabilir ve istemci gizli anahtarı (anahtar) değerlerini oluşturabilirsiniz. 

![Hizmet sorumlusu sayfasıyla bağlantı](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started04.png)

**Hizmet sorumlusu** dikey penceresi açıldığında, aşağıdaki ölçütlere uyan Ilk Azure AD uygulaması seçilidir:

- Kayıtlı bir Azure AD uygulamasıdır.
- Hesapta katkıda bulunan veya sahip rol tabanlı Access Control izinleri vardır.

Bir Azure AD uygulaması oluşturduktan veya seçtikten sonra, bir istemci parolası (anahtar) ve istemci KIMLIĞI (uygulama KIMLIĞI) oluşturup kopyalayabilirsiniz. Bu senaryoda erişim belirtecini almak için istemci parolası ve istemci KIMLIĞI gereklidir.

Etki alanında Azure AD uygulamaları oluşturma izniniz yoksa, dikey pencerenin Azure AD uygulama denetimleri gösterilmez ve bir uyarı iletisi görüntülenir.

Media Services .NET SDK 'Yı kullanarak Media Services API 'sine bağlanıyorsanız, bkz. [.NET ile Azure MEDIA SERVICES API 'sine erişmek Için Azure AD kimlik doğrulamasını kullanma](media-services-dotnet-get-started-with-aad.md).

.NET istemci SDK Media Services kullanmıyorsanız, daha önce tartışılan parametreleri kullanarak el ile bir Azure AD belirteç isteği oluşturmanız gerekir. Daha fazla bilgi için bkz. Azure AD [kimlik doğrulama kitaplığı 'nı kullanarak Azure AD belirtecini alma](../../active-directory/azuread-dev/active-directory-authentication-libraries.md).

### <a name="get-the-client-id-and-client-secret"></a>İstemci KIMLIĞINI ve gizli anahtarı al

Mevcut bir Azure AD uygulamasını seçip yeni bir tane oluşturma seçeneğini belirledikten sonra aşağıdaki düğmeler görüntülenir:

![İzinleri Yönet düğmesi ve uygulamayı Yönet düğmesi](./media/media-services-portal-get-started-with-aad/media-services-portal-manage.png)

Azure AD uygulaması dikey penceresini açmak için, **Uygulamayı Yönet**' e tıklayın. **Uygulamayı Yönet** dikey penceresinde, UYGULAMANıN istemci kimliğini (uygulama kimliği) alabilirsiniz. Bir istemci gizli anahtarı (anahtar) oluşturmak için **anahtarlar**' ı seçin.

![Uygulama dikey penceresi tuşlarını Yönet seçeneği](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started06.png) 

### <a name="manage-permissions-and-the-application"></a>İzinleri ve uygulamayı yönetme

Azure AD uygulamasını seçtikten sonra, uygulamayı ve izinleri yönetebilirsiniz. Azure AD uygulamanızı diğer uygulamalara erişecek şekilde ayarlamak için **Izinleri Yönet**' e tıklayın. Anahtar ve yanıt URL 'Leri değiştirme veya uygulamanın bildirimini düzenleme gibi yönetim görevleri için, **Uygulamayı Yönet**' e tıklayın.

### <a name="edit-the-apps-settings-or-manifest"></a>Uygulamanın ayarlarını veya bildirimini düzenleme

Uygulamanın ayarlarını veya bildirimini düzenlemek için, **Uygulamayı Yönet**' e tıklayın.

![Uygulama sayfasını yönet](./media/media-services-portal-get-started-with-aad/media-services-portal-get-started05.png)

## <a name="next-steps"></a>Sonraki adımlar

[Hesabınıza dosya yüklemeye](media-services-portal-upload-files.md)başlayın.
