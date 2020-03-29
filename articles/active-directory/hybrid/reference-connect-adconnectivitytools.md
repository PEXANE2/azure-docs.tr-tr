---
title: 'Azure AD Connect: ADConnectivityTools PowerShell Başvuru | Microsoft Dokümanlar'
description: Bu belge ADConnectivityTools.psm1 PowerShell modülü için referans bilgileri sağlar.
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.date: 05/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.topic: reference
ms.collection: M365-identity-device-management
ms.openlocfilehash: d6b90ff82601acca1249c7d8c353944e39e89f95
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66473792"
---
# <a name="azure-ad-connect--adconnectivitytools-powershell-reference"></a>Azure AD Connect: ADConnectivityTools PowerShell Başvurusu

Aşağıdaki belgeler, Azure AD Connect ile birlikte verilen ADConnectivityTools.psm1 PowerShell Modülü için referans bilgileri sağlar.

## <a name="confirm-dnsconnectivity"></a>Confirm-DnsConnectivity

### <a name="synopsis"></a>ÖZET

Yerel Dns sorunlarını algılar.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Confirm-DnsConnectivity [-Forest] <String> [-DCs] <Array> [-ReturnResultAsPSObject] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Yerel Dns bağlantı testleri çalıştırıyor.
Active Directory bağlayıcısını yapılandırmak için, kullanıcının bağlanmaya çalıştığı orman için hem de bu ormanla ilişkili etki alanı denetleyicilerinde hem ad çözümlü olması gerekir.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Confirm-DnsConnectivity -Forest "TEST.CONTOSO.COM" -DCs "MYDC1.CONTOSO.COM","MYDC2.CONTOSO.COM"
```

#### <a name="example-2"></a>ÖRNEK 2

```powershell
Confirm-DnsConnectivity -Forest "TEST.CONTOSO.COM"
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-forest"></a>-Orman

Karşı test etmek için ormanın adını belirtir.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-dcs"></a>-DC'ler

Karşı sınamak için DC'ler belirtin.

```yml
Type: Array
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-returnresultaspsobject"></a>-ReturnResultAsPSObject

Bu tanının sonucunu PSObject şeklinde döndürür.
Bu araçla manuel etkileşim sırasında gerekli değildir.

```yml
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

## <a name="confirm-forestexists"></a>Onayla-OrmanVar

### <a name="synopsis"></a>ÖZET

Belirtilen bir ormanın var olup olmadığını belirler.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Confirm-ForestExists [-Forest] <String> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Bir ormanla ilişkili IP adresleri için bir DNS sunucusunu sorgular.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Confirm-TargetsAreReachable -Forest "TEST.CONTOSO.COM"
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-forest"></a>-Orman

Karşı test etmek için ormanın adını belirtir.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters

Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi içinhttps://go.microsoft.com/fwlink/?LinkID=113216)about_CommonParameters ( .

## <a name="confirm-functionallevel"></a>Onayla-İşlevsel Düzey

### <a name="synopsis"></a>ÖZET

AD orman işlevsel düzeyini doğrular.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="samaccount"></a>SamAccount

