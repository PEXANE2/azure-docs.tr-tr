---
title: Azure Lab Services sınıf laboratuvarlarında kullanım ayarlarını yapılandırma | Microsoft Docs
description: Laboratuvar için Kullanıcı sayısını yapılandırmayı, laboratuvara kaydolmalarını ve VM 'yi kullanbilecekleri saat sayısını denetlemeyi ve daha fazlasını öğrenin.
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
ms.date: 06/11/2019
ms.author: spelluru
ms.openlocfilehash: 86f22864c416ad2a90bea09c02675d6eb3322308
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68385647"
---
# <a name="configure-usage-settings-and-policies"></a>Kullanım ayarlarını ve ilkeleri yapılandırma
Bu makalede, laboratuvara Kullanıcı ekleme, laboratuvarda nasıl kayıt yapılacağı, VM 'nin kullanabileceği saat sayısını denetleme ve daha fazla bilgi edinebilirsiniz. 


## <a name="add-users-to-the-lab"></a>Laboratuvara Kullanıcı ekleme
**Erişimi kısıtla** etkinse, listeye Kullanıcı (e-posta adresleri) ekleyin.

1. Sol menüdeki **Kullanıcılar** ' ı seçin.
2. Araç çubuğundan **Kullanıcı Ekle** ' yi seçin. 

    ![Kullanıcı Ekle düğmesi](../media/how-to-configure-student-usage/add-users-button.png)
