---
title: 'Azure AD Connect: ADSyncConfig PowerShell Başvurusu | Microsoft Docs'
description: Bu belge ADSyncConfig. psm1 PowerShell modülü için başvuru bilgileri sağlar.
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.date: 01/24/2019
ms.subservice: hybrid
ms.author: billmath
ms.topic: reference
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2a2126aceba8724b46de094d14db754d704500c6
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85850968"
---
# <a name="azure-ad-connect--adsyncconfig-powershell-reference"></a>Azure AD Connect: ADSyncConfig PowerShell Başvurusu
Aşağıdaki belgeler Azure AD Connect eklenen ADSyncConfig. psm1 PowerShell modülü için başvuru bilgileri sağlar.


## <a name="get-adsyncadconnectoraccount"></a>Get-ADSyncADConnectorAccount

### <a name="synopsis"></a>ÖZET
Her AD bağlayıcısında yapılandırılan hesap adını ve etki alanını alır

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ADSyncADConnectorAccount
```

### <a name="description"></a>AÇIKLAMA
Bu işlev, AD Bağlayıcısı hesabını gösteren bir tablo olan bağlantı parametrelerinden almak için AAD Connect 'te bulunan ' Get-ADSyncConnector ' cmdlet 'ini kullanır.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Get-ADSyncADConnectorAccount
```

## <a name="get-adsyncobjectswithinheritancedisabled"></a>Get-Adsyncobjectswithınheritancedisabled

### <a name="synopsis"></a>ÖZET
İzin devralma ile AD nesnelerini alır devre dışı

### <a name="syntax"></a>SÖZ DİZİMİ

```
Get-ADSyncObjectsWithInheritanceDisabled [-SearchBase] <String> [[-ObjectClass] <String>] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
AD içinde SearchBase parametresinden başlayarak arar ve ACL devralım durumunda olan, ObjectClass parametresine göre filtrelenen tüm nesneleri döndürür.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
' Contoso ' etki alanında devre dışı devralma içeren nesneleri bul (varsayılan olarak yalnızca ' organizationalUnit ' nesnelerini döndürür)
```
Get-ADSyncObjectsWithInheritanceDisabled -SearchBase 'Contoso'
```

#### <a name="example-2"></a>ÖRNEK 2
' Contoso ' etki alanında devralmayı devre dışı olan ' Kullanıcı ' nesnelerini bul
```
Get-ADSyncObjectsWithInheritanceDisabled -SearchBase 'Contoso' -ObjectClass 'user'
```

#### <a name="example-3"></a>ÖRNEK 3
Bir OU 'da devre dışı devralma içeren tüm nesne türlerini bulma
```
Get-ADSyncObjectsWithInheritanceDisabled -SearchBase OU=AzureAD,DC=Contoso,DC=com -ObjectClass '*'
```



### <a name="parameters"></a>PARAMETRELER

#### <a name="-searchbase"></a>-SearchBase
LDAP sorgusunun bir AD etki alanı distinguishedName 'dir veya FQDN olabilecek SearchBase

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
Arama yapılacak nesnelerin sınıfı, ' * ' (herhangi bir nesne sınıfı için), ' user ', ' Group ', ' Container ' vb. olabilir. Varsayılan olarak, bu işlev ' organizationalUnit ' nesne sınıfı için arama yapılır.

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .

## <a name="set-adsyncbasicreadpermissions"></a>Set-ADSyncBasicReadPermissions

### <a name="synopsis"></a>ÖZET
Active Directory ormanınızı ve etki alanınızı temel okuma izinleri için başlatın.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>USERDOMAIN
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName 'dir
```
Set-ADSyncBasicReadPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [-SkipAdminSdHolders]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncBasicReadPermissions Işlevi, AD eşitleme hesabına aşağıdakiler dahil olmak üzere gerekli izinleri verecektir:
1.
Tüm alt bilgisayar nesneleri için tüm özniteliklerde özellik erişimini oku
2.
Tüm alt cihaz nesneleri için tüm özniteliklerde özellik erişimini oku
3.
Tüm alt foreignsecurityprincipal nesneleri için tüm özniteliklerde özellik erişimini oku
5.
Tüm bağımlı Kullanıcı nesneleri için tüm özniteliklerde özellik erişimini oku
6.
Tüm alt inetOrgPerson nesneleri için tüm özniteliklerde özellik erişimini oku
7.
Tüm alt grup nesneleri için tüm özniteliklerde özellik erişimini oku
8.
Tüm alt kişi nesneleri için tüm özniteliklerde özellik erişimini oku

