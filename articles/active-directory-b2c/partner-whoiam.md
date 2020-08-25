---
title: Azure Active Directory B2C, Whoıam ile yapılandırma öğreticisi
titleSuffix: Azure AD B2C
description: Kullanıcı doğrulaması için Azure AD B2C kimlik doğrulamasını Whoıam ile tümleştirmeyi öğrenin
services: active-directory-b2c
author: gargi-sinha
manager: martinco
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 08/20/2020
ms.author: gasinh
ms.subservice: B2C
ms.openlocfilehash: 6545c5f40004dc50904618a8ea734eb73eeee933
ms.sourcegitcommit: d39f2cd3e0b917b351046112ef1b8dc240a47a4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88817823"
---
# <a name="tutorial-for-configuring-whoiam-with-azure-active-directory-b2c"></a>Azure Active Directory B2C ile Whoıam yapılandırma öğreticisi

Bu örnek öğreticide, ortamınızda [Whoıam](https://www.whoiam.ai/brims/) markalı kimlik yönetimi sisteminin (brims) nasıl yapılandırılacağı ve Azure AD B2C ile tümleştirilmesine yönelik yönergeler sağlıyoruz.

Whoıam 's BRIMS, ortamınızda dağıtılan bir uygulama ve hizmet kümesidir. Kullanıcı tabanınız için ses, SMS ve e-posta doğrulaması sağlar. BRIT, mevcut kimlik ve erişim yönetimi çözümünüzle birlikte çalışarak platform belirsiz.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için şunlar gerekir:

- Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.

- Azure aboneliğinize bağlı [bir Azure AD B2C kiracısı](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant) .

- Bir Whoıam [deneme hesabı](https://www.whoiam.ai/contact-us/)

## <a name="scenario-description"></a>Senaryo açıklaması

Whoıam tümleştirmesi aşağıdaki bileşenleri içerir:

- Azure AD B2C kiracı – yetkilendirme sunucusu, Kullanıcı kimlik bilgilerinin kiracıda tanımlı özel ilkelere göre doğrulanmasından sorumludur. Kimlik sağlayıcısı olarak da bilinir.

- İstemcileri ve bunların yapılandırmasını yönetmeye yönelik bir yönetim portalı

- Uç noktalar aracılığıyla çeşitli özellikler sunan bir API hizmeti  

- Hem BRIMS yönetim portalı hem de API hizmeti için arka uç olarak kullanılan Azure Cosmos DB

Aşağıdaki mimari diyagram uygulamayı gösterir.

![whoıam için ekran görüntüsü-mimari-diyagram](media/partner-whoiam/whoiam-architecture-diagram.png)

|Adım | Açıklama |
|:-----| :-----------|
| 1. | Kullanıcı bir oturum açma sayfasına ulaştı. Kullanıcı, kimlik sağlayıcısı olarak Azure AD B2C kullanan bir uygulamaya kaydolma veya oturum açma isteği başlatır.
| 2. | Kimlik doğrulamanın bir parçası olarak Kullanıcı, e-posta veya telefonunun sahipliğini doğrulamayı ister veya sesini bir biyometrik doğrulama faktörü olarak kullanır.  
| 3. | Azure AD B2C, kullanıcının e-posta adresi, telefon numarası ve bunların ses kayıtları üzerinden geçen BRIMS API hizmetine bir çağrı yapar
| 4. | BRIT, kullanıcının kendi dillerinde uygulamanın görünümü ve hissi ile etkileşimde bulunmak için tamamen özelleştirilebilir e-posta ve SMS şablonları gibi önceden tanımlanmış yapılandırma kullanır.
| 5. | Kullanıcının kimlik doğrulaması tamamlandıktan sonra, BRIMS doğrulamanın sonucunu belirten Azure AD B2C bir belirteç döndürür. Azure AD B2C, kullanıcıya uygulamaya erişim izni verir veya kimlik doğrulama denemelerini başarısız olur.  

## <a name="onboard-with-whoiam"></a>Whoıam ile ekleme

1. [Whoıam](https://www.whoiam.ai/contact-us/) ile iletişim kurun ve bır brims hesabı oluşturun.

2. Bir hesap oluşturulduktan sonra, kullanıma sunulan ekleme yönergelerini kullanın ve aşağıdaki Azure hizmetlerini yapılandırın:

    - [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) -posta hizmeti parolaları gibi güvenli parola depolaması için kullanılır.

    - [Azure App Service](https://azure.microsoft.com/services/app-service/) -BRIMS API ve yönetim portalı hizmetlerini barındırmak için kullanılır

    - [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) -yönetim portalı için yönetici kullanıcıların kimliğini doğrulamak için kullanılır

    - [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) -ayarları depolamak ve almak için kullanılır

    - [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview#:~:text=Application%20Insights%2C%20a%20feature%20of%20Azure%20Monitor%2C%20is,professionals.%20Use%20it%20to%20monitor%20your%20live%20applications.) (isteğe bağlı)-hem API hem de yönetici portalında oturum açmak için kullanılır

3. BRIMS API ve BRIMS yönetim portalını Azure ortamınızda dağıtın.

4. Azure AD B2C özel ilke örnekleri, BRIMS ekleme belgelerinizde mevcuttur. Uygulamanızı yapılandırmak ve Kullanıcı kimlik doğrulaması için BRIMS platformunu kullanmak için belgeyi izleyin.  

Whoıam 'in BRIMS hakkında daha fazla bilgi için bkz. [Ürün belgeleri](https://www.whoiam.ai/brims/)

## <a name="test-the-user-flow"></a>Kullanıcı akışını test etme

1. Azure AD B2C kiracıyı açın ve Ilkeler altında **kimlik deneyimi çerçevesi**' ni seçin.

2. Daha önce oluşturduğunuz **Signupsignın**' ı seçin.

3. **Kullanıcı akışını Çalıştır** ' ı seçin ve ayarları seçin:

   a. **Uygulama**: kayıtlı uygulamayı seçin (örnek JWT)

   b. **Yanıt URL 'si**: **yeniden yönlendirme URL** 'sini seçin

   c. **Kullanıcı akışını Çalıştır**' ı seçin.

4. Kaydolma akışına gidin ve hesap oluşturun

5. Kullanıcı özniteliği oluşturulduktan sonra BRIMS hizmeti akış sırasında çağrılacaktır. Akış tamamlanmamışsa, kullanıcının dizine kaydedilip kaydedilmediğini kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makaleleri gözden geçirin:

- [Azure AD B2C'deki özel ilkeler](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-overview)

- [Azure AD B2C özel ilkeleri kullanmaya başlama](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started?tabs=applications)
