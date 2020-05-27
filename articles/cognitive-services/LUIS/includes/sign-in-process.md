---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: include file
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 05/05/2020
ms.topic: include
ms.author: diberry
ms.openlocfilehash: ef7208596ead8f7d3903bb614ccb980df782e581
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83837928"
---
## <a name="sign-in-to-luis-portal"></a>Lug portalında oturum açma

LUBUNA yeni bir kullanıcının bu yordamı izlemesi gerekir:

1. [Luo portalında](https://www.luis.ai)oturum açın, ülkenizi/bölgenizi seçin ve kullanım koşullarını kabul edin. Bunun yerine **uygulamalarımı** görürseniz, bir Luo kaynağı zaten mevcuttur ve bir uygulama oluşturmak için bir atlama yapmanız gerekir. Desteklenen bölgeler için [yazma ve yayımlama bölgelerini ve ilişkili anahtarları](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions)ziyaret edin.

1. **Azure kaynağı oluştur** ' u seçin ve ardından **uygulamalarınızı geçirmek Için yazma kaynağı oluştur** ' u seçin.

    ![Language Understanding yazma kaynağı türünü seçin](../media/luis-how-to-azure-subscription/sign-in-create-resource.png)

1. Kaynağın ayrıntılarını girin.

    ![Yazma kaynağı oluşturma](../media/migrate-authoring-key/choose-authoring-resource-form.png)

    **Yeni bir yazma kaynağı oluştururken**aşağıdaki bilgileri sağlayın:

    * **Kaynak adı** -seçtiğiniz özel bir ad, yazma ve tahmin uç noktası sorgularınız için URL 'nin bir parçası olarak kullanılır.
    * **Kiracı** -Azure aboneliğinizin ilişkilendirildiği kiracı.
    * **Abonelik adı** -kaynak için faturalandırılacak abonelik.
    * **Kaynak grubu** -seçtiğiniz veya oluşturduğunuz özel bir kaynak grubu adı. Kaynak grupları, erişim ve yönetim için Azure kaynaklarını gruplandıreklemenize olanak tanır.
    * **Konum** -konum seçimi, **kaynak grubu** seçimini temel alır.
    * **Fiyatlandırma katmanı** -fiyatlandırma katmanı, saniye başına en fazla işlemi ve ayı belirler.

1. Oluşturulacak kaynağın bir özeti görüntülenir. **İleri**’yi seçin.

    ![Yazma kaynağı oluşturma](../media/sign-in/sign-in-confirm-key-selection.png)

1. **Devam**' i seçerek onaylayın.

    ![Yazma kaynağı oluşturma](../media/sign-in/sign-in-confirm-continue.png)
