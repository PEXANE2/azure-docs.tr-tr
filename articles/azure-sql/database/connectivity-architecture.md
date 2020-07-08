---
title: Azure SQL Veritabanı Bağlantı Mimarisi
description: Bu belgede, Azure içinden veya Azure dışından veritabanı bağlantıları için Azure SQL veritabanı bağlantı mimarisi açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: fasttrack-edit, sqldbrb=1
titleSuffix: Azure SQL Database and Azure Synapse Analytics
ms.devlang: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: carlrab, vanto
ms.date: 06/26/2020
ms.openlocfilehash: 3d8bf23dcb9bfc390b9e165bcf38bf8feea5135f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85833681"
---
# <a name="azure-sql-database-and-azure-synapse-analytics-connectivity-architecture"></a>Azure SQL veritabanı ve Azure SYNAPSE Analytics bağlantı mimarisi
[!INCLUDE[appliesto-sqldb-asa](../includes/appliesto-sqldb-asa.md)]

Bu makalede, ağ trafiğini Azure SQL veritabanı veya Azure SYNAPSE Analytics 'teki bir sunucuya yönlendirecek çeşitli bileşenlerin mimarisi açıklanmaktadır. Ayrıca, farklı bağlantı ilkelerini ve Azure içinden bağlanan istemcileri ve Azure dışından bağlanan istemcileri nasıl etkilediğini açıklar.

> [!IMPORTANT]
> Bu *Makale,* **Azure SQL yönetilen örneği**için geçerlidir. [Yönetilen bir örnek Için bağlantı mimarisi](../managed-instance/connectivity-architecture-overview.md)' ne bakın.

## <a name="connectivity-architecture"></a>Bağlantı mimarisi

Aşağıdaki diyagramda bağlantı mimarisine yüksek düzeyde bir genel bakış sunulmaktadır.

![mimariye genel bakış](./media/connectivity-architecture/connectivity-overview.png)

Aşağıdaki adımlarda, Azure SQL veritabanı 'na bir bağlantının nasıl kurulduğu açıklanır:

- İstemciler, genel bir IP adresine sahip olan ağ geçidine bağlanır ve 1433 numaralı bağlantı noktasını dinler.
- Ağ Geçidi, etkin bağlantı ilkesine bağlı olarak, trafiği doğru veritabanı kümesine yönlendirir veya proxy 'ye yönlendirir.
- Veritabanı kümesi trafiğinin içinde ilgili veritabanına iletilir.

## <a name="connection-policy"></a>Bağlantı ilkesi

SQL veritabanı ve Azure SYNAPSE sunucuları, sunucunun bağlantı ilkesi ayarı için aşağıdaki üç seçeneği destekler:

- **Yeniden yönlendir (önerilir):** İstemciler, veritabanını barındıran düğüme doğrudan bağlantı kurar, azaltılmış gecikme süresi ve geliştirilmiş işleme için önde gelen bir işlem sağlar. Bu modu kullanmak için istemcilerin şunları yapması gerekir:
  - İstemciden giden iletişimin, 11000 11999 aralığındaki bağlantı noktalarında bulunan bölgedeki tüm Azure SQL IP adreslerine erişmesine izin verin. Daha kolay yönetilmesini sağlamak için SQL için hizmet etiketlerini kullanın.  
  - Bağlantı noktası 1433 üzerinde istemciden Azure SQL veritabanı ağ geçidi IP adreslerine giden iletişime izin verin.

- **Proxy:** Bu modda, tüm bağlantılar Azure SQL veritabanı ağ geçitleri aracılığıyla, artan gecikme süresi ve düşük aktarım hızı için önde gelen aracılardır. Bu modu kullanma bağlantıları için istemcilerin, bağlantı noktası 1433 üzerinde istemciden Azure SQL veritabanı ağ geçidi IP adreslerine giden iletişime izin vermek gerekir.

- **Varsayılan:** Bu, bağlantı ilkesini açıkça değiştirmediğiniz müddetçe, oluşturulduktan sonra tüm sunucularda etkin olan bağlantı ilkesidir `Proxy` `Redirect` . Varsayılan ilke, `Redirect` Azure 'ın içinden (örneğin, bir Azure sanal makinesinden) gelen tüm istemci bağlantılarına ve `Proxy` dış kaynaklı tüm istemci bağlantılarına (örneğin, yerel iş istasyonunuzun bağlantıları) yöneliktir.

