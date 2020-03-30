---
title: Azure Active Directory Pass-through Authentication security derin dalış| Microsoft Dokümanlar
description: Bu makalede, Azure Etkin Dizin (Azure AD) Geçiş Kimlik Doğrulaması'nın şirket içi hesaplarınızı nasıl koruduğu açıklanmaktadır
services: active-directory
keywords: Azure AD Connect Pass-through Authentication, yükleme Active Directory, Azure AD, SSO, Tek Oturum açma için gerekli bileşenler
documentationcenter: ''
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/15/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1ddce8d4d7ca1f03c0a57d0f0c8c41ac122973e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77185551"
---
# <a name="azure-active-directory-pass-through-authentication-security-deep-dive"></a>Azure Active Directory Pass-through Kimlik Doğrulama güvenliği derin dalış

Bu makalede, Azure Etkin Dizin (Azure AD) Geçiş Kimlik Doğrulaması'nın nasıl çalıştığına ilgili daha ayrıntılı bir açıklama sağlanır. Özelliğin güvenlik yönlerine odaklanır. Bu makale, güvenlik ve BT yöneticileri, baş uyumluluk ve güvenlik görevlileri ve küçük ve orta ölçekli kuruluşlarda veya büyük işletmelerde BT güvenliği ve uyumluluğundan sorumlu diğer BT uzmanları içindir.

Ele alınacak konular şunlardır:
- Kimlik Doğrulama Aracıları'nın nasıl yüklenir ve kaydolunur hakkında ayrıntılı teknik bilgi.
- Kullanıcı oturum açma sırasında parolaların şifrelemesi hakkında ayrıntılı teknik bilgi.
- Şirket içi Kimlik Doğrulama Aracıları ve Azure AD arasındaki kanalların güvenliği.
- Kimlik Doğrulama Aracıları'nın operasyonel olarak nasıl güvende tutacağıhakkında ayrıntılı teknik bilgi.
- Güvenlikle ilgili diğer konular.

## <a name="key-security-capabilities"></a>Önemli güvenlik yetenekleri

