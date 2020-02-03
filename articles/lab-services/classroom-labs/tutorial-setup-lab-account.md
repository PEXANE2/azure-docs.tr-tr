---
title: Azure Lab Services ile laboratuvar hesabı ayarlama | Microsoft Docs
description: Azure Lab Services ile laboratuvar hesabı ayarlamayı, laboratuvar Oluşturucu eklemeyi ve laboratuvar hesabındaki laboratuvarlar tarafından kullanılacak Market görüntülerini belirtmeyi öğrenin.
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
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: 62ccda96563a53ebc77337bb2f89cfddd4410a26
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76719050"
---
# <a name="tutorial-set-up-a-lab-account-with-azure-lab-services"></a>Öğretici: Azure Lab Services ile bir laboratuvar hesabı ayarlama
Azure Lab Services’te, bir laboratuvar hesabı, kuruluşunuzdaki laboratuvarların yönetildiği merkezi hesap olarak görev yapar. Laboratuvar hesabınızda, laboratuvar oluşturmak üzere başkalarına izin verin ve laboratuvar hesabı altındaki tüm laboratuvarlara uygulanan ilkeler ayarlayın. Bu öğreticide, laboratuvar yöneticisi olarak bir laboratuvar hesabı oluşturmayı öğrenin. 

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Laboratuvar hesabı oluşturma
> * Laboratuvar Oluşturan rolüne kullanıcı ekleme
> * Laboratuvar sahiplerine sunulan Market görüntülerini belirtme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-a-lab-account"></a>Laboratuvar hesabı oluşturma
Aşağıdaki adımlar, Azure portalını kullanarak Azure Lab Services ile nasıl bir laboratuvar hesabı oluşturulacağını göstermektedir. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol taraftaki menüden **tüm hizmetler** ' i seçin. **DevOps** bölümünde **Laboratuvar Hizmetleri** ' ni seçin. **Laboratuvar Hizmetleri**' nin yanında yıldız (`*`) seçeneğini belirlerseniz Sol menüdeki **Sık Kullanılanlar** bölümüne eklenir. Sonraki zamanda, **Sık Kullanılanlar**altında **Laboratuvar Hizmetleri** ' ni seçersiniz.

    ![Tüm Hizmetler-> Laboratuvar Hizmetleri](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. **Laboratuvar Hizmetleri** sayfasında, araç çubuğunda **Ekle** ' yi seçin. 

    ![Laboratuvar hesapları sayfasında Ekle ' yi seçin](../media/tutorial-setup-lab-account/add-lab-account-button.png)
4. **Laboratuvar hesabı** sayfasında, aşağıdaki işlemleri yapın: 
    1. **Laboratuvar hesabı adı** için bir ad girin. 
    2. Laboratuvar hesabı oluşturmak istediğiniz **Azure aboneliğini** seçin.
    3. **Kaynak grubu** için, **Yeni oluştur**’u seçip kaynak grubu için bir ad girin.
    4. **Konum** için, laboratuvar hesabının oluşturulmasını istediğiniz bir konumu/bölgeyi seçin. 
    5. Var olan bir **paylaşılan görüntü Galerisi** seçin veya bir tane oluşturun. Şablon VM 'yi, başkaları tarafından yeniden kullanılabilmesi için paylaşılan görüntü galerisine kaydedebilirsiniz. Paylaşılan görüntü galerileriyle ilgili ayrıntılı bilgi için, bkz. [Azure Lab Services paylaşılan görüntü Galerisi kullanma](how-to-use-shared-image-gallery.md). 
    6. **Eş sanal ağ**için, laboratuvar ağı için bir eş sanal ağ (VNet) seçin. Bu hesapta oluşturulan Labs seçili VNet 'e bağlı ve seçilen VNet 'teki kaynaklara erişime sahip. 
    7. Laboratuvardaki VM 'Ler için bir **adres aralığı** belirtin. Adres aralığı, sınıfsız etki alanları arası yönlendirme (CıDR) gösteriminde olmalıdır (örnek: 10.20.0.0/23). Laboratuvardaki sanal makineler, bu adres aralığında oluşturulacaktır. Daha fazla bilgi için bkz. [laboratuvardaki VM 'ler için bir adres aralığı belirtme](how-to-configure-lab-accounts.md#specify-an-address-range-for-vms-in-the-lab).
    8. Laboratuvar **oluşturucunun laboratuvar konumunu seçmesine Izin ver** alanı için, laboratuvar oluşturucularının laboratuvar için bir konum seçebilmesini isteyip istemediğinizi belirtin. Varsayılan olarak, seçeneği devre dışıdır. Laboratuvar oluşturucuları, devre dışı bırakıldığında, oluşturdukları laboratuvar için bir konum belirtemez. Labs, en yakın coğrafi konumda laboratuvar hesabına oluşturulur. Laboratuvar Oluşturucusu etkinleştirildiğinde Laboratuvar oluşturma sırasında bir konum seçebilir. 
    9. **Oluştur**’u seçin. 

        ![Laboratuvar hesabı oluştur penceresi](../media/tutorial-setup-lab-account/lab-account-settings.png)
5. Araç çubuğundaki **zil simgesini** (**Bildirimler**) seçin, dağıtımın başarılı olduğunu doğrulayın ve ardından **Kaynağa Git**' i seçin. 

    Alternatif olarak, **Laboratuvar hesapları** sayfasında **Yenile** ' yi seçin ve oluşturduğunuz laboratuvar hesabını seçin. 

    ![Laboratuvar hesabı oluştur penceresi](../media/tutorial-setup-lab-account/go-to-lab-account.png)    
6. Aşağıdaki **laboratuvar hesabı** sayfasını görürsünüz:

    ![Laboratuvar hesabı sayfası](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Laboratuvar Oluşturan rolüne kullanıcı ekleme
Bir laboratuvar hesabında sınıf laboratuvarı ayarlamak için kullanıcının ilgili laboratuvar hesabında **Laboratuvar Oluşturan** rolünün üyesi olması gerekir. Laboratuvar hesabını oluşturmak için kullandığınız hesap otomatik olarak bu role eklenir. Sınıf laboratuvarı oluşturmak için aynı kullanıcıyı kullanmayı planlıyorsanız bu adımı atlayabilirsiniz. Sınıf laboratuvarı oluşturmak için başka bir kullanıcı hesabı kullanmak istiyorsanız aşağıdaki adımları uygulayın: 

Eğitimcilere, sınıfları için laboratuvar oluşturma ve **Laboratuvar Oluşturan** rolüne bunları ekleme izni sağlamak için:

1. **Laboratuvar hesabı** sayfasında **ERIŞIM denetimi (IAM)** seçeneğini belirleyin, araç çubuğunda **+ Ekle** ' yi seçin ve ardından araç çubuğunda **+ rol ataması Ekle** ' yi seçin. 

    ![Access Control > rol ataması Ekle düğmesi](../media/tutorial-setup-lab-account/add-role-assignment-button.png)
1. **Rol ataması Ekle** sayfasında, **rol**için **Laboratuvar Oluşturucu** ' yı seçin, laboratuvar oluşturucuları rolüne eklemek istediğiniz kullanıcıyı seçin ve **Kaydet**' i seçin. 

    ![Laboratuvar Oluşturucu Ekle](../media/tutorial-setup-lab-account/add-lab-creator.png)

## <a name="specify-marketplace-images-available-to-lab-creators"></a>Laboratuvar oluşturucuları için kullanılabilen Market görüntülerini belirtme
Laboratuvar sahibi olarak laboratuvar oluşturucuların laboratuvar hesabında laboratuvar oluşturmak için kullanacağı Market görüntülerini belirtebilirsiniz. 

1. Sol taraftaki menüden **Market görüntüleri**'ni seçin. Listede varsayılan olarak tüm görüntüler (hem etkin hem devre dışı) yer alır. Yalnızca etkin/devre dışı görüntüleri görmek için en üstteki açılan listeden **Yalnızca etkin**/**Yalnızca devre dışı** seçeneğini belirleyerek listeyi filtreleyebilirsiniz. 
    
    ![Market görüntüleri sayfası](../media/tutorial-setup-lab-account/marketplace-images-page.png)

    Listede görüntülenen Market görüntüleri yalnızca aşağıdaki koşulları karşılayan görüntülerdir:
        
    - Tek bir VM oluşturur.
    - VM'leri sağlamak için Azure Resource Manager'ı kullanır
    - Ek lisans planı satın alınmasını gerektirmez
2. Etkin durumdaki bir Market görüntüsünü **devre dışı bırakmak** için aşağıdaki eylemlerden birini gerçekleştirin: 
    1. Son sütundaki **... (üç nokta)** simgesini ve ardından **Görüntüyü devre dışı bırak**'ı seçin. 

        ![Tek bir görüntüyü devre dışı bırakma](../media/tutorial-setup-lab-account/disable-one-image.png) 
    2. Listede görüntü adlarının yanında yer alan onay kutularını seçerek bir daha fazla görüntüyü seçin ve **Seçilen görüntüleri devre dışı bırak**'a tıklayın. 

        ![Birden fazla görüntüyü devre dışı bırakma](../media/tutorial-setup-lab-account/disable-multiple-images.png) 
1. Benzer şekilde bir Market görüntüsünü **etkinleştirmek** için aşağıdaki eylemlerden birini gerçekleştirebilirsiniz: 
    1. Son sütundaki **... (üç nokta)** simgesini ve ardından **Görüntüyü etkinleştir**'i seçin. 
    2. Listede görüntü adlarının yanında yer alan onay kutularını seçerek bir daha fazla görüntüyü seçin ve **Seçilen görüntüleri etkinleştir**'e tıklayın. 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir laboratuvar hesabı oluşturdunuz. Bir Mesleme olarak bir ders Laboratuvarı oluşturma hakkında bilgi edinmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Bir sınıf laboratuvarı ayarlama](tutorial-setup-classroom-lab.md)

