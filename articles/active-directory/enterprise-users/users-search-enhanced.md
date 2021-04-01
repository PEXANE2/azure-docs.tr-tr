---
title: Kullanıcı yönetimi geliştirmeleri (Önizleme)-Azure Active Directory | Microsoft Docs
description: Azure Active Directory, Kullanıcı arama, filtreleme ve kullanıcılarınız hakkında daha fazla bilgiye nasıl izin sağladığını açıklar.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: enterprise-users
ms.workload: identity
ms.topic: how-to
ms.date: 01/11/2020
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5038bde01a6b183a25a47f3b4e206c1ce80e6b6d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98127847"
---
# <a name="user-management-enhancements-preview-in-azure-active-directory"></a>Azure Active Directory 'de Kullanıcı yönetimi geliştirmeleri (Önizleme)

Bu makalede, Azure Active Directory (Azure AD) portalında Kullanıcı yönetimi iyileştirmeleri önizlemesi 'nin nasıl kullanılacağı açıklanır. **Tüm kullanıcılar** ve **silinen kullanıcılar** sayfaları daha fazla bilgi sağlamak ve Kullanıcı bulmayı kolaylaştırmak için güncelleştirilmiştir. Önizlemeler hakkında daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Önizlemedeki değişiklikler şunları içerir:

- Nesne KIMLIĞI, Dizin eşitleme durumu, oluşturma türü ve kimlik veren gibi daha görünür Kullanıcı Özellikleri
- Arama Şimdi, ad, e-posta ve nesne kimliklerinin alt dize aramasına ve birleştirilmiş aramasına izin veriyor
- Kullanıcı türüne göre Gelişmiş filtreleme (üye, Konuk, yok), Dizin eşitleme durumu, oluşturma türü, şirket adı ve etki alanı adı
- Ad ve Kullanıcı asıl adı gibi özelliklerde yeni sıralama özellikleri
- Yeni bir Toplam Kullanıcı sayısı aramalarla veya filtrelerle güncelleştirilir

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
- Kullanıcı türü: üye, Konuk, yok.
- Oluşturma saati: kullanıcının oluşturulduğu tarih ve saat.
- İş unvanı: kullanıcının iş unvanı.
- Departman: kullanıcının içinde çalışması Için departman.
- Eşitlenen Dizin: kullanıcının şirket içi dizinden eşitlenip eşitlenmediğini belirtir.
- Kimlik veren: bir kullanıcı hesabında oturum açmak için kullanılan kimlik verenler.
- Nesne KIMLIĞI: kullanıcının nesne KIMLIĞI.
- Oluşturma türü: Kullanıcı hesabının nasıl oluşturulduğunu gösterir.
- Şirket adı: kullanıcının ilişkilendirildiği şirket adı.
- Davet durumu: Konuk Kullanıcı davetinin durumu.
- Posta: kullanıcının e-postası.
- Son oturum açma: kullanıcının son oturum açmasından geçen tarih. Bu özellik yalnızca denetim günlüklerini okuma izni olan kullanıcılar için görünür (Reporting_ApplicationAuditLogs_Read)

![Tüm kullanıcılar ve silinen Kullanıcı sayfalarında yeni kullanıcı özellikleri gösteriliyor](./media/users-search-enhanced/user-properties.png)

### <a name="deleted-users-page"></a>Silinen kullanıcılar sayfası

**Silinen kullanıcılar** sayfası, **tüm kullanıcılar** sayfasında kullanılabilir olan tüm sütunları ve bazı ek sütunları içerir, yani:

- Silme tarihi: kullanıcının kuruluştan ilk silindiği tarih (Kullanıcı yeniden geri alındı).
- Kalıcı silme tarihi: kullanıcının kuruluştan kalıcı olarak silinme işleminin otomatik olarak başladığı tarih.
- Özgün Kullanıcı asıl adı: nesne KIMLIKLERI, silinen UPN 'lerine önek olarak eklenmeden önce kullanıcının özgün UPN 'si.

