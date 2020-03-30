---
title: Yönetilen bir kimliğe sahip verilere erişimi yetkilendirme
titleSuffix: Azure Storage
description: Azure sanal makinelerinde, işlev uygulamalarında, sanal makine ölçek kümelerinde ve diğer uygulamalarda çalışan uygulamalardan gelen blob ve sıra verilerine erişimi yetkilendirmek için Azure kaynakları için yönetilen kimlikleri nasıl kullanacağınızı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/04/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: f3bac0d47a53da1ec4d1fa08b5f0933f5f65dc56
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255346"
---
# <a name="authorize-access-to-blob-and-queue-data-with-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimliklerle blob ve sıra verilerine erişimi yetkilendirme

Azure Blob ve Sıra depolama, Azure kaynakları için [yönetilen kimliklerle](../../active-directory/managed-identities-azure-resources/overview.md)Azure Etkin Dizin (Azure AD) kimlik doğrulamasını destekler. Azure kaynakları için yönetilen kimlikler, Azure sanal makinelerinde (VM), işlev uygulamalarında, sanal makine ölçek kümelerinde ve diğer hizmetlerde çalışan uygulamalardan Azure AD kimlik bilgilerini kullanarak blob ve sıra verilerine erişimi yetkilendirmeye izin verebilir. Azure kaynakları için yönetilen kimlikleri ve Azure AD kimlik doğrulaması kullanarak, bulutta çalışan uygulamalarınızla kimlik bilgilerini depolamaktan kaçınabilirsiniz.  

Bu makalede, Azure Kaynakları için yönetilen kimlikleri kullanarak bir Azure VM'den blob veya sıra verilerine erişimin nasıl yetkiverilen olduğu gösterilmektedir. Ayrıca, kodunuzu geliştirme ortamında nasıl sınaylayacağınızı da açıklar.

## <a name="enable-managed-identities-on-a-vm"></a>VM'de yönetilen kimlikleri etkinleştirme

VM'nizden blob'lara ve kuyruklara erişimi yetkilendirmek için Azure Kaynakları için yönetilen kimlikleri kullanabilmeniz için önce VM'deki Azure Kaynakları için yönetilen kimlikleri etkinleştirmeniz gerekir. Azure Kaynakları için yönetilen kimlikleri nasıl etkinleştireceklerini öğrenmek için şu makalelerden birine bakın:

- [Azure portalında](https://docs.microsoft.com/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm)
- [Azure PowerShell](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure CLI](../../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonu](../../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure Kaynak Yöneticisi istemci kitaplıkları](../../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

Yönetilen kimlikler hakkında daha fazla bilgi için Azure [kaynakları için Yönetilen kimlikler'e](../../active-directory/managed-identities-azure-resources/overview.md)bakın.

## <a name="authenticate-with-the-azure-identity-library"></a>Azure Kimlik kitaplığıyla kimlik doğrulaması

Azure Kimlik istemcikitaplığı, [Azure SDK](https://github.com/Azure/azure-sdk)için Azure AD belirteç kimlik doğrulama desteği sağlar. .NET, Java, Python ve JavaScript için Azure Depolama istemci kitaplıklarının en son sürümleri, Azure Depolama isteklerinin yetkilendirmesi için bir OAuth 2.0 belirteci elde etmek için basit ve güvenli bir araç sağlamak için Azure Kimlik kitaplığıyla tümleşir.

Azure Kimlik istemcikitaplığı'nın bir avantajı, uygulamanızın geliştirme ortamında mı yoksa Azure'da mı çalıştığını doğrulamak için aynı kodu kullanmanıza olanak sağlamasıdır. .NET için Azure Identity istemci kitaplığı bir güvenlik ilkesinin kimliğini doğrular. Kodunuz Azure'da çalışırken, güvenlik ilkesi Azure kaynakları için yönetilen bir kimliktir. Geliştirme ortamında, yönetilen kimlik yoktur, bu nedenle istemci kitaplığı sınama amacıyla kullanıcının veya hizmet ilkesinin kimliğini doğrular.

Kimlik doğrulaması yaptıktan sonra, Azure Kimlik istemcikitaplığı bir belirteç kimlik bilgisi alır. Bu belirteç kimlik bilgisi, Azure Depolama'ya karşı işlemleri gerçekleştirmek için oluşturduğunuz hizmet istemcisi nesnesinde kapsüllenir. Kitaplık, uygun belirteç kimlik belgesini alarak bunu sizin için sorunsuz bir şekilde işler.

.NET için Azure Identity istemci kitaplığı hakkında daha fazla bilgi için [.NET için Azure Identity istemci kitaplığına](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/identity/Azure.Identity)bakın. Azure Kimlik istemci kitaplığı için başvuru belgeleri için [Azure.Identity Ad Alanı'na](/dotnet/api/azure.identity)bakın.

### <a name="assign-role-based-access-control-rbac-roles-for-access-to-data"></a>Verilere erişim için rol tabanlı erişim denetimi (RBAC) rolleri atama

Bir Azure REKLAM güvenlik ilkesi blob veya sıra verilerine erişmeye çalıştığında, bu güvenlik ilkesinin kaynak için izinleri olmalıdır. Güvenlik ilkesi ister Azure'da yönetilen bir kimlik olsun ister geliştirme ortamında kod çalıştıran bir Azure AD kullanıcı hesabı olsun, güvenlik ilkesine Azure Depolama'daki blob veya sıra verilerine erişim sağlayan bir RBAC rolü atanmalıdır. RBAC üzerinden izin atama hakkında daha fazla bilgi için, Azure Etkin [Dizini'ni kullanarak Azure blob'larına ve kuyruklarına erişim yetkisindeki](../common/storage-auth-aad.md#assign-rbac-roles-for-access-rights) **erişim hakları için RBAC rolleri ata** başlıklı bölüme bakın.

### <a name="authenticate-the-user-in-the-development-environment"></a>Geliştirme ortamında kullanıcının kimliğini doğrulatın

Kodunuz geliştirme ortamında çalışırken, kimlik doğrulama otomatik olarak işlenebilir veya kullandığınız araçlara bağlı olarak bir tarayıcı girişi gerektirebilir. Örneğin, Microsoft Visual Studio tek oturum açma (SSO) destekler, böylece etkin Azure AD kullanıcı hesabı otomatik olarak kimlik doğrulama için kullanılır. SSO hakkında daha fazla bilgi [için, tek oturum açma uygulamalarına](../../active-directory/manage-apps/what-is-single-sign-on.md)bakın.

Diğer geliştirme araçları bir web tarayıcısı üzerinden giriş yapmanıza gerek kaçabilir.

### <a name="authenticate-a-service-principal-in-the-development-environment"></a>Geliştirme ortamında bir hizmet müdürünün doğrulamasını

Geliştirme ortamınız bir web tarayıcısı üzerinden tek oturum açma veya oturum açmayı desteklemiyorsa, geliştirme ortamından kimlik doğrulamak için bir hizmet ilkesi kullanabilirsiniz.

#### <a name="create-the-service-principal"></a>Hizmet sorumlusunu oluşturma

Azure CLI ile bir hizmet ilkesi oluşturmak ve bir RBAC rolü atamak [için az reklam sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) komutunu arayın. Yeni hizmet ilkesine atamak için bir Azure Depolama veri erişim rolü sağlayın. Ayrıca, rol ataması için kapsamı sağlayın. Azure Depolama için sağlanan yerleşik roller hakkında daha fazla bilgi için Azure [kaynakları için Yerleşik rollere](../../role-based-access-control/built-in-roles.md)bakın.

Hizmet yöneticisine bir rol atamak için yeterli izine sahip değilseniz, hesap sahibinden veya yöneticisinden rol atamasını gerçekleştirmesini istemeniz gerekebilir.

Aşağıdaki örnekte, yeni bir hizmet ilkesi oluşturmak ve hesap kapsamıyla **depolama Blob Veri Okuyucu** rolünü atamak için Azure CLI kullanılır

```azurecli-interactive
az ad sp create-for-rbac \
    --name <service-principal> \
    --role "Storage Blob Data Reader" \
    --scopes /subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
```

Komut, `az ad sp create-for-rbac` JSON formatında hizmet temel özelliklerinin listesini döndürür. Bir sonraki adımda gerekli ortam değişkenlerini oluşturmak için bunları kullanabilmeniz için bu değerleri kopyalayın.

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
> RBAC rol atamalarının yayılması birkaç dakika sürebilir.

#### <a name="set-environment-variables"></a>Ortam değişkenlerini belirleme

Azure Identity istemci kitaplığı, hizmet sorumlusunun kimliğini doğrulamak için çalışma zamanında üç ortam değişkeninden gelen değerleri okur. Aşağıdaki tabloda her ortam değişkeni için ayarlanan değer açıklanmaktadır.

|Ortam değişkeni|Değer
|-|-
|`AZURE_CLIENT_ID`|Hizmet sorumlusunun uygulama kimliği
|`AZURE_TENANT_ID`|Hizmet sorumlusunun Azure AD kiracı kimliği
|`AZURE_CLIENT_SECRET`|Hizmet sorumlusu için oluşturulan parola

> [!IMPORTANT]
> Ortam değişkenlerini ayarladıktan sonra konsol pencerenizi kapatın ve yeniden açın. Visual Studio veya başka bir geliştirme ortamı kullanıyorsanız, yeni ortam değişkenlerini kaydedebilmesi için geliştirme ortamını yeniden başlatmanız gerekebilir.

Daha fazla bilgi için [bkz.](../../active-directory/develop/howto-create-service-principal-portal.md)

[!INCLUDE [storage-install-packages-blob-and-identity-include](../../../includes/storage-install-packages-blob-and-identity-include.md)]

## <a name="net-code-example-create-a-block-blob"></a>.NET kodu örneği: Bir blok blob oluşturma

Azure Kimliği `using` ve Azure Depolama istemci kitaplıklarını kullanmak için kodunuza aşağıdaki yönergeleri ekleyin.

```csharp
using Azure;
using Azure.Identity;
using Azure.Storage.Blobs;
using System;
using System.IO;
using System.Text;
using System.Threading.Tasks;
```

Kodunuzu Azure Depolama'ya istekleri yetkilendirmek için kullanabileceği bir belirteç kimlik bilgisi almak için [Varsayılan Azure Credential](/dotnet/api/azure.identity.defaultazurecredential) sınıfının bir örneğini oluşturun. Aşağıdaki kod örneği, kimlik doğrulaması verilen belirteç kimlik bilgisinin nasıl alınıp bir hizmet istemcisi nesnesi oluşturmak için nasıl kullanılacağını, ardından yeni bir blob yüklemek için servis istemcisini nasıl kullanacağımı gösterir:

```csharp
async static Task CreateBlockBlobAsync(string accountName, string containerName, string blobName)
{
    // Construct the blob container endpoint from the arguments.
    string containerEndpoint = string.Format("https://{0}.blob.core.windows.net/{1}",
                                                accountName,
                                                containerName);

    // Get a credential and create a client object for the blob container.
    BlobContainerClient containerClient = new BlobContainerClient(new Uri(containerEndpoint),
                                                                    new DefaultAzureCredential());

    try
    {
        // Create the container if it does not exist.
        await containerClient.CreateIfNotExistsAsync();

        // Upload text to a new block blob.
        string blobContents = "This is a block blob.";
        byte[] byteArray = Encoding.ASCII.GetBytes(blobContents);

        using (MemoryStream stream = new MemoryStream(byteArray))
        {
            await containerClient.UploadBlobAsync(blobName, stream);
        }
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

> [!NOTE]
> Azure AD ile blob veya sıra verilerine karşı istekleri yetkilendirmek için bu istekler için HTTPS'yi kullanmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [RBAC ile depolama verilerine erişim haklarını yönetin.](storage-auth-aad-rbac.md)
- [Depolama uygulamalarıyla Azure AD'yi kullanın.](storage-auth-aad-app.md)
- [Blob veya kuyruk verilerine erişmek için Azure AD kimlik bilgileriyle Azure CLI veya PowerShell komutlarını çalıştırın.](authorize-active-directory-powershell.md)
