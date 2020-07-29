---
title: Ticari Market 'te Azure Active Directory ve transactable SaaS teklifleri
description: Azure Active Directory, Microsoft Commercial Market 'teki transactable SaaS tekliflerinde nasıl çalıştığını öğrenin.
author: mingshen-ms
ms.author: mingshen
ms.reviewer: dannyevers
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/10/2020
ms.openlocfilehash: 0d8e2ee684bc08ec23e052229d50b7e9d62c0ecb
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87328499"
---
# <a name="azure-ad-and-transactable-saas-offers-in-the-commercial-marketplace"></a>Ticari Market 'te Azure AD ve transactable SaaS teklifleri

Microsoft bulut tabanlı kimlik ve erişim yönetimi hizmeti [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) (Azure AD), kullanıcıların oturum açıp iç ve dış kaynaklara erişmesine yardımcı olur. Microsoft ticari Market 'te, Azure AD transactable SaaS 'ın yayımcılar, alıcılar ve kullanıcılar dahil herkes için daha kolay ve daha güvenli olmasını sağlar. Yayımcılar, Azure AD ile kullanıcıların hizmet olarak yazılım (SaaS) uygulamaları için sağlama işlemini otomatikleştirebilir ve alıcıların kendileri bu sağlanan kullanıcıları yönetebilir. 

[Azure AD çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on) (SSO), KULLANıCıLARıN Azure AD 'de uygulamalarda oturum açarken güvenlik ve kolaylık sağlar. Daha hızlı katılım ve en iyi duruma getirilmiş deneyimler, bir yayımcının SaaS uygulamasıyla ilk etkileşiminden alıcı ve Kullanıcı güveni de sağlar. Bu, görünürlük oluşturan ve yinelenen iş etkileyen olumlu bir izlenim sağlar.

