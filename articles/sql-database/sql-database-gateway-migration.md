---
title: Ağ geçidi trafik geçiş bildirimi
description: Makale, Kullanıcılara Azure SQL Veritabanı Ağ Geçitleri IP adreslerinin geçişi hakkında bildirim sağlar
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto
ms.date: 07/01/2019
ms.openlocfilehash: fe35dc4c22f3852934cde0d6f33084b56266d514
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73807698"
---
# <a name="azure-sql-database-traffic-migration-to-newer-gateways"></a>Azure SQL Veritabanı trafiğinin yeni ağ geçitlerine geçişi

Azure altyapısı geliştikçe, Microsoft mümkün olan en iyi müşteri deneyimini sağladığımızdan emin olmak için donanımı düzenli aralıklarla yeniler. Önümüzdeki aylarda, yeni donanım nesilleri üzerine inşa edilmiş Ağ Geçitleri eklemeyi, trafiği onlara geçirmeyi ve sonunda bazı bölgelerde ki eski donanımlar üzerine inşa edilmiş Ağ Geçitlerini devre dışı bırakmayı planlıyoruz.  

Müşteriler, her bölgede bulunan Ağ Geçitleri'nde yapılacak herhangi bir değişiklikten çok önce e-posta yoluyla ve Azure portalında bilgilendirilecektir. En güncel bilgiler Azure SQL Veritabanı ağ [geçidi IP adresleri](sql-database-connectivity-architecture.md#azure-sql-database-gateway-ip-addresses) tablosunda tutulur.

## <a name="impact-of-this-change"></a>Bu değişikliğin etkisi

Yeni Ağ Geçitlerine trafik geçişinin ilk turu **14 Ekim 2019'da** aşağıdaki bölgelerde yapılacaktır:
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

Trafik geçişi, DNS'nin SQL Veritabanınız için çözdüğünortak IP adresini değiştirir.
Varsa etkilenirsiniz:
- Şirket içi güvenlik duvarınızdaki belirli bir Ağ Geçidi'nin IP adresini sabit kodladı
- Microsoft.SQL'i Hizmet Bitiş Noktası olarak kullanan ancak Ağ Geçidi IP adresleriyle iletişim kuramayan alt ağlar

Eğer varsa etkilenmez:
- Bağlantı ilkesi olarak yeniden yönlendirme
- Azure'un içinden SQL Veritabanına bağlantılar ve Hizmet Etiketleri kullanma
- SQL Server için JDBC Driver'ın desteklenen sürümleri kullanılarak yapılan bağlantılarhiçbir etki görmez. Desteklenen JDBC sürümleri [için SQL Server için Microsoft JDBC Sürücüsünü İndir'e](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server)bakın.

## <a name="what-to-do-you-do-if-youre-affected"></a>Etkilenirseniz ne yapabilirsiniz?

TCP 1433 ve bağlantı noktası aralığı 11000-11999'daki bölgedeki tüm [Azure SQL Veritabanı ağ geçidi IP adresleri](sql-database-connectivity-architecture.md#azure-sql-database-gateway-ip-addresses) için IP adreslerine giden trafiğe izin vermenizi öneririz. Bu öneri, şirket içinde bağlantı kuran istemciler ve Hizmet Bitiş Noktaları üzerinden bağlanan müşteriler için geçerlidir. Bağlantı noktası aralıkları hakkında daha fazla bilgi için [Bağlantı ilkesine](sql-database-connectivity-architecture.md#connection-policy)bakın.

Sürüm 4.0'ın altındaki Microsoft JDBC Driver'ı kullanan uygulamalardan yapılan bağlantılar sertifika doğrulamada başarısız olabilir. Microsoft JDBC'nin alt sürümleri, sertifikanın Konu alanında Ortak Ad'a (CN) dayanır. Azaltma, hostNameInCertificate özelliğinin *.database.windows.net olarak ayarlanmasını sağlamaktır. HostNameInCertificate özelliğinin nasıl ayarlandığı hakkında daha fazla bilgi için [bkz.](/sql/connect/jdbc/connecting-with-ssl-encryption)

Yukarıdaki azaltma işe yaramazsa, aşağıdaki URL'yi kullanarak SQL Veritabanı için bir destek isteği dosyala:https://aka.ms/getazuresupport

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL [Bağlantı Mimarisi](sql-database-connectivity-architecture.md) hakkında daha fazla bilgi edinin
