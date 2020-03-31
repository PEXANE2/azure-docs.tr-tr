---
title: 'Azure AD Connect: ADSyncConfig PowerShell Başvuru | Microsoft Dokümanlar'
description: Bu belge, ADSyncConfig.psm1 PowerShell modülü için referans bilgileri sağlar.
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.date: 01/24/2019
ms.subservice: hybrid
ms.author: billmath
ms.topic: reference
ms.collection: M365-identity-device-management
ms.openlocfilehash: 554bb99121190198982f64deb6ee0674aa8831ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60381204"
---
# <a name="azure-ad-connect--adsyncconfig-powershell-reference"></a>Azure AD Connect: ADSyncConfig PowerShell Başvurusu
Aşağıdaki belgeler, Azure AD Connect ile birlikte verilen ADSyncConfig.psm1 PowerShell Modülü için referans bilgileri sağlar.


## <a name="get-adsyncadconnectoraccount"></a>Get-ADSyncADConnectorAccount

### <a name="synopsis"></a>ÖZET
Her AD Bağlayıcısı'nda yapılandırılan hesap adını ve etki alanını alır

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ADSyncADConnectorAccount
```

### <a name="description"></a>AÇIKLAMA
Bu işlev, Bağlantı Parametreleri'nden AD Bağlayıcısı(lar) hesabını gösteren bir tablo almak için AAD Connect'te bulunan 'Get-ADSyncConnector' cmdlet'i kullanır.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Get-ADSyncADConnectorAccount
```

## <a name="get-adsyncobjectswithinheritancedisabled"></a>Al-ADSyncObjectsWithInheritanceDisabled

