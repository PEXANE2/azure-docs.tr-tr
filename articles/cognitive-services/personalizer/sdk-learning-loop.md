---
title: 'Hızlı başlangıç: SDK-kişiselleştirici ile öğrenme döngüsü oluşturma ve kullanma'
description: Bu hızlı başlangıçta, istemci SDK 'sını kullanarak bilgi bankaınızı oluşturma ve yönetme işlemlerinin nasıl yapılacağı gösterilir.
ms.topic: quickstart
ms.date: 01/15/2020
zone_pivot_groups: programming-languages-set-six
ms.openlocfilehash: e09476bc084465cf08087a3200d8b7d663b0685e
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76122911"
---
# <a name="quickstart-personalizer-client-library"></a>Hızlı başlangıç: kişiselleştirici istemci kitaplığı

Kişiselleştirilmiş içeriği bu Python hızlı penceresinde kişiselleştirici hizmeti ile görüntüleyin.

Kişiselleştirici istemci kitaplığını kullanmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin.

 * Derecelendirme API 'SI-içerik ve bağlam hakkında sağladığınız gerçek zamanlı bilgilere göre içerik öğelerinden en iyi öğeyi seçer.
 * Ödül API-iş gereksinimlerinize göre ödül puanı belirlenir, ardından bu API ile kişiselleştiriciye gönderebilirsiniz. Bu puan, 1 gibi tek bir değer olabilir ve hatalı için 0 veya iş gereksinimlerinize göre oluşturduğunuz bir algoritmadır.

::: zone pivot="programming-language-csharp"
[!INCLUDE [Get intent with C# SDK](./includes/quickstart-sdk-csharp.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Get intent with Python SDK](./includes/quickstart-sdk-python.md)]
::: zone-end

::: zone pivot="programming-language-nodejs"
[!INCLUDE [Get intent with Node.js SDK](./includes/quickstart-sdk-nodejs.md)]
::: zone-end

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, onunla ilişkili diğer tüm kaynakları da siler.

* [Portal](../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[Kişiselleştirici nasıl kullanılır?](how-personalizer-works.md)

* [Kişiselleştirici nedir?](what-is-personalizer.md)
* [Kişiselleştirici 'yi nerede kullanabilirsiniz?](where-can-you-use-personalizer.md)
* [Sorun giderme](troubleshooting.md)
* Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/quickstarts/python/sample.py)' da bulunabilir.
