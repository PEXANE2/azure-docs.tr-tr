---
title: Azure Lab Services sınıf laboratuvarlarında kullanım ayarlarını yapılandırma
description: Bir laboratuvarda öğrenci sayısını yapılandırmayı, laboratuvara kaydolmasını ve VM 'nin kullanabileceği saat sayısını denetlemeyi ve daha fazlasını öğrenin.
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
ms.date: 02/20/2020
ms.author: spelluru
ms.openlocfilehash: 4dfaaf750c80198000ef08af4dff90c41a46f094
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84896028"
---
# <a name="add-and-manage-lab-users"></a>Laboratuvar kullanıcılarını ekleme ve yönetme

Bu makalede, bir laboratuvara Kullanıcı ekleme, laboratuvar ile kaydetme, sanal makineyi (VM) tarafından kullanılabilecek ek saatlerin sayısını denetleme ve daha fazla bilgi edinme açıklanır. 

## <a name="add-users-to-a-lab"></a>Laboratuvara Kullanıcı ekleme

Bu bölümde, öğrencileri el ile veya bir CSV dosyası karşıya yükleyerek bir laboratuvarya ekleyebilirsiniz. Şunları yapın:

1. Sol bölmede **Kullanıcılar**' ı seçin. 

    Varsayılan olarak, **erişimi kısıtla** seçeneği açıktır ve kullanıcılar listesinde olmadıkları takdirde, öğrenciler bir kayıt bağlantısına sahip olsalar bile laboratuvara kayıt yaptıramazlar. Yalnızca listelenen kullanıcılar, göndereceğiniz kayıt bağlantısını kullanarak laboratuvara kayıt yapabilir. Bu yordamda, kullanıcıları listeye eklersiniz. Alternatif olarak, kayıt bağlantısına sahip oldukları sürece öğrencilerin laboratuvara kaydedilmesine izin veren **erişimi kısıtla**seçeneğini devre dışı bırakabilirsiniz. 

1. **Kullanıcılar** bölmesinin en üstünde **Kullanıcı Ekle**' yi seçin ve ardından **e-posta adresi ekle**' yi seçin. 

    !["Kullanıcı Ekle" düğmesi](./media/how-to-configure-student-usage/add-users-button.png)

1. **Kullanıcıları Ekle** bölmesinde, öğrenciler için e-posta adreslerini ayrı satırlara veya noktalı virgülle ayrılmış tek bir satıra girin. 

    ![Kullanıcıların e-posta adreslerini ekleme](./media/how-to-configure-student-usage/add-users-email-addresses.png)

1. **Kaydet**’i seçin. 

    Bu liste, geçerli kullanıcıların e-posta adreslerini ve laboratuvarda kayıtlı olup olmadığını gösterir. 

    ![Kullanıcı listesi](./media/how-to-configure-student-usage/list-of-added-users.png)

    > [!NOTE]
    > Öğrenciler laboratuvara kaydedildikten sonra liste adlarını görüntüler. Listede gösterilen ad, Azure Active Directory öğrencilerinin ilk ve soyadlarını kullanarak oluşturulur. 

### <a name="add-users-by-uploading-a-csv-file"></a>CSV dosyası karşıya yükleyerek Kullanıcı ekleme

Ayrıca, e-posta adreslerini içeren bir CSV dosyasını karşıya yükleyerek da kullanıcı ekleyebilirsiniz.

1. Microsoft Excel 'de öğrencilerin e-posta adreslerini tek bir sütunda listeleyen bir CSV dosyası oluşturun.

    ![Bir CSV dosyasındaki kullanıcıların listesi](./media/how-to-configure-student-usage/csv-file-with-users.png)

1. **Kullanıcılar** bölmesinin en üstünde **Kullanıcı Ekle**' yı seçin ve ardından **CSV 'yi karşıya yükle**' yi seçin.

    !["CSV 'yi karşıya yükle" düğmesi](./media/how-to-configure-student-usage/upload-csv-button.png)

1. Öğrencilerin e-posta adreslerini içeren CSV dosyasını seçin ve sonra **Aç**' ı seçin.

    **Kullanıcı Ekle** PENCERESI, CSV dosyasındaki e-posta adresi listesini görüntüler. 

    ![CSV dosyasından e-posta adreslerine sahip "Kullanıcı Ekle" penceresi](./media/how-to-configure-student-usage/add-users-window.png)

