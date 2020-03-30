---
title: Bağlantı mimarisi - MySQL için Azure Veritabanı
description: MySQL sunucusu için Azure Veritabanınızın bağlantı mimarisini açıklar.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 03/16/2020
ms.openlocfilehash: e7098056356c84469d3dc6bd4e3222fbb5b7f908
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79474297"
---
# <a name="connectivity-architecture-in-azure-database-for-mysql"></a>MySQL için Azure Veritabanı'nda bağlantı mimarisi
Bu makalede, MySQL bağlantı mimarisi için Azure Veritabanı'nın yanı sıra, trafiğin Azure içindeki ve dışındaki istemcilerden MySQL örneği için Azure Veritabanınıza nasıl yönlendirileceği açıklanmaktadır.

## <a name="connectivity-architecture"></a>Bağlantı mimarisi
MySQL için Azure Veritabanınıza bağlantı, gelen bağlantıları nkümelerimizdeki sunucunuzun fiziksel konumuna yönlendirmekten sorumlu bir ağ geçidi aracılığıyla kurulur. Aşağıdaki diyagram trafik akışını göstermektedir.

![Bağlantı mimarisine genel bakış](./media/concepts-connectivity-architecture/connectivity-architecture-overview-proxy.png)

İstemci veritabanına bağlanırken, ağ geçidine bağlanan bir bağlantı dizesi elde ederler. Bu ağ geçidi, bağlantı noktası 3306'yı dinleyen genel bir IP adresine sahiptir. Veritabanı kümesi içinde, trafik MySQL için uygun Azure Veritabanı'na iletilir. Bu nedenle, şirket ağları gibi sunucunuza bağlanmak için, giden trafiğin ağ geçitlerimize erişebilmesi için istemci tarafındaki güvenlik duvarını açmak gerekir. Aşağıda bölge ağ geçitlerimiz tarafından kullanılan IP adreslerinin tam listesini bulabilirsiniz.

## <a name="azure-database-for-mysql-gateway-ip-addresses"></a>MySQL ağ geçidi IP adresleri için Azure Veritabanı
Aşağıdaki tabloda, tüm veri bölgeleri için MySQL ağ geçidi için Azure Veritabanı'nın birincil ve ikincil IP'leri listelemektedir. Birincil IP adresi ağ geçidinin geçerli IP adresidir ve ikinci IP adresi birincil hata durumunda bir failover IP adresidir. Belirtildiği gibi, müşteriler her iki IP adresine de giden izin vermelidir. İkinci IP adresi, MySQL için Azure Veritabanı tarafından bağlantıları kabul etmek üzere etkinleştirilene kadar hiçbir hizmeti dinlemez.

| **Bölge Adı** | **Ağ Geçidi IP Adresleri** |
|:----------------|:-------------|
| Orta Avustralya| 20.36.105.0     |
| Avustralya Merkez2     | 20.36.113.0     |
| Doğu Avustralya | 13.75.149.87, 40.79.161.1     |
| Avustralya Güneydoğu |191.239.192.109, 13.73.109.251     |
| Güney Brezilya | 104.41.11.5, 191.233.201.8, 191.233.200.16     |
| Orta Kanada |40.85.224.249     |
| Doğu Kanada | 40.86.226.166     |
| Orta ABD | 23.99.160.139, 13.67.215.62     |
| Doğu Çin | 139.219.130.35     |
| Çin Doğu 2 | 40.73.82.1     |
| Kuzey Çin | 139.219.15.17     |
| Çin Kuzey 2 | 40.73.50.0     |
| Doğu Asya | 191.234.2.139, 52.175.33.150, 13.75.33.20, 13.75.33.21     |
| Doğu ABD | 40.121.158.30, 191.238.6.43     |
| Doğu ABD 2 |40.79.84.180, 191.239.224.107, 52.177.185.181     |
| Orta Fransa | 40.79.137.0, 40.79.129.1     |
| Orta Almanya | 51.4.144.100     |
| Almanya Kuzey Doğu | 51.5.144.179     |
| Hindistan Orta | 104.211.96.159     |
| Hindistan Güney | 104.211.224.146     |
| Hindistan Batı | 104.211.160.80     |
| Doğu Japonya | 13.78.61.196, 191.237.240.43     |
| Batı Japonya | 104.214.148.156, 191.238.68.11, 40.74.96.7, 40.74.96.6 |
| Güney Kore - Orta | 52.231.32.42     |
| Güney Kore - Güney | 52.231.200.86     |
| Orta Kuzey ABD | 23.96.178.199, 23.98.55.75, 52.162.104.35, 52.162.104.36     |
| Kuzey Avrupa | 40.113.93.91, 191.235.193.75     |
| Güney Afrika Kuzey  | 102.133.152.0     |
| Güney Afrika Batı    | 102.133.24.0     |
| Orta Güney ABD |13.66.62.124, 23.98.162.75, 104.214.16.39, 20.45.120.0     |
| Güneydoğu Asya | 104.43.15.0, 23.100.117.95, 40.78.233.2, 23.98.80.12     |
| BAE Merkez | 20.37.72.64     |
| BAE Kuzey | 65.52.248.0     |
| Güney Birleşik Krallık | 51.140.184.11     |
| Batı Birleşik Krallık | 51.141.8.11     |
| Orta Batı ABD | 13.78.145.25     |
| Batı Avrupa | 40.68.37.158, 191.237.232.75     |
| Batı ABD | 104.42.238.205, 23.99.34.75     |
| Batı ABD 2 | 13.66.226.202     |
||||

## <a name="connection-redirection"></a>Bağlantı yeniden yönlendirme

MySQL için Azure Veritabanı, istemci uygulamaları ve MySQL sunucuları arasındaki ağ gecikmesini azaltmaya yardımcı olan ek bir bağlantı ilkesini, **yeniden yönlendirmeyi**destekler. Bu özellik sayesinde, ilk TCP oturumu MySQL sunucusu için Azure Veritabanı'na kurulduktan sonra, sunucu MySQL sunucusunu barındıran düğümün arka uç adresini istemciye döndürür. Bundan sonra, sonraki tüm paketler ağ geçidini atlayarak doğrudan sunucuya akar. Paketler doğrudan sunucuya aktazken, gecikme ve iş akışı performansı artırdı.

Bu özellik, 5.6, 5.7 ve 8.0 motor sürümlerine sahip MySQL sunucuları için Azure Veritabanı'nda desteklenir.

Yeniden yönlendirme desteği, Microsoft tarafından geliştirilen PHP [mysqlnd_azure](https://github.com/microsoft/mysqlnd_azure) uzantısında mevcuttur ve [PECL'de](https://pecl.php.net/package/mysqlnd_azure)kullanılabilir. Uygulamalarınızda [yeniden yönlendirmenin](./howto-redirection.md) nasıl kullanılacağı hakkında daha fazla bilgi için yapılandırma yeniden yönlendirme makalesine bakın.

> [!IMPORTANT]
> PHP [mysqlnd_azure](https://github.com/microsoft/mysqlnd_azure) uzantısı yeniden yönlendirme desteği şu anda önizleme de.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portalını kullanarak MySQL güvenlik duvarı kuralları için Azure Veritabanı oluşturma ve yönetme](./howto-manage-firewall-using-portal.md)
* [Azure CLI'yi kullanarak MySQL güvenlik duvarı kuralları için Azure Veritabanı oluşturma ve yönetme](./howto-manage-firewall-using-cli.md)
* [MySQL için Azure Veritabanı ile yeniden yönlendirmeyi yapılandırma](./howto-redirection.md)