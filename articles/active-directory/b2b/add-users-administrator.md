---
title: Azure portalına B2B işbirliği kullanıcıları ekleme - Azure AD
description: Azure Active Directory (Azure AD) B2B işbirliğini kullanarak bir yöneticinin iş ortağı kuruluşundan gelen konuk kullanıcıları dizinlerine nasıl ekleyebileceğini gösterir.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: cf2ca3bde0cfee97adee8bb875df1522e95b1f53
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79263510"
---
# <a name="add-azure-active-directory-b2b-collaboration-users-in-the-azure-portal"></a>Azure portalında Azure Active Directory B2B işbirliği kullanıcıları ekleme

Sınırlı yönetici dizin rollerinden herhangi biri atanmış bir kullanıcı olarak, B2B işbirliği kullanıcılarını davet etmek için Azure portalını kullanabilirsiniz. Konuk kullanıcıları dizine, gruba veya bir uygulamaya davet edebilirsiniz. Bu yöntemlerden herhangi biriyle bir kullanıcıyı davet ettikten sonra, davet edilen kullanıcının hesabı, kullanıcı türü *konuk*olan Azure Etkin Dizini'ne (Azure AD) eklenir. Konuk kullanıcı daha sonra kaynaklara erişim davetini kullanmalıdır. Bir kullanıcının davetinin süresi dolmaz.

Dizine bir konuk kullanıcı ekledikten sonra, konuk kullanıcıya paylaşılan bir uygulamaya doğrudan bağlantı gönderebilir veya konuk kullanıcı davet e-postasında kullanım URL'sini tıklatabilirsiniz. Kullanım süreci hakkında daha fazla bilgi için [B2B işbirliği daveti itfası'na](redemption-experience.md)bakın.

