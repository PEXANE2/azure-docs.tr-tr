---
ms.service: logic-apps
ms.topic: include
author: ecfan
ms.author: estfan
ms.date: 07/21/2020
ms.openlocfilehash: 06a503e800309d8689735417931820d9f16a6e75
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87103076"
---
## <a name="prerequisites"></a>Önkoşullar

* [OneDrive iş](https://OneDrive.com) hesabı 

OneDrive Iş hesabınızı Logic Apps ile kullanabilmeniz için önce Azure portal OneDrive Iş hesabınıza bağlanmak üzere Logic Apps yetkilendirmelisiniz.

OneDrive Iş hesabınıza bağlanmak üzere Logic Apps yetkilendirmek için aşağıdaki adımları izleyin:  

1. Azure portalında oturum açın. 

1. **Azure hizmetleri**altında **Logic Apps**' yi seçin. Ardından, listeden mantıksal uygulamanızın adını seçin.

1. Mantıksal uygulamanızın menüsünde, **geliştirme araçları**altında **Logic App Designer** ' ı seçin.

1. Logic Apps tasarımcısında, açılan listeden **Microsoft yönetilen API 'Leri göster** ' i seçin, sonra arama kutusuna *OneDrive iş* girin. Kullanılacak tetikleyiciyi veya eylemi seçin:  

   ![OneDrive Iş API 'SI eylemleriyle yineleme tetikleyicisi gösteren Logic Apps tasarımcısının ekran görüntüsü.](./media/connectors-create-api-onedriveforbusiness/onedriveforbusiness-1.png)

2. Daha önce OneDrive Iş ile herhangi bir bağlantı oluşturmadıysanız, OneDrive Iş kimlik bilgilerinizi sağlamak için istemi izleyin. Bu kimlik bilgileri, mantıksal uygulamanızı OneDrive Iş hesabınızın verilerine erişmek üzere yetkilendirmek için kullanılır:  

   ![OneDrive Iş için oturum açma istemi gösteren Logic Apps Designer 'ın ekran görüntüsü.](./media/connectors-create-api-onedriveforbusiness/onedriveforbusiness-2.png)

3. Mantıksal uygulamanızı yetkilendirmek için OneDrive Iş Kullanıcı adınızı ve parolanızı sağlayın:  

   ![OneDrive Iş oturum açma sayfasının, oturum açma istemi gösteren ekran görüntüsü.](./media/connectors-create-api-onedriveforbusiness/onedriveforbusiness-3.png)   

4. Bağlantı artık adımda listelenmiştir. Kaydet ' i seçin ve ardından mantıksal uygulamanızı oluşturmaya devam edin. 

   ![Logic Apps Tasarımcısı 'nın, OneDrive Iş bağlantısı olan tetikleyiciyi listelenmiş şekilde gösterildiği ekran görüntüsü.](./media/connectors-create-api-onedriveforbusiness/onedriveforbusiness-4.png)   
