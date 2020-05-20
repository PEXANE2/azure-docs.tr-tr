---
title: Market ölçüm hizmeti kimlik doğrulama stratejileri | Azure Marketi
description: Azure Marketi 'nde desteklenen ölçüm hizmeti kimlik doğrulama stratejileri.
author: qianw211
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/13/2020
ms.openlocfilehash: 4b3a2ed71845b8848c9cb0ac5002e0c69a170410
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83642306"
---
# <a name="marketplace-metering-service-authentication-strategies"></a>Market ölçüm hizmeti kimlik doğrulama stratejileri

Market ölçüm hizmeti iki kimlik doğrulama stratejisi destekler:

* [Azure AD güvenlik belirteci](https://docs.microsoft.com/azure/active-directory/develop/access-tokens)
* [Yönetilen kimlikler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) 

Market ölçüm hizmetini kullanarak özel ölçümleri güvenli bir şekilde göndermek için farklı kimlik doğrulama stratejilerinin ne zaman ve nasıl kullanılacağını açıklayacağız.

## <a name="using-the-azure-ad-security-token"></a>Azure AD güvenlik belirtecini kullanma

Uygun teklif türleri, yönetilen uygulama planı türüne sahip SaaS ve Azure uygulamalardır.  

Kimlik doğrulaması için önceden tanımlanmış bir sabit uygulama KIMLIĞI kullanarak özel ölçümler gönderebilirsiniz.

SaaS teklifleri için, Azure AD kullanılabilir tek seçenektir.

Yönetilen uygulama planına sahip Azure uygulamaları için, aşağıdaki durumlarda bu stratejiyi kullanmayı göz önünde bulundurmanız gerekir:

* Arka uç hizmetlerinize iletişim kurmak için zaten bir mekanizmanız var ve bu mekanizmayı bir merkezi hizmetten özel ölçümleri yaymaya uzatmak istiyorsunuz.
* Karmaşık özel ölçüm mantığınızı kullanabilirsiniz.  Bu mantığı yönetilen uygulama kaynakları yerine merkezi bir konumda çalıştırın.

Uygulamanızı kaydettikten sonra programlı bir şekilde Azure AD güvenlik belirteci isteyebilirsiniz. Yayımcının bu belirteci kullanması ve bunu çözmek için bir istek yapması beklenmektedir.

Bu belirteçler hakkında daha fazla bilgi için bkz. [Azure Active Directory erişim belirteçleri](https://docs.microsoft.com/azure/active-directory/develop/access-tokens).

### <a name="get-a-token-based-on-the-azure-ad-app"></a>Azure AD uygulaması 'nı temel alan bir belirteç alın

#### <a name="http-method"></a>HTTP yöntemi

**Yayınla**

#### <a name="request-url"></a>*İstek URL’si*

**`https://login.microsoftonline.com/*{tenantId}*/oauth2/token`**

#### <a name="uri-parameter"></a>*URI parametresi*

|  **Parametre adı** |  **Gerekli**  |  **Açıklama**          |
|  ------------------ |--------------- | ------------------------  |
|  `tenantId`         |   True         | Kayıtlı Azure AD uygulamasının kiracı KIMLIĞI.   |
| | | |

#### <a name="request-header"></a>*İstek üst bilgisi*

|  **Üst bilgi adı**    |  **Gerekli**  |  **Açıklama**          |
|  ------------------ |--------------- | ------------------------  |
|  `Content-Type`     |   True         | İstekle ilişkilendirilmiş içerik türü. Varsayılan değer: `application/x-www-form-urlencoded`.  |
| | | |

#### <a name="request-body"></a>*İstek gövdesi*

|  **Özellik adı**  |  **Gerekli**  |  **Açıklama**          |
|  ------------------ |--------------- | ------------------------  |
|  `Grant_type`       |   True         | Verme türü. Varsayılan değer: `client_credentials`. |
|  `Client_id`        |   True         | Azure AD uygulamasıyla ilişkili istemci/uygulama tanımlayıcısı.|
|  `client_secret`    |   True         | Azure AD uygulamasıyla ilişkili parola.  |
|  `Resource`         |   True         | Belirtecin istendiği hedef kaynak. Varsayılan değer: `20e940b3-4c77-4b0b-9a53-9e16a1b010a7`.  |
| | | |

#### <a name="response"></a>*Yanıtıyla*

|  **Adı**    |  **Tür**  |  **Açıklama**          |
|  ------------------ |--------------- | ----------------------  |
|  `200 OK`     |   `TokenResponse`    | İstek başarılı oldu.  |
| | | |

#### <a name="tokenresponse"></a>*TokenResponse*

Örnek yanıt belirteci:

```JSON
  {
      "token_type": "Bearer",
      "expires_in": "3600",
      "ext_expires_in": "0",
      "expires_on": "15251…",
      "not_before": "15251…",
      "resource": "20e940b3-4c77-4b0b-9a53-9e16a1b010a7",
      "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayIsImtpZCI6ImlCakwxUmNxemhpeTRmcHhJeGRacW9oTTJZayJ9…"
  }
```

## <a name="using-the-azure-managed-identities-token"></a>Azure tarafından yönetilen kimlikler belirtecini kullanma

Geçerli teklif türü, yönetilen uygulama planı türüne sahip Azure uygulamalardır.

Bu yaklaşımın kullanılması, dağıtılan kaynak kimliğinin özel ölçüm kullanım olaylarını göndermek için kimlik doğrulaması yapmasına olanak sağlar.  Kullanımınız, dağıtımınızın sınırları içinde yayan kodu gömebilirsiniz.

>[!Note]
>Yayımcı, kullanımı yayan kaynakların kilitli olduğundan emin olmalıdır, bu nedenle değişiklik yapılmayacaktır.

Yönetilen uygulamanız, sanal makinelerden Azure Işlevlerine kadar farklı kaynak türleri içerebilir.  Farklı hizmetler için Yönetilen kimlikler kullanarak kimlik doğrulaması yapma hakkında daha fazla bilgi için bkz. [Azure kaynakları için Yönetilen kimlikler kullanma](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview#how-can-i-use-managed-identities-for-azure-resources).

Örneğin, bir Windows VM kullanarak kimlik doğrulamak için aşağıdaki adımları izleyin.

1. Yönetilen kimliğin yöntemlerden birini kullanarak yapılandırıldığından emin olun:
    * [Azure portal Kullanıcı arabirimi](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm)
    * [CLı](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm)
    * [PowerShell](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm)
    * [Azure Resource Manager şablonu](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm)
    * [REST](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-rest-vm#system-assigned-managed-identity)
    * [Azure SDK’ları](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm)

1. Market ölçüm hizmeti uygulama KIMLIĞI ( `20e940b3-4c77-4b0b-9a53-9e16a1b010a7` ) için sistem kimliğini, VM 'nın RDP 'sini kullanarak bir erişim belirteci alın, PowerShell konsolunu açın ve aşağıdaki komutu çalıştırın

    ```powershell
    # curl is an alias to Web-Invoke PowerShell command
    # Get system identity access tokenn
    $MetadataUrl = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F"
    $Token = curl -H @{"Metadata" = "true"} $MetadataUrl | Select-Object -Expand Content | ConvertFrom-Json
    $Headers = @{}
    $Headers.Add("Authorization","$($Token.token_type) "+ " " + "$($Token.access_token)")
    ```

1. ' ManagedBy ' özelliğinden geçerli kaynak gruplarından yönetilen uygulama KIMLIĞI al

    ```powershell
    # Get subscription and resource group
    $metadata = curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2019-06-01 | select -ExpandProperty Content | ConvertFrom-Json 
    
    # Make sure the system identity has at least reader permission on the resource group
    $managementUrl = "https://management.azure.com/subscriptions/" + $metadata.compute.subscriptionId + "/resourceGroups/" + $metadata.compute.resourceGroupName + "?api-version=2019-10-01"
    $resourceGroupInfo = curl -Headers $Headers $managementUrl | select -ExpandProperty Content | ConvertFrom-Json
    $managedappId = $resourceGroupInfo.managedBy 
    ```

1. Market ölçüm hizmeti, bir üzerinde kullanımı `resourceID` ve yönetilen bir uygulamayı rapor etmek için gereklidir `resourceUsageId` .

    ```powershell
    # Get resourceUsageId from the managed app
    $managedAppUrl = "https://management.azure.com/subscriptions/" + $metadata.compute.subscriptionId + "/resourceGroups/" + $metadata.compute.resourceGroupName + "/providers/Microsoft.Solutions/applications/" + $managedappId + "\?api-version=2019-07-01"
    $ManagedApp = curl $managedAppUrl -H $Headers | Select-Object -Expand Content | ConvertFrom-Json
    # Use this resource ID to emit usage 
    $resourceUsageId = $ManagedApp.properties.billingDetails.resourceUsageId
    ```

1. Kullanım için [Market ölçüm HIZMETI API](./marketplace-metering-service-apis.md) 'sini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure uygulama teklifi oluşturma](./create-new-azure-apps-offer.md)