---
title: Müşteri Katılımı için Dynamics 365 için müşteri yönetimi | Azure Marketi
description: Müşteri Katılımı için Dynamics 365 için müşteri yönetimi yapılandırın.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: dsindona
ms.openlocfilehash: 5b3e35b6d19905e3c5262dfea3e52511510c9ffe
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81252971"
---
# <a name="configure-lead-management-for-dynamics-365-for-customer-engagement"></a>Müşteri Katılımı için Dynamics 365 için müşteri yönetimi yapılandırın

Bu makalede, Müşteri Katılımı için Dynamics 365'in (önceden Dynamics CRM Online) nasıl ayarlanabileceği açıklanmaktadır, pazar teklifinizden satış müşteri adaylarını işlemek için [buradaki](https://docs.microsoft.com/dynamics365/customerengagement/on-premises/admin/on-prem-server-based-sharepoint-online) değişiklik hakkında daha fazla bilgi edinin. 

>[!Note]
>Bu yönergeler, Müşteri Etkileşimi ortamı için Microsoft barındırılan bulut Dynamics 365'e özeldir. Dinamikler ön hazırlık ortamına doğrudan bağlanmak şu anda desteklenmemektedir, müşteri adaylarını almak için [bir https bitiş noktası](./commercial-marketplace-lead-management-instructions-https.md) veya Azure [tablosu](./commercial-marketplace-lead-management-instructions-azure-table.md) yapılandırma gibi müşteri adaylarını almanız için başka seçenekler de vardır.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlamak için aşağıdaki kullanıcı izinleri gereklidir:

* Bir çözüm yükleyebilmek ve bu yönergeleri izleyebilmek için Müşteri Katılımı örneğinde Dynamics 365'te yönetici olmanız gerekir.
* Pazar tekliflerinden müşteri adayları göndermek için kullanılan müşteri adayı hizmeti için yeni bir hizmet hesabı oluşturmak için kiracı yönetici olmanız gerekir.
* Office 365 yönetici portalına erişiminiz gerekir.
* Azure portalına erişiminiz olması gerekir.

## <a name="install-the-solution"></a>Çözümü yükleyin

