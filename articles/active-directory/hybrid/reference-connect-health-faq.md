---
title: Azure Active Directory Connect Health SSS - Azure | Microsoft Dokümanlar
description: Bu SSS, Azure AD Bağlantı Durumu ile ilgili soruları yanıtlar. Bu SSS; faturalama modeli, özellikler, sınırlamalar ve destek dahil olmak üzere hizmetin kullanımı hakkındaki soruları kapsar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: f1b851aa-54d7-4cb4-8f5c-60680e2ce866
ms.service: active-directory
ms.subservice: hybrid
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 07/18/2017
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f0c6484f46731e0ff2d16d00cb0038202511d193
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80331072"
---
# <a name="azure-ad-connect-health-frequently-asked-questions"></a>Azure AD Connect Health sık sorulan sorular
Bu makalede, Azure Etkin Dizini (Azure AD) Bağlantı Durumu hakkında sık sorulan soruların (SSSS) yanıtları yer almaktadır. Bu SSS'ler, faturalandırma modelini, yetenekleri, sınırlamaları ve desteği içeren hizmetin nasıl kullanılacağı yla ilgili soruları kapsar.

## <a name="general-questions"></a>Genel sorular
**S: Birden çok Azure REKLAM dizinlerini yönetiyorum. Azure Active Directory Premium'a nasıl geçerim?**

Farklı Azure AD kiracıları arasında geçiş yapmak için sağ üst köşedeki şu anda oturum açmış **Olan Kullanıcı Adını** seçin ve ardından uygun hesabı seçin. Hesap burada listelenmemişse, **Oturum Aç'ı**seçin ve ardından Azure Active Directory Premium'un oturum açma özelliğine sahip dizinin genel yönetici kimlik bilgilerini kullanın.

**S: Kimlik rollerinin hangi sürümü Azure AD Connect Health tarafından desteklenir?**

Aşağıdaki tabloda roller ve desteklenen işletim sistemi sürümleri listelenir.

|Rol| İşletim sistemi / Sürüm|
|--|--|
|Active Directory Federasyon Hizmetleri (AD FS)| <ul> <li> Windows Server 2008 R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|
|Azure AD Connect | Sürüm 1.0.9125 veya üstü|
|Active Directory Etki Alanı Hizmetleri (AD DS)| <ul> <li> Windows Server 2008 R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|

Hizmet tarafından sağlanan özelliklerin role ve işletim sistemine bağlı olarak farklılık kaydedebileceğini unutmayın. Başka bir deyişle, tüm özellikler tüm işletim sistemi sürümleri için kullanılamayabilir. Ayrıntılar için özellik açıklamalarına bakın.

**S: Altyapımı izlemek için kaç lisansa ihtiyacım var?**

* İlk Connect Health Agent için en az bir Azure AD Premium lisansı gerektirir.
* Her ek kayıtlı aracı için 25 ek Azure AD Premium lisansı gerektirir.
* Aracı sayısı, izlenen tüm rollerde (AD FS, Azure AD Connect ve/veya AD DS) kayıtlı toplam aracı sayısına eşdeğerdir.
* AAD Connect Health lisanslama, lisansı belirli kullanıcılara atamanızı gerektirmez. Yalnızca gerekli sayıda geçerli lisansa sahip olmanız gerekir.

