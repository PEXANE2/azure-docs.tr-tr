---
title: Azure HDInsight'ta Özel Apache Ambari veritabanı
description: Kendi özel Apache Ambari veritabanınızla HDInsight kümelerini nasıl oluşturacağınızı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/29/2019
ms.author: hrasheed
ms.openlocfilehash: e7351e2f39c7e4eed84f4a47e3eeb2214a062a94
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240167"
---
# <a name="set-up-hdinsight-clusters-with-a-custom-ambari-db"></a>ÖZEL Ambari DB ile HDInsight kümelerini ayarlama

Apache Ambari, bir Apache Hadoop kümesinin yönetimini ve izlenmesini kolaylaştırır. Ambari kullanımı kolay bir web kullanıcı arama hizmeti ve REST API sağlar. Ambari HDInsight kümelerinde yer alan ve kümeizlemek ve yapılandırma değişiklikleri yapmak için kullanılır.

[HDInsight'ta kümeleri ayarlama](hdinsight-hadoop-provision-linux-clusters.md)gibi diğer makalelerde açıklandığı gibi normal küme oluşturmada Ambari, HDInsight tarafından yönetilen ve kullanıcılar tarafından erişilemeyen bir [S0 Azure SQL veritabanında](../sql-database/sql-database-dtu-resource-limits-single-databases.md#standard-service-tier) dağıtılır.

Özel Ambari DB özelliği, yönettiğiniz harici bir veritabanında ambari'yi yeni bir küme ve kurulum adabına dağıtmanızı sağlar. Dağıtım, Azure Kaynak Yöneticisi şablonuyla yapılır. Bu özelliğin aşağıdaki avantajları vardır:

- Özelleştirme - veritabanının boyutunu ve işleme kapasitesini seçersiniz. Yoğun iş yüklerini işleyen büyük kümeler varsa, daha düşük belirtimlere sahip bir Ambari veritabanı yönetim işlemleri için bir darboğaz alabilirsiniz.
- Esneklik - veritabanını gereksinimlerinize uyacak şekilde gerektiği gibi ölçeklendirebilirsiniz.
- Denetim - Veritabanınızın yedeklemelerini ve güvenliğini kuruluş gereksinimlerinize uygun şekilde yönetebilirsiniz.

Bu makalenin geri kalanı aşağıdaki noktaları tartışır:

- özel Ambari DB özelliğini kullanmak için gereksinimleri
- Apache Ambari için kendi harici veritabanınızı kullanarak HDInsight kümelerini sağlamak için gerekli adımlar

## <a name="custom-ambari-db-requirements"></a>Özel Ambari DB gereksinimleri

Tüm küme türleri ve sürümleri ile özel bir Ambari DB dağıtabilirsiniz. Birden çok küme aynı Ambari DB kullanamaz.

Özel Ambari DB aşağıdaki diğer gereksinimleri vardır:

- Varolan bir Azure SQL DB sunucunuz ve veritabanınız olmalıdır.
- Ambari kurulumu için sağladığınız veritabanı boş olmalıdır. Varsayılan dbo şemasında tablo olmamalıdır.
- Veritabanına bağlanmak için kullanılan kullanıcının veritabanında SELECT, CREATE TABLE ve INSERT izinleri olmalıdır.
- Ambari'yi barındıracağınız Azure SQL sunucusunda [Azure hizmetlerine erişime izin](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#azure-portal-steps) verme seçeneğini açın.
- HDInsight hizmetinden yönetim IP adreslerinin SQL Server'da izin verilmesi gerekir. SQL sunucu güvenlik duvarına eklenmesi gereken IP adreslerinin listesi için [HDInsight yönetim IP adreslerine](hdinsight-management-ip-addresses.md) bakın.

Apache Ambari DB'nizi harici bir veritabanında barındırdığınızda aşağıdaki noktaları hatırlayın:

- Ambari'yi tutan Azure SQL DB'nin ek maliyetlerinden siz sorumlusunuz.
- Özel Ambari DB'nizi düzenli olarak yedekle. Azure SQL Veritabanı otomatik olarak yedekleme ler oluşturur, ancak yedekleme bekletme zaman çerçevesi değişir. Daha fazla bilgi için bkz: [Otomatik SQL Veritabanı yedeklemeleri hakkında bilgi edinin.](../sql-database/sql-database-automated-backups.md)

## <a name="deploy-clusters-with-a-custom-ambari-db"></a>Özel Ambari DB ile kümeleri dağıtma

Kendi harici Ambari veritabanınızı kullanan bir HDInsight kümesi oluşturmak için [özel Ambari DB Quickstart şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-custom-ambari-db)kullanın.

Yeni kümeniz ve `azuredeploy.parameters.json` Ambari'yi tutacak veritabanı hakkında bilgi belirtmek için parametreleri düzenleyin.

Azure CLI'yi kullanarak dağıtıma başlayabilirsiniz. Kümenizi dağıtmak istediğiniz kaynak grubuyla değiştirin. `<RESOURCEGROUPNAME>`

```azurecli
az group deployment create --name HDInsightAmbariDBDeployment \
    --resource-group <RESOURCEGROUPNAME> \
    --template-file azuredeploy.json \
    --parameters azuredeploy.parameters.json
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure HDInsight’ta dış meta veri depolarını kullanma](hdinsight-use-external-metadata-stores.md)