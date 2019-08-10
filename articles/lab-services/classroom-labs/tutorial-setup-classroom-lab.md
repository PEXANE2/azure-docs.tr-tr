---
title: Azure Lab Services kullanarak sınıf laboratuvarı ayarlama | Microsoft Docs
description: Bu öğreticide, bir sınıfta kullanılacak laboratuvar ayarlarsınız.
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
ms.date: 06/11/2019
ms.author: spelluru
ms.openlocfilehash: 964ecca015e440439885bbbd85cb720a3abd10a9
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68883508"
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Öğretici: Bir sınıf laboratuvarı ayarlama 
Bu öğreticide, sınıftaki öğrenciler tarafından kullanılan sanal makinelerle bir sınıf laboratuvarı ayarlayacaksınız.  

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Sınıf laboratuvarı oluşturma
> * Laboratuvara Kullanıcı ekleme
> * Öğrencilere kayıt bağlantısı gönderme

## <a name="prerequisites"></a>Önkoşullar
Laboratuvar hesabında bir sınıf Laboratuvarı ayarlamak için, laboratuvar hesabındaki Bu rollerden birinin üyesi olmanız gerekir: Sahip, laboratuvar Oluşturucu veya katkıda bulunan. Laboratuvar hesabı oluşturmak için kullandığınız hesap, sahip rolüne otomatik olarak eklenir.

Laboratuvar sahibi, başka kullanıcıları **Laboratuvar Oluşturucu** rolüne ekleyebilir. Örneğin, bir laboratuar sahibi, laboratuvar Oluşturucu rolüne meslektedir ekler. Daha sonra, mesleler, sınıfları için VM 'Ler ile laboratuvarları oluşturur. Öğrenciler, mesleler tarafından laboratuvara kaydolmak için aldıkları kayıt bağlantısını kullanır. Kaydolduktan sonra, iş ve ev işlerini yapmak için laboratuvarlarda VM 'Leri kullanabilirler. Laboratuvar Oluşturucu rolüne kullanıcı ekleme hakkında ayrıntılı adımlar için bkz. [Laboratuvar Oluşturucu rolüne kullanıcı ekleme](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).


## <a name="create-a-classroom-lab"></a>Sınıf laboratuvarı oluşturma

