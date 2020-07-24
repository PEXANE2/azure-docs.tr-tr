---
ms.service: logic-apps
ms.topic: include
author: ecfan
ms.author: estfan
ms.date: 07/21/2020
ms.openlocfilehash: 27df83552b450b7657c0595d1d419001df98148c
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87040237"
---
## <a name="prerequisites"></a>Önkoşullar

* [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3) hesabı 

OneDrive hesabınızı Logic Apps ile kullanabilmeniz için, Azure portal OneDrive hesabınıza bağlanmak üzere Logic Apps yetkilendirmelisiniz.

OneDrive hesabınıza bağlanmak üzere Logic Apps yetkilendirmek için aşağıdaki adımları izleyin:  

1. Azure portalında oturum açın. 

1. **Azure hizmetleri**altında **Logic Apps**' yi seçin. Ardından, listeden mantıksal uygulamanızın adını seçin.

1. Mantıksal uygulamanızın menüsünde, **geliştirme araçları**altında **Logic App Designer** ' ı seçin.

1. Logic Apps tasarımcısında, açılan listeden **Microsoft yönetilen API 'Leri göster** ' i seçin, sonra arama kutusuna *OneDrive* yazın. Kullanılacak tetikleyiciyi veya eylemi seçin:

   ![Eklenecek OneDrive API eylemlerinin listesini gösteren Logic Apps Designer 'ın ekran görüntüsü.](./media/connectors-create-api-onedrive/onedrive-1.png)

2. Daha önce OneDrive ile herhangi bir bağlantı oluşturmadıysanız OneDrive kimlik bilgilerinizi kullanarak oturum açmak için istemi izleyin:  

   ![OneDrive API için oturum açma istemi gösteren Logic Apps Designer 'ın ekran görüntüsü.](./media/connectors-create-api-onedrive/onedrive-2.png)

3. **Oturum aç**' ı seçin ve Kullanıcı adınızı ve parolanızı girin. **Oturum aç '** ı seçin: 

   ![OneDrive API yetkilendirmesi için Microsoft hesabı oturum açma sayfasının ekran görüntüsü.](./media/connectors-create-api-onedrive/onedrive-3.png)   

    Bu kimlik bilgileri, mantıksal uygulamanızı OneDrive hesabınızdaki verilere erişecek şekilde yetkilendirmek için kullanılır. 

4. Mantıksal uygulamayı OneDrive hesabınızı kullanacak şekilde yetkilendirmek için **Evet** ' i seçin:  

   ![İzin verilen eylemleri gösteren Logic Apps için Microsoft hesabı yetkilendirmesi ekran görüntüsü.](./media/connectors-create-api-onedrive/onedrive-4.png)   
   
5. Bağlantı artık adımda listelenmiştir. Kaydet ' i seçin ve ardından mantıksal uygulamanızı oluşturmaya devam edin. 

   ![OneDrive API bağlantısı ile Eylem Düzenleyicisi 'ni gösteren Logic Apps Tasarımcısı 'nın ekran görüntüsü.](./media/connectors-create-api-onedrive/onedrive-5.png)
