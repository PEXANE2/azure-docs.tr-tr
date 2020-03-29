---
title: Azure AD'de eski aygıtlar nasıl yönetilir | Microsoft Dokümanlar
description: Azure Active Directory'deki kayıtlı aygıtlar veritabanınızdan eski aygıtları nasıl kaldırmış oyla öğreneceğinizi öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: spunukol
ms.collection: M365-identity-device-management
ms.openlocfilehash: 46be728216ed4b9c9e84c1c7f68c5ddf2051f42b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672323"
---
# <a name="how-to-manage-stale-devices-in-azure-ad"></a>Nasıl Kullanılır: Azure AD'de eski aygıtları yönetme

İdeal koşullarda, kayıtlı cihazların yaşam döngüsünün tamamlanması için, bu cihazlara artık gerek kalmadığında kayıtlarının silinmesi gerekir. Bununla birlikte, örneğin kayıp, çalınmış, bozulmuş cihazlardan veya işletim sistemi yeniden yüklemelerinden dolayı ortamınızda eski cihazlar bulunur. BT yöneticisi olarak, büyük olasılıkla eski cihazları kaldırmak için bir yönteminiz olmasını istersiniz; böylelikle kaynaklarınızın gerçekten yönetilmesi gereken cihazların yönetimine odaklanmasını sağlayabilirsiniz.

Bu makalede, ortamınızdaki eski cihazları verimli bir yöntemle nasıl yönetebileceğinizi öğreneceksiniz.
  

## <a name="what-is-a-stale-device"></a>Eski cihaz nedir?

Eski cihaz, Azure AD'ye kayıtlı olan ama belirli bir zaman çerçevesinde herhangi bir bulut uygulamasına erişmek için kullanılmamış olan cihazdır. Eski cihazlar, kiracıdaki cihazlarınızı ve kullanıcılarınızı yönetme ve destekleme becerinizi etkiler çünkü: 

- Çift cihazlar yardım masanızın hangi cihazın şu anda etkin olduğunu belirlemesini zorlaştırabilir.
- Artan sayıda aygıt, Azure AD bağlantı eşitlemelerinin süresini artırarak gereksiz aygıt geri yazmaları oluşturur.
- Genel sağlık açısından ve uyumluluğun gereklerini karşılamak için, cihazların temiz durumda olmasını isteyebilirsiniz. 

Azure AD'deki eski cihazlar kuruluşunuzdaki cihazlara yönelik genel yaşam döngüsü ilkelerine müdahale edebilir.

## <a name="detect-stale-devices"></a>Eski cihazları algılama

Eski cihaz, belirli bir zaman çerçevesinde hiçbir bulut uygulamasına erişmek için kullanılmamış kayıtlı cihaz olarak tanımlandığından, eski cihazları algılamak için zaman damgasıyla ilgili bir özellik gerekir. Azure AD'de bu özellik **ApproximateLastLogonTimestamp** veya **etkinlik zaman damgası** olarak adlandırılır. Şimdi ile **etkinlik zaman damgasının** değeri arasındaki delta etkin cihazlar için tanımladığınız zaman çerçevesini aşıyorsa, cihazın eski olduğu düşünülür. Bu **etkinlik zaman damgası** şu anda genel önizleme aşamasındadır.

## <a name="how-is-the-value-of-the-activity-timestamp-managed"></a>Etkinlik zaman damgasının değeri nasıl yönetilir?  

Etkinlik zaman damgasının hesaplanması, bir cihazın kimlik doğrulama girişimiyle tetiklenir. Azure AD aşağıdaki durumlarda etkinlik zaman damgasını hesaplar:

- [Yönetilen aygıtlar](../conditional-access/require-managed-devices.md) veya onaylı istemci [uygulamaları](../conditional-access/app-based-conditional-access.md) gerektiren Koşullu Erişim ilkeleri tetiklendi.
- Azure AD'ye katılmış veya hibrit Azure AD'ye katılmış Windows 10 cihazları ağda etkin olduğunda. 
- Intune tarafından yönetilen cihazlar hizmete giriş yaptığında.

Etkinlik zaman damgasının varolan değeri ile geçerli değer arasındaki delta 14 günden (+/-5 gün varyansı) fazlaysa, varolan değer yeni değerle değiştirilir.

## <a name="how-do-i-get-the-activity-timestamp"></a>Etkinlik zaman damgasını nasıl alabilirim?

Etkinlik zaman damgasının değerini almak için iki seçeneğiniz vardır:

- Azure portaldaki [cihazlar sayfasının](https://portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/Devices)**Etkinlik** sütunu

    ![Etkinlik zaman damgası](./media/manage-stale-devices/01.png)

- [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet'i

    ![Etkinlik zaman damgası](./media/manage-stale-devices/02.png)

## <a name="plan-the-cleanup-of-your-stale-devices"></a>Eski cihazların temizliğini planlama

Ortamınızda eski cihazları verimli bir şekilde temizlemek için, ilgili bir ilke tanımlamalısınız. Bu ilke, eski cihazlarla ilgili tüm konuları yakaladığınızdan emin olmanıza yardımcı olur. Aşağıdaki bölümlerde yaygın ilke konularına ilişkin örnekler sağlanmıştır. 

### <a name="cleanup-account"></a>Temizleme hesabı

Azure AD'de bir cihazı güncelleştirmek için, aşağıdaki rollerden birinin atandığı bir hesap gerekir:

- Genel Yönetici
- Bulut Aygıt Yöneticisi
- Intune Hizmet Yöneticisi

Temizleme ilkenizde, gerekli rollerin atandığı hesapları seçin. 

### <a name="timeframe"></a>Zaman çerçevesi

Eski cihaz için göstergeniz olacak bir zaman çerçevesi tanımlayın. Zaman diliminizi tanımlarken, etkinlik zaman damgasını değerinize güncelleştirmek için belirtilen pencereyi faktör haline getirin. Örneğin, 21 günden daha genç (varyans içerir) bir zaman damgası eski bir aygıt için bir gösterge olarak dikkate almamalısınız. Eski olmayan bir cihazın eski gibi görünmesine neden olabilecek senaryolar vardır. Örneğin, etkilenen cihazın sahibi, eski cihazlara ilişkin zaman çerçevenizi aşan bir süre  tatile çıkmış veya hastalık iznine ayrılmış olabilir.

### <a name="disable-devices"></a>Cihazları devre dışı bırakma

Eski gibi görünen bir cihazı hemen silmeniz önerilmez, çünkü yanlış pozitif göstergelerde silme işlemini geri almak mümkün değildir. En iyi yöntem, cihazı silmeden önce belirli bir bekleme süresince devre dışı bırakmaktır. İlkenizde, silmeden önce cihazı devre dışı bırakmak için bir zaman çerçevesi tanımlayın.

### <a name="mdm-controlled-devices"></a>MDM tarafından denetlenen cihazlar

Cihazınız Intune'un veya başka bir MDM çözümünün denetimi altındaysa, cihazı devre dışı bırakmadan veya silmeden önce yönetim sisteminden kaldırın.

### <a name="system-managed-devices"></a>Sistem tarafından yönetilen cihazlar

Sistem tarafından yönetilen cihazları silmeyin. Bunlar genellikle Otomatik Pilot gibi aygıtlardır. Silindikten sonra, bu aygıtlar yeniden sağlanabilir. `get-msoldevice` cmdlet'i sistem tarafından yönetilen cihazları varsayılan olarak dışlar. 

### <a name="hybrid-azure-ad-joined-devices"></a>Hibrit Azure AD’ye katılmış cihazlar

Hibrit Azure AD'ye katılmış cihazlarınızın şirket içi eski cihaz yönetimi ilkelerinize uyması gerekir. 

Azure AD'yi temizlemek için:

- **Windows 10 cihazları** - Windows 10 cihazlarını şirket içi AD'nizde devre dışı bırakın veya silin, sonra da Azure AD Connect'in değişen cihaz durumunu Azure AD'ye eşitlemesini sağlayın.
- **Windows 7/8** - Önce şirket içi AD'nizde Windows 7/8 aygıtlarını devre dışı veya sile. Windows 7/8 cihazlarını Azure AD'de devre dışı bırakmak veya silmek için Azure AD Connect kullanamazsınız. Bunun yerine, şirket içi değişikliği yaptığınızda, Azure AD'de devre dışı/silmeniz gerekir.

> [!NOTE]
>* Şirket içi AD veya Azure AD'nizdeki aygıtları silerken, istemciye yapılan kaydı kaldırmaz. Yalnızca kimlik olarak aygıtı kullanan kaynaklara erişimi engeller (örn. Koşullu Erişim). [İstemci deki kaydın](faq.md#hybrid-azure-ad-join-faq)nasıl kaldırılabildiğini hakkında ek bilgileri okuyun.
>* Windows 10 aygıtını yalnızca Azure AD'de silerken, Azure AD bağlantısını kullanarak şirket içi cihazınızdan ancak "Beklemede" durumunda yeni bir nesne olarak aygıtı yeniden senkronize edin. Cihaza yeniden kaydedilmesi gerekir.
>* Aygıtın Windows 10/Server 2016 aygıtları için eşitleme kapsamından kaldırılması Azure AD aygıtını siler. Eşitleme kapsamına geri eklemek yeni bir nesneyi "Bekleme" durumuna yerleştirilecektir. Aygıtın yeniden kaydedilmesi gerekir.
>* Senkronize etmek için Windows 10 aygıtları için Azure AD Connect kullanmıyorsanız (örn. yalnızca kayıt için AD FS kullanmak), Windows 7/8 aygıtlarına benzer yaşam döngüsünü yönetmeniz gerekir.


### <a name="azure-ad-joined-devices"></a>Azure AD’ye katılmış cihazlar

Azure AD'ye katılmış cihazları Azure AD'de devre dışı bırakın veya silin.

> [!NOTE]
>* Azure AD aygıtını silerken istemcideki kayıt kaldırmaz. Yalnızca kimlik olarak aygıtı kullanan kaynaklara erişimi engeller (örn. Koşullu Erişim). 
>* [Azure AD'de nasıl katılmadan katılacağız](faq.md#azure-ad-join-faq) hakkında daha fazla bilgi edinin 

### <a name="azure-ad-registered-devices"></a>Azure AD kayıtlı cihazlar

Azure AD'ye kayıtlı cihazları Azure AD'de devre dışı bırakın veya silin.

> [!NOTE]
>* Azure AD'de Azure AD kayıtlı bir aygıtı silerken istemciye yapılan kayıt kaldırılmaz. Yalnızca kimlik olarak aygıtı kullanan kaynaklara erişimi engeller (örn. Koşullu Erişim).
>* [İstemci deki bir kaydın nasıl kaldırılılabildiğini](faq.md#azure-ad-register-faq) daha fazla okuyun

## <a name="clean-up-stale-devices-in-the-azure-portal"></a>Eski cihazları Azure portalda temizleme  

Eski cihazları Azure portalda temizleyebilirsiniz ama bu işlemi PowerShell betiği kullanarak gerçekleştirmek daha verimli olur. Zaman damgası filtresini kullanmak ve Otomatik Pilot gibi sistem tarafından yönetilen aygıtları filtrelemek için en son PowerShell V1 modüllerini kullanın. Bu noktada PowerShell V2'nin kullanılması önerilmez.

Tipik bir yordam aşağıdaki adımlardan oluşur:

1. [Connect-MsolService](/powershell/module/msonline/connect-msolservice?view=azureadps-1.0) cmdlet'ini kullanarak Azure Active Directory'ye bağlanma
1. Cihaz listesini alma
1. [Disable-MsolDevice](/powershell/module/msonline/disable-msoldevice?view=azureadps-1.0) cmdlet'ini kullanarak cihazı devre dışı bırakma. 
1. Cihazı silmeden önce seçtiğiniz yetkisiz kullanım süresinin tamamlanmasını bekleyin.
1. [Remove-MsolDevice](/powershell/module/msonline/remove-msoldevice?view=azureadps-1.0) cmdlet'ini kullanarak cihazı kaldırma.

### <a name="get-the-list-of-devices"></a>Cihaz listesini alma

Tüm cihazları almak ve döndürülen verileri CSV dosyasında depolamak için:

```PowerShell
Get-MsolDevice -all | select-object -Property Enabled, DeviceId, DisplayName, DeviceTrustType, Approxi
mateLastLogonTimestamp | export-csv devicelist-summary.csv
```

Dizininizde çok sayıda aygıt varsa, döndürülen aygıt sayısını daraltmak için zaman damgası filtresini kullanın. Zaman damgası belirli bir tarihten daha eski olan tüm cihazları almak ve döndürülen verileri CSV dosyasında depolamak için: 

```PowerShell
$dt = [datetime]’2017/01/01’
Get-MsolDevice -all -LogonTimeBefore $dt | select-object -Property Enabled, DeviceId, DisplayName, DeviceTrustType, ApproximateLastLogonTimestamp | export-csv devicelist-olderthan-Jan-1-2017-summary.csv
```

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

### <a name="why-is-the-timestamp-not-updated-more-frequently"></a>Zaman damgası neden daha sık güncelleştirilmiyor?

Zaman damgası cihaz yaşam döngüsü senaryolarını desteklemek için güncelleştirilir. Bu bir denetim değildir. Cihazda daha sık güncelleştirmeler için oturum açma denetim günlüklerini kullanın.

### <a name="why-should-i-worry-about-my-bitlocker-keys"></a>BitLocker anahtarlarımla ilgili olarak neden kaygılanmam gerekiyor?

Windows 10 cihazlarında BitLocker anahtarları yapılandırıldığında, bu anahtarlar Azure AD'deki cihaz nesnesinde depolanır. Eski cihazı silerseniz, cihazda depolanan BitLocker anahtarlarını da silersiniz. Eski cihazı silmeden önce, temizleme ilkenizin cihazınızın gerçek yaşam döngüsüyle uyumlu olup olmadığını saptamalısınız. 

### <a name="why-should-i-worry-about-windows-autopilot-devices"></a>Windows Otomatik Pilot aygıtları hakkında neden endişelenmem gerekir?

Bir Azure AD aygıtı bir Windows Otomatik Pilot nesnesi ile ilişkilendirildiğinde, aygıtın gelecekte yeniden kullanımdan kaldırılması durumunda aşağıdaki üç senaryo oluşabilir:
- Beyaz eldiven kullanmadan Windows Autopilot kullanıcı odaklı dağıtımlarla yeni bir Azure AD aygıtı oluşturulur, ancak ZTDID ile etiketlendirilmez.
- Windows Otomatik Pilot kendi kendine dağıtma modu dağıtımları ile, ortak bir Azure AD aygıtı bulunamadığıniçin başarısız olurlar.  (Bu, hiçbir "sahtekar" aygıtın kimlik bilgileri olmadan Azure AD'ye katılmaya çalışmadığından emin olmak için bir güvenlik mekanizmasıdır.) Hata ztdid uyuşmazlığı gösterir.
- Windows Autopilot beyaz eldiven dağıtımlarında, ilişkili bir Azure AD aygıtı bulunamadığından başarısız olurlar. (Arka planda, beyaz eldiven dağıtımları aynı kendi kendine dağıtma modu işlemini kullanır, böylece aynı güvenlik mekanizmalarını uygularlar.)

### <a name="how-do-i-know-all-the-type-of-devices-joined"></a>Katılmış olan tüm cihaz türlerini nasıl bilebilirim?

Farklı türler hakkında daha fazla bilgi edinmek için, bkz. [Cihaz yönetimine genel bakış](overview.md).

### <a name="what-happens-when-i-disable-a-device"></a>Cihazı devre dışı bıraktığımda ne olur?

Cihazın Azure AD'de kimliğini doğrulaması için kullanılan tüm kimlik doğrulamaları reddedilir. Sık karşılaşılan örnekler:

- **Hibrit Azure AD'ye katılan cihaz** - Kullanıcılar cihazı şirket içi etki alanlarında oturum açmak için kullanabilir. Bununla birlikte, Office 365 gibi Azure AD kaynaklarına erişemez.
- **Azure AD'ye katılan cihaz** - Kullanıcılar cihazı oturum açmak için kullanamaz. 
- **Mobil cihazlar** - Kullanıcılar Office 365 gibi Azure AD kaynaklarına erişemez. 

## <a name="next-steps"></a>Sonraki adımlar

Azure portal'da cihaz yönetimine ilişkin genel bir bakış edinmek için bkz. [Azure portal'ı kullanarak cihazları yönetme](device-management-azure-portal.md)
