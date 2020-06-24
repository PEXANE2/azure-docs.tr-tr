---
title: Sessiz install Azure AD Uygulaması Proxy Bağlayıcısı | Microsoft Docs
description: Şirket içi uygulamalarınıza güvenli uzaktan erişim sağlamak için Azure AD Uygulama Ara Sunucusu bağlayıcısının katılımsız yüklemesinin nasıl gerçekleştirileceğini ele alır.
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 01/24/2020
ms.author: kenwith
ms.reviewer: japere
ms.custom: it-pro, has-adal-ref
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0b959649074e68d50a266f05841ce4c87e2b3e20
ms.sourcegitcommit: bc943dc048d9ab98caf4706b022eb5c6421ec459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2020
ms.locfileid: "84760023"
---
# <a name="create-an-unattended-installation-script-for-the-azure-ad-application-proxy-connector"></a>Azure AD Uygulama Ara Sunucusu Bağlayıcısı için katılımsız yükleme betiği oluşturma

Bu konu, Azure AD Uygulama Ara Sunucusu Bağlayıcınız için katılımsız yükleme ve kayıt sağlayan bir Windows PowerShell betiği oluşturmanıza yardımcı olur.

Bu özellik şunları yapmak istediğinizde yararlıdır:

* Bağlayıcıyı, Kullanıcı arabirimi etkinleştirilmemiş olan veya uzak masaüstü ile erişemeyen Windows sunucularına yükleyebilirsiniz.
* Aynı anda birçok bağlayıcıyı yükleyip kaydettirin.
* Bağlayıcı yüklemesini ve kaydını başka bir yordamın parçası olarak tümleştirin.
* Bağlayıcı bitlerini içeren, ancak kayıtlı olmayan standart bir sunucu görüntüsü oluşturun.

[Uygulama proxy bağlayıcısının](application-proxy-connectors.md) çalışması için, uygulama Yöneticisi ve parolası KULLANıLARAK Azure AD dizininizle birlikte kaydedilmelidir. Normalde bu bilgiler, bağlayıcı yüklemesi sırasında açılan bir iletişim kutusunda girilir, ancak bunun yerine bu süreci otomatikleştirmek için PowerShell kullanabilirsiniz.

Katılımsız yükleme için iki adım vardır. İlk olarak bağlayıcıyı yüklemeniz gerekir. İkincisi, bağlayıcıyı Azure AD 'ye kaydedin.

## <a name="install-the-connector"></a>Bağlayıcıyı yükler
Bağlayıcıyı kaydetmeden yüklemek için aşağıdaki adımları kullanın:

1. Bir komut istemi açın.
2. Aşağıdaki komutu çalıştırın, burada/q sessiz yükleme anlamına gelir. Sessiz bir yükleme, Son Kullanıcı Lisans sözleşmesini kabul etmenizi istemez.

        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="register-the-connector-with-azure-ad"></a>Bağlayıcıyı Azure AD 'ye kaydetme
Bağlayıcıyı kaydetmek için kullanabileceğiniz iki yöntem vardır:

* Windows PowerShell kimlik bilgisi nesnesi kullanarak bağlayıcıyı kaydetme
* Çevrimdışı oluşturulan bir belirteci kullanarak bağlayıcıyı kaydetme

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>Windows PowerShell kimlik bilgisi nesnesi kullanarak bağlayıcıyı kaydetme
1. `$cred`Dizininiz için Yönetici Kullanıcı adı ve parola içeren bir Windows PowerShell kimlik bilgileri nesnesi oluşturun. Ve yerine aşağıdaki komutu çalıştırın *\<username\>* *\<password\>* :

        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. **C:\Program FILES\MICROSOFT AAD uygulama proxy Bağlayıcısı** ' na gidin ve oluşturduğunuz nesneyi kullanarak aşağıdaki betiği çalıştırın `$cred` :

        .\RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred -Feature ApplicationProxy

