---
title: Azure Data Factory için roller ve izinler
description: Veri Fabrikaları oluşturmak ve alt kaynaklarla çalışmak için gereken rolleri ve izinleri açıklar.
ms.date: 11/5/2018
ms.topic: conceptual
ms.service: data-factory
services: data-factory
documentationcenter: ''
ms.workload: data-services
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.openlocfilehash: 8b7791c5c04e986b30959d2fcae17142fdd8b7ec
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418329"
---
# <a name="roles-and-permissions-for-azure-data-factory"></a>Azure Data Factory için roller ve izinler

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]


Bu makalede, Azure Veri Fabrikası kaynaklarını oluşturmak ve yönetmek için gereken roller ve bu roller tarafından verilen izinler açıklanmaktadır.

## <a name="roles-and-requirements"></a>Roller ve gereksinimler

Data Factory örnekleri oluşturmak için, Azure’da oturum açarken kullandığınız kullanıcı hesabı, *katkıda bulunan*, *sahip* veya *yönetici* rollerinin üyesi ya da bir Azure aboneliğinin yöneticisi olmalıdır. Abonelikte sahip olduğunuz izinleri görüntülemek için Azure portalına gidin, sağ üst köşeden kullanıcı adınızı ve sonra **İzinler**’i seçin. Birden çok aboneliğe erişiminiz varsa uygun aboneliği seçin. 

Data Factory için veri kümeleri, bağlı hizmetler, işlem hatları, tetikleyiciler ve tümleştirme çalışma zamanları gibi alt kaynaklar oluşturup yönetmek için aşağıdaki gereksinimlerin karşılanması gerekir:
- Azure portalda alt öğe oluşturup yönetebilmek için kaynak grubu düzeyinde **Data Factory Katılımcısı** rolüne veya daha üst düzey bir role sahip olmanız gerekir.
- PowerShell veya SDK ile alt öğe oluşturup yönetebilmek için kaynak düzeyinde **katkıda bulunan** rolü veya daha üst düzey bir rol yeterli olacaktır.

Kullanıcı rolü eklemeye ilişkin örnek yönergeler için [Rol ekleme](../cost-management-billing/manage/add-change-subscription-administrator.md) makalesine bakın.

## <a name="set-up-permissions"></a>İzinleri ayarlama

Bir Veri Fabrikası oluşturduktan sonra, diğer kullanıcıların veri fabrikasıyla çalışmasına izin vermek isteyebilirsiniz. Bu erişimi diğer kullanıcılara vermek için, bunları veri fabrikasını içeren kaynak grubundaki yerleşik **Veri Fabrikası Katılımcısı** rolüne eklemeniz gerekir.

### <a name="scope-of-the-data-factory-contributor-role"></a>Veri Fabrikası Katılımcısıfatı'nun Kapsamı

**Veri Fabrikası Katılımcısı** rolüne üyelik, kullanıcıların aşağıdaki şeyleri yapmasına olanak tanır:
- Veri kümeleri, bağlantılı hizmetler, ardışık etkiler, tetikleyiciler ve tümleştirme çalışma süreleri dahil olmak üzere veri fabrikalarını ve alt kaynakları oluşturun, düzenleyin ve silin.
- Kaynak Yöneticisi şablonlarını dağıtın. Kaynak Yöneticisi dağıtımı, Azure portalındaki Veri Fabrikası tarafından kullanılan dağıtım yöntemidir.
- Bir veri fabrikası için App Insights uyarılarını yönetin.
- Destek biletleri oluşturun.