### <a name="synopsis"></a>ÖZET
İzin devralma devre dışı bırakılmış AD nesnelerini alır

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ADSyncObjectsWithInheritanceDisabled [-SearchBase] <String> [[-ObjectClass] <String>] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
SearchBase parametresinden başlayarak AD'de aramalar ve ObjectClass parametresi tarafından filtre uygulanmış, şu anda devre dışı bırakılan ACL Kalıtım'ı olan tüm nesneleri döndürür.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Find objects with disabled inheritance in 'Contoso' domain (by default returns 'organizationalUnit' objects only)
```

Get-ADSyncObjectsWithInheritanceDisabled -SearchBase 'Contoso'

#### <a name="example-2"></a>ÖRNEK 2
```
Find 'user' objects with disabled inheritance in 'Contoso' domain
```

Get-ADSyncObjectsWithInheritanceDisabled -SearchBase 'Contoso' -ObjectClass 'kullanıcı'

#### <a name="example-3"></a>ÖRNEK 3
```
Find all types of objects with disabled inheritance in a OU
```

Get-ADSyncObjectsWithInheritanceDisabled -SearchBase OU=AzureAD,DC=Contoso,DC=com -ObjectClass '*'

### <a name="parameters"></a>PARAMETRELER

#### <a name="-searchbase"></a>-Arama Üssü
AD Etki Alanı DistinguishedName veya FQDN olabilecek LDAP sorgusu için SearchBase

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

#### <a name="-objectclass"></a>-ObjectClass
'*' (herhangi bir nesne sınıfı için), 'kullanıcı', 'grup', 'kapsayıcı' vb. olabilecek arama nesnelerinin sınıfı Varsayılan olarak, bu işlev 'organizationalUnit' nesne sınıfara.

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: OrganizationalUnit
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi içinhttps://go.microsoft.com/fwlink/?LinkID=113216)about_CommonParameters ( .

## <a name="set-adsyncbasicreadpermissions"></a>Set-ADSyncBasicReadPermissions

### <a name="synopsis"></a>ÖZET
Temel okuma izinleri için Active Directory ormanınızı ve etki alanınızı başlangıç olarak edinin.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>Seçkin Ad
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [-SkipAdminSdHolders]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncBasicReadPermissions Fonksiyonu, aşağıdakileri içeren AD eşitleme hesabına gerekli izinleri verir:
1.
Tüm soyundan gelen bilgisayar nesneleri için tüm özniteliklerde Özellik erişimi okuma
2.
Tüm soyundan gelen aygıt nesneleri için tüm özniteliklerde Özellik erişimini okuma
3.
Tüm soyundan gelen yabancı güvenlik nesneleri için tüm özniteliklerde Özellik erişimi okuyun
5.
Tüm soyundan gelen kullanıcı nesneleri için tüm özniteliklerde Özellik erişimini okuma
6.
Tüm soyundan gelen inetorgperson nesneleri için tüm özniteliklerde Özellik erişimini okuyun
7.
Tüm soyundan gelen grup nesneleri için tüm özniteliklerde Özellik erişimini okuma
8.
Tüm soyundan gelen kişi nesneleri için tüm özniteliklerde Özellik erişimini okuma

Bu izinler ormandaki tüm etki alanlarında uygulanır.
İsteğe bağlı olarak, bu izinleri yalnızca bu AD Nesnesi üzerinde ayarlamak için (alt nesnelere devralma dahil) ADobjectDN parametresinde bir DistinguishedName sağlayabilirsiniz.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>ÖRNEK 2
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>ÖRNEK 3
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>ÖRNEK 4
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adconnectoraccountname"></a>-ADConnectorHesap Adı
Dizindeki nesneleri yönetmek için Azure AD Connect Eşitle'si tarafından kullanılan veya kullanılacak Olan Etkin Dizin hesabının Adı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Dizindeki nesneleri yönetmek için Azure AD Connect Sync Eşitleme'de olan veya bu hesap tarafından kullanılacak Olan Etkin Dizin hesabının Etki Alanı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
Dizindeki nesneleri yönetmek için Azure AD Connect Sync Eşitle'si olan veya bu hesap tarafından kullanılacak Olan Etkin Dizin hesabının Seçkin Adı.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
İzinleri ayarlamak için hedef AD nesnesinin Ayırt Edici Adı (isteğe bağlı)

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdSahipleri
AdminSDHolder kapsayıcının bu izinlerle güncelleştirilmemesi gerekip gerekmediğini belirtmek için isteğe bağlı parametre

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

## <a name="set-adsyncexchangehybridpermissions"></a>Set-ADSyncExchangeHybridPermissions

### <a name="synopsis"></a>ÖZET
Exchange Hybrid özelliği için Active Directory ormanınızı ve etki alanınızı başlangıç olarak ele alanın.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>Seçkin Ad
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [-SkipAdminSdHolders]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncExchangeHybridPermissions Fonksiyonu, aşağıdakileri içeren AD eşitleme hesabına gerekli izinleri verecektir:
1.
Tüm soyundan gelen kullanıcı nesneleri için tüm özniteliklere özellik erişimini okuma/yazma
2.
Tüm soyundan gelen inetorgperson nesneleri için tüm özniteliklere özellik erişimini okuma/yazma
3.
Tüm soyundan gelen grup nesneleri için tüm özniteliklere özellik erişimini okuma/yazma
4.
Tüm soyundan gelen kişi nesneleri için tüm özniteliklere özellik erişimini okuma/yazma

Bu izinler ormandaki tüm etki alanlarında uygulanır.
İsteğe bağlı olarak, bu izinleri yalnızca bu AD Nesnesi üzerinde ayarlamak için (alt nesnelere devralma dahil) ADobjectDN parametresinde bir DistinguishedName sağlayabilirsiniz.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>ÖRNEK 2
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>ÖRNEK 3
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>ÖRNEK 4
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adconnectoraccountname"></a>-ADConnectorHesap Adı
Dizindeki nesneleri yönetmek için Azure AD Connect Eşitle'si tarafından kullanılan veya kullanılacak Olan Etkin Dizin hesabının Adı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Dizindeki nesneleri yönetmek için Azure AD Connect Sync Eşitleme'de olan veya bu hesap tarafından kullanılacak Olan Etkin Dizin hesabının Etki Alanı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
Dizindeki nesneleri yönetmek için Azure AD Connect Sync Eşitle'si olan veya bu hesap tarafından kullanılacak Olan Etkin Dizin hesabının Seçkin Adı.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
İzinleri ayarlamak için hedef AD nesnesinin Ayırt Edici Adı (isteğe bağlı)

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdSahipleri
AdminSDHolder kapsayıcının bu izinlerle güncelleştirilmemesi gerekip gerekmediğini belirtmek için isteğe bağlı parametre

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

## <a name="set-adsyncexchangemailpublicfolderpermissions"></a>Set-ADSyncExchangeMailPublicFolderPermissions

### <a name="synopsis"></a>ÖZET
Exchange Mail Public Folder özelliği için Active Directory ormanınızı ve etki alanınızı kullanıma açın.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountName <String>
 -ADConnectorAccountDomain <String> [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

#### <a name="distinguishedname"></a>Seçkin Ad
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncExchangeMailPublicFolderPermissions Fonksiyonu, aşağıdakileri içeren AD eşitleme hesabına gerekli izinleri verecektir:
1.
Tüm soyundan gelen publicfolder nesnelerinin tüm özniteliklerinde Özellik erişimini okuma

Bu izinler ormandaki tüm etki alanlarında uygulanır.
İsteğe bağlı olarak, bu izinleri yalnızca bu AD Nesnesi üzerinde ayarlamak için (alt nesnelere devralma dahil) ADobjectDN parametresinde bir DistinguishedName sağlayabilirsiniz.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>ÖRNEK 2
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>ÖRNEK 3
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>ÖRNEK 4
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adconnectoraccountname"></a>-ADConnectorHesap Adı
Dizindeki nesneleri yönetmek için Azure AD Connect Eşitle'si tarafından kullanılan veya kullanılacak Olan Etkin Dizin hesabının Adı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Dizindeki nesneleri yönetmek için Azure AD Connect Sync Eşitleme'de olan veya bu hesap tarafından kullanılacak Olan Etkin Dizin hesabının Etki Alanı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
Dizindeki nesneleri yönetmek için Azure AD Connect Sync Eşitle'si olan veya bu hesap tarafından kullanılacak Olan Etkin Dizin hesabının Seçkin Adı.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
İzinleri ayarlamak için hedef AD nesnesinin Ayırt Edici Adı (isteğe bağlı)

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdSahipleri
AdminSDHolder kapsayıcının bu izinlerle güncelleştirilmemesi gerekip gerekmediğini belirtmek için isteğe bağlı parametre

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

## <a name="set-adsyncmsdsconsistencyguidpermissions"></a>Set-ADSyncMsDsConsistencyGuidPermissions

### <a name="synopsis"></a>ÖZET
MS-DS-ConsistencyGuid özelliği için Active Directory ormanınızı ve etki alanınızı başlangıç olarak adlandırın.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>Seçkin Ad
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncMsDsConsistencyGuidPermissions İşlevi, aşağıdakileri içeren AD eşitleme hesabına gerekli izinleri verir:
1.
Tüm soyundan gelen kullanıcı nesneleri için mS-DS-ConsistencyGuid özniteliği üzerinde Özellik erişimini okuma/Yazma

Bu izinler ormandaki tüm etki alanlarında uygulanır.
İsteğe bağlı olarak, bu izinleri yalnızca bu AD Nesnesi üzerinde ayarlamak için (alt nesnelere devralma dahil) ADobjectDN parametresinde bir DistinguishedName sağlayabilirsiniz.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>ÖRNEK 2
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>ÖRNEK 3
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>ÖRNEK 4
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adconnectoraccountname"></a>-ADConnectorHesap Adı
Dizindeki nesneleri yönetmek için Azure AD Connect Eşitle'si tarafından kullanılan veya kullanılacak Olan Etkin Dizin hesabının Adı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Dizindeki nesneleri yönetmek için Azure AD Connect Sync Eşitleme'de olan veya bu hesap tarafından kullanılacak Olan Etkin Dizin hesabının Etki Alanı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
Dizindeki nesneleri yönetmek için Azure AD Connect Sync Eşitle'si olan veya bu hesap tarafından kullanılacak Olan Etkin Dizin hesabının Seçkin Adı.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
İzinleri ayarlamak için hedef AD nesnesinin Ayırt Edici Adı (isteğe bağlı)

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdSahipleri
AdminSDHolder kapsayıcının bu izinlerle güncelleştirilmemesi gerekip gerekmediğini belirtmek için isteğe bağlı parametre

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

## <a name="set-adsyncpasswordhashsyncpermissions"></a>Set-ADSyncPasswordHashSyncPermissions

### <a name="synopsis"></a>ÖZET
Parola karma eşitleme için Active Directory ormanınızı ve etki alanınızı başlatma.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>Seçkin Ad
```
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountDN <String> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncPasswordHashSyncPermissions Fonksiyonu, aşağıdakileri içeren AD eşitleme hesabına gerekli izinleri verir:
1.
Dizin Değişikliklerini Çoğaltma
2.
Çoğaltma Dizin Değişiklikleri Tümü

