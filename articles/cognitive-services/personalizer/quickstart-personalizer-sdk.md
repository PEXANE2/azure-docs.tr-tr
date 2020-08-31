---
title: 'Hızlı başlangıç: SDK-kişiselleştirici ile öğrenme döngüsü oluşturma ve kullanma'
description: Bu hızlı başlangıçta, kişiselleştirici istemci kitaplığını kullanarak bilgi bankasını oluşturma ve yönetme işlemlerinin nasıl yapılacağı gösterilir.
ms.topic: quickstart
ms.date: 08/27/2020
ms.custom: cog-serv-seo-aug-2020
keywords: kişiselleştirici, Azure kişiselleştirici, makine öğrenimi
zone_pivot_groups: programming-languages-set-six
ms.openlocfilehash: dccdec0888f2968fb7089c4ff80c9338215de135
ms.sourcegitcommit: 420c30c760caf5742ba2e71f18cfd7649d1ead8a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89056002"
---
# <a name="quickstart-personalizer-client-library"></a>Hızlı başlangıç: kişiselleştirici istemci kitaplığı

Bu hızlı başlangıçta kişiselleştirilmiş içeriği kişiselleştirici hizmeti ile görüntüleyin.

Kişiselleştirici istemci kitaplığını kullanmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin.

 * Derecelendirme API 'SI-içerik ve bağlam hakkında sağladığınız gerçek zamanlı bilgileri temel alarak eylemlerden en iyi öğeyi seçer.
 * Ödül API-iş gereksinimlerinize göre ödül puanı belirlenir, ardından bu API ile kişiselleştiriciye gönderebilirsiniz. Bu puan, 1 gibi tek bir değer olabilir ve hatalı için 0 veya iş gereksinimlerinize göre oluşturduğunuz bir algoritmadır.

::: zone pivot="programming-language-csharp"
[!INCLUDE [Get intent with C# SDK](./includes/quickstart-sdk-csharp.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [Get intent with Node.js SDK](./includes/quickstart-sdk-nodejs.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Get intent with Python SDK](./includes/quickstart-sdk-python.md)]
::: zone-end

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kişiselleştirici nasıl kullanılır?](how-personalizer-works.md) 
>  [Kişiselleştirici nerede kullanılır?](where-can-you-use-personalizer.md)
