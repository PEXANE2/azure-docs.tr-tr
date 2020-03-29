---
title: Şirket içi Azure AD Parola Koruması'nı izleme
description: Şirket içi Active Directory Etki Alanı Hizmetleri ortamı için Azure AD Parola Koruması günlüklerini nasıl izleyeceğinizi ve gözden geçireceğinizi öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: fbb533d5565009fb22d686e4082c9b4bfaae6dc1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78671653"
---
# <a name="monitor-and-review-logs-for-on-premises-azure-ad-password-protection-environments"></a>Şirket içi Azure AD Parola Koruması ortamları için günlükleri izleme ve inceleme

Azure AD Parola Koruması'nın dağıtımından sonra, izleme ve raporlama temel görevlerdir. Bu makale, her hizmetin bilgileri nerede günlüğe kaydettiği ve Azure AD Parola Koruması'nın kullanımı hakkında nasıl rapor verebileceğiniz dahil olmak üzere çeşitli izleme tekniklerini anlamanıza yardımcı olmak için ayrıntılara girer.

İzleme ve raporlama olay günlüğü iletileri veya PowerShell cmdlets çalıştırılarak yapılır. DC aracısı ve proxy hizmetleri hem olay günlüğü iletilerini kaydeder. Aşağıda açıklanan tüm PowerShell cmdlets yalnızca proxy sunucusunda mevcuttur (AzureADPasswordProtection PowerShell modülüne bakın). DC aracı yazılımı PowerShell modülü yüklemez.

## <a name="dc-agent-event-logging"></a>DC aracı olay günlüğü

Her etki alanı denetleyicisi üzerinde, DC aracı hizmeti yazılımı her bir parola doğrulama işleminin (ve diğer durumun) sonuçlarını yerel bir olay günlüğüne yazar:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Admin`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace`

DC aracısı Yönetici günlüğü, yazılımın nasıl davrandığına yönelik birincil bilgi kaynağıdır.

İzleme günlüğünün varsayılan olarak kapalı olduğunu unutmayın.

Çeşitli DC aracı bileşenleri tarafından günlüğe kaydedilen olaylar aşağıdaki aralıklara girer:

|Bileşen |Olay kimliği aralığı|
| --- | --- |
|DC Agent şifre filtresi dll| 10000-19999|
|DC acente hizmeti barındırma işlemi| 20000-29999|
|DC aracı hizmet ilkesi doğrulama mantığı| 30000-39999|

## <a name="dc-agent-admin-event-log"></a>DC ajan Yönetici olay günlüğü

### <a name="password-validation-outcome-events"></a>Parola doğrulama sonucu olayları

Her etki alanı denetleyicisi üzerinde, DC aracısı hizmet yazılımı DC aracısı yönetici olay günlüğüne her bir şifre doğrulama sonuçlarını yazar.

Başarılı bir parola doğrulama işlemi için, genellikle DC aracısı parola filtresi dll günlüğe kaydedilen bir olay vardır. Başarısız bir parola doğrulama işlemi için, genellikle iki olay günlüğe kaydedilir, biri DC aracı hizmetinden, diğeri dc agent parola filtresi dll'den.

Bu durumları yakalamak için ayrı kılmış olaylar, aşağıdaki etkenler etrafında kaydedilir:

* Belirli bir parolanın ayarlanıp ayarlanmadığı veya değiştirilip değiştirilmediği.
* Belirli bir parolanın doğrulanması geçti veya başarısız oldu.
* Doğrulama, Microsoft genel ilkesi, kuruluş ilkesi veya bir kombinasyon nedeniyle başarısız oldu.
* Geçerli parola ilkesi için yalnızca denetim modunun şu anda açık mı yoksa kapalı mı olduğu.

Anahtar parola doğrulama ile ilgili olaylar aşağıdaki gibidir:

