---
title: include dosyası
description: include dosyası
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 10/30/2020
ms.topic: include
ms.openlocfilehash: e592c11062e81d48014a90895a0e42b460d4b77c
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93128243"
---
## <a name="sign-in-to-luis-portal"></a>LUIS portalında oturum açma

LUBUNA yeni bir kullanıcının bu yordamı izlemesi gerekir:

1. [Luu portalında](https://www.luis.ai)oturum açın, ülkenizi/bölgenizi seçin ve kullanım koşullarını kabul edin. Bunun yerine **uygulamalarımı** görürseniz, bir Luo kaynağı zaten mevcuttur ve bir uygulama oluşturmak için bir atlama yapmanız gerekir. Kaydolan iki seçeneğe sahip olursunuz:

    * Azure kaynağı kullanarak (önerilir), Lua hesabınızı yeni veya mevcut bir Azure yazma kaynağıyla bağlantılandırın. Bu, zaten geçirilmiş imza ile eşdeğerdir. Üzerinde [geçiş işlemini](../luis-migration-authoring.md#what-is-migration) daha sonra ilermenize gerek kalmaz.

    * Deneme anahtarı kullanma. Bu, BASıS 'de ayarlamanız gerekmeyen bir deneme kaynağı ile oturum açmanıza olanak tanır. Bu seçeneği belirlerseniz, sonunda [Hesabınızı geçirmeniz](../luis-migration-authoring.md#migration-steps) ve uygulamalarınızı bir yazma kaynağına bağlamanız gerekecektir.

1. Görüntülenen **bir yazma penceresi seçin** penceresinde, Azure ABONELIĞINIZI ve lusıs yazma kaynağını bulun. Bir kaynağınız yoksa yeni bir tane oluşturabilirsiniz.

    :::image type="content" source="../media/luis-how-to-azure-subscription/choose-authoring-resource.png" alt-text="Language Understanding yazma kaynağı türünü seçin.":::
    
    Yeni bir yazma kaynağı oluşturduğunuzda, aşağıdaki bilgileri sağlayın:
    * **Kiracı adı** -Azure aboneliğinizin ilişkilendirildiği kiracı.
    * **Azure abonelik adı** -kaynak için faturalandırılacak abonelik.
    * **Azure Kaynak grubu adı** -seçtiğiniz veya oluşturduğunuz özel bir kaynak grubu adı. Kaynak grupları, erişim ve yönetim için Azure kaynaklarını gruplandıreklemenize olanak tanır.
    * **Azure Kaynak adı** -seçtiğiniz özel bir ad, yazma ve tahmin uç noktası sorgularının URL 'sinin bir parçası olarak kullanılır.
    * **Fiyatlandırma katmanı** -fiyatlandırma katmanı, saniye başına en fazla işlemi ve ayı belirler.


