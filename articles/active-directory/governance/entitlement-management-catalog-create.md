---
title: Yetkilendirme yönetiminde kaynak kataloğu oluşturma & yönetin - Azure AD
description: Azure Active Directory yetkilendirme yönetiminde yeni bir kaynak kapsayıcısı ve erişim paketleri oluşturmayı öğrenin.
services: active-directory
documentationCenter: ''
author: barclayn
manager: daveba
editor: HANKI
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 03/22/2020
ms.author: barclayn
ms.reviewer: hanki
ms.collection: M365-identity-device-management
ms.openlocfilehash: ffa6f2fbdcde41d81e268779c3a0586bd8310792
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437547"
---
# <a name="create-and-manage-a-catalog-of-resources-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde bir kaynak kataloğu oluşturma ve yönetme

## <a name="create-a-catalog"></a>Katalog oluşturma

Katalog, kaynak ve erişim paketlerinin bir kapsayıcısIdur. İlgili kaynakları gruplandırmak ve paketlere erişmek istediğinizde bir katalog oluşturursunuz. Kataloğu kim oluşturursa ilk katalog sahibi olur. Katalog sahibi ek katalog sahipleri ekleyebilir.

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi veya Katalog oluşturucu

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde **Kataloglar'ı**tıklatın.

    ![Azure portalında yetkilendirme yönetimi katalogları](./media/entitlement-management-catalog-create/catalogs.png)

1. **Yeni kataloğu**tıklatın.

1. Katalog için benzersiz bir ad girin ve bir açıklama sağlayın.

    Kullanıcılar bu bilgileri bir erişim paketinin ayrıntılarında görür.

1. Bu katalogdaki erişim paketlerinin kullanıcıların oluşturuldukları anda talep edilebilmesini istiyorsanız, **Etkin'den** **Evet'e**ayarlayın.

1. Seçili dış dizinlerde kullanıcıların bu katalogdaki erişim paketlerini isteyebilmelerine izin vermek istiyorsanız, **dış kullanıcılar için Etkin'i** **Evet**olarak ayarlayın.

    ![Yeni katalog bölmesi](./media/entitlement-management-shared/new-catalog.png)

1. Kataloğu oluşturmak için **Oluştur'u** tıklatın.

### <a name="creating-a-catalog-programmatically"></a>Programlı bir katalog oluşturma

Microsoft Graph'ı kullanarak da katalog oluşturabilirsiniz.  Yetki nettirene `EntitlementManagement.ReadWrite.All` sahip bir uygulamayla uygun bir roldeki bir kullanıcı [accessPackageCatalog oluşturmak](https://docs.microsoft.com/graph/api/accesspackagecatalog-post?view=graph-rest-beta)için API'yi arayabilir.

## <a name="add-resources-to-a-catalog"></a>Kataloğuna kaynak ekleme

Kaynakları bir erişim paketine eklemek için kaynakların bir katalogda bulunması gerekir. Ekleyebileceğiniz kaynak türleri gruplar, uygulamalar ve SharePoint Online siteleridir. Gruplar bulut tarafından oluşturulan Office 365 Grupları veya bulut tarafından oluşturulan Azure REKLAM güvenlik grupları olabilir. Uygulamalar, hem SaaS uygulamaları hem de Azure AD'ye aktarılmış kendi uygulamalarınız da dahil olmak üzere Azure AD kurumsal uygulamaları olabilir. Siteler SharePoint Online siteleri veya SharePoint Online site koleksiyonları olabilir.

