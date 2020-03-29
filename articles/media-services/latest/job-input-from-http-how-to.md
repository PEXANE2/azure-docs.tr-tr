---
title: HTTPS URL'sinden Azure Medya Hizmetleri İş girişi oluşturma | Microsoft Dokümanlar
description: Bu konu, BIR HTTPS URL'sinden Azure Medya Hizmetleri İş girişinin nasıl oluşturulabildiğini gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/13/2019
ms.author: juliako
ms.openlocfilehash: a3d4568dd237491f28ae2880bdd78dd236870c3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74899820"
---
# <a name="create-a-job-input-from-an-https-url"></a>HTTPS URL'sinden iş girişi oluşturma

Media Services v3 sürümünde işlenecek İşleri videolarınıza gönderirken Media Services'a giriş videosunun yerini de bildirmeniz gerekir. Seçeneklerden biri, iş girişi olarak bir HTTPS URL'si belirtmektir (bu örnekte gösterildiği gibi). AMS v3’ün şu anda HTTPS URL'leri üzerinden yığın halinde aktarım kodlamasını desteklemediğini unutmayın. Tam bir örnek için bu [GitHub örneğine](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs)bakın.

> [!TIP]
> Geliştirmeye başlamadan önce, [Media Services v3 API'leri ile Geliştirme'yi](media-services-apis-overview.md) gözden geçirin (API'lere erişim, adlandırma kuralları vb. hakkında bilgi içerir)

## <a name="net-sample"></a>.NET örneği

Aşağıdaki kod, HTTPS URL girişi olan bir işin nasıl oluşturulup oluşturulabildiğini gösterir.

[!code-csharp[Main](../../../media-services-v3-dotnet-quickstarts/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="job-error-codes"></a>İş hata kodları

[Bkz. Hata kodları.](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode)

## <a name="next-steps"></a>Sonraki adımlar

[Yerel bir dosyadan iş girişi oluşturun.](job-input-from-local-file-how-to.md)