|   |Parola değiştirme |Parola kümesi|
| --- | :---: | :---: |
|Geçirmek |10014 |10015|
|Başarısız (müşteri parola ilkesi nedeniyle)| 10016, 30002| 10017, 30003|
|Başarısız (Microsoft parola ilkesi nedeniyle)| 10016, 30004| 10017, 30005|
|Başarısız (birleştirilmiş Microsoft ve müşteri parola ilkeleri nedeniyle)| 10016, 30026| 10017, 30027|
|Yalnızca Denetim Geçişi (müşteri parola ilkesinde başarısız olurdu)| 10024, 30008| 10025, 30007|
|Yalnızca Denetim Geçişi (Microsoft parola ilkesinde başarısız olurdu)| 10024, 30010| 10025, 30009|
|Yalnızca Denetim Geçişi (Microsoft ve müşteri parolası ilkelerini birleştirmiş olur)| 10024, 30028| 10025, 30029|

Yukarıdaki tabloda "birleşik ilkeler" anlamına gelen durumlar, kullanıcının parolasının hem Microsoft'un yasaklı parola listesinden hem de müşterinin yasaklı parola listesinden en az bir belirteç içerdiği durumlara atıfta bulunur.

Bir çift olay birlikte günlüğe kaydedildiğinde, her iki olay da açıkça aynı KorelasyonId'e sahip olarak ilişkilendirilir.

### <a name="password-validation-summary-reporting-via-powershell"></a>PowerShell üzerinden şifre doğrulama özeti raporlaması

Cmdlet, `Get-AzureADPasswordProtectionSummaryReport` parola doğrulama etkinliğinin özet görünümünü oluşturmak için kullanılabilir. Bu cmdlet bir örnek çıktı aşağıdaki gibidir:

```powershell
Get-AzureADPasswordProtectionSummaryReport -DomainController bplrootdc2
DomainController                : bplrootdc2
PasswordChangesValidated        : 6677
PasswordSetsValidated           : 9
PasswordChangesRejected         : 10868
PasswordSetsRejected            : 34
PasswordChangeAuditOnlyFailures : 213
PasswordSetAuditOnlyFailures    : 3
PasswordChangeErrors            : 0
PasswordSetErrors               : 1
```

Cmdlet'in raporlama kapsamı ,Orman, -Etki Alanı veya -DomainController parametrelerinden biri kullanılarak etkilenebilir. Bir parametre belirtilmesi anlamına gelir -Orman.

Cmdlet, `Get-AzureADPasswordProtectionSummaryReport` DC aracısı yönetici olay günlüğünü sorgulayarak ve sonra görüntülenen her sonuç kategorisine karşılık gelen toplam olay sayısını sayarak çalışır. Aşağıdaki tablo, her sonuç ve karşılık gelen olay kimliği arasındaki eşlemeleri içerir:

|AzureADPasswordProtectionSummaryReport özelliği |Karşılık gelen olay kimliği|
| :---: | :---: |
|PasswordChangesValidated |10014|
|PasswordSetsValidated |10015|
|Parola Değişiklikleri Reddedildi |10016|
|ParolaSetleri Reddedildi |10017|
|PasswordChangeAuditOnlyFailures |10024|
|PasswordSetAuditOnlyFailures |10025|
|PasswordChangeErrors |10012|
|PasswordSetErrors |10013|

Cmdlet'in `Get-AzureADPasswordProtectionSummaryReport` PowerShell komut dosyası formunda gönderildiğini ve gerekirse doğrudan aşağıdaki konumda başvurulabileceğini unutmayın:

`%ProgramFiles%\WindowsPowerShell\Modules\AzureADPasswordProtection\Get-AzureADPasswordProtectionSummaryReport.ps1`

> [!NOTE]
> Bu cmdlet, her etki alanı denetleyicisine bir PowerShell oturumu açarak çalışır. Başarılı olmak için PowerShell uzaktan oturum desteği nin her etki alanı denetleyicisinde etkinleştirilmesi ve istemcinin yeterli ayrıcalıklara sahip olması gerekir. PowerShell uzaktan oturum gereksinimleri hakkında daha fazla bilgi için PowerShell penceresinde 'Yardım about_Remote_Troubleshooting Al' çalıştırın.

