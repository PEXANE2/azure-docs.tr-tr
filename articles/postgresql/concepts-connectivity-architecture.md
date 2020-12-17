---
title: Bağlantı mimarisi-PostgreSQL için Azure veritabanı-tek sunucu
description: PostgreSQL için Azure veritabanı 'nın bağlantı mimarisini açıklar-tek sunucu.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: conceptual
ms.date: 05/23/2019
ms.openlocfilehash: d5476bf1bfe2e222e115146c13f46e776d4bb497
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97657201"
---
# <a name="connectivity-architecture-in-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı 'nda bağlantı mimarisi
Bu makalede PostgreSQL için Azure veritabanı bağlantı mimarisi ve trafiğin Azure 'daki ve dışındaki istemcilerden PostgreSQL için Azure veritabanı örneğine nasıl yönlendirildiği açıklanmaktadır.

## <a name="connectivity-architecture"></a>Bağlantı mimarisi
PostgreSQL için Azure veritabanı 'na bağlantı, gelen bağlantıları kümelerimize ait fiziksel konuma yönlendirmekten sorumlu bir ağ geçidiyle oluşturulur. Aşağıdaki diyagramda trafik akışı gösterilmektedir.

:::image type="content" source="./media/concepts-connectivity-architecture/connectivity-architecture-overview-proxy.png" alt-text="Bağlantı mimarisine genel bakış":::


İstemci veritabanına bağlıyken, sunucuya bağlantı dizesi ağ geçidi IP adresine çözümlenir. Ağ Geçidi 5432 bağlantı noktasındaki IP adresini dinler. Veritabanı kümesi içinde trafik, PostgreSQL için uygun Azure veritabanına iletilir. Bu nedenle, şirket ağları gibi sunucunuza bağlanmak için, **giden trafiğin ağ geçitlerimize erişebilmesini sağlamak üzere istemci tarafı güvenlik duvarını** açmanız gerekir. Aşağıda, bölge başına ağ geçitlerimiz tarafından kullanılan IP adreslerinin tamamen bir listesini bulabilirsiniz.

## <a name="azure-database-for-postgresql-gateway-ip-addresses"></a>PostgreSQL için Azure veritabanı ağ geçidi IP adresleri

Ağ Geçidi Hizmeti, bir IP adresinin arkasında yer alan durum bilgisiz işlem düğümleri grubunda barındırılır. Bu, istemcinizin ilk kez PostgreSQL sunucusu için Azure veritabanına bağlanmaya çalışırken ulaşacak. 

Devam eden hizmet bakımının bir parçası olarak, en güvenli ve yüksek performanslı deneyim sağlamamız için ağ geçitlerini barındıran işlem donanımını düzenli olarak yenileyeceğiz. Ağ Geçidi donanımı yenilendiğinde, önce işlem düğümlerinin yeni bir halkası oluşturulur. Bu yeni halka, tüm yeni oluşturulan PostgreSQL için Azure veritabanı sunucuları için trafiğe hizmet verir ve trafiği ayırt etmek için aynı bölgedeki eski ağ geçidi halkalarından farklı bir IP adresine sahip olur. Yeni halka tam çalışır olduktan sonra, mevcut sunuculara hizmet veren eski ağ geçidi donanımı kullanımdan kaldırma için planlanmaktadır. Bir ağ geçidi donanımını kullanımdan kaldırmadan önce, sunucularını çalıştıran ve eski ağ geçidi halkalarına bağlanan müşteriler, kullanımdan kaldırmadan önce üç ay içinde ve Azure portal üzerinden gönderilir. Ağ geçitlerinin yetkisini alma, şu durumlarda sunucularınız için bağlantıyı etkileyebilir 

* Ağ geçidi IP adreslerini uygulamanızın bağlantı dizesinde sabit olarak kodlayın. **Önerilmez**. Uygulamanızın bağlantı dizesinde. postgres.database.azure.com biçiminde sunucunuzun tam etki alanı adını (FQDN) kullanmanız gerekir <servername> . 
* Giden trafiğin yeni ağ geçidi halkalarımıza erişebilmesini sağlamak için istemci tarafı güvenlik duvarında daha yeni ağ geçidi IP adreslerini güncelleştirmeyin.

Aşağıdaki tabloda, tüm veri bölgeleri için PostgreSQL için Azure veritabanı ağ geçidi IP adresleri listelenmektedir. Her bölge için ağ geçidi IP adreslerinin en güncel bilgileri aşağıdaki tabloda tutulur. Aşağıdaki tabloda sütunlar aşağıdakileri temsil eder:

