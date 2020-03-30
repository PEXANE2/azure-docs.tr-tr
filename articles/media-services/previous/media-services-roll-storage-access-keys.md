---
title: Depolama erişim anahtarlarını yuvarladıktan sonra Medya Hizmetlerini Güncelleştir | Microsoft Dokümanlar
description: Bu makaleler, depolama erişim anahtarlarını yuvarladıktan sonra Medya Hizmetlerini nasıl güncelleştirisiniz konusunda size rehberlik sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: a892ebb0-0ea0-4fc8-b715-60347cc5c95b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.reviewer: milanga;cenkdin
ms.openlocfilehash: 2a0d1c5af572c88dc11bed950b46706f0a2f081f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75981954"
---
# <a name="update-media-services-after-rolling-storage-access-keys"></a>Depolama erişim anahtarlarını dağıttıktan sonra Media Services'i güncelleştirme 

Yeni bir Azure Medya Hizmetleri (AMS) hesabı oluşturduğunuzda, medya içeriğinizi depolamak için kullanılan bir Azure Depolama hesabı seçmeniz de istenir. Medya Hizmetleri hesabınıza birden fazla depolama hesabı ekleyebilirsiniz. Bu makalede, depolama anahtarları döndürülür nasıl gösterir. Ayrıca, bir medya hesabına depolama hesaplarının nasıl ekleyeceğini de gösterir. 

Bu makalede açıklanan eylemleri gerçekleştirmek için [Azure Kaynak Yöneticisi API'lerini](/rest/api/media/operations/azure-media-services-rest-api-reference) ve [Powershell'i](https://docs.microsoft.com/powershell/module/az.media)kullanıyor olmalısınız.  Daha fazla bilgi için [PowerShell ve Kaynak Yöneticisi ile Azure kaynaklarını nasıl yönetebilirsiniz'e](../../azure-resource-manager/management/manage-resource-groups-powershell.md)bakın.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış

Yeni bir depolama hesabı oluşturulduğunda, Azure depolama hesabınıza erişimin kimliğini doğrulamak için kullanılan iki 512 bit depolama erişim anahtarı oluşturur. Depolama bağlantılarınızın daha güvenli olması için, depolama erişim anahtarınızı düzenli aralıklarla yeniden oluşturmanız ve döndürmeniz önerilir. Diğer erişim anahtarını yeniden oluştururken bir erişim anahtarını kullanarak depolama hesabına bağlantı kurmanızı sağlamak için iki erişim anahtarı (birincil ve ikincil) sağlanır. Bu yordam da "haddeleme erişim anahtarları" olarak adlandırılır.

Medya Hizmetleri, ona sağlanan bir depolama anahtarına bağlıdır. Özellikle, varlıklarınızı akış veya indirmek için kullanılan yer bulucular belirtilen depolama erişim anahtarına bağlıdır. Bir AMS hesabı oluşturulduğunda, varsayılan olarak birincil depolama erişim anahtarına bağımlılık sağlar, ancak bir kullanıcı olarak AMS'nin sahip olduğu depolama anahtarını güncelleştirebilirsiniz. Bu makalede açıklanan adımları izleyerek Medya Hizmetleri'nin hangi anahtarı kullanacağınızı bilmesini sağlamalısınız.  

>[!NOTE]
> Birden çok depolama hesabınız varsa, bu yordamı her depolama hesabıyla gerçekleştirirsiniz. Depolama anahtarlarını döndürme sırası sabit değildir. Önce ikincil anahtarı, sonra birincil anahtarı veya tam tersini döndürebilirsiniz.
>
> Bu makalede üretim hesabında açıklanan adımları yürütmeden önce, bunları üretim öncesi hesapta sınayıp sınattığından emin olun.
>

## <a name="steps-to-rotate-storage-keys"></a>Depolama anahtarlarını döndürme adımları 
 
 1. Powershell cmdlet veya [Azure](https://portal.azure.com/) portalı aracılığıyla depolama hesabı Birincil anahtarını değiştirin.
 2. Medya hesabını depolama hesabı anahtarlarını almaya zorlamak için uygun paramlarla Sync-AzMediaServiceStorageKeys cmdlet'i arayın
 
    Aşağıdaki örnek, anahtarların depolama hesaplarıyla nasıl eşitlenebildiğini gösterir.
  
         Sync-AzMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId
  
 3. Bir saat kadar bekle. Akış senaryolarının çalıştığını doğrulayın.
 4. Powershell cmdlet veya Azure portalı aracılığıyla depolama hesabı ikincil anahtarını değiştirin.
 5. Media hesabını yeni depolama hesabı anahtarlarını almaya zorlamak için uygun paramlarla Sync-AzMediaServiceStorageKeys powershell'i arayın. 
 6. Bir saat kadar bekle. Akış senaryolarının çalıştığını doğrulayın.
 
### <a name="a-powershell-cmdlet-example"></a>Bir powershell cmdlet örneği 

Aşağıdaki örnek, depolama hesabının nasıl alınıp AMS hesabıyla senkronize edilebildiğini gösterir.

    $regionName = "West US"
    $resourceGroupName = "SkyMedia-USWest-App"
    $mediaAccountName = "sky"
    $storageAccountName = "skystorage"
    $storageAccountId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Storage/storageAccounts/$storageAccountName"

    Sync-AzMediaServiceStorageKeys -ResourceGroupName $resourceGroupName -AccountName $mediaAccountName -StorageAccountId $storageAccountId

 
## <a name="steps-to-add-storage-accounts-to-your-ams-account"></a>AMS hesabınıza depolama hesapları ekleme adımları

Aşağıdaki makalede, AMS hesabınıza depolama hesapları nın nasıl ekleyeceğiniz gösterilmektedir: [Bir Medya Hizmetleri hesabına birden çok depolama hesabı ekleyin.](meda-services-managing-multiple-storage-accounts.md)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a>İlgili kaynaklar
Bu belgenin oluşturulmasında emeği geçen lere teşekkür ederiz: Cenk Dingiloğlu, Milan Gada, Seva Titov.
