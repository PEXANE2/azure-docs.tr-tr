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
ms.date: 09/24/2019
ms.openlocfilehash: 7e67b9e1c611d14b78db53f8662fb4e1f493a8df
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2019
ms.locfileid: "72990187"
---
> [!IMPORTANT]
> * Yeni **Azure Active Directory** > **uygulama kayıtları** dikey penceresi, eski **Azure Active Directory** > **uygulama kayıtları (eski)** dikey penceresinin 2019 Mayıs.
> * Eski dikey pencerede oluşturulan veya görüntülenen Uygulama kayıtları, otomatik olarak yeni dikey pencerede görüntülenir.
> * Yeni Azure uygulama kayıt deneyimine geçiş hakkında kapsamlı bilgi için [azure uygulama kayıtları Eğitim Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/app-registrations-training-guide) ' nu okuyun ve [hızlı başlangıç Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app).

1. [Azure portal](https://ms.portal.azure.com/) **Azure Active Directory** > **Yeni kayıt** > **uygulama kayıtları** seçin.

   [Yeni uygulama kaydı![Azure Active Directory](media/time-series-insights-aad-registration/active-directory-new-application-registration.png)](media/time-series-insights-aad-registration/active-directory-new-application-registration.png#lightbox)

   > [!TIP]
   > Yeni Azure Active Directory uygulama kayıt bölmesi, **sahip olunan uygulamaları**seçerek, görüntülenecek uygulamaları filtrelemenize olanak tanır.

    Uygulamanız kaydedildikten sonra burada listelenecektir.

1. Uygulamaya bir ad verin ve yalnızca API 'ye erişebilen **Desteklenen hesap türlerini** belirtmek için **bu kuruluş dizinindeki hesapları** seçin. Kullanıcıları kimlik doğrulamasından sonra yeniden yönlendirmek için geçerli bir URI seçin, sonra **kaydolun**.

   [uygulamayı Azure Active Directory oluşturmak![](media/time-series-insights-aad-registration/active-directory-registration.png)](media/time-series-insights-aad-registration/active-directory-registration.png#lightbox)

1. Önemli Azure Active Directory uygulama bilgileri, listelenen uygulamanızın **genel bakış** dikey penceresinde görüntülenir. **Sahip olunan uygulamalar**altında uygulamanızı seçin ve **genel bakış**' a tıklayın.

   [Uygulama KIMLIĞINI![kopyalamak](media/time-series-insights-aad-registration/active-directory-copy-application-id.png)](media/time-series-insights-aad-registration/active-directory-copy-application-id.png#lightbox)

   İstemci uygulamanızda kullanmak için **uygulamanızın (istemci) kimliğini** kopyalayın.

1. **Kimlik doğrulama** dikey penceresi, önemli kimlik doğrulama yapılandırma ayarlarını belirtir. 

    1. **Yeniden yönlendirme URI 'leri** , kimlik doğrulama isteği tarafından sağlanan adresle eşleşmelidir:

        * Yerel bir geliştirme ortamında barındırılan uygulamalar için **ortak istemci (mobil & Masaüstü)** öğesini seçin. **Varsayılan istemci türünü** Evet olarak ayarladığınızdan emin olun.
        * Azure App Service barındırılan tek sayfalı uygulamalar için **Web**' i seçin.

    1. **Kimlik belirteçlerini**denetleyerek örtük izin akışını etkinleştirin.

   [Yeni bir istemci parolası oluşturmak![](media/time-series-insights-aad-registration/active-directory-auth-blade.png)](media/time-series-insights-aad-registration/active-directory-auth-blade.png#lightbox)

   **Kaydet** düğmesine tıklayın.

1. Sertifikaların kimliğini kanıtlamak için kullanabileceği bir uygulama parolası oluşturmak için **sertifikalar & gizlilikler** ' ı ve sonra **yeni istemci gizli** anahtarını seçin.

   [Yeni bir istemci parolası oluşturmak![](media/time-series-insights-aad-registration/active-directory-application-keys-save.png)](media/time-series-insights-aad-registration/active-directory-application-keys-save.png#lightbox)

   Daha sonra, istemci gizli parolanız görüntülenir. Anahtarı en sevdiğiniz metin düzenleyiciye kopyalayın.

   > [!NOTE]
   > Bunun yerine bir sertifikayı içeri aktarma olanağınız vardır. Gelişmiş güvenlik için bir sertifika önerilir. Bir sertifika kullanmak için **sertifikayı karşıya yükle**' yi seçin.

1. Azure Active Directory uygulamanızı Azure zaman serisi öngörülerini ilişkilendirin. **Kuruluşumun kullandığı** **bir Izin** > API 'leri eklemek > **API izinleri** ' ni seçin. 

    [![bir API 'YI Azure Active Directory uygulamanızla Ilişkilendirme](media/time-series-insights-aad-registration/active-directory-app-api-permission.png)](media/time-series-insights-aad-registration/active-directory-app-api-permission.png#lightbox)

   Arama çubuğuna `Azure Time Series Insights` yazın ve `Azure Time Series Insights`' ı seçin.

1. Ardından, uygulamanızın gerektirdiği tür API iznini belirtin. Varsayılan olarak, **temsilci izinleri** vurgulanacaktır. Bir izin türü seçin, sonra **Izin Ekle**' yi seçin.

    [![uygulamanızın gerektirdiği API izninin türünü belirtin](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png)](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png#lightbox)