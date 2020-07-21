---
title: Web uygulaması öğreticisi-Istemci uygulama kurulumu
description: Bu öğretici, bir Web uygulamasını dağıtmaya hazırlanma için ortak bir uygulamayı kaydetme adımlarında size yol gösterir
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: mihansen
ms.author: cavoeg
author: caitlinv39
ms.date: 01/03/2020
ms.openlocfilehash: 8414a84190659ff31596bc202d29fe45eefdc588
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86536716"
---
# <a name="client-application-registration"></a>İstemci uygulama kaydı
Önceki öğreticide, FHıR için Azure API 'nizi dağıttığınızı ve ayarlamış olursunuz. Artık FHıR kurulumu için Azure API 'niz olduğuna göre, bir ortak istemci uygulaması kaydedebiliyoruz. Daha fazla ayrıntı veya sorun giderme için [genel bir istemci uygulaması](register-public-azure-ad-client-app.md) ile ilgili nasıl yapılır Kılavuzu ' nu okuyun, ancak aşağıda bu öğreticide önemli adımları adlandırdık.

1. Azure Active Directory git
1. **Uygulama kaydı**  -->  **Yeni kayıt** seçeneğini belirleyin
1. Uygulamanızı adlandırın ve yeniden yönlendirme URI 'sini ayarlayınhttps://www.getpostman.com/oauth2/callback

   :::image type="content" source="media/tutorial-web-app/register-public-app.png" alt-text="Uygulama kaydetme bölmesinin ekran görüntüsü ve örnek uygulama adı ve yönlendirme URL 'SI.":::

## <a name="client-application-settings"></a>İstemci uygulaması ayarları

İstemci uygulamanız kaydedildikten sonra, genel bakış sayfasından uygulama (istemci) KIMLIĞI ve kiracı KIMLIĞI ' ni kopyalayın. İstemciye erişirken bu iki değere daha sonra ihtiyacınız olacak.

:::image type="content" source="media/tutorial-web-app/client-id-tenant-id.png" alt-text="Uygulama ve Dizin kimlikleri vurgulanmış şekilde istemci uygulama ayarları bölmesinin ekran görüntüsü.":::

### <a name="connect-with-web-app"></a>Web uygulamasıyla bağlantı

[Web uygulamanızı](tutorial-web-app-write-web-app.md) fhır için Azure API 'sine bağlamak üzere yazdıysanız, doğru kimlik doğrulama seçeneklerini de ayarlamanız gerekir. 

1. Sol taraftaki menüde, **Yönet**altında **kimlik doğrulaması**' nı seçin. 

1. Yeni bir platform yapılandırması eklemek için **Web**' i seçin.

1. Bu öğreticinin dördüncü bölümünde Web uygulamanızı oluştururken hazırlık aşamasında yeniden yönlendirme URI 'sini ayarlayın. Bunu yapmak için `https://\<WEB-APP-NAME>.azurewebsites.net` yeniden YÖNLENDIRME URI listesine ekleyin. [Web uygulamanızı yazdığınız](tutorial-web-app-write-web-app.md)adım sırasında farklı bir ad seçerseniz, bu seçeneği geri dönüp güncelleştirmeniz gerekir.

1. **Erişim belirteci** ve **kimlik belirteci** onay kutularını seçin.

   :::image type="content" source="media/tutorial-web-app/web-app-authentication.png" alt-text="Uygulama kimlik doğrulama ayarları dikey penceresinin, vurgulanan bir platform ekleme adımlarıyla ilgili ekran görüntüsü.":::

## <a name="add-api-permissions"></a>API izinleri ekleme

Doğru kimlik doğrulamasını kurdığınıza göre, API izinlerini ayarlayın:

1. **API izinleri** ' ni seçin ve **izin Ekle**' ye tıklayın.
1. **Kuruluşumun kullandığı API 'ler**altında Azure sağlık API 'leri için arama yapın.
1. **User_impersonation** seçin ve **izin Ekle**' ye tıklayın.

:::image type="content" source="media/tutorial-web-app/api-permissions.png" alt-text="API izinleri ekleme dikey penceresinin, API izinleri ekleme adımları ile ekran görüntüsü.":::

## <a name="next-steps"></a>Sonraki Adımlar
Artık ortak bir istemci uygulamanız var. Sonraki öğreticide, bu uygulamaya Postman aracılığıyla test ve erişim kazanıyoruz.

>[!div class="nextstepaction"]
>[Postman 'da istemci uygulamasını test etme](tutorial-web-app-test-postman.md)
