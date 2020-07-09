---
title: Bağlantı mimarisi-MySQL için Azure veritabanı
description: MySQL için Azure veritabanı sunucunuza yönelik bağlantı mimarisini açıklar.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 03/16/2020
ms.openlocfilehash: f4d90693f2cd3bdd440b7cb914e7fc037103d362
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86121002"
---
# <a name="connectivity-architecture-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda bağlantı mimarisi
Bu makalede, MySQL için Azure veritabanı bağlantı mimarisinin yanı sıra trafiğin Azure 'daki ve dışındaki istemcilerden gelen MySQL için Azure veritabanı örneğine nasıl yönlendirildiği açıklanmaktadır.

## <a name="connectivity-architecture"></a>Bağlantı mimarisi
MySQL için Azure veritabanı 'na bağlantı, gelen bağlantıları kümelerimize ait fiziksel konuma yönlendirmekten sorumlu bir ağ geçidiyle oluşturulur. Aşağıdaki diyagramda trafik akışı gösterilmektedir.

![Bağlantı mimarisine genel bakış](./media/concepts-connectivity-architecture/connectivity-architecture-overview-proxy.png)

İstemci veritabanına bağlandığında, ağ geçidine bağlanan bir bağlantı dizesi alırlar. Bu ağ geçidinin 3306 numaralı bağlantı noktasını dinleyen bir genel IP adresi vardır. Veritabanı kümesi içinde trafik, MySQL için uygun Azure veritabanına iletilir. Bu nedenle, şirket ağları gibi sunucunuza bağlanmak için, giden trafiğin ağ geçitlerimize ulaşmasını sağlamak üzere istemci tarafı güvenlik duvarını açmanız gerekir. Aşağıda, bölge başına ağ geçitlerimiz tarafından kullanılan IP adreslerinin tamamen bir listesini bulabilirsiniz.

## <a name="azure-database-for-mysql-gateway-ip-addresses"></a>MySQL için Azure veritabanı ağ geçidi IP adresleri
Aşağıdaki tabloda, tüm veri bölgeleri için MySQL için Azure veritabanı Gateway 'in birincil ve ikincil IP 'Leri listelenmektedir. Birincil IP adresi, ağ geçidinin geçerli IP adresidir ve ikinci IP adresi birincil hata durumunda bir yük devretme IP adresidir. Belirtildiği gibi, müşteriler her iki IP adresine de giden trafiği izin vermelidir. İkinci IP adresi, MySQL için Azure veritabanı tarafından bağlantıları kabul etmek üzere etkinleştirilinceye kadar hiçbir hizmet üzerinde dinleme yapmaz.

| **Bölge Adı** | **Ağ geçidi IP adresleri** |
|:----------------|:-------------|
| Orta Avustralya| 20.36.105.0     |
| Avustralya Central2     | 20.36.113.0   |
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
| Doğu ABD | 40.121.158.30, 191.238.6.43, 40.71.8.203, 40.71.83.113 |
| Doğu ABD 2 |40.79.84.180, 191.239.224.107, 52.177.185.181, 40.70.144.38, 52.167.105.38  |
| Orta Fransa | 40.79.137.0, 40.79.129.1  |
| Güney Fransa | 40.79.177.0     |
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
| BAE Orta | 20.37.72.64  |
| BAE Kuzey | 65.52.248.0    |
| Güney Birleşik Krallık | 51.140.184.11   |
| Batı Birleşik Krallık | 51.141.8.11  |
| Orta Batı ABD | 13.78.145.25     |
| Batı Avrupa | 40.68.37.158, 191.237.232.75, 13.69.105.208  |
| Batı ABD | 104.42.238.205, 23.99.34.75  |
| Batı ABD 2 | 13.66.226.202  |
||||

## <a name="connection-redirection"></a>Bağlantı yeniden yönlendirme

MySQL için Azure veritabanı, istemci uygulamaları ve MySQL sunucuları arasındaki ağ gecikmesini azaltmaya yardımcı olan ek bir bağlantı ilkesini, **yeniden yönlendirmeyi**destekler. Bu özellik ile, ilk TCP oturumu MySQL sunucusu için Azure veritabanı 'na kurulduktan sonra sunucu, MySQL sunucusunu istemciye barındıran düğümün arka uç adresini döndürür. Bundan sonra, sonraki tüm paketler doğrudan sunucuya akar ve ağ geçidini atlar. Paketler doğrudan sunucuya akar, gecikme süresi ve aktarım hızı performansı artırılmıştır.

Bu özellik, 5,6, 5,7 ve 8,0 Altyapı sürümleriyle MySQL sunucuları için Azure veritabanı 'nda desteklenir.

Yeniden yönlendirme desteği, Microsoft tarafından geliştirilen PHP [mysqlnd_azure](https://github.com/microsoft/mysqlnd_azure) uzantısında mevcuttur ve, [PECL](https://pecl.php.net/package/mysqlnd_azure) Uygulamalarınızda yeniden yönlendirmeyi kullanma hakkında daha fazla bilgi için [yeniden yönlendirmeyi yapılandırma](./howto-redirection.md) makalesine bakın.

> [!IMPORTANT]
> PHP [mysqlnd_azure](https://github.com/microsoft/mysqlnd_azure) uzantısında yeniden yönlendirme desteği şu anda önizleme aşamasındadır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portal kullanarak MySQL için Azure Güvenlik duvarı kuralları oluşturma ve yönetme](./howto-manage-firewall-using-portal.md)
* [Azure CLı kullanarak MySQL için Azure Güvenlik duvarı kuralları oluşturma ve yönetme](./howto-manage-firewall-using-cli.md)
* [MySQL için Azure veritabanı ile yeniden yönlendirmeyi yapılandırma](./howto-redirection.md)
