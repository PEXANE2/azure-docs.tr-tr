---
title: Azure blok zinciri çalışma ekranı 'nda kullanıcıları yönetme
description: Azure blok zinciri çalışma ekranı 'nda kullanıcıları yönetme.
ms.date: 05/09/2019
ms.topic: how-to
ms.reviewer: brendal
ms.openlocfilehash: 78841ca830ae56f6c07f796bdde85f6a6ba9c921
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85212743"
---
# <a name="manage-users-in-azure-blockchain-workbench"></a>Azure blok zinciri çalışma ekranı 'nda kullanıcıları yönetme

Azure blok zinciri çalışma ekranı, konsorsiyumun bir parçası olan kişiler ve kuruluşlar için Kullanıcı yönetimini içerir.

## <a name="prerequisites"></a>Ön koşullar

Blok zinciri çalışma ekranı dağıtımı gereklidir. Dağıtım hakkındaki ayrıntılar için bkz. [Azure blok zinciri çalışma ekranı dağıtımı](deploy.md) .

## <a name="add-azure-ad-users"></a>Azure AD kullanıcıları ekleme

Azure blok zinciri çalışma ekranı, kimlik doğrulaması, erişim denetimi ve roller için Azure Active Directory (Azure AD) kullanır. Blok zincirindeki kullanıcılar Azure AD kiracısı kimlik doğrulaması yapabilir ve blok zinciri çalışma ekranı kullanabilir. Etkileşim kurmak ve eylemler gerçekleştirmek için yönetici uygulama rolüne kullanıcı ekleyin.

Blok zinciri çalışma ekranı, uygulamaları ve rollere atamadan önce kullanıcıların Azure AD kiracısında mevcut olmaları gerekir. Azure AD 'ye Kullanıcı eklemek için aşağıdaki adımları kullanın:

1.  [Azure Portal](https://portal.azure.com) oturum açın.
2.  Sağ üst köşede hesabınızı seçin ve blok zinciri çalışma ekranı ile ilişkili Azure AD kiracısına geçiş yapın.
3.  **Azure Active Directory > kullanıcıları**' nı seçin. Dizininizde bulunan kullanıcıların listesini görürsünüz.
4.  Dizine kullanıcı eklemek için **Yeni Kullanıcı**' yı seçin. Dış kullanıcılar için **Yeni Konuk Kullanıcı**' yı seçin.

    ![Yeni kullanıcı](./media/manage-users/add-ad-user.png)

5.  Yeni Kullanıcı için gerekli alanları doldurun. **Oluştur**'u seçin.

Azure AD 'de kullanıcıları yönetme hakkında daha fazla bilgi için [Azure AD](../../active-directory/fundamentals/add-users-azure-active-directory.md) belgelerini ziyaret edin.

## <a name="manage-blockchain-workbench-administrators"></a>Blok zincirini yönetme çalışma ekranı yöneticileri

Kullanıcılar dizine eklendikten sonra, bir sonraki adım, hangi kullanıcıların blok zinciri çalışma ekranı yöneticileri olduğunu seçmelidir. **Yönetici** grubundaki kullanıcılar, blok zinciri çalışma ekranı 'ndaki **Yönetici uygulama rolüyle** ilişkilendirilir. Yöneticiler kullanıcı ekleyebilir veya kaldırabilir, belirli senaryolara Kullanıcı atayabilir ve yeni uygulamalar oluşturabilir.

Kullanıcıları Azure AD dizininde **yönetici** grubuna eklemek için:

1.  [Azure Portal](https://portal.azure.com) oturum açın.
2.  Sağ üst köşede hesabınızı seçerek blok zinciri ile ilişkili Azure AD kiracısında olduğunuzdan emin olun.
3.  **Kurumsal uygulamalar > Azure Active Directory**seçin.
4.  Blok zinciri için Azure AD istemci uygulamasını seçme çalışma ekranı
    
    ![Tüm kurumsal uygulama kayıtları](./media/manage-users/select-blockchain-client-app.png)

5.  **Kullanıcı ekle > kullanıcılar ve gruplar ' ı**seçin.
6.  **Atama Ekle** bölümünde **Kullanıcılar**’ı seçin. Yönetici olarak eklemek istediğiniz kullanıcıyı seçin veya arayın. Bitirdiğinizde **Seç** ' e tıklayın.

    ![Atama Ekle](./media/manage-users/add-user-assignment.png)

9.  **Rolün** **yönetici** olarak ayarlandığını doğrula
10. **Ata**'yı seçin. Eklenen kullanıcılar listede yönetici rolü atanmış olarak görüntülenir.

    ![Blok zinciri istemci uygulaması kullanıcıları](./media/manage-users/blockchain-admin-list.png)

## <a name="managing-blockchain-workbench-members"></a>Blok zinciri çalışma ekranı üyelerini yönetme

Konsorsiyumun parçası olan kullanıcıları ve kuruluşları yönetmek için blok zinciri çalışma ekranı uygulamasını kullanın. Uygulamaları ve rolleri kullanıcı ekleyebilir veya kaldırabilirsiniz.

1. Tarayıcınızda [blok zinciri çalışma ekranı](deploy.md#blockchain-workbench-web-url) ' nı açın ve yönetici olarak oturum açın.

    ![Blockchain Workbench](./media/manage-users/blockchain-workbench-applications.png)

    Üyeler her uygulamaya eklenir. Üyeler, sözleşmeleri başlatmak veya eylem gerçekleştirmek için bir veya daha fazla uygulama rolüne sahip olabilir.

2. Bir uygulamanın üyelerini yönetmek için **uygulamalar** bölmesinde bir uygulama kutucuğu seçin.

    Seçilen uygulamayla ilişkili üyelerin sayısı, Üyeler kutucuğunda yansıtılır.

    ![Uygulama seçin](./media/manage-users/blockchain-workbench-select-application.png)


#### <a name="add-member-to-application"></a>Uygulamaya üye ekleme

1. Geçerli üyelerin listesini göstermek için üye kutucuğunu seçin.
2. **Üye Ekle**' yi seçin.

    ![Üye ekle](./media/manage-users/application-add-members.png)

3. Kullanıcının adını arayın.  Yalnızca blok zinciri çalışma ekranı kiracısında mevcut olan Azure AD kullanıcıları listelenir. Kullanıcı bulunamazsa, [Azure AD kullanıcıları eklemeniz](#add-azure-ad-users)gerekir.

    ![Üye ekle](./media/manage-users/find-user.png)

4. Açılan listeden bir **rol** seçin.

    ![Rol üyelerini seçin](./media/manage-users/application-select-role.png)

5. İlişkili rolün bulunduğu üyeyi uygulamaya eklemek için **Ekle** ' yi seçin.

#### <a name="remove-member-from-application"></a>Üyeyi uygulamadan kaldır

1. Geçerli üyelerin listesini göstermek için üye kutucuğunu seçin.
2. Kaldırmak istediğiniz kullanıcı için rol açılır listesinden **Kaldır** ' ı seçin.

    ![Üyeyi kaldırma](./media/manage-users/application-remove-member.png)

#### <a name="change-or-add-role"></a>Rol değiştirme veya ekleme

1. Geçerli üyelerin listesini göstermek için üye kutucuğunu seçin.
2. Değiştirmek istediğiniz kullanıcının açılan düğmesine tıklayın ve yeni rolü seçin.

    ![Rolü değiştir](./media/manage-users/application-change-role.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılır makalesinde, Azure blok zinciri çalışma ekranı için kullanıcıları nasıl yönetebileceğinizi öğrendiniz. Bir blok zinciri uygulaması oluşturmayı öğrenmek için, sonraki nasıl yapılır makalesine ilerleyin.

> [!div class="nextstepaction"]
> [Azure blok zinciri çalışma ekranı 'nda bir blok zinciri uygulaması oluşturma](create-app.md)