**Önkoşul rolü:** [Kataloga kaynak eklemek için gerekli rolleri](entitlement-management-delegate.md#required-roles-to-add-resources-to-a-catalog) görme

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde **Kataloglar'ı** tıklatın ve ardından kaynak eklemek istediğiniz kataloğu açın.

1. Sol menüde **Kaynaklar'ı**tıklatın.

1. **Kaynak Ekle'yi**tıklatın.

1. Kaynak türünü tıklatın: **Gruplar ve Takımlar,** **Uygulamalar**veya **SharePoint siteleri.**

    Eklemek istediğiniz bir kaynak görmüyorsanız veya kaynak ekleyemiyorsanız, gerekli Azure REKLAM dizin ideliği rolüne ve yetkilendirme yönetimi rolüne sahip olduğundan emin olun. Gerekli rollere sahip birinin kaynağı kataloğunuza eklemesi gerekebilir. Daha fazla bilgi [için, bir kataloğa kaynak eklemek için Gerekli rollere](entitlement-management-delegate.md#required-roles-to-add-resources-to-a-catalog)bakın.

1. Kataloğuna eklemek istediğiniz türün bir veya daha fazla kaynağını seçin.

    ![Kataloğuna kaynak ekleme](./media/entitlement-management-catalog-create/catalog-add-resources.png)

1. Bittiğinde **Ekle'yi**tıklatın.

    Bu kaynaklar artık katalogdaki erişim paketlerine eklenebilir.

### <a name="adding-a-resource-to-a-catalog-programmatically"></a>Kataloga kaynak ekleme

Microsoft Graph'ı kullanarak bir kataloğuna kaynak da ekleyebilirsiniz.  Uygun bir rolde bir kullanıcı veya bir katalog ve kaynak sahibi, temsilci `EntitlementManagement.ReadWrite.All` izni olan bir uygulama ile bir [accessPackageResourceRequest oluşturmak](https://docs.microsoft.com/graph/api/accesspackageresourcerequest-post?view=graph-rest-beta)için API arayabilirsiniz.

## <a name="remove-resources-from-a-catalog"></a>Kaynakları katalogdan kaldırma

Kaynakları katalogdan kaldırabilirsiniz. Kaynak yalnızca kataloğun erişim paketlerinin hiçbirinde kullanılıyorsa katalogdan kaldırılabilir.

**Önkoşul rolü:** [Kataloga kaynak eklemek için gerekli rolleri](entitlement-management-delegate.md#required-roles-to-add-resources-to-a-catalog) görme

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde **Kataloglar'ı** tıklatın ve ardından kaynakları kaldırmak istediğiniz kataloğu açın.

1. Sol menüde **Kaynaklar'ı**tıklatın.

1. Kaldırmak istediğiniz kaynakları seçin.

1. **Kaldır'ı** tıklatın (veya elipsleri (**...**) tıklatın ve sonra **kaynağı kaldır'ı**tıklatın).

## <a name="add-additional-catalog-owners"></a>Ek katalog sahipleri ekleme

Katalog oluşturan kullanıcı ilk katalog sahibi olur. Bir kataloğun yönetimini temsilci olarak, kullanıcıları katalog sahibi rolüne eklersiniz. Bu, katalog yönetimi sorumluluklarının paylaşılabına yardımcı olur. 

Bir kullanıcıyı katalog sahibi rolüne atamak için aşağıdaki adımları izleyin:

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi veya Katalog sahibi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde **Kataloglar'ı** tıklatın ve ardından yöneticilereklemek istediğiniz kataloğu açın.

1. Sol menüde, **Roller ve yöneticiler'i**tıklatın.

    ![Katalogrolleri ve yöneticileri](./media/entitlement-management-shared/catalog-roles-administrators.png)

1. Bu rollerin üyelerini seçmek için **sahipleri ekle'yi** tıklatın.

1. Bu üyeleri eklemek için **Seç'i** tıklatın.

## <a name="edit-a-catalog"></a>Kataloğu edin

Bir kataloğun adını ve açıklamasını atabilirsiniz. Kullanıcılar bu bilgileri bir erişim paketinin ayrıntılarında görür.

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi veya Katalog sahibi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde **Kataloglar'ı** tıklatın ve ardından yeniden yapmak istediğiniz kataloğu açın.

1. Kataloğun Genel **Bakış** **sayfasında, Edit'i**tıklatın.

1. Kataloğun adını, açıklamasını veya etkin ayarlarını edin.

    ![Katalog ayarlarını değiştir](./media/entitlement-management-shared/catalog-edit.png)

1. **Kaydet**'e tıklayın.

## <a name="delete-a-catalog"></a>Kataloğu silme

Bir kataloğu silebilirsiniz, ancak yalnızca herhangi bir erişim paketi yoksa.

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi veya Katalog sahibi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde **Kataloglar'ı** tıklatın ve ardından silmek istediğiniz kataloğu açın.

1. Kataloğun Genel **Bakışı'nda** **Sil'i**tıklatın.

1. Görünen ileti kutusunda **Evet'i**tıklatın.

### <a name="deleting-a-catalog-programmatically"></a>Kataloğu programlı olarak silme

Microsoft Graph'ı kullanarak bir kataloğu da silebilirsiniz.  Yetki nettirene `EntitlementManagement.ReadWrite.All` sahip bir uygulamayla uygun bir roldeki [bir kullanıcı, accessPackageCatalog'u silmek](https://docs.microsoft.com/graph/api/accesspackagecatalog-delete?view=graph-rest-beta)için API'yi arayabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim paketi yöneticilerine erişim yönetimini temsilci olarak](entitlement-management-delegate-managers.md)
