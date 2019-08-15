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
ms.openlocfilehash: e571c65e64fad73c646aa05366cab685aa745caa
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68968954"
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

1. Portaldan, Visual Interface veya otomatik ML denemeleri gibi bir kod içermeyen seçenek kullanacaksanız, artık bu işlemi tamamladınız. Bir [Not defteri VM](../articles/machine-learning/service/tutorial-1st-experiment-sdk-setup.md#azure) 'si oluşturuyorsanız, bu sanal makine de yapılır. 

1. Yerel ortamınızda bu çalışma alanına başvuruda bulunan kodu kullanmayı planlıyorsanız, çalışma alanının **genel bakış** bölümünden **config. json dosyasını indir** ' i seçin.  

   ![Config.json dosyasını indir](./media/aml-create-in-portal/configure.png)
   
   Dosyayı Python betikleriniz veya Jupyıter Not defterleriniz ile dizin yapısına yerleştirin. Aynı dizinde, *. azureml*adlı bir alt dizin veya bir üst dizin içinde olabilir. Bir not defteri VM oluşturduğunuzda, bu dosya VM 'deki doğru dizine eklenir.

    

