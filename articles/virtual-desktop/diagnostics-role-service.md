---
title: Windows Sanal Masaüstü sorunları tanılamak - Azure
description: Sorunları tanılamak için Windows Sanal Masaüstü tanılama özelliğinasıl kullanılır.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: ce85fb70e1480ad285eee78fe20faa8d77b9a147
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254267"
---
# <a name="identify-and-diagnose-issues"></a>Sorunları belirleme ve tanılama

Windows Sanal Masaüstü, yöneticinin sorunları tek bir arabirim üzerinden tanımlamasına olanak tanıyan bir tanılama özelliği sunar. Windows Sanal Masaüstü rolleri, bir kullanıcı sistemle etkileşimde olduğunda bir tanılama etkinliğini günlüğe kaydeder. Her günlük, harekette yer alan Windows Sanal Masaüstü rolleri, hata iletileri, kiracı bilgileri ve kullanıcı bilgileri gibi alakalı bilgileri içerir. Tanılama etkinlikleri hem son kullanıcı hem de yönetim eylemleri tarafından oluşturulur ve üç ana kovaya ayrılabilir:

* Özet akışı aboneliği etkinlikleri: Son kullanıcı, Microsoft Uzak Masaüstü uygulamaları aracılığıyla özet akışlarına bağlanmaya çalıştıklarında bu etkinlikleri tetikler.
* Bağlantı etkinlikleri: Son kullanıcı, Microsoft Remote Desktop uygulamaları aracılığıyla bir masaüstüne veya RemoteApp'a bağlanmaya çalıştıklarında bu etkinlikleri tetikler.
* Yönetim etkinlikleri: Yönetici, ana bilgisayar havuzları oluşturma, kullanıcıları uygulama gruplarına atama ve rol atamaları oluşturma gibi sistemde yönetim işlemleri gerçekleştirdiğinde bu etkinlikleri tetikler.
  
Tanılama rol hizmetiwindows sanal masaüstünün bir parçası olduğundan, Windows Sanal Masaüstü'ne ulaşmayan bağlantılar tanılama sonuçlarında görünmez. Windows Sanal Masaüstü bağlantı sorunları, son kullanıcı ağ bağlantısı sorunları yla karşılaştığında olabilir.

Başlamak için, powershell oturumunuzda kullanmak üzere [Windows Virtual Desktop PowerShell modülünü indirin ve içe aktarın.](/powershell/windows-virtual-desktop/overview/) Bundan sonra, hesabınızda oturum açabilmek için aşağıdaki cmdlet'i çalıştırın:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

## <a name="diagnose-issues-with-powershell"></a>PowerShell ile ilgili sorunları tanılama

Windows Sanal Masaüstü Tanılama yalnızca bir PowerShell cmdlet kullanır, ancak sorunları daraltmaya ve yalıtmaya yardımcı olmak için birçok isteğe bağlı parametre içerir. Aşağıdaki bölümlerde sorunları tanılamak için çalıştırabileceğiniz cmdletler listeleilmektedir. Çoğu filtre birlikte uygulanabilir. Parantez içinde listelenen değerler, `<tenantName>`örneğin, durumunuziçin geçerli değerlerle değiştirilmelidir.

