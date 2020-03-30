---
title: Referans Swagger belgeleri nasıl kullanılır - Azure Digital Twins | Microsoft Dokümanlar
description: Azure Digital Twins Swagger başvuru belgelerinin nasıl kullanılacağını anlama.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 02/03/2020
ms.custom: seodec18
ms.openlocfilehash: 27874e5731bd6fb9821e7aeda9333adbdbb79099
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77023302"
---
# <a name="azure-digital-twins-swagger-reference-documentation"></a>Azure Digital Twins Swagger başvuru belgeleri

Sağlanan her Azure Dijital İkizler örneği, kendi otomatik olarak oluşturulan Swagger başvuru belgelerini içerir.

[Swagger](https://swagger.io/), veya [OpenAPI](https://www.openapis.org/), karmaşık API bilgilerini etkileşimli ve dil-agnostik bir başvuru kaynağında birleştirir. Swagger, JSON'un bir API'ye karşı işlemleri gerçekleştirmek için kullanacağı kritik başvuru materyali, HTTP yöntemleri ve belirli uç noktaları sağlar.

## <a name="swagger-summary"></a>Swagger özeti

Swagger, API'nizin etkileşimli bir özetini sağlar:

* API ve nesne modeli bilgileri.
* Gerekli istek yüklerini, üstbilgilerini, parametreleri, bağlam yollarını ve HTTP yöntemlerini belirten REST API uç noktaları.
* API işlevlerinin sınantIsi.
* HTTP yanıtlarını doğrulamak ve onaylamak için kullanılan örnek yanıt bilgileri.
* Hata kodu bilgileri.

Swagger, Azure Dijital İkizler Yönetimi API'lerine yapılan geliştirme ve test çağrılarına yardımcı olmak için kullanışlı bir araçtır.

[!INCLUDE [Digital Twins Swagger](../../includes/digital-twins-swagger.md)]

## <a name="reference-material"></a>Referans materyali

Otomatik olarak oluşturulan Swagger başvuru malzemesi, geliştirme ve sınama lara yardımcı olmak için önemli kavramlara, kullanılabilir Yönetim API uç noktalarına ve her nesne modelinin açıklamasına hızlı bir genel bakış sağlar.

Kısa bir özet API açıklar.

[![Swagger özeti ve API genel bakış bilgileri](media/how-to-use-swagger/swagger-management-top-img.png)](media/how-to-use-swagger/swagger-management-top-img.png#lightbox)

Yönetim API nesne modelleri de listelenir.

[![Swagger UI'nin altında listelenen Swagger modelleri](media/how-to-use-swagger/swagger-management-models-img.png)](media/how-to-use-swagger/swagger-management-models-img.png#lightbox)

Anahtar özniteliklerin daha ayrıntılı bir özeti için listelenen her nesne modelini seçebilirsiniz.

[![Swagger modelleri modellerin içeriğini okumak için genişletilmiş](media/how-to-use-swagger/swagger-management-model-img.png)](media/how-to-use-swagger/swagger-management-model-img.png#lightbox)

Oluşturulan Swagger nesne modelleri, kullanılabilir tüm Azure Digital Twins [nesnelerini ve API'lerini](./concepts-objectmodel-spatialgraph.md)okumak için uygundur. Geliştiriciler, Azure Digital Twins'te çözüm oluştururken bu kaynağı kullanabilir.

## <a name="endpoint-summary"></a>Bitiş noktası özeti

Swagger ayrıca Yönetim API'lerini oluşturan tüm uç noktaların ayrıntılı bir özetini de sağlar.

Listelenen her bitiş noktası, aşağıdakiler gibi gerekli istek bilgilerini de içerir:

* Gerekli parametreler.
* Gerekli parametre veri türleri.
* KAYNAĞA erişmek için HTTP yöntemi.

[![Swagger UI'da görüntülenen Swagger uç noktaları](media/how-to-use-swagger/swagger-management-endpoints-img.png)](media/how-to-use-swagger/swagger-management-endpoints-img.png#lightbox)

Daha ayrıntılı bir genel bakış elde etmek için ek içeriklerini görüntülemek için her kaynağı seçin.

## <a name="use-swagger-to-test-endpoints"></a>Uç noktaları test etmek için Swagger'ı kullanma

Swagger'ın sağladığı güçlü işlevlerden biri, bir API bitiş noktasını doğrudan belgeleme Kullanıcı Arabirimi üzerinden test edebilme yeteneğidir.

Belirli bir bitiş noktası seçtikten sonra, **Try it out** düğmesi görüntülenir.

[![Swagger deneyin düğmesi](media/how-to-use-swagger/swagger-management-try-img.png)](media/how-to-use-swagger/swagger-management-try-img.png#lightbox)

Gerekli ve isteğe bağlı her parametre için giriş alanlarını getirmek için bu bölümü genişletin. Doğru değerleri girin ve **Execute**Yürüt'ün'u seçin.

[![Swagger Deneyin sonuç örneği](media/how-to-use-swagger/swagger-management-tried-img.png)](media/how-to-use-swagger/swagger-management-tried-img.png#lightbox)

Testi yürüttükten sonra yanıt verilerini doğrulayabilirsiniz.

## <a name="swagger-response-data"></a>Swagger yanıt verileri

Listelenen her bitiş noktası, geliştirme nizi ve testlerinizi doğrulamak için yanıt gövdesi verilerini de içerir. Bu örnekler, başarılı HTTP istekleri için durum kodlarını ve JSON'u içerir.

[![Swagger JSON yanıt örneği](media/how-to-use-swagger/swagger-management-response-img.png)](media/how-to-use-swagger/swagger-management-response-img.png#lightbox)

Örnekler, hata ayıklama veya başarısız testleri iyileştirmeye yardımcı olmak için hata kodları da içerir.

## <a name="swagger-oauth-20-authorization"></a>Swagger OAuth 2.0 yetkilendirme

> [!NOTE]
> * Azure Dijital İkizler kaynağını oluşturan kullanıcı ilkesi, Bir Uzay Yöneticisi rol atamasına sahip olacak ve diğer kullanıcılar için ek rol atamaları oluşturabilecektir. Bu kullanıcılar ve rolleri API'leri arama yetkisine sahiptir.

1. Azure Active Directory uygulaması oluşturmak ve yapılandırmak için [Quickstart'taki](quickstart-view-occupancy-dotnet.md#set-permissions-for-your-app) adımları izleyin. Alternatif olarak, varolan bir uygulama kaydını yeniden kullanabilirsiniz.

1. Aşağıdaki **Yeniden Yönlendirme URI'yi** Azure Active Directory uygulama kaydınıza ekleyin:

    [![Kayıt Swagger AAD url yönlendirme](media/how-to-use-swagger/swagger-aad-redirect-url-registration.png)](media/how-to-use-swagger/swagger-aad-redirect-url-registration.png#lightbox)

    ```plaintext
    https://YOUR_SWAGGER_URL/ui/oauth2-redirect-html
    ```
    | Adı  | Şununla değiştir | Örnek |
    |---------|---------|---------|
    | YOUR_SWAGGER_URL | Portalda bulunan Yönetim REST API dokümantasyon URL'si  | `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger` |

1. OAuth 2.0 örtülü hibe akışının kullanılmasına izin vermek için **Örtülü hibe** > **Erişim belirteçleri** onay kutusunu seçin. **Yapılaşı'yı**seçin, ardından **Kaydet'i**.

1. Azure Active Directory uygulamanızın **İstemci Kimliğini** kopyalayın.

Azure Active Directory kaydını tamamladıktan sonra:

1. Sayfanızdaki **Yetkilendirme** düğmesini seçin.

    [![Swagger yetkilendirme düğmesini seçin](media/how-to-use-swagger/swagger-select-authorize-btn.png)](media/how-to-use-swagger/swagger-select-authorize-btn.png#lightbox)

1. Uygulama kimliğini **client_id** alanına yapıştırın.

    [![Swagger client_id alanı](media/how-to-use-swagger/swagger-auth-form.png)](media/how-to-use-swagger/swagger-auth-form.png#lightbox)

1. Daha sonra aşağıdaki başarı modal yönlendirilecektir.

    [![Swagger yönlendirme modal](media/how-to-use-swagger/swagger-auth-redirect-img.png)](media/how-to-use-swagger/swagger-auth-redirect-img.png#lightbox)

OAuth 2.0 tarafından korunan etkileşimli test istekleri hakkında daha fazla bilgi edinmek için [resmi belgeleri](https://swagger.io/docs/specification/authentication/oauth2/)okuyun.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Digital Twins nesne modelleri ve uzamsal zeka grafiği hakkında daha fazla bilgi için [Azure Digital Twins nesne modellerini anla'yı](./concepts-objectmodel-spatialgraph.md)okuyun.

- Yönetim API'nizle kimlik doğrulaması yapmayı öğrenmek için [API'lerle Kimlik Doğrula'yı](./security-authenticating-apis.md)okuyun.
