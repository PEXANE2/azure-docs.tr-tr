---
title: 'Azure AD Connect: ADSyncTools PowerShell Başvuru | Microsoft Dokümanlar'
description: Bu belge ADSyncTools.psm1 PowerShell modülü için referans bilgileri sağlar.
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.date: 10/19/2018
ms.subservice: hybrid
ms.author: billmath
ms.topic: reference
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9a1b8abf15233c06e8ff9e507b315cc8a3703970
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60454668"
---
# <a name="azure-ad-connect--adsynctools-powershell-reference"></a>Azure AD Connect: ADSyncTools PowerShell Başvurusu
Aşağıdaki belgeler, Azure AD Connect ile birlikte verilen ADSyncTools.psm1 PowerShell Modülü için başvuru bilgilerini sağlar.

## <a name="clear-adsynctoolsconsistencyguid"></a>Clear-ADSyncToolsConsistencyGuid

### <a name="synopsis"></a>ÖZET
MS-Ds-ConsistencyGuid'i AD Kullanıcısından Temizle

### <a name="syntax"></a>SÖZ DİZİMİ

```
Clear-ADSyncToolsConsistencyGuid [-User] <Object> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Hedef AD kullanıcısı için mS-Ds-ConsistencyGuid değerini temizleyin

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-user"></a>-User
AD'de Hedef Kullanıcı yı ayarlamak için

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi içinhttps://go.microsoft.com/fwlink/?LinkID=113216)about_CommonParameters ( .

## <a name="confirm-adsynctoolsadmoduleloaded"></a>Confirm-ADSyncToolsADModuleLoaded

### <a name="synopsis"></a>ÖZET
{{Özet'i doldurun}}

### <a name="syntax"></a>SÖZ DİZİMİ

```
Confirm-ADSyncToolsADModuleLoaded
```

### <a name="description"></a>AÇIKLAMA
{{Açıklamayı doldurun}}

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>Örnek 1
```powershell
PS C:\> {{ Add example code here }}
```

{{ Örnek açıklama ekleyin buraya }}

## <a name="connect-adsyncdatabase"></a>Connect-AdSyncDatabase

### <a name="synopsis"></a>ÖZET
{{Özet'i doldurun}}

### <a name="syntax"></a>SÖZ DİZİMİ

```
Connect-AdSyncDatabase [-Server] <String> [[-Instance] <String>] [[-Database] <String>] [[-UserName] <String>]
 [[-Password] <String>] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
{{Açıklamayı doldurun}}

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>Örnek 1
```powershell
PS C:\> {{ Add example code here }}
```

{{ Örnek açıklama ekleyin buraya }}

### <a name="parameters"></a>PARAMETRELER

