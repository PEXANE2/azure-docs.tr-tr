---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: include file
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 02/14/2020
ms.topic: include
ms.author: diberry
ms.openlocfilehash: 155c88ec4766391f70701b17038b915c399d8b0c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77371719"
---
## <a name="sign-in-to-luis-portal"></a>LUIS portalında oturum açın

LUIS'e yeni bir kullanıcının bu yordamı izlemesi gerekir:

1. [LUIS portalında (önizleme)](https://preview.luis.ai)oturum açın, ülkenizi seçin ve kullanım koşullarını kabul edin. Bunun yerine **Uygulamalarımı** görürseniz, bir LUIS kaynağı zaten var ve bir uygulama oluşturmak için ileri atlamanız gerekir.

1. **Azure kaynağı oluştur'u** seçin ve ardından **uygulamalarınızı geçirmek için yazarlık kaynağı oluştur'u seçin.**

    ![Bir dil anlama türü yazma kaynağı seçin](../media/luis-how-to-azure-subscription/sign-in-create-resource.png)

1. Kaynak la ilgili ayrıntıları doldurun.

    ![Yazarlık kaynağı oluşturma](../media/migrate-authoring-key/choose-authoring-resource-form.png)

    **Yeni bir yazarlık kaynağı oluştururken**aşağıdaki bilgileri sağlayın:

    * **Kaynak adı** - seçtiğiniz özel bir ad, yazma ve tahmin bitiş noktası sorguları için URL'nin bir parçası olarak kullanılır.
    * **Kiracı** - Azure aboneliğinizin ilişkili olduğu kiracı.
    * **Abonelik adı** - kaynak için faturalandırılacak abonelik.
    * **Kaynak grubu** - seçtiğiniz veya oluşturduğunuz özel bir kaynak grubu adı. Kaynak grupları, Azure kaynaklarını erişim ve yönetim için gruplandırmanıza olanak tanır.
    * **Konum** - konum seçimi **kaynak grubu** seçimini temel alır.
    * **Fiyatlandırma katmanı** - fiyatlandırma katmanı saniye ve ay başına maksimum işlemi belirler.

1. Oluşturulacak kaynağın bir özeti görüntülenir. **Sonraki'ni**seçin.

    ![Yazarlık kaynağı oluşturma](../media/sign-in/sign-in-confirm-key-selection.png)

1. **Continue'yi**seçerek onayla.

    ![Yazarlık kaynağı oluşturma](../media/sign-in/sign-in-confirm-continue.png)