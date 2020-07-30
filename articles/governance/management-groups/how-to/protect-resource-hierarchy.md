---
title: Kaynak hiyerarşinizi koruma-Azure Idare
description: Kaynak hiyerarşinizi, varsayılan yönetim grubunu ayarlamayı içeren hiyerarşi ayarlarıyla nasıl koruyacağınızı öğrenin.
ms.date: 05/21/2020
ms.topic: conceptual
ms.openlocfilehash: cdaad59d136e89c595a6a42a9760c73523a977c0
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87422836"
---
# <a name="how-to-protect-your-resource-hierarchy"></a>Kaynak hiyerarşinizi koruma

Kaynaklarınız, kaynak gruplarınız, abonelikleriniz, yönetim gruplarınız ve kiracınız, kaynak hiyerarşinizi topluca yapar. Azure özel rolleri veya Azure Ilke ilkesi atamaları gibi kök yönetim grubundaki ayarlar, kaynak hiyerarşinizdeki her kaynağı etkileyebilir. Kaynak hiyerarşisinin tüm kaynakları olumsuz yönde etkileyebilecek değişikliklerden korunması önemlidir.

Yönetim gruplarında artık, kiracı yöneticisinin bu davranışları denetlemesini sağlayan hiyerarşi ayarları vardır. Bu makale, kullanılabilir hiyerarşi ayarlarının her birini ve bunların nasıl ayarlanacağını ele alır.

## <a name="rbac-permissions-for-hierarchy-settings"></a>Hiyerarşi ayarları için RBAC izinleri

Hiyerarşi ayarlarından herhangi birini yapılandırmak kök yönetim grubunda aşağıdaki iki RBAC işlemini gerektirir:

- `Microsoft.Management/managementgroups/settings/write`
- `Microsoft.Management/managementgroups/settings/read`

Bu işlemler yalnızca bir kullanıcının hiyerarşi ayarlarını okumasına ve güncelleştirmesine izin verir. İşlemler, hiyerarşideki yönetim grubu hiyerarşisine veya kaynaklarına başka bir erişim sağlamaz. Bu işlemlerin her ikisi de Azure yerleşik rol **hiyerarşisi ayarları Yöneticisi**' nde bulunur.

## <a name="setting---default-management-group"></a>Ayarlama-varsayılan Yönetim grubu

Varsayılan olarak, kiracı içine eklenen yeni bir abonelik kök yönetim grubunun bir üyesi olarak eklenir. İlke atamaları, rol tabanlı erişim denetimi (RBAC) ve diğer idare yapıları kök yönetim grubuna atanırsa, bu yeni abonelikleri hemen etkiler. Bu nedenle, birçok kuruluş bu yapıları, bu yapıları atamak için istenen yer olsa bile kök yönetim grubuna uygulamaz. Diğer durumlarda, yeni abonelikler için daha kısıtlayıcı bir denetim kümesi istenir, ancak tüm aboneliklere atanmamalıdır. Bu ayar hem kullanım durumlarını destekler.

Yeni abonelikler için varsayılan yönetim grubunun tanımlanmasına izin vererek, kuruluş genelinde idare yapıları kök yönetim grubuna uygulanabilir ve ilke atamaları veya Azure rolü atamaları olan ayrı bir yönetim grubu, yeni bir aboneliğe daha uygun olabilir.

Bu ayarı yapılandırmak için, [Hiyerarşi ayarları](/rest/api/resources/hierarchysettings) REST API uç noktası çağrılır. Bunu yapmak için aşağıdaki REST API URI ve gövde biçimini kullanın. `{rootMgID}`Kök yönetim GRUBUNUZUN kimliğiyle ve `{defaultGroupID}` varsayılan Yönetim grubu olacak YÖNETIM grubu kimliğiyle değiştirin:

- REST API URI'si

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{rootMgID}/settings/default?api-version=2020-02-01
  ```

- İstek Gövdesi

  ```json
  {
      "properties": {
          "defaultManagementGroup": "/providers/Microsoft.Management/managementGroups/{defaultGroupID}"
      }
  }
  ```

Varsayılan yönetim grubunu kök yönetim grubuna geri ayarlamak için aynı uç noktayı kullanın ve **Defaultmanagementgroup** değerini değerine ayarlayın `/providers/Microsoft.Management/managementGroups/{rootMgID}` .

## <a name="setting---require-authorization"></a>Ayar-yetkilendirme gerektir

Herhangi bir Kullanıcı, varsayılan olarak bir kiracı içinde yeni yönetim grupları oluşturabilir. Bir kiracının yöneticileri, yönetim grubu hiyerarşisinde tutarlılık ve uyumluluk sağlamak için yalnızca belirli kullanıcılara bu izinleri sağlamak isteyebilir. Etkinleştirilirse, bir Kullanıcı, `Microsoft.Management/managementGroups/write` kök yönetim grubu üzerinde yeni alt yönetim grupları oluşturmak için bu işlemi gerektirir.

Bu ayarı yapılandırmak için, [Hiyerarşi ayarları](/rest/api/resources/hierarchysettings) REST API uç noktası çağrılır. Bunu yapmak için aşağıdaki REST API URI ve gövde biçimini kullanın. Bu değer bir _Boole_değeridir, bu nedenle değer için **true** veya **false** değerini sağlayın. **Doğru** değeri, bu yönetim grubu hiyerarşinizi koruma yöntemini sunar:

- REST API URI'si

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{rootMgID}/settings/default?api-version=2020-02-01
  ```

- İstek Gövdesi

  ```json
  {
      "properties": {
          "requireAuthorizationForGroupCreation": true
      }
  }
  ```

Ayarı yeniden kapatmak için aynı uç noktayı kullanın ve **Requireauthorizationforgroupoluşturmayı** **false**değerine ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Yönetim grupları hakkında daha fazla bilgi almak için bkz.:

- [Azure kaynaklarını düzenlemek için yönetim grupları oluşturma](../create.md)
- [Yönetim gruplarınızı değiştirme, silme veya yönetme](../manage.md)