>[!IMPORTANT]
>Tanılama özelliği tek kullanıcılı sorun giderme içindir. PowerShell'i kullanan tüm sorgular *-Kullanıcı Adı* veya *-ActivityID* parametrelerini içermelidir. İzleme özellikleri için Log Analytics'i kullanın. Tanılama verilerini çalışma alanınıza nasıl göndereceğiniz hakkında daha fazla bilgi için [tanılama özelliği için Günlük Analizini Kullan'a](diagnostics-log-analytics.md) bakın. 

### <a name="filter-diagnostic-activities-by-user"></a>Tanılama etkinliklerini kullanıcıya göre filtreleme

**-UserName** parametresi, aşağıdaki örnek cmdlet'te gösterildiği gibi, belirtilen kullanıcı tarafından başlatılan tanılama etkinliklerinin listesini döndürür.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -UserName <UserUPN>
```

**-Kullanıcı Adı** parametresi diğer isteğe bağlı filtreleme parametreleri ile de birleştirilebilir.

### <a name="filter-diagnostic-activities-by-time"></a>Tanılama faaliyetlerini zamana göre filtreleme

Döndürülen tanılama etkinlik listesini **-Başlangıç Zamanı** ve **-Bitiş Saati** parametreleri ile filtreleyebilirsiniz. **-StartTime** parametresi, aşağıdaki örnekte gösterildiği gibi, belirli bir tarihten başlayarak bir tanılama etkinlik listesi döndürür.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -UserName <UserUPN> -StartTime "08/01/2018"
```

**-EndTime** parametresi, sonuç almak istediğiniz belirli bir süreyi belirtmek için **-StartTime** parametresi ile bir cmdlet'e eklenebilir. Aşağıdaki örnek cmdlet 1 Ağustos ve 10 Ağustos arasında tanılama faaliyetlerinin bir listesini döndürecektir.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -UserName <UserUPN> -StartTime "08/01/2018" -EndTime "08/10/2018"
```

**-StartTime** ve **-EndTime** parametreleri diğer isteğe bağlı filtreleme parametreleri ile de birleştirilebilir.

### <a name="filter-diagnostic-activities-by-activity-type"></a>Tanılama etkinliklerini etkinlik türüne göre filtreleme

Tanılama etkinliklerini etkinlik türüne **göre -ActivityType** parametresi ile de filtreleyebilirsiniz. Aşağıdaki cmdlet son kullanıcı bağlantılarının bir listesini döndürür:

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -UserName <UserUPN> -ActivityType Connection
```

Aşağıdaki cmdlet yönetici yönetimi görevlerinin bir listesini döndürür:

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -ActivityType Management
```

**Get-RdsDiagnosticActivities** cmdlet şu anda Etkinlik Türü olarak Feed belirtilmesi desteklemiyor.

### <a name="filter-diagnostic-activities-by-outcome"></a>Tanılama faaliyetlerini sonuca göre filtreleme

İade edilen tanılama etkinlik listesini **-Sonuç** parametresi ile sonuca göre filtreleyebilirsiniz. Aşağıdaki örnek cmdlet başarılı tanılama faaliyetlerinin bir listesini döndürecektir.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -UserName <UserUPN> -Outcome Success
```

Aşağıdaki örnek cmdlet başarısız tanılama faaliyetlerinin bir listesini döndürecektir.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -Outcome Failure
```

**-Sonuç** parametresi diğer isteğe bağlı filtreleme parametreleri ile de birleştirilebilir.

### <a name="retrieve-a-specific-diagnostic-activity-by-activity-id"></a>Etkinlik kimliğine göre belirli bir tanılama etkinliği alma

**-ActivityId** parametresi, aşağıdaki örnekcmdlet'te gösterildiği gibi, varsa belirli bir tanılama aktivitesidöndürür.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -ActivityId <ActivityIdGuid>
```

### <a name="view-error-messages-for-a-failed-activity-by-activity-id"></a>Etkinlik kimliğine göre başarısız bir etkinlik için hata iletilerini görüntüleme

Başarısız bir etkinlik için hata iletilerini görüntülemek için cmdlet'i **-Ayrıntılı** parametreile çalıştırmanız gerekir. **Seç-Nesne** cmdlet'ini çalıştırarak hata listesini görüntüleyebilirsiniz.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantname> -ActivityId <ActivityGuid> -Detailed | Select-Object -ExpandProperty Errors
```

### <a name="retrieve-detailed-diagnostic-activities"></a>Ayrıntılı tanılama etkinliklerini alın

**-Ayrıntılı** parametre döndürülen her tanılama etkinliği için ek ayrıntılar sağlar. Her etkinliğin biçimi, etkinlik türüne bağlı olarak değişir. **-Ayrıntılı** parametre aşağıdaki örnekte gösterildiği gibi, herhangi bir **Get-RdsDiagnosticActivities** sorgusuna eklenebilir.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -ActivityId <ActivityGuid> -Detailed
```

## <a name="common-error-scenarios"></a>Yaygın hata senaryoları

Hata senaryoları, hizmetin içinde ve Windows Sanal Masaüstü'nün dışında kategorize edilir.

* İç Sorun: kiracı yönetici tarafından hafifletilmeyen ve destek sorunu olarak çözülmesi gereken senaryoları belirtir. Windows Virtual Desktop [Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop)aracılığıyla geri bildirim sağlarken, etkinlik kimliğini ve sorunun ne zaman oluştuğunun yaklaşık zaman dilimini içerir.
* Dış Sorun: sistem yöneticisi tarafından azaltılabilen senaryolarla ilgilidir. Bunlar Windows Sanal Masaüstü'ne ek olarak yapılır.

Aşağıdaki tabloda, yöneticilerinizin karşısına sık karşılaşılan sık karşılaşılan hataları listeleniz.

>[!NOTE]
>Bu liste en sık yapılan hataları içerir ve düzenli olarak güncelleştirilir. En güncel bilgilere sahip olduğundan emin olmak için, bu makaleyi ayda en az bir kez kontrol ettiğinizden emin olun.

### <a name="external-management-error-codes"></a>Dış yönetim hata kodları

|Sayısal kod|Hata kodu|Önerilen çözüm|
|---|---|---|
|3|Yetkisiz Erişim|Yönetim powershell cmdlet çalıştırmak için çalıştı kullanıcı ya bunu yapmak için izinleri yok ya da kullanıcı adını yanlış yazdı.|
|1000|Kiracı Bulunamadı|Girdiğiniz kiracı adı varolan kiracılarla eşleşmiyor. Yazım hataları için kiracı adını gözden geçirin ve yeniden deneyin.|
|1006|KiracıCannotBeRemovedHasSessionHostPools|Nesneleri içerdiği sürece kiracıyı silemezsiniz. Önce oturum ana bilgisayar havuzlarını silin, sonra yeniden deneyin.|
|2000|HostPoolNotFound|Girdiğiniz ana bilgisayar havuzu adı varolan ana bilgisayar havuzlarıyla eşleşmiyor. Yazım hataları için ana bilgisayar havuzu adını gözden geçirin ve yeniden deneyin.|
|2005|HostPoolCannotBeRemovedHasApplicationGroups|Nesneleri içerdiği sürece ana bilgisayar havuzunü silemezsiniz. Önce ana bilgisayar havuzundaki tüm uygulama gruplarını kaldırın.|
|2004|HostPoolcannotberemovedhassessionhosts|Oturum ana bilgisayar havuzunu silmeden önce tüm oturumları ana bilgisayarları kaldırın.|
|5001|SessionHostNotFound|Sorguladığınız oturum ana bilgisayar çevrimdışı olabilir. Ana bilgisayar havuzunun durumunu kontrol edin.|
|5008|SessionHostUserSessionsExist |Hedeflediğiniz yönetim etkinliğini gerçekleştirmeden önce oturum ana bilgisayardaki tüm kullanıcıları oturumu niçin imzalamanız gerekir.|
|6000|AppGroupNotFound|Girdiğiniz uygulama grubu adı, varolan uygulama gruplarıyla eşleşmiyor. Yazım hataları için uygulama grubu adını gözden geçirin ve yeniden deneyin.|
|6022|RemoteAppNotFound|Girdiğiniz RemoteApp adı hiçbir RemoteApps ile eşleşmiyor. Yazım hataları için RemoteApp adını inceleyin ve yeniden deneyin.|
|6010|YayınlandıÖğelerVar|Yayımlamaya çalıştığınız kaynağın adı, zaten var olan bir kaynakla aynıdır. Kaynak adını değiştirin ve yeniden deneyin.|
|7002|NameNotValidWhiteSpace|Adına beyaz boşluk kullanmayın.|
|8000|Geçersiz YetkilendirmeRoleScope|Girdiğiniz rol adı varolan rol adlarıyla eşleşmiyor. Yazım hataları için rol adını gözden geçirin ve yeniden deneyin. |
|8001|Kullanıcı Bulunamadı |Girdiğiniz kullanıcı adı varolan kullanıcı adlarıyla eşleşmiyor. Yazım hataları için adı gözden geçirin ve yeniden deneyin.|
|8005|KullanıcıNotFoundInAAD |Girdiğiniz kullanıcı adı varolan kullanıcı adlarıyla eşleşmiyor. Yazım hataları için adı gözden geçirin ve yeniden deneyin.|
|8008|Kiracı Onayı Gerekli|Kiracınızın rızasını sağlamak için [buradaki](tenant-setup-azure-active-directory.md#grant-permissions-to-windows-virtual-desktop) talimatları uygulayın.|

### <a name="external-connection-error-codes"></a>Dış bağlantı hata kodları

|Sayısal kod|Hata kodu|Önerilen çözüm|
|---|---|---|
|-2147467259|BağlantıFailedAdTrustedİlişkiHatası|Oturum ana bilgisayarı Etkin Dizine doğru bir şekilde katılmaz.|
|-2146233088|BağlantıFailedUserHasValidSessionButRdshIsSağlıksız|Oturum ana bilgisayarı kullanılamadığı için bağlantılar başarısız oldu. Oturum sahibinin durumunu kontrol edin.|
|-2146233088|BağlantıFailedIstemci Bağlantısı|Bu hatayı sık sık görüyorsanız, kullanıcının bilgisayarının ağa bağlı olduğundan emin olun.|
|-2146233088|BağlantıFailedNoHealthyRdshKullanılabilir|Ana bilgisayar kullanıcısının bağlanmaya çalıştığı oturum sağlıklı değil. Sanal makineyi hata ayıklama.|
|-2146233088|BağlantıFailedUserNotAuthorized|Kullanıcının yayınlanan uygulamaya veya masaüstüne erişme izni yoktur. Yönetici yayımlanmış kaynakları kaldırdıktan sonra hata görünebilir. Kullanıcıdan Uzak Masaüstü uygulamasında özet akışını yenilemesini isteyin.|
|2|FileNotFound|Kullanıcının erişmeye çalıştığı uygulama ya yanlış yüklenir veya yanlış bir yola ayarlanır.|
|3|Geçersiz Kimlik Bilgileri|Kullanıcının girdiği kullanıcı adı veya parola, varolan kullanıcı adı veya parolalarla eşleşmiyor. Yazım hataları için kimlik bilgilerini gözden geçirin ve yeniden deneyin.|
|8|Bağlantı Kırık|İstemci ve Ağ Geçidi veya Sunucu arasındaki bağlantı kesildi. Beklenmeyen bir şekilde gerçekleşmedikçe eylem gerekmez.|
|14|Beklenmeyen Ağ Bağlantısı|Ağ bağlantısı kesildi. Kullanıcıdan yeniden bağlanmasını isteyin.|
|24|Ters Bağlantı Başarısız|Ana bilgisayar sanal makinerd Gateway görüş doğrudan hattı vardır. Ağ Geçidi IP adresinin çözülebileceğinden emin olun.|

## <a name="next-steps"></a>Sonraki adımlar

Windows Sanal Masaüstü'nde roller hakkında daha fazla bilgi edinmek için [Windows Sanal Masaüstü ortamına](environment-setup.md)bakın.

Windows Sanal Masaüstü için kullanılabilir PowerShell cmdlets listesini görmek için [PowerShell başvurusuna](/powershell/windows-virtual-desktop/overview)bakın.
