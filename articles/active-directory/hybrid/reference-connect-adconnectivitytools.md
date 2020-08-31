---
title: 'Azure AD Connect: ADConnectivityTools PowerShell Başvurusu | Microsoft Docs'
description: Bu belge, ADConnectivityTools. psm1 PowerShell modülü için başvuru bilgileri sağlar.
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.date: 05/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.topic: reference
ms.collection: M365-identity-device-management
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: d5e06d0a6f7693ca9cc69f05f6b2318d6b60b5e3
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89070811"
---
# <a name="azure-ad-connect--adconnectivitytools-powershell-reference"></a>Azure AD Connect: ADConnectivityTools PowerShell Başvurusu

Aşağıdaki belgeler Azure AD Connect eklenen ADConnectivityTools. psm1 PowerShell modülü için başvuru bilgileri sağlar.

## <a name="confirm-dnsconnectivity"></a>Onayla-DnsConnectivity

### <a name="synopsis"></a>ÖZET

Yerel DNS sorunlarını algılar.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Confirm-DnsConnectivity [-Forest] <String> [-DCs] <Array> [-ReturnResultAsPSObject] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Yerel DNS bağlantısı testlerini çalıştırır.
Active Directory bağlayıcısını yapılandırmak için, kullanıcının bağlanmayı denedikleri ormana ve bu ormanla ilişkilendirilmiş etki alanı denetleyicilerine, her iki ada de sahip olması gerekir.

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

Sınanacak ormanın adını belirtir.

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

#### <a name="-dcs"></a>-DCs

Sınanacak DC 'leri belirtin.

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

Bu tanılamada bir PSObject biçiminde sonucunu döndürür.
Bu araçla el ile etkileşim sırasında gerekli değildir.

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .

## <a name="confirm-forestexists"></a>Onayla-ForestExists

### <a name="synopsis"></a>ÖZET

Belirtilen bir ormanın mevcut olup olmadığını belirler.

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

Sınanacak ormanın adını belirtir.

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .

## <a name="confirm-functionallevel"></a>Onayla-FunctionalLevel

### <a name="synopsis"></a>ÖZET

AD Ormanı işlev düzeyini doğrular.

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

AD Ormanı işlev düzeyinin verilen bir MinAdForestVersion (WindowsServer2003) ile aynı veya daha fazla olduğunu doğrular.
Hesap (etkialanı \ Kullanıcı adı) ve parola istenebilir.

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
Varsayılan değer, şu anda oturum açmış kullanıcının ormanıdır.

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

Hedef ForestFQDN nesnesi.

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

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-Runwithcurrentlyloggedınusercredentials

İşlevi, Kullanıcı tarafından kullanıcıdan özel kimlik bilgileri istemek yerine şu anda bilgisayarda oturum açmış olan kullanıcının kimlik bilgilerini kullanır.

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .

## <a name="confirm-networkconnectivity"></a>Onayla-NetworkConnectivity

### <a name="synopsis"></a>ÖZET

Yerel ağ bağlantısı sorunlarını algılar.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Confirm-NetworkConnectivity [-DCs] <Array> [-SkipDnsPort] [-ReturnResultAsPSObject] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Yerel ağ bağlantısı testlerini çalıştırır.

Yerel ağ testlerinde, AAD Connect, 53 (DNS), 88 (Kerberos) ve 389 (LDAP) bağlantı noktalarında adlandırılmış etki alanı denetleyicileriyle iletişim kurabilmesi gerekir, bu da bu testin Şu anda tümleştirildiği anlamına gelir.
Başka bir DNS sunucusu belirtilmişse bağlantı noktası 53 atlanmalıdır.

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

#### <a name="-dcs"></a>-DCs

Sınanacak DC 'leri belirtin.

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

Kullanıcı AD sitesi/oturum açma DC tarafından sunulan DNS hizmetlerini kullanmadıysa, bağlantı noktası 53 ' ı denetlemeyi atlamak isteyebilir.
Kullanıcı yine de _. LDAP. _tcp çözümleyebilmelidir.\<forestfqdn\>
Active Directory Bağlayıcısı yapılandırmasının başarılı olması için.

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

Bu tanılamada bir PSObject biçiminde sonucunu döndürür.
Bu araçla el ile etkileşim sırasında gerekli değildir.

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .

## <a name="confirm-targetsarereachable"></a>Onayla-TargetsAreReachable

### <a name="synopsis"></a>ÖZET

Belirtilen bir ormanın ve ilişkili etki alanı denetleyicilerinin erişilebilir olup olmadığını belirler.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Confirm-TargetsAreReachable [-Forest] <String> [-DCs] <Array> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

"Ping" testlerini çalıştırır (bir bilgisayarın, ağ ve/veya internet üzerinden bir hedef bilgisayara erişip erişemeyeceğini belirtir)

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

Sınanacak ormanın adını belirtir.

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

#### <a name="-dcs"></a>-DCs

Sınanacak DC 'leri belirtin.

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .

## <a name="confirm-validdomains"></a>Onaylama-ValidDomains

### <a name="synopsis"></a>ÖZET

Alınan orman FQDN 'SI içindeki etki alanlarının erişilebilir olduğunu doğrulama

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

DomainGuid ve DomainDN 'leri almaya çalışırken, alınan orman FQDN içindeki tüm etki alanlarının erişilebilir olduğunu doğrulayın.
Hesap (etkialanı \ Kullanıcı adı) ve parola istenebilir.

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

Hedef ForestFQDN nesnesi.

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

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-Runwithcurrentlyloggedınusercredentials

İşlevi, Kullanıcı tarafından kullanıcıdan özel kimlik bilgileri istemek yerine şu anda bilgisayarda oturum açmış olan kullanıcının kimlik bilgilerini kullanır.

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .

