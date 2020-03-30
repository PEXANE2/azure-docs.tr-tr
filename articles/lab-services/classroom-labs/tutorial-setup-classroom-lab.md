---
title: Azure Lab Services kullanarak sınıf laboratuvarı ayarlama | Microsoft Docs
description: Bu eğitimde, sınıfınızdaki öğrenciler tarafından kullanılan sanal makinelerle bir sınıf laboratuvarı oluşturmak için Azure Lab Hizmetlerini kullanırsınız.
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
ms.openlocfilehash: 166ec4db2a2891d25a1e80526f8c1bd9770f9eef
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77592229"
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Öğretici: Bir sınıf laboratuvarı ayarlama 
Bu öğreticide, sınıftaki öğrenciler tarafından kullanılan sanal makinelerle bir sınıf laboratuvarı ayarlayacaksınız.  

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Sınıf laboratuvarı oluşturma
> * Kullanıcıları laboratuvara ekleme
> * Laboratuvar için zamanlama yı ayarlama
> * Öğrencilere davet e-postası gönderme

## <a name="prerequisites"></a>Ön koşullar
Bu eğitimde, sınıfınız için sanal makineleriçeren bir laboratuvar kurdunuz. Bir laboratuvar hesabında sınıf laboratuvarı kurmak için, laboratuvar hesabındaki bu rollerden birinin üyesi olmalısınız: Sahibi, Laboratuvar Oluşturucusu veya Katkıda Bulunan. Laboratuvar hesabı oluşturmak için kullandığınız hesap otomatik olarak sahip rolüne eklenir. Bu nedenle, bir sınıf laboratuvarı oluşturmak için bir laboratuvar hesabı oluşturmak için kullandığınız kullanıcı hesabını kullanabilirsiniz. 

Azure Laboratuvar Hizmetleri'ni kullanırken tipik iş akışı aşağıda veda edinilir:

1. Bir laboratuvar hesabı oluşturucusu, Diğer kullanıcıları **Lab Creator** rolüne ekler. Örneğin, laboratuvar hesabı oluşturucusu/yöneticisi, sınıfları için laboratuvarlar oluşturabilmeleri için **Laboratuvar Oluşturucurolüne** profesörler ekler. 
2. Daha sonra, profesörler sınıfları için VM'ler içeren laboratuvarlar oluşturur ve sınıftaki öğrencilere kayıt bağlantıları gönderirler. 
3. Öğrenciler laboratuvara kayıt yaptırmak için profesörlerden aldıkları kayıt bağlantısını kullanırlar. Bir kez onlar kayıtlı, onlar sınıf çalışması ve ev işi yapmak için laboratuarlarda VM kullanabilirsiniz. 

## <a name="create-a-classroom-lab"></a>Sınıf laboratuvarı oluşturma
Bu adımda, Azure'da sınıfınız için bir laboratuvar oluşturursunuz. 