* **Ağ GEÇIDI IP adresleri:** Bu sütunda, en son nesil donanımda barındırılan ağ geçitlerinin geçerli IP adresleri listelenir. Yeni bir sunucu sağlıyorsanız, bu sütunda listelenen IP adresleri için giden trafiğe izin vermek üzere istemci tarafı güvenlik duvarını açmanız önerilir.
* **Ağ GEÇIDI IP adresleri (yetki alma):** Bu sütun, şu anda kullanımdan kaldırılan daha eski nesil bir donanımda barındırılan ağ geçitlerinin IP adreslerini listeler. Yeni bir sunucu sağlıyorsanız, bu IP adreslerini yoksayabilirsiniz. Var olan bir sunucunuz varsa, henüz kullanımdan çıkarmadığından bu IP adreslerinin güvenlik duvarının giden kuralını tutmaya devam edin. Bu IP adreslerine yönelik güvenlik duvarı kurallarını düşürüriz, bağlantı hataları alabilirsiniz. Bunun yerine, kullanımdan kaldırma bildirimi aldıktan hemen sonra ağ geçidi IP adresleri sütununda listelenen yeni IP adreslerini giden güvenlik duvarı kuralına önceden eklemeniz beklenir. Bu, sunucunuzun en son ağ geçidi donanımına geçirilmesi durumunda, sunucunuza yönelik bir kesinti olmadığından emin olur.
* **Ağ GEÇIDI IP adresleri (kullanımdan çıkarıldı):** Bu sütunlar, kullanımdan kaldırılan ve artık işlemlerdeki ağ geçidi halkalarının IP adreslerini listeler. Bu IP adreslerini giden güvenlik duvarı kuralınızdan güvenli bir şekilde kaldırabilirsiniz. 


| **Bölge adı** | **Ağ geçidi IP adresleri** |**Ağ geçidi IP adresleri (yetki alma)** | **Ağ geçidi IP adresleri (kullanımdan kaldırıldı)** |
|:----------------|:-------------------------|:-------------------------------------------|:------------------------------------------|
| Orta Avustralya| 20.36.105.0  | | |
| Avustralya Central2     | 20.36.113.0  | | |
| Doğu Avustralya | 13.75.149.87, 40.79.161.1     |  | |
| Avustralya Güneydoğu |191.239.192.109, 13.73.109.251   |  | |
| Brezilya Güney |191.233.201.8, 191.233.200.16    |  | 104.41.11.5|
| Orta Kanada |40.85.224.249  | | |
| Doğu Kanada | 40.86.226.166    | | |
| Central US | 23.99.160.139, 13.67.215.62, 52.182.136.37, 52.182.136.38 | | |
| Doğu Çin | 139.219.130.35    | | |
| Çin Doğu 2 | 40.73.82.1  | | |
| Kuzey Çin | 139.219.15.17    | | |
| Çin Kuzey 2 | 40.73.50.0     | | |
| Doğu Asya | 191.234.2.139, 52.175.33.150, 13.75.33.20, 13.75.33.21     | | |
| Doğu ABD |40.71.8.203, 40.71.83.113 |40.121.158.30|191.238.6.43 |
| Doğu ABD 2 |40.79.84.180, 191.239.224.107, 52.177.185.181, 40.70.144.38, 52.167.105.38  | | |
| Orta Fransa | 40.79.137.0, 40.79.129.1  | | |
| Güney Fransa | 40.79.177.0     | | |
| Orta Almanya | 51.4.144.100     | | |
| Almanya Kuzey Doğu | 51.5.144.179  | | |
| Hindistan Orta | 104.211.96.159     | | |
| Hindistan Güney | 104.211.224.146  | | |
| Hindistan Batı | 104.211.160.80    | | |
| Doğu Japonya | 13.78.61.196, 191.237.240.43, 40.79.192.23 | | |
| Batı Japonya | 104.214.148.156, 191.238.68.11, 40.74.96.6, 40.74.96.7    | | |
| Güney Kore - Orta | 52.231.32.42   | | |
| Güney Kore - Güney | 52.231.200.86    | | |
| Orta Kuzey ABD | 23.96.178.199, 23.98.55.75, 52.162.104.35, 52.162.104.36    | | |
| Kuzey Avrupa | 52.138.224.6, 52.138.224.7  |40.113.93.91 |191.235.193.75 |
| Güney Afrika - Kuzey  | 102.133.152.0    | | |
| Güney Afrika - Batı | 102.133.24.0   | | |
| Orta Güney ABD |104.214.16.39, 20.45.120.0  |13.66.62.124  |23.98.162.75 |
| Güneydoğu Asya | 104.43.15.0, 23.100.117.95, 40.78.233.2, 23.98.80.12     | | |
| BAE Orta | 20.37.72.64  | | |
| BAE Kuzey | 65.52.248.0    | | |
| Güney Birleşik Krallık | 51.140.184.11   | | |
| Batı Birleşik Krallık | 51.141.8.11  | | |
| Orta Batı ABD | 13.78.145.25     | | |
| West Europe |13.69.105.208,104.40.169.187 |40.68.37.158 | 191.237.232.75|
| Batı ABD |13.86.216.212, 13.86.217.212 |104.42.238.205  | 23.99.34.75|
| Batı ABD 2 | 13.66.226.202  | | |
||||

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portal kullanarak PostgreSQL için Azure veritabanı güvenlik duvarı kuralları oluşturma ve yönetme](./howto-manage-firewall-using-portal.md)
* [Azure CLı kullanarak PostgreSQL için Azure veritabanı güvenlik duvarı kuralları oluşturma ve yönetme](./howto-manage-firewall-using-cli.md)
