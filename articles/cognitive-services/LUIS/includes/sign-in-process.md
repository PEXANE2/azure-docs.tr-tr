---
title: include dosyası
description: include dosyası
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 05/05/2020
ms.topic: include
ms.openlocfilehash: fda9df6c7e9651bbd3b0b70ad9d47f23c0c19d01
ms.sourcegitcommit: f5580dd1d1799de15646e195f0120b9f9255617b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2020
ms.locfileid: "91541449"
---
## <a name="sign-in-to-luis-portal"></a>LUIS portalında oturum açma

LUBUNA yeni bir kullanıcının bu yordamı izlemesi gerekir:

1. [Luo portalında](https://www.luis.ai)oturum açın, ülkenizi/bölgenizi seçin ve kullanım koşullarını kabul edin. Bunun yerine **uygulamalarımı** görürseniz, bir Luo kaynağı zaten mevcuttur ve bir uygulama oluşturmak için bir atlama yapmanız gerekir. Desteklenen bölgeler için [yazma ve yayımlama bölgelerini ve ilişkili anahtarları](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions)ziyaret edin.

1. **Azure kaynağı oluştur** ' u seçin ve ardından **uygulamalarınızı geçirmek Için yazma kaynağı oluştur** ' u seçin.

    ![Language Understanding yazma kaynağı türünü seçin](../media/luis-how-to-azure-subscription/sign-in-create-resource.png)

1. Kaynağın ayrıntılarını girin.

    ![Ekran görüntüsü yazma için yeni kaynak oluştur bölmesini gösterir.](../media/migrate-authoring-key/choose-authoring-resource-form.png)

    **Yeni bir yazma kaynağı oluştururken**aşağıdaki bilgileri sağlayın:

    * **Kaynak adı** -seçtiğiniz özel bir ad, yazma ve tahmin uç noktası sorgularınız için URL 'nin bir parçası olarak kullanılır.
    * **Kiracı** -Azure aboneliğinizin ilişkilendirildiği kiracı.
    * **Abonelik adı** -kaynak için faturalandırılacak abonelik.
    * **Kaynak grubu** -seçtiğiniz veya oluşturduğunuz özel bir kaynak grubu adı. Kaynak grupları, erişim ve yönetim için Azure kaynaklarını gruplandıreklemenize olanak tanır.
    * **Konum** -konum seçimi, **kaynak grubu** seçimini temel alır.
    * **Fiyatlandırma katmanı** -fiyatlandırma katmanı, saniye başına en fazla işlemi ve ayı belirler.

1. Oluşturulacak kaynağın bir özeti görüntülenir. **İleri**’yi seçin.

    ![Ekran görüntüsü, Azure hesabınıza bağlanma seçeneğiyle birlikte hoş geldiniz sayfasını gösterir.](../media/sign-in/sign-in-confirm-key-selection.png)

1. **Devam**' i seçerek onaylayın.

    ![Ekran görüntüsü, Azure hesabınıza bağlandıktan sonra hoş geldiniz sayfasını gösterir.](../media/sign-in/sign-in-confirm-continue.png)
