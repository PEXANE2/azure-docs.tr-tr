---
title: Azure portal ile SAP Çözümleri için Azure Izleyici 'yi dağıtma
description: Azure portal ile SAP Çözümleri için Azure Izleyici 'yi dağıtma
author: sameeksha91
ms.author: sakhare
ms.topic: how-to
ms.service: virtual-machines-sap
ms.date: 08/17/2020
ms.openlocfilehash: d9febb4efba85d47abe1cc11a3cb52dc0393c036
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101672007"
---
# <a name="deploy-azure-monitor-for-sap-solutions-with-azure-portal"></a>Azure portal ile SAP Çözümleri için Azure Izleyici dağıtma

SAP Çözümleri için Azure Izleyici kaynakları [Azure Portal](https://azure.microsoft.com/features/azure-portal)aracılığıyla oluşturulabilir. Bu yöntem, SAP Çözümleri için Azure Izleyici 'yi dağıtmak ve sağlayıcıları yapılandırmak için tarayıcı tabanlı bir kullanıcı arabirimi sağlar.

## <a name="sign-in-to-azure-portal"></a>Azure portalda oturum açın

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-monitoring-resource"></a>İzleme kaynağı oluştur

1. **Azure Marketi**'Nden **SAP Çözümleri için Azure İzleyicisi** ' ni seçin.

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-1.png" alt-text="Görüntüde, Azure Marketi 'nden SAP Çözümleri için Azure Izleyicisi teklifinin seçilmesi gösterilmektedir." lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-1.png":::

2. **Temel bilgiler** sekmesinde gerekli değerleri sağlayın. Uygulanabiliyorsa, mevcut bir Log Analytics çalışma alanını kullanabilirsiniz.

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-2.png" alt-text="Azure portal yapılandırma seçeneklerini görüntüleyin." lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-2.png":::

3. Bir sanal ağ seçerken, izlemek istediğiniz sistemlerin bu VNET 'in içinden erişilebilir olduğundan emin olun. 

   > [!IMPORTANT]
   > Microsoft ile veri paylaşımı için **Paylaşım** ' i seçmek, destek ekiplerimizin ek destek sağlamasına olanak sağlar.

## <a name="configure-providers"></a>Sağlayıcıları yapılandırma

### <a name="sap-hana-provider"></a>SAP HANA sağlayıcı 

1. Yapılandırmak istediğiniz sağlayıcıları eklemek için **sağlayıcı** sekmesini seçin. Birden çok sağlayıcıyı bir veya daha sonra ekleyebilirsiniz veya izleme kaynağını dağıttıktan sonra ekleyebilirsiniz. 

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-3.png" alt-text="SAP Çözümleri için Azure Izleyicisine ek sağlayıcılar eklemek için sağlayıcı sekmesini gösterir." lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-3.png":::

2. **Sağlayıcı Ekle** ' yi seçin ve açılan listeden **SAP HANA** seçin. 

   > [!IMPORTANT]
   > SAP HANA sağlayıcının SAP HANA ' Ana ' düğümü için yapılandırıldığından emin olun.

3. HANA sunucusu için özel IP 'yi girin.

4. Kullanmak istediğiniz veritabanı kiracının adını girin. Herhangi bir kiracıyı seçebilirsiniz, ancak daha geniş bir izleme alanı dizisi sağladığından **SystemDB** kullanılması önerilir. 

5. HANA veritabanınıza ilişkin SQL bağlantı noktası numarasını girin. Bağlantı noktası numarası **[3]**[  +  **örnek #]**  +  **[13]** biçiminde olmalıdır. Örneğin, 30013. 

6. Kullanmak istediğiniz veritabanı kullanıcı adını girin. Veritabanı kullanıcısına, **izleme** ve **Katalog okuma** rollerinin atanmış olduğundan emin olun. 

7. İşiniz bittiğinde **Sağlayıcı Ekle**' yi seçin. Gerektiğinde daha fazla sağlayıcı eklemeye devam edin veya dağıtımı tamamladıktan sonra **gözden geçir + oluştur** ' u seçin.

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-4.png" alt-text="Sağlayıcı bilgileri eklenirken yapılandırma seçeneklerinin görüntüsü." lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-4.png":::

### <a name="high-availability-cluster-pacemaker-provider"></a>Yüksek kullanılabilirlik kümesi (pacemaker) sağlayıcısı

1. Açılan listeden **yüksek kullanılabilirlik kümesini (Paceyapıcısı)** seçin. 

   > [!IMPORTANT]
   > Yüksek kullanılabilirlik kümesi (pacemaker) sağlayıcısını yapılandırmak için ha_cluster_provider her bir düğümde yüklü olduğundan emin olun. Daha fazla bilgi için bkz. [ha kümesi Dışarı Aktarıcı](https://github.com/ClusterLabs/ha_cluster_exporter#installation)

2. Prometheus uç noktasını biçiminde girin http://IP:9664/metrics . 
 
3. Sistem KIMLIĞINI (SID), ana bilgisayar adını ve küme adını girin.

4. İşiniz bittiğinde **Sağlayıcı Ekle**' yi seçin. Gerektiğinde daha fazla sağlayıcı eklemeye devam edin veya dağıtımı tamamladıktan sonra **gözden geçir + oluştur** ' u seçin.

   :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-5.png" alt-text="Görüntü, HA kümesi pacemaker sağlayıcısıyla ilgili seçenekleri gösterir." lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-5.png":::


### <a name="os-linux-provider"></a>OS (Linux) sağlayıcısı 

1. Açılan listeden OS (Linux) seçeneğini belirleyin 

> [!IMPORTANT]
> İşletim sistemi (Linux) sağlayıcısını yapılandırmak için, Node_Exporter her bir BareMetal örneğine yüklendiğinden emin olun. Daha fazla bilgi için bkz. [Node_Exporter](https://github.com/prometheus/node_exporter)

2. BareMetal örneği için tanımlayıcı olacak bir ad girin.
3. Düğüm verme uç noktasını biçiminde girin http://IP:9100/metrics .
4. İşiniz bittiğinde **Sağlayıcı Ekle**' yi seçin. Gerektiğinde daha fazla sağlayıcı eklemeye devam edin veya dağıtımı tamamladıktan sonra **gözden geçir + oluştur**' u seçin   . 


### <a name="microsoft-sql-server-provider"></a>Microsoft SQL Server sağlayıcı

1. Microsoft SQL Server sağlayıcıyı eklemeden önce, sağlayıcıyı yapılandırmak için gerekli izinlere sahip bir kullanıcı oluşturmak için aşağıdaki betiği SQL Server Management Studio çalıştırmalısınız.

   ```sql
   USE [<Database to monitor>]
   DROP USER [AMS]
   GO
   USE [master]
   DROP USER [AMS]
   DROP LOGIN [AMS]
   GO
   CREATE LOGIN [AMS] WITH PASSWORD=N'<password>', DEFAULT_DATABASE=[<Database to monitor>], DEFAULT_LANGUAGE=[us_english], CHECK_EXPIRATION=OFF, CHECK_POLICY=OFF
   CREATE USER AMS FOR LOGIN AMS
   ALTER ROLE [db_datareader] ADD MEMBER [AMS]
   ALTER ROLE [db_denydatawriter] ADD MEMBER [AMS]
   GRANT CONNECT TO AMS
   GRANT VIEW SERVER STATE TO AMS
   GRANT VIEW SERVER STATE TO AMS
   GRANT VIEW ANY DEFINITION TO AMS
   GRANT EXEC ON xp_readerrorlog TO AMS
   GO
   USE [<Database to monitor>]
   CREATE USER [AMS] FOR LOGIN [AMS]
   ALTER ROLE [db_datareader] ADD MEMBER [AMS]
   ALTER ROLE [db_denydatawriter] ADD MEMBER [AMS]
   GO
   ``` 

2. **Sağlayıcı Ekle** ' yi seçin ve açılan listeden **Microsoft SQL Server** seçin. 

3. Microsoft SQL Server ile ilişkili bilgileri kullanarak alanları doldurun. 

4. İşiniz bittiğinde **Sağlayıcı Ekle**' yi seçin. Gerektiğinde daha fazla sağlayıcı eklemeye devam edin veya dağıtımı tamamladıktan sonra **gözden geçir + oluştur** ' u seçin.

     :::image type="content" source="./media/azure-monitor-sap/azure-monitor-quickstart-6.png" alt-text="Görüntüde Microsoft SQL Server sağlayıcısı eklemeyle ilgili bilgiler gösterilir." lightbox="./media/azure-monitor-sap/azure-monitor-quickstart-6.png":::

## <a name="next-steps"></a>Sonraki adımlar

[SAP Çözümleri Için Azure izleyici](azure-monitor-overview.md) hakkında daha fazla bilgi edinin
