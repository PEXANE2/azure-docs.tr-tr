---
title: Azure EA portalı yönetimi
description: Bu makalede, bir yöneticinin Azure EA portalında gerçekleştirdiği genel görevler açıklanmaktadır.
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/09/2019
ms.topic: conceptual
ms.service: billing
manager: boalcsva
ms.openlocfilehash: 7b594cef2fefa164ef900cbfd65fcf95d5d47b94
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70901143"
---
# <a name="azure-ea-portal-administration"></a>Azure EA portalı yönetimi

Bu makalede, bir yöneticinin Azure EA portalında gerçekleştirdiği genel görevler açıklanmaktadır (https://ea.azure.com). Azure EA portalı, müşterilerin Azure EA hizmetlerinin maliyetini yönetmesine yardımcı olan bir çevrimiçi yönetim portalıdır. Azure EA portalı hakkında giriş bilgileri için [Azure EA portalı ile çalışmaya başlama](billing-ea-portal-get-started.md) makalesini okuyun.

## <a name="add-a-new-enterprise-administrator"></a>Yeni bir kuruluş yöneticisi ekleme

Kuruluş yöneticileri bir Azure EA kaydını yönetirken en fazla ayrıcalığa sahip olan kullanıcılardır. İlk Azure EA yöneticisi, EA sözleşmesi ayarlanırken oluşturulmuştur. Ancak, dilediğiniz zaman yeni yöneticiler ekleyebilir veya kaldırabilirsiniz. Yeni yöneticiler yalnızca var olan yöneticiler tarafından eklenir. Ek kuruluş yöneticileri ekleme hakkında daha fazla bilgi için bkz. [Başka bir kuruluş yöneticisi oluşturma](billing-ea-portal-get-started.md#create-another-enterprise-admin). Faturalama profili rolleri ve görevleri hakkında daha fazla bilgi için bkz. [Faturalama profili rolleri ve görevleri](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

## <a name="update-user-state-from-pending-to-active"></a>Beklemede olan kullanıcı durumunu Etkin olarak güncelleştirme

Bir Azure EA kaydına ilk kez yeni Hesap Sahipleri (AO) eklendiğinde durumları _beklemede_ olarak görünür. Yeni bir hesap sahibi etkinleştirme karşılama e-postasını aldığında, hesabını etkinleştirmek için oturum açabilir. Hesabını etkinleştirdiğinde, _beklemede_ olan hesap durumu _etkin_ olarak güncelleştirilir. Yeni kullanıcılardan Bir Ticari Hesap oluşturmak için ad ve soyadı girmeleri istenebilir. Bu durumda, devam etmek için gerekli bilgileri eklemesi gerekir. Bundan sonra hesap etkinleştirilir.

## <a name="add-a-department-admin"></a>Departman yöneticisi ekleme

Azure EA yöneticisi bir departman oluşturduktan sonra, Azure Kuruluş yöneticisi departman yöneticileri ekleyebilir ve her birini bir departmanla ilişkilendirebilir. Bir departman yöneticisi yeni hesaplar oluşturabilir. Azure EA aboneliklerinin oluşturulabilmesi için yeni hesaplar gereklidir.

Departman ekleme hakkında daha fazla bilgi için bkz. Azure EA departmanı oluşturma.

## <a name="enterprise-user-roles"></a>Kurumsal kullanıcı rolleri

Azure EA portalı, Azure EA maliyetlerinizi ve kullanımınızı yönetmenize yardımcı olur. Azure EA portalında üç ana rol vardır:

- EA yöneticisi
- Departman yöneticisi
- Hesap sahibi

Her rolün farklı bir erişim düzeyi ve yetkisi vardır.

Kullanıcı rolleri hakkında daha fazla bilgi için bkz. Kurumsal kullanıcı rolleri.

## <a name="add-an-azure-ea-account"></a>Azure EA hesabı ekleme

Azure EA hesabı, Azure EA portalında abonelikleri yönetmek için kullanılan ve aynı zamanda raporlama için kullanılan bir kurumsal birimdir. Azure hizmetlerine erişmek için bir hesap oluşturmanız ve sizin için oluşturulmuş bir hesap olması gerekir.

Azure hesapları hakkında daha fazla bilgi için bkz. Hesap ekleme.

## <a name="transfer-an-enterprise-account-to-a-new-enrollment"></a>Kurumsal bir hesabı yeni bir kayda aktarma

Kurumsal bir hesabı yeni bir kayda aktarırken aşağıdaki noktaları aklınızda bulundurun:

- Yalnızca istekte belirtilen hesaplar aktarılır. Tüm hesaplar seçilirse hepsi aktarılır.
- Kaynak kaydının durumu etkin veya genişletilmiş olarak kalır. Kaydı süresi dolana kadar kullanmaya devam edebilirsiniz.

### <a name="effective-transfer-date"></a>Geçerli aktarım tarihi

Geçerli aktarım tarihi, aktarmak istediğiniz kaydın başlangıç tarihi veya sonrasındaki bir tarih olabilir. Aktarımı yapmakta olduğunuz kayıt _hedef kayıttır_. Hesap aktarımından sonra, hesapta geçerli aktarım tarihinden önceki tüm kullanım bilgileri aktardığınız kayıt içinde kalır. Aktarmakta olduğunuz kayıt _kaynak kayıttır_.  Kaynak kayıt kullanımı, parasal taahhüt karşılığında veya fazla kullanım olarak ücretlendirilir. Geçerli aktarım tarihinden sonra gerçekleşen kullanımlar yeni kayda aktarılır ve uygun şekilde ücretlendirilir.

Bir hesap aktarımını, hedef kaydın başlangıç tarihine kadar geçerli olacak şekilde ayarlayabilirsiniz. Veya kaynak kaydının geçerli olduğu başlangıç tarihine kadar.

### <a name="monetary-commitment"></a>Parasal taahhüt

Parasal taahhüt, kayıtlar arasında aktarılamaz. Parasal taahhüt bakiyeleri, sipariş edildiği kayda sözleşmeyle bağlıdır. Parasal taahhüt, hesap veya kayıt aktarma işleminin bir parçası olarak aktarılmaz.

### <a name="services-affected"></a>Etkilenen hizmetler

Hesap aktarımı sırasında hizmet kesintisi olmaz. Gerekli tüm bilgiler sağlanırsa isteğiniz gün içinde tamamlanabilir.

### <a name="prerequisites"></a>Ön koşullar

Bir hesap aktarımı istediğinizde aşağıdaki bilgileri sağlayın:


- Aktarılacak hesabın hesap adı ve hesap sahibi kimliği
- Kaynak kaydı için, aktarılacak kayıt numarası ve hesap
- Kaynak kaydı için, aktarımın yapılacağı kayıt numarası
- Hesap aktarımı geçerlilik tarihi, hedef kaydın başlangıç tarihi veya sonrasındaki bir tarih olabilir

Hesap aktarımından önce göz önünde bulundurmanız gereken diğer noktalar:

- Hedef ve kaynak kaydı için bir EA yöneticisinden onay gerekir
  - Bazı durumlarda Microsoft, kaynak kaydının EA yöneticisinden ek onay isteyebilir
- Hesap aktarımı gereksinimlerinizi karşılamıyorsa, kayıt aktarımı yapmayı düşünün.
- Hesap aktarımı tüm EA departman yöneticileri de dahil olmak üzere tüm hizmetleri, abonelikleri, hesapları, departmanları ve kayıt yapısının tamamını aktarır.
- Hesap aktarımı, kaynak kayıt durumunu _Aktarıldı_ olarak ayarlar. Aktarılan hesap yalnızca geçmiş kullanım raporlama amacıyla kullanılabilir.
- Durumu aktarıldı olan bir kayda rol veya abonelik ekleyemezsiniz. Durum, kayda aykırı ek kullanım yapılmasını önler.
- Kaynak anlaşmasındaki kalan parasal taahhüt bakiyeleri, geleceğe dönük hükümler de dahil olmak üzere kaybedilir.


## <a name="transfer-enterprise-enrollment-to-a-new-one"></a>Kuruluş kaydını yeni bir kayda aktarma

Bir kuruluş kaydının tamamını bir kayda aktarmak istediğinizde aşağıdaki eylemler gerçekleşir:

- Tüm EA departman yöneticileri de dahil olmak üzere tüm Azure hizmetleri, abonelikleri, hesapları, departmanları ve kayıt yapısının tamamı aktarılır.
- Kayıt durumu _Aktarıldı_ olarak ayarlanır. Aktarılan kayıt yalnızca geçmiş kullanım raporlama amacıyla kullanılabilir.
- Aktarılan bir kayda rol veya abonelik ekleyemezsiniz. Aktarıldı durumu, kayda aykırı ek kullanım yapılmasını önler.
- Anlaşmadaki kalan parasal taahhüt bakiyeleri, geleceğe dönük hükümler de dahil olmak üzere kaybedilir.

### <a name="effective-transfer-date"></a>Geçerli aktarım tarihi

Geçerli aktarım tarihi, hedef kayda aktarmak istediğiniz kaydın başlangıç tarihi veya sonrasındaki bir tarih olabilir.

Kaynak kayıt kullanımı, parasal taahhüt karşılığında veya fazla kullanım olarak ücretlendirilir. Geçerli aktarım tarihinden sonra gerçekleşen kullanımlar yeni kayda aktarılır ve uygun şekilde ücretlendirilir.

### <a name="effective-transfer-date-in-the-past"></a>Geçmişteki geçerli aktarım tarihi

Bir hesap aktarımını, hedef kaydın başlangıç tarihine kadar geçerli olacak şekilde ayarlayabilirsiniz. Veya kaynak kaydının geçerli olduğu başlangıç tarihine kadar.

### <a name="monetary-commitment"></a>Parasal taahhüt

Parasal taahhüt, kayıtlar arasında aktarılamaz. Parasal taahhüt bakiyeleri, sipariş edildiği kayda sözleşmeyle bağlıdır. Parasal taahhüt, hesap veya kayıt aktarma işleminin bir parçası olarak aktarılmaz.

### <a name="services-affected"></a>Etkilenen hizmetler

Hesap aktarımı sırasında hizmet kesintisi olmaz. Gerekli tüm bilgiler sağlanırsa isteğiniz gün içinde tamamlanabilir.

### <a name="prerequisites"></a>Ön koşullar

Bir kayıt aktarımı istediğinizde aşağıdaki bilgileri sağlayın:

- Kaynak kaydı için, aktarılacak kayıt numarası ve hesap
- Kaynak kaydı için, aktarımın yapılacağı kayıt numarası
- Kayıt aktarımı geçerlilik tarihi, hedef kaydın başlangıç tarihi veya sonrasındaki bir tarih olabilir. Seçilen tarih, daha önce düzenlenmiş bir fazla kullanım faturası için kullanımı etkilemez.

Kayıt aktarımından önce göz önünde bulundurmanız gereken diğer noktalar:

- Hedef ve kaynak kaydı için bir EA yöneticisinden onay gerekir
  - Bazı durumlarda Microsoft, kaynak kaydının EA yöneticisinden ek onay isteyebilir
- Kayıt aktarımı gereksinimlerinizi karşılamıyorsa, hesap aktarımı yapmayı düşünün.
- Yalnızca belirttiğiniz hesaplar aktarılır. Tüm hesaplarınızı aktarmayı isteyebilirsiniz.
- Kaynak kaydının durumu etkin/genişletilmiş olarak kalır. Kaydı süresi dolana kadar kullanmaya devam edebilirsiniz.

## <a name="change-account-owner"></a>Hesap sahibini değiştirme

Azure EA portalı, abonelikleri bir hesap sahibinden diğerine aktarabilir. Daha fazla bilgi için bkz. [Hesap sahibini değiştirme](billing-ea-portal-get-started.md#change-account-owner).

## <a name="subscription-transfer-effects"></a>Abonelik aktarımının etkileri

Bir Azure aboneliği aynı Azure Active Directory kiracısındaki bir hesaba aktarılırsa, abonelikteki kaynakları yönetmek için [rol tabanlı erişim denetimine (RBAC)](../role-based-access-control/overview.md) sahip olan tüm kullanıcılar, gruplar ve hizmet sorumluları aynı erişime sahip olmaya devam eder.

Aboneliğe RBAC erişimi olan kullanıcıları görüntülemek için:

1. Azure portalında **Abonelikler**’i açın.
2. Görüntülemek istediğiniz aboneliği seçin ve ardından **Erişim denetimi (IAM)** seçeneğini belirleyin.
3. **Rol atamaları**’nı seçin. Rol atamaları sayfasında, aboneliğe RBAC erişimi olan tüm kullanıcılar listelenir.

Abonelik farklı bir Azure AD kiracısındaki bir hesaba aktarılırsa, abonelikteki kaynakları yönetmek için [RBAC](../role-based-access-control/overview.md) iznine sahip olan tüm kullanıcılar, gruplar ve hizmet sorumluları _erişimi_ kaybeder. RBAC erişimi mevcut olmasa da aboneliğe aşağıdaki güvenlik mekanizmaları aracılığıyla erişilebilir:

- Abonelik kaynaklarına kullanıcı yöneticisi hakları veren yönetim sertifikaları. Daha fazla bilgi için bkz. [Azure için Yönetim Sertifikası Oluşturma ve Karşıya Yükleme](../cloud-services/cloud-services-certs-create.md).
- Depolama gibi hizmetler için erişim anahtarları. Daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](../storage/common/storage-account-overview.md).
- Azure Sanal Makineleri gibi hizmetler için Uzaktan Erişim kimlik bilgileri.

Alıcının Azure kaynaklarına erişimi kısıtlaması gerekiyorsa, hizmetle ilişkili tüm gizli dizileri güncelleştirmeyi düşünmelidir. Çoğu kaynak aşağıdaki adımlar kullanılarak güncelleştirilir:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Merkez menüsünde **Tüm kaynaklar**'ı seçin.
3. Kaynağı seçin.
4. Kaynak sayfasında, mevcut gizli dizileri görüntülemek ve güncelleştirmek için **Ayarlar**’a tıklayın.



## <a name="close-an-azure-enterprise-enrollment"></a>Azure kurumsal kaydını kapatma

Azure EA kaydınızı kapatmak istiyorsanız şunları yapabilirsiniz:

- Azure portalı üzerinde Azure EA ile ilişkili tüm aboneliklerinizi iptal etme.
- Müşteri yazılım danışmanı veya iş ortağınız ile iletişim kurarak Azure Kurumsal Anlaşmanızı kapatmasını isteme.

## <a name="update-notification-settings"></a>Bildirim ayarlarını güncelleştirme

Kuruluş Yöneticileri, kayıtlarıyla ilişkili kullanım bildirimlerini almak için otomatik olarak kaydedilir. Her Kuruluş Yöneticisi, bildirimlerin aralığını tek tek değiştirebilir veya hepsini tamamen devre dışı bırakabilir.

Bildirim kişileri, Azure EA portalındaki **Bildirim Kişisi** bölümünde gösterilir. Bildirim kişilerinizi yönetmek, kuruluşunuzdaki doğru kişilerin Azure EA bildirimleri almasını sağlar.

Geçerli bildirim ayarlarını görüntülemek için:

1. Azure EA portalında **Yönet** > **Bildirim Kişisi**’ne gidin.
2. E-posta Adresi – Kuruluş Yöneticisinin bildirimleri alan Microsoft Hesabı, İş veya Okul Hesabı ile ilişkili e-posta adresi.
3. Faturalandırılmamış Bakiye Bildirimi Sıklığı – Bildirimlerin her bir Kuruluş Yöneticisine gönderilmek üzere ayarlandığı sıklık.

Kişi eklemek için:

1. **+Kişi Ekle**’ye tıklayın.
2. E-posta adresini girin ve sonra onaylayın.
3. **Kaydet**’e tıklayın.

Yeni bildirim kişisi **Bildirim Kişisi** bölümünde gösterilir. Bildirim sıklığını değiştirmek için, bildirim kişisini seçin ve seçili satırın sağ tarafındaki kalem simgesine tıklayın. Sıklığı **günlük**, **haftalık**, **aylık** veya **hiçbiri** olarak ayarlayın.

_Yaklaşan kapsama dönemi bitiş tarihi_ ve _yaklaşan tarih_ yaşam döngüsü bildirimlerini devre dışı bırakıp sağlamayı bırakabilirsiniz. Yaşam döngüsü bildirimlerini devre dışı bırakmak, kapsama dönemi ve anlaşma bitiş tarihi hakkındaki bildirimleri gizler.

## <a name="manage-partner-notifications"></a>İş ortağı bildirimlerini yönetme

İş Ortağı Yöneticileri, kayıtları için kullanım bildirimlerini ne sıklıkla alacaklarını yönetebilir. Bu yöneticiler, faturalandırılmamış bakiyeleriyle ilgili haftalık bildirimleri otomatik olarak alırlar. Bildirimlerin sıklığını aylık, haftalık, günlük olarak tek tek değiştirebilir veya tamamen devre dışı bırakabilirler.

Bir kullanıcı tarafından bildirim alınmadıysa, kullanıcının bildirim ayarlarının doğru olduğunu aşağıdaki adımlarla onaylayın.

1. Azure EA portalında İş Ortağı Yöneticisi olarak oturum açın.
2. **Yönet**’e ve sonra **İş Ortağı** sekmesine tıklayın.
3. **Yönetici** bölümü altındaki yönetici listesini görüntüleyin.
4. Bildirim tercihlerini düzenlemek için uygun yöneticinin üzerine gelin ve kurşun kalem simgesine tıklayın.
5. Gerektiğinde bildirim sıklığını ve yaşam döngüsü bildirimlerini güncelleştirin.
6. Gerekirse bir kişi ekleyin ve **Ekle**'ye tıklayın.
7. **Kaydet**’e tıklayın.

![Eklemek için Kişi Ekle seçeneğini gösteren örnek ](./media/billing-ea-portal-administration/create-ea-manage-partner-notification.png)

## <a name="azure-sponsorship-offer"></a>Azure Sponsorluğu teklifi
Azure Sponsorluğu teklifi, bir sınırlı sponsorlu Microsoft Azure hesabıdır. Yalnızca Microsoft tarafından seçilen sınırlı müşteriler tarafından e-posta daveti ile kullanılabilir. Microsoft Azure Sponsorluğu teklifine hak kazandıysanız hesap kimliğinize bir e-posta davetiyesi alırsınız.
Daha fazla bilgi için bkz.

- Sponsorluk teklifine genel bakış - https://azure.microsoft.com/en-us/offers/ms-azr-0143p/
- Sponsorluk bakiyesi portalı - https://www.microsoftazuresponsorships.com/balance  
- Sponsorluk dış SSS - https://azuresponsorships-staging.azurewebsites.net/faq
- Sponsorluk Etkinleştirme için destek isteği - http://aka.ms/azrsponsorship

## <a name="next-steps"></a>Sonraki adımlar
- [Sanal makine rezervasyonlarının](billing-ea-portal-vm-reservations.md) paradan tasarruf etmenize nasıl yardımcı olduğu hakkında bilgi edinin.
- Azure EA portalı sorunlarını gidermek için yardıma ihtiyacınız varsa [Azure EA portalı erişim sorunlarını giderme](billing-ea-portal-troubleshoot.md) konusuna bakın.
