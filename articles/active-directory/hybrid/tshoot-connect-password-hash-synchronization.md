---
title: Azure AD Connect eşitlemeile parola karma senkronizasyonu | Microsoft Dokümanlar
description: Bu makalede, parola karma eşitleme sorunları giderilmesi hakkında bilgi sağlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6feed11fcfc597658f3ec148b5dd18bb7e3f8f83
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253552"
---
# <a name="troubleshoot-password-hash-synchronization-with-azure-ad-connect-sync"></a>Azure AD Connect eşitlemesi ile parola karması eşitleme sorunlarını giderme

Bu konu, parola karma eşitleme ile sorunları gidermek için nasıl adımlar sağlar. Parolalar beklendiği gibi eşitlemiyorsa, bu kullanıcı alt kümesi veya tüm kullanıcılar için olabilir.

Azure Etkin Dizini (Azure AD) Sürüm 1.1.614.0 veya sonrası ile dağıtımı bağlamak için, parola karma eşitleme sorunlarını gidermek için sihirbazdaki sorun giderme görevini kullanın:

* Parolaların eşitlenmediği bir sorununuz varsa, parola yok bölümüne [bakın: sorun giderme görev](#no-passwords-are-synchronized-troubleshoot-by-using-the-troubleshooting-task) bölümünü kullanarak sorun giderme.

* Tek tek nesnelerle ilgili bir sorununuzun varsa, [tek bir nesnenin parolaları eşitlemediğini belirtin: sorun giderme görev](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-troubleshooting-task) bölümünü kullanarak sorun giderme.

Sürüm 1.1.524.0 veya sonraki sürümlerle dağıtım için, parola karma eşitleme sorunlarını gidermek için kullanabileceğiniz bir tanılama cmdleti vardır:

* Parolanın eşitlenmediği bir sorununuz varsa, parola yok bölümüne [bakın: tanılama cmdlet](#no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet) bölümünü kullanarak sorun giderme.

* Tek tek nesnelerle ilgili bir sorununuzun varsa, [tek nesnenin parolaları eşitlemediğini belirtin: tanılama cmdlet](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet) bölümünü kullanarak sorun giderme.

Azure AD Connect dağıtımının eski sürümleri için:

* Parolanın eşitlenmediği bir sorununuz varsa, parola yok bölümüne [bakın: el ile sorun giderme adımları](#no-passwords-are-synchronized-manual-troubleshooting-steps) bölümüne bakın.

* Tek tek nesnelerle ilgili bir sorununuz varsa, tek nesneye bakın [parolaları eşitlemiyor: el ile sorun giderme adımları](#one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps) bölümü.



## <a name="no-passwords-are-synchronized-troubleshoot-by-using-the-troubleshooting-task"></a>Parola senkronize olmaz: sorun giderme görevini kullanarak sorun giderme

Parolaların neden eşitlenmedığını anlamak için sorun giderme görevini kullanabilirsiniz.

> [!NOTE]
> Sorun giderme görevi yalnızca Azure AD Connect sürümü 1.1.614.0 veya sonraki sürümiçin kullanılabilir.

### <a name="run-the-troubleshooting-task"></a>Sorun giderme görevini çalıştırın

Parolanın eşitlenmediği sorunları gidermek için:

1. Azure AD Connect sunucunuzda **Yönetici olarak Çalıştır** seçeneğiyle yeni bir Windows PowerShell oturumu açın.

2. Çalıştır `Set-ExecutionPolicy RemoteSigned` `Set-ExecutionPolicy Unrestricted`ya da .

3. Azure AD Bağlantısı sihirbazını başlatın.

4. **Ek Görevler** sayfasına gidin, **Sorun Giderme'yi**seçin ve **İleri'yi**tıklatın.

5. Sorun Giderme sayfasında, PowerShell'deki sorun giderme menüsünü başlatmak için **Başlat'ı** tıklatın.

6. Ana menüde **Sorun Giderme parola karma senkronizasyonu'nu**seçin.

7. Alt menüde Parola **karma senkronizasyonu'nu seçin.**

### <a name="understand-the-results-of-the-troubleshooting-task"></a>Sorun giderme görevinin sonuçlarını anlama

Sorun giderme görevi aşağıdaki denetimleri gerçekleştirir:

* Azure AD kiracınız için parola karma eşitleme özelliğinin etkin olduğunu doğrular.

* Azure AD Connect sunucusunun hazırlama modunda olmadığını doğrular.

* Varolan her şirket içi Active Directory bağlayıcısı için (varolan bir Active Directory ormanına karşılık gelir):

   * Parola karma eşitleme özelliğinin etkin olduğunu doğrular.
   
   * Windows Application Event günlüklerinde parola karma eşitleme sinyali olaylarını arar.

   * Şirket içi Active Directory bağlayıcısı altındaki her Active Directory etki alanı için:

      * Etki alanına Azure AD Connect sunucusundan erişilebildiğini doğrular.

      * Şirket içi Active Directory bağlayıcısı tarafından kullanılan Active Directory Domain Services (AD DS) hesaplarının parola karma senkronizasyonu için gerekli doğru kullanıcı adı, parola ve izinlere sahip olduğunu doğrular.

Aşağıdaki diyagram, cmdlet'in tek alanlı, şirket içi Active Directory topolojisi sonuçlarını göstermektedir:

![Parola karma senkronizasyonu için tanılama çıktısı](./media/tshoot-connect-password-hash-synchronization/phsglobalgeneral.png)

Bu bölümün geri kalanı, görev ve ilgili sorunlar tarafından döndürülen belirli sonuçları açıklar.

#### <a name="password-hash-synchronization-feature-isnt-enabled"></a>şifre karma eşitleme özelliği etkin değil

Azure AD Connect sihirbazını kullanarak parola karma eşitlemesi etkinleştirmediyseniz, aşağıdaki hata döndürülür:

![parola karma eşitleme etkin değil](./media/tshoot-connect-password-hash-synchronization/phsglobaldisabled.png)

#### <a name="azure-ad-connect-server-is-in-staging-mode"></a>Azure AD Connect sunucusu evreleme modunda

Azure AD Connect sunucusu hazırlama modundaysa, parola karma eşitleme geçici olarak devre dışı bırakılır ve aşağıdaki hata döndürülür:

![Azure AD Connect sunucusu evreleme modunda](./media/tshoot-connect-password-hash-synchronization/phsglobalstaging.png)

#### <a name="no-password-hash-synchronization-heartbeat-events"></a>Parola karma eşitleme kalp atışı olayları yok

Her şirket içi Active Directory bağlayıcısı kendi şifre karma eşitleme kanalı vardır. Parola karma eşitleme kanalı kurulduğunda ve eşitlenecek parola değişikliği olmadığında, Windows Application Event Log altında her 30 dakikada bir bir sinyal olayı (EventId 654) oluşturulur. Her şirket içi Active Directory bağlayıcısı için cmdlet, son üç saat içinde ilgili kalp atışı olaylarını arar. Sinyal olayı bulunamazsa, aşağıdaki hata döndürülür:

![Şifre hash senkronizasyon kalp atışı olay](./media/tshoot-connect-password-hash-synchronization/phsglobalnoheartbeat.png)

#### <a name="ad-ds-account-does-not-have-correct-permissions"></a>AD DS hesabının doğru izinleri yok

Parola açıklarını eşitlemek için şirket içi Active Directory bağlayıcısı tarafından kullanılan AD DS hesabı uygun izinlere sahip değilse, aşağıdaki hata döndürülür:

![Yanlış kimlik bilgisi](./media/tshoot-connect-password-hash-synchronization/phsglobalaccountincorrectpermission.png)

#### <a name="incorrect-ad-ds-account-username-or-password"></a>Yanlış AD DS hesabı kullanıcı adı veya şifre

İşyeri içinde Active Directory bağlayıcısı tarafından parola hatalarını veya parolayı eşitlemek için kullanılan AD DS hesabının yanlış bir kullanıcı adı veya parolası varsa, aşağıdaki hata döndürülür:

![Yanlış kimlik bilgisi](./media/tshoot-connect-password-hash-synchronization/phsglobalaccountincorrectcredential.png)



## <a name="one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-troubleshooting-task"></a>Bir nesne parolaları eşitleme değildir: sorun giderme görevini kullanarak sorun giderme

Bir nesnenin parolaları neden eşitlemediğini belirlemek için sorun giderme görevini kullanabilirsiniz.

> [!NOTE]
> Sorun giderme görevi yalnızca Azure AD Connect sürümü 1.1.614.0 veya sonraki sürümiçin kullanılabilir.

### <a name="run-the-diagnostics-cmdlet"></a>Tanılama cmdlet çalıştırın

Belirli bir kullanıcı nesnesi için sorunları gidermek için:

1. Azure AD Connect sunucunuzda **Yönetici olarak Çalıştır** seçeneğiyle yeni bir Windows PowerShell oturumu açın.

2. Çalıştır `Set-ExecutionPolicy RemoteSigned` `Set-ExecutionPolicy Unrestricted`ya da .

3. Azure AD Bağlantısı sihirbazını başlatın.

4. **Ek Görevler** sayfasına gidin, **Sorun Giderme'yi**seçin ve **İleri'yi**tıklatın.

5. Sorun Giderme sayfasında, PowerShell'deki sorun giderme menüsünü başlatmak için **Başlat'ı** tıklatın.

6. Ana menüde **Sorun Giderme parola karma senkronizasyonu'nu**seçin.

7. Alt menüde, Parola'yı seçin **belirli bir kullanıcı hesabı için senkronize edilmez.**

### <a name="understand-the-results-of-the-troubleshooting-task"></a>Sorun giderme görevinin sonuçlarını anlama

Sorun giderme görevi aşağıdaki denetimleri gerçekleştirir:

* Active Directory bağlayıcısı, Metaverse ve Azure AD bağlayıcısı alanında Etkin Dizin nesnesinin durumunu inceler.

* Etkin Dizin nesnesine uygulanan parola karma eşitlemesi etkin leştirilmiş eşitleme kuralları olduğunu doğrular.

* Nesnenin parolasını eşitlemek için son denemenin sonuçlarını almaya ve görüntülemeye çalışır.

Aşağıdaki diyagram, tek bir nesne için parola karma eşitleme sinisorun giderme cmdlet sonuçlarını gösterir:

![Parola karma senkronizasyonu için tanılama çıktısı - tek nesne](./media/tshoot-connect-password-hash-synchronization/phssingleobjectgeneral.png)

Bu bölümün geri kalanında cmdlet ve ilgili sorunlar tarafından döndürülen özel sonuçlar açıklanmaktadır.

#### <a name="the-active-directory-object-isnt-exported-to-azure-ad"></a>Etkin Dizin nesnesi Azure AD'ye dışa aktarılmaz

Azure AD kiracısında karşılık gelen bir nesne olmadığından, bu şirket içi Active Directory hesabı için parola karma eşitleme başarısız olur. Aşağıdaki hata döndürülür:

![Azure AD nesnesi eksik](./media/tshoot-connect-password-hash-synchronization/phssingleobjectnotexported.png)

#### <a name="user-has-a-temporary-password"></a>Kullanıcının geçici bir şifresi var

Şu anda Azure AD Connect, geçici parolaları Azure AD ile eşitlemeyi desteklemez. **Bir sonraki oturum açma seçeneğindeki Değiştir parolası** şirket içi Active Directory kullanıcısında ayarlanırsa, parola geçici olarak kabul edilir. Aşağıdaki hata döndürülür:

![Geçici parola dışa aktarılmaz](./media/tshoot-connect-password-hash-synchronization/phssingleobjecttemporarypassword.png)

#### <a name="results-of-last-attempt-to-synchronize-password-arent-available"></a>Parolayı eşitlemek için yapılan son girişimin sonuçları kullanılamıyor

Varsayılan olarak, Azure AD Connect parola karma eşitleme denemelerinin sonuçlarını yedi gün boyunca saklar. Seçili Active Directory nesnesi için sonuç yoksa, aşağıdaki uyarı döndürülür:

![Tek bir nesne için tanılama çıktısı - parola eşitleme geçmişi yok](./media/tshoot-connect-password-hash-synchronization/phssingleobjectnohistory.png)



## <a name="no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet"></a>Hiçbir parola senkronize edilir: tanılama cmdlet kullanılarak sorun giderme

Hiçbir parolanın `Invoke-ADSyncDiagnostics` neden senkronize olmadığını anlamak için cmdlet'i kullanabilirsiniz.

> [!NOTE]
> Cmdlet `Invoke-ADSyncDiagnostics` yalnızca Azure AD Connect sürümü 1.1.524.0 veya sonraki sürümiçin kullanılabilir.

### <a name="run-the-diagnostics-cmdlet"></a>Tanılama cmdlet çalıştırın

Parolanın eşitlenmediği sorunları gidermek için:

1. Azure AD Connect sunucunuzda **Yönetici olarak Çalıştır** seçeneğiyle yeni bir Windows PowerShell oturumu açın.

2. Çalıştır `Set-ExecutionPolicy RemoteSigned` `Set-ExecutionPolicy Unrestricted`ya da .

3. `Import-Module ADSyncDiagnostics` öğesini çalıştırın.

4. `Invoke-ADSyncDiagnostics -PasswordSync` öğesini çalıştırın.



## <a name="one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet"></a>Bir nesne parolaları eşitleme değildir: tanılama cmdlet kullanarak sorun giderme

Bir nesnenin `Invoke-ADSyncDiagnostics` neden parolaları eşitlemediğini belirlemek için cmdlet'i kullanabilirsiniz.

> [!NOTE]
> Cmdlet `Invoke-ADSyncDiagnostics` yalnızca Azure AD Connect sürümü 1.1.524.0 veya sonraki sürümiçin kullanılabilir.

### <a name="run-the-diagnostics-cmdlet"></a>Tanılama cmdlet çalıştırın

Kullanıcı için parola eşitlenmediği sorunları gidermek için:

1. Azure AD Connect sunucunuzda **Yönetici olarak Çalıştır** seçeneğiyle yeni bir Windows PowerShell oturumu açın.

2. Çalıştır `Set-ExecutionPolicy RemoteSigned` `Set-ExecutionPolicy Unrestricted`ya da .

3. `Import-Module ADSyncDiagnostics` öğesini çalıştırın.

4. Aşağıdaki cmdlet'i çalıştırın:

   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName <Name-of-AD-Connector> -DistinguishedName <DistinguishedName-of-AD-object>
   ```

   Örnek:

   ```powershell
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName "contoso.com" -DistinguishedName "CN=TestUserCN=Users,DC=contoso,DC=com"
   ```



## <a name="no-passwords-are-synchronized-manual-troubleshooting-steps"></a>Parola senkronize olmaz: el ile sorun giderme adımları

Parolanın neden eşitlenmeyiş olduğunu belirlemek için aşağıdaki adımları izleyin:

1. Connect sunucusu [evreleme modunda](how-to-connect-sync-staging-server.md)mı? Hazırlama modundaki bir sunucu parolaları eşitlemez.

2. Komut dosyasını [parola eşitleme ayarları](#get-the-status-of-password-sync-settings) nın durumunu al bölümünde çalıştırın. Parola eşitleme yapılandırması hakkında genel bir bakış sağlar.  

    ![Parola eşitleme ayarlarından PowerShell komut dosyası çıkışı](./media/tshoot-connect-password-hash-synchronization/psverifyconfig.png)  

3. Özellik Azure AD'de etkinleştirilemezse veya eşitleme kanalı durumu etkin değilse, Bağla yükleme sihirbazını çalıştırın. **Eşitleme seçeneklerini özelleştir'i**ve parola eşitlemeyi seçilmeyi seçin. Bu değişiklik özelliği geçici olarak devre dışı kılabilir. Ardından sihirbazı yeniden çalıştırın ve parola eşitlemesini yeniden etkinleştirin. Yapılandırmanın doğru olup olmadığını doğrulamak için komut dosyasını yeniden çalıştırın.

4. Hatalar için olay günlüğüne bakın. Bir sorunu gösteren aşağıdaki olaylara bakın:
    * Kaynak: "Dizin eşitleme" ID: 0, 611, 652, 655 Bu olayları görürseniz, bir bağlantı sorunu var. Olay günlüğü iletisi, sorun yaşadığın orman bilgilerini içerir. Daha fazla bilgi için Bağlantı [sorununa](#connectivity problem)bakın.

5. Sinyal atlayış görmüyorsanız veya başka bir şey çalışmıyorsa, [tüm parolaların tam eşitlesini tetikle'yi çalıştırın.](#trigger-a-full-sync-of-all-passwords) Komut dosyasını yalnızca bir kez çalıştırın.

6. Parolaları eşitlemeyen bir nesneyi giderme sorununa bakın.

### <a name="connectivity-problems"></a>Bağlantı sorunları

Azure AD ile bağlantınız var mı?

Hesabın tüm etki alanlarındaki şifre yazılarını okumak için izinleri gerekli mi? Express ayarlarını kullanarak Bağlan'ı yüklediyseniz, izinlerin zaten doğru olması gerekir. 

Özel yükleme kullandıysanız, aşağıdakileri yaparak izinleri el ile ayarlayın:
    
1. Active Directory bağlayıcısı tarafından kullanılan hesabı bulmak için **Eşitleme Hizmet Yöneticisi'ni**başlatın. 
 
2. **Bağlayıcılar'a**gidin ve ardından sorun giderdiğiniz şirket içi Active Directory ormanını arayın. 
 
3. Bağlayıcıyı seçin ve ardından **Özellikler'i**tıklatın. 
 
4. Active **Directory Forest'a Bağlan'a**gidin.  
    
    ![Active Directory konektörü tarafından kullanılan hesap](./media/tshoot-connect-password-hash-synchronization/connectoraccount.png)  
    Kullanıcı adını ve hesabın bulunduğu etki alanını not edin.
    
5. **Etkin Dizin Kullanıcıları ve Bilgisayarlarını**başlatın ve daha önce bulduğunuz hesabın ormanınızdaki tüm etki alanlarının kökünde ayarlanmış izleme izinlerine sahip olduğunu doğrulayın:
    * Dizin Değişikliklerini Çoğaltma
    * Dizin Değişikliklerini Tümünü Çoğaltma

6. Etki alanı denetleyicilerine Azure AD Connect tarafından erişilebilir mi? Connect sunucusu tüm etki alanı denetleyicilerine bağlanamıyorsa, yalnızca **tercih edilen etki alanı denetleyicisini kullanın.**  
    
    ![Active Directory bağlayıcısı tarafından kullanılan etki alanı denetleyicisi](./media/tshoot-connect-password-hash-synchronization/preferreddc.png)  
    
7. **Eşitleme Hizmet Yöneticisi'ne** geri dön ve **Dizin Bölümü'ni yapılandır.** 
 
8. **Dizin bölümlerini seç'te**etki alanınızı seçin, **Yalnızca kullanımda tercih edilen etki alanı denetleyicileri** onay kutusunu seçin ve sonra **Yapıl'ı**tıklatın. 

9. Listede, Connect'in parola eşitlemesi için kullanması gereken etki alanı denetleyicilerini girin. Aynı liste ithalat ve dışa aktarma için de kullanılır. Tüm etki alanlarınız için bu adımları yapın.

10. Komut dosyası sinyal atışını olmadığını gösteriyorsa, komut dosyasını [tüm parolaların tam eşitle'sini tetikle'de](#trigger-a-full-sync-of-all-passwords)çalıştırın.

## <a name="one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps"></a>Bir nesne parolaları eşitleme değildir: el ile sorun giderme adımları

Bir nesnenin durumunu gözden geçirerek parola karma eşitleme sorunlarını kolayca giderebilirsiniz.

1. **Etkin Dizin Kullanıcıları ve Bilgisayar'da,** kullanıcıyı arayın ve ardından **kullanıcının bir sonraki oturum açma** onay kutusunda parolayı değiştirmesi gerektiğini doğrulayın.  

    ![Etkin Dizin üretken parolalar](./media/tshoot-connect-password-hash-synchronization/adprodpassword.png)  

    Onay kutusu seçilirse, kullanıcıdan oturum açmasını ve parolayı değiştirmesini isteyin. Geçici parolalar Azure AD ile eşitlenmez.

2. Etkin Dizin'de parola doğru görünüyorsa, eşitleme motorundaki kullanıcıyı izleyin. Kullanıcıyı şirket içi Active Directory'den Azure AD'ye kadar takip ederek, nesneüzerinde açıklayıcı bir hata olup olmadığını görebilirsiniz.

    a. [Eşitleme Hizmet Yöneticisi'ni](how-to-connect-sync-service-manager-ui.md)başlatın.

    b. **Bağlayıcılar'ı**tıklatın.

    c. Kullanıcının bulunduğu **Active Directory Bağlayıcısı'nı** seçin.

    d. **Arama Bağlayıcısı Alanı'nı**seçin.

    e. **Kapsam** kutusunda, **DN veya Anchor'ı**seçin ve ardından sorun giderdiğiniz kullanıcının tam DN'sini girin.

    ![DN ile bağlayıcı alanında kullanıcı arama](./media/tshoot-connect-password-hash-synchronization/searchcs.png)  

    f. Aradığınız kullanıcıyı bulun ve tüm öznitelikleri görmek için **Özellikler'i** tıklatın. Kullanıcı arama sonucunda değilse, [filtreleme kurallarınızı](how-to-connect-sync-configure-filtering.md) doğrulayın ve [Uygula'yı](how-to-connect-sync-configure-filtering.md#apply-and-verify-changes) çalıştırdığınızdan emin olun ve kullanıcının Connect'te görünmesi için değişiklikleri doğrulayın.

    g. Nesnenin son bir haftaya ait parola eşitleme ayrıntılarını görmek için **Günlük'ü**tıklatın.  

    ![Nesne günlüğü ayrıntıları](./media/tshoot-connect-password-hash-synchronization/csobjectlog.png)  

    Nesne günlüğü boşsa, Azure AD Connect Active Directory'den parola karmasını okuyamaz. Bağlantı Hataları ile sorun giderme devam edin. **Başarıdan**başka bir değer görürseniz, [Parola eşitleme günlüğündeki](#password-sync-log)tabloya bakın.

    h. **Soy** sekmesini seçin ve **PasswordSync** sütunundaki en az bir eşitleme kuralının **Doğru**olduğundan emin olun. Varsayılan yapılandırmada, eşitleme kuralının adı **AD'den - Kullanıcı Hesabı Etkin'den**gelir.  

    ![Bir kullanıcı hakkında soy bilgileri](./media/tshoot-connect-password-hash-synchronization/cspasswordsync.png)  

    i. Kullanıcı özniteliklerinin listesini görüntülemek için **Metaverse Nesne Özellikleri'ni** tıklatın.  

    ![Metaverse bilgi](./media/tshoot-connect-password-hash-synchronization/mvpasswordsync.png)  

    **BulutFiltreleştirilmiş** öznitelik bulunmadığını doğrulayın. Etki alanı özniteliklerinin (domainFQDN ve domainNetBios) beklenen değerlere sahip olduğundan emin olun.

    j. **Bağlayıcılar** sekmesini tıklatın. Hem şirket içi Active Directory hem de Azure AD'ye bağlı bağlayıcıları gördüğünüzden emin olun.

    ![Metaverse bilgi](./media/tshoot-connect-password-hash-synchronization/mvconnectors.png)  

    k. Azure AD'yi temsil eden satırı seçin, **Özellikler'i**tıklatın ve ardından **Soy** sekmesini tıklatın. Bağlayıcı alan **nesnesinin, ParolaSync** sütununda **True**olarak ayarlanmış bir giden kural alabilmeli. Varsayılan yapılandırmada, eşitleme kuralının adı **AAD'ye out - User Join**.  

    ![Bağlayıcı Alan Nesne Özellikleri iletişim kutusu](./media/tshoot-connect-password-hash-synchronization/cspasswordsync2.png)  

### <a name="password-sync-log"></a>Parola eşitleme günlüğü

Durum sütunu aşağıdaki değerlere sahip olabilir:

| Durum | Açıklama |
| --- | --- |
| Başarılı |Parola başarıyla eşitlendi. |
| FilteredByTarget |Parola Kullanıcı olarak ayarlanır **bir sonraki oturumda şifreyi değiştirmek gerekir.** Parola eşitlenmedi. |
| NoTargetConnection |Metaverse veya Azure AD bağlayıcı sı'nda nesne yok. |
| KaynakConnectorNotPresent |Şirket içi Active Directory bağlayıcı alanında nesne bulunamadı. |
| TargetNotExportedToDirectory |Azure AD bağlayıcısı alanındaki nesne henüz dışa aktarılmadı. |
| GöçCheckDetailsForMoreInfo |Günlük girişi 1.0.9125.0 oluşturmadan önce oluşturulmuştur ve eski durumunda gösterilir. |
| Hata |Hizmet bilinmeyen bir hata döndürür. |
| Bilinmiyor |Bir yığın parola işbirliği işlemeye çalışırken bir hata oluştu.  |
| MissingAttribute |Azure AD Etki Alanı Hizmetleri tarafından gereken belirli öznitelikler (örneğin, Kerberos karması) kullanılamaz. |
| Yeniden DenemeRequestedByTarget |Azure AD Etki Alanı Hizmetleri tarafından gereken belirli öznitelikler (örneğin, Kerberos karma) önceden kullanılamadı. Kullanıcının parola karmasını yeniden eşitleme girişimi yapılır. |

## <a name="scripts-to-help-troubleshooting"></a>Sorun gidermeye yardımcı olacak komut dosyaları

### <a name="get-the-status-of-password-sync-settings"></a>Parola eşitleme ayarlarının durumunu alın

```powershell
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update the script to use the appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a>Tüm parolaların tam eşitlesini tetikleme

> [!NOTE]
> Bu komut dosyalarını yalnızca bir kez çalıştırın. Birden fazla kez çalıştırmak gerekiyorsa, başka bir şey sorundur. Sorunu gidermek için Microsoft desteğine başvurun.

Aşağıdaki komut dosyasını kullanarak tüm parolaların tam eşitlesini tetikleyebilirsiniz:

```powershell
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD Connect eşitlemeile parola karma senkronizasyonu uygulama](how-to-connect-password-hash-synchronization.md)
* [Azure AD Connect Sync: Eşitleme seçeneklerini özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)