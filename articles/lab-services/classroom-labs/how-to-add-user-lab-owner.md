---
title: Kullanıcı Azure Lab Services laboratuvar sahibi olarak nasıl eklenir
description: Bu makalede, bir yöneticinin bir kullanıcıyı bir laboratuvara sahip olarak nasıl ekleyebileceğiniz gösterilmektedir.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2019
ms.author: spelluru
ms.openlocfilehash: 9e007ee6c95843b7e8ef9d7a701237034811489e
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75480859"
---
# <a name="how-to-add-a-user-as-an-owner-of-a-classroom-lab-in-azure-lab-services"></a>Azure Lab Services ' de bir derslik Laboratuvarı sahibi olarak Kullanıcı ekleme
Bu makalede, Azure Lab Services ' de bir laboratuvarın sahibi olarak bir kullanıcı ekleme gösterilmektedir.

## <a name="add-user-to-the-reader-role-for-the-lab-account"></a>Laboratuvar hesabı için okuyucu rolüne kullanıcı ekleme
1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
2. Sol taraftaki menüden **tüm hizmetler** ' i seçin. **Laboratuvar Hizmetleri**araması yapın ve ardından seçin.
3. Listeden **Laboratuvar hesabınızı** seçin. 
2. **Laboratuvar hesabı sayfasında**sol menüdeki **Access Control (IAM)** öğesini seçin. 
2. **Erişim denetimi (IAM)** sayfasında, araç çubuğunda **Ekle** ' yi ve **Rol Ekle atamasını**seçin.

    ![Laboratuvar hesabı için rol ataması ](../media/how-to-add-user-lab-owner/lab-account-access-control-page.png)
3. **Rol ataması Ekle** sayfasında, aşağıdaki adımları uygulayın: 
    1. **Rol**için **okuyucu** ' yı seçin. 
    2. Kullanıcıyı seçin. 
    3. **Kaydet**’i seçin. 

        ![Laboratuvar hesabı için okuyucu rolüne kullanıcı ekleme ](../media/how-to-add-user-lab-owner/reader-lab-account.png)

## <a name="add-user-to-the-owner-role-for-the-lab"></a>Laboratuvar için Kullanıcı sahibi rolüne kullanıcı ekleme

1. **Laboratuvar hesabı** sayfasında, sol taraftaki menüden **Tüm Labs** ' i seçin.
2. Sahip olarak Kullanıcı eklemek istediğiniz **Laboratuvarı** seçin. 
    
    ![Laboratuvarı seçme ](../media/how-to-add-user-lab-owner/select-lab.png)    
3. **Laboratuvar** sayfasında sol menüdeki **ERIŞIM denetimi (IAM)** seçeneğini belirleyin.
4. **Erişim denetimi (IAM)** sayfasında, araç çubuğunda **Ekle** ' yi ve **Rol Ekle atamasını**seçin.
5. **Rol ataması Ekle** sayfasında, aşağıdaki adımları uygulayın: 
    1. Rolün **sahibini** seçin. 
    2. Kullanıcıyı seçin. 
    3. **Kaydet**’i seçin. 

## <a name="next-steps"></a>Sonraki adımlar
Kullanıcının [Laboratuvar Hizmetleri portalında](https://labs.azure.com)oturum açarken Laboratuvarı görtiğini onaylayın.