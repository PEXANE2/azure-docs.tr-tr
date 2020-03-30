---
title: Geçici hatalarla çalışma
description: Azure SQL Veritabanı'nda bir SQL bağlantı hatasını veya geçici hatasını nasıl giderecek, tanılave önleyin.
keywords: sql bağlantısı,bağlantı dizesi,bağlantı sorunları,geçici hata,bağlantı hatası
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dalechen
manager: dcscontentpm
ms.author: ninarn
ms.reviewer: carlrab, vanto
ms.date: 01/14/2020
ms.openlocfilehash: d2b56e259f551f7655936c975a7a864a27a1df79
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79269087"
---
# <a name="troubleshooting-transient-connection-errors-to-sql-database"></a>SQL Veritabanı'nda geçici bağlantı hatalarını giderme sorunu

Bu makalede, istemci uygulamanızın Azure SQL Veritabanı ile etkileşimde karşılaştığı bağlantı hatalarını ve geçici hataları nasıl önleyeceğiniz, sorun gidereceğimiz, tanılanın ve azaltılatanınız açıklanmaktadır. Yeniden deneme mantığını yapılandırmayı, bağlantı dizesini oluşturmayı ve diğer bağlantı ayarlarını nasıl ayarlayabilirsiniz öğrenin.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Geçici hatalar (geçici hatalar)

Geçici hata olarak da bilinen geçici bir hatanın, kısa sürede kendiliğinden düzelen bir nedeni vardır. Geçici hataların zaman zaman nedeni, Azure sisteminin donanım kaynaklarını çeşitli iş yüklerini daha iyi yük dengelemek için hızla değiştirmesidir. Bu yeniden yapılandırma olaylarının çoğu 60 saniyeden kısa bir sürede sona erebiliyor. Bu yeniden yapılandırma süresi sırasında, SQL Veritabanı'na bağlantı sorunlarınız olabilir. SQL Veritabanı'na bağlanan uygulamalar, bu geçici hataları beklemek üzere oluşturulmalıdır. Bunları işlemek için, uygulama hatası olarak kullanıcılara yüzeye çıkarmak yerine kodlarında yeniden deneme mantığını uygulayın.

İstemci programınız ADO.NET kullanıyorsa, programınıza **SqlException**atılarak geçici hata dan bahsedilir. 

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-vs-command"></a>Bağlantı ve komut

Aşağıdakibağlı olarak SQL bağlantısını yeniden deneyin veya yeniden kurun:

- **Bağlantı denemesi sırasında geçici bir hata oluşur**

Birkaç saniye lik bir gecikmeden sonra bağlantıyı yeniden deneyin.

- **SQL sorgu komutu sırasında geçici bir hata oluşur**

Komutu hemen yeniden denemeyin. Bunun yerine, bir gecikmeden sonra, bağlantıyı yeni kurun. O zaman komutu yeniden dene.

<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

## <a name="retry-logic-for-transient-errors"></a>Geçici hatalar için yeniden deneme mantığı

Bazen geçici bir hatayla karşılaşan istemci programları, yeniden deneme mantığı içerdiklerinde daha sağlamdır. Programınız ÜÇÜNCÜ taraf ara yazılımlar aracılığıyla SQL Veritabanı ile iletişim kurduğunda, satıcıya ara yazılımın geçici hatalar için yeniden deneme mantığı içerip içermediğini sorun.

<a id="principles-for-retry" name="principles-for-retry"></a>

### <a name="principles-for-retry"></a>Yeniden deneme prensipleri

- Hata geçiciyse, bağlantıyı yeniden açmayı deneyin.
- Geçici bir hatayla `SELECT` başarısız olan bir SQL deyimini doğrudan yeniden denemeyin. Bunun yerine, yeni bir bağlantı kurun ve sonra `SELECT`yeniden deneyin.
- Bir SQL `UPDATE` deyimi geçici bir hatayla başarısız olduğunda, UPDATE'i yeniden denemeden önce yeni bir bağlantı kurun. Yeniden deneme mantığı, tüm veritabanı hareketinin tamamlandığından veya tüm hareketin geri alolduğundan emin olmalıdır.

