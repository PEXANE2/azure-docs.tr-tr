---
title: Bağlantı mimarisi
description: Bu belgede, Azure içinden veya Azure dışından veritabanı bağlantıları için Azure SQL bağlantı mimarisi açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: fasttrack-edit
titleSuffix: Azure SQL Database and SQL Data Warehouse
ms.devlang: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: carlrab, vanto
ms.date: 03/09/2020
ms.openlocfilehash: 6fdfbce6dce2428a8f2757b0755e6f982f02240f
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78945865"
---
# <a name="azure-sql-connectivity-architecture"></a>Azure SQL bağlantı mimarisi
> [!NOTE]
> Bu makale Azure SQL Server ve Azure SQL Server 'da oluşturulan SQL veritabanı ve SQL veri ambarı veritabanları için geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır.

> [!IMPORTANT]
> Bu *Makale,* **Azure SQL veritabanı yönetilen örneği**için geçerlidir. [Yönetilen bir örnek Için bağlantı mimarisi](sql-database-managed-instance-connectivity-architecture.md)' ne bakın.

Bu makalede, ağ trafiğini Azure SQL veritabanı veya SQL veri ambarı 'na yönlendirdiği çeşitli bileşenlerin mimarisi açıklanmaktadır. Ayrıca, farklı bağlantı ilkelerini ve Azure içinden bağlanan istemcileri ve Azure dışından bağlanan istemcileri nasıl etkilediğini açıklar. 

## <a name="connectivity-architecture"></a>Bağlantı mimarisi

Aşağıdaki diyagramda Azure SQL veritabanı bağlantı mimarisine yönelik yüksek düzeyde bir genel bakış sunulmaktadır.

![mimariye genel bakış](./media/sql-database-connectivity-architecture/connectivity-overview.png)

Aşağıdaki adımlarda, bir bağlantının Azure SQL veritabanına nasıl kurulduğu açıklanır:

- İstemciler, genel bir IP adresine sahip olan ağ geçidine bağlanır ve 1433 numaralı bağlantı noktasını dinler.
- Ağ Geçidi, etkin bağlantı ilkesine bağlı olarak, trafiği doğru veritabanı kümesine yönlendirir veya proxy 'ye yönlendirir.
- Veritabanı kümesi trafiğinin içinde uygun Azure SQL veritabanına iletilir.

## <a name="connection-policy"></a>Bağlantı ilkesi

Azure SQL veritabanı, SQL veritabanı sunucusunun bağlantı ilkesi ayarı için aşağıdaki üç seçeneği destekler:

- **Yeniden yönlendir (önerilir):** İstemciler, veritabanını barındıran düğüme doğrudan bağlantı kurar, azaltılmış gecikme süresi ve geliştirilmiş işleme için önde gelen bir işlem sağlar. Bu modu kullanmak için istemcilerin şunları yapması gerekir:
   - İstemciden giden iletişimin, 11000 11999 aralığındaki bağlantı noktalarında bulunan bölgedeki tüm Azure IP adreslerine erişmesine izin verin. Daha kolay yönetilmesini sağlamak için SQL için hizmet etiketlerini kullanın.  
   - Bağlantı noktası 1433 üzerinde istemciden Azure SQL veritabanı ağ geçidi IP adreslerine giden iletişime izin verin.

- **Proxy:** Bu modda, tüm bağlantılar Azure SQL veritabanı ağ geçitleri aracılığıyla, artan gecikme süresi ve düşük aktarım hızı için önde gelen aracılardır. Bu modu kullanma bağlantıları için istemcilerin, bağlantı noktası 1433 üzerinde istemciden Azure SQL veritabanı ağ geçidi IP adreslerine giden iletişime izin vermek gerekir.