Bu izinler ormandaki tüm etki alanları için verilir.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>ÖRNEK 2
```
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adconnectoraccountname"></a>-ADConnectorHesap Adı
Dizindeki nesneleri yönetmek için Azure AD Connect Sync tarafından kullanılacak Olan Etkin Dizin hesabının adı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Dizindeki nesneleri yönetmek için Azure AD Connect Sync tarafından kullanılacak Olan Etkin Dizin hesabının Etki Alanı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
Dizindeki nesneleri yönetmek için Azure AD Connect Sync tarafından kullanılacak Olan Etkin Dizin hesabının Seçkin Adı.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
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

## <a name="set-adsyncpasswordwritebackpermissions"></a>Set-ADSyncPasswordWritebackPermissions

### <a name="synopsis"></a>ÖZET
Azure AD'den parola geri yazmak için Active Directory ormanınızı ve etki alanınızı başlangıç olarak alın.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>Seçkin Ad
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncPasswordWritebackPermissions İşlevi, aşağıdakileri içeren AD eşitleme hesabına gerekli izinleri verir:
1.
Soyundan gelen kullanıcı nesnelerinde Parolayı Sıfırlama
2.
Tüm soyundan gelen kullanıcı nesneleri için kilitlemeZaman özniteliği üzerinde Özellik erişimi yazın
3.
Tüm soyundan gelen kullanıcı nesneleri için pwdLastSet özniteliği özellik erişimi yazın

Bu izinler ormandaki tüm etki alanlarında uygulanır.
İsteğe bağlı olarak, bu izinleri yalnızca bu AD Nesnesi üzerinde ayarlamak için (alt nesnelere devralma dahil) ADobjectDN parametresinde bir DistinguishedName sağlayabilirsiniz.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>ÖRNEK 2
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>ÖRNEK 3
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>ÖRNEK 4
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adconnectoraccountname"></a>-ADConnectorHesap Adı
Dizindeki nesneleri yönetmek için Azure AD Connect Eşitle'si tarafından kullanılan veya kullanılacak Olan Etkin Dizin hesabının Adı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Dizindeki nesneleri yönetmek için Azure AD Connect Sync Eşitleme'de olan veya bu hesap tarafından kullanılacak Olan Etkin Dizin hesabının Etki Alanı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
Dizindeki nesneleri yönetmek için Azure AD Connect Sync Eşitle'si olan veya bu hesap tarafından kullanılacak Olan Etkin Dizin hesabının Seçkin Adı.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
İzinleri ayarlamak için hedef AD nesnesinin Ayırt Edici Adı (isteğe bağlı)

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdSahipleri
AdminSDHolder kapsayıcının bu izinlerle güncelleştirilmemesi gerekip gerekmediğini belirtmek için isteğe bağlı parametre

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

## <a name="set-adsyncrestrictedpermissions"></a>Set-ADSyncRestrictedPermissions

### <a name="synopsis"></a>ÖZET
Ad korumalı güvenlik grubuna başka şekilde dahil olmayan bir AD nesnesi üzerindeki izinleri sıkılaştırın.
Tipik bir örnek, AAD Connect tarafından otomatik olarak oluşturulan AD Connect hesabıdır (MSOL).
Bu hesap, tüm etki alanlarında izinleri çoğaltmavardır, ancak korumalı olmadığı için kolayca tehlikeye atılabilir.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Set-ADSyncRestrictedPermissions [-ADConnectorAccountDN] <String> [-Credential] <PSCredential>
 [-DisableCredentialValidation] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncRestrictedPermissions İşlevi, sağlanan hesapoo izinleri sıkılaştırır.
Sıkılaştırma izinleri aşağıdaki adımları içerir:
1.
Belirtilen nesnede devralmayı devre dışı
2.
SELF'e özgü ACE'ler dışında, belirli bir nesnedeki tüm ACE'leri kaldırın.
SELF söz konusu olduğunda varsayılan izinleri sağlam tutmak istiyoruz.
3.
Bu özel izinleri atama:

        Type    Name                                        Access              Applies To
        =============================================================================================
        Allow   SYSTEM                                      Full Control        This object
        Allow   Enterprise Admins                           Full Control        This object
        Allow   Domain Admins                               Full Control        This object
        Allow   Administrators                              Full Control        This object

        Allow   Enterprise Domain Controllers               List Contents
                                                            Read All Properties
                                                            Read Permissions    This object

        Allow   Authenticated Users                         List Contents
                                                            Read All Properties
                                                            Read Permissions    This object

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Set-ADSyncRestrictedPermissions -ADConnectorAccountDN "CN=TestAccount1,CN=Users,DC=Contoso,DC=com" -Credential $(Get-Credential)
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
İzinleri sıkılaştırılması gereken Active Directory hesabının Seçkin Adı.
Bu genellikle MSOL_nnnnnnnnnn hesabı veya AD Bağlayıcısı'nda yapılandırılan özel bir etki alanı hesabıdır.

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

#### <a name="-credential"></a>-Credential
ADConnectorAccountDN hesabındaki izinleri kısıtlamak için gerekli ayrıcalıklara sahip yönetici kimlik bilgisi. Bu genellikle Enterprise veya Domain yöneticisidir. Hesap arama hatalarını önlemek için yönetici hesabının tam nitelikli alan adını kullanın.
Örnek: CONTOSO\admin

```yaml
Type: PSCredential
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-disablecredentialvalidation"></a>-Devre Dışı BırakmaCredentialValidation
DisableCredentialValidation kullanıldığında, işlev -Kimlik Bilgileri'nde sağlanan kimlik bilgilerinin AD'de geçerli olup olmadığını ve sağlanan hesabın ADConnectorAccountDN hesabındaki izinleri kısıtlamak için gerekli ayrıcalıklara sahip olup olmadığını denetlemez.

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

