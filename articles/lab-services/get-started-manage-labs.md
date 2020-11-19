---
title: Azure Lab Services kullanmaya başlayın
description: Bu makalede Azure Lab Services ile çalışmaya başlama açıklanmaktadır.
ms.topic: article
ms.date: 11/18/2020
ms.openlocfilehash: bdc4f4af06d70a1798e7409b78f6b47530ddd7f4
ms.sourcegitcommit: f6236e0fa28343cf0e478ab630d43e3fd78b9596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2020
ms.locfileid: "94917112"
---
# <a name="get-started-with-lab-services"></a>Laboratuvar Hizmetleri 'ni kullanmaya başlama 

Azure Lab Services, doğrudan kendi bilgisayarlarından sanal bilgisayar laboratuvarlarına erişimi olan öğrenciler ve öğretmenler sağlar.

Öğretmenler, öğrenciler/ebeveynler için, tek bir öğrenciye verilen donanımlar aracılığıyla kendi yönergesinden Laboratuvar Hizmetleri 'ni kullanma konusunda bilgi sahibi olmak zorunda. Sonuç olarak, öğrenciler, sanal makineler (VM) aracılığıyla kendi çalışma programları için gereken sektör standardı yazılımlara erişebiliyor. 

VM, sanal bilgisayar olarak davranan sanal bir ortamdır. VM 'Lerin kendi işlemcisi, belleği ve depolaması vardır. VM 'Ler gerçek bir makine için bir alternatif sağlar ve kullanıcılara kendi cihazlarında gerek duymadan, işletim sistemlerine ve yazılıma erişim izni verebilir. Azure Lab Services, öğrenciler için VM 'lere erişmek ve bunlara gitmek ve personelin sanal bilgisayar laboratuvarlarını yönetmesi için bir araç sağlar. 

Bu makalede, öğrenciler/üst üste Azure Lab Services kullanmak için nasıl erişebileceğiniz, yönetebileceğiniz ve öğretme personeli hakkında bilgi sağlanır.

## <a name="key-concepts"></a>Önemli kavramlar

### <a name="quota-hours"></a>Kota saatleri

Öğrenciler, kota saatlerini etkilemeden, her zaman kendi VM 'lerine, zamanlanan sınıf saati sırasında erişebilir. Kota saatleri tüm Yarıyılı: için ayarlanır ve bir öğrencinin sanal makinesini düzenli olarak zamanlanan sınıf süresi dışında kullanabileceği saat sayısını tespit edebilir.

hafta başına 8 saat, Pazar günü sıfırlama-kümülatif değil.