```
Confirm-FunctionalLevel -Forest <String> [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

#### <a name="forestfqdn"></a>ForestFQDN

```
Confirm-FunctionalLevel -ForestFQDN <Forest> [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

AD ormanı işlev düzeyinin belirli bir MinAdForestVersion'a (WindowsServer2003) eşit veya daha fazla olduğunu doğrular.
Hesap (Domain\Username) ve Şifre istenebilir.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Confirm-FunctionalLevel -Forest "test.contoso.com"
```

#### <a name="example-2"></a>ÖRNEK 2

```powershell
Confirm-FunctionalLevel -Forest "test.contoso.com" -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

#### <a name="example-3"></a>ÖRNEK 3

```powershell
Confirm-FunctionalLevel -ForestFQDN $ForestFQDN -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-forest"></a>-Orman

Hedef orman.
Varsayılan değer, şu anda oturum açmış olan kullanıcının Ormanı'dır.

```yml
Type: String
Parameter Sets: SamAccount
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-forestfqdn"></a>-ForestFQDN

ForestFQDN nesnesi hedef.

```yml
Type: Forest
Parameter Sets: ForestFQDN
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials

İşlev, kullanıcıdan özel kimlik bilgileri istemek yerine, şu anda bilgisayarda günlüğe kaydedilmiş olan kullanıcının kimlik bilgilerini kullanır.

```yml
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

## <a name="confirm-networkconnectivity"></a>Onayla-AğBağlantısı

### <a name="synopsis"></a>ÖZET

Yerel ağ bağlantısı sorunlarını algılar.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Confirm-NetworkConnectivity [-DCs] <Array> [-SkipDnsPort] [-ReturnResultAsPSObject] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Yerel ağ bağlantısı testlerini çalıştırın.

Yerel ağ testleri için AAD Connect, 53 (DNS), 88 (Kerberos) ve 389 (LDAP) bağlantı noktalarında ki adlandırılmış etki alanı denetleyicileriyle iletişim kurabilmeli ve çoğu kuruluş DC'lerinde DNS çalıştırabilir ve bu nedenle bu test şu anda tümleşik tir.
Başka bir DNS sunucusu belirtilmişse Bağlantı Noktası 53 atlanmalıdır.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Confirm-NetworkConnectivity -SkipDnsPort -DCs "MYDC1.CONTOSO.COM","MYDC2.CONTOSO.COM"
```

#### <a name="example-2"></a>ÖRNEK 2

```powershell
Confirm-NetworkConnectivity -DCs "MYDC1.CONTOSO.COM","MYDC2.CONTOSO.COM" -Verbose
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-dcs"></a>-DC'ler

Karşı sınamak için DC'ler belirtin.

```yml
Type: Array
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipdnsport"></a>-SkipDnsPort

Kullanıcı AD Sitesi / Logon DC tarafından sağlanan DNS hizmetlerini kullanmıyorsa, bağlantı noktası 53'ü denetlemeyi atlamak isteyebilir.
Kullanıcı hala _.ldap._tcp çözmek gerekir. \<Active Directory\> Connector yapılandırmasının başarılı olması için forestfqdn.

```yml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-returnresultaspsobject"></a>-ReturnResultAsPSObject

Bu tanının sonucunu PSObject şeklinde döndürür.
Bu araçla manuel etkileşim sırasında gerekli değildir.

```yml
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

## <a name="confirm-targetsarereachable"></a>Onayla-HedeflerUlaşılabilir

### <a name="synopsis"></a>ÖZET

Belirli bir ormana ve ilişkili Etki Alanı Denetleyicilerine erişilip erişilemeyilebildiğini belirler.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Confirm-TargetsAreReachable [-Forest] <String> [-DCs] <Array> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

"Ping" testlerini çalıştırıyor (bir bilgisayarın hedef bilgisayara ağ ve/veya internet üzerinden erişip ulaşamayacağı)

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Confirm-TargetsAreReachable -Forest "TEST.CONTOSO.COM" -DCs "MYDC1.CONTOSO.COM","MYDC2.CONTOSO.COM"
```

#### <a name="example-2"></a>ÖRNEK 2

```powershell
Confirm-TargetsAreReachable -Forest "TEST.CONTOSO.COM"
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-forest"></a>-Orman

Karşı test etmek için ormanın adını belirtir.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-dcs"></a>-DC'ler

Karşı sınamak için DC'ler belirtin.

```yml
Type: Array
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

## <a name="confirm-validdomains"></a>Onayla-Geçerli Etki Alanları

### <a name="synopsis"></a>ÖZET

Elde edilen Orman FQDN'deki etki alanlarının ulaşılabilir olduğunu doğrulayın

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="samaccount"></a>SamAccount

```
Confirm-ValidDomains [-Forest <String>] [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

#### <a name="forestfqdn"></a>ForestFQDN

```
Confirm-ValidDomains -ForestFQDN <Forest> [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Alan Adı Guid ve DomainDN'i almaya çalışarak, elde edilen Orman FQDN'deki tüm etki alanlarının erişilebildiğini doğrulayın.
Hesap (Domain\Username) ve Şifre istenebilir.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Confirm-ValidDomains -Forest "test.contoso.com" -Verbose
```

#### <a name="example-2"></a>ÖRNEK 2

```powershell
Confirm-ValidDomains -Forest "test.contoso.com" -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

#### <a name="example-3"></a>ÖRNEK 3

```powershell
Confirm-ValidDomains -ForestFQDN $ForestFQDN -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-forest"></a>-Orman

Hedef orman.

```yml
Type: String
Parameter Sets: SamAccount
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-forestfqdn"></a>-ForestFQDN

ForestFQDN nesnesi hedef.

```yml
Type: Forest
Parameter Sets: ForestFQDN
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials

İşlev, kullanıcıdan özel kimlik bilgileri istemek yerine, şu anda bilgisayarda günlüğe kaydedilmiş olan kullanıcının kimlik bilgilerini kullanır.

```yml
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

## <a name="confirm-validenterpriseadmincredentials"></a>Confirm-ValidEnterpriseAdminCredentials

### <a name="synopsis"></a>ÖZET

Bir kullanıcının Enterprise Admin kimlik bilgileri varsa doğrular.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Confirm-ValidEnterpriseAdminCredentials [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Sağlanan kullanıcının Enterprise Admin kimlik bilgileri varsa arar.
Hesap (Domain\Username) ve Şifre istenebilir.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Confirm-ValidEnterpriseAdminCredentials -DomainName test.contoso.com -Verbose
```

#### <a name="example-2"></a>ÖRNEK 2

```powershell
Confirm-ValidEnterpriseAdminCredentials -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials

İşlev, kullanıcıdan özel kimlik bilgileri istemek yerine, şu anda bilgisayarda günlüğe kaydedilmiş olan kullanıcının kimlik bilgilerini kullanır.

```yml
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

## <a name="get-domainfqdndata"></a>DomainFQDNData

### <a name="synopsis"></a>ÖZET

Bir hesap ve parola kombinasyonundan bir DomainFQDN alır.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-DomainFQDNData [[-DomainFQDNDataType] <String>] [-RunWithCurrentlyLoggedInUserCredentials]
 [-ReturnExceptionOnError] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Sağlanan kimlik bilgileri dışında bir etki alanıFQDN nesnesi elde etmeye çalışır.
Etki alanıFQDN geçerliyse, kullanıcının seçimine bağlı olarak bir DomainFQDNName veya RootDomainName döndürülür.
Hesap (Domain\Username) ve Şifre istenebilir.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Get-DomainFQDNData -DomainFQDNDataType DomainFQDNName -Verbose
```

#### <a name="example-2"></a>ÖRNEK 2

```powershell
Get-DomainFQDNData -DomainFQDNDataType RootDomainName -RunWithCurrentlyLoggedInUserCredentials
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-domainfqdndatatype"></a>-DomainFQDNDataType

Alınacak istenilen veri türü.
Şu anda "DomainFQDNName" veya "RootDomainName" ile sınırlıdır.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials

İşlev, kullanıcıdan özel kimlik bilgileri istemek yerine, şu anda bilgisayarda günlüğe kaydedilmiş olan kullanıcının kimlik bilgilerini kullanır.

```yml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-returnexceptiononerror"></a>-İadeÖzel Hatası

Start-NetworkConnectivityDiagnosisTools fonksiyonu tarafından kullanılan yardımcı parametre

```yml
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

## <a name="get-forestfqdn"></a>Al-ForestFQDN

### <a name="synopsis"></a>ÖZET

Bir hesap ve parola kombinasyonundan forestFQDN alır.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ForestFQDN [-Forest] <String> [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Sağlanan kimlik bilgilerinin dışında bir ForestFQDN elde etmeye çalışır.
Hesap (Domain\Username) ve Şifre istenebilir.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Get-ForestFQDN -Forest CONTOSO.MICROSOFT.COM -Verbose
```

#### <a name="example-2"></a>ÖRNEK 2

```powershell
Get-ForestFQDN -Forest CONTOSO.MICROSOFT.COM -RunWithCurrentlyLoggedInUserCredentials -Verbose
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-forest"></a>-Orman

Hedef orman. Varsayılan değer, şu anda oturum açmış olan kullanıcının Etki Alanıdır.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-RunWithCurrentlyLoggedInUserCredentials

İşlev, kullanıcıdan özel kimlik bilgileri istemek yerine, şu anda bilgisayarda günlüğe kaydedilmiş olan kullanıcının kimlik bilgilerini kullanır.

```yml
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

## <a name="start-connectivityvalidation"></a>Başlangıç-Bağlantı Doğrulama

### <a name="synopsis"></a>ÖZET

Ana fonksiyon.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Start-ConnectivityValidation [-Forest] <String> [-AutoCreateConnectorAccount] <Boolean> [[-UserName] <String>]
 [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

AD kimlik bilgilerinin geçerli olduğunu doğrulayan tüm kullanılabilir mekanizmaları çalıştırın.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Start-ConnectivityValidation -Forest "test.contoso.com" -AutoCreateConnectorAccount $True -Verbose
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-forest"></a>-Orman

Hedef orman.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-autocreateconnectoraccount"></a>-AutoCreateConnectorAccount

Özel yüklemeler için: Kullanıcı AADConnect sihirbazının AD Orman Hesabı penceresinde "Yeni AD hesabı oluştur" sözcüünü seçtiyse $True bayrak.
kullanıcı "Varolan AD hesabını kullan" ifadesini seçip seçmedi $False.
Ekspres yüklemeler için: Bu değişkenin değeri Express yüklemeleri için $True olmalıdır.

```yml
Type: Boolean
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-username"></a>-UserName

Kullanıcı kimlik bilgileri istendiğinde Kullanıcı adı alanını önceden dolduran parametre.

```yml
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

## <a name="start-networkconnectivitydiagnosistools"></a>Başlangıç-AğConnectivityDiagnosisTools

### <a name="synopsis"></a>ÖZET

Ağ bağlantısı testleri için ana işlev.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Start-NetworkConnectivityDiagnosisTools [[-Forest] <String>] [-Credentials] <PSCredential>
 [[-LogFileLocation] <String>] [[-DCs] <Array>] [-DisplayInformativeMessage] [-ReturnResultAsPSObject]
 [-ValidCredentials] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Yerel ağ bağlantısı testlerini çalıştırın.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1

```powershell
Start-NetworkConnectivityDiagnosisTools -Forest "TEST.CONTOSO.COM"
```

#### <a name="example-2"></a>ÖRNEK 2

```powershell
Start-NetworkConnectivityDiagnosisTools -Forest "TEST.CONTOSO.COM" -DCs "DC1.TEST.CONTOSO.COM", "DC2.TEST.CONTOSO.COM"
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-forest"></a>-Orman

Karşı test etmek için orman adını belirtir.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-credentials"></a>-Kimlik bilgileri

Testi çalıştıran kullanıcının kullanıcı adı ve parolası.
Azure AD Bağlantı Sihirbazı'nı çalıştırmak için gereken izin düzeyinin aynıolmasını gerektirir.

```yml
Type: PSCredential
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-logfilelocation"></a>-LogFileLocation

Bu işlevin çıktısını içerecek bir günlük dosyasının konumunu belirtir.

```yml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-dcs"></a>-DC'ler

Karşı sınamak için DC'ler belirtin.

```yml
Type: Array
Parameter Sets: (All)
Aliases:

Required: False
Position: 4
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-displayinformativemessage"></a>-Görüntülü BilgilendiriciMesaj

Bu işlevin amacı hakkında bir ileti nin görüntülenmesine izin veren bayrak.

```yml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-returnresultaspsobject"></a>-ReturnResultAsPSObject

Bu tanının sonucunu PSObject şeklinde döndürür.
Bu araçla manuel etkileşim sırasında belirtmeniz gerekmez.

```yml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-validcredentials"></a>-Geçerli Kimlik Bilgileri

Kullanıcının yazdığı kimlik bilgilerinin geçerli olup olmadığını gösterir.
Bu araçla manuel etkileşim sırasında belirtmeniz gerekmez.

```yml
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