#### <a name="-database"></a>-Veritabanı
{{Veritabanı Açıklamasını Doldur}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-instance"></a>-Örnek
{{Fill Instance Description}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-password"></a>-Password
{{Parola Açıklamasını Doldur}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 4
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-server"></a>-Server
{{Sunucu Açıklamasını Doldur}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-username"></a>-UserName
{{Kullanıcı Adı Açıklamasını Doldur}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi içinhttps://go.microsoft.com/fwlink/?LinkID=113216)about_CommonParameters ( .

## <a name="export-adsynctoolsconsistencyguidmigration"></a>Dışa Aktarma-ADSyncToolsConsistencyGuidMigration

### <a name="synopsis"></a>ÖZET
İhracat TutarlılığıGuid Raporu

### <a name="syntax"></a>SÖZ DİZİMİ

```
Export-ADSyncToolsConsistencyGuidMigration [-AlternativeLoginId] [-UserPrincipalName] <String>
 [-ImmutableIdGUID] <String> [-Output] <String> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Alma-ADSyncToolsImmutableIdMigration bir alma CSV dosyasına dayalı bir TutarlılıkGuid raporu oluşturur

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Import-Csv .\AllSyncUsers.csv | Export-ADSyncToolsConsistencyGuidMigration -Output ".\AllSyncUsers-Report"
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-alternativeloginid"></a>-AlternativeLoginId
Alternatif Giriş Kimliği (posta) kullanın

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-userprincipalname"></a>-UserPrincipalName
UserPrincipalName

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-immutableidguid"></a>-DeğişmezIdGUID
DeğişmezIdGUID

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-output"></a>-Çıktı
CSV ve LOG dosyaları için çıktı dosya adı

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi içinhttps://go.microsoft.com/fwlink/?LinkID=113216)about_CommonParameters ( .

## <a name="get-adsyncsqlbrowserinstances"></a>Get-ADSyncSQLBrowserInstances

### <a name="synopsis"></a>ÖZET
{{Özet'i doldurun}}

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ADSyncSQLBrowserInstances [[-hostName] <String>]
```

### <a name="description"></a>AÇIKLAMA
{{Açıklamayı doldurun}}

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>Örnek 1
```powershell
PS C:\> {{ Add example code here }}
```

{{ Örnek açıklama ekleyin buraya }}

### <a name="parameters"></a>PARAMETRELER

#### <a name="-hostname"></a>-hostName
{{Doldur hostName Açıklama}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

## <a name="get-adsynctoolsaduser"></a>Get-ADSyncToolsADuser

### <a name="synopsis"></a>ÖZET
AD'den kullanıcı alma

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ADSyncToolsADuser [-User] <Object> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Yapılacak bir AD nesnesini döndürür: Çoklu orman desteği

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-user"></a>-User
Tutarlılık Guid ayarlamak için AD Hedef Kullanıcı

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi içinhttps://go.microsoft.com/fwlink/?LinkID=113216)about_CommonParameters ( .

## <a name="get-adsynctoolsconsistencyguid"></a>Get-ADSyncToolsConsistencyGuid

### <a name="synopsis"></a>ÖZET
AD Kullanıcısından mS-Ds-ConsistencyGuid alın

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ADSyncToolsConsistencyGuid [-User] <Object> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
GUID biçiminde hedef AD kullanıcısının mS-Ds-ConsistencyGuid özniteliğindeki değeri verir

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-user"></a>-User
AD'de Hedef Kullanıcı yı ayarlamak için

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi içinhttps://go.microsoft.com/fwlink/?LinkID=113216)about_CommonParameters ( .

## <a name="get-adsynctoolsobjectguid"></a>Get-ADSyncToolsObjectGuid

### <a name="synopsis"></a>ÖZET
OBJECTGuid'i AD Kullanıcısından Alın

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ADSyncToolsObjectGuid [-User] <Object> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Guid biçiminde hedef AD kullanıcısının ObjectGUID özniteliğindeki değeri verir

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-user"></a>-User
AD'de Hedef Kullanıcı yı ayarlamak için

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi içinhttps://go.microsoft.com/fwlink/?LinkID=113216)about_CommonParameters ( .

## <a name="get-adsynctoolsrunhistory"></a>Get-ADSyncToolsRunHistory

### <a name="synopsis"></a>ÖZET
AAD Connect Run Geçmişi'ni alın

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ADSyncToolsRunHistory [[-Days] <Int32>] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
AAD Connect Run Geçmişini XML biçiminde döndüren işlev

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Get-ADSyncToolsRunHistory
```

#### <a name="example-2"></a>ÖRNEK 2
```
Get-ADSyncToolsRunHistory -Days 1
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-days"></a>-Gün Sayısı
{{Fill Gün Açıklaması}}

```yaml
Type: Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: 3
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi içinhttps://go.microsoft.com/fwlink/?LinkID=113216)about_CommonParameters ( .

## <a name="get-adsynctoolssourceanchorchanged"></a>Get-ADSyncToolsSourceAnchor Değiştirildi

### <a name="synopsis"></a>ÖZET
SourceAnchor değiştirilen hataları olan kullanıcıları alın

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ADSyncToolsSourceAnchorChanged [-sourcePath] <Object> [-outputPath] <Object> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
İşlev sorguları AAD Connect Run History ve Hata bildiren tüm kullanıcılar dışa aktarım: "SourceAnchor özniteliği değişti."

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
#Required Parameters
```

$sourcePath = Read-Host -Prompt "Dosya adı ile günlük\<\>dosya yolunuzu girin" #" Source_Path " $outputPath = Oku-Host -Prompt "Dosya adı ile dosya dışına yol girin" #"\<Out_Path\>"
 
 Get-ADSyncToolsUsersSourceAnchorChanged -sourcePath $sourcePath -outputPath $outputPath

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-sourcepath"></a>-sourcePath
{{Fill sourcePath Description}}

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-outputpath"></a>-outputPath
{{Fill outputPath Description}}

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi içinhttps://go.microsoft.com/fwlink/?LinkID=113216)about_CommonParameters ( .

## <a name="import-adsynctoolsimmutableidmigration"></a>İthalat-ADSyncToolsImmutableIdMigration

### <a name="synopsis"></a>ÖZET
AAD'den İthalat DeğişmezID

### <a name="syntax"></a>SÖZ DİZİMİ

```
Import-ADSyncToolsImmutableIdMigration [-Output] <String> [-IncludeSyncUsersFromRecycleBin]
 [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
GUID biçim Gereksinimlerinde Değişmez ID değerini içeren tüm Azure AD Senkronize kullanıcılarıyla bir dosya oluşturur: MSOnline PowerShell Modülü

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Import-ADSyncToolsImmutableIdMigration -OutputFile '.\AllSyncUsers.csv'
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-output"></a>-Çıktı
Çıktı CSV dosyası

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-includesyncusersfromrecyclebin"></a>-IncludeSyncUsersFromRecycleBin
Azure AD Geri Dönüşüm Kutusu'ndan Senkronize Kullanıcıları Alın

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi içinhttps://go.microsoft.com/fwlink/?LinkID=113216)about_CommonParameters ( .


## <a name="invoke-adsyncdatabasequery"></a>Invoke-AdSyncDatabaseQuery

### <a name="synopsis"></a>ÖZET
{{Özet'i doldurun}}

### <a name="syntax"></a>SÖZ DİZİMİ

```
Invoke-AdSyncDatabaseQuery [-SqlConnection] <SqlConnection> [[-Query] <String>] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
{{Açıklamayı doldurun}}

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>Örnek 1
```powershell
PS C:\> {{ Add example code here }}
```

{{ Örnek açıklama ekleyin buraya }}

### <a name="parameters"></a>PARAMETRELER

#### <a name="-query"></a>-Sorgula
{{Sorgu Açıklamasını Doldur}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-sqlconnection"></a>-SqlConnection
{{SqlConnection Açıklamalarını Doldur}}

```yaml
Type: SqlConnection
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi içinhttps://go.microsoft.com/fwlink/?LinkID=113216)about_CommonParameters ( .

## <a name="remove-adsynctoolsexpiredcertificates"></a>Kaldır-ADSyncToolsExpiredCertificates

### <a name="synopsis"></a>ÖZET
Süresi Dolan Sertifikaları UserCertificate Özniteliğinden Kaldırmak Için Komut Dosyası

### <a name="syntax"></a>SÖZ DİZİMİ

```
Remove-ADSyncToolsExpiredCertificates [-TargetOU] <String> [[-BackupOnly] <Boolean>] [-ObjectClass] <String>
 [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Bu komut dosyası, Object Class (Kullanıcı/Bilgisayar) tarafından filtrelenen Active Directory etki alanınızdaki hedef Kuruluş Birimi'nden tüm nesneleri alır ve UserCertificate özniteliğinde bulunan süresi dolmuş tüm sertifikaları siler.
Varsayılan olarak (BackupOnly modu) yalnızca süresi dolmuş sertifikaları bir dosyaya yedekler ve AD'de herhangi bir değişiklik yapmaz.
-BackupOnly $false kullanırsanız, bu nesneler için UserCertificate özniteliği nde bulunan süresi dolmuş herhangi bir Sertifika, dosyaya kopyalandıktan sonra AD'den kaldırılır.
Her sertifika ayrılmış bir dosya adına yedeklenir: ObjectClass_ObjectGUID_CertThumprint.cer Komut dosyası, gerçek eylem (Atlanan/Dışa Aktarılan/Silinen) dahil olmak üzere geçerli veya süresi dolmuş sertifikalara sahip tüm kullanıcıları gösteren CSV formatında bir günlük dosyası da oluşturur.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Check all users in target OU - Expired Certificates will be copied to separated files and no certificates will be removed
```

Remove-ADSyncToolsExpiredCertificates -TargetOU "OU=Kullanıcılar,OU=Corp,DC=Contoso,DC=com" -ObjectClass kullanıcısı

#### <a name="example-2"></a>ÖRNEK 2
```
Delete Expired Certs from all Computer objects in target OU - Expired Certificates will be copied to files and removed from AD
```

Remove-ADSyncToolsExpiredCertificates -TargetOU "OU=Computers,OU=Corp,DC=Contoso,DC=com" -ObjectClass bilgisayar -BackupOnly$false

### <a name="parameters"></a>PARAMETRELER

#### <a name="-targetou"></a>-TargetOU
HEDEF OU AD nesneleri için aramak için

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-backuponly"></a>-BackupOnly
BackupOnly AD herhangi bir sertifika silmek olmaz

```yaml
Type: Boolean
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: True
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-objectclass"></a>-ObjectClass
Nesne Sınıfı filtresi

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi içinhttps://go.microsoft.com/fwlink/?LinkID=113216)about_CommonParameters ( .

## <a name="repair-adsynctoolsautoupgradestate"></a>Onarım-ADSyncToolsAutoUpgradeState

### <a name="synopsis"></a>ÖZET
Kısa açıklama

### <a name="syntax"></a>SÖZ DİZİMİ

```
Repair-ADSyncToolsAutoUpgradeState
```

### <a name="description"></a>AÇIKLAMA
Uzun açıklama

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

## <a name="resolve-adsynchostaddress"></a>Çözüm-ADSyncHostAddress

### <a name="synopsis"></a>ÖZET
{{Özet'i doldurun}}

### <a name="syntax"></a>SÖZ DİZİMİ

```
Resolve-ADSyncHostAddress [[-hostName] <String>]
```

### <a name="description"></a>AÇIKLAMA
{{Açıklamayı doldurun}}

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>Örnek 1
```powershell
PS C:\> {{ Add example code here }}
```

{{ Örnek açıklama ekleyin buraya }}

### <a name="parameters"></a>PARAMETRELER

#### <a name="-hostname"></a>-hostName
{{Doldur hostName Açıklama}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

## <a name="restore-adsynctoolsexpiredcertificates"></a>Geri Yükleme-ADSyncToolsExpiredCertificates

### <a name="synopsis"></a>ÖZET
(YapılacakLAR) Ad UserCertificate özniteliğini sertifika dosyasından geri yükler

### <a name="syntax"></a>SÖZ DİZİMİ

```
Restore-ADSyncToolsExpiredCertificates
```

### <a name="description"></a>AÇIKLAMA
Uzun açıklama

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

## <a name="set-adsynctoolsconsistencyguid"></a>Set-ADSyncToolsConsistencyGuid

### <a name="synopsis"></a>ÖZET
MS-Ds-ConsistencyGuid'i AD Kullanıcısında Ayarlama

### <a name="syntax"></a>SÖZ DİZİMİ

```
Set-ADSyncToolsConsistencyGuid [-User] <Object> [-Value] <Object> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Hedef AD kullanıcısı için mS-Ds-ConsistencyGuid özniteliğinde bir değer ayarlama

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-user"></a>-User
Tutarlılık Guid ayarlamak için AD Hedef Kullanıcı

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-value"></a>-Değer
Değişmez (Bayt dizisi, GUID, GUID dizesi veya Base64 dizesi)

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi içinhttps://go.microsoft.com/fwlink/?LinkID=113216)about_CommonParameters ( .

## <a name="test-adsyncnetworkport"></a>Test-ADSyncNetworkPort

### <a name="synopsis"></a>ÖZET
{{Özet'i doldurun}}

### <a name="syntax"></a>SÖZ DİZİMİ

```
Test-ADSyncNetworkPort [[-hostName] <String>] [[-port] <String>]
```

### <a name="description"></a>AÇIKLAMA
{{Açıklamayı doldurun}}

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>Örnek 1
```powershell
PS C:\> {{ Add example code here }}
```

{{ Örnek açıklama ekleyin buraya }}

### <a name="parameters"></a>PARAMETRELER

#### <a name="-hostname"></a>-hostName
{{Doldur hostName Açıklama}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-port"></a>-bağlantı noktası
{{Dolgu bağlantı noktası Açıklaması}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

## <a name="trace-adsynctoolsadimport"></a>İzleme-ADSyncToolsADImport

### <a name="synopsis"></a>ÖZET
Bir izleme dosyası ve AD Alma Adımı oluşturur

### <a name="syntax"></a>SÖZ DİZİMİ

```
Trace-ADSyncToolsADImport [[-ADConnectorXML] <String>] [[-dc] <String>] [[-rootDN] <String>]
 [[-filter] <String>] [-SkipCredentials] [[-ADwatermark] <String>] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Belirli bir AD filigran denetim noktasından (bölüm çerezi) AAD Connect AD Alma çalışmasının tüm ldap sorgularını izler. Geçerli klasörde '.\ADimportTrace_yyyyMMddHHmmss.log' izleme dosyası oluşturur.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adconnectorxml"></a>-ADConnectorXML
{{DOLGU ADConnectorXML Açıklama}}

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-dc"></a>-dc
AD Bağlayıcısı Dışa Aktarma XML dosyası

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: DC1.domain.contoso.com
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-rootdn"></a>-rootDN
Hedef Etki Alanı Denetleyicisi

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 3
Default value: DC=Domain,DC=Contoso,DC=com
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-filter"></a>-filtre
Orman Kökü DN

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 4
Default value: (&(objectClass=*))
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipcredentials"></a>-Atlama Kimlik Bilgileri
İzlemek \> için AD nesnesi türleri * = tüm nesne türleri

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adwatermark"></a>-ADwatermark
Etki Alanı Yöneticisi olarak çalışıyorsanız, AD kimlik bilgilerini sağlamaya gerek yoktur.
Filigran Manuel giriş, xml dosyası yerine örneğin $ADwatermark = "TVNEUwMAAAAXyK9ir1zSAAAAAAAAAAAAAA(...)"

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 5
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi içinhttps://go.microsoft.com/fwlink/?LinkID=113216)about_CommonParameters ( .

## <a name="trace-adsynctoolsldapquery"></a>İz-ADSyncToolsLdapQuery

### <a name="synopsis"></a>ÖZET
Kısa açıklama

### <a name="syntax"></a>SÖZ DİZİMİ

```
Trace-ADSyncToolsLdapQuery [-Context] <String> [-Server] <String> [-Port] <Int32> [-Filter] <String>
 [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Uzun açıklama

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Example of how to use this cmdlet
```

#### <a name="example-2"></a>ÖRNEK 2
```
Another example of how to use this cmdlet
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-context"></a>-Bağlam
Param1 yardım açıklaması

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-server"></a>-Server
Param2 yardım açıklaması

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: Localhost
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-port"></a>-Bağlantı noktası
Param2 yardım açıklaması

```yaml
Type: Int32
Parameter Sets: (All)
Aliases:

Required: True
Position: 3
Default value: 389
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-filter"></a>-Filtre
Param2 yardım açıklaması

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 4
Default value: (objectClass=*)
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi içinhttps://go.microsoft.com/fwlink/?LinkID=113216)about_CommonParameters ( .

## <a name="update-adsynctoolsconsistencyguidmigration"></a>Güncelleme-ADSyncToolsConsistencyGuidMigration

### <a name="synopsis"></a>ÖZET
Kullanıcıları yeni ConsistencyGuid (ImmutableId) ile güncelleştirir

### <a name="syntax"></a>SÖZ DİZİMİ

```
Update-ADSyncToolsConsistencyGuidMigration [[-DistinguishedName] <String>] [-ImmutableIdGUID] <String>
 [-Action] <String> [-Output] <String> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
TutarlılıkGuid (ImmutableId) değerindeki kullanıcıları güncellerTutarlılıkGuid Raporu'ndan alınan bu işlev WhatIf anahtarını destekler Not: TutarlılıkGuid Raporu Sekme Demiliter ile içe aktarılmalıdır

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Import-Csv .\AllSyncUsersTEST-Report.csv -Delimiter "`t"| Update-ADSyncToolsConsistencyGuidMigration -Output .\AllSyncUsersTEST-Result2 -WhatIf
```

#### <a name="example-2"></a>ÖRNEK 2
```
Import-Csv .\AllSyncUsersTEST-Report.csv -Delimiter "`t"| Update-ADSyncToolsConsistencyGuidMigration -Output .\AllSyncUsersTEST-Result2
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-distinguishedname"></a>-Seçkin Name
Seçkin Ad

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: False
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-immutableidguid"></a>-DeğişmezIdGUID
DeğişmezIdGUID

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-action"></a>-Eylem
Eylem

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 3
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

#### <a name="-output"></a>-Çıktı
LOG dosyaları için çıktı dosya adı

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 4
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-whatif"></a>-WhatIf
Cmdlet çalıştırılıyorsa ne olacağını gösterir.
Cmdlet çalıştırılmaz.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-confirm"></a>-Confirm
Cmdlet'i çalıştırmadan önce sizden onay ister.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi içinhttps://go.microsoft.com/fwlink/?LinkID=113216)about_CommonParameters ( .