Bu izinler, ormandaki tüm etki alanlarına uygulanır.
İsteğe bağlı olarak, bu izinleri yalnızca bu AD nesnesinde (alt nesnelere devralma dahil) ayarlamak için ADobjectDN parametresinde bir distinguishedName 'dir sağlayabilirsiniz.

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

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Veya dizindeki nesneleri yönetmek için Azure AD Connect Sync tarafından kullanılacak Active Directory hesabının adı.

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
Veya dizindeki nesneleri yönetmek için Azure AD Connect Sync tarafından kullanılan Active Directory hesabının etki alanı.

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
Veya Azure AD Connect eşitleme tarafından dizindeki nesneleri yönetmek için kullanılacak olan Active Directory hesabının distinguishedName 'dir.

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
İzinleri ayarlamak için hedef AD nesnesinin distinguishedName 'dir (isteğe bağlı)

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

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
AdminSDHolder kapsayıcısının bu izinlerle güncelleştirilip güncelleştirilmediğini belirten isteğe bağlı parametre

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .

## <a name="set-adsyncexchangehybridpermissions"></a>Set-ADSyncExchangeHybridPermissions

### <a name="synopsis"></a>ÖZET
Active Directory ormanınızı ve etki alanınızı Exchange karma özelliği için başlatın.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>USERDOMAIN
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName 'dir
```
Set-ADSyncExchangeHybridPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>] [-SkipAdminSdHolders]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncExchangeHybridPermissions Işlevi, AD eşitleme hesabına aşağıdakiler dahil olmak üzere gerekli izinleri verecektir:
1.
Tüm bağımlı Kullanıcı nesneleri için tüm özniteliklerde özellik erişimini oku/yaz
2.
Tüm alt inetOrgPerson nesneleri için tüm özniteliklerde özellik erişimini oku/yaz
3.
Tüm alt grup nesneleri için tüm özniteliklerde özellik erişimini oku/yaz
4.
Tüm alt kişi nesneleri için tüm özniteliklerde özellik erişimini oku/yaz

Bu izinler, ormandaki tüm etki alanlarına uygulanır.
İsteğe bağlı olarak, bu izinleri yalnızca bu AD nesnesinde (alt nesnelere devralma dahil) ayarlamak için ADobjectDN parametresinde bir distinguishedName 'dir sağlayabilirsiniz.

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

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Veya dizindeki nesneleri yönetmek için Azure AD Connect Sync tarafından kullanılacak Active Directory hesabının adı.

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
Veya dizindeki nesneleri yönetmek için Azure AD Connect Sync tarafından kullanılan Active Directory hesabının etki alanı.

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
Veya Azure AD Connect eşitleme tarafından dizindeki nesneleri yönetmek için kullanılacak olan Active Directory hesabının distinguishedName 'dir.

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
İzinleri ayarlamak için hedef AD nesnesinin distinguishedName 'dir (isteğe bağlı)

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

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
AdminSDHolder kapsayıcısının bu izinlerle güncelleştirilip güncelleştirilmediğini belirten isteğe bağlı parametre

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .

## <a name="set-adsyncexchangemailpublicfolderpermissions"></a>Set-ADSyncExchangeMailPublicFolderPermissions

### <a name="synopsis"></a>ÖZET
Exchange mail ortak klasörü özelliği için Active Directory ormanınızı ve etki alanınızı başlatın.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>USERDOMAIN
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountName <String>
 -ADConnectorAccountDomain <String> [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName 'dir
```
Set-ADSyncExchangeMailPublicFolderPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncExchangeMailPublicFolderPermissions Işlevi, AD eşitleme hesabına aşağıdakiler dahil olmak üzere gerekli izinleri verecektir:
1.
Tüm alt public Folder nesneleri için tüm özniteliklerde özellik erişimini oku

