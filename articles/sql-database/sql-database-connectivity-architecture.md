---
title: Bağlantı Mimarisi
description: Bu belge, Azure'un içinden veya Azure dışından veritabanı bağlantıları için Azure SQL bağlantı mimarisini açıklar.
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
ms.openlocfilehash: 2028aac9c01aedc4baa568d370c9f7d21c920647
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81419272"
---
# <a name="azure-sql-connectivity-architecture"></a>Azure SQL Bağlantı Mimarisi
> [!NOTE]
> Bu makale, Azure SQL sunucusu ve Azure SQL sunucusunda oluşturulan SQL Veritabanı ve SQL Veri Ambarı veritabanları için geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır.

> [!IMPORTANT]
> Bu makale, **Azure SQL Veritabanı Yönetilen Örnek**için geçerli *değildir.* Yönetilen [bir örnek için Bağlantı mimarisine](sql-database-managed-instance-connectivity-architecture.md)bakın.

Bu makalede, ağ trafiğini Azure SQL Veritabanı'na veya SQL Veri Ambarı'na yönlendiren çeşitli bileşenlerin mimarisi açıklanmaktadır. Ayrıca, farklı bağlantı ilkelerini ve Azure'un içinden bağlanan istemcileri ve Azure dışından bağlanan istemcileri nasıl etkilediğini de açıklar. 

## <a name="connectivity-architecture"></a>Bağlantı mimarisi

Aşağıdaki diyagram, Azure SQL Veritabanı bağlantı mimarisine üst düzey bir genel bakış sağlar.

![mimariye genel bakış](./media/sql-database-connectivity-architecture/connectivity-overview.png)

Aşağıdaki adımlar, bir Azure SQL veritabanına nasıl bağlantı kurulduğunu açıklar:

- İstemciler, genel bir IP adresi olan ve bağlantı noktası 1433'te dinleyen ağ geçidine bağlanır.
- Ağ geçidi, etkili bağlantı ilkesine bağlı olarak trafiği doğru veritabanı kümesine yönlendirir veya ileder.
- Veritabanı kümesi trafiğinin içinde ilgili Azure SQL veritabanına iletilir.

## <a name="connection-policy"></a>Bağlantı ilkesi

Azure SQL Veritabanı, bir SQL Veritabanı sunucusunun bağlantı ilkesi ayarı için aşağıdaki üç seçeneği destekler:

- **Yönlendirme (önerilir):** İstemciler, daha az gecikme gecikmesine ve gelişmiş iş elde etme önüne geçerek doğrudan veritabanını barındıran düğüme bağlantılar kurar. Bağlantıların bu modu kullanabilmek için şunları
   - İstemciden 11000 11999 aralığındaki bağlantı noktalarında bölgedeki tüm Azure SQL IP adreslerine giden iletişime izin verin. Bunu yönetmeyi kolaylaştırmak için SQL için Hizmet Etiketleri'ni kullanın.  
   - İstemciden 1433 bağlantı noktasındaki Azure SQL Veritabanı ağ geçidi IP adreslerine giden iletişime izin verin.

- **Proxy:** Bu modda, tüm bağlantılar Azure SQL Veritabanı ağ geçitleri aracılığıyla yakınlaştırılarak gecikme gecikmesi ve daha az iş ortası elde edilmesine yol açar. Bağlantıların bu modu kullanabilmesi için istemcilerin istemciden 1433 bağlantı noktasındaki Azure SQL Veritabanı ağ geçidi IP adreslerine giden iletişime izin vermeleri gerekir.