## <a name="confirm-validenterpriseadmincredentials"></a>Onay-Valıdenterpriseadmincredentials

### <a name="synopsis"></a>ÖZET

Bir kullanıcının kuruluş yöneticisi kimlik bilgilerine sahip olup olmadığını doğrular.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Confirm-ValidEnterpriseAdminCredentials [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Belirtilen kullanıcının kuruluş yöneticisi kimlik bilgileri varsa arar.
Hesap (etkialanı \ Kullanıcı adı) ve parola istenebilir.

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

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-Runwithcurrentlyloggedınusercredentials

İşlevi, Kullanıcı tarafından kullanıcıdan özel kimlik bilgileri istemek yerine şu anda bilgisayarda oturum açmış olan kullanıcının kimlik bilgilerini kullanır.

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .

## <a name="get-domainfqdndata"></a>Get-DomainFQDNData

### <a name="synopsis"></a>ÖZET

Bir hesap ve parola birleşiminin dışında bir DomainFQDN alır.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-DomainFQDNData [[-DomainFQDNDataType] <String>] [-RunWithCurrentlyLoggedInUserCredentials]
 [-ReturnExceptionOnError] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Bir domainFQDN nesnesini, belirtilen kimlik bilgilerinden elde etme girişiminde bulunur.
DomainFQDN geçerliyse, kullanıcının seçimine bağlı olarak bir DomainFQDNName veya RootDomainName döndürülür.
Hesap (etkialanı \ Kullanıcı adı) ve parola istenebilir.

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

Alınacak istenen veri türü.
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

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-Runwithcurrentlyloggedınusercredentials

İşlevi, Kullanıcı tarafından kullanıcıdan özel kimlik bilgileri istemek yerine şu anda bilgisayarda oturum açmış olan kullanıcının kimlik bilgilerini kullanır.

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

#### <a name="-returnexceptiononerror"></a>-ReturnExceptionOnError

Start-NetworkConnectivityDiagnosisTools işlevi tarafından kullanılan yardımcı parametre

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .

## <a name="get-forestfqdn"></a>Get-ForestFQDN

### <a name="synopsis"></a>ÖZET

Bir hesap ve parola birleşiminin dışında bir ForestFQDN alır.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ForestFQDN [-Forest] <String> [-RunWithCurrentlyLoggedInUserCredentials] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Belirtilen kimlik bilgilerinden bir ForestFQDN almaya çalışır.
Hesap (etkialanı \ Kullanıcı adı) ve parola istenebilir.

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

Hedef orman. Varsayılan değer, şu anda oturum açmış olan kullanıcının etki alanıdır.

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

#### <a name="-runwithcurrentlyloggedinusercredentials"></a>-Runwithcurrentlyloggedınusercredentials

İşlevi, Kullanıcı tarafından kullanıcıdan özel kimlik bilgileri istemek yerine şu anda bilgisayarda oturum açmış olan kullanıcının kimlik bilgilerini kullanır.

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .

## <a name="start-connectivityvalidation"></a>Start-ConnectivityValidation

### <a name="synopsis"></a>ÖZET

Main işlevi.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Start-ConnectivityValidation [-Forest] <String> [-AutoCreateConnectorAccount] <Boolean> [[-UserName] <String>]
 [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

AD kimlik bilgilerini doğrulayan tüm kullanılabilir mekanizmaların geçerli olduğunu doğrular.

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

#### <a name="-autocreateconnectoraccount"></a>-Oto Createconnectoraccount

Özel yüklemeler için: Kullanıcı, AADConnect sihirbazının AD orman hesabı penceresinde "yeni AD hesabı oluştur" seçeneğini seçerse $True bayrak.
Kullanıcı "var olan AD hesabını kullan" seçeneğini seçerse $False.
Hızlı yüklemeler için: Bu değişkenin değeri, hızlı yüklemeler için $True olmalıdır.

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

Kullanıcının kimlik bilgileri istendiğinde Kullanıcı adı alanını önceden dolduran parametre.

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .

## <a name="start-networkconnectivitydiagnosistools"></a>Start-NetworkConnectivityDiagnosisTools

### <a name="synopsis"></a>ÖZET

Ağ bağlantısı testleri için ana işlev.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Start-NetworkConnectivityDiagnosisTools [[-Forest] <String>] [-Credentials] <PSCredential>
 [[-LogFileLocation] <String>] [[-DCs] <Array>] [-DisplayInformativeMessage] [-ReturnResultAsPSObject]
 [-ValidCredentials] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA

Yerel ağ bağlantısı testlerini çalıştırır.

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

Sınanacak orman adını belirtir.

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

Testi çalıştıran kullanıcının Kullanıcı adı ve parolası.
Azure AD Connect Sihirbazı 'Nı çalıştırmak için gereken izin düzeyini gerektirir.

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

Bu işlevin çıkışını içerecek bir günlük dosyasının konumunu belirtir.

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

#### <a name="-dcs"></a>-DCs

Sınanacak DC 'leri belirtin.

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

#### <a name="-displayinformativemessage"></a>-Display, Tivemessage

Bu işlevin amacı hakkında bir ileti görüntülenmesini sağlayan bayrak.

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

Bu tanılamada bir PSObject biçiminde sonucunu döndürür.
Bu araçla el ile etkileşim sırasında belirtmeniz gerekmez.

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

#### <a name="-validcredentials"></a>-ValidCredentials

Kullanıcının girdiği kimlik bilgilerinin geçerli olup olmadığını gösterir.
Bu araçla el ile etkileşim sırasında belirtmeniz gerekmez.

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .
