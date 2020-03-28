---
title: Azure Resource Manager şablonları
description: Azure SQL Veritabanı oluşturmak ve yapılandırmak için Azure Kaynak Yöneticisi şablonlarını kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: overview-samples
ms.devlang: ''
ms.topic: sample
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 02/04/2019
ms.openlocfilehash: b254621cc414fb9b2b76263957adc80da6e9c22d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79213997"
---
# <a name="azure-resource-manager-templates-for-azure-sql-database"></a>Azure SQL Veritabanı için Azure Kaynak Yöneticisi şablonları

Azure Kaynak Yöneticisi şablonları, altyapınızı kod olarak tanımlamanızı ve çözümlerinizi Azure bulutuna dağıtmanızı sağlar.

## <a name="single-database--elastic-pool"></a>[Tek veritabanı & elastik havuz](#tab/single-database)

Aşağıdaki tablo, Azure SQL Veritabanı için Azure Kaynak Yöneticisi şablonlarına bağlantılar içerir.

| |  |
|---|---|
| [Tek veritabanı](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-database-transparent-encryption-create) | Bu Azure Kaynak Yöneticisi şablonu, mantıksal sunucuya sahip tek bir Azure SQL Veritabanı oluşturur ve güvenlik duvarı kurallarını yapılandırır. |
| [Mantıksal sunucu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-logical-server) | Bu Azure Kaynak Yöneticisi şablonu, Azure SQL Veritabanı için mantıksal bir sunucu oluşturur. |
| [Elastik havuz](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-elastic-pool-create) | Bu şablon, yeni ilişkili SQL Server ve yeni SQL Veritabanları ile yeni bir Elastik havuz dağıtmak için ona atamak için izin verir. |
| [Başarısız gruplar](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-with-failover-group) | Bu şablon, iki Azure SQL mantıksal sunucu, bir SQL veritabanı ve bir başarısız grup oluşturur.|
| [Tehdit Algılama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-threat-detection-db-policy-multiple-databases) | Bu şablon, her veritabanı için uyarılar için bir e-posta adresi yle, Tehdit Algılama etkinleştirilmiş bir Azure SQL mantıksal sunucusu ve bir dizi Azure SQL Veritabanı dağıtmanızı sağlar. Tehdit Algılama, SQL Gelişmiş Tehdit Koruması (ATP) teklifinin bir parçasıdır ve SQL sunucuları ve veritabanları üzerindeki olası tehditlere yanıt veren bir güvenlik katmanı sağlar.|
| [Azure Blob Depolamasına Denetim](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-auditing-server-policy-to-blob-storage) | Bu şablon, denetim günlüklerini blob depolamasına yazmak için Denetim etkinleştirilmiş bir Azure SQL mantıksal sunucusu dağıtmanızı sağlar. Azure SQL Veritabanı denetimi veritabanı olaylarını izler ve bunları Azure depolama hesabınıza, OMS çalışma alanınıza veya Olay Hub'larınıza yerleştirilebilen bir denetim günlüğüne yazar.|
| [Azure Etkinlik Hub'ına Denetim](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-auditing-server-policy-to-eventhub) | Bu şablon, denetim günlüklerini çıkan bir Olay Hub'ına yazmak için Denetim etkinleştirilmiş bir Azure SQL sunucusu dağıtmanızı sağlar. Denetim olaylarını Event Hub'a göndermek için `Enabled` `State` denetim `IsAzureMonitorTargetEnabled` ayarlarını ayarlayın ve ' ' olarak `true`ayarlayın. Ayrıca, `master` Veritabanında günlük `SQLSecurityAuditEvents` kategorisi ile Tanılama Ayarları yapılandırın (hizmet düzeyi denetimi için). Azure SQL Veritabanı ve SQL Veri Ambarı denetimi veritabanı olaylarını izler ve bunları Azure depolama hesabınıza, OMS çalışma alanınıza veya Olay Hub'larınıza yerleştirilebilen bir denetim günlüğüne yazar.|
| [SQL Veritabanı ile Azure Web Uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database) | Bu örnek, "Temel" hizmet düzeyinde ücretsiz bir Azure Web Uygulaması ve SQL Veritabanı oluşturur.|
| [SQL Veritabanı ile Azure Web Uygulaması ve Redis Önbelleği](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) | Bu şablon, aynı kaynak grubunda bir Web Uygulaması, Redis Önbelleği ve SQL Veritabanı oluşturur ve SQL Veritabanı ve Redis Önbelleği için Web Uygulamasında iki bağlantı dizeleri oluşturur.|
| [ADF V2 kullanarak blob depolamadan veri alma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-v2-blob-to-sql-copy) | Bu Azure Kaynak Yöneticisi şablonu, Azure Blob Depolama'dan SQL Veritabanı'na kadar verileri kopyalayan Azure Veri Fabrikası V2 oluşturur.|
| [SQL Veritabanı ile HDInsight kümesi](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-with-sql-database) | Bu şablon, bir HDInsight kümesi, bir SQL Veritabanı sunucusu, bir SQL Veritabanı ve iki tablo oluşturmanıza olanak tanır. Bu şablon [HDInsight makalesinde Hadoop ile Sqoop kullanın](https://docs.microsoft.com/azure/hdinsight/hadoop/hdinsight-use-sqoop) tarafından kullanılır |
| [Bir zamanlamada SQL Stored Yordamı çalıştıran Azure Mantık Uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/101-logic-app-sql-proc) | Bu şablon, sql depolanmış yordamı zamanında çalıştıracak bir Mantık Uygulaması oluşturmanıza olanak tanır. Yordam için herhangi bir bağımsız değişken şablonun gövde bölümüne konulabilir.|

## <a name="managed-instance"></a>[Yönetilen Örnek](#tab/managed-instance)

Aşağıdaki tablo, Azure SQL Veritabanı - Yönetilen Örnek için Azure Kaynak Yöneticisi şablonlarına bağlantılar içerir.

| |  |
|---|---|
| [Yeni bir VNet'te Yönetilen Örnek](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sqlmi-new-vnet) | Bu Azure Kaynak Yöneticisi şablonu, VNet'te yeni yapılandırılmış bir Azure VNet ve Yönetilen Örnek oluşturur. |
| [Yönetilen Örnek için ağ ortamı](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-managed-instance-azure-environment) | Bu dağıtım, biri Yönetilen Örneklerinize, diğeri de başka kaynaklar (örneğin VM'ler, Uygulama Hizmeti ortamları vb.) yerleştirebileceğiniz iki alt ağiçeren yapılandırılmış bir Azure Sanal Ağı oluşturur. Bu şablon, Yönetilen Örnekleri dağıtabileceğiniz düzgün yapılandırılmış bir ağ ortamı oluşturur. |
| [P2S bağlantısı ile Yönetilen Örnek](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sqlmi-new-vnet-w-point-to-site-vpn) | Bu dağıtım, iki alt ağa `ManagedInstance` ve `GatewaySubnet`. Yönetilen Örnek ManagedInstance alt ağına dağıtılır. Sanal ağ ağ geçidi `GatewaySubnet` alt ağda oluşturulur ve Noktaya Sayfa VPN bağlantısı için yapılandırılır. |
| [Sanal makine ile Yönetilen Örnek](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sqlmi-new-vnet-w-jumpbox) | Bu dağıtım, iki alt ağa `ManagedInstance` ve `Management`. Yönetilen Örnek alt ağda `ManagedInstance` dağıtılacaktır. SQL Server Management Studio 'nın (SSMS) en son `Management` sürümüne sahip sanal makine alt ağda dağıtılacaktır. |

---
