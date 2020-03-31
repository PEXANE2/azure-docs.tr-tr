---
title: Azure Lab Hizmetleri'nde bir kullanıcıyı laboratuvar oluşturucusu olarak ekleme
description: Bu makalede, Azure Lab Hizmetleri'ndeki bir laboratuvar hesabı için bir kullanıcının Laboratuvar Oluşturucurolüne nasıl ekleyeceğiniz gösterilmektedir. Laboratuvar yaratıcıları bu laboratuvar hesabında laboratuvarlar oluşturabilir.
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
ms.openlocfilehash: 0538747ec639b3fab1a7b38193796d80a7736170
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77444777"
---
# <a name="add-lab-creators-to-a-lab-account-in-azure-lab-services"></a>Azure Laboratuvar Hizmetleri'ndeki bir laboratuvar hesabına laboratuvar oluşturucuları ekleme
Bu makalede, Azure Lab Hizmetleri'ndeki bir laboratuvar hesabına laboratuvar oluşturucusu olarak kullanıcıları nasıl ekleyeceğiniz gösterilmektedir. Bu kullanımlar daha sonra laboratuvar hesabında sınıf laboratuarları oluşturabilirsiniz. 

## <a name="add-microsoft-user-account-to-lab-creator-role"></a>Microsoft kullanıcı hesabını Lab Creator rolüne ekleme
Bir laboratuvar hesabında sınıf laboratuvarı ayarlamak için kullanıcının ilgili laboratuvar hesabında **Laboratuvar Oluşturan** rolünün üyesi olması gerekir. Laboratuvar hesabını oluşturmak için kullandığınız hesap otomatik olarak bu role eklenir. Sınıf laboratuvarı oluşturmak için aynı kullanıcıyı kullanmayı planlıyorsanız bu adımı atlayabilirsiniz. Sınıf laboratuvarı oluşturmak için başka bir kullanıcı hesabı kullanmak istiyorsanız aşağıdaki adımları uygulayın: 

Eğitimcilere, sınıfları için laboratuvar oluşturma ve **Laboratuvar Oluşturan** rolüne bunları ekleme izni sağlamak için:

1. Laboratuvar **Hesabı** sayfasında **Access denetimi (IAM) seçeneğini**belirleyin ve araç çubuğuna + Rol **Atamaekle'yi** tıklatın. 

    ![Erişim Denetimi -> Rol Atama ekle düğmesi](../media/tutorial-setup-lab-account/add-role-assignment-button.png)
1. Rol **ataması ekle** sayfasında, **Rol**için **Lab Creator'ı** seçin, Laboratuvar Oluşturucuları rolüne eklemek istediğiniz kullanıcıyı seçin ve **Kaydet'i**seçin. 

    ![Laboratuvar oluşturucuekleme](../media/tutorial-setup-lab-account/add-lab-creator.png)

    > [!NOTE]
    > Microsoft hesabı olmayan bir kullanıcıyı laboratuvar oluşturucusu olarak ekliyorsanız, [Microsoft hesabı olmayan bir kullanıcıyı laboratuvar oluşturucusu](#add-a-non-microsoft-account-user-as-a-lab-creator) bölümü olarak ekleyin bölümüne bakın. 

## <a name="add-a-non-microsoft-account-user-as-a-lab-creator"></a>Microsoft hesabı olmayan bir kullanıcıyı laboratuvar oluşturucusu olarak ekleme
Bir kullanıcıyı laboratuvar oluşturucusu olarak eklemek için e-posta hesaplarını kullanırsınız. Aşağıdaki e-posta hesapları türleri kullanılabilir:

- Üniversitenizin Office 365 Azure Etkin Dizini (AAD) tarafından sağlanan bir e-posta hesabı. 
- Bir Microsoft e-posta `@outlook.com` `@hotmail.com`hesabı, , , `@msn.com`, veya `@live.com`.
- Yahoo veya Google tarafından sağlanan gibi Microsoft'a ait olmayan bir e-posta hesabı. Ancak, bu tür hesapların bir Microsoft hesabına bağlanması gerekir.
- GitHub hesabı. Bu hesabın bir Microsoft hesabına bağlanması gerekir.

### <a name="using-a-non-microsoft-email-account"></a>Microsoft'a ait olmayan bir e-posta hesabı kullanma
Laboratuvar oluşturucuları/eğitmenleri, bir sınıf laboratuvarına kaydolmak ve oturum açabilmek için Microsoft'a ait olmayan e-posta hesaplarını kullanabilir.  Ancak, Laboratuvar Hizmetleri portalında oturum açma, eğitmenlerin önce Microsoft olmayan e-posta adreslerine bağlı bir Microsoft hesabı oluşturmalarını gerektirir.

Birçok eğitmenin Microsoft üyesi olmayan e-posta adreslerine bağlı bir Microsoft hesabı olabilir. Örneğin, e-posta adreslerini Microsoft'un Office, Skype, OneDrive veya Windows gibi diğer ürün veya hizmetleriyle birlikte kullanmışlarsa eğitmenlerin zaten bir Microsoft hesabı vardır.  

Eğitmenler Laboratuvar Hizmetleri portalında oturum açtıklarında, e-posta adresleri ve şifreleri istenir. Eğitmen, microsoft hesabı bağlı olmayan bir Microsoft hesabıyla oturum açmaya çalışırsa, eğitmen aşağıdaki hata iletisini alır: 

![Hata iletisi](../media/how-to-configure-student-usage/cant-find-account.png)

Bir Microsoft hesabına kaydolmak için eğitmenler [http://signup.live.com](http://signup.live.com).  


### <a name="using-a-github-account"></a>GitHub Hesabı Kullanma
Eğitmenler ayrıca bir sınıf laboratuvarına kaydolmak ve oturum açabilmek için mevcut bir GitHub hesabını da kullanabilirler. Eğitmenin Zaten GitHub hesabına bağlı bir Microsoft hesabı varsa, önceki bölümde gösterildiği gibi oturum açabilir ve parolalarını sağlayabilir. Henüz GitHub hesaplarını bir Microsoft hesabına bağlayamazsa, **Oturum Açma seçeneklerini**seçmelidir:

![Oturum açma seçenekleri bağlantısı](../media/how-to-configure-student-usage/signin-options.png)

Oturum **Açma seçenekleri** sayfasında **GitHub ile Oturum Aç'ı**seçin.

![GitHub bağlantısıyla oturum açın](../media/how-to-configure-student-usage/signin-github.png)

Son olarak, github hesabına bağlı bir Microsoft hesabı oluşturmaları istenir. Eğitmen **İleri'yi**seçtiğinde otomatik olarak gerçekleşir.  Eğitmen daha sonra hemen oturum açar ve sınıf laboratuvarına bağlanır.


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Laboratuvar sahibi olarak, laboratuvarlar oluşturun ve yönetin](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak şablonları ayarlama ve yayımlama](how-to-create-manage-template.md)
- [Laboratuvar sahibi olarak, bir laboratuvarın kullanımını yapılandırın ve kontrol edin](how-to-configure-student-usage.md)
- [Laboratuvar kullanıcısı olarak, sınıf laboratuvarlarına erişin](how-to-use-classroom-lab.md)
