---
title: Self Servis uygulama erişimini kullanma | Microsoft Docs
description: Kullanıcıların kendi uygulamalarını bulmasına izin vermek için self servis uygulama erişimini etkinleştirme
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 07/11/2017
ms.author: kenwith
ms.reviewer: japere,asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 29ff3ca98c0fa5929a5e1c0ad5678222253130b6
ms.sourcegitcommit: 628be49d29421a638c8a479452d78ba1c9f7c8e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88641798"
---
# <a name="how-to-use-self-service-application-access"></a>Self Servis uygulama erişimini kullanma

Kullanıcılarınızın uygulamalarımın uygulamalarını kendi kendine bulabilmesi için, kullanıcıların kendi kendine bulmasını ve erişim istemesini sağlamak istediğiniz uygulamalara **self servis uygulama erişimini** etkinleştirmeniz gerekir.

Bu özellik, bir BT grubu olarak zaman ve para tasarrufu yapmanız için harika bir yoldur ve Azure Active Directory ile modern uygulamalar dağıtımının bir parçası olarak önemle önerilir.

Bu özelliği kullanarak şunları yapabilirsiniz:

-   Kullanıcıların [uygulama erişim panelinden](https://myapps.microsoft.com/) bothering BT grubu olmadan uygulamaları kendi kendine bulmasına izin verin.

-   Bu kullanıcıları önceden yapılandırılmış bir gruba ekleyerek kimin erişimi olduğunu, erişimi kaldırabileceğini görebilir ve bunlara atanan rolleri yönetebilirsiniz.

-   İsteğe bağlı olarak, bir iş onaylayanın uygulama erişim isteklerini onaylaması için BT grubunun olmaması gerekir.

-   İsteğe bağlı olarak, bu uygulamaya erişimi onaylayabilecek en fazla 10 kişi yapılandırın.

-   İsteğe bağlı olarak, bir iş onaylayanın, kullanıcıların uygulamada oturum açmak için kullanabileceği parolaları ayarlamaya, iş onaylayanın [uygulama erişim panelinden](https://myapps.microsoft.com/)doğrudan izin verin.

-   İsteğe bağlı olarak otomatik olarak self servis atanan kullanıcıları doğrudan bir uygulama rolüne atar.

## <a name="enable-self-service-application-access-to-allow-users-to-find-their-own-applications"></a>Kullanıcıların kendi uygulamalarını bulmasına izin vermek için self servis uygulama erişimini etkinleştirme

Self Servis uygulama erişimi, kullanıcıların uygulamaları kendi kendine bulmasına izin vermek için harika bir yoldur ve isteğe bağlı olarak iş grubunun bu uygulamalara erişimi onaylamasını sağlar. İş grubunun, erişim panellerinden, parola çoklu oturum açma uygulamalarında bu kullanıcılara atanan kimlik bilgilerini yönetmesine izin verebilirsiniz.

Bir uygulamaya Self Servis uygulama erişimini etkinleştirmek için aşağıdaki adımları izleyin:

1. [**Azure portalını**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2. Ana sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

   * Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6. Self servis erişimini listeden etkinleştirmek istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Self-Service** ' e tıklayın.

8. Bu uygulama için self servis uygulama erişimini etkinleştirmek üzere **kullanıcıların bu uygulamaya erişim Istemesine Izin ver** ' i açın **. Evet** ' e geçin.

9. Ardından, bu uygulamaya erişim isteyen kullanıcıların eklenmesi gereken grubu seçmek için, **grubun atanması** gereken etiketin yanındaki seçiciye tıklayın ve bir grup seçin.

10. **Isteğe bağlı:** Kullanıcıların erişimine izin verilmeden önce bir iş onayı gerektirmek istiyorsanız, **Bu uygulamaya erişim izni vermeden önce onay iste** seçeneğini belirleyin. **Evet**' e geçin.

11. **Isteğe bağlı: yalnızca parola çoklu oturum açma kullanan uygulamalar için,** bu iş onaylayanlara onaylanan kullanıcılar için bu uygulamaya gönderilen parolaları belirtmesini sağlamak istiyorsanız, **onaylayanlara bu uygulama için Kullanıcı parolalarını ayarlamaya izin ver** ' i ayarlayın. **Evet**' e geçin.

12. **Isteğe bağlı:** Bu uygulamaya erişimi onaylamaya izin verilen iş onaylayanlarını belirtmek için, **Bu uygulamaya erişimi onaylamaya izin verilen** etiketin yanındaki seçiciye tıklayın. en fazla 10 bireysel iş onaylayı seçmek için.

    * Gruplar desteklenmiyor.

13. **Isteğe bağlı:** **rolleri kullanıma sunan uygulamalar için**, bir role self servis onaylı kullanıcılar atamak istiyorsanız, **Bu uygulamada hangi rolün hangi rolün atanması gerektiğini seçin?** seçeneğinin yanındaki seçiciye tıklayın. bu kullanıcıların atanması gereken rolü seçin.

14. Son olarak dikey pencerenin en üstündeki **Kaydet** düğmesine tıklayın.

Self Servis uygulama yapılandırmasını tamamladıktan sonra, kullanıcılar kendi [uygulama erişim paneline](https://myapps.microsoft.com/) gidebilir ve self servis erişimini etkinleştirdiğiniz uygulamaları bulmak Için **+ Ekle** düğmesine tıklamanıza izin verebilir. İş onaylayanları Ayrıca [uygulama erişimi panelinde](https://myapps.microsoft.com/)bir bildirim görür. Bir Kullanıcı onay gerektiren bir uygulamaya erişim isteğinde bulunduğunda bu e-postayı bildiren bir e-posta sağlayabilirsiniz. 

Bu onaylar yalnızca tek onay iş akışlarını destekler, yani birden çok onaylayan belirtirseniz, tek bir onaylayanın uygulamaya erişimi onaylayabileceği anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar
[Self servis grup yönetimi için Azure Active Directory'yi ayarlama](../users-groups-roles/groups-self-service-management.md)
