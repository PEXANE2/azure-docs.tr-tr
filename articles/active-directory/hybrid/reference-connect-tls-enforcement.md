---
title: 'Azure AD Connect: Azure Active Directory Connect için TLS 1,2 zorlaması | Microsoft Docs'
description: Bu makalede Azure AD Connect ve Azure AD Eşitleme tüm sürümleri listelenir
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/28/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9ff5c75785622b43e66b808009c4674d4b2f2b50
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78300857"
---
# <a name="tls-12-enforcement-for-azure-ad-connect"></a>Azure AD Connect için TLS 1,2 zorlaması

Aktarım Katmanı Güvenliği (TLS) Protokolü sürüm 1,2, güvenli iletişim sağlamak için tasarlanan bir şifreleme protokolüdür.  TLS protokolü birincil olarak gizlilik ve veri bütünlüğü sağlamaya yöneliktir.  TLS, 1,2 sürümüne sahip çok sayıda yinelemeden, [RFC 5246](https://tools.ietf.org/html/rfc5246)' de tanımlı.  Azure Active Directory Connect sürüm 1.2.65.0 ve üzeri, Azure ile iletişim kurmak için yalnızca TLS 1,2 kullanarak tam destek.  Bu belge Azure AD Connect sunucunuzu yalnızca TLS 1,2 ' i kullanacak şekilde zorlama hakkında bilgi sağlayacaktır.

## <a name="update-the-registry"></a>Kayıt defterini güncelleştirme
Azure AD Connect sunucusunu yalnızca TLS 1,2 kullanacak şekilde zorlamak için Windows Server kayıt defteri 'nin güncellenmesi gerekir.  Azure AD Connect sunucuda aşağıdaki kayıt defteri anahtarlarını ayarlayın.

>[!IMPORTANT]
>Kayıt defterini güncelleştirdikten sonra, değişikliklerin etkili olması için Windows Server 'ı yeniden başlatmanız gerekir.


### <a name="enable-tls-12"></a>TLS 1.2’yi etkinleştirme
- [HKEY_LOCAL_MACHINE \SOFTWARE\WOW6432Node\Microsoft \\ . NETFramework\v4.0.30319]
  - "SystemDefaultTlsVersions" = DWORD: 00000001
  - "Schusestrongşifre" = DWORD: 0000001
- [HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft \\ . NETFramework\v4.0.30319]
  - "SystemDefaultTlsVersions" = DWORD: 00000001
  - "Schusestrongşifre" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server]
  - "Etkin" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server]
  - "DisabledByDefault" = DWORD: 00000000 
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Client]
  - "Etkin" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Client]
  - "DisabledByDefault" = DWORD: 00000000

### <a name="powershell-script-to-enable-tls-12"></a>TLS 1,2 ' i etkinleştirmek için PowerShell betiği
Azure AD Connect sunucunuzda TLS 1,2 ' i etkinleştirmek için aşağıdaki PowerShell betiğini kullanabilirsiniz.

```powershell
    New-Item 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -name 'SystemDefaultTlsVersions' -value '1' -PropertyType 'DWord' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null

    New-Item 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -name 'SystemDefaultTlsVersions' -value '1' -PropertyType 'DWord' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null

    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value '1' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 0 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value '1' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 0 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.2 has been enabled.'
```

### <a name="disable-tls-12"></a>TLS 1,2 'yi devre dışı bırakma
- [HKEY_LOCAL_MACHINE \SOFTWARE\WOW6432Node\Microsoft \\ . NETFramework\v4.0.30319]
  - "SystemDefaultTlsVersions" = DWORD: 00000000
  - "Schusestrongşifre" = DWORD: 0000000
- [HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft \\ . NETFramework\v4.0.30319]
  - "SystemDefaultTlsVersions" = DWORD: 00000000
  - "Schusestrongşifre" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server]
  - "Etkin" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Server]
  - "DisabledByDefault" = DWORD: 00000001
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Client]
  - "Etkin" = DWORD: 00000000
- [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2 \ Client]
  - "DisabledByDefault" = DWORD: 00000001 

### <a name="powershell-script-to-disable-tls-12"></a>TLS 1,2 'yi devre dışı bırakmak için PowerShell betiği
Azure AD Connect sunucunuzda TLS 1,2 'yi devre dışı bırakmak için aşağıdaki PowerShell betiğini kullanabilirsiniz. \

```powershell
    New-Item 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -name 'SystemDefaultTlsVersions' -value '0' -PropertyType 'DWord' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '0' -PropertyType 'DWord' -Force | Out-Null

    New-Item 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -name 'SystemDefaultTlsVersions' -value '0' -PropertyType 'DWord' -Force | Out-Null

    New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '0' -PropertyType 'DWord' -Force | Out-Null

    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    
    New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value '0' -PropertyType 'DWord' -Force | Out-Null
    
    New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 1 -PropertyType 'DWord' -Force | Out-Null
    Write-Host 'TLS 1.2 has been disabled.'
```

## <a name="next-steps"></a>Sonraki adımlar
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