1. **Kullanıcı Ekle** sayfasında, kullanıcıların e-posta adreslerini ayrı satırlarda veya noktalı virgülle ayrılmış tek bir satıra girin. 

    ![Kullanıcı e-posta adresleri Ekle](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. **Kaydet**’i seçin. Kullanıcıların e-posta adreslerini ve bunların durumlarını (kayıtlı veya değil) listede görürsünüz. 

    ![Kullanıcı listesi](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="share-registration-link-with-students"></a>Kayıt bağlantısını öğrencilerle paylaşma
Öğrenciye kayıt bağlantısını göndermek için aşağıdaki yöntemlerden birini kullanın. İlk yöntem, öğrenciler için kayıt bağlantısı ve isteğe bağlı bir ileti ile e-posta gönderme işlemlerinin nasıl yapılacağını gösterir. İkinci yöntemde, başkalarıyla dilediğiniz şekilde paylaşabileceğiniz kayıt bağlantısını nasıl alacağınız gösterilmektedir. 

Laboratuvar için **erişimi kısıtla** etkinleştirilirse, yalnızca Kullanıcı listesindeki kullanıcılar, laboratuvara kaydolmak için kayıt bağlantısını kullanabilir. Bu seçenek varsayılan olarak etkindir. 

### <a name="send-email-to-users"></a>Kullanıcılara e-posta gönder
Azure Lab Services öğretmenlerin, başka bir e-posta istemcisi kullanmak zorunda kalmadan, tüm veya seçili öğrencilerle laboratuvar davetlerini e-posta Öğretmenler her öğrenciye ait e-posta simgesini görmek veya bir ya da daha fazla öğrenci seçmek ve araç çubuğunda **davet gönder** ' i seçmek için listedeki her öğrenciye kadar bir tane olabilir. Bu özellik, bir kayıt bağlantısı içeren bir e-posta gönderir ve öğretmentarafından eklenen bir ileti (varsa). Davet gönderildikten sonra, öğretmenler, kayıt bağlantısını ve gönderildiği tarihi ve gönderilme tarihini izlemek üzere davet durumu **gönderildi** olarak değişir.

1. Zaten sayfada değilseniz, **Kullanıcılar** görünümüne geçin. 
2. Listeden belirli veya tüm kullanıcıları seçin. Belirli kullanıcıları seçmek için, listenin ilk sütunundaki onay kutularını seçin. Tüm kullanıcıları seçmek için, ilk sütunun başlığının önünde onay kutusunu seçin (**ad**) veya listedeki tüm kullanıcılar için tüm onay kutularını seçin. **Davet durumunun** durumunu bu listede görebilirsiniz.  Aşağıdaki görüntüde tüm öğrenciler için davetiye durumu **davetiye gönderilmedi**olarak ayarlanmıştır. 

    ![Öğrencileri seçin](../media/tutorial-setup-classroom-lab/select-students.png)
1. Satırlardan birindeki **e-posta simgesini (zarf)** seçin (veya) araç çubuğunda **daveti Gönder** ' i seçin. Ayrıca, e-posta simgesini görmek için fare işaretçisini listede bir öğrenci adı üzerinde gezdirin. 

    ![Kayıt bağlantısını e-posta ile gönder](../media/tutorial-setup-classroom-lab/send-email.png)
4. **Bağlantıyı e-posta Ile gönder** sayfasında, aşağıdaki adımları izleyin: 
    1. Öğrencilerine göndermek istediğiniz **isteğe bağlı bir ileti** yazın. E-posta otomatik olarak kayıt bağlantısını içerir. 
    2. **Bağlantıyı e-posta Ile gönder** sayfasında **Gönder**' i seçin. Davet **göndermek** ve ardından **gönderilmek**üzere davet değişikliği durumunu görürsünüz. 
        
        ![Gönderilen Davetler](../media/tutorial-setup-classroom-lab/invitations-sent.png)

## <a name="get-registration-link"></a>Kayıt bağlantısını al
1. Sol menüdeki **Kullanıcılar** ' a tıklayarak **Kullanıcılar** görünümüne geçin. 
2. **Kayıt bağlantısını al** kutucuğunu seçin.

    ![Öğrenci kaydı bağlantısı](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. **Kullanıcı kaydı** iletişim kutusunda **Kopyala** düğmesini seçin. Bağlantı, panoya kopyalanır. Bağlantıyı bir e-posta düzenleyiciye yapıştırın ve öğrenciye e-posta gönderin. 

    ![Öğrenci kaydı bağlantısı](../media/tutorial-setup-classroom-lab/registration-link.png)
2. **Kullanıcı kaydı** iletişim kutusunda **Kapat**'ı seçin. 
4. Öğrencinin sınıfa kaydolabilmesi için **kayıt bağlantısını** bir öğrenci ile paylaşabilirsiniz. 

## <a name="view-users-registered-with-the-lab"></a>Laboratuvara kayıtlı kullanıcıları görüntüleme

Laboratuvara kayıtlı kullanıcıların listesini görmek için sol menüdeki **Kullanıcılar** ' ı seçin. 

![Laboratuvara kayıtlı kullanıcıların listesi](../media/how-to-configure-student-usage/users-list-new.png)

## <a name="set-quotas-for-users"></a>Kullanıcılar için kota ayarlama
Aşağıdaki adımları kullanarak, Kullanıcı başına kota ayarlayabilirsiniz: 

1. Sayfa zaten etkin değilse Sol menüdeki **Kullanıcılar** ' ı seçin. 
2. Kullanıcı **başına kota seçin: araç çubuğunda** 10 saat. 
3. **Kullanıcı başına kota** sayfasında, her kullanıcıya vermek istediğiniz saat sayısını belirtin (öğrenci): 
    1. **Kullanıcı başına toplam laboratuvar saati sayısı**. Kullanıcılar, **zamanlanan saate ek**olarak, VM 'lerini (Bu alan için belirtilen) saat sayısını ayarlamak için kullanabilir. Bu seçeneği belirlerseniz, metin kutusuna **saat sayısını** girin. 

        ![Kullanıcı başına saat sayısı](../media/how-to-configure-student-usage/number-of-hours-per-user.png). 
    1. **0 saat (yalnızca zamanlama)** . Kullanıcılar, sanal makinelerini yalnızca zamanlanan sürede veya laboratuvar sahibi bunlar için VM 'Leri açtığında kullanabilir.

        ![Sıfır saat-yalnızca zamanlanan süre](../media/how-to-configure-student-usage/zero-hours.png)
    4. **Kaydet**’i seçin. 
5. Değiştirilen değerleri şimdi araç çubuğunda görürsünüz: **Kullanıcı başına kota: &lt;saat&gt;sayısı**. 

    ![Kullanıcı başına kota](../media/how-to-configure-student-usage/quota-per-user.png)



> [!IMPORTANT]
> Öğrencilerin kayıt bağlantısını göndermeden önce, öğretmenler 0 kota saati seçer veya laboratuvar için kota saatlerini belirttiklerinde sınıf için zamanlamayı ayarlanmalıdır.
>
> [VM 'lerin zamanlanan çalışma süresi](how-to-create-schedules.md) , bir kullanıcıya ayrılan kotaya göre sayılmaz. Kota, bir öğrencinin VM 'lerde harcadığı zamanlama saatleri dışında bir süredir. 

### <a name="add-users-by-uploading-a-csv-file"></a>CSV dosyası karşıya yükleyerek Kullanıcı ekleme
Kullanıcıları, kullanıcıların e-posta adreslerine sahip bir CSV dosyası karşıya yükleyerek da ekleyebilirsiniz.

1. Tek bir sütundaki kullanıcıların e-posta adreslerini içeren bir CSV dosyası oluşturun.

    ![Kullanıcı başına kota](../media/how-to-configure-student-usage/csv-file-with-users.png)
2. Laboratuvarın **Kullanıcılar** sayfasında, araç çubuğunda CSV 'Yi **karşıya yükle** ' yi seçin.

    ![CSV 'yi Karşıya Yükle düğmesi](../media/how-to-configure-student-usage/upload-csv-button.png)
3. Kullanıcı e-posta adreslerini içeren CSV dosyasını seçin. CSV dosyasını seçtikten sonra **Aç** ' ı seçtiğinizde, aşağıdaki **Kullanıcı Ekle** penceresini görürsünüz. E-posta adresi listesi CSV dosyasındaki e-posta adresleriyle doldurulur. 

    ![CSV dosyasından e-posta adresleriyle doldurulmuş Kullanıcı ekleme penceresi](../media/how-to-configure-student-usage/add-users-window.png)
4. **Kullanıcı Ekle** penceresinde **Kaydet** ' i seçin. 
5. Kullanıcılar listesinde kullanıcıları görtığınızdan emin olun. 

    ![Eklenen kullanıcıların listesi](../media/how-to-configure-student-usage/list-of-added-users.png)

## <a name="manage-user-vms"></a>Kullanıcı VM 'lerini yönetme
Öğrenciler, verdiğiniz kayıt bağlantısını kullanarak Azure Lab Services kaydolduktan sonra **sanal makineler** sekmesinde öğrencilere atanmış VM 'leri görürsünüz. 

![Öğrencilerine atanan sanal makineler](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

Bir öğrenci sanal makinesinde aşağıdaki görevleri gerçekleştirebilirsiniz: 

- VM çalışıyorsa VM 'yi durdurun. 
- VM durdurulmuşsa bir VM başlatın. 
- VM’ye bağlanın. 
- VM 'yi silin. 
- Kullanıcıların sanal makineyi kullandığı saat sayısını görüntüleyin. 

## <a name="update-number-of-virtual-machines-in-lab"></a>Laboratuvardaki sanal makine sayısını Güncelleştir
Laboratuvardaki sanal makine sayısını güncelleştirmek için, **sanal makineler** sayfasında aşağıdaki adımları uygulayın:

1. Sol taraftaki menüden **sanal makineler** ' i seçin. 
2. **Laboratuvar kapasitesi: &lt;araç çubuğunda makine sayısı&gt;**  ' nı seçin. 
3. Sanal makine **sayısını** girin.
4. **Kaydet**’i seçin.

    ![Laboratuvardaki sanal makineler](../media/how-to-configure-student-usage/number-virtual-machines.png)


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Yönetici olarak, laboratuvar hesapları oluşturun ve yönetin](how-to-manage-lab-accounts.md)
- [Laboratuvar sahibi olarak, Labs oluşturma ve yönetme](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak, şablonları ayarlama ve yayımlama](how-to-create-manage-template.md)
- [Laboratuvar kullanıcısı olarak, sınıf laboratuvarlarına erişin](how-to-use-classroom-lab.md)
