---
title: SAP Solutions sağlayıcıları için Azure Izleyici | Microsoft Docs
description: Bu makale, SAP Çözümleri için Azure izleyici hakkında sık sorulan soruların yanıtlarını sağlar
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 5e514964-c907-4324-b659-16dd825f6f87
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/30/2020
ms.author: radeltch
ms.openlocfilehash: 267d600270e834cf4f1f077452fda7459fac3029
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86525458"
---
# <a name="azure-monitor-for-sap-solutions-providers-preview"></a>SAP Solutions sağlayıcıları için Azure izleyici (Önizleme)

## <a name="overview"></a>Genel Bakış  

SAP Çözümleri için Azure Izleyici bağlamında, *sağlayıcı türü* belirli bir *sağlayıcıyı*ifade eder. Örneğin, SAP yatay içindeki belirli bir bileşen için SAP HANA veritabanı gibi yapılandırılmış *SAP HANA*. Sağlayıcı, ilgili bileşene ait bağlantı bilgilerini içerir ve bu bileşenden telemetri verileri toplamaya yardımcı olur. SAP Çözümleri kaynağı (SAP izleme kaynağı olarak da bilinir) için bir Azure Izleyici, aynı sağlayıcı türünün veya birden çok sağlayıcı türündeki birden çok sağlayıcı ile yapılandırılabilir.
   
Müşteriler, SAP yatay içindeki ilgili bileşenden veri toplamayı etkinleştirmek için farklı sağlayıcı türlerini yapılandırmayı seçebilirler. Örneğin, müşteriler SAP HANA sağlayıcı türü için bir sağlayıcıyı, yüksek kullanılabilirlik kümesi sağlayıcısı türü için başka bir sağlayıcıyı yapılandırabilir ve bu şekilde devam edebilir.  

Müşteriler aynı SAP izleyici kaynağını ve ilişkili yönetilen grubu yeniden kullanmak için belirli bir sağlayıcı türünün birden çok sağlayıcısını yapılandırmayı da tercih edebilir. Yönetilen kaynak grubu hakkında daha fazla bilgi edinin. Genel önizleme için aşağıdaki sağlayıcı türleri desteklenir:   
- SAP HANA
- Yüksek kullanılabilirlik kümesi
- Microsoft SQL Server

![SAP Solutions sağlayıcıları için Azure Izleyici](./media/azure-monitor-sap/azure-monitor-providers.png)

Müşteriler, SAP Monitor kaynağını dağıtma sırasında kullanılabilir sağlayıcı türlerinden en az bir sağlayıcıyı yapılandırmak için önerilir. Bir sağlayıcı yapılandırarak, müşteriler sağlayıcının yapılandırıldığı ilgili bileşenden veri toplamayı başlatır.   

Müşteriler SAP izleyici kaynağını dağıtırken herhangi bir sağlayıcı yapılandırmadığında, SAP izleyici kaynağı başarıyla dağıtılırsa, hiçbir telemetri verisi toplanmaz. Müşterilerin, Azure portal içindeki SAP Monitor kaynağı aracılığıyla dağıtımdan sonra sağlayıcı ekleme seçeneği vardır. Müşteriler, her zaman SAP Monitor kaynağından sağlayıcı ekleyebilir veya silebilir.

> [!Tip]
> Microsoft 'un belirli bir sağlayıcıyı uygulamasını uygulamasını istiyorsanız, lütfen bu belgenin sonundaki bağlantıyı geri bildirimde bulunun veya hesap ekibinize ulaşın.  

## <a name="provider-type-sap-hana"></a>Sağlayıcı türü SAP HANA

Müşteriler, SAP HANA veritabanından veri toplamayı etkinleştirmek için *SAP HANA* bir veya daha fazla sağlayıcı türü sağlayıcısı yapılandırabilir. SAP HANA sağlayıcı, SQL bağlantı noktası üzerinden SAP HANA veritabanına bağlanır, veri kaynağından telemetri verilerini çeker ve müşteri aboneliğinde Log Analytics çalışma alanına gönderir. SAP HANA sağlayıcısı SAP HANA veritabanından her 1 dakikada bir veri toplar.  

Genel önizlemede, müşteriler SAP HANA sağlayıcı ile aşağıdaki verileri görmeyi bekleyebilir: temel altyapı kullanımı, SAP HANA ana bilgisayar durumu, SAP HANA sistem çoğaltması ve SAP HANA yedekleme telemetri verileri. SAP HANA sağlayıcıyı yapılandırmak için ana bilgisayar IP adresi, HANA SQL bağlantı noktası numarası ve SYSTEMDB Kullanıcı adı ve parola gereklidir. Müşterilerin SAP HANA sağlayıcısı 'nı SYSTEMDB 'ye karşı yapılandırmak önerilir, ancak ek sağlayıcılar diğer veritabanı Kiracılarına karşı yapılandırılabilir.

