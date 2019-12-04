---
ms.service: logic-apps
ms.topic: include
author: ecfan
ms.author: estfan
ms.date: 11/03/2016
ms.openlocfilehash: 635a69fc8e75e3a6249728dc1df31cf839b345c6
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74789618"
---
## <a name="prerequisites"></a>Önkoşullar

* Bir Azure hesabı; [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturabilirsiniz
* [Office 365](https://office365.com) hesabı  

Office 365 hesabınızı bir mantıksal uygulamada kullanmadan önce, mantıksal uygulamayı Office 365 hesabınıza bağlanmak üzere yetkilendirin. Bu, Azure portal mantıksal uygulamanız dahilinde kolayca yapabilirsiniz.  

Aşağıdaki adımları kullanarak mantıksal uygulamanızı Office 365 hesabınıza bağlanmak üzere yetkilendirin:

1. Mantıksal uygulama oluşturun. Logic Apps tasarımcısında, açılan listeden **Microsoft yönetilen API 'Leri göster** ' i seçin ve arama kutusuna "Office 365" yazın. Tetikleyiciler veya eylemlerden birini seçin:  
    ![Office 365 bağlantısı oluşturma adımı](./media/connectors-create-api-office365-outlook/office365-sendemail.png)  
2. Daha önce Office 365 bağlantısı oluşturmadıysanız, Office 365 kimlik bilgilerinizi kullanarak oturum açmanız istenir:  
    ![Office 365 bağlantısı oluşturma adımı](./media/connectors-create-api-office365-outlook/office365-signin.png)  
3. **Oturum aç**' ı seçin ve Kullanıcı adınızı ve parolanızı girin. **Oturum aç '** ı seçin:  
    ![Office 365 bağlantısı oluşturma adımı](./media/connectors-create-api-office365-outlook/office365-usernamepassword.png)
   
    Bu kimlik bilgileri, mantıksal uygulamanızı bağlanmak ve Office 365 hesabınıza erişmek üzere yetkilendirmek için kullanılır. 
4. Bağlantının oluşturulduğuna dikkat edin. Şimdi mantıksal uygulamanızdaki diğer adımlara ilerleyin:   
    ![Office 365 bağlantısı oluşturma adımı](./media/connectors-create-api-office365-outlook/office365-sendemailproperties.png)  

