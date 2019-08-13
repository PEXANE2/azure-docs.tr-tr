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
ms.date: 07/31/2019
ms.openlocfilehash: 0a497ce506e1aa3b0f9afc47bf1ab8382c9c0405
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68951456"
---
1. Kullandığınız Azure aboneliğine ait kimlik bilgilerini kullanarak [Azure Portal](https://portal.azure.com/) oturum açın. 

1. Azure portal sol üst köşesinde **kaynak oluştur**' u seçin.

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

1. Kodunuz yerel ortamınızdan bu çalışma alanına başvuruyorsa, çalışma alanının **genel bakış** bölümünde **config. json dosyasını indir** ' i seçin.  

   ![Config.json dosyasını indir](./media/aml-create-in-portal/configure.png)

    Bu JSON dosyasını Python betiklerinizi veya Jupyıter not defterlerini içeren dizin yapısına yerleştirin. Aynı dizinde, *. azureml*adlı bir alt dizin veya bir üst dizin içinde olabilir.

   Bir [Not defteri sanal makinesi](../articles/machine-learning/service/tutorial-1st-experiment-sdk-setup.md#azure) kullanacaksanız, bu dosyayı indirmeniz gerekmez, sanal makine üzerinde doğru dizine eklenir.