Bu makaledeki kılavuzunuzu izleyerek, SaaS teklifinizi ticari Market 'te sertifikalandırmaya yardımcı olacaksınız. Sertifika hakkında daha fazla ayrıntı için, [SaaS 'ye özgü](https://aka.ms/commercial-marketplace-certification-policies#1000-software-as-a-service-saas)olanlar da dahil olmak üzere ayrıntılı [ticari Market sertifika ilkelerini](https://aka.ms/commercial-marketplace-certification-policies#100-general)okuyun.

## <a name="before-you-begin"></a>Başlamadan önce

[SaaS teklifinizi](./partner-center-portal/create-new-saas-offer.md) Iş Ortağı Merkezi 'nde oluşturduğunuzda, teklif listesinde görüntülenecek olan eylem seçeneklerine yönelik belirli bir çağrı kümesinden seçim yapabilirsiniz. Seçiminiz, teklifinizin ticari Market 'te nasıl işlem yapıldığını belirler. Microsoft ile satılan tekliflere transactable teklifleri denir. Müşteriyi tüm transactable teklifleri için sizin adınıza faturalandırırız. Microsoft üzerinden satış yapın ve işlemleri sizin adınıza ( **Evet** seçeneği) barındırdıysanız, bir transactable teklifi oluşturmayı seçtiniz ve bu makale size yöneliktir. Bunu tamamen okumanızı öneririz.

Teklifinizi yalnızca ticari Market ve işlem işlemlerini bağımsız olarak ( **Hayır** seçeneği) seçerek, müşterilerin teklifinizi nasıl erişebileceği hakkında üç seçeneğiniz vardır: şimdi alın (ücretsiz), ücretsiz deneme ve benimle iletişim kurun. **Şimdi al (ücretsiz)** veya **ücretsiz deneme sürümünü**seçerseniz bu makale sizin için değildir. Bunun yerine, daha fazla bilgi için bkz. [ticari Market 'te ücretsiz veya deneme SaaS teklifinizin giriş sayfasını oluşturma](./azure-ad-free-or-trial-landing-page.md) . **Benimle Iletişim kurun**' i seçerseniz, doğrudan yayımcı sorumluluğu yoktur. Teklifinizi Iş ortağı merkezinde oluşturmaya devam edin.

## <a name="how-azure-ad-works-with-the-commercial-marketplace-for-saas-offers"></a>Azure AD, SaaS teklifleri için ticari Market ile nasıl kullanılır?

Azure AD, ticari Market çözümlerini sorunsuz bir şekilde satın alma, karşılama ve yönetme imkanı sunar. Şekil 1 ' de yayımcının, alıcının ve kullanıcının bir aboneliği satın alma ve etkinleştirme işlemlerinin nasıl etkileşim kurduğu gösterilmektedir. Ayrıca, müşterilerin ticari Market 'ten aldığı SaaS uygulamalarını nasıl kullandığını ve yönettikleri gösterilmektedir. Bu çizimin amaçları doğrultusunda, alıcı, ticari Market 'ten satın alma işlemini başlatan SaaS uygulaması kullanıcısına yöneliktir.

Şekil 1 ' de gösterildiği gibi, bir alıcı teklifinizin seçtiği zaman, satın alma, abonelik ve Kullanıcı yönetimi dahil iş akışlarının zincirini başlatılarlar. Bu zincir içinde, Microsoft, önemli noktalarda destek sunarak, yayımcı belirli gereksinimlerden sorumludur.

***Şekil 1: ticari Market 'te SaaS teklifleri için Azure AD kullanma***

:::image type="content" source="./media/azure-ad-saas/azure-ad-saas-flow.png" alt-text="Satın alma yönetimi, abonelik yönetimi ve isteğe bağlı kullanıcı yönetimi işlem adımlarını gösterir.":::

Aşağıdaki bölümler, her bir işlem adımının gereksinimleriyle ilgili ayrıntıları sağlar.

## <a name="process-steps-for-purchase-management"></a>Satın alma yönetimi için adımları işleme

Bu şekilde, satın alma yönetimi için dört işlem adımı gösterilmektedir.

:::image type="content" source="./media/azure-ad-saas/azure-ad-saas-flow-1-4.png" alt-text="Satın alma yönetimi için dört işlem adımını gösterir.":::

Bu tablo, satın alma yönetimi işlem adımlarının ayrıntılarını sağlar.

| İşlem adımı | Yayımcı eylemi | Yayımcılar için önerilen veya gerekli |
| ------------ | ------------- | ------------- |
| 1. alıcı, ticari Market 'te Azure KIMLIK kimliğiyle oturum açar ve bir SaaS teklifi seçer. | Yayımcı eylemi gerekli değildir. | Geçerli değil |
| 2. satın alma işleminden sonra, alıcı Azure Marketi 'nde **hesabı Yapılandır** ' ı seçer veya şimdi Appsource 'ta **yapılandırır** . Bu, alıcı bu teklifin yayımcı giriş sayfasına yönlendirir. Alıcı, yayımcının SaaS uygulamasında Azure AD SSO ile oturum açabiliyor ve yalnızca Azure AD yönetici onayı gerektirmeyen en düşük onay için sorulmalıdır. | Bir kullanıcıyı Azure AD veya Microsoft hesabı (MSA) kimliğiyle alacak ve gereken ek sağlama veya kurulumu kolaylaştıran teklif için bir [giriş sayfası](azure-ad-transactable-saas-landing-page.md) tasarlayın. | Gerekli |
| 3. Yayımcı, SaaS karşılama API 'sinden satın alma ayrıntılarını ister. | Giriş sayfasının uygulama KIMLIĞINDEN oluşturulan bir [erişim belirtecini](./partner-center-portal/pc-saas-registration.md) kullanarak, satın alma hakkındaki özellikleri almak için [Çözümle bitiş noktasını çağırın](./partner-center-portal/pc-saas-fulfillment-api-v2.md#resolve-a-purchased-subscription) . | Gerekli |
| 4. Azure AD ve Microsoft Graph API 'SI aracılığıyla Yayımcı, kuruluşun SaaS uygulamasında alıcı sağlamak için gereken şirket ve Kullanıcı ayrıntılarını toplar.  | Ad ve e-posta bulmak için Azure AD kullanıcı belirtecini oluşturun veya [MICROSOFT Graph API 'sini çağırın](https://docs.microsoft.com/graph/use-the-api) ve oturum açan kullanıcı hakkında [bilgi almak](https://docs.microsoft.com/graph/api/user-get) için temsilci izinleri kullanın. | Gerekli |
||||

## <a name="process-steps-for-subscription-management"></a>Abonelik yönetimi için işlem adımları

Bu şekilde, abonelik yönetimi için iki işlem adımı gösterilmektedir.

:::image type="content" source="./media/azure-ad-saas/azure-ad-saas-flow-5-6.png" alt-text="Abonelik yönetimi için iki işlem adımını gösterir.":::

Bu tabloda, abonelik yönetimi işlem adımlarıyla ilgili ayrıntılar açıklanmaktadır.

| İşlem adımı | Yayımcı eylemi | Yayımcılar için önerilen veya gerekli |
| ------------ | ------------- | ------------- |
| 5. yayımcı SaaS uygulamasına olan aboneliği SaaS karşılama API 'SI aracılığıyla yönetir. | [SaaS karşılama API 'leri](./partner-center-portal/pc-saas-fulfillment-api-v2.md)aracılığıyla abonelik değişikliklerini ve diğer yönetim görevlerini işleyin.<br><br>Bu adım, işlem adım 3 ' te açıklandığı şekilde bir erişim belirteci gerektirir. | Gerekli |
| 6. tarifeli fiyatlandırma kullanılırken, yayımcı kullanım olaylarını ölçüm hizmeti API 'sine yayar. | SaaS uygulamanız kullanım tabanlı faturalandırma özelliği ile, [Market ölçüm hizmeti API 'leri](./partner-center-portal/marketplace-metering-service-apis.md)aracılığıyla kullanım bildirimleri oluşturun.<br><br>Bu adım, adım 3 ' te açıklanan şekilde bir erişim belirteci gerektirir. | Ölçüm için gerekli |
||||

