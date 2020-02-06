---
title: include dosyası
description: include dosyası
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 02/03/2020
ms.custom: include file
ms.openlocfilehash: cfe3eb4c0ac1378b7c519b3b34094945612d8508
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77029232"
---
>[!NOTE]
>Bu bölümde [Azure AD uygulama kaydı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app)için yönergeler sağlanmaktadır.

1. [Azure Portal](https://portal.azure.com), Genişletilebilir sol menüden **Azure Active Directory** açın ve **uygulama kayıtları** bölmesini açın. 

    [![Azure Active Directory bölmesini seçin](./media/digital-twins-permissions/azure-portal-select-aad-pane.png)](./media/digital-twins-permissions/azure-portal-select-aad-pane.png#lightbox)

1. **+ Yeni kayıt** düğmesini seçin.

    [![yeni kayıt düğmesini seçin](./media/digital-twins-permissions/aad-app-register.png)](./media/digital-twins-permissions/aad-app-register.png#lightbox)

1. **Ad** kutusuna bu uygulama kaydı için kolay bir ad verin. 

    1. **Yeniden yönlendirme URI 'si (isteğe bağlı)** bölümünde, metin kutusuna `https://microsoft.com` girin.     

    1. Azure Active Directory uygulamanız tarafından hangi hesapların ve kiracıların desteklendiğini doğrulayın.

    1. **Kaydol**’u seçin.

    [![oluştur bölmesi](./media/digital-twins-permissions/aad-app-reg-create.png)](./media/digital-twins-permissions/aad-app-reg-create.png#lightbox)

1. **Kimlik doğrulama** dikey penceresi, önemli kimlik doğrulama yapılandırma ayarlarını belirtir. 

    1. **+ Platform Ekle**' ye tıklayarak **yeniden yönlendirme URI 'Leri** ekleyin ve **erişim belirteçlerini** yapılandırın.

    1. Uygulamanın **ortak bir istemci**olduğunu belirtmek için **Evet** ' i seçin.

    1. Azure Active Directory uygulamanız tarafından hangi hesapların ve kiracıların desteklendiğini doğrulayın.

    [![ortak istemci yapılandırma ayarı](./media/digital-twins-permissions/aad-configure-public-client.png)](./media/digital-twins-permissions/aad-configure-public-client.png#lightbox)

1. Uygun platformu seçtikten sonra, **yeniden yönlendirme URI** 'larınızı ve yan paneldeki **erişim belirteçlerinizi** Kullanıcı arabiriminin sağına yapılandırın.

    1. **Yeniden yönlendirme URI 'leri** , kimlik doğrulama isteği tarafından sağlanan adresle eşleşmelidir:

        * Yerel bir geliştirme ortamında barındırılan uygulamalar için **ortak istemci (mobil & Masaüstü)** öğesini seçin. **Ortak Istemciyi** **Evet**olarak ayarladığınızdan emin olun.
        * Azure App Service barındırılan tek sayfalı uygulamalar için **Web**' i seçin.

    1. **Oturum kapatma URL 'sinin** uygun olup olmadığını belirleme.

    1. **Erişim belirteçlerini** veya **kimlik belirteçlerini**denetleyerek örtük izin akışını etkinleştirin.
                
    [Yeniden yönlendirme URI 'Lerini yapılandırma ![](./media/digital-twins-permissions/aad-app-configure-redirect-uris.png)](./media/digital-twins-permissions/aad-app-configure-redirect-uris.png#lightbox)

    **Yapılandır**' a ve ardından **Kaydet**' e tıklayın.

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
