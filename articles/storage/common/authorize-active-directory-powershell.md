---
title: Blob veya sıra verilerine erişmek için Azure AD kimlik bilgilerine sahip PowerShell komutlarını çalıştırın
titleSuffix: Azure Storage
description: PowerShell, Azure Depolama blob ve kuyruk verileri üzerinde komutları çalıştırmak için Azure AD kimlik bilgileriyle oturum açmayı destekler. Oturum için bir erişim belirteci sağlanır ve arama işlemlerine yetki vermek için kullanılır. İzinler, Azure AD güvenlik ilkesine atanan RBAC rolüne bağlıdır.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/30/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 373b939ac63f31ccaf6a9f01fac92920e19074ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75553455"
---
# <a name="run-powershell-commands-with-azure-ad-credentials-to-access-blob-or-queue-data"></a>Blob veya sıra verilerine erişmek için Azure AD kimlik bilgilerine sahip PowerShell komutlarını çalıştırın

Azure Depolama, PowerShell için Azure Active Directory (Azure AD) kimlik bilgileriyle oturum açmanızı ve komut komutlarını çalıştırmanızı sağlayan uzantılar sağlar. Azure AD kimlik bilgileriyle PowerShell'de oturum açtığınızda, OAuth 2.0 erişim jetonu döndürülür. Bu belirteç, PowerShell tarafından Blob veya Queue depolamasına karşı sonraki veri işlemlerini yetkilendirmek için otomatik olarak kullanılır. Desteklenen işlemler için artık komutla bir hesap anahtarı veya SAS belirteci geçmeniz gerekmez.

Rol tabanlı erişim denetimi (RBAC) aracılığıyla bir Azure AD güvenlik ilkesine veri blob ve sıralama izinleri atayabilirsiniz. Azure Depolama'daki RBAC rolleri hakkında daha fazla bilgi için [bkz.](storage-auth-aad-rbac.md)

## <a name="supported-operations"></a>Desteklenen operasyonlar

Azure Depolama uzantıları, blob ve sıra verilerindeki işlemler için desteklenir. Hangi işlemleri çağırabileceğiniz, PowerShell'de oturum açtığınızda Azure AD güvenlik ilkesine verilen izinlere bağlıdır. Azure Depolama kapsayıcılarına veya kuyruklarına izinler RBAC aracılığıyla atanır. Örneğin, **Blob Data Reader** rolüne atanmışsa, bir kapsayıcıdan veya kuyruktan verileri okuyan komut dosyası komutları çalıştırabilirsiniz. **Blob Veri Katılımcısı** rolü olarak atanmışsa, bir kapsayıcıyı veya sırayı veya içerdikleri verileri okuyan, yazan veya silen komut komutları çalıştırabilirsiniz.

Bir kapsayıcı veya kuyruktaki her Azure Depolama işlemi için gereken izinler hakkında ayrıntılı bilgi için, [OAuth belirteçleri içeren Arama depolama işlemlerine](/rest/api/storageservices/authorize-with-azure-active-directory#call-storage-operations-with-oauth-tokens)bakın.  

## <a name="call-powershell-commands-using-azure-ad-credentials"></a>Azure AD kimlik bilgilerini kullanarak PowerShell komutlarını arayın

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure AD kimlik bilgilerini kullanarak Azure Depolama'ya karşı sonraki işlemleri oturum açıp çalıştırmak için Azure PowerShell'i kullanmak, depolama hesabına başvurmak için bir depolama bağlamı oluşturun ve parametreyi `-UseConnectedAccount` ekleyin.

Aşağıdaki örnek, Azure AD kimlik bilgilerinizi kullanarak Azure PowerShell'den yeni bir depolama hesabında kapsayıcının nasıl oluşturulturunu gösterir. Açı parantezindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

1. [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) komutuyla Azure hesabınızda oturum açın:

    ```powershell
    Connect-AzAccount
    ```

    PowerShell ile Azure'da oturum açma hakkında daha fazla bilgi için Azure [PowerShell ile Oturum Aç'a](/powershell/azure/authenticate-azureps)bakın.

1. [Yeni-AzResourceGroup'u](/powershell/module/az.resources/new-azresourcegroup)arayarak bir Azure kaynak grubu oluşturun. 

    ```powershell
    $resourceGroup = "sample-resource-group-ps"
    $location = "eastus"
    New-AzResourceGroup -Name $resourceGroup -Location $location
    ```

1. [Yeni Depolama Hesabı'nı](/powershell/module/az.storage/new-azstorageaccount)arayarak bir depolama hesabı oluşturun.

    ```powershell
    $storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
      -Name "<storage-account>" `
      -SkuName Standard_LRS `
      -Location $location `
    ```

1. [Yeni Depolama Bağlamını](/powershell/module/az.storage/new-azstoragecontext)arayarak yeni depolama hesabını belirten depolama hesabı bağlamını alın. Bir depolama hesabında hareket ederken, kimlik bilgilerini tekrar tekrar geçmek yerine içeriğe başvuruda bulunabilirsiniz. Azure `-UseConnectedAccount` AD kimlik bilgilerinizi kullanarak sonraki veri işlemlerini çağırmak için parametreyi ekleyin:

    ```powershell
    $ctx = New-AzStorageContext -StorageAccountName "<storage-account>" -UseConnectedAccount
    ```

1. Kapsayıcıyı oluşturmadan [önce, Depolama Blob Veri Katılımcısı](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor) rolünü kendinize atayın. Hesap sahibi olsanız bile, depolama hesabına karşı veri işlemleri gerçekleştirmek için açık izinlere ihtiyacınız vardır. RBAC rolleri atama hakkında daha fazla bilgi için Azure [portalında RBAC ile Azure blob ve kuyruk verilerine erişim izni için](storage-auth-aad-rbac.md)bkz.

    > [!IMPORTANT]
    > RBAC rol atamalarının yayılması birkaç dakika sürebilir.

1. [Yeni-AzStorageContainer'ı](/powershell/module/az.storage/new-azstoragecontainer)arayarak bir kapsayıcı oluşturun. Bu çağrı önceki adımlarda oluşturulan bağlamı kullandığından, kapsayıcı Azure REKLAM kimlik bilgileriniz kullanılarak oluşturulur.

    ```powershell
    $containerName = "sample-container"
    New-AzStorageContainer -Name $containerName -Context $ctx
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Blob ve sıra verilerine erişmek için bir RBAC rolü atamak için PowerShell'i kullanma](storage-auth-aad-rbac-powershell.md)
- [Azure kaynakları için yönetilen kimliklerle blob ve sıra verilerine erişimi yetkilendirme](storage-auth-aad-msi.md)