1.  Microsoft [Marketplace Lead Writer çözümlerini](https://mpsapiprodwus.blob.core.windows.net/documentation/MicrosoftMarketplacesLeadIntegrationSolution_1_0_0_0_target_CRM_6.1_managed.zip) indirin ve yerel olarak bilgisayarınıza kaydedin.

2.  Dynamics örneğinizin URL'sine gezinerek Müşteri Katılımı için Dynamics `https://tenant.crm.dynamics.com`365'i açın (örneğin).

3.  Üstteki gezinme çubuğunda vites simgesini ve **Gelişmiş Ayarlar'ı** seçerek Ayarlar'a erişin.
 
    ![Dinamikler - Gelişmiş Ayarlar](./media/commercial-marketplace-lead-management-instructions-dynamics/dynamics-advanced-settings.png)

4.  Ayarlar sayfasına girince, üstteki gezinme çubuğundan Ayarla menüsüne erişin ve **Çözümler'i**seçin.

    >[!Note]
    >Bir sonraki ekran yakalamadaki seçenekleri görmüyorsanız, devam etmek için gereken izinlere sahip değilsiniz demektir. Müşteri Katılımı örneği için Dynamics 365'inizde bir yöneticiye ulaşın.

    ![Dynamics 365 - Çözümler](./media/commercial-marketplace-lead-management-instructions-dynamics/dynamics-solutions.png)

5. Çözümler sayfasına **girince, Alma'yı** seçin ve adım 1'de indirdiğiniz *Microsoft Marketi Müşteri Adayı Yazar* çözümünü kaydettiğiniz yere gidin.

    ![Müşteri Katılımı için Dynamics 365 - İthalat](./media/commercial-marketplace-lead-management-instructions-dynamics/dynamics-crm-import.png)

6. İçe Alma çözüm sihirbazını izleyerek çözümü alma işlemi tamamlayın.

## <a name="configure-user-permissions"></a>Kullanıcı izinlerini yapılandırma

Müşteri Katılımı örneği için Dynamics 365'inize müşteri adayları yazmak için, bir hizmet hesabını bizimle paylaşmanız ve hesap için izinleri yapılandırmanız gerekir.

Hizmet hesabı oluşturmak ve izinleri atamak için aşağıdaki adımları kullanın. **Azure Etkin Dizin** veya **Office 365'i**kullanabilirsiniz.

>[!Note]
>Seçtiğiniz kimlik doğrulama seçeneğine bağlı olarak, seçiminizi temel alarak ilgili yönergeleri atlayabilirsiniz. Bkz. [Azure Etkin Dizini](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics#azure-active-directory) veya [Office 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics#office-365).

### <a name="azure-active-directory"></a>Azure Active Directory

Müşteri adayları almaya devam etmek için kullanıcı adınızı/parolanızı güncellemenize gerek kalmadan hiçbir zaman ek avantaj elde ettiğiniz için bu seçeneği öneririz. Azure Etkin Dizin seçeneğini kullanmak için Active Directory uygulamanızdan Uygulama Kimliği, Uygulama Anahtarı ve Dizin Kimliği'ni sağlarsınız.

Müşteri Etkileşimi için Azure Active Directory for Dynamics 365'i yapılandırmak için aşağıdaki adımları kullanın.

1. [Azure portalında](https://portal.azure.com/)oturum açın ve ardından sol gezintiden Azure Etkin Dizin hizmetini seçin.

2. Azure Etkin Dizini'nden **Özellikler'i** sol gezintiden seçin ve bu sayfadaki **Dizin Kimliği** değerini kopyalayın. Pazar yeri teklifiniz için müşteri adaylarını almak için yayımlama portalında sağlamanız gereken Dizin Kimliği değeri olduğundan, bu değeri *kaydedin.*

    ![Azure Etkin Dizini - Özellikler](./media/commercial-marketplace-lead-management-instructions-dynamics/aad-properties.png)

3. Azure Etkin Dizini'nden **Uygulama kayıtlarını** seçin ve ardından bu sayfada **Yeni kayıt'ı** seçin.
4. Uygulama adı için bir ad girin. Anlamlı bir uygulama adı sağlayın.
5. Desteklenen hesap türleri altında, **herhangi bir kuruluş dizininde Hesapları**seçin.
6. Redirect URI altında, **Web'i** seçin `https://contosoapp1/auth`ve bir URI sağlayın (örneğin). 
7. **Kaydol**’u seçin.

    ![Bir uygulamayı kaydetme](./media/commercial-marketplace-lead-management-instructions-dynamics/register-an-application.png)

8. Uygulamanız kayıtlı olduğuna göre, uygulamanın genel bakış sayfasına erişin ve o sayfadaki **Uygulama (istemci) kimlik** değerini kopyalayın. Pazar yeri teklifiniz için müşteri adayları almak için yayımlama portalında ve Dynamics'te sağlamanız gereken *Uygulama (istemci) kimlik* değeri olduğu için bu değeri kaydedin.

    ![Uygulama (istemci) kimliği](./media/commercial-marketplace-lead-management-instructions-dynamics/application-id.png)

9. Uygulamanın sol navigasyonundan **Sertifikalar ve sırlar** seçin ve bu sayfada **Yeni istemci sırrını** seçin. İstemci sırrı için anlamlı bir açıklama girin ve Sona Erdirme ler altında **Asla** seçeneğini seçin. İstemci sırrını oluşturmak için **Ekle'yi** seçin.

    ![Uygulama - Belgelendirme ve Sırlar](./media/commercial-marketplace-lead-management-instructions-dynamics/aad-certificates-secrets.png)

10. İstemci sırrı başarıyla oluşturulur oluşturulmaz, **istemci gizli değerini kopyalayın.** Sayfadan uzaklaştıktan sonra değeri alamazsınız. Pazar teklifiniz için müşteri adayları almak için yayımlama portalında sağlamanız gereken *Müşteri gizli* değeri olduğu için bu değeri kaydedin. 
11. Uygulamaların sol navigasyonundan **API izinlerini** seçin ve ardından **İzin ekle'yi**seçin.
12. Microsoft API'lerini seçin ve ardından API olarak **Dynamics CRM'yi** seçin.
13. *Uygulamanızın gerektirdiği izintürü*altında, Temsilci **li izinlerin seçildiğinden** emin olun. Kuruluş kullanıcıları **user_impersonation** *olarak Ortak Veri Hizmetine user_impersonation Erişim*İzni'ni denetleyin. **İzin Ekle'yi**seçin.

    ![İzin ekleme](./media/commercial-marketplace-lead-management-instructions-dynamics/api-permissions.png)

14. Azure portalında 1-13 adımlarını tamamladıktan sonra, URL'de gezinerek (örneğin) `https://tenant.crm.dynamics.com`Müşteri Katılımı için Dynamics 365'e gidin.
15. Vites simgesini ve üst teki gezinme çubuğundaki **Gelişmiş Ayarlar'ı** seçerek Ayarlar'a erişin.
16. Ayarlar sayfasına girince, üstteki gezinti çubuğundan Ayarla menüsüne erişin ve **Güvenlik'i**seçin.
17. Güvenlik sayfasına **girdiğinde, Kullanıcılar'ı**seçin.  Kullanıcılar sayfasında, **Uygulama Kullanıcıları'na**geçmek için "Etkin Kullanıcılar" açılır sayfasını seçin.
18. Yeni bir kullanıcı oluşturmak için **Yeni'yi** seçin. 

    ![Yeni kullanıcı oluşturma](./media/commercial-marketplace-lead-management-instructions-dynamics/application-users.png)

19. **Yeni**Kullanıcı'da, KULLANICI: UYGULAMA KULLANICIsI seçildiğinden emin olun. Bu bağlantıda kullanmak istediğiniz kullanıcı için bir kullanıcı adı, tam ad ve e-posta adresi sağlayın. Ayrıca, Azure portalında oluşturduğunuz uygulamanın **Uygulama Kimliği'ne** adım 8'den yapıştırın. Kullanıcıeklemeyi tamamlamak için **Kaydet ve Kapat'ı** seçin.

    ![Yeni kullanıcı](./media/commercial-marketplace-lead-management-instructions-dynamics/new-user-info.png)

20. Bu kullanıcının bağlantısını yapılandırmayı bitirmek için bu makaledeki "Güvenlik ayarları"na gidin.

### <a name="office-365"></a>Office 365

Azure Active Directory'yi kullanmak istemiyorsanız, *Microsoft 365 yönetici merkezine*yeni bir kullanıcı kaydedebilirsiniz. Müşteri adayları almaya devam etmek için kullanıcı adınızı/parolanızı her 90 günde bir güncellemeniz gerekir.

Müşteri Etkileşimi için Dynamics 365 için Office 365'i yapılandırmak için aşağıdaki adımları kullanın.

1. [Microsoft 365 yönetim merkezinde](https://admin.microsoft.com) oturum açın.

2. **Kullanıcı ekle'yi**seçin.

    ![Microsoft 365 yönetici merkezi - kullanıcı eklemek](./media/commercial-marketplace-lead-management-instructions-dynamics/ms-365-add-user.png)

4. Baş yazar hizmeti için yeni bir kullanıcı oluşturun. Aşağıdaki ayarları yapılandırın:

    * Kullanıcı adı sağlama
    * Parola sağlayın ve "Bu kullanıcının ilk oturum açınca parolasını değiştirmesini sağlayın" seçeneğini belirleyin.
    * Kullanıcının rolü olarak "Kullanıcı (yönetici erişimi yok)" seçeneğini belirleyin.
    * Bir sonraki ekran yakalamada gösterilen ürün lisansı olarak "Dynamics 365 Müşteri Katılımı planı"nı seçin. Seçtiğiniz lisans için ücretlendirilirsiniz. 

Pazar yeri teklifiniz için müşteri adaylarını almak için yayımlama portalında sağlamanız gereken *Kullanıcı Adı ve Parola* değerleri olarak bu değerleri kaydedin.

![Microsoft 365 yönetici merkezi - yeni kullanıcı](./media/commercial-marketplace-lead-management-instructions-dynamics/ms-365-new-user.png)

## <a name="security-settings"></a>Güvenlik ayarları

Son adım, oluşturduğunuz Kullanıcının müşteri adaylarını yazmasını sağlamaktır.

1. Dynamics örneğinizin URL'sine gezinerek Müşteri Katılımı için Dynamics `https://tenant.crm.dynamics.com`365'i açın (örneğin).
2. Üstteki gezinme çubuğunda vites simgesini ve **Gelişmiş Ayarlar'ı** seçerek Ayarlar'a erişin.
3. Ayarlar sayfasına girince, üstteki gezinti çubuğundan Ayarla menüsüne erişin ve **Güvenlik'i**seçin.
4. Güvenlik sayfasına **girince, Kullanıcılar'ı** seçin ve bu belgenin Kullanıcı İzinlerini Yapılandır bölümünde oluşturduğunuz kullanıcıyı seçin ve ardından **Rolleri Yönet'i**seçin. 

    ![Rolleri yönetme](./media/commercial-marketplace-lead-management-instructions-dynamics/security-manage-roles.png)

5. "Microsoft Marketplace Lead Writer" rol adını arayın ve kullanıcıya rolü atamak için seçin.

    ![Kullanıcı rollerini yönetme](./media/commercial-marketplace-lead-management-instructions-dynamics/security-manage-user-roles.png)

    >[!Note]
    >Bu rol, içe aktardığınız çözüm tarafından oluşturulur ve yalnızca müşteri adaylarını yazma ve uyumluluğu sağlamak için çözüm sürümünü izleme izinleri vardır.

6. Güvenlik sayfasına geri gidin ve **Güvenlik Rolleri'ni**seçin. "Microsoft Marketplace Lead Writer" rolünü arayın ve seçin.

    ![Güvenlik rolleri](./media/commercial-marketplace-lead-management-instructions-dynamics/security-roles.png)

7. Güvenlik rolünde bir kez, **Çekirdek Kayıtlar** sekmesini seçin. "Kullanıcı Varlık Ara Ayarları" varlığını arayın ve ilgili dairelerin her birine bir kez tıklayarak kullanıcıiçin Oluşturma, Okuma ve Yazma izinlerini (1/4 sarı daire) etkinleştirin.

    ![Microsoft Marketplace Baş Yazar - Core Records](./media/commercial-marketplace-lead-management-instructions-dynamics/marketplace-lead-writer.png)

8. Şimdi **Özelleştirme** sekmesine gidin. "Sistem İşi" varlığını arayın ve ilgili çevrelerin her birine dört kez tıklayarak bu varlık için Kuruluşa Okuma, Yazma ve Ekizin izinlerini (düz yeşil) sağlar.

    ![Microsoft Marketplace Baş Yazar - özelleştirme](./media/commercial-marketplace-lead-management-instructions-dynamics/marketplace-lead-writer-customization.png)

9. **Kaydet ve kapat.**

## <a name="configure-your-offer-to-send-leads-to-dynamics-365-for-customer-engagement"></a>Müşteri Katılımı için Dynamics 365 müşteri adayları göndermek için teklifinizi yapılandırın

Yayımlama portalında teklifiniz için müşteri adayı yönetimi bilgilerini yapılandırmaya hazır olduğunuzda aşağıdaki adımları izleyin:

1. **Teklifiniz** için Teklif kurulum sayfasına gidin.
2. Müşteri Adayı Yönetimi bölümü altında **Bağlan'ı** seçin.

    ![Müşteri Adayı Yönetimine Bağlan](./media/commercial-marketplace-lead-management-instructions-dynamics/connect-lead-management.png)

3. Bağlantı ayrıntıları açılır penceresinde, Müşteri **Etkileşimi için Dinamikler 365'i**

    ![Bağlantı ayrıntıları - varış noktası](./media/commercial-marketplace-lead-management-instructions-dynamics/connection-details-lead-destination.png)

4. Dynamics **365 Instance URL'sini** sağlayın. `https://contoso.crm4.dynamics.com`

5. **Kimlik Doğrulama**yöntemini, Azure Etkin Dizini'ni veya Office 365'i seçin. 
6. Azure Etkin Dizin'i seçtiyseniz, **Uygulama (istemci) kimliğini** (örnek: `23456052-aaaa-bbbb-8662-1234df56788f`), **Dizin Kimliği** (örnek: `12345678-8af1-4asf-1234-12234d01db47`) ve **İstemci sırrı** (örnek: ) `1234ABCDEDFRZ/G/FdY0aUABCEDcqhbLn/ST122345nBc=`sağlayın.

    ![Bağlantı ayrıntıları - Azure Active Directory](./media/commercial-marketplace-lead-management-instructions-dynamics/connection-details-application-id.png)

7. Office 365'i seçtiyseniz, **Kullanıcı** adını `contoso@contoso.onmicrosoft.com`(örnek: ) `P@ssw0rd`ve Parola 'yı (örnek: ).

    ![Bağlantı ayrıntıları - Kullanıcı adı](./media/commercial-marketplace-lead-management-instructions-dynamics/connection-details-authentication.png)

8. **Kişi e-postası** - Şirketinizde yeni bir müşteri adayı geldiğinde e-posta bildirimleri alması gereken kişiler için e-posta sağlayın. Birden çok e-postayı yarı kolon ile ayırarak sağlayabilirsiniz.
9. **Tamam'ı**seçin.

Bir müşteri adayı hedefine başarıyla bağlandığınızdan emin olmak için doğrulama düğmesini tıklatın. Başarılı olursa, müşteri adayı hedefinde bir test müşteri adayınız olur.

![Müşteri adayı yönetimi - bağlantı ayrıntıları depolama hesabı](./media/commercial-marketplace-lead-management-instructions-dynamics/dynamics-connection-details.png)

>[!Note]
>Teklifin geri kalanını yapılandırmayı bitirmeniz ve teklifiçin müşteri adayı alabilmek için yayımlamanız gerekir.
