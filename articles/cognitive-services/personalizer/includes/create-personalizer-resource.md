---
title: dosya dahil etme
description: dosya dahil etme
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: include
ms.custom: include file
ms.date: 01/15/2020
ms.author: diberry
ms.openlocfilehash: b1013b261e9449aef45f629c729579f4c87c1f6b
ms.sourcegitcommit: bf8c447dada2b4c8af017ba7ca8bfd80f943d508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85378573"
---
## <a name="create-a-personalizer-azure-resource"></a>Bir kişiselleştirici Azure kaynağı oluşturma

Yerel makinenizde [Azure Portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) veya [Azure CLI](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) kullanarak kişiselleştirici için bir kaynak oluşturun. 

Kaynağından bir anahtar aldıktan sonra, iki [ortam değişkeni](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)oluşturun:

* `PERSONALIZER_RESOURCE_KEY`Kaynak anahtarı için.
* `PERSONALIZER_RESOURCE_ENDPOINT`Kaynak uç noktası için.

Azure portal, hem anahtar hem de uç nokta değerleri **hızlı başlangıç** sayfasından kullanılabilir.
