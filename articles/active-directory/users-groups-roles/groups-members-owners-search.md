---
title: Üyeleri ve sahipleri arama ve filtreleme (önizleme) - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure portalında üyeleri ve sahipleri arayın ve filtreleyin.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 02/28/2020
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a815446b79b3e5ec0a75e5d179953956643b16c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78206121"
---
# <a name="search-groups-and-members-preview-in-azure-active-directory"></a>Azure Etkin Dizini'nde arama grupları ve üyeler (önizleme)

Bu makalede, bir grubun üyeleri ve sahiplerini nasıl arayacağınız ve Azure Etkin Dizin (Azure AD) portalındaki grup geliştirme önizlemesinin bir parçası olarak arama filtrelerini nasıl kullanacağınız anlatılıyor. Grup deneyimlerinde, üyeler ve sahipler de dahil olmak üzere gruplarınızı hızlı ve kolay bir şekilde yönetmenize yardımcı olacak birçok iyileştirme vardır. Önizlemeler hakkında daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Bu önizlemedeki değişiklikler şunlardır:

- Grup adlarında alt dize arama gibi yeni gruplar arama yetenekleri
- Üye ve sahip listelerinde yeni filtreleme ve sıralama seçenekleri
- Üye ve sahip listeleri için yeni arama özellikleri
- Büyük gruplar için daha doğru grup sayıları

## <a name="enabling-and-managing-the-preview"></a>Önizlemeyi etkinleştirme ve yönetme

Önizlemeye katılmayı kolaylaştırdık:

  1. Azure AD [portalında](https://portal.azure.com)oturum açın ve **Gruplar'ı**seçin.
  2. Gruplar - Tüm gruplar sayfasından, önizlemeye katılmak için sayfanın üst kısmındaki banner'ı seçin.

**Ayrıca, Tüm gruplar** sayfasında **Önizleme bilgilerini** seçerek en son özellikleri ve iyileştirmeleri de inceleyebilirsiniz. Önizlemeye katıldıktan sonra, geliştirmeleri olan ve önizlemenin bir parçası olan tüm grup sayfalarında önizleme etiketini görebilirsiniz. Her grup sayfası bu önizlemenin bir parçası olarak güncelleştirilemedi.

Herhangi bir sorun yaşıyorsanız, **Tüm gruplar** sayfasının üst kısmındaki banner'ı seçerek eski deneyimi geri alabilirsiniz. Deneyimimizi geliştirebilmemiz için geri bildirimleriniz için teşekkür ederiz.

## <a name="group-search-and-sorting"></a>Grup arama ve sıralama

Grup listesi araması, bir arama dizesini girebildiğinizde, aramanın grup adları listesinde otomatik olarak bir `startswith` ve alt dize araması gerçekleştirmesi için geliştirilmiştir. Alt string arama yalnızca tam sözcüklerle gerçekleştirilir ve özel karakterler içermez. Substring arama büyük/küçük harf duyarlıdır.

![Tüm Gruplar sayfasında yeni alt dize aramaları](./media/groups-members-owners-search/groups-search-preview.png)

Örneğin, "ilke" için bir arama şimdi hem "MDM ilke - Batı" ve "İlke grubu döndürecek." "New_policy" adlı bir grup iade edilmez.

- Aynı aramayı grup üyelik listelerinde de gerçekleştirebilirsiniz.
- Şimdi, listeyi artan veya azalan sırada sıralamak için ad sütununun sağındaki okları kullanarak gruplar listesini ada göre sıralayabilirsiniz.

## <a name="group-member-search-and-filtering"></a>Grup üyesi arama ve filtreleme

### <a name="search-group-member-and-owner-lists"></a>Arama grubu üyesi ve sahip listeleri

Artık belirli bir grubun üyelerini ada göre arayabilir ve aynı aramayı grubun sahiplerinin listesinde de gerçekleştirebilirsiniz. Yeni deneyimde, arama kutusuna bir dize girerseniz, arama yla başlayan lar otomatik olarak gerçekleştirilir. Örneğin, "Scott" için bir arama Scott Wilkinson dönecektir.

![grup üyeleri ve sahipler listelerinde yeni alt dize aramaları](./media/groups-members-owners-search/members-list.png)

### <a name="filter-member-and-owners-list"></a>Üye ve sahipler listesini filtreleyin

Aramaya ek olarak, artık üye ve sahip listelerini kullanıcı türüne göre filtreleyebilirsiniz. Bu, listenin Kullanıcı Türü sütununda bulunan bilgilerdir. Bu nedenle, grupta konuk olup olmadığını belirlemek için listeyi üyelere ve misafirlere göre filtreleyebilirsiniz.

### <a name="view-and-manage-membership"></a>Üyeliği görüntüleme ve yönetme

Belirli bir grubun doğrudan üyelerini görüntülemenin yanı sıra, artık Üyeler sayfasındaki grubun tüm üyelerinin listesini görüntüleyebilirsiniz. Üye listesi, geçişli üyeler de dahil olmak üzere grubun tüm benzersiz üyelerini içerir.

Ayrıca, doğrudan üye listesini ve tüm üyeler listesini tek tek arayabilir ve filtreleyebilirsiniz. Tüm üye listesinin filtrelanması, doğrudan üyeler listesinde uygulanan filtreleri etkilemez.

## <a name="improved-group-member-counts"></a>Geliştirilmiş grup üye sayıları

Her boyuttaki gruplar için grup üye sayıları sağlamak için gruba **Genel Bakış** sayfasını geliştirdik. 1.000'den fazla üyesi olan gruplar için bile üye sayısının olduğunu görebilirsiniz. Artık bir grubun toplam doğrudan üye sayısını ve toplam üyelik sayısını (geçişli üyeler de dahil olmak üzere grubun tüm benzersiz üyeleri) **Genel Bakış** sayfasında görebilirsiniz.

![Grup üyelik sayılarında daha yüksek doğruluk](./media/groups-members-owners-search/member-numbers.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makaleler, Azure AD'deki gruplarla çalışma hakkında ek bilgiler sağlar.

- [Grupları ve üyeleri görüntüleme](../fundamentals/active-directory-groups-view-azure-portal.md)
- [Grup üyeliğini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
- [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](groups-create-rule.md)
- [Grup ayarlarınızı düzenleme](../fundamentals/active-directory-groups-settings-azure-portal.md)
- [Grupları kullanarak kaynaklara erişimi yönetme](../fundamentals/active-directory-manage-groups.md)
- [Grupları kullanarak SaaS uygulamalarına erişimi yönetme](groups-saasapps.md)
- [PowerShell komutlarını kullanarak grupları yönetme](groups-settings-v2-cmdlets.md)
- [Azure Etkin Dizin'e Azure aboneliği ekleme](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
