---
title: Bağlantı mimarisi - MariaDB için Azure Veritabanı
description: MariaDB sunucusu için Azure Veritabanınızın bağlantı mimarisini açıklar.
author: kummanish
ms.author: manishku
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 38b6f797541ef07fa3fb5e1dc71029a4cbcf5d22
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80546302"
---
# <a name="connectivity-architecture-in-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı'nda bağlantı mimarisi
Bu makalede, MariaDB bağlantı mimarisi için Azure Veritabanı'nın yanı sıra, trafiğin Azure içindeki ve dışındaki istemcilerden MariaDB örneği için Azure Veritabanınıza nasıl yönlendirileceği açıklanmaktadır.

## <a name="connectivity-architecture"></a>Bağlantı mimarisi

MariaDB için Azure Veritabanınıza bağlantı, gelen bağlantıların kümelerimizdeki sunucunuzun fiziksel konumuna yönlendirmeden sorumlu bir ağ geçidi aracılığıyla kurulur. Aşağıdaki diyagram trafik akışını göstermektedir.

![Bağlantı mimarisine genel bakış](./media/concepts-connectivity-architecture/connectivity-architecture-overview-proxy.png)

İstemci veritabanına bağlanırken, ağ geçidine bağlanan bir bağlantı dizesi elde ederler. Bu ağ geçidi, bağlantı noktası 3306'yı dinleyen genel bir IP adresine sahiptir. Veritabanı kümesi içinde, trafik MariaDB için uygun Azure Veritabanı'na iletilir. Bu nedenle, şirket ağları gibi sunucunuza bağlanmak için, giden trafiğin ağ geçitlerimize erişebilmesi için istemci tarafındaki güvenlik duvarını açmak gerekir. Aşağıda bölge ağ geçitlerimiz tarafından kullanılan IP adreslerinin tam listesini bulabilirsiniz.

## <a name="azure-database-for-mariadb-gateway-ip-addresses"></a>MariaDB ağ geçidi IP adresleri için Azure Veritabanı

Aşağıdaki tabloda, tüm veri bölgeleri için MariaDB ağ geçidi için Azure Veritabanı'nın birincil ve ikincil IP'leri listelemektedir. Birincil IP adresi ağ geçidinin geçerli IP adresidir ve ikinci IP adresi birincil hata durumunda bir failover IP adresidir. Belirtildiği gibi, müşteriler her iki IP adresine de giden izin vermelidir. İkinci IP adresi, MariaDB'nin bağlantıları kabul etmesi için Azure Veritabanı tarafından etkinleştirilene kadar hiçbir hizmeti dinlemez.

| **Bölge Adı** | **Ağ Geçidi IP Adresleri** |
|:----------------|:-------------|
| Orta Avustralya| 20.36.105.0     |
| Avustralya Merkez2     | 20.36.113.0   |
| Doğu Avustralya | 13.75.149.87, 40.79.161.1     |
| Avustralya Güneydoğu |191.239.192.109, 13.73.109.251   |
| Güney Brezilya | 104.41.11.5, 191.233.201.8, 191.233.200.16  |
| Orta Kanada |40.85.224.249  |
| Doğu Kanada | 40.86.226.166    |
| Orta ABD | 23.99.160.139, 13.67.215.62, 52.182.136.37, 52.182.136.38     |
| Doğu Çin | 139.219.130.35    |
| Çin Doğu 2 | 40.73.82.1  |
| Kuzey Çin | 139.219.15.17    |
| Çin Kuzey 2 | 40.73.50.0     |
| Doğu Asya | 191.234.2.139, 52.175.33.150, 13.75.33.20, 13.75.33.21     |
| Doğu ABD | 40.121.158.30, 191.238.6.43  |
| Doğu ABD 2 |40.79.84.180, 191.239.224.107, 52.177.185.181, 40.70.144.38, 52.167.105.38  |
| Orta Fransa | 40.79.137.0, 40.79.129.1  |
| Orta Almanya | 51.4.144.100     |
| Almanya Kuzey Doğu | 51.5.144.179  |
| Hindistan Orta | 104.211.96.159     |
| Hindistan Güney | 104.211.224.146  |
| Hindistan Batı | 104.211.160.80    |
| Doğu Japonya | 13.78.61.196, 191.237.240.43  |
| Batı Japonya | 104.214.148.156, 191.238.68.11, 40.74.96.6, 40.74.96.7    |
| Güney Kore - Orta | 52.231.32.42   |
| Güney Kore - Güney | 52.231.200.86    |
| Orta Kuzey ABD | 23.96.178.199, 23.98.55.75, 52.162.104.35, 52.162.104.36    |
| Kuzey Avrupa | 40.113.93.91, 191.235.193.75, 52.138.224.6, 52.138.224.7    |
| Güney Afrika Kuzey  | 102.133.152.0    |
| Güney Afrika Batı | 102.133.24.0   |
| Orta Güney ABD |13.66.62.124, 23.98.162.75, 104.214.16.39, 20.45.120.0   |
| Güneydoğu Asya | 104.43.15.0, 23.100.117.95, 40.78.233.2, 23.98.80.12     |
| BAE Merkez | 20.37.72.64  |
| BAE Kuzey | 65.52.248.0    |
| Güney Birleşik Krallık | 51.140.184.11   |
| Batı Birleşik Krallık | 51.141.8.11  |
| Orta Batı ABD | 13.78.145.25     |
| Batı Avrupa | 40.68.37.158, 191.237.232.75, 13.69.105.208  |
| Batı ABD | 104.42.238.205, 23.99.34.75  |
| Batı ABD 2 | 13.66.226.202  |
||||

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portalını kullanarak MariaDB güvenlik duvarı kuralları için Azure Veritabanı oluşturma ve yönetme](./howto-manage-firewall-portal.md)
* [Azure CLI'yi kullanarak MariaDB güvenlik duvarı kuralları için Azure Veritabanı oluşturma ve yönetme](./howto-manage-firewall-cli.md)
