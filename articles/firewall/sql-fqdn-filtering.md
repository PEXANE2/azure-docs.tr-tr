---
title: SQL FQDN’lerle Azure Güvenlik Duvarı uygulama kurallarını yapılandırma
description: Bu makalede, Azure Güvenlik Duvarı uygulama kurallarında SQL FQDN 'leri yapılandırmayı öğreneceksiniz.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 06/18/2020
ms.author: victorh
ms.openlocfilehash: 744fe22b6b2c9fbeb9b149760145267ccb6fa6f8
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89435221"
---
# <a name="configure-azure-firewall-application-rules-with-sql-fqdns"></a>SQL FQDN’lerle Azure Güvenlik Duvarı uygulama kurallarını yapılandırma

Artık, Azure Güvenlik Duvarı uygulama kurallarını SQL FQDN 'leri ile yapılandırabilirsiniz. Bu, sanal ağlarınızdaki erişimi yalnızca belirtilen SQL Server örneklerine sınırlamanıza izin verir.

SQL FQDN'leri ile şu trafiği filtreleyebilirsiniz:

- Sanal ağlarınızdaki Azure SQL veritabanı veya Azure SYNAPSE Analytics 'e. Örneğin: yalnızca *SQL-Server1.Database.Windows.net*erişimine izin verin.
- Şirket içinden Azure SQL yönetilen örneklerine veya sanal ağlarınızdaki SQL IaaS 'ye çalışır.
- Sanal ağlarınız üzerinde çalışan Azure SQL yönetilen örneklerine veya SQL IaaS 'ye bağlı noktadan

SQL FQDN filtrelemesi yalnızca [proxy modunda](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-architecture#connection-policy) desteklenir (bağlantı noktası 1433). Varsayılan yeniden yönlendirme modunda SQL kullanıyorsanız, [ağ kurallarının](features.md#network-traffic-filtering-rules)bir PARÇASı olarak SQL hizmeti etiketini kullanarak erişimi filtreleyebilirsiniz.
SQL IaaS trafiği için varsayılanın dışında bağlantı noktaları kullanıyorsanız bu bağlantı noktalarını güvenlik duvarı uygulama kurallarından yapılandırabilirsiniz.

## <a name="configure-using-azure-cli"></a>Azure CLı kullanarak yapılandırma

1. [Azure CLI kullanarak bir Azure Güvenlik Duvarı](deploy-cli.md)dağıtın.
2. Trafiği Azure SQL veritabanı, Azure SYNAPSE Analytics veya SQL yönetilen örneği 'ne filtrelemeniz durumunda SQL bağlantı modunun **Ara sunucu**olarak ayarlandığından emin olun. SQL bağlantı modunu değiştirme hakkında bilgi edinmek için bkz. [Azure SQL bağlantı ayarları](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-settings#change-connection-policy-via-azure-cli).

   > [!NOTE]
   > SQL *proxy* modu *yeniden yönlendirmeye*kıyasla daha fazla gecikme süresine neden olabilir. Azure 'da bağlanan istemciler için varsayılan olan yeniden yönlendirme modunu kullanmaya devam etmek istiyorsanız, erişimi güvenlik duvarı [ağ KURALLARıNDA](tutorial-firewall-deploy-portal.md#configure-a-network-rule)SQL [hizmeti etiketi](service-tags.md) kullanarak filtreleyebilirsiniz.

3. SQL Server 'a erişim izni vermek için SQL FQDN ile bir uygulama kuralı yapılandırın:

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
1. [Azure CLI kullanarak bir Azure Güvenlik Duvarı](deploy-cli.md)dağıtın.
2. Trafiği Azure SQL veritabanı, Azure SYNAPSE Analytics veya SQL yönetilen örneği 'ne filtrelemeniz durumunda SQL bağlantı modunun **Ara sunucu**olarak ayarlandığından emin olun. SQL bağlantı modunu değiştirme hakkında bilgi edinmek için bkz. [Azure SQL bağlantı ayarları](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-settings#change-connection-policy-via-azure-cli).  

   > [!NOTE]
   > SQL *proxy* modu *yeniden yönlendirmeye*kıyasla daha fazla gecikme süresine neden olabilir. Azure 'da bağlanan istemciler için varsayılan olan yeniden yönlendirme modunu kullanmaya devam etmek istiyorsanız, erişimi güvenlik duvarı [ağ KURALLARıNDA](tutorial-firewall-deploy-portal.md#configure-a-network-rule)SQL [hizmeti etiketi](service-tags.md) kullanarak filtreleyebilirsiniz.
3. Uygulama kuralını uygun protokol, bağlantı noktası ve SQL FQDN 'SI ile ekleyin ve ardından **Kaydet**' i seçin.
   ![SQL FQDN ile uygulama kuralı](media/sql-fqdn-filtering/application-rule-sql.png)
4. Ağ üzerinden trafiği filtreleyen bir sanal makineden SQL 'e erişin. 
5. [Azure Güvenlik Duvarı günlüklerinin](log-analytics-samples.md) trafiğe izin verildiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

SQL proxy ve yeniden yönlendirme modları hakkında bilgi edinmek için bkz. [Azure SQL veritabanı bağlantı mimarisi](../azure-sql/database/connectivity-architecture.md).