---
ms.service: logic-apps
ms.topic: include
author: ecfan
ms.author: estfan
ms.date: 11/03/2016
ms.openlocfilehash: 951ab2300aa4ffed2c5f1039ff993cd7f6af543f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789677"
---
## <a name="prerequisites"></a>Ön koşullar

* Bir Azure hesabı; ücretsiz bir [hesap](https://azure.microsoft.com/free) oluşturabilirsiniz
* [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3) hesabı 

OneDrive hesabınızı bir mantık uygulamasında kullanmadan önce, OneDrive hesabınıza bağlanmak için mantık uygulamasına yetki verin.  Bunu Azure portalındaki mantık uygulamanızda kolayca yapabilirsiniz. 

Aşağıdaki adımları kullanarak OneDrive hesabınıza bağlanması için mantık uygulamanızı yetkilendirmeyin:

1. Bir mantık uygulaması oluşturun. Logic Apps tasarımcısında, açılan listede **Microsoft yönetilen API'leri göster'i** seçin ve ardından arama kutusuna "onedrive" girin. Tetikleyicilerden veya eylemlerden birini seçin:  
   ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. OneDrive'a daha önce herhangi bir bağlantı oluşturmadıysanız, OneDrive kimlik bilgilerinizi kullanarak oturum açmanız istenir:  
   ![](./media/connectors-create-api-onedrive/onedrive-2.png)
3. **Oturum Aç'ı**seçin ve kullanıcı adınızı ve şifrenizi girin. **Oturum Aç'ı**seçin :  
   ![](./media/connectors-create-api-onedrive/onedrive-3.png)   
   
    Bu kimlik bilgileri, mantık uygulamanıza bağlanmak ve OneDrive hesabınızdaki verilere erişmek için yetki vermek için kullanılır. 
4. OneDrive hesabınızı kullanmak için mantık uygulamasını yetkilendirmek için **Evet'i** seçin:  
   ![](./media/connectors-create-api-onedrive/onedrive-4.png)   
5. Bağlantının oluşturulduğuna dikkat edin. Şimdi, mantık uygulamanızdaki diğer adımlara devam edin:  
   ![](./media/connectors-create-api-onedrive/onedrive-5.png)

