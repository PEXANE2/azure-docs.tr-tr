---
title: Azure AD Yetkilendirme Yönetimi 'nde Katalog oluşturma ve yönetme (Önizleme)-Azure Active Directory
description: Azure Active Directory yetkilendirme yönetimi 'nde (Önizleme) yeni bir kaynak kapsayıcısı oluşturma ve paketlerin erişim hakkında bilgi edinin.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: HANKI
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 07/23/2019
ms.author: ajburnle
ms.reviewer: hanki
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0e563d86abe3817e4c77cc0d5c8df928e41563f8
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489081"
---
# <a name="create-and-manage-a-catalog-in-azure-ad-entitlement-management-preview"></a>Azure AD Yetkilendirme Yönetimi 'nde Katalog oluşturma ve yönetme (Önizleme)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) yetkilendirme yönetimi şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="create-a-catalog"></a>Katalog oluşturma

Katalog, kaynak ve erişim paketlerinin bir kapsayıcısıdır. İlgili kaynakları ve erişim paketlerini gruplamak istediğinizde bir katalog oluşturursunuz. Her koşulda Katalog, ilk katalog sahibi olur. Katalog sahibi, ek Katalog sahipleri ekleyebilir.

**Önkoşul rolü:** Kullanıcı Yöneticisi veya katalog Oluşturucu

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol taraftaki menüden **kataloglar**' a tıklayın.

    ![Azure portal 'de Yetkilendirme Yönetimi katalogları](./media/entitlement-management-catalog-create/catalogs.png)

1. **Yeni Katalog**' a tıklayın.

1. Katalog için benzersiz bir ad girin ve bir açıklama sağlayın.

    Kullanıcılar bu bilgileri bir erişim paketinin ayrıntılarında görür.

1. Bu katalogdaki erişim paketlerinin kullanıcıların, oluşturulduktan hemen sonra istemesi için kullanılabilir olmasını istiyorsanız, **etkin** ' i **Evet**olarak ayarlayın.

1. Seçili dış dizinlerde bulunan kullanıcıların bu katalogda erişim paketleri isteyebilmesini sağlamak istiyorsanız, **dış kullanıcılar Için etkin** ayarını **Evet**olarak belirleyin.

    ![Yeni Katalog bölmesi](./media/entitlement-management-catalog-create/new-catalog.png)

1. Kataloğu oluşturmak için **Oluştur** ' a tıklayın.

## <a name="add-resources-to-a-catalog"></a>Kataloğa kaynak ekleme

Kaynakları bir erişim paketine dahil etmek için, kaynakların bir katalogda mevcut olması gerekir. Ekleyebileceğiniz kaynak türleri gruplar, uygulamalar ve SharePoint Online siteleridir. Gruplar, bulutta oluşturulmuş Office 365 grupları veya bulutta oluşturulmuş Azure AD güvenlik grupları olabilir. Uygulamalar hem SaaS uygulamaları hem de Azure AD 'ye federe olan uygulamalar dahil olmak üzere Azure AD kurumsal uygulamaları olabilir. Siteler SharePoint Online siteleri veya SharePoint Online site koleksiyonları olabilir.

**Önkoşul rolü:** [Kataloğa kaynak eklemek Için gerekli rollere](entitlement-management-delegate.md#required-roles-to-add-resources-to-a-catalog) bakın

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol taraftaki menüden **kataloglar** ' a tıklayın ve ardından kaynak eklemek istediğiniz kataloğu açın.

1. Sol taraftaki menüden **kaynaklar**' a tıklayın.

1. **Kaynak Ekle**' ye tıklayın.

1. Kaynak türüne tıklayın: **Gruplar**, **uygulamalar**veya **SharePoint siteleri**.

    Eklemek istediğiniz bir kaynağı görmüyorsanız veya kaynak ekleyemezseniz, gerekli Azure AD dizin rolüne ve Yetkilendirme Yönetimi rolüne sahip olduğunuzdan emin olun. Gerekli rollere sahip bir kişiye kaynağı kataloğunuza eklemeniz gerekebilir. Daha fazla bilgi için bkz. [bir kataloğa kaynak eklemek Için gerekli roller](entitlement-management-delegate.md#required-roles-to-add-resources-to-a-catalog).

1. Kataloğa eklemek istediğiniz bir veya daha fazla kaynak türünü seçin.

1. İşiniz bittiğinde **Ekle**' ye tıklayın.

    Bu kaynaklar artık kataloğun içindeki erişim paketlerine dahil edilebilir.

## <a name="remove-resources-from-a-catalog"></a>Katalogdan kaynakları kaldırma

Bir katalogdan kaynakları kaldırabilirsiniz. Bir kaynak, kataloğun erişim paketlerinde kullanılmıyorsa bir katalogdan kaldırılabilir,.

**Önkoşul rolü:** [Kataloğa kaynak eklemek Için gerekli rollere](entitlement-management-delegate.md#required-roles-to-add-resources-to-a-catalog) bakın

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol taraftaki menüden **kataloglar** ' a tıklayın ve kaynakları kaldırmak istediğiniz kataloğu açın.

1. Sol taraftaki menüden **kaynaklar**' a tıklayın.

1. Kaldırmak istediğiniz kaynakları seçin.

1. **Kaldır** ' a tıklayın (veya üç nokta ( **...** ) simgesine tıklayın ve ardından **kaynağı kaldır**' a tıklayın.

## <a name="edit-a-catalog"></a>Katalog düzenleme

Bir kataloğun adını ve açıklamasını düzenleyebilirsiniz. Kullanıcılar bu bilgileri bir erişim paketinin ayrıntılarında görür.

**Önkoşul rolü:** Kullanıcı Yöneticisi veya katalog sahibi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol taraftaki menüden **kataloglar** ' a tıklayın ve ardından düzenlemek istediğiniz kataloğu açın.

1. Kataloğun **genel bakış** sayfasında, **Düzenle**' ye tıklayın.

1. Kataloğun adını veya açıklamasını düzenleyin.

1. **Kaydet**’e tıklayın.

## <a name="delete-a-catalog"></a>Katalog silme

Bir kataloğu yalnızca herhangi bir erişim paketi yoksa silebilirsiniz.

**Önkoşul rolü:** Kullanıcı Yöneticisi veya katalog sahibi

1. Azure portal, **Azure Active Directory** ' a ve ardından **kimlik**Yönetimi ' ne tıklayın.

1. Sol taraftaki menüden **kataloglar** ' a tıklayın ve ardından silmek istediğiniz kataloğu açın.

1. Kataloğun **genel bakış**sayfasında **Sil**' e tıklayın.

1. Görüntülenen ileti kutusunda **Evet**' e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Katalog Oluşturucu Ekleme](entitlement-management-delegate.md#add-a-catalog-creator)
- [Erişim paketi oluşturma ve yönetme](entitlement-management-access-package-create.md)
