---
title: Sanal ağ oluşturma
titleSuffix: Azure SQL Managed Instance
description: Bu makalede, Azure SQL yönetilen örneği dağıtımı desteklemek için yapılandırılmış bir sanal ağın nasıl oluşturulacağı açıklanır.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 09/12/2019
ms.openlocfilehash: d6458794b8d3c5ba68109dbc90a075d6a2a577b9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84711332"
---
# <a name="create-a-virtual-network-for-azure-sql-managed-instance"></a>Azure SQL Yönetilen Örneği için sanal ağ oluşturma
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bu makalede, Azure SQL yönetilen örneğini dağıtabileceğiniz geçerli bir sanal ağ ve alt ağ oluşturma açıklanır.

Azure SQL yönetilen örneğinin bir Azure [sanal ağı](../../virtual-network/virtual-networks-overview.md)içinde dağıtılması gerekir. Bu dağıtım, aşağıdaki senaryolara izin vermez:

- Güvenli özel IP adresi
- Şirket içi ağdan doğrudan SQL yönetilen örneğine bağlanma
- SQL yönetilen örneğini bağlı bir sunucuya veya başka bir şirket içi veri deposuna bağlama
- SQL yönetilen örneği Azure kaynaklarına bağlama  

> [!NOTE]
> İlk örneği dağıtmadan önce [SQL yönetilen örneği için alt ağın boyutunu belirlemelisiniz](vnet-subnet-determine-size.md) . Kaynakları içine koyduktan sonra alt ağı yeniden boyutlandıramazsınız.
>
> Var olan bir sanal ağı kullanmayı planlıyorsanız, SQL yönetilen örneği kapsayacak şekilde bu ağ yapılandırmasını değiştirmeniz gerekir. Daha fazla bilgi için bkz. [SQL yönetilen örneği için var olan bir sanal ağı değiştirme](vnet-existing-add-subnet.md).
>
> Yönetilen bir örnek oluşturulduktan sonra, yönetilen örneği veya sanal ağı başka bir kaynak grubuna veya aboneliğe taşıma desteklenmez.  Yönetilen örneği başka bir alt ağa taşımak de desteklenmez.
>

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Bir sanal ağ oluşturmanın ve yapılandırmanın en kolay yolu Azure Resource Manager dağıtım şablonu kullanmaktır.

1. Azure Portal’da oturum açın.

2. **Azure 'A dağıt** düğmesini seçin:

   <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="https://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

   Bu düğme, SQL yönetilen örneği dağıtabileceğiniz ağ ortamını yapılandırmak için kullanabileceğiniz bir form açar.

   > [!Note]
   > Bu Azure Resource Manager şablonu, iki alt ağa sahip bir sanal ağ dağıtır. **ManagedInstances**adlı bir alt ağ SQL yönetilen örneği için ayrılmıştır ve önceden yapılandırılmış bir yol tablosuna sahiptir. **Varsayılan**olarak adlandırılan diğer alt ağ, SQL yönetilen örneğine (örneğin, Azure sanal makineleri) erişmesi gereken diğer kaynaklar için kullanılır.

3. Ağ ortamını yapılandırın. Aşağıdaki biçimde, ağ ortamınızın parametrelerini yapılandırabilirsiniz:

   ![Azure ağını yapılandırmak için Kaynak Yöneticisi şablonu](./media/virtual-network-subnet-create-arm-template/create-mi-network-arm.png)

   Sanal ağ ve alt ağların adlarını değiştirebilir ve ağ kaynaklarınızla ilişkili IP aralıklarını ayarlayabilirsiniz. **Satın alma** düğmesini seçtikten sonra bu form ortamınızı oluşturur ve yapılandırır. İki alt ağa ihtiyacınız yoksa, varsayılan olanı silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [SQL yönetilen örneği nedir?](sql-managed-instance-paas-overview.md).
- [SQL yönetilen örneği 'nde bağlantı mimarisi](connectivity-architecture-overview.md)hakkında bilgi edinin.
- [SQL yönetilen örneği için var olan bir sanal ağı değiştirme](vnet-existing-add-subnet.md)hakkında bilgi edinin.
- Bir sanal ağ oluşturmayı, yönetilen bir örnek oluşturmayı ve bir veritabanını bir veritabanı yedeğinden geri yüklemeyi gösteren bir öğretici için bkz. [yönetilen örnek oluşturma](instance-create-quickstart.md).
- DNS sorunları için bkz. [Özel BIR DNS yapılandırma](custom-dns-configure.md).
