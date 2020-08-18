---
title: Ağ Geçidi trafiği geçiş bildirimi
description: Makale, Azure SQL veritabanı ağ geçidi IP adreslerini geçirme hakkında kullanıcılara bildirim sağlar
services: sql-database
ms.service: sql-db-mi
ms.subservice: service
ms.custom: sqldbrb=1 
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto
ms.date: 07/01/2019
ms.openlocfilehash: af5486630eb89de198b6ed2975a919b04b01a902
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88507527"
---
# <a name="azure-sql-database-traffic-migration-to-newer-gateways"></a>Azure SQL veritabanı trafiğini daha yeni ağ geçitlerine geçirme
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Azure altyapısı gelişdiğinde, Microsoft, olası en iyi müşteri deneyimini sağlamamızı sağlamak için donanımı düzenli olarak yenileyecek. Önümüzdeki aylarda, daha yeni donanım oluşturma, trafiği geçirme ve son olarak bazı bölgelerde daha eski donanımlar üzerinde oluşturulmuş ağ geçitlerinin yetkisini alma planlıyoruz.  

Müşteriler, e-posta ile ve her bölgede kullanılabilir olan ağ geçitlerinde yapılan herhangi bir değişikliğe göre Azure portal iyi bir şekilde bildirilir. En güncel bilgiler [Azure SQL veritabanı ağ GEÇIDI IP adresleri](connectivity-architecture.md#gateway-ip-addresses) tablosunda tutulacaktır.

## <a name="status-updates"></a>Durum güncelleştirmeleri

# <a name="in-progress"></a>[Sürüyor](#tab/in-progress-ip)
### <a name="september-2020"></a>Eylül 2020
Yeni SQL ağ geçitleri aşağıdaki bölgelere ekleniyor. Bu SQL ağ geçitleri, **15 eylül 2020**' de müşteri trafiğini kabul etmeye başlayacaktır:

- Avustralya Güneydoğu: 13.77.48.10
- Kanada Doğu: 40.86.226.166, 52.242.30.154
- UK Güney: 51.140.184.11, 51.105.64.0

Mevcut SQL ağ geçitleri aşağıdaki bölgelerde trafiği kabul etmeye başlayacaktır. Bu SQL ağ geçitleri, **15 eylül 2020** ' de müşteri trafiğini kabul etmeye başlayacaktır:

- Avustralya Güneydoğu: 191.239.192.109 ve 13.73.109.251
- Orta ABD: 13.67.215.62, 52.182.137.15, 23.99.160.139, 104.208.16.96 ve 104.208.21.1
- Doğu Asya: 191.234.2.139, 52.175.33.150 ve 13.75.32.4
- Doğu ABD: 40.121.158.30, 40.79.153.12, 191.238.6.43 ve 40.78.225.32
- Doğu ABD 2:40.79.84.180, 52.177.185.181, 52.167.104.0, 191.239.224.107 ve 104.208.150.3
- Fransa Orta: 40.79.137.0 ve 40.79.129.1
- Japonya Batı: 104.214.148.156, 40.74.100.192, 191.238.68.11 ve 40.74.97.10
- Orta Kuzey ABD: 23.96.178.199, 23.98.55.75 ve 52.162.104.33
- Güneydoğu Asya: 104.43.15.0, 23.100.117.95 ve 40.78.232.3
- Batı ABD: 104.42.238.205, 23.99.34.75 ve 13.86.216.196

Yeni SQL ağ geçitleri aşağıdaki bölgelere ekleniyor. Bu SQL ağ geçitleri, **10 eylül 2020**' de müşteri trafiğini kabul etmeye başlayacaktır:

- Orta Batı ABD: 13.78.248.43 
- Güney Afrika Kuzey: 102.133.120.2  

Yeni SQL ağ geçitleri aşağıdaki bölgelere ekleniyor. Bu SQL ağ geçitleri, **1 eylül 2020**' de müşteri trafiğini kabul etmeye başlayacaktır:

- Kuzey Avrupa: 13.74.104.113 
- Batı ABD2:40.78.248.10 
- Batı Avrupa: 52.236.184.163 
- Orta Güney ABD: 20.45.121.1, 20.49.88.1 

Mevcut SQL ağ geçitleri aşağıdaki bölgelerde trafiği kabul etmeye başlayacaktır. Bu SQL ağ geçitleri, **1 eylül 2020** ' de müşteri trafiğini kabul etmeye başlayacaktır:
- Japonya Doğu: 40.79.184.8, 40.79.192.5

# <a name="completed"></a>[Tamamlandı](#tab/completed-ip)

Aşağıdaki ağ geçidi geçişleri tamamlanmıştır: 

### <a name="august-2020"></a>Ağustos 2020

Yeni SQL ağ geçitleri aşağıdaki bölgelere ekleniyor:

- Avustralya Doğu: 13.70.112.9
- Kanada Orta: 52.246.152.0, 20.38.144.1 
- Batı ABD 2:40.78.240.8

Bu SQL ağ geçitleri, 10 Ağustos 2020 ' de müşteri trafiğini kabul etmeye başlar. 

### <a name="october-2019"></a>Ekim 2019
- Brezilya Güney
- Batı ABD
- West Europe
- Doğu ABD
- Central US
- Güneydoğu Asya
- Orta Güney ABD
- Kuzey Avrupa
- Orta Kuzey ABD
- Batı Japonya
- Doğu Japonya
- Doğu ABD 2
- Doğu Asya

---

## <a name="impact-of-this-change"></a>Bu değişikliğin etkisi

Trafik geçişi, Azure SQL veritabanı 'nda veritabanınız için DNS tarafından çözümlenen genel IP adresini değiştirebilir.
Şu durumlarda etkilenmiş olabilirsiniz:

- Şirket içi güvenlik duvarınızdaki belirli bir ağ geçidinin IP adresini sabit olarak kodlanmış
- Hizmet uç noktası olarak Microsoft. SQL kullanan ancak ağ geçidi IP adresleriyle iletişim kuramayan alt ağlarınız var
- Veritabanınız için bölgesel olarak [yedekli yapılandırmayı](high-availability-sla.md#zone-redundant-configuration) kullanın

Şunları yaptıysanız etkilenmeyecektir:
 
- Bağlantı ilkesi olarak yeniden yönlendirme
- Azure 'un içinden ve hizmet etiketlerini kullanarak SQL veritabanı 'na bağlantı
- SQL Server için JDBC sürücüsünün desteklenen sürümleri kullanılarak yapılan bağlantılar, hiçbir etkisi görmez. Desteklenen JDBC sürümleri için bkz. [SQL Server Için MICROSOFT JDBC sürücüsü indirme](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server).

## <a name="what-to-do-you-do-if-youre-affected"></a>Etkileniyorsanız yapmanız gerekenler

TCP bağlantı noktası 1433 ve bağlantı noktası aralığı 11000-11999 ' deki bölgedeki tüm [ağ GEÇIDI IP adresleri](connectivity-architecture.md#gateway-ip-addresses) için IP adreslerine giden trafiğe izin vermeniz önerilir. Bu öneri, Şirket içinden bağlanan istemciler ve ayrıca hizmet uç noktaları aracılığıyla bağlanan istemciler için geçerlidir. Bağlantı noktası aralıkları hakkında daha fazla bilgi için bkz. [bağlantı ilkesi](connectivity-architecture.md#connection-policy).

4,0 sürümünün altında Microsoft JDBC sürücüsü kullanılarak gerçekleştirilen uygulamalardan yapılan bağlantılar sertifika doğrulaması başarısız olabilir. Microsoft JDBC 'nın daha düşük sürümleri, sertifikanın konu alanında ortak adı (CN) kullanır. Risk azaltma, Hostnameincercertificate özelliğinin *. database.windows.net olarak ayarlandığından emin olunması. Hostnameincercertificate özelliğinin nasıl ayarlanacağı hakkında daha fazla bilgi için bkz. [şifreleme Ile bağlanma](/sql/connect/jdbc/connecting-with-ssl-encryption).

Yukarıdaki hafifletme işe yaramazsa, aşağıdaki URL 'YI kullanarak SQL veritabanı veya SQL yönetilen örneği için bir destek isteği dosyası kullanın: https://aka.ms/getazuresupport

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL bağlantı mimarisi](connectivity-architecture.md) hakkında daha fazla bilgi edinin
