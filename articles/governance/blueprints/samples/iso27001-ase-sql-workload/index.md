---
title: ISO 27001 ASE/SQL iş yükü şema örneğine genel bakış
description: ISO 27001 App Service Ortamı/SQL Veritabanı iş yükü şema örneğinin genel bakış bilgileri ve mimarisi.
ms.date: 07/13/2020
ms.topic: sample
ms.openlocfilehash: 71044ae1754cf4bdfc42be7b6585a1905dce426c
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86536842"
---
# <a name="overview-of-the-iso-27001-app-service-environmentsql-database-workload-blueprint-sample"></a>ISO 27001 App Service Ortamı/SQL Veritabanı iş yükü şema örneğine genel bakış

ISO 27001 App Service Ortamı/SQL Veritabanı iş yükü şema örneği, [ISO 27001 Paylaşılan Hizmetler](../iso27001-shared/index.md) şema örneğine ek altyapı sağlar.
Bu şema müşterilerin akreditasyon ve uyumluluk gereksinimleri olan senaryolara çözüm sunan bulut tabanlı mimariler dağıtmasına yardımcı olur.

İki ISO 27001 şema örneği vardır: bu örnek ve [ISO 27001 Paylaşılan Hizmetler](../iso27001-shared/index.md) şema örneği.

> [!IMPORTANT]
> Bu örnek [ISO 27001 Paylaşılan Hizmetler](../iso27001-shared/index.md) şema örneği tarafından dağıtılan altyapıya bağımlıdır. Önce o örnek dağıtılmalıdır.

## <a name="architecture"></a>Mimari

ISO 27001 App Service Ortamı/SQL Veritabanı iş yükü şema örneği, hizmet olarak platform temelinde bir web ortamının dağıtımını yapar. Ortam, ISO 27001 standartlarına uyan birden çok web uygulamasını, web API'sini ve SQL Veritabanı örneğini barındırmak için kullanılabilir. Bu şema örneği, [ISO 27001 Paylaşılan Hizmetler](../iso27001-shared/index.md) şema örneğine bağımlıdır.

:::image type="content" source="../../media/sample-iso27001-ase-sql-workload/iso27001-ase-sql-workload-blueprint-sample-design.png" alt-text="ISO 27001 ASE/SQL iş yükü şema örneği tasarımı" border="false":::

Bu ortam, ISO 27001 standartlarında güvenli, tümüyle izlenen, kurumsal kullanıma hazır bir iş yükü altyapısı sağlamak için kullanılan çeşitli Azure hizmetlerinden oluşur. Bu ortam şunlardan oluşur:

- Şema örneği tarafından dağıtılan [Azure App Service Ortamlarında](../../../../app-service/environment/intro.md) kaynakları dağıtma ve yönetme haklarına sahip olan DevOps adlı [rol tabanlı erişim denetimi](../../../../role-based-access-control/overview.md) (RBAC) rolü
- Ortama dağıtılabilecek hizmetleri belirlemek ve herhangi bir genel IP adresi (PIP) kaynağı oluşturma işlemini reddetmek için [Azure İlkeleri](../../../policy/overview.md)
- Tek bir alt ağ içeren, önceden varolan [paylaşılan hizmetler](../iso27001-shared/index.md) ortamıyla geri eşlenen ve tüm trafiğin [paylaşılan hizmetler](../iso27001-shared/index.md) güvenlik duvarından geçmesini zorunlu tutan bir sanal ağ. Sanal ağ aşağıdaki kaynakları barındırır:
  - Bir veya birden çok web uygulamasını, web API'sini veya işlevi barındırmak için kullanılabilen [Azure App Service Ortamları](../../../../app-service/environment/intro.md)
  - İş yükü ortamında çalıştırılan uygulamaların kullandığı gizli dizileri depolamak için, sanal ağ hizmeti uç noktasını kullanan bir [Azure Key Vault](../../../../key-vault/general/overview.md) örneği
  - İş yükü ortamındaki uygulamalarda kullanılan veritabanlarını depolamak için, sanal ağ hizmeti uç noktasını kullanan bir [Azure SQL Veritabanı](../../../../azure-sql/database/sql-database-paas-overview.md) sunucu örneği

## <a name="next-steps"></a>Sonraki adımlar

ISO 27001 App Service Ortamı/SQL Veritabanı iş yükü şema örneğinin genel bakış bilgilerini ve mimarisini gözden geçirdiniz. Şimdi, denetim eşlemesi ve bu örneğin nasıl dağıtılacağı hakkında bilgi edinmek için şu makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [ISO 27001 App Service Ortamı/SQL Veritabanı iş yükü şeması - Denetim eşlemesi](./control-mapping.md)
> [ISO 27001 App Service Ortamı/SQL Veritabanı iş yükü şeması - Dağıtım adımları](./deploy.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.
