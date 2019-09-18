---
title: Azure EA portalı ile çalışmaya başlama
description: Bu makalede, Azure EA müşterilerinin Azure EA portalını nasıl kullanacağı açıklanmaktadır.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/09/2019
ms.topic: conceptual
ms.service: billing
manager: boalcsva
ms.openlocfilehash: 1882b283f376a1bb8706132263c83e1a24ec0705
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70900939"
---
# <a name="get-started-with-the-azure-ea-portal"></a>Azure EA portalı ile çalışmaya başlama

Bu makale, doğrudan ve dolaylı Azure EA müşterilerinin [Azure EA portalını](https://ea.azure.com) kullanmaya başlamasına yardımcı olur:

- Azure EA portalının yapılandırılması.
- Azure EA portalında kullanılan roller.
- Abonelikler oluşturmaya başlama.
- Azure EA portalında ve Azure portalında maliyetleri çözümleme.

Tam bir Azure EA portalı ekleme oturumu gösteren bir video aşağıda verilmiştir:

[Azure EA portalı ekleme videosu ](https://www.youtube.com/watch?v=OiZ1GdBpo-I)

>[!VIDEO https://www.youtube.com/embed/OiZ1GdBpo-I]

## <a name="azure-ea-portal-hierarchy"></a>Azure EA portalı hiyerarşisi

Azure EA portalı hiyerarşisi şunlardan oluşur:

**Microsoft Azure EA portalı** - Azure EA portalı, Azure EA hizmetleriniz için maliyetleri yönetmenize yardımcı olan bir çevrimiçi yönetim portalıdır. Departmanlar, hesaplar ve abonelikler içeren bir Azure EA hiyerarşisi oluşturmak için kullanılır. Ayrıca, tüketilen hizmetlerin maliyetlerini mutabık kılmak, kullanım raporlarını indirmek ve fiyat listelerini görüntülemek için de kullanabilirsiniz. Ayrıca, kaydınız için API anahtarları oluşturabilirsiniz.

**Departmanlar** - Maliyetleri mantıksal gruplar halinde segmentlere ayırmak ve sonra departman düzeyinde bir bütçe veya kota ayarlamak için departmanlar oluşturun.

**Hesaplar** – Hesaplar, Azure EA portalındaki kuruluş birimleridir ve abonelikleri yönetmek için kullanılır. Hesaplar raporlama için de kullanılır.

**Abonelikler** – Abonelikler, Azure EA portalındaki en küçük birimdir. Hizmet yöneticisi tarafından yönetilen Azure hizmetlerine yönelik kapsayıcılardır.

Aşağıdaki diyagramda basit Azure EA hiyerarşileri gösterilmektedir.

![Basit Azure EA hiyerarşileri diyagramı](./media/billing-ea-portal-get-started/ea-hierarchies.png)

## <a name="enterprise-user-roles"></a>Kurumsal kullanıcı rolleri

Kaydınızdaki Azure hizmetlerini yönetmek için dört farklı kurumsal yönetici kullanıcı rolü vardır:

- Kuruluş yöneticisi
- Departman yöneticisi
- Hesap sahibi
- Hizmet yöneticisi

Roller iki farklı Microsoft Azure portalındaki görevleri gerçekleştirmek için kullanılır. Azure EA portalı (https://ea.azure.com) fatura ve maliyetleri yönetmenize yardımcı olmak için kullanılır. Azure portalı (https://portal.azure.com), Azure hizmetlerini yönetmek için kullanılır.

Kullanıcı rolleri bir kullanıcı hesabıyla ilişkilendirilir. Kullanıcının orijinalliğini doğrulamak için her kullanıcının geçerli bir İş, Okul veya Microsoft Hesabının olması gerekir. Her hesabın etkin bir şekilde izlenen bir e-posta adresiyle ilişkilendirildiğinden emin olun. Hesap bildirimleri e-posta adresine gönderilir.

Kullanıcıları ayarlarken, Kuruluş Yöneticisi rolüne birden fazla İş, Okul veya Microsoft hesabı atayabilirsiniz. Ancak, Hesap Sahibi rolüne yalnızca bir İş, Okul veya Microsoft hesabı atayabilirsiniz. Ayrıca, tek bir İş, Okul veya Microsoft hesabına hem Kuruluş Yöneticisi hem de Hesap Sahibi rolleri uygulanabilir.

### <a name="enterprise-administrator"></a>Kuruluş yöneticisi

Kuruluş yöneticisi rolü en yüksek erişim düzeyine sahiptir. Bu role sahip kullanıcılar şunları yapabilir:

- Hesapları ve hesap sahiplerini yönetme
- Diğer kuruluş yöneticilerini yönetme
- Departman yöneticilerini yönetme
- Bildirim kişilerini yönetme
- Tüm hesaplarda kullanımı görüntüleme
- Tüm hesaplarda faturalandırılmamış ücretleri görüntüleme

Bir kuruluş kaydında birden çok kuruluş yöneticiniz olabilir. Kurumsal yöneticilere salt okuma erişimi verebilirsiniz. Hepsi departman yöneticisi rolünü devralırlar.

### <a name="department-administrator"></a>Departman yöneticisi

Bu role sahip kullanıcılar şunları yapabilir:

- Departmanlar oluşturma ve yönetme
- Yeni hesap sahipleri oluşturma
- Yönettikleri departmanların kullanım ayrıntılarını görüntüleme
- Gerekli izinler verildiyse maliyetleri görüntüleme

Her kuruluş kaydı için birden çok departman yöneticiniz olabilir.

Departman yöneticilerine salt okuma erişimi verebilirsiniz. Salt okuma erişimi vermek için departman yöneticisini düzenleyin veya yeni bir tane oluşturun ve salt okunur seçeneğini **Evet** olarak ayarlayın.

### <a name="account-owner"></a>Hesap sahibi

Bu role sahip kullanıcılar şunları yapabilir:

- Abonelik oluşturma ve yönetme
- Hizmet yöneticilerini yönetme
- Aboneliklerin kullanımını görüntüleme

Her hesap için benzersiz bir İş, Okul veya Microsoft Hesabı gerekir. Azure EA Portalı yönetici rolleri hakkında daha fazla bilgi için bkz. [Azure’daki Azure Kurumsal Anlaşma yönetici rollerini anlama](billing-understand-ea-roles.md).

### <a name="service-administrator"></a>Hizmet yöneticisi

Hizmet yöneticisinin Azure portalındaki hizmetleri yönetme ve kullanıcıları eş yönetici rolüne atama izinleri vardır.

## <a name="activate-your-enrollment"></a>Kaydınızı etkinleştirme

Hizmetinizi etkinleştirmek için başlangıçtaki kuruluş yöneticisi [https://ea.azure.com](https://ea.azure.com) adresindeki Azure EA portalını açar ve davet e-postasındaki e-posta adresini kullanarak oturum açar.

Birden fazla kaydınız varsa etkinleştirilecek bir kayıt seçin. Varsayılan olarak, yalnızca etkin kayıtlar gösterilir. Kayıt geçmişini görüntülemek için, Azure EA portalının sağ üst köşesindeki **Etkin** seçeneğinin işaretini kaldırın.

Kayıt altında, durum **Etkin** olarak gösterilir.

![Etkin kaydı gösteren örnek](./media/billing-ea-portal-get-started/ea-enrollment-status.png)

Yalnızca mevcut Azure kuruluş yöneticileri, diğer kuruluş yöneticilerini oluşturabilir.

### <a name="create-another-enterprise-admin"></a>Başka bir kuruluş yöneticisi oluşturma

- [Azure EA portalında](https://ea.azure.com) oturum açıp **Yönet** > **Kayıt Ayrıntısı**’na gidin ve sayfanın sağ üst köşesindeki **+ Yönetici Ekle**’ye tıklayın.

Kullanıcının e-posta adreslerine ve İş veya Okul kimlik doğrulaması ya da Microsoft hesabı gibi tercih edilen kimlik doğrulama yöntemine sahip olduğunuzdan emin olun. Kullanıcı eklemek için bu bilgiler gereklidir.

EA yöneticisi değilseniz, sizi bir kayda eklemesini istemek için bir EA yöneticisiyle iletişime geçin. Bir kayda eklendikten sonra etkinleştirme e-postası alırsınız.

EA yöneticiniz size yardımcı olamıyorsa bir [Azure EA portalı destek isteği](https://support.microsoft.com/supportrequestform/cf791efa-485b-95a3-6fad-3daf9cd4027c) oluşturun. Şu bilgileri belirtin:

- Kayıt numarası
- Eklenecek e-posta adresi ve kimlik doğrulama türü (İş, Okul veya Microsoft hesabı)
- Mevcut bir EA yöneticisinden e-posta onayı
  - Mevcut EA yöneticisine ulaşılamıyorsa, VLSC aracı üzerinden iletişim ayrıntılarını değiştirmesini istemek için iş ortağınız veya yazılım danışmanınızla iletişime geçin.

Kuruluş yöneticisi rolleri hakkında daha fazla bilgi için bkz. [Azure’daki Azure Kurumsal Anlaşma yönetici rollerini anlama](billing-understand-ea-roles.md).

## <a name="create-an-azure-ea-department"></a>Azure EA departmanı oluşturma

Kuruluş yöneticileri ve departman yöneticileri, kurumsal Azure hizmetlerini ve kullanımı departman ve maliyet merkezine göre düzenleyip raporlamak için departmanları kullanır. Kuruluş yöneticisi şunları yapabilir:

- Departman ekleme veya kaldırma
- Bir hesabı bir departmanla ilişkilendirme
- Departman yöneticileri oluşturma
- Departman yöneticilerinin fiyat ve maliyetleri görüntülemesine izin verme

Departman yöneticisi, departmanına yeni hesaplar ekleyebilir. Departmanlarından hesapları kaldırabilir, ancak kayıttan kaldıramaz.

Bir departman eklemek için:

1. Sol gezinti alanında **Yönet**'e tıklayın.
2. **Departman** sekmesine, ardından **+ Departman Ekle**’ye tıklayın ve sonra gerekli bilgileri girin.
3. Tek gerekli alan Departman adı’dır. En az 3 karakter uzunluğunda olmalıdır.
4. İşlem tamamlandığında **Ekle**’ye tıklayın.

## <a name="add-a-department-admin"></a>Departman yöneticisi ekleme

Bir departman oluşturulduktan sonra, Azure kuruluş yöneticisi departman yöneticileri ekleyebilir ve her birini bir departmanla ilişkilendirebilir. Departman yöneticisi şunları yapabilir:

- Diğer departman yöneticilerini oluşturma
- Ad veya maliyet merkezi gibi departman özelliklerini görüntüleme ve düzenleme
- Departmanları için hesap ekleme
- Departmanlarından hesap kaldırma
- Departmanlarının kullanım ayrıntılarını indirme
- Kuruluş yöneticisi tarafından izin verilmişse departmanlarına ilişkin aylık kullanım ve ücretleri görüntüleme <sup>1</sup>

### <a name="to-add-a-department-admin"></a>Departman yöneticisi eklemek için

Kuruluş yöneticisi olarak:

1. Sol gezinti alanında **Yönet**'e tıklayın.
2. **Departman** sekmesine ve ardından departmana tıklayın.
3. **+ Yönetici Ekle**’ye tıklayıp gerekli bilgileri ekleyin.
4. Salt okuma erişimi için **Salt Okuma** seçeneğini **Evet** yapıp **Ekle**’ye tıklayın.

![Departman Yöneticisi Ekle iletişim kutusunu gösteren örnek](./media/billing-ea-portal-get-started/ea-create-add-department-admin.png)

### <a name="to-set-read-only-access"></a>Salt okuma erişimini ayarlamak için

Departman yöneticilerine salt okuma erişimi verebilirsiniz. Yeni bir departman yöneticisi oluştururken:

- Salt okuma seçeneğini **Evet** olarak ayarlayın.

Mevcut bir departman yöneticisini düzenlemek için:

1. Bir departman seçin ve ardından düzenlemek istediğiniz **Departman Yöneticisinin** yanındaki kurşun kalem simgesine tıklayın.
2. Salt okuma seçeneğini **Evet** olarak ayarlayın. ve sonra **Kaydet**'e tıklayın.

Kuruluş yöneticisi rolüne sahip kullanıcılar departman yöneticisi izinlerini otomatik olarak alır.

<sup>1</sup> Departmanın aylık kullanım ve ücretlerini görüntülemenize izin verilmesine rağmen görüntüleyemiyorsanız iş ortağınızla iletişime geçin.

## <a name="add-an-account"></a>Hesap ekleme

Hesap ve aboneliğin yapısı, bunların nasıl yönetildiğini ve faturalarınızda ve raporlarınızda nasıl göründüğünü etkiler. İşletme bölümlerine, görev ekiplerine ve coğrafi konumlara göre yapılandırma, tipik organizasyon örnekleridir.

Hesap eklemek için:

1. Azure EA portalında, sol gezinti alanındaki **Yönet**'e tıklayın.
2. **Hesap** sekmesine ve sonra **Hesap** sayfasındaki **+Hesap Ekle**’ye tıklayın.
3. Bir departman seçin veya atanmamış olarak bırakın ve sonra istediğiniz kimlik doğrulaması türünü seçin.
4. Raporlama sırasında hesabı tanımlamak için kullanılacak kolay bir ad yazın.
5. Yeni hesapla ilişkilendirilecek **Hesap Sahibi E-posta** adresini yazın.
6. E-posta adresini onaylayın ve **Ekle**'ye tıklayın.

![Hesapların listesini ve +Hesap Ekle seçeneğini gösteren örnek](./media/billing-ea-portal-get-started/create-ea-add-an-account.png)

**Başka Bir Hesap Ekle**’ye tıklayarak başka bir hesap ekleyebilir ya da soldaki araç çubuğunun sağ alt köşesindeki **Ekle**’ye tıklayabilirsiniz.

Hesap sahipliğini onaylamak için:

1. Azure EA portalında oturum açın.
2. Durumu görüntüleyerek hesap sahipliğini onaylayın. **Beklemede** olan durum **Başlangıç/Bitiş tarihi** olarak değişmelidir. Başlangıç/Bitiş tarihi, kullanıcının ilk kez oturum açtığı tarih ve anlaşma bitiş tarihidir.


## <a name="change-account-owner"></a>Hesap sahibini değiştirme

Kuruluş yöneticileri bir kayıttaki abonelik hesap sahipliğini aktarmak için Azure EA portalını kullanabilir. Bu eylem, bir kaynak kullanıcı hesabındaki tüm abonelikleri hedef kullanıcı hesabına taşır.

Kullanıcı hesabı bilgilerini aktarma hakkında önemli noktalar:

- Bir İş veya Okul hesabından başka bir İş veya Okul hesabına yapılan aktarımlar desteklenir.
- Bir Microsoft hesabından İş veya Okul hesabına yapılan aktarımlar desteklenir.
- Bir İş veya Okul hesabından Microsoft hesabına yapılan aktarımlar desteklenmez.
- Microsoft hesabından Microsoft hesabına yapılan aktarımlar desteklenir. Hedef hesabın aktarımlara yönelik geçerli bir hedef olması için geçerli bir Azure Ticari hesabı olması gerekir. Yeni hesaplar için, Azure EA portalında oturum açarken bir Azure Ticari hesabı oluşturmanız istenir. Mevcut hesaplar için, hesabın uygun olabilmesi için yeni bir Azure aboneliği oluşturmanız gerekir.
- Abonelik aktarımını tamamladığınızda, Microsoft hesap sahibini güncelleştirir.

RBAC İlkeleri:

- Mevcut Azure rol tabanlı erişim denetimi (RBAC) ilkelerini, hizmet yöneticisi rol atamalarını ve eş yönetici rol atamalarını yalnızca aynı kiracıdaki iki kuruluş kimliği arasında yapılan Azure aboneliği aktarımları korur. Diğer abonelik aktarımları, RBAC ilkelerinizin ve hizmet yöneticisi ile eş yönetici rol atamalarınızın kaybedilmesiyle sonuçlanır. İlkeler ve yönetici rolleri farklı dizinler arasında aktarılmaz. Hizmet yöneticileri hedef hesabın sahibiyle güncelleştirilir.
- Aynı kiracıdaki iki kuruluş kimliği arasında abonelik aktarımları gerçekleştirdiğinizde, RBAC ilkeleri ve mevcut hizmet yöneticisi ile eş yönetici rolleri korunur.

Hesap sahibini değiştirmeden önce:

1. **Hesap** sekmesini görüntüleyin ve kaynak hesabı belirleyin. Kaynak hesap etkin olmalıdır.
2. Hedef hesabı belirleyin. Bu hesap etkin olmalıdır.

Tüm aboneliklerin hesap sahipliğini aktarmak için:

1. Sol gezinti alanında **Yönet**'e tıklayın.
2. **Hesap** sekmesine tıklayın ve bir hesabın üzerine gelin.
3. Sağ taraftaki hesap sahibini değiştir simgesine tıklayın. Simge bir kişi gibi görünür.
4. Uygun bir hesap seçin ve ardından **İleri**'ye tıklayın.
5. Aktarımı onaylayın ve **Gönder**'e tıklayın.

![Hesap Sahibini Değiştir simgesini gösteren görüntü](./media/billing-ea-portal-get-started/create-ea-create-sub-transfer-account-ownership-of-sub.png)

Tek bir aboneliğin hesap sahipliğini aktarmak için:

1. Sol gezinti alanında **Yönet**'e tıklayın.
2. **Hesap** sekmesine tıklayın ve bir hesabın üzerine gelin.
3. Sağdaki abonelikleri aktar simgesine tıklayın. Simge bir sayfa gibi görünür.
4. Uygun bir abonelik seçin ve ardından **İleri**'ye tıklayın.
5. Aktarımı onaylayın ve sonra **Gönder**'e tıklayın.

![Abonelikleri Aktar simgesini gösteren görüntü](./media/billing-ea-portal-get-started/ea-transfer-subscriptions.png)

Azure EA portalı kullanıcı yönetimini gösteren bir video aşağıda verilmiştir:

[Azure EA portalı kullanıcı yönetimi videosu](https://www.youtube.com/watch?v=621jVkvmwm8)

>[!VIDEO https://www.youtube.com/embed/621jVkvmwm8]

## <a name="create-a-subscription"></a>Abonelik oluşturma

Hesap sahipleri, abonelikleri görüntüleyebilir ve yönetebilir. Kuruluşunuzdaki takımlara geliştirme ortamları ve projelere yönelik erişim izni vermek için abonelikleri kullanabilirsiniz. Örneğin, test, üretim, geliştirme ve hazırlama. Her uygulama ortamı için farklı abonelikler oluşturduğunuzda her bir ortamın güvenliğini sağlamaya yardımcı olursunuz. Ayrıca, her abonelik için farklı bir hizmet yöneticisi hesabı da atayabilirsiniz. Abonelikleri dilediğiniz sayıda hizmet ile ilişkilendirebilirsiniz. Hesap sahibi, abonelikler oluşturur ve hesaplarındaki her aboneliğe bir hizmet yöneticisi hesabı atar.

### <a name="add-a-subscription"></a>Abonelik ekleme

Bir abonelik eklemek için aşağıdaki bilgileri kullanın.

Hesabınıza ilk kez bir abonelik eklediğinizde, MOSA sözleşmesini ve bir ücret planını kabul etmeniz istenir. Kurumsal Anlaşma müşterileri için geçerli olmasa da, aboneliğinizin oluşturulması için gerekir. Microsoft Azure Kurumsal Anlaşma Kaydı Değişikliği, yukarıdaki öğelerin yerini alır ve sözleşme ilişkiniz değişmez. Sorulduğunda, koşulları kabul ettiğinizi belirten kutuyu seçin.

Tüm yeni abonelikler _Microsoft Azure Kurumsal_ varsayılan abonelik adı ile oluşturulur. Abonelik adını, kaydınızdaki diğer aboneliklerden ayırt etmek için güncelleştirebilirsiniz. Ayrıca, kurumsal düzeydeki raporlarda ayırt edilebildiğinden emin olun.

Abonelik eklemek için:

1. Azure EA portalında hesabınızla oturum açın.
2. **Yönetici** sekmesine ve sonra sayfanın üst kısmındaki **Abonelik**’e tıklayın.
2. Hesabın hesap sahibi olarak oturum açtığınızı doğrulayın.
3. **+Abonelik Ekle** ve ardından **Satın Al**’a tıklayın.
  Bir hesaba ilk kez abonelik eklediğinizde iletişim bilgilerinizi sağlamanız gerekir. Başka abonelikler eklerken, iletişim bilgileriniz sizin için eklenir.
4. **Abonelikler**’e tıklayın, ardından oluşturduğunuz aboneliği seçin ve sonra **Abonelik Ayrıntılarını Düzenle**’ye tıklayın.
5. **Abonelik Adı** ve **Hizmet Yöneticisi**’ni güncelleştirip onay işaretini seçin.
  Abonelik adı raporlarda görünür ve geliştirme portalındaki abonelikle ilişkili projenin adıdır.

Yeni aboneliklerin abonelikler listesinde görünmesi 24 saate kadar sürebilir. Bir abonelik oluşturduktan sonra şunları yapabilirsiniz:

- [Abonelik ayrıntılarını düzenleme](https://account.azure.com/Subscriptions)
- [Abonelik hizmetlerini yönetme](https://portal.azure.com/#home)

## <a name="transfer-pay-as-you-go-subscription-to-ea-subscription"></a>Kullandıkça öde aboneliğini EA aboneliğine aktarma

Kullandıkça öde fiyatlarına sahip bir aboneliği EA aboneliğine aktarmak için Azure portalında yeni bir destek isteği oluşturmanız gerekir. Destek isteği oluşturmak için Yardım ve Destek alanındaki **+ Yeni destek isteği**'ne tıklayın.


## <a name="view-usage-summary-and-download-reports"></a>Kullanım özetini görüntüleme ve raporları indirme

Kuruluş yöneticileri Azure EA portalında kullanım verilerinin özetini, tüketilen parasal taahhüdü ve ek kullanımla ilişkili ücretleri görüntüleyebilir. Ücretler, tüm hesaplar ve abonelikler genelinde özet düzeyinde gösterilir.

Belirli hesapların ayrıntılı kullanımını görüntülemek için

Kullanım Ayrıntısı raporunu indirin. **Raporlar** ve ardından **Kullanımı İndir** sekmesine tıklayın. Rapor listesinde, almak istediğiniz aylık rapor için **İndir**'e tıklayın.

Rapor hiçbir geçerli vergiyi içermez. Kullanımın tahakkuk etmesiyle rapora yansıtılması arasında sekiz saatlik bir gecikme olabilir.

Kullanım Özeti raporlarını ve grafiklerini görüntülemek için:

1. Azure EA portalındaki sol gezinti alanında **Raporlar**’a tıklayın ve **Kullanım Özeti** sekmesini görüntüleyin.  
  ![](./media/billing-ea-portal-get-started/create-ea-view usage-summary-and-download-reports.png)
2. Bir taahhüt dönemi seçin.
3. Sayfanın sağ üst kısmındaki **A** (Aylık) ve **Ö** (Özel) seçenekleri arasında geçiş yaparak özel başlangıç ve bitiş tarihleri ile **Kullanım Özeti**’ni görüntüleyin.  
  ![](./media/billing-ea-portal-get-started/create-ea-view-usage-summary-and-download-reports-custom-view.png)
4. Ek ayrıntıları görüntülemek için grafikte bir dönem veya ay seçin.
5. Grafik, kullanılan kullanım, fazla servis ücreti, ayrı olarak faturalandırılan ücretler ve market ücretlerinin dökümü ile aylık kullanım üzerinden ayı gösterir.
6. Seçilen ay için grafiğin altından departmanlara, hesaplara ve aboneliklere göre filtreleyin.
7. **Hizmetlere Göre Ücret** ve **Hiyerarşiye Göre Ücret** arasında geçiş yapın.
8. Ayrıntıları görüntülemek için **Azure Hizmeti**, **Ayrı Olarak Faturalandırılan Ücretler** ve **Azure Market** seçenekleri genişletip daraltın.

Kullanımının nasıl görüntüleneceğini gösteren bir videoyu aşağıda bulabilirsiniz:

[Azure EA portalı kullanım videosu](https://www.youtube.com/watch?v=Cv2IZ9QCn9E)

>[!VIDEO https://www.youtube.com/embed/Cv2IZ9QCn9E]

### <a name="download-csv-reports"></a>CSV raporlarını indirme

Kuruluş Yöneticileri, birkaç raporu CSV dosyası olarak indirmek için Aylık Rapor İndirme sayfasını kullanır. Şunları içerir:

- Bakiye ve Ücret
- Kullanım Ayrıntısı
- Market Ücretleri
- Fiyat Listesi

Raporları indirmek için:


1. Azure EA portalında **Raporlar**’a tıklayın.
2. Sayfanın üst kısmındaki **Kullanımı İndir**'e tıklayın.
3. Ay raporunun yanındaki **İndir**'i seçin.

Tahakkuk eden kullanım tarihi ile kullanımın raporlarda gösterilmesi arasında beş güne kadar gecikme olabilir.

Safari ile Excel'e CSV dosyalarını indiren kullanıcılar biçimlendirme hatalarıyla karşılaşabilir. Hataları önlemek için dosyayı bir metin düzenleyicisi kullanarak açın.

![Kullanımı İndir sayfasını gösteren örnek](./media/billing-ea-portal-get-started/create-ea-download-csv-reports.png)

Kullanım bilgilerinin nasıl indirileceği ile ilgili bir video aşağıda verilmiştir:

[Azure EA portalı kullanım videosu](https://www.youtube.com/watch?v=eY797htT1qg)

>[!VIDEO https://www.youtube.com/embed/eY797htT1qg]

## <a name="schedule-an-onboarding-call"></a>Bir ekleme çağrısı zamanlama

Bizzat müşteri ekleme oturumu zamanlamak istiyorsanız, [Azure EA Portalı Desteği](https://support.microsoft.com/supportrequestform/e114582c-4e51-af46-10b1-1f0cc141e133)'nde bir destek isteği oluşturun. **Sorun Kategorisi** olarak **Ekleme**’yi seçin.

## <a name="next-steps"></a>Sonraki adımlar
- Azure EA portalı yöneticilerinin yaygın yönetim görevleri hakkında bilgi edinmek için [Azure EA portalı yönetimi](billing-ea-portal-administration.md) makalesini okumaları gerekir.
- Azure EA portalı sorunlarını gidermek için yardıma ihtiyacınız varsa [Azure EA portalı erişim sorunlarını giderme](billing-ea-portal-troubleshoot.md) konusuna bakın.
