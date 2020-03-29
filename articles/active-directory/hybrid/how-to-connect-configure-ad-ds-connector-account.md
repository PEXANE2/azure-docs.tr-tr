---
title: 'Azure AD Connect: AD DS Bağlayıcı Hesap İzinlerini Yapılandırın | Microsoft Dokümanlar'
description: Bu belge, AD DS Konektör hesabının yeni ADSyncConfig PowerShell modülü ile nasıl yapılandırılabildiğini ayrıntılarıyla anlatır
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/29/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: eeb80c3a94e63a886e4a16c0b8fa445b2a8a34e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72515826"
---
# <a name="azure-ad-connectconfigure-ad-ds-connector-account-permissions"></a>Azure AD Connect: AD DS Bağlayıcı Hesap İzinlerini Yapılandır 

[ADSyncConfig.psm1](reference-connect-adsyncconfig.md) adlı PowerShell Modülü, Azure AD Connect dağıtımınız için doğru Active Directory izinlerini yapılandırmanıza yardımcı olmak için cmdlets koleksiyonunu içeren build 1.1.880.0 (Ağustos 2018'de piyasaya sürüldü) ile tanıtıldı. 

## <a name="overview"></a>Genel Bakış 
Aşağıdaki PowerShell cmdlets, Azure AD Connect'te etkinleştirmek üzere seçtiğiniz her özellik için AD DS Bağlayıcısı hesabının Active Directory izinlerini kurmak için kullanılabilir. Herhangi bir sorunu önlemek için, ormanınıza bağlanmak için özel bir etki alanı hesabı kullanarak Azure AD Connect'i yüklemek istediğinizde Etkin Dizin izinlerini önceden hazırlamanız gerekir. Bu ADSyncConfig modülü, Azure AD Connect dağıtıldıktan sonra izinleri yapılandırmak için de kullanılabilir.

![reklam ds hesabına genel bakış](media/how-to-connect-configure-ad-ds-connector-account/configure1.png)

Azure AD Connect Express yüklemesi için, etkin dizinde gerekli tüm izinlerle otomatik olarak oluşturulan bir hesap (MSOL_nnnnnnnnnn) oluşturulur, bu nedenle izinleri engellemediğiniz sürece bu ADSyncConfig modülünü kullanmanıza gerek yoktur kurumsal birimlerde veya Azure AD ile eşitlemek istediğiniz belirli Active Directory nesnelerinde devralma. 
 
### <a name="permissions-summary"></a>İzin özeti 
Aşağıdaki tablo, AD nesnelerinde gereken izinlerin bir özetini sağlar: 

| Özellik | İzinler |
| --- | --- |
| ms-DS-ConsistencyGuid özelliği |Tasarım Kavramlarıbelgelenen ms-DS-TutarlılıkGuid özniteliği için okuma ve yazma izinleri [- kaynak Anchor olarak ms-DS-ConsistencyGuid kullanma](plan-connect-design-concepts.md#using-ms-ds-consistencyguid-as-sourceanchor). | 
| Parola karma eşitleme |<li>Dizin Değişikliklerini Çoğaltma</li>  <li>Dizin Değişikliklerini Tümünü Çoğaltma |
| Exchange hibrit dağıtım |Kullanıcılar, gruplar ve kişiler için [Exchange karma geri yazma'da](reference-connect-sync-attributes-synchronized.md#exchange-hybrid-writeback) belgelenen özniteliklerin izinlerini okuma ve yazma izinleri. |
| Exchange Mail Ortak Klasörü |Ortak klasörler için Exchange [Mail Ortak](reference-connect-sync-attributes-synchronized.md#exchange-mail-public-folder) Klasörü'nde belgelenen özniteliklerin izinlerini okuyun. | 
| Parola geri yazma |Kullanıcılar için [parola yönetimine başlarken](../authentication/howto-sspr-writeback.md) belgelenen özniteliklere yönelik izinleri okuma ve yazma. |
| Cihaz geri yazma |[Aygıt yazma](how-to-connect-device-writeback.md)geri sinde belgelenen aygıt nesneleri ve kapsayıcıları için okuma ve yazma izinleri. |
| Grup geri yazma |Eşitlenmiş **Office 365 grupları**için grup nesnelerini okuyun, oluşturun, güncelleştirin ve silin.  Daha fazla bilgi için [Bkz. Grup Yazma.](how-to-connect-preview.md#group-writeback)|

## <a name="using-the-adsyncconfig-powershell-module"></a>ADSyncConfig PowerShell Modüllerini Kullanma 
ADSyncConfig modülü, AD DS PowerShell modülüne ve araçlarına bağlı olduğundan [AD DS için Uzak Sunucu Yönetim Araçları (RSAT)](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools) gerektirir. AD DS için RSAT'yi yüklemek için 'Yönetici Olarak Çalıştır' ile bir Windows PowerShell penceresi açın ve çalıştırın: 

``` powershell
Install-WindowsFeature RSAT-AD-Tools 
```
![Yapılandırma](media/how-to-connect-configure-ad-ds-connector-account/configure2.png)

>[!NOTE]
>Ayrıca **C:\Program Files\Microsoft Azure Active Directory Connect\AdSyncConfig\ADSyncConfig.psm1** dosyasını AD DS için RSAT yüklü olan bir Etki Alanı Denetleyicisine kopyalayabilir ve bu PowerShell modülünü buradan kullanabilirsiniz.

ADSyncConfig'i kullanmaya başlamak için modülü Windows PowerShell penceresine yüklemeniz gerekir: 

``` powershell
Import-Module "C:\Program Files\Microsoft Azure Active Directory Connect\AdSyncConfig\AdSyncConfig.psm1" 
```

Bu modülde yer alan tüm cmdletleri kontrol etmek için şunları yazabilirsiniz:  

``` powershell
Get-Command -Module AdSyncConfig  
```

![İşaretli](media/how-to-connect-configure-ad-ds-connector-account/configure3.png)

Her cmdlet, AD DS Bağlayıcı Hesabı'nı ve AdminSDHolder anahtarını girdirecek aynı parametrelere sahiptir. AD DS Bağlayıcı Hesabınızı belirtmek için, hesap adını ve etki alanını veya sadece hesap Seçkin Adı (DN) sağlayabilirsiniz,

Örneğin:

```powershell
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountName <ADAccountName> -ADConnectorAccountDomain <ADDomainName>
```

Ya da;

```powershell
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountDN <ADAccountDN>
```

Ortamınız için `<ADAccountName>` `<ADDomainName>` uygun `<ADAccountDN>` değerleri değiştirdiğinizden ve uygun değerlerle değiştirdiğinizden emin olun.

AdminSDHolder kapsayıcısındaki izinleri değiştirmek istemiyorsanız, anahtarı `-SkipAdminSdHolders`kullanın. 

Varsayılan olarak, cmdlets'in belirlediği tüm izinler, Ad DS izinlerini Ormandaki her Etki Alanı'nın kökünde ayarlamaya çalışır, bu da PowerShell oturumunu çalıştıran kullanıcının Orman'daki her etki alanında Etki Alanı Yöneticisi haklarını gerektirdiği anlamına gelir.  Bu gereksinim nedeniyle, Orman kökünden bir Kurumsal Yönetici kullanılması önerilir. Azure AD Connect dağıtımınızda birden çok AD DS Bağlayıcısı varsa, AD DS Bağlayıcısı olan her ormanda aynı cmdlet'i çalıştırması gerekir. 

İzinleri ayarlamak istediğiniz hedef nesnenin DN'sini izleyen parametreyi `-ADobjectDN` kullanarak belirli bir OU veya AD DS nesnesi üzerinde izinler de ayarlayabilirsiniz. Bir hedef ADobjectDN kullanırken, cmdlet izinleri yalnızca etki alanı köküveya AdminSDHolder kapsayıcısına değil, yalnızca bu nesne üzerinde ayarlar. Bu parametre, izin devralma devre dışı bırakılmış belirli OS'ler veya AD DS nesneleriniz olduğunda yararlı olabilir (bkz. 

Bu ortak parametrelerin istisnaları, AD DS Bağlayıcı Hesabı'nda izinleri ayarlamak için kullanılan `Set-ADSyncRestrictedPermissions` `Set-ADSyncPasswordHashSyncPermissions` cmdlettir ve Parola Hash Sync için gerekli izinler yalnızca etki alanı kökünde `-ObjectDN` ayarlandığı için cmdlet, dolayısıyla bu cmdlet veya `-SkipAdminSdHolders` parametreleri içermez.

### <a name="determine-your-ad-ds-connector-account"></a>AD DS Bağlayıcı Hesabınızı Belirleme 
Azure AD Connect zaten yüklenmişse ve Şu anda Azure AD Connect tarafından kullanılmakta olan AD DS Bağlayıcı Hesabının ne olduğunu kontrol etmek istiyorsanız, cmdlet'i çalıştırabilirsiniz: 

``` powershell
Get-ADSyncADConnectorAccount 
```
### <a name="locate-ad-ds-objects-with-permission-inheritance-disabled"></a>İzin devralma devre dışı bırakılmış AD DS nesnelerini bulma 
İzin devralma devre dışı bırakılmış herhangi bir AD DS nesnesi olup olmadığını denetlemek isterseniz, çalıştırabilirsiniz: 

``` powershell
Get-ADSyncObjectsWithInheritanceDisabled -SearchBase '<DistinguishedName>' 
```
Varsayılan olarak, bu cmdlet yalnızca devre dışı bırakılmış kalıtımlı OSB'leri `-ObjectClass` arar, ancak parametredeki diğer AD DS nesne sınıflarını belirtebilir veya tüm nesne sınıfları için '*' olarak aşağıdaki gibi kullanabilirsiniz: 

``` powershell
Get-ADSyncObjectsWithInheritanceDisabled -SearchBase '<DistinguishedName>' -ObjectClass * 
```
 
### <a name="view-ad-ds-permissions-of-an-object"></a>Bir nesnenin AD DS izinlerini görüntüleme 
Aşağıdaki cmdlet'i kullanarak Active Directory nesnesi üzerinde şu anda ayarlanan izinlerin listesini DistinguishedName'sini vererek görüntüleyebilirsiniz: 

``` powershell
Show-ADSyncADObjectPermissions -ADobjectDN '<DistinguishedName>' 
```

## <a name="configure-ad-ds-connector-account-permissions"></a>AD DS Bağlayıcı Hesabı İzinlerini Yapılandırma 
 
### <a name="configure-basic-read-only-permissions"></a>Temel Salt Okunur İzinleri Yapılandırma 
Herhangi bir Azure AD Connect özelliği kullanmadığında AD DS Bağlayıcısı hesabı için salt okunur temel izinleri ayarlamak için çalıştırın: 

``` powershell
Set-ADSyncBasicReadPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [-SkipAdminSdHolders] [<CommonParameters>] 
```


veya; 

``` powershell
Set-ADSyncBasicReadPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [<CommonParameters>] 
```


Bu cmdlet aşağıdaki izinleri belirler: 
 

|Tür |Adı |Erişim |Uygulanan Öğe| 
|-----|-----|-----|-----|
|İzin Ver |AD DS Bağlayıcı Hesabı |Tüm özellikleri okuyun |Soyundan gelen aygıt nesneleri| 
|İzin Ver |AD DS Bağlayıcı Hesabı|Tüm özellikleri okuyun |Descendant InetOrgPerson nesneleri| 
|İzin Ver |AD DS Bağlayıcı Hesabı |Tüm özellikleri okuyun |Descendant Bilgisayar nesneleri| 
|İzin Ver |AD DS Bağlayıcı Hesabı |Tüm özellikleri okuyun |Soyundan gelen yabancıSecurityPrincipal nesneleri| 
|İzin Ver |AD DS Bağlayıcı Hesabı |Tüm özellikleri okuyun |Soyundan Gelen Grup nesneleri| 
|İzin Ver |AD DS Bağlayıcı Hesabı |Tüm özellikleri okuyun |Descendant Kullanıcı nesneleri| 
|İzin Ver |AD DS Bağlayıcı Hesabı |Tüm özellikleri okuyun |Descendant Contact nesneleri| 

 
### <a name="configure-ms-ds-consistency-guid-permissions"></a>MS-DS-Tutarlılık-Kılavuz İzinlerini Yapılandırma 
Kaynak çapa olarak ms-Ds-Tutarlılık-Guid özniteliğini kullanırken AD DS Bağlayıcısı hesabı için izinler ayarlamak için (aka "Azure benim için kaynak çapayı yönetsin" seçeneği) çalıştırın: 

``` powershell
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [-SkipAdminSdHolders] [<CommonParameters>] 
```

veya; 

``` powershell
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [<CommonParameters>] 
```

Bu cmdlet aşağıdaki izinleri belirler: 

|Tür |Adı |Erişim |Uygulanan Öğe|
|-----|-----|-----|-----| 
|İzin Ver|AD DS Bağlayıcı Hesabı|Okuma/Yazma özelliği|Descendant Kullanıcı nesneleri|

### <a name="permissions-for-password-hash-synchronization"></a>Parola Karma Senkronizasyonu için izinler 
Parola Hash Eşitleme'yi kullanırken AD DS Bağlayıcısı hesabı için izinleri ayarlamak için aşağıdakileri çalıştırın: 

``` powershell
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [<CommonParameters>] 
```


veya; 

``` powershell
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountDN <String> [<CommonParameters>] 
```

Bu cmdlet aşağıdaki izinleri belirler: 

|Tür |Adı |Erişim |Uygulanan Öğe|
|-----|-----|-----|-----| 
|İzin Ver |AD DS Bağlayıcı Hesabı |Dizin Değişikliklerini Çoğaltma |Yalnızca bu nesne (Etki alanı kökü)| 
|İzin Ver |AD DS Bağlayıcı Hesabı |Çoğaltma Dizin Değişiklikleri Tümü |Yalnızca bu nesne (Etki alanı kökü)| 
  
### <a name="permissions-for-password-writeback"></a>Parola Yazma İzinleri 
Password Writeback'i kullanırken AD DS Bağlayıcısı hesabı için izinler ayarlamak için aşağıdakileri çalıştırın: 

``` powershell
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [-SkipAdminSdHolders] [<CommonParameters>] 
```


veya;

``` powershell
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [<CommonParameters>] 
```
Bu cmdlet aşağıdaki izinleri belirler: 

|Tür |Adı |Erişim |Uygulanan Öğe|
|-----|-----|-----|-----| 
|İzin Ver |AD DS Bağlayıcı Hesabı |Parola Sıfırlama |Descendant Kullanıcı nesneleri| 
|İzin Ver |AD DS Bağlayıcı Hesabı |Özellik kilitlemeZaman yaz |Descendant Kullanıcı nesneleri| 
|İzin Ver |AD DS Bağlayıcı Hesabı |Özellik pwdLastSet yaz |Descendant Kullanıcı nesneleri| 

### <a name="permissions-for-group-writeback"></a>Grup Yazma İzinleri 
Grup Yazma Geri Lemi kullanırken AD DS Bağlayıcısı hesabı için izinler ayarlamak için aşağıdakileri çalıştırın: 

``` powershell
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [-SkipAdminSdHolders] [<CommonParameters>] 
```
veya; 

``` powershell
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [<CommonParameters>]
```
 
Bu cmdlet aşağıdaki izinleri belirler: 

|Tür |Adı |Erişim |Uygulanan Öğe|
|-----|-----|-----|-----| 
|İzin Ver |AD DS Bağlayıcı Hesabı |Genel Okuma/Yazma |Nesne türü grubu ve alt nesnelerin tüm öznitelikleri| 
|İzin Ver |AD DS Bağlayıcı Hesabı |Alt nesne oluşturma/silme |Nesne türü grubu ve alt nesnelerin tüm öznitelikleri| 
|İzin Ver |AD DS Bağlayıcı Hesabı |Ağaç nesnelerini silme/silme|Nesne türü grubu ve alt nesnelerin tüm öznitelikleri|

### <a name="permissions-for-exchange-hybrid-deployment"></a>Exchange Karma Dağıtım Için İzinler 
Exchange Karma dağıtımını kullanırken AD DS Bağlayıcısı hesabı için izinler ayarlamak için çalıştırın: 

``` powershell
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [-SkipAdminSdHolders] [<CommonParameters>] 
```


veya; 

``` powershell
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [<CommonParameters>] 
```

Bu cmdlet aşağıdaki izinleri belirler:  
 

|Tür |Adı |Erişim |Uygulanan Öğe|
|-----|-----|-----|-----| 
|İzin Ver |AD DS Bağlayıcı Hesabı |Tüm özellikleri okuma/yazma |Descendant Kullanıcı nesneleri| 
|İzin Ver |AD DS Bağlayıcı Hesabı |Tüm özellikleri okuma/yazma |Descendant InetOrgPerson nesneleri| 
|İzin Ver |AD DS Bağlayıcı Hesabı |Tüm özellikleri okuma/yazma |Soyundan Gelen Grup nesneleri| 
|İzin Ver |AD DS Bağlayıcı Hesabı |Tüm özellikleri okuma/yazma |Descendant Contact nesneleri| 

### <a name="permissions-for-exchange-mail-public-folders-preview"></a>Exchange Mail Ortak Klasörleri için İzinler (Önizleme) 
Exchange Mail Public Folders özelliğini kullanırken AD DS Bağlayıcısı hesabı için izinler ayarlamak için aşağıdakileri çalıştırın: 

``` powershell
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String> [-SkipAdminSdHolders] [<CommonParameters>] 
```


veya; 

``` powershell
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [<CommonParameters>] 
```
Bu cmdlet aşağıdaki izinleri belirler: 

|Tür |Adı |Erişim |Uygulanan Öğe|
|-----|-----|-----|-----| 
|İzin Ver |AD DS Bağlayıcı Hesabı |Tüm özellikleri okuyun |Descendant PublicFolder nesneleri| 

### <a name="restrict-permissions-on-the-ad-ds-connector-account"></a>AD DS BağlayıcıSı Hesabındaki İzinleri Kısıtlama 
Bu PowerShell komut dosyası, parametre olarak sağlanan AD Bağlayıcı Hesabı için izinleri sıkılaştırır. Sıkılaştırma izinleri aşağıdaki adımları içerir: 

- Belirtilen nesnede devralmayı devre dışı 
- SELF söz konusu olduğunda varsayılan izinleri sağlam tutmak istediğimiz için SELF'e özgü ACE'ler hariç, belirli nesnedeki tüm ACE'leri kaldırın. 
 
  -ADConnectorAccountDN parametresi, izinleri sıkılaştırılması gereken AD hesabıdır. Bu genellikle AD DS Bağlayıcısı'nda yapılandırılan MSOL_nnnnnnnnnnnn etki alanı hesabıdır (bkz. AD DS Bağlayıcı Hesabınızı Belirleyin). -Kimlik bilgisi parametresi, hedef AD nesnesindeki Active Directory izinlerini kısıtlamak için gerekli ayrıcalıklara sahip Yönetici hesabını belirtmek için gereklidir. Bu genellikle Kurumsal veya Etki Alanı Yöneticisi'dir.  

``` powershell
Set-ADSyncRestrictedPermissions [-ADConnectorAccountDN] <String> [-Credential] <PSCredential> [-DisableCredentialValidation] [-WhatIf] [-Confirm] [<CommonParameters>] 
```
 
Örneğin: 

``` powershell
$credential = Get-Credential 
Set-ADSyncRestrictedPermissions -ADConnectorAccountDN'CN=ADConnectorAccount,CN=Users,DC=Contoso,DC=com' -Credential $credential  
```

Bu cmdlet aşağıdaki izinleri belirler: 

|Tür |Adı |Erişim |Uygulanan Öğe|
|-----|-----|-----|-----| 
|İzin Ver |SİSTEM |Tam Denetim |Bu nesne 
|İzin Ver |Enterprise Admins |Tam Denetim |Bu nesne 
|İzin Ver |Etki Alanı Yöneticileri |Tam Denetim |Bu nesne 
|İzin Ver |Yöneticiler |Tam Denetim |Bu nesne 
|İzin Ver |Kurumsal Etki Alanı Denetleyicileri |Liste İçerikleri |Bu nesne 
|İzin Ver |Kurumsal Etki Alanı Denetleyicileri |Tüm Özellikleri Oku |Bu nesne 
|İzin Ver |Kurumsal Etki Alanı Denetleyicileri |İzinleri Okuma |Bu nesne 
|İzin Ver |Kimliği Doğrulanmış Kullanıcılar |Liste İçerikleri |Bu nesne 
|İzin Ver |Kimliği Doğrulanmış Kullanıcılar |Tüm Özellikleri Oku |Bu nesne 
|İzin Ver |Kimliği Doğrulanmış Kullanıcılar |İzinleri Okuma |Bu nesne 

## <a name="next-steps"></a>Sonraki Adımlar
- [Azure AD Connect: Hesaplar ve izinler](reference-connect-accounts-permissions.md)
- [Ekspres Kurulum](how-to-connect-install-express.md)
- [Özel Kurulum](how-to-connect-install-custom.md)
- [ADSyncConfig Başvurusu](reference-connect-adsyncconfig.md)

