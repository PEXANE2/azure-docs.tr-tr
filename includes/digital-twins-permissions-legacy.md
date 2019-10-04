---
title: içerme dosyası
description: içerme dosyası
services: digital-twins
author: alinamstanciu
ms.service: digital-twins
ms.topic: include
ms.date: 08/16/2019
ms.author: alinast
ms.custom: include file
ms.openlocfilehash: fcb4b3d54e1e62a7f197f2f499e2b176bb707fd8
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71843821"
---
1. [Azure Portal](https://portal.azure.com)sol bölmeden **Azure Active Directory** açın ve ardından **Özellikler** bölmesini açın. **DIZIN kimliğini** geçici bir dosyaya kopyalayın. Bu değeri bir sonraki bölümde örnek bir uygulama yapılandırmak için kullanacaksınız.

    [![Azure Active Directory dizin KIMLIĞI](./media/digital-twins-permissions-legacy/aad-app-reg-tenant.png)](./media/digital-twins-permissions-legacy/aad-app-reg-tenant.png#lightbox)

1. [Azure Portal](https://portal.azure.com)sol bölmeden **Azure Active Directory** açın ve sonra **uygulama kayıtları (eski)** bölmesini açın. **Yeni uygulama kaydı** düğmesini seçin.

1. **Ad** kutusuna bu uygulama kaydı için kolay bir ad verin. **Uygulama türünü** **Yerel**olarak seçin ve URI 'yi `https://microsoft.com` olarak **yeniden yönlendirin** . **Oluştur**' u seçin.

    [![ pencere oluştur](./media/digital-twins-permissions-legacy/aad-app-reg-create.png)](./media/digital-twins-permissions-legacy/aad-app-reg-create.png#lightbox)

1.  Kayıtlı uygulamayı açın ve **uygulama kimliği** alanının değerini geçici bir dosyaya kopyalayın. Bu değer Azure Active Directory uygulamanızı tanımlar. Uygulama KIMLIĞINI aşağıdaki bölümlerde örnek uygulamanızı yapılandırmak için kullanacaksınız.

    [![Azure Active Directory uygulama KIMLIĞI](./media/digital-twins-permissions-legacy/aad-app-reg-app-id.png)](./media/digital-twins-permissions-legacy/aad-app-reg-app-id.png#lightbox)

1. Uygulama kayıt bölmenizi açın. @No__t **ayarları**-1**gerekli izinler**' i seçin ve ardından:

   a. Sol üstteki **Ekle** ' yı seçerek **API erişimi ekle** bölmesini açın.

   b. **BIR API seçin** ' i seçin ve **Azure dijital TWINS**'i arayın. Aramanız API 'YI bulamıyorsa, bunun yerine **Azure akıllı boşluklar** ' ı arayın.

   ,. **Azure dijital TWINS (Azure akıllı boşluklar hizmeti)** seçeneğini belirleyin ve **Seç**' i seçin.

   TID. **Izinleri Seç ' i**seçin. **Okuma/yazma erişimi** temsilcisi izinleri onay kutusunu seçin ve **Seç**' i seçin.

   a. **API erişimi ekle** bölmesinde **bitti** ' yi seçin.

   vadeli. **Gerekli izinler** bölmesinde, **izin ver** düğmesini seçin ve görüntülenen bildirimi kabul edin. Bu API için izin verilmezse yöneticinize başvurun.

      [![Gerekli izinler bölmesi](./media/digital-twins-permissions-legacy/aad-app-req-permissions.png)](./media/digital-twins-permissions-legacy/aad-app-req-permissions.png#lightbox)

 
