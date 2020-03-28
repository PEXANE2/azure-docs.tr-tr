---
title: Azure Lab Services ile laboratuvar hesabı ayarlama | Microsoft Docs
description: Azure Lab Hizmetleri ile nasıl bir laboratuvar hesabı oluşturabileceğinizi, bir laboratuvar oluşturucusu eklemeyi ve laboratuvar hesabındaki laboratuvarlar tarafından kullanılacak Market görüntülerini belirtmeyi öğrenin.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 02/10/2020
ms.author: spelluru
ms.openlocfilehash: dba6a4c07691f3d7ec88d8b889e68d6ac7116f07
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79239451"
---
# <a name="tutorial-set-up-a-lab-account-with-azure-lab-services"></a>Öğretici: Azure Lab Services ile bir laboratuvar hesabı ayarlama
Azure Lab Services’te, bir laboratuvar hesabı, kuruluşunuzdaki laboratuvarların yönetildiği merkezi hesap olarak görev yapar. Laboratuvar hesabınızda, laboratuvar oluşturmak üzere başkalarına izin verin ve laboratuvar hesabı altındaki tüm laboratuvarlara uygulanan ilkeler ayarlayın. Bu eğitimde, nasıl bir laboratuvar hesabı oluşturabilirsiniz öğrenin. 

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Laboratuvar hesabı oluşturma
> * Laboratuvar Oluşturan rolüne kullanıcı ekleme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="create-a-lab-account"></a>Laboratuvar hesabı oluşturma
Aşağıdaki adımlar, Azure portalını kullanarak Azure Lab Services ile nasıl bir laboratuvar hesabı oluşturulacağını göstermektedir. 

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Sol menüden **Tüm Hizmetler'i** seçin. **Kategorilerden** **DevOps'leri** seçin. Ardından, **Laboratuvar Hizmetleri'ni**seçin. Laboratuvar`*` **Hizmetleri'nin**yanındaki yıldız ( ) seçeneğini seçerseniz, sol menüdeki SıK **Kullanılanlar** bölümüne eklenir. Bir sonraki andan itibaren, **Sık Kullanılanlar**altında Laboratuvar **Hizmetleri'ni** seçersiniz.

    ![Tüm Hizmetler -> Laboratuvar Hizmetleri](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. Laboratuvar **Hizmetleri** sayfasında, araç çubuğunda **Ekle'yi** seçin veya sayfadaki **laboratuvar hesabı oluştur** düğmesini seçin. 

    ![Laboratuvar Hesapları sayfasında Ekle'yi seçin](../media/tutorial-setup-lab-account/add-lab-account-button.png)
4. **Laboratuvar hesabı oluştur** sayfasının **Temeller** sekmesinde aşağıdaki işlemleri yapın: 
    1. **Laboratuvar hesabı adı** için bir ad girin. 
    2. Laboratuvar hesabı oluşturmak istediğiniz **Azure aboneliğini** seçin.
    3. **Kaynak grubu**için, varolan bir kaynak grubu seçin veya **yeni oluştur'u**seçin ve kaynak grubu için bir ad girin.
    4. **Konum**için, laboratuvar hesabını oluşturmak istediğiniz bir konum/bölge seçin. 

        ![Laboratuvar hesabı - temel ler sayfası](../media/tutorial-setup-lab-account/lab-account-basics-page.png)
    5. **İncele ve oluştur**’u seçin.
    6. Özeti gözden geçirin ve **Oluştur'u**seçin. 

        ![İnceleme + oluşturma -> Oluştur](../media/tutorial-setup-lab-account/create-button.png)    
5. Dağıtım tamamlandığında, Sonraki **adımları**genişletin ve **kaynağa Git'i**seçin. 

    ![Laboratuvar hesabı sayfasına gitme](../media/tutorial-setup-lab-account/go-to-lab-account.png)
6. **Laboratuvar Hesabı** sayfasını gördüğünüzden onaylayın. 

    ![Laboratuvar hesabı sayfası](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Laboratuvar Oluşturan rolüne kullanıcı ekleme
Bir laboratuvar hesabında sınıf laboratuvarı ayarlamak için kullanıcının ilgili laboratuvar hesabında **Laboratuvar Oluşturan** rolünün üyesi olması gerekir. Eğitimcilere, sınıfları için laboratuvar oluşturma ve **Laboratuvar Oluşturan** rolüne bunları ekleme izni sağlamak için:

> [!NOTE]
> Laboratuvar hesabını oluşturmak için kullandığınız hesap otomatik olarak bu role eklenir. Bu öğreticide bir sınıf laboratuvarı oluşturmak için aynı kullanıcı hesabını kullanmayı planlıyorsanız, bu adımı atlayın. 

1. Laboratuvar **Hesabı** sayfasında **Access denetimi (IAM) seçeneğini**seçin, araç çubuğuna + **Ekle'yi** seçin ve ardından araç çubuğunda **rol ataması ekle'** yi seçin. 

    ![Erişim Denetimi -> Rol Atama ekle düğmesi](../media/tutorial-setup-lab-account/add-role-assignment-button.png)
1. Rol **ataması ekle** sayfasında, **Rol**için **Lab Creator'ı** seçin, Laboratuvar Oluşturucuları rolüne eklemek istediğiniz kullanıcıyı seçin ve **Kaydet'i**seçin. 

    ![Laboratuvar oluşturucuekleme](../media/tutorial-setup-lab-account/add-lab-creator.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir laboratuvar hesabı oluşturdunuz. Profesör olarak sınıf laboratuvarı oluşturma hakkında bilgi edinmek için bir sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Bir sınıf laboratuvarı ayarlama](tutorial-setup-classroom-lab.md)

