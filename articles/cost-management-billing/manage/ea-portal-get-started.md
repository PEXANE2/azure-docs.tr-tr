---
title: Azure Enterprise Portal'ı kullanmaya başlama
description: Bu makalede Azure Kurumsal Anlaşma (Azure EA) müşterilerinin Azure Enterprise Portal'ı nasıl kullanacağı açıklanmaktadır.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 01/24/2020
ms.topic: conceptual
ms.service: cost-management-billing
manager: boalcsva
ms.openlocfilehash: 1cc795a7d74790b8f3ef2b913bd1be3e6ee523ac
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76769590"
---
# <a name="get-started-with-the-azure-enterprise-portal"></a>Azure Enterprise Portal'ı kullanmaya başlama

Bu makale, doğrudan ve dolaylı Azure Kurumsal Anlaşma (Azure EA) müşterilerinin [Azure Enterprise Portal](https://ea.azure.com)'ı kullanmalarına yardımcı olmaktadır. Şu konular hakkında temel bilgilere sahip olun:

- Azure Enterprise Portal'ın yapısı.
- Azure Enterprise Portal'da kullanılan roller.
- Abonelik oluşturma.
- Azure Enterprise Portal'da ve Azure portalında maliyet analizi.

Tam kapsamlı Azure Enterprise Portal ekleme oturumunu izlemek için şu videoyu görüntüleyin:

> [!VIDEO https://www.youtube.com/embed/OiZ1GdBpo-I]

## <a name="azure-enterprise-portal-hierarchy"></a>Azure Enterprise Portal hiyerarşisi

Azure Enterprise Portal hiyerarşisi şunlardan oluşur:

- **Azure Enterprise Portal**, Azure EA hizmetlerinin maliyetini yönetmenize yardımcı olan bir çevrimiçi yönetim portalıdır. Şunları yapabilirsiniz:

  - Departmanlar, hesaplar ve abonelikler içeren bir Azure EA hiyerarşisi oluşturma.
  - Tüketilen hizmetlerin maliyetlerini mutabık kılma, kullanım raporlarını indirme ve fiyat listelerini görüntüleme.
  - Kaydınız için API anahtarları oluşturma.

- **Departmanlar**, maliyetleri mantıksal gruplar halinde segmentlere ayırmanıza yardımcı olur. Departmanlar sayesinde departman düzeyinde bütçe veya kota belirleyebilirsiniz.

- **Hesaplar**, Azure Enterprise Portal'daki kuruluş birimleridir. Hesapları kullanarak abonelikleri ve erişimi raporlarını yönetebilirsiniz.

- **Abonelikler**, Azure Enterprise Portal'daki en küçük birimdir. Hizmet yöneticisi tarafından yönetilen Azure hizmetlerine yönelik kapsayıcılardır.

Aşağıdaki diyagramda basit Azure EA hiyerarşileri gösterilmektedir.

![Basit Azure EA hiyerarşileri diyagramı](./media/ea-portal-get-started/ea-hierarchies.png)

## <a name="enterprise-user-roles"></a>Kurumsal kullanıcı rolleri

Aşağıdaki yönetici kullanıcı rolleri, kuruluş kaydınızın bir parçasıdır:

- Kuruluş yöneticisi
- Departman yöneticisi
- Hesap sahibi
- Hizmet yöneticisi
- Bildirim ilgili kişisi

Roller, farklı görevleri tamamlamak için iki farklı portalda çalışır. Faturaları ve maliyetleri yönetmek için [Azure Enterprise Portal](https://ea.azure.com)'ı, Azure hizmetlerini yönetmek için ise [Azure portalını](https://portal.azure.com) kullanırsınız.

Kullanıcı rolleri bir kullanıcı hesabıyla ilişkilendirilir. Kullanıcının orijinalliğini doğrulamak için her kullanıcının geçerli bir iş, okul veya Microsoft hesabının olması gerekir. Her hesabın etkin bir şekilde izlenen bir e-posta adresiyle ilişkilendirildiğinden emin olun. Hesap bildirimleri e-posta adresine gönderilir.

Kullanıcıları ayarlarken, kuruluş yöneticisi rolüne birden fazla hesap atayabilirsiniz. Ancak hesap sahibi rolü yalnızca bir hesaba ait olabilir. Ayrıca hem kuruluş yöneticisi hem de hesap sahibi rollerini tek bir hesaba atayabilirsiniz.

### <a name="enterprise-administrator"></a>Kuruluş yöneticisi

Bu role sahip olan kullanıcılar en yüksek erişim düzeyine sahiptir. Bu kullanıcılar şunları yapabilir:

- Hesapları ve hesap sahiplerini yönetme.
- Diğer kuruluş yöneticilerini yönetme.
- Departman yöneticilerini yönetme.
- Bildirim kişilerini yönetme.
- Tüm hesaplarda kullanımı görüntüleme.
- Tüm hesaplarda faturalandırılmamış ücretleri görüntüleme.

Bir kuruluş kaydında birden çok kuruluş yöneticiniz olabilir. Kurumsal yöneticilere salt okuma erişimi verebilirsiniz. Hepsi departman yöneticisi rolünü devralırlar.

### <a name="department-administrator"></a>Departman yöneticisi

Bu role sahip kullanıcılar şunları yapabilir:

- Departmanlar oluşturma ve yönetme.
- Yeni hesap sahipleri oluşturma.
- Yönettikleri departmanların kullanım ayrıntılarını görüntüleme.
- Gerekli izinlere sahip olmaları halinde maliyetleri görüntüleme.

Her kuruluş kaydı için birden çok departman yöneticiniz olabilir.

Yeni bir departman yöneticisi oluşturduğunuzda veya var olan departman yöneticisini düzenlediğinizde salt okuma erişimi verebilirsiniz. Salt okuma seçeneğini **Evet** olarak ayarlayın.

### <a name="account-owner"></a>Hesap sahibi

Bu role sahip kullanıcılar şunları yapabilir:

- Abonelik oluşturma ve yönetme.
- Hizmet yöneticilerini yönetme.
- Aboneliklerin kullanımını görüntüleme.

Her hesap için benzersiz bir iş, okul veya Microsoft hesabı gerekir. Azure Enterprise Portal yönetici rolleri hakkında daha fazla bilgi için bkz. [Azure’daki Azure Kurumsal Anlaşma yönetici rollerini anlama](understand-ea-roles.md).

### <a name="service-administrator"></a>Hizmet yöneticisi

Hizmet yöneticisi rolünün Azure portalındaki hizmetleri yönetme ve kullanıcıları eş yönetici rolüne atama izinleri vardır.

### <a name="notification-contact"></a>Bildirim ilgili kişisi

Bildirim ilgili kişisi kayıtla ilgili kullanım bildirimlerini alır.

## <a name="activate-your-enrollment"></a>Kaydınızı etkinleştirme

Hizmetinizi etkinleştirmek için başlangıçtaki kuruluş yöneticisi [Azure Enterprise Portal](https://ea.azure.com)'ı açar ve davet e-postasındaki e-posta adresini kullanarak oturum açar.

Kuruluş yöneticisi olarak belirlendiyseniz, etkinleştirme e-postasını almanız gerekmez. [Azure Enterprise Portal](https://ea.azure.com)'a giderek iş, okul veya Microsoft hesabı e-posta adresinizle ve parolanızla oturum açabilirsiniz.

Birden fazla kaydınız varsa etkinleştirilecek bir kayıt seçin. Varsayılan olarak, yalnızca etkin kayıtlar gösterilir. Kayıt geçmişini görüntülemek için, Azure Enterprise Portal'ın sağ üst köşesindeki **Etkin** seçeneğinin işaretini kaldırın.

**Kayıt** altında, durum **Etkin** olarak gösterilir.

![Etkin kaydı gösteren örnek](./media/ea-portal-get-started/ea-enrollment-status.png)

Yalnızca mevcut Azure kuruluş yöneticileri, diğer kuruluş yöneticilerini oluşturabilir.

### <a name="create-another-enterprise-administrator"></a>Başka bir kuruluş yöneticisi oluşturma

Başka bir kuruluş yöneticisi eklemek için:

1. [Azure Enterprise Portal](https://ea.azure.com)'da oturum açın.
1. **Yönet** > **Kayıt Ayrıntıları**'na gidin.
1. Sağ üstten **+ Yönetici Ekle**'yi seçin.

Kullanıcının e-posta adresine ve iş, okul veya Microsoft hesabı gibi tercih edilen kimlik doğrulama yöntemine sahip olduğunuzdan emin olun.

Kuruluş yöneticisi değilseniz, sizi bir kayda eklemesini istemek için bir kuruluş yöneticisiyle iletişime geçin. Bir kayda eklendikten sonra etkinleştirme e-postası alırsınız.

Kuruluş yöneticiniz size yardımcı olamıyorsa bir [Azure Enterprise Portal destek isteği](https://support.microsoft.com/supportrequestform/cf791efa-485b-95a3-6fad-3daf9cd4027c) oluşturun. Şu bilgileri belirtin:

- Kayıt numarası
- Eklenecek e-posta adresi ve kimlik doğrulama türü (iş, okul veya Microsoft hesabı)
- Mevcut bir kuruluş yöneticisinden e-posta onayı
  - Mevcut kuruluş yöneticisine ulaşılamıyorsa, Toplu Lisanslama Hizmeti Merkezi (VLSC) aracı üzerinden iletişim ayrıntılarını değiştirmesini istemek için iş ortağınız veya yazılım danışmanınızla iletişime geçin.

Kuruluş yöneticisi rolleri hakkında daha fazla bilgi için bkz. [Azure’daki Azure Kurumsal Anlaşma yönetici rollerini anlama](understand-ea-roles.md).

## <a name="create-an-azure-enterprise-department"></a>Azure Kurumsal departmanı oluşturma

Kuruluş yöneticileri ve departman yöneticileri, kurumsal Azure hizmetlerini ve kullanımı departman ve maliyet merkezine göre düzenleyip raporlamak için departmanları kullanır. Kuruluş yöneticisi şunları yapabilir:

- Departman ekleme veya kaldırma.
- Bir hesabı bir departmanla ilişkilendirme.
- Departman yöneticileri oluşturma.
- Departman yöneticilerinin fiyat ve maliyetleri görüntülemesine izin verme.

Departman yöneticisi, departmanına yeni hesaplar ekleyebilir. Departmanlarından hesapları kaldırabilir, ancak kayıttan kaldıramaz.

Bir departman eklemek için:

1. Azure Enterprise Portal'da oturum açın.
1. Sol bölmede **Yönet**’i seçin.
1. **Departman** sekmesinde **+ Departman Ekle**'yi seçin.
1. Bilgileri girin.
   Tek gerekli alan departman adıdır. En az 3 karakter uzunluğunda olmalıdır.
1. Bittiğinde **Ekle**’yi seçin.

## <a name="add-a-department-administrator"></a>Departman yöneticisi ekleme

Bir departman oluşturulduktan sonra, kuruluş yöneticisi departman yöneticileri ekleyebilir ve her birini bir departmanla ilişkilendirebilir. Departman yöneticileri, kendi departmanlarında aşağıdaki eylemleri gerçekleştirebilir:

- Diğer departman yöneticilerini oluşturma
- Ad veya maliyet merkezi gibi departman özelliklerini görüntüleme ve düzenleme
- Hesap ekleme
- Hesapları kaldırma
- Kullanım ayrıntılarını indirme
- Aylık kullanımı ve ücretleri görüntüleme <sup>1</sup>

> <sup>1</sup> Kuruluş yöneticisi bu izinleri vermiş olmalıdır. Departmanın aylık kullanım ve ücretlerini görüntülemenize izin verilmesine rağmen görüntüleyemiyorsanız iş ortağınızla iletişime geçin.

### <a name="to-add-a-department-administrator"></a>Departman yöneticisi eklemek için

Kuruluş yöneticisi olarak:

1. Azure Enterprise Portal'da oturum açın.
1. Sol bölmede **Yönet**’i seçin.
1. **Departman** sekmesini ve ardından istediğiniz departmanı seçin.
1. **+ Yönetici Ekle**’yi seçip gerekli bilgileri ekleyin.
1. Salt okuma erişimi için **Salt Okuma** seçeneğini **Evet** yapıp **Ekle**’yi seçin.

![Departman Yöneticisi Ekle iletişim kutusunu gösteren örnek](./media/ea-portal-get-started/ea-create-add-department-admin.png)

### <a name="to-set-read-only-access"></a>Salt okuma erişimini ayarlamak için

Departman yöneticilerine salt okuma erişimi verebilirsiniz.

- Yeni bir departman yöneticisi oluşturduğunuzda salt okuma seçeneğini **Evet** olarak ayarlayın.

- Mevcut bir departman yöneticisini düzenlemek için:
   1. Bir departman seçin ve ardından düzenlemek istediğiniz **Departman Yöneticisinin** yanındaki kalem simgesini seçin.
   1. Salt okuma değerini **Evet** olarak ayarlayıp **Kaydet**'i seçin.

Kuruluş yöneticileri, departman yöneticisi izinlerini otomatik olarak alır.

## <a name="add-an-account"></a>Hesap ekleme

Hesapların ve aboneliklerin yapısı, bunların nasıl yönetildiğini ve faturalarınızda ve raporlarınızda nasıl göründüğünü etkiler. İşletme bölümlerine, görev ekiplerine ve coğrafi konumlara göre yapılandırma, tipik organizasyon örnekleridir.

Hesap eklemek için:

1. Azure Enterprise Portal'ın sol tarafındaki gezinti alanında **Yönet**'i seçin.
1. **Hesap** sekmesini seçin. **Hesap** sayfasında **+Hesap Ekle**'yi seçin.
1. Bir departman seçin veya atanmamış olarak bırakın ve sonra istediğiniz kimlik doğrulaması türünü seçin.
1. Raporlama sırasında hesabı tanımlamak için kullanılacak kolay bir ad girin.
1. Yeni hesapla ilişkilendirilecek **Hesap Sahibi E-posta** adresini girin.
1. E-posta adresini onaylayın ve **Ekle**'yi seçin.

![Hesapların listesini ve +Hesap Ekle seçeneğini gösteren örnek](./media/ea-portal-get-started/create-ea-add-an-account.png)

**Başka Bir Hesap Ekle**’yi seçerek başka bir hesap ekleyebilir ya da soldaki araç çubuğunun sağ alt köşesindeki **Ekle**’yi seçebilirsiniz.

Hesap sahipliğini onaylamak için:

1. Azure Enterprise Portal'da oturum açın.
1. Durumu görüntüleyin.

   **Beklemede** olan durum **Başlangıç/Bitiş tarihi** olarak değişmelidir. Başlangıç/Bitiş tarihi, kullanıcının ilk kez oturum açtığı tarih ve anlaşma bitiş tarihidir.
1. Hesap sahibi Azure Enterprise Portal'da ilk kez oturum açtığında açılan **Uyarı** iletisinde **Devam**'ı seçerek hesabı etkinleştirmesi gerekir.

## <a name="change-account-owner"></a>Hesap sahibini değiştirme

Kuruluş yöneticileri bir kayıttaki abonelik hesap sahipliğini aktarmak için Azure Enterprise Portal'ı kullanabilir. Bu eylem, bir kaynak kullanıcı hesabındaki tüm abonelikleri hedef kullanıcı hesabına taşır.

Hesapları aktarırken şu önemli bilgileri unutmayın:

- Şu aktarımları yapabilirsiniz:
  - Bir iş veya okul hesabından başka bir iş veya okul hesabına.
  - Bir Microsoft hesabından bir iş veya okul hesabına.
  - Bir Microsoft hesabından başka bir Microsoft hesabına.
  
    Hedef hesabın aktarımlara yönelik geçerli bir hedef olması için geçerli bir Azure Ticari hesabı olması gerekir. Yeni hesaplar için, Azure Enterprise Portal'da oturum açarken bir Azure Ticari hesabı oluşturmanız istenir. Mevcut hesaplar için, hesabın uygun olabilmesi için yeni bir Azure aboneliği oluşturmanız gerekir.

- İş veya okul hesabından Microsoft hesabına aktarım yapamazsınız.

- Abonelik aktarımını tamamladığınızda, Microsoft hesap sahibini güncelleştirir.

Şu rol tabanlı erişim denetimi (RBAC) ilkelerini kavrayın:

- Aynı kiracıdaki iki kuruluş kimliği arasında abonelik aktarımları gerçekleştirdiğinizde, RBAC ilkeleri ve mevcut hizmet yöneticisi ile eş yönetici rolleri korunur.
- Diğer abonelik aktarımları, RBAC ilkelerinizin ve rol atamalarınızın kaybedilmesiyle sonuçlanır.
- İlkeler ve yönetici rolleri farklı dizinler arasında aktarılmaz. Hizmet yöneticileri hedef hesabın sahibiyle güncelleştirilir.

Hesap sahibini değiştirmeden önce:

1. Azure Enterprise Portal'da **Hesap** sekmesini görüntüleyin ve kaynak hesabı belirleyin. Kaynak hesap etkin olmalıdır.
1. Hedef hesabı belirleyin ve etkin olduğundan emin olun.

Tüm aboneliklerin hesap sahipliğini aktarmak için:

1. Azure Enterprise Portal'da oturum açın.
1. Sol gezinti alanında **Yönet**’i seçin.
1. **Hesap** sekmesini seçin ve bir hesabın üzerine gelin.
1. Sağ taraftaki hesap sahibini değiştir simgesini seçin. Simge bir kişi gibi görünür.
1. Uygun bir hesap seçin ve ardından **İleri**'yi seçin.
1. Aktarımı onaylayın ve **Gönder**'i seçin.

![Hesap Sahibini Değiştir simgesini gösteren görüntü](./media/ea-portal-get-started/create-ea-create-sub-transfer-account-ownership-of-sub.png)

Tek bir aboneliğin hesap sahipliğini aktarmak için:

1. Azure Enterprise Portal'da oturum açın.
1. Sol gezinti alanında **Yönet**’i seçin.
1. **Hesap** sekmesini seçin ve bir hesabın üzerine gelin.
1. Sağdaki abonelikleri aktar simgesini seçin. Simge bir sayfa gibi görünür.
1. Uygun bir abonelik seçin ve ardından **İleri**'yi seçin.
1. Aktarımı onaylayın ve sonra **Gönder**'i seçin.

![Abonelikleri Aktar simgesini gösteren görüntü](./media/ea-portal-get-started/ea-transfer-subscriptions.png)

Azure Enterprise Portal kullanıcı yönetimini görmek için şu videoyu izleyin:

> [!VIDEO https://www.youtube.com/embed/621jVkvmwm8]

## <a name="create-a-subscription"></a>Abonelik oluşturma

Hesap sahipleri, abonelikleri görüntüleyebilir ve yönetebilir. Kuruluşunuzdaki takımlara geliştirme ortamları ve projelere yönelik erişim izni vermek için abonelikleri kullanabilirsiniz. Örneğin: test, üretim, geliştirme ve hazırlama.

Her uygulama ortamı için farklı abonelikler oluşturduğunuzda her bir ortamın güvenliğini sağlamaya yardımcı olursunuz.

- Ayrıca, her abonelik için farklı bir hizmet yöneticisi hesabı da atayabilirsiniz. 
- Abonelikleri dilediğiniz sayıda hizmet ile ilişkilendirebilirsiniz.
- Hesap sahibi, abonelikler oluşturur ve hesaplarındaki her aboneliğe bir hizmet yöneticisi hesabı atar.

### <a name="add-a-subscription"></a>Abonelik ekleme

Bir abonelik eklemek için aşağıdaki bilgileri kullanın.

Hesabınıza ilk kez bir abonelik eklediğinizde, Microsoft Çevrimiçi Abonelik Sözleşmesi'ni (MOSA) ve bir ücret planını kabul etmeniz istenir. Kurumsal Anlaşma müşterileri için geçerli olmasa da, aboneliğinizin oluşturulması için MOSA ve ücret planı gereklidir. Microsoft Azure Kurumsal Anlaşma Kaydı Değişikliği, yukarıdaki öğelerin yerini alır ve sözleşme ilişkiniz değişmez. Sorulduğunda, koşulları kabul ettiğinizi belirten kutuyu seçin.

_Microsoft Azure Enterprise_, oluşturulan abonelikler için seçilen varsayılan addır. Kaydınızdaki diğer aboneliklerden ayırmak ve kuruluş düzeyindeki raporlarda ayırt edilebilmesini sağlamak için abonelik adını değiştirebilirsiniz.

Abonelik eklemek için:

1. Azure Enterprise Portal'da hesap oturumu açın.
1. **Yönetici** sekmesini ve sonra sayfanın üst kısmındaki **Abonelik** girişini seçin.
1. Hesabın hesap sahibi olarak oturum açtığınızı doğrulayın.
1. **+Abonelik Ekle** ve ardından **Satın Al**’ı seçin.

   Bir hesaba ilk kez abonelik eklediğinizde iletişim bilgilerinizi sağlamanız gerekir. Başka abonelikler eklerken, iletişim bilgileriniz sizin için eklenir.

1. **Abonelikler**'i ve ardından oluşturduğunuz aboneliği seçin.
1. **Abonelik Ayrıntılarını Düzenle**'yi seçin.
1. **Abonelik Adı** ve **Hizmet Yöneticisi**’ni düzenleyip onay işaretini seçin.
  
   Abonelik adı raporlarda görünür. Geliştirme portalındaki abonelikle ilişkili projenin adıdır.

Yeni aboneliklerin abonelikler listesinde görünmesi 24 saate kadar sürebilir. Bir abonelik oluşturduktan sonra şunları yapabilirsiniz:

- [Abonelik ayrıntılarını düzenleme](https://account.azure.com/Subscriptions)
- [Abonelik hizmetlerini yönetme](https://portal.azure.com/#home)

## <a name="transfer-an-enterprise-subscription-to-a-pay-as-you-go-subscription"></a>Kurumsal aboneliği Kullandıkça Öde aboneliğine aktarma

Bir Kurumsal aboneliği Kullandıkça Öde fiyatlarına sahip bir aboneliğe aktarmak için Azure Enterprise Portal'da yeni bir destek isteği oluşturmanız gerekir. Destek isteği oluşturmak için **Yardım ve Destek** alanındaki **+ Yeni destek isteği**'ni seçin.

## <a name="associate-an-existing-account-with-your-pay-as-you-go-subscription"></a>Mevcut bir hesabı Kullandıkça Öde aboneliğiyle ilişkilendirme

Azure portalında bir Microsoft Azure hesabınız varsa bunu Kurumsal Anlaşma kaydınızla ilişkilendirmek için ilgili okul, iş veya Microsoft hesabını girin.

### <a name="associate-an-existing-account"></a>Mevcut bir hesabı ilişkilendirme

1. Azure Enterprise Portal'da **Yönet**'i seçin.
1. **Hesap** sekmesini seçin.
1. **+Hesap ekle**'yi seçin.
1. Mevcut Azure hesabıyla ilişkilendirilmiş olan iş, okul veya Microsoft hesabını girin.
1. Mevcut Azure hesabıyla ilişkilendirilmiş olan hesabı onaylayın.
1. Bu hesabı raporlarda tanımlayabilmek için kullanmak istediğiniz adı yazın.
1. **Add (Ekle)** seçeneğini belirleyin.
1. **+Hesap Ekle** seçeneğini yeniden seçerek başka bir hesap ekleyebilir veya **Yönetim** düğmesini seçerek giriş sayfasına dönebilirsiniz.
1. **Hesap** sayfasını görüntülediğinizde yeni hesabın **Beklemede** olduğunu görürsünüz.

### <a name="confirm-account-ownership"></a>Hesap sahipliğini onaylama

1. Girdiğiniz iş, okul veya Microsoft hesabıyla ilişkilendirilmiş olan e-posta hesabında oturum açın.
1. _"Microsoft Toplu Lisanslama'dan Microsoft Azure Hizmeti Hesabınızı Etkinleştime Daveti"_ başlıklı e-posta bildirimini açın.
1. Davetin içindeki **Microsoft Azure Enterprise Portal'da oturum aç** bağlantısını seçin.
1. **Oturum aç**'ı seçin.
1. Oturum açmak ve hesabın sahibi olduğunuzu doğrulamak için iş, okul veya Microsoft hesabınızı ve parolanızı girin.

### <a name="azure-marketplace"></a>Azure Market

Çoğu abonelik Kullandıkça Öde ortamından Azure Kurumsal Anlaşma'ya dönüştürülebilir ancak Azure Market bunlardan biri değildir. Tüm abonelikleri ve ücretleri tek bir yerden görüntülemek için Azure Market hizmetlerini Azure Enterprise Portal'a eklemenizi öneririz.

1. Azure Enterprise Portal'da oturum açın.
1. Sol gezinti panelinde **Yönet**’i seçin.
1. **Kayıt** sekmesini seçin.
1. **Kayıt Ayrıntıları** bölümünü görüntüleyin.
1. Azure Market alanının sağ tarafında yer alan kalem simgesini seçerek hizmeti etkinleştirin. **Kaydet**’i seçin.

Hesap sahibi artık daha önce Kullandıkça Öde aboneliğiyle sahip olduğu Azure Market hizmetlerini satın alabilir.

Yeni Azure Market abonelikleri Azure EA kaydınızda etkinleştirildikten sonra Kullandıkça Öde ortamında oluşturulan Azure Market aboneliklerini iptal edin. Kullandıkça Öde ortamındaki ödeme aracınızın süresi dolduğunda Azure Market aboneliklerinizin kullanım dışı kalmaması için bu adım kritik öneme sahiptir.

### <a name="msdn"></a>MSDN

MSDN abonelikleri otomatik olarak MSDN Geliştirme ve Test aboneliğine dönüştürülür ve Azure EA teklifindeki mevcut parasal kredi silinir.

### <a name="azure-in-open"></a>Open ile Azure

Bir Open ile Azure aboneliğini bir Kurumsal Anlaşma ile ilişkilendirmek, tüketilmemiş tüm Open ile Azure kredilerinin kaybedilmesine yol açar. Bu nedenle Kurumsal Anlaşma'ya eklemeden önce Open ile Azure aboneliğinizdeki tüm krediyi kullanmanız önerilir.  

### <a name="accounts-with-support-subscriptions"></a>Destek aboneliğine sahip hesaplar

Kurumsal Anlaşma hesabınızda destek aboneliği yoksa ve Azure Enterprise Portal'a destek aboneliğine sahip bir hesap eklerseniz, MOSA destek aboneliğiniz otomatik olarak aktarılmaz. Kullanım süresi içinde (takip eden ayın sonuna kadar) Azure EA'da yeniden destek aboneliği satın almanız gerekir.

## <a name="view-usage-summary-and-download-reports"></a>Kullanım özetini görüntüleme ve raporları indirme

Kuruluş yöneticileri, Azure Enterprise Portal'da kullanım verilerinin özetini, tüketilen parasal taahhüdü ve ek kullanımla ilişkili ücretleri görüntüleyebilir. Ücretler, tüm hesaplar ve abonelikler genelinde özet düzeyinde gösterilir.

Belirli hesaplardaki ayrıntılı kullanımı görüntülemek için kullanım ayrıntıları raporunu indirin:

1. Azure Enterprise Portal'da oturum açın.
1. **Raporlar**’ı seçin.
1. **Kullanımı İndir** sekmesini seçin.
1. Rapor listesinde indirmek istediğiniz aylık rapora ait olan **İndir** bağlantısını seçin.

   > [!NOTE]
   > Kullanım ayrıntıları raporu hiçbir geçerli vergiyi içermez.
   >
   > Kullanımın tahakkuk etmesiyle rapora yansıtılması arasında sekiz saatlik bir gecikme olabilir.

Kullanım özeti raporlarını ve grafiklerini görüntülemek için:

1. Azure Enterprise Portal'da oturum açın.

1. Bir taahhüt dönemi seçin.

   **Kullanım Özeti** tarih aralığını değiştirmek için sayfanın sağ üst tarafından **M** (Aylık) yerine **C** (Özel) görünüme geçiş yapabilir ve özel başlangıç ve bitiş tarihleri girebilirsiniz.

   ![Özel görünümde kullanım özetini oluşturup görüntüleme ve raporları indirme](./media/ea-portal-get-started/create-ea-view-usage-summary-and-download-reports-custom-view.png)
1. Ek ayrıntıları görüntülemek için grafikte bir dönem veya ay seçebilirsiniz.

   - Grafik, kullanılan kullanım, fazla servis ücreti, ayrı olarak faturalandırılan ücretler ve Azure Market ücretlerinin dökümü ile aylık kullanım üzerinden ayı gösterir.
   - Seçilen ay için grafiğin altındaki alanları kullanarak departmanlara, hesaplara ve aboneliklere göre filtreleme yapabilirsiniz.
   - **Hizmetlere Göre Ücret** ve **Hiyerarşiye Göre Ücret** arasında geçiş yapabilirsiniz.
   - İlgili bölümleri genişleterek **Azure Hizmeti**, **Ayrı Olarak Faturalandırılan Ücretler** ve **Azure Market** ile ilgili ayrıntıları görüntüleyebilirsiniz.

Kullanımı nasıl görüntüleyeceğinizi öğrenmek için şu videoyu izleyin:

> [!VIDEO https://www.youtube.com/embed/Cv2IZ9QCn9E]

### <a name="download-csv-reports"></a>CSV raporlarını indirme

Kuruluş yöneticileri, aşağıdaki raporları CSV dosyası olarak indirmek için Aylık Rapor İndirme sayfasını kullanır:

- Bakiye ve ücret
- Kullanım ayrıntıları
- Azure Market ücretleri
- Fiyat listesi

Raporları indirmek için:

1. Azure Enterprise Portal'da **Rapor**'u seçin.
2. Sayfanın üst kısmındaki **Kullanımı İndir**'i seçin.
3. Ay raporunun yanındaki **İndir**'i seçin.

   > [!NOTE]
   > Tahakkuk eden kullanım tarihi ile kullanımın raporlarda gösterilmesi arasında beş güne kadar gecikme olabilir.
   >
   > Safari ile Excel'e CSV dosyalarını indiren kullanıcılar biçimlendirme hatalarıyla karşılaşabilir. Hataları önlemek için dosyayı bir metin düzenleyicisi kullanarak açın.

![Kullanımı İndir sayfasını gösteren örnek](./media/ea-portal-get-started/create-ea-download-csv-reports.png)

Kullanım bilgilerinin nasıl indirileceğini görmek için şu videoyu izleyin:

> [!VIDEO https://www.youtube.com/embed/eY797htT1qg]

### <a name="advanced-report-download"></a>Gelişmiş rapor indirme

Belirli tarih aralıklarını veya hesapları kapsayan raporlar oluşturmak için gelişmiş rapor indirme işlevini kullanabilirsiniz. Çıkış dosyası, büyük kayıt kümelerinin kullanılabilmesi için CSV biçimindedir.

1. Azure Enterprise Portal'da **Gelişmiş Rapor İndirme**'yi seçin.
1. Uygun bir tarih aralığı ve uygun hesapları seçin.
1. **Kullanım Verilerini İste**'yi seçin.
1. Rapor durumu **İndir** olana kadar **Yenile** düğmesini seçin.
1. Raporu indirin.

### <a name="download-usage-reports-and-billing-information-for-a-prior-enrollment"></a>Eski bir kaydın kullanım raporlarını ve ödeme bilgilerini indirme

Kayıt aktarımı gerçekleştirildikten sonra eski kaydın kullanım raporlarını ve ödeme bilgilerini indirebilirsiniz. Geçmişe dönük raporlama hem Azure Enterprise Portal'da hem de maliyet yönetiminde sunulmaktadır.

Azure Enterprise Portal, etkin olmayan kayıtları filtreleyerek görünümün dışında tutar. Etkin olmayan aktarılmış kayıtları görüntülemek için **Etkin** kutusunun işaretini kaldırmanız gerekir.  

![Etkin kutusunun işaretini kaldırarak devre dışı olan kayıtları görebilirsiniz](./media/ea-portal-get-started/unchecked-active-box.png)

## <a name="azure-ea-term-glossary"></a>Azure EA terimleri sözlüğü

- **Hesap**: Azure Enterprise Portal'daki kuruluş birimleridir. Abonelikleri yönetmek ve raporlama için kullanılır.
- **Hesap sahibi**: Azure'da abonelikleri ve hizmet yöneticilerini yöneten kişidir. Bu kullanıcılar hesapla ve ilişkilendirilmiş aboneliklerle ilgili kullanım verilerini görüntüleyebilir.
- **Değişiklik aboneliği**: Kayıt değişikliği kapsamında bir yıllık veya uyuşan dönem boyunca devam eden aboneliktir.
- **Taahhüt**: Azure hizmetlerinin ön ödeme karşılığında kullanılması için taahhüt edilmiş indirimli ücretlerden karşılanması amacıyla gerçekleştirilen yıllık parasal tutar taahhüttür.
- **Bölüm yöneticisi**: Bölümleri yöneten, yeni hesap ve hesap sahibi oluşturan, yönetilen bölümlerle ilgili kullanım ayrıntılarını görüntüleyen ve izin verildiğinde maliyetleri görüntüleyebilen kişidir.
- **Kayıt numarası**: Microsoft tarafından Kurumsal Anlaşma ile ilişkilendirilmiş olan kaydı tanımlamak için verilen benzersiz tanıtıcıdır.
- **Kuruluş yöneticisi**: Azure'daki departmanları, departman sahiplerini, hesapları ve hesap sahiplerini yöneten kişidir. Kuruluş yöneticileri, diğer kuruluş yöneticilerini de yönetmenin yanı sıra kurumsal kayıtla ilişkilendirilmiş olan tüm hesaplara ve aboneliklere ait olan kullanım verilerini, faturalandırılan miktarları ve faturalandırılmamış ücretleri görüntüleyebilir.
- **Kurumsal anlaşma**: Kuruluşlarının tamamında Microsoft teknolojilerini standart hale getirmek ve Microsoft yazılım standardını temel alan bir bilgi teknolojisi altyapısı kurmak isteyen, merkezi satın alma sistemine sahip olan müşterilere yönelik bir Microsoft lisans sözleşmesidir.
- **Kurumsal anlaşma kaydı**: Microsoft ürünlerini toplu olarak ve indirimli fiyatlarla sunan Kurumsal Anlaşma programı kaydıdır.
- **Microsoft hesabı**: Katılan sitelerin kullanıcının kimliğini tek bir kimlik bilgisi ile doğrulamasını sağlayan Web tabanlı bir hizmettir.
- **Microsoft Azure Kurumsal Kayıt Değişikliği (kayıt değişikliği)** : Bir kuruluş tarafından imzalanan ve kurumsal kayıt çerçevesinde Azure erişimi sağlayan değişikliktir.
- **Azure Enterprise Portal**: Kurumsal müşterilerimizin Azure hesaplarını ve ilişkili aboneliklerini yönetmek için kullandığı portaldır.
- **Kullanılan kaynak miktarı**: Bir ay içinde kullanılan tek bir Azure hizmetinin miktarıdır.
- **Hizmet yöneticisi**: Azure Enterprise Portal'daki aboneliklere ve geliştirme projelerine erişebilen ve bunları yönetebilen kişidir.
- **Abonelik**: Bir Azure Enterprise Portal aboneliğini temsil eder ve aynı hizmet yöneticisi tarafından yönetilen Azure hizmetlerini içeren bir kapsayıcıdır.
- **İş veya okul hesabı**: Buluta federasyon ile Active Directory ayarlamış olan ve tüm hesapları tek bir kiracıda bulunan kuruluşlara yöneliktir.

### <a name="enrollment-statuses"></a>Kayıt durumları

- **Beklemede**: Kayıt yöneticisinin Azure Enterprise Portal'da oturum açması gerekir. Oturum açıldıktan sonra kayıt "Etkin" duruma geçer.
- **Etkin**: Kayıt "Etkin" durumdadır, Azure Enterprise Portal'da hesaplar ve abonelikler oluşturulabilir. Kurumsal Anlaşma'nın bitiş tarihine kadar kayıt etkin durumda kalır.
- **Süresiz uzatılan süre**: Süresiz uzatılan süre durumu, Kurumsal Anlaşma bitiş tarihi geçtikten sonra gerçekleşir. Uzatılan süreyi kabul eden Azure EA müşterilerinin Azure hizmetlerini Kurumsal Anlaşmaları bitene kadar sınırsız bir şekilde kullanmalarını sağlar.

   Azure EA kaydı Kurumsal Anlaşma bitiş tarihine ulaşana kadar layıt yöneticisinin aşağıdaki seçeneklerden birini belirlemesi gerekir:

  - Parasal taahhüt ekleyerek kaydı yenileme.
  - Yeni bir kayda aktarma.
  - Microsoft Çevrimiçi Abonelik Programı'na (MOSP) geçme.
  - Kayıtla ilişkili tüm hizmetleri devre dışı bırakmayı onaylama.
- **Süresi Doldu**: Azure EA müşterisi uzatılan süreyi kabul etmemiş ve Azure EA kaydı Kurumsal Anlaşma bitiş tarihine gelmiştir. Kaydın süresi dolacak ve ilgili tüm hizmetler devre dışı bırakılacaktır.
- **Aktarıldı**: İlişkilendirilmiş hesapların ve hizmetlerin yeni bir kayda aktarıldığı kayıtların durumu "Aktarıldı" olarak görünür.
  >[!NOTE]
  > Yenileme sırasında yeni bir kayıt numarası oluşturulduysa kayıtlar otomatik olarak aktarılmaz. Otomatik aktarım için yenileme belgelerinize önceki kayıt numaranızı eklemeniz gerekir.

## <a name="get-started-on-azure-ea---faq"></a>Azure EA ile çalışmaya başlama hakkında SSS

Bu bölümde, katılım sürecinde müşteriler tarafından sorulan tipik sorulara yer verilmiştir.  

### <a name="can-i-associate-my-existing-azure-account-to-azure-ea-enrollment"></a>Mevcut Azure hesabımı Azure EA kaydıyla ilişkilendirebilir miyim?

Evet. Hesap sahibi olduğunuz tüm Azure aboneliklerinin Kurumsal Anlaşmanıza dönüştürülecek olmasıdır. Visual Studio, AzurePass, MPN ve BizSpark gibi aylık kredi kullanan abonelikler dahildir. Bu tür abonelikler dönüştürülürken aylık krediyi kaybedersiniz.

### <a name="i-accidentally-associated-my-existing-azure-account-with-azure-ea-enrollment-as-a-result-i-lost-my-monthly-credit-can-i-get-my-monthly-credit-back"></a>Mevcut Azure hesabımı yanlışlıkla Azure EA kaydıyla ilişkilendirdim. Bunun sonucunda aylık kredimi kaybettim. Aylık kredimi geri alabilir miyim?

Azure EA hesap sahibi olarak oturum açarken Visual Studio aboneliğinizle aynı kimlik bilgilerini kullandıysanız aşağıdaki eylemlerden birini gerçekleştirerek bireysel Visual Studio aboneliği Azure avantajınızı kurtarabilirsiniz:

- İlgili Azure aboneliklerini kaldırdıktan veya taşıdıktan sonra hesap sahibinizi Azure Enterprise Portal'dan silin. Ardından bireysel Visual Studio Azure avantajlarına yeniden kaydolun.
- VLSC'deki yönetici sitesinden Visual Studio abonesini silin ve aboneliği bu kez farklı kimlik bilgilerine sahip bir hesaba atayın. Ardından bireysel Visual Studio Azure avantajlarına yeniden kaydolun.

### <a name="what-type-of-subscription-should-i-create"></a>Ne tür bir abonelik oluşturmalıyım?

Azure Enterprise Portal, kurumsal müşteriler için iki tür abonelik sunar:

- Microsoft Azure Kurumsal; şu kullanımlar için idealdir:
  - Her türlü üretim amaçlı kullanımlar
  - Altyapı harcamasına göre en iyi fiyatlar
  
  Daha fazla bilgi için [Azure satış ekibiyle iletişime geçin](https://azure.microsoft.com/pricing/enterprise-agreement/).

- Kurumsal Geliştirme ve Test; şu kullanımlar için idealdir:
  - Ekiple yürütülen tüm geliştirme ve test iş yükleri
  - Orta ve ağır düzeyde bireysel geliştirme/test iş yükleri
  - Özel MSDN görüntülerine erişim ve tercihe bağlı hizmet ücretleri
  
  Daha fazla bilgi için bkz. [Kurumsal Geliştirme ve Test teklifi](https://azure.microsoft.com/offers/ms-azr-0148p/).

### <a name="is-it-possible-to-transfer-subscription-ownership-to-another-account"></a>Aboneliğin sahipliği başka bir hesaba aktarılabilir mi?

Evet, aboneliğin sahipliğini başka bir hesaba aktarabilirsiniz. Örneğin A hesabında üç abonelik varsa kuruluş yöneticisi bu aboneliklerden birini B hesabına, birini C hesabına, diğerini de D hesabına veya tüm abonelikleri birden E hesabına aktarabilir.

Abonelikleri aktarmak için:

1. Azure Enterprise Portal'da **Yönet** > **Hesap** yolunu izleyin.
1. En sağdaki **Hesap** girişinin üzerine gelerek **Sahipliği Aktar** (kişi simgesi) ve **Aboneliği Aktar** (liste simgesi) seçeneklerini görüntüleyin. Bu seçenekler yalnızca etkin hesaplar için görünür.

### <a name="my-subscription-name-is-the-same-as-the-offer-name-should-i-change-the-subscription-name-to-something-meaningful-to-my-organization"></a>Aboneliğimin adı, teklifin adıyla aynı. Aboneliğin adını kuruluşum için daha anlamlı olacak bir adla değiştirmeli miyim?

Yeni bir abonelik oluşturduğunuzda ad için varsayılan olarak seçtiğiniz teklif türü kullanılır. Daha kolay takip edebilmeniz için aboneliğin adını daha anlamlı olacak şekilde değiştirmeniz önerilir.

Adı değiştirmek için:

1. [https://account.windowsazure.com](https://account.windowsazure.com) adresinde oturum açın.
1. Abonelik listesini seçin.
1. Düzenlemek istediğiniz aboneliği seçin.
1. **Aboneliği Yönet** simgesini seçin.
1. Abonelik ayrıntılarını düzenleyin.

### <a name="how-can-i-track-costs-incurred-by-a-cost-center"></a>Maliyetleri maliyet merkezine göre nasıl izleyebilirim?

Maliyetleri maliyet merkezine göre izlemek için şu düzeylerden birinde maliyet merkezi tanımlamanız gerekir:

- Bölüm
- Hesap
- Abonelik

Gereksinimlerinize bağlı olarak belirli bir maliyet merkezi ile ilişkilendirilmiş olan kullanımı ve maliyeti izlemek için aynı maliyet merkezini kullanabilirsiniz.

Örneğin birden fazla bölümün dahil olduğu özel bir projenin maliyetini izlemek için kullanım ve maliyet takibini abonelik düzeyinde maliyet merkezi tanımlayarak yapabilirsiniz.

Hizmet düzeyinde maliyet merkezi tanımlayamazsınız. Kullanımı hizmet düzeyinde izlemek istiyorsanız hizmet düzeyindeki _Etiket_ özelliğini kullanabilirsiniz.

### <a name="how-do-i-track-usage-and-spend-by-different-departments-in-my-organization"></a>Kuruluşumun farklı bölümleri tarafından gerçekleştirilen kullanımları ve harcamaları nasıl izleyebilirim?

Azure EA kaydı çerçevesinde istediğiniz kadar bölüm oluşturabilirsiniz. Kullanımı doğru şekilde izlemek için aboneliklerin birden fazla bölüm arasında paylaşılmadığından emin olmanız gerekir.

Departmanları ve abonelikleri oluşturduktan sonra verileri kullanım raporunda görebilirsiniz. Bu bilgiler, departman düzeyinde kullanımı izlemenize ve maliyet ile harcama yönetimi gerçekleştirmenize yardımcı olabilir.

Kullanım verilerine raporlama API'si aracılığıyla da erişebilirsiniz. Ayrıntılı bilgi ve örnek kod için bkz. [Raporlama API'si belgeleri](https://ea.azure.com/helpdocs/reportingAPI).

### <a name="can-i-set-a-spending-quota-and-get-alerts-as-i-approach-my-limit"></a>Harcama kotası ayarlayabilir ve belirlediğim limite yaklaştığımda uyarı gönderilmesini sağlayabilir miyim?

Bölüm düzeyinde harcama kotası ayarlayabilirsiniz. Harcama limitleriniz tanımladığınız kotanın %50, %75, %90 ve %100'üne ulaştığında sistem otomatik bildirim gönderir.

Harcama kotanızı tanımlamak için bir departman seçin ve ardından düzenle simgesini seçin. Harcama limiti ayrıntılarını düzenledikten sonra **Kaydet**'i seçin.

### <a name="i-used-resource-groups-to-implement-rbac-and-track-usage-how-can-i-view-the-associated-usage-details"></a>RBAC uygulamak ve kullanımı izlemek için kaynak gruplarını kullandım. İlgili kullanım ayrıntılarını nasıl görüntüleyebilirim?

_Kaynak grupları_ ve _etiketler_ kullandıysanız bu bilgiler hizmet düzeyinde izlenir ve erişmek için ayrıntılı kullanım dosyasını (CSV) indirebilirsiniz. Azure Enterprise Portal'da [kullanım raporunu indirme](https://ea.azure.com/report/downloadusage) sayfasını inceleyin.

Ayrıca API aracılığıyla da erişebilirsiniz. Ayrıntılı bilgiler ve kod örneği için Azure Enterprise Portal'da [Raporlama API'si](https://ea.azure.com/helpdocs/reportingAPI) belgelerini inceleyin.

> [!NOTE]
> Etiketleri yalnızca Azure Resource Manager işlemlerini destekleyen kaynaklara uygulayabilirsiniz. Klasik dağıtım modeli (klasik portal gibi) ile oluşturduğunuz sanal makine, sanal ağ veya depolama alanı gibi kaynaklara etiket uygulayamazsınız. Etiketleme desteği eklemek için bu kaynakları Resource Manager üzerinden yeniden dağıtmanız gerekir. Diğer tüm kaynaklar etiketlemeyi destekler.

### <a name="can-i-perform-analyses-using-power-bi"></a>Power BI kullanarak analiz yapabilir miyim?

Evet. Power BI için Microsoft Azure Kurumsal içerik paketi sayesinde şunları yapabilirsiniz:

- Kurumsal kaydınız için Azure tüketimini hızlı bir şekilde içeri aktarma ve analiz etme.
- En yüksek kullanımı gerçekleştiren departmanı, hesabı veya aboneliği bulma.
- Kuruluşunuzun en çok kullandığı hizmeti öğrenme.
- Harcama ve kullanım eğilimlerini izleme.

Power BI'ı kullanmak için:

1. Power BI web sitesine gidin.
1. Geçerli bir iş veya okul hesabıyla oturum açın.

   Bu iş veya okul hesabı, Azure Enterprise Portal üzerinden kayda erişmek için kullanılan hesapla aynı veya ondan farklı olabilir.
1. Hizmet panosunda Microsoft Azure Kurumsal'ı seçin ve ardından **Bağlan**'ı seçin.
1. **Azure Enterprise'a Bağlan** ekranında şunu girin:
    - Azure Ortamı URL'si: [https://ea.azure.com](https://ea.azure.com)
    - Ay Sayısı: 1 ile 36 arasında bir değer
    - Kayıt Numarası: kayıt numaranızı girin
1. **İleri**’yi seçin.
1. **Hesap Anahtarı** kutusuna API Anahtarını girin.

   API anahtarını Azure Enterprise Portal'da bulabilirsiniz. **Kullanımı İndir** sekmesine bakın ve **API Erişim Anahtarı**'nı seçin. Anahtarı kopyalayıp Power BI'daki **Hesap Anahtarı** kutusuna yapıştırın.

Veri kümesinin boyutuna bağlı olarak verilerin Power BI'a yüklenmesi 5-30 dakika arasında sürebilir.

Power BI raporlarını Azure EA doğrudan, iş ortağı ve fatura bilgilerini görüntüleme yetkisine sahip olan dolaylı müşteriler kullanabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Enterprise Portal yöneticilerinin yaygın yönetim görevleri hakkında bilgi edinmek için [Azure Enterprise Portal yönetimi](ea-portal-administration.md) makalesini okumaları gerekir.
- Azure Enterprise Portal sorunlarını gidermek için yardıma ihtiyacınız varsa [Azure Enterprise Portal erişim sorunlarını giderme](ea-portal-troubleshoot.md) konusuna bakın.
- Azure EA katılım kılavuzu için bkz. [Azure EA Katılım Kılavuzu (PDF)](https://ea.azure.com/api/v3Help/v2AzureEAOnboardingGuide).