1. [Azure Lab Services web sitesine](https://labs.azure.com) gidin. Internet Explorer 11'in henüz desteklenmediğini unutmayın. 
2. **Oturum aç**’ı seçip kimlik bilgilerinizi girin. Azure Lab Services, kuruluş hesaplarını ve Microsoft hesaplarını destekler. 
3. **Yeni laboratuarı**seçin. 
    
    ![Sınıf laboratuvarı oluşturma](../media/tutorial-setup-classroom-lab/new-lab-button.png)
4. **Yeni Laboratuvar** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. Laboratuvarınız için bir **ad** belirtin ve **İleri'yi**seçin.  

        ![Sınıf laboratuvarı oluşturma](../media/tutorial-setup-classroom-lab/new-lab-window.png)
    2. Sanal **makine kimlik bilgileri** sayfasında, laboratuardaki tüm VM'ler için varsayılan kimlik bilgilerini belirtin. Kullanıcının **adını** ve **parolasını** belirtin ve sonra **İleri'yi**seçin.  

        ![Yeni laboratuvar penceresi](../media/tutorial-setup-classroom-lab/virtual-machine-credentials.png)

        > [!IMPORTANT]
        > Kullanıcı adını ve parolayı not edin. Bunlar tekrar gösterilmeyecektir.
    3. Laboratuvar **ilkeleri** sayfasında **Finish'i**seçin. 

        ![Her kullanıcı için kota](../media/tutorial-setup-classroom-lab/quota-for-each-user.png)
5. Şablon VM oluşturma durumunu gösteren aşağıdaki ekranı görmeniz gerekir. Bu işlem 20 dakika kadar sürer. 

    ![Şablon VM oluşturma durumu](../media/tutorial-setup-classroom-lab/create-template-vm-progress.png)
8. **Şablon** sayfasında aşağıdaki adımları yapın: Bu adımlar öğretici için **isteğe bağlıdır.**

    1. **Bağlan**'ı seçerek şablon VM'sine bağlanın. Linux şablonu VM ise, SSH veya RDP kullanarak bağlanmak isteyip istemediğinizi (RDP etkinse) seçersiniz.
    3. Sınıfınız için gerekli yazılımı VM şablonuna yükleyin ve yapılandırın. 
    4. VM şablonuna **durdurun.**  

## <a name="publish-the-template-vm"></a>Şablon VM'yi yayımlama
Bu adımda, VM şablonu yayımlarsınız. VM şablonu yayımladığınızda, Azure Lab Hizmetleri şablonu kullanarak laboratuvarda VM'ler oluşturur. Tüm sanal makineler, şablonla aynı yapılandırmaya sahiptir.

1. **Şablon** sayfasında, araç çubuğunda **Yayımla'yı** seçin. 

    ![Şablon düğmesini yayımla](../media/tutorial-setup-classroom-lab/template-page-publish-button.png)

    > [!WARNING]
    > Yayımlama işlemini geri alamazsınız. 
2. **Şablonu Yayımla** sayfasında, laboratuvarda oluşturmak istediğiniz sanal makine sayısını girin ve ardından **Yayımla'yı**seçin. 

    ![Şablon yayımla - VM sayısı](../media/tutorial-setup-classroom-lab/publish-template-number-vms.png)
3. Şablonu sayfada **yayımlama durumunu** görürsünüz. Bu işlemin tamamlanması bir saat sürebilir. 

    ![Şablonu yayımlama - ilerleme](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
4. Yayımlama tamamlanana kadar bekleyin ve sol menüdeki Sanal **makineleri** seçerek veya **Sanal makineler** döşemesini seçerek **Sanal makineler havuzu** sayfasına geçin. Atanmamış durumda olan sanal makineleri gördüğünüzden **onaylayın.** Bu VM’ler henüz bir öğrenciye atanmamıştır. Bu makinelerin durumu **Durduruldu** olmalıdır. Bu sayfadan bir öğrenci VM'sini başlatabilir, VM'ye bağlanabilir, VM'yi durdurabilir ve VM'yi silebilirsiniz. VM'leri bu sayfadan başlatabilir veya öğrencilerinizin başlatmasını sağlayabilirsiniz. 

    ![Durdurulmuş durumdaki sanal makineler](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)   

    > [!NOTE]
    > Bir eğitimci öğrenci nin VM'sini açarsa, öğrencinin kontenjanı etkilenmez. Kullanıcı kotası, kullanıcının zamanlanan ders saati dışında kullanabileceği laboratuvar saatsayısını belirtir. Kotalar hakkında daha fazla bilgi için, [kullanıcılar için kota ları ayarla'ya](how-to-configure-student-usage.md?#set-quotas-for-users)bakın.

## <a name="set-a-schedule-for-the-lab"></a>Laboratuvar için bir zamanlama ayarlama
Laboratuardaki VM'lerin belirli zamanlarda otomatik olarak başlatılabilmesi/durdurulması için laboratuvar için zamanlanmış bir olay oluşturun. Daha önce belirttiğiniz kullanıcı kotası (varsayılan: 10 saat), bu zamanlanan saat dışında her kullanıcıya atanan ek süredir. 

1. **Zamanlamalar** sayfasına geçin ve araç çubuğunda **zamanlanmış olay ekle'yi** seçin. 

    ![Zamanlamalar sayfasında zamanlama düğmesi ekleme](../media/how-to-create-schedules/add-schedule-button.png)
2. **Zamanlanan olay** ekle sayfasında aşağıdaki adımları yapın:
    1. **Standart'ın** Olay **türü**seçildiğini onaylayın.  
    2. Sınıfın **başlangıç tarihini** seçin. 
    4. VM'lerin başlatılmasını istediğiniz **başlangıç saatini** seçin.
    5. VM'lerin kapatılacağınız **durak saatini** seçin. 
    6. Belirttiğiniz başlangıç ve durdurma saatleri için **saat dilimini** seçin. 
3. Aynı **Zamanlı olay ekle** sayfasında, Yinele bölümündeki geçerli zamanlamayı seçin. **Repeat**  

    ![Zamanlamalar sayfasında zamanlama düğmesi ekleme](../media/how-to-create-schedules/select-current-schedule.png)
5. **Yinele** iletişim kutusunda aşağıdaki adımları yapın:
    1. **Her** **hafta** tekrar alanı için ayarlı olduğunu doğrulayın. 
    2. Zamanlamanın etkili olmasını istediğiniz günleri seçin. Aşağıdaki örnekte, Pazartesi-Cuma seçilir. 
    3. Zamanlama için bir **bitiş tarihi** seçin.
    8. **Kaydet'i**seçin. 

        ![Yineleme çizelgesini ayarlama](../media/how-to-create-schedules/set-repeat-schedule.png)

3. Şimdi, Notlar için **zamanlanan olay** ekle sayfasında **(isteğe bağlı)** zamanlama için herhangi bir açıklama veya not girin. 
4. **Zamanlanan olay** ekle sayfasında **Kaydet'i**seçin. 

    ![Haftalık program](../media/how-to-create-schedules/add-schedule-page-weekly.png)
5. Zamanlamanın ayarlı olduğunu doğrulamak için takvimdeki başlangıç tarihine gidin.
    
    ![Takvimde zamanlama](../media/how-to-create-schedules/schedule-calendar.png)

    Bir sınıfın zamanlamaları oluşturma ve yönetme hakkında daha fazla bilgi için sınıf [laboratuarları için zamanlama oluştur ve yönet'](how-to-create-schedules.md)e bakın.


## <a name="add-users-to-the-lab"></a>Kullanıcıları laboratuvara ekleme

1. Sol menüde **Kullanıcılar'ı** seçin. Varsayılan olarak, **erişimi kısıtla** seçeneği etkinleştirilir. Bu ayar ayarı olduğunda, kullanıcı kullanıcı listesinde olmadığı sürece kayıt bağlantısı olsa bile kullanıcı laboratuvara kaydolamaz. Yalnızca listedeki kullanıcılar gönderdiğiniz kayıt bağlantısını kullanarak laboratuvara kaydolabilir. Bu yordamda, kullanıcıları listeye eklersiniz. Alternatif olarak, kullanıcıların kayıt bağlantısı na sahip oldukları sürece laboratuvara kaydolmalarını sağlayan **Erişimi Kısıtla'yı**kapatabilirsiniz. 
2. Araç çubuğunda **kullanıcı ekle'yi** seçin ve ardından **e-posta adresine göre ekle'yi**seçin. 

    ![Kullanıcı ekle düğmesi](../media/how-to-configure-student-usage/add-users-button.png)
1. Kullanıcı **Ekle** sayfasında, kullanıcıların e-posta adreslerini ayrı satırlara veya yarı sütunlarla ayrılmış tek bir satıra girin. 

    ![Kullanıcı e-posta adresleri ekleme](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. **Kaydet'i**seçin. Listede kullanıcıların e-posta adreslerini ve durumlarını (kayıtlı veya değil) görürsünüz. 

    ![Kullanıcı listesi](../media/how-to-configure-student-usage/users-list-new.png)

    Laboratuvara kaydolduktan sonra listede kullanıcıların adlarını görürsünüz. 
    

## <a name="send-invitation-emails-to-users"></a>Kullanıcılara davet e-postaları gönderme

1. Sayfada zaten değilseniz **Kullanıcılar** görünümüne geçin ve araç çubuğundaki **Tümünü Davet** et'i seçin. 

    ![Öğrencileri seçin](../media/tutorial-setup-classroom-lab/invite-all-button.png)
1. **E-posta** yla Davetiye Gönder sayfasında isteğe bağlı bir ileti girin ve ardından **Gönder'i**seçin. E-posta otomatik olarak kayıt bağlantısını içerir. Bu kayıt linkini seçerek alabilirsiniz **... (elips)** araç çubuğunda ve **Kayıt bağlantısında.** 

    ![Kayıt bağlantısını e-posta ile gönder](../media/tutorial-setup-classroom-lab/send-email.png)
4. **Davetdurumunu Kullanıcılar** **invitation** listesinde görürsünüz. Durum **Gönderme** ve daha sonra **tarihinde &lt;&gt;Gönderildi**olarak değiştirilmelidir. 

    Bir sınıfa öğrenci ekleme ve laboratuvar kullanımlarını yönetme hakkında daha fazla bilgi için [öğrenci kullanımını nasıl yapılandırılabilirsiniz.](how-to-configure-student-usage.md)

## <a name="next-steps"></a>Sonraki adımlar
Bu eğitimde, Azure'da sınıfınız için bir laboratuvar oluşturdunuz. Bir öğrencinin, kayıt bağlantısını kullanarak laboratuvardaki bir sanal makineye nasıl erişebileceğinizi öğrenmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Sınıf laboratuvarındaki bir sanal makineye bağlanma](tutorial-connect-virtual-machine-classroom-lab.md)