Bu izinler, ormandaki tüm etki alanlarına uygulanır.
İsteğe bağlı olarak, bu izinleri yalnızca bu AD nesnesinde (alt nesnelere devralma dahil) ayarlamak için ADobjectDN parametresinde bir distinguishedName 'dir sağlayabilirsiniz.

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

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Veya dizindeki nesneleri yönetmek için Azure AD Connect Sync tarafından kullanılacak Active Directory hesabının adı.

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
Veya dizindeki nesneleri yönetmek için Azure AD Connect Sync tarafından kullanılan Active Directory hesabının etki alanı.

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
Veya Azure AD Connect eşitleme tarafından dizindeki nesneleri yönetmek için kullanılacak olan Active Directory hesabının distinguishedName 'dir.

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
İzinleri ayarlamak için hedef AD nesnesinin distinguishedName 'dir (isteğe bağlı)

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

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
AdminSDHolder kapsayıcısının bu izinlerle güncelleştirilip güncelleştirilmediğini belirten isteğe bağlı parametre

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .

## <a name="set-adsyncmsdsconsistencyguidpermissions"></a>Set-Adsyncmsdsımorguıdizinleri

### <a name="synopsis"></a>ÖZET
Active Directory ormanınızı ve etki alanınızı, mS-DS-ımdsguıd özelliği için başlatın.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>USERDOMAIN
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName 'dir
```
Set-ADSyncMsDsConsistencyGuidPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-Adsyncmsdsımorguidpermissions Işlevi, AD eşitleme hesabına aşağıdakiler dahil olmak üzere gerekli izinleri verecektir:
1.
Tüm alt Kullanıcı nesneleri için mS-DS-Imst GUID özniteliğinde okuma/yazma özelliği erişimi

Bu izinler, ormandaki tüm etki alanlarına uygulanır.
İsteğe bağlı olarak, bu izinleri yalnızca bu AD nesnesinde (alt nesnelere devralma dahil) ayarlamak için ADobjectDN parametresinde bir distinguishedName 'dir sağlayabilirsiniz.

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

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Veya dizindeki nesneleri yönetmek için Azure AD Connect Sync tarafından kullanılacak Active Directory hesabının adı.

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
Veya dizindeki nesneleri yönetmek için Azure AD Connect Sync tarafından kullanılan Active Directory hesabının etki alanı.

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
Veya Azure AD Connect eşitleme tarafından dizindeki nesneleri yönetmek için kullanılacak olan Active Directory hesabının distinguishedName 'dir.

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
İzinleri ayarlamak için hedef AD nesnesinin distinguishedName 'dir (isteğe bağlı)

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

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
AdminSDHolder kapsayıcısının bu izinlerle güncelleştirilip güncelleştirilmediğini belirten isteğe bağlı parametre

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .

## <a name="set-adsyncpasswordhashsyncpermissions"></a>Set-ADSyncPasswordHashSyncPermissions

### <a name="synopsis"></a>ÖZET
Parola karması eşitlemesi için Active Directory ormanınızı ve etki alanınızı başlatın.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>USERDOMAIN
```
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName 'dir
```
Set-ADSyncPasswordHashSyncPermissions -ADConnectorAccountDN <String> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncPasswordHashSyncPermissions Işlevi, AD eşitleme hesabına aşağıdakiler dahil olmak üzere gerekli izinleri verecektir:
1.
Dizin değişikliklerini çoğaltma
2.
Dizin değişikliklerini çoğaltma

Bu izinler, ormandaki tüm etki alanlarına verilir.

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

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Dizindeki nesneleri yönetmek için Azure AD Connect Sync tarafından kullanılacak Active Directory hesabının adı.

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
Dizindeki nesneleri yönetmek için Azure AD Connect eşitleme tarafından kullanılacak Active Directory hesabının etki alanı.

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
Dizindeki nesneleri yönetmek için Azure AD Connect Sync tarafından kullanılacak Active Directory hesabının distinguishedName 'dir.

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .

## <a name="set-adsyncpasswordwritebackpermissions"></a>Set-ADSyncPasswordWritebackPermissions

### <a name="synopsis"></a>ÖZET
Active Directory ormanınızı ve etki alanınızı Azure AD 'den parola geri yazma için başlatın.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>USERDOMAIN
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName 'dir
```
Set-ADSyncPasswordWritebackPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncPasswordWritebackPermissions Işlevi, AD eşitleme hesabına aşağıdakiler dahil olmak üzere gerekli izinleri verecektir:
1.
Alt kullanıcı nesnelerinde Parolayı Sıfırla
2.
Tüm alt Kullanıcı nesneleri için lockoutTime özniteliğinde Özellik erişimi yazma
3.
Tüm alt Kullanıcı nesneleri için pwdLastSet özniteliğinde Özellik erişimi yazma

