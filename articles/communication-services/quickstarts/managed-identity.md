---
title: Iletişim hizmetlerinde Yönetilen kimlikler kullanma (.NET)
titleSuffix: An Azure Communication Services quickstart
description: Yönetilen kimlikler, Azure Iletişim Hizmetleri 'ne Azure VM 'Leri, işlev uygulamaları ve diğer kaynaklarda çalışan uygulamalardan erişim yetkisi verir.
services: azure-communication-services
author: stefang931
ms.service: azure-communication-services
ms.topic: how-to
ms.date: 12/04/2020
ms.author: gistefan
ms.reviewer: mikben
ms.openlocfilehash: 7e8d9b56077819fc404d6c2bdc39f9f697224136
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101692190"
---
# <a name="use-managed-identities-net"></a>Yönetilen kimlikler kullanma (.NET)

.NET 'teki yönetilen kimlikleri kullanarak Azure Iletişim Hizmetleri ile çalışmaya başlayın. Iletişim Hizmetleri Yönetimi ve SMS istemci kitaplıkları, [Azure kaynakları için yönetilen kimliklerle](../../active-directory/managed-identities-azure-resources/overview.md)Azure Active Directory (Azure AD) kimlik doğrulamasını destekler.

Bu hızlı başlangıçta, yönetilen kimlikleri destekleyen bir Azure ortamından yönetim ve SMS istemci kitaplıklarına erişim yetkisi verme işlemleri gösterilir. Ayrıca, kodunuzun bir geliştirme ortamında nasıl test edileceğini açıklar.

## <a name="prerequisites"></a>Önkoşullar

 - Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free)
 - Etkin bir Iletişim Hizmetleri kaynağı ve bağlantı dizesi. [Iletişim Hizmetleri kaynağı oluşturun](./create-communication-resource.md?pivots=platform-azp&tabs=windows).

## <a name="setting-up"></a>Ayarlanıyor

### <a name="enable-managed-identities-on-a-virtual-machine-or-app-service"></a>Bir sanal makine veya App Service üzerinde yönetilen kimlikleri etkinleştirme

Yönetilen kimlikler, yetkilendirmekte olduğunuz Azure kaynaklarında etkinleştirilmelidir. Azure kaynakları için yönetilen kimliklerin nasıl etkinleştireceğinizi öğrenmek için şu makalelerden birine bakın:

- [Azure portalındaki](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure CLI](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonu](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure Resource Manager istemci kitaplıkları](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)
- [Uygulama Hizmetleri](../../app-service/overview-managed-identity.md)

#### <a name="assign-azure-roles-with-the-azure-portal"></a>Azure rollerini Azure portal atama

1. Azure portalına gidin.
1. Azure Iletişim hizmeti kaynağına gidin.
1. Access Control (ıAM) menüsünde > + Add-> rol ataması Ekle ' ye gidin.
1. "Katkıda bulunan" rolünü seçin (Bu, desteklenen tek roldür).
1. "Kullanıcı tarafından atanan yönetilen kimlik" (veya "sistem tarafından atanmış bir kimlik") seçeneğini belirleyin ve istediğiniz kimliği seçin. Seçiminizi kaydedin.

![Yönetilen kimlik rolü](media/managed-identity-assign-role.png)

#### <a name="assign-azure-roles-with-powershell"></a>PowerShell ile Azure rolleri atama

PowerShell kullanarak rol ve izinler atamak için bkz. [Azure PowerShell kullanarak Azure rol atamaları ekleme veya kaldırma](../../../articles/role-based-access-control/role-assignments-powershell.md)

## <a name="add-managed-identity-to-your-communication-services-solution"></a>Iletişim Hizmetleri çözümünüze yönetilen kimlik ekleme

### <a name="install-the-client-library-packages"></a>İstemci kitaplığı paketlerini yükler

```console
dotnet add package Azure.Communication.Identity
dotnet add package Azure.Communication.Configuration
dotnet add package Azure.Communication.Sms
dotnet add package Azure.Identity
```

### <a name="use-the-client-library-packages"></a>İstemci kitaplığı paketlerini kullanma

`using`Azure kimlik ve Azure depolama istemci kitaplıklarını kullanmak için aşağıdaki yönergeleri kodunuza ekleyin.

```csharp
using Azure.Identity;
using Azure.Communication.Identity;
using Azure.Communication.Configuration;
using Azure.Communication.Sms;
```

Aşağıdaki örneklerde [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential)kullanılmaktadır. Bu kimlik bilgisi üretim ve geliştirme ortamları için uygundur.

### <a name="create-an-identity-and-issue-a-token"></a>Bir kimlik oluşturma ve belirteç verme

Aşağıdaki kod örneği, Azure Active Directory belirteçleriyle bir hizmet istemci nesnesinin nasıl oluşturulduğunu gösterir ve ardından yeni bir kullanıcı için bir belirteç vermek üzere istemciyi kullanır:

```csharp
     public async Task<Response<CommunicationUserToken>> CreateIdentityAndIssueTokenAsync(Uri resourceEdnpoint) 
     {
          TokenCredential credential = new DefaultAzureCredential();
     
          var client = new CommunicationIdentityClient(resourceEndpoint, credential);
          var identityResponse = await client.CreateUserAsync();
     
          var tokenResponse = await client.IssueTokenAsync(identity, scopes: new [] { CommunicationTokenScope.VoIP });

          return tokenResponse;
     }
```

### <a name="send-an-sms-with-azure-active-directory-tokens"></a>Azure Active Directory belirteçleriyle SMS gönderin

Aşağıdaki kod örneği, Azure Active Directory belirteçleriyle bir hizmet istemci nesnesinin nasıl oluşturulduğunu gösterir ve ardından bir SMS iletisi göndermek için istemcisini kullanır:

```csharp

     public async Task SendSmsAsync(Uri resourceEndpoint, PhoneNumber from, PhoneNumber to, string message)
     {
          TokenCredential credential = new DefaultAzureCredential();
     
          SmsClient smsClient = new SmsClient(resourceEndpoint, credential);
          smsClient.Send(
               from: from,
               to: to,
               message: message,
               new SendSmsOptions { EnableDeliveryReport = true } // optional
          );
     }
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kimlik doğrulaması hakkında bilgi edinin](../concepts/authentication.md)

Ayrıca şunları yapmak isteyebilirsiniz:

- [Azure rol tabanlı erişim denetimi hakkında daha fazla bilgi edinin](../../../articles/role-based-access-control/index.yml)
- [.NET için Azure kimlik Kitaplığı hakkında daha fazla bilgi edinin](/dotnet/api/overview/azure/identity-readme)
- [Kullanıcı erişim belirteçleri oluşturma](../quickstarts/access-tokens.md)
- [SMS iletisi gönderme](../quickstarts/telephony-sms/send.md)
- [SMS hakkında daha fazla bilgi](../concepts/telephony-sms/concepts.md)