Daha fazla bilgi için bkz. [kotayı ayarlama](how-to-configure-student-usage.md#set-quotas-for-users).

### <a name="automatic-shut-down"></a>Otomatik kapatma

Maliyetlerin korunmasını ve öğrencilerin kota saatlerini kaydetmenizi sağlamak için otomatik kapatmalar laboratuvarlar için etkinleştirilir. Otomatik kapatmalar, bir süre işlem yapılmadığında (fare veya klavye girişi olmadan) VM 'Leri kapatır. İki aşamada iş otomatik olarak kapandığında, bir süre işlem yapılmadan sonra bir öğrencinin VM bağlantısı kesilir. Bu noktada, VM **çalışmaya devam eder** ve öğrenciler bağlanabilir. Bağlantısı kesildikten sonra, başka bir süre etkinlik dışı kaldıktan sonra, VM kendisini kapatır.

Otomatik kapatmalar önemli bir maliyet tasarrufu aracıdır, ancak bunların işlerini kaydetme ve büyük proje dosyalarını işleme gibi öğrenciler için bir zorluk sunduklarında. Öğrencilerinizin bağlantısı kesilirse veya VM 'Ler çok hızlı bir şekilde kapatılırsa, CTE yöneticinize ulaşın. 

Daha fazla bilgi için bkz. [Laboratuvar hesabı Için sanal makinelerin otomatik olarak kapatılmasını yapılandırma](how-to-configure-lab-accounts.md).

### <a name="managing-virtual-machines"></a>Sanal Makineleri Yönetme

Laboratuvarın yönetilmesi, öğretmenlerin laboratuvar kapasitesi (öğrenciler için kullanılabilen VM sayısı) ve VM 'Leri el ile başlatma, durdurma veya sıfırlama gibi şeyleri denetlemesine olanak tanır. öğretmenler aynı zamanda öğrenciye bağlantı kurmak, dosyalara erişmek ve yazılımla ya da VM 'nin kendisiyle ilgili sorunları gidermek için VM 'lere bağlanabilir.

VM 'Leri yönetirken dikkat edilmesi gereken en önemli şey, makinenin her zaman **çalışıyor** olması durumunda, VM 'ye bağlı olmaması durumunda bile maliyetlerden biridir.

## <a name="lab-dashboards"></a>Laboratuvar panoları

### <a name="overview"></a>Genel Bakış

Azure Lab Services sınıf laboratuvarları için panolar, VM bilgileri, atanan ve atanmamış VM sayısı, kayıtlı ve kaydedilmemiş Kullanıcı sayısı ve laboratuvar zamanlamalarıyla ilgili bilgiler dahil olmak üzere belirli bir laboratuvarın farklı yönlerinin bir anlık görüntüsünü sağlar. 

> [!NOTE]
> Panonun ve [Azure Lab Services Web sitesinin](https://labs.azure.com/) yönetim yönlerinin çoğu öğretmenler tarafından görülüp, rolünüze özgü izinler panodaki belirli ölçütleri değiştirme yeteneğinizi etkileyebilir. Belirli bir laboratuvar kurulumu ile ilgili bir sorunla karşılaşırsanız CTE yöneticinize ulaşın.

:::image type="content" source="./media/use-dashboard/dashboard.png" alt-text="Azure Lab Services portalı":::

### <a name="examine-a-dashboard"></a>Bir panoyu İnceleme

1. [Azure Lab Services Web sitesinde](https://labs.azure.com/)gezin ve oturum açın.
1. Laboratuvarınızı seçin.
1. Pencerenin sol tarafında bir **Pano** görürsünüz. **Panoya** tıkladığınızda, panonuzda bir dizi kutucuk görürsünüz.
1. **Ücretler & faturalandırma** kutucuğunun altında Şablonlar, sanal makine havuzları, kullanıcılar ve zamanlamalar için de Kutucuklar vardır ve bu sayede, sınıf laboratuvarında daha fazla ayrıntı görüntüleyebilirsiniz.

    * Şablon-şablonun oluşturulduğu tarihi ve son yayımlanma tarihini açıklar. 
    * Sanal makine havuzu-atanan ve atanmamış VM sayısı.
    * Kullanıcılar-laboratuvara eklenmiş ancak kayıtlı olmayan, kayıtlı Kullanıcı ve Kullanıcı sayısı.
    * Zamanlamalar-laboratuvar için yaklaşan zamanlanmış olayları ve daha fazla olay görüntülemek için bir bağlantıyı görüntüler.

Daha fazla bilgi için bkz. [panoyu kullanma](use-dashboard.md).

### <a name="manually-starting-vms"></a>VM 'Leri el ile başlatma

1. **Sanal makine havuzu** sayfasında, bir laboratuvardaki tüm VM 'leri, sayfanın en üstündeki **Tümünü Başlat** düğmesine tıklayarak başlatabilirsiniz.

    :::image type="content" source="./media/how-to-set-virtual-machine-passwords/start-all-vms-button.png" alt-text="VM 'lerinizi başlatın":::
1. Tek tek VM 'Ler durum geçiş öğesine tıklanarak başlatılabilir. 

    Geçiş, VM başladığında **başlar ve** sonra VM başlatıldıktan sonra **çalışır** .
1. Ayrıca, **ad** sütununun solunda bulunan denetimleri kullanarak bir dizi VM seçebilirsiniz. 

    İstenen VM 'Leri seçtikten sonra, ekranın üst kısmındaki **Başlat** düğmesine tıklayın.
1. Başlatıldıktan sonra tüm VM 'Leri durdurmak için **Tümünü Durdur** düğmesine tıklayabilirsiniz.

    :::image type="content" source="./media/how-to-set-virtual-machine-passwords/stop-all-vms-button.png" alt-text="VM 'lerinizi durdurun":::

### <a name="stopping-and-resetting-vms"></a>VM 'Leri durdurma ve sıfırlama

* Durum geçişi ' ne tıklayarak tek tek VM 'Leri durdurabilirsiniz.
* Ayrıca, denetimleri kullanarak ve ekranın üst kısmındaki "Durdur" düğmesine tıklayarak birden fazla VM 'yi durdurabilirsiniz.

Bir öğrenciye sanal makineye bağlanma sorunları yaşanıyorsa veya başka bir nedenle VM 'nin sıfırlanması gerekiyorsa, Reset işlevini kullanabilirsiniz.
1. Bir veya daha fazla VM 'yi sıfırlamak için, denetimleri kullanarak bunları seçin ve ardından sayfanın en üstündeki **Sıfırla** düğmesine tıklayın.
1. Açılır pencerede **Sıfırla**' ya tıklayın.

    :::image type="content" source="./media/how-to-set-virtual-machine-passwords/reset-vms-dialog.png" alt-text="VM 'nizi sıfırlayın":::

    > [!NOTE]
    > Öğrenci sanal makinesini açmak öğrenci kotasını etkilemez. Kullanıcılar için kotalar, kullanıcının zamanlanan sınıf süresi dışında kullanabileceği laboratuvar saatleri sayısını belirtir.

### <a name="connect-to-vms"></a>VM 'lere bağlanma

Öğretmenler, açık olduğu sürece bir öğrenci VM 'sine bağlanabilir ve öğrenci sanal makineye bağlı DEĞILDIR. VM 'ye bağlanarak, sanal makinede yerel dosyalara erişebiliyor ve öğrencilerin sorunları gidermelerine yardımcı olacak.

1. Öğrenci VM 'sine bağlanmak için, listedeki sanal makineye fare üzerine gelin ve **Bağlan** düğmesine tıklayın. 
1. Ardından, Bermebook, Mac veya PC 'Ler için öğrenciler için Başlarken Kılavuzunu izleyin

:::image type="content" source="./media/how-to-set-virtual-machine-passwords/connect-student-vm.png" alt-text="Öğrenci VM 'sine bağlanma düğmesi":::

## <a name="manage-users-in-a-lab"></a>Laboratuvardaki kullanıcıları yönetme

Öğretmenler bir laboratuvara öğrenci kullanıcıları ekleyebilir ve saat kotalarını izleyebilir. 

### <a name="add-users-by-email-address"></a>E-posta adresine göre Kullanıcı ekleme

1. [Azure Laboratuvar Hizmetleri Web sitesinden](https://labs.azure.com/) pencerenin sol tarafındaki **Kullanıcılar** ' a tıklayın.
1. Pencerenin üst kısmında, **Kullanıcı Ekle** ' ye tıklayın ve **e-posta adresine göre Ekle**' yi seçin. 

    :::image type="content" source="./media/how-to-configure-student-usage/add-users-button.png" alt-text="' Kullanıcı Ekle ' düğmesi":::
1. Sağ tarafta görünen **kullanıcıları Ekle** bölmesinde, öğrenciler için e-posta adreslerini ayrı satırlara veya noktalı virgülle ayırarak tek bir satıra girin.
1. **Kaydet**’e tıklayın.

    :::image type="content" source="./media/get-started-manage-labs/add-students.png" alt-text="Laboratuvarınızın öğrencileri ekleyin":::
1. Kullanıcı listeniz artık e-postalar, durum, davet ve kota saatleriyle güncelleştirilecektir.

    Öğrenciler bir laboratuvar için kaydedildikten sonra, Azure Active Directory adları ilk ve son adlarla güncelleştirilir.

    > [!NOTE]
    > Erişimi kısıtla seçeneğinin etkin tutulması kullanıcılar için açıktır. Bu, yalnızca listenizdeki kullanıcıların, göndereceğiniz kayıt bağlantısını kullanarak laboratuvara kaydedebileceği anlamına gelir.

### <a name="add-users-using-a-spreadsheet"></a>Elektronik tablo kullanarak Kullanıcı ekleme 

Ayrıca, e-posta adreslerini içeren bir CSV dosyasını karşıya yükleyerek da kullanıcı ekleyebilirsiniz.

1. Microsoft Excel 'de öğrencilerin e-posta adreslerini tek bir sütunda listeleyen bir CSV dosyası oluşturun.
1. [Azure Lab Services Web sitesinden](https://labs.azure.com/), **Kullanıcılar** sayfasının en üstündeki **Kullanıcı Ekle** düğmesine tıklayın.
1. **CSV 'Yi karşıya yükle** seçeneğini belirleyin.
1. Öğrencilerin e-posta adreslerini içeren CSV dosyasını seçin ve **Aç**' a tıklayın.

    :::image type="content" source="./media/get-started-manage-labs/add-users-spreadsheet.png" alt-text="Elektronik tablo kullanarak Kullanıcı ekleme":::
1. E-postalar artık sağdaki pencerede görüntülenir. **Kaydet**’e tıklayın.

    :::image type="content" source="./media/get-started-manage-labs/register-users.png" alt-text="Kullanıcıları kaydet":::

### <a name="register-users"></a>Kullanıcıları kaydet

Kullanıcılar laboratuvara eklendikten sonra, sanal makinelere erişmek için kaydolmaları gerekir. Bu işlem, portaldan Kullanıcı davet eden bir e-posta gönderecek şekilde, portaldan Kullanıcı davet ederek yapılabilir. Ya da kayıt bağlantısını kopyalayıp bir e-postaya ya da öğrencilerle başka bir iletişim biçimine kopyalayarak veya yapıştırarak.

1. **Kullanıcılar** sayfasında, listeden bir öğrenci veya birden çok öğrenci seçin.

    Seçtiğiniz öğrencinin satırındaki listede bulunan zarf simgesini seçin veya ekranın üst kısmındaki **davet** ' i tıklatın.

    :::image type="content" source="./media/get-started-manage-labs/send-invitation.png" alt-text="Davet gönderme":::
    
    E-posta ile **davet gönder** penceresinde, öğrenciler için isteğe bağlı bir ileti (Kullanıcı adı ve parola gibi) girin ve ardından **Gönder**' e tıklayın. 
    
    :::image type="content" source="./media/get-started-manage-labs/send-invitation-mail.png" alt-text="E-posta ile davet gönder":::

    Alternatif olarak, aynı **Kullanıcılar** sayfasından ekranın üst kısmında bulunan **kayıt bağlantısı** düğmesine tıklayabilirsiniz. 

    :::image type="content" source="./media/get-started-manage-labs/registration-link.png" alt-text="Kullanıcı kaydı bağlantısı":::
    
    Metin alanından kayıt bağlantısını kopyalayın ve e-postaya veya tercih ettiğiniz güvenli mesajlaşma aracınızdan yapıştırın.  
    
    :::image type="content" source="./media/get-started-manage-labs/user-registration.png" alt-text="Kullanıcı kaydı gönder":::

Kullanıcıları davet ettikten veya bağlantıyı paylaştırdıktan sonra, **durum** sütunundaki **Kullanıcılar** sayfasında başarıyla kaydolan kullanıcıları izleyebileceksiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıçta oluşturduğunuz kaynakları kullanmaya devam edemeyeceksanız, kaynakları silin.

## <a name="next-steps"></a>Sonraki adımlar

[Bir laboratuvar hesabı ayarlama](tutorial-setup-lab-account.md)
