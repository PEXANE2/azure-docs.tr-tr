---
title: Twitter kimlik doğrulamasını Yapılandırma-Azure App Service
description: Uygulama Hizmetleri uygulamanız için Twitter kimlik doğrulamasını nasıl yapılandıracağınızı öğrenin.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: syntaxc4
editor: ''
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 04/19/2018
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: 728eacdcb0ee0d0bee878ff4764b1ca5e430c59c
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70088209"
---
# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a>App Service uygulamanızı Twitter oturum açma bilgilerini kullanacak şekilde yapılandırma
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu konuda, Azure App Service bir kimlik doğrulama sağlayıcısı olarak Twitter kullanmak üzere nasıl yapılandırılacağı gösterilmektedir.

Bu konudaki yordamı tamamlayabilmeniz için, doğrulanmış bir e-posta adresi ve telefon numarası olan bir Twitter hesabınızın olması gerekir. Yeni bir Twitter hesabı oluşturmak için <a href="https://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">Twitter.com</a>adresine gidin.

## <a name="register"> </a>Uygulamanızı Twitter 'a kaydetme
1. [Azure Portal]oturum açın ve uygulamanıza gidin. **URL**'nizi kopyalayın. Bunu, Twitter uygulamanızı yapılandırmak için kullanacaksınız.
2. [Twitter geliştiricileri] Web sitesine gidin, Twitter hesabı kimlik bilgilerinizle oturum açın ve **Yeni uygulama oluştur**' a tıklayın.
3. Yeni uygulamanız için **ad** ve **Açıklama** yazın. **Web sitesi** değeri Için uygulamanızın **URL 'sini** yapıştırın. Ardından, **geri arama URL 'si**için, daha önce kopyaladığınız **geri arama URL** 'sini yapıştırın. Bu, */. Auth/login/dallı geri çağırma*yoluna eklenmiş mobil uygulama ağ geçidiniz. Örneğin: `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. HTTPS şemasını kullandığınızdan emin olun.
4. Sayfanın alt kısmındaki koşulları okuyun ve kabul edin. Ardından **Twitter uygulamanızı oluştur**' a tıklayın. Bu, uygulamayı kaydeder uygulama ayrıntılarını görüntüler.
5. **Ayarlar** sekmesine tıklayın, **Bu uygulamanın Twitter 'da oturum açmak Için kullanılmasına izin ver**' i işaretleyin ve **Ayarları Güncelleştir**' e tıklayın.
6. **Anahtarlar ve erişim belirteçleri** sekmesini seçin. **Tüketici anahtarı (API anahtarı)** ve **Tüketici gızlı dizisi (API gizli)** değerlerini bir yere göz önünde alın.
   
   > [!NOTE]
   > Tüketici gizli anahtarı önemli bir güvenlik kimlik bilgileridir. Bu parolayı herkesle paylaşmayın veya uygulamanızla dağıtın.
   > 
   > 

## <a name="secrets"> </a>Uygulamanıza Twitter bilgilerini ekleyin
1. [Azure Portal]geri dönüp uygulamanıza gidin. **Ayarlar**' a ve ardından **kimlik doğrulama/yetkilendirme**' ye tıklayın.
2. Kimlik doğrulama/yetkilendirme özelliği etkinleştirilmemişse, geçişi **Açık**duruma getirin.
3. **Twitter**'a tıklayın. Daha önce edindiğiniz uygulama KIMLIĞI ve uygulama gizli anahtarı değerlerini yapıştırın. Daha sonra, **Tamam**'a tıklayın.
   
   ![][1]
   
   Varsayılan olarak, App Service kimlik doğrulaması sağlar ancak site içeriğinize ve API 'lerinize yetkili erişimi kısıtlamaz. Uygulama kodunuzda kullanıcıları yetkilendirmelisiniz.
4. Seçim Sitenize erişimi yalnızca Twitter tarafından kimliği doğrulanan kullanıcılarla kısıtlamak için, isteğin **Twitter**'da **kimlik doğrulaması olmadığında gerçekleştirilecek eylemi** ayarlayın. Bu, tüm isteklerin doğrulanmasını gerektirir ve kimliği doğrulanmamış tüm istekler kimlik doğrulaması için Twitter 'a yönlendirilir.

> [!CAUTION]
> Erişimin bu şekilde kısıtlanması, uygulamanıza yönelik tüm çağrılar için geçerlidir. Bu, birçok tek sayfalı uygulamalarda olduğu gibi genel kullanıma açık bir giriş sayfası gerektiren uygulamalar için istenmeyebilir. Bu tür uygulamalar için, [burada](overview-authentication-authorization.md#authentication-flow)açıklandığı gibi **anonim isteklere izin ver (eylem yok)** tercih edilebilir ve uygulamanın kendisi el ile oturum açma işlemi başlar.

5. **Kaydet**’e tıklayın.

Artık uygulamanızda kimlik doğrulaması için Twitter kullanmaya hazırsınız.

## <a name="related-content"> </a>İlgili içerik
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter geliştiricileri]: https://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
