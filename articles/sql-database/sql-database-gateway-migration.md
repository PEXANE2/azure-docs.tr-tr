---
title: Azure SQL veritabanı için ağ geçidi trafik geçişi bildirimi | Microsoft Docs
description: Makale, kullanıcılara Azure SQL veritabanı ağ geçitleri IP adreslerini geçirme hakkında bildirim sağlar
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto
ms.date: 07/01/2019
ms.openlocfilehash: 1fc6c054b32c62fbebaa2af738e25ef0dec362ac
ms.sourcegitcommit: 6d2a147a7e729f05d65ea4735b880c005f62530f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69981298"
---
# <a name="azure-sql-database-traffic-migration-to-newer-gateways"></a>Azure SQL veritabanı trafiğini daha yeni ağ geçitlerine geçirme

Azure altyapısı gelişdiğinde, Microsoft, olası en iyi müşteri deneyimini sağlamamızı sağlamak için donanımı düzenli olarak yenileyecek. Önümüzdeki aylarda, daha yeni donanım oluşturma, trafiği geçirme ve son olarak bazı bölgelerde daha eski donanımlar üzerinde oluşturulmuş ağ geçitlerinin yetkisini alma planlıyoruz.  

Müşteriler, e-posta ile ve her bölgede kullanılabilir olan ağ geçitlerinde yapılan herhangi bir değişikliğe göre Azure portal iyi bir şekilde bildirilir. En güncel bilgiler [Azure SQL veritabanı ağ GEÇIDI IP adresleri](sql-database-connectivity-architecture.md#azure-sql-database-gateway-ip-addresses) tablosunda tutulacaktır.

## <a name="impact-of-this-change"></a>Bu değişikliğin etkisi

Trafik geçişinin yeni ağ geçitlerine ilk turu, aşağıdaki bölgelerde **14 ekim 2019** için zamanlanır:
- Güney Brezilya
- Batı ABD
- Batı Avrupa
- East US
- Orta ABD
- Güneydoğu Asya
- Orta Güney ABD
- Kuzey Avrupa
- Orta Kuzey ABD
- Japonya Batı
- Japonya Doğu
- Doğu ABD 2
- Doğu Asya

Trafik geçişi, DNS 'nin SQL veritabanınız için çözümlediği genel IP adresini değiştirecek.
Varsa bu sorundan etkilenirsiniz.
- Şirket içi güvenlik duvarınızdaki belirli bir ağ geçidinin IP adresini sabit olarak kodlanmış
- Hizmet uç noktası olarak Microsoft. SQL kullanan tüm alt ağlar, ancak ağ geçidi IP adresleriyle iletişim kuramaz

Sahip olmanız durumunda etkilenmeyecektir. 
- Bağlantı ilkesi olarak yeniden yönlendirme
- Azure 'un içinden ve hizmet etiketlerini kullanarak SQL veritabanı 'na bağlantı
- SQL Server için JDBC sürücüsünün desteklenen sürümleri kullanılarak yapılan bağlantılar, hiçbir etkisi görmez. Desteklenen JDBC sürümleri için bkz. [SQL Server Için MICROSOFT JDBC sürücüsü indirme](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server).

## <a name="what-to-do-you-do-if-youre-affected"></a>Etkileniyorsanız yapmanız gerekenler

TCP bağlantı noktası 1433 ' deki bölgedeki tüm [Azure SQL veritabanı ağ GEÇIDI IP adresleri](sql-database-connectivity-architecture.md#azure-sql-database-gateway-ip-addresses) ve güvenlik duvarı cihazınızda 11000-11999 bağlantı noktası ARALıĞı için IP adreslerine giden trafiğe izin vermeniz önerilir. Bağlantı noktası aralıkları hakkında daha fazla bilgi için bkz. [bağlantı ilkesi](sql-database-connectivity-architecture.md#connection-policy).

4,0 sürümünün altında Microsoft JDBC sürücüsü kullanılarak gerçekleştirilen uygulamalardan yapılan bağlantılar sertifika doğrulaması başarısız olabilir. Microsoft JDBC 'nın daha düşük sürümleri, sertifikanın konu alanında ortak adı (CN) kullanır. Risk azaltma, Hostnameincercertificate özelliğinin *. database.windows.net olarak ayarlandığından emin olunması. Hostnameincercertificate özelliğinin nasıl ayarlanacağı hakkında daha fazla bilgi için bkz. [SSL şifrelemesiyle bağlanma](/sql/connect/jdbc/connecting-with-ssl-encryption).

Yukarıdaki hafifletme işe yaramazsa, aşağıdaki URL 'YI kullanarak SQL veritabanı için bir destek isteği dosyası kullanın: https://aka.ms/getazuresupport

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL bağlantı mimarisi](sql-database-connectivity-architecture.md) hakkında daha fazla bilgi edinin