---
title: İş Ortağı Merkezi'nde Ticari Pazar Yeri hesabı nasıl yönetilir?
description: İş Ortağı Merkezi'nde bir Ticari Pazar Yeri hesabını nasıl yönetebilirsiniz öğrenin.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/06/2019
ms.openlocfilehash: 6d3952c38703d8102a45c4117e3c59e3fa464957
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275824"
---
# <a name="how-to-manage-your-commercial-marketplace-account-in-partner-center"></a>İş Ortağı Merkezi'nde Ticari Market hesabınızı yönetme

Bir İş [Ortağı Merkezi hesabı oluşturduktan](./create-account.md)sonra, Hesabınızı ve tekliflerinizi [Ticari Pazar panosunu](https://partner.microsoft.com/dashboard/commercial-marketplace/overview)kullanarak yönetebilirsiniz.

Bu makalede, İş Ortağı Merkezi hesabınızı nasıl yöneteceğiniz ve şunları yapacağımız hakkında bilgi vereceğiz:

- [İş Ortağı Merkezi hesap ayarlarınıza erişin](#access-your-account-settings)
- [Publisher Id, Symantec Kimliği, Satıcı Kimliği, Kullanıcı Kimliği, MPN Kimliği ve Azure AD kiracılarınızı bulun](#account-details)
- [Kişi bilgilerini güncelleştir](#contact-info)
- [Müşteri kullanımını izlemek için Izleme GUID'leri ayarlama](#tracking-guids)
- [Kullanıcıları yönetme](#manage-users)
- [Grupları yönetme](#manage-groups)
- [Azure AD uygulamalarını yönetme](#manage-azure-ad-applications)
- [Kullanıcı rollerini ve izinlerini tanımlama](#define-user-roles-and-permissions)
- [Azure AD kiracılarını (iş hesapları) yönetme](#manage-tenants)
- [İş Ortağı Merkezi Sözleşmelerini Yönetin](#agreements)

## <a name="access-your-account-settings"></a>Hesap ayarlarınıza erişin

Bunu daha önce yapmadıysanız, İş Ortağı Merkezi hesabınızın [hesap ayarlarına](https://partner.microsoft.com/dashboard/account/management) şu şekilde erişmelisiniz:
- Şirketinizin hesap doğrulama durumunu kontrol edin
- Symantec kimliğinizi, Satıcı kimliğinizi, MPN Kimliğinizi, Yayıncı kimliğinizi ve şirket onaylayıcısı ve satıcı iletişim bilgileri dahil iletişim bilgilerinizi onaylayın
- İş Ortağı Merkezi'nde işletme hesabınızı kullanacak herkes için kullanıcı hesapları oluşturma

### <a name="open-developer-settings"></a>Geliştirici ayarlarını açma

Hesap ayarları, İş Ortağı Merkezi'ndeki [Ticari Pazar Panosu'nun](https://partner.microsoft.com/dashboard/commercial-marketplace) sağ üst köşesinde yer alır. Dişli simgesini (gösterge tablosunun sağ üst köşesine yakın) seçin ve ardından **Geliştirici ayarlarını**seçin.

![İş Ortağı Merkezi'nde hesap ayarları menüsü](./media/dashboard-developer-settings.png)

**Hesap içi ayarlarınızda**şunları görüntüleyebilirsiniz:
- **Hesap detayları**: Hesap türü ve Hesap durumu
- **Yayımcı Kimlikleri**: Satıcı Kimliği, Kullanıcı Kimliği, Publisher Id, Azure AD kiracıları, vb.
- **İletişim bilgileri**: Yayıncı ekran adı, satıcı nın kişi adı, e-posta, telefon ve adresi
- **Takip GUIDs**: Herhangi bir izleme GUIDs hesabınızla ilişkilendirmek

### <a name="account-details"></a>Hesap ayrıntıları

Hesap ayrıntıları bölümünde, **Hesap türünüz** (Şirket veya Bireysel) ve hesabınızın **Doğrulama durumu** gibi temel bilgileri görebilirsiniz. Hesap doğrulama işleminiz sırasında, bu ayarlar e-posta doğrulaması, istihdam doğrulaması ve iş doğrulaması dahil olmak üzere gereken her adımı görüntüler. Ayrıca e-postanızı buradan güncelleyebilir ve gerekirse doğrulamayı yeniden gönderebilirsiniz.

### <a name="publisher-ids"></a>Yayımcı T'leri

Publisher Kimlikleri bölümünde, **Symantec Kimliğinizi,** Satıcı **Kimliğinizi,** Kullanıcı Kimliğinizi, **MPN** **Kimliğinizi**ve **Azure AD kiracılarınızı**görebilirsiniz. Bu değerler Microsoft tarafından geliştirici hesabınızı benzersiz olarak tanımlamak üzere atanır ve düzenlenemez.

### <a name="contact-info"></a>İletişim bilgileri

İletişim bilgileri bölümünde, **Publisher ekran adınızı,** **Satıcı iletişim bilgilerinizi** (şirket satıcısının kişi adı, e-posta, telefon numarası ve adresi) ve Şirket **onaylayıcısını** (şirket kararlarını onaylama yetkisine sahip kişinin adı, e-postası ve telefon numarası) görebilirsiniz.

#### <a name="payout-account"></a>Ödeme hesabı

Ödeme hesabı, satışlarınızdan elde edilen gelirin gönderildiği banka hesabıdır. Bu banka hesabı, Ortak Merkezi hesabınızı kaydettiğiniz aynı ülkede olmalıdır.

Ödeme hesabınızı kurmak için Microsoft **Hesabınızı ilişkilendirmeniz**gerekir:
1. İş Ortağı Merkezi'ndeki [Commercial Marketplace genel bakış sayfasına](https://partner.microsoft.com/dashboard/commercial-marketplace/overview) gidin.
2. Profil bölümünde, **Microsoft Hesabınızı Ilişkilendir'i**seçin.
3. İstendiğinde, Microsoft Hesabınızla (MSA) oturum açın. Bu hesap zaten başka bir İş Ortağı Merkezi hesabıyla ilişkilendirilemez.
4. Ödeme hesabınızın kurulumunu tamamlamak için, İş Ortağı Merkezi'nden tamamen oturum açın ve ardından Microsoft Hesabınızla (iş hesabınız yerine) oturum açın.

Microsoft Hesabınız ilişkili olduğuna göre, bir ödeme hesabı eklemek için şunları yapmanız gerekir:
- **Bir ödeme yöntemi seçin:** Banka hesabı veya PayPal
- **Ödeme bilgileri ekleme**: Hesap türünü seçme (kontrol veya tasarruf), hesap sahibinin adını, hesap numarasını ve yönlendirme numarasını, fatura adresini, telefon numarasını veya PayPal e-posta adresini girmeyi içerebilir. *PayPal'ı hesap ödeme yönteminiz olarak kullanma hakkında daha fazla bilgi edinmek ve pazar bölgenizde desteklenip desteklenmediğini öğrenmek için [PayPal bilgilerine](https://docs.microsoft.com/windows/uwp/publish/setting-up-your-payout-account-and-tax-forms#paypal-info)bakın.

> [!IMPORTANT]
> Ödeme hesabınızı değiştirmek, ödemelerinizi en fazla bir ödeme döngüsüne kadar geciktirebilir. Bu gecikme, ödeme hesabını ilk kurarken yaptığımız gibi hesap değişikliğini doğrulamamız gerektiğinden oluşur. Hesabınız doğrulandıktan sonra tutarın tamamı için ödeme almaya devam eedeceksiniz; geçerli ödeme döngüsü için ödenmesi gereken ödemeler bir sonrakine eklenir.  

#### <a name="tax-profile"></a>Vergi profili

Geçerli vergi profil durumunuzu gözden geçirerek doğru **Varlık türünü** ve Vergi **Sertifikası Bilgilerinin** görüntülendiğini onaylar. Gerekli formları güncelleştirmek veya tamamlamak için **Edit'i** seçin.

Vergi durumunuzu belirlemek için ikamet ettiğiniz ülke ve vatandaşlık durumunu belirtmeniz ve ülkenizle/bölgenizle ilgili uygun vergi formlarını doldurmanız gerekir.

Yaşadığınız ülke veya vatandaşlık ne olursa olsun, Microsoft aracılığıyla herhangi bir teklifi satmak için Amerika Birleşik Devletleri vergi formlarını doldurmanız gerekir. Belirli Amerika Birleşik Devletleri ikamet gereksinimlerini karşılayan ortaklar bir IRS W-9 formu doldurmak zorundadır. Amerika Birleşik Devletleri dışındaki diğer ortaklar bir IRS W-8 formu doldurmak zorundadır. Vergi profilinizi doldururken bu formları çevrimiçi olarak doldurabilirsiniz.

Bir Amerika Birleşik Devletleri Bireysel Vergi Mükellefi Kimlik Numarası (veya ITIN) Microsoft'tan ödeme almak veya vergi anlaşması yararları talep etmek için gerekli değildir.

Vergi formlarınızı Ortak Merkezi'nde elektronik ortamda tamamlayabilir ve gönderebilirsiniz; çoğu durumda, herhangi bir form yazdırmanız ve postalamalısınız.

Farklı ülke ve bölgelerin farklı vergi gereksinimleri vardır. Vergi olarak ödemeniz gereken tutar, tekliflerinizi sattığınız ülkelere ve bölgelere bağlıdır. Microsoft, bazı ülkelerde satış ve kullanım vergisini sizin adınıza kullanır. Bu ülkeler, teklifinizi listeleme sürecinde tanımlanır. Diğer ülkelerde, kayıtlı olduğunuz yere bağlı olarak, satışlarınızı doğrudan yerel vergi dairesine havale etmeniz ve vergi kullanmanız gerekebilir. Buna ek olarak, aldığınız satış gelirleri gelir olarak vergiye tabi olabilir. Microsoft satış işlemleriniz için doğru vergi bilgilerini belirlemenize en iyi şekilde yardımcı olabilecek ülkeniz veya bölgeniz için ilgili kuruma başvurmanızı önemle tavsiye ederiz.

##### <a name="withholding-rates"></a>Stopaj oranları
Vergi formlarınızda gönderdiğiniz bilgiler, uygun vergi stopaj oranını belirler. Stopaj oranı yalnızca ABD'de yaptığınız satışlar için geçerlidir; ABD dışı konumlara yapılan satışlar stopaj konusu na tabi değildir. Stopaj oranları farklılık gösterir, ancak ABD dışında kayıt yaptıran çoğu geliştirici için varsayılan oran %30'dur. Ülkeniz Amerika Birleşik Devletleri ile bir gelir vergisi anlaşması kabul ettiyse, bu oranı azaltma seçeneğiniz vardır.

##### <a name="tax-treaty-benefits"></a>Vergi anlaşması faydaları
Amerika Birleşik Devletleri dışında iseniz, vergi anlaşması yararları yararlanmak mümkün olabilir. Bu avantajlar ülkeden ülkeye değişir ve Microsoft'un gİzemettiği vergi miktarını azaltmanıza izin verebilir. W-8BEN formunun Bölüm II'sini tamamlayarak vergi anlaşması avantajlarından yararlanabilirsiniz. Bu avantajların sizin için geçerli olup olmadığını belirlemek için ülkenizdeki veya bölgenizdeki uygun kaynaklarla iletişim kurmanızı öneririz.

[Windows uygulama/oyun geliştiricileri ve Azure Marketi yayıncıları için vergi ayrıntıları hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/windows/uwp/publish/tax-details-for-paid-apps)

#### <a name="payout-hold-status"></a>Ödeme tutma durumu

Varsayılan olarak, Microsoft ödemeleri aylık olarak gönderir. Ancak, ödemelerinizi beklemeye alma seçeneğiniz vardır ve bu da hesabınıza ödeme gönderilmesini önler. Ödemelerinizi beklemeye almayı seçerseniz, kazandığınız tüm gelirleri kaydetmeye ve ayrıntıları **Ödeme özetinize**vermeye devam edeceğiz. Ancak, siz beklemeyi kaldırana kadar hesabınıza herhangi bir ödeme göndermeyiz. 

Ödemelerinizi beklemeye almak için **Hesap ayarlarına**gidin. **Finansal ayrıntılar**altında, Ödeme **tutma durumu** bölümünde, kaydırıcıyı **On'a**kaydırın. Ödeme tutma durumunuzu istediğiniz zaman değiştirebilirsiniz, ancak kararınızın bir sonraki aylık ödemeyi etkileyeceğini unutmayın. Örneğin, April'ın ödemesini tutmak istiyorsanız, ödeme tutma durumunuzu Mart ayı sonundan önce **A'ya** ayarladığınızdan emin olun.

Ödeme tutma durumunuzu **On**olarak ayarladıktan sonra, kaydırıcıyı **Off'a**geri kaydırana kadar tüm ödemeler beklemede olacaktır. Bunu yaptığınızda, bir sonraki aylık ödeme döngüsüne dahil edileceksiniz (geçerli ödeme eşikleri karşılanmışsa). Örneğin, ödemelerinizi beklemeye aldıysanız, ancak Haziran ayında bir ödeme nin oluşturulmasını istiyorsanız, ödeme tutma durumunu Mayıs ayı sonundan önce **Kapatma'ya** yaptığınızdan emin olun.

> [!NOTE]
> **Ödeme tutma durum** seçiminiz, Azure Marketi, AppSource, Microsoft Mağazası, reklamcılık vb. dahil olmak üzere Microsoft İş Ortağı Merkezi aracılığıyla ödenen **tüm** gelir kaynakları için geçerlidir). Her gelir kaynağı için farklı bekleme durumları seçemezsiniz.

### <a name="devices"></a>Cihazlar

Aygıt yönetimi ayarları yalnızca UWP yayımlama için geçerlidir. [Daha fazla bilgi edinin](https://docs.microsoft.com/windows/uwp/publish/manage-account-settings-and-profile#additional-settings-and-info).

### <a name="tracking-guids"></a>GUI'leri Izleme

Genel Olarak Benzersiz Tanımlayıcılar (GUID'ler), Azure kullanımınızı izlemek için kullanılabilecek benzersiz referans numaralarıdır (32 hexadecimal basamaklı). 

İzleme için GUID'ler oluşturmak için bir GUID jeneratörü kullanmanız gerekir. Azure Depolama ekibi, size doğru biçimde bir GUID e-postagönderebilen ve farklı izleme sistemlerinde yeniden kullanılabilen bir [GUID jeneratör formu](https://aka.ms/StoragePartners) oluşturmuştur.

Her ürün için her teklif ve dağıtım kanalı için benzersiz bir GUID oluşturmanızı öneririz. Raporlamanın bölünmesini istemiyorsanız, ürünün birden çok dağıtım kanalı için tek bir GUID kullanmayı tercih edebilirsiniz.

Bir ürünü bir şablon kullanarak dağıtıyorsanız ve hem Azure Marketi'nde hem de GitHub'da kullanılabilirse, 2 farklı GUIDS oluşturabilir ve kaydedebilirsiniz:

*   Azure Marketinde Ürün A
*   Ürün A GitHub üzerinde

Raporlama, iş ortağı değeri (Microsoft İş Ortağı Kimliği) ve GUID'ler tarafından yapılır. GuiD'leri, teklifiniz dahilindeki her plana hizalayarak daha ayrıntılı bir düzeyde de izleyebilirsiniz.

Daha fazla bilgi için [GUIDs SSS ile Azure müşteri kullanımını izleme](https://docs.microsoft.com/azure/marketplace/azure-partner-customer-usage-attribution#faq)bölümüne bakın.

## <a name="create-a-billing-profile"></a>Faturaprofili oluşturma

[Müşteri Katılımı için Bir Dynamics 365](./create-new-customer-engagement-offer.md) & Power Apps veya Dynamics [365 for Operations](./create-new-operations-offer.md) teklifi yayınlıyorsanız, **faturalandırma profilinizi**tamamlamanız gerekir.

Fatura adresi tüzel kişiliğinizden önceden doldurulur ve bu adresi daha sonra güncelleştirebilirsiniz. KDV ve KDV kimlik alanları isteğe bağlıdır.  Ülke adı ve şirket adı düzenlenemez.

## <a name="multi-user-account-management"></a>Çok kullanıcılı hesap yönetimi

İş Ortağı Merkezi, çok kullanıcılı hesap erişimi ve yönetimi için [Azure Active Directory'den](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) (Azure AD) yararlanır. Kuruluşunuzun Azure REKLAMı, kayıt işleminin bir parçası olarak İş Ortağı Hesabınızla otomatik olarak ilişkilendirilir.

## <a name="manage-users"></a>Kullanıcıları yönetme

İş Ortağı Merkezi'nin **Kullanıcılar** bölümü **(Hesap Ayarları**altında), İş Ortağı Merkezi hesabınıza erişimi olan kullanıcıları, grupları ve Azure AD uygulamalarını yönetmek için Azure AD'yi kullanalım. Kullanıcıları yönetmek için [iş hesabınızda](./company-work-accounts.md) (ilişkili Azure AD kiracısı) oturum açmanız gerekir. Farklı bir iş hesabı / kiracı içinde kullanıcıları yönetmek için, oturum ve daha sonra bu iş hesabı / kiracı **üzerinde Yönetici** izinleri ile bir kullanıcı olarak geri oturum açmanız gerekir.

İş hesabınızla oturum unuzu imzaladıktan sonra (Azure AD kiracısı), şunları yapabilirsiniz:
- [Kullanıcı ekleme veya kaldırma](#add-or-remove-users)
- [Kullanıcı parolası değiştirme](#change-a-user-password)
- [Gruplar ekleme veya kaldırma](#add-or-remove-users)
- [Azure AD uygulamaları ekleme veya kaldırma](#add-new-azure-ad-applications)
- [Azure AD uygulaması için anahtarları yönetme](#manage-keys-for-an-azure-ad-application)
- [Kullanıcı rollerini ve izinlerini tanımlama](#define-user-roles-and-permissions)

Tüm İş Ortağı Merkezi kullanıcılarının (gruplar ve Azure AD uygulamaları dahil) İş Ortağı Merkezi hesabınızla ilişkili bir [Azure AD kiracısında](#manage-tenants) etkin bir iş hesabı olması gerektiğini unutmayın.

### <a name="add-or-remove-users"></a>Kullanıcı ekleme veya kaldırma

Hesabınızda, kullanıcıları eklemek veya değiştirmek istediğiniz [işhesabı (Azure AD kiracısı)](./company-work-accounts.md) için [**Yönetici düzeyinde**](#define-user-roles-and-permissions) izinler için sahip olunması gerekir.

#### <a name="add-existing-users"></a>Varolan kullanıcıları ekleme

Şirketinizin iş hesabında zaten var olan Kullanıcı Merkezi hesabınıza kullanıcı eklemek [için (Azure AD kiracı)](./company-work-accounts.md):

1. **Kullanıcılara** gidin **(Hesap ayarları**altında) ve **kullanıcı ekle'yi**seçin.
2. Görünen listeden bir veya daha fazla kullanıcı seçin. Belirli kullanıcıları aramak için arama kutusunu kullanabilirsiniz.
*İş Ortağı Merkezi hesabınıza eklemek için birden fazla kullanıcı seçerseniz, onlara aynı rolü veya özel izinler kümesini atamanız gerekir. Farklı rollere/izinlere sahip birden çok kullanıcı eklemek için, her rol veya özel izin kümesi için bu adımları yineleyin.
3. Kullanıcıları seçmeyi bitirdiğinizde, **seçili ekle'yi**tıklatın.
4. **Roller** bölümünde, seçili kullanıcı(lar) için rol(ler) veya özelleştirilmiş izinleri belirtin.
5. **Kaydet'i**seçin.

#### <a name="create-new-users"></a>Yeni kullanıcılar oluşturun

Yepyeni kullanıcı hesapları oluşturmak için [**Global yönetici**](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) izinlerine sahip bir hesabınız olması gerekir. 

1. **Kullanıcılara** git **(Hesap ayarları**altında), **Kullanıcı Ekle'yi**seçin, ardından yeni **kullanıcı oluştur'u**seçin.
1. Her yeni kullanıcı için bir ad, soyadı ve kullanıcı adı girin. 
1. Yeni kullanıcının kuruluşunuzun dizininde genel bir yönetici hesabı olmasını istiyorsanız, **tüm dizin kaynakları üzerinde tam denetimle bu kullanıcıyı Azure AD'nizde Global yönetici yapın**etiketli kutuyu işaretleyin. Bu, kullanıcıya şirketinizin Azure AD'sindeki tüm yönetim özelliklerine tam erişim sağlar. Hesabınıza uygun rolü/izinleri vermemediğiniz sürece, İş Merkezi'nde olmasa da, kuruluşunuzun iş hesabındaki (Azure AD kiracısı) kullanıcı ekleyebilir ve yönetebilecekler.
1. **Bu kullanıcıyı Global yönetici yapmak**için kutuyu işaretlediyseniz, gerekirse kullanıcının parolasını kurtarması için bir Parola kurtarma **e-postası** sağlamanız gerekir.
1. Grup **üyeliği** bölümünde, yeni kullanıcının ait olmasını istediğiniz grupları seçin.
1. **Roller** bölümünde, kullanıcıiçin rol(ler veya özelleştirilmiş izinleri belirtin.
1. **Kaydet'i**seçin.

İş Ortağı Merkezi'nde yeni bir kullanıcı oluşturmak, oturum açıldığınız iş hesabındaki (Azure AD kiracısı) bu kullanıcı için bir hesap da oluşturur. İş Ortağı Merkezi'nde bir kullanıcının adında değişiklik yapmak, kuruluşunuzun iş hesabında da aynı değişiklikleri yapar (Azure AD kiracı).

#### <a name="invite-new-users-by-email"></a>Yeni kullanıcıları e-posta ile davet edin

Şu anda şirket iş hesabınızın (Azure AD kiracısı) bir parçası olmayan kullanıcıları e-posta yoluyla davet etmek [**için, Global yönetici**](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) izinlerine sahip bir hesabınız olması gerekir.

1. **Kullanıcılara** git **(Hesap ayarları**altında), **Kullanıcı Ekle'yi**seçin, ardından **kullanıcıları e-postayla davet et'i**seçin.
2. Virgülveya yarı sütunlarla ayrılmış bir veya daha fazla e-posta adresi (en fazla on) girin.
3. **Roller** bölümünde, kullanıcıiçin rol(ler veya özelleştirilmiş izinleri belirtin.
4. **Kaydet'i**seçin.

Davet ettiğiniz kullanıcılar, İş Ortağı Merkezi hesabınıza katılmak için bir e-posta daveti alır. İş hesabınızda yeni bir konuk kullanıcı hesabı oluşturulur (Azure AD kiracı). Her kullanıcının hesabınıza erişebilmeleri için davetini kabul etmesi gerekir.

Bir daveti yeniden göndermeniz gerekiyorsa, **Kullanıcılar** sayfasını ziyaret edin, kullanıcılar listesindeki daveti bulun, e-posta adreslerini (veya *bekleyen Davet*yazan metni) seçin. Ardından, sayfanın alt kısmında, **Daveti Yeniden Gönder'i**seçin.

> [!NOTE]
> Kuruluşunuz şirket içi dizin hizmetini Azure REKLAM'ınızla senkronize etmek için [dizin tümleştirmesi](https://go.microsoft.com/fwlink/p/?LinkID=724033) kullanıyorsa, İş Ortağı Merkezi'nde yeni kullanıcılar, gruplar veya Azure AD uygulamaları oluşturamazsınız. İş Ortağı Merkezi'nde görebilmeniz ve eklemeden önce bunları doğrudan şirket içi dizinde oluşturmanız gerekir.

#### <a name="remove-a-user"></a>Kullanıcıyı kaldırma

Bir kullanıcıyı iş hesabınızdan kaldırmak için (Azure AD kiracı), **Kullanıcılar'a** gidin **(Hesap ayarları**altında), sağ sütundaki onay kutusunu kullanarak kaldırmak istediğiniz kullanıcıyı seçin ve ardından kullanılabilir eylemlerden **kaldır'ı** seçin. Seçili kullanıcıyı (lar) kaldırmak istediğinizi onaylamanız için bir açılır pencere görüntülenir.

#### <a name="change-a-user-password"></a>Kullanıcı parolası değiştirme

Kullanıcınızdan birinin parolasını değiştirmesi gerekiyorsa, kullanıcı hesabını oluştururken parola **kurtarma e-postası** verdiyseniz bunu kendileri yapabilir. Aşağıdaki adımları izleyerek kullanıcının parolasını da güncelleştirebilirsiniz. Şirket iş hesabınızda (Azure AD kiracısı) bir kullanıcının parolasını değiştirmek [**için, Global yönetici**](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) izinlerine sahip bir hesapta oturum açmanız gerekir. Bunun, Ortak Merkezi'ne erişmek için kullandıkları parolayla birlikte Azure AD kiracınızdaki kullanıcıparolasını değiştireceğini unutmayın.

1. **Kullanıcılar** sayfasından **(Hesap ayarları**altında), yeniden yapmak istediğiniz kullanıcı hesabının adını seçin.
2. Sayfanın altındaki **parolayı sıfırla** düğmesini seçin.
3. Geçici bir parola da dahil olmak üzere kullanıcının giriş bilgilerini gösteren bir onay sayfası görüntülenir. Bu sayfadan ayrıldıktan sonra geçici parolaya erişemeyeceğinden, bu bilgileri yazdırdığınızdan veya kopyalayıp kullanıcıya sağladığından emin olun.

## <a name="manage-groups"></a>Grupları yönetme

Gruplar, birden çok kullanıcı rollerini ve izinleri hep birlikte denetlemenize olanak sağlar.

#### <a name="add-an-existing-group"></a>Varolan bir grup ekleme

Kuruluşunuzun iş hesabında zaten var olan bir grubu (Azure AD kiracısı) İş Ortağı Merkezi hesabınıza eklemek için:

1. **Kullanıcılar** sayfasından **(Hesap ayarları**altında), **Gruplar Ekle'yi**seçin.
2. Görünen listeden bir veya daha fazla grup seçin. Belirli grupları aramak için arama kutusunu kullanabilirsiniz.
İş Ortağı Merkezi hesabınıza eklemek üzere birden fazla grup seçerseniz, onlara aynı rolü veya özel izinler kümesini atamanız gerekir. Farklı rollere/izinlere sahip birden çok grup eklemek için, her rol veya özel izin kümesi için bu adımları yineleyin.
3. Grupları seçmeyi bitirdiğinizde, **seçili ekle'yi**tıklatın.
4. **Roller** bölümünde, seçili grup(lar) için rol(ler) veya özelleştirilmiş izinleri belirtin. Grubun tüm üyeleri, bireysel hesaplarıyla ilişkili rollerden ve izinlerden bağımsız olarak, gruba uyguladığınız izinlerle İş Ortağı Merkezi hesabınıza erişebilir.
5. **Kaydet'i**seçin.

Varolan bir grup eklediğinizde, bu grubun üyesi olan her kullanıcı, grubun atanan rolüyle ilişkili izinlerle İş Ortağı Merkezi hesabınıza erişebilir.

#### <a name="add-a-new-group"></a>Yeni grup ekleme

İş Ortağı Merkezi hesabınıza yepyeni bir grup eklemek için:

1. **Kullanıcılar** sayfasından **(Hesap ayarları**altında), **Gruplar Ekle'yi**seçin.
2. Sonraki sayfada Yeni **grubu**seçin.
3. Yeni grubun görüntü adını girin.
4. Grup için rol(ler) veya özelleştirilmiş izinleri belirtin. Grubun tüm üyeleri, bireysel hesaplarıyla ilişkili rollerden/izinlerden bağımsız olarak, burada uyguladığınız izinlerle İş Ortağı Merkezi hesabınıza erişebilecektir.
5. Görünen listeden yeni grup için kullanıcı(lar)ı seçin. Belirli kullanıcıları aramak için arama kutusunu kullanabilirsiniz.
6. Kullanıcıları seçmeyi bitirdiğinizde, yeni gruba eklemek için **seçili Ekle'yi** tıklatın.
7. **Kaydet'i**seçin.

Bu yeni grubun kuruluşunuzun iş hesabında (Azure AD kiracısı) da oluşturulacağını unutmayın, sadece İş Ortağı Merkezi hesabınızda değil.

#### <a name="remove-a-group"></a>Grubu kaldırma

Bir grubu iş hesabınızdan kaldırmak için (Azure AD kiracı), **Kullanıcılar'a** gidin **(Hesap ayarları**altında), sağ sütundaki onay kutusunu kullanarak kaldırmak istediğiniz grubu seçin ve ardından kullanılabilir eylemlerden **kaldır'ı** seçin. Seçili grubu(lar) kaldırmak istediğinizi onaylamanız için bir açılır pencere görüntülenir.

## <a name="manage-azure-ad-applications"></a>Azure AD uygulamalarını yönetme

Şirketinizin Azure REKLAM'ının bir parçası olan uygulamaların veya hizmetlerin İş Ortağı Merkezi hesabınıza erişmesine izin verebilirsiniz.

#### <a name="add-existing-azure-ad-applications"></a>Varolan Azure AD uygulamalarını ekleme

Şirketinizin Azure Etkin Dizini'nde zaten var olan uygulamaları eklemek için:

1. **Kullanıcılar** sayfasından **(Hesap ayarları**altında), Azure REKLAM **uygulamaları ekle'yi**seçin.
2. Görünen listeden bir veya daha fazla Azure REKLAM uygulaması seçin. Belirli Azure AD uygulamalarını aramak için arama kutusunu kullanabilirsiniz. İş Ortağı Merkezi hesabınıza eklemek için birden fazla Azure REKLAM uygulaması seçerseniz, onlara aynı rolü veya özel izinler kümesini atamanız gerekir. Farklı rollere/izinlere sahip birden çok Azure REKLAM uygulaması eklemek için, her rol veya özel izin kümesi için bu adımları yineleyin.
3. Azure AD uygulamalarını seçmeyi tamamladığınızda, **seçili ekle'yi**tıklatın.
4. **Roller** bölümünde, seçili Azure AD uygulaması(lar) için rol(ler) veya özelleştirilmiş izinleri belirtin.
5. **Kaydet'i**seçin.

#### <a name="add-new-azure-ad-applications"></a>Yeni Azure AD uygulamaları ekleme

İş Ortağı Merkezi'ne yepyeni bir Azure AD uygulama hesabına erişim izni vermek istiyorsanız, **Kullanıcılar** bölümünde bir tane oluşturabilirsiniz. Bunun, şirket iş hesabınızda (Azure AD kiracısı) yeni bir hesap oluşturacağını unutmayın, sadece İş Ortağı Merkezi hesabınızda değil. İş Ortağı Merkezi kimlik doğrulaması için öncelikle bu Azure REKLAM uygulamasını kullanıyorsanız ve kullanıcıların doğrudan erişmesi gerekmiyorsa, bu değerler dizininizdeki başka bir Azure REKLAM uygulaması tarafından kullanılmadığı sürece **YanıtURL'si** ve **App ID URI**için geçerli bir adres girebilirsiniz.

1. **Kullanıcılar** sayfasından **(Hesap ayarları**altında), Azure REKLAM **uygulamaları ekle'yi**seçin.
2. Sonraki sayfada Yeni **Azure AD uygulamasını**seçin.
3. Yeni Azure AD uygulaması için **YanıtURL'sini** girin. Bu URL, kullanıcıların Azure REKLAM uygulamanızı oturum kurup kullanabileceği (bazen Uygulama URL'si veya Oturum Açma URL'si olarak da bilinir). **Yanıt** URL'si 256 karakterden uzun olamaz ve dizininizde benzersiz olmalıdır.
4. Yeni Azure REKLAM uygulaması için **App ID URI'yi** girin. Bu, Azure AD uygulaması için tek bir oturum açma isteği Azure AD'ye gönderildiğinde sunulan mantıksal bir tanımlayıcıdır. **App ID URI'nin** dizininizdeki her Azure REKLAM uygulaması için benzersiz olması gerektiğini unutmayın. Bu kimlik 256 karakterden uzun olamaz. App ID URI hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/develop/quickstart-modify-supported-accounts#change-the-application-registration-to-support-different-accounts)
5. **Roller** bölümünde, Azure REKLAM uygulamasıiçin rol(ler veya özelleştirilmiş izinleri belirtin.
6. **Kaydet'i**seçin.

Bir Azure AD uygulaması ekledikten veya oluşturduktan sonra, **Kullanıcılar** bölümüne dönebilir ve Kiracı Kimliği, İstemci Kimliği, Yanıt URL'si ve Uygulama Kimliği URI dahil olmak üzere uygulamanın ayarlarını gözden geçirmek için uygulama adını seçebilirsiniz.

#### <a name="remove-an-application"></a>Uygulamayı kaldırma

Bir uygulamayı iş hesabınızdan kaldırmak için (Azure AD kiracı), **Kullanıcılar'a** gidin **(Hesap ayarları**altında), sağ daki onay kutusunu kullanarak kaldırmak istediğiniz uygulamayı seçin ve ardından kullanılabilir eylemlerden **kaldır'ı** seçin. Seçili uygulama(lar)ı kaldırmak istediğinizi onaylamanız için bir açılır pencere görüntülenir.

#### <a name="manage-keys-for-an-azure-ad-application"></a>Azure AD uygulaması için anahtarları yönetme

Azure AD uygulamanız Microsoft Azure AD'de verileri okuyup yazıyorsa, bir anahtar gerekir. Bir Azure REKLAM uygulaması nın anahtarlarını İş Ortağı Merkezi'nde bilgilerini düzenleyerek oluşturabilirsiniz. Artık gerekkolmayan anahtarları da kaldırabilirsiniz.

1. **Kullanıcılar** sayfasından **(Hesap ayarları**altında), Azure AD uygulamasının adını seçin. Anahtarın oluşturulduğu tarih ve süresinin ne zaman dolacağı da dahil olmak üzere Azure AD uygulamasının tüm etkin anahtarlarını görürsünüz. 
2. Artık gerekmeyen bir anahtarı kaldırmak için **Kaldır'ı**seçin.
3. Yeni bir anahtar eklemek için **yeni anahtar ekle'yi**seçin.
4. **İstemci Kimliğini** ve Anahtar **değerlerini**gösteren bir ekran görürsünüz. Bu sayfadan ayrıldıktan sonra bir daha erişemeyeceğinden, bu bilgileri yazdırdığınızdan veya kopyaladığınızdan emin olun.
5. Daha fazla anahtar oluşturmak istiyorsanız, **başka bir anahtar ekle'yi**seçin.

## <a name="define-user-roles-and-permissions"></a>Kullanıcı rollerini ve izinlerini tanımlama

Şirketinizin kullanıcılarına İş Ortağı Merkezi'ndeki Ticari Pazar programı için aşağıdaki roller ve izinler atanabilir:

- **Manager**
  - Vergi ve ödeme ayarları dışında tüm Microsoft hesap özelliklerine erişebilirsiniz
  - Kullanıcıları, rolleri ve iş hesaplarını (kiracı) yönetebilir
- **Geliştirici**
  - Teklifleri yönetebilir ve yayınlayabilir
  - Bazı yayıncı raporlarını görüntüleyebilir

> [!NOTE]
> Commercial Marketplace programı için Global yönetici, İş Katkıda Bulunan, Finansal Katkıda Bulunan ve Pazarlamacı rolleri kullanılmaz. Bu rolleri kullanıcılara atamanın hiçbir etkisi yoktur. Yalnızca Yönetici ve Geliştirici rolleri kullanıcılara izin verir.

Azure Active Directory (AD), Cloud Solution Provider (CSP), Denetim Masası Satıcısı (CPV), Konuk Kullanıcılar veya Microsoft İş Ortağı Ağı (MPN) gibi İş Ortağı Merkezi'nin diğer alanlarındaki rolleri ve [izinleri](https://docs.microsoft.com/partner-center/permissions-overview)yönetme hakkında daha fazla bilgi için bkz.

## <a name="manage-tenants"></a>Kiracıları yönetme

Bu belgeler de "iş hesabınız" olarak da adlandırılan Azure Etkin Dizin (AD) kiracısı, Azure portalında ayarlanan kuruluşunuzun bir temsilidir ve dahili bilgileriniz için Microsoft bulut hizmetlerinin belirli bir örneğini yönetmenize yardımcı olur ve dış kullanıcılar. Kuruluşunuz Azure, Microsoft Intune veya Office 365 gibi bir Microsoft bulut hizmetine abone olduysa, sizin için bir Azure AD kiracısı kuruldu.

İş Ortağı Merkezi ile kullanmak üzere birden çok kiracı ayarlayabilirsiniz. İş Ortağı Merkezi hesabında **Yönetici** rolü ne olursa olsun, Azure AD kiracılarını hesaptan ekleme ve kaldırma seçeneğine sahip olan herhangi bir kullanıcı.  

### <a name="add-an-existing-tenant"></a>Varolan bir kiracı ekleme

Başka bir Azure AD kiracısını İş Ortağı Merkezi hesabınızla ilişkilendirmek için:

1. **Kiracılar** sayfasından **(Hesap ayarları**altında), başka bir Azure **AD kiracısını ilişkilendir'i**seçin.
2. Ilişkilendirmek istediğiniz kiracı için Azure REKLAM kimlik bilgilerinizi girin.
3. Azure AD kiracınızın kuruluş ve etki alanı adını gözden geçirin. İlişkilendirmeyi tamamlamak için **Onayla'yı**seçin.

İlişkilendirme başarılı olursa, İş Ortağı Merkezi'ndeki **Kullanıcılar** bölümüne hesap kullanıcılarını eklemeye ve yönetmeye hazır olursunuz.

### <a name="create-a-new-tenant"></a>Yeni bir kiracı oluşturma

İş Ortağı Merkezi hesabınızla yepyeni bir Azure AD kiracıoluşturmak için:

1. **Kiracılar** sayfasından **(Hesap ayarları**altında), yeni bir Azure **AD kiracısı oluştur'u**seçin.
2. Yeni Azure REKLAM'ınız için dizin bilgilerini girin:
    - **Alan adı**: Azure AD etki alanınız için kullanacağımız benzersiz ad ve ".onmicrosoft.com" adı. Örneğin, "örnek" girdiyseniz, Azure AD etki alanınız "example.onmicrosoft.com" olur.
    - **İletişim e-posta**: Gerekirse hesabınız hakkında sizinle iletişim kurabileceğimiz bir e-posta adresi.
    - **Global yönetici kullanıcı hesabı bilgileri**: Yeni genel yönetici hesabı için kullanmak istediğiniz ad, soyad, kullanıcı adı ve paroladır.
3. Yeni etki alanını ve hesap bilgilerini onaylamak için **Oluştur'u** seçin.
4. [Kullanıcıları eklemeye ve yönetmeye](#manage-users)başlamak için yeni Azure AD global yönetici kullanıcı adı ve parolanızla oturum açın.

İş Ortağı Portalı portalı üzerinden değil, Azure portalınızda yeni kiracılar oluşturma hakkında daha fazla bilgi için Azure [Etkin Dizini'nde yeni bir kiracı oluştur](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)makalesine bakın.

### <a name="remove-a-tenant"></a>Kiracıyı kaldırma

İş Ortağı Merkezi hesabınızdan bir kiracıyı kaldırmak için, adını **Kiracı** sayfasında **(Hesap ayarlarında)** bulun ve ardından **Kaldır'ı**seçin. Kiracıyı kaldırmak istediğinizi onaylamanız istenir. Bunu yaptığınızda, bu kiracıdaki hiçbir kullanıcı İş Ortağı Merkezi hesabında oturum açamaz ve bu kullanıcılar için yapılandırdığınız izinler kaldırılır.

Bir kiracıyı kaldırdığınızda, Ortak Merkezi hesabına o kiracıdan eklenen tüm kullanıcılar artık hesapta oturum açamaz.

> [!TIP]
> Aynı kiracıdaki bir hesabı kullanarak İş Ortağı Merkezi'nde oturum açtıysanız kiracıyı kaldıramazsınız. Kiracıyı kaldırmak için, hesapla ilişkili başka bir kiracı için **Yönetici** olarak Ortak Merkezi'nde oturum açmanız gerekir. Hesapla ilişkili yalnızca bir kiracı varsa, bu kiracı yalnızca hesabı açan Microsoft hesabıyla oturum açtıktan sonra kaldırılabilir.

## <a name="agreements"></a>Sözleşmeler

İş Ortağı Merkezi'nin **Anlaşmalar** bölümü **(Hesap Ayarları**altında) yetki verdiğiniz yayımlama anlaşmalarının listesini görüntülemenizi sağlar. Bu anlaşmalar, kabul edildiği tarih ve sözleşmeyi kabul eden kullanıcının adı da dahil olmak üzere ad ve sürüm numarasına göre listelenir.

Dikkatinizi gerektiren anlaşma güncelleştirmeleri **varsa, gereken eylemler** bu sayfanın üst kısmında görünebilir. Güncelleştirilmiş bir anlaşmayı kabul etmek için önce bağlantılı Sözleşme Sürümünü okuyun, ardından **anlaşmayı kabul et'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Yeni bir SaaS teklifi oluşturma](./create-new-saas-offer.md)