Lisans bilgileri Azure REKLAM [Fiyatlandırması sayfasında](https://aka.ms/aadpricing)da bulunur.

Örnek:

| Kayıtlı acenteler | Gerekli lisanslar | Örnek izleme yapılandırması |
| ------ | --------------- | --- |
| 1 | 1 | 1 Azure AD Connect sunucusu |
| 2 | 26| 1 Azure AD Connect sunucusu ve 1 etki alanı denetleyicisi |
| 3 | 51 | 1 Active Directory Federation Services (AD FS) sunucusu, 1 AD FS proxy ve 1 etki alanı denetleyicisi |
| 4 | 76 | 1 AD FS sunucusu, 1 AD FS proxy ve 2 etki alanı denetleyicileri |
| 5 | 101 | 1 Azure AD Connect sunucusu, 1 AD FS sunucusu, 1 AD FS proxy ve 2 etki alanı denetleyicisi |

**S: Azure AD Connect Health Azure Almanya Bulutu'na destek veriyor mu?**

Azure AD Connect Health, [eşitleme hataları raporu özelliği](how-to-connect-health-sync.md#object-level-synchronization-error-report)dışında Almanya Bulut'ta desteklenmez.

| Roller | Özellikler | Alman Bulutu'nda Desteklendi |
| ------ | --------------- | --- |
| Eşitleme için Sistem Durumu'na Bağlayın | İzleme / Kavrayış / Uyarılar / Analiz | Hayır |
|  | Eşitleme hata raporu | Evet |
| ADFS için Sistem Durumu'na Bağlanma | İzleme / Kavrayış / Uyarılar / Analiz | Hayır |
| ADDS için Sağlık Bağlayın | İzleme / Kavrayış / Uyarılar / Analiz | Hayır |

Eşitleme için Connect Health aracı bağlantısı sağlamak için, yükleme [gereksinimini](how-to-connect-health-agent-install.md#outbound-connectivity-to-the-azure-service-endpoints) buna göre yapılandırın.

## <a name="installation-questions"></a>Kurulum soruları

**S: Azure AD Connect Sağlık Aracısını tek tek sunucular üzerinde yüklemenin etkisi nedir?**

Microsoft Azure AD Connect Health Agent, AD FS, web application proxy sunucuları, Azure AD Connect (eşitleme) sunucuları, etki alanı denetleyicileri yüklemenin etkisi CPU, bellek tüketimi, ağ bant genişliği ve depolama açısından çok azdır.

Aşağıdaki sayılar bir yaklaşımdır:

* CPU tüketimi: ~1-5% artış.
* Bellek tüketimi: Toplam sistem belleği kadar% 10.

> [!NOTE]
> Aracı Azure ile iletişim kuramıyorsa, aracı verileri tanımlı bir maksimum sınır için yerel olarak depolar. Aracı, "önbelleğe alınmış" verilerin üzerine "en az son hizmet verilen" olarak yazar.
>
>

* Azure AD Connect Health Aracıları için yerel arabellek depolama: ~20 MB.
* AD FS sunucuları için, Azure AD Connect Health Aracıları için AD FS denetim kanalı için tüm denetim verilerini üzerine yazılmadan önce işlemek için 1.024 MB (1 GB) bir disk alanı sağlamanızı öneririz.

**S: Azure AD Connect Sağlık Aracıları'nın yüklenmesi sırasında sunucularımı yeniden başlatmam gerekecek mi?**

Hayır. Aracıların yüklenmesi, sunucuyu yeniden başlatmanızı gerektirmez. Ancak, bazı ön koşul adımlarının yüklenmesi sunucunun yeniden başlatılmasını gerektirebilir.

Örneğin, Windows Server 2008 R2'de .NET 4.5 Framework'ün yüklenmesi için sunucunun yeniden başlatılması gerekiyor.

**S: Azure AD Connect Health geçiş http proxy ile çalışır mı?**

Evet. Devam eden işlemler için, giden HTTP isteklerini iletmek için bir HTTP proxy'si kullanacak şekilde Sağlık Aracısı yapılandırabilirsiniz.
[Sağlık Aracıları için HTTP Proxy yapılandırma](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy)hakkında daha fazla bilgi edinin.

Aracı kaydı sırasında bir proxy yapılandırmanız gerekiyorsa, Internet Explorer Proxy ayarlarınızı önceden değiştirmeniz gerekebilir.

1. Internet Explorer > **Ayarlar** > **Internet Seçenekleri** > **Bağlantılar** > LAN**Ayarları**açın.
2. **LAN'ınız için Proxy Sunucusu Kullan'ı**seçin.
3. HTTP ve HTTPS/Secure için farklı proxy bağlantı noktalarınız varsa **Gelişmiş'i** seçin.

**S: Azure AD Connect Health, HTTP yakınlık bilgilerine bağlanırken Temel kimlik doğrulamasını destekliyor mu?**

Hayır. Temel kimlik doğrulaması için rasgele bir kullanıcı adı ve parola belirten bir mekanizma şu anda desteklenmez.

**S: Azure AD Connect Health Agent'ın çalışması için hangi güvenlik duvarı bağlantı noktalarını açmam gerekir?**

Güvenlik duvarı bağlantı noktaları ve diğer bağlantı gereksinimleri listesi için [gereksinimler bölümüne](how-to-connect-health-agent-install.md#requirements) bakın.

**S: Azure AD Connect Health portalında neden aynı ada sahip iki sunucu görüyorum?**

Bir aracıyı sunucudan kaldırdığınızda, sunucu Otomatik olarak Azure AD Connect Health portalından kaldırılmaz. Bir aracıyı bir sunucudan el ile kaldırırsanız veya sunucunun kendisini kaldırırsanız, sunucu girişini Azure AD Connect Health portalından el ile silmeniz gerekir.

Bir sunucuyu yeniden görüntüleyebilir veya aynı ayrıntılara (makine adı gibi) sahip yeni bir sunucu oluşturabilirsiniz. Zaten kayıtlı olan sunucuyu Azure AD Connect Health portalından kaldırmadıysanız ve aracıyı yeni sunucuya yüklediyseniz, aynı ada sahip iki giriş görebilirsiniz.

Bu durumda, eski sunucuya ait girişi el ile silin. Bu sunucunun verileri güncel olmalıdır.

## <a name="health-agent-registration-and-data-freshness"></a>Sağlık Aracısı kayıt ve veri tazeliği

**S: Sağlık Aracısı kayıt hatalarının yaygın nedenleri nelerdir ve sorunları nasıl giderebilirim?**

Sağlık aracısı aşağıdaki olası nedenlerden dolayı kayıt başarısız olabilir:

* Güvenlik duvarı trafiği engellediği için aracı gerekli uç noktalarla iletişim kuramıyor. Bu özellikle web uygulaması proxy sunucularında yaygındır. Gerekli uç noktalara ve bağlantı noktalarına giden iletişime izin verdiğinden emin olun. Ayrıntılar için [gereksinimler bölümüne](how-to-connect-health-agent-install.md#requirements) bakın.
* Giden iletişim, ağ katmanı tarafından TLS denetimine tabi tutulur. Bu, aracının kullandığı sertifikanın denetim sunucusu/varlığı tarafından değiştirilmesine neden olur ve aracı kaydını tamamlamak için gereken adımlar başarısız olur.
* Kullanıcının aracının kaydını gerçekleştirmek için erişimi yoktur. Genel yöneticiler varsayılan olarak erişebilir. Diğer kullanıcılara erişimi devretmek için [Role Based Access Denetimi'ni](how-to-connect-health-operations.md#manage-access-with-role-based-access-control) kullanabilirsiniz.

**S: "Sağlık Hizmeti verilerinin güncel olmadığı" konusunda uyarı alıyorum. Sorunu nasıl giderebilirim?**

Azure AD Connect Health, son iki saat içinde sunucudan tüm veri noktalarını almadığında uyarıyı oluşturur. [Devamını oku](how-to-connect-health-data-freshness.md).

## <a name="operations-questions"></a>Operasyon soruları
**S: Web uygulaması proxy sunucularında denetimi etkinleştirmem gerekiyor mu?**

Hayır, web uygulaması proxy sunucularında denetim etkin olması gerekmez.

**S: Azure AD Connect Sistem Durumu Uyarıları nasıl çözülür?**

Azure AD Connect Health uyarıları bir başarı koşuluyla çözülür. Azure AD Connect Sağlık Aracıları başarı koşullarını düzenli aralıklarla algılar ve hizmete bildirir. Birkaç uyarı için bastırma zamana dayalıdır. Başka bir deyişle, aynı hata durumu uyarı nın üretilmesinden itibaren 72 saat içinde gözlenmezse, uyarı otomatik olarak çözülür.

**S: "Test Kimlik Doğrulama İsteği (Sentetik İşlem) belirteci alamadığı konusunda uyarı alıyorum." Sorunu nasıl giderebilirim?**

AD FS için Azure AD Connect Health, BIR AD FS sunucusuna yüklenen Sistem Durumu Aracısı, Sağlık Aracısı tarafından başlatılan sentetik bir işlemin parçası olarak bir belirteç elde edemediğinde bu uyarıyı oluşturur. Sağlık aracısı yerel sistem bağlamını kullanır ve kendine güvenen bir taraf için bir belirteç almaya çalışır. Bu, AD FS'nin belirteç verme durumunda olduğundan emin olmak için tüm bunları yakalama testidir.

Sağlık Aracısı AD FS çiftlik adını çözemediğinden çoğu zaman bu test başarısız olur. AD FS sunucuları bir ağ yük dengeleyicilerinin arkasındaysa ve istek yük dengeleyicisinin arkasındaki düğümden başlatılırsa (yük bakiyeleyicisinin önünde ki normal istemcinin aksine) bu durum olabilir. Bu durum, "C:\Windows\System32\drivers\etc" altında bulunan "hosts" dosyasının AD FS sunucusunun IP adresini veya AD FS çiftlik adı (sts.contoso.com gibi) için bir döngü ip adresini (127.0.0.0.1) içerecek şekilde güncellenmesiyle düzeltilebilir. Ana bilgisayar dosyasının eklenmesi, ağ çağrısını kısa devre olarak devre dışı katarak Sağlık Aracısı'nın belirteci almasına olanak tanır.

**S: Benim makineleri son ransomware saldırıları için yamalı DeğİlOLDUĞUNU belirten bir e-posta var. Bu e-postayı neden aldım?**

Azure AD Connect Health hizmeti, gerekli düzeltme eklerinin yüklendiğinden emin olmak için izlediği tüm makineleri taradı. En az bir makinekritik düzeltme emaları yoksa e-posta kiracı yöneticilere gönderildi. Bu belirlemeyi yapmak için aşağıdaki mantık kullanılmıştır.
1. Makineye yüklenen tüm düzeltmeleri bulun.
2. Tanımlanan listeden HotFixes'lerden en az birinin bulunip bulunmadığını kontrol edin.
3. Evet ise, makine korunur. Değilse, makine saldırı için risk altındadır.

Bu denetimi el ile gerçekleştirmek için aşağıdaki PowerShell komut dosyasını kullanabilirsiniz. Yukarıdaki mantığı uygular.

```powershell
Function CheckForMS17-010 ()
{
    $hotfixes = "KB3205409", "KB3210720", "KB3210721", "KB3212646", "KB3213986", "KB4012212", "KB4012213", "KB4012214", "KB4012215", "KB4012216", "KB4012217", "KB4012218", "KB4012220", "KB4012598", "KB4012606", "KB4013198", "KB4013389", "KB4013429", "KB4015217", "KB4015438", "KB4015546", "KB4015547", "KB4015548", "KB4015549", "KB4015550", "KB4015551", "KB4015552", "KB4015553", "KB4015554", "KB4016635", "KB4019213", "KB4019214", "KB4019215", "KB4019216", "KB4019263", "KB4019264", "KB4019472", "KB4015221", "KB4019474", "KB4015219", "KB4019473"

    #checks the computer it's run on if any of the listed hotfixes are present
    $hotfix = Get-HotFix -ComputerName $env:computername | Where-Object {$hotfixes -contains $_.HotfixID} | Select-Object -property "HotFixID"

    #confirms whether hotfix is found or not
    if (Get-HotFix | Where-Object {$hotfixes -contains $_.HotfixID})
    {
        "Found HotFix: " + $hotfix.HotFixID
    } else {
        "Didn't Find HotFix"
    }
}

CheckForMS17-010

```

**S: PowerShell cmdlet <i>Get-MsolDirSyncProvisioningError</i> neden daha az eşitleme hataları gösterir?**

<i>Get-MsolDirSyncProvisioningError</i> yalnızca DirSync sağlama hatalarını döndürür. Bunun yanı sıra, Connect Health portalı dışa aktarma hataları gibi diğer eşitleme hata türlerini de gösterir. Bu, Azure AD Connect delta sonucuyla tutarlıdır. [Azure AD Connect Eşitleme hataları](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-sync-errors)hakkında daha fazla bilgi edinin.

**S: ADFS denetimlerim neden oluşturulmuyor?**

Denetim günlüklerinin devre dışı bırakıldığından emin olmak için lütfen PowerShell cmdlet <i>Get-AdfsProperties -AuditLevel'i</i> kullanın. [ADFS denetim günlükleri](https://docs.microsoft.com/windows-server/identity/ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server#auditing-levels-in-ad-fs-for-windows-server-2016)hakkında daha fazla bilgi edinin. ADFS sunucusuna itilen gelişmiş denetim ayarları varsa, auditpol.exe ile yapılan tüm değişikliklerin üzerine yazıldığına dikkat edin (Oluşturulan Uygulama Yapılandırılmazsa olay). Bu durumda, lütfen yerel güvenlik ilkesini Uygulama Oluşturulan hataları ve başarıyı günlüğe kaydetmek için ayarlayın.

**S: Aracı sertifikası son kullanma tarihinden önce otomatik olarak ne zaman yenilenecek?**
Acente sertifikası, son kullanma tarihinden **6 ay** önce otomatik olarak yenilenecektir. Yenilenmezse, lütfen aracının ağ bağlantısının kararlı olduğundan emin olun. Aracı hizmetlerini yeniden başlatın veya en son sürüme güncelleştirin, sorunu da çözebilir.


## <a name="related-links"></a>İlgili bağlantılar
* [Azure AD Connect Health](whatis-hybrid-identity-health.md)
* [Azure AD Connect Health Aracısı yüklemesi](how-to-connect-health-agent-install.md)
* [Azure AD Connect Sistem işlemleri](how-to-connect-health-operations.md)
* [Azure AD Connect Health'i AD FS ile Kullanma](how-to-connect-health-adfs.md)
* [Eşitleme için Azure AD Connect Health'i kullanma](how-to-connect-health-sync.md)
* [Azure AD Connect Health'i AD DS ile Kullanma](how-to-connect-health-adds.md)
* [Azure AD Connect Health sürüm geçmişi](reference-connect-health-version-history.md)