![SAP Solutions sağlayıcıları için Azure Izleyici-SAP HANA](./media/azure-monitor-sap/azure-monitor-providers-hana.png)

## <a name="provider-type-high-availability-cluster"></a>Sağlayıcı türü yüksek kullanılabilirlik kümesi
Müşteriler, bir veya daha fazla sağlayıcı türü *yüksek kullanılabilirlik kümesi* SAĞLAYıCıSıNı, SAP yatay Içindeki pacemaker kümesinden veri toplamayı etkinleştirecek şekilde yapılandırabilir. Yüksek kullanılabilirlik kümesi sağlayıcısı, [ha_cluster_exporter](https://github.com/ClusterLabs/ha_cluster_exporter) uç noktası kullanarak pacemaker 'a bağlanır, verileri veritabanından çeker ve müşteri aboneliğinde Log Analytics çalışma alanına gönderir. Yüksek kullanılabilirlik kümesi sağlayıcısı, pacemaker 'dan her 60 saniyede bir veri toplar.  

Genel önizlemede, müşteriler yüksek kullanılabilirlik kümesi sağlayıcısıyla aşağıdaki verileri görmeyi bekleyebilir:   
 - Düğüm ve kaynak durumu toplama olarak temsil edilen küme durumu 
 - [bilgisayarlardan](https://github.com/ClusterLabs/ha_cluster_exporter/blob/master/doc/metrics.md) 

![SAP çözüm sağlayıcıları için Azure Izleyici-yüksek kullanılabilirlik kümesi](./media/azure-monitor-sap/azure-monitor-providers-pacemaker-cluster.png)

Yüksek kullanılabilirlik kümesi sağlayıcısını yapılandırmak için, söz konusu iki temel adım vardır: 
1. Pacemaker kümesi içindeki *her* bir düğüme [ha_cluster_exporter](https://github.com/ClusterLabs/ha_cluster_exporter) yüklemesi 
    - Müşteriler, yüksek kullanılabilirlik kümesi dağıtmak için Azure Otomasyonu betikleri kullanabilir. Betikler her bir küme düğümüne [ha_cluster_exporter](https://github.com/ClusterLabs/ha_cluster_exporter) yükler.  
    - veya müşteriler el ile yükleme gerçekleştirebilir ve [Bu sayfadaki](https://github.com/ClusterLabs/ha_cluster_exporter) adımları takip edebilir 
2. Pacemaker kümesi içindeki *her* düğümde yüksek kullanılabilirliğe sahip küme sağlayıcısını yapılandırma  
  Yüksek kullanılabilirlik kümesi sağlayıcısını yapılandırmak için, Prometheus URL 'SI, küme adı, ana bilgisayar adı ve sistem KIMLIĞI gereklidir.   
  Müşterilerin her küme düğümü için bir sağlayıcıyı yapılandırması önerilir.   

## <a name="provider-type-microsoft-sql-server"></a>Sağlayıcı türü Microsoft SQL Server

Müşteriler, [sanal makinelerde SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/)veri toplamayı etkinleştirmek için *Microsoft SQL Server* bir veya daha fazla sağlayıcı türü sağlayıcısı yapılandırabilir. SQL Server sağlayıcı, SQL bağlantı noktası üzerinden Microsoft SQL Server bağlanır, veritabanından telemetri verileri çeker ve müşteri aboneliğinde Log Analytics çalışma alanına gönderir. SQL Server, SQL kimlik doğrulaması için yapılandırılmalı ve bir SQL Server oturum açması gerekir ve bu, sağlayıcının varsayılan veritabanı olarak SAP DB ile oluşturulmalıdır. SQL Server sağlayıcı, SQL Server 'dan her saate kadar her 60 saniyede veri toplar.  

Genel önizlemede, müşteriler SQL Server sağlayıcı ile aşağıdaki verileri görmeyi bekleyebilir: temeldeki altyapı kullanımı, popüler SQL deyimleri, en büyük tablo, SQL Server hata günlüklerinde kaydedilen sorunlar ve diğer sorunları engelleme.  

Microsoft SQL Server sağlayıcıyı yapılandırmak için, SAP sistem KIMLIĞI, ana bilgisayar IP adresi, SQL Server bağlantı noktası numarası ve SQL Server oturum açma adı ve parolası gerekir.

![SAP Solutions sağlayıcıları için Azure Izleyici-SQL](./media/azure-monitor-sap/azure-monitor-providers-sql.png)

## <a name="next-steps"></a>Sonraki adımlar

- SAP Çözümleri için ilk Azure Izleyici kaynağını oluşturun.
- SAP Çözümleri için Azure Izleyici hakkında sorularınız mı var? [SSS](./azure-monitor-faq.md) bölümüne bakın