## <a name="set-adsyncunifiedgroupwritebackpermissions"></a>Set-ADSyncUnifiedGroupWritebackPermissions

### <a name="synopsis"></a>ÖZET
Azure AD'den Grup geri yazma için Active Directory ormanınızı ve etki alanınızı başlangıç olarak yazın.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>UserDomain
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>Seçkin Ad
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncUnifiedGroupWritebackPermissions İşlevi, aşağıdakileri içeren AD eşitleme hesabına gerekli izinleri verir:
1.
Tüm grup Nesne türleri ve Alt Nesneler için Genel Okuma/Yazma, Silme, Ağacı Sil ve Oluştur\Delete Alt

Bu izinler ormandaki tüm etki alanlarında uygulanır.
İsteğe bağlı olarak, bu izinleri yalnızca bu AD Nesnesi üzerinde ayarlamak için (alt nesnelere devralma dahil) ADobjectDN parametresinde bir DistinguishedName sağlayabilirsiniz.
Bu durumda, ADobjectDN GroupWriteback özelliği ile bağlamak istediğiniz Kapsayıcının Ayırt Edici Adı olacaktır.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com'
```

#### <a name="example-2"></a>ÖRNEK 2
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com'
```

#### <a name="example-3"></a>ÖRNEK 3
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN 'CN=ADConnector,OU=AzureAD,DC=Contoso,DC=com' -SkipAdminSdHolders
```

#### <a name="example-4"></a>ÖRNEK 4
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountName 'ADConnector' -ADConnectorAccountDomain 'Contoso.com' -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adconnectoraccountname"></a>-ADConnectorHesap Adı
Dizindeki nesneleri yönetmek için Azure AD Connect Eşitle'si tarafından kullanılan veya kullanılacak Olan Etkin Dizin hesabının Adı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdomain"></a>-ADConnectorAccountDomain
Dizindeki nesneleri yönetmek için Azure AD Connect Sync Eşitleme'de olan veya bu hesap tarafından kullanılacak Olan Etkin Dizin hesabının Etki Alanı.

```yaml
Type: String
Parameter Sets: UserDomain
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
Dizindeki nesneleri yönetmek için Azure AD Connect Sync Eşitle'si olan veya bu hesap tarafından kullanılacak Olan Etkin Dizin hesabının Seçkin Adı.

