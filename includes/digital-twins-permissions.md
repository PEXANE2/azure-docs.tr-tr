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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77029232"
---
>[!NOTE]
>Bu bölümde [Azure AD uygulama kaydı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app)için yönergeler verilmektedir.

1. Azure [portalında,](https://portal.azure.com)genişletilebilir sol menüden **Azure Etkin Dizini'ni** açın ve ardından Uygulama **kayıtları** bölmesini açın. 

    [![Azure Etkin Dizin bölmesini seçin](./media/digital-twins-permissions/azure-portal-select-aad-pane.png)](./media/digital-twins-permissions/azure-portal-select-aad-pane.png#lightbox)

1. + **Yeni kayıt** düğmesini seçin.

    [![Yeni kayıt düğmesini seçin](./media/digital-twins-permissions/aad-app-register.png)](./media/digital-twins-permissions/aad-app-register.png#lightbox)

1. **Ad** kutusundaki bu uygulama kaydı için dostça bir ad verin. 

    1. **URI'yi Yönlendirme (isteğe bağlı)** bölümü altında, metin kutusuna girin. `https://microsoft.com`     

    1. Azure Active Directory uygulamanız tarafından hangi hesapların ve kiracıların desteklenerek desteklendiklerine doğrulayın.

    1. **Kaydol**’u seçin.

    [![Bölme oluşturma](./media/digital-twins-permissions/aad-app-reg-create.png)](./media/digital-twins-permissions/aad-app-reg-create.png#lightbox)

1. **Kimlik Doğrulama** bıçağı önemli kimlik doğrulama yapılandırma ayarlarını belirtir. 

    1. **Yeniden Yönlendirme URL'leri** ekleyin ve + **Bir platform ekleyin'i**seçerek **Access Belirteçleri'ni** yapılandırın.

    1. Uygulamanın **ortak istemci**olduğunu belirtmek için **Evet'i** seçin.

    1. Azure Active Directory uygulamanız tarafından hangi hesapların ve kiracıların desteklenerek desteklendiklerine doğrulayın.

    [![Genel istemci yapılandırma ayarı](./media/digital-twins-permissions/aad-configure-public-client.png)](./media/digital-twins-permissions/aad-configure-public-client.png#lightbox)

1. Uygun platformu seçtikten sonra yan paneldeki **Yönlendirme URL'lerinizi** ve **Erişim Belirteçlerinizi** kullanıcı arabiriminin sağına doğru yapılandırın.

    1. **Yeniden Yönlendirme ÜR'leri,** kimlik doğrulama isteği tarafından sağlanan adresle eşleşmelidir:

        * Yerel geliştirme ortamında barındırılan uygulamalar için **Ortak istemci (mobil & masaüstü)** seçeneğini belirleyin. **Genel istemciyi** **Evet**olarak ayarladıklıolun.
        * Azure Uygulama Hizmetinde barındırılan Tek Sayfalı Uygulamalar için **Web'i**seçin.

    1. **Oturum Açma URL'nin** uygun olup olmadığını belirleyin.

    1. **Erişim belirteçlerini** veya **kimlik belirteçlerini**denetleyerek örtülü hibe akışını etkinleştirin.
                
    [![Yeniden Yönlendirme ÜR'leri Yapılandır](./media/digital-twins-permissions/aad-app-configure-redirect-uris.png)](./media/digital-twins-permissions/aad-app-configure-redirect-uris.png#lightbox)

    **Yapılaşı'yı**tıklatın, ardından **Kaydet'** i tıklatın.

1.  Kayıtlı uygulamanızın **Genel Bakış** bölmesini açın ve aşağıdaki varlıkların değerlerini geçici bir dosyaya kopyalayın. Örnek uygulamanızı aşağıdaki bölümlerde yapılandırmak için bu değerleri kullanırsınız.

    - **Uygulama (istemci) kimliği**
    - **Dizin (kiracı) kimliği**

    [![Azure Active Directory uygulama kimliği](./media/digital-twins-permissions/aad-app-reg-app-id.png)](./media/digital-twins-permissions/aad-app-reg-app-id.png#lightbox)

1. Uygulama kaydınız için **API izinbölmesini** açın. + İzin düğmesi **ekle'yi** seçin. İstek **API izinleri** bölmesinde, **kuruluşumun kullandığı API'leri** seçin ve ardından aşağıdakilerden birini arayın:
    
    1. `Azure Digital Twins`. Azure **Digital Twins** API'sını seçin.

        [![API veya Azure Dijital İkizler'de arama yapın](./media/digital-twins-permissions/aad-aap-search-api-dt.png)](./media/digital-twins-permissions/aad-aap-search-api-dt.png#lightbox)

    1. Alternatif olarak, `Azure Smart Spaces Service`arama . Azure **Akıllı Alanlar Hizmet** API'sini seçin.

        [![Azure Akıllı Alanlar için API'de arama](./media/digital-twins-permissions/aad-app-search-api.png)](./media/digital-twins-permissions/aad-app-search-api.png#lightbox)

    > [!IMPORTANT]
    > Görünecek Azure AD API adı ve kimliği kiracınıza bağlıdır:
    > * Test kiracı ve müşteri `Azure Digital Twins`hesapları aramak gerekir.
    > * Diğer Microsoft hesapları `Azure Smart Spaces Service`aramak gerekir.

1. API, seçildikten sonra aynı **İstek API izinleri** bölmesinde **Azure Dijital İkizler** olarak görünür. **Açılan** alt okuma seçeneğini seçin ve ardından **Oku.Yaz** onay kutusunu seçin. İzin **Ekle** düğmesini seçin.

    [![API izinleri ekleme](./media/digital-twins-permissions/aad-app-req-permissions.png)](./media/digital-twins-permissions/aad-app-req-permissions.png#lightbox)

1. Kuruluşunuzun ayarlarına bağlı olarak, yöneticinin bu API'ye erişmesini sağlamak için ek adımlar atmanız gerekebilir. Daha fazla bilgi için yöneticinize başvurun. Yönetici erişimi onaylandıktan sonra, **API izinleri** bölmesindeki **Yönetici Onayı Gerekli** sütuniziniz görüntülenir. 

    [![Yönetici onayı onayı](./media/digital-twins-permissions/aad-app-admin-consent.png)](./media/digital-twins-permissions/aad-app-admin-consent.png#lightbox)

    Azure **Dijital İkizlerinin** göründüğünü doğrulayın.
