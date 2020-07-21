---
ms.service: logic-apps
ms.topic: include
author: ecfan
ms.author: estfan
ms.date: 11/03/2016
ms.openlocfilehash: 8b5f72f18ee5bf9c1e4dbaaa87d6705f55b09bc5
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86524225"
---
## <a name="prerequisites"></a>Önkoşullar

* Bir Azure hesabı; [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturabilirsiniz
* [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3) hesabı 

OneDrive hesabınızı bir mantıksal uygulamada kullanabilmeniz için, mantıksal uygulamayı OneDrive hesabınıza bağlamak üzere yetkilendirin.  Bu, Azure portal mantıksal uygulamanız dahilinde kolayca yapabilirsiniz. 

Aşağıdaki adımları kullanarak mantıksal uygulamanızı OneDrive hesabınıza bağlamak için yetkilendirin:

1. Mantıksal uygulama oluşturun. Logic Apps tasarımcısında, açılan listeden **Microsoft yönetilen API 'Leri göster** ' i seçin ve arama kutusuna "OneDrive" yazın. Tetikleyiciler veya eylemlerden birini seçin:  
   !["Microsoft tarafından yönetilen API 'Leri göster" başlıklı iletişim kutusunda "OneDrive" içeren bir arama kutusu bulunur. Aşağıda dört tetikleyici listesi verilmiştir. İlk olarak listede "OneDrive-bir dosya oluşturulduğunda" (seçili).](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Daha önce OneDrive ile herhangi bir bağlantı oluşturmadıysanız OneDrive kimlik bilgilerinizi kullanarak oturum açmanız istenir:  
   !["OneDrive-bir dosya oluşturulduğunda" başlıklı bir iletişim kutusu, "oturum aç" etiketli bir düğmeye sahiptir.](./media/connectors-create-api-onedrive/onedrive-2.png)
3. **Oturum aç**' ı seçin ve Kullanıcı adınızı ve parolanızı girin. **Oturum aç '** ı seçin:  
   !["Oturum aç" başlıklı bir iletişim kutusu, "Microsoft hesabı kullanmanızı" sağlar. "E-posta veya telefon" ve "parola" şeklinde etiketlenmiş iki metin kutusu vardır ve "Oturumumu Açık tut" etiketli bir onay kutusu ve "oturum aç" etiketli bir düğme vardır.](./media/connectors-create-api-onedrive/onedrive-3.png)   
   
    Bu kimlik bilgileri, mantıksal uygulamanızı OneDrive hesabınızdaki verilere bağlanmak ve verilere erişmek üzere yetkilendirmek için kullanılır. 
4. Mantıksal uygulamayı OneDrive hesabınızı kullanacak şekilde yetkilendirmek için **Evet** ' i seçin:  
   !["Bu uygulamanın bilgilerinize erişmesine Izin ver" başlıklı iletişim kutusu Aşağıdaki dört şeyi yapmak için izin ister: 1) "otomatik olarak oturum aç", 2) "e-posta adreslerinize erişin", 3) "dosyalarınıza her zaman erişin" ve 4) "OneDrive dosyalarına erişin." İzin vermek için bir "Evet" düğmesi ve bunu reddetmek için bir "Hayır" düğmesi vardır. Bu uygulama izinlerini değiştirmek için bir bağlantı vardır.](./media/connectors-create-api-onedrive/onedrive-4.png)   
5. Bağlantının oluşturulduğuna dikkat edin. Şimdi mantıksal uygulamanızdaki diğer adımlara ilerleyin:  
   !["Dosya oluşturulduğunda" başlıklı bir iletişim kutusu, ilişkili bir gözatmasına sahip "klasör" başlıklı bir metin kutusuna sahiptir.](./media/connectors-create-api-onedrive/onedrive-5.png)

