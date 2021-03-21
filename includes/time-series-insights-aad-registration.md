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
ms.date: 10/02/2020
ms.openlocfilehash: 0ce9575f078058c821ffffe1b9fe45eed5a4ad94
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101724186"
---
* [Platform ayarlarını yapılandırma](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app#configure-platform-settings) adım 4 ' te uygun platformu seçtikten sonra, arka planda **yeniden yönlendirme URI 'Leri** ve **erişim belirteçlerinizi** Kullanıcı arabiriminin sağına doğru yapılandırın.

    * **Yeniden yönlendirme URI 'leri** , kimlik doğrulama isteği tarafından sağlanan adresle eşleşmelidir:

        * Yerel bir geliştirme ortamında barındırılan uygulamalar için **ortak istemci (mobil & Masaüstü)** öğesini seçin. **Ortak Istemciyi** **Evet** olarak ayarladığınızdan emin olun.
        * Azure App Service barındırılan Single-Page uygulamalar için **Web**' i seçin.

    * **Oturum kapatma URL 'sinin** uygun olup olmadığını belirleme.

    * **Erişim belirteçlerini** veya **kimlik belirteçlerini** denetleyerek örtük izin akışını etkinleştirin.

    [![Yeniden yönlendirme URI 'Leri oluşturma](media/time-series-insights-aad-registration/active-directory-auth-redirect-uri.png)](media/time-series-insights-aad-registration/active-directory-auth-redirect-uri.png#lightbox)

    **Yapılandır**' a ve ardından **Kaydet**' e tıklayın.

* Azure Active Directory uygulamanızın Azure Time Series Insights ilişkilendirin. **API izinlerini** seçin  >    >  **Kuruluşumun kullandığı izin API 'leri** ekleyin.

    [![Azure Active Directory uygulamanızla bir API 'YI ilişkilendirme](media/time-series-insights-aad-registration/active-directory-app-api-permission.png)](media/time-series-insights-aad-registration/active-directory-app-api-permission.png#lightbox)

   `Azure Time Series Insights`Arama çubuğuna yazın ve ardından öğesini seçin `Azure Time Series Insights` .

* Ardından, uygulamanızın gerektirdiği tür API iznini belirtin. Varsayılan olarak, **temsilci izinleri** vurgulanacaktır. Bir izin türü seçin, sonra **Izin Ekle**' yi seçin.

    [![Uygulamanızın gerektirdiği API izninin türünü belirtin](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png)](media/time-series-insights-aad-registration/active-directory-app-permission-grant.png#lightbox)

* Uygulama ortamınızın API 'lerinizi kendisi olarak çağırmak için [kimlik bilgilerini ekleyin](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app#add-credentials) . Kimlik bilgileri, uygulamanızın çalışma zamanında Kullanıcı etkileşimi gerektirmeksizin kendisi olarak kimlik doğrulaması yapmasına olanak sağlar.