> [!NOTE]
> Silme tarihleri Eşgüdümlü Evrensel Saat (UTC) biçiminde görüntülenir.

Bazı sütunlar varsayılan olarak görüntülenir. Diğer sütunları eklemek için sayfada **sütunlar** ' ı seçin, eklemek istediğiniz sütun adlarını seçin ve tercihlerinizi kaydetmek için **Tamam** ' ı seçin.

### <a name="identity-issuers"></a>Kimlik verenler

Herhangi bir kullanıcı için **kimlik veren** sütununda, oturum açma türünü ve veren tarafından atanan kimliği de dahil olmak üzere, veren hakkında ek ayrıntıları görüntülemek için bir giriş seçin. **Kimlik veren** sütunundaki girişler çok değerli olabilir. Kullanıcının kimliğini birden çok veren varsa, **tüm kullanıcılar** ve **silinen kullanıcılar** sayfalarındaki **kimlik veren** sütununda birden çok sözcüğü görürsünüz ve Ayrıntılar bölmesinde tüm verenler listeleyin.

> [!NOTE]
> **Kaynak** sütun, daha ayrıntılı filtreleme için **oluşturma türü**, **eşitlenen Dizin** ve **kimlik veren** gibi birden çok sütunla değiştirilmiştir.

## <a name="user-list-search"></a>Kullanıcı listesi arama

Arama dizesi girdiğinizde, arama artık tek bir aramada adları, e-postaları veya nesne kimliklerini eşleştirmek için "ile başlar" ve alt dize aramasını kullanır. Bu özniteliklerden herhangi birini arama kutusuna girebilirsiniz ve arama tüm bu özellikleri tüm eşleşen sonuçları döndürmek için otomatik olarak arar. Alt dize araması yalnızca tüm sözcükler üzerinde gerçekleştirilir. Aynı aramayı **tüm kullanıcılar** ve **silinen kullanıcılar** sayfalarında gerçekleştirebilirsiniz.

## <a name="user-list-filtering"></a>Kullanıcı listesi filtreleme

Filtreleme özellikleri, **tüm kullanıcılar** ve **silinen kullanıcılar** sayfaları için daha fazla filtreleme seçeneği sunacak şekilde geliştirilmiştir. Artık aynı anda birden fazla özelliğe göre filtre uygulayabilir ve daha fazla özelliğe göre filtre uygulayabilirsiniz.

### <a name="filtering-all-users-list"></a>Tüm kullanıcılar listesi filtreleniyor

Aşağıda, **tüm kullanıcılar** sayfasında filtrelenebilir Özellikler verilmiştir:

- Kullanıcı türü: üye, Konuk, yok
- Dizin eşitleme durumu: Evet, Hayır
- Oluşturma türü: davet, e-posta doğrulandı, yerel hesap
- Oluşturma zamanı: son 7, 14, 30, 90, 360 veya >360 gün önce
- İş unvanı: iş unvanı girin
- Departman: bir departman adı girin
- Grup: bir grup arayın
- Davet durumu – kabul bekleniyor, kabul edildi
- Etki alanı adı: bir etki alanı adı girin
- Şirket adı: bir şirket adı girin
- Yönetim birimi: görüntülediğiniz kullanıcıların kapsamını tek bir yönetim birimine kısıtlamak için bu seçeneği belirleyin. Daha fazla bilgi için bkz. [yönetim birimleri yönetim önizlemesi](../roles/administrative-units.md).

### <a name="filtering-deleted-users-list"></a>Silinen kullanıcılar listesi filtreleniyor

**Silinen kullanıcılar** sayfasında, **tüm kullanıcılar** sayfasında bulunmayan ek filtreler bulunur. **Silinen kullanıcılar** sayfasında filtrelenebilir özellikler aşağıda verilmiştir:

