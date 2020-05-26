---
title: Azure konuşma CLı
titleSuffix: Azure Cognitive Services
description: Konuşma CLı, herhangi bir kod yazmadan konuşma hizmetini kullanmaya yönelik bir komut satırı aracıdır. Konuşma CLı, en düşük düzeyde kurulum gerektirir ve kullanım durumlarınızın karşılanıp karşılanmadığını görmek için konuşma hizmetinin temel özellikleriyle denemeye hemen başlamak kolaydır.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: trbye
ms.openlocfilehash: 3fb0b71cbb82b3b9acad1d1ce093baa86c700a51
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83800332"
---
# <a name="what-is-the-speech-cli"></a>Konuşma CLı nedir?

Konuşma CLı, herhangi bir kod yazmadan konuşma hizmetini kullanmaya yönelik bir komut satırı aracıdır. Konuşma CLı, en az kurulum gerektirir ve kullanım durumlarınızın karşılanıp karşılanmadığını görmek için konuşma hizmetinin temel özellikleriyle denemeye hemen başlamak kolaydır. Dakikalar içinde, bir dosya dizininden Batch konuşma tanıma gibi basit test iş akışlarını çalıştırabilir veya bir dosyadaki dizeler koleksiyonunda metinden konuşmaya dönüştürebilirsiniz. Basit iş akışlarının ötesinde, konuşma CLı, üretime hazırdır ve otomatikleştirilmiş veya kabuk betikleri kullanılarak daha büyük süreçler çalıştırmak için ölçeklendirilebilir `.bat` .

Konuşma SDK 'sının birincil özelliklerinin büyük bölümü konuşma CLı 'da bulunur, ancak bazı gelişmiş özellikler ve özelleştirmeler konuşma CLı 'de basitleştirilmiştir. Konuşma CLı veya konuşma SDK 'sının ne zaman kullanılacağına karar vermek için aşağıdaki kılavuzu göz önünde bulundurun.

Şu durumlarda konuşma CLı 'Yı kullan:
* Minimum kurulum ve kod olmadan konuşma hizmeti özellikleriyle denemeler yapmak istiyorsunuz
* Konuşma hizmetini kullanarak bir üretim uygulaması için görece basit gereksinimleriniz vardır

Şu durumlarda konuşma SDK 'sını kullan:
* Konuşma hizmeti işlevlerini belirli bir dil veya platform (ör. C#, Python, C++) içinde tümleştirmek istiyorsunuz
* Gelişmiş hizmet istekleri gerektirebilecek karmaşık gereksinimleriniz var veya yanıt akışı dahil özel davranış geliştirmiş olabilirsiniz

## <a name="core-features"></a>Temel Özellikler

* Konuşma tanıma-konuşma 'yı ses dosyalarından veya doğrudan bir mikrofondan metne dönüştürün ya da kayıtlı bir konuşma yapın.

* Konuşma senşi-metin dosyalarından giriş veya doğrudan komut satırından giriş kullanarak metin okumayı dönüştürür. [SSML yapılandırmalarının](speech-synthesis-markup.md)ve [Standart ya da sinir seslerinden](speech-synthesis-markup.md#standard-neural-and-custom-voices)yararlanarak konuşma çıkış özelliklerini özelleştirin.

* Konuşma çevirisi-bir kaynak dildeki sesi hedef dilde metne çevirin.

* Azure işlem kaynaklarında Çalıştır-kullanarak bir Azure uzaktan işlem kaynağında çalışacak SPX komutları gönderin `spx webjob` .

## <a name="get-started"></a>başlarken

Konuşma CLı 'yı kullanmaya başlamak için [temel bilgiler makalesine](spx-basics.md)bakın. Bu makalede, bazı temel komutların SPX ile nasıl çalıştırılacağı ve ayrıca konuşmayı metne dönüştürme ve metinden konuşmaya yönelik toplu işlemler çalıştırmak için biraz daha gelişmiş komutlar gösterilmektedir. Temel bilgiler makalesini okuduktan sonra bazı özel komutlar yazmaya başlamak veya basit konuşma işlemlerini otomatik hale getirmek için SPX sözdizimi hakkında yeterli bilgiye sahip olmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma CLı temelleri](spx-basics.md)
- Kullanım örneği daha karmaşıksa, [konuşma SDK 'sını alın](speech-sdk.md)
