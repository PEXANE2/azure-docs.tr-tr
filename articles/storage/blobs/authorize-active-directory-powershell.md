---
title: Blob verilerine erişmek için Azure AD kimlik bilgileriyle PowerShell komutlarını çalıştırma
titleSuffix: Azure Storage
description: PowerShell, Azure depolama 'daki blob verilerinde komut çalıştırmak için Azure AD kimlik bilgileriyle oturum açmayı destekler. Oturum için bir erişim belirteci sağlanır ve çağrı işlemlerine yetki vermek için kullanılır. İzinler, Azure AD güvenlik sorumlusu 'na atanan Azure rolüne bağımlıdır.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 09/14/2020
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: blobs
ms.openlocfilehash: f936991f25023303a0a673c2c2b0550474e96028
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90108593"
---
# <a name="run-powershell-commands-with-azure-ad-credentials-to-access-blob-data"></a>Blob verilerine erişmek için Azure AD kimlik bilgileriyle PowerShell komutlarını çalıştırma

Azure depolama, PowerShell için, Azure Active Directory (Azure AD) kimlik bilgileriyle oturum açmanızı ve betik komutlarının çalıştırılmasını sağlayan uzantılar sağlar. PowerShell 'de Azure AD kimlik bilgileriyle oturum açtığınızda bir OAuth 2,0 erişim belirteci döndürülür. Bu belirteç, blob depolamaya yönelik sonraki veri işlemlerini yetkilendirmek için PowerShell tarafından otomatik olarak kullanılır. Desteklenen işlemler için artık komutuyla bir hesap anahtarını veya SAS belirtecini iletmeniz gerekmez.

Blob verilerine, rol tabanlı erişim denetimi (RBAC) aracılığıyla bir Azure AD güvenlik sorumlusuna izinler atayabilirsiniz. Azure depolama 'daki Azure rolleri hakkında daha fazla bilgi için bkz. [RBAC Ile Azure depolama verilerine erişim haklarını yönetme](../common/storage-auth-aad-rbac.md).

## <a name="supported-operations"></a>Desteklenen işlemler

Azure depolama uzantıları blob verilerinde işlemler için desteklenir. Hangi işlemler çağrlayabileceği, PowerShell 'de oturum açmak için Azure AD güvenlik sorumlusuna verilen izinlere bağlıdır. Azure depolama kapsayıcıları izinleri RBAC aracılığıyla atanır. Örneğin, **BLOB veri okuyucusu** rolünü atadıysanız, bir kapsayıcıdan veri okuyan betik komutlarını çalıştırabilirsiniz. **BLOB veri katılımcısı** rolüne atandıysanız, bir kapsayıcıyı veya içerdikleri verileri okuyan, yazan veya silen betik komutlarını çalıştırabilirsiniz.

Bir kapsayıcıdaki her bir Azure depolama işlemi için gereken izinler hakkında daha fazla bilgi için bkz. [OAuth belirteçleriyle depolama Işlemlerini çağırma](/rest/api/storageservices/authorize-with-azure-active-directory#call-storage-operations-with-oauth-tokens).  

## <a name="call-powershell-commands-using-azure-ad-credentials"></a>Azure AD kimlik bilgilerini kullanarak PowerShell komutlarını çağırma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure AD kimlik bilgilerini kullanarak Azure depolama 'da oturum açmak ve sonraki işlemleri çalıştırmak için Azure PowerShell kullanmak için depolama hesabına başvurmak üzere bir depolama bağlamı oluşturun ve `-UseConnectedAccount` parametresini ekleyin.

Aşağıdaki örnek, Azure AD kimlik bilgilerinizi kullanarak Azure PowerShell yeni bir depolama hesabında kapsayıcının nasıl oluşturulacağını gösterir. Açılı ayraçlar içindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

1. [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) komutuyla Azure hesabınızda oturum açın:

    ```powershell
    Connect-AzAccount
    ```

    Azure 'da PowerShell ile oturum açma hakkında daha fazla bilgi için bkz. [Azure PowerShell Ile oturum açma](/powershell/azure/authenticate-azureps).

1. [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)çağırarak bir Azure Kaynak grubu oluşturun. 

    ```powershell
    $resourceGroup = "sample-resource-group-ps"
    $location = "eastus"
    New-AzResourceGroup -Name $resourceGroup -Location $location
    ```

1. [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount)öğesini çağırarak bir depolama hesabı oluşturun.

    ```powershell
    $storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
      -Name "<storage-account>" `
      -SkuName Standard_LRS `
      -Location $location `
    ```

1. [New-AzStorageContext](/powershell/module/az.storage/new-azstoragecontext)' i çağırarak yeni depolama hesabını belirten depolama hesabı bağlamını alın. Bir depolama hesabında davrandığında, kimlik bilgilerini tekrar tekrar geçirmek yerine bağlama başvurabilirsiniz. `-UseConnectedAccount`Azure AD kimlik bilgilerinizi kullanarak sonraki veri işlemlerini çağırmak için parametresini ekleyin:

    ```powershell
    $ctx = New-AzStorageContext -StorageAccountName "<storage-account>" -UseConnectedAccount
    ```

1. Kapsayıcıyı oluşturmadan önce, [Depolama Blobu veri katılımcısı](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) rolünü kendinize atayın. Hesap sahibi olsanız bile, depolama hesabında veri işlemleri gerçekleştirmek için açık izinlere sahip olmanız gerekir. Azure rolleri atama hakkında daha fazla bilgi için bkz. [Azure Blob 'a erişim verme ve Azure Portal RBAC ile kuyruk verileri](../common/storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > Azure rolü atamalarının yayılması birkaç dakika sürebilir.

1. [New-AzStorageContainer](/powershell/module/az.storage/new-azstoragecontainer)çağırarak bir kapsayıcı oluşturun. Bu çağrı önceki adımlarda oluşturulan bağlamı kullandığından, kapsayıcı Azure AD kimlik bilgileriniz kullanılarak oluşturulur.

    ```powershell
    $containerName = "sample-container"
    New-AzStorageContainer -Name $containerName -Context $ctx
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Blob ve kuyruk verilerine erişim için bir Azure rolü atamak üzere PowerShell 'i kullanma](../common/storage-auth-aad-rbac-powershell.md)
- [Azure kaynakları için yönetilen kimliklerle blob ve kuyruk verilerine erişim yetkisi verme](../common/storage-auth-aad-msi.md)
