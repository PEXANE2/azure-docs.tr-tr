---
title: 'Azure AD Connect: Aygıt geri yazmayı etkinleştirme | Microsoft Dokümanlar'
description: Bu belge, Azure AD Connect kullanarak aygıt geri yazmayı nasıl etkinleştireceklerini ayrıntılarıyla anlatıyor
services: active-directory
documentationcenter: ''
author: billmath
manager: femila
editor: curtand
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/08/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 632f6f80184c6ba3409bd30ae070cbaefc77f036
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67109493"
---
# <a name="azure-ad-connect-enabling-device-writeback"></a>Azure AD Bağlantısı: Aygıt geri yazmayı etkinleştirme
> [!NOTE]
> Aygıt geri yazımı için Azure AD Premium aboneliği gereklidir.
> 
> 

Aşağıdaki belgeler, Azure AD Connect'te aygıt geri yazma özelliğinin nasıl etkinleştirilen hakkında bilgi sağlar. Aygıt Writeback aşağıdaki senaryolarda kullanılır:

* [Karma sertifika güven dağıtımını kullanarak İşletmeler için Windows Hello'yu](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-cert-trust-prereqs#device-registration) etkinleştirme
* ADFS (2012 R2 veya üzeri) korumalı uygulamalara (parti güvenlerine bağlı) cihazlara dayalı Koşullu Erişimi etkinleştirin.

Bu, uygulamalara erişimin yalnızca güvenilen aygıtlara verildiğine dair ek güvenlik ve güvence sağlar. Koşullu Erişim hakkında daha fazla bilgi için bkz: [Koşullu Erişim ile Riski Yönetme](../active-directory-conditional-access-azure-portal.md) ve Azure Active [Directory Device Kaydı'nı kullanarak Şirket İçi Koşullu Erişimi Ayarlama.](../../active-directory/active-directory-device-registration-on-premises-setup.md)

> [!IMPORTANT]
> <li>Aygıtlar, kullanıcılarla aynı ormanda bulunmalıdır. Aygıtların tek bir ormana geri yazılması gerektiğinden, bu özellik şu anda birden çok kullanıcı ormanı olan bir dağıtımı desteklemez.</li>
> <li>Şirket içi Active Directory ormanına yalnızca bir aygıt kaydı yapılandırma nesnesi eklenebilir. Bu özellik, şirket içi Etkin Dizin'in birden çok Azure REKLAM diziniyle senkronize edildiği bir topolojiyle uyumlu değildir.</li>

## <a name="part-1-install-azure-ad-connect"></a>Bölüm 1: Azure AD Connect'i yükleme
Azure AD Connect'i Özel veya Express ayarlarını kullanarak yükleyin. Microsoft, aygıt geri yazmayı etkinleştirmeden önce tüm kullanıcılar ve grupların başarıyla eşitlenmiş olarak başlatılmasını önerir.

## <a name="part-2-enable-device-writeback-in-azure-ad-connect"></a>Bölüm 2: Azure AD Connect'te aygıt geri yazmayı etkinleştirme
1. Yükleme sihirbazını yeniden çalıştırın. Ek Görevler sayfasından **aygıt seçeneklerini yapılandır'ı** seçin ve **İleri'yi**tıklatın. 

    ![Aygıt seçeneklerini yapılandırma](./media/how-to-connect-device-writeback/deviceoptions.png)

    >[!NOTE]
    > Yeni Yapılandırma aygıtı seçenekleri yalnızca sürüm 1.1.819.0 ve daha yeni sürümlerde kullanılabilir.

2. Aygıt seçenekleri sayfasında, **aygıtı yeniden yapılandırma'yı**seçin. Aygıt **geri yüklemesi** etkinleştirilene kadar aygıt geri yazmayı devre dışı etme seçeneği kullanılamaz. Sihirbazın bir sonraki sayfasına geçmek için **İleri'yi** tıklatın.
    ![Aygıt işlemini seçti](./media/how-to-connect-device-writeback/configuredevicewriteback1.png)

3. Geri yazma sayfasında, sağlanan etki alanını varsayılan Aygıt yazma ormanı olarak görürsünüz.
   ![Özel Yükleme aygıtı writeback hedef orman](./media/how-to-connect-device-writeback/writebackforest.png)

4. **Aygıt kapsayıcı** sayfası, kullanılabilir iki seçenekten birini kullanarak etkin dizini hazırlama seçeneği sunar:

    a. **Kurumsal yönetici kimlik bilgilerini sağlama**: Aygıtların geri yazılması gereken orman için kurumsal yönetici kimlik bilgileri sağlanmışsa, Azure AD Connect aygıtın yeniden yazılması sırasında ormanı otomatik olarak hazırlar.

    b. **PowerShell komut dosyasını İndir**: Azure AD Connect, etkin dizini aygıt geri yazımı için hazırlayabilen bir PowerShell komut dosyası otomatik olarak oluşturur. Kurumsal yönetici kimlik bilgilerinin Azure AD Connect'te sağlanamaması durumunda PowerShell komut dosyasını karşıdan yüklemesi önerilir. İndirilen PowerShell komut dosyası **CreateDeviceContainer.psq'yi,** cihazların geri yazılacağı ormanın kurumsal yöneticisine sağlayın.
    ![Etkin dizin ormanı hazırlama](./media/how-to-connect-device-writeback/devicecontainercreds.png)
    
    Etkin dizin ormanının hazırlanması için aşağıdaki işlemler gerçekleştirilir:
    * Zaten yoksa, CN=Aygıt Kaydı Yapılandırması,CN=Hizmetler,CN=Yapılandırma,[orman-dn] altında yeni kapsayıcılar ve nesneler oluşturur ve yapılandırır.
    * Zaten yoksa, CN=RegisteredDevices(etki alanı-dn) altında yeni kapsayıcılar ve nesneler oluşturur ve yapılandırır. Aygıt nesneleri bu kapsayıcıda oluşturulur.
    * Etkin Dizininizdeki aygıtları yönetmek için Azure AD Bağlayıcısı hesabında gerekli izinleri ayarlar.
    * Azure AD Connect birden çok ormana yüklü olsa bile yalnızca tek bir ormanda çalışması gerekir.

## <a name="verify-devices-are-synchronized-to-active-directory"></a>Aygıtların Active Directory ile senkronize edildiğini doğrulayın
Aygıt geri yazma artık düzgün çalışıyor olmalıdır. Aygıt nesnelerinin AD'ye geri yazılmasının 3 saat kadar sürebileceğini unutmayın.  Aygıtlarınızın düzgün eşitlendiğini doğrulamak için eşitleme kuralları tamamlandıktan sonra aşağıdakileri yapın:

1. Active Directory Yönetim Merkezi'ni başlatın.
2. Kayıtlı Cihazları, federe edilmekte olan Etki Alanı içinde genişletin.

   ![Active Directory Admin Center Kayıtlı Cihazlar](./media/how-to-connect-device-writeback/devicewriteback5.png)

3. Geçerli kayıtlı aygıtlar burada listelenecektir.

   ![Active Directory Admin Center Kayıtlı Cihazlar Listesi](./media/how-to-connect-device-writeback/devicewriteback6.png)

## <a name="enable-conditional-access"></a>Koşullu Erişimi Etkinleştir
Bu senaryoyu etkinleştirmek için ayrıntılı [talimatlar, Azure Active Directory Device Kaydı'nı kullanarak Şirket İçi Koşullu Erişimi Ayarlama'da](../../active-directory/active-directory-device-registration-on-premises-setup.md)kullanılabilir.

## <a name="troubleshooting"></a>Sorun giderme
### <a name="the-writeback-checkbox-is-still-disabled"></a>Geri yazma onay kutusu hala devre dışı
Yukarıdaki adımları izlemiş olsanız bile aygıt geri yüklemesi onay kutusu etkinleştirilemezse, aşağıdaki adımlar, yükleme sihirbazının kutu etkinleştirilmeden önce neyi doğrulattığını size yönlendirecektir.

İlk olarak:

* Aygıtların bulunduğu orman, aygıt nesnesinin ve ilişkili özniteliklerin bulunması için orman şemasını Windows 2012 R2 düzeyine yükseltmelidir.
* Yükleme sihirbazı zaten çalışıyorsa, herhangi bir değişiklik algılanmaz. Bu durumda, yükleme sihirbazını tamamlayın ve yeniden çalıştırın.
* Başlatma komut dosyasında sağladığınız hesabın aslında Active Directory Connector tarafından kullanılan doğru kullanıcı olduğundan emin olun. Bunu doğrulamak için aşağıdaki adımları izleyin:
  * Başlat menüsünden **Eşitleme Hizmeti'ni**açın.
  * **Bağlayıcılar** sekmesini açın.
  * Active Directory Domain Services türüne sahip Bağlayıcıyı bulun ve seçin.
  * **Eylemler**altında, **Özellikleri**seçin.
  * Active **Directory Forest'a Bağlan'a**gidin. Bu ekranda belirtilen etki alanının ve kullanıcı adının komut dosyasında sağlanan hesapla eşleşip eşleşmediğini doğrulayın.
    ![Eşitleme Servis Yöneticisi'ndeki bağlayıcı hesabı](./media/how-to-connect-device-writeback/connectoraccount.png)

Etkin Dizini'nde yapılandırmayı doğrulayın:

* Aygıt Kayıt Hizmetinin yapılandırma adlandırma bağlamında aşağıdaki konumda bulunduğunu doğrulayın (CN=DeviceRegistrationService,CN=Device Registration Services,CN=Device Registration Configuration,CN=Services,CN=Configuration)

![Sorun Giderme, Configuration namespace'te DeviceRegistrationService](./media/how-to-connect-device-writeback/troubleshoot1.png)

* Yapılandırma ad alanını arayarak yalnızca bir yapılandırma nesnesi olduğunu doğrulayın. Birden fazla varsa, yinelenen silin.

![Sorun giderme, yinelenen nesneleri arama](./media/how-to-connect-device-writeback/troubleshoot2.png)

* Aygıt Kayıt Hizmeti nesnesi üzerinde, msDS-DeviceLocation özniteliğinin mevcut olduğundan ve bir değere sahip olduğundan emin olun. Bu konumu arayın ve objectType msDS-DeviceContainer ile birlikte bulunduğundan emin olun.

![Sorun Giderme, msDS-DeviceLocation](./media/how-to-connect-device-writeback/troubleshoot3.png)

![Sorun Giderme, RegisteredDevices nesne sınıfı](./media/how-to-connect-device-writeback/troubleshoot4.png)

* Active Directory Connector tarafından kullanılan hesabın, önceki adımda bulunan Kayıtlı Aygıtlar kapsayıcısı üzerinde gerekli izinleri gerektirdiğini doğrulayın. Bu kapsayıcıda beklenen izinler:

![Sorun giderme, kapsayıcıdaki izinleri doğrulama](./media/how-to-connect-device-writeback/troubleshoot5.png)

* Active Directory hesabının CN=Aygıt Kaydı Yapılandırması,CN=Hizmetler,CN=Yapılandırma nesnesi üzerinde izinleri olduğunu doğrulayın.

![Aygıt Kayıt Yapılandırması'ndaki izinleri giderme, izinleri doğrulama](./media/how-to-connect-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>Ek Bilgi
* [Koşullu Erişim ile Risk Yönetimi](../active-directory-conditional-access-azure-portal.md)
* [Azure Active Directory Cihaz Kaydı nı kullanarak Şirket Içi Koşullu Erişimi Ayarlama](../../active-directory/active-directory-device-registration-on-premises-setup.md)

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.