Bu özelliğin temel güvenlik yönleri şunlardır:
- Kiracılar arasında oturum açma isteklerinin yalıtımını sağlayan çok kiracılı güvenli bir mimari üzerine inşa edilmiştir.
- Şirket içi parolalar hiçbir biçimde bulutta depolanır.
- Parola doğrulama isteklerini dinleyen ve yanıtlayan şirket içi Kimlik Doğrulama Aracıları yalnızca ağınız içinden giden bağlantılar yapar. Bu Kimlik Doğrulama Aracılarını çevre ağına (DMZ) yükleme zorunluluğu yoktur. En iyi yöntem olarak, Kimlik Doğrulama Aracıları çalıştıran tüm sunucuları Katman 0 sistemleri olarak ele abakın [(bkz. başvuru).](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)
- Kimlik Doğrulama Aracılarından Azure AD'ye giden iletişim için yalnızca standart bağlantı noktaları (80 ve 443) kullanılır. Güvenlik duvarınızda gelen bağlantı noktalarını açmanız gerekmez. 
  - Port 443, tüm kimlik doğrulaması giden iletişim için kullanılır.
  - Bağlantı noktası 80, bu özellik tarafından kullanılan sertifikaların hiçbirinin iptal edilmemiş olduğundan emin olmak için yalnızca Sertifika İptal Listelerini (CRLs) indirmek için kullanılır.
  - Ağ gereksinimlerinin tam listesi için Azure [Active Directory Pass-through Authentication: Quickstart](how-to-connect-pta-quick-start.md#step-1-check-the-prerequisites)bölümüne bakın.
- Oturum açma sırasında kullanıcıların sağladığı parolalar, şirket içi Kimlik Doğrulama Aracıları Active Directory'ye karşı doğrulama için kabul etmeden önce bulutta şifrelenir.
- Azure AD ile şirket içi Kimlik Doğrulama Aracısı arasındaki HTTPS kanalı, karşılıklı kimlik doğrulaması kullanılarak güvenli hale sunulur.
- Çok Faktörlü Kimlik Doğrulama (MFA) dahil olmak üzere [Azure AD Koşullu Erişim ilkeleriyle](../active-directory-conditional-access-azure-portal.md)sorunsuz çalışarak, [eski kimlik doğrulamasını engelleyerek](../conditional-access/concept-conditional-access-conditions.md) ve [kaba kuvvet parola saldırılarını filtreleyerek](../authentication/howto-password-smart-lockout.md)kullanıcı hesaplarınızı korur.

## <a name="components-involved"></a>İlgili bileşenler

Azure AD işletimi, hizmeti ve veri güvenliği hakkında genel ayrıntılar için [Güven Merkezi'ne](https://azure.microsoft.com/support/trust-center/)bakın. Kullanıcı oturum açma için Geçiş Kimlik Doğrulaması kullandığınızda aşağıdaki bileşenler dahil olur:
- **Azure AD STS**: Oturum açma isteklerini işleyen ve gerektiğinde kullanıcıların tarayıcılarına, istemcilerine veya hizmetlerine güvenlik belirteçleri veren, devletsiz bir güvenlik belirteç hizmeti (STS).
- **Azure Hizmet Veri Servisi**: Kurumsal mesajlaşma ile bulut özellikli iletişim sağlar ve şirket içi çözümleri bulutla bağlamanıza yardımcı olan iletişimi iler.
- **Azure AD Connect Kimlik Doğrulama Aracısı**: Parola doğrulama isteklerini dinleyen ve yanıtlayan şirket içi bileşendir.
- **Azure SQL Veritabanı**: Kiracınızın kimlik doğrulama aracıları hakkındaki bilgileri tutar, meta verileri ve şifreleme anahtarları da dahil olmak üzere.
- **Active Directory**: Kullanıcı hesaplarınızın ve parolalarınızın depolandığı şirket içi Active Directory.

## <a name="installation-and-registration-of-the-authentication-agents"></a>Kimlik Doğrulama Aracılarının Kurulumu ve Kaydı

Kimlik Doğrulama Aracıları aşağıdakileri yaptığınızda Azure AD'ye yüklenir ve kaydedilir:
   - [Azure AD Connect üzerinden Geçiş Kimlik Doğrulaması'nı etkinleştirme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-2-enable-the-feature)
   - [Oturum açma isteklerinin yüksek kullanılabilirliğini sağlamak için daha fazla Kimlik Doğrulama Aracısı ekleyin](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-quick-start#step-4-ensure-high-availability) 
   
Kimlik Doğrulama Aracısı'nın çalışmasını almak üç ana aşamadan gerektirir:

1. Kimlik Doğrulama Aracısı yükleme
2. Kimlik Doğrulama Aracısı kaydı
3. Kimlik Doğrulama Aracısı başlatma

Aşağıdaki bölümlerde bu aşamaları ayrıntılı olarak ele alıkonan bölümler yer adabEdi.

### <a name="authentication-agent-installation"></a>Kimlik Doğrulama Aracısı yükleme

Yalnızca genel yöneticiler, şirket içi bir sunucuya bir Kimlik Doğrulama Aracısı (Azure AD Connect'i kullanarak veya bağımsız olarak) yükleyebilir. Yükleme, **Denetim Masası** > **Programları Programları** > **ve Özellikleri** listesine iki yeni giriş ekler:
- Kimlik Doğrulama Aracısı uygulamasının kendisi. Bu uygulama [NetworkService](https://msdn.microsoft.com/library/windows/desktop/ms684272.aspx) ayrıcalıkları ile çalışır.
- Kimlik Doğrulama Aracısı'nı otomatik güncelleştirmek için kullanılan Updater uygulaması. Bu uygulama [LocalSystem](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) ayrıcalıkları ile çalışır.

### <a name="authentication-agent-registration"></a>Kimlik Doğrulama Aracısı kaydı

Kimlik Doğrulama Aracısını yükledikten sonra, kendisini Azure AD'ye kaydettirmesi gerekir. Azure AD, her Kimlik Doğrulama Aracısı'na Azure AD ile güvenli iletişim için kullanabileceği benzersiz, dijital kimlik sertifikası atar.

Kayıt yordamı, Kimlik Doğrulama Aracısını kiracınıza da bağlar. Bu, Azure AD'nin bu özel Kimlik Doğrulama Aracısının kiracınız için parola doğrulama isteklerini işlemek için yetkilendirilen tek kişi olduğunu bilmesini sağlar. Bu yordam, kaydettiğiniz her yeni Kimlik Doğrulama Aracısı için yinelenir.

Kimlik Doğrulama Aracıları, Kendilerini Azure AD'ye kaydettirmek için aşağıdaki adımları kullanır:

![Acente kaydı](./media/how-to-connect-pta-security-deep-dive/pta1.png)

1. Azure AD ilk olarak, global bir yöneticinin kimlik bilgileriyle Azure AD'de oturum açmasını ister. Oturum açma sırasında Kimlik Doğrulama Aracısı, genel yönetici adına kullanabileceği bir erişim belirteci elde eder.
2. Kimlik Doğrulama Aracısı daha sonra bir anahtar çifti oluşturur: ortak anahtar ve özel anahtar.
    - Anahtar çifti standart RSA 2048-bit şifreleme ile oluşturulur.
    - Özel anahtar, Kimlik Doğrulama Aracısı'nın bulunduğu şirket içi sunucuda kalır.
3. Kimlik Doğrulama Aracısı, https üzerinden Azure AD'ye bir "kayıt" isteğinde bulunarak isteğe aşağıdaki bileşenler dahildir:
    - 1. adımda edinilen erişim belirteci.
    - 2. adımda oluşturulan ortak anahtar.
    - Sertifika İmzalama İsteği (CSR veya Sertifika İsteği). Bu istek, sertifika yetkilisi (CA) olarak Azure AD olan bir dijital kimlik sertifikası için geçerlidir.
4. Azure AD, kayıt isteğindeki erişim belirtecidoğrular ve isteğin genel bir yöneticiden geldiğini doğrular.
5. Azure AD daha sonra kimlik doğrulama aracısına dijital kimlik sertifikası imzalar ve gönderir.
    - Azure AD'deki CA kökü sertifikayı imzalamak için kullanılır. 

      > [!NOTE]
      > Bu CA, Windows Trusted Root Certificate Authorities deposunda _yok._
    - CA yalnızca Geçiş Kimlik Doğrulama özelliği tarafından kullanılır. CA yalnızca Kimlik Doğrulama Aracısı kaydı sırasında CSR'ları imzalamak için kullanılır.
    -  Diğer Azure AD hizmetlerinin hiçbiri bu CA'yı kullanmaz.
    - Sertifikanın konusu (Seçkin Ad veya DN) kiracı kimliğinize ayarlanır. Bu DN, kiracınızı benzersiz olarak tanımlayan bir GUID'dir. Bu DN, yalnızca kiracınızla kullanılmak üzere sertifikayı kapsama.
6. Azure AD, Kimlik Doğrulama Aracısı'nın ortak anahtarını yalnızca Azure AD'nin erişediği bir Azure SQL veritabanında depolar.
7. Sertifika (adım 5'te verilir) Windows sertifika deposundaki şirket içi sunucuda (özellikle [CERT_SYSTEM_STORE_LOCAL_MACHINE](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_LOCAL_MACHINE) konumunda) depolanır. Hem Kimlik Doğrulama Aracısı hem de Güncelleyici uygulamaları tarafından kullanılır.

### <a name="authentication-agent-initialization"></a>Kimlik Doğrulama Aracısı başlatma

Kimlik Doğrulama Aracısı, kayıt tan sonra veya sunucu yeniden başlatıldıktan sonra ilk kez başlatıldığında, Azure AD hizmetiyle güvenli bir şekilde iletişim kurmanın ve parola doğrulama isteklerini kabul etmeye başlamanın bir yolunu netmesi gerekir.

![Aracı başlatma](./media/how-to-connect-pta-security-deep-dive/pta2.png)

Kimlik Doğrulama Aracıları'nın baş harfleri şu şekilde dir:

1. Kimlik Doğrulama Aracısı, Azure AD'ye giden bir önyükleme isteğinde bulundu. 
    - Bu istek port 443 üzerinden yapılır ve karşılıklı kimlik doğrulaması yapılan bir HTTPS kanalı üzerindedir. İstek, Kimlik Doğrulama Aracısı kaydı sırasında verilen sertifikayı kullanır.
2. Azure AD, isteğe, kiracınıza özgü ve kiracı kimliğiniz tarafından tanımlanan bir Azure Hizmet Veri Hizmeti Veri Hizmeti kuyruğuna erişim anahtarı sağlayarak yanıt verir.
3. Kimlik Doğrulama Aracısı, kuyruğa kalıcı giden bir HTTPS bağlantısı (bağlantı noktası 443 üzerinden) yapar. 
    - Kimlik Doğrulama Aracısı artık parola doğrulama isteklerini almaya ve işlemeye hazırdır.

Kiracınızda kayıtlı birden çok Kimlik Doğrulama Aracısı varsa, başlatma yordamı her birinin aynı Servis Veri Servisi kuyruğuna bağlanmasını sağlar. 

## <a name="process-sign-in-requests"></a>Oturum açma isteklerini işleme 

Aşağıdaki diyagram, Geçiş Kimlik Doğrulama'nın kullanıcı oturum açma isteklerini nasıl işlediğini gösterir.

![İşlem oturum açma](./media/how-to-connect-pta-security-deep-dive/pta3.png)

Geçiş Kimlik Doğrulama, kullanıcı oturum açma isteğini aşağıdaki gibi işler: 

1. Bir kullanıcı bir uygulamaya erişmeye çalışır, örneğin, [Outlook Web App](https://outlook.office365.com/owa).
2. Kullanıcı zaten oturum açmamışsa, uygulama tarayıcıyı Azure AD oturum açma sayfasına yönlendirir.
3. Azure AD STS **hizmeti, Kullanıcı oturum açma** sayfasına yanıt verir.
4. Kullanıcı kullanıcı adını Kullanıcı **oturum açma** sayfasına girer ve **sonra Sonraki** düğmesini seçer.
5. Kullanıcı parolasını Kullanıcı **oturum açma** sayfasına girer ve oturum **açma** düğmesini seçer.
6. Kullanıcı adı ve parola, bir HTTPS POST isteğiyle Azure AD STS'ye gönderilir.
7. Azure AD STS, Azure SQL veritabanından kiracınızda kayıtlı tüm Kimlik Doğrulama Aracıları için ortak anahtarları alır ve bunları kullanarak parolayı şifreler.
    - Kiracınızda kayıtlı "N" Kimlik Doğrulama Aracıları için "N" şifreli parola değerleri üretir.
8. Azure AD STS, kullanıcı adı ve şifrelenmiş parola değerlerinden oluşan parola doğrulama isteğini kiracınıza özel Servis Veri Servisi kuyruğuna yerleştirir.
9. İlk olarak kimlik doğrulama aracıları ısrarla Hizmet Veri Servisi sırasına bağlı olduğundan, kullanılabilir Kimlik Doğrulama Aracılarından biri parola doğrulama isteğini alır.
10. Kimlik Doğrulama Aracısı, bir tanımlayıcı kullanarak ortak anahtarına özgü şifrelenmiş parola değerini bulur ve özel anahtarını kullanarak şifresini çözer.
11. Kimlik Doğrulama Aracısı, **LOGON32_LOGON_NETWORK**ayarlanan **dwLogonType** parametresi ile [Win32 LogonUser API'sini](https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx) kullanarak kullanıcı adını ve parolayı şirket içi Active Directory'ye karşı doğrulamaya çalışır. 
    - Bu API, Aktif Dizin Federasyon Hizmetleri (AD FS) tarafından federe oturum açma senaryosunda kullanıcıları oturum etmek için kullanılan API ile aynıdır.
    - Bu API, etki alanı denetleyicisini bulmak için Windows Server'daki standart çözümleme işlemine dayanır.
12. Kimlik Doğrulama Aracısı, başarı, kullanıcı adı veya parola nın yanlış olması veya parolanın süresi nin dolması gibi Etkin Dizini'nden sonucu alır.

   > [!NOTE]
   > Kimlik Doğrulama Aracısı oturum açma işlemi sırasında başarısız olursa, oturum açma isteğinin tamamı bırakılır. Bir Kimlik Doğrulama Aracısından şirket içinde başka bir Kimlik Doğrulama Aracısı'na oturum açma istekleri nin teslimi yoktur. Bu aracılar birbirleriyle değil, yalnızca bulutla iletişim kurarlar.
   
13. Kimlik Doğrulama Aracısı, 443 bağlantı noktası üzerinden giden karşılıklı kimlik doğrulaması yapılan bir HTTPS kanalı üzerinden sonucu Azure AD STS'ye geri yönlendirir. Karşılıklı kimlik doğrulaması, kayıt sırasında kimlik doğrulama temsilcisine daha önce verilmiş olan sertifikayı kullanır.
14. Azure AD STS, bu sonucun kiracınızdaki belirli oturum açma isteğiyle ilişkili olduğunu doğrular.
15. Azure AD STS, yapılandırıldıkça oturum açma yordamıyla devam eder. Örneğin, parola doğrulama başarılı olduysa, kullanıcı Çok Faktörlü Kimlik Doğrulama için meydan veya uygulamaya geri yönlendirilmiş olabilir.

## <a name="operational-security-of-the-authentication-agents"></a>Kimlik Doğrulama Aracılarının Operasyonel Güvenliği

Azure AD, Geçiş Kimlik Doğrulaması'nın operasyonel olarak güvenli kalmasını sağlamak için kimlik doğrulama aracılarının sertifikalarını düzenli aralıklarla yeniler. Azure AD yenilemeleri tetikler. Yenilemeler Kimlik Doğrulama Aracıları tarafından yönetilmez.

![İşletimsel güvenlik](./media/how-to-connect-pta-security-deep-dive/pta4.png)

Azure AD ile bir Kimlik Doğrulama Aracısı'nın güvenini yenilemek için:

1. Kimlik Doğrulama Aracısı, sertifikasını yenileme zamanının geldiğini kontrol etmek için Azure AD'yi birkaç saatte bir düzenli olarak pingler. Sertifika, süresinin dolmasından 30 gün önce yenilenir.
    - Bu denetim, karşılıklı kimlik doğrulaması yapılan bir HTTPS kanalı üzerinden yapılır ve kayıt sırasında verilen sertifikayı kullanır.
2. Hizmet yenileme zamanının geldiğini gösterirse, Kimlik Doğrulama Aracısı yeni bir anahtar çifti oluşturur: ortak anahtar ve özel anahtar.
    - Bu anahtarlar standart RSA 2048 bit şifreleme ile oluşturulur.
    - Özel anahtar, şirket içi sunucudan asla ayrılmaz.
3. Kimlik Doğrulama Aracısı daha sonra HTTPS üzerinden Azure AD'ye bir "sertifika yenileme" isteği nde bulunarak isteğe aşağıdaki bileşenler dahildir:
    - Windows sertifika deposundaki CERT_SYSTEM_STORE_LOCAL_MACHINE konumundan alınan varolan sertifika. Bu yordamda yer alan hiçbir genel yönetici yoktur, bu nedenle genel yönetici adına gerekli erişim belirteci yoktur.
    - 2. adımda oluşturulan ortak anahtar.
    - Sertifika İmzalama İsteği (CSR veya Sertifika İsteği). Bu istek, sertifika yetkilisi olarak Azure AD'nin olduğu yeni bir dijital kimlik sertifikası için geçerlidir.
4. Azure AD, sertifika yenileme isteğinde varolan sertifikayı doğrular. Daha sonra, isteğin kiracınızda kayıtlı bir Kimlik Doğrulama Aracısından geldiğini doğrular.
5. Varolan sertifika hala geçerliyse, Azure AD yeni bir dijital kimlik sertifikası imzalar ve yeni sertifikayı Kimlik Doğrulama Aracısı'na geri sunar. 
6. Varolan sertifikanın süresi dolmuşsa, Azure AD Kimlik Doğrulama Aracısını kiracınızın kayıtlı Kimlik Doğrulama Aracıları listesinden siler. Ardından, genel bir yöneticinin yeni bir Kimlik Doğrulama Aracısını el ile yüklemesi ve kaydetmesi gerekir.
    - Sertifikayı imzalamak için Azure AD kökü CA'yı kullanın.
    - Sertifikanın öznesini (Seçkin Ad veya DN) kiracı kimliğinize, kiracınızı benzersiz olarak tanımlayan bir GUID olarak ayarlayın. DN, sertifikayı yalnızca kiracınıza doğru kapsamlıdır.
6. Azure AD, Kimlik Doğrulama Aracısı'nın yeni ortak anahtarını yalnızca erişebilen bir Azure SQL veritabanında saklar. Ayrıca, Kimlik Doğrulama Aracısı ile ilişkili eski ortak anahtarı geçersiz k.
7. Yeni sertifika (adım 5'te verilir) daha sonra Windows sertifika deposundaki sunucuda (özellikle [CERT_SYSTEM_STORE_CURRENT_USER](https://msdn.microsoft.com/library/windows/desktop/aa388136.aspx#CERT_SYSTEM_STORE_CURRENT_USER) konumunda) depolanır.
    - Güven yenileme yordamı etkileşimli olmayan (genel yöneticinin varlığı olmadan) gerçekleştiğinden, Kimlik Doğrulama Aracısı artık CERT_SYSTEM_STORE_LOCAL_MACHINE konumundaki varolan sertifikayı güncelleştirmeye erişememiştir. 
    
   > [!NOTE]
   > Bu yordam, sertifikanın kendisini CERT_SYSTEM_STORE_LOCAL_MACHINE konumundan kaldırmaz.
8. Yeni sertifika bu noktadan itibaren kimlik doğrulaması için kullanılır. Sertifikanın sonraki her yenilemesi, CERT_SYSTEM_STORE_LOCAL_MACHINE konumundaki sertifikanın yerini alır.

## <a name="auto-update-of-the-authentication-agents"></a>Kimlik Doğrulama Aracılarının Otomatik Güncelleştirmesi

Updater uygulaması, yeni bir sürüm (hata düzeltmeleri veya performans geliştirmeleri ile) yayımlandığında Kimlik Doğrulama Aracısı'nı otomatik olarak güncelleştirir. Updater uygulaması kiracınız için herhangi bir parola doğrulama isteklerini işlemez.

Azure AD, yazılımın yeni sürümünü imzalı **Windows Yükleyici paketi (MSI)** olarak barındırıyor. MSI, microsoft [authenticode](https://msdn.microsoft.com/library/ms537359.aspx) sha256 ile sindirimi algoritması olarak kullanılarak imzalanır. 

![Otomatik güncelleme](./media/how-to-connect-pta-security-deep-dive/pta5.png)

Kimlik Doğrulama Aracısını otomatik olarak güncelleştirmek için:

1. Updater uygulaması, Kimlik Doğrulama Aracısı'nın kullanılabilir yeni bir sürümü olup olmadığını kontrol etmek için Azure AD'yi her saat başı pingler. 
    - Bu denetim, kayıt sırasında verilen aynı sertifika kullanılarak karşılıklı kimlik doğrulaması yapılan bir HTTPS kanalı üzerinden yapılır. Kimlik Doğrulama Aracısı ve Güncelleyici, sunucuda depolanan sertifikayı paylaşır.
2. Yeni bir sürüm varsa, Azure AD imzalı MSI'ı Updater'a geri döndürür.
3. Updater, MSI'ın Microsoft tarafından imzalanmış olduğunu doğrular.
4. Updater MSI çalıştırın. Bu eylem aşağıdaki adımları içerir:

   > [!NOTE]
   > Updater Yerel [Sistem](https://msdn.microsoft.com/library/windows/desktop/ms684190.aspx) ayrıcalıkları ile çalışır.

    - Kimlik Doğrulama Aracısı hizmetini durdurur
    - Kimlik Doğrulama Aracısı'nın yeni sürümünü sunucuya yükler
    - Kimlik Doğrulama Aracısı hizmetini yeniden başlatır

>[!NOTE]
>Kiracınızda kayıtlı birden çok Kimlik Doğrulama Aracısı varsa, Azure AD sertifikalarını yenilemez veya aynı anda güncelleştirmez. Bunun yerine, Azure AD bunu oturum açma isteklerinin yüksek kullanılabilirliğini sağlamak için teker teker yapar.
>


## <a name="next-steps"></a>Sonraki adımlar
- [Geçerli sınırlamalar](how-to-connect-pta-current-limitations.md): Hangi senaryoların desteklenip hangilerinin desteklenmediğini öğrenin.
- [Quickstart](how-to-connect-pta-quick-start.md): Azure AD Pass-through Authentication'da çalışmaya başlayın.
- [AD FS'den Geçiş Kimlik Doğrulamasına geçiş](https://aka.ms/adfstoptadpdownload) - AD FS'den (veya diğer federasyon teknolojilerinden) Geçiş Kimlik Doğrulaması'na geçiş için ayrıntılı bir kılavuz.
- [Akıllı Kilitleme](../authentication/howto-password-smart-lockout.md): Kullanıcı hesaplarını korumak için kiracınızdaki Akıllı Kilitleme özelliğini yapılandırın.
- [Nasıl çalışır](how-to-connect-pta-how-it-works.md): Azure AD Pass-through Kimlik Doğrulamasının nasıl çalıştığına bağlı temel bilgileri öğrenin.
- [Sık sorulan sorular](how-to-connect-pta-faq.md): Sık sorulan soruların yanıtlarını bulun.
- [Sorun Giderme](tshoot-connect-pass-through-authentication.md): Geçiş Kimlik Doğrulama özelliğiyle ilgili sık karşılaşılan sorunları nasıl çözeceğinizi öğrenin.
- [Azure AD Sorunsuz SSO](how-to-connect-sso.md): Bu tamamlayıcı özellik hakkında daha fazla bilgi edinin.
