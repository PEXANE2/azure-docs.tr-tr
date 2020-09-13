---
title: Kullanıcı yönetimi geliştirmeleri (Önizleme)-Azure Active Directory | Microsoft Docs
description: Azure Active Directory, Kullanıcı arama, filtreleme ve kullanıcılarınız hakkında daha fazla bilgiye nasıl izin sağladığını açıklar.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: how-to
ms.date: 09/03/2020
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6e0c8e6fb3bab179483d03320e6d90ab712ec528
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89493387"
---
# <a name="user-management-enhancements-preview-in-azure-active-directory"></a>Azure Active Directory 'de Kullanıcı yönetimi geliştirmeleri (Önizleme)

Bu makalede, Azure Active Directory (Azure AD) portalında geliştirilmiş Kullanıcı yönetimi önizlemenin nasıl kullanılacağı açıklanır. **Tüm kullanıcılar** ve **silinen kullanıcılar** sayfaları daha fazla bilgi sağlamak ve Kullanıcı bulmayı kolaylaştırmak için güncelleştirilmiştir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Önizlemedeki değişiklikler şunları içerir:

- Nesne KIMLIĞI, Dizin eşitleme durumu, oluşturma türü ve kimlik veren gibi daha görünür Kullanıcı Özellikleri
- Arama şimdi ad, e-posta ve nesne kimliklerinin birleştirilmiş aramasına izin veriyor
- Kullanıcı türüne göre Gelişmiş filtreleme (üye ve konuk), Dizin eşitleme durumu ve oluşturma türü

> [!NOTE]
> Bu önizleme Şu anda Azure AD B2C kiracılar için kullanılamaz.

## <a name="find-the-preview"></a>Önizlemeyi bulun

Önizleme varsayılan olarak açıktır, bu sayede hemen kullanılabilir. **Tüm kullanıcılar** sayfasında **Önizleme özellikleri** ' ni seçerek en son özellikleri ve geliştirmeleri kullanıma alabilirsiniz. Bu önizlemenin bir parçası olarak güncelleştirilmiş tüm sayfalar önizleme etiketi görüntüler. Herhangi bir sorun yaşıyorsanız eski deneyimle geri dönebilirsiniz:

1. [Azure AD Yönetim merkezinde](https://aad.portal.azure.com) oturum açın ve **Kullanıcılar**' ı seçin.
1. **Kullanıcılar – tüm kullanıcılar** sayfasında, sayfanın üst kısmındaki başlık ' ı seçin.
1. **Önizleme özellikleri** bölmesinde, **Gelişmiş Kullanıcı yönetimi** ' ni kapatın.

   ![Gelişmiş Kullanıcı yönetiminin nasıl ve ne şekilde kapatılacağı ve kapatılacağı](./media/users-search-enhanced/enable-preview.png)

Deneyimimizi iyileştirebilmemiz için geri bildiriminiz bizim için teşekkür ederiz.

## <a name="more-user-properties"></a>Daha fazla Kullanıcı özelliği

**Tüm kullanıcılar** ve **silinen kullanıcılar** sayfalarında bulunan sütunlarda bazı değişiklikler yaptık. Kullanıcı listenizi yönetmek için sağladığımız mevcut sütunlara ek olarak, birkaç sütun daha ekledik.

### <a name="all-users-page"></a>Tüm kullanıcılar sayfası

Aşağıda, **tüm kullanıcılar** sayfasında görüntülenen kullanıcı özellikleri verilmiştir:

- Ad: kullanıcının görünen adı.
- Kullanıcı asıl adı: kullanıcının Kullanıcı asıl adı (UPN).
- Kullanıcı türü: kullanıcının Kullanıcı türü, üye veya konuk.
- Eşitlenen Dizin: kullanıcının şirket içi dizinden eşitlenip eşitlenmediğini belirtir.
- Kimlik veren: bir kullanıcı hesabında oturum açmak için kullanılan kimlik verenler.
- Nesne KIMLIĞI: kullanıcının nesne KIMLIĞI.
- Oluşturma türü: Kullanıcı hesabının nasıl oluşturulduğunu gösterir.
- Şirket adı: kullanıcının ilişkilendirildiği şirket adı.
- Davet durumu: Konuk Kullanıcı davetinin durumu.
- Posta: kullanıcının e-postası.

   ![Tüm kullanıcılar ve silinen Kullanıcı sayfalarında yeni kullanıcı özellikleri gösteriliyor](./media/users-search-enhanced/user-properties.png)

### <a name="deleted-users-page"></a>Silinen kullanıcılar sayfası

**Silinen kullanıcılar** sayfası, **tüm kullanıcılar** sayfasında kullanılabilir olan tüm sütunları ve bazı ek sütunları içerir, yani:

- Silme tarihi: kullanıcının kuruluştan ilk silindiği tarih (Kullanıcı yeniden geri alındı).
- Kalıcı silme tarihi: kullanıcının kuruluştan kalıcı olarak silindiği tarih.

Bazı sütunlar varsayılan olarak görüntülenir. Diğer sütunları eklemek için sayfada **sütunlar** ' ı seçin, eklemek istediğiniz sütun adlarını seçin ve tercihlerinizi kaydetmek için **Tamam** ' ı seçin.

### <a name="identity-issuers"></a>Kimlik verenler

Herhangi bir kullanıcı için **kimlik veren** sütununda, oturum açma türünü ve veren tarafından atanan kimliği de dahil olmak üzere, veren hakkında ek ayrıntıları görüntülemek için bir giriş seçin. **Kimlik veren** sütunundaki girişler çok değerli olabilir. Kullanıcının kimliğini birden çok veren varsa, **tüm kullanıcılar** ve **silinen kullanıcılar** sayfalarındaki **kimlik veren** sütununda birden çok sözcüğü görürsünüz ve Ayrıntılar bölmesinde tüm verenler listeleyin.

> [!NOTE]
> **Kaynak** sütun, daha ayrıntılı filtreleme için **oluşturma türü**, **eşitlenen Dizin**ve **kimlik veren** gibi birden çok sütunla değiştirilmiştir.

## <a name="user-list-search"></a>Kullanıcı listesi arama

Arama dizesi girdiğinizde, arama "ile başlar" araması kullanır ve artık tek bir aramada adları, e-postaları veya nesne kimliklerini eşleştirebilir. Bu özniteliklerden herhangi birini arama kutusuna girebilirsiniz ve arama, eşleşen sonuçları döndürmek için bu özelliklerden herhangi birini otomatik olarak arar. Aynı aramayı **tüm kullanıcılar** ve **silinen kullanıcılar** sayfalarında gerçekleştirebilirsiniz.

## <a name="user-list-filtering"></a>Kullanıcı listesi filtreleme

Filtreleme özellikleri, **tüm kullanıcılar** ve **silinen kullanıcılar** sayfaları için daha fazla filtreleme seçeneği sunacak şekilde geliştirilmiştir. Artık aynı anda birden fazla özelliğe göre filtre uygulayabilir ve daha fazla özelliğe göre filtre uygulayabilirsiniz.

### <a name="filtering-all-users-list"></a>Tüm kullanıcılar listesi filtreleniyor

Aşağıda, **tüm kullanıcılar** sayfasında filtrelenebilir Özellikler verilmiştir:

- Kullanıcı türü-üye veya Konuk
- Eşitlenmiş Dizin durumu-Evet
- Oluşturma türü-davet, e-posta doğrulandı, yerel hesap
- Davet durumu – kabul bekleniyor, kabul edildi
- Yönetim birimi-görüntülediğiniz kullanıcıların kapsamını tek bir yönetim birimine kısıtlamak için bu seçeneği belirleyin. Daha fazla bilgi için bkz. [yönetim birimleri yönetim önizlemesi](directory-administrative-units.md).

## <a name="filtering-deleted-users-list"></a>Silinen kullanıcılar listesi filtreleniyor

**Silinen kullanıcılar** sayfasında, **tüm kullanıcılar** sayfasında bulunmayan ek filtreler bulunur. **Silinen kullanıcılar** sayfasında filtrelenebilir özellikler aşağıda verilmiştir:

- Kullanıcı türü-üye veya Konuk
- Eşitlenmiş Dizin durumu-Evet
- Oluşturma türü-davet, e-posta doğrulandı, yerel hesap
- Davet durumu – kabul bekleniyor, kabul edildi
- Silme tarihi-son 7, 14 veya 30 gün
- Kalıcı silme tarihi-son 7, 14 veya 30 gün

## <a name="frequently-asked-questions-faq"></a>Sık Sorulan Sorular (SSS)

Soru | Yanıt
-------- | ------
Kullanıcılar ve konuklar için toplu yeteneklere ne olacak? | Toplu işlemler, toplu oluşturma, toplu davet etme, toplu silme ve Kullanıcı indirme dahil olmak üzere kullanıcılar ve konuklar için hala kullanılabilir. Onları **toplu işlemler**adlı bir menüde birleştirdik. **Toplu işlemler** seçeneklerini **tüm kullanıcılar** sayfasının en üstünde bulabilirsiniz.
Kaynak sütununa ne oldu? | **Kaynak** sütunu benzer bilgiler sağlayan diğer sütunlarla değiştirilmiştir ve bu değerleri bağımsız olarak filtrelemenize izin verir. **Oluşturma türü**, **Dizin eşitlenmiş** ve **kimlik veren**örnekleri sayılabilir.
Kullanıcı adı sütununa ne oldu? | **Kullanıcı adı** sütunu hala orada bulunur, ancak **Kullanıcı asıl adı**olarak yeniden adlandırılmıştır. Bu, söz konusu sütunda yer alan bilgileri daha iyi yansıtır. Ayrıca, tam Kullanıcı asıl adının artık B2B konukları için görüntülendiğini fark edersiniz. Bu, MS grafiğinde alacağınız ile eşleşir.  
"Şunu içerir" araması gerçekleştirmem ve "içermez" araması yapmam neden | "İçerir" araması gerçekleştirmenize izin vermemize engel olan bazı sınırlamalar vardır. Geri bildirimleri duyduk, bu nedenle sürekli olarak kalın.
Sütunları neden sıralayamıyorum? | Sütunları sıralamanıza izin vermemize engel olan bazı sınırlamalar vardır. Geri bildirimleri duyduk, bu nedenle sürekli olarak kalın.
**Dizin eşitlenmiş** sütununu yalnızca Evet olarak filtreleyebilirim? | Bu özelliği, hiçbir değere göre filtrelemenize izin vermemize engel olan bazı sınırlamalar vardır. Geri bildirimleri duyduk, bu nedenle sürekli olarak kalın.

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcı işlemleri

- [Profil bilgilerini ekleme veya değiştirme](../fundamentals/active-directory-users-profile-azure-portal.md)
- [Kullanıcı ekleme veya silme](../fundamentals/add-users-azure-active-directory.md)

Toplu işlemler

- [Kullanıcı listesini indir](users-bulk-download.md)
- [Toplu Kullanıcı ekleme](users-bulk-add.md)
- [Kullanıcıları toplu silme](users-bulk-delete.md)
- [Kullanıcıları toplu geri yükleme](users-bulk-restore.md)
