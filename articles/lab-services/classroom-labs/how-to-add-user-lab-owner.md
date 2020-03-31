---
title: Azure Laboratuvar Hizmetleri'ndeki bir laboratuvara ek sahip ekleme
description: Bu makalede, bir yöneticinin Azure Lab Hizmetleri'ndeki bir laboratuvara bir kullanıcıyı nasıl sahibi olarak ekleyebileceği gösterilmektedir.
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
ms.date: 02/14/2020
ms.author: spelluru
ms.openlocfilehash: 9302cadf0b900c9faac382bad86b51cea7d828fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77443525"
---
# <a name="how-to-add-additional-owners-to-an-existing-lab-in-azure-lab-services"></a>Azure Lab Hizmetleri'ndeki mevcut bir laboratuvara ek sahip ekleme
Bu makalede, bir yönetici olarak varolan bir laboratuvara nasıl ek sahipler ekleyebileceğiniz gösterilmektedir.

## <a name="add-user-to-the-reader-role-for-the-lab-account"></a>Laboratuvar hesabı için kullanıcıyı okuyucu rolüne ekleme
Bir kullanıcıyı varolan bir laboratuvara ek bir sahip olarak eklemek için, öncelikle kullanıcıya laboratuar hesabında **okuma** izinleri vermeniz gerekir.

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sol menüden **Tüm Hizmetler'i** seçin. Laboratuvar **Hizmetlerini**arayın ve sonra seçin.
3. Listeden **laboratuvar hesabınızı** seçin. 
2. Laboratuvar **Hesabı sayfasında,** sol menüde **Erişim Denetimi 'ni (IAM)** seçin. 
2. Access **denetimi (IAM)** sayfasında araç çubuğuna **Ekle'yi** ve **rol atamaekle'yi**seçin.

    ![Laboratuvar hesabı için rol ataması ](../media/how-to-add-user-lab-owner/lab-account-access-control-page.png)
3. Rol **ataması ekle** sayfasında aşağıdaki adımları yapın: 
    1. **Rol**için **Reader'ı** seçin. 
    2. Kullanıcıyı seçin. 
    3. **Kaydet'i**seçin. 

        ![Laboratuvar hesabı için kullanıcıyı okuyucu rolüne ekleme ](../media/how-to-add-user-lab-owner/reader-lab-account.png)

## <a name="add-user-to-the-owner-role-for-the-lab"></a>Laboratuvar için kullanıcıyı sahibi rolüne ekleme

1. **Laboratuvar Hesabı** sayfasında, sol menüdeki **Tüm laboratuarları** seçin.
2. Kullanıcıyı sahip olarak eklemek istediğiniz **laboratuarı** seçin. 
    
    ![Laboratuvarı seçme ](../media/how-to-add-user-lab-owner/select-lab.png)    
3. **Laboratuvar** sayfasında, sol menüde **Access denetimi (IAM)** seçeneğini belirleyin.
4. Access **denetimi (IAM)** sayfasında araç çubuğuna **Ekle'yi** ve **rol atamaekle'yi**seçin.
5. Rol **ataması ekle** sayfasında aşağıdaki adımları yapın: 
    1. **Rol**için **Sahibi'ni** seçin. 
    2. Kullanıcıyı seçin. 
    3. **Kaydet'i**seçin. 

## <a name="next-steps"></a>Sonraki adımlar
Kullanıcının [Laboratuvar Hizmetleri portalına](https://labs.azure.com)giriş yaptıktan sonra laboratuarı gördüğünü doğrulayın.