```yaml
Type: String
Parameter Sets: DistinguishedName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-adobjectdn"></a>-ADobjectDN
İzinleri ayarlamak için hedef AD nesnesinin Ayırt Edici Adı (isteğe bağlı)

```yaml
Type: String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="-skipadminsdholders"></a>-SkipAdminSdSahipleri
AdminSDHolder kapsayıcının bu izinlerle güncelleştirilmemesi gerekip gerekmediğini belirtmek için isteğe bağlı parametre

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

## <a name="show-adsyncadobjectpermissions"></a>Göster-ADSyncADObjectPermissions

### <a name="synopsis"></a>ÖZET
Belirtilen bir AD nesnesinin izinlerini gösterir.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Show-ADSyncADObjectPermissions [-ADobjectDN] <String> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Bu işlev, parametre -ADobjectDN sağlanan belirli bir AD nesnesi için şu anda ayarlanan tüm AD izinleri döndürür.
ADobjectDN Bir DistinguishedName biçiminde sağlanmalıdır.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Show-ADSyncADObjectPermissions -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adobjectdn"></a>-ADobjectDN
{{DDobjectDN Açıklamasını Doldurun}}

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

#### <a name="commonparameters"></a>CommonParameters
Bu cmdlet şu genel parametreleri destekler: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction ve -WarningVariable.
Daha fazla bilgi içinhttps://go.microsoft.com/fwlink/?LinkID=113216)about_CommonParameters ( .
