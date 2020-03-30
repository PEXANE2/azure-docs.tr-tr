---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 01/30/2019
ms.author: alkohli
ms.openlocfilehash: 94fe099984fae77c65658d7085a8540ff4f2448b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188827"
---
Bu bölümde, Veri Kutusu Ağ Geçidi/Veri Kutusu Kenarı hizmetinde geçerli olduğu gibi Azure Depolama hizmetinin sınırları ve Azure Dosyaları, Azure blok blob'ları ve Azure sayfa lekeleri için gerekli adlandırma kuralları açıklanmaktadır. Depolama sınırlarını dikkatle gözden geçirin ve tüm önerileri uygulayın.

Azure depolama hizmeti sınırları ve paylaşımları, kapsayıcıları ve dosyaları adlandırmak için en iyi uygulamalar hakkında en son bilgiler için şu bilgilere gidin:

- [Kapsayıcıları adlandırma ve başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Paylaşımları adlandırma ve onlara başvurma](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Blobs ve sayfa blob kuralları blok](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> Azure Depolama hizmet sınırlarını aşan veya Azure Files/Blob adlandırma kurallarına uymayan dosyalar veya dizinler varsa, bu dosyalar veya dizinler Veri Kutusu Ağ Geçidi/Veri Kutusu Kenarı hizmeti aracılığıyla Azure Depolama alanına alınmaz.