1. **Kaydet**’i seçin. 

1. **Kullanıcılar** bölmesinde, eklenen öğrencilerin listesini görüntüleyin. 

    !["Kullanıcılar" bölmesindeki eklenen kullanıcıların listesi](./media/how-to-configure-student-usage/list-of-added-users.png)

## <a name="send-invitations-to-users"></a>Kullanıcılara davet gönder

Yeni kullanıcılara bir kayıt bağlantısı göndermek için aşağıdaki yöntemlerden birini kullanın. 

Laboratuvar için **erişimi kısıtla** seçeneği etkinleştirilirse, yalnızca listelenen kullanıcılar laboratuvara kaydolmak için kayıt bağlantısını kullanabilir. Bu seçenek varsayılan olarak etkindir. 

### <a name="invite-all-users"></a>Tüm kullanıcıları davet et

Bu yöntemde, listelenen tüm öğrencilere bir kayıt bağlantısı ve isteğe bağlı bir ileti ile e-posta gönderme işlemlerinin nasıl yapılacağı gösterilir.

1. **Kullanıcılar** bölmesinde **Tümünü davet et**' i seçin. 

    !["Tümünü davet et" düğmesi](./media/tutorial-setup-classroom-lab/invite-all-button.png)

1. **E-posta ile davet gönder** penceresinde, isteğe bağlı bir ileti girin ve ardından **Gönder**' i seçin. 

    E-posta otomatik olarak kayıt bağlantısını içerir. Kayıt bağlantısını ayrı olarak almak ve kaydetmek için, **Kullanıcılar** bölmesinin üst kısmındaki üç nokta (**...**) simgesini seçin ve ardından **kayıt bağlantısı**' nı seçin. 

    !["E-postaya göre kayıt bağlantısı gönder" penceresi](./media/tutorial-setup-classroom-lab/send-email.png)

    **Kullanıcılar** listesinin **davet** sütunu, eklenen her kullanıcı için davetiye durumunu görüntüler. Durum, **gönderilmesi** ve sonrasında ** \<date> gönderilmesi **için değişiklik yapılmalıdır. 

### <a name="invite-selected-users"></a>Seçili kullanıcıları davet et

Bu yöntem, yalnızca belirli öğrencileri davet etmeyi ve diğer kişilerle paylaşabileceğiniz bir kayıt bağlantısı almanızı gösterir.

1. **Kullanıcılar** bölmesinde, listeden bir öğrenci veya birden çok öğrenci seçin. 

1. Seçtiğiniz öğrencinin satırındaki **zarf** simgesini seçin ya da araç çubuğunda **davet et**' i seçin. 

    ![Seçili kullanıcıları davet et](./media/how-to-configure-student-usage/invite-selected-users.png)

1. **E-posta ile davet gönder** penceresinde, isteğe bağlı bir **ileti**girin ve ardından **Gönder**' i seçin. 

    ![Seçili kullanıcılara e-posta gönder](./media/how-to-configure-student-usage/send-invitation-to-selected-users.png)

    **Kullanıcılar** bölmesi, bu işlemin durumunu tablonun **davet** sütununda görüntüler. Davet e-postası, öğrencilerin laboratuvara kaydolmak için kullanabileceği kayıt bağlantısını içerir.

## <a name="get-the-registration-link"></a>Kayıt bağlantısını al

Bu bölümde, kayıt bağlantısını portaldan alabilir ve kendi e-posta uygulamanızı kullanarak gönderebilirsiniz. 

1. **Kullanıcılar** bölmesinde, **kayıt bağlantısı**' nı seçin.

    ![Öğrenci kaydı bağlantısı](./media/how-to-configure-student-usage/registration-link-button.png)

1. **Kullanıcı kaydı** penceresinde **Kopyala**' yı seçin ve **bitti**' yi seçin. 

    !["Kullanıcı kaydı" penceresi](./media/how-to-configure-student-usage/registration-link.png)

    Bağlantı, panoya kopyalanır. 
    
1. E-posta uygulamanızda, kayıt bağlantısını yapıştırın ve sonra öğrencinin sınıfa kaydolabilmesi için e-postayı bir öğrenciye gönderin. 

## <a name="view-registered-users"></a>Kayıtlı kullanıcıları görüntüle

