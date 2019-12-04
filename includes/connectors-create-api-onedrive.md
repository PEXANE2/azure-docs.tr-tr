---
ms.service: logic-apps
ms.topic: include
author: ecfan
ms.author: estfan
ms.date: 11/03/2016
ms.openlocfilehash: 951ab2300aa4ffed2c5f1039ff993cd7f6af543f
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74789677"
---
## <a name="prerequisites"></a>Önkoşullar

* Bir Azure hesabı; [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturabilirsiniz
* [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3) hesabı 

OneDrive hesabınızı bir mantıksal uygulamada kullanabilmeniz için, mantıksal uygulamayı OneDrive hesabınıza bağlamak üzere yetkilendirin.  Bu, Azure portal mantıksal uygulamanız dahilinde kolayca yapabilirsiniz. 

Aşağıdaki adımları kullanarak mantıksal uygulamanızı OneDrive hesabınıza bağlamak için yetkilendirin:

1. Mantıksal uygulama oluşturun. Logic Apps tasarımcısında, açılan listeden **Microsoft yönetilen API 'Leri göster** ' i seçin ve arama kutusuna "OneDrive" yazın. Tetikleyiciler veya eylemlerden birini seçin:  
   ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Daha önce OneDrive ile herhangi bir bağlantı oluşturmadıysanız OneDrive kimlik bilgilerinizi kullanarak oturum açmanız istenir:  
   ![](./media/connectors-create-api-onedrive/onedrive-2.png)
3. **Oturum aç**' ı seçin ve Kullanıcı adınızı ve parolanızı girin. **Oturum aç '** ı seçin:  
   ![](./media/connectors-create-api-onedrive/onedrive-3.png)   
   
    Bu kimlik bilgileri, mantıksal uygulamanızı OneDrive hesabınızdaki verilere bağlanmak ve verilere erişmek üzere yetkilendirmek için kullanılır. 
4. Mantıksal uygulamayı OneDrive hesabınızı kullanacak şekilde yetkilendirmek için **Evet** ' i seçin:  
   ![](./media/connectors-create-api-onedrive/onedrive-4.png)   
5. Bağlantının oluşturulduğuna dikkat edin. Şimdi mantıksal uygulamanızdaki diğer adımlara ilerleyin:  
   ![](./media/connectors-create-api-onedrive/onedrive-5.png)

