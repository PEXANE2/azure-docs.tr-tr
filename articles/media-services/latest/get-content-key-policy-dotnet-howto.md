---
title: Azure Media Services v3 .NET kullanarak bir ilkeden imzalama anahtarı alın
description: Bu konuda, Media Services v3 .NET SDK kullanılarak mevcut ilkeden nasıl imza anahtarı alınacağı gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 04/15/2019
ms.author: juliako
ms.openlocfilehash: ff27ae0fd639316f03fe89ffc906561b3ef85f6f
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76515080"
---
# <a name="get-a-signing-key-from-the-existing-policy"></a>Mevcut ilkeden bir imzalama anahtarı al

v3 API’nin temel tasarım ilkelerinden biri API’yi daha güvenli hale getirmektir. v3 API 'Leri, **Get** veya **list** işlemlerinde gizli dizileri veya kimlik bilgilerini döndürmez. Ayrıntılı açıklamayı burada görebilirsiniz: daha fazla bilgi Için bkz. [RBAC ve Media Services hesapları](rbac-overview.md)

Bu makaledeki örnekte, mevcut ilkeden imzalama anahtarı almak için .NET kullanımı gösterilmektedir. 
 
## <a name="download"></a>İndirin 

Aşağıdaki komutu kullanarak, tam .NET örneğini içeren bir GitHub deposunu makinenize kopyalayın:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
Gizli dizileri olan ContentKeyPolicy, [Encryptwithdrm](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/EncryptWithDRM) klasöründe bulunur.

## <a name="get-contentkeypolicy-with-secrets"></a>Gizli dizileri olan ContentKeyPolicy al 

Anahtarı almak için aşağıdaki örnekte gösterildiği gibi **Getpolicypropertieswithsecretsasync**öğesini kullanın.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="next-steps"></a>Sonraki adımlar

[Erişim denetimi ile çoklu DRM'ye sahip içerik koruma sistemi tasarlama](design-multi-drm-system-with-access-control.md) 
