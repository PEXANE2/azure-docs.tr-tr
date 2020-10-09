---
title: dosya dahil etme
description: dosya dahil etme
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 11/04/2019
ms.openlocfilehash: f5f132d257e30cd8f4fa1153087bf0df2f0f5b2c
ms.sourcegitcommit: b87c7796c66ded500df42f707bdccf468519943c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91841873"
---
1. Azure aboneliğinizin kimlik bilgilerini kullanarak [Azure Portal](https://portal.azure.com/) oturum açın.

1. Azure portal sol üst köşesinde **+ kaynak oluştur**' u seçin.

    ![Kaynak oluştur seçeneğini gösteren ekran görüntüsü.](media/aml-create-in-portal/create-workspace.gif)

1. **Machine Learning**bulmak için arama çubuğunu kullanın.

1. **Machine Learning**seçin.

1. Başlamak için **Machine Learning** bölmesinde **Oluştur** ' u seçin.

1. Yeni çalışma alanınızı yapılandırmak için aşağıdaki bilgileri sağlayın:

   Alan|Açıklama
   ---|---
   Çalışma alanı adı |Çalışma alanınızı tanımlayan benzersiz bir ad girin. Bu örnekte **docs-WS**kullanıyoruz. Adlar, kaynak grubu genelinde benzersiz olmalıdır. Başkaları tarafından oluşturulan çalışma alanlarını birbirinden ayırmak ve geri çekmek için kolay bir ad kullanın.
   Abonelik |Kullanmak istediğiniz Azure aboneliğini seçin.
   Kaynak grubu | Aboneliğinizde var olan bir kaynak grubunu kullanın veya yeni bir kaynak grubu oluşturmak için bir ad girin. Kaynak grubu, bir Azure çözümü için ilgili kaynakları barındırır. Bu örnekte **docs-AML**kullanılır. 
   Konum | Çalışma alanınızı oluşturmak için kullanıcılarınıza en yakın konumu ve veri kaynaklarını seçin.
   Çalışma alanı sürümü | Bu öğretici için çalışma alanı türü olarak **temel** ' yı seçin. Çalışma alanı türü, erişim ve fiyatlandırmaya sahip olduğunuz özellikleri belirler. Bu öğreticideki her şey, temel veya kurumsal çalışma alanıyla gerçekleştirilebilir.

1. Çalışma alanını yapılandırmayı tamamladıktan sonra, **gözden geçir + oluştur**' u seçin.

   > [!Warning]
   > Çalışma alanınızı bulutta oluşturmak birkaç dakika sürebilir.

   İşlem tamamlandığında, bir dağıtım başarı iletisi görüntülenir.
 
 1. Yeni çalışma alanını görüntülemek için **Kaynağa Git**' i seçin.

