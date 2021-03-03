---
title: Azure CLı 'dan Azure Active Directory yönetilen bir kimlik uygulaması oluşturma
titleSuffix: An Azure Communication Services quickstart
description: Yönetilen kimlikler, Azure Iletişim Hizmetleri 'ne Azure VM 'Leri, işlev uygulamaları ve diğer kaynaklarda çalışan uygulamalardan erişim yetkisi verir.
services: azure-communication-services
author: jbeauregardb
ms.service: azure-communication-services
ms.topic: how-to
ms.date: 02/25/2021
ms.author: jbeauregardb
ms.reviewer: mikben
ms.openlocfilehash: f60c7b0b4c89833d489a435fde85896a8f8bd72f
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101664097"
---
# <a name="authorize-access-with-managed-identity-to-your-communication-resource-in-your-development-environment"></a>Geliştirme ortamınızdaki iletişim kaynağınız ile yönetilen kimlik erişimi yetkisi verin

Azure Identity istemci kitaplığı, Azure SDK için Azure Active Directory (Azure AD) belirteç kimlik doğrulama desteği sağlar. .NET, Java, Python ve JavaScript için Azure Communication Services istemci kitaplıklarının en son sürümleri, Azure Iletişim Hizmetleri isteklerinin yetkilendirmesi için bir OAuth 2,0 belirteci elde etmek üzere basit ve güvenli bir yöntem sağlamak üzere Azure Identity Library ile tümleşir.

Azure Identity istemci kitaplığı 'nın bir avantajı, uygulamanızın geliştirme ortamında veya Azure 'da çalışıp çalışmadığını birden çok hizmet üzerinde kimlik doğrulaması yapmak için aynı kodu kullanmanıza olanak tanımasıdır. Azure Identity istemci kitaplığı bir güvenlik sorumlusunun kimliğini doğrular. Kodunuz Azure 'da çalışırken, güvenlik sorumlusu Azure kaynakları için yönetilen bir kimliktir. Geliştirme ortamında, yönetilen kimlik yok, bu nedenle istemci kitaplığı Kullanıcı veya test amacıyla kayıtlı bir uygulamanın kimliğini doğrular.

## <a name="prerequisites"></a>Önkoşullar

 - Azure CLI. [Yükleme kılavuzu](https://docs.microsoft.com/cli/azure/install-azure-cli)
 - Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free)

## <a name="setting-up"></a>Ayarlanıyor

Yönetilen kimlikler, yetkilendirmekte olduğunuz Azure kaynaklarında etkinleştirilmelidir. Azure kaynakları için yönetilen kimliklerin nasıl etkinleştireceğinizi öğrenmek için şu makalelerden birine bakın:

- [Azure portalındaki](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure CLI](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonu](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure Resource Manager istemci kitaplıkları](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)
- [Uygulama Hizmetleri](../../app-service/overview-managed-identity.md)

## <a name="authenticate-a-registered-application-in-the-development-environment"></a>Geliştirme ortamında kayıtlı bir uygulamanın kimliğini doğrulama

Geliştirme ortamınız bir Web tarayıcısı aracılığıyla çoklu oturum açmayı veya oturum açmayı desteklemiyorsa, geliştirme ortamından kimlik doğrulaması yapmak için kayıtlı bir uygulama kullanabilirsiniz.

### <a name="creating-an-azure-active-directory-registered-application"></a>Azure Active Directory kayıtlı bir uygulama oluşturma

Azure CLı 'dan kayıtlı bir uygulama oluşturmak için, işlemlerin gerçekleşmesini istediğiniz Azure hesabında oturum açmanız gerekir. Bunu yapmak için `az login` komutunu kullanabilir ve kimlik bilgilerinizi tarayıcıda girebilirsiniz. CLı 'dan Azure hesabınızda oturum açtıktan sonra, `az ad sp create-for-rbac` kayıtlı uygulamayı oluşturmak için komutunu çağırabiliriz.

Aşağıdaki örnekler, yeni bir kayıtlı uygulama oluşturmak için Azure CLı kullanır

```azurecli
az ad sp create-for-rbac --name <application-name> 
```

`az ad sp create-for-rbac`Komut, JSON biçiminde hizmet sorumlusu özelliklerinin bir listesini döndürür. Bir sonraki adımda gerekli ortam değişkenlerini oluşturmak için bunları kullanabilmeniz için bu değerleri kopyalayın.

```json
{
    "appId": "generated-app-ID",
    "displayName": "service-principal-name",
    "name": "http://service-principal-uri",
    "password": "generated-password",
    "tenant": "tenant-ID"
}
```
> [!IMPORTANT]
> Azure rolü atamalarının yayılması birkaç dakika sürebilir.

#### <a name="set-environment-variables"></a>Ortam değişkenlerini belirleme

Azure Identity istemci kitaplığı, uygulamanın kimliğini doğrulamak için çalışma zamanında üç ortam değişkeninden değerleri okur. Aşağıdaki tabloda her ortam değişkeni için ayarlanacak değer açıklanmaktadır.

|Ortam değişkeni|Değer
|-|-
|`AZURE_CLIENT_ID`|`appId` oluşturulan JSON 'daki değer 
|`AZURE_TENANT_ID`|`tenant` oluşturulan JSON 'daki değer
|`AZURE_CLIENT_SECRET`|`password` oluşturulan JSON 'daki değer

> [!IMPORTANT]
> Ortam değişkenlerini ayarladıktan sonra konsol pencerenizi kapatıp yeniden açın. Visual Studio veya başka bir geliştirme ortamı kullanıyorsanız, yeni ortam değişkenlerini kaydedebilmesi için yeniden başlatmanız gerekebilir.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kimlik doğrulaması hakkında bilgi edinin](../concepts/authentication.md)

Ayrıca şunları yapmak isteyebilirsiniz:

- [Azure kimlik Kitaplığı hakkında daha fazla bilgi](/dotnet/api/overview/azure/identity-readme)
