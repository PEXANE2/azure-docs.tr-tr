---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/02/2020
ms.author: trbye
ms.openlocfilehash: fdcb5bb57afe145b57cb1223242d4d5625a2dfb7
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400694"
---
Konuşma kaynağı abonelik anahtarı ve bölgesi, konuşma yapılandırma nesnesi oluşturmak için gereklidir. Yapılandırma nesnesi, bir konuşma tanıyıcısı nesneyi anında anileştirmek için gereklidir.

Tanıyıcı örneği, konuşmayı tanımanın birden çok yolunu ortaya çıkarır. Bu örnekte, konuşma bir kez tanınır. Bu işlev, Konuşma hizmetinin tanınmak için tek bir ifade gönderdiğinizi ve ifade tanımlandıktan sonra konuşmayı tanımayı durdurmak için tanımladığını bilmesini sağlar. Sonuç elde edildikten sonra, kod tanıma nedenini konsola yazar.

> [!TIP]
> Konuşma SDK varsayılan dil için `en-us` kullanarak tanıma, kaynak dil seçimi hakkında bilgi [için metin konuşma için kaynak dil belirtin](../../../how-to-specify-source-language.md) bakın.