- **Varsayılan değer:** Bu, bağlantı ilkesini açıkça değiştirmediğiniz sürece, oluşturulduktan sonra tüm `Proxy` `Redirect`sunucularda geçerli olan bağlantı ilkesidir. Varsayılan ilke, Azure'un içinden kaynaklanan tüm istemci bağlantıları (örneğin, `Proxy`bir Azure Sanal Makinesi'nden) ve dışarıdan kaynaklanan tüm istemci bağlantıları (örneğin, yerel iş istasyonunuzdaki bağlantılar) içindir.`Redirect`

 En düşük `Redirect` gecikme ve `Proxy` en yüksek iş sonu için bağlantı ilkesi üzerinden bağlantı ilkesini şiddetle öneririz. Ancak, yukarıda belirtildiği gibi ağ trafiğine izin vermek için ek gereksinimleri karşılamanız gerekir. İstemci bir Azure Sanal Makineise, [bunu hizmet etiketleriyle](../virtual-network/security-overview.md#service-tags)Ağ Güvenlik Grupları 'nı (NSG) kullanarak gerçekleştirebilirsiniz. İstemci şirket içinde bir iş istasyonundan bağlanıyorsa, şirket güvenlik duvarınız aracılığıyla ağ trafiğine izin vermek için ağ yöneticinizle birlikte çalışmanız gerekebilir.

## <a name="connectivity-from-within-azure"></a>Azure içinden bağlantı

Azure içinden bağlanıyorsanız, bağlantılarınızın varsayılan olarak `Redirect` bir bağlantı ilkesi vardır. `Redirect` TCP oturumu Azure SQL veritabanına kurulduktan sonra istemci oturumu, Azure SQL Veritabanı ağ geçidinden hedef sanal IP'ye yapılan bir değişiklikle doğru veritabanı kümesine yönlendirilir. Bundan sonra, sonraki tüm paketler Azure SQL Veritabanı ağ geçidini atlayarak doğrudan kümeye akar. Aşağıdaki diyagram bu trafik akışını göstermektedir.

![mimariye genel bakış](./media/sql-database-connectivity-architecture/connectivity-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>Azure dışından bağlantı

Azure dışından bağlanıyorsanız, bağlantılarınızın varsayılan olarak bir `Proxy` bağlantı ilkesi vardır. Bir ilke, `Proxy` TCP oturumunun Azure SQL Veritabanı ağ geçidi üzerinden oluşturuldurulur ve sonraki tüm paketler ağ geçidi nden akar. Aşağıdaki diyagram bu trafik akışını göstermektedir.

![mimariye genel bakış](./media/sql-database-connectivity-architecture/connectivity-onprem.png)

> [!IMPORTANT]
> Ayrıca [DAC ile bağlantı](https://docs.microsoft.com/sql/database-engine/configure-windows/diagnostic-connection-for-database-administrators?view=sql-server-2017#connecting-with-dac) sağlamak için 14000-14999 bağlantı noktalarını açın


## <a name="azure-sql-database-gateway-ip-addresses"></a>Azure SQL Veritabanı ağ geçidi IP adresleri

Aşağıdaki tabloda bölgelere göre Ağ Geçitlerinin IP Adresleri listelanmaktadır. Azure SQL Veritabanına bağlanmak için ağ trafiğinin bölgedeki **tüm** Ağ Geçitlerinden & izin vermeniz gerekir.

Trafiğin belirli bölgelerdeki yeni Ağ Geçitlerine nasıl geçirileceğinin ayrıntıları aşağıdaki makalede yer almaktadır: [Azure SQL Veritabanı trafik geçişi yeni ağ geçitlerine](sql-database-gateway-migration.md)


| Bölge Adı          | Ağ Geçidi IP Adresleri |
| --- | --- |
| Orta Avustralya    | 20.36.105.0 |
| Avustralya Merkez2   | 20.36.113.0 |
| Doğu Avustralya       | 13.75.149.87, 40.79.161.1 |
| Avustralya Güneydoğu | 191.239.192.109, 13.73.109.251 |
| Güney Brezilya         | 104.41.11.5, 191.233.200.14 |
| Orta Kanada       | 40.85.224.249      |
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
| Kuzey Avrupa         | 40.113.93.91, 191.235.193.75, 52.138.224.1 | 
| Norveç Doğu          | 51.120.96.0        |
| Norveç Batı          | 51.120.216.0       |
| Güney Afrika Kuzey   | 102.133.152.0      |
| Güney Afrika Batı    | 102.133.24.0       |
| Orta Güney ABD     | 13.66.62.124, 23.98.162.75, 104.214.16.32   | 
| Güneydoğu Asya      | 104.43.15.0, 23.100.117.95, 40.78.232.3   | 
| BAE Merkez          | 20.37.72.64        |
| BAE Kuzey            | 65.52.248.0        |
| Güney Birleşik Krallık             | 51.140.184.11      |
| Batı Birleşik Krallık              | 51.141.8.11        |
| Orta Batı ABD      | 13.78.145.25       |
| Batı Avrupa          | 40.68.37.158, 191.237.232.75, 104.40.168.105  |
| Batı ABD              | 104.42.238.205, 23.99.34.75, 13.86.216.196   |
| Batı ABD 2            | 13.66.226.202      |
|                      |                    |



## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL Veritabanı sunucusu için Azure SQL Veritabanı bağlantı ilkesini nasıl değiştireceğiniz hakkında bilgi için [conn-policy'ye](https://docs.microsoft.com/cli/azure/sql/server/conn-policy)bakın.
- 4,5 veya daha sonraki bir sürümü ADO.NET kullanan istemciler için Azure SQL Veritabanı bağlantı davranışı hakkında bilgi için, [ADO.NET 4,5 için 1433'ün ötesindeki Bağlantı Noktaları'na](sql-database-develop-direct-route-ports-adonet-v12.md)bakın.
- Genel uygulama geliştirme genel bakış bilgileri için, [bkz.](sql-database-develop-overview.md)
