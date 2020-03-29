---
ms.service: logic-apps
ms.topic: include
author: ecfan
ms.author: estfan
ms.date: 03/02/2018
ms.openlocfilehash: 1aabc1b33722c571c6519b6620fb62211a44c2e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789742"
---
1. Azure [portalında](https://portal.azure.com)boş bir mantık uygulaması oluşturun. 

2. Logic Apps Designer'a filtreniz olarak "github" girin. 

3. GitHub konektörünü ve kullanmak istediğiniz tetikleyiciyi seçin.

   ![GitHub konektörünü ve tetikleyiciyi seçin](./media/connectors-create-api-github/github-connector.png)

   > [!NOTE]
   > Tüm mantık uygulaması iş akışları bir tetikleyici ile başlamalıdır. Eylemleri yalnızca mantık iş akışınız zaten bir tetikleyiciyle başladığında seçebilirsiniz. 

4. Daha önce bir bağlantı oluşturmadıysanız, istendiğinde GitHub kimlik bilgilerinizi sağlayabilmek için **Oturum Aç'ı** seçin.  

   ![GitHub kimlik bilgilerinizle oturum açın](./media/connectors-create-api-github/github-connector-sign-in-credentials.png)

   Mantık uygulamanız, GitHub hesabınıziçin veri bağlamayı ve bunlara erişmeye yetki vermek için bu kimlik bilgilerini kullanır. 

5. GitHub kullanıcı adınızı ve parolanızı girin ve yetkilendirmenizi onaylayın.

   ![Kimlik bilgileri sağlayın ve yetkilendirmeyi onaylayın](./media/connectors-create-api-github/github-connector-authorize.png)   

   Bağlantınız artık Azure portalında oluşturuldu ve kullanıma hazır.

6. Mantık uygulaması iş akışınızı tanımlamaya devam edin.

   ![Mantık uygulaması iş akışınıza daha fazla eylem ekleme](./media/connectors-create-api-github/github-connector-logic-app.png)

