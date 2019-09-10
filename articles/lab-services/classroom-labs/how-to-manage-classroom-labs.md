---
title: Azure Lab Services sınıf laboratuvarlarını yönetme | Microsoft Docs
description: Bir derslik Laboratuvarı oluşturma ve yapılandırma, tüm sınıf laboratuvarlarını görüntüleme, kayıt bağlantısını laboratuar kullanıcısı ile paylaşma veya bir laboratuvarı silme hakkında bilgi edinin.
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
ms.date: 06/07/2019
ms.author: spelluru
ms.openlocfilehash: 1f9cb82abd5bc0823f5e7bc23fe437007bccc8e0
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70873587"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Azure Lab Services sınıf laboratuvarlarını yönetme 
Bu makalede bir sınıf Laboratuvarı oluşturma ve silme açıklanmaktadır. Ayrıca, tüm sınıf laboratuvarlarını bir laboratuvar hesabında nasıl görüntüleyekullanacağınızı gösterir. 

## <a name="prerequisites"></a>Önkoşullar
Bir laboratuvar hesabında sınıf laboratuvarı ayarlamak için ilgili laboratuvar hesabında **Laboratuvar Oluşturan** rolünün üyesi olmanız gerekir. Laboratuvar hesabını oluşturmak için kullandığınız hesap otomatik olarak bu role eklenir. Laboratuvar sahibi, aşağıdaki makaledeki adımları kullanarak diğer kullanıcıları laboratuvar Oluşturucu rolüne ekleyebilir: [Laboratuvar Oluşturucu rolüne kullanıcı ekleyin](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Sınıf laboratuvarı oluşturma

1. [Azure Lab Services web sitesine](https://labs.azure.com) gidin. Internet Explorer 11 ' in henüz desteklenmediğini unutmayın. 
2. **Oturum aç '** ı seçin. Laboratuvar hesabındaki **Laboratuvar Oluşturucu** rolünün bir üyesi olan BIR **Kullanıcı kimliği** seçin veya girin ve parola girin. Azure Lab Services, kuruluş hesaplarını ve Microsoft hesaplarını destekler. 
3. **Yeni Laboratuvar** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. Laboratuvarınız için bir **ad** belirtin. 
    2. Laboratuvardaki en fazla **sanal makine sayısını** belirtin. Laboratuvardaki sanal makinelerin sayısını daha sonra artırabilir veya azaltabilirsiniz. 
    6. **Kaydet**’i seçin.

        ![Sınıf laboratuvarı oluşturma](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. **Sanal makine özelliklerini seçin** sayfasında aşağıdaki adımları gerçekleştirin:
    1. Laboratuvarda oluşturulan sanal makineler (VM) için bir **boyut** seçin. Şu anda, **küçük**, **Orta**, **Orta (sanallaştırma)** , **büyük**ve **GPU** boyutlarına izin verilir. Ayrıntılar için bkz. [VM boyutları](#vm-sizes) bölümü.
    1. VM'lerin oluşturulmasını istediğiniz **bölgeyi** seçin. 
    1. Laboratuvardaki VM'leri oluşturmak için kullanılacak **VM görüntüsünü** seçin. Bir Linux görüntüsü seçerseniz, bunun için Uzak Masaüstü bağlantısını etkinleştirme seçeneğini görürsünüz. Ayrıntılar için bkz. [Linux için Uzak Masaüstü bağlantısını etkinleştirme](how-to-enable-remote-desktop-linux.md).
    1. **İleri**’yi seçin.

        ![VM özellikleri belirtme](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
5. **Kimlik bilgilerini ayarla** sayfasında laboratuvardaki tüm VM'ler için varsayılan kimlik bilgilerini belirtin. 
    1. Laboratuvardaki tüm VM'ler için **kullanıcı adını** belirtin.
    2. Kullanıcının **parolasını** belirtin. 

        > [!IMPORTANT]
        > Kullanıcı adını ve parolayı not edin. Bunlar tekrar gösterilmeyecektir.
    3. Öğrencilerin kendi parolalarını ayarlamanızı istiyorsanız **tüm sanal makineler için aynı parolayı kullan** seçeneğini devre dışı bırakın. Bu adım **isteğe bağlıdır**. 

        Öğretmen, laboratuvardaki tüm VM 'Ler için aynı parolayı kullanmayı seçebilir veya öğrencilerin VM 'Leri için parola değiştirmesine izin verebilir. Varsayılan olarak, bu ayar Ubuntu hariç tüm Windows ve Linux görüntüleri için etkinleştirilmiştir. **Ubuntu** VM ' yi seçtiğinizde, bu ayar devre dışıdır, bu nedenle ilk kez oturum açtıklarında öğrencilerin bir parola ayarlaması istenir.
    1. **Oluştur**’u seçin. 

        ![Kimlik bilgilerini ayarla](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. **Şablonu yapılandır** sayfasında laboratuvar oluşturma işleminin durumunu görebilirsiniz. Laboratuvar şablonunun oluşturulması 20 dakika sürebilir. Laboratuvardaki şablon, tüm kullanıcıların sanal makinelerinin oluşturulduğu bir temel sanal makine görüntüsüdür. Şablon sanal makinesini, tam olarak laboratuvar kullanıcılarına sağlamak istediklerinizle yapılandırılacak şekilde ayarlayın.  

    ![Şablonu yapılandır](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Şablon yapılandırma işlemleri tamamlandıktan sonra şu sayfayı görürsünüz: 

    ![Tamamlanmış şablon yapılandırma sayfası](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. Bu öğreticide aşağıdaki adımlar isteğe bağlıdır: 
    2. **Bağlan**'ı seçerek şablon VM'sine bağlanın. Bir Linux şablon sanal makinesi ise, SSH veya RDP kullanarak bağlanmak isteyip istemediğinizi seçin (RDP etkinse).
    1. VM 'nin parolasını sıfırlamak için **Parolayı Sıfırla** ' yı seçin. 
    1. Şablon VM'sinde yazılım yükleme ve yapılandırma işlemlerini gerçekleştirin. 
    1. VM'yi **durdurun**.  
    1. Şablon için bir **açıklama** girin
9. Şablon sayfasında **İleri**'yi seçin. 
10. **Şablonu yayımla** sayfasında aşağıdaki işlemleri gerçekleştirin. 
    1. Şablonu hemen yayımlamak için *Şablonu yayımladıktan sonra değiştiremeyeceğimi anlıyorum. Bu işlem yalnızca bir kez gerçekleştirilebilir ve bir saat sürebilir* onay kutusunu işaretleyip **Yayımla**'yı seçin.  Şablon sanal makinesinin laboratuvar kullanıcılarınız tarafından kullanılabilmesi için şablonu yayımlayın.

        > [!WARNING]
        > Yayımlama işlemini geri alamazsınız. 
    2. Daha sonra yayımlamak istiyorsanız **Sonrası için kaydet**'i seçin. Şablon VM'sini sihirbaz tamamlandıktan sonra yayımlayabilirsiniz. Sihirbaz tamamlandıktan sonra yapılandırma ve yayımlama hakkında daha fazla bilgi için, Sihirbaz tamamlandıktan sonra yapılandırma ve yayımlama hakkında daha fazla bilgi Için bkz. [sınıf laboratuvarlarını yönetme](how-to-manage-classroom-labs.md) makalesindeki şablonu yayımlama bölümü.

        ![Şablonu yayımlama](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Şablonun **yayımlama ilerleme durumunu** görürsünüz. Bu işlemin tamamlanması bir saat sürebilir. 

    ![Şablonu yayımlama - ilerleme](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Şablon başarıyla yayımlandığında aşağıdaki sayfayı görürsünüz. **Done** (Bitti) öğesini seçin.

    ![Şablonu yayımlama - başarılı](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Laboratuvar **panosunu** görürsünüz. 
    
    ![Sınıf laboratuvarı panosu](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. **Sanal makineler** sayfasına geçin ve **Atanmamış** durumundaki sanal makineleri gördüğünüzü doğrulayın. Bu VM’ler henüz bir öğrenciye atanmamıştır. Bu makinelerin durumu **Durduruldu** olmalıdır. Bu sayfadan bir öğrenci VM'sini başlatabilir, VM'ye bağlanabilir, VM'yi durdurabilir ve VM'yi silebilirsiniz. VM'leri bu sayfadan başlatabilir veya öğrencilerinizin başlatmasını sağlayabilirsiniz. 

    ![Durdurulmuş durumdaki sanal makineler](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

### <a name="vm-sizes"></a>VM boyutları  

| Size | Çekirdek | RAM | Açıklama | 
| ---- | ----- | --- | ----------- | 
| Küçük | 2 | 3,5 GB | Bu boyut, komut satırı, Web tarayıcısı, düşük trafikli web sunucuları, küçük ve orta ölçekli veritabanları için idealdir. |
| Orta | 4 | 7 GB | Bu boyut, ilişkisel veritabanları, bellek içi önbelleğe alma ve analiz için idealdir | 
| Orta (Iç Içe sanallaştırma) | 4 | 16 GB | Bu boyut, ilişkisel veritabanları, bellek içi önbelleğe alma ve analiz için idealdir. Bu boyut, iç içe sanallaştırmayı da destekler. <p>Bu boyut, her öğrencinin birden çok VM gerektiren senaryolarda kullanılabilir. Öğretmenler, sanal makine içinde birkaç küçük boyutlu iç içe sanal makine ayarlamak için iç içe sanallaştırmayı kullanabilir. </p> |
| Büyük | 8 | 32 GB | Bu boyut daha hızlı CPU, daha iyi yerel disk performansı, büyük veritabanları, büyük bellek önbellekler gerektiren uygulamalar için idealdir. Bu boyut, iç içe sanallaştırmayı da destekler |  
| Küçük GPU (görselleştirme) | 6 | 56 GB | Bu boyut, OpenGL ve DirectX gibi çerçeveleri kullanarak uzaktan görselleştirme, akış, oyun ve kodlama için idealdir. | 
| Küçük GPU (Işlem) | 6 | 56 GB | Bu boyut, yapay zeka ve derin öğrenme uygulamaları gibi işlem yoğunluğu ve yoğun ağ kullanımı gerektiren uygulamalar için idealdir. | 
| Orta ölçekli GPU (görselleştirme) | 12 | 112 GB | Bu boyut, OpenGL ve DirectX gibi çerçeveleri kullanarak uzaktan görselleştirme, akış, oyun ve kodlama için idealdir. | 

## <a name="view-all-classroom-labs"></a>Tüm sınıf laboratuvarlarını görüntüleyin
1. [Azure Lab Services Portal](https://labs.azure.com)' a gidin.
2. **Oturum aç '** ı seçin. Laboratuvar hesabındaki **Laboratuvar Oluşturucu** rolünün bir üyesi olan BIR **Kullanıcı kimliği** seçin veya girin ve parola girin. Azure Lab Services, kuruluş hesaplarını ve Microsoft hesaplarını destekler. 
3. Seçilen laboratuvar hesabındaki tüm laboratuvarları görtığınızdan emin olun. 

    ![Tüm laboratuvarlar](../media/how-to-manage-classroom-labs/all-labs.png)
3. En üstteki açılan listeyi kullanarak farklı bir laboratuvar hesabı seçin. Laboratuvarları seçilen laboratuvar hesabında görürsünüz. 

## <a name="delete-a-classroom-lab"></a>Sınıf Laboratuvarı silme
1. Laboratuvarın kutucuğunda, köşedeki üç nokta (...) seçeneğini belirleyin. 

    ![Laboratuvarı seçme](../media/how-to-manage-classroom-labs/select-three-dots.png)
2. **Sil**’i seçin. 

    ![Sil düğmesi](../media/how-to-manage-classroom-labs/delete-button.png)
3. **Laboratuvar Sil** Iletişim kutusunda **Sil**' i seçin. 

    ![İletişim kutusunu Sil](../media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)

## <a name="switch-to-another-classroom-lab"></a>Başka bir sınıf laboratuvarına geç
Geçerli olan başka bir sınıf laboratuvarına geçiş yapmak için, en üstteki laboratuvar hesabındaki laboratuvarların açılan listesini seçin.

![Üstteki aşağı açılan listeden laboratuvar listesini seçin](../media/how-to-manage-classroom-labs/switch-lab.png)


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Laboratuvar sahibi olarak, şablonları ayarlama ve yayımlama](how-to-create-manage-template.md)
- [Laboratuvar sahibi olarak, bir laboratuvarın kullanımını yapılandırma ve denetleme](how-to-configure-student-usage.md)
- [Laboratuvar kullanıcısı olarak, sınıf laboratuvarlarına erişin](how-to-use-classroom-lab.md)

