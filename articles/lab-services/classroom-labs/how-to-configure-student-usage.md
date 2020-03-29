---
title: Azure Lab Hizmetleri'nin sınıf laboratuarlarında kullanım ayarlarını yapılandırın
description: Bir laboratuvar için öğrenci sayısını nasıl yapılandırabileceğinizi, laboratuvara kaydedin, VM'yi kullanabilecekleri saat sayısını ve daha fazlasını kontrol edin.
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
ms.openlocfilehash: 2697bed77263bb5b8349898765851a9b87992279
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80159464"
---
# <a name="add-and-manage-lab-users"></a>Laboratuvar kullanıcılarını ekleme ve yönetme

Bu makalede, öğrenci kullanıcılarıbir laboratuvara nasıl ekleyeceğiniz, bunları laboratuvara kaydetme, sanal makineyi (VM) kullanabilecekleri ek saat sayısını ve daha fazlasını nasıl denetleyecekleri açıklanmaktadır. 

## <a name="add-users-to-a-lab"></a>Kullanıcıları laboratuvara ekleme

Bu bölümde, öğrencileri bir laboratuvara el ile veya bir CSV dosyası yükleyerek eklersiniz. Şunları yapın:

1. Sol **bölmede, Kullanıcılar'ı**seçin. 

    Varsayılan olarak, **Erişimi Kısıtla** seçeneği açık ve kullanıcı listesinde olmadıkları sürece, öğrenciler bir kayıt bağlantıları olsa bile laboratuvara kaydolamazlar. Yalnızca listelenen kullanıcılar gönderdiğiniz kayıt bağlantısını kullanarak laboratuvara kaydolabilir. Bu yordamda, kullanıcıları listeye eklersiniz. Alternatif olarak, öğrencilerin kayıt bağlantısı na sahip oldukları sürece laboratuvara kaydolmalarını sağlayan **Erişimi Kısıtla'yı**kapatabilirsiniz. 

1. **Kullanıcılar** bölmesinin üst **kısmında, kullanıcı ekle'yi**seçin ve ardından **e-posta adresine göre ekle'yi**seçin. 

    !["Kullanıcı ekle" düğmesi](../media/how-to-configure-student-usage/add-users-button.png)

1. Kullanıcı **Ekle** bölmesine, öğrencilerin e-posta adreslerini ayrı satırlara veya yarı iki nokta ile ayrılmış tek bir satıra girin. 

    ![Kullanıcıların e-posta adreslerini ekleme](../media/how-to-configure-student-usage/add-users-email-addresses.png)

1. **Kaydet'i**seçin. 

    Liste, laboratuvara kayıtlı olsun veya olmasın, geçerli kullanıcıların e-posta adreslerini ve durumlarını görüntüler. 

    ![Kullanıcı listesi](../media/how-to-configure-student-usage/list-of-added-users.png)

    > [!NOTE]
    > Öğrenciler laboratuvara kaydolduktan sonra, liste adlarını gösterir. Listede gösterilen ad, Azure Etkin Dizini'ndeki öğrencilerin ad ve soyadları kullanılarak oluşturulur. 

### <a name="add-users-by-uploading-a-csv-file"></a>CSV dosyası yükleyerek kullanıcı ekleme

Ayrıca, e-posta adreslerini içeren bir CSV dosyası yükleyerek kullanıcıları ekleyebilirsiniz.

1. Microsoft Excel'de, öğrencilerin e-posta adreslerini tek bir sütunda listeleyen bir CSV dosyası oluşturun.

    ![CSV dosyasındaki kullanıcıların listesi](../media/how-to-configure-student-usage/csv-file-with-users.png)

1. **Kullanıcılar** bölmesinin üst **kısmında, kullanıcı ekle'yi**seçin ve ardından **Upload CSV'yi**seçin.

    !["Upload CSV" düğmesi](../media/how-to-configure-student-usage/upload-csv-button.png)

1. Öğrencilerin e-posta adreslerini içeren CSV dosyasını seçin ve sonra **Aç'ı**seçin.

    **Kullanıcı Ekle** penceresi CSV dosyasından e-posta adres listesini görüntüler. 

    ![CSV dosyasından e-posta adresleri içeren "Kullanıcı ekle" penceresi](../media/how-to-configure-student-usage/add-users-window.png)

1. **Kaydet'i**seçin. 