## <a name="process-steps-for-user-management"></a>Kullanıcı yönetimi için adımları işleme

Bu şekilde Kullanıcı yönetimi için üç işlem adımı gösterilmektedir.

:::image type="content" source="./media/azure-ad-saas/azure-ad-saas-flow-7-9.png" alt-text="Kullanıcı yönetimi için üç isteğe bağlı işlem adımını gösterir.":::

7 ila 9 arasındaki adımları isteğe bağlı kullanıcı yönetimi işlem adımlardır. Azure AD çoklu oturum açma (SSO) desteği sunan yayımcılar için ek avantajlar sağlar. Bu tabloda Kullanıcı yönetimi işlem adımlarıyla ilgili ayrıntılar açıklanmaktadır.

| İşlem adımı | Yayımcı eylemi | Yayımcılar için önerilen veya gerekli |
| ------------ | ------------- | ------------- |
| 7. alıcının şirketindeki Azure AD yöneticileri, Azure AD aracılığıyla kullanıcılar ve gruplar için isteğe bağlı olarak erişimi yönetebilir. | Kullanıcılar için Azure AD SSO ayarlandıysa, bunu etkinleştirmek için yayımcı eylemi gerekmez (adım 9). | Geçerli değil |
| 8. Azure AD sağlama hizmeti, Azure AD ile yayımcının SaaS uygulaması arasındaki değişikliklerle iletişim kurar. | Kullanıcılar eklendikçe ve kaldırıldığında Azure AD 'den güncelleştirmeleri almak için [BIR SCıM uç noktası uygulayın](https://docs.microsoft.com/azure/active-directory/app-provisioning/use-scim-to-provision-users-and-groups) . | Önerilen |
| 9. uygulama eklendikten ve sağlandıktan sonra, alıcı şirketinin kullanıcıları yayımcının SaaS uygulamasında oturum açmak için Azure AD SSO 'yu kullanabilir. | Kullanıcıların, yayımcının SaaS uygulamasına bir hesap ile bir kez oturum açmasını sağlamak için [Azure AD SSO 'Yu kullanın](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on) . | Önerilen |
||||

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Market 'te SaaS teklifi oluşturma](./partner-center-portal/create-new-saas-offer.md)
- [Ticari Market 'te transactable SaaS teklifinizin giriş sayfasını oluşturun](./azure-ad-transactable-saas-landing-page.md)