1. [Azure Lab Services](https://labs.azure.com) Web sitesine gidin. 
1. **Oturum aç**' ı seçin ve ardından kimlik bilgilerinizi girin. Azure Lab Services, kuruluş hesaplarını ve Microsoft hesaplarını destekler.
1. **Laboratuvarlarım** sayfasında, kullanımını izlemek istediğiniz Laboratuvarı seçin. 
1. Sol bölmede **Kullanıcılar**' ı seçin veya **Kullanıcılar** kutucuğunu seçin. 

    **Kullanıcılar** bölmesi, laboratuvarınızda kayıtlı olan öğrencilerin bir listesini görüntüler.  

    ![Kayıtlı kullanıcıların listesi](./media/tutorial-track-usage/registered-users.png)

## <a name="set-quotas-for-users"></a>Kullanıcılar için kota ayarlama

Aşağıdakileri yaparak her öğrenci için bir saat kotası ayarlayabilirsiniz: 

1. **Kullanıcılar** bölmesinde Kullanıcı başına kota ' i seçin: araç çubuğunda ** \<number> saat (ler)** . 
1. **Kullanıcı başına kota** penceresinde, her öğrenciye, zamanlanan sınıf süresi dışında vermek istediğiniz saat sayısını belirtin ve ardından **Kaydet**' i seçin.

    !["Kullanıcı başına kota" penceresi](./media/how-to-configure-student-usage/quota-per-user.png)    

    Değiştirilen değerler artık **Kullanıcı başına kota: \<number of hours> ** araç çubuğunda ve kullanıcılar listesinde, burada gösterildiği gibi görüntülenir:

    ![Kullanıcı başına kota saati](./media/how-to-configure-student-usage/quot-per-user-after.png)

    > [!IMPORTANT]
    > [VM 'lerin zamanlanan çalışma süresi](how-to-create-schedules.md) , bir öğrenciye ayrılan kotaya göre sayılmaz. Kota, bir öğrencinin VM 'lerde harcadığı zamanlanan saatlerin dışında kalan süredir. 

## <a name="set-additional-quotas-for-specific-users"></a>Belirli kullanıcılar için ek kotalar ayarlama

Önceki bölümde yer alan tüm kullanıcılar için ayarlanan ortak kotaların ötesinde belirli öğrenciler için kotalar belirtebilirsiniz. Örneğin, bir eğitmen olarak tüm öğrenciler için kotayı 10 saat olarak ayarlayıp belirli bir öğrenci için 5 saatlik ek bir kota ayarlarsanız, bu öğrenci 15 (10 + 5) saatlik kotayı alır. Ortak kotayı daha sonra olarak değiştirirseniz, öğrencinin 20 (15 + 5) saat kotası alır. Bu genel kotanın zamanlanan sürenin dışında olduğunu unutmayın. Bir öğrencinin zamanlanan süre boyunca bir laboratuvar VM 'sinde harcadığı süre bu kotaya göre sayılmaz. 

Ek kotalar ayarlamak için şunları yapın:

1. **Kullanıcılar** bölmesinde, listeden bir öğrenci seçin ve ardından araç çubuğunda **kotayı ayarla** ' yı seçin. 

    !["Kotayı ayarla" düğmesi](./media/how-to-configure-student-usage/adjust-quota-button.png)

1. ** \<selected user or users email address> Kotayı ayarla **' da, seçilen öğrenciye veya öğrencilere vermek istediğiniz ek laboratuvar saatleri sayısını girin ve ardından **Uygula**' yı seçin. 

    !["Kotayı ayarla..." penceresine](./media/how-to-configure-student-usage/additional-quota.png)

    **Kullanım** sütunu seçili öğrenciler için güncelleştirilmiş kotayı görüntüler. 

    ![Kullanıcı için yeni kullanım](./media/how-to-configure-student-usage/new-usage-hours.png)

## <a name="student-accounts"></a>Öğrenci hesapları

Bir sınıf laboratuvarına öğrenci eklemek için e-posta hesaplarını kullanın. Öğrenciler aşağıdaki e-posta hesabı türlerine sahip olabilir:

- Office 365 için University 'in Azure Active Directory örneği tarafından sunulan bir öğrenci e-posta hesabı. 
- *Outlook.com*, *hotmail.com*, *msn.com*veya *Live.com*gibi bir Microsoft-Domain e-posta hesabı.
- Yahoo! tarafından sağlana gibi Microsoft olmayan bir e-posta hesabı veya Google. Ancak, bu tür hesapların bir Microsoft hesabı bağlantılı olması gerekir.
- GitHub hesabı. Bu hesabın bir Microsoft hesabı ile bağlantılı olması gerekir.

### <a name="use-a-non-microsoft-email-account"></a>Microsoft olmayan bir e-posta hesabı kullan
Öğrenciler, bir sınıf laboratuvarına kaydolmak ve oturum açmak için Microsoft olmayan e-posta hesaplarını kullanabilir.  Ancak kayıt, ilk olarak Microsoft olmayan e-posta adreslerine bağlı bir Microsoft hesabı oluşturmasını gerektirir.

Birçok öğrenciye, Microsoft olmayan e-posta adreslerine bağlı bir Microsoft hesabı zaten sahip olabilir. Örneğin, öğrenciler, e-posta adreslerini Office, Skype, OneDrive veya Windows gibi diğer Microsoft ürünleri veya hizmetleriyle kullandıklarında zaten bir Microsoft hesabı sahiptir.  

Öğrenciler bir sınıfta oturum açmak için kayıt bağlantısını kullandıklarında, onlara e-posta adresi ve parolası sorulur. Bir Microsoft hesabı bağlantılı olmayan bir Microsoft hesabı oturum açmayı deneyen öğrenciler aşağıdaki hata iletisini alır: 

![Oturum açma sırasında hata iletisi](./media/how-to-configure-student-usage/cant-find-account.png)

Öğrenciler için [Microsoft hesabı kaydolmak](http://signup.live.com)üzere bir bağlantı aşağıda verilmiştir.  

> [!IMPORTANT]
> Öğrenciler bir derslik laboratuvarında oturum açtıklarında, Microsoft hesabı oluşturma seçeneği verilmemiş demektir. Bu nedenle, bu kayıt bağlantısını, http://signup.live.com Microsoft dışı hesapları kullanan öğrencilerle göndereceğiniz sınıf Laboratuvarı kayıt e-postasına dahil etmenizi öneririz.

### <a name="use-a-github-account"></a>GitHub hesabı kullan
Öğrenciler, bir sınıf laboratuvarına kaydolmak ve oturum açmak için mevcut bir GitHub hesabı da kullanabilir. Zaten GitHub hesaplarına bağlanmış bir Microsoft hesabı varsa, öğrenciler oturum açabilir ve önceki bölümde gösterildiği gibi parolasını sağlayabilir. 

Henüz GitHub hesabını bir Microsoft hesabı bağlamadıysa, şunları yapabilir:

1. Burada gösterildiği gibi, **oturum açma seçenekleri** bağlantısını seçin:

    !["Oturum açma seçenekleri" bağlantısı](./media/how-to-configure-student-usage/signin-options.png)

1. **Oturum açma seçenekleri** penceresinde **GitHub ile oturum aç**' ı seçin.

    !["GitHub ile oturum aç" bağlantısı](./media/how-to-configure-student-usage/signin-github.png)

    İstemde, öğrenciler daha sonra GitHub hesaplarına bağlı bir Microsoft hesabı oluşturur. Bağlama, **İleri ' yi**seçtiklerinde otomatik olarak gerçekleşir. Bunlar daha sonra hemen oturum açırlar ve derslik laboratuvarına bağlanır.

## <a name="export-a-list-of-users-to-a-csv-file"></a>Bir CSV dosyasına kullanıcı listesini dışarı aktarma

1. **Kullanıcılar** bölmesine gidin.
1. Araç çubuğunda üç nokta (**...**) simgesini seçin ve ardından **CSV 'yi dışarı aktar**' ı seçin. 

    !["CSV 'yi dışarı aktar" düğmesi](./media/how-to-export-users-virtual-machines-csv/users-export-csv.png)


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- Yöneticiler için: [Laboratuvar hesapları oluşturma ve yönetme](how-to-manage-lab-accounts.md)
- Laboratuvar sahipleri için: [Labs oluşturma ve yönetme](how-to-manage-classroom-labs.md) ve [şablonları ayarlama ve yayımlama](how-to-create-manage-template.md)
- Laboratuvar kullanıcıları için: [sınıf laboratuvarlarına erişin](how-to-use-classroom-lab.md)