- **Varsayılan:** Bu, bağlantı ilkesini açıkça `Proxy` veya `Redirect`olarak değiştirmediğiniz takdirde, oluşturulduktan sonra tüm sunucularda etkin olan bağlantı ilkesidir. Varsayılan ilke, Azure 'ın içinden (örneğin, bir Azure sanal makinesinden) kaynaklanan tüm istemci bağlantıları için`Redirect` ve dış kaynaklı tüm istemci bağlantıları için (örneğin, yerel iş istasyonunuzdan gelen bağlantılar) `Proxy`.

 En düşük gecikme süresi ve en yüksek aktarım hızı için `Proxy` bağlantı ilkesi üzerinde `Redirect` bağlantı ilkesini kesinlikle öneririz. Ancak, yukarıda özetlenen ağ trafiğine izin vermek için ek gereksinimleri karşılamanız gerekecektir. İstemci bir Azure sanal makinedir, bunu [hizmet etiketleriyle](../virtual-network/security-overview.md#service-tags)ağ güvenlik grupları (NSG) kullanarak gerçekleştirebilirsiniz. İstemci Şirket içindeki bir iş istasyonundan bağlanıyorsa, şirket güvenlik duvarınız üzerinden ağ trafiğine izin vermek için ağ yöneticinizle birlikte çalışmanız gerekebilir.

## <a name="connectivity-from-within-azure"></a>Azure içinden bağlantı

Azure içinden bağlanıyorsanız bağlantılarınız, varsayılan olarak `Redirect` bağlantı ilkesine sahiptir. `Redirect` ilkesi, TCP oturumu Azure SQL veritabanı 'na kurulduktan sonra, istemci oturumu daha sonra doğru veritabanı kümesine yeniden yönlendirilir ve bu, Azure SQL veritabanı ağ geçidindeki hedef sanal IP 'ye yapılan bir değişikliğe göre kümeye yönlendirilir. Bundan sonra, Azure SQL veritabanı ağ geçidini atlayarak sonraki tüm paketler doğrudan kümeye akar. Aşağıdaki diyagramda bu trafik akışı gösterilmektedir.

![mimariye genel bakış](./media/sql-database-connectivity-architecture/connectivity-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>Azure dışından bağlantı

Azure dışından bağlanıyorsanız, bağlantılarınızın varsayılan olarak `Proxy` bağlantı ilkesi vardır. `Proxy` ilkesi, TCP oturumunun Azure SQL veritabanı ağ geçidi aracılığıyla ve sonraki tüm paketlerin ağ geçidiyle akış üzerinden kurulduğu anlamına gelir. Aşağıdaki diyagramda bu trafik akışı gösterilmektedir.

![mimariye genel bakış](./media/sql-database-connectivity-architecture/connectivity-onprem.png)

> [!IMPORTANT]
> Ayrıca [, dac Ile bağlanmayı](https://docs.microsoft.com/sql/database-engine/configure-windows/diagnostic-connection-for-database-administrators?view=sql-server-2017#connecting-with-dac) etkinleştirmek için 14000-14999 bağlantı noktalarını açın


## <a name="azure-sql-database-gateway-ip-addresses"></a>Azure SQL Veritabanı ağ geçidi IP adresleri

Aşağıdaki tabloda bölgeye göre ağ geçitlerinin IP adresleri listelenir. Bir Azure SQL veritabanına bağlanmak için ağ trafiğinin bölge için **Tüm** ağ geçitlerinden & izin vermeniz gerekir.

Trafiğin belirli bölgelerde yeni ağ geçitlerine nasıl geçirilme ayrıntıları aşağıdaki makalede verilmiştir: [Azure SQL veritabanı trafiğini daha yeni ağ geçitlerine geçirme](sql-database-gateway-migration.md)


| Bölge Adı          | Ağ geçidi IP adresleri |
| --- | --- |
| Orta Avustralya    | 20.36.105.0 |
| Avustralya Central2   | 20.36.113.0 |
| Doğu Avustralya       | 13.75.149.87, 40.79.161.1 |
| Avustralya Güneydoğu | 191.239.192.109, 13.73.109.251 |
| Güney Brezilya         | 104.41.11.5, 191.233.200.14 |
| Orta Kanada       | 40.85.224.249      |
| Doğu Kanada          | 40.86.226.166      |
| Orta ABD           | 13.67.215.62, 52.182.137.15, 23.99.160.139, 104.208.16.96, 104.208.21.1 | 
| Çin Doğu           | 139.219.130.35     |
| Çin Doğu 2         | 40.73.82.1         |
| Çin Kuzey          | 139.219.15.17      |
| Çin Kuzey 2        | 40.73.50.0         |
| Doğu Asya            | 191.234.2.139, 52.175.33.150, 13.75.32.4 |
| Doğu ABD              | 40.121.158.30, 40.79.153.12, 191.238.6.43, 40.78.225.32 |
| Doğu ABD 2            | 40.79.84.180, 52.177.185.181, 52.167.104.0, 191.239.224.107, 104.208.150.3 | 
| Orta Fransa       | 40.79.137.0, 40.79.129.1 |
| Orta Almanya      | 51.4.144.100       |
| Almanya Kuzey Doğu   | 51.5.144.179       |
| Hindistan Orta        | 104.211.96.159     |
| Hindistan Güney          | 104.211.224.146    |
| Hindistan Batı           | 104.211.160.80     |
| Doğu Japonya           | 13.78.61.196, 40.79.184.8, 13.78.106.224, 191.237.240.43, 40.79.192.5 | 
| Batı Japonya           | 104.214.148.156, 40.74.100.192, 191.238.68.11, 40.74.97.10 | 
| Güney Kore - Orta        | 52.231.32.42       |
| Güney Kore - Güney          | 52.231.200.86      |
| Orta Kuzey ABD     | 23.96.178.199, 23.98.55.75, 52.162.104.33 |
| Kuzey Avrupa         | 40.113.93.91, 191.235.193.75, 52.138.224.1 | 
| Norveç Doğu          | 51.120.96.0        |
| Norveç Batı          | 51.120.216.0       |
| Güney Afrika Kuzey   | 102.133.152.0      |
| Güney Afrika Batı    | 102.133.24.0       |
| Orta Güney ABD     | 13.66.62.124, 23.98.162.75, 104.214.16.32   | 
| Güneydoğu Asya      | 104.43.15.0, 23.100.117.95, 40.78.232.3   | 
| BAE Orta          | 20.37.72.64        |
| BAE Kuzey            | 65.52.248.0        |
| Güney Birleşik Krallık             | 51.140.184.11      |
| Batı Birleşik Krallık              | 51.141.8.11        |
| Orta Batı ABD      | 13.78.145.25       |
| Batı Avrupa          | 40.68.37.158, 191.237.232.75, 104.40.168.105  |
| Batı ABD              | 104.42.238.205, 23.99.34.75, 13.86.216.196   |
| Batı ABD 2            | 13.66.226.202      |
|                      |                    |



## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL veritabanı sunucusu için Azure SQL veritabanı bağlantı ilkesini değiştirme hakkında daha fazla bilgi için bkz. [Conn-Policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).
- ADO.NET 4,5 veya sonraki bir sürümünü kullanan istemciler için Azure SQL veritabanı bağlantı davranışı hakkında bilgi için bkz. [1433 sonrasındaki bağlantı noktaları ADO.NET 4,5](sql-database-develop-direct-route-ports-adonet-v12.md).
- Genel uygulama geliştirmeye genel bakış bilgileri için bkz. [SQL veritabanı uygulaması geliştirmeye genel bakış](sql-database-develop-overview.md).
