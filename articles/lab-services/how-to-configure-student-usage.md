---
title: Azure Lab Services laboratuvarlarında kullanım ayarlarını yapılandırma
description: Bir laboratuvarda öğrenci sayısını yapılandırmayı, laboratuvara kaydolmasını ve VM 'nin kullanabileceği saat sayısını denetlemeyi ve daha fazlasını öğrenin.
ms.topic: article
ms.date: 12/01/2020
ms.openlocfilehash: 380a587eecb276c457b93ca3c3f3ac08b2239275
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98791972"
---
# <a name="add-and-manage-lab-users"></a>Laboratuvar kullanıcılarını ekleme ve yönetme

Bu makalede, bir laboratuvara Kullanıcı ekleme, laboratuvar ile kaydetme, sanal makineyi (VM) tarafından kullanılabilecek ek saatlerin sayısını denetleme ve daha fazla bilgi edinme açıklanır. 

Kullanıcıları eklediğinizde, varsayılan olarak **erişimi kısıtla** seçeneği açıktır ve kullanıcılar listesinde olmadıkları takdirde, öğrenciler bir kayıt bağlantısı olsa bile laboratuvara kayıt yaptıramazlar. Yalnızca listelenen kullanıcılar, göndereceğiniz kayıt bağlantısını kullanarak laboratuvara kayıt yapabilir. Kayıt bağlantısına sahip oldukları sürece öğrencilerin laboratuvara kaydedilmesine izin veren **erişimi kısıtla** seçeneğini devre dışı bırakabilirsiniz. 

Bu makalede, bir laboratuvara Kullanıcı ekleme gösterilmektedir.

## <a name="add-users-from-an-azure-ad-group"></a>Azure AD grubundan Kullanıcı ekleme

### <a name="overview"></a>Genel Bakış

Artık kullanıcıları el ile eklemek veya silmek zorunda kalmaması için bir laboratuar Kullanıcı listesini mevcut bir Azure Active Directory (Azure AD) grubuyla eşitleyebilirsiniz. 

Kurumsal kaynaklara ve bulut tabanlı uygulamalara erişimi yönetmek için kuruluşunuzun Azure Active Directory içinde bir Azure AD grubu oluşturulabilir. Daha fazla bilgi için bkz. [Azure AD grupları](../active-directory/fundamentals/active-directory-manage-groups.md). Kuruluşunuz Microsoft Office 365 veya Azure hizmetlerini kullanıyorsa, kuruluşunuzda Azure Active Directory yönettiğiniz Yöneticiler zaten olur. 

### <a name="sync-users-with-azure-ad-group"></a>Kullanıcıları Azure AD grubuyla eşitleme

> [!IMPORTANT]
> Kullanıcı listesinin boş olduğundan emin olun. Laboratuvar içinde el ile eklediğiniz veya bir CSV dosyası içeri aktardığınız mevcut kullanıcılar varsa, Laboratuvarı mevcut bir gruba eşitleme seçeneği görünmez. 

