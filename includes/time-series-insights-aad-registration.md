---
title: include dosyası
description: include dosyası
ms.topic: include
ms.custom: include file
services: time-series-insights
ms.service: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.date: 02/03/2020
ms.openlocfilehash: 5be6e7937a6e1f710b8e2576a9058963413fb6c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76984598"
---
1. Azure [portalında](https://ms.portal.azure.com/)Azure **Active Directory** > **App kayıtlarını** > seçin**Yeni kayıt**.

   [![Azure Active Directory'de yeni uygulama kaydı](media/time-series-insights-aad-registration/active-directory-new-application-registration.png)](media/time-series-insights-aad-registration/active-directory-new-application-registration.png#lightbox)

    Uygulamanız kaydettikten sonra burada listelenecektir.

1. Yalnızca API'ye erişebilecek **Desteklenen hesap türlerini** belirtmek için uygulamaya bir ad verin ve **bu kuruluş dizinindeki Hesapları** seçin. Kullanıcıları kimlik doğrulaması yaptıktan sonra yönlendirmek için geçerli bir URI seçin, ardından **Kaydolun.**

   [![Azure Active Directory'de uygulama oluşturma](media/time-series-insights-aad-registration/active-directory-registration.png)](media/time-series-insights-aad-registration/active-directory-registration.png#lightbox)

1. Önemli Azure Active Directory uygulama bilgileri, listelenen uygulamanızın **Genel Bakış** bıçaklarında görüntülenir. **Uygulamanızı Owned uygulamaları**altında seçin, ardından **Genel Bakış**.

   [![Başvuru kimliğini kopyalama](media/time-series-insights-aad-registration/active-directory-copy-application-id.png)](media/time-series-insights-aad-registration/active-directory-copy-application-id.png#lightbox)

   Müşteri başvurunuzda kullanmak üzere **Başvuru (istemci) kimliğinizi** kopyalayın.

1. **Kimlik Doğrulama** bıçağı önemli kimlik doğrulama yapılandırma ayarlarını belirtir. 

    1. **Yeniden Yönlendirme URL'leri** ekleyin ve + **Bir platform ekleyin'i**seçerek **Access Belirteçleri'ni** yapılandırın.

    1. **Evet** veya **Hayır'ı**seçerek uygulamanın **ortak** istemci olup olmadığını belirleyin.

    1. Hangi hesapların ve kiracıların desteklendiğinden doğrulayın.

    [![Örtülü hibeyi yapılandır](media/time-series-insights-aad-registration/active-directory-auth-blade.png)](media/time-series-insights-aad-registration/active-directory-auth-blade.png#lightbox)

1. Uygun platformu seçtikten sonra yan paneldeki **Yönlendirme URL'lerinizi** ve **Erişim Belirteçlerinizi** kullanıcı arabiriminin sağına doğru yapılandırın.

    1. **Yeniden Yönlendirme ÜR'leri,** kimlik doğrulama isteği tarafından sağlanan adresle eşleşmelidir:

        * Yerel geliştirme ortamında barındırılan uygulamalar için **Ortak istemci (mobil & masaüstü)** seçeneğini belirleyin. **Genel istemciyi** **Evet**olarak ayarladıklıolun.
        * Azure Uygulama Hizmetinde barındırılan Tek Sayfalı Uygulamalar için **Web'i**seçin.

    1. **Oturum Açma URL'nin** uygun olup olmadığını belirleyin.

    1. **Erişim belirteçlerini** veya **kimlik belirteçlerini**denetleyerek örtülü hibe akışını etkinleştirin.

    [![Yönlendirme URL'leri Oluştur](media/time-series-insights-aad-registration/active-directory-auth-redirect-uri.png)](media/time-series-insights-aad-registration/active-directory-auth-redirect-uri.png#lightbox)

    **Yapılaşı'yı**tıklatın, ardından **Kaydet'** i tıklatın.

1. **Sertifikalar & sırları** sonra **yeni istemci gizli** istemci uygulaması kimliğini kanıtlamak için kullanabileceğiniz bir uygulama parolası oluşturmak için seçin.

   [![Yeni bir istemci sırrı oluşturma](media/time-series-insights-aad-registration/active-directory-application-keys-save.png)](media/time-series-insights-aad-registration/active-directory-application-keys-save.png#lightbox)

   İstemci gizli parolanız daha sonra görüntülenir. En sevdiğiniz metin düzenleyicisinin anahtarını kopyalayın.

   > [!NOTE]
   > Bunun yerine bir sertifika alma olanağınız var. Gelişmiş güvenlik için bir sertifika önerilir. Sertifika kullanmak için **Yükle sertifikasını**seçin.

1. Azure Active Directory uygulamanızı Azure TIme Series Insights ile ilişkilendirin. API **izinlerini** > seçin Kuruluşumun kullandığı izin > **API'leri****ekleyin.** 

    [![Bir API'yi Azure Etkin Dizin uygulamanızla ilişkilendirin](media/time-series-insights-aad-registration/active-directory-app-api-permission.png)](media/time-series-insights-aad-registration/active-directory-app-api-permission.png#lightbox)

   Arama `Azure Time Series Insights` çubuğuna yazın `Azure Time Series Insights`ve sonra .

1. Ardından, uygulamanızın gerektirdiği TÜR API iznini belirtin. Varsayılan olarak, **Temsilciverilen izinler** vurgulanır. Sonra bir izin türü seçin, **İzin Ekle'yi**seçin.

    [![Uygulamanızın gerektirdiği API izni türünü belirtin](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png)](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png#lightbox)
