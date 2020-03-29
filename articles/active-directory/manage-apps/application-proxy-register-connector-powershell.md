---
title: Sessiz yükleme Azure AD App Proxy bağlayıcısı | Microsoft Dokümanlar
description: Şirket içi uygulamalarınıza güvenli uzaktan erişim sağlamak için Azure AD Application Proxy Bağlayıcısı'nın gözetimsiz yüklemesinin nasıl gerçekleştirilip gerçekleştirildirilebildiğini kapsar.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/24/2020
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: b43d2de0a366d7e69a025b2e4e2998dccda2038e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76756220"
---
# <a name="create-an-unattended-installation-script-for-the-azure-ad-application-proxy-connector"></a>Azure AD Application Proxy bağlayıcısı için katılımsız bir yükleme komut dosyası oluşturma

Bu konu, Azure AD Application Proxy bağlayıcınız için katılımsız yükleme ve kayıt sağlayan bir Windows PowerShell komut dosyası oluşturmanıza yardımcı olur.

Bu özellik, aşağıdakileri yapmak istediğinizde yararlıdır:

* Kullanıcı arabirimi etkin olmayan veya Uzak Masaüstü ile erişemediğiniz Windows sunucularına bağlayıcıyı yükleyin.
* Aynı anda birçok konektör yükleyin ve kaydedin.
* Bağlayıcı yüklemeve kaydı başka bir yordamın parçası olarak entegre edin.
* Bağlayıcı bitlerini içeren ancak kayıtlı olmayan standart bir sunucu görüntüsü oluşturun.

Uygulama [Proxy bağlayıcısının](application-proxy-connectors.md) çalışması için, bir uygulama yöneticisi ve parola kullanılarak Azure AD dizininize kaydedilmesi gerekir. Normalde bu bilgiler Connector yüklemesi sırasında açılır iletişim kutusuna girilir, ancak bunun yerine bu işlemi otomatikleştirmek için PowerShell'i kullanabilirsiniz.

Katılımsız yükleme için iki adım vardır. İlk olarak, konektörü yükleyin. İkinci olarak, bağlayıcıyı Azure AD'ye kaydedin. 

## <a name="install-the-connector"></a>Konektörü yükleme
Konektörü kaydetmeden yüklemek için aşağıdaki adımları kullanın:

1. Bir komut istemi açın.
2. /q'nun sessiz kurulum anlamına geldiğini aşağıdaki komutu çalıştırın. Sessiz bir yükleme, Son Kullanıcı Lisans Sözleşmesini kabul etmenizi istenmez.
   
        AADApplicationProxyConnectorInstaller.exe REGISTERCONNECTOR="false" /q

## <a name="register-the-connector-with-azure-ad"></a>Bağlayıcıyı Azure AD ile kaydetme
Bağlayıcıyı kaydetmek için kullanabileceğiniz iki yöntem vardır:

* Bir Windows PowerShell kimlik nesnesi kullanarak bağlayıcıyı kaydetme
* Çevrimdışı oluşturulan bir belirteç kullanarak bağlayıcıyı kaydetme

### <a name="register-the-connector-using-a-windows-powershell-credential-object"></a>Bir Windows PowerShell kimlik nesnesi kullanarak bağlayıcıyı kaydetme
1. Dizininiz için yönetimkullanıcı adı ve parola içeren bir Windows PowerShell Kimlik Bilgileri nesnesi `$cred` oluşturun. * \<Kullanıcı\> adı* ve * \<parolayerine\>* aşağıdaki komutu çalıştırın:
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
2. **C:\Program Files\Microsoft AAD App Proxy** Bağlayıcısı'na gidin `$cred` ve oluşturduğunuz nesneyi kullanarak aşağıdaki komut dosyasını çalıştırın:
   
        .\RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred -Feature ApplicationProxy

### <a name="register-the-connector-using-a-token-created-offline"></a>Çevrimdışı oluşturulan bir belirteç kullanarak bağlayıcıyı kaydetme
1. Aşağıdaki kod parçacıklarındaki veya PowerShell cmdlets'teki değerleri kullanarak Kimlik Doğrulama Bağlamı sınıfını kullanarak çevrimdışı bir belirteç oluşturun:

    **C#'ı kullanma:**

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

    **PowerShell'i kullanma:**

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

2. Belirteci aldıktan sonra, belirteci kullanarak bir SecureString oluşturun:

   `$SecureToken = $Token | ConvertTo-SecureString -AsPlainText -Force`

3. Kiracı GUID'i \<\> dizin kimliğinizle değiştirerek aşağıdaki Windows PowerShell komutunu çalıştırın:

   `.\RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft AAD App Proxy Connector\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Token -Token $SecureToken -TenantId <tenant GUID> -Feature ApplicationProxy`

## <a name="next-steps"></a>Sonraki adımlar 
* [Kendi etki alanı adınızı kullanarak uygulama yayımlama](application-proxy-configure-custom-domain.md)
* [Çoklu oturum açmayı etkinleştirme](application-proxy-configure-single-sign-on-with-kcd.md)
* [Uygulama Ara Sunucusu ile ilgili sorunları giderme](application-proxy-troubleshoot.md)


