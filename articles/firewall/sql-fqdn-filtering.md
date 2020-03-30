---
title: Azure Güvenlik Duvarı uygulama kurallarını SQL FQDN'lerle yapılandırma
description: Bu makalede, Azure Güvenlik Duvarı uygulama kurallarında SQL FQDN'leri nasıl yapılandırabileceğinizi öğreneceksiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 07/19/2019
ms.author: victorh
ms.openlocfilehash: 858cfc9a8c15f1e33e688bb5086a58f194e7173f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79501503"
---
# <a name="configure-azure-firewall-application-rules-with-sql-fqdns"></a>Azure Güvenlik Duvarı uygulama kurallarını SQL FQDN'lerle yapılandırma

> [!IMPORTANT]
> SQL FQDN'lere sahip Azure Güvenlik Duvarı uygulama kuralları şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Artık Azure Güvenlik Duvarı uygulama kurallarını SQL FQDN'lerle yapılandırabilirsiniz. Bu, sanal ağlarınızdaki erişimi yalnızca belirtilen SQL sunucu örnekleriyle sınırlamanızı sağlar.

SQL FQDN'ler ile trafiği filtreleyebilirsiniz:

- VNet'lerinizden Azure SQL Veritabanı'na veya Azure SQL Veri Ambarı'na kadar. Örneğin: Yalnızca *sql-server1.database.windows.net*erişime izin verin.
- Şirket içi, VNet'lerinizde çalışan Azure SQL Yönetilen Örnekleriveya SQL IaaS'a kadar.
- VNet'lerinizde çalışan Azure SQL Yönetilen Örnekleri veya SQL IaaS'ye konuşanlardan

Genel önizleme sırasında, SQL FQDN filtreleme yalnızca [proxy modunda](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-architecture#connection-policy) desteklenir (bağlantı noktası 1433). Varsayılan yönlendirme modunda SQL kullanıyorsanız, [ağ kurallarının](overview.md#network-traffic-filtering-rules)bir parçası olarak SQL hizmet etiketini kullanarak erişimfiltreleyebilirsiniz.
SQL IaaS trafiği için varsayılan olmayan bağlantı noktaları kullanıyorsanız, bu bağlantı noktalarını güvenlik duvarı uygulama kurallarında yapılandırabilirsiniz.

SQL FQDN'lere sahip uygulama kuralları şu anda tüm bölgelerde Azure portalı, Azure CLI, REST ve şablonlar üzerinden kullanılabilir.

## <a name="configure-using-azure-cli"></a>Azure CLI kullanarak yapılandırma

1. Azure [CLI'yi kullanarak bir Azure Güvenlik Duvarı dağıtın.](deploy-cli.md)
2. Trafiği Azure SQL Veritabanı, SQL Veri Ambarı veya SQL Yönetilen Örnek'e filtrelerseniz, SQL bağlantı modunun **Proxy**olarak ayarlandığından emin olun. SQL bağlantı modunu nasıl değiştirebilirsiniz öğrenmek için [Azure SQL Bağlantı Ayarları'na](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-settings#change-connection-policy-via-azure-cli)bakın.

   > [!NOTE]
   > SQL *proxy* modu *yeniden yönlendirmeye*göre daha fazla gecikme ye neden olabilir. Azure'a bağlanan istemciler için varsayılan olan yeniden yönlendirme modunu kullanmaya devam etmek istiyorsanız, güvenlik duvarı [ağı kurallarında](tutorial-firewall-deploy-portal.md#configure-a-network-rule)SQL [servis etiketini](service-tags.md) kullanarak erişime filtre uygulayabilirsiniz.

3. Bir SQL sunucusuna erişime izin vermek için bir uygulama kuralını SQL FQDN ile yapılandırın:

   ```azurecli
   az extension add -n azure-firewall

   az network firewall application-rule create \
   -g FWRG \
   -f azfirewall \
   -c FWAppRules \
   -n srule \
   --protocols mssql=1433 \
   --source-addresses 10.0.0.0/24 \
   --target-fqdns sql-serv1.database.windows.net
   ```

## <a name="configure-using-the-azure-portal"></a>Azure portalını kullanarak yapılandırma
1. Azure [CLI'yi kullanarak bir Azure Güvenlik Duvarı dağıtın.](deploy-cli.md)
2. Trafiği Azure SQL Veritabanı, SQL Veri Ambarı veya SQL Yönetilen Örnek'e filtrelerseniz, SQL bağlantı modunun **Proxy**olarak ayarlandığından emin olun. SQL bağlantı modunu nasıl değiştirebilirsiniz öğrenmek için [Azure SQL Bağlantı Ayarları'na](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-settings#change-connection-policy-via-azure-cli)bakın.  

   > [!NOTE]
   > SQL *proxy* modu *yeniden yönlendirmeye*göre daha fazla gecikme ye neden olabilir. Azure'a bağlanan istemciler için varsayılan olan yeniden yönlendirme modunu kullanmaya devam etmek istiyorsanız, güvenlik duvarı [ağı kurallarında](tutorial-firewall-deploy-portal.md#configure-a-network-rule)SQL [servis etiketini](service-tags.md) kullanarak erişime filtre uygulayabilirsiniz.
3. Uygun protokol, bağlantı noktası ve SQL FQDN ile uygulama kuralını ekleyin ve sonra **Kaydet'i**seçin.
   ![SQL FQDN ile uygulama kuralı](media/sql-fqdn-filtering/application-rule-sql.png)
4. Trafiği güvenlik duvarından filtreleyen bir VNet'teki sanal makineden SQL'e erişin. 
5. Azure [Güvenlik Duvarı günlüklerinin](log-analytics-samples.md) trafiğe izin verildiğini gösterdiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

SQL proxy ve yönlendirme modları hakkında bilgi edinmek için [Azure SQL veritabanı bağlantı mimarisine](../sql-database/sql-database-connectivity-architecture.md)bakın.