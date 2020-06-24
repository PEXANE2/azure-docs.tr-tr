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
ms.openlocfilehash: 58e21e46edfe1d03d42bf2202bcf1f22282631a9
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "84872644"
---
# <a name="client-application-registration"></a>İstemci uygulama kaydı
Önceki öğreticide, FHıR için Azure API 'nizi dağıttığınızı ve ayarlamış olursunuz. Artık FHıR kurulumu için Azure API 'niz olduğuna göre, bir ortak istemci uygulaması kaydedebiliyoruz. Daha fazla ayrıntı veya sorun giderme için [genel bir istemci uygulaması](register-public-azure-ad-client-app.md) ile ilgili nasıl yapılır Kılavuzu ' nu okuyun, ancak aşağıda bu öğreticide önemli adımları adlandırdık.

1. Azure Active Directory git
1. **Uygulama kaydı**  -->  **Yeni kayıt** seçeneğini belirleyin
1. Uygulamanızı adlandırın ve yeniden yönlendirme URI 'sini ayarlayınhttps://www.getpostman.com/oauth2/callback


![İstemci uygulama kaydı](media/tutorial-web-app/reg-public-app.png)

## <a name="client-application-settings"></a>İstemci uygulaması ayarları
İstemci uygulamanız kaydedildikten sonra, genel bakış sayfasından uygulama (istemci) KIMLIĞINI kopyalayın. İstemciye erişirken bu değere daha sonra ihtiyacınız olacak.

![Uygulama KIMLIĞINI Kopyala](media/tutorial-web-app/app-id.png)

Ardından, doğru kimlik doğrulama seçeneklerini ayarlayın. Sol taraftaki **kimlik doğrulaması** ' nı seçin. **Erişim belirteci** ve **kimlik belirteci** kutularına bakın. Ayrıca, Bu öğreticinin dördüncü bölümünde Web uygulamanızı oluştururken hazırlık aşamasında yeniden yönlendirme URI 'sini de ayarlayabilirsiniz. Bunu yapmak için \< yeniden YÖNLENDIRME URI listesine https://Web-App-NAME>. azurewebsites.net ekleyin. [Web uygulamanızı yazdığınız](tutorial-web-app-write-web-app.md)adım sırasında farklı bir ad seçerseniz, bu seçeneği geri dönüp güncelleştirmeniz gerekir.

![Uygulama kimlik doğrulama ayarları](media/tutorial-web-app/app-authentication.png)

Doğru kimlik doğrulamasını kurdığınıza göre, API izinlerini ayarlayın. 
1. **API izinleri** ' ni seçin ve **izin Ekle** ' ye tıklayın.
1. **Kuruluşumun kullandığı API 'ler**altında Azure sağlık API 'leri için arama yapın
1. **User_impersonation** seçin ve **izin Ekle** ' ye tıklayın.

## <a name="next-steps"></a>Sonraki Adımlar
Artık ortak bir istemci uygulamanız var. Sonraki öğreticide, bu uygulamaya Postman aracılığıyla test ve erişim kazanıyoruz.

>[!div class="nextstepaction"]
>[Postman 'da istemci uygulamasını test etme](tutorial-web-app-test-postman.md)
