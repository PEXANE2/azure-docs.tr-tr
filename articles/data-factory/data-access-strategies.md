---
title: Veri erişimi stratejileri
description: Azure Data Factory artık statik IP adresi aralıklarını desteklemektedir.
services: data-factory
ms.author: abnarain
author: nabhishek
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/11/2020
ms.openlocfilehash: 40da43b88e5f95335e8c1766128ac60502630c7b
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83853788"
---
# <a name="data-access-strategies"></a>Veri erişimi stratejileri

Kuruluşun önemli bir güvenlik amacı, veri depolarını Internet üzerinden rastgele erişimden korumak, şirket içi veya bulut/SaaS veri deposu olabilir. 

Genellikle bir bulut veri deposu aşağıdaki mekanizmalardan yararlanarak erişimi denetler:
* Bağlantıyı IP adresine göre sınırlayan güvenlik duvarı kuralları
* Kullanıcıların kimliklerini kanıtlamasını gerektiren kimlik doğrulama mekanizmaları
* Kullanıcıları belirli eylemlerle ve verilerle kısıtlayan Yetkilendirme mekanizmaları

> [!TIP]
> [STATIK IP adres aralığının kullanıma sunulmasıyla](https://docs.microsoft.com/azure/data-factory/azure-integration-runtime-ip-addresses)birlikte, bulut veri depolarındaki tüm Azure IP adreslerine izin vermek zorunda olmadığınızdan emin olmak Için belirli Azure tümleştirme çalışma zamanı BÖLGESI için IP aralıkları listesine izin verebilirsiniz. Bu şekilde, veri depolarına erişmelerine izin verilen IP adreslerini kısıtlayabilirsiniz.

> [!NOTE] 
> IP adresi aralıkları Azure tümleştirme çalışma zamanı için engellenir ve şu anda yalnızca veri taşıma, işlem hattı ve dış etkinlikler için kullanılır. Veri akışları artık bu IP aralıklarını kullanmaz. 

Bu pek çok senaryoda çalışmalıdır ve tümleştirme çalışma zamanı başına benzersiz bir statik IP adresi istenmekte olduğunu anladık, ancak bu, sunucusuz olan Azure Integration Runtime Şu anda mümkün olmayabilir. Gerekirse, her zaman kendi kendine barındırılan bir Integration Runtime ayarlayabilir ve statik IP 'nizi bununla birlikte kullanabilirsiniz. 

## <a name="data-access-strategies-through-azure-data-factory"></a>Azure Data Factory aracılığıyla veri erişimi stratejileri

* **[Güvenilen hizmet](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions)** -Azure depolama (Blob, ADLS 2.), güvenilen Azure platform hizmetlerinin depolama hesabına güvenli bir şekilde erişmesini sağlayan güvenlik duvarı yapılandırmasını destekler. Güvenilen hizmetler yönetilen kimlik kimlik doğrulamasını zorunlu kılar, bu da yönetilen kimlik doğrulamasını kullanmak için daha önce listelenmedikleri takdirde, bu depolama alanına başka bir veri fabrikası bağlanmamasını sağlar. **[Bu blogda](https://techcommunity.microsoft.com/t5/azure-data-factory/data-factory-is-now-a-trusted-service-in-azure-storage-and-azure/ba-p/964993)** daha fazla ayrıntı bulabilirsiniz. Bu nedenle, bu son derece güvenlidir ve önerilir. 
* **Benzersiz STATIK IP** -Data Factory bağlayıcıları IÇIN statik IP almak üzere şirket içinde barındırılan bir tümleştirme çalışma zamanı ayarlamanız gerekir. Bu mekanizma, diğer tüm IP adreslerinden erişimi engellemenize de engel olur. 
* **[STATIK IP aralığı](https://docs.microsoft.com/azure/data-factory/azure-integration-runtime-ip-addresses)** -Azure INTEGRATION RUNTIME kendi IP adreslerini, depolama ortamınızda listelemek için kullanabilirsiniz (S3, Salesforce, vb.). Veri depolarına bağlanabilecek, ancak kimlik doğrulama/yetkilendirme kurallarına de dayanan IP adreslerini kesinlikle kısıtlar.
* **[Hizmet etiketi](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)** -hizmet etiketi, belirli bir Azure hizmetinden (Azure Data Factory gibi) bir IP adresi ön eki grubunu temsil eder. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir ve ağ güvenlik kuralları için sık sık güncelleştirmelerin karmaşıklığını en aza indirir. Sanal ağ 'da IaaS barındırılan veri depolarında veri erişimi için beyaz listeye alma işlemi yararlı olur.
* **Azure hizmetlerine Izin ver** -bazı hizmetler, bu seçeneği tercih etmeniz durumunda tüm Azure hizmetlerinin bağlanmasına izin vermenizi sağlar. 

Azure Integration Runtime ve şirket içinde barındırılan Integration Runtime veri depolarında desteklenen ağ güvenlik mekanizmaları hakkında daha fazla bilgi için, iki tablonun altına bakın.  
* **Azure Integration Runtime**

    | Veri Depoları                  | Veri depolarında desteklenen ağ güvenlik mekanizması         | Güvenilen hizmet     | Statik IP aralığı | Hizmet Etiketleri | Azure hizmetlerine izin ver |
    |------------------------------|-------------------------------------------------------------|---------------------|-----------------|--------------|----------------------|
    | Azure PaaS veri depoları       | Azure Cosmos DB                                             | -                   | Yes             | -            | Yes                  |
    |                              | Azure Veri Gezgini                                         | -                   | Evet*            | Evet*         | -                    |
    |                              | Azure Data Lake Gen1                                        | -                   | Yes             | -            | Yes                  |
    |                              | MariaDB, MySQL, PostgreSQL için Azure veritabanı               | -                   | Yes             | -            | Yes                  |
    |                              | Azure Dosya Depolama                                          | -                   | Yes             | -            | .                    |
    |                              | Azure depolama (blog, ADLS 2.)                             | Evet (yalnızca MSI kimlik doğrulaması) | Yes             | -            | .                    |
    |                              | Azure SQL DB, SQL DW (SYNAPSE Analytics), SQL ml          | -                   | Yes             | -            | Yes                  |
    |                              | Azure Key Vault (gizli dizileri/bağlantı dizesi getiriliyor) | Yes                 | Yes             | -            | -                    |
    | Diğer PaaS/SaaS veri depoları | AWS S3, SalesForce, Google bulut depolama, vb.            | -                   | Yes             | -            | -                    |
    | Azure laaS                   | SQL Server, Oracle, vb.                                  | -                   | Yes             | Yes          | -                    |
    | Şirket içi laaS              | SQL Server, Oracle, vb.                                  | -                   | Yes             | -            | -                    |
    
    **Yalnızca Azure Veri Gezgini, sanal ağ eklendiğinde ve IP aralığı NSG/güvenlik duvarında uygulanabildiğinde geçerlidir.* 

* **Şirket içinde barındırılan Integration Runtime (VNet/şirket içi)**
    
    | Veri depoları                  | Veri depolarında desteklenen ağ güvenlik mekanizması         | Statik IP | Güvenilen hizmetler  |
    |--------------------------------|---------------------------------------------------------------|-----------|---------------------|
    | Azure PaaS veri depoları       | Azure Cosmos DB                                               | Yes       | -                   |
    |                                | Azure Veri Gezgini                                           | -         | -                   |
    |                                | Azure Data Lake Gen1                                          | Yes       | -                   |
    |                                | MariaDB, MySQL, PostgreSQL için Azure veritabanı               | Yes       | -                   |
    |                                | Azure Dosya Depolama                                            | Yes       | -                   |
    |                                | Azure depolama (blog, ADLS 2.)                             | Yes       | Evet (yalnızca MSI kimlik doğrulaması) |
    |                                | Azure SQL DB, SQL DW (SYNAPSE Analytics), SQL ml          | Yes       | -                   |
    |                                | Azure Key Vault (gizli dizileri/bağlantı dizesi getiriliyor) | Yes       | Yes                 |
    | Diğer PaaS/SaaS veri depoları | AWS S3, SalesForce, Google bulut depolama, vb.              | Yes       | -                   |
    | Azure laaS                     | SQL Server, Oracle, vb.                                  | Yes       | -                   |
    | Şirket içi laaS              | SQL Server, Oracle, vb.                                  | Evet       | -                   |    

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki ilgili makalelere bakın:
* [Desteklenen veri depoları](https://docs.microsoft.com/azure/data-factory/copy-activity-overview#supported-data-stores-and-formats)
* [' Güvenilir hizmetler ' Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview-vnet-service-endpoints#trusted-services)
* [Azure Storage ' güvenilir Microsoft Hizmetleri '](https://docs.microsoft.com/azure/storage/common/storage-network-security#trusted-microsoft-services)
* [Data Factory için yönetilen kimlik](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity)
