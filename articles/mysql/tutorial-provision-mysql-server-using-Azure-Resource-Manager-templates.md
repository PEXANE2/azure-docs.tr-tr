---
title: 'Öğretici: MySQL için Azure Veritabanı Oluşturma - Azure Kaynak Yöneticisi şablonu'
description: Bu öğretici, Azure Kaynak Yöneticisi şablonu kullanarak MySQL sunucu dağıtımları için Azure Veritabanı'nın nasıl sağlanıp otomatikleştirilebildiğini açıklar.
author: savjani
ms.author: pariks
ms.service: mysql
ms.devlang: json
ms.topic: tutorial
ms.date: 12/02/2019
ms.custom: mvc
ms.openlocfilehash: f4960482c88bf9768be1c1c9dbb3652409a8f1b8
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74771109"
---
# <a name="tutorial-provision-an-azure-database-for-mysql-server-using-azure-resource-manager-template"></a>Öğretici: Azure Kaynak Yöneticisi şablonu kullanarak MySQL sunucusu için Azure Veritabanı sağlama

[MySQL REST API için Azure Veritabanı,](https://docs.microsoft.com/rest/api/mysql/) DevOps mühendislerinin Azure'da yönetilen MySQL sunucularının ve veritabanlarının sağlama, yapılandırma ve işlemlerini otomatikleştirmesini ve tümleştirmesini sağlar.  API, MySQL hizmeti için Azure Veritabanı'nda MySQL sunucularının ve veritabanlarının oluşturulmasına, numaralandırmasına, yönetimine ve silinmesine olanak tanır.

Azure Kaynak Yöneticisi, dağıtımlar için gereken Azure kaynaklarını ölçekolarak beyan etmek ve programlamak ve altyapıyla kod kavramı olarak hizalamak için temel REST API'den yararlanır. Şablon, Azure kaynak adını, SKU'yu, ağı, güvenlik duvarı yapılandırmasını ve ayarlarını parametreye vererek bir kez oluşturulmasını ve birden çok kez kullanılmasını sağlar.  Azure Kaynak Yöneticisi şablonları [Azure portalı](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal) veya Visual [Studio Kodu](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-visual-studio-code?tabs=CLI)kullanılarak kolayca oluşturulabilir. DevOps CI/CD boru hattına entegre edilebilen uygulama paketleme, standardizasyon ve dağıtım otomasyonuna olanak sağlarlar.  Örneğin, MySQL arka uç için Azure Veritabanı ile bir Web Uygulamasını hızla dağıtmak istiyorsanız, GitHub galerisindeki bu [QuickStart şablonunu](https://azure.microsoft.com/resources/templates/101-webapp-managed-mysql/) kullanarak uçtan uca dağıtımı gerçekleştirebilirsiniz.

Bu öğreticide, şunları öğrenmek için Azure Kaynak Yöneticisi şablonu ve diğer yardımcı programları kullanıyorsunuz:

> [!div class="checklist"]
> * Azure Kaynak Yöneticisi şablonu kullanarak VNet Service Endpoint ile MySQL sunucusu için bir Azure Veritabanı oluşturma
> * Veritabanı oluşturmak için [mysql komut satırı aracı](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) kullanma
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-an-azure-database-for-mysql-server-with-vnet-service-endpoint-using-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak VNet Service Endpoint ile MySQL sunucusu için bir Azure Veritabanı oluşturma

MySQL sunucusu için bir Azure Veritabanı için JSON şablon umasını almak için [Microsoft.DBforMySQL sunucuları](/azure/templates/microsoft.dbformysql/servers) şablon ubaşvurusu'na gidin. Aşağıda, VNet Service Endpoint ile MySQL için Azure Veritabanı çalıştıran yeni bir sunucu oluşturmak için kullanılabilecek örnek JSON şablonu yer almaktadır.
```json
{
  "apiVersion": "2017-12-01",
  "type": "Microsoft.DBforMySQL/servers",
  "name": "string",
  "location": "string",
  "tags": "string",
  "properties": {
    "version": "string",
    "sslEnforcement": "string",
    "administratorLogin": "string",
    "administratorLoginPassword": "string",
    "storageProfile": {
      "storageMB": "string",
      "backupRetentionDays": "string",
      "geoRedundantBackup": "string"
    }
  },
  "sku": {
    "name": "string",
    "tier": "string",
    "capacity": "string",
    "family": "string"
  },
  "resources": [
    {
      "name": "AllowSubnet",
      "type": "virtualNetworkRules",
      "apiVersion": "2017-12-01",
      "properties": {
        "virtualNetworkSubnetId": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('virtualNetworkName'), parameters('subnetName'))]",
        "ignoreMissingVnetServiceEndpoint": true
      },
      "dependsOn": [
        "[concat('Microsoft.DBforMySQL/servers/', parameters('serverName'))]"
      ]
    }
  ]
}
```
Bu istekte, özelleştirilmesi gereken değerler şunlardır:
+   `name`- MySQL Server'ınızın adını belirtin (etki alanı adı olmadan).
+   `location`- MySQL Sunucunuz için geçerli bir Azure veri merkezi bölgesi belirtin. Örneğin, westus2.
+   `properties/version`- Dağıtmak için MySQL sunucu sürümünü belirtin. Örneğin, 5.6 veya 5.7.
+   `properties/administratorLogin`- Sunucu için MySQL yönetici girişi belirtin. Yönetici oturum açma adı azure_superuser, admin, administrator, root, guest veya public olamaz.
+   `properties/administratorLoginPassword`- Yukarıda belirtilen MySQL yönetici kullanıcısının parolasını belirtin.
+   `properties/sslEnforcement`- SslEnforcement'i etkinleştirmek/devre dışı etmek için Etkin/Devre Dışı'yı belirtin.
+   `storageProfile/storageMB`- Megabaytlarda sunucu için gerekli olan maksimum kullanılabilir depolama boyutunu belirtin. Örneğin, 5120.
+   `storageProfile/backupRetentionDays`- Gün içinde istenilen yedekleme bekletme süresini belirtin. Örneğin, 7. 
+   `storageProfile/geoRedundantBackup`- Geo-DR gereksinimlerine bağlı olarak Etkin/Devre Dışı Belirtin.
+   `sku/tier`- Dağıtım için Temel, Genel Amaç veya MemoryOptimized katmanı belirtin.
+   `sku/capacity`- vCore kapasitesini belirtin. Olası değerler 2, 4, 8, 16, 32 veya 64 içerir.
+   `sku/family`- Sunucu dağıtımı için donanım oluşturma yı seçmek için Gen5'i belirtin.
+   `sku/name`- TierPrefix_family_capacity belirtin. Örneğin B_Gen5_1, GP_Gen5_16, MO_Gen5_32. Bölge ve katman başına geçerli değerleri anlamak için [fiyatlandırma katmanları](./concepts-pricing-tiers.md) belgelerine bakın.
+   `resources/properties/virtualNetworkSubnetId`- Azure MySQL sunucusunun yerleştirilebileceği VNet'te alt netin Azure tanımlayıcısını belirtin. 
+   `tags(optional)`- İsteğe bağlı etiketlerin, faturalandırma vb. kaynakları kategorilere ayırmak için kullanacağınız anahtar değer çiftleri olduğunu belirtin.

Kuruluşunuz için MySQL dağıtımları için Azure Veritabanı'nı otomatikleştirmek için bir Azure Kaynak Yöneticisi şablonu oluşturmak istiyorsanız, öneri önce Azure Quickstart GitHub Galerisi'ndeki örnek [Azure Kaynak Yöneticisi şablonundan](https://github.com/Azure/azure-quickstart-templates/tree/master/101-managed-mysql-with-vnet) başlayıp bunun üzerine oluşturmak olacaktır. 

Azure Kaynak Yöneticisi şablonlarında yeniyseniz ve denemek istiyorsanız, aşağıdaki adımları izleyerek başlayabilirsiniz:
+   Azure Quickstart galerisinden Örnek [Azure Kaynak Yöneticisi şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-managed-mysql-with-vnet) kopyala veya indirin.  
+   Tercihinize göre parametre değerlerini güncelleştirmek ve dosyayı kaydetmek için azuredeploy.parameters.json'u değiştirin. 
+   Aşağıdaki komutları kullanarak Azure MySQL sunucusunu oluşturmak için Azure CLI'yi kullanın

Bu öğreticide kod bloklarını çalıştırmak için tarayıcıda Azure Cloud Shell kullanabilir ya da kendi bilgisayarınıza Azure CLI yükleyebilirsiniz.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

```azurecli-interactive
az login
az group create -n ExampleResourceGroup  -l "West US2"
az group deployment create -g $ ExampleResourceGroup   --template-file $ {templateloc} --parameters $ {parametersloc}
```

## <a name="get-the-connection-information"></a>Bağlantı bilgilerini alma
Sunucunuza bağlanmak için ana bilgisayar bilgilerini ve erişim kimlik bilgilerini sağlamanız gerekir.
```azurecli-interactive
az mysql server show --resource-group myresourcegroup --name mydemoserver
```

Sonuç JSON biçimindedir. **fullyQualifiedDomainName** ve **administratorLogin** bilgilerini not alın.
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mydemoserver.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMySQL/servers/mydemoserver",
  "location": "westus2",
  "name": "mydemoserver",
  "resourceGroup": "myresourcegroup",
 "sku": {
    "capacity": 2,
    "family": "Gen5",
    "name": "GP_Gen5_2",
    "size": null,
    "tier": "GeneralPurpose"
  },
  "sslEnforcement": "Enabled",
  "storageProfile": {
    "backupRetentionDays": 7,
    "geoRedundantBackup": "Disabled",
    "storageMb": 5120
  },
  "tags": null,
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": "5.7"
}
```

## <a name="connect-to-the-server-using-mysql"></a>mysql kullanarak sunucuya bağlanma
MySQL sunucusu için Azure Veritabanınız ile bağlantı kurmak üzere [mysql komut satırı aracını](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) kullanın. Bu örnekte, komut şöyledir:
```cmd
mysql -h mydemoserver.database.windows.net -u myadmin@mydemoserver -p
```

## <a name="create-a-blank-database"></a>Boş veritabanı oluşturma
Sunucuya bağlandıktan sonra, boş bir veritabanı oluşturun.
```sql
mysql> CREATE DATABASE mysampledb;
```

İstemde, bağlantıyı bu yeni oluşturulan veritabanına değiştirmek için şu komutu çalıştırın:
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-the-database"></a>Veritabanında tablo oluşturma
Artık MySQL veritabanı için Azure Veritabanına nasıl bağlanacağınızı bildiğinize göre bazı temel görevleri tamamlayın.

İlk olarak, bir tablo oluşturun ve bu tabloya bazı veriler yükleyin. Envanter bilgilerini depolayan bir tablo oluşturalım.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a>Tablolara veri yükleme
Bir tablonuz olduğuna göre içine bazı veriler ekleyin. Açık komut istemi penceresinde, birkaç veri satırı eklemek için şu sorguyu çalıştırın.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Daha önce oluşturduğunuz tabloda artık iki satırlık örnek verileriniz var.

## <a name="query-and-update-the-data-in-the-tables"></a>Tablolardaki verileri sorgulama ve güncelleştirme
Veritabanı tablosundan bilgi almak için şu sorguyu yürütün.
```sql
SELECT * FROM inventory;
```

Ayrıca tablolardaki verileri de güncelleştirebilirsiniz.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

Siz veri aldıkça satır güncelleştirilir.
```sql
SELECT * FROM inventory;
```

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> * Azure Kaynak Yöneticisi şablonu kullanarak VNet Service Endpoint ile MySQL sunucusu için bir Azure Veritabanı oluşturma
> * Veritabanı oluşturmak için [mysql komut satırı aracı](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) kullanma
> * Örnek verileri yükleme
> * Verileri sorgulama
> * Verileri güncelleştirme
> 
> [MySQL için Azure Veritabanına nasıl uygulama bağlanır](./howto-connection-string.md)
