---
title: 'Hızlı başlangıç: SDK-kişiselleştirici ile öğrenme döngüsü oluşturma ve kullanma'
description: Bu hızlı başlangıçta, istemci SDK 'sını kullanarak bilgi bankaınızı oluşturma ve yönetme işlemlerinin nasıl yapılacağı gösterilir.
ms.topic: quickstart
ms.date: 07/30/2020
ms.custom: devx-track-python, devx-track-javascript
zone_pivot_groups: programming-languages-set-six
ms.openlocfilehash: 76504da214a3777dba7b2356bea160e86999bb45
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88207541"
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
>[Kişiselleştirme nasıl çalışır?](how-personalizer-works.md)

* [Kişiselleştirme nedir?](what-is-personalizer.md)
* [Kişiselleştirme’yi nerelerde kullanabilirsiniz?](where-can-you-use-personalizer.md)
* [Sorun giderme](troubleshooting.md)
* Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/python/Personalizer)' da bulunabilir.
