---
title: Azure Resource Manager şablonları-SQL yönetilen örneği & Azure SQL veritabanı
description: Azure SQL veritabanı ve Azure SQL yönetilen örneği oluşturmak ve yapılandırmak için Azure Resource Manager şablonlarını kullanın.
services: sql-database
ms.service: sql-db-mi
ms.subservice: service
ms.custom: overview-samples sqldbrb=2
ms.devlang: ''
ms.topic: sample
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 02/04/2019
ms.openlocfilehash: ee2c6580285246d3b170602e507aed038b2507f4
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85983648"
---
# <a name="azure-resource-manager-templates-for-azure-sql-database--sql-managed-instance"></a>Azure SQL veritabanı & SQL yönetilen örneği için Azure Resource Manager şablonları
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Azure Resource Manager şablonları, altyapınızı kod olarak tanımlamanızı ve çözümlerinizi Azure SQL Veritabanı ile Azure SQL Yönetilen Örneği için Azure bulutuna dağıtmanızı sağlar.

## <a name="azure-sql-database"></a>[Azure SQL Veritabanı](#tab/single-database)

Aşağıdaki tabloda, Azure SQL veritabanı için Azure Resource Manager şablonlarının bağlantıları yer almaktadır.

|Bağlantı |Description|
|---|---|
| [SQL Veritabanı](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-database-transparent-encryption-create) | Bu Azure Resource Manager şablonu, Azure SQL veritabanı 'nda tek bir veritabanı oluşturur ve sunucu düzeyinde IP güvenlik duvarı kurallarını yapılandırır. |
| [Sunucu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-logical-server) | Bu Azure Resource Manager şablonu, Azure SQL veritabanı için bir sunucu oluşturur. |
| [Elastik havuz](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-elastic-pool-create) | Bu şablon, elastik bir havuz dağıtmanıza ve veritabanına veritabanı atamanıza olanak tanır. |
| [Yük devretme grupları](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-with-failover-group) | Bu şablon, Azure SQL veritabanı 'nda iki sunucu, tek bir veritabanı ve bir yük devretme grubu oluşturur.|
| [Tehdit algılama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-threat-detection-db-policy-multiple-databases) | Bu şablon, tehdit algılama özelliği etkin olan bir sunucu ve veritabanı kümesini, her veritabanı için uyarılar için bir e-posta adresiyle dağıtmanıza olanak tanır. Tehdit algılama, SQL Gelişmiş tehdit koruması (ATP) sunumunun bir parçasıdır ve sunucular ve veritabanları üzerinde olası tehditlere yanıt veren bir güvenlik katmanı sağlar.|
| [Azure Blob depolamaya yönelik denetim](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-auditing-server-policy-to-blob-storage) | Bu şablon, Denetim günlüklerini blob depolamaya yazmak için denetim etkinleştirilmiş bir sunucu dağıtmanıza olanak tanır. Azure SQL veritabanı için denetim, veritabanı olaylarını izler ve bunları Azure depolama hesabınıza, OMS çalışma alanınıza veya Event Hubs yerleştirilebilecek bir denetim günlüğüne yazar.|
| [Azure Olay Hub 'ına denetim](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-auditing-server-policy-to-eventhub) | Bu şablon, mevcut bir olay hub 'ına denetim günlükleri yazmak için denetim etkinleştirilmiş bir sunucu dağıtmanızı sağlar. Event Hubs denetim olayları göndermek için denetim ayarlarını ile ayarlayın `Enabled` `State` ve `IsAzureMonitorTargetEnabled` olarak ayarlayın `true` . Ayrıca, tanılama ayarlarını `SQLSecurityAuditEvents` veritabanındaki günlük kategorisiyle yapılandırın `master` (sunucu düzeyinde denetim için). Denetim, veritabanı olaylarını izler ve bunları Azure depolama hesabınıza, OMS çalışma alanınıza veya Event Hubs yerleştirilebilecek bir denetim günlüğüne yazar.|
| [SQL veritabanı ile Azure Web uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database) | Bu örnek, ücretsiz bir Azure Web uygulaması ve Azure SQL veritabanı 'nda "temel" hizmet düzeyinde bir veritabanı oluşturur.|
| [Azure Web uygulaması ve SQL veritabanı ile Redis Cache](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) | Bu şablon, aynı kaynak grubunda bir Web uygulaması, Redis Cache ve veritabanı oluşturur ve veritabanı ve Redis Cache için Web uygulamasında iki bağlantı dizesi oluşturur.|
| [ADF v2 kullanarak blob depolamadan veri aktarma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-v2-blob-to-sql-copy) | Bu Azure Resource Manager şablonu, verileri Azure Blob depolama alanından SQL veritabanına kopyalayan bir Azure Data Factory v2 örneği oluşturur.|
| [Veritabanı içeren HDInsight kümesi](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-with-sql-database) | Bu şablon, bir HDInsight kümesi, bir mantıksal SQL Server, veritabanı ve iki tablo oluşturmanıza olanak sağlar. Bu şablon, [HDInsight 'Ta Hadoop Ile Sqoop kullanma makalesinde](https://docs.microsoft.com/azure/hdinsight/hadoop/hdinsight-use-sqoop)kullanılır. |
| [Bir SQL saklı yordamını zamanlamaya göre çalıştıran Azure Logic App](https://github.com/Azure/azure-quickstart-templates/tree/master/101-logic-app-sql-proc) | Bu şablon, zamanlamaya göre bir SQL saklı yordamı çalıştıracak bir mantıksal uygulama oluşturmanıza olanak tanır. Yordamın tüm bağımsız değişkenleri, şablonun gövde bölümüne yerleştirilebilir.|

## <a name="azure-sql-managed-instance"></a>[Azure SQL Yönetilen Örnek](#tab/managed-instance)

Aşağıdaki tabloda, Azure SQL yönetilen örneği için Azure Resource Manager şablonlarının bağlantıları yer almaktadır.

|Bağlantı|Description|
|---|---|
| [Yeni bir VNet 'te SQL yönetilen örneği](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sqlmi-new-vnet) | Bu Azure Resource Manager şablonu, sanal ağda yeni bir yapılandırılmış Azure sanal ağı ve yönetilen örnek oluşturur. |
| [SQL yönetilen örneği için ağ ortamı](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-managed-instance-azure-environment) | Bu dağıtım, yönetilen örneklerinize ve diğer kaynakları yerleştirebileceğiniz (örneğin, VM 'Ler, App Service ortamları, vb.) bir tane olmak üzere iki alt ağ içeren yapılandırılmış bir Azure sanal ağı oluşturacak. Bu şablon, yönetilen örnekleri dağıtabileceğiniz, düzgün şekilde yapılandırılmış bir ağ ortamı oluşturacak. |
| [P2S bağlantısı olan SQL yönetilen örneği](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sqlmi-new-vnet-w-point-to-site-vpn) | Bu dağıtım, iki alt ağ ile bir Azure sanal ağı oluşturur `ManagedInstance` ve `GatewaySubnet` . SQL yönetilen örneği, ManagedInstance alt ağında dağıtılacak. Alt ağda bir sanal ağ geçidi oluşturulur `GatewaySubnet` ve noktadan sıteye VPN bağlantısı için yapılandırılır. |
| [Sanal makineyle SQL yönetilen örneği](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sqlmi-new-vnet-w-jumpbox) | Bu dağıtım, iki alt ağ ile bir Azure sanal ağı oluşturur `ManagedInstance` ve `Management` . SQL yönetilen örneği `ManagedInstance` alt ağda dağıtılacak. Alt ağda en son SQL Server Management Studio (SSMS) sürümüne sahip bir sanal makine dağıtılır `Management` . |

---

