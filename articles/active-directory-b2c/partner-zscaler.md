---
title: Zscaler ile Azure Active Directory B2C yapılandırma öğreticisi
titleSuffix: Azure AD B2C
description: Azure AD B2C kimlik doğrulamasını Zscaler ile tümleştirmeyi öğrenin
services: active-directory-b2c
author: gargi-sinha
manager: martinco
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 12/09/2020
ms.author: gasinh
ms.subservice: B2C
ms.openlocfilehash: ff51c2a71dfcaec580733a92e265628ac816e229
ms.sourcegitcommit: 5db975ced62cd095be587d99da01949222fc69a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97096179"
---
# <a name="tutorial-to-configure-zscaler-private-access-with-azure-active-directory-b2c-for-secure-hybrid-access"></a>Güvenli karma erişim için Azure Active Directory B2C Zscaler özel erişimini yapılandırma öğreticisi

Bu öğreticide, Azure AD B2C kimlik doğrulamasını [Zscaler özel erişimi (ZPA)](https://www.zscaler.com/products/zscaler-private-access)ile tümleştirmeyi öğrenin. ZPA, bir sanal özel ağın (VPN) maliyet, sorun veya güvenlik riskleri olmadan özel uygulamalara ve varlıklara ilke tabanlı, güvenli erişim sağlar. Zscaler 'un güvenli karma erişim teklifi, Azure AD B2C ile birleştirildiğinde tüketiciye yönelik uygulamalar için sıfır saldırı yüzeyi sunar.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için şunlar gerekir:

- Azure aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.

- Azure aboneliğinize bağlı [bir Azure AD B2C kiracısı](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant) .

- [ZPA aboneliği](https://azuremarketplace.microsoft.com/marketplace/apps/aad.zscalerprivateaccess?tab=Overview)

## <a name="scenario-description"></a>Senaryo açıklaması

ZPA tümleştirmesi aşağıdaki bileşenleri içerir:

- Azure AD B2C-kullanıcının kimlik bilgilerini doğrulamaktan sorumlu kimlik sağlayıcısı (IDP). Yeni bir kullanıcının kaydolmasından de sorumludur.

- ZPA-bir Web uygulamasının güvenliğini sağlamaya yönelik olarak, [sıfır-Trust erişimini](https://www.microsoft.com/security/blog/2018/12/17/zero-trust-part-1-identity-and-access-management/#:~:text=Azure%20Active%20Directory%20%28Azure%20AD%29%20provides%20the%20strong%2C,to%20express%20their%20access%20requirements%20in%20simple%20terms.)zorunlu hale getirerek sorumlu hizmet.

- Web uygulaması-kullanıcının erişmeye çalıştığı hizmeti barındırır.

Aşağıdaki diyagramda ZPA 'nin Azure AD B2C ile tümleştirilme şekli gösterilmektedir.

![Resim, Zscaler mimari diyagramını gösterir](media/partner-zscaler/zscaler-architecture-diagram.png)

|Adım | Description |
|:-----| :-----------|
| 1. | Kullanıcı bir ZPA Kullanıcı portalına veya ZPA tarayıcı erişimi uygulamasına ulaşır.
| 2. | ZPA, kullanıcının Web uygulamasına erişmesine izin verip vermeyeceğine karar vermeden önce Kullanıcı bağlamı bilgilerini gerektirir. ZPA kullanıcının kimliğini doğrulamak için Azure AD B2C oturum açma sayfasına bir SAML yönlendirmesi uygular.  
| 3. | Kullanıcı Azure AB B2C oturum açma sayfasına ulaştı. Yeni bir kullanıcı ise, Kullanıcı yeni bir hesap oluşturmak üzere kaydolur. Mevcut bir Kullanıcı mevcut kimlik bilgilerini kullanarak oturum açacaktı. Azure AD B2C kullanıcının kimliğini doğrular.
| 4. | Başarılı kimlik doğrulamasından sonra, Azure AD B2C, kullanıcıyı SAML onaylama işlemi ile birlikte ZPA 'ye yeniden yönlendirir. ZPA SAML onaylama işlemi doğrular ve kullanıcı bağlamını ayarlar.
| 5. | ZPA Kullanıcı için erişim ilkelerini değerlendirir. Kullanıcının Web uygulamasına erişmesine izin veriliyorsa, bağlantının geçmesine izin verilir.

## <a name="onboard-to-zpa"></a>ZPA 'ya ekleme

Bu öğreticide, ZPA 'nin çalışır durumdaki bir kurulumuna sahip olduğunuz varsayılır. ZPA 'yi kullanmaya başladıysanız, [ZPA için adım adım yapılandırma kılavuzu](https://help.zscaler.com/zpa/step-step-configuration-guide-zpa)' na bakın.

## <a name="integrate-with-azure-ad-b2c"></a>Azure AD B2C ile tümleştirme

### <a name="part-1---configure-azure-ad-b2c-as-an-idp-on-zpa"></a>Bölüm 1-ZPA üzerinde bir IDP olarak Azure AD B2C yapılandırma

Azure AD B2C, [ZPA üzerinde kimlik sağlayıcısı (IDP)](https://help.zscaler.com/zpa/configuring-idp-single-sign)olarak yapılandırmak için:

1. [ZPA yönetim portalında](https://admin.private.zscaler.com) oturum açın

2. **Yönetim**  >  **IDP yapılandırmasına** git

3. **IDP Yapılandırması Ekle** 'yi seçin

4. **1 IDP bilgileri** için aşağıdakileri girin:

   a. **Ad**: Azure AD B2C

   b. **Çoklu oturum açma**: Kullanıcı Seç

   c. **Etki alanları**: Bu IDP ile ilişkilendirmek istediğiniz kimlik doğrulama etki alanlarını seçin ve **bitti** ' yi seçin.

   ![Görüntü IDP bilgilerini gösterir](media/partner-zscaler/add-idp-configuration.png)

5. **İleri**’yi seçin

6. **2 SP meta verileri** için:

   a. Hizmet sağlayıcı URL 'sini kopyalayın ve daha sonra kullanmak üzere buraya göz önünde edin.

   b. Hizmet sağlayıcısı varlık KIMLIĞINI kopyalayın ve daha sonra kullanmak üzere notun.

   ![Görüntü, SP meta veri bilgilerini gösterir](media/partner-zscaler/sp-metadata.png)

7. **Duraklat** 'ı seçin

IDP yapılandırmasının geri kalanı Azure AD B2C yapılandırıldıktan sonra sürdürülecek.

### <a name="part-2---configure-custom-policy-in-azure-ad-b2c"></a>Bölüm 2-Azure AD B2C özel ilke yapılandırma

>[!Note]
>Bu adım yalnızca özel ilkeleriniz yoksa gereklidir. Zaten bir veya daha fazla özel ilkeleriniz varsa, bu adımı atlayabilirsiniz.

[Azure Active Directory B2C özel ilkelerle çalışmaya başlama](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started) makalesi, Azure AD B2C kiracınızda özel ilkelerin nasıl yapılandırılacağı hakkında yönergeler sağlar.

### <a name="part-3---register-zpa-as-a-saml-application-in-azure-ad-b2c"></a>3. kısım-ZPA 'yi Azure AD B2C SAML uygulaması olarak kaydetme

[Azure AD B2C BIR SAML uygulaması kaydetme](https://docs.microsoft.com/azure/active-directory-b2c/connect-with-saml-service-providers) makalesi, Azure AD B2C bir SAML uygulamasının nasıl yapılandırılacağı hakkında yönergeler sağlar. [Adım 3,2](https://docs.microsoft.com/azure/active-directory-b2c/connect-with-saml-service-providers#32-upload-and-test-your-policy-metadata)' de, Azure AD B2C tarafından kullanılan ıDP SAML meta veri URL 'si ile birlikte sağlanacaktır. Daha sonra kullanmak üzere buna ihtiyacınız olacak.

[Adım 4,2](https://docs.microsoft.com/azure/active-directory-b2c/connect-with-saml-service-providers#42-update-the-app-manifest)' ye kadar olan yönergeleri izleyin. Yönergenin adım 4,2 ' de, uygulama bildirimini güncelleştirmeniz gerekir. Özellikleri aşağıdaki gibi güncelleştirin:

- **ıdentifieruris**: **1. adım 6a**'Da BELIRTILEN hizmet sağlayıcısı varlık kimliğini kullanın.

- **Samlmetadataurl**: ZPA bir SAML meta veri URL 'si barındırmazsa bu özelliği atlayın.

- **Replyurlswithtype**: **1. adım 6b**'da belirtilen hizmet sağlayıcı URL 'sini kullanın.

- **LogoutURL**: ZPA bir oturum kapatma URL 'sini desteklemediğinden bu özelliği atlayın.

Adımların geri kalanı bu öğreticiyle ilgili değildir.

### <a name="part-4---extract-the-idp-saml-metadata-from-azure-ad-b2c"></a>4. Bölüm-IDP SAML meta verilerini Azure AD B2C Ayıkla

Önceki adımdan, aşağıdaki biçimde bir SAML meta veri URL 'SI edinmeniz gerekir:

```https://<tenant-name>.b2clogin.com/<tenant-name>.onmicrosoft.com/<policy-name>/Samlp/metadata```

Burada `<tenant-name>` Azure AD B2C kiracınızın adıdır ve `<policy-name>` son adımda oluşturduğunuz özel SAML ilkesinin adıdır.

Örneğin, https://safemarch.b2clogin.com/safemarch.onmicrosoft.com/B2C_1A_signup_signin_saml//Samlp/metadata

Bir Web tarayıcısı açın ve SAML meta veri URL 'sine gidin. Sayfa yüklendiğinde, sayfada herhangi bir yere sağ tıklayın. **Sayfayı farklı kaydet** ' i seçin ve dosyayı bilgisayarınıza kaydedin; Bunu bir sonraki bölümde kullanacaksınız.

### <a name="part-5---complete-idp-configuration-on-zpa"></a>5. Bölüm-ZPA 'de IDP yapılandırmasını tamamlanma

[ZPA yönetim portalında](https://help.zscaler.com/zpa/configuring-idp-single-sign) , 1. bölümde kısmen yapılandırılmış IDP yapılandırmasını doldurun:

1. [ZPA yönetim portalında](https://admin.private.zscaler.com) oturum açın

2. **Yönetim**  >  **IDP yapılandırmasına** gidin.

3. 1. Bölüm ' de yapılandırılan IDP 'yi seçin ve sonra da **özgeçmişi** seçin.

4. **3 Için IDP oluşturma**

   a. **IDP meta veri dosyasına** gidin  >  **Dosya** ' yı seçin ve Bölüm 4 ' te kaydettiğiniz meta veri dosyasını karşıya yükleyin.

   b. IDP **yapılandırması 'Nın** **etkin** olduğunu doğrulayın.

   c. **Kaydet**’i seçin.

      ![Görüntü IDP bilgilerini oluşturma](media/partner-zscaler/create-idp.png)

## <a name="test-the-solution"></a>Çözümü test etme

Bir ZPA Kullanıcı portalına veya tarayıcı erişim uygulamasına gidin ve kaydolma veya oturum açma sürecini test edin. Bu, başarılı bir SAML kimlik doğrulamasına neden olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makaleleri gözden geçirin:

- [Azure Active Directory B2C özel ilkeleri kullanmaya başlama](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started)

- [Azure AD B2C bir SAML uygulaması kaydetme](https://docs.microsoft.com/azure/active-directory-b2c/connect-with-saml-service-providers)

- [ZPA için adım adım yapılandırma kılavuzu](https://help.zscaler.com/zpa/step-step-configuration-guide-zpa)

- [IDP 'Yi çoklu oturum açma için yapılandırma](https://help.zscaler.com/zpa/configuring-idp-single-sign)
