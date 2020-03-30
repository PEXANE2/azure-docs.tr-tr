---
title: Azure Monitör'de Linux uygulama performansını toplayın | Microsoft Dokümanlar
description: Bu makalede, MySQL ve Apache HTTP Server için performans sayaçları toplamak için Linux için Log Analytics aracısını yapılandırmak için ayrıntılar verilmektedir.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/04/2017
ms.openlocfilehash: 2fd148dbb85a4fd60fe63d4fb73128bf92dea1d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670568"
---
# <a name="collect-performance-counters-for-linux-applications-in-azure-monitor"></a>Azure Monitor'da Linux uygulamaları için performans sayaçları toplama 
[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]
Bu makalede, Belirli uygulamalar için performans sayaçları toplamak [için Linux için Log Analytics aracısını](https://github.com/Microsoft/OMS-Agent-for-Linux) Azure Monitor'da yapılandırmak için ayrıntılar verilmektedir.  Bu makalede yer alan uygulamalar şunlardır:  

- [Mysql](#mysql)
- [Apache HTTP Sunucu](#apache-http-server)

## <a name="mysql"></a>MySQL
Log Analytics aracısı yüklendiğinde bilgisayarda MySQL Server veya MariaDB Server algılanırsa, MySQL Server için bir performans izleme sağlayıcısı otomatik olarak yüklenir. Bu sağlayıcı, performans istatistiklerini ortaya çıkarmak için yerel MySQL/MariaDB sunucusuna bağlanır. MySQL kullanıcı kimlik bilgileri, sağlayıcının MySQL Server'a erişebilmeleri için yapılandırılmalıdır.

### <a name="configure-mysql-credentials"></a>MySQL kimlik bilgilerini yapılandırma
MySQL OMI sağlayıcısı, MySQL örneğindeki performans ve sistem durumu bilgilerini sorgulamak için önceden yapılandırılmış bir MySQL kullanıcısına ve MySQL istemci kitaplıklarına yüklenir.  Bu kimlik bilgileri, Linux aracısında depolanan bir kimlik doğrulama dosyasında depolanır.  Kimlik doğrulama dosyası, MySQL örneğinin hangi bağlama adresini ve bağlantı noktasını dinlediğini ve ölçümleri toplamak için hangi kimlik bilgilerini kullanacağımı belirtir.  

Linux için Log Analytics aracısının yüklenmesi sırasında MySQL OMI sağlayıcısı, bağlama adresi ve bağlantı noktası için MySQL my.cnf yapılandırma dosyalarını (varsayılan konumlar) tarar ve MySQL OMI kimlik doğrulama dosyasını kısmen ayarlar.

MySQL kimlik doğrulama dosyası `/var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth`.


### <a name="authentication-file-format"></a>Kimlik doğrulama dosya biçimi
Aşağıdaki MySQL OMI kimlik doğrulama dosyası için biçimidir

    [Port]=[Bind-Address], [username], [Base64 encoded Password]
    (Port)=(Bind-Address), (username), (Base64 encoded Password)
    (Port)=(Bind-Address), (username), (Base64 encoded Password)
    AutoUpdate=[true|false]

Kimlik doğrulama dosyasındaki girişler aşağıdaki tabloda açıklanmıştır.

| Özellik | Açıklama |
|:--|:--|
| Bağlantı noktası | MySQL örneğinin dinlediği geçerli bağlantı noktasını temsil eder. Bağlantı noktası 0, aşağıdaki özelliklerin varsayılan örnek için kullanıldığını belirtir. |
| Bind Adresi| Geçerli MySQL bind adresi. |
| kullanıcı adı| MySQL kullanıcı MySQL sunucu örneğini izlemek için kullanılır. |
| Base64 kodlanmış Şifre| Base64'te kodlanmış MySQL izleme kullanıcısının şifresi. |
| Autoupdate| MySQL OMI Sağlayıcısı yükseltildiğinde my.cnf dosyasındaki değişiklikleri yeniden mi yeniden tarayıp değiştirmeyeceğinizve MySQL OMI Kimlik Doğrulama dosyasının üzerine yazıp yazılmayacağımı belirtir. |

### <a name="default-instance"></a>Varsayılan örnek
MySQL OMI kimlik doğrulama dosyası, birden çok MySQL örneğini tek bir Linux ana bilgisayarında yönetmeyi kolaylaştırmak için varsayılan bir örnek ve bağlantı noktası numarası tanımlayabilir.  Varsayılan örnek, bağlantı noktası 0 olan bir örnek le gösterilir. Farklı değerler belirtmedikleri sürece, tüm ek örnekler varsayılan örnekten ayarlanan özellikleri devralır. Örneğin, '3308' portundan dinleyen MySQL örneği eklenirse, 3308'i dinlemeyi deneyen örneği izlemek için varsayılan örneğin bağlama adresi, kullanıcı adı ve Base64 kodlanmış parolası kullanılır. 3308'deki örnek başka bir adrese bağlıysa ve aynı MySQL kullanıcı adı ve parola çiftini kullanıyorsa yalnızca bind adresi gereklidir ve diğer özellikler devralınır.

Aşağıdaki tabloda örnek durum ayarları vardır 

| Açıklama | Dosya |
|:--|:--|
| Varsayılan örnek ve örnek port 3308 ile. | `0=127.0.0.1, myuser, cnBwdA==`<br>`3308=, ,`<br>`AutoUpdate=true` |
| Varsayılan örnek ve örnek port 3308 ve farklı kullanıcı adı ve şifre ile. | `0=127.0.0.1, myuser, cnBwdA==`<br>`3308=127.0.1.1, myuser2,cGluaGVhZA==`<br>`AutoUpdate=true` |


### <a name="mysql-omi-authentication-file-program"></a>MySQL OMI Kimlik Doğrulama Dosya Programı
MySQL OMI sağlayıcısının kurulumu ile mysql OMI Kimlik Doğrulama dosyasını düzenleyen kullanılabilecek bir MySQL OMI kimlik doğrulama dosyası programı dır. Kimlik doğrulama dosyası programı aşağıdaki konumda bulunabilir.

    /opt/microsoft/mysql-cimprov/bin/mycimprovauth

> [!NOTE]
> Kimlik bilgileri dosyası omsagent hesabı tarafından okunabilir olmalıdır. Omsgent olarak mycimprovauth komutu çalıştırılsın önerilir.

Aşağıdaki tablomyimprovauth kullanmak için sözdizimi ayrıntıları sağlar.

| İşlem | Örnek | Açıklama
|:--|:--|:--|
| yanlış *veya doğru* otomatik güncelleştirme | mycimprovauth otomatik güncelleme yanlış | Kimlik doğrulama dosyasının yeniden başlat'ta veya güncelleştirmede otomatik olarak güncelleştirilip güncelleştirilmeyeceğini ayarlar. |
| varsayılan *bind adresi kullanıcı adı parolası* | mycimprovauth varsayılan 127.0.0.1 kök pwd | MySQL OMI kimlik doğrulama dosyasındaki varsayılan örneği ayarlar.<br>Parola alanı düz metin olarak girilmelidir - MySQL OMI kimlik doğrulama dosyasındaki parola Base 64 kodlu olacaktır. |
| *varsayılan veya port_num* silme | mycimprovauth 3308 | Belirtilen örneği varsayılan veya bağlantı noktası numarasıyla siler. |
| Yardım | mycimprov yardım | Kullanılacak komutların listesini yazdırır. |
| yazdır | mycimprov baskı | Kolay okunan MySQL OMI kimlik doğrulama dosyasını yazdırır. |
| port_num *bind adresi kullanıcı adı parolası güncelleştirme* | mycimprov güncelleme 3307 127.0.0.1 kök pwd | Belirtilen örneği güncelleştirir veya yoksa örneği ekler. |

Aşağıdaki örnek komutlar, localhost'taki MySQL sunucusu için varsayılan bir kullanıcı hesabı tanımlar.  Parola alanı düz metin olarak girilmelidir - MySQL OMI kimlik doğrulama dosyasındaki parola Base 64 kodlu olacaktır

    sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>'
    sudo /opt/omi/bin/service_control restart

### <a name="database-permissions-required-for-mysql-performance-counters"></a>MySQL Performans Sayaçları Için Gerekli Veritabanı İzinleri
MySQL Kullanıcı MySQL Server performans verilerini toplamak için aşağıdaki sorgulara erişim gerektirir. 

    SHOW GLOBAL STATUS;
    SHOW GLOBAL VARIABLES:


MySQL kullanıcısı ayrıca aşağıdaki varsayılan tablolara SELECT erişimi gerektirir.

- ınformatıon_schema
- Mysql. 

Bu ayrıcalıklar aşağıdaki hibe komutları çalıştırılarak verilebilir.

    GRANT SELECT ON information_schema.* TO ‘monuser’@’localhost’;
    GRANT SELECT ON mysql.* TO ‘monuser’@’localhost’;


> [!NOTE]
> MySQL izleme kullanıcısına izin vermek için, verilen kullanıcının 'GRANT seçeneği' ayrıcalığına ve ayrıcalığa sahip olması gerekir.

### <a name="define-performance-counters"></a>Performans sayaçlarını tanımlama

Linux'un Azure Monitor'a veri gönderecek şekilde Log Analytics aracısını yapılandırdıktan sonra, performans sayaçlarını toplayacak şekilde yapılandırmanız gerekir.  Aşağıdaki tablodaki sayaçlarla [Azure Monitor'daki Windows ve Linux performans veri kaynaklarında](data-sources-performance-counters.md) yordamı kullanın.

| Nesne Adı | Sayaç Adı |
|:--|:--|
| MySQL Veritabanı | Baytlarda Disk Alanı |
| MySQL Veritabanı | Tablolar |
| MySQL Sunucusu | İptal Bağlantı Pct |
| MySQL Sunucusu | Bağlantı Kullanımı Pct |
| MySQL Sunucusu | Baytlarda Disk Alanı Kullanımı |
| MySQL Sunucusu | Tam Masa Scan Pct |
| MySQL Sunucusu | InnoDB Tampon Havuzu Hit Pct |
| MySQL Sunucusu | InnoDB Tampon Havuzu Kullanımı Pct |
| MySQL Sunucusu | InnoDB Tampon Havuzu Kullanımı Pct |
| MySQL Sunucusu | Anahtar Önbellek Hit Pct |
| MySQL Sunucusu | Anahtar Önbellek Kullanımı Pct |
| MySQL Sunucusu | Anahtar Önbellek Pct yazma |
| MySQL Sunucusu | Sorgu Önbellek Hit Pct |
| MySQL Sunucusu | Sorgu Önbellek Prunes Pct |
| MySQL Sunucusu | Sorgu Önbellek Kullanımı Pct |
| MySQL Sunucusu | Tablo Önbellek Hit Pct |
| MySQL Sunucusu | Tablo Önbellek Kullanımı Pct |
| MySQL Sunucusu | Masa Kilidi Çekişme Pct |

## <a name="apache-http-server"></a>Apache HTTP Sunucu 
Omsagent paketi yüklendiğinde bilgisayarda Apache HTTP Server algılanırsa, Apache HTTP Server için bir performans izleme sağlayıcısı otomatik olarak yüklenir. Bu sağlayıcı, performans verilerine erişmek için Apache HTTP Sunucusuna yüklenmesi gereken bir Apache modülüne dayanır. Modül aşağıdaki komutla yüklenebilir:
```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

Apache izleme modülünü boşaltmak için aşağıdaki komutu çalıştırın:
```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```

### <a name="define-performance-counters"></a>Performans sayaçlarını tanımlama

Linux'un Azure Monitor'a veri gönderecek şekilde Log Analytics aracısını yapılandırdıktan sonra, performans sayaçlarını toplayacak şekilde yapılandırmanız gerekir.  Aşağıdaki tablodaki sayaçlarla [Azure Monitor'daki Windows ve Linux performans veri kaynaklarında](data-sources-performance-counters.md) yordamı kullanın.

| Nesne Adı | Sayaç Adı |
|:--|:--|
| Apache HTTP Sunucu | Meşgul İşçiler |
| Apache HTTP Sunucu | Boşta Çalışanlar |
| Apache HTTP Sunucu | Pct Meşgul İşçiler |
| Apache HTTP Sunucu | Toplam Pct CPU |
| Apache Sanal Host | Dakika Başına Hatalar - İstemci |
| Apache Sanal Host | Dakika Başına Hatalar - Sunucu |
| Apache Sanal Host | İstek Başına KB |
| Apache Sanal Host | Saniyede KB Istekleri |
| Apache Sanal Host | Saniye Başına İstekler |



## <a name="next-steps"></a>Sonraki adımlar
* Linux ajanlarından [performans sayaçları toplayın.](data-sources-performance-counters.md)
* Veri kaynaklarından ve çözümlerinden toplanan verileri analiz etmek için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin. 