### <a name="other-considerations-for-retry"></a>Yeniden deneme için diğer hususlar

- Otomatik olarak çalışma saatlerinden sonra başlayan ve sabahtan önce biten bir toplu iş programı, yeniden deneme denemeleri arasında uzun zaman aralıkları ile çok sabırlı olmayı göze alabilir.
- Bir kullanıcı arabirimi programı çok uzun bir bekleyişten sonra vazgeçmek için insan eğilimi için hesap gerekir. Bu ilke sistemi isteklerle doldurabileceğinden, çözüm birkaç saniyede bir yeniden denememelidir.

### <a name="interval-increase-between-retries"></a>Yeniden denemeler arasında aralık artışı

İlk denemenizden önce 5 saniye beklemenizi öneririz. 5 saniyeden kısa bir gecikmeden sonra yeniden deneme, bulut hizmetini ezme riskitaşır. Sonraki her yeniden deneme için gecikme katlanarak, en fazla 60 saniyeye kadar artmalıdır.

ADO.NET kullanan istemciler için engelleme süresinin tartışılması için [SQL Server bağlantı havuzuna (ADO.NET)](https://msdn.microsoft.com/library/8xx3tyca.aspx)bakın.

Ayrıca, program kendi kendine sonlandırmadan önce en fazla sayıda yeniden deneme ayarlamak isteyebilirsiniz.

### <a name="code-samples-with-retry-logic"></a>Yeniden deneme mantığına sahip kod örnekleri

Yeniden deneme mantığına sahip kod örnekleri aşağıdaki leri mevcuttur:

- [ADO.NET ile SQL'e esnek bir şekilde bağlanın][step-4-connect-resiliently-to-sql-with-ado-net-a78n]
- [PHP ile SQL'e esnek bir şekilde bağlanın][step-4-connect-resiliently-to-sql-with-php-p42h]

<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

### <a name="test-your-retry-logic"></a>Yeniden deneme mantığını test edin

Yeniden deneme mantığını test etmek için, programınız çalışırken düzeltilebilen bir hataya benzetmeniz veya neden olmanız gerekir.

#### <a name="test-by-disconnecting-from-the-network"></a>Ağbağlantısını keserek test etme

Yeniden deneme mantığını test etmenin bir yolu, program çalışırken istemci bilgisayarınızıağdan ayırmaktır. Hata:

- **SqlException.Number** = 11001
- İleti: "Böyle bir ana bilgisayar bilinmiyor"

İlk yeniden deneme girişiminin bir parçası olarak, istemci bilgisayarınızı ağa yeniden bağlayabilir ve sonra bağlanmayı deneyebilirsiniz.

Bu testi pratik hale getirmek için, programınızı başlatmadan önce bilgisayarınızın fişini ağınızdan çekin. Ardından programınız, programın aşağıdakilere neden olan bir çalışma zamanı parametresini tanır:

- Geçici olarak dikkate alınması gereken hatalar listesine geçici olarak 11001 ekleyin.
- İlk bağlantısını her zamanki gibi dene.
- Hata yakalandıktan sonra, listeden 11001 kaldırın.
- Kullanıcıya bilgisayarı ağa takmasını söyleyen bir ileti görüntüleyin.
- **Console.ReadLine** yöntemini veya Tamam düğmesine sahip bir iletişim kutusunu kullanarak yürütmeyi daha da duraklatın. Kullanıcı, bilgisayar ağa bağlandıktan sonra Enter tuşuna basıyor.
- Yeniden bağlanmak için, başarı bekliyor deneyin.

#### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Bağlanırken veritabanı adını yanlış yazarak test etme

Programınız ilk bağlantı girişiminden önce kullanıcı adını bilerek yanlış yazabilir. Hata:

- **SqlException.Number** = 18456
- İleti: "Giriş kullanıcı 'WRONG_MyUserName' için başarısız oldu."

İlk yeniden deneme girişiminin bir parçası olarak, programınız yazım hatalarını düzeltebilir ve sonra bağlanmayı deneyebilir.

Bu testi pratik hale getirmek için, programınız aşağıdakilere neden olan bir çalışma zamanı parametresini tanır:

- Geçici olarak dikkate alınması gereken hatalar listesine geçici olarak 18456 ekleyin.
- Kullanıcı adına kasıtlı olarak 'WRONG_' ekleyin.
- Hata yakalandıktan sonra, 18456'yı listeden çıkarın.
- Kullanıcı adından 'WRONG_'i kaldırın.
- Yeniden bağlanmak için, başarı bekliyor deneyin.

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

## <a name="net-sqlconnection-parameters-for-connection-retry"></a>.NET SqlBağlantı yeniden denemesi için bağlantı parametreleri

İstemci programınız .NET Framework class **System.Data.SqlClient.SqlConnection**kullanarak SQL Veritabanı'na bağlanırsa, bağlantı yeniden deneme özelliğini kullanabilmeniz için .NET 4.6.1 veya sonraki (veya .NET Core) kullanın. Özellik hakkında daha fazla bilgi için [bu web sayfasına](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlconnection)bakın.

<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->

**SqlConnection** nesneniz için [bağlantı dizesini](https://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) oluştururken, aşağıdaki parametreler arasındaki değerleri koordine edin:

- **ConnectRetryCount**&nbsp;&nbsp;: Varsayılan değer 1'dir. Aralık 0'dan 255'e kadar.
- **ConnectRetryInterval**&nbsp;&nbsp;: Varsayılan değer 10 saniyedir. Menzil 1'den 60'a kadar.
- **Bağlantı Zaman**&nbsp;&nbsp;Süresi : Varsayılan değer 15 saniyedir. Aralık 0 ile 2147483647 arasındadır.

Özellikle, seçtiğiniz değerler aşağıdaki eşitliği doğru yapmalıdır: Bağlantı Zaman Aşımı = ConnectRetryCount * ConnectionRetryInterval

Örneğin, sayım 3'e eşitse ve aralık 10 saniyeye eşitse, yalnızca 29 saniyelik bir zaman aralığı sisteme üçüncü ve son yeniden denemesinin bağlanması için yeterli zaman vermez: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

## <a name="connection-vs-command"></a>Bağlantı ve komut

**ConnectRetryCount** ve **ConnectRetryInterval** parametreleri, **SqlConnection** nesnenizin programınızı söylemeden veya programınıza denetim döndürme gibi rahatsız etmeden connect işlemini yeniden denemesini sağlar. Yeniden denemeler aşağıdaki durumlarda oluşabilir:

- mySqlConnection.Open yöntem çağrısı
- mySqlConnection.Execute yöntemi arama

Bir incelik var. *Sorgunuz* yürütülürken geçici bir hata oluşursa, **SqlConnection** nesneniz connect işlemini yeniden denemez. Kesinlikle sorgunuzu yeniden denemez. Ancak, **SqlConnection** yürütme için sorgugöndermeden önce çok hızlı bir şekilde bağlantıyı denetler. Hızlı denetim bir bağlantı sorunu algılarsa, **SqlConnection** bağlantı işlemini yeniden dener. Yeniden deneme başarılı olursa, sorgunuz yürütme için gönderilir.

### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>ConnectRetryCount uygulama yeniden deneme mantığı ile birleştirilmiş olmalıdır

Uygulamanızın sağlam özel yeniden deneme mantığı olduğunu varsayalım. Bağlantı işlemini dört kez yeniden deneyebilir. Bağlantı dizenize **ConnectRetryInterval** ve **ConnectRetryCount** =3 eklerseniz, yeniden deneme sayısını 4 * 3 = 12 yeniden denemesayısına artırırsınız. Bu kadar çok sayıda yeniden deneme yi düşünmüyor olabilirsiniz.

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-sql-database"></a>SQL Veritabanına Bağlantılar

<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Bağlantı: Bağlantı dizesi

SQL Veritabanı'na bağlanmak için gerekli olan bağlantı dizesi, SQL Server'a bağlanmak için kullanılan dizeden biraz farklıdır. [Azure portalından](https://portal.azure.com/)veritabanınızın bağlantı dizesini kopyalayabilirsiniz.

[!INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]

<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Bağlantı: IP adresi

Sql Database sunucusunu, istemci programınızı barındıran bilgisayarın IP adresinden iletişimi kabul etmek üzere yapılandırmanız gerekir. Bu yapılandırmayı ayarlamak için Güvenlik Duvarı ayarlarını [Azure portalı](https://portal.azure.com/)üzerinden düzenleme.

IP adresini yapılandırmayı unutursanız, programınız gerekli IP adresini belirten kullanışlı bir hata iletisiyle başarısız olur.

[!INCLUDE [sql-database-include-ip-address-22-portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]

Daha fazla bilgi için [bkz.](sql-database-configure-firewall-settings.md)
<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Bağlantı: Bağlantı noktaları

Genellikle, istemci programınızı barındıran bilgisayarda yalnızca bağlantı noktası 1433'ün giden iletişim için açık olduğundan emin olmanız gerekir.

Örneğin, istemci programınız bir Windows bilgisayarında barındırıldığında, ana bilgisayarda Windows Güvenlik Duvarı'nı kullanarak 1433 no'yu açabilirsiniz.

1. Denetim Masası'nı açın.
2. **Tüm Denetim Masası Öğeleri** > **Seçin Windows Güvenlik Duvarı** > **Gelişmiş Ayarlar** > **Giden Kurallar** > **Eylemler** > **Yeni Kural**.

İstemci programınız bir Azure sanal makinesinde (VM) barındırılıyorsa, [4,5 ADO.NET ve SQL Veritabanı için 1433'ün ötesindeki Bağlantı Noktalarını](sql-database-develop-direct-route-ports-adonet-v12.md)okuyun.

Bağlantı noktalarının ve IP adreslerinin yapılandırması hakkında arka plan bilgileri için [Azure SQL Veritabanı güvenlik duvarı'na](sql-database-firewall-configure.md)bakın.

<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-462-or-later"></a>Bağlantı: ADO.NET 4.6.2 veya sonraki

Programınız SQL Veritabanı'na bağlanmak için **System.Data.SqlClient.SqlConnection** gibi ADO.NET sınıf kullanıyorsa, .NET Framework sürüm 4.6.2 veya sonrası sürüm kullanmanızı öneririz.

#### <a name="starting-with-adonet-462"></a>4.6.2 ADO.NET ile başlayan

- Bağlantı açık girişimi Azure SQL veritabanları için hemen yeniden denenecek ve bulut özellikli uygulamaların performansını artırır.

#### <a name="starting-with-adonet-461"></a>ADO.NET 4.6.1 ile başlayan

- SQL Veritabanı **için, SqlConnection.Open** yöntemini kullanarak bir bağlantı açtığınızda güvenilirlik artırılır. **Açık** yöntem artık bağlantı zaman ayarı döneminde ki belirli hatalar için geçici hatalara yanıt olarak en iyi çaba yeniden deneme mekanizmalarını içermektedir.
- Bağlantı havuzu, programınıza verdiği bağlantı nesnesinin çalıştığını niçin etkin bir doğrulama yı içerir, desteklenir.

Bağlantı havuzundan bir bağlantı nesnesi kullandığınızda, hemen kullanılmadığında programınızın bağlantıyı geçici olarak kapatmasını öneririz. Bir bağlantıyı yeniden açmak pahalı değildir, ancak yeni bir bağlantı oluşturmaktır.

4.0 veya daha önce ADO.NET kullanıyorsanız, en son ADO.NET yükseltmenizi öneririz. Ağustos 2018 itibariyle [4.6.2 ADO.NET indirebilirsiniz.](https://blogs.msdn.microsoft.com/dotnet/20../../announcing-the-net-framework-4-7-2/)

<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Tanılama

<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Tanılama: Yardımcı programların bağlanıp bağlanamadığını test edin

Programınız SQL Veritabanı'na bağlanamazsa, tanılama seçeneklerinden biri yardımcı programa bağlanmayı denemektir. İdeal olarak, yardımcı program, programınızın kullandığı kitaplığı kullanarak bağlanır.

Herhangi bir Windows bilgisayarında, şu yardımcı programları deneyebilirsiniz:

- SQL Server Management Studio (ssms.exe), ADO.NET kullanarak bağlanır
- `sqlcmd.exe`, [ODBC](https://msdn.microsoft.com/library/jj730308.aspx) kullanarak bağlanır

Programınız bağlandıktan sonra, kısa bir SQL SELECT sorgusunun çalışıp çalışmadığını test edin.

<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Tanılama: Açık bağlantı noktalarını kontrol edin

Bağlantı noktası sorunları nedeniyle bağlantı denemelerinin başarısız olduğundan şüpheleniyorsanız, bilgisayarınızda bağlantı noktası yapılandırmaları hakkında rapor veren bir yardımcı program çalıştırabilirsiniz.

Linux'ta, aşağıdaki yardımcı programlar yararlı olabilir:

- `netstat -nap`
- `nmap -sS -O 127.0.0.1`: Örnek değeri IP adresiniz olacak şekilde değiştirin.

Windows'da [PortQry.exe](https://www.microsoft.com/download/details.aspx?id=17148) yardımcı programı yararlı olabilir. Burada, SQL Veritabanı sunucusunda bağlantı noktası durumunu sorgulayan ve dizüstü bilgisayarda çalıştırılabilen bir örnek yürütme aşağıda verilmiştir:

```cmd
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called: johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```

<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Tanılama: Hatalarınızı kaydedin

Aralıklı bir sorun bazen en iyi gün veya hafta boyunca genel bir desen tespiti ile teşhis edilir.

Müşteriniz karşılaştığı tüm hataları günlüğe kaydederek tanıda yardımcı olabilir. Günlük girişlerini SQL Veritabanı'nın kendi içinde oturum kurduğu hata verileriyle ilişkilendirebilirsiniz.

Kurumsal Kütüphane 6 (EntLib60) .NET yönetilen sınıflar günlük yardımcı olmak için sunuyor. Daha fazla bilgi için bkz: [5 - Günlükten düşmek kadar kolay: Günlük Uygulama Bloğunu kullanın.](https://msdn.microsoft.com/library/dn440731.aspx)

<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Tanılama: Sistem günlüklerini hatalar için inceleyin

Burada bazı Transact-SQL SELECT ifadeleri sorgu hata günlükleri ve diğer bilgiler vardır.

| Günlük sorgusu | Açıklama |
|:--- |:--- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` |[sys.event_log](https://msdn.microsoft.com/library/dn270018.aspx) görünümü, geçici hatalara veya bağlantı hatalarına neden olabilecek bazı olayları içeren tek tek olaylar hakkında bilgi sunar.<br/><br/>İdeal olarak, istemci programınızın ne zaman sorun yaşadığına ilişkin bilgilerle **start_time** veya **end_time** değerleri ilişkilendirebilirsiniz.<br/><br/>Bu sorguyu çalıştırmak için *ana* veritabanına bağlanmanız gerekir. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` |[sys.database_connection_stats](https://msdn.microsoft.com/library/dn269986.aspx) görünümü, ek tanılama için toplu olay türleri saymalar sunar.<br/><br/>Bu sorguyu çalıştırmak için *ana* veritabanına bağlanmanız gerekir. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Tanılama: SQL Veritabanı günlüğündeki sorunlu olayları arama

SQL Veritabanı günlüğündeki sorunlu olaylarla ilgili girişleri arayabilirsiniz. *Ana* veritabanında aşağıdaki Transact-SQL SELECT deyimini deneyin:

```sql
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```

#### <a name="a-few-returned-rows-from-sysfn_xe_telemetry_blob_target_read_file"></a>sys.fn_xe_telemetry_blob_target_read_file birkaç satır döndü

Aşağıdaki örnek, döndürülen bir satırın nasıl görünebileceğini gösterir. Gösterilen null değerleri genellikle diğer satırlarda null değildir.

```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```

<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Kurumsal Kütüphane 6

Kurumsal Kitaplık 6 (EntLib60), biri SQL Veritabanı hizmeti olan bulut hizmetlerinin güçlü istemcilerini uygulamanıza yardımcı olan .NET sınıflarının bir çerçevesidir. EntLib60'ın yardımcı olabileceği her alana adanmış konuları bulmak için, [Enterprise Library 6 - Nisan 2013'e](https://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)bakın.

Geçici hataları işlemek için yeniden deneme mantığı, EntLib60'ın yardımcı olabileceği bir alandır. Daha fazla bilgi için [bkz: 4 - Azim, tüm zaferlerin sırrı: Geçici Arıza İşleme Uygulama Bloğu kullanın.](https://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)

> [!NOTE]
> EntLib60 için kaynak kodu [Download Center'dan](https://go.microsoft.com/fwlink/p/?LinkID=290898)genel olarak indirilebilir. Microsoft'un EntLib için daha fazla özellik güncelleştirmesi veya bakım güncelleştirmesi yapma gibi bir planı yoktur.

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>Geçici hatalar ve yeniden deneme için EntLib60 sınıfları

Aşağıdaki EntLib60 sınıfları özellikle yeniden deneme mantığı için yararlıdır. Tüm bu sınıflar veya namespace **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**altında bulunur.

Ad alanında **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:

- **RetryPolicy** sınıfı
  - **ExecuteAction** yöntemi
- **ÜstelBackoff** sınıfı
- **SqlDatabaseTransientErrorDetectionStrategy** sınıfı
- **ReliableSqlConnection** sınıfı
  - **ExecuteCommand** yöntemi

Ad alanında **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

- **AlwaysTransientErrorDetectionStrategy** sınıfı
- **NeverTransientErrorDetectionStrategy** sınıfı

Burada EntLib60 hakkında bilgi için bazı bağlantılar şunlardır:

- Ücretsiz kitap indir: [Microsoft Enterprise Library, 2 baskı için Geliştirici Kılavuzu](https://www.microsoft.com/download/details.aspx?id=41145).
- En iyi uygulamalar: [Retry genel rehberlik](../best-practices-retry-general.md) yeniden deneme mantığı mükemmel bir derinlemesine tartışma vardır.
- NuGet indir: [Kurumsal Kütüphane - Geçici Arıza İşleme Uygulama Bloğu 6.0](https://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/).

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: günlük bloğu

- Günlük bloğu, aşağıdakileri için kullanabileceğiniz son derece esnek ve yapılandırılabilir bir çözümdür:
  - Günlük iletilerini çok çeşitli konumlarda oluşturun ve depolayın.
  - İletileri kategorilere ayırın ve filtreleyin.
  - Hata ayıklama ve izlemenin yanı sıra denetim ve genel günlük leme gereksinimleri için yararlı olan bağlamsal bilgileri toplayın.
- Günlük bloğu, günlük hedefinden günlük işlevini soyutlar, böylece uygulama kodu hedef günlük deposunun konumu ve türüne bakılmaksızın tutarlı olur.

Daha fazla bilgi için bkz: [5 - Günlükten düşmek kadar kolay: Günlük Uygulama Bloğunu kullanın.](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>EntLib60 IsTransient yöntem kaynak kodu

Sonraki, **SqlDatabaseTransientErrorDetectionStrategy** sınıfından, **IsTransient** yöntemi için C # kaynak kodudur. Kaynak kodu, Nisan 2013 itibariyle hangi hataların geçici ve yeniden denemeye değer olarak kabul edildiğini açıklar.

```csharp
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in the exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // The service is currently busy. Retry the request after 10 seconds.
            // Code: (reason code to be decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode the reason code from the error message to
            // determine the grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach the decoded values as additional attributes to
            // the original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // The instance of SQL Server you attempted to connect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [SQL Database ve SQL Server için bağlantı kitaplıkları](sql-database-libraries.md)
- [SQL Server bağlantı havuzu (ADO.NET)](https://docs.microsoft.com/dotnet/framework/data/adonet/sql-server-connection-pooling)
- [ *Yeniden deneme,* Python'da yazılmış bir Apache 2.0 lisanslı genel amaçlı yeniden deneme kitaplığıdır](https://pypi.python.org/pypi/retrying) ve hemen hemen her şeye yeniden deneme davranışı ekleme görevini basitleştirmek içindir.

<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-sql-ado-net

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php
