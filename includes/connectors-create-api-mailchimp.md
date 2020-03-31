---
ms.service: logic-apps
ms.topic: include
author: ecfan
ms.author: estfan
ms.date: 11/03/2016
ms.openlocfilehash: a7290e46a0c5e228689dd1f1a0dc7f9a1182ad60
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789627"
---
## <a name="prerequisites"></a>Ön koşullar

* Bir [MailChimp](https://www.MailChimp.com/) hesabı 

Bir Logic uygulamasında MailChimp hesabınızı kullanabilmek için, MailChimp hesabınıza bağlanmak için Mantık uygulamasını yetkilendirmeniz gerekir. Neyse ki, bunu Azure Portalı'ndaki Logic uygulamanızın içinden kolayca yapabilirsiniz. 

MailChimp hesabınıza bağlanmak için Mantık uygulamanızı yetkilendirme adımları şunlardır:

1. Logic uygulama tasarımcısında MailChimp'e bağlantı oluşturmak için açılan listede **Microsoft yönetilen API'leri göster'i** seçin ve ardından arama kutusuna *MailChimp* girin. Kullanmak istediğiniz tetikleyiciyi veya eylemi seçin:  
   ![MailChimp adım 1](./media/connectors-create-api-mailchimp/mailchimp-1.png)
2. Daha önce MailChimp ile herhangi bir bağlantı oluşturmadıysanız, MailChimp kimlik bilgilerinizi vermeniz istenir. Bu kimlik bilgileri, Logic uygulamanıza bağlanmak ve MailChimp hesabınızın verilerine erişmek için yetki vermek için kullanılacaktır:  
   ![MailChimp adım 2](./media/connectors-create-api-mailchimp/mailchimp-2.png)
3. Logic uygulamanızı yetkilendirmek için MailChimp kullanıcı adınızı ve şifrenizi girin:  
   ![MailChimp adım 3](./media/connectors-create-api-mailchimp/mailchimp-3.png)   
4. Bağlantının oluşturulduğuna dikkat edin ve artık Logic uygulamanızdaki diğer adımlara devam etmekte özgürolabilirsiniz:  
   ![MailChimp adım 4](./media/connectors-create-api-mailchimp/mailchimp-4.png)