Bu rol hakkında daha fazla bilgi için [Veri Fabrikası Katılımcısı rolüne](../role-based-access-control/built-in-roles.md#data-factory-contributor)bakın.

### <a name="resource-manager-template-deployment"></a>Kaynak Yöneticisi şablon dağıtımı

Kaynak grubu düzeyinde veya üstündeki **Veri Fabrikası Katılımcısı** rolü, kullanıcıların Kaynak Yöneticisi şablonlarını dağıtmasına olanak tanır. Sonuç olarak, rolün üyeleri veri kümeleri, bağlantılı hizmetler, ardışık hatlar, tetikleyiciler ve tümleştirme çalışma saatleri de dahil olmak üzere hem veri fabrikalarını hem de alt kaynaklarını dağıtmak için Kaynak Yöneticisi şablonlarını kullanabilir. Ancak, bu role üyelik, kullanıcının başka kaynaklar oluşturmasına izin vermez.

Azure Repos ve GitHub'daki izinler Veri Fabrikası izinlerinden bağımsızdır. Sonuç olarak, yalnızca Reader rolünün bir üyesi olan repo izinlerine sahip bir kullanıcı Veri Fabrikası alt kaynaklarını düzenlemeye ve repo'da değişiklikler gerçekleştirebilir, ancak bu değişiklikleri yayımlayamaz.

> [!IMPORTANT]
> **Veri Fabrikası Katılımcısı** rolüyle Kaynak Yöneticisi şablondağıtımı izinlerinizi yükseltmez. Örneğin, Azure sanal makinesi oluşturan bir şablon dağıtırsanız ve sanal makineler oluşturma izniniz yoksa, dağıtım bir yetkilendirme hatasıyla başarısız olur.

### <a name="custom-scenarios-and-custom-roles"></a>Özel senaryolar ve özel roller

Bazen farklı veri fabrikası kullanıcıları için farklı erişim düzeyleri vermek gerekebilir. Örneğin:
- Kullanıcıların yalnızca belirli bir veri fabrikasında izinleri olan bir gruba ihtiyacınız olabilir.
- Veya kullanıcıların yalnızca bir veri fabrikasını (veya fabrikaları) izleyebilecekleri ancak değiştirilemeyecekleri bir gruba ihtiyacınız olabilir.

Özel roller oluşturarak ve kullanıcıları bu rollere atayarak bu özel senaryoları elde edebilirsiniz. Özel roller hakkında daha fazla bilgi için [Azure'daki Özel rolleri](..//role-based-access-control/custom-roles.md)görün.

Özel rollerle neler başarabileceğinizi gösteren birkaç örnek aşağıda verilmiştir:

- Bir kullanıcının Azure portalından kaynak grubundaki herhangi bir veri fabrikasını oluşturmasına, düzenlemasına veya silmesine izin verin.

  Kullanıcı için kaynak grubu düzeyinde yerleşik **Veri Fabrikası katılımcısı** rolünü atayın. Abonelikteki herhangi bir veri fabrikasına erişime izin vermek istiyorsanız, rolü abonelik düzeyinde atayın.

- Bir kullanıcının bir veri fabrikasını görüntülemesine (okumasına ve izlemesine) izin verin, ancak bunu değiştirmeyin veya değiştirmeyin.

  Kullanıcı için veri fabrikası kaynağındaki yerleşik **okuyucu** rolünü atayın.

- Bir kullanıcının Azure portalında tek bir veri fabrikasını yeniden atmasına izin verin.

  Bu senaryo iki rol atamaları gerektirir.

  1. Yerleşik **katılımcı** rolünü veri fabrikası düzeyinde atayın.
  2. **Microsoft.Resources/deployments/** izniyle özel bir rol oluşturun. Bu özel rolü kaynak grubu düzeyinde ki kullanıcıya atayın.

- Bir kullanıcının yalnızca bağlantılı bir hizmetteki bağlantıyı test edebilmesine izin verin

    Aşağıdaki eylemler için izinlerle özel bir rol rolü oluşturun: **Microsoft.DataFactory/factorys/getFeatureValue/read** ve **Microsoft.DataFactory/factorys/getDataPlaneAccess/read**. Bu özel rolü kullanıcı için veri fabrikası kaynağına atayın.

- Bir kullanıcının PowerShell veya SDK'dan bir veri fabrikasını güncelleştirmesine izin verin, ancak Azure portalında güncelleştirmeyin.

  Kullanıcı için veri fabrikası kaynağındaki yerleşik **katılımcı** rolünü atayın. Bu rol, kullanıcının Azure portalındaki kaynakları görmesini sağlar, ancak kullanıcı **Tüm düğmeleri** Yayımla ve **Yayımla** düğmelerine erişemez.

## <a name="next-steps"></a>Sonraki adımlar

- Azure'daki roller hakkında daha fazla bilgi edinin - [Rol tanımlarını anlama](../role-based-access-control/role-definitions.md)

- Veri Fabrikası **katılımcısı** rolü - [Veri Fabrikası Katılımcısı rolü](../role-based-access-control/built-in-roles.md#data-factory-contributor)hakkında daha fazla bilgi edinin.
