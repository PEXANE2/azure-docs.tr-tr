---
title: SPX-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: SPX, herhangi bir kod yazmadan konuşma hizmetini kullanmaya yönelik bir komut satırı aracıdır. SPX, en düşük düzeyde kurulum gerektirir ve kullanım durumlarınızın karşılanıp karşılanmadığını görmek için konuşma hizmetinin temel özellikleriyle denemeye hemen başlamak kolaydır.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: trbye
ms.openlocfilehash: b473bdc516c59b55eeb44f227352497142a4383b
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83202287"
---
# <a name="what-is-spx"></a>SPX nedir?

SPX, herhangi bir kod yazmadan konuşma hizmetini kullanmaya yönelik bir komut satırı aracıdır. SPX, en az kurulum gerektirir ve kullanım durumlarınızın karşılanıp karşılanmadığını görmek için konuşma hizmetinin temel özellikleriyle denemeye hemen başlamak kolaydır. Dakikalar içinde, bir dosya dizininden Batch konuşma tanıma gibi basit test iş akışlarını çalıştırabilir veya bir dosyadaki dizeler koleksiyonunda metinden konuşmaya dönüştürebilirsiniz. Basit iş akışlarının ötesinde, SPX üretime hazırdır ve otomatikleştirilmiş veya kabuk betikleri kullanılarak daha büyük süreçler çalıştırmak için ölçeklendirilebilir `.bat` .

Konuşma SDK 'sının birincil özelliklerinin büyük bölümü SPX 'de kullanılabilir, ancak bazı gelişmiş özellikler ve özelleştirmeler SPX 'de basitleştirilmiştir. SPX veya SDK ne zaman kullanılacağına karar vermek için aşağıdaki kılavuzu göz önünde bulundurun.

Şu durumlarda SPX kullanın:
* Minimum kurulum ve kod olmadan konuşma hizmeti özellikleriyle denemeler yapmak istiyorsunuz
* Konuşma hizmetini kullanarak bir üretim uygulaması için görece basit gereksinimleriniz vardır

Şu durumlarda SDK 'Yı kullanın:
* Konuşma hizmeti işlevlerini belirli bir dil veya platform (ör. C#, Python, C++) içinde tümleştirmek istiyorsunuz
* Gelişmiş hizmet istekleri gerektirebilecek karmaşık gereksinimleriniz var veya yanıt akışı dahil özel davranış geliştirmiş olabilirsiniz

## <a name="core-features"></a>Temel Özellikler

* Konuşma tanıma-konuşma 'yı ses dosyalarından veya doğrudan bir mikrofondan metne dönüştürün ya da kayıtlı bir konuşma yapın.

* Konuşma senşi-metin dosyalarından giriş veya doğrudan komut satırından giriş kullanarak metin okumayı dönüştürür. [SSML yapılandırmalarının](speech-synthesis-markup.md)ve [Standart ya da sinir seslerinden](speech-synthesis-markup.md#standard-neural-and-custom-voices)yararlanarak konuşma çıkış özelliklerini özelleştirin.

* Konuşma çevirisi-bir kaynak dildeki sesi hedef dilde metne çevirin.

* Azure işlem kaynaklarında Çalıştır-kullanarak bir Azure uzaktan işlem kaynağında çalışacak SPX komutları gönderin `spx webjob` .

## <a name="get-started"></a>Kullanmaya başlayın

SPX ile çalışmaya başlamak için [temel bilgiler makalesine](spx-basics.md)bakın. Bu makalede, bazı temel komutların SPX 'de nasıl çalıştırılacağı ve ayrıca konuşmayı metne dönüştürme ve metinden konuşmaya yönelik toplu işlemler çalıştırmak için biraz daha gelişmiş komutlar gösterilmektedir. Temel bilgiler makalesini okuduktan sonra bazı özel komutlar yazmaya başlamak veya basit konuşma işlemlerini otomatik hale getirmek için SPX sözdizimi hakkında yeterli bilgiye sahip olmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [SPX temel bilgileri](spx-basics.md)
- Kullanım örneği daha karmaşıksa, [konuşma SDK 'sını alın](speech-sdk.md)
