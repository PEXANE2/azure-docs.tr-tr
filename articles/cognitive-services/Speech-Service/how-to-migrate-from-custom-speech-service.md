---
title: Özel Konuşma Tanıma hizmetinden konuşma hizmetine geçiş
titleSuffix: Azure Cognitive Services
description: Özel Konuşma Tanıma hizmeti artık konuşma hizmetinin bir parçasıdır. En son kalite ve özellik güncelleştirmelerinden yararlanmak için konuşma hizmetine geçin.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 9c93286329316d081f8fd99ebd360195931b7b09
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74805935"
---
# <a name="migrate-from-the-custom-speech-service-to-the-speech-service"></a>Özel Konuşma Tanıma hizmetinden konuşma hizmetine geçiş

Uygulamalarınızı Özel Konuşma Tanıma hizmetten konuşma hizmetine geçirmek için bu makaleyi kullanın.

Özel Konuşma Tanıma hizmeti artık konuşma hizmetinin bir parçasıdır. En son kalite ve özellik güncelleştirmelerinden yararlanmak için konuşma hizmetine geçin.

## <a name="migration-for-new-customers"></a>Yeni müşteriler için geçiş

Fiyatlandırma modeli, konuşma hizmeti için saat tabanlı bir fiyatlandırma modeli kullanılarak daha basittir.  

1. Uygulamanızın kullanılabilir olduğu her bölgede bir Azure kaynağı oluşturun. Azure kaynak adı **konuşma**. Tek bir Azure kaynağı için ayrı kaynaklar oluşturmak yerine aynı bölgede aşağıdaki hizmetleri kullanabilirsiniz:

    * Konuşmayı metne dönüştürme
    * Özel Konuşmayı metne dönüştürme
    * Metin okuma
    * Konuşma çevirisi

2. İndirme [konuşma SDK](speech-sdk.md).

3. Hızlı Başlangıç kılavuzları ve SDK'sı örnekleri doğru API'lerini kullanmayı izleyin. REST API'lerini kullanmanız durumunda Ayrıca kaynak anahtarları ve doğru Uç noktalara kullanmanız gerekir.

4. Konuşma hizmetini ve API 'Leri kullanmak için istemci uygulamasını güncelleştirin.

## <a name="migration-for-existing-customers"></a>Mevcut müşteriler için geçiş

Mevcut kaynak anahtarlarınızı konuşma hizmeti portalındaki konuşma hizmetine geçirin. Aşağıdaki adımları kullanın:

> [!NOTE]
> Kaynak anahtarları yalnızca aynı bölge içinde geçirilebilir.

1. Oturum [cris.ai](https://cris.ai/Home/CustomSpeech) Portalı ' nı seçip sağ üst menüdeki abonelikte.

2. Seçin **seçili abonelik geçişi**.

3. Metin kutusuna abonelik anahtarını girin ve seçin **geçirme**.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmetini ücretsiz deneyin](get-started.md).
* Bilgi [Konuşmayı metne dönüştürme](./speech-to-text.md) kavramları.

## <a name="see-also"></a>Ayrıca bkz.

* [Konuşma hizmeti nedir?](overview.md)
* [Konuşma hizmeti ve konuşma SDK belgeleri](speech-sdk.md#get-the-sdk)
