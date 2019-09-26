---
title: include dosyası
description: include dosyası
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: include
ms.date: 09/24/2019
ms.author: dkshir
ms.custom: include file
ms.openlocfilehash: 1fc30ea5aa843329b6227bfa564b3d10e2273cd7
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71310501"
---
>[!NOTE]
>Bu bölüm, [yeni Azure AD uygulama kaydı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app)için yönergeler sağlar. Hala eski bir yerel uygulama kaydınız varsa, desteklenmiş olduğu sürece bu dosyayı kullanabilirsiniz. Ek olarak, bazı nedenlerle uygulama kayıt defteri 'nin yeni yolu kurulumda çalışmadıysanız, eski bir yerel AAD uygulaması oluşturmayı deneyebilirsiniz. Daha fazla yönerge için [Azure dijital Twins uygulamanızı Azure Active Directory eski adıyla kaydet](../articles/digital-twins/how-to-use-legacy-aad.md) bölümünü okuyun. 

1. [Azure Portal](https://portal.azure.com)sol bölmeden **Azure Active Directory** açın ve **uygulama kayıtları** bölmesini açın. **Yeni kayıt** düğmesini seçin.

    [![Uygulama kaydedildi](./media/digital-twins-permissions/aad-app-register.png)](./media/digital-twins-permissions/aad-app-register.png#lightbox)

1. **Ad** kutusuna bu uygulama kaydı için kolay bir ad verin. **Yeniden yönlendirme URI 'si (isteğe bağlı)** bölümünde, sol taraftaki açılan kutuda **ortak istemci (mobil & Masaüstü)** öğesini seçin ve sağdaki metin kutusuna girin `https://microsoft.com` . **Kaydol**’u seçin.

    [![Bölme oluştur](./media/digital-twins-permissions/aad-app-reg-create.png)](./media/digital-twins-permissions/aad-app-reg-create.png#lightbox)

1. [Uygulamanın *yerel bir uygulama*olarak kaydedildiğinden](https://docs.microsoft.com/azure/active-directory/develop/scenario-desktop-app-registration)emin olmak için, uygulama kaydlarınızın **kimlik doğrulama** bölmesini açın ve bu bölmeyi aşağı kaydırın. **Varsayılan istemci türü** bölümünde, **uygulamayı ortak istemci olarak değerlendir**için **Evet** ' i seçin. 

    [![Varsayılan yerel](./media/digital-twins-permissions/aad-app-default-native.png)](./media/digital-twins-permissions/aad-app-default-native.png#lightbox)

1.  Kayıtlı uygulamanızın **genel bakış** bölmesini açın ve aşağıdaki varlıkların değerlerini geçici bir dosyaya kopyalayın. Aşağıdaki bölümlerde örnek uygulamanızı yapılandırmak için bu değerleri kullanacaksınız.

    - **Uygulama (istemci) KIMLIĞI**
    - **Dizin (kiracı) KIMLIĞI**

    [![Azure Active Directory uygulama KIMLIĞI](./media/digital-twins-permissions/aad-app-reg-app-id.png)](./media/digital-twins-permissions/aad-app-reg-app-id.png#lightbox)

1. Uygulama kaydınız için **API izinleri** bölmesini açın. **Izin Ekle** düğmesini seçin. **API Izinleri iste** bölmesinde **Kuruluşumun kullandığı API 'leri** seçin ve ardından şunu arayın:
    
    1. **Azure dijital TWINS**. **Azure dijital TWINS** API 'sini seçin.

        [![Arama API 'SI veya Azure dijital TWINS](./media/digital-twins-permissions/aad-aap-search-api-dt.png)](./media/digital-twins-permissions/aad-aap-search-api-dt.png#lightbox)

    1. Alternatif olarak, **Azure akıllı alanları**için arama yapın. **Azure akıllı boşluklar hizmeti** API 'sini seçin.

        [![Azure akıllı alanları için API arama](./media/digital-twins-permissions/aad-app-search-api.png)](./media/digital-twins-permissions/aad-app-search-api.png#lightbox)

    > [!NOTE]
    > Arama sırasında görüntülenecek tam ad, ait olduğunuz Azure kiracısına göre farklılık gösterebilir.

1. Seçilen API, aynı **istek API 'si izinleri** bölmesinde **Azure dijital TWINS** olarak gösterilir. **Oku (1)** açılır öğesini seçin ve ardından **oku. yazma** onay kutusunu seçin. **Izin Ekle** düğmesini seçin.

    [![API izinleri ekleme](./media/digital-twins-permissions/aad-app-req-permissions.png)](./media/digital-twins-permissions/aad-app-req-permissions.png#lightbox)

1. Kuruluşunuzun ayarlarına bağlı olarak, bu API 'ye yönetici erişimi sağlamak için ek adımlar gerçekleştirmeniz gerekebilir. Daha fazla bilgi için yöneticinize başvurun. Yönetici erişimi onaylandıktan sonra **API izinleri** BÖLMESINDEKI **yönetici onayı gerekli** sütunu, API 'leriniz için aşağıdakine benzer şekilde görünür:

    [![API izinleri ekleme](./media/digital-twins-permissions/aad-app-admin-consent.png)](./media/digital-twins-permissions/aad-app-admin-consent.png#lightbox)