1. **Kullanıcılar** bölmesinde, eklenen öğrencilerin listesini görüntüleyin. 

    !["Kullanıcılar" bölmesine eklenen kullanıcılar listesi](../media/how-to-configure-student-usage/list-of-added-users.png)

## <a name="send-invitations-to-users"></a>Kullanıcılara davetiye gönderme

Yeni kullanıcılara kayıt bağlantısı göndermek için aşağıdaki yöntemlerden birini kullanın. 

Erişimi **Kısıtla** seçeneği laboratuvar için etkinleştirilirse, yalnızca listelenen kullanıcılar laboratuvara kaydolmak için kayıt bağlantısını kullanabilir. Bu seçenek varsayılan olarak etkindir. 

### <a name="invite-all-users"></a>Tüm kullanıcıları davet et

Bu yöntem, listelenen tüm öğrencilere bir kayıt bağlantısı ve isteğe bağlı bir ileti içeren e-posta göndermenin nasıl yapılacağını gösterir.

1. **Kullanıcılar** bölmesinde **Tümünü Davet**et'i seçin. 

    !["Tümlerini davet et" düğmesi](../media/tutorial-setup-classroom-lab/invite-all-button.png)

1. **E-posta yla Davetiye Gönder** penceresinde isteğe bağlı bir ileti girin ve sonra **Gönder'i**seçin. 

    E-posta otomatik olarak kayıt bağlantısını içerir. Kayıt bağlantısını ayrı ayrı almak ve kaydetmek **için, Kullanıcılar** bölmesinin üst kısmındaki elipsleri (**...**) seçin ve ardından **Kayıt bağlantısını**seçin. 

    !["Kayıt bağlantısını e-posta ile gönder" penceresi](../media/tutorial-setup-classroom-lab/send-email.png)

    **Kullanıcılar** listesinin **Davet** sütunu, eklenen her kullanıcı için davet durumunu görüntüler. Durum **Gönderme** ve daha sonra ** \<>tarihinde Gönderildi **olarak değiştirilmelidir. 

### <a name="invite-selected-users"></a>Seçili kullanıcıları davet et

Bu yöntem, yalnızca belirli öğrencileri nasıl davet edeceğinizi ve diğer kişilerle paylaşabileceğiniz bir kayıt bağlantısını nasıl alacağınızı gösterir.

1. **Kullanıcılar** bölmesinde, listedeki bir öğrenci yi veya birden çok öğrenciyi seçin. 

1. Seçtiğiniz öğrenci için satırda **zarf** simgesini seçin veya araç çubuğunda **Davet et'i**seçin. 

    ![Seçili kullanıcıları davet et](../media/how-to-configure-student-usage/invite-selected-users.png)

1. **E-posta yla Davetiye Gönder** penceresine isteğe bağlı bir **ileti**girin ve sonra **Gönder'i**seçin. 

    ![Seçili kullanıcılara e-posta gönderme](../media/how-to-configure-student-usage/send-invitation-to-selected-users.png)

    **Kullanıcılar** bölmesi tablonun **Davet** sütununda bu işlemin durumunu görüntüler. Davet e-postası, öğrencilerin laboratuvara kayıt yaptırmak için kullanabilecekleri kayıt bağlantısını içerir.

## <a name="get-the-registration-link"></a>Kayıt bağlantısını alın

Bu bölümde, portaldan kayıt linkini alabilir ve kendi e-posta uygulamanızı kullanarak gönderebilirsiniz. 

1. **Kullanıcılar** bölmesinde Kayıt **bağlantısını**seçin.

    ![Öğrenci kaydı bağlantısı](../media/how-to-configure-student-usage/registration-link-button.png)

1. Kullanıcı **kayıt** penceresinde **Kopyala'yı**seçin ve **ardından Bitti'yi**seçin. 

    !["Kullanıcı kaydı" penceresi](../media/how-to-configure-student-usage/registration-link.png)

    Bağlantı, panoya kopyalanır. 
    
1. E-posta başvurunuzda, kayıt bağlantısını yapıştırın ve öğrencinin sınıfa kaydolması için e-postayı bir öğrenciye gönderin. 

## <a name="view-registered-users"></a>Kayıtlı kullanıcıları görüntüleme