- Kullanıcı türü: üye, Konuk, yok
- Dizin eşitleme durumu: Evet, Hayır
- Oluşturma türü: davet, e-posta doğrulandı, yerel hesap
- Oluşturma zamanı: son 7, 14, 30, 90, 360 veya > 360 gün önce
- İş unvanı: iş unvanı girin
- Departman: bir departman adı girin
- Davet durumu: kabul bekleniyor, kabul edildi
- Silme tarihi: son 7, 14 veya 30 gün
- Etki alanı adı: bir etki alanı adı girin
- Şirket adı: bir şirket adı girin
- Kalıcı silme tarihi: son 7, 14 veya 30 gün

## <a name="user-list-sorting"></a>Kullanıcı listesi sıralaması

Artık **tüm kullanıcılar** ve **silinen kullanıcılar** sayfalarındaki ada ve Kullanıcı asıl adına göre sıralama yapabilirsiniz. Ayrıca, **silinen kullanıcılar** listesindeki silme tarihine göre de sıralayabilirsiniz.

## <a name="user-list-counts"></a>Kullanıcı listesi sayıları

**Tüm** kullanıcılar ve **silinen kullanıcılar** sayfalarındaki Toplam Kullanıcı sayısını görüntüleyebilirsiniz. Listeleri ararken veya filtrelerken, sayı, bulunan Toplam Kullanıcı sayısını yansıtacak şekilde güncelleştirilir.

![Tüm Kullanıcılar sayfasında kullanıcı listesi sayılarının gösterimi](./media/users-search-enhanced/user-list-sorting.png)

## <a name="frequently-asked-questions-faq"></a>Sık Sorulan Sorular (SSS)

Soru | Yanıt
-------- | ------
Kalıcı silme tarihi geçtiğinde silinen kullanıcı neden hala görüntüleniyor? | Kalıcı silme tarihi UTC saat diliminde görüntülenir, bu nedenle bu durum geçerli saat diliminize eşleşmeyebilir. Ayrıca, bu tarih, kullanıcının kuruluştan kalıcı olarak silineceği en erken tarihtir, bu yüzden hala işleme devam edebilir. Kalıcı olarak silinen kullanıcılar otomatik olarak listeden kaldırılacak.
Kullanıcılar ve konuklar için toplu yeteneklere ne olacak? | Toplu işlemler, toplu oluşturma, toplu davet etme, toplu silme ve Kullanıcı indirme dahil olmak üzere kullanıcılar ve konuklar için hala kullanılabilir. Onları **toplu işlemler** adlı bir menüde birleştirdik. **Toplu işlemler** seçeneklerini **tüm kullanıcılar** sayfasının en üstünde bulabilirsiniz.
Kaynak sütununa ne oldu? | **Kaynak** sütunu benzer bilgiler sağlayan diğer sütunlarla değiştirilmiştir ve bu değerleri bağımsız olarak filtrelemenize izin verir. **Oluşturma türü**, **Dizin eşitlenmiş** ve **kimlik veren** örnekleri sayılabilir.
Kullanıcı adı sütununa ne oldu? | **Kullanıcı adı** sütunu hala orada bulunur, ancak **Kullanıcı asıl adı** olarak yeniden adlandırılmıştır. Bu, söz konusu sütunda yer alan bilgileri daha iyi yansıtır. Ayrıca, tam Kullanıcı asıl adının artık B2B konukları için görüntülendiğini fark edersiniz. Bu, MS grafiğinde alacağınız ile eşleşir.  

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcı işlemleri

- [Profil bilgilerini ekleme veya değiştirme](../fundamentals/active-directory-users-profile-azure-portal.md)
- [Kullanıcı ekleme veya silme](../fundamentals/add-users-azure-active-directory.md)

Toplu işlemler

- [Kullanıcı listesini indir](users-bulk-download.md)
- [Toplu Kullanıcı ekleme](users-bulk-add.md)
- [Kullanıcıları toplu silme](users-bulk-delete.md)
- [Kullanıcıları toplu geri yükleme](users-bulk-restore.md)
