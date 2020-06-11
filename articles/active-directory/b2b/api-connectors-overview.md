---
title: Self Servis kaydolma akışlarında API bağlayıcılarını yönetme
description: Self Servis kaydolma Kullanıcı akışlarınızı özelleştirmek ve genişletmek için API bağlayıcılarını kullanın
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/20/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45bb22f5f6c15e326bed2524fbc541cbdec26a70
ms.sourcegitcommit: f01c2142af7e90679f4c6b60d03ea16b4abf1b97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84680164"
---
# <a name="use-api-connectors-to-customize-and-extend-self-service-sign-up"></a>Self Servis kaydolma 'yı özelleştirmek ve genişletmek için API bağlayıcılarını kullanma 

## <a name="overview"></a>Genel Bakış 
Geliştirici veya BT Yöneticisi olarak, Web API 'Lerini kullanarak [self servis kaydolma Kullanıcı](self-service-sign-up-overview.md) akışlarınızı dış sistemlerle BÜTÜNLEŞTIRMEK için API bağlayıcılarını kullanabilirsiniz. Örneğin, API bağlayıcılarını kullanarak şunları yapabilirsiniz:

- [**Özel bir onay iş akışlarıyla tümleştirin**](self-service-sign-up-add-approvals.md). Hesap oluşturmayı yönetmek için özel bir onay sistemine bağlanın.
<!-- - [**Perform identity proofing**](code-samples-self-service-sign-up.md#identity-proofing). Use an identity proofing and verification service to add an extra level of security to account creation decisions. -->
- **Kimlik doğrulama Işlemi gerçekleştirin**. Hesap oluşturma kararlarında ek bir güvenlik düzeyi eklemek için kimlik doğrulama ve doğrulama hizmeti kullanın.
- **Kullanıcı giriş verilerini doğrulayın**. Hatalı biçimlendirilmiş veya geçersiz kullanıcı verilerine karşı doğrulayın. Örneğin, bir dış veri deposundaki veya izin verilen değerler listesindeki mevcut verilere karşı Kullanıcı tarafından belirtilen verileri doğrulayabilirsiniz. Geçersiz ise, kullanıcıdan geçerli veri sağlamasını isteyebilir veya kullanıcının kaydolma akışına devam etmesini engelleyebilirsiniz.
- **Kullanıcı özniteliklerinin üzerine yaz**. Kullanıcıdan toplanan bir özniteliğe yeniden biçimlendirin veya bir değer atayın. Örneğin, bir Kullanıcı ilk adı küçük harfle veya tüm büyük harflerde girerse, adı yalnızca ilk harfi büyük harfle biçimlendirebilirsiniz. 
<!-- - **Enrich user data**. Integrate with your external cloud systems that store user information to integrate them with the sign-up flow. For example, your API can receive the user's email address, query a CRM system, and return the user's loyalty number. Returned claims can be used to pre-fill form fields or return additional data in the application token.  -->
- **Özel iş mantığını çalıştırın**. Anında iletme bildirimleri göndermek, kurumsal veritabanlarını güncelleştirmek, izinleri yönetmek, veritabanlarını denetlemek ve başka özel eylemler gerçekleştirmek için bulut sistemlerinizdeki aşağı akış olaylarını tetikleyebilirsiniz.

API Bağlayıcısı, HTTP uç noktası, kimlik doğrulama, istek ve beklenen yanıtı tanımlayarak Azure AD ile API uç noktası arasındaki bir sözleşmeyi temsil eder. Bir API bağlayıcısını yapılandırdıktan sonra, Kullanıcı akışındaki belirli bir adım için etkinleştirebilirsiniz. Kullanıcı kaydolma akışındaki bu adıma ulaştığında, API Bağlayıcısı çağrılır ve bir HTTP POST isteği olarak çalışır ve seçilen talepler bir JSON gövdesinde anahtar-değer çiftleri olarak gönderilir. API yanıtı, Kullanıcı akışının yürütülmesini etkileyebilir. Örneğin, API yanıtı bir kullanıcının kaydolmasını engelleyebilir, kullanıcıdan bilgi yeniden girmelerini veya kullanıcı özniteliklerinin üzerine yazmasına ya da bu özniteliklerin üzerine yazılmasına neden olabilir.

## <a name="where-you-can-enable-an-api-connector-in-a-user-flow"></a>Bir Kullanıcı akışında bir API bağlayıcısını etkinleştirebileceğiniz durumlar

Bir API bağlayıcısını etkinleştirebileceğiniz bir Kullanıcı akışında iki konum vardır:

- Bir kimlik sağlayıcısıyla oturum açtıktan sonra
- Kullanıcı oluşturmadan önce

Her iki durumda da, API bağlayıcıları kaydolma sırasında çağrılır, oturum açma desteklenmez.

### <a name="after-signing-in-with-an-identity-provider"></a>Bir kimlik sağlayıcısıyla oturum açtıktan sonra

Kaydolma işleminde bu adımdaki bir API Bağlayıcısı, Kullanıcı kimlik sağlayıcısıyla (Google, Facebook, Azure AD) kimliğini doğruladıktan hemen sonra çağrılır. Bu adım, kullanıcı özniteliklerinin toplanması için kullanıcıya sunulan form olan ***öznitelik koleksiyonu sayfasından***önce gelir. Bu adımda etkinleştirebileceğiniz API Bağlayıcısı senaryolarının örnekleri aşağıda verilmiştir:

- Kullanıcının mevcut bir sistemde talepleri aramak için verdiği e-postayı veya federal kimliği kullanın. Mevcut sistemden bu talepleri döndürün, öznitelik koleksiyonu sayfasını önceden doldurabilir ve belirtece döndürmek için kullanılabilir hale getirin.
- Kullanıcının izin verme veya reddetme listesine dahil edilip edilmeyeceğini doğrulayın ve kaydolma akışına devam edip etmediğini denetleyin.

### <a name="before-creating-the-user"></a>Kullanıcı oluşturmadan önce

Kaydolma işleminde bu adımda bulunan bir API Bağlayıcısı, varsa öznitelik toplama sayfasından sonra çağrılır. Bu adım, Azure AD 'de bir kullanıcı hesabı oluşturulmadan önce her zaman çağrılır. Kayıt sırasında bu noktada etkinleştirebileceğiniz senaryoların örnekleri aşağıda verilmiştir:

- Kullanıcı giriş verilerini doğrulayın ve kullanıcıdan veri göndermesini isteyin.
- Kullanıcı tarafından girilen verileri temel alan bir kullanıcının kaydolup engellenme.
- Kimlik doğrulama işlemi gerçekleştirin.
- Kullanıcı hakkındaki mevcut veriler için dış sistemleri, uygulama belirtecine döndürmek veya Azure AD 'de depolamak üzere sorgulayın.

<!-- > [!IMPORTANT]
> If an invalid response is returned or another error occurs (for example, a network error), the user will be redirected to the app with the error re -->

## <a name="frequently-asked-questions-faq"></a>Sık sorulan sorular (SSS)

### <a name="how-do-i-integrate-with-an-existing-api-endpoint"></a>Nasıl yaparım? var olan bir API uç noktasıyla tümleştirsin mi?
[Azure işlevleri 'Ndeki http tetikleyicisini](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook-trigger?tabs=csharp) , diğer Web API 'lerini çağırmak ve çağırmak için basit bir yöntem olarak kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- [Bir Kullanıcı AKıŞıNA API Bağlayıcısı ekleme](self-service-sign-up-add-api-connector.md) hakkında bilgi edinin
- [Self servis kaydolma 'ya özel bir onay sistemi eklemeyi](self-service-sign-up-add-approvals.md) öğrenin
<!--#TODO: Make doc, link.-->