---
title: Tam zamanında erişimi onaylama
description: Azure Yönetilen Uygulamalar tüketicilerinin yönetilen bir uygulamaya tam zamanında erişim isteklerini nasıl onayladığını açıklar.
author: MSEvanhi
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: evanhi
ms.openlocfilehash: 4a3604a3256fb2d3f4253891bbc28c7685748cd2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75651026"
---
# <a name="configure-and-approve-just-in-time-access-for-azure-managed-applications"></a>Azure Yönetilen Uygulamalar için tam zamanında erişimi yapılandırma ve onaylama

Yönetilen bir uygulamanın tüketicisi olarak, yayımcıya yönetilen kaynak grubuna kalıcı erişim sağlamak sizi rahatsız edebilir. Azure Yönetilen Uygulamalar, yönetilen kaynaklara erişim sağlama konusunda size daha fazla denetim sağlamak için, şu anda önizlemede olan tam zamanında (JIT) erişim adı verilen bir özellik sağlar. Yayımcının kaynak grubuna ne zaman ve ne kadar süreyle erişebildiğini onaylamanızı sağlar. Yayımcı bu süre içinde gerekli güncelleştirmeleri yapabilir, ancak bu süre bittiğinde yayımcının erişiminin süresi doluyor.

Erişim izni için iş akışı:

1. Yayımcı, pazara yönetilen bir uygulama ekler ve JIT erişiminin kullanılabilir olduğunu belirtir.

1. Dağıtım sırasında, yönetilen uygulama örneğiniz için JIT erişimini etkinleştirin.

1. Dağıtımdan sonra JIT erişimi ayarlarını değiştirebilirsiniz.

1. Yayımcı erişim için bir istek gönderir.

1. İsteği onaylarsınız.

Bu makalede, tüketicilerin JIT erişimine ve istekleri onaylamak için gerçekleştirdiği eylemler e odaklanMaktadır. Yönetilen bir uygulamayı JIT erişimiyle yayımlama hakkında bilgi edinmek için [Azure Yönetilen Uygulamalar'da tam zamanında erişim isteği'ne](request-just-in-time-access.md)bakın.

> [!NOTE]
> Tam zamanında erişimi kullanmak için [Azure Active Directory P2 lisansınolması](../../active-directory/privileged-identity-management/subscription-requirements.md)gerekir.

## <a name="enable-during-deployment"></a>Dağıtım sırasında etkinleştirme

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. JIT etkinleştirilmiş yönetilen bir uygulama için bir pazar girişi bulun. **Oluştur'u**seçin.

1. **JIT Yapılandırma** adımı, yeni yönetilen uygulama için değerler sağlarken, yönetilen uygulama için JIT erişimini etkinleştirmenize veya devre dışı atmanıza olanak tanır. **JIT Erişimini Etkinleştirmek**için **Evet'i** seçin. Bu seçenek, pazarda Etkinleştirilen JIT ile tanımlanan yönetilen uygulamalar için varsayılan olarak seçilir.

   ![Erişimi yapılandırma](./media/approve-just-in-time-access/configure-jit-access.png)

   Yalnızca dağıtım sırasında JIT erişimini etkinleştirebilirsiniz. **Hayır'ı**seçerseniz, yayımcı yönetilen kaynak grubuna kalıcı erişim elde eve gelir. JIT erişimini daha sonra etkinleştiremezsiniz.

1. Varsayılan onay ayarlarını değiştirmek için **JIT Yapılandırmasını Özelleştir'i**seçin.

   ![Erişimi özelleştir](./media/approve-just-in-time-access/customize-jit-access.png)

   Varsayılan olarak, JIT etkinleştirilmiş yönetilen bir uygulama aşağıdaki ayarları içerir:

   * Onay modu – otomatik
   * Maksimum erişim süresi – 8 saat
   * Onaylayanlar – yok

   Onay modu **otomatik**olarak ayarlandığında, onaylayanlar her istek için bir bildirim alır, ancak istek otomatik olarak onaylanır. **El ile**ayarlandığında, onaylayanlar her istek için bir bildirim alır ve bunlardan birinin bunu onaylaması gerekir.

   Etkinleştirme maksimum süresi, bir yayımcının yönetilen kaynak grubuna erişmek için isteyebileceği maksimum süreyi belirtir.

   Onaylayanlar listesi, JIT erişim isteklerini onaylayabilecek Azure Etkin Dizin kullanıcılarıdır. Onaylayıcı eklemek için **Onaylayıcı Ekle'yi** ve kullanıcıyı arayın'ı seçin.

   Ayarı güncelledikten sonra **Kaydet'i**seçin.

## <a name="update-after-deployment"></a>Dağıtımdan sonra güncelleştir

İsteklerin onaylanma şekline yönelik değerleri değiştirebilirsiniz. Ancak, dağıtım sırasında JIT erişimini etkinleştirmediyseniz, daha sonra etkinleştiremezsiniz.

Dağıtılmış yönetilen bir uygulamanın ayarlarını değiştirmek için:

1. Portalda, uygulamayı yönet'i seçin.

1. **JIT** Yapılandırması'nı seçin ve ayarları gerektiği gibi değiştirin.

   ![Erişim ayarlarını değiştirme](./media/approve-just-in-time-access/change-settings.png)

1. İşiniz bittiğinde **Kaydet**’i seçin.

## <a name="approve-requests"></a>İstekleri onaylama

Yayımcı erişim istediğinde, istek size bildirilir. JIT erişim isteklerini doğrudan yönetilen uygulama aracılığıyla veya Azure AD Ayrıcalıklı Kimlik Yönetimi hizmeti aracılığıyla yönetilen tüm uygulamalarda onaylayabilirsiniz. Tam zamanında erişimi kullanmak için [Azure Active Directory P2 lisansınolması](../../active-directory/privileged-identity-management/subscription-requirements.md)gerekir.

Yönetilen uygulama aracılığıyla istekleri onaylamak için:

1. Yönetilen uygulama için **JIT Access'i** seçin ve **İstekleri Onayla'yı**seçin.

   ![İstekleri onaylama](./media/approve-just-in-time-access/approve-requests.png)
 
1. Onaylamak için isteği seçin.

   ![İstek seçin](./media/approve-just-in-time-access/select-request.png)

1. Formda, onay nedenini belirtin ve **Onayla'yı**seçin.

Azure AD Ayrıcalıklı Kimlik Yönetimi aracılığıyla istekleri onaylamak için:

1. **Tüm hizmetleri** seçin ve Azure AD Ayrıcalıklı **Kimlik Yönetimi'ni**aramaya başlayın. Kullanılabilir seçeneklerarasından seçin.

   ![Hizmet ara](./media/approve-just-in-time-access/search.png)

1. **İstekleri Onayla'yı**seçin.

   ![Onay isteklerini seçme](./media/approve-just-in-time-access/select-approve-requests.png)

1. **Azure yönetilen uygulamaları**seçin ve onay isteği seçin.

   ![İstekleri seçme](./media/approve-just-in-time-access/view-requests.png)

## <a name="next-steps"></a>Sonraki adımlar

Yönetilen bir uygulamayı JIT erişimiyle yayımlama hakkında bilgi edinmek için [Azure Yönetilen Uygulamalar'da tam zamanında erişim isteği'ne](request-just-in-time-access.md)bakın.
