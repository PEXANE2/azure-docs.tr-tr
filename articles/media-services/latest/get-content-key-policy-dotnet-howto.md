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
ms.date: 04/15/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 2dea262fadb61adc9e219b76f9ac048c11e650d5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80065971"
---
# <a name="get-a-signing-key-from-the-existing-policy"></a>Var olan ilkeden imzalama anahtarı alma

v3 API’nin temel tasarım ilkelerinden biri API’yi daha güvenli hale getirmektir. v3 API 'Leri, **Get** veya **list** işlemlerinde gizli dizileri veya kimlik bilgilerini döndürmez. Ayrıntılı açıklamayı burada görebilirsiniz: daha fazla bilgi Için bkz. [RBAC ve Media Services hesapları](rbac-overview.md)

Bu makaledeki örnekte, mevcut ilkeden imzalama anahtarı almak için .NET kullanımı gösterilmektedir. 
 
## <a name="download"></a>İndir 

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