1. [Azure Lab Services web sitesine](https://labs.azure.com) gidin. Internet Explorer 11 ' in henüz desteklenmediğini unutmayın. 
2. **Oturum aç**’ı seçip kimlik bilgilerinizi girin. Azure Lab Services, kuruluş hesaplarını ve Microsoft hesaplarını destekler. 
3. **Yeni Laboratuvar** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. Laboratuvarınız için bir **ad** belirtin. 
    2. Laboratuvardaki en fazla **sanal makine sayısını** belirtin. Laboratuvar oluşturduktan sonra veya var olan bir laboratuvarda sanal makine sayısını artırabilir veya azaltabilirsiniz. Daha fazla bilgi için bkz. [bir laboratuvarda sanal makine sayısını güncelleştirme](how-to-configure-student-usage.md#update-number-of-virtual-machines-in-lab)
    6. **Kaydet**’i seçin.

        ![Sınıf laboratuvarı oluşturma](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. **Sanal makine özelliklerini seçin** sayfasında aşağıdaki adımları gerçekleştirin:
    1. Laboratuvarda oluşturulan sanal makineler (VM) için bir **boyut** seçin. Şu anda, **küçük**, **Orta**, **Orta (sanallaştırma)** , **büyük**ve **GPU** boyutlarına izin verilir.
    3. Laboratuvardaki VM'leri oluşturmak için kullanılacak **VM görüntüsünü** seçin. Bir Linux görüntüsü seçerseniz, bunun için Uzak Masaüstü bağlantısını etkinleştirme seçeneğini görürsünüz. Ayrıntılar için bkz. [Linux için Uzak Masaüstü bağlantısını etkinleştirme](how-to-enable-remote-desktop-linux.md).
    4. **İleri**’yi seçin.

        ![VM özellikleri belirtme](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
5. **Kimlik bilgilerini ayarla** sayfasında laboratuvardaki tüm VM'ler için varsayılan kimlik bilgilerini belirtin. 
    1. Laboratuvardaki tüm VM'ler için **kullanıcı adını** belirtin.
    2. Kullanıcının **parolasını** belirtin. 

        > [!IMPORTANT]
        > Kullanıcı adını ve parolayı not edin. Bunlar tekrar gösterilmeyecektir.
    3. **Oluştur**’u seçin. 

        ![Kimlik bilgilerini ayarla](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. **Şablonu yapılandır** sayfasında laboratuvar oluşturma işleminin durumunu görebilirsiniz. Laboratuvar şablonunun oluşturulması 20 dakika sürebilir. 

    ![Şablonu yapılandır](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Şablon yapılandırma işlemleri tamamlandıktan sonra şu sayfayı görürsünüz: 

    ![Tamamlanmış şablon yapılandırma sayfası](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. **Şablonu Yapılandır** sayfasında, aşağıdaki adımları uygulayın: Bu adımlar öğretici için **isteğe bağlıdır** .
    2. **Bağlan**'ı seçerek şablon VM'sine bağlanın. Bir Linux şablon sanal makinesi ise, SSH veya RDP kullanarak bağlanmak isteyip istemediğinizi seçin (RDP etkinse).
    1. VM 'nin parolasını sıfırlamak için **Parolayı Sıfırla** ' yı seçin. 
    1. Şablon VM'sinde yazılım yükleme ve yapılandırma işlemlerini gerçekleştirin. 
    1. VM'yi **durdurun**.  
    1. Şablon için bir **açıklama** girin
9. Şablon sayfasında **İleri**'yi seçin. 
10. **Şablonu yayımla** sayfasında aşağıdaki işlemleri gerçekleştirin. 
    1. Şablonu hemen yayınlamak için **Yayımla**' yı seçin.  

        > [!WARNING]
        > Yayımlama işlemini geri alamazsınız. 
    2. Daha sonra yayımlamak istiyorsanız **Sonrası için kaydet**'i seçin. Sihirbaz bittikten sonra şablon VM 'yi yayımlayabilirsiniz. Sihirbaz bittikten sonra yapılandırma ve yayımlama hakkında daha fazla bilgi için bkz. [sınıf laboratuvarlarını yönetme](how-to-manage-classroom-labs.md) makalesindeki [şablonu yayımlama](how-to-create-manage-template.md#publish-the-template-vm) bölümü.

        ![Şablonu yayımlama](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Şablonun **yayımlama ilerleme durumunu** görürsünüz. Bu işlemin tamamlanması bir saat sürebilir. 

    ![Şablonu yayımlama - ilerleme](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Şablon başarıyla yayımlandığında aşağıdaki sayfayı görürsünüz. **Done** (Bitti) öğesini seçin.

    ![Şablonu yayımlama - başarılı](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Laboratuvar **panosunu** görürsünüz. 
    
    ![Sınıf laboratuvarı panosu](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. Sol taraftaki menüden sanal makineler ' i seçerek veya sanal makineler kutucuğunu seçerek **sanal makineler** sayfasına geçin. **Atanmamış** durumda olan sanal makineleri görtığınızdan emin olun. Bu VM’ler henüz bir öğrenciye atanmamıştır. Bu makinelerin durumu **Durduruldu** olmalıdır. Bu sayfadan bir öğrenci VM'sini başlatabilir, VM'ye bağlanabilir, VM'yi durdurabilir ve VM'yi silebilirsiniz. VM'leri bu sayfadan başlatabilir veya öğrencilerinizin başlatmasını sağlayabilirsiniz. 

    ![Durdurulmuş durumdaki sanal makineler](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

## <a name="add-users-to-the-lab"></a>Laboratuvara Kullanıcı ekleme

1. Sol menüdeki **Kullanıcılar** ' ı seçin. Varsayılan olarak, **erişimi kısıtla** seçeneği etkinleştirilmiştir. Bu ayar açık olduğunda, Kullanıcı kullanıcılar listesinde olmadığı takdirde Kullanıcı kayıt bağlantısına sahip olsa bile, Kullanıcı laboratuvara kayıt yapamıyor. Yalnızca listedeki kullanıcılar, göndereceğiniz kayıt bağlantısını kullanarak laboratuvara kayıt yapabilir. Bu yordamda, kullanıcıları listeye eklersiniz. Alternatif olarak, kayıt bağlantısı olduğu sürece kullanıcıların laboratuvara kaydolmaları için **erişimi kısıtla**' yı devre dışı bırakabilirsiniz. 
2. Araç çubuğundan **Kullanıcı Ekle** ' yi seçin. 

    ![Kullanıcı Ekle düğmesi](../media/how-to-configure-student-usage/add-users-button.png)
1. **Kullanıcı Ekle** sayfasında, kullanıcıların e-posta adreslerini ayrı satırlarda veya noktalı virgülle ayrılmış tek bir satıra girin. 

    ![Kullanıcı e-posta adresleri Ekle](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. **Kaydet**’i seçin. Kullanıcıların e-posta adreslerini ve bunların durumlarını (kayıtlı veya değil) listede görürsünüz. 

    ![Kullanıcı listesi](../media/how-to-configure-student-usage/users-list-new.png)

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

## <a name="set-a-schedule-for-the-lab"></a>Laboratuvar için zamanlama ayarlama
Kota ayarını **0 saat (yalnızca zamanlama)** olarak yapılandırdıysanız, laboratuvar için bir zamanlama ayarlamanız gerekir. Bu öğreticide, zamanlamayı yinelenen haftalık zamanlama olarak ayarlarsınız.

1. **Zamanlamalar** sayfasına geçin ve araç çubuğunda **zamanlama Ekle** ' yi seçin. 

    ![Zamanlamalar sayfasında zamanlama Ekle düğmesi](../media/how-to-create-schedules/add-schedule-button.png)
2. **Zamanlama Ekle** sayfasında, en üstte **haftalık** olarak geçiş yapın. 
3. **Zamanlama günleri (gerekli)** için, zamanlamanın etkili olmasını istediğiniz günleri seçin. Aşağıdaki örnekte Pazartesi-Cuma seçilidir. 
4. **Kimden** alanı için **Zamanlama başlangıç tarihini** girin veya **Takvim** düğmesini seçerek bir tarih seçin. Bu alan zorunludur. 
5. **Zamanlama bitiş tarihi**Için, VM 'lerin kapatılacak bitiş tarihini girin veya seçin. 
6. **Başlangıç zamanı**Için, VM 'lerin başlamasını istediğiniz saati seçin. Durdurma zamanı ayarlanmamışsa başlangıç zamanı gereklidir. Yalnızca durdurma zamanını belirtmek istiyorsanız **Başlangıç olayını kaldır** ' ı seçin. **Başlangıç saati** devre dışıysa, etkinleştirmek için açılan listenin yanındaki **Başlangıç olayı Ekle** ' yi seçin. 
7. **Durdurma zamanı**Için, sanal makinelerin kapatılmasını istediğiniz saati seçin. Başlangıç saati ayarlanmamışsa durdurma zamanı gereklidir. Yalnızca başlangıç saatini belirtmek istiyorsanız **durdurma olayını kaldır** ' ı seçin. **durdurma zamanı** devre dışıysa, etkinleştirmek için açılan listenin yanındaki **Durdur olayını Ekle** ' yi seçin.
8. **Saat dilimi (gerekli)** için belirttiğiniz başlangıç ve durdurma zamanları için saat dilimini seçin.  
9. **Notlar**için, zamanlama için herhangi bir açıklama veya Not girin. 
10. **Kaydet**’i seçin. 

    ![Haftalık zamanlama](../media/how-to-create-schedules/add-schedule-page-weekly.png)

## <a name="send-an-email-with-the-registration-link"></a>Kayıt bağlantısı ile bir e-posta gönderin

1. Zaten sayfada değilseniz, **Kullanıcılar** görünümüne geçin. 
2. Listeden belirli veya tüm kullanıcıları seçin. Belirli kullanıcıları seçmek için, listenin ilk sütunundaki onay kutularını seçin. Tüm kullanıcıları seçmek için, ilk sütunun başlığının önünde onay kutusunu seçin (**ad**) veya listedeki tüm kullanıcılar için tüm onay kutularını seçin. **Davet durumunun** durumunu bu listede görebilirsiniz.  Aşağıdaki görüntüde tüm öğrenciler için davetiye durumu **davetiye gönderilmedi**olarak ayarlanmıştır. 

    ![Öğrencileri seçin](../media/tutorial-setup-classroom-lab/select-students.png)
1. Satırlardan birindeki **e-posta simgesini (zarf)** seçin (veya) araç çubuğunda **daveti Gönder** ' i seçin. Ayrıca, e-posta simgesini görmek için fare işaretçisini listede bir öğrenci adı üzerinde gezdirin. 

    ![Kayıt bağlantısını e-posta ile gönder](../media/tutorial-setup-classroom-lab/send-email.png)
4. **Bağlantıyı e-posta Ile gönder** sayfasında, aşağıdaki adımları izleyin: 
    1. Öğrencilerine göndermek istediğiniz **isteğe bağlı bir ileti** yazın. E-posta otomatik olarak kayıt bağlantısını içerir. 
    2. **Bağlantıyı e-posta Ile gönder** sayfasında **Gönder**' i seçin. Davet **göndermek** ve ardından **gönderilmek**üzere davet değişikliği durumunu görürsünüz. 
        
        ![Gönderilen Davetler](../media/tutorial-setup-classroom-lab/invitations-sent.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir sınıf laboratuvarı oluşturdunuz ve laboratuvarı yapılandırdınız. Bir öğrencinin, kayıt bağlantısını kullanarak laboratuvardaki bir sanal makineye nasıl erişebileceğinizi öğrenmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Sınıf laboratuvarındaki bir sanal makineye bağlanma](tutorial-connect-virtual-machine-classroom-lab.md)