1. [Azure Lab Services Web sitesinde](https://labs.azure.com/)oturum açın.
1. Birlikte çalışmak istediğiniz Laboratuvarı seçin.
1. Sol bölmede **Kullanıcılar**' ı seçin. 
1. **Gruptan Eşitle**' ye tıklayın. 

    :::image type="content" source="./media/how-to-configure-student-usage/add-users-sync-group.png" alt-text="Bir Azure AD grubundan eşitleme yaparak Kullanıcı ekleme":::
    
1. Laboratuvarınızı ile eşitlemek için mevcut bir Azure AD grubu seçmeniz istenir. 
    
    Listede bir Azure AD grubu görmüyorsanız, aşağıdakilerden biri olabilir:

    -   Bir Azure Active Directory Konuk Kullanıcı (genellikle Azure AD 'nin sahibi olan kuruluşun dışındaysanız) varsa ve Azure AD içinde gruplar arayabileceksiniz. Bu durumda, bu durumda laboratuvara bir Azure AD grubu ekleyemeyeceksiniz. 
    -   Takımlar aracılığıyla oluşturulan Azure AD grupları bu listede gösterilmez. Laboratuvarların içinden doğrudan içinden laboratuvar oluşturmak ve yönetmek için Azure Lab Services uygulama ekleyebilirsiniz. [Bir laboratuvarın Kullanıcı listesini takımlar içinden yönetme](how-to-manage-user-lists-within-teams.md)hakkında daha fazla bilgi için bkz.. 
1. Laboratuvarınızı eşitlemek için Azure AD grubunu girdikten sonra **Ekle**' ye tıklayın.
1. Laboratuvar eşitlendikten sonra, Azure AD grubunun içindeki herkesi Kullanıcı olarak laboratuvara çeker ve kullanıcı listesinin güncelleştirildiğini görürsünüz. Yalnızca bu Azure AD grubundaki kişilerin laboratuvarınızda erişimi olur. Kullanıcı listesi, Azure AD grubunun en son üyeliğiyle eşleşecek şekilde her 24 saatte bir yenilenir. Azure AD grubundaki en son değişikliklerle el ile eşitleme yapmak için kullanıcılar sekmesindeki Eşitle düğmesine de tıklayabilirsiniz.
1. Tüm kullanıcılara, laboratuvara kayıt bağlantısı olan tüm kullanıcılara bir e-posta gönderecek olan **Tümünü davet et** düğmesine tıklayarak kullanıcıları laboratuvara davet edin. 

### <a name="automatic-management-of-virtual-machines-based-on-changes-to-the-azure-ad-group"></a>Azure AD grubundaki değişikliklere göre sanal makinelerin otomatik yönetimi 

Laboratuvar bir Azure AD grubuyla eşitlendiğinde, laboratuvardaki sanal makinelerin sayısı gruptaki kullanıcı sayısıyla otomatik olarak eşleşir. Artık laboratuvar kapasitesini el ile güncelleştiremezsiniz. Bir Kullanıcı Azure AD grubuna eklendiğinde, bir laboratuvar otomatik olarak bu kullanıcı için bir sanal makine ekler. Bir Kullanıcı Azure AD grubundan silindiğinde, bir laboratuvar kullanıcının sanal makinesini laboratuvara otomatik olarak siler. 

## <a name="add-users-manually-from-emails-or-csv-file"></a>Kullanıcıları e-postalardan veya CSV dosyasından el ile Ekle

Bu bölümde öğrencileri el ile (e-posta adresine göre veya bir CSV dosyası karşıya yükleyerek) eklersiniz. 

### <a name="add-users-by-email-address"></a>E-posta adresine göre Kullanıcı ekleme

1. Sol bölmede **Kullanıcılar**' ı seçin. 
1. **Kullanıcıları el Ile Ekle**' ye tıklayın. 

    :::image type="content" source="./media/how-to-configure-student-usage/add-users-manually.png" alt-text="Kullanıcıları el ile ekleme":::
1. **E-posta adresi ekle** ' yi seçin (varsayılan), öğrenciler için e-posta adreslerini ayrı satırlara veya noktalı virgülle ayrılmış tek bir satıra girin. 

    :::image type="content" source="./media/how-to-configure-student-usage/add-users-email-addresses.png" alt-text="Kullanıcıların e-posta adreslerini ekleme":::
1. **Kaydet**’i seçin. 

    Bu liste, geçerli kullanıcıların e-posta adreslerini ve laboratuvarda kayıtlı olup olmadığını gösterir. 

    :::image type="content" source="./media/how-to-configure-student-usage/list-of-added-users.png" alt-text="Kullanıcı listesi":::

    > [!NOTE]
    > Öğrenciler laboratuvara kaydedildikten sonra liste adlarını görüntüler. Listede gösterilen ad, Azure Active Directory öğrencilerinin ilk ve soyadlarını kullanarak oluşturulur. 

### <a name="add-users-by-uploading-a-csv-file"></a>CSV dosyası karşıya yükleyerek Kullanıcı ekleme

Ayrıca, e-posta adreslerini içeren bir CSV dosyasını karşıya yükleyerek da kullanıcı ekleyebilirsiniz. 

Bir CSV metin dosyası, virgülle ayrılmış (CSV) tablolu verileri (sayılar ve metin) depolamak için kullanılır. Sütunları sütunlar alanlarında (örneğin, elektronik tablolarda) depolamak yerine, bir CSV dosyası virgüllerle ayrılmış bilgileri depolar. Bir CSV dosyasındaki her satır aynı sayıda virgülle ayrılmış "alan" alanına sahip olur. CSV dosyalarını kolayca oluşturmak ve düzenlemek için Excel 'i kullanabilirsiniz.

1. Microsoft Excel 'de öğrencilerin e-posta adreslerini tek bir sütunda listeleyen bir CSV dosyası oluşturun.

    :::image type="content" source="./media/how-to-configure-student-usage/csv-file-with-users.png" alt-text="Bir CSV dosyasındaki kullanıcıların listesi":::
1. **Kullanıcılar** bölmesinin en üstünde **Kullanıcı Ekle**' yı seçin ve ardından **CSV 'yi karşıya yükle**' yi seçin.
1. Öğrencilerin e-posta adreslerini içeren CSV dosyasını seçin ve sonra **Aç**' ı seçin.

    **Kullanıcı Ekle** PENCERESI, CSV dosyasındaki e-posta adresi listesini görüntüler. 
1. **Kaydet**’i seçin. 
1. **Kullanıcılar** bölmesinde, eklenen öğrencilerin listesini görüntüleyin. 

    :::image type="content" source="./media/how-to-configure-student-usage/list-of-added-users.png" alt-text="Kullanıcılar bölmesinde eklenen kullanıcıların listesi":::

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

    **Kullanıcılar** listesinin **davet** sütunu, eklenen her kullanıcı için davetiye durumunu görüntüler. Durum, **gönderilmesi** ve sonrasında **\<date> gönderilmesi** için değişiklik yapılmalıdır. 

### <a name="invite-selected-users"></a>Seçili kullanıcıları davet et

Bu yöntem, yalnızca belirli öğrencileri davet etmeyi ve diğer kişilerle paylaşabileceğiniz bir kayıt bağlantısı almanızı gösterir.

1. **Kullanıcılar** bölmesinde, listeden bir öğrenci veya birden çok öğrenci seçin. 

1. Seçtiğiniz öğrencinin satırındaki **zarf** simgesini seçin ya da araç çubuğunda **davet et**' i seçin. 

    ![Seçili kullanıcıları davet et](./media/how-to-configure-student-usage/invite-selected-users.png)

1. **E-posta ile davet gönder** penceresinde, isteğe bağlı bir **ileti** girin ve ardından **Gönder**' i seçin. 

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

1. **Kullanıcılar** bölmesinde Kullanıcı başına kota ' i seçin: araç çubuğunda **\<number> saat (ler)** . 
1. **Kullanıcı başına kota** penceresinde, her öğrenciye, zamanlanan sınıf süresi dışında vermek istediğiniz saat sayısını belirtin ve ardından **Kaydet**' i seçin.

    !["Kullanıcı başına kota" penceresi](./media/how-to-configure-student-usage/quota-per-user.png)    

    Değiştirilen değerler artık **Kullanıcı başına kota: \<number of hours>** araç çubuğunda ve kullanıcılar listesinde, burada gösterildiği gibi görüntülenir:

    ![Kullanıcı başına kota saati](./media/how-to-configure-student-usage/quot-per-user-after.png)

    > [!IMPORTANT]
    > [VM 'lerin zamanlanan çalışma süresi](how-to-create-schedules.md) , bir öğrenciye ayrılan kotaya göre sayılmaz. Kota, bir öğrencinin VM 'lerde harcadığı zamanlanan saatlerin dışında kalan süredir. 

## <a name="set-additional-quotas-for-specific-users"></a>Belirli kullanıcılar için ek kotalar ayarlama

Önceki bölümde yer alan tüm kullanıcılar için ayarlanan ortak kotaların ötesinde belirli öğrenciler için kotalar belirtebilirsiniz. Örneğin, bir eğitmen olarak tüm öğrenciler için kotayı 10 saat olarak ayarlayıp belirli bir öğrenci için 5 saatlik ek bir kota ayarlarsanız, bu öğrenci 15 (10 + 5) saatlik kotayı alır. Ortak kotayı daha sonra olarak değiştirirseniz, öğrencinin 20 (15 + 5) saat kotası alır. Bu genel kotanın zamanlanan sürenin dışında olduğunu unutmayın. Bir öğrencinin zamanlanan süre boyunca bir laboratuvar VM 'sinde harcadığı süre bu kotaya göre sayılmaz. 

Ek kotalar ayarlamak için şunları yapın:

1. **Kullanıcılar** bölmesinde, listeden bir öğrenci seçin ve ardından araç çubuğunda **kotayı ayarla** ' yı seçin. 

    !["Kotayı ayarla" düğmesi](./media/how-to-configure-student-usage/adjust-quota-button.png)

1. **\<selected user or users email address> Kotayı ayarla**' da, seçilen öğrenciye veya öğrencilere vermek istediğiniz ek laboratuvar saatleri sayısını girin ve ardından **Uygula**' yı seçin. 

    !["Kotayı ayarla..." penceresine](./media/how-to-configure-student-usage/additional-quota.png)

    **Kullanım** sütunu seçili öğrenciler için güncelleştirilmiş kotayı görüntüler. 

    ![Kullanıcı için yeni kullanım](./media/how-to-configure-student-usage/new-usage-hours.png)

## <a name="student-accounts"></a>Öğrenci hesapları

Bir sınıf laboratuvarına öğrenci eklemek için e-posta hesaplarını kullanın. Öğrenciler aşağıdaki e-posta hesabı türlerine sahip olabilir:

- University in Azure Active Directory örneği tarafından sunulan bir öğrenci e-posta hesabı.
- *Outlook.com*, *hotmail.com*, *msn.com* veya *Live.com* gibi bir Microsoft-Domain e-posta hesabı.
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

    İstemde, öğrenciler daha sonra GitHub hesaplarına bağlı bir Microsoft hesabı oluşturur. Bağlama, **İleri ' yi** seçtiklerinde otomatik olarak gerçekleşir. Bunlar daha sonra hemen oturum açırlar ve derslik laboratuvarına bağlanır.

## <a name="export-a-list-of-users-to-a-csv-file"></a>Bir CSV dosyasına kullanıcı listesini dışarı aktarma

1. **Kullanıcılar** bölmesine gidin.
1. Araç çubuğunda üç nokta (**...**) simgesini seçin ve ardından **CSV 'yi dışarı aktar**' ı seçin. 

    !["CSV 'yi dışarı aktar" düğmesi](./media/how-to-export-users-virtual-machines-csv/users-export-csv.png)

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelere bakın:

- Yöneticiler için: [Laboratuvar hesapları oluşturma ve yönetme](how-to-manage-lab-accounts.md)
- Laboratuvar sahipleri için: [Labs oluşturma ve yönetme](how-to-manage-classroom-labs.md) ve [şablonları ayarlama ve yayımlama](how-to-create-manage-template.md)
- Laboratuvar kullanıcıları için: [erişim laboratuvarları](how-to-use-classroom-lab.md)