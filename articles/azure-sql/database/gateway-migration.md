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
ms.openlocfilehash: e9bf1f06b1ec1f99da1ce653b4bc72f4638ba451
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86084979"
---
# <a name="azure-sql-database-traffic-migration-to-newer-gateways"></a>Azure SQL veritabanı trafiğini daha yeni ağ geçitlerine geçirme
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Azure altyapısı gelişdiğinde, Microsoft, olası en iyi müşteri deneyimini sağlamamızı sağlamak için donanımı düzenli olarak yenileyecek. Önümüzdeki aylarda, daha yeni donanım oluşturma, trafiği geçirme ve son olarak bazı bölgelerde daha eski donanımlar üzerinde oluşturulmuş ağ geçitlerinin yetkisini alma planlıyoruz.  

Müşteriler, e-posta ile ve her bölgede kullanılabilir olan ağ geçitlerinde yapılan herhangi bir değişikliğe göre Azure portal iyi bir şekilde bildirilir. En güncel bilgiler [Azure SQL veritabanı ağ GEÇIDI IP adresleri](connectivity-architecture.md#gateway-ip-addresses) tablosunda tutulacaktır.

## <a name="status-updates"></a>Durum güncelleştirmeleri

# <a name="in-progress"></a>[Sürüyor](#tab/in-progress-ip)

### <a name="august-2020"></a>Ağustos 2020

Yeni SQL ağ geçitleri aşağıdaki bölgelere ekleniyor:

- Avustralya Doğu: 13.70.112.9
- Kanada Orta: 52.246.152.0, 20.38.144.1 
- Batı ABD 2:40.78.240.8

Bu SQL ağ geçitleri, 10 Ağustos 2020 ' de müşteri trafiğini kabul etmeye başlar. 

# <a name="completed"></a>[Tamamlandı](#tab/completed-ip)

Aşağıdaki ağ geçidi geçişleri tamamlanmıştır: 

### <a name="october-2019"></a>Ekim 2019
- Güney Brezilya
- Batı ABD
- Batı Avrupa
- Doğu ABD
- Orta ABD
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

Yukarıdaki hafifletme işe yaramazsa, aşağıdaki URL 'YI kullanarak SQL veritabanı veya SQL yönetilen örneği için bir destek isteği dosyası kullanın:https://aka.ms/getazuresupport

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL bağlantı mimarisi](connectivity-architecture.md) hakkında daha fazla bilgi edinin
