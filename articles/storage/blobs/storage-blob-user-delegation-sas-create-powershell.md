---
title: Bir kapsayıcı veya blob için bir kullanıcı delegasyonu SAS oluşturmak için PowerShell'i kullanın
titleSuffix: Azure Storage
description: PowerShell'i kullanarak Azure Active Directory kimlik bilgileriyle bir kullanıcı delegasyonu SAS nasıl oluşturabilirsiniz öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/18/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: blobs
ms.openlocfilehash: 5250a27e6c5fcf012207f1edb95ad46c0aabfe63
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79536182"
---
# <a name="create-a-user-delegation-sas-for-a-container-or-blob-with-powershell"></a>PowerShell ile bir konteyner veya blob için bir kullanıcı delegasyonu SAS oluşturun

[!INCLUDE [storage-auth-sas-intro-include](../../../includes/storage-auth-sas-intro-include.md)]

Bu makalede, Azure PowerShell ile bir kapsayıcı veya blob için bir kullanıcı delegasyonu SAS oluşturmak için Azure Active Directory (Azure AD) kimlik bilgilerini nasıl kullanacağınız gösterilmektedir.

[!INCLUDE [storage-auth-user-delegation-include](../../../includes/storage-auth-user-delegation-include.md)]

## <a name="install-the-powershell-module"></a>PowerShell modüllerini yükleyin

PowerShell ile bir kullanıcı delegasyonu SAS oluşturmak için, Sürüm 1.10.0 veya daha sonra Az.Storage modülü yükleyin. Modülün en son sürümünü yüklemek için aşağıdaki adımları izleyin:

1. Azure PowerShell'in önceki yüklemelerini kaldırın:

    - **Ayarlar**altında ayarlar & **Uygulamalar'ı** kullanarak Azure PowerShell'in önceki tüm yüklemelerini Windows'dan kaldırın.
    - Tüm **Azure** modüllerini `%Program Files%\WindowsPowerShell\Modules`kaldırın.

1. PowerShellGet'in en son sürümünün yüklü olduğundan emin olun. Windows PowerShell penceresini açın ve en son sürümü yüklemek için aşağıdaki komutu çalıştırın:

    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```

1. PowerShellGet'i yükledikten sonra PowerShell penceresini kapatın ve yeniden açın.

1. Azure PowerShell'in en son sürümünü yükleyin:

    ```powershell
    Install-Module Az –Repository PSGallery –AllowClobber
    ```

1. Azure PowerShell sürüm 3.2.0 veya sonraki sürümlerini yüklediğinizden emin olun. Azure Depolama PowerShell modülünün en son sürümünü yüklemek için aşağıdaki komutu çalıştırın:

    ```powershell
    Install-Module -Name Az.Storage -Repository PSGallery -Force
    ```

1. PowerShell penceresini kapatın ve yeniden açın.

Az.Storage modülünün hangi sürümünün yüklü olduğunu denetlemek için aşağıdaki komutu çalıştırın:

```powershell
Get-Module -ListAvailable -Name Az.Storage -Refresh
```

Azure PowerShell'i yükleme hakkında daha fazla bilgi için [PowerShellGet ile Azure PowerShell'i Yükleyin'e](/powershell/azure/install-az-ps)bakın.

## <a name="sign-in-to-azure-powershell-with-azure-ad"></a>Azure AD ile Azure PowerShell'de oturum açın

Azure AD hesabınızla oturum açmanız için [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) komutunu arayın:

```powershell
Connect-AzAccount
```

PowerShell ile oturum açma hakkında daha fazla bilgi için [Azure PowerShell ile Oturum Aç'a](/powershell/azure/authenticate-azureps)bakın.

## <a name="assign-permissions-with-rbac"></a>RBAC ile izin atama

Azure PowerShell'den bir kullanıcı delegasyonu SAS oluşturmak için, PowerShell'de oturum açmada kullanılan Azure REKLAM hesabına **Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey** eylemini içeren bir rol atanması gerekir. Bu izin, Azure AD hesabının *kullanıcı temsilciliği anahtarını*istemesini sağlar. Kullanıcı delegasyonu anahtarı, kullanıcı delegasyonu SAS'ı imzalamak için kullanılır. **Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey** eylemini sağlayan rol, depolama hesabı, kaynak grubu veya abonelik düzeyinde atanmalıdır. Bir kullanıcı delegasyonu SAS oluşturmak için RBAC izinleri hakkında daha fazla bilgi için, [bir kullanıcı delegasyonu SAS oluşturun](/rest/api/storageservices/create-user-delegation-sas)RBAC bölümü ile atama **izinleri** bakın.

Bir Azure REKLAM güvenlik ilkesine RBAC rollerini atamak için yeterli izniniz yoksa, hesap sahibinden veya yöneticisinden gerekli izinleri atamasını istemeniz gerekebilir.

Aşağıdaki örnek, **Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey** eylemini içeren **Depolama Blob Veri Katılımcısı** rolünü atar. Rol, depolama hesabı düzeyinde kapsamlıdır.

Açı parantezindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Blob Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>"
```

**Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey** eylemini içeren yerleşik roller hakkında daha fazla bilgi [için Azure kaynakları için Yerleşik rollere](../../role-based-access-control/built-in-roles.md)bakın.

## <a name="use-azure-ad-credentials-to-secure-a-sas"></a>SAS'yi korumak için Azure REKLAM kimlik bilgilerini kullanma