> [!IMPORTANT]
> Nasıl İş Lenir adımlarını izlemelisiniz: Kuruluşunuzun gizlilik bildiriminin URL'sini eklemek için [kuruluşunuzun gizlilik bilgilerini Azure Active Directory'ye ekleyin.](https://aka.ms/adprivacystatement) İlk kez davet kullanım sürecinin bir parçası olarak, davet edilen bir kullanıcının gizlilik koşullarınızın devam etmesini kabul etmesi gerekir. 

## <a name="before-you-begin"></a>Başlamadan önce

Kuruluşunuzun dış işbirliği ayarlarının, konukları davet etme izniniz olacak şekilde yapılandırıldığından emin olun. Varsayılan olarak, tüm kullanıcılar ve yöneticiler konukları davet edebilir. Ancak kuruluşunuzun dış işbirliği ilkeleri, belirli kullanıcı türlerinin veya yöneticilerin konuk davet etmesini önlemek için yapılandırılabilir. Bu ilkeleri nasıl görüntüleyip ayarlayabileceğinizi öğrenmek için [B2B dış işbirliğini etkinleştirme ve konukları kimlerin davet edebileceğini yönetin.](delegate-invitations.md)

## <a name="add-guest-users-to-the-directory"></a>Dizine konuk kullanıcı ekleme

B2B işbirliği kullanıcılarını dizine eklemek için aşağıdaki adımları izleyin:

1. Sınırlı yönetici dizin rolü veya Konuk Davetli rolü atanmış bir kullanıcı olarak [Azure portalında](https://portal.azure.com) oturum açın.
2. Herhangi bir sayfadan **Azure Active Directory'yi** arayın ve seçin.
3. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
4. **Yeni konuk kullanıcı**’yı seçin.

   ![Kullanıcı Birkullanıcının Kullanıcı Bira'sında nerede olduğunu gösterir](./media/add-users-administrator/new-guest-user-in-all-users.png) 
 
   > [!NOTE]
   > **Yeni konuk kullanıcı** seçeneği, Kuruluş **ilişkileri** sayfasında da kullanılabilir. **Azure Etkin Dizini'nde**, **Yönet**altında, **Kuruluş ilişkileri**seçin.

5. Yeni **kullanıcı** **sayfasında, Kullanıcıyı Davet** et'i seçin ve ardından konuk kullanıcının bilgilerini ekleyin. 

    > [!NOTE]
    > Grup e-posta adresleri desteklenmez; bir kişinin e-posta adresini girin. Ayrıca, bazı e-posta sağlayıcıları kullanıcıların gelen kutusu filtreleme gibi konularda yardımcı olmak için e-posta adreslerine bir artı sembol (+) ve ek metin eklemelerine izin verir. Ancak Azure AD şu anda e-posta adreslerindeki artı sembolleri desteklemez. Teslimat sorunlarını önlemek için, artı sembolü ve @sembolüne kadar takip eden karakterleri atla.

   - **Adı.** Konuk kullanıcının adı ve soyadı.
   - **E-posta adresi (gerekli)**. Konuk kullanıcının e-posta adresi.
   - **Kişisel mesaj (isteğe bağlı)** Konuk kullanıcıya kişisel bir karşılama iletisi ekleyin.
   - **Gruplar**: Konuk kullanıcıyı varolan bir veya daha fazla gruba ekleyebilirsiniz veya daha sonra yapabilirsiniz.
   - **Dizin rolü**: Kullanıcı için Azure AD yönetim izinleri gerekiyorsa, bunları Azure AD rolüne ekleyebilirsiniz. 

7. Konuk kullanıcıya otomatik olarak daveti göndermek için **Davet Et**’i seçin. 
 
Daveti göndermenizin ardından kullanıcı hesabı otomatik olarak dizine konuk olarak eklenir.


![Konuk kullanıcı türüne sahip B2B kullanıcılarını gösterir](./media/add-users-administrator/GuestUserType.png)  

## <a name="add-guest-users-to-a-group"></a>Gruba konuk kullanıcılar ekleme
B2B işbirliği kullanıcılarını bir gruba el ile eklemeniz gerekiyorsa, aşağıdaki adımları izleyin:

1. [Azure portalda](https://portal.azure.com) Azure AD yöneticisi olarak oturum açın.
2. Herhangi bir sayfadan **Azure Active Directory'yi** arayın ve seçin.
3. **Yönet**altında **Gruplar'ı**seçin.
4. Bir grup seçin (veya yeni bir grup oluşturmak için **Yeni grubu** tıklatın). Grubun B2B konuk kullanıcı içerdiğini grup açıklamasına eklemek iyi bir fikirdir.
5. **Üyeler**’i seçin. 
6. Aşağıdakilerden birini yapın:
   - Konuk kullanıcı dizinde zaten varsa, B2B kullanıcısını arayın. Kullanıcıyı seçin ve ardından kullanıcıyı gruba eklemek için **Seç'i** tıklatın.
   - Konuk kullanıcı dizinde zaten yoksa, arama kutusuna e-posta adreslerini yazarak, isteğe bağlı kişisel mesaj yazarak ve sonra **Seç'i**tıklatarak onları gruba davet edin. Davet otomatik olarak davet edilen kullanıcıya gider.
     
     ![Konuk üye eklemek için davet düğmesi ekle](./media/add-users-administrator/GroupInvite.png)
   
Azure AD B2B işbirliği ile dinamik grupları da kullanabilirsiniz. Daha fazla bilgi için [Dinamik gruplar ve Azure Etkin Dizin B2B işbirliğine](use-dynamic-groups.md)bakın.

## <a name="add-guest-users-to-an-application"></a>Uygulamaya konuk kullanıcılar ekleme

B2B işbirliği kullanıcılarını bir uygulamaya eklemek için aşağıdaki adımları izleyin:

1. [Azure portalda](https://portal.azure.com) Azure AD yöneticisi olarak oturum açın.
2. Herhangi bir sayfadan **Azure Active Directory'yi** arayın ve seçin.
3. **Yönet**altında, **Kurumsal uygulamaları** > seçin**Tüm uygulamalar**.
4. Konuk kullanıcı eklemek istediğiniz uygulamayı seçin.
5. Uygulamanın panosunda, **Kullanıcılar ve gruplar** bölmesini açmak için Toplam **Kullanıcı** seçeneğini belirleyin.

    ![Açık Kullanıcılar ve Gruplar eklemek için Toplam Kullanıcı düğmesi](./media/add-users-administrator/AppUsersAndGroups.png)

6. **Kullanıcı Ekle'yi**seçin.
7. **Atama Ekle** **altında, Kullanıcı ve grupları**seçin.
8. Aşağıdakilerden birini yapın:
   - Konuk kullanıcı dizinde zaten varsa, B2B kullanıcısını arayın. Kullanıcıyı seçin, **Seç'i**tıklatın ve ardından kullanıcıyı uygulamaya eklemek için **Atla'yı** tıklatın.
   - Konuk kullanıcı dizinde zaten yoksa, Üye **Seç'in**altında veya harici bir kullanıcıyı davet edin, kullanıcının e-posta adresini yazın. İleti kutusuna isteğe bağlı kişisel ileti yazın. İleti kutusunun altında **Davet Et'i**tıklatın.
           
       ![Konuk üye eklemek için davet düğmesi ekle](./media/add-users-administrator/AppInviteUsers.png)
   
      **Seç'i**tıklatın ve ardından kullanıcıyı uygulamaya eklemek için **Atla'yı** tıklatın. Davet otomatik olarak davet edilen kullanıcıya gider.

9. Konuk kullanıcı, varsayılan **erişim**atanmış rolü ile uygulamanın **Kullanıcılar ve gruplar** listesinde görünür. Rolü değiştirmek istiyorsanız, aşağıdakileri yapın:
   - Konuk kullanıcıyı seçin ve ardından **Edit'i**seçin. 
   - **Atamayı Edit**altında **Rolü Seç'i**tıklatın ve seçili kullanıcıya atamak istediğiniz rolü seçin.
   - **Seç'i**tıklatın.
   - **Ata**'ya tıklayın.
 
## <a name="resend-invitations-to-guest-users"></a>Konuk kullanıcılara davetleri yeniden gönderme

Konuk kullanıcı davetini henüz kullanmamışsa, davet e-postasını yeniden gönderebilirsiniz.

1. [Azure portalda](https://portal.azure.com) Azure AD yöneticisi olarak oturum açın.
2. Herhangi bir sayfadan **Azure Active Directory'yi** arayın ve seçin.
3. **Yönet** bölümünde **Kullanıcılar**’ı seçin.
5. Kullanıcı hesabını seçin.
6. **Yönet**altında, **Profili**seçin.
7. Kullanıcı daveti henüz kabul etmediyse, **Yeniden Gönder davetiyesi** seçeneği kullanılabilir. Yeniden göndermek için bu düğmeyi seçin.

   ![Kullanıcı profilinde daveti yeniden gönderme seçeneği](./media/add-users-administrator/b2b-user-resend-invitation.png)

> [!NOTE]
> Kullanıcıyı özgün olarak belirli bir uygulamaya yönlendiren bir daveti yeniden gönderirseniz, yeni davetteki bağlantının kullanıcıyı üst düzey Access Panel'e götürdeğini anlayın.

## <a name="next-steps"></a>Sonraki adımlar

- Azure ad olmayan yöneticilerin B2B konuk kullanıcılarını nasıl ekleyebileceğini öğrenmek için [bilgi çalışanları b2B işbirliği kullanıcılarını nasıl ekler?](add-users-information-worker.md)
- Davet e-postası hakkında bilgi için [B2B işbirliği daveti e-postasının öğelerine](invitation-email-elements.md)bakın.

