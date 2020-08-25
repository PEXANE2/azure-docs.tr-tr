---
title: Azure Lab Services ile laboratuvar hesabı ayarlama | Microsoft Docs
description: Azure Lab Services ile laboratuvar hesabı ayarlamayı, laboratuvar Oluşturucu eklemeyi ve laboratuvar hesabındaki laboratuvarlar tarafından kullanılacak Market görüntülerini belirtmeyi öğrenin.
ms.topic: tutorial
ms.date: 06/26/2020
ms.openlocfilehash: fba4dbc5386407bd796606d86a5b7bdc7c10fd61
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "85445075"
---
# <a name="tutorial-set-up-a-lab-account-with-azure-lab-services"></a>Öğretici: Azure Lab Services ile bir laboratuvar hesabı ayarlama
Azure Lab Services’te, bir laboratuvar hesabı, kuruluşunuzdaki laboratuvarların yönetildiği merkezi hesap olarak görev yapar. Laboratuvar hesabınızda, laboratuvar oluşturmak üzere başkalarına izin verin ve laboratuvar hesabı altındaki tüm laboratuvarlara uygulanan ilkeler ayarlayın. Bu öğreticide laboratuvar hesabı oluşturmayı öğrenin. 

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Laboratuvar hesabı oluşturma
> * Laboratuvar Oluşturan rolüne kullanıcı ekleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-a-lab-account"></a>Laboratuvar hesabı oluşturma
Aşağıdaki adımlar, Azure portalını kullanarak Azure Lab Services ile nasıl bir laboratuvar hesabı oluşturulacağını göstermektedir. 

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Sol taraftaki menüden **tüm hizmetler** ' i seçin. Kategorilerden **DevOps** ' **Categories**u seçin. Ardından **Laboratuvar Hizmetleri**' ni seçin. `*` **Laboratuvar Hizmetleri**' nin yanında Star () seçeneğini belirlerseniz Sol menüdeki **Sık Kullanılanlar** bölümüne eklenir. Sonraki zamanda, **Sık Kullanılanlar**altında **Laboratuvar Hizmetleri** ' ni seçersiniz.

    ![Tüm hizmetler-> Laboratuvar Hizmetleri](./media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. **Laboratuvar Hizmetleri** sayfasında, araç çubuğunda **Ekle** ' yi seçin veya sayfada **Laboratuvar hesabı oluştur** düğmesini seçin. 

    ![Laboratuvar hesapları sayfasında Ekle ' yi seçin](./media/tutorial-setup-lab-account/add-lab-account-button.png)
4. **Laboratuvar hesabı oluşturma** sayfasının **temel bilgiler** sekmesinde, aşağıdaki işlemleri yapın: 
    1. **Laboratuvar hesabı adı** için bir ad girin. 
    2. Laboratuvar hesabı oluşturmak istediğiniz **Azure aboneliğini** seçin.
    3. **Kaynak grubu**için, var olan bir kaynak grubunu seçin veya **Yeni oluştur**' u seçin ve kaynak grubu için bir ad girin.
    4. **Konum**için, laboratuvar hesabını oluşturmak istediğiniz bir konum/bölge seçin. 

        ![Laboratuvar hesabı-temel bilgiler sayfası](./media/tutorial-setup-lab-account/lab-account-basics-page.png)
    5. **Gözden geçir + oluştur**’u seçin.
    6. Özeti gözden geçirin ve **Oluştur**' u seçin. 

        ![Gözden geçir + oluştur-> oluştur](./media/tutorial-setup-lab-account/create-button.png)    
5. Dağıtım tamamlandığında, **sonraki adımlar**' ı genişletin ve **Kaynağa Git**' i seçin. 

    ![Laboratuvar hesabı sayfasına git](./media/tutorial-setup-lab-account/go-to-lab-account.png)
6. **Laboratuvar hesabı** sayfasını görtığınızdan emin olun. 

    ![Laboratuvar hesabı sayfası](./media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Laboratuvar Oluşturan rolüne kullanıcı ekleme
Bir laboratuvar hesabında sınıf laboratuvarı ayarlamak için kullanıcının ilgili laboratuvar hesabında **Laboratuvar Oluşturan** rolünün üyesi olması gerekir. Eğitimcilere, sınıfları için laboratuvar oluşturma ve **Laboratuvar Oluşturan** rolüne bunları ekleme izni sağlamak için:

> [!NOTE]
> Laboratuvar hesabını oluşturmak için kullandığınız hesap otomatik olarak bu role eklenir. Bu öğreticide bir derslik laboratuvarı oluşturmak için aynı kullanıcı hesabını kullanmayı planlıyorsanız, bu adımı atlayın. 

1. **Laboratuvar hesabı** sayfasında **ERIŞIM denetimi (IAM)** seçeneğini belirleyin, araç çubuğunda **+ Ekle** ' yi seçin ve ardından araç çubuğunda **+ rol ataması Ekle** ' yi seçin. 

    ![Access Control > rol ataması Ekle düğmesi](./media/tutorial-setup-lab-account/add-role-assignment-button.png)
1. **Rol ataması Ekle** sayfasında, **rol**için **Laboratuvar Oluşturucu** ' yı seçin, laboratuvar oluşturucuları rolüne eklemek istediğiniz kullanıcıyı seçin ve **Kaydet**' i seçin. 

    ![Laboratuvar Oluşturucu Ekle](./media/tutorial-setup-lab-account/add-lab-creator.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir laboratuvar hesabı oluşturdunuz. Bir eğitimci olarak bir ders Laboratuvarı oluşturma hakkında bilgi edinmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Bir sınıf laboratuvarı ayarlama](tutorial-setup-classroom-lab.md)

