---
author: rothja
ms.service: cost-management-billing
ms.topic: include
ms.date: 03/04/2020
ms.author: jroth
ms.openlocfilehash: 505e2d8eec20853fba3743b40cbe289585d14d61
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78305004"
---
| Kaynak | Ücretsiz | Paylaşımlı | Temel | Standart | Premium (v2) | Yalıtılmış </th> |
| --- | --- | --- | --- | --- | --- | --- |
| [Azure Uygulama Hizmeti planına](../articles/app-service/overview-hosting-plans.md)göre [Web, mobil veya API uygulamaları](https://azure.microsoft.com/services/app-service/) <sup>1</sup> |10 |100 |Sınırsız<sup>2</sup> |Sınırsız<sup>2</sup> |Sınırsız<sup>2</sup> |Sınırsız<sup>2</sup>|
| [Uygulama Hizmeti planı](../articles/app-service/overview-hosting-plans.md) |Bölge başına 10 |Kaynak grubu başına 10 |Kaynak grubu başına 100 |Kaynak grubu başına 100 |Kaynak grubu başına 100 |Kaynak grubu başına 100|
| İşlem örneği türü |Paylaşımlı |Paylaşımlı |Özel<sup>3</sup> |Özel<sup>3</sup> |Özel<sup>3</sup></p> |Özel<sup>3</sup>|
| [Ölçeklendirme](../articles/app-service/manage-scale-up.md) (maksimum örnekler) |1 paylaşılan |1 paylaşılan |3 özel<sup>3</sup> |10 özel<sup>3</sup> |30 özel<sup>3</sup>|100 özel<sup>4</sup>|
| Depolama<sup>5</sup> |1 GB<sup>5</sup> |1 GB<sup>5</sup> |10 GB<sup>5</sup> |50 GB<sup>5</sup> |250 GB<sup>5</sup></p> |1 TB<sup>5</sup>|
| CPU süresi (5 dakika)<sup>6</sup> |3 dakika |3 dakika |Sınırsız, standart [fiyatlarla](https://azure.microsoft.com/pricing/details/app-service/) ödeme</a> |Sınırsız, standart [fiyatlarla](https://azure.microsoft.com/pricing/details/app-service/) ödeme</a> |Sınırsız, standart [fiyatlarla](https://azure.microsoft.com/pricing/details/app-service/) ödeme</a> |Sınırsız, standart [fiyatlarla](https://azure.microsoft.com/pricing/details/app-service/) ödeme</a>|
| CPU saati (gün)<sup>6</sup> |60 dakika |240 dakika |Sınırsız, standart [fiyatlarla](https://azure.microsoft.com/pricing/details/app-service/) ödeme</a> |Sınırsız, standart [fiyatlarla](https://azure.microsoft.com/pricing/details/app-service/) ödeme</a> |Sınırsız, standart [fiyatlarla](https://azure.microsoft.com/pricing/details/app-service/) ödeme</a> |Sınırsız, standart [fiyatlarla](https://azure.microsoft.com/pricing/details/app-service/) ödeme</a> |
| Bellek (1 saat) |Uygulama Hizmeti planı başına 1.024 MB |Uygulama başına 1.024 MB |Yok |Yok |Yok |Yok |
| Bant genişliği |165 MB |Sınırsız, [veri aktarım oranları](https://azure.microsoft.com/pricing/details/data-transfers/) geçerlidir |Sınırsız, [veri aktarım oranları](https://azure.microsoft.com/pricing/details/data-transfers/) geçerlidir |Sınırsız, [veri aktarım oranları](https://azure.microsoft.com/pricing/details/data-transfers/) geçerlidir |Sınırsız, [veri aktarım oranları](https://azure.microsoft.com/pricing/details/data-transfers/) geçerlidir |Sınırsız, [veri aktarım oranları](https://azure.microsoft.com/pricing/details/data-transfers/) geçerlidir |
| Uygulama mimarisi |32 bit |32 bit |32-bit/64-bit |32-bit/64-bit |32-bit/64-bit |32-bit/64-bit |
| Örnek başına web soketleri<sup>7</sup> |5 |35 |350 |Sınırsız |Sınırsız |Sınırsız |
| IP bağlantıları | 600 | 600 | Örnek<sup>boyutuna</sup> göre değişir 8 | Örnek<sup>boyutuna</sup> göre değişir 8 | Örnek<sup>boyutuna</sup> göre değişir 8 | 16.000 |
| Uygulama başına eşzamanlı [hata ayıklama bağlantıları](../articles/app-service/troubleshoot-dotnet-visual-studio.md) |1 |1 |1 |5 |5 |5 |
| Abonelik başına Uygulama Hizmet Sertifikaları<sup>9</sup>| Desteklenmiyor | Desteklenmiyor |10 |10 |10 |10 |
| Uygulama başına özel etki alanları</a> |0 (yalnızca azurewebsites.net alt etki alanı)|500 |500 |500 |500 |500 |
| Özel etki alanı [SSL desteği](../articles/app-service/configure-ssl-certificate.md) |Desteklenmiyor, varsayılan olarak *.azurewebsites.net için joker karakter sertifikası|Desteklenmiyor, varsayılan olarak *.azurewebsites.net için joker karakter sertifikası|Sınırsız SNI SSL bağlantısı |Sınırsız SNI SSL ve 1 IP SSL bağlantısı dahil |Sınırsız SNI SSL ve 1 IP SSL bağlantısı dahil | Sınırsız SNI SSL ve 1 IP SSL bağlantısı dahil|
| Plan başına karma bağlantılar | | | 5 | 25 | 200 | 200 |
| Entegre yük dengeleyici | |X |X |X |X |X<sup>10</sup> |
| [Her Zaman Anın](../articles/app-service/configure-common.md) | | |X |X |X |X |
| [Zamanlanmış yedeklemeler](../articles/app-service/manage-backup.md) | | | | Zamanlanmış yedeklemeler her 2 saatte bir, günde en fazla 12 yedekleme (manuel + zamanlanmış) | Zamanlanmış yedeklemeler her saat, günde en fazla 50 yedekleme (manuel + zamanlanmış) | Zamanlanmış yedeklemeler her saat, günde en fazla 50 yedekleme (manuel + zamanlanmış) |
| [Otomatik Ölçeklendirme](../articles/app-service/manage-scale-up.md) | | | |X |X |X |
| [Webİşler](../articles/app-service/webjobs-create.md)<sup>11</sup> |X |X |X |X |X |X |
| [Uç Nokta izleme](../articles/app-service/web-sites-monitor.md) | | |X |X |X |X |
| Uygulama başına [evreleme yuvaları](../articles/app-service/deploy-staging-slots.md)| | | |5 |20 |20 |
| SLA | |  |%99,95|%99,95|%99,95|%99,95|  

<sup>1.1.2</sup> Uygulamalar ve depolama kotaları, aksi belirtilmedikçe Uygulama Hizmeti planına göre dir.  
<sup>2.000</sup> Bu makinelerde barındırabileceğiniz gerçek uygulama sayısı, uygulamaların etkinliğine, makine örneklerinin boyutuna ve ilgili kaynak kullanımına bağlıdır.  
<sup>3.2.2</sup> Özel örnekleri farklı boyutlarda olabilir. Daha fazla bilgi için [Uygulama Hizmeti fiyatlandırması'na](https://azure.microsoft.com/pricing/details/app-service/)bakın.  
<sup>4.2.2</sup> İstek üzerine daha fazla izin verilir.  
<sup>5.000</sup> Depolama sınırı, aynı Uygulama hizmet planındaki tüm uygulamalardaki toplam içerik boyutudur. Tek bir kaynak grubu ve bölgedeki tüm Uygulama hizmet planlarında yer alan tüm uygulamaların toplam içerik boyutu 500 GB'ı geçemez.  
<sup>6.000</sup> Bu kaynaklar, ilgili örneklerdeki (örnek boyutu ve örnek sayısı) fiziksel kaynaklarla sınırlandırılmıştır.  
<sup>7.000</sup> Temel katmandaki bir uygulamayı iki örnekle ölçeklendirirseniz, iki örneğin her biri için 350 eşzamanlı bağlantınız olur. Standart katman ve üzeri için, web soketleri için teorik sınır yoktur, ancak diğer etkenler web soketlerinin sayısını sınırlayabilir. Örneğin, izin verilen maksimum eşzamanlı `maxConcurrentRequestsPerCpu`istekler (tanımlanan) şunlardır: küçük VM başına 7.500, orta VM başına 15.000 (7.500 x 2 çekirdek) ve 75.000 büyük VM (18.750 x 4 çekirdek) başına.  
<sup>8.000</sup> Maksimum IP bağlantıları örnek başına ve örnek boyutuna bağlıdır: B1/S1/P1V2 örneğine 1.920, B2/S2/P2V2 örneğine göre 3.968, B3/S3/P3V2 örneğine göre 8.064.  
<sup>9.000</sup> Abonelik başına Uygulama Hizmeti Sertifikası kota sınırı, destek isteği yoluyla maksimum 200 sınırına yükseltilebilir.  
<sup>10.000</sup> Uygulama Hizmeti İzole SK'leri Azure Yük Dengeleyicisi ile dahili olarak dengeli (ILB) yüklenebilir, böylece internetten genel bağlantı yoktur. Sonuç olarak, ILB İzole Uygulama Hizmetinin bazı özellikleri, ILB ağ bitiş noktasına doğrudan erişimi olan makinelerden kullanılmalıdır.  
<sup>11.11.20</sup> İsteğe bağlı olarak, zamanlamada veya Uygulama Hizmeti örneğiniz içinde sürekli olarak arka plan görevi olarak özel yürütülebilir ve/veya komut dosyaları çalıştırın. Sürekli Webİşler yürütmesi için Her Zaman Açık gereklidir. Bir Uygulama Hizmeti örneğinde çalıştırılabilen Webİşler in sayısında önceden tanımlanmış bir sınır yoktur. Uygulama kodunun ne yapmaya çalıştığına bağlı olarak pratik sınırlar vardır.  
