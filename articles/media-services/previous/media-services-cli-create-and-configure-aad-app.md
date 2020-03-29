---
title: Azure AD uygulaması oluşturmak ve Azure Medya Hizmetleri API'sine erişmek üzere yapılandırmak için Azure CLI'yi kullanın | Microsoft Dokümanlar
description: Bu konu, Azure AD uygulaması oluşturmak ve Azure Medya Hizmetleri API'sine erişmek üzere yapılandırmak için Azure CLI'nin nasıl kullanılacağını gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/26/2019
ms.author: juliako
ms.openlocfilehash: f136fb666e93adc0fe92aee014e3da9a37bbd6aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70035805"
---
# <a name="use-azure-cli-to-create-an-azure-ad-app-and-configure-it-to-access-media-services-api"></a>Azure AD uygulaması oluşturmak ve Medya Hizmetleri API'sine erişmek üzere yapılandırmak için Azure CLI'yi kullanın 

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Bu konu, Azure Medya Hizmetleri kaynaklarına erişmek için bir Azure Etkin Dizin (Azure AD) uygulaması ve hizmet ilkesi oluşturmak için Azure CLI'yi nasıl kullanacağınızı gösterir. 

## <a name="prerequisites"></a>Ön koşullar

- Bir Azure hesabı. Ayrıntılar için [Azure ücretsiz deneme sürümüne](https://azure.microsoft.com/pricing/free-trial/)bakın. 
- Bir Media Services hesabı. Daha fazla bilgi için [bkz.](media-services-portal-create-account.md)

## <a name="use-the-azure-cloud-shell"></a>Azure Bulut Kabuğunu Kullanma

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Bulut Kabuğunu portalın üst gezinti bölmesinden başlatın.

    ![Cloud Shell](./media/media-services-cli-create-and-configure-aad-app/media-services-cli-create-and-configure-aad-app01.png) 

Daha fazla bilgi için Azure [Bulut BulutU'na Genel Bakış](../../cloud-shell/overview.md)bölümüne bakın.

## <a name="create-an-azure-ad-app-and-configure-access-to-the-media-account-with-azure-cli"></a>Azure CLI ile bir Azure AD uygulaması oluşturun ve medya hesabına erişimi yapılandırma
 
```azurecli
az login
az ad sp create-for-rbac --name <appName> 
az role assignment create --assignee < user/app id> --role Contributor --scope <subscription/subscription id>
```

Örnek:

```azurecli
az role assignment create --assignee a3e068fa-f739-44e5-ba4d-ad57866e25a1 --role Contributor --scope /subscriptions/0b65e280-7917-4874-9fed-1307f2615ea2/resourceGroups/Default-AzureBatch-SouthCentralUS/providers/microsoft.media/mediaservices/sbbash
```

Bu örnekte, **kapsam** medya hizmetleri hesabı için tam kaynak yoludur. Ancak, **kapsam** herhangi bir düzeyde olabilir.

Örneğin, aşağıdaki düzeylerden biri olabilir:
 
* **Abonelik** düzeyi.
* **Kaynak grubu** düzeyi.
* **Kaynak** düzeyi (örneğin, bir Medya hesabı).

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli)

Ayrıca bkz. [Azure komut satırı arabirimiyle Rol Tabanlı Erişim Denetimini Yönet.](../../role-based-access-control/role-assignments-cli.md) 

## <a name="next-steps"></a>Sonraki adımlar

[Hesabınıza dosya yüklemeye](media-services-portal-upload-files.md)başlayın.