Azure PowerShell ile bir kullanıcı delegasyonu SAS oluşturduğunuzda, SAS'ı imzalamak için kullanılan kullanıcı delegasyonu anahtarı sizin için zımni olarak oluşturulur. SAS için belirttiğiniz başlangıç ve son kullanma süresi, kullanıcı delegasyonu anahtarının başlangıç ve son kullanma süresi olarak da kullanılır. 

Kullanıcı delegasyonu anahtarının geçerli olduğu maksimum aralık başlangıç tarihinden itibaren 7 gün olduğundan, Başlangıç saatinden itibaren 7 gün içinde SAS için bir son kullanma süresi belirtmeniz gerekir. SAS, kullanıcı delegasyonu anahtarının süresi dolduktan sonra geçersizdir, bu nedenle 7 günden uzun bir süreye sahip bir SAS yalnızca 7 gün boyunca geçerli olacaktır.

Azure PowerShell ile bir kapsayıcı veya blob için bir kullanıcı delegasyonu SAS oluşturmak `-UseConnectedAccount` için, önce parametreyi belirten yeni bir Azure Depolama bağlam nesnesi oluşturun. Parametre, `-UseConnectedAccount` komutun oturum imzaladığınız Azure AD hesabı nın altında bağlam nesnesi oluşturduğunu belirtir.

Açı parantezindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
$ctx = New-AzStorageContext -StorageAccountName <storage-account> -UseConnectedAccount
```

### <a name="create-a-user-delegation-sas-for-a-container"></a>Kapsayıcı için kullanıcı delegasyonu SAS oluşturma

Bir kapsayıcı için bir kullanıcı delegasyonu SAS belirteci döndürmek için, daha önce oluşturduğunuz Azure Depolama bağlam nesnesinde geçerek [Yeni DepolamaContainerSASToken](/powershell/module/az.storage/new-azstoragecontainersastoken) komutunu arayın.

Aşağıdaki örnek, bir kapsayıcı için bir kullanıcı delegasyonu SAS belirteci döndürür. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
New-AzStorageContainerSASToken -Context $ctx `
    -Name <container> `
    -Permission racwdl `
    -ExpiryTime <date-time>
```

Döndürülen kullanıcı delegasyonu SAS belirteci aşağıdakilere benzer olacaktır:

```output
?sv=2018-11-09&sr=c&sig=<sig>&skoid=<skoid>&sktid=<sktid>&skt=2019-08-05T22%3A24%3A36Z&ske=2019-08-07T07%3A
00%3A00Z&sks=b&skv=2018-11-09&se=2019-08-07T07%3A00%3A00Z&sp=rwdl
```

### <a name="create-a-user-delegation-sas-for-a-blob"></a>Bir damla için bir kullanıcı delegasyonu SAS oluşturma

Bir kullanıcı delegasyonu SAS belirtecibir blob için döndürmek için, daha önce oluşturduğunuz Azure Depolama bağlam nesnesi geçen [Yeni-AzStorageBlobSASToken](/powershell/module/az.storage/new-azstorageblobsastoken) komutunu arayın.

Aşağıdaki sözdizimi bir damla için bir kullanıcı delegasyonu SAS döndürür. Örnek, eklenen SAS belirteciyle URI blob'u döndüren `-FullUri` parametreyi belirtir. Parantez içinde yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
New-AzStorageBlobSASToken -Context $ctx `
    -Container <container> `
    -Blob <blob> `
    -Permission racwd `
    -ExpiryTime <date-time>
    -FullUri
```

SAS URI döndürülen kullanıcı delegasyonu aşağıdakilere benzer olacaktır:

```output
https://storagesamples.blob.core.windows.net/sample-container/blob1.txt?sv=2018-11-09&sr=b&sig=<sig>&skoid=<skoid>&sktid=<sktid>&skt=2019-08-06T21%3A16%3A54Z&ske=2019-08-07T07%3A00%3A00Z&sks=b&skv=2018-11-09&se=2019-08-07T07%3A00%3A00Z&sp=racwd
```

> [!NOTE]
> Bir kullanıcı delegasyonu SAS, depolanmış bir erişim ilkesiyle izintanımlamayı desteklemez.

## <a name="revoke-a-user-delegation-sas"></a>Bir kullanıcı delegasyonu SAS iptal

Azure PowerShell'den bir kullanıcı delegasyonu SAS'ı iptal etmek için **İptal-AzStorageAccountUserDelegationKeys** komutunu arayın. Bu komut, belirtilen depolama hesabıyla ilişkili tüm kullanıcı delegasyonu anahtarlarını iptal eder. Bu anahtarlarla ilişkili tüm paylaşılan erişim imzaları geçersiz kılınur.

Açı parantezindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```powershell
Revoke-AzStorageAccountUserDelegationKeys -ResourceGroupName <resource-group> `
    -StorageAccountName <storage-account>
```

> [!IMPORTANT]
> Hem kullanıcı delegasyonu anahtarı hem de RBAC rol atamaları Azure Depolama tarafından önbelleğe alınır, bu nedenle iptal işlemini başlattığınız zaman ile varolan bir kullanıcı delegasyonu SAS'ın geçersiz olması arasında bir gecikme olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir kullanıcı delegasyonu SAS (REST API) oluşturma](/rest/api/storageservices/create-user-delegation-sas)
- [Kullanıcı Delegasyonu Anahtarı işlemi alın](/rest/api/storageservices/get-user-delegation-key)
