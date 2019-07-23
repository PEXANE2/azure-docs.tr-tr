---
title: include dosyası
description: include dosyası
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 07/21/2019
ms.openlocfilehash: 51bd3dfb33b1f445db8672e1b987ee6c6242e09c
ms.sourcegitcommit: c71306fb197b433f7b7d23662d013eaae269dc9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68370884"
---
1. Kullandığınız Azure aboneliğine ait kimlik bilgilerini kullanarak [Azure Portal](https://portal.azure.com/) oturum açın. 

1. Portalın sol üst köşesinde **kaynak oluştur**' u seçin.

   ![Azure portalında kaynak oluşturma](./media/aml-create-in-portal/portal-create-a-resource-07-2019.png)

1. **Machine Learning hizmeti çalışma alanını**bulmak için arama çubuğunu kullanın.

1. **Machine Learning hizmet çalışma alanı**' nı seçin.

1. **Ml hizmeti çalışma alanı** bölmesinde başlamak için **Oluştur** ' u seçin.

1. Yeni çalışma alanınızı, çalışma alanı adı, abonelik, kaynak grubu ve konum sağlayarak yapılandırın.

    ![Çalışma alanı oluşturma](./media/aml-create-in-portal/workspace-create-main-tab.png)

   Alan|Açıklama
   ---|---
   Çalışma alanı adı |Çalışma alanınızı tanımlayan benzersiz bir ad girin. Bu örnekte **docs-WS**kullanıyoruz. Adlar, kaynak grubu genelinde benzersiz olmalıdır. Başkaları tarafından oluşturulan çalışma alanlarını birbirinden ayırmak ve geri çekmek için kolay bir ad kullanın.  
   Subscription |Kullanmak istediğiniz Azure aboneliğini seçin.
   Resource group | Aboneliğinizde var olan bir kaynak grubunu kullanın veya yeni bir kaynak grubu oluşturmak için bir ad girin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları barındırır. Bu örnekte **docs-AML**kullanılır. 
   Location | Çalışma alanınızı oluşturmak için kullanıcılarınıza en yakın konumu ve veri kaynaklarını seçin.

1. Çalışma alanını yapılandırmayı tamamladıktan sonra **Oluştur**' u seçin. 

   Çalışma alanını oluşturmak birkaç dakika sürebilir.

   İşlem tamamlandığında, bir dağıtım başarı iletisi görüntülenir. Ayrıca Bildirimler bölümünde de bulunur. Yeni çalışma alanını görüntülemek için **Kaynağa Git**' i seçin.

   ![Çalışma alanı oluşturma durumu](./media/aml-create-in-portal/notifications.png)
