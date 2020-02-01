---
title: Azure Media Services API 'sine erişin-Azure CLı | Microsoft Docs
description: Azure Media Services API 'sine erişmek için bu nasıl yapılır makalesine yönelik adımları izleyin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: mvc
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 06ca26ee9a8af7f49aba6a48a1831d30d5886851
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76896344"
---
# <a name="access-azure-media-services-api-with-the-azure-cli"></a>Azure CLı ile Azure Media Services API 'sine erişme
 
Azure Media Services API 'sine bağlanmak için Azure AD hizmet sorumlusu kimlik doğrulamasını kullanmak üzere, uygulamanızın aşağıdaki parametrelere sahip bir Azure AD belirteci istemesi gerekir:

* Azure AD kiracı uç noktası
* Media Services kaynak URI 'SI
* REST Media Services için kaynak URI 'SI
* Azure AD uygulama değerleri: istemci KIMLIĞI ve istemci parolası

> [!IMPORTANT]
> Ayrıntılı açıklama için [Media Services v3 API 'Lerine erişimi](media-services-apis-overview.md#accessing-the-azure-media-services-api)gözden geçirin.

Bu makalede, Azure CLı kullanarak bir Azure AD uygulaması ve hizmet sorumlusu oluşturma ve Azure Media Services kaynaklarına erişmek için gereken değerleri alma işlemlerinin nasıl yapılacağı gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar 

[Bir Media Services hesabı oluşturma](create-account-cli-how-to.md).

Kaynak grubu adı ve Media Services hesap adı için kullandığınız değerleri anımsadığınızdan emin olun.
 
[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="see-also"></a>Ayrıca bkz.

- [Ölçek medya ayrılmış birimleri-CLı](media-reserved-units-cli-how-to.md)
- [Media Services hesabı oluşturma-CLı](create-account-cli-how-to.md) 
- [Hesap kimlik bilgilerini sıfırlama-CLı](cli-reset-account-credentials.md)
- [Varlık oluşturma-CLı](cli-create-asset.md)
- [Dosya yükleme-CLı](cli-upload-file-asset.md)
- [Dönüşümler oluşturma-CLı](cli-create-transform.md)
- [Özel dönüşümle kodlama-CLı](custom-preset-cli-howto.md)
- [İş oluşturma-CLı](cli-create-jobs.md)
- [EventGrid oluşturma-CLı](job-state-events-cli-how-to.md)
- [Varlık yayımlama-CLı](cli-publish-asset.md)
- [Filtre-CLı](filters-dynamic-manifest-cli-howto.md)
- [Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)

## <a name="next-steps"></a>Sonraki adımlar

İçerik akışı yapmak istediğiniz akış uç noktasının çalışıyor durumda olması gerekir. Aşağıdaki CLı komutu varsayılan akış uç noktanızı başlatır:

`az ams streaming-endpoint start -n default -a <amsaccount> -g <amsResourceGroup>`

