---
title: Azure Lab Services 'de laboratuvara ek sahipler ekleme
description: Bu makalede, bir yöneticinin Azure Lab Services bir laboratuvara sahip olarak Kullanıcı nasıl ekleyebileceğiniz gösterilmektedir.
ms.topic: article
ms.date: 09/04/2020
ms.openlocfilehash: 4b46dd08a3b9527cdfc7bdf766ee4976e67656d0
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89482678"
---
# <a name="how-to-add-additional-owners-to-an-existing-lab-in-azure-lab-services"></a>Azure Lab Services ' de var olan laboratuvara ek sahipler ekleme
Bu makalede, yönetici olarak, var olan laboratuvara nasıl ek sahipler ekleyebileceğiniz gösterilmektedir.

## <a name="add-user-to-the-reader-role-for-the-lab-account"></a>Laboratuvar hesabı için okuyucu rolüne kullanıcı ekleme
Bir kullanıcıyı mevcut bir laboratuvara ek bir sahip olarak eklemek için, önce laboratuvar hesabında kullanıcıya **okuma** izinleri vermeniz gerekir.

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Sol taraftaki menüden **tüm hizmetler** ' i seçin. **Laboratuvar Hizmetleri**araması yapın ve ardından seçin.
3. Listeden **Laboratuvar hesabınızı** seçin. 
2. **Laboratuvar hesabı sayfasında**sol menüdeki **Access Control (IAM)** öğesini seçin. 
2. **Erişim denetimi (IAM)** sayfasında, araç çubuğunda **Ekle** ' yi ve **Rol Ekle atamasını**seçin.

    ![Laboratuvar hesabı için rol ataması ](./media/how-to-add-user-lab-owner/lab-account-access-control-page.png)
3. **Rol ataması Ekle** sayfasında, aşağıdaki adımları uygulayın: 
    1. **Rol**için **okuyucu** ' yı seçin. 
    2. Kullanıcıyı seçin. 
    3. **Kaydet**’i seçin. 

        ![Laboratuvar hesabı için okuyucu rolüne kullanıcı ekleme ](./media/how-to-add-user-lab-owner/reader-lab-account.png)

## <a name="add-user-to-the-owner-role-for-the-lab"></a>Laboratuvar için Kullanıcı sahibi rolüne kullanıcı ekleme

> [!NOTE]
> Kullanıcının laboratuvara yalnızca okuyucu erişimi varsa, laboratuvar labs.azure.com içinde gösterilmez.

1. **Laboratuvar hesabı** sayfasında, sol taraftaki menüden **Tüm Labs** ' i seçin.
2. Sahip olarak Kullanıcı eklemek istediğiniz **Laboratuvarı** seçin. 
    
    ![Laboratuvarı seçme ](./media/how-to-add-user-lab-owner/select-lab.png)    
3. **Laboratuvar** sayfasında sol menüdeki **ERIŞIM denetimi (IAM)** seçeneğini belirleyin.
4. **Erişim denetimi (IAM)** sayfasında, araç çubuğunda **Ekle** ' yi ve **Rol Ekle atamasını**seçin.
5. **Rol ataması Ekle** sayfasında, aşağıdaki adımları uygulayın: 
    1. Rolün **sahibini** seçin. **role** 
    2. Kullanıcıyı seçin. 
    3. **Kaydet**’i seçin. 

## <a name="next-steps"></a>Sonraki adımlar
Kullanıcının [Laboratuvar Hizmetleri portalında](https://labs.azure.com)oturum açarken Laboratuvarı görtiğini onaylayın.
