---
title: Azure Media Services v3 .NET'i kullanarak bir ilkenin imzalama anahtarı nı alma
description: Bu konu, Media Services v3 .NET SDK'yı kullanarak varolan ilkeden imza anahtarının nasıl alınış edildiğini gösterir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80065971"
---
# <a name="get-a-signing-key-from-the-existing-policy"></a>Var olan ilkeden imzalama anahtarı alma

v3 API’nin temel tasarım ilkelerinden biri API’yi daha güvenli hale getirmektir. v3 API'ler **Get** or **List** işlemlerindeki sırları veya kimlik bilgilerini döndürmez. Ayrıntılı açıklamaya buradan bakın: Daha fazla bilgi için [RBAC ve Medya Hizmetleri hesaplarına](rbac-overview.md) bakın

Bu makaledeki örnek, varolan ilkeden bir imzalama anahtarı almak için .NET'in nasıl kullanılacağını gösterir. 
 
## <a name="download"></a>İndirme 

Aşağıdaki komutu kullanarak makinenize tam .NET örneğini içeren bir GitHub deposunu klonla:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
Secrets örnek contentKeyPolicy [EncryptWithDRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/EncryptWithDRM) klasöründe yer almaktadır.

## <a name="get-contentkeypolicy-with-secrets"></a>Secrets ile ContentKeyPolicy alın 

Anahtara ulaşmak için, aşağıdaki örnekte gösterildiği gibi **GetPolicyPropertiesWithSecretsAsync'i**kullanın.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="next-steps"></a>Sonraki adımlar

[Erişim denetimi ile çoklu DRM'ye sahip içerik koruma sistemi tasarlama](design-multi-drm-system-with-access-control.md) 
