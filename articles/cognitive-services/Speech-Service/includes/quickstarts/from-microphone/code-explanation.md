---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/02/2020
ms.author: trbye
ms.openlocfilehash: fdcb5bb57afe145b57cb1223242d4d5625a2dfb7
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81400694"
---
Konuşma yapılandırma nesnesi oluşturmak için konuşma kaynağı abonelik anahtarı ve bölgesi gereklidir. Yapılandırma nesnesi, bir konuşma tanıyıcı nesnesinin örneğini oluşturmak için gereklidir.

Tanıyıcı örneği, konuşmayı tanımak için birçok yol sunar. Bu örnekte, konuşma bir kez tanınmalıdır. Bu işlevsellik, konuşma hizmetinin tanıma için tek bir tümcecik gönderdiğini ve bu ifadenin konuşmayı tanımayı durdur olarak belirlenmesinin ardından olduğunu bilmesini sağlar. Sonuç sonuçlandırıldıktan sonra kod, konsola tanınma sebebini yazar.

> [!TIP]
> Konuşma SDK 'Sı, dil için kullanarak `en-us` varsayılan olarak tanıma yapılır, kaynak dili seçme hakkında bilgi için bkz. konuşmayı için [kaynak dilini belirtme](../../../how-to-specify-source-language.md) .