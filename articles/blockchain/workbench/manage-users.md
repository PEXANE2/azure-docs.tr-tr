---
title: Azure Blockchain Workbench'teki kullanıcıları yönetme
description: Azure Blockchain Workbench'teki kullanıcıları yönetme.
ms.date: 05/09/2019
ms.topic: article
ms.reviewer: brendal
ms.openlocfilehash: 573ec477a3e75beb91f90da0545fb7d4c0f9bf39
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252187"
---
# <a name="manage-users-in-azure-blockchain-workbench"></a>Azure Blockchain Workbench'teki Kullanıcıları Yönetme

Azure Blockchain Workbench, konsorsiyumunuzun bir parçası olan kişi ve kuruluşlar için kullanıcı yönetimini içerir.

## <a name="prerequisites"></a>Ön koşullar

Blockchain Workbench dağıtımı gereklidir. Dağıtımla ilgili ayrıntılar için [Azure Blockchain Workbench dağıtımına](deploy.md) bakın.

## <a name="add-azure-ad-users"></a>Azure AD kullanıcıları ekleme

Azure Blockchain Workbench, kimlik doğrulama, erişim denetimi ve roller için Azure Etkin Dizini (Azure AD) kullanır. Blockchain Workbench Azure AD kiracısındaki kullanıcılar Blockchain Workbench'in kimliğini doğrulayabilir ve kullanabilir. Etkileşim de bulunmak ve eylemleri gerçekleştirmek için kullanıcıları Yönetici uygulama rolüne ekleyin.

Blockchain Workbench kullanıcılarının uygulamalara ve rollere atamadan önce Azure AD kiracısında bulunması gerekir. Kullanıcıları Azure AD'ye eklemek için aşağıdaki adımları kullanın:

1.  [Azure portalında](https://portal.azure.com)oturum açın.
2.  Sağ üst köşede hesabınızı seçin ve Blockchain Workbench ile ilişkili Azure AD kiracısına geçin.
3.  **Azure Etkin Dizini > Kullanıcıları'nı**seçin. Dizininizde kullanıcıların listesini görürsünüz.
4.  Dizinlere kullanıcı eklemek için **Yeni kullanıcıyı**seçin. Harici kullanıcılar için **Yeni konuk kullanıcıyı**seçin.

    ![Yeni kullanıcı](./media/manage-users/add-ad-user.png)

5.  Yeni kullanıcı için gerekli alanları tamamlayın. **Oluştur'u**seçin.

Azure AD içindeki kullanıcıları nasıl yönetireci hakkında daha fazla bilgi için [Azure REKLAM](../../active-directory/fundamentals/add-users-azure-active-directory.md) belgelerini ziyaret edin.

## <a name="manage-blockchain-workbench-administrators"></a>Blockchain Workbench yöneticilerini yönetme

Kullanıcılar dizine eklendikten sonra, bir sonraki adım hangi kullanıcıların Blockchain Workbench yöneticileri olduğunu seçmektir. **Yönetici** grubundaki kullanıcılar Blockchain Workbench'teki **Yönetici uygulama rolüyle** ilişkilidir. Yöneticiler kullanıcıları ekleyebilir veya kaldırabilir, kullanıcıları belirli senaryolara atayabilir ve yeni uygulamalar oluşturabilir.

Azure AD dizinindeki **Yönetici** grubuna kullanıcı eklemek için:

1.  [Azure portalında](https://portal.azure.com)oturum açın.
2.  Sağ üst köşedeki hesabınızı seçerek Blockchain Workbench ile ilişkili Azure AD kiracısında olduğunuzu doğrulayın.
3.  **Azure Active Directory > Kurumsal uygulamalarını**seçin.
4.  Blockchain Workbench için Azure AD istemcisi uygulamasını seçin
    
    ![Tüm kurumsal başvuru kayıtları](./media/manage-users/select-blockchain-client-app.png)

5.  **Kullanıcı ekle > Kullanıcıları ve grupları**seçin.
6.  **Atama Ekle** bölümünde **Kullanıcılar**’ı seçin. Yönetici olarak eklemek istediğiniz kullanıcıyı seçin veya arayın. Seçimi tamamlandığında **Seç'i** tıklatın.

    ![Atama ekleme](./media/manage-users/add-user-assignment.png)

9.  **Rolü** **Niçin Yönetici** olarak ayarlanır
10. **Ata**'yı seçin. Eklenen kullanıcılar, yönetici rolü atanmış listede görüntülenir.

    ![Blockchain istemci uygulaması kullanıcıları](./media/manage-users/blockchain-admin-list.png)

## <a name="managing-blockchain-workbench-members"></a>Blockchain Workbench üyelerini yönetme

Konsorsiyumunuzun bir parçası olan kullanıcıları ve kuruluşları yönetmek için Blockchain Workbench uygulamasını kullanın. Uygulamaları ve rolleri kullanıcılara ekleyebilir veya kaldırabilirsiniz.

1. Tarayıcınızda [Blockchain Çalışma Tezgahı'nı açın](deploy.md#blockchain-workbench-web-url) ve yönetici olarak oturum açın.

    ![Blockchain Workbench](./media/manage-users/blockchain-workbench-applications.png)

    Üyeler her uygulamaya eklenir. Üyeler, sözleşmeleri başlatmak veya eylemde bulunmak için bir veya daha fazla uygulama rolüne sahip olabilir.

2. Bir uygulamanın üyelerini yönetmek için **Uygulamalar** bölmesinde bir uygulama döşemesi seçin.

    Seçilen uygulamayla ilişkili üye sayısı üye döşemesine yansıtılır.

    ![Uygulamayı seçin](./media/manage-users/blockchain-workbench-select-application.png)


#### <a name="add-member-to-application"></a>Uygulamaya üye ekleme

1. Geçerli üyelerin listesini görüntülemek için üye döşemesini seçin.
2. **Üye Ekle'yi**seçin.

    ![Üye ekle](./media/manage-users/application-add-members.png)

3. Kullanıcının adını arayın.  Yalnızca Blockchain Workbench kiracısında bulunan Azure AD kullanıcıları listelenir. Kullanıcı bulunamazsa, [Azure AD kullanıcıları eklemeniz](#add-azure-ad-users)gerekir.

    ![Üye ekle](./media/manage-users/find-user.png)

4. Açılan yerden bir **Rol** seçin.

    ![Rol üyelerini seçme](./media/manage-users/application-select-role.png)

5. İlişkili role sahip üyeyi uygulamaya eklemek için **Ekle'yi** seçin.

#### <a name="remove-member-from-application"></a>Üyeyi uygulamadan kaldırma

1. Geçerli üyelerin listesini görüntülemek için üye döşemesini seçin.
2. Kaldırmak istediğiniz kullanıcı için, rol açılır tarihinden **kaldır'ı** seçin.

    ![Üyeyi kaldırma](./media/manage-users/application-remove-member.png)

#### <a name="change-or-add-role"></a>Rolü değiştirme veya ekleme

1. Geçerli üyelerin listesini görüntülemek için üye döşemesini seçin.
2. Değiştirmek istediğiniz kullanıcı için açılır açılır a'yı tıklatın ve yeni rolü seçin.

    ![Rolü değiştir](./media/manage-users/application-change-role.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılsa makalede, Azure Blockchain Workbench kullanıcılarını nasıl yönetileceğinizi öğrendiniz. Blockchain uygulamasını nasıl oluşturabilirsiniz öğrenmek için bir sonraki nasıl yapılacağını makaleye devam edin.

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench'te blockchain uygulaması oluşturma](create-app.md)
