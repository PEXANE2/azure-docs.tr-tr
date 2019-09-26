---
title: include dosyası
description: include dosyası
ms.topic: include
ms.custom: include file
services: time-series-insights
ms.service: time-series-insights
author: kingdomofends
ms.author: adgera
ms.date: 09/24/2019
ms.openlocfilehash: ccfbd16f4db770558f1bc0284860a5f8d9fb8b68
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266951"
---
> [!IMPORTANT]
> * Yeni **Azure Active Directory** > **uygulama kayıtları** dikey penceresi, eski **Azure Active Directory** > **uygulama kayıtları (eski)** dikey pencerenin yerini 2019 olabilir.
> * Eski dikey pencerede oluşturulan veya görüntülenen Uygulama kayıtları, otomatik olarak yeni dikey pencerede görüntülenir.
> * Yeni Azure uygulama kayıt deneyimine geçiş hakkında kapsamlı bilgi için [azure uygulama kayıtları Eğitim Kılavuzu](https://docs.microsoft.com/azure/active-directory/develop/app-registrations-training-guide) ' nu okuyun ve [hızlı başlangıç Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app).

1. [Azure Portal](https://ms.portal.azure.com/)**Yeni kayıt** **uygulama kayıtları** > AzureActiveDirectory > seçin.

   [![Azure Active Directory yeni uygulama kaydı](media/time-series-insights-aad-registration/active-directory-new-application-registration.png)](media/time-series-insights-aad-registration/active-directory-new-application-registration.png#lightbox)

   > [!TIP]
   > Yeni Azure Active Directory uygulama kayıt bölmesi, **sahip olunan uygulamaları**seçerek, görüntülenecek uygulamaları filtrelemenize olanak tanır.

    Uygulamanız kaydedildikten sonra burada listelenecektir.

1. Uygulamaya bir ad verin ve yalnızca API 'ye erişebilen **Desteklenen hesap türlerini** belirtmek için **bu kuruluş dizinindeki hesapları** seçin. Kullanıcıları kimlik doğrulamasından sonra yeniden yönlendirmek için geçerli bir URI seçin, sonra **kaydolun**.

   [![Uygulamayı Azure Active Directory oluşturma](media/time-series-insights-aad-registration/active-directory-registration.png)](media/time-series-insights-aad-registration/active-directory-registration.png#lightbox)

1. Önemli Azure Active Directory uygulama bilgileri, listelenen uygulamanızın **genel bakış** dikey penceresinde görüntülenir. **Sahip olunan uygulamalar**altında uygulamanızı seçin ve **genel bakış**' a tıklayın.

   [![Uygulama KIMLIĞINI kopyalama](media/time-series-insights-aad-registration/active-directory-copy-application-id.png)](media/time-series-insights-aad-registration/active-directory-copy-application-id.png#lightbox)

   İstemci uygulamanızda kullanmak için **uygulamanızın (istemci) kimliğini** kopyalayın.

1. **Kimlik doğrulama** dikey penceresi, önemli kimlik doğrulama yapılandırma ayarlarını belirtir. 

    1. **Yeniden yönlendirme URI 'leri** , kimlik doğrulama isteği tarafından sağlanan adresle eşleşmelidir:

        * Yerel bir geliştirme ortamında barındırılan uygulamalar için **ortak istemci (mobil & Masaüstü)** öğesini seçin. **Varsayılan istemci türünü** Evet olarak ayarladığınızdan emin olun.
        * Azure App Service barındırılan tek sayfalı uygulamalar için **Web**' i seçin.

    1. **Kimlik belirteçlerini**denetleyerek örtük izin akışını etkinleştirin.

   [![Yeni bir istemci parolası oluştur](media/time-series-insights-aad-registration/active-directory-auth-blade.png)](media/time-series-insights-aad-registration/active-directory-auth-blade.png#lightbox)

   **Kaydet**’e tıklayın.

1. Sertifikaların kimliğini kanıtlamak için kullanabileceği bir uygulama parolası oluşturmak için **sertifikalar & gizlilikler** ' ı ve sonra **yeni istemci gizli** anahtarını seçin.

   [![Yeni bir istemci parolası oluştur](media/time-series-insights-aad-registration/active-directory-application-keys-save.png)](media/time-series-insights-aad-registration/active-directory-application-keys-save.png#lightbox)

   Daha sonra, istemci gizli parolanız görüntülenir. Anahtarı en sevdiğiniz metin düzenleyiciye kopyalayın.

   > [!NOTE]
   > Bunun yerine bir sertifikayı içeri aktarma olanağınız vardır. Gelişmiş güvenlik için bir sertifika önerilir. Bir sertifika kullanmak için **sertifikayı karşıya yükle**' yi seçin.

1. Azure Active Directory uygulamanızı Azure zaman serisi öngörülerini ilişkilendirin. **API izinlerini** > seçin**Kuruluşumun kullandığı** **izin** > API 'leri ekleyin. 

    [![Azure Active Directory uygulamanızla bir API 'YI ilişkilendirme](media/time-series-insights-aad-registration/active-directory-app-api-permission.png)](media/time-series-insights-aad-registration/active-directory-app-api-permission.png#lightbox)

   Arama `Azure Time Series Insights` çubuğuna yazın ve ardından öğesini seçin `Azure Time Series Insights`.

1. Ardından, uygulamanızın gerektirdiği tür API iznini belirtin. Varsayılan olarak, **temsilci izinleri** vurgulanacaktır. Bir izin türü seçin, sonra **Izin Ekle**' yi seçin.

    [![Uygulamanızın gerektirdiği API izninin türünü belirtin](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png)](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png#lightbox)