Bu izinler, ormandaki tüm etki alanlarına uygulanır.
İsteğe bağlı olarak, bu izinleri yalnızca bu AD nesnesinde (alt nesnelere devralma dahil) ayarlamak için ADobjectDN parametresinde bir distinguishedName 'dir sağlayabilirsiniz.

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

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Veya dizindeki nesneleri yönetmek için Azure AD Connect Sync tarafından kullanılacak Active Directory hesabının adı.

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
Veya dizindeki nesneleri yönetmek için Azure AD Connect Sync tarafından kullanılan Active Directory hesabının etki alanı.

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
Veya Azure AD Connect eşitleme tarafından dizindeki nesneleri yönetmek için kullanılacak olan Active Directory hesabının distinguishedName 'dir.

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
İzinleri ayarlamak için hedef AD nesnesinin distinguishedName 'dir (isteğe bağlı)

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

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
AdminSDHolder kapsayıcısının bu izinlerle güncelleştirilip güncelleştirilmediğini belirten isteğe bağlı parametre

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .

## <a name="set-adsyncrestrictedpermissions"></a>Set-ADSyncRestrictedPermissions

### <a name="synopsis"></a>ÖZET
Herhangi bir AD korumalı güvenlik grubuna başka türlü dahil olmayan bir AD nesnesi üzerindeki izinleri güçlendir.
Tipik bir örnek, AAD Connect tarafından otomatik olarak oluşturulan AD Connect hesabıdır (MSOL).
Bu hesap, tüm etki alanları üzerinde çoğaltma izinlerine sahiptir, ancak korunmadığı için kolayca tehlikeye girebilir.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Set-ADSyncRestrictedPermissions [-ADConnectorAccountDN] <String> [-Credential] <PSCredential>
 [-DisableCredentialValidation] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncRestrictedPermissions Işlevi, belirtilen hesap için izinleri sıkılacaktır.
Sıkı bir şekilde izinleri aşağıdaki adımları içerir:
1. Belirtilen nesnede devralmayı devre dışı bırak
2. KENDISINE özgü Ace 'Ler hariç belirli bir nesnedeki tüm Ace 'Leri kaldırın.
Kendi kendine geldiğinde varsayılan izinleri değiştirmeden tutmak istiyoruz.
3. Bu özel izinleri ata:

   | Tür | Name | Access | Uygulanan Öğe |
   |------|------|--------|------------|
   | İzin Ver | SİSTEM | Tam Denetim | Bu nesne |
   | İzin Ver | Enterprise Admins | Tam Denetim | Bu nesne |
   | İzin Ver | Etki Alanı Yöneticileri | Tam Denetim | Bu nesne | 
   | İzin Ver | Yöneticiler | Tam Denetim | Bu nesne |
   | İzin Ver | Kurumsal etki alanı denetleyicileri | Liste Içerikleri <br> Tüm Özellikleri Oku <br> Okuma Izinleri | Bu nesne |
   | İzin Ver | Kimliği Doğrulanmış Kullanıcılar | Liste Içerikleri <br> Tüm Özellikleri Oku <br> Okuma Izinleri | Bu nesne |

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Set-ADSyncRestrictedPermissions -ADConnectorAccountDN "CN=TestAccount1,CN=Users,DC=Contoso,DC=com" -Credential $(Get-Credential)
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adconnectoraccountdn"></a>-ADConnectorAccountDN
İzinlerinin daha sıkı bir şekilde getirilmesi gereken Active Directory hesabı distinguishedName 'dir.
Bu, genellikle MSOL_nnnnnnnnnn hesabıdır veya AD bağlayıcınıza yapılandırılmış özel bir etki alanı hesabıdır.

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
ADConnectorAccountDN hesabında izinleri kısıtlamak için gereken ayrıcalıklara sahip yönetici kimlik bilgileri. Bu genellikle kuruluş veya etki alanı yöneticisidir. Hesap arama hatalarından kaçınmak için yönetici hesabının tam etki alanı adını kullanın.
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

