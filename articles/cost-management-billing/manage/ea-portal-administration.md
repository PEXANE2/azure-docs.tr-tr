---
title: Azure EA portalı yönetimi
description: Bu makalede, bir yöneticinin Azure EA portalında gerçekleştirdiği genel görevler açıklanmaktadır.
author: bandersmsft
ms.author: banders
ms.date: 05/07/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: boalcsva
ms.openlocfilehash: a3551b4f09202b33cee768a69f31a6c5d5d36777
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83648199"
---
# <a name="azure-ea-portal-administration"></a>Azure EA portalı yönetimi

Bu makalede, bir yöneticinin Azure EA portalında gerçekleştirdiği genel görevler açıklanmaktadır (https://ea.azure.com). Azure EA portalı, müşterilerin Azure EA hizmetlerinin maliyetini yönetmesine yardımcı olan bir çevrimiçi yönetim portalıdır. Azure EA portalı hakkında giriş bilgileri için [Azure EA portalı ile çalışmaya başlama](ea-portal-get-started.md) makalesini okuyun.

## <a name="associate-an-account-to-a-department"></a>Bir hesabı bir departmanla ilişkilendirme

Kuruluş Yöneticileri, mevcut hesapları kayıt kapsamındaki Bölümlerle ilişkilendirebilir.

### <a name="to-associate-an-account-to-a-department"></a>Bir hesabı bir departmanla ilişkilendirmek için

1. Azure EA Portal'da kuruluş yöneticisi olarak oturum açın.
1. Sol gezinti panelinde **Yönet**’i seçin.
1. **Bölüm**'ü seçin.
1. Hesabın bulunduğu satırın üzerine gelin ve sağ taraftaki kalem simgesini seçin.
1. Açılan menüden bölümü seçin.
1. **Kaydet**’i seçin.

## <a name="department-spending-quotas"></a>Departman harcama kotaları

EA müşterileri, bir kayıt kapsamındaki bölümler için ayrı harcama kotaları belirleyebilir veya mevcut kotaları değiştirebilir. Harcama kotasının miktarı geçerli taahhüt dönemini kapsayacak şekilde ayarlanır. Geçerli taahhüt döneminin sonunda değerler güncelleştirilmediği sürece sistem, mevcut harcama kotasını takip eden taahhüt dönemine uzatır.

Bölüm yöneticisi, harcama kotasını görüntüleyebilir ancak kota miktarı yalnızca kuruluş yöneticisi tarafından güncelleştirilebilir. Kota %50, %75, %90 ve %100 düzeyine ulaştığında kuruluş yöneticisine ve bölüm yöneticisine bildirim gönderilir.

### <a name="enterprise-administrator-to-set-the-quota"></a>Kuruluş yöneticisi olarak kotayı belirlemek için:

 1. Azure EA Portal'ı açın.
 1. Sol gezinti panelinde **Yönet**’i seçin.
 1. **Bölüm** sekmesini seçin.
 1. Bölümü seçin.
 1. Bölüm Ayrıntıları bölümündeki kalem simgesini seçin veya yeni bir bölüm ve harcama kotası eklemek için **+ Bölüm Ekle** simgesini seçin.
 1. Bölüm Ayrıntıları’nın altındaki Harcama Kotası $ kutusuna kaydın para birimini kullanarak bir harcama kotası girin (değer 0'dan büyük olmalıdır).
    - Bu adımda Bölüm Adı ve Maliyet Merkezi bilgilerini de düzenleyebilirsiniz.
 1. **Kaydet**’i seçin.

Bölüm harcama kotası, Bölüm sekmesindeki Bölüm Listesi görünümünde görünür duruma gelir. Azure EA Portal, geçerli taahhüt döneminin sonunda harcama kotalarını bir sonraki taahhüt dönemine aktarır.

Bölüm kotası tutarı, mevcut parasal taahhütten bağımsızdır ve kota miktarı ile uyarılar yalnızca birinci taraf kullanımı için geçerlidir. Bölüm harcama kotası yalnızca bilgilendirme amaçlıdır ve harcama limiti uygulamaz.

### <a name="department-administrator-to-view-the-quota"></a>Bölüm yöneticisi olarak kotayı görüntülemek için:

1. Azure EA Portal'ı açın.
1. Sol gezinti panelinde **Yönet**’i seçin.
1. **Bölüm** sekmesini seçin ve harcama kotalarının yer aldığı Bölüm Listesi görünümünü açın.

Dolaylı müşteriyseniz ücret özelliklerinin kanal iş ortağınız tarafından etkinleştirilmesi gerekir.

## <a name="enterprise-user-roles"></a>Kurumsal kullanıcı rolleri

Azure EA portalı, Azure EA maliyetlerinizi ve kullanımınızı yönetmenize yardımcı olur. Azure EA portalında üç ana rol vardır:

- EA yöneticisi
- Departman yöneticisi
- Hesap sahibi

Her rolün farklı bir erişim düzeyi ve yetkisi vardır.

Kullanıcı rolleri hakkında daha fazla bilgi için bkz. [Kurumsal kullanıcı rolleri](https://docs.microsoft.com/azure/billing/billing-ea-portal-get-started#enterprise-user-roles).

## <a name="add-an-azure-ea-account"></a>Azure EA hesabı ekleme

Azure EA hesabı, Azure EA portalındaki bir kuruluş birimidir. Hem abonelikleri yönetmek için hem de raporlama amacıyla kullanılır. Azure hizmetlerine erişmek için bir hesap oluşturmanız ve sizin için oluşturulmuş bir hesap olması gerekir.

Azure hesapları hakkında daha fazla bilgi için bkz. Hesap ekleme.

## <a name="enterprise-devtest-offer"></a>Kurumsal Geliştirme ve Test Teklifi

Azure kuruluş yöneticisi olarak kuruluşunuzdaki hesap sahiplerinin EA Geliştirme ve Test teklifini kullanarak abonelik oluşturmasını sağlayabilirsiniz. Bunu yapmak için Azure EA Portalında hesap sahibi için Geliştirme ve Test kutusunu seçin.

Geliştirme ve Test kutusunu işaretledikten sonra hesap sahibine Geliştirme ve Test aboneleri için ihtiyaç duyulan EA Geliştirme ve Test aboneliklerini oluşturabileceğini bildirin.

Bu teklif Visual Studio aboneleri, geliştirme ve test iş yüklerini Azure'da özel Geliştirme ve Test fiyatlarıyla çalıştırabilir. Windows 8.1 ve Windows 10 da dahil olmak üzere tüm Geliştirme ve Test görünümlerini içeren bir galeriye erişim sağlar.

### <a name="to-set-up-the-enterprise-devtest-offer"></a>Kurumsal Geliştirme ve Test teklifini ayarlamak için:

1. Kuruluş yöneticisi olarak oturum açın.
1. Sol gezinti panelinde **Yönet**’i seçin.
1. **Hesap** sekmesini seçin.
1. Geliştirme ve Test erişimini etkinleştirmek istediğiniz hesabın yer aldığı satırı seçin.
1. Satırın sağ tarafındaki kalem simgesini seçin.
1. Geliştirme ve Test onay kutusunu seçin.
1. **Kaydet**’i seçin.

Bir kullanıcı Azure EA Portal aracılığıyla hesap sahibi olarak eklendiğinde hesap sahibiyle ilişkilendirilmiş olan kullandıkça öde Geliştirme ve Test teklifini Visual Studio için aylık kredi tekliflerini temel alan abonelerle ilişkilendirilmiş olan Azure abonelikleri, EA Geliştirme ve Test teklifine dönüştürülür. Kullandıkça öde gibi Hesap Sahibiyle ilişkilendirilmiş olan diğer teklif türlerini temel alan abonelikler, Microsoft Azure Kurumsal tekliflerine dönüştürülür.

Geliştirme ve Test Teklifi şu an için Azure Kamu müşterilerinin kullanımına açık değildir.

## <a name="delete-subscription"></a>Aboneliği silme

Hesap sahibi olduğunuz bir aboneliği silmek için:

1. Hesabınızla ilişkilendirilmiş kimlik bilgileriyle Azure portalında oturum açın.
1. Hub menüsünde **Abonelikler**'i seçin.
1. Sayfanın sol üst köşesindeki abonelikler sekmesinde iptal etmek istediğiniz aboneliği seçin ve iptal sekmesini açmak için **Aboneliği iptal et**'i seçin.
1. Abonelik adını girip iptal için bir neden seçin ve ardından **Aboneliği iptal et** düğmesini seçin.

Abonelikleri yalnızca hesap yöneticileri iptal edebilir.

Daha fazla bilgi için bkz. [Aboneliğimi iptal edersem ne olur?](cancel-azure-subscription.md#what-happens-after-i-cancel-my-subscription)

## <a name="delete-an-account"></a>Hesap silme

Hesap kaldırma işlemi yalnızca etkin aboneliği bulunmayan etkin hesaplarda gerçekleştirilebilir.

1. Enterprise Portal'ın sol tarafındaki gezintisinde **Yönet**'i seçin.
1. **Hesap** sekmesini seçin.
1. Hesaplar tablosundan silmek istediğiniz hesabı seçin.
1. Hesap satırının sağ tarafındaki X simgesini seçin.
1. Hesabın altında etkin abonelik kalmadığında Hesap satırının altındaki **Evet**’i seçerek Hesap kaldırma işlemini onaylayın.

## <a name="update-notification-settings"></a>Bildirim ayarlarını güncelleştirme

Kuruluş Yöneticileri, kayıtlarıyla ilişkili kullanım bildirimlerini almak için otomatik olarak kaydedilir. Her Kuruluş Yöneticisi, bildirimlerin aralığını tek tek değiştirebilir veya hepsini tamamen devre dışı bırakabilir.

Bildirim kişileri, Azure EA portalındaki **Bildirim Kişisi** bölümünde gösterilir. Bildirim kişilerinizi yönetmek, kuruluşunuzdaki doğru kişilerin Azure EA bildirimleri almasını sağlar.

Geçerli bildirim ayarlarını görüntülemek için:

1. Azure EA portalında **Yönet** > **Bildirim Kişisi**’ne gidin.
2. E-posta Adresi – Kuruluş Yöneticisinin bildirimleri alan Microsoft Hesabı, İş veya Okul Hesabı ile ilişkili e-posta adresi.
3. Faturalandırılmamış Bakiye Bildirimi Sıklığı – Bildirimlerin her bir Kuruluş Yöneticisine gönderilmek üzere ayarlandığı sıklık.

Kişi eklemek için:

1. **+Kişi Ekle**’yi seçin.
2. E-posta adresini girin ve sonra onaylayın.
3. **Kaydet**’i seçin.

Yeni bildirim kişisi **Bildirim Kişisi** bölümünde gösterilir. Bildirim sıklığını değiştirmek için, bildirim kişisini seçin ve seçili satırın sağ tarafındaki kalem simgesini seçin. Sıklığı **günlük**, **haftalık**, **aylık** veya **hiçbiri** olarak ayarlayın.

_Yaklaşan kapsama dönemi bitiş tarihi_ ve _yaklaşan tarih_ yaşam döngüsü bildirimlerini devre dışı bırakıp sağlamayı bırakabilirsiniz. Yaşam döngüsü bildirimlerini devre dışı bırakmak, kapsama dönemi ve anlaşma bitiş tarihi hakkındaki bildirimleri gizler.

## <a name="azure-sponsorship-offer"></a>Azure Sponsorluğu Teklifi

Azure Sponsorluğu Teklifi, sınırlı ve sponsorlu bir Microsoft Azure hesabıdır. Yalnızca Microsoft tarafından seçilen sınırlı müşteriler tarafından e-posta daveti ile kullanılabilir. Microsoft Azure Sponsorluğu teklifine hak kazandıysanız hesap kimliğinize bir e-posta davetiyesi alırsınız.

Daha fazla bilgi için [sponsorluğu etkinleştirme amacıyla bir destek isteği oluşturun](https://aka.ms/azrsponsorship).

## <a name="conversion-to-work-or-school-account-authentication"></a>İş veya okul hesabı kimlik doğrulamasına dönüştürme

Azure Kurumsal kullanıcıları, Microsoft hesabı (MSA veya Live Kimliği) yerine iş veya okul hesabı (Azure Active Directory kullanan) kullanmayı tercih edebilir.

Başlamak için:

1. İş veya okul hesabını Azure EA Portal'da istediğiniz rollere ekleyin.
1. Hata alırsanız, hesap geçerli bir Active Directory hesabı olmayabilir.  Azure, Kullanıcı Asıl Adı (UPN) değerini kullanır ve bu her zaman e-posta adresiyle aynı olmayabilir.
1. İş veya okul hesabını kullanarak Azure EA Portal'da kimliğinizi doğrulayın.

### <a name="to-convert-subscriptions-from-microsoft-accounts-to-work-or-school-accounts"></a>Aboneliklerde Microsoft hesabı yerine iş veya okul hesabını kullanmak için:

1. Aboneliklerin sahibi olan Microsoft hesabını kullanarak yönetim portalında oturum açın.
1. Yeni hesaba geçiş yapmak için hesap sahipliği aktarma işlevini kullanın.
1. Bunu yaptığınızda Microsoft hesabında silinebilecek etkin abonelik kalmamalıdır.
1. Silinen hesaplar geçmişe dönük faturalama nedeniyle portalda kalır ancak devre dışı durumda görünür.  Yalnızca etkin hesapların görüntülenmesini sağlayan onay kutusunu seçerek eski hesapları görünümden kaldırabilirsiniz.

## <a name="account-subscription-ownership-faq"></a>Hesap aboneliği sahipliği hakkında SSS

Bu belgede hesap aboneliği sahipliğiyle ilgili sık sorulan sorular ve yanıtları yer almaktadır.

### <a name="how-many-azure-account-owners-can-you-have-per-subscription"></a>Bir abonelikte kaç Azure hesabı sahibi olabilir?

Her abonelikte yalnızca bir tane hesap sahibi bulunabilir.  Rol Tabanlı Erişim veya Erişim Denetimi (IAM) işlevlerini kullanarak daha fazla rol ekleyebilirsiniz. Bunun için şu sayfanın sol üst köşesindeki abonelik sekmesini kullanın: [portal.azure.com]](https://portal.azure.com).

### <a name="can-an-azure-account-owner-be-listed-under-more-than-one-department"></a>Bir Azure hesabı sahibi birden fazla bölümde listelenebilir mi?

Hayır, hesap sahibi yalnızca bir bölümle ilişkilendirilebilir. Bu ilke bölümle ilişkilendirilmiş olan maliyetlerle harcamaların Azure EA Portal'daki EA kaydı altında doğru şekilde izlenebilmesine ve ayrılabilmesine yardımcı olur.

### <a name="can-an-azure-account-owner-be-listed-as-a-security-group"></a>Bir Azure hesabı sahibi güvenlik grubu olarak listelenebilir mi?

Hayır, abonelik sahibi benzersiz bir Microsoft hesabı (MSA) veya Azure Active Directory (AAD) kimlik doğrulamasına sahip olmalıdır. Kuruluşunuzdaki kullanıcı değişikliklerinden etkilenmemek için genel hesaplar oluşturup abonelik erişimini AAD ile yönetebilirsiniz.

### <a name="can-an-individual-user-own-multiple-subscriptions"></a>Bir kullanıcı birden fazla aboneliğe sahip olabilir mi?

Bir Azure hesabı sahibi sınırsız sayıda abonelik oluşturup yönetebilir.

### <a name="how-can-i-accessview-all-my-organizations-subscriptions"></a>Kuruluşumun tüm aboneliklerine erişmek ve onları görüntülemek için ne yapabilirim?

Şu an için bu işlevin ilke yoluyla sağlanması gerekir. Başka bir deyişle hesabınızın oluşturulan her abonelikte rol tabanlı erişim kullanılarak bir abonelik rolüne eklenmesini sağlamanız gerekir.

### <a name="where-do-i-go-to-create-a-subscription"></a>Nereden abonelik oluşturabilirim?

Kurumsal Azure (EA) teklifi aboneliği oluşturabilmeniz için hesabınızın Azure EA Portal'da EA kayıt yöneticiniz tarafından hesap sahibi rolüne eklenmesi gerekir. Ardından EA teklif türü aboneliklerini oluşturma yetkisini elde etmek için Azure EA Portal'da oturum açmanız gerekir. İlk EA aboneliğinizin EA Portal'daki abonelik sekmesinde yer alan ‘+ Abonelik Ekle’ bağlantısıyla oluşturulmasını öneririz.  Ancak hesabınız yetkilendirildikten sonra portal.azure.com adresindeki Abonelikler sekmesinin sol üst köşesinden hesap oluşturmak daha kolay olabilir. Burada tek adımda aboneliğinizi hem oluşturup hem de adını değiştirebilirsiniz.

### <a name="who-can-create-a-subscription"></a>Kimler abonelik oluşturabilir?

Kurumsal Azure teklifi türünde bir abonelik oluşturabilmek için [EA Portal](https://ea.azure.com)'da hesap sahibi rolüne sahip olmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Sanal makine rezervasyonlarının](ea-portal-vm-reservations.md) paradan tasarruf etmenize nasıl yardımcı olduğu hakkında bilgi edinin.
- Azure EA portalı sorunlarını gidermek için yardıma ihtiyacınız varsa [Azure EA portalı erişim sorunlarını giderme](ea-portal-troubleshoot.md) konusuna bakın.
