---
title: Microsoft Iş Ortağı Merkezi 'nde ticari Market hesabını yönetme
description: Microsoft Iş Ortağı Merkezi 'nde ticari Market hesabını yönetmeyi öğrenin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: parthpandyaMSFT
ms.author: parthp
ms.date: 08/27/2020
ms.openlocfilehash: 7438351aa9849e3110952043eff8f7d860b5b7de
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89015841"
---
# <a name="manage-your-commercial-marketplace-account-in-partner-center"></a>Iş Ortağı Merkezi 'nde ticari Market hesabınızı yönetme

[Bir Iş Ortağı Merkezi hesabı](./create-account.md)oluşturduktan sonra, hesabınızı ve tekliflerini yönetmek için [ticari Market panosunu](https://partner.microsoft.com/dashboard/commercial-marketplace/overview) kullanabilirsiniz.

Bu makalede, aşağıdakiler de dahil olmak üzere Iş Ortağı Merkezi hesabınızı nasıl yönetebileceğinizi inceleyeceğiz:

- [Iş ortağı merkezi hesap ayarlarınıza erişin](#access-your-account-settings)
- [Yayımcı KIMLIĞI, Symantec ID, satıcı KIMLIĞI, Kullanıcı KIMLIĞI, MPN KIMLIĞI ve Azure AD Kiracılarınızı bulun](#account-details)
- [İletişim bilgilerini güncelleştir](#contact-info)
- [Müşteri kullanımını izlemek için Izleme GUID 'Leri ayarlama](#tracking-guids)
- [Kullanıcıları yönetme](#manage-users)
- [Grupları yönetme](#manage-groups)
- [Azure AD uygulamalarını yönetme](#manage-azure-ad-applications)
- [Kullanıcı rollerini ve izinlerini tanımlama](#define-user-roles-and-permissions)
- [Azure AD kiracılarını yönetme (iş hesapları)](#manage-tenants)
- [Iş Ortağı Merkezi sözleşmelerini yönetme](#agreements)

## <a name="access-your-account-settings"></a>Hesap ayarlarınıza erişin

Daha önce yapmadıysanız, sizin (veya kuruluşunuzun Yöneticisi) Iş Ortağı Merkezi hesabınızın [Hesap ayarlarına](https://partner.microsoft.com/dashboard/account/management) şu şekilde erişmelidir:
- Şirketinizin hesap doğrulama durumunu denetleme
- Şirket onaylayanı ve satıcı ilgili kişisi dahil olmak üzere Symantec ID, satıcı KIMLIĞI, Microsoft İş Ortağı Ağı (MPN) KIMLIĞI, yayımcı KIMLIĞI ve iletişim bilgilerini onaylayın
- İş hesabınızı ortak merkezi 'nde kullanacak herkes için Kullanıcı hesapları oluşturma

### <a name="open-developer-settings"></a>Geliştirici ayarlarını aç

**Hesap ayarları** , Iş Ortağı Merkezi ' nde [ticari Market panonuzun](https://partner.microsoft.com/dashboard/commercial-marketplace) sağ üst köşesinde bulunur. Dişli simgesini (panonun sağ üst köşesinin yakınında) seçin ve ardından **Geliştirici ayarları**' nı seçin.

![Iş Ortağı Merkezi 'nde hesap ayarları menüsü](./media/dashboard-developer-settings.png)

**Hesap ayarları**içinde şunları görüntüleyebileceksiniz:
- **Hesap ayrıntıları**: hesap türü ve hesap durumu
- **Yayımcı kimlikleri**: satıcı kimliği, Kullanıcı kimliği, Yayımcı kimliği, Azure AD kiracılar vb.
- **İletişim bilgisi**: yayımcı görünen adı, satıcı iletişim adı, e-posta, telefon ve adres
- **Izleme GUID 'leri**: tüm Izleme GUID 'leri hesabınızla ilişkilendirin

### <a name="account-details"></a>Hesap ayrıntıları

Hesap Ayrıntıları bölümünde, hesap **türü** (Şirket veya bireysel) gibi temel bilgileri ve hesabınızın **doğrulama durumunu** görebilirsiniz. Hesap doğrulama işleminiz sırasında, bu ayarlar, e-posta doğrulama, istihdam doğrulaması ve iş doğrulaması dahil olmak üzere gereken her adımı görüntüler. Ayrıca, e-postanızı buradan güncelleştirebilir ve gerekirse doğrulamayı yeniden gönderebilirsiniz.

### <a name="publisher-ids"></a>Yayımcı kimlikleri

Yayımcı kimlikleri bölümünde **Symantec kimliğiniz**, **satıcı KIMLIĞI**, **Kullanıcı kimliği**, **MPN kimliği**ve **Azure AD Kiracılarınızı**görebilirsiniz. Bu değerler, geliştirici hesabınızı benzersiz şekilde tanımlamak için Microsoft tarafından atanır ve düzenlenemez.

### <a name="contact-info"></a>İletişim bilgileri

Kişi bilgileri bölümünde, **yayımcının görünen adını**, **satıcı iletişim bilgilerini** (ilgili kişi adı, e-posta, telefon numarası ve şirket satıcı Için adres) ve **şirket onaylayanı** (şirketin kararlarını onaylama yetkisi olan kişinin adı, e-posta ve telefon numarası) görebilirsiniz.

#### <a name="payout-account"></a>Ödeme hesabı

Bir ödeme hesabı, satışınızdan gönderilecek devam eden banka hesabıdır. Bu banka hesabı, Iş Ortağı Merkezi hesabınızı kaydettiğiniz ülkede/bölgede olmalıdır.

Ödeme hesabınızı ayarlamak için:

1. Iş Ortağı Merkezi ' nde [ticari Market genel bakış sayfasına](https://partner.microsoft.com/dashboard/commercial-marketplace/overview) gidin.
2. Profil bölümünün altında, **ödeme profili**' nin yanında **Güncelleştir**' i seçin.
3. **Bir ödeme yöntemi seçin**: banka hesabı veya PayPal.
4. **Ödeme bilgilerini ekle**: Bu, hesap sahibi adı, hesap numarası ve yönlendirme numarası, fatura adresi, telefon numarası veya PayPal e-posta adresini girerek bir hesap türü (denetleme veya tasarruf) seçmeyi içerebilir. * Hesap ödeme yönteminiz olarak PayPal kullanma hakkında daha fazla bilgi edinmek ve Pazar bölgenizde desteklenip desteklenmediğini öğrenmek için bkz. [PayPal bilgileri](https://docs.microsoft.com/windows/uwp/publish/setting-up-your-payout-account-and-tax-forms#paypal-info).

> [!IMPORTANT]
> Ödeme hesabınızı değiştirmek, ödemenizi tek bir ödeme döngüsüne erteleyebilir. Bu gecikme, hesap değişikliğini doğrulamanız gerektiği gibi, yalnızca ödeme hesabını ayarlarken yaptığımız için oluşur. Hesabınız doğrulandıktan sonra da tam miktar için ücret ödeirsiniz; geçerli ödeme döngüsünün süresi dolan tüm ödemeler bir sonrakine eklenecektir.  

#### <a name="tax-profile"></a>Vergi profili

Doğru **varlık türü** ve **Vergi Sertifikası bilgilerinin** görüntülendiğini onaylayan geçerli vergi profili durumunuzu gözden geçirin. Gerekli formları güncelleştirmek veya gerçekleştirmek için **Düzenle** ' yi seçin.

Vergi durumunuzu oluşturmak için, Kona ve vatandaşlık ülke/bölgenizi belirtmeniz ve ülkeniz/bölgeniz ile ilişkili uygun vergi formlarını doldurmanız gerekir.

İkamet veya vatandaşlık ülkeniz/bölgeinizden bağımsız olarak, Microsoft aracılığıyla herhangi bir teklif satmanız için Birleşik Devletler vergi formlarını doldurmanız gerekir. Belirli Birleşik Devletler yer gereksinimini karşılayan iş ortakları bir ıRS W-9 formunu doldurmalıdır. Birleşik Devletler dışındaki diğer iş ortakları bir ıRS W-8 formu doldurmanız gerekir. Vergi profilinizi tamamladığınızda, bu formları çevrimiçi olarak doldurabilirsiniz.

Birleşik Devletler bireysel vergi mükellefi kimlik numarası (veya ITIN), Microsoft 'tan ödeme almak veya vergi anlaşmalar avantajlarına talep etmek için gerekli değildir.

Iş Ortağı Merkezi ' nde vergi formlarınızı elektronik olarak tamamlayabilir ve gönderebilirsiniz. Çoğu durumda, herhangi bir formu yazdırmanız ve postalarınıza gerek kalmaz.

Farklı ülkeler ve bölgeler farklı vergi gereksinimlerine sahiptir. Vergilerle ödeme yapmanız gereken tam tutar, tekliflerinizi sattığınız ülkelere ve bölgelere göre değişir. Microsoft remits Sales ve bazı ülkelerde/bölgelerde sizin adınıza vergi kullanır. Bu ülkeler/bölgeler, teklifinizi listeleme sürecinde tanımlanacaktır. Diğer ülkelerde/bölgelerde, kaydettiğiniz yere bağlı olarak, satış için satış ve Kullanım vergisini doğrudan yerel talama yetkilisine havale etmeniz gerekebilir. Bunlara ek olarak, aldığınız satış gelirleri gelir olarak vergilendirilebilir olabilir. Ülkeniz veya bölgeniz için, Microsoft satış işlemleriniz için doğru vergi bilgilerini belirlemenize en iyi şekilde yardımcı olabilecek ilgili yetkiliyle iletişim kurmanız önemle önerilir.

##### <a name="withholding-rates"></a>Stopaj oranları

Vergi formlarınızda gönderdiğiniz bilgiler uygun vergi stopajı oranını belirler. Stopaj oranı yalnızca Birleşik Devletler yaptığınız satış için geçerlidir; ABD dışı konumlara yapılan satışlar, stopaja tabi değildir. Stopaj ücretleri farklılık gösterir, ancak Birleşik Devletler dışında kayıt yapan çoğu geliştirici için varsayılan oran %30 ' dur. Ülkeniz/bölgeniz Birleşik Devletler bir gelir vergi anlaşmasının kabul etmiş olması durumunda bu oranı azaltma seçeneğiniz vardır.

##### <a name="tax-treaty-benefits"></a>Vergi anlaşmalar avantajları

Birleşik Devletler dışındaysanız, vergi anlaşmalar avantajlarından yararlanabilirsiniz. Bu avantajlar ülke/bölge ile ülke/bölge arasında farklılık gösterir ve Microsoft 'un tuttuğu vergi miktarını azaltmanıza olanak tanıyabilir. W-8BEN formunun Bölüm II ' ni tamamlayarak vergi anlaşmalar avantajlarına talep edebilirsiniz. Bu avantajlardan sizin için uygun olup olmadığını öğrenmek üzere ülkenizde veya bölgenizde uygun kaynaklarla iletişim kurmanızı öneririz.

[Windows uygulama/oyun geliştiricileri ve Azure Market yayımcılarının vergi ayrıntıları hakkında daha fazla bilgi edinin](https://docs.microsoft.com/windows/uwp/publish/tax-details-for-paid-apps).

#### <a name="payout-hold-status"></a>Ödeme tutma durumu

Varsayılan olarak, Microsoft ödemeleri aylık olarak gönderir. Bununla birlikte, ücretlerinizi isteğe bağlı olarak bir araya getirebilirsiniz ve bu da, ödemelerin hesabınıza gönderilmesini engeller. Ödelerinizi beklemeye almaya çalışırsanız, aldığınız herhangi bir geliri kaydetmeye devam edeceğiz ve **ödek özetinizdeki**ayrıntıları sağlarsınız. Ancak, tutmayı kaldırana kadar hesabınıza herhangi bir ödeme gönderemeyiz.

Ödemelere beklemeye koymak için **Hesap ayarları**' na gidin. **Finans ayrıntıları**' nın altında, **ödeme bekletme durumu** bölümünde, kaydırıcıyı **Açık**' a değiştirin. Ödeme tutma durumunuzu dilediğiniz zaman değiştirebilirsiniz, ancak kararlarınızın bir sonraki aylık ödeyerek etkileyebileceğini unutmayın. Örneğin, Nisan 'un payından birini tutmak istiyorsanız, Mart 'un sonundan önce, ödeme tutma durumunuzu **Açık** olarak ayarladığınızdan emin olun.

Ödeme bekletme durumunuzu **Açık**olarak ayarladıktan sonra, kaydırıcıyı **devre dışı**olarak açıp tüm ödemeler beklemeye alınır. Bunu yaptığınızda, bir sonraki aylık ödeme döngüsünün (ilgili ödeme eşikleri karşılandığında) dahil edersiniz. Örneğin, ödeticinizi beklemeye aldıysanız, Haziran ayında üretilmiş bir ödeme sahip olmak istiyorsanız, Işlem sonundan önce, ödememiş tutma durumunu **kapalı** durumuna geçirdiğinizden emin olun.

> [!NOTE]
> **Ödeme bekletme durumu** seçiminiz, Microsoft Iş Ortağı Merkezi aracılığıyla ödenen Azure Marketi, appsource, Microsoft Store, reklam, vb. gibi **Tüm** gelir kaynakları için geçerlidir. Her bir gelir kaynağı için farklı saklama durumları seçemezsiniz.

### <a name="devices"></a>Cihazlar

Cihaz yönetimi ayarları yalnızca UWP yayımlaması için geçerlidir. [Daha fazla bilgi edinin](https://docs.microsoft.com/windows/uwp/publish/manage-account-settings-and-profile#additional-settings-and-info).

### <a name="tracking-guids"></a>İzleme GUID 'Leri

Genel benzersiz tanımlayıcılar (GUID 'Ler), Azure kullanımınızı izlemek için kullanılabilen benzersiz başvuru sayılarıdır (32 onaltılık basamakla birlikte). 

İzleme için GUID 'Ler oluşturmak üzere bir GUID Oluşturucu kullanmanız gerekir. Azure depolama ekibi, doğru biçimin bir GUID 'sine e-posta sağlayacak bir [GUID Oluşturucu formu](https://aka.ms/StoragePartners) oluşturdu ve farklı izleme sistemleri arasında yeniden kullanılabilir.

Her ürün için her bir teklif ve dağıtım kanalı için benzersiz bir GUID oluşturmanız önerilir. Raporlama 'nın bölünmesini istemiyorsanız, ürünün birden çok dağıtım kanalı için tek bir GUID kullanmayı tercih edebilirsiniz.

Bir ürünü bir şablon kullanarak dağıtırsanız ve hem Azure Marketi hem de GitHub üzerinde mevcutsa, iki ayrı GUID oluşturup kaydedebilirsiniz:

- Azure Market 'te A ürünü
- GitHub 'da A ürünü

Raporlama, iş ortağı değeri (Microsoft Iş ortağı KIMLIĞI) ve GUID 'Ler tarafından yapılır. Ayrıca, bu GUID 'Leri teklifiniz içindeki her plana göre daha ayrıntılı bir düzeyde izleyebilirsiniz.

Daha fazla bilgi için bkz. [Azure müşteri kullanımını GUID 'Lerle izleme hakkında SSS](../azure-partner-customer-usage-attribution.md#faq)).

## <a name="create-a-billing-profile"></a>Faturalandırma profili oluşturma

Müşteriler için bir [dynamics 365 & Power Apps](./create-new-customer-engagement-offer.md) veya [Operations teklifi için Dynamics 365](./create-new-operations-offer.md) ' i yayımlıyorsanız, **faturalandırma profilinizi**doldurmanız gerekir.

Faturalandırma adresi yasal varlığınızdan önceden doldurulur ve bu adresi daha sonra güncelleştirebilirsiniz. VERGI ve KDV KIMLIĞI alanları isteğe bağlıdır.  Ülke/bölge adı ve şirket adı düzenlenemez.

## <a name="multi-user-account-management"></a>Çok kullanıcılı hesap yönetimi

İş Ortağı Merkezi, çok kullanıcılı hesap erişimi ve yönetimi için [Azure Active Directory](../../active-directory/fundamentals/active-directory-whatis.md) (Azure AD) kullanır. Kuruluşunuzun Azure AD, kayıt işleminin bir parçası olarak Iş Ortağı Merkezi hesabınızla otomatik olarak ilişkilendirilir.

## <a name="manage-users"></a>Kullanıcıları yönetme

Iş Ortağı Merkezi 'nin ( **Hesap ayarları**altında) **Kullanıcılar** bölümü, iş ortağı merkezi hesabınıza erişimi olan kullanıcıları, grupları ve Azure AD UYGULAMALARıNı yönetmek için Azure AD 'yi kullanmanıza olanak sağlar. Hesabınız, Kullanıcı eklemek veya düzenlemek istediğiniz [iş hesabı (Azure AD kiracısı)](./company-work-accounts.md) için [**yönetici düzeyinde**](#define-user-roles-and-permissions) izinlere sahip olmalıdır. Farklı bir iş hesabındaki/Kiracıdaki kullanıcıları yönetmek için, oturumu kapatıp bu iş hesabı/kiracısında **yönetici** izinlerine sahip bir kullanıcı olarak yeniden oturum açmanız gerekir.

İş hesabınızla (Azure AD kiracısı) oturum açtıktan sonra şunları yapabilirsiniz:

- [Kullanıcı ekleme veya kaldırma](#add-existing-users)
- [Kullanıcı parolasını değiştirme](#change-a-user-password)
- [Grup Ekle veya Kaldır](#manage-groups)
- [Azure AD uygulamaları ekleme veya kaldırma](#add-new-azure-ad-applications)
- [Bir Azure AD uygulaması için anahtarları yönetme](#manage-keys-for-an-azure-ad-application)
- [Kullanıcı rollerini ve izinlerini tanımlama](#define-user-roles-and-permissions)

Tüm Iş ortağı merkezi kullanıcılarının (gruplar ve Azure AD uygulamaları dahil), Iş Ortağı Merkezi hesabınızla ilişkili bir [Azure AD kiracısında](#manage-tenants) etkin bir iş hesabına sahip olması gerektiğini aklınızda bulundurun.

### <a name="add-existing-users"></a>Mevcut kullanıcıları Ekle

İş Ortağı Merkezi hesabınıza, şirketinizin [iş hesabında (Azure AD kiracısı)](./company-work-accounts.md)zaten mevcut olan kullanıcıları eklemek için:

1. **Kullanıcılara** ( **Hesap ayarları**altında) gidin ve **Kullanıcı Ekle**' yi seçin.
2. Görüntülenen listeden bir veya daha fazla kullanıcı seçin. Belirli kullanıcıları aramak için arama kutusunu kullanabilirsiniz.
* Iş Ortağı Merkezi hesabınıza eklemek için birden fazla kullanıcı seçerseniz, bunlara aynı rol veya özel izinler kümesi atamanız gerekir. Farklı rollere/izinlere sahip birden fazla kullanıcı eklemek için, bu adımları her bir rol veya özel izinler kümesi için tekrarlayın.
3. Kullanıcıları seçmeyi tamamladığınızda **Seçili Ekle**' yi seçin.
4. Roller bölümünde, seçili kullanıcılar için **rolleri** veya özelleştirilmiş izinleri belirtin.
5. **Kaydet**’i seçin.

### <a name="create-new-users"></a>Yeni Kullanıcı Oluştur

Yeni Kullanıcı hesapları oluşturmak için [**genel yönetici**](../../active-directory/users-groups-roles/directory-assign-admin-roles.md) izinlerine sahip bir hesabınız olması gerekir.

1. **Kullanıcılara** ( **Hesap ayarları**altında) gidin, **Kullanıcı Ekle**' yi seçin ve ardından **Yeni Kullanıcı oluştur**' u seçin.
1. Her yeni kullanıcı için bir ad, son ad ve Kullanıcı adı girin. 
1. Yeni kullanıcının kuruluşunuzun dizininde genel yönetici hesabına sahip olmasını istiyorsanız, **tüm dizin kaynakları üzerinde tam denetim ile bu kullanıcıyı Azure AD 'niz üzerinde genel yönetici yap**etiketli kutuyu işaretleyin. Bu, kullanıcıya şirketinizin Azure AD 'deki tüm yönetim özelliklerine tam erişim sağlayacak. Hesaba uygun rolü/izinleri vermediğiniz müddetçe, iş ortağı merkezi 'nde değil, kuruluşunuzun iş hesabına (Azure AD kiracısı) Kullanıcı ekleyebilir ve bunları yönetebilecektir.
1. **Bu kullanıcıya genel yönetici yapma**kutusunu işaretlenirse, gerekirse parolasını kurtarmak için **parola kurtarma e-postası** sağlamanız gerekir.
1. **Grup üyeliği** bölümünde, yeni kullanıcının ait olmasını istediğiniz grupları seçin.
1. **Roller** bölümünde, Kullanıcı için rol (ler) veya özelleştirilmiş izinleri belirtin.
1. **Kaydet**’i seçin.

İş Ortağı Merkezi 'nde yeni bir kullanıcı oluşturmak, oturum açtığınız iş hesabındaki (Azure AD kiracısı) bu kullanıcı için bir hesap de oluşturur. Iş ortağı merkezindeki bir kullanıcının adında değişiklik yapmak, kuruluşunuzun iş hesabında (Azure AD kiracısı) aynı değişiklikleri yapar.

### <a name="invite-new-users-by-email"></a>Yeni kullanıcıları e-posta ile davet et

Şirket iş hesabınızın (Azure AD kiracısı) bir parçası olmayan kullanıcıları e-posta ile davet etmek için, [**genel yönetici**](../../active-directory/users-groups-roles/directory-assign-admin-roles.md) izinlerine sahip bir hesabınız olmalıdır.

1. **Kullanıcılara** ( **Hesap ayarları**altında) gidin, **Kullanıcı Ekle**' yi seçin ve ardından **kullanıcıları e-postayla davet et**' i seçin
2. Virgülle veya noktalı virgülle ayırarak bir veya daha fazla e-posta adresi (en fazla 10) girin.
3. **Roller** bölümünde, Kullanıcı için rol (ler) veya özelleştirilmiş izinleri belirtin.
4. **Kaydet**’i seçin.

Davet ettiğiniz kullanıcılar, Iş Ortağı Merkezi hesabınıza katılması için bir e-posta daveti alacak. İş hesabınızda (Azure AD kiracısı) yeni bir Konuk Kullanıcı hesabı oluşturulacaktır. Hesabınıza erişebilmek için her kullanıcının davetini kabul etmesi gerekir.

Bir daveti yeniden göndermeniz gerekiyorsa, **Kullanıcılar** sayfasını ziyaret edin, Kullanıcı listesinde daveti bulun, e-posta adresini (veya *daveti bekleyen*metni) seçin. Ardından sayfanın alt kısmındaki **daveti yeniden gönder**' i seçin.

> [!NOTE]
> Kuruluşunuzda şirket içi dizin hizmetini Azure AD ile eşitlemek için [Dizin tümleştirmesi](https://go.microsoft.com/fwlink/p/?LinkID=724033) kullanılıyorsa, Iş Ortağı Merkezi 'nde yeni kullanıcılar, gruplar veya Azure AD uygulamaları oluşturamazsınız. Siz (veya şirket içi dizininizde bulunan başka bir yöneticinin), Iş Ortağı Merkezi 'nde görebilmeniz ve ekleyebilmeniz için doğrudan şirket içi dizinde oluşturulması gerekir.

### <a name="remove-a-user"></a>Kullanıcıyı kaldırma

Bir kullanıcıyı iş hesabınızdan (Azure AD kiracısı) kaldırmak için **kullanıcılara** gidin ( **Hesap ayarları**altında), en sağdaki sütunda onay kutusunu kullanarak kaldırmak istediğiniz kullanıcıyı seçin ve ardından kullanılabilir eylemlerden **Kaldır** ' ı seçin. Seçili kullanıcıları kaldırmak istediğinizi onaylamanız için bir açılır pencere görüntülenir.

### <a name="change-a-user-password"></a>Kullanıcı parolasını değiştirme

Kullanıcılarınızın, parolasını değiştirmesi gerekiyorsa, Kullanıcı hesabı oluştururken **parola kurtarma e-postası** sağlanmışsa, bunları kendileri yapabilir. Ayrıca, aşağıdaki adımları izleyerek bir kullanıcının parolasını güncelleştirebilirsiniz. Şirket iş hesabınızdaki (Azure AD kiracısı) bir kullanıcının parolasını değiştirmek için, [**genel yönetici**](../../active-directory/users-groups-roles/directory-assign-admin-roles.md) izinlerine sahip bir hesapta oturum açmış olmanız gerekir. Bu işlem, kullanıcının Azure AD kiracınızdaki parolasını, Iş Ortağı Merkezi 'ne erişmek için kullandıkları parolayı değiştirecek şekilde değiştirir.

1. **Kullanıcılar** sayfasından ( **Hesap ayarları**altında), düzenlemek istediğiniz kullanıcı hesabının adını seçin.
2. Sayfanın alt kısmındaki **Parolayı Sıfırla** düğmesini seçin.
3. Geçici bir parola dahil olmak üzere, kullanıcının oturum açma bilgilerini göstermek için bir onay sayfası görüntülenir. Bu sayfadan ayrıldıktan sonra geçici parolaya erişemeyeceksiniz, bu bilgileri yazdırdığınızdan veya kopyalamanız ve kullanıcıya sunduğunuzdan emin olun.

## <a name="manage-groups"></a>Grupları yönetme

Gruplar, birden fazla kullanıcı rolünü ve izinlerini birlikte denetlemenize olanak tanır.

### <a name="add-an-existing-group"></a>Var olan bir grubu Ekle

Kuruluşunuzun iş hesabında (Azure AD kiracısı) zaten mevcut olan bir grubu Iş Ortağı Merkezi hesabınıza eklemek için:

1. **Kullanıcılar** sayfasından ( **Hesap ayarları**altında) **Grup Ekle**' yi seçin.
2. Görüntülenen listeden bir veya daha fazla grup seçin. Belirli grupları aramak için arama kutusunu kullanabilirsiniz.
Iş Ortağı Merkezi hesabınıza eklemek için birden fazla grup seçerseniz, bunları aynı rol veya özel izinler kümesi atamanız gerekir. Farklı rollere/izinlere sahip birden çok grup eklemek için, bu adımları her bir rol veya özel izinler kümesi için tekrarlayın.
3. Grupları seçmeyi tamamladığınızda **Seçili Ekle**' yi seçin.
4. Roller bölümünde, Seçili gruplar için **rolleri** veya özelleştirilmiş izinleri belirtin. Grubun tüm üyeleri Iş Ortağı Merkezi hesabınıza, bireysel hesabıyla ilişkili roller ve izinler ne olursa olsun, gruba uyguladığınız izinlerle erişebilecektir.
5. **Kaydet**’i seçin.

Var olan bir grubu eklediğinizde, bu grubun üyesi olan her Kullanıcı, Iş Ortağı Merkezi hesabınıza, grubun atanan rolüyle ilişkili izinlerle erişebilecek.

### <a name="add-a-new-group"></a>Yeni grup ekleme

Iş Ortağı Merkezi hesabınıza yepyeni bir grup eklemek için:

1. **Kullanıcılar** sayfasından ( **Hesap ayarları**altında) **Grup Ekle**' yi seçin.
2. Sonraki sayfada **Yeni Grup**' u seçin.
3. Yeni grup için görünen adı girin.
4. Grup için rol (ler) veya özelleştirilmiş izinleri belirtin. Grubun tüm üyeleri, bireysel hesabıyla ilişkili rollerden/izinlerden bağımsız olarak, burada uyguladığınız izinlerle Iş Ortağı Merkezi Hesabınıza erişebilecektir.
5. Görüntülenen listeden yeni grup için Kullanıcı seçin. Belirli kullanıcıları aramak için arama kutusunu kullanabilirsiniz.
6. Kullanıcıları seçmeyi tamamladığınızda, yeni gruba eklemek için **Seçileni Ekle** ' yi seçin.
7. **Kaydet**’i seçin.

Bu yeni Grup, kuruluşunuzun iş hesabında (Azure AD kiracısı), ancak yalnızca Iş Ortağı Merkezi hesabınızda oluşturulacak.

### <a name="remove-a-group"></a>Bir grubu kaldır

İş hesabınızdan (Azure AD kiracısı) bir grubu kaldırmak için **kullanıcılara** gidin ( **Hesap ayarları**altında), en sağdaki sütunda onay kutusunu kullanarak kaldırmak istediğiniz grubu seçin ve ardından kullanılabilir eylemlerden **Kaldır** ' ı seçin. Seçili grupları kaldırmak istediğinizi onaylamanız için bir açılır pencere görüntülenir.

## <a name="manage-azure-ad-applications"></a>Azure AD uygulamalarını yönetme

Şirketinizin Azure AD 'nin bir parçası olan uygulamalara veya hizmetlere Iş Ortağı Merkezi hesabınıza erişmesine izin verebilirsiniz.

### <a name="add-existing-azure-ad-applications"></a>Mevcut Azure AD uygulamalarını ekleme

Şirketinizin Azure Active Directory zaten var olan uygulamaları eklemek için:

1. **Kullanıcılar** sayfasından ( **Hesap ayarları**altında) **Azure AD uygulamaları Ekle**' yi seçin.
2. Görüntülenen listeden bir veya daha fazla Azure AD uygulaması seçin. Belirli Azure AD uygulamalarını aramak için arama kutusunu kullanabilirsiniz. Iş Ortağı Merkezi hesabınıza eklemek için birden fazla Azure AD uygulaması seçerseniz, bunlara aynı rol veya özel izin kümesi atamanız gerekir. Farklı rollere/izinlere sahip birden çok Azure AD uygulaması eklemek için, bu adımları her bir rol veya özel izin kümesi için tekrarlayın.
3. Azure AD uygulamaları ' nı seçtikten sonra **Seçili Ekle**' yi seçin.
4. **Roller** bölümünde, SEÇILI Azure AD uygulamaları için rol (ler) veya özelleştirilmiş izinleri belirtin.
5. **Kaydet**’i seçin.

### <a name="add-new-azure-ad-applications"></a>Yeni Azure AD uygulamaları ekleme

Yeni bir Azure AD uygulama hesabına Iş ortağı merkezi erişimi vermek istiyorsanız, **Kullanıcılar** bölümünde bir tane oluşturabilirsiniz. Bu işlem, yalnızca Iş Ortağı Merkezi hesabınızda değil, şirketinizin iş hesabınızda (Azure AD kiracısı) yeni bir hesap oluşturur. Birincil olarak bu Azure AD uygulamasını Iş ortağı merkezi kimlik doğrulaması için kullanıyorsanız ve kullanıcılara doğrudan erişim gereksinimi yoksa, bu değerler dizininizde başka bir Azure AD uygulaması tarafından kullanılmıyorsa, **yanıt URL** 'Si ve **uygulama kimliği URI 'si**için geçerli bir adres girebilirsiniz.

1. **Kullanıcılar** sayfasından ( **Hesap ayarları**altında) **Azure AD uygulamaları Ekle**' yi seçin.
2. Sonraki sayfada, **yeni Azure AD uygulaması**' nı seçin.
3. Yeni Azure AD uygulamasının **yanıt URL 'sini** girin. Bu URL, kullanıcıların oturum açmak ve Azure AD uygulamanızı kullanabilmesi (bazen uygulama URL 'SI veya oturum açma URL 'SI olarak da bilinir). **Yanıt URL 'si** 256 karakterden uzun olamaz ve dizininiz içinde benzersiz olmalıdır.
4. Yeni Azure AD uygulaması için **uygulama KIMLIĞI URI** 'sini girin. Bu, Azure AD 'ye çoklu oturum açma isteği gönderildiğinde sunulan Azure AD uygulaması için mantıksal bir tanıtıcıdır. Dizininizdeki her bir Azure AD uygulaması için **uygulama kimliği URI 'si** benzersiz olmalıdır. Bu KIMLIK 256 karakterden daha uzun olamaz. Uygulama KIMLIĞI URI 'SI hakkında daha fazla bilgi için bkz. [uygulamaları Azure Active Directory Ile tümleştirme](../../active-directory/develop/quickstart-modify-supported-accounts.md#change-the-application-registration-to-support-different-accounts).
5. **Roller** bölümünde, Azure AD uygulaması için rol (ler) veya özelleştirilmiş izinleri belirtin.
6. **Kaydet**’i seçin.

Bir Azure AD uygulaması ekledikten veya oluşturduktan sonra, **Kullanıcı** bölümüne dönüp Kiracı kimliği, istemci kimliği, yanıt URL 'si ve uygulama kimliği URI 'si de dahil olmak üzere uygulamanın ayarlarını gözden geçirmek için uygulama adını seçebilirsiniz.

### <a name="remove-an-azure-ad-application"></a>Azure AD uygulamasını kaldırma

İş hesabınızdan (Azure AD kiracısı) bir uygulamayı kaldırmak için **kullanıcılara** gidin ( **Hesap ayarları**altında), en sağdaki sütunda onay kutusunu kullanarak kaldırmak istediğiniz uygulamayı seçin ve ardından kullanılabilir eylemlerden **Kaldır** ' ı seçin. Seçili uygulamaları kaldırmak istediğinizi onaylamanız için bir açılır pencere görüntülenir.

### <a name="manage-keys-for-an-azure-ad-application"></a>Bir Azure AD uygulaması için anahtarları yönetme

Azure AD uygulamanız Microsoft Azure AD verileri okuyup yazıyorsa, anahtar gerekir. Iş Ortağı Merkezi 'nde bilgilerini düzenleyerek bir Azure AD uygulaması için anahtarlar oluşturabilirsiniz. Artık gerekli olmayan anahtarları da kaldırabilirsiniz.

1. **Kullanıcılar** sayfasından ( **Hesap ayarları**altında) Azure AD uygulamasının adını seçin. Anahtarın oluşturulduğu tarih ve ne zaman sona ereceği 50 dahil olmak üzere Azure AD uygulaması için etkin anahtarların tümünü görürsünüz.
2. Artık gerekli olmayan bir anahtarı kaldırmak için **Kaldır**' ı seçin.
3. Yeni bir anahtar eklemek için **Yeni anahtar Ekle**' yi seçin.
4. **ISTEMCI kimliği** ve **anahtar değerlerini**gösteren bir ekran görürsünüz. Bu sayfadan ayrıldıktan sonra bir daha erişemeyeceksiniz, bu bilgileri yazdırdığınızdan veya kopyalamadığınızdan emin olun.
5. Daha fazla anahtar oluşturmak istiyorsanız **başka bir anahtar Ekle**' yi seçin.

## <a name="define-user-roles-and-permissions"></a>Kullanıcı rollerini ve izinlerini tanımlama

Şirketinizin kullanıcılarına, Iş Ortağı Merkezi 'nde ticari Market programı için aşağıdaki roller ve izinler atanabilir:

- **Sahibi**
  - "Bu, tüm hesap kullanıcılarını oluşturma ve düzenleme ve tüm finansal ve hesap ayarlarını değiştirme özelliği de dahil olmak üzere, ilk olarak hesabı oluşturan ve ona tamamen erişim sağlayan kişidir. Hesap genellikle yalnızca bir sahibe sahiptir. "
- **Manager**
  - Vergi ve ödeme ayarları hariç tüm Microsoft hesabı özelliklerine erişebilir
  - Kullanıcıları, rolleri ve iş hesaplarını (kiracılar) yönetebilir
- **Geliştirici**
  - Teklifleri yönetebilir ve yayımlayabilir
  - Bazı yayımcı raporlarını görüntüleyebilir

> [!NOTE]
> Ticari Market programı için genel yönetici, Iş katılımcısı, mali katkı ve Pazar layıcı rolleri kullanılmaz. Bu rollerin kullanıcılara atanması etkisizdir. Yalnızca yönetici ve geliştirici rolleri kullanıcılara izinler verir.

Azure Active Directory (AD), bulut çözümü sağlayıcısı (CSP), Denetim Masası satıcısı (CPV), Konuk kullanıcılar veya Microsoft İş Ortağı Ağı (MPN) gibi Iş Ortağı Merkezi alanlarında rol ve izinleri yönetme hakkında daha fazla bilgi için bkz. [Iş Ortağı Merkezi 'nde Kullanıcı rolleri ve Izinleri atama](https://docs.microsoft.com/partner-center/permissions-overview).

## <a name="manage-tenants"></a>Kiracılar yönetme

Bu belgede "iş hesabınız" olarak da adlandırılan bir Azure Active Directory (AD) kiracısı, kuruluşunuzun Azure portal ayarlanmış bir gösterimidir ve iç ve dış kullanıcılarınız için Microsoft bulut hizmetlerinin belirli bir örneğini yönetmenize yardımcı olur. Kuruluşunuz Azure, Microsoft Intune veya Office 365 gibi bir Microsoft bulut hizmetine abone olduysa, sizin için bir Azure AD kiracısı oluşturulmuştur.

Iş Ortağı Merkezi ile kullanmak için birden çok kiracı ayarlayabilirsiniz. Iş Ortağı Merkezi hesabında **yönetici** rolüne sahip herhangi bir Kullanıcı, hesaptan Azure AD kiracılar ekleme ve kaldırma seçeneğine sahip olur.  

### <a name="add-an-existing-tenant"></a>Mevcut bir kiracıyı ekleyin

Başka bir Azure AD kiracısını Iş Ortağı Merkezi hesabınızla ilişkilendirmek için:

1. **Kiracılar** sayfasından ( **Hesap ayarları**altında), **başka bir Azure AD kiracısını ilişkilendir**' i seçin.
2. İlişkilendirmek istediğiniz kiracının Azure AD kimlik bilgilerinizi girin.
3. Azure AD kiracınız için kuruluş ve etki alanı adını gözden geçirin. İlişkilendirmeyi gerçekleştirmek için **Onayla**' yı seçin.

İlişkilendirme başarılı olursa, Iş Ortağı Merkezi 'ndeki **Kullanıcılar** bölümünde Hesap kullanıcılarını eklemek ve yönetmek için bu işlemi gerçekleştirebilirsiniz.

### <a name="create-a-new-tenant"></a>Yeni kiracı oluşturma

Iş Ortağı Merkezi hesabınızla yeni bir Azure AD kiracısı oluşturmak için:

1. **Kiracılar** sayfasından ( **Hesap ayarları**altında) **Yeni bir Azure AD kiracısı oluştur**' u seçin.
2. Yeni Azure AD 'niz için dizin bilgilerini girin:
    - **Etki alanı adı**: Azure AD etki alanınız için kullanacağımız benzersiz ad ". onmicrosoft.com" ile birlikte. Örneğin, "örnek" girdiyseniz, Azure AD etki alanınız "example.onmicrosoft.com" olacaktır.
    - **İletişim e-postası**: gerekirse hesabınız hakkında sizinle iletişim kurabildiğimiz bir e-posta adresi.
    - **Genel yönetici kullanıcı hesabı bilgileri**: yeni genel yönetici hesabı için kullanmak istediğiniz ilk ad, soyadı, Kullanıcı adı ve parola.
3. Yeni etki alanı ve hesap bilgilerini onaylamak için **Oluştur** ' u seçin.
4. [Kullanıcı eklemeye ve yönetmeye](#manage-users)başlamak Için yenı Azure AD Genel Yönetici Kullanıcı adı ve parolasıyla oturum açın.

Azure portal içinde Iş Ortağı Merkezi portalı yerine yeni kiracılar oluşturma hakkında daha fazla bilgi için, [Azure Active Directory yeni bir kiracı oluşturma](../../active-directory/fundamentals/active-directory-access-create-new-tenant.md)makalesine bakın.

### <a name="remove-a-tenant"></a>Kiracı kaldırma

Iş Ortağı Merkezi hesabınızdan bir kiracı kaldırmak için **kiracılar** sayfasında adını bulun ( **Hesap ayarları**' nda) ve ardından **Kaldır**' ı seçin. Kiracıyı kaldırmak istediğinizi onaylamanız istenir. Bunu yaptığınızda, söz konusu Kiracıdaki hiçbir Kullanıcı Iş Ortağı Merkezi hesabında oturum açamaz ve bu kullanıcılar için yapılandırdığınız tüm izinler kaldırılır.

Bir kiracıyı kaldırdığınızda, Iş Ortağı Merkezi hesabına bu kiracıya eklenen tüm kullanıcılar artık hesapta oturum açamaz.

> [!TIP]
> Halen aynı Kiracıdaki bir hesabı kullanarak Iş Ortağı Merkezi 'nde oturum açtıysanız, kiracıyı kaldıramazsınız. Bir kiracıyı kaldırmak için Iş Ortağı Merkezi ' nde hesapla ilişkili başka bir kiracının **Yöneticisi** olarak oturum açmanız gerekir. Hesapla ilişkili yalnızca bir kiracı varsa, bu kiracı yalnızca hesabı açan Microsoft hesabı oturum açtıktan sonra kaldırılabilir.

## <a name="agreements"></a>Sözleşmeler

Iş Ortağı Merkezi 'nin **anlaşmalar** bölümü ( **Hesap ayarları**altında) yetkilendirdiğiniz yayımlama anlaşmalarının bir listesini görüntülemenize olanak sağlar. Bu anlaşmalar, kabul edildiği tarih ve sözleşmeyi kabul eden kullanıcının adı dahil olmak üzere ad ve sürüm numarasına göre listelenir.

İlgilenmeniz gereken anlaşma güncelleştirmeleri varsa, bu sayfanın en üstünde **gerekli eylemler** görünebilir. Güncelleştirilmiş bir sözleşmeyi kabul etmek için önce bağlantılı sözleşme sürümünü okuyun, sonra **sözleşmeyi kabul et**' i seçin.
