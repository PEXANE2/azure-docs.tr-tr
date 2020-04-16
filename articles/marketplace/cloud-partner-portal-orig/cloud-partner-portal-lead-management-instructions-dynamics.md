---
title: Dinamikler CRM | Azure Marketi
description: Dynamics CRM için müşteri adayı yönetimini yapılandırın.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: dsindona
ms.openlocfilehash: d64c8460f5653f28b96396025f29ea13af15c8c3
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416316"
---
# <a name="configure-lead-management-for-dynamics-crm-online"></a>Dynamics CRM çevrimiçi için müşteri adayı yönetimini yapılandırma

Bu makalede, satış müşteri adaylarını işlemek için Dynamics CRM Online'ın nasıl ayarlanır.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlamak için aşağıdaki kullanıcı izinlerinin gerekli dir:
- Bir çözüm yüklemek için Dynamics CRM Online örneğinde yönetici olmanız gerekir.
- Müşteri adayı hizmeti için yeni bir hizmet hesabı oluşturmak için kiracı yönetici olmanız gerekir.

<a name="install-the-solution"></a>Çözümü yükleyin
--------------------

1.  Microsoft [Marketplace Lead Writer çözümlerini](https://mpsapiprodwus.blob.core.windows.net/documentation/MicrosoftMarketplacesLeadIntegrationSolution_1_0_0_0_target_CRM_6.1_managed.zip) indirin ve yerel olarak kaydedin.

2.  Dynamics CRM Online'ı açın ve Ayarlar'a gidin.
    >[!NOTE]
    >Bir sonraki ekran yakalamadaki seçenekleri görmüyorsanız, ihtiyacınız olan izinlere sahip değilsiniz demektir.
 
       ![Dinamikler kurulum görünümü](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline1.png)

3.  **İçe Aktar'ı**seçin ve ardından adım 1'de indirdiğiniz çözümü seçin.
 
    ![Dinamikleri alma seçeneği](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline2.png)

4.  Çözümü yüklemeyi bitirin.

## <a name="configure-user-permissions"></a>Kullanıcı izinlerini yapılandırma

Dynamics CRM örneğinize müşteri adayları yazmak için bir hizmet hesabını bizimle paylaşmanız ve hesap için izinleri yapılandırmanız gerekir.

Hizmet hesabı oluşturmak ve izinleri atamak için aşağıdaki adımları kullanın. **Azure Etkin Dizin** veya **Office 365'i**kullanabilirsiniz.

### <a name="azure-active-directory"></a>Azure Active Directory

Müşteri adayları almaya devam etmek için kullanıcı adınızı/parolanızı güncellemenize gerek kalmadan ek avantajlar elde ettiğiniz için bu seçeneği öneririz. Azure Active Directory seçeneğini kullanmak için Active Directory uygulamanızdan Uygulama Kimliği, Uygulama Anahtarı ve Dizin Kimliği'ni sağlarsınız.

Dinamikler CRM için Azure Etkin Dizini yapılandırmak için aşağıdaki adımları kullanın.

1.  [Azure portalında](https://portal.azure.com/) oturum açın ve ardından Azure Etkin Dizin hizmetini seçin.

2.  **Özellikler'i** seçin ve ardından **Dizin Kimliğini**kopyalayın. Bu, Bulut İş Ortağı Portalı'nda kullanmanız gereken kiracı hesap kimliğinizdir.

    ![Dizin Kimliği Alın](./media/cloud-partner-portal-lead-management-instructions-dynamics/directoryid.png)

3.  **Uygulama kayıtlarını**seçin ve ardından Yeni **uygulama kaydı**nı seçin.
4.  Uygulama adını girin.
5.  Türü **için, Web uygulaması / API**seçin.
6.  Bir URL sağlayın. Bu alan müşteri adayları için gerekli değildir, ancak bir uygulama oluşturmak için gereklidir.
7. **Oluştur**’u seçin.
8.  Başvurunuz kayıtlı olduğuna göre, **Özellikler'i** seçin ve ardından **Uygulama Kimliği'ni kopyala'yı**seçin. Bu bağlantı bilgilerini Bulut İş Ortağı Portalı'nda kullanırsınız.
9.  Özellikler'de, uygulamayı Çok kiracılı olarak ayarlayın ve ardından **Kaydet'i**seçin.

10. **Anahtarları** seçin ve Süre'nin süresi *nin hiç dolmasına göre*ayarlanmış yeni bir anahtar oluşturun. Anahtarı oluşturmak için **Kaydet'i** seçin. 
11. Keys menüsünde **anahtar değerini kopyala'yı seçin.** Bulut İş Ortağı Portalı için ihtiyacınız olacak olduğundan bu değerin bir kopyasını kaydedin.
    
    ![Dinamikler kayıtlı anahtar alır](./media/cloud-partner-portal-lead-management-instructions-dynamics/registerkeys.png)
    
12. **Gerekli izinleri** seçin ve sonra **Ekle'yi**seçin. 
13. **Dynamics CRM Online'ı** yeni API olarak seçin ve *kuruluş kullanıcıları olarak Access CRM Online*için izni denetleyin.

14. Dynamics CRM'de Kullanıcılar'a gidin ve **Uygulama Kullanıcıları'na**geçmek için "Etkin Kullanıcılar" açılır geçişini seçin.
    
    ![Uygulama kullanıcıları](./media/cloud-partner-portal-lead-management-instructions-dynamics/applicationuserfirst.PNG)

15. Yeni bir kullanıcı oluşturmak için **Yeni'yi** seçin. **KULLANICI'yi seçin: UYGULAMA KULLANICI** açılır.
    
    ![Yeni uygulama kullanıcısı ekleme](./media/cloud-partner-portal-lead-management-instructions-dynamics/applicationuser.PNG)

16. **Yeni**Kullanıcı'da, bu bağlantıyla kullanmak istediğiniz adı ve e-postayı sağlayın. Azure portalında oluşturduğunuz uygulama için **Uygulama Kimliği'ne** yapıştırın.

     ![Yeni kullanıcıyı yapılandırma](./media/cloud-partner-portal-lead-management-instructions-dynamics/leadgencreateuser.PNG)

17. Bu kullanıcının bağlantısını yapılandırmayı bitirmek için bu makaledeki "Güvenlik ayarları"na gidin.

### <a name="office-365"></a>Office 365

Azure Active Directory'yi kullanmak istemiyorsanız, *Microsoft 365 yönetici merkezine*yeni bir kullanıcı kaydedebilirsiniz. Müşteri adayları almaya devam etmek için kullanıcı adınızı/parolanızı her 90 günde bir güncellemeniz gerekir.

Dynamics CRM için Office 365'i yapılandırmak için aşağıdaki adımları kullanın.

1. [Microsoft 365 yönetim merkezinde](https://admin.microsoft.com) oturum açın.

2. **Yönetici** döşemesini seçin.

    ![Ofis Online Yönetici](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline3.png)

3. **Kullanıcı ekle'yi**seçin.

    ![Kullanıcı ekleme](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline4.png)

4. Baş yazar hizmeti için yeni bir kullanıcı oluşturun. Aşağıdaki ayarları yapılandırın:

    -   Parola sağlayın ve "Bu kullanıcının ilk oturum açınca parolasını değiştirmesini sağlayın" seçeneğini işaretleyin.
    -   Kullanıcının rolü olarak "Kullanıcı (yönetici erişimi yok)" seçeneğini belirleyin.
    -   Bir sonraki ekran yakalamada gösterilen ürün lisansını seçin. Seçtiğiniz lisans için ücretlendirilirsiniz. Çözüm ayrıca Dynamics CRM Online Basic lisansı ile de çalışacaktır.
    
    ![Kullanıcı izinlerini ve lisanslarını yapılandırma](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline5.png)

## <a name="security-settings"></a>Güvenlik ayarları

Son adım, oluşturduğunuz Kullanıcının müşteri adaylarını yazmasını sağlamaktır.

1.  Dynamics CRM çevrimiçi oturum açın.
2.  **Ayarlar'da** **Güvenlik'i**seçin.
    
    ![Güvenlik ayarları](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline6.png)

3.  **Kullanıcı izinlerinde**oluşturduğunuz kullanıcıyı seçin ve ardından **Kullanıcı Rollerini Yönet'i**seçin. Rolü atamak için **Microsoft Marketplace Baş Yazarı'nı** denetleyin.

    ![Kullanıcı rolünü atama](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline7.png)\

    >[!NOTE]
    >Bu rol, içe aktardığınız çözüm tarafından oluşturulur ve yalnızca müşteri adaylarını yazma ve uyumluluğu sağlamak için çözüm sürümünü izleme izinleri vardır.

4.  Güvenlik'te **Güvenlik Rolleri'ni** seçin ve Microsoft Market Idamı Baş Yazarı rolünü bulun.
    
    ![Güvenlik baş yazarını yapılandırma](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline10.jpg)\

5. Çekirdek **Kayıtları** sekmesini seçin. Kullanıcı Varlık Kullanıcı Arabirimi için Oluştur/Oku/Yaz'ı etkinleştir.

    ![Kullanıcı için oluşturma/okuma/yazma'yı etkinleştirme](./media/cloud-partner-portal-lead-management-instructions-dynamics/crmonline11.jpg)\

## <a name="wrap-up"></a>Sarın

Oluşturulan hesap bilgilerini Bulut İş Ortağı Portalı'na ekleyerek müşteri adayı yönetimi için Dynamics CRM yapılandırmayı bitirin. Örneğin:

-   **Azure Active Directory** - **Application Id** (örnek: *23456052-aaaa-bbbb-8662-1234df56788f ),* **Dizin Kimliği** (örnek: *12345678-8af1-4asf-- 1234-12234d01db47*), ve **Uygulama Anahtarı** (örnek: *1234ABCDEDFRZ/G/FdY0aUABCEDcqhbLn/ST122345nBc= ).*
-   **Office 365** - **Url** **`https://contoso.crm4.dynamics.com`**(örnek: ), **`contoso\@contoso.onmicrosoft.com`** Kullanıcı **Adı** (örnek: ), ve **Parola** (örnek: *P\@ssw0rd*).
