---
title: Microsoft Graph PowerShell SDK ve Azure Active Directory Kimlik Koruması
description: Microsoft Graph risk algılamalarını ve ilgili bilgileri nasıl sorguleyeceğinizi öğrenin Azure Active Directory
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: how-to
ms.date: 01/25/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2db8cfe652c0fca4b68b00d846e345c1b60cd05d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98880245"
---
# <a name="azure-active-directory-identity-protection-and-the-microsoft-graph-powershell-sdk"></a>Azure Active Directory Kimlik Koruması ve Microsoft Graph PowerShell SDK 'Sı

Microsoft Graph, Microsoft Birleşik API uç noktası ve [Azure Active Directory kimlik koruması](./overview-identity-protection.md) API 'lerinin ana adresidir. Bu makalede, PowerShell kullanarak riskli Kullanıcı ayrıntılarını almak için [Microsoft Graph PowerShell SDK 'sının](/graph/powershell/get-started) nasıl kullanılacağı gösterilir. Microsoft Graph API 'Leri doğrudan sorgulamak isteyen kuruluşlar, öğretici: Bu yolculuğa başlamak için [Microsoft Graph API 'leri kullanarak riskleri belirlemek ve](/graph/tutorial-riskdetection-api) düzeltmek için bu makaleyi kullanabilir.


## <a name="connect-to-microsoft-graph"></a>Microsoft Graph Bağlan

Kimlik koruma verilerine Microsoft Graph aracılığıyla erişmenin dört adımı vardır:

- [Sertifika oluşturma](#create-a-certificate)
- [Yeni bir uygulama kaydı oluşturun](#create-a-new-app-registration)
- [API izinlerini yapılandırma](#configure-api-permissions)
- [Geçerli bir kimlik bilgisi yapılandırın](#configure-a-valid-credential)

### <a name="create-a-certificate"></a>Sertifika oluşturma

Üretim ortamında, üretim Sertifika yetkilinizden bir sertifika kullanırsınız, ancak bu örnekte otomatik olarak imzalanan bir sertifika kullanacağız. Aşağıdaki PowerShell komutlarını kullanarak sertifikayı oluşturun ve dışarı aktarın.

```powershell
$cert = New-SelfSignedCertificate -Subject "CN=MSGraph_ReportingAPI" -CertStoreLocation "Cert:\CurrentUser\My" -KeyExportPolicy Exportable -KeySpec Signature -KeyLength 2048 -KeyAlgorithm RSA -HashAlgorithm SHA256
Export-Certificate -Cert $cert -FilePath "C:\Reporting\MSGraph_ReportingAPI.cer"
```

### <a name="create-a-new-app-registration"></a>Yeni bir uygulama kaydı oluşturun

1. Azure Portal **Azure Active Directory**  >  **uygulama kayıtları**' e gidin.
1. **Yeni kayıt** seçeneğini belirleyin.
1. **Oluştur** sayfasında, aşağıdaki adımları uygulayın:
   1. **Ad** metin kutusuna uygulamanız için bir ad yazın (örneğin: Azure AD risk algılama API 'si).
   1. **Desteklenen hesap türleri** altında, API 'leri kullanacak hesapların türünü seçin.
   1. **Kaydet**’i seçin.
1. Bu öğelere daha sonra ihtiyacınız olacağı için **uygulama (istemci) kimliği** ve **Dizin (kiracı) kimliğini** de göz önünde bulabilirsiniz.

### <a name="configure-api-permissions"></a>API izinlerini yapılandırma

Bu örnekte, bu örneğe katılımsız olarak kullanılmasına izin veren uygulama izinlerini yapılandıracağız. Oturum açan bir kullanıcıya izin veriyorsanız, bunun yerine temsilci izinleri ' ni seçin. Farklı izin türleri hakkında daha fazla bilgi, [Microsoft Identity platformunda makalesinde, izinlerde ve onayda](../develop/v2-permissions-and-consent.md#permission-types)bulunabilir.

1. Oluşturduğunuz **uygulamadan** , **API izinleri**' ni seçin.
1. **Yapılandırılan izinler** sayfasında, üstteki araç çubuğundan **izin Ekle**' ye tıklayın.
1. **API erişimi ekle** sayfasında, **bir API seçin**' e tıklayın.
1. **BIR API seçin** sayfasında **Microsoft Graph**' yi seçin ve ardından **Seç**' e tıklayın.
1. **API Izinleri iste** sayfasında: 
   1. **Uygulama izinleri**' ni seçin.
   1. Ve ' nin yanındaki onay kutularını seçin `IdentityRiskEvent.Read.All` `IdentityRiskyUser.Read.All` .
   1. **Izin Ekle**' yi seçin.
1. **Etki alanı için yönetici onayı ver** ' i seçin 

### <a name="configure-a-valid-credential"></a>Geçerli bir kimlik bilgisi yapılandırın

1. Oluşturduğunuz **uygulamadan** **Sertifikalar & parolaları**' nı seçin.
1. **Sertifikalar** altında **sertifikayı karşıya yükle**' yi seçin.
   1. Açılan pencereden daha önce aktarılmış sertifikayı seçin.
   1. **Add (Ekle)** seçeneğini belirleyin.
1. Bir sonraki adımda bu bilgiye ihtiyacınız olacağı için sertifikanın **parmak izini** göz önünde bulabilirsiniz.

## <a name="list-risky-users-using-powershell"></a>PowerShell kullanarak riskli kullanıcıları listeleme

Microsoft Graph sorgulama özelliğini etkinleştirmek için, `Microsoft.Graph` komutunu kullanarak modülü PowerShell penceremiz olarak yüklememiz gerekir `Install-Module Microsoft.Graph` .

Aşağıdaki değişkenleri önceki adımlarda oluşturulan bilgileri içerecek şekilde değiştirin ve ardından PowerShell kullanarak riskli Kullanıcı ayrıntılarını almak için bunları bir bütün olarak çalıştırın.

```powershell
$ClientID       = "<your client ID here>"        # Application (client) ID gathered when creating the app registration
$tenantdomain   = "<your tenant domain here>"    # Directory (tenant) ID gathered when creating the app registration
$Thumbprint     = "<your client secret here>"    # Certificate thumbprint gathered when configuring your credential

Select-MgProfile -Name "beta"
  
Connect-MgGraph -ClientId $ClientID -TenantId $tenantdomain -CertificateThumbprint $Thumbprint

Get-MgRiskyUser -All
```

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft Graph PowerShell SDK 'sını kullanmaya başlama](/graph/powershell/get-started)
- [Öğretici: Microsoft Graph API 'Leri kullanarak riskleri tanımla ve Düzelt](/graph/tutorial-riskdetection-api)
- [Microsoft Graph’a genel bakış](https://developer.microsoft.com/graph/docs)
- [Kullanıcı olmadan erişim sağlayın](/graph/auth-v2-service)
- [Azure AD Kimlik Koruması hizmeti kökü](/graph/api/resources/identityprotectionroot)
- [Azure Active Directory Kimlik Koruması](./overview-identity-protection.md)