#### <a name="-disablecredentialvalidation"></a>-DisableCredentialValidation doğrulaması
DisableCredentialValidation kullanıldığında, kimlik bilgisi içinde belirtilen kimlik bilgilerinin AD 'de geçerli olup olmadığını ve sağlanmış hesabın ADConnectorAccountDN hesabında izinleri kısıtlamak için gerekli ayrıcalıklara sahip olup olmadığını kontrol etmez.

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .

## <a name="set-adsyncunifiedgroupwritebackpermissions"></a>Set-ADSyncUnifiedGroupWritebackPermissions

### <a name="synopsis"></a>ÖZET
Active Directory ormanınızı ve etki alanınızı Azure AD 'den grup geri yazma için başlatın.

### <a name="syntax"></a>SÖZ DİZİMİ

#### <a name="userdomain"></a>USERDOMAIN
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountName <String> -ADConnectorAccountDomain <String>
 [-ADobjectDN <String>] [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

#### <a name="distinguishedname"></a>DistinguishedName 'dir
```
Set-ADSyncUnifiedGroupWritebackPermissions -ADConnectorAccountDN <String> [-ADobjectDN <String>]
 [-SkipAdminSdHolders] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Set-ADSyncUnifiedGroupWritebackPermissions Işlevi, AD eşitleme hesabına aşağıdakiler dahil olmak üzere gerekli izinleri verecektir:
1.
Tüm grup nesne türleri ve alt nesneler için genel okuma/yazma, silme, silme ağacı ve Create\Delete alt

Bu izinler, ormandaki tüm etki alanlarına uygulanır.
İsteğe bağlı olarak, bu izinleri yalnızca bu AD nesnesinde (alt nesnelere devralma dahil) ayarlamak için ADobjectDN parametresinde bir distinguishedName 'dir sağlayabilirsiniz.
Bu durumda, ADobjectDN, Groupgeri yazma özelliğiyle bağlamak istediğiniz kapsayıcının ayırt edici adı olacaktır.

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

#### <a name="-adconnectoraccountname"></a>-ADConnectorAccountName
Veya dizindeki nesneleri yönetmek için Azure AD Connect Sync tarafından kullanılacak Active Directory hesabının adı.

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
Veya dizindeki nesneleri yönetmek için Azure AD Connect Sync tarafından kullanılan Active Directory hesabının etki alanı.

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
Veya Azure AD Connect eşitleme tarafından dizindeki nesneleri yönetmek için kullanılacak olan Active Directory hesabının distinguishedName 'dir.

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
İzinleri ayarlamak için hedef AD nesnesinin distinguishedName 'dir (isteğe bağlı)

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

#### <a name="-skipadminsdholders"></a>-SkipAdminSdHolders
AdminSDHolder kapsayıcısının bu izinlerle güncelleştirilip güncelleştirilmediğini belirten isteğe bağlı parametre

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .

## <a name="show-adsyncadobjectpermissions"></a>Show-ADSyncADObjectPermissions

### <a name="synopsis"></a>ÖZET
Belirtilen bir AD nesnesinin izinlerini gösterir.

### <a name="syntax"></a>SÖZ DİZİMİ

```
Show-ADSyncADObjectPermissions [-ADobjectDN] <String> [<CommonParameters>]
```

### <a name="description"></a>AÇIKLAMA
Bu işlev, ADobjectDN parametresinde sağlanan belirli bir AD nesnesi için şu anda ayarlanmış olan tüm AD izinlerini döndürür.
ADobjectDN, bir distinguishedName 'dir biçiminde sağlanmalıdır.

### <a name="examples"></a>ÖRNEKLER

#### <a name="example-1"></a>ÖRNEK 1
```
Show-ADSyncADObjectPermissions -ADobjectDN 'OU=AzureAD,DC=Contoso,DC=com'
```

### <a name="parameters"></a>PARAMETRELER

#### <a name="-adobjectdn"></a>-ADobjectDN
{{Fill ADobjectDN açıklaması}}

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
Daha fazla bilgi için bkz. about_CommonParameters ( https://go.microsoft.com/fwlink/?LinkID=113216) .