`Redirect` `Proxy` En düşük gecikme süresi ve en yüksek aktarım hızı için bağlantı ilkesi üzerinde bağlantı ilkesini kesinlikle öneririz. Ancak, yukarıda özetlenen ağ trafiğine izin vermek için ek gereksinimleri karşılamanız gerekecektir. İstemci bir Azure sanal makinedir, bunu [hizmet etiketleriyle](../../virtual-network/security-overview.md#service-tags)ağ güvenlik grupları (NSG) kullanarak gerçekleştirebilirsiniz. İstemci Şirket içindeki bir iş istasyonundan bağlanıyorsa, şirket güvenlik duvarınız üzerinden ağ trafiğine izin vermek için ağ yöneticinizle birlikte çalışmanız gerekebilir.

## <a name="connectivity-from-within-azure"></a>Azure içinden bağlantı

Azure içinden bağlanıyorsanız bağlantılarınız, varsayılan olarak bir bağlantı ilkesine sahiptir `Redirect` . Bir ilke, `Redirect` TCP oturumu Azure SQL veritabanı 'na kurulduktan sonra, istemci oturumu daha sonra doğru veritabanı kümesine yeniden yönlendirilir ve bu, Azure SQL veritabanı ağ geçidindeki hedef sanal IP 'ye yapılan bir değişikliğe göre kümeye yönlendirilir. Bundan sonra, Azure SQL veritabanı ağ geçidini atlayarak sonraki tüm paketler doğrudan kümeye akar. Aşağıdaki diyagramda bu trafik akışı gösterilmektedir.

![mimariye genel bakış](./media/connectivity-architecture/connectivity-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>Azure dışından bağlantı

Azure dışından bağlanıyorsanız, bağlantılarınızın varsayılan olarak bir bağlantı ilkesi vardır `Proxy` . Bir ilke, `Proxy` TCP oturumunun Azure SQL veritabanı ağ geçidi aracılığıyla ve sonraki tüm paketlerin ağ geçidiyle akış üzerinden kurulduğu anlamına gelir. Aşağıdaki diyagramda bu trafik akışı gösterilmektedir.

![mimariye genel bakış](./media/connectivity-architecture/connectivity-onprem.png)

> [!IMPORTANT]
> Ayrıca [, dac Ile bağlanmayı](https://docs.microsoft.com/sql/database-engine/configure-windows/diagnostic-connection-for-database-administrators?view=sql-server-2017#connecting-with-dac) etkinleştirmek için 1434 ve 14000-14999 TCP bağlantı noktalarını açın

## <a name="gateway-ip-addresses"></a>Ağ geçidi IP adresleri

Aşağıdaki tabloda bölgeye göre ağ geçitlerinin IP adresleri listelenir. SQL veritabanı veya Azure SYNAPSE 'e bağlanmak için, bölge için **Tüm** ağ geçitlerine ve bu ağ geçidine ağ trafiğine izin vermeniz gerekir.

Trafiğin belirli bölgelerde yeni ağ geçitlerine nasıl geçirilme ayrıntıları aşağıdaki makalede verilmiştir: [Azure SQL veritabanı trafiğini daha yeni ağ geçitlerine geçirme](gateway-migration.md)

| Bölge adı          | Ağ geçidi IP adresleri |
| --- | --- |
| Orta Avustralya    | 20.36.105.0 |
| Avustralya Central2   | 20.36.113.0 |
| Doğu Avustralya       | 13.75.149.87, 40.79.161.1, 13.70.112.9 |
| Avustralya Güneydoğu | 191.239.192.109, 13.73.109.251 |
| Güney Brezilya         | 104.41.11.5, 191.233.200.14 |
| Orta Kanada       | 40.85.224.249, 52.246.152.0, 20.38.144.1 |
| Doğu Kanada          | 40.86.226.166      |
| Orta ABD           | 13.67.215.62, 52.182.137.15, 23.99.160.139, 104.208.16.96, 104.208.21.1 |
| Doğu Çin           | 139.219.130.35     |
| Çin Doğu 2         | 40.73.82.1         |
| Kuzey Çin          | 139.219.15.17      |
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
| Kuzey Avrupa         | 40.113.93.91, 191.235.193.75, 52.138.224.1, 13.74.104.113 |
| Norveç Doğu          | 51.120.96.0        |
| Norveç Batı          | 51.120.216.0       |
| Güney Afrika Kuzey   | 102.133.152.0      |
| Güney Afrika Batı    | 102.133.24.0       |
| Orta Güney ABD     | 13.66.62.124, 23.98.162.75, 104.214.16.32   |
| Güneydoğu Asya      | 104.43.15.0, 23.100.117.95, 40.78.232.3   |
| İsviçre Kuzey    | 51.107.56.0, 51.107.57.0 |
| İsviçre Batı     | 51.107.152.0, 51.107.153.0 |
| BAE Orta          | 20.37.72.64        |
| BAE Kuzey            | 65.52.248.0        |
| Güney Birleşik Krallık             | 51.140.184.11      |
| Batı Birleşik Krallık              | 51.141.8.11        |
| Orta Batı ABD      | 13.78.145.25       |
| Batı Avrupa          | 40.68.37.158, 191.237.232.75, 104.40.168.105  |
| Batı ABD              | 104.42.238.205, 23.99.34.75, 13.86.216.196   |
| Batı ABD 2            | 13.66.226.202, 40.78.240.8  |
|                      |                    |

## <a name="next-steps"></a>Sonraki adımlar

- Bir sunucu için Azure SQL veritabanı bağlantı ilkesini değiştirme hakkında daha fazla bilgi için bkz. [Conn-Policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).
- ADO.NET 4,5 veya sonraki bir sürümünü kullanan istemciler için Azure SQL veritabanı bağlantı davranışı hakkında bilgi için bkz. [1433 sonrasındaki bağlantı noktaları ADO.NET 4,5](adonet-v12-develop-direct-route-ports.md).
- Genel uygulama geliştirmeye genel bakış bilgileri için bkz. [SQL veritabanı uygulaması geliştirmeye genel bakış](develop-overview.md).