### <a name="register-the-connector-using-a-token-created-offline"></a>Çevrimdışı oluşturulan bir belirteci kullanarak bağlayıcıyı kaydetme
1. Aşağıdaki kod parçacığı veya PowerShell cmdlet 'lerinde bulunan değerleri kullanarak AuthenticationContext sınıfını kullanarak bir çevrimdışı belirteç oluşturun:

    **C# kullanarak:**

        using System;
        using System.Diagnostics;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

        class Program
        {
        #region constants
        /// <summary>
        /// The AAD authentication endpoint uri
        /// </summary>
        static readonly Uri AadAuthenticationEndpoint = new Uri("https://login.microsoftonline.com/common/oauth2/token?api-version=1.0");

        /// <summary>
        /// The application ID of the connector in AAD
        /// </summary>
        static readonly string ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489";

        /// <summary>
        /// The reply address of the connector application in AAD
        /// </summary>
        static readonly Uri ConnectorRedirectAddress = new Uri("urn:ietf:wg:oauth:2.0:oob");

        /// <summary>
        /// The AppIdUri of the registration service in AAD
        /// </summary>
        static readonly Uri RegistrationServiceAppIdUri = new Uri("https://proxy.cloudwebappproxy.net/registerapp");

        #endregion

        #region private members
        private string token;
        private string tenantID;
        #endregion

        public void GetAuthenticationToken()
        {
            AuthenticationContext authContext = new AuthenticationContext(AadAuthenticationEndpoint.AbsoluteUri);

            AuthenticationResult authResult = authContext.AcquireToken(RegistrationServiceAppIdUri.AbsoluteUri,
                ConnectorAppId,
                ConnectorRedirectAddress,
                PromptBehavior.Always);

            if (authResult == null || string.IsNullOrEmpty(authResult.AccessToken) || string.IsNullOrEmpty(authResult.TenantId))
            {
                Trace.TraceError("Authentication result, token or tenant id returned are null");
                throw new InvalidOperationException("Authentication result, token or tenant id returned are null");
            }

            token = authResult.AccessToken;
            tenantID = authResult.TenantId;
        }

    **PowerShell 'i kullanma:**

        # Locate AzureAD PowerShell Module
        # Change Name of Module to AzureAD after what you have installed
        $AADPoshPath = (Get-InstalledModule -Name AzureAD).InstalledLocation
        # Set Location for ADAL Helper Library
        $ADALPath = $(Get-ChildItem -Path $($AADPoshPath) -Filter Microsoft.IdentityModel.Clients.ActiveDirectory.dll -Recurse ).FullName | Select-Object -Last 1

        # Add ADAL Helper Library
        Add-Type -Path $ADALPath

        #region constants

        # The AAD authentication endpoint uri
        [uri]$AadAuthenticationEndpoint = "https://login.microsoftonline.com/common/oauth2/token?api-version=1.0/"

        # The application ID of the connector in AAD
        [string]$ConnectorAppId = "55747057-9b5d-4bd4-b387-abf52a8bd489"

        # The reply address of the connector application in AAD
        [uri]$ConnectorRedirectAddress = "urn:ietf:wg:oauth:2.0:oob"

        # The AppIdUri of the registration service in AAD
        [uri]$RegistrationServiceAppIdUri = "https://proxy.cloudwebappproxy.net/registerapp"

        #endregion

        #region GetAuthenticationToken

        # Set AuthN context
        $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $AadAuthenticationEndpoint

        # Build platform parameters
        $promptBehavior = [Microsoft.IdentityModel.Clients.ActiveDirectory.PromptBehavior]::Always
        $platformParam = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.PlatformParameters" -ArgumentList $promptBehavior

        # Do AuthN and get token
        $authResult = $authContext.AcquireTokenAsync($RegistrationServiceAppIdUri.AbsoluteUri, $ConnectorAppId, $ConnectorRedirectAddress, $platformParam).Result

        # Check AuthN result
        If (($authResult) -and ($authResult.AccessToken) -and ($authResult.TenantId) ) {
        $token = $authResult.AccessToken
        $tenantId = $authResult.TenantId
        }
        Else {
        Write-Output "Authentication result, token or tenant id returned are null"
        }

        #endregion

2. Belirteci aldıktan sonra belirteci kullanarak bir SecureString oluşturun:

   `$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`

3. Aşağıdaki Windows PowerShell komutunu çalıştırarak \<tenant GUID\> DIZIN Kimliğinizle değiştirin:

   `.\RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID> -Feature ApplicationProxy`

## <a name="next-steps"></a>Sonraki adımlar
* [Kendi etki alanı adınızı kullanarak uygulama yayımlama](application-proxy-configure-custom-domain.md)
* [Çoklu oturum açmayı etkinleştirme](application-proxy-configure-single-sign-on-with-kcd.md)
* [Uygulama Ara Sunucusu ile ilgili sorunları giderme](application-proxy-troubleshoot.md)
