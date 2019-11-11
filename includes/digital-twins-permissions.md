---
title: include dosyası
description: include dosyası
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 09/30/2019
ms.custom: include file
ms.openlocfilehash: 3ad4a3391a8d0174a0a031920f40a47464753767
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2019
ms.locfileid: "73903865"
---
>[!NOTE]
>Bu bölüm, [yeni Azure AD uygulama kaydı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app)için yönergeler sağlar. Hala eski bir yerel uygulama kaydınız varsa, desteklenmiş olduğu sürece bu dosyayı kullanabilirsiniz. Ek olarak, bazı nedenlerle uygulama kayıt defteri 'nin yeni yolu kurulumda çalışmadıysanız, eski bir yerel AAD uygulaması oluşturmayı deneyebilirsiniz. Daha fazla yönerge için [Azure dijital Twins uygulamanızı Azure Active Directory eski adıyla kaydet](../articles/digital-twins/how-to-use-legacy-aad.md) bölümünü okuyun. 

1. [Azure Portal](https://portal.azure.com), Genişletilebilir sol menüden **Azure Active Directory** açın ve **uygulama kayıtları** bölmesini açın. **Yeni kayıt** düğmesini seçin.

    [![uygulama kaydedildi](./media/digital-twins-permissions/aad-app-register.png)](./media/digital-twins-permissions/aad-app-register.png#lightbox)

1. **Ad** kutusuna bu uygulama kaydı için kolay bir ad verin. **Yeniden yönlendirme URI 'si (isteğe bağlı)** bölümünde, sol taraftaki açılan menüden **ortak istemci/yerel (mobil & Masaüstü)** öğesini seçin ve sağdaki metin kutusuna `https://microsoft.com` girin. **Kaydol**’u seçin.

    [![oluştur bölmesi](./media/digital-twins-permissions/aad-app-reg-create.png)](./media/digital-twins-permissions/aad-app-reg-create.png#lightbox)

1. [Uygulamanın *yerel bir uygulama*olarak kaydedildiğinden](https://docs.microsoft.com/azure/active-directory/develop/scenario-desktop-app-registration)emin olmak için, uygulama kaydlarınızın **kimlik doğrulama** bölmesini açın ve bu bölmeyi aşağı kaydırın. **Varsayılan istemci türü** bölümünde, **uygulamayı ortak istemci olarak değerlendir**' **i seçin ve** **Kaydet**' i tıklayın.

    [![varsayılan yerel](./media/digital-twins-permissions/aad-app-default-native.png)](./media/digital-twins-permissions/aad-app-default-native.png#lightbox)

1.  Kayıtlı uygulamanızın **genel bakış** bölmesini açın ve aşağıdaki varlıkların değerlerini geçici bir dosyaya kopyalayın. Aşağıdaki bölümlerde örnek uygulamanızı yapılandırmak için bu değerleri kullanacaksınız.

    - **Uygulama (istemci) KIMLIĞI**
    - **Dizin (kiracı) KIMLIĞI**

    [![Azure Active Directory uygulama KIMLIĞI](./media/digital-twins-permissions/aad-app-reg-app-id.png)](./media/digital-twins-permissions/aad-app-reg-app-id.png#lightbox)

1. Uygulama kaydınız için **API izinleri** bölmesini açın. **Izin Ekle** düğmesini seçin. **API Izinleri iste** bölmesinde **Kuruluşumun kullandığı API 'leri** seçin ve ardından aşağıdakilerden birini arayın:
    
    1. `Azure Digital Twins`. **Azure dijital TWINS** API 'sini seçin.

        [![arama API 'SI veya Azure dijital TWINS](./media/digital-twins-permissions/aad-aap-search-api-dt.png)](./media/digital-twins-permissions/aad-aap-search-api-dt.png#lightbox)

    1. Alternatif olarak, `Azure Smart Spaces Service`için arama yapın. **Azure akıllı boşluklar hizmeti** API 'sini seçin.

        [Azure akıllı alanları için Search API ![](./media/digital-twins-permissions/aad-app-search-api.png)](./media/digital-twins-permissions/aad-app-search-api.png#lightbox)

    > [!IMPORTANT]
    > Görüntülenecek Azure AD API adı ve KIMLIĞI, kiracınıza bağlı olarak değişir:
    > * Test kiracının ve müşteri hesaplarının `Azure Digital Twins`araması gerekir.
    > * Diğer Microsoft hesaplarının `Azure Smart Spaces Service`araması gerekir.

1. Her iki API, aynı **istek API 'si izinleri** bölmesinde **Azure dijital TWINS** olarak gösterilir. **Oku** açılan seçeneğini belirleyin ve ardından **oku. yazma** onay kutusunu seçin. **Izin Ekle** düğmesini seçin.

    [![API izinleri ekleme](./media/digital-twins-permissions/aad-app-req-permissions.png)](./media/digital-twins-permissions/aad-app-req-permissions.png#lightbox)

1. Kuruluşunuzun ayarlarına bağlı olarak, bu API 'ye yönetici erişimi sağlamak için ek adımlar gerçekleştirmeniz gerekebilir. Daha fazla bilgi için yöneticinize başvurun. Yönetici erişimi onaylandıktan sonra **API izinleri** bölmesindeki **yönetici onayı gerekli** sütunu, API 'leriniz için aşağıdakine benzer şekilde görünür:

    [![API izinleri ekleme](./media/digital-twins-permissions/aad-app-admin-consent.png)](./media/digital-twins-permissions/aad-app-admin-consent.png#lightbox)

