---
title: "Hızlı başlangıç: REST API 'Leri kullanarak arama dizini oluşturma"
titleSuffix: Azure Cognitive Search
description: Bu REST API hızlı başlangıçta, Postman veya Visual Studio Code kullanarak Azure Bilişsel Arama REST API 'Lerinin nasıl çağrılacağını öğrenin.
zone_pivot_groups: URL-test-interface-rest-apis
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: quickstart
ms.devlang: rest-api
ms.date: 11/17/2020
ms.openlocfilehash: 971ee806d8b5f9a336b40d96d500ce47d2f5166e
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94714216"
---
# <a name="quickstart-create-an-azure-cognitive-search-index-using-rest-apis"></a>Hızlı başlangıç: REST API 'Leri kullanarak Azure Bilişsel Arama dizini oluşturma

Bu makalede, istekleri göndermek ve almak için [Azure BILIŞSEL arama REST API 'lerini](/rest/api/searchservice) ve API istemcisini kullanarak REST API isteklerini etkileşimli olarak nasıl ifade edeceğinizi açıklanmaktadır. Bir API istemcisiyle bu yönergelerle, herhangi bir kod yazmadan önce istek gönderebilir ve yanıtları görüntüleyebilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

::: zone pivot="url-test-tool-rest-postman"

[!INCLUDE [Send requests using Postman](includes/search-get-started-rest-postman.md)]

::: zone-end

::: zone pivot="url-test-tool-rest-vscode-ext"

[!INCLUDE [Send requests using Visual Studio Code](includes/search-get-started-rest-vscode-ext.md)]

::: zone-end

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol gezinti bölmesindeki **tüm kaynaklar** veya **kaynak grupları** bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç Dizin, Dizin Oluşturucu ve veri kaynağı ile sınırlı olduğunu unutmayın. Sınırın altında kalmak için portalda ayrı ayrı öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Temel görevlerin nasıl gerçekleştirileceğini öğrenmiş olduğunuza göre, Dizin oluşturucular veya dizin oluşturma için içerik dönüştürmeleri ekleyen [bir zenginleştirme işlem hattı ayarlama](cognitive-search-tutorial-blob.md) gibi daha gelişmiş özellikler için daha ileri bir REST API çağrısı yapabilirsiniz. Sonraki adımınız için aşağıdaki bağlantıyı öneririz:

> [!div class="nextstepaction"]
> [Öğretici: Azure Bloblarından aranabilir içerik oluşturmak için REST ve AI kullanma](cognitive-search-tutorial-blob.md)