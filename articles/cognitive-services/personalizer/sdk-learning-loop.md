---
title: 'Quickstart: SDK ile öğrenme döngüsü oluşturun ve kullanın - Personalizer'
description: Bu hızlı başlangıç, istemci SDK'yı kullanarak bilgi tabanınızı nasıl oluşturup yöneteceklerinizi gösterir.
ms.topic: quickstart
ms.date: 01/15/2020
zone_pivot_groups: programming-languages-set-six
ms.openlocfilehash: 7ebe22227b4323b2e6b1c3fc9ca31e171d1d97cd
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77524878"
---
# <a name="quickstart-personalizer-client-library"></a>Quickstart: Personalizer istemci kitaplığı

Kişiselleştirilmiş içeriği Personalizer hizmetiyle bu hızlı başlangıçta görüntüleyin.

Personalizer istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin.

 * Rank API - İçerik ve bağlam hakkında sağladığınız gerçek zamanlı bilgilere dayanarak içerik öğelerinden en iyi öğeyi seçer.
 * Ödül API - Ödül puanını işletme gereksinimlerinize göre belirler, ardından bu API ile Personalizer'a gönderirsiniz. Bu puan, iyi için 1 ve kötü için 0 gibi tek bir değer veya iş gereksinimlerinize göre oluşturduğunuz bir algoritma olabilir.

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

Bilişsel Hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynak veya kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, bununla ilişkili diğer kaynakları da siler.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[Kişiselleştirme nasıl çalışır?](how-personalizer-works.md)

* [Kişiselleştirme nedir?](what-is-personalizer.md)
* [Kişiselleştirme’yi nerelerde kullanabilirsiniz?](where-can-you-use-personalizer.md)
* [Sorun giderme](troubleshooting.md)
* Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/quickstarts/python/sample.py)bulunabilir.
