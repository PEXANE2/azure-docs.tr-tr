---
title: Azure AD Galeri uygulaması için yanlış kullanıcı kümesi sağlandı
description: Farklı bir kullanıcı kümesinin neden beklediğiniz bir uygulamaya sağlandığını öğrenin
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 77e8f31bd0adeabea79cd11553fd77a3e5ab6404
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74275767"
---
# <a name="wrong-set-of-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a>Azure AD Galeri uygulamasına yanlış kullanıcı kümesi sağlanıyor

Uygulamaya hangi kullanıcıların sağlanacağı, öncelikle uygulamaya **atanan** kullanıcılar ve gruplar tarafından çalıştırılır.

Azure Active Directory içindeki bir uygulamaya hangi kullanıcıların ve grupların atandığını nasıl denetleyeceğinizi öğrenmek için aşağıdaki kaynakları kullanın.

## <a name="assign-a-user-directly-as-an-administrator"></a>Kullanıcıyı doğrudan yönetici olarak atama

Bir uygulamaya doğrudan bir veya daha fazla kullanıcı atamak için şu adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Kullanıcılar ve gruplar** ' a tıklayın.

8. **Atama Ekle** bölmesini açmak için **Kullanıcılar ve gruplar** listesinin üstündeki **Ekle** düğmesine tıklayın.

9. tıklayın **kullanıcılar ve gruplar** seçiciden **atama Ekle** bölmesi.

10. Yazın **tam adı** veya **e-posta adresi** içine atama isteyen kullanıcının **adına veya e-posta adresine göre arama** arama kutusu.

11. Üzerine **kullanıcı** göstermek için listedeki bir **onay kutusu**. Kullanıcının profil fotoğrafı veya kullanıcı için eklenecek logosu yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** isteyip istemediğini **birden fazla kullanıcı eklemek**, başka bir tür **tam adı** veya **e-posta adresi** içine **adına göre arama veya e-posta adresi** arama kutusuna ve bu kullanıcıyı eklemek için onay kutusunu **seçili** listesi.

13. Kullanıcı seçme işlemini tamamladığınızda, tıklayın **seçin** uygulamaya atanan kullanıcıların ve grupların listesi eklemek için düğme.

14. **İsteğe bağlı:** tıklayın **rolü Seç** seçicide **atama Ekle** bölmesinde seçtiğiniz kullanıcılara atamak için bir rol seçin.

15. Tıklayın **atama** düğmesi Seçilen kullanıcılara uygulamayı atamak için.

Sağlama yapılandırıldıysa ve bir uygulama için zaten çalışıyorsa, yeni kullanıcıların bir uygulama için yaklaşık 10 dakika içinde sağlanması gerekir. Ayrıntılar için **Denetim günlüklerine** bakın.

## <a name="assign-a-group-directly-to-an-application-as-an-administrator"></a>Bir grubu doğrudan bir uygulamaya yönetici olarak atama

Bir uygulamaya doğrudan bir veya daha fazla grup atamak için şu adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Kullanıcılar ve gruplar** ' a tıklayın.

8. **Atama Ekle** bölmesini açmak için **Kullanıcılar ve gruplar** listesinin üstündeki **Ekle** düğmesine tıklayın.

9. tıklayın **kullanıcılar ve gruplar** seçiciden **atama Ekle** bölmesi.

10. Yazın **tam grup adı** içine atama ilgilenen grubunun **adına veya e-posta adresine göre arama** arama kutusu.

11. Üzerine **grubu** göstermek için listedeki bir **onay kutusu**. Grubun profil fotoğrafı veya kullanıcı için eklenecek logosu yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** isteyip istemediğini **birden fazla Grup Ekle**, başka bir tür **tam grup adı** içine **adına veya e-posta adresine göre arama** arama kutusu ve Bu gruba eklemek için onay kutusunu **seçili** listesi.

13. Grupları seçme işiniz bittiğinde, tıklayın **seçin** uygulamaya atanan kullanıcıların ve grupların listesi eklemek için düğme.

14. **İsteğe bağlı:** tıklayın **rolü Seç** seçicide **atama Ekle** bölmesinde seçtiğiniz gruplara atamak için bir rol seçin.

15. Tıklayın **atama** düğmesi seçili gruplara uygulama atama.

Sağlama yapılandırıldıysa ve bir uygulama için zaten çalışıyorsa, Grup içinde yer alan yeni kullanıcıların, yaklaşık 10 dakika içinde bir uygulamaya sağlanması gerekir. Ayrıntılar için **Denetim günlüklerine** bakın.

>[!IMPORTANT]
>Bazı uygulamalarda destekleniyorsa, üyelere ek olarak grup adı ve grup ayrıntılarının sağlanması. **Sağlama** sekmesinde gösterilen Grup nesneleri **eşlemesini** etkinleştirerek veya devre dışı bırakarak bu işlevselliği etkinleştirebilir veya devre dışı bırakabilirsiniz. 
>
>

Sağlama grupları etkinleştirilmişse, "eşleşen KIMLIK" için uygun bir alanın kullanıldığından emin olmak üzere öznitelik eşlemelerini gözden geçirdiğinizden emin olun. Bu eşleşen KIMLIK görünen ad veya e-posta diğer adı olabilir. Eşleşen özellik boşsa veya Azure AD 'de bir grup için doldurulmamışsa, Grup ve üyeleri sağlanmadı.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile SaaS Uygulamalarına Kullanıcı Hazırlama ve Sağlamayı Kaldırma İşlemlerini Otomatik Hale Getirme](user-provisioning.md)
