---
title: "Hızlı başlangıç: REST API 'Leri ile amaç al-LUıS"
description: Bu REST API hızlı başlangıçta, kullanıcının konuşma metinleriyle bir amaç olduğunu öğrenmek için kullanılabilir bir genel LUO uygulaması kullanın.
ms.topic: quickstart
ms.date: 05/18/2020
zone_pivot_groups: programming-languages-set-one
ms.openlocfilehash: 90d98e56e53e28991fb5aada9eab5a7e9c2e69c3
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83654293"
---
# <a name="quickstart-get-intent-with-rest-apis"></a>Hızlı başlangıç: REST API 'Leri ile amaç alın

Bu hızlı başlangıçta, bir kullanıcının konuşma metinden bir amaç olduğunu öğrenmek için bir LUO uygulaması kullanacaksınız. Kullanıcının amaç metnini, pizza uygulamasının HTTP tahmin uç noktasına gönderin. Uç noktada, Lu, doğal dil metnini anlam açısından analiz etmek için pizza uygulamasının modelini uygular, bu da genel amaçlı olarak belirlenir ve uygulamanın konu etki alanıyla ilgili verileri ayıklamaktır.

Bu hızlı başlangıç REST API uç noktasını kullanır. Daha fazla bilgi için bkz: [uç API belgeleri](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78).

Bu makale için ücretsiz bir [LUIS](https://www.luis.ai) hesabınız olması gerekir.

<a name="create-luis-subscription-key"></a>

::: zone pivot="programming-language-csharp"
[!INCLUDE [Get intent with C# and REST](./includes/get-started-get-intent-rest-csharp.md)]
::: zone-end

::: zone pivot="programming-language-go"
[!INCLUDE [Get intent with Go and REST](./includes/get-started-get-intent-rest-go.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Get intent with Java and REST](./includes/get-started-get-intent-rest-java.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [Get intent with Node.js and REST](./includes/get-started-get-intent-rest-nodejs.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Get intent with Python and REST](./includes/get-started-get-intent-rest-python.md)]
::: zone-end
