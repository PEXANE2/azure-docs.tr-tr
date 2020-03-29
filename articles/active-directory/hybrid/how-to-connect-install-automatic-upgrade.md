---
title: 'Azure AD Connect: Otomatik yükseltme | Microsoft Dokümanlar'
description: Bu konu, Azure AD Connect eşitlemesinde yerleşik otomatik yükseltme özelliğini açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 6b395e8f-fa3c-4e55-be54-392dd303c472
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/26/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: bfd61b78ca3027ade1f2f48dec33e0a8ed508d3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60349853"
---
# <a name="azure-ad-connect-automatic-upgrade"></a>Azure AD Connect: Otomatik yükseltme
Bu özellik build [1.1.105.0 (Şubat 2016'da yayımlandı)](reference-connect-version-history.md#111050)ile tanıtıldı.  Bu özellik [build 1.1.561'de](reference-connect-version-history.md#115610) güncelleştirildi ve şimdi daha önce desteklenmeyen ek senaryoları destekliyor.

## <a name="overview"></a>Genel Bakış
**Otomatik yükseltme** özelliği yle Azure AD Connect yüklemenizin her zaman güncel olduğundan emin olmak hiç bu kadar kolay olmamıştı. Bu özellik, ekspres yüklemeler ve DirSync yükseltmeleri için varsayılan olarak etkinleştirilir. Yeni bir sürüm yayımlandığında, yüklemeniz otomatik olarak yükseltilir.
Otomatik yükseltme varsayılan olarak aşağıdakiler için etkinleştirilir:

* Express ayarları yükleme ve DirSync yükseltmeleri.
* Express ayarlarının her zaman kullandığı SQL Express LocalDB'ı kullanın. SQL Express ile DirSync de LocalDB kullanın.
* AD hesabı, Express ayarları ve DirSync tarafından oluşturulan varsayılan MSOL_ hesabıdır.
* Metaverse'de 100.000'den az nesne var.

Otomatik yükseltme nin geçerli durumu PowerShell cmdlet `Get-ADSyncAutoUpgrade`ile görüntülenebilir. Aşağıdaki durumlara sahiptir:

| Durum | Açıklama |
| --- | --- |
| Etkin |Otomatik yükseltme etkinleştirildi. |
| Askıya alındı |Yalnızca sistem tarafından ayarlanır. Sistem **şu anda** otomatik yükseltmeleri almaya uygun değildir. |
| Devre dışı |Otomatik yükseltme devre dışı bırakılır. |

**Etkin** ve Devre **Dışı** `Set-ADSyncAutoUpgrade`arasında ' ile değiştirebilirsiniz. Yalnızca sistem askıya **alınan**durumu ayarlamalıdır.  Önce 1.1.750.0 Otomatik yükseltme durumu Askıya ayarlandı otomatik yükseltme durumu Otomatik yükseltme engelleyecek Bu işlev artık otomatik yükseltmeyi engellememesi için değiştirildi.

Otomatik yükseltme, yükseltme altyapısı için Azure AD Connect Health'i kullanıyor. Otomatik yükseltmenin çalışması için, [Office 365 URL'leri ve IP adres aralıklarında](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)belgelenen **Azure AD Connect Health** için proxy sunucunuzdaki URL'leri açtığınızdan emin olun.


**Eşitleme Hizmet Yöneticisi** Kullanıcı Arabirimi sunucuda çalışıyorsa, kullanıcı arabirimi kapatılana kadar yükseltme askıya alınır.

## <a name="troubleshooting"></a>Sorun giderme
Connect yüklemeniz kendisini beklendiği gibi yükseltmezse, sorunun ne olabileceğini öğrenmek için aşağıdaki adımları izleyin.

İlk olarak, otomatik yükseltmenin yeni bir sürümün yayımlandığı ilk gün denenmesini beklememelisiniz. Yükseltme denenmeden önce kasıtlı bir rastgelelik vardır, bu nedenle yüklemeniz hemen yükseltilmezse paniğe kapılmayın.

Bir şeyin doğru olmadığını düşünüyorsanız, `Get-ADSyncAutoUpgrade` otomatik yükseltmenin etkin olduğundan emin olmak için önce çalıştırın.

Ardından, proxy veya güvenlik duvarınızda gerekli URL'leri açtığınızdan emin olun. Otomatik güncelleştirme, [genel bakışta](#overview)açıklandığı gibi Azure AD Connect Health'i kullanıyor. Proxy kullanıyorsanız, Sağlık'ın [proxy sunucusu](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy)kullanacak şekilde yapılandırıldığından emin olun. Ayrıca Azure AD'ye [Sistem Durumu bağlantısını](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) da test edin.

Azure AD bağlantısı doğrulandıysa, olay günlüklerine bakma nın zamanı geldiğinden. Olay görüntüleyicisi başlatın ve **Uygulama** olay günlüğüne bakın. Kaynak Azure AD Connect **Yükseltmesi** ve olay id aralığı **300-399**için bir eventlog filtresi ekleyin.  
![Otomatik yükseltme için Eventlog filtresi](./media/how-to-connect-install-automatic-upgrade/eventlogfilter.png)  

Artık otomatik yükseltme durumuyla ilişkili olay günlüklerini görebilirsiniz.  
![Otomatik yükseltme için Eventlog filtresi](./media/how-to-connect-install-automatic-upgrade/eventlogresult.png)  

Sonuç kodu, duruma genel bir bakış içeren bir önek vardır.

| Sonuç kodu öneki | Açıklama |
| --- | --- |
| Başarılı |Yükleme başarıyla yükseltildi. |
| Yükseltme İptal edildi |Geçici bir koşul yükseltmeyi durdurdu. Tekrar denenecek ve beklenti daha sonra başarılı olmasıdır. |
| YükseltmeNotSupported |Sistem, sistemin otomatik olarak yükseltilmesine engel olan bir yapılandırmaya sahiptir. Devletin değişip değişmediğini görmek için yeniden denenecek, ancak beklenti sistemin el ile yükseltilmesi gerektiği. |

Burada bulduğunuz en yaygın iletilerin listesi vereceksiniz. Tümlerini listelemiyor, ancak sonuç iletisi sorunun ne olduğu konusunda açık olmalıdır.

| Sonuç İletisi | Açıklama |
| --- | --- |
| **Yükseltme İptal edildi** | |
| UpgradeAbortedCouldNotSetUpgradeMarker |Kayıt defterine yazamadı. |
| YükseltmeAbortedInsufficientDatabasePermissions |Yerleşik yöneticiler grubunun veritabanına izinleri yoktur. Bu sorunu gidermek için Azure AD Connect'in en son sürümüne el ile yükseltin. |
| YükseltmeAbortedInsufficientDiskSpace |Yükseltmeyi desteklemek için yeterli disk alanı yoktur. |
| YükseltmeAbortedSecurityGroupsNotPresent |Eşitleme altyapısı tarafından kullanılan tüm güvenlik gruplarını bulup çözemedi. |
| UpgradeAbortedServiceCanNotStarted |NT Hizmeti **Microsoft Azure AD Eşitlemesi** başlatılamamış. |
| YükseltmeAbortedServiceCanNotBeStopped |NT Hizmeti **Microsoft Azure AD Eşitlemesi** duramadı. |
| YükseltmeAbortedServiceIsNotRunning |NT Hizmeti **Microsoft Azure AD Eşitlemesi** çalışmıyor. |
| YükseltmeAbortedSyncCycleDisabled |[Zamanlayıcıdaki](how-to-connect-sync-feature-scheduler.md) Eşitleme seçeneği devre dışı bırakıldı. |
| YükseltmeAbortedSyncExeInUse |[Eşitleme hizmeti yöneticisi Kullanıcı UI](how-to-connect-sync-service-manager-ui.md) sunucuda açıktır. |
| UpgradeAbortedSyncOrConfigurationInProgress |Yükleme sihirbazı çalışıyor veya zamanlayıcının dışında bir eşitleme zamanlandı. |
| **YükseltmeNotSupported** | |
| YükseltmeNotSupportedAdfsSignInMethod | Oturum açma yöntemi olarak Adfs'i seçtiniz. |
| YükseltmeNotSupportedÖzelleştirilmişSyncRules |Yapılandırmaya kendi özel kurallarınızı eklediniz. |
| YükseltmeNotSupportedDeviceWritebackEnabled |Aygıt geri [yazma](how-to-connect-device-writeback.md) özelliğini etkinleştirdin. |
| YükseltmeNotSupportedGroupWritebackEtkin |Grup geri [yazma](how-to-connect-preview.md#group-writeback) özelliğini etkinleştirdin. |
| YükseltmeNotSupportedInvalidPersistedState |Yükleme Express ayarları veya DirSync yükseltmesi değildir. |
| YükseltmeNotSupportedMetaverseSizeExceeeded |Metaverse'de 100.000'den fazla nesne var. |
| YükseltmeNotSupportedMultiForestSetup |Birden fazla ormana bağlanıyorsun. Ekspres kurulum yalnızca bir ormana bağlanır. |
| YükseltmeNotSupportedNonLocalDbInstall |SQL Server Express LocalDB veritabanı kullanmıyorsunuz. |
| YükseltmeNotSupportedNonMsolAccount |[AD DS Bağlayıcı hesabı](reference-connect-accounts-permissions.md#ad-ds-connector-account) artık varsayılan MSOL_ hesabı değildir. |
| YükseltmeNotSupportedNotConfiguredSignInMethod | AAD Connect'i ayarlarken, oturum açma yöntemini seçerken *Yapılandırma yapmayı* seçtiniz. |
| YükseltmeNotSupportedPtaSignInMethod | Oturum açma yöntemi olarak Geçiş Kimlik Doğrulaması'nı seçtiniz. |
| YükseltmeNotSupportedStagingModeEnabled |Sunucu [evreleme modunda](how-to-connect-sync-staging-server.md)olarak ayarlanır. |
| YükseltmeNotSupportedUserWritebackEnabled |Kullanıcı geri [yazma](how-to-connect-preview.md#user-writeback) özelliğini etkinleştirdin. |

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
