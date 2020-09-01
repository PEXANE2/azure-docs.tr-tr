---
title: 'Hızlı başlangıç: Portal ile bir yönetim grubu oluşturma'
description: Bu hızlı başlangıçta, kaynaklarınızı bir kaynak hiyerarşisinde düzenlemek üzere bir yönetim grubu oluşturmak için Azure portal kullanırsınız.
ms.date: 08/31/2020
ms.topic: quickstart
ms.openlocfilehash: 28b530e1da6b75dcc20e4a5dfebc494a959040b3
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89237374"
---
# <a name="quickstart-create-a-management-group"></a>Hızlı başlangıç: yönetim grubu oluşturma

Yönetim grupları, birden çok abonelik üzerinde erişimi, ilkeyi ve uyumluluğu yönetmenize yardımcı olan kapsayıcılardır. [Azure ilkesi](../policy/overview.md) ve [Azure rol tabanlı erişim denetimleriyle](../../role-based-access-control/overview.md)kullanılabilecek etkili ve verimli bir hiyerarşi oluşturmak için bu kapsayıcıları oluşturun. Yönetim grupları hakkında daha fazla bilgi için bkz. [Azure Yönetim gruplarıyla kaynaklarınızı düzenleme](overview.md).

Dizinde oluşturulan ilk yönetim grubunun tamamlanması 15 dakika sürebilir. Dizininiz için Azure 'da yönetim grupları hizmetini ayarlamak için ilk kez çalışan süreçler vardır. İşlem tamamlandığında bir bildirim alırsınız. Daha fazla bilgi için bkz. [yönetim gruplarının ilk kurulumu](./overview.md#initial-setup-of-management-groups).

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

- Kiracıdaki tüm Azure AD kullanıcıları, [hiyerarşi koruması](./how-to/protect-resource-hierarchy.md#setting---require-authorization) etkinleştirilmemişse, bu kullanıcıya atanan yönetim grubu yazma izni olmadan bir yönetim grubu oluşturabilir. Bu yeni yönetim grubu kök yönetim grubunun veya [Varsayılan yönetim grubunun](./how-to/protect-resource-hierarchy.md#setting---default-management-group) bir alt öğesi haline gelir ve oluşturucuya "Owner" rol ataması verilir. Yönetim grubu hizmeti, bu becerisine, rol atamalarının kök düzeyinde gerekmemesi için izin verir. Kök yönetim grubuna, bir kullanıcı oluşturulduğunda erişemez. Yönetim grupları 'nı kullanmaya başlamak üzere Azure AD Genel yöneticilerini bulmanın önüne geçmek için, kök düzeyinde ilk yönetim gruplarının oluşturulmasına izin veririz.

### <a name="create-in-portal"></a>Portalda oluştur

1. [Azure Portal](https://portal.azure.com)oturum açın.

1. **Tüm hizmetler**  >  **yönetimi + idare**' ı seçin.

1. **Yönetim grupları**seçin.

1. **+ Yönetim grubu Ekle**' yi seçin.

   :::image type="content" source="./media/main.png" alt-text="Yönetim gruplarıyla çalışma sayfası" border="false":::

1. **Yeni oluştur** ' a seçili bırakın ve YÖNETIM grubu kimliği alanını girin.

   - **Yönetim grubu kimliği** , bu yönetim grubundaki komutları göndermek için kullanılan dizin benzersiz tanımlayıcısıdır. Bu tanımlayıcı, bu grubu tanımlamak için Azure sisteminde kullanıldığından oluşturulduktan sonra düzenlenebilir değildir. [Kök yönetim grubu](./overview.md#root-management-group-for-each-directory) , Azure Active Directory kimliği olan bir kimlikle otomatik olarak oluşturulur. Diğer tüm yönetim grupları için benzersiz bir KIMLIK atayın.
   - Görünen ad alanı Azure portal içinde görüntülenen addır. Ayrı bir görünen ad, yönetim grubu oluşturulurken isteğe bağlı bir alandır ve herhangi bir zamanda değiştirilebilir.

   :::image type="content" source="./media/create_context_menu.png" alt-text="Yeni yönetim grubu oluşturmak için seçenekler bölmesi":::

1. **Kaydet**’i seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturulan yönetim grubunu kaldırmak için şu adımları izleyin:

1. **Tüm hizmetler**  >  **yönetimi + idare**' ı seçin.

1. **Yönetim grupları**seçin.

1. Yukarıda oluşturulan yönetim grubunu bulun, seçin ve ardından adın yanındaki **Ayrıntılar** ' ı seçin.
   Ardından **Sil** ' i seçin ve istemi onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, kaynak hiyerarşinizi düzenlemek için bir yönetim grubu oluşturdunuz. Yönetim grubu, abonelikleri veya diğer yönetim gruplarını içerebilir.

Yönetim grupları ve kaynak hiyerarşinizi yönetme hakkında daha fazla bilgi edinmek için şu çalışmaya devam edin:

> [!div class="nextstepaction"]
> [Kaynakları yönetim gruplarıyla yönetme](./manage.md)