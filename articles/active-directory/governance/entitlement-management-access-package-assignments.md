---
title: Azure AD yetkilendirme yönetiminde bir erişim paketi için atamaları görüntüleme, ekleme ve kaldırma - Azure Etkin Dizini
description: Azure Active Directory yetkilendirme yönetiminde bir erişim paketi için atamaları nasıl görüntüleyeceğinizi, ekleyeceğinizi ve kaldırmayı öğrenin.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/15/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: d5a2107974cd63c0d02aaeb555430453c39990bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79262028"
---
# <a name="view-add-and-remove-assignments-for-an-access-package-in-azure-ad-entitlement-management"></a>Azure AD yetkilendirme yönetiminde bir erişim paketi için atamaları görüntüleme, ekleme ve kaldırma

Azure AD yetkilendirme yönetiminde, paketlere erişmek için kimlerin atandığını, ilkelerini ve durumlarını görebilirsiniz. Bir erişim paketinin uygun bir ilkesi varsa, kullanıcıyı doğrudan bir erişim paketine atayabilirsiniz. Bu makalede, erişim paketleri için atamaların nasıl görüntüleneneme, eklenip kaldırılacak olduğu açıklanmaktadır.

## <a name="view-who-has-an-assignment"></a>Kimin ataması olduğunu görüntüleme

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde, **Access paketlerini** tıklatın ve ardından erişim paketini açın.

1. Etkin atamaların listesini görmek için **Atamalar'ı** tıklatın.

    ![Erişim paketine yapılan atamaların listesi](./media/entitlement-management-access-package-assignments/assignments-list.png)

1. Ek ayrıntıları görmek için belirli bir atamayı tıklatın.

1. Tüm kaynak rollerinin düzgün bir şekilde karşılanmasına sahip olmayan atamaların listesini görmek için filtre durumunu tıklatın ve **Teslim'i**seçin.

    Kullanıcının ilgili isteğini **İstekler** sayfasında bularak teslim hatalarıyla ilgili ek ayrıntıları görebilirsiniz.

1. Süresi dolan atamaları görmek için filtre durumunu tıklatın ve **Süresi Dolmuş'u**seçin.

1. Filtre uygulanmış listenin CSV dosyasını indirmek için **İndir'i**tıklatın.

### <a name="viewing-assignments-programmatically"></a>Atamaları programlı olarak görüntüleme

Microsoft Graph'ı kullanarak bir erişim paketinde atamaları da alabilirsiniz.  Temsilci izni `EntitlementManagement.ReadWrite.All` olan bir uygulama ile uygun bir rolde bir kullanıcı [accessPackageAssignments listelemek](https://docs.microsoft.com/graph/api/accesspackageassignment-list?view=graph-rest-beta)için API arayabilirsiniz.

## <a name="directly-assign-a-user"></a>Doğrudan bir kullanıcı atama

Bazı durumlarda, kullanıcıların erişim paketini isteme sürecinden geçmek zorunda kalmaması için belirli kullanıcıları doğrudan bir erişim paketine atamak isteyebilirsiniz. Kullanıcıları doğrudan atamak için, erişim paketinin yöneticinin doğrudan atamalarına izin veren bir ilkesi olmalıdır.

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde, **Access paketlerini** tıklatın ve ardından erişim paketini açın.

1. Sol menüde **Atamalar'ı**tıklatın.

1. Pakete erişmek için Kullanıcı Ekle'yi açmak için **Yeni atama'yı** tıklatın.

    ![Atamalar - Erişim paketine kullanıcı ekleme](./media/entitlement-management-access-package-assignments/assignments-add-user.png)

1. Bu erişim paketini atamak istediğiniz kullanıcıları seçmek için **kullanıcı ekle'yi** tıklatın.

1. **İlke Seç** listesinde, kullanıcıların gelecekteki isteklerinin ve yaşam döngüsünün yönetilip izlenecek bir ilke seçin. Seçili kullanıcıların farklı ilke ayarlarına sahip olmasını istiyorsanız, yeni bir ilke eklemek için **yeni ilke oluştur'u** tıklatabilirsiniz.

1. Seçili kullanıcıların atamasının başlayıp bitmesini istediğiniz tarih ve saati ayarlayın. Bir bitiş tarihi sağlanmazsa, ilkenin yaşam döngüsü ayarları kullanılır.

1. İsteğe bağlı olarak, doğrudan atamanız için kayıt tutma için bir gerekçe sağlayın.

1. Seçilen kullanıcıları doğrudan erişim paketine atamak için **Ekle'yi** tıklatın.

    Birkaç dakika sonra, Atamalar listesindeki kullanıcıları görmek için **Yenile'yi** tıklatın.

### <a name="directly-assigning-users-programmatically"></a>Kullanıcıları doğrudan programlı olarak atama

Microsoft Graph'ı kullanarak bir kullanıcıyı doğrudan bir erişim paketine atayabilirsiniz.  Yetki nettirene `EntitlementManagement.ReadWrite.All` sahip bir uygulamayla uygun bir roldeki bir kullanıcı [accessPackageAssignmentRequest oluşturmak](https://docs.microsoft.com/graph/api/accesspackageassignmentrequest-post?view=graph-rest-beta)için API'yi arayabilir.

## <a name="remove-an-assignment"></a>Atamayı kaldırma

**Önkoşul rolü:** Genel yönetici, Kullanıcı yöneticisi, Katalog sahibi veya Access paket yöneticisi

1. Azure portalında **Azure Etkin Dizin'i** tıklatın ve ardından **Kimlik Yönetimi'ni**tıklatın.

1. Sol menüde, **Access paketlerini** tıklatın ve ardından erişim paketini açın.

1. Sol menüde **Atamalar'ı**tıklatın.
 
1. Atamasını erişim paketinden kaldırmak istediğiniz kullanıcının yanındaki onay kutusunu tıklatın. 

1. Sol bölmenin üst kısmındaki **Kaldır** düğmesini tıklatın. 
 
    ![Atamalar - Kullanıcıyı erişim paketinden kaldırma](./media/entitlement-management-access-package-assignments/remove-assignment-select-remove-assignment.png)

    Atamanın kaldırıldığını bildiren bir bildirim görüntülenir. 

## <a name="next-steps"></a>Sonraki adımlar

- [Erişim paketi için istek ve ayarları değiştirme](entitlement-management-access-package-request-policy.md)
- [Raporları ve günlükleri görüntüleme](entitlement-management-reports.md)