> [!NOTE]
> Bu cmdlet, her DC aracı hizmetinin Yönetici olay günlüğünü uzaktan sorgulayarak çalışır. Olay günlükleri çok sayıda olay içeriyorsa, cmdlet'in tamamlanması uzun sürebilir. Buna ek olarak, büyük veri kümelerinin toplu ağ sorguları etki alanı denetleyicisi performansını etkileyebilir. Bu nedenle bu cmdlet üretim ortamlarında dikkatle kullanılmalıdır.

### <a name="sample-event-log-message-for-event-id-10014-successful-password-change"></a>Olay Kimliği 10014 için örnek olay günlüğü iletisi (başarılı parola değişikliği)

```text
The changed password for the specified user was validated as compliant with the current Azure password policy.

 UserName: BPL_02885102771
 FullName:
```

### <a name="sample-event-log-message-for-event-id-10017-and-30003-failed-password-set"></a>Olay Kimliği 10017 ve 30003 için örnek olay günlüğü iletisi (başarısız parola kümesi)

10017:

```text
The reset password for the specified user was rejected because it did not comply with the current Azure password policy. Please see the correlated event log message for more details.

 UserName: BPL_03283841185
 FullName:
```

30003:

```text
The reset password for the specified user was rejected because it matched at least one of the tokens present in the per-tenant banned password list of the current Azure password policy.

 UserName: BPL_03283841185
 FullName:
```

### <a name="sample-event-log-message-for-event-id-30001-password-accepted-due-to-no-policy-available"></a>Olay Kimliği 30001 için örnek olay günlüğü iletisi (kullanılabilir bir ilke olmadığı için şifre kabul edilir)

```text
The password for the specified user was accepted because an Azure password policy is not available yet

UserName: SomeUser
FullName: Some User

This condition may be caused by one or more of the following reasons:%n

1. The forest has not yet been registered with Azure.

   Resolution steps: an administrator must register the forest using the Register-AzureADPasswordProtectionForest cmdlet.

2. An Azure AD password protection Proxy is not yet available on at least one machine in the current forest.

   Resolution steps: an administrator must install and register a proxy using the Register-AzureADPasswordProtectionProxy cmdlet.

3. This DC does not have network connectivity to any Azure AD password protection Proxy instances.

   Resolution steps: ensure network connectivity exists to at least one Azure AD password protection Proxy instance.

4. This DC does not have connectivity to other domain controllers in the domain.

   Resolution steps: ensure network connectivity exists to the domain.
```

### <a name="sample-event-log-message-for-event-id-30006-new-policy-being-enforced"></a>Olay Kimliği 30006 için örnek olay günlüğü iletisi (yeni ilke uygulanıyor)

```text
The service is now enforcing the following Azure password policy.

 Enabled: 1
 AuditOnly: 1
 Global policy date: ‎2018‎-‎05‎-‎15T00:00:00.000000000Z
 Tenant policy date: ‎2018‎-‎06‎-‎10T20:15:24.432457600Z
 Enforce tenant policy: 1
```

### <a name="sample-event-log-message-for-event-id-30019-azure-ad-password-protection-is-disabled"></a>Event ID 30019 için örnek olay günlüğü iletisi (Azure AD Parola Koruması devre dışı bırakıldı)

```text
The most recently obtained Azure password policy was configured to be disabled. All passwords submitted for validation from this point on will automatically be considered compliant with no processing performed.

No further events will be logged until the policy is changed.%n

```

## <a name="dc-agent-operational-log"></a>DC Ajan Operasyonel günlük

DC aracı hizmeti ayrıca operasyonel olaylarla ilgili olayları aşağıdaki günlüğe kaydeder:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Operational`

## <a name="dc-agent-trace-log"></a>DC Agent Trace günlüğü

DC aracı hizmeti, ayrıntılı hata ayıklama düzeyindeki izleme olaylarını aşağıdaki günlüğe de kaydedebilir:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\DCAgent\Trace`

İzleme günlüğü varsayılan olarak devre dışı bırakılır.

> [!WARNING]
> Etkinleştirildiğinde, İzleme günlüğü yüksek hacimli olaylar alır ve etki alanı denetleyicisi performansını etkileyebilir. Bu nedenle, bu gelişmiş günlük yalnızca bir sorun daha derin bir araştırma gerektiriyorsa ve daha sonra yalnızca en az bir süre için etkinleştirilmelidir.

