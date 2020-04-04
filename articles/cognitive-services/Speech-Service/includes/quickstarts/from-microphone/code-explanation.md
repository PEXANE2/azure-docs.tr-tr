---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 04/02/2020
ms.author: dapine
ms.openlocfilehash: 3125b6203f04b4cabd8cd0822a97317185cbf194
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80638118"
---
Konuşma kaynağı abonelik anahtarı ve bölgesi, konuşma yapılandırma nesnesi oluşturmak için gereklidir. Yapılandırma nesnesi, bir konuşma tanıyıcısı nesneyi anında anileştirmek için gereklidir.

Tanıyıcı örneği, konuşmayı tanımanın birden çok yolunu ortaya çıkarır. Bu örnekte, konuşma bir kez tanınır. Bu işlev, Konuşma hizmetinin tanınmak için tek bir ifade gönderdiğinizi ve ifade tanımlandıktan sonra konuşmayı tanımayı durdurmak için tanımladığını bilmesini sağlar. Sonuç elde edildikten sonra, kod tanıma nedenini konsola yazar.

> [!TIP]
> Konuşma SDK varsayılan dil için `en-us` kullanarak tanıma, kaynak dil seçimi hakkında bilgi [için metin konuşma için kaynak dil belirtin](../../../how-to-specify-source-language.md) bakın.