---
title: include dosyası
description: include dosyası
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 02/20/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: ace42278269ff6af31902dbecead81329815af12
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67188607"
---
## <a name="create-a-topic-using-the-azure-portal"></a>Azure portalını kullanarak konu oluşturma
1. **Service Bus ad alanı** sayfasında sol menüdeki **konular** ' ı seçin.
2. Araç çubuğunda **+ konu başlığını** seçin. 
4. Konu için bir **ad** girin. Diğer seçenekleri varsayılan değerlerinde bırakın.
5. **Oluştur**’u seçin.

    ![Konu Oluştur](./media/service-bus-create-topics-subscriptions-portal/create-topic.png)

## <a name="create-subscriptions-to-the-topic"></a>Konuya abonelikler oluşturma
1. Önceki bölümde oluşturduğunuz **konuyu** seçin. 
    
    ![Konu seçin](./media/service-bus-create-topics-subscriptions-portal/select-topic.png)
2. **Service Bus konu** sayfasında, sol menüden **abonelikler** ' i seçin ve ardından araç çubuğunda **+ aboneliği** ' ni seçin. 
    
    ![Abonelik Ekle düğmesi](./media/service-bus-create-topics-subscriptions-portal/add-subscription-button.png)
3. **Abonelik oluştur** sayfasında, abonelik **adı** için **S1** girin ve ardından **Oluştur**' u seçin. 

    ![Abonelik Oluştur sayfası](./media/service-bus-create-topics-subscriptions-portal/create-subscription-page.png)
4. **S2** ve **S3**adlı abonelikler oluşturmak için önceki adımı iki kez tekrarlayın.