## <a name="dc-agent-text-logging"></a>DC Agent metin günlüğü

DC aracı hizmeti, aşağıdaki kayıt defteri değerini ayarlayarak metin günlüğüne yazacak şekilde yapılandırılabilir:

```text
HKLM\System\CurrentControlSet\Services\AzureADPasswordProtectionDCAgent\Parameters!EnableTextLogging = 1 (REG_DWORD value)
```

Metin günlüğe kaydetme varsayılan olarak devre dışı bırakılır. Dc aracı hizmetinin yeniden başlatılması, bu değerdeki değişikliklerin etkili olması için gereklidir. DC aracı hizmeti etkinleştirildiğinde, aşağıdakiler altında bulunan bir günlük dosyasına yazar:

`%ProgramFiles%\Azure AD Password Protection DC Agent\Logs`

> [!TIP]
> Metin günlüğü, İzleme günlüğüne günlüğe kaydedilebilen hata ayıklama düzeyi girişlerini alır, ancak genellikle gözden geçirilmesi ve çözümlemesi daha kolay bir biçimdedir.

> [!WARNING]
> Etkinleştirildiğinde, bu günlük yüksek hacimli olaylar alır ve etki alanı denetleyicisi performansını etkileyebilir. Bu nedenle, bu gelişmiş günlük yalnızca bir sorun daha derin bir araştırma gerektiriyorsa ve daha sonra yalnızca en az bir süre için etkinleştirilmelidir.

## <a name="dc-agent-performance-monitoring"></a>DC ajan performans izleme

DC aracı hizmeti **yazılımı, Azure AD Parola Koruması**adlı bir performans sayacı nesnesi yükler. Aşağıdaki perf sayaçları şu anda mevcuttur:

|Perf sayaç adı | Açıklama|
| --- | --- |
|İşlenen parolalar |Bu sayaç, son yeniden başlatmadan bu yana işlenen (kabul edilen veya reddedilen) toplam parola sayısını görüntüler.|
|Parolalar kabul edildi |Bu sayaç, son yeniden başlatmadan bu yana kabul edilen toplam parola sayısını görüntüler.|
|Parolalar reddedildi |Bu sayaç, son yeniden başlatmadan bu yana reddedilen toplam parola sayısını görüntüler.|
|Parola filtresi istekleri devam ediyor |Bu sayaç, şu anda devam etmekte olan parola filtresi isteklerinin sayısını görüntüler.|
|En yüksek parola filtresi istekleri |Bu sayaç, son yeniden başlatmadan bu yana eşzamanlı parola filtresi isteklerinin en yüksek sayısını görüntüler.|
|Parola filtresi isteği hataları |Bu sayaç, son yeniden başlatmadan bu yana bir hata nedeniyle başarısız olan toplam parola filtresi isteği sayısını görüntüler. Azure AD Parola Koruması DC aracı hizmeti çalışmıyorsa hatalar oluşabilir.|
|Parola filtresi istekleri/sn |Bu sayaç, parolaların işlenme hızını görüntüler.|
|Parola filtresi isteği işleme süresi |Bu sayaç, parola filtresi isteğini işlemek için gereken ortalama süreyi görüntüler.|
|En yüksek parola filtresi isteği işleme süresi |Bu sayaç, son yeniden başlatmadan bu yana en yüksek parola filtresi isteği işleme süresini görüntüler.|
|Denetim modu nedeniyle kabul edilen parolalar |Bu sayaç, normalde reddedilmiş, ancak parola ilkesi denetim modunda (son yeniden başlatmadan beri) olarak yapılandırıldığından kabul edilen toplam parola sayısını görüntüler.|

## <a name="dc-agent-discovery"></a>DC Ajan bulma

Cmdlet, `Get-AzureADPasswordProtectionDCAgent` bir etki alanında veya ormanda çalışan çeşitli DC aracıları hakkında temel bilgileri görüntülemek için kullanılabilir. Bu bilgiler, çalışan DC aracı servisi(ler) tarafından kayıtlı serviceConnectionPoint object(s)'den alınır.

Bu cmdlet bir örnek çıktı aşağıdaki gibidir:

```powershell
Get-AzureADPasswordProtectionDCAgent
ServerFQDN            : bplChildDC2.bplchild.bplRootDomain.com
Domain                : bplchild.bplRootDomain.com
Forest                : bplRootDomain.com
PasswordPolicyDateUTC : 2/16/2018 8:35:01 AM
HeartbeatUTC          : 2/16/2018 8:35:02 AM
```

Çeşitli özellikler, her DC aracı sıyrık hizmeti tarafından yaklaşık saatlik olarak güncelleştirilir. Veriler hala Etkin Dizin çoğaltma gecikmesine tabidir.

Cmdlet sorgusunun kapsamı -Orman veya Etki Alanı parametreleri kullanılarak etkilenebilir.

HeartbeatUTC değeri eskiye girerse, bu durum, söz alan adı denetleyicisindeki Azure AD Parola Koruması DC Aracısının çalışmadığının veya kaldırıldığının veya makinenin düşürüldüğüne ve artık bir etki alanı denetleyicisinin olmadığının bir belirtisi olabilir.

PasswordPolicyDateUTC değeri eskiye girerse, bu, o makinedeki Azure AD Parola Koruması DC Aracısının düzgün çalışmadığının bir belirtisi olabilir.

## <a name="dc-agent-newer-version-available"></a>DC ajan yeni sürümü kullanılabilir

DC aracı hizmeti, örneğin DC aracı yazılımının daha yeni bir sürümünün mevcut olduğunu tespit ettikten sonra 30034 uyarı olayını Operasyonel günlüğüne kaydeder:

```text
An update for Azure AD Password Protection DC Agent is available.

If autoupgrade is enabled, this message may be ignored.

If autoupgrade is disabled, refer to the following link for the latest version available:

https://aka.ms/AzureADPasswordProtectionAgentSoftwareVersions

Current version: 1.2.116.0
```

Yukarıdaki olay, yeni yazılımın sürümünü belirtmez. Bu bilgiler için olay iletisindeki bağlantıya gitmeniz gerekir.

> [!NOTE]
> Yukarıdaki olay iletisinde "otomatik yükseltme" için yapılan başvurulara rağmen, DC aracı yazılımı şu anda bu özelliği desteklemez.

## <a name="proxy-service-event-logging"></a>Proxy hizmeti olay günlüğü

Proxy hizmeti, aşağıdaki olay günlüklerine en az olay kümesi yayır:

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Admin`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Operational`

`\Applications and Services Logs\Microsoft\AzureADPasswordProtection\ProxyService\Trace`

İzleme günlüğünün varsayılan olarak kapalı olduğunu unutmayın.

> [!WARNING]
> Etkinleştirildiğinde, İzleme günlüğü yüksek hacimli olaylar alır ve bu proxy ana bilgisayar performansını etkileyebilir. Bu nedenle, bu günlük yalnızca bir sorun daha derin bir araştırma gerektiriyorsa ve daha sonra yalnızca en az bir süre için etkinleştirilmelidir.

Olaylar aşağıdaki aralıkları kullanarak çeşitli Proxy bileşenleri tarafından günlüğe kaydedilir:

|Bileşen |Olay kimliği aralığı|
| --- | --- |
|Proxy hizmet barındırma işlemi| 10000-19999|
|Proxy hizmet çekirdek iş mantığı| 20000-29999|
|PowerShell cmdlet'leri| 30000-39999|

## <a name="proxy-service-text-logging"></a>Proxy hizmeti metin günlüğü

Proxy hizmeti, aşağıdaki kayıt defteri değerini ayarlayarak metin günlüğüne yazacak şekilde yapılandırılabilir:

HKLM\System\CurrentControlSet\Services\AzureADPasswordProtectionProxy\Parameters! EnableTextLogging = 1 (REG_DWORD değer)

Metin günlüğe kaydetme varsayılan olarak devre dışı bırakılır. Bu değerdeki değişikliklerin etkili olması için Proxy hizmetinin yeniden başlatılması gerekir. Proxy hizmeti etkinleştirildiğinde altında bulunan bir günlük dosyasına yazar:

`%ProgramFiles%\Azure AD Password Protection Proxy\Logs`

> [!TIP]
> Metin günlüğü, İzleme günlüğüne günlüğe kaydedilebilen hata ayıklama düzeyi girişlerini alır, ancak genellikle gözden geçirilmesi ve çözümlemesi daha kolay bir biçimdedir.

> [!WARNING]
> Etkinleştirildiğinde, bu günlük yüksek hacimli olaylar alır ve makinenin performansını etkileyebilir. Bu nedenle, bu gelişmiş günlük yalnızca bir sorun daha derin bir araştırma gerektiriyorsa ve daha sonra yalnızca en az bir süre için etkinleştirilmelidir.

## <a name="powershell-cmdlet-logging"></a>PowerShell cmdlet günlüğü

Durum değişikliğiyle sonuçlanan PowerShell cmdlets (örneğin, Register-AzureADPasswordProtectionProxy) normalde bir sonuç olayını Operasyonel günlüğüne kaydeder.

Buna ek olarak, Azure AD Parola Koruma PowerShell cmdlets çoğu altında bulunan bir metin günlüğüne yazacaktır:

`%ProgramFiles%\Azure AD Password Protection Proxy\Logs`

Bir cmdlet hatası oluşursa ve neden ve\veya çözüm kolayca görülmezse, bu metin günlüklerine de danışılabilir.

## <a name="proxy-discovery"></a>Proxy bulma

Cmdlet, `Get-AzureADPasswordProtectionProxy` bir etki alanında veya ormanda çalışan çeşitli Azure AD Parola Koruma Proxy hizmetleri yle ilgili temel bilgileri görüntülemek için kullanılabilir. Bu bilgiler, çalışan Proxy hizmeti(ler) tarafından kayıtlı serviceConnectionPoint object(s)'den alınır.

Bu cmdlet bir örnek çıktı aşağıdaki gibidir:

```powershell
Get-AzureADPasswordProtectionProxy
ServerFQDN            : bplProxy.bplchild2.bplRootDomain.com
Domain                : bplchild2.bplRootDomain.com
Forest                : bplRootDomain.com
HeartbeatUTC          : 12/25/2018 6:35:02 AM
```

Çeşitli özellikler, her Proxy hizmeti tarafından yaklaşık saatlik olarak güncelleştirilir. Veriler hala Etkin Dizin çoğaltma gecikmesine tabidir.

Cmdlet sorgusunun kapsamı -Orman veya Etki Alanı parametreleri kullanılarak etkilenebilir.

HeartbeatUTC değeri eskiye girerse, bu durum o makinedeki Azure AD Parola Koruma Proxy'sinin çalışmadığının veya kaldırıldığının bir belirtisi olabilir.

## <a name="proxy-agent-newer-version-available"></a>Proxy aracısı yeni sürümü kullanılabilir

Proxy hizmeti, örneğin proxy yazılımının daha yeni bir sürümünün mevcut olduğunu algılayarak 20002'deki bir uyarı olayını Operasyonel günlüğüne kaydeder:

```text
An update for Azure AD Password Protection Proxy is available.

If autoupgrade is enabled, this message may be ignored.

If autoupgrade is disabled, refer to the following link for the latest version available:

https://aka.ms/AzureADPasswordProtectionAgentSoftwareVersions

Current version: 1.2.116.0
.
```

Yukarıdaki olay, yeni yazılımın sürümünü belirtmez. Bu bilgiler için olay iletisindeki bağlantıya gitmeniz gerekir.

Proxy aracısı otomatik yükseltme etkinleştirilmiş olarak yapılandırıldıysa bile bu olay yayımlanır.

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD Parola Koruması için Sorun Giderme](howto-password-ban-bad-on-premises-troubleshoot.md)

Genel ve özel yasaklı parola listeleri hakkında daha fazla bilgi için, makaleyi [Ban kötü parolalar](concept-password-ban-bad.md) bakın
