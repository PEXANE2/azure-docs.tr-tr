---
title: Bir ilkeden imza anahtarı al .NET
description: Bu konuda, Media Services v3 .NET SDK kullanılarak mevcut ilkeden nasıl imza anahtarı alınacağı gösterilmektedir.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: how-to
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: seodec18
ms.openlocfilehash: 1436561f7c82446038c231fadec3bd62c94d4ff9
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98955114"
---
# <a name="get-a-signing-key-from-the-existing-policy"></a>Var olan ilkeden imzalama anahtarı alma

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

v3 API’nin temel tasarım ilkelerinden biri API’yi daha güvenli hale getirmektir. v3 API 'Leri, **Get** veya **list** işlemlerinde gizli dizileri veya kimlik bilgilerini döndürmez. Ayrıntılı açıklamayı burada görebilirsiniz: daha fazla bilgi Için bkz. [Azure RBAC ve Media Services hesapları](rbac-overview.md)

Bu makaledeki örnekte, mevcut ilkeden imzalama anahtarı almak için .NET kullanımı gösterilmektedir. 
 
## <a name="download"></a>İndir 

Aşağıdaki komutu kullanarak, tam .NET örneğini içeren bir GitHub deposunu makinenize kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
Gizli dizileri olan ContentKeyPolicy, [Encryptwithdrm](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/EncryptWithDRM) klasöründe bulunur.

## <a name="get-contentkeypolicy-with-secrets"></a>Gizli dizileri olan ContentKeyPolicy al 

Anahtarı almak için aşağıdaki örnekte gösterildiği gibi **Getpolicypropertieswithsecretsasync** öğesini kullanın.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="next-steps"></a>Sonraki adımlar

[Erişim denetimi ile çoklu DRM'ye sahip içerik koruma sistemi tasarlama](design-multi-drm-system-with-access-control.md) 
