---
title: Azure Laboratuvar Hizmetleri'nde sınıf laboratuvarlarını yönetme | Microsoft Dokümanlar
description: Sınıf laboratuarı oluşturma ve yapılandırma, tüm sınıf laboratuvarlarını görüntüleme, kayıt bağlantısını bir laboratuvar kullanıcısıyla paylaşma veya bir laboratuvarı silme yi öğrenin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 02/20/2020
ms.author: spelluru
ms.openlocfilehash: cc937589b2fc4f394b44cf6890a352d770751d15
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79502025"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Azure Laboratuvar Hizmetleri'nde sınıf laboratuvarlarını yönetme 
Bu makalede, bir sınıf laboratuvarı nın nasıl oluşturulup silinilmeye açıklanmaktadır. Ayrıca, bir laboratuar hesabındaki tüm sınıf laboratuvarlarını nasıl görüntüleyebilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar
Bir laboratuvar hesabında sınıf laboratuvarı ayarlamak için ilgili laboratuvar hesabında **Laboratuvar Oluşturan** rolünün üyesi olmanız gerekir. Laboratuvar hesabını oluşturmak için kullandığınız hesap otomatik olarak bu role eklenir. Laboratuvar sahibi, şu makaledeki adımları kullanarak Laboratuvar Oluşturan rolüne kullanıcı ekleyebilir: [Laboratuvar Oluşturan rolüne kullanıcı ekleme](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Sınıf laboratuvarı oluşturma

1. [Azure Lab Services web sitesine](https://labs.azure.com) gidin. Internet Explorer 11 henüz desteklenmedi. 
2. **Oturum aç**’ı seçip kimlik bilgilerinizi girin. Laboratuvar hesabında **Ki Lab Creator** rolünün bir üyesi olan bir kullanıcı **kimliği** seçin veya girin ve parola girin. Azure Lab Services, kuruluş hesaplarını ve Microsoft hesaplarını destekler. 
3. **Yeni laboratuarı**seçin. 
    
    ![Sınıf laboratuvarı oluşturma](../media/tutorial-setup-classroom-lab/new-lab-button.png)
3. **Yeni Laboratuvar** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. Laboratuvarınız için bir **ad** belirtin. 
    2. Sınıf için ihtiyacınız olan **sanal makinelerin boyutunu** seçin. Kullanılabilir boyutlar listesi için [VM Boyutları](#vm-sizes) bölümüne bakın. 
    3. Sınıf laboratuarı için kullanmak istediğiniz **sanal makine görüntüsünü** seçin. Bir Linux görüntüsü seçerseniz, bunun için uzak masaüstü bağlantısını etkinleştirme seçeneğini görürsünüz. Ayrıntılar için bkz: [Linux için uzak masaüstü bağlantısını etkinleştir.](how-to-enable-remote-desktop-linux.md)
    4. Sayfada görüntülenen **saat başına toplam fiyatı** gözden geçirin. 
    6. **Kaydet'i**seçin.

        ![Yeni laboratuvar penceresi](../media/tutorial-setup-classroom-lab/new-lab-window.png)

        > [!NOTE]
        > Laboratuvar hesabı [laboratuvar oluşturucusu laboratuvar konumu](allow-lab-creator-pick-lab-location.md) seçeneğini seçmek için izin verecek şekilde yapılandırılmışsa, laboratuvar ınız için bir konum seçme seçeneği görürsünüz. 
4. Sanal **makine kimlik bilgileri** sayfasında, laboratuardaki tüm VM'ler için varsayılan kimlik bilgilerini belirtin.
    1. Laboratuvardaki tüm VM'ler için **kullanıcı adını** belirtin.
    2. Kullanıcının **parolasını** belirtin. 

        > [!IMPORTANT]
        > Kullanıcı adını ve parolayı not edin. Bunlar tekrar gösterilmeyecektir.
    3. Öğrencilerin kendi parolalarını ayarlamalarını **istiyorsanız, tüm sanal makineler seçeneği için aynı parolayı** kullanın. Bu adım **isteğe bağlıdır.** 

        Öğretmen, laboratuardaki tüm VM'ler için aynı parolayı kullanmayı seçebilir veya öğrencilerin VM'leri için parola ayarlamasına izin verebilir. Varsayılan olarak, bu ayar Ubuntu hariç tüm Windows ve Linux görüntüleri için etkinleştirilir. **Ubuntu** VM'yi seçtiğinizde, bu ayar devre dışı bırakılır, bu nedenle öğrencilerden ilk kez oturum açtıklarında bir parola ayarlamaları istenir.  

        ![Yeni laboratuvar penceresi](../media/tutorial-setup-classroom-lab/virtual-machine-credentials.png)
    4. Ardından, **Sanal makine kimlik bilgileri** sayfasında **İleri'yi** seçin. 
5. Laboratuvar **ilkeleri** sayfasında aşağıdaki adımları yapın:
    1. Laboratuvar için zamanlanan saatin dışında her kullanıcı için ayrılan saat sayısını **(her kullanıcı için kota)** girin. 
    2. Sanal **makinelerin Otomatik kapatma** seçeneği için, kullanıcı bağlantı kesildiğinde VM'nin otomatik olarak kapanmasını isteyip istemediğinizi belirtin. Ayrıca, Otomatik olarak kapanmadan önce VM'nin kullanıcının yeniden bağlanmasıiçin ne kadar beklemesi gerektiğini de belirtebilirsiniz... Daha fazla bilgi için bkz: [Bağlantıyı kesmede VM'lerin otomatik olarak kapatılmasını etkinleştir.](how-to-enable-shutdown-disconnect.md)
    3. Ardından, **Finish'i**seçin. 

        ![Her kullanıcı için kota](../media/tutorial-setup-classroom-lab/quota-for-each-user.png)
    
5. Şablon VM oluşturma durumunu gösteren aşağıdaki ekranı görmeniz gerekir. Laboratuvar şablonunun oluşturulması 20 dakika sürebilir. 

    ![Şablon VM oluşturma durumu](../media/tutorial-setup-classroom-lab/create-template-vm-progress.png)
8. **Şablon** sayfasında aşağıdaki adımları yapın: Bu adımlar öğretici için **isteğe bağlıdır.**

    2. **Bağlan**'ı seçerek şablon VM'sine bağlanın. Linux şablonu VM ise, SSH veya RDP kullanarak bağlanmak isteyip istemediğinizi (RDP etkinse) seçersiniz.
    1. VM parolasını sıfırlamak için **Parolayı Sıfırla'yı** seçin. 
    1. Şablon VM'sinde yazılım yükleme ve yapılandırma işlemlerini gerçekleştirin. 
    1. VM'yi **durdurun**.  
    1. Şablon için bir **açıklama** girin
10. **Şablon** sayfasında, araç çubuğunda **Yayımla'yı** seçin. 

    ![Şablon düğmesini yayımla](../media/tutorial-setup-classroom-lab/template-page-publish-button.png)

    > [!WARNING]
    > Yayımlama işlemini geri alamazsınız. 
8. **Şablonu Yayımla** sayfasında, laboratuvarda oluşturmak istediğiniz sanal makine sayısını girin ve ardından **Yayımla'yı**seçin. 

    ![Şablon yayımla - VM sayısı](../media/tutorial-setup-classroom-lab/publish-template-number-vms.png)
11. Şablonu sayfada **yayımlama durumunu** görürsünüz. Bu işlemin tamamlanması bir saat sürebilir. 

    ![Şablonu yayımlama - ilerleme](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
4. Sol menüde Sanal makineleri seçerek veya Sanal makineler döşemesini seçerek **Sanal makineler havuzu** sayfasına geçin. Atanmamış durumda olan sanal makineleri gördüğünüzden **onaylayın.** Bu VM’ler henüz bir öğrenciye atanmamıştır. Bu makinelerin durumu **Durduruldu** olmalıdır. Bu sayfadan bir öğrenci VM'sini başlatabilir, VM'ye bağlanabilir, VM'yi durdurabilir ve VM'yi silebilirsiniz. VM'leri bu sayfadan başlatabilir veya öğrencilerinizin başlatmasını sağlayabilirsiniz. 

    ![Durdurulmuş durumdaki sanal makineler](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

    Bu sayfada aşağıdaki görevleri yaparsınız (öğretici için aşağıdaki adımları yapmayın. Bu adımlar yalnızca bilgileriniz içindir.): 
    
    1. Laboratuvar kapasitesini (laboratuardaki VM sayısı) değiştirmek için araç çubuğundaki **Laboratuvar kapasitesini** seçin.
    2. Tüm VM'leri aynı anda başlatmak için araç çubuğundaki **Tümünü Başlat'ı** seçin. 
    3. Belirli bir VM başlatmak için **Durum'daki**aşağı okunu seçin ve ardından **Başlat'ı**seçin. Ayrıca, ilk sütunda bir VM seçerek ve araç çubuğunda **Başlat'ı** seçerek de VM başlatabilirsiniz.                

### <a name="vm-sizes"></a>VM boyutları  

| Boyut | Çekirdekler | RAM | Açıklama | 
| ---- | ----- | --- | ----------- | 
| Küçük | 2 | 3,5 GB | Bu boyut en iyi komut satırı için uygundur, web tarayıcısı açılış, düşük trafik web sunucuları, küçük ve orta veritabanları. |
| Orta | 4 | 7 GB | Bu boyut ilişkisel veritabanları, bellek içi önbelleğe alma ve analitik için en uygun | 
| Orta (İç içe sanallaştırma) | 4 | 16 GB | Bu boyut, ilişkisel veritabanları, bellek içi önbelleğe alma ve analiz için en uygun uyrdu. Bu boyut iç içe sanallaştırmayı da destekler. <p>Bu boyut, her öğrencinin birden fazla VM'ye ihtiyaç duyduğu senaryolarda kullanılabilir. Öğretmenler, sanal makinenin içinde birkaç küçük boyutlu iç içe sanal makineler kurmak için iç içe sanallaştırmayı kullanabilir. </p> |
| Büyük | 8 | 32 GB | Bu boyut, daha hızlı CPU'lar, daha iyi yerel disk performansı, büyük veritabanları, büyük bellek önbellekleri gerektiren uygulamalar için en uygunudur. Bu boyut aynı zamanda iç içe sanallaştırmayı da destekler |  
| Küçük GPU (Görselleştirme) | 6 | 56 GB | Bu boyut en iyi uzaktan görselleştirme, akış, oyun, OpenGL ve DirectX gibi çerçeveleri kullanarak kodlama için uygundur. | 
| Küçük GPU (İşlem) | 6 | 56 GB | Bu boyut, yapay zeka ve derin öğrenme uygulamaları gibi bilgi işlem yoğun ve ağ yoğun uygulamalar için en uygun uyrmuştur. | 
| Orta GPU (Görselleştirme) | 12 | 112 GB | Bu boyut en iyi uzaktan görselleştirme, akış, oyun, OpenGL ve DirectX gibi çerçeveleri kullanarak kodlama için uygundur. | 

> [!NOTE]
> Azure Laboratuvar Hizmetleri, GPU görüntüleri içeren bir laboratuvar oluşturduğunuzda sizin için gerekli GPU sürücülerini otomatik olarak yükler ve yapılandırır.  

## <a name="view-all-classroom-labs"></a>Tüm sınıf laboratuvarlarını görüntüle
1. Azure [Laboratuvar Hizmetleri portalına](https://labs.azure.com)gidin.
2. **Oturum aç**'ı seçin. Laboratuvar hesabında **Ki Lab Creator** rolünün bir üyesi olan bir kullanıcı **kimliği** seçin veya girin ve parola girin. Azure Lab Services, kuruluş hesaplarını ve Microsoft hesaplarını destekler. 
3. Seçili laboratuvar hesabındaki tüm laboratuvarları gördüğünüzden onaylayın. Laboratuvarın döşemesinde, laboratuardaki sanal makine sayısını ve her kullanıcı için kotayı (zamanlanan saatin dışında) görürsünüz.

    ![Tüm laboratuvarlar](../media/how-to-manage-classroom-labs/all-labs.png)
3. Farklı bir laboratuvar hesabı seçmek için üstteki açılır listeyi kullanın. Seçilen laboratuvar hesabında ki laboratuvarları görürsünüz. 

## <a name="delete-a-classroom-lab"></a>Sınıf laboratuarını silme
1. Laboratuar döşemesinde, köşede üç nokta (...) seçin ve sonra **Sil'i**seçin. 

    ![Sil düğmesi](../media/how-to-manage-classroom-labs/delete-button.png)
3. Sil **laboratuvarı** iletişim kutusunda silme işlemine devam etmek için **Sil'i** seçin. 

## <a name="switch-to-another-classroom-lab"></a>Başka bir sınıf laboratuvarına geçme
Geçerliden başka bir sınıf laboratuvarına geçmek için, en üstteki laboratuar hesabındaki laboratuvarların açılır listesini seçin.

![En üstteki açılır listeden laboratuarı seçin](../media/how-to-manage-classroom-labs/switch-lab.png)

Ayrıca, bu açılır listede **Yeni laboratuarı** kullanarak yeni bir laboratuvar oluşturabilirsiniz. 

> [!NOTE]
> Laboratuvarları yönetmek için Az.LabServices PowerShell modüllerini (önizleme) de kullanabilirsiniz. Daha fazla bilgi için [GitHub'daki Az.LabServices ana sayfasına](https://github.com/Azure/azure-devtestlab/tree/master/samples/ClassroomLabs/Modules/Library)bakın.

Farklı bir laboratuvar hesabına geçmek için, laboratuar hesabının yanındaki açılır hesabı seçin ve diğer laboratuvar hesabını seçin. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Laboratuvar sahibi olarak şablonları ayarlama ve yayımlama](how-to-create-manage-template.md)
- [Laboratuvar sahibi olarak, bir laboratuvarın kullanımını yapılandırın ve kontrol edin](how-to-configure-student-usage.md)
- [Laboratuvar kullanıcısı olarak, sınıf laboratuvarlarına erişin](how-to-use-classroom-lab.md)