1. [Azure Laboratuvar Hizmetleri](https://labs.azure.com) web sitesine gidin. 
1. **Oturum Aç'ı**seçin ve ardından kimlik bilgilerinizi girin. Azure Lab Services, kuruluş hesaplarını ve Microsoft hesaplarını destekler.
1. **Laboratuvarlarım** sayfasında, kullanımını izlemek istediğiniz laboratuarı seçin. 
1. Sol **bölmede, Kullanıcılar'ı**seçin veya **Kullanıcılar** döşemesini seçin. 

    **Kullanıcılar** bölmesi, laboratuvarınıza kaydolan öğrencilerin listesini görüntüler.  

    ![Kayıtlı kullanıcılar listesi](../media/tutorial-track-usage/registered-users.png)

## <a name="set-quotas-for-users"></a>Kullanıcılar için kota ayarlama

Aşağıdakileri yaparak her öğrenci için bir saat kontenjanı ayarlayabilirsiniz: 

1. **Kullanıcılar** bölmesinde, kullanıcı başına Kota'yı seçin: ** \<** araç çubuğundaki> saat(ler) sayısı. 
1. Kullanıcı **başına Kota** penceresinde, zamanlanan ders saati dışında her öğrenciye vermek istediğiniz saat sayısını belirtin ve ardından **Kaydet'i**seçin.

    !["Kullanıcı başına kota" penceresi](../media/how-to-configure-student-usage/quota-per-user.png)    

    Değiştirilen değerler artık kullanıcı **başına Kota'da görüntülenir: \<** araç çubuğundaki ve kullanıcı listesindeki>saat sayısı: Burada gösterildiği gibi:

    ![Kullanıcı başına kota saatleri](../media/how-to-configure-student-usage/quot-per-user-after.png)

    > [!IMPORTANT]
    > [VM'lerin planlanan çalışma süresi,](how-to-create-schedules.md) öğrenciye ayrılan kontenjana dahil edilmez. Kontenjan, öğrencinin VM'lerde geçirdiği zamanlanan saatler in dışındaki süreler içindir. 

## <a name="set-additional-quotas-for-specific-users"></a>Belirli kullanıcılar için ek kotalar ayarlama

Önceki bölümdeki tüm kullanıcılar için ayarlanan ortak kotaların ötesinde belirli öğrenciler için kotalar belirtebilirsiniz. Örneğin, bir eğitmen olarak, tüm öğrenciler için kontenjanı 10 saatolarak ayarlar sanız ve belirli bir öğrenci için 5 saatlik ek bir kontenjan belirlerseniz, o öğrenci 15 (10 + 5) saatlik kontenjan alır. Ortak kontenjanı daha sonra 15'e değiştirirseniz, öğrenci 20 (15 + 5) saat kontenjan alır. Bu genel kotanın zamanlanan saatin dışında olduğunu unutmayın. Bir öğrencinin planlanan süre içinde laboratuvar VM'de geçirdiği süre bu kotaya dahil değildir. 

Ek kotalar ayarlamak için aşağıdakileri yapın:

1. **Kullanıcılar** bölmesinde, listeden bir öğrenci seçin ve ardından araç çubuğunda **kotayı** ayarla'yı seçin. 

    !["Kotayı Ayarla" düğmesi](../media/how-to-configure-student-usage/adjust-quota-button.png)

1. Seçilen **kullanıcı veya \<kullanıcıların e-posta adresi>için kotayı **ayarla'da, seçilen öğrenci veya öğrencilere vermek istediğiniz ek laboratuvar saatlerinin sayısını girin ve ardından **Uygula'yı**seçin. 

    !["Kotayı ayarla ..." Pencere](../media/how-to-configure-student-usage/additional-quota.png)

    **Kullanım** sütunu, seçilen öğrenciler için güncelleştirilmiş kotayı görüntüler. 

    ![Kullanıcı için yeni kullanım](../media/how-to-configure-student-usage/new-usage-hours.png)

## <a name="student-accounts"></a>Öğrenci hesapları

Öğrencileri sınıf laboratuvarına eklemek için e-posta hesaplarını kullanırsınız. Öğrenciler aşağıdaki e-posta hesapları türlerine sahip olabilir:

- Üniversitenizin Office 365 için Azure Etkin Dizin örneği tarafından sağlanan bir öğrenci e-posta hesabı. 
- *outlook.com,* *hotmail.com,* *msn.com*veya *live.com*gibi bir Microsoft etki alanı e-posta hesabı.
- Yahoo tarafından sağlanan gibi Microsoft dışı bir e-posta hesabı veya Google. Ancak, bu tür hesapların bir Microsoft hesabına bağlanması gerekir.
- GitHub hesabı. Bu hesabın bir Microsoft hesabına bağlanması gerekir.

### <a name="use-a-non-microsoft-email-account"></a>Microsoft'a ait olmayan bir e-posta hesabı kullanma
Öğrenciler, bir sınıf laboratuvarına kaydolmak ve oturum açabilmek için Microsoft'a ait olmayan e-posta hesaplarını kullanabilir.  Ancak, kayıt, önce Microsoft olmayan e-posta adreslerine bağlı bir Microsoft hesabı oluşturmalarını gerektirir.

Birçok öğrencinin Microsoft olmayan e-posta adreslerine bağlı bir Microsoft hesabı olabilir. Örneğin, e-posta adreslerini Office, Skype, OneDrive veya Windows gibi diğer Microsoft ürünleri veya hizmetleriyle birlikte kullanan öğrenciler zaten bir Microsoft hesabı vardır.  

Öğrenciler bir sınıfta oturum açtıkları kayıt bağlantısını kullandıklarında, e-posta adresleri ve şifreleri istenir. Microsoft hesabıyla bağlantılı olmayan bir Microsoft hesabıyla oturum açmagirişiminde bulunan öğrenciler aşağıdaki hata iletisini alır: 

![Oturum açmada hata iletisi](../media/how-to-configure-student-usage/cant-find-account.png)

Burada öğrencilerin bir Microsoft [hesabına kaydolması için bir](http://signup.live.com)bağlantı bulunmaktadır.  

> [!IMPORTANT]
> Öğrenciler bir sınıf laboratuvarında oturum ettiklerinde, bir Microsoft hesabı oluşturma seçeneği verilmez. Bu nedenle, Microsoft hesabı olmayan öğrencilere gönderdiğiniz http://signup.live.comsınıf laboratuvar kayıt e-postasına bu kayıt bağlantısını eklemenizi öneririz.

### <a name="use-a-github-account"></a>GitHub hesabı kullanma
Öğrenciler ayrıca bir sınıf laboratuvarına kaydolmak ve oturum açabilmek için mevcut bir GitHub hesabını da kullanabilirler. Zaten GitHub hesabına bağlı bir Microsoft hesapları varsa, öğrenciler oturum açabilir ve önceki bölümde gösterildiği gibi parolalarını sağlayabilir. 

Henüz GitHub hesaplarını bir Microsoft hesabına bağlayamamışlarsa, aşağıdakileri yapabilir:

1. Burada gösterildiği gibi **Oturum Açma seçenekleri** bağlantısını seçin:

    !["Oturum açma seçenekleri" bağlantısı](../media/how-to-configure-student-usage/signin-options.png)

1. Oturum **Açma seçenekleri** **penceresinde, GitHub ile Oturum Aç'ı**seçin.

    !["GitHub ile oturum aç" bağlantısı](../media/how-to-configure-student-usage/signin-github.png)

    İsteme sırasında, öğrenciler daha sonra GitHub hesaplarıyla bağlantılı bir Microsoft hesabı oluştururlar. Bağlantı, **İleri'yi**seçtiklerinde otomatik olarak gerçekleşir. Daha sonra hemen oturum açarlar ve sınıf laboratuarına bağlanır.

## <a name="export-a-list-of-users-to-a-csv-file"></a>CSV dosyasına kullanıcı listesini dışa aktarma

1. **Kullanıcılar** bölmesine gidin.
1. Araç çubuğunda elipsis **(...**) seçeneğini belirleyin ve ardından **CSV'yi dışa**aktar'ı seçin. 

    !["CSV'yi dışa aktarma" düğmesi](../media/how-to-export-users-virtual-machines-csv/users-export-csv.png)


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- Yöneticiler için: [Laboratuvar hesapları oluşturma ve yönetme](how-to-manage-lab-accounts.md)
- Laboratuvar sahipleri için: [Laboratuvarlar oluşturun ve yönetin](how-to-manage-classroom-labs.md) ve [şablonları ayarlayın ve yayımlayın](how-to-create-manage-template.md)
- Laboratuvar kullanıcıları için: [Sınıf laboratuvarlarına erişin](how-to-use-classroom-lab.md)
