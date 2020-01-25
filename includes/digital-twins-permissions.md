---
title: include dosyası
description: include dosyası
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 01/23/2020
ms.custom: include file
ms.openlocfilehash: a1576e4a97af5de0b936c662de636aae542a19b5
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76748839"
---
>[!NOTE]
>Bu bölümde [Azure AD uygulama kaydı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app)için yönergeler sağlanmaktadır.

1. [Azure Portal](https://portal.azure.com), Genişletilebilir sol menüden **Azure Active Directory** açın ve **uygulama kayıtları** bölmesini açın. 

    [![Azure Active Directory bölmesini seçin](./media/digital-twins-permissions/azure-portal-select-aad-pane.png)](./media/digital-twins-permissions/azure-portal-select-aad-pane.png#lightbox)

1. **+ Yeni kayıt** düğmesini seçin.

    [![yeni kayıt düğmesini seçin](./media/digital-twins-permissions/aad-app-register.png)](./media/digital-twins-permissions/aad-app-register.png#lightbox)

1. **Ad** kutusuna bu uygulama kaydı için kolay bir ad verin. **Yeniden yönlendirme URI 'si (isteğe bağlı)** bölümünde, sol taraftaki açılan menüden **ortak istemci/yerel (mobil & Masaüstü)** öğesini seçin ve sağdaki metin kutusuna `https://microsoft.com` girin. **Kaydol**’u seçin.

    [![oluştur bölmesi](./media/digital-twins-permissions/aad-app-reg-create.png)](./media/digital-twins-permissions/aad-app-reg-create.png#lightbox)

1. [Uygulamanın bir **ortak istemci**olarak kaydedildiğinden](https://docs.microsoft.com/azure/active-directory/develop/scenario-desktop-app-registration)emin olmak için, uygulama kaydlarınızın **kimlik doğrulama** bölmesini açın ve bu bölmeyi aşağı kaydırın. **Varsayılan istemci türü** bölümünde, **uygulamayı ortak istemci olarak değerlendir**' **i seçin ve** **Kaydet**' i tıklayın.

    1. **Yeniden yönlendirme URI 'leri** , kimlik doğrulama isteği tarafından sağlanan adresle eşleşmelidir:

        * Yerel bir geliştirme ortamında barındırılan uygulamalar için **ortak istemci (mobil & Masaüstü)** öğesini seçin. **Varsayılan istemci türünü** Evet olarak ayarladığınızdan emin olun.
        * Azure App Service barındırılan tek sayfalı uygulamalar için **Web**' i seçin.

        **Ortak istemci (mobil & Masaüstü)** öğesini seçin ve `http://localhost:8080/`girin.

        [Yeniden yönlendirme URI 'Lerini yapılandırma ![](./media/digital-twins-permissions/aad-app-configure-redirect-uris.png)](./media/digital-twins-permissions/aad-app-configure-redirect-uris.png#lightbox)

    1. **Oauth2AllowImplicitFlow** ayarını kaynağınızın **bildiriminde** `true` üzere yapılandırmak için **erişim belirteçlerine** bakın.

        [![ortak istemci yapılandırma ayarı](./media/digital-twins-permissions/aad-configure-public-client.png)](./media/digital-twins-permissions/aad-configure-public-client.png#lightbox)

1.  Kayıtlı uygulamanızın **genel bakış** bölmesini açın ve aşağıdaki varlıkların değerlerini geçici bir dosyaya kopyalayın. Aşağıdaki bölümlerde örnek uygulamanızı yapılandırmak için bu değerleri kullanacaksınız.

    - **Uygulama (istemci) KIMLIĞI**
    - **Dizin (kiracı) KIMLIĞI**

    [![Azure Active Directory uygulama KIMLIĞI](./media/digital-twins-permissions/aad-app-reg-app-id.png)](./media/digital-twins-permissions/aad-app-reg-app-id.png#lightbox)

1. Uygulama kaydınız için **API izinleri** bölmesini açın. **+ Izin Ekle** düğmesini seçin. **API Izinleri iste** bölmesinde **Kuruluşumun kullandığı API 'leri** seçin ve ardından aşağıdakilerden birini arayın:
    
    1. `Azure Digital Twins`. **Azure dijital TWINS** API 'sini seçin.

        [![arama API 'SI veya Azure dijital TWINS](./media/digital-twins-permissions/aad-aap-search-api-dt.png)](./media/digital-twins-permissions/aad-aap-search-api-dt.png#lightbox)

    1. Alternatif olarak, `Azure Smart Spaces Service`için arama yapın. **Azure akıllı boşluklar hizmeti** API 'sini seçin.

        [Azure akıllı alanları için Search API ![](./media/digital-twins-permissions/aad-app-search-api.png)](./media/digital-twins-permissions/aad-app-search-api.png#lightbox)

    > [!IMPORTANT]
    > Görüntülenecek Azure AD API adı ve KIMLIĞI, kiracınıza bağlı olarak değişir:
    > * Test kiracının ve müşteri hesaplarının `Azure Digital Twins`araması gerekir.
    > * Diğer Microsoft hesaplarının `Azure Smart Spaces Service`araması gerekir.

1. Her iki API de aynı **istek API 'si izinleri** bölmesinde seçildikten sonra **Azure dijital TWINS** olarak görünür. **Oku** açılan seçeneğini belirleyin ve ardından **oku. yazma** onay kutusunu seçin. **Izin Ekle** düğmesini seçin.

    [![API izinleri ekleme](./media/digital-twins-permissions/aad-app-req-permissions.png)](./media/digital-twins-permissions/aad-app-req-permissions.png#lightbox)

1. Kuruluşunuzun ayarlarına bağlı olarak, bu API 'ye yönetici erişimi sağlamak için ek adımlar gerçekleştirmeniz gerekebilir. Daha fazla bilgi için yöneticinize başvurun. Yönetici erişimi onaylandıktan sonra, **API izinleri** bölmesindeki **yönetici onayı gerekli** sütunu, izinlerinizi görüntüler. 

    [Yönetici onay onayını ![](./media/digital-twins-permissions/aad-app-admin-consent.png)](./media/digital-twins-permissions/aad-app-admin-consent.png#lightbox)

    **Azure dijital TWINS** 'in göründüğünü doğrulayın.
