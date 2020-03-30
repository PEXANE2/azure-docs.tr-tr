---
title: 'Azure AD Connect eşitlemi: Zamanlayıcı | Microsoft Dokümanlar'
description: Bu konu, Azure AD Connect eşitlemesinde yerleşik zamanlayıcı özelliğini açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 6b1a598f-89c0-4244-9b20-f4aaad5233cf
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/01/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 309adfbebd4f4b615ac1f4061823ca01f3d3ee15
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79261079"
---
# <a name="azure-ad-connect-sync-scheduler"></a>Azure AD Connect eşitleme: Scheduler
Bu konu, Azure AD Connect eşitleme (eşitleme motoru) yerleşik zamanlayıcı açıklar.

Bu özellik build 1.1.105.0 (Şubat 2016 yayımlandı) ile tanıtıldı.

## <a name="overview"></a>Genel Bakış
Azure AD Connect eşitleme, şirket içi dizininizde bir zamanlayıcı kullanarak meydana gelen değişiklikleri eşitler. Biri parola eşitleme ve diğeri nesne/öznitelik eşitleme ve bakım görevleri için olmak üzere iki zamanlayıcı işlemi vardır. Bu konu ikincikapsar.

Önceki sürümlerde, nesneler ve öznitelikler için zamanlayıcı eşitleme altyapısının dışındaydı. Eşitleme işlemini tetiklemek için Windows görev zamanlayıcısı veya ayrı bir Windows hizmeti kullanılır. Zamanlayıcı eşitleme motoruna dahili 1.1 sürümleri ile ve bazı özelleştirme sağlar. Yeni varsayılan eşitleme sıklığı 30 dakikadır.

Zamanlayıcı iki görevden sorumludur:

* **Eşitleme döngüsü.** Değişiklikleri alma, eşitleme ve dışa aktarma işlemi.
* **Bakım görevleri**. Parola sıfırlama ve Cihaz Kayıt Hizmeti (DRS) için anahtarları ve sertifikaları yenileyin. İşlem günlüğündeki eski girişleri temizle.

Zamanlayıcının kendisi her zaman çalışır, ancak bu görevlerden yalnızca birini veya hiçbirini çalıştıracak şekilde yapılandırılabilir. Örneğin, kendi eşitleme döngüsü işlemine sahip olmanız gerekiyorsa, bu görevi zamanlayıcıda devre dışı ancak yine de bakım görevini çalıştırabilirsiniz.

## <a name="scheduler-configuration"></a>Zamanlayıcı yapılandırması
Geçerli yapılandırma ayarlarınızı görmek için PowerShell'e gidin ve çalıştırın. `Get-ADSyncScheduler` Size bu resim gibi bir şey gösterir:

![SyncScheduler'ı alın](./media/how-to-connect-sync-feature-scheduler/getsynccyclesettings2016.png)

Bu cmdlet çalıştırdığınızda **eşitleme komutu veya cmdlet'in kullanılmadığını** görürseniz, PowerShell modülü yüklenmez. Bu sorun, bir etki alanı denetleyicisi veya varsayılan ayarlardan daha yüksek PowerShell kısıtlama düzeylerine sahip bir sunucuda Azure AD Connect çalıştırın durumunda bu sorun olabilir. Bu hatayı görürseniz, `Import-Module ADSync` cmdlet'i kullanılabilir hale getirmek için çalıştırın.

* **İzin Verilen SyncCycleInterval**. Azure AD tarafından izin verilen eşitleme döngüleri arasındaki en kısa zaman aralığı. Bu ayarın daha sık eşitlenemez ve yine de desteklenir.
* **Şu anda etkilisynccycleInterval**. Zamanlama şu anda geçerli. İzin VerilenSyncInterval'den daha sık değilse, CustomizedSyncInterval (eğer ayarlanırsa) ile aynı değere sahiptir. 1.1.281'den önce bir yapı kullanıyorsanız ve Özelleştirilmiş SyncCycleInterval'ı değiştirirseniz, bu değişiklik bir sonraki eşitleme döngüsünden sonra etkinolur. Yapı 1.1.281'den itibaren değişiklik hemen yürürlüğe girer.
* **ÖzelleştirilmişSyncCycleInterval**. Zamanlayıcının varsayılan 30 dakika dışında herhangi bir frekansta çalışmasını istiyorsanız, bu ayarı yapılandırın. Yukarıdaki resimde, zamanlayıcı bunun yerine her saat çalışacak şekilde ayarlanmıştır. Bu ayarı İzin Verilen SyncInterval'den daha düşük bir değere ayarlarsanız, ikincisi kullanılır.
* **SonrakiSyncCyclePolicyType**. Delta ya da Initial. Bir sonraki çalıştırmanın yalnızca delta değişikliklerini işlemesi mi yoksa bir sonraki çalıştırmanın tam alma ve eşitleme yapıp yapmaması gerektiğini tanımlar. İkincisi de herhangi bir yeni veya değiştirilmiş kuralları yeniden işlenir.
* **SonrakiSyncCycleStartTimeInUTC**. Bir sonraki zamanlayıcı bir sonraki eşitleme döngüsünü başlattığında.
* **PurgeRunHistoryInterval**. Zaman işlem günlükleri tutulmalıdır. Bu günlükler eşitleme hizmeti yöneticisinde gözden geçirilebilir. Varsayılan olarak, bu günlükleri 7 gün boyunca tutmaktır.
* **SyncCycleEtkin**. Zamanlayıcının işleminin bir parçası olarak alma, eşitleme ve dışa aktarma işlemlerini çalıştırıp çalıştırmayacağı gösterir.
* **MaintenanceEnabled**. Bakım işleminin etkin olup olmadığını gösterir. Sertifikaları/anahtarları güncelleştirir ve işlem günlüğünü temizler.
* **EvrelemeModeEnabled**. Hazırlama [modu](how-to-connect-sync-staging-server.md) etkin olup olmadığını gösterir. Bu ayar etkinse, dışa aktarımları çalışmaktan bastırır, ancak yine de alma ve eşitleme çalıştırır.
* **ZamançizelgesiSuspended**. Zamanlayıcının çalışmasını geçici olarak engellemek için yükseltme sırasında Bağlan tarafından ayarlayın.

Bu ayarların bazılarını ' `Set-ADSyncScheduler`ile değiştirebilirsiniz. Aşağıdaki parametreler değiştirilebilir:

* ÖzelleştirilmişSyncCycleInterval
* SonrakiSyncCyclePolicyType
* PurgeRunHistoryInterval
* SyncCycleEtkin
* Bakım Etkin

Azure AD Connect'in önceki **yapılarında, isStagingModeEnabled** Set-ADSyncScheduler'da ortaya çıkmıştır. Bu özelliği ayarlamak **desteklenmez.** Özellik **SchedulerSuspended** yalnızca Connect tarafından değiştirilmelidir. PowerShell ile doğrudan bu ayarlamak için **desteklenmeyen.**

Zamanlayıcı yapılandırması Azure AD'de depolanır. Bir evreleme sunucunuz varsa, birincil sunucudaki herhangi bir değişiklik de evreleme sunucusunu etkiler (IsStagingModeEnabled hariç).

### <a name="customizedsynccycleinterval"></a>ÖzelleştirilmişSyncCycleInterval
Sözdizimi:`Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
d - gün, HH - saat, mm - dakika, ss - saniye

Örnek: `Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
Zamanlayıcıyı her 3 saatte bir çalışacak şekilde değiştirir.

Örnek: `Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
Değişiklikler, zamanlayıcıyı günlük çalışacak şekilde değiştirir.

### <a name="disable-the-scheduler"></a>Zamanlayıcıyı devre dışı  
Yapılandırma değişiklikleri yapmanız gerekiyorsa, zamanlayıcıyı devre dışı kılmış olmak istersiniz. Örneğin, [filtreleme yapılandırdığınızda](how-to-connect-sync-configure-filtering.md) veya [eşitleme kurallarında değişiklik yaptığınızda.](how-to-connect-sync-change-the-configuration.md)

Zamanlayıcıyı devre dışı kıvrandırmak için çalıştırın. `Set-ADSyncScheduler -SyncCycleEnabled $false`

![Zamanlayıcıyı devre dışı](./media/how-to-connect-sync-feature-scheduler/schedulerdisable.png)

Değişikliklerinizi yaptığınızda, zamanlayıcıyı tekrar `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="start-the-scheduler"></a>Zamanlayıcıyı başlatın
Zamanlayıcı varsayılan olarak her 30 dakikada bir çalıştırılır. Bazı durumlarda, zamanlanan döngüler arasında bir eşitleme döngüsü çalıştırmak isteyebilirsiniz veya farklı bir tür çalıştırmanız gerekir.

### <a name="delta-sync-cycle"></a>Delta eşitleme döngüsü
Delta eşitleme döngüsü aşağıdaki adımları içerir:


- Tüm Konektörlerde Delta alma
- Tüm Bağlayıcılarda Delta senkronizasyonu
- Tüm Konektörlere dışa aktarma

### <a name="full-sync-cycle"></a>Tam eşitleme döngüsü
Tam eşitleme döngüsü aşağıdaki adımları içerir:

- Tüm Bağlayıcılarda Tam İthalat
- Tüm Konektörlerde Tam Eşitleme
- Tüm Konektörlere dışa aktarma

Hemen senkronize edilmesi gereken acil bir değişikliğiniz olabilir, bu nedenle bir döngüyü el ile çalıştırmanız gerekir. 

Bir eşitleme döngüsünü el ile çalıştırmanız gerekiyorsa, `Start-ADSyncSyncCycle -PolicyType Delta`PowerShell çalıştırın.

Tam eşitleme döngüsünü başlatmak `Start-ADSyncSyncCycle -PolicyType Initial` için PowerShell komut isteminden çalıştırın.   

Tam eşitleme döngüsü çalıştırmak çok zaman alabilir, bu işlemi en iyi duruma getirmek için sonraki bölümü okuyun.

### <a name="sync-steps-required-for-different-configuration-changes"></a>Farklı yapılandırma değişiklikleri için gereken eşitleme adımları
Farklı yapılandırma değişiklikleri, değişikliklerin tüm nesnelere doğru şekilde uygulandığından emin olmak için farklı eşitleme adımları gerektirir.

- Kaynak dizinden alınacak daha fazla nesne veya öznitelik eklendi (eşitleme kurallarını ekleyerek/değiştirerek)
    - Bu kaynak dizini için Bağlayıcı'da Tam Alma gereklidir
- Eşitleme kurallarında değişiklik yapıldı
    - Değiştirilen Eşitleme kuralları için Bağlayıcı'da Tam Eşitleme gereklidir
- Farklı sayıda nesnenin eklenmesi için [filtreleme](how-to-connect-sync-configure-filtering.md) değiştirildi
    - Eşitleme motoruna zaten aktarılmakta olan özniteliklere göre Öznitelik tabanlı filtreleme kullanmıyorsanız, her AD Bağlayıcısı için Bağlayıcıda Tam Alma gereklidir

### <a name="customizing-a-sync-cycle-run-the-right-mix-of-delta-and-full-sync-steps"></a>Eşitleme döngüsünü özelleştirme Delta ve Tam eşitleme adımlarının doğru karışımını çalıştırın
Tam eşitleme döngüsünü çalıştırmaktan kaçınmak için aşağıdaki cmdlets'i kullanarak belirli Bağlayıcıları tam adım çalıştırmak için işaretleyebilirsiniz.

`Set-ADSyncSchedulerConnectorOverride -Connector <ConnectorGuid> -FullImportRequired $true`

`Set-ADSyncSchedulerConnectorOverride -Connector <ConnectorGuid> -FullSyncRequired $true`

`Get-ADSyncSchedulerConnectorOverride -Connector <ConnectorGuid>` 

Örnek: Bağlayıcı "AD Forest A" için yeni özniteliklerin alınmasını gerektirmeyen eşitleme kurallarında değişiklikler yaptıysanız, bu Bağlayıcı için tam eşitleme adımı da yapan bir delta eşitleme döngüsünü çalıştırmak için aşağıdaki cmdlets'i çalıştırırsınız.

`Set-ADSyncSchedulerConnectorOverride -ConnectorName “AD Forest A” -FullSyncRequired $true`

`Start-ADSyncSyncCycle -PolicyType Delta`

Örnek: Bağlayıcı "AD Forest A" için eşitleme kurallarında değişiklikler yaptıysanız, böylece artık yeni bir öznitelik gerektirecek şekilde içe aktarılmak üzere aşağıdaki cmdlets'i çalıştırırsınız ve bu bağlayıcı için tam alma, tam eşitleme adımı da yapan bir delta eşitleme döngüsünü çalıştırırsınız.

`Set-ADSyncSchedulerConnectorOverride -ConnectorName “AD Forest A” -FullImportRequired $true`

`Set-ADSyncSchedulerConnectorOverride -ConnectorName “AD Forest A” -FullSyncRequired $true`

`Start-ADSyncSyncCycle -PolicyType Delta`


## <a name="stop-the-scheduler"></a>Zamanlayıcıyı durdurun
Zamanlayıcı şu anda bir eşitleme döngüsü çalıştırıyorsa, bunu durdurmanız gerekebilir. Örneğin yükleme sihirbazını çalıştırıp şu hatayı alırsanız:

![SyncCycleRunningError](./media/how-to-connect-sync-feature-scheduler/synccyclerunningerror.png)

Eşitleme döngüsü çalışırken yapılandırma değişiklikleri yapamazsınız. Zamanlayıcı işlemi tamamlayana kadar bekleyebilir, ancak değişikliklerinizi hemen yapabilmeniz için işlemi de durdurabilirsiniz. Geçerli döngüyü durdurmak zararlı değildir ve bekleyen değişiklikler bir sonraki çalıştırmayla işlenir.

1. PowerShell cmdlet `Stop-ADSyncSyncCycle`ile mevcut döngüsünü durdurmak için zamanlayıcı söyleyerek başlayın.
2. 1.1.281'den önce bir yapı kullanıyorsanız, zamanlayıcıyı durdurmak geçerli Bağlayıcıyı geçerli görevinden alıkoymaz. Konektörü durdurmaya zorlamak için aşağıdaki ![işlemleri yapmak için: StopAConnector](./media/how-to-connect-sync-feature-scheduler/stopaconnector.png)
   * **Eşitleme Hizmetini** başlat menüsünden başlatın. **Bağlayıcılara**gidin, **Çalışan**durumla bağlayıcıyı vurgulayın ve Eylemlerden **Durdur'u** seçin.

Zamanlayıcı hala etkindir ve bir sonraki fırsatta yeniden başlar.

## <a name="custom-scheduler"></a>Özel zamanlayıcı
Bu bölümde belgelenen cmdletler sadece build [1.1.130.0](reference-connect-version-history.md#111300) ve sonraki yıllarda mevcuttur.

Yerleşik zamanlayıcı gereksinimlerinizi karşılamıyorsa, Connectors'ı PowerShell'i kullanarak zamanlayabilirsiniz.

### <a name="invoke-adsyncrunprofile"></a>Invoke-ADSyncRunProfile
Bağlayıcı için bir profil için bu şeyi bu şere yle başlayabilirsiniz:

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

[Bağlayıcı adlar](how-to-connect-sync-service-manager-ui-connectors.md) ve Run [Profil Adları](how-to-connect-sync-service-manager-ui-connectors.md#configure-run-profiles) için kullanılacak adlar [Eşitleme Hizmet Yöneticisi Kullanıcı](how-to-connect-sync-service-manager-ui.md)Arabirimi'nde bulunabilir.

![Çalıştır Profilini Çağır](./media/how-to-connect-sync-feature-scheduler/invokerunprofile.png)  

Cmdlet `Invoke-ADSyncRunProfile` senkrondur, yani Konektör işlemi başarıyla veya bir hatayla tamamlayana kadar denetimi döndürmez.

Bağlayıcılarınızı zamanladiğinizde, öneri bunları aşağıdaki sırada zamanlamaktır:

1. (Tam /Delta) Active Directory gibi şirket içi dizinlerden alma
2. (Tam /Delta) Azure AD'den alma
3. (Tam /Delta) Active Directory gibi şirket içi dizinlerden eşitleme
4. (Tam /Delta) Azure AD'den eşitleme
5. Azure AD'ye dışa aktarma
6. Active Directory gibi şirket içi dizinlere dışa aktarma

Bu sipariş, yerleşik zamanlayıcının Bağlayıcıları nasıl çalıştırdığının bir şeklidir.

### <a name="get-adsyncconnectorrunstatus"></a>Get-ADSyncConnectorRunStatus
Meşgul veya boşta olup olmadığını görmek için eşitleme motorlarını da izleyebilirsiniz. Eşitleme motoru boştaysa ve Bağlayıcı çalışmıyorsa, bu cmdlet boş bir sonuç verir. Bağlayıcı çalışıyorsa, Bağlayıcının adını döndürür.

```
Get-ADSyncConnectorRunStatus
```

![Bağlayıcı Çalıştırma Durumu](./media/how-to-connect-sync-feature-scheduler/getconnectorrunstatus.png)  
Yukarıdaki resimde, ilk satır eşitleme altyapısının boşta olduğu bir durumdan geliyor. Azure AD Bağlayıcısı'nın çalıştırıldığındaki ikinci satır.

## <a name="scheduler-and-installation-wizard"></a>Zamanlayıcı ve yükleme sihirbazı
Yükleme sihirbazını başlatırsanız, zamanlayıcı geçici olarak askıya alınır. Bu davranış, yapılandırma değişiklikleri yaptığınız ın varsayılmasından ve eşitleme altyapısı etkin çalışıyorsa bu ayarların uygulanamamasıdır. Bu nedenle, eşitleme altyapısının herhangi bir eşitleme eylemini gerçekleştirmesini engellediğinden yükleme sihirbazını açık bırakmayın.

## <a name="next-steps"></a>Sonraki adımlar
Azure AD [Connect eşitleme](how-to-connect-sync-whatis.md) yapılandırması hakkında daha fazla bilgi edinin.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
