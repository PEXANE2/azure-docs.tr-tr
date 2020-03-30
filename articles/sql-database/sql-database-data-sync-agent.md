---
title: SQL Veri Eşitlemi için Veri Eşitleme Aracısı
description: Verileri şirket içi SQL Server veritabanlarıyla senkronize etmek için Azure SQL Veri Eşitlemi veri eşitle'sini nasıl yükleyip çalıştırabilirsiniz öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: carlrab
ms.date: 12/20/2018
ms.openlocfilehash: 6d0a728401ac9f0156cc8fa913ce486bb577c6dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73825165"
---
# <a name="data-sync-agent-for-azure-sql-data-sync"></a>Azure SQL Veri Eşitlemi için Veri Eşitleme Aracısı

Azure SQL Veri Eşitlemi için Veri Eşitleme Aracısını yükleyip yapılandırarak verileri şirket içi SQL Server veritabanlarıyla eşitleyin. SQL Veri Eşitlemi hakkında daha fazla bilgi için, [SQL Veri Eşitlemi ile birden çok bulut ve şirket içi veritabanları nda eşitleme verilerini](sql-database-sync-data.md)görün.

> [!IMPORTANT]
> Azure SQL Veri Eşitlemesi şu anda Azure SQL Veritabanı Yönetilen Örneği'ni **desteklemez.**

## <a name="download-and-install"></a>İndirme ve yükleme

Veri Eşitleme Aracısını indirmek için [SQL Azure Veri Eşitleme Aracısı'na](https://www.microsoft.com/download/details.aspx?id=27693)gidin.

### <a name="install-silently"></a>Sessizce yükleyin

Veri Eşitleme Aracısını komut isteminden sessizce yüklemek için aşağıdaki örneğe benzer bir komut girin. İndirilen .msi dosyasının dosya adını kontrol edin ve **TARGETDIR** ve **SERVICEACCOUNT** bağımsız değişkenleri için kendi değerlerinizi sağlayın.

- **TARGETDIR**için bir değer sağlamazsanız, varsayılan `C:\Program Files (x86)\Microsoft SQL Data Sync 2.0`değer .

- **SERVICEACCOUNT** `LocalSystem` değeri olarak sağlıyorsanız, aracıyı şirket içi SQL Server'a bağlanmak üzere yapılandırırken SQL Server kimlik doğrulamasını kullanın.

- **SERVICEACCOUNT**değeri olarak bir etki alanı kullanıcı hesabı veya yerel bir kullanıcı hesabı sağlarsanız, **parolayı SERVICEPASSWORD** bağımsız değişkenini de sağlamanız gerekir. Örneğin, `SERVICEACCOUNT="<domain>\<user>"  SERVICEPASSWORD="<password>"`.

```cmd
msiexec /i "SQLDataSyncAgent-2.0-x86-ENU.msi" TARGETDIR="C:\Program Files (x86)\Microsoft SQL Data Sync 2.0" SERVICEACCOUNT="LocalSystem" /qn
```

## <a name="sync-data-with-sql-server-on-premises"></a>Verileri SQL Server ile şirket içinde senkronize edin

Verileri bir veya daha fazla şirket içi SQL Server veritabanıyla eşitleyebilmeniz için Veri Eşitleme Aracısını yapılandırmak için, şirket [içi SQL Server veritabanı ekle'ye](sql-database-get-started-sql-data-sync.md#add-on-prem)bakın.

## <a name="data-sync-agent-faq"></a><a name="agent-faq"></a>Veri Eşitleme Aracısı SSS

### <a name="why-do-i-need-a-client-agent"></a>Neden bir müşteri temsilcisine ihtiyacım var?

SQL Data Sync hizmeti istemci aracısı aracılığıyla SQL Server veritabanları ile iletişim kurar. Bu güvenlik özelliği, güvenlik duvarının arkasındaki veritabanlarıyla doğrudan iletişimi engeller. SQL Veri Eşitleme hizmeti aracıile iletişim kurduğunda, bunu şifreli bağlantılar ve benzersiz bir belirteç veya *aracı anahtarı*kullanarak yapar. SQL Server veritabanları bağlantı dizesi ve aracı anahtarını kullanarak aracının kimliğini doğrular. Bu tasarım, verileriniz için yüksek düzeyde güvenlik sağlar.

### <a name="how-many-instances-of-the-local-agent-ui-can-be-run"></a>Yerel aracı UI'nin kaç örneği çalıştırılabilir?

UI'nin yalnızca bir örneği çalıştırılabilir.

### <a name="how-can-i-change-my-service-account"></a>Hizmet hesabımı nasıl değiştirebilirim?

Bir istemci aracısı yükledikten sonra, hizmet hesabını değiştirmenin tek yolu hesabı kaldırmak ve yeni hizmet hesabıyla yeni bir istemci aracısı yüklemektir.

### <a name="how-do-i-change-my-agent-key"></a>Temsilci anahtarımı nasıl değiştiririm?

Aracı anahtarı bir aracı tarafından yalnızca bir kez kullanılabilir. Yeni bir aracıyı kaldırdığınızda yeniden kullanılamaz ve birden çok aracı tarafından kullanılamaz. Varolan bir aracı için yeni bir anahtar oluşturmanız gerekiyorsa, aynı anahtarın istemci aracısı ve SQL Veri Eşitleme hizmetiyle kaydedildiğinden emin olmalısınız.

### <a name="how-do-i-retire-a-client-agent"></a>Bir müşteri temsilcisinasıl emekli olurum?

Bir aracıyı hemen geçersiz ksaymak veya emekli etmek için, anahtarını portalda yeniden oluşturun, ancak Aracı UI'ye göndermeyin. Bir anahtarı yeniden oluşturmak, ilgili aracı çevrimiçi veya çevrimdışı ysa, önceki anahtarı geçersiz kılır.

### <a name="how-do-i-move-a-client-agent-to-another-computer"></a>İstemci aracıyı başka bir bilgisayara nasıl taşıyabilirim?

Yerel aracıyı şu anda olduğundan farklı bir bilgisayardan çalıştırmak istiyorsanız, aşağıdaki leri yapın:

1. Aracıyı istenilen bilgisayara yükleyin.
2. SQL Veri Eşitleme portalında oturum açın ve yeni aracı için bir aracı anahtarıyeniden oluşturun.
3. Yeni aracı anahtarını göndermek için yeni temsilcinin ue'sini kullanın.
4. İstemci aracısı daha önce kaydedilmiş şirket içi veritabanlarının listesini karşıdan yüklerken bekleyin.
5. Erişimümkün olarak görüntüleyen tüm veritabanları için veritabanı kimlik bilgilerini sağlayın. Bu veritabanlarına aracının yüklendiği yeni bilgisayardan erişilebilmelidir.

## <a name="troubleshoot-data-sync-agent-issues"></a><a name="agent-tshoot"></a>Veri Eşitleme Aracısı sorunları

- [İstemci aracısını yükleme, kaldırma veya onarım başarısız oldu](#agent-install)

- [Kaldırmayı iptal ettikten sonra istemci temsilcisi çalışmıyor](#agent-uninstall)

- [Veritabanım aracı listesinde listelenmez](#agent-list)

- [İstemci aracısı başlatılmıyor (Hata 1069)](#agent-start)

- [Ajan anahtarını gönderemiyorum.](#agent-key)

- [İlişkili şirket içi veritabanına erişilenemiyorsa istemci aracısı portaldan silinemez](#agent-delete)

- [Yerel Eşitleme Aracısı uygulaması yerel eşitleme hizmetine bağlanamıyor](#agent-connect)

### <a name="the-client-agent-install-uninstall-or-repair-fails"></a><a name="agent-install"></a>İstemci aracısını yükleme, kaldırma veya onarım başarısız oldu

- **Çünkü.** Birçok senaryo bu hataya neden olabilir. Bu hatanın belirli nedenini belirlemek için günlüklere bakın.

- **Çözünürlük**. Hatanın belirli nedenini bulmak için, Windows Yükleyici günlüklerini oluşturun ve inceleyin. Komut isteminde günlüğe kaydetmeyi açabilirsiniz. Örneğin, indirilen yükleme dosyası `SQLDataSyncAgent-2.0-x86-ENU.msi`aşağıdaki komut satırlarını kullanarak günlük dosyalarını oluşturup inceleyebilirse:

  - Yüklemeler için:`msiexec.exe /i SQLDataSyncAgent-2.0-x86-ENU.msi /l*v LocalAgentSetup.Log`
  - Kaldırmalar için:`msiexec.exe /x SQLDataSyncAgent-2.0-x86-ENU.msi /l*v LocalAgentSetup.Log`

    Windows Installer tarafından gerçekleştirilen tüm yüklemeler için günlüğe kaydetmeyi de açabilirsiniz. Microsoft Bilgi Bankası makalesi [Windows Installer günlüğe kaydetmeyi etkinleştirmek](https://support.microsoft.com/help/223300/how-to-enable-windows-installer-logging) için Windows Installer için günlüğe kaydetmeyi açmak için tek tıklatma çözümü sağlar. Ayrıca günlüklerin konumunu sağlar.

### <a name="the-client-agent-doesnt-work-after-i-cancel-the-uninstall"></a><a name="agent-uninstall"></a>Kaldırmayı iptal ettikten sonra istemci temsilcisi çalışmıyor

İstemci aracısı, yüklemeyi iptal ettikten sonra bile çalışmıyor.

- **Çünkü.** Bunun nedeni, SQL Veri Eşitleme istemci aracısının kimlik bilgilerini depolamaması dır.

- **Çözünürlük**. Bu iki çözümü deneyebilirsiniz:

    -   İstemci aracısının kimlik bilgilerini yeniden girmek için services.msc'yi kullanın.
    -   Bu istemci aracıyı kaldırın ve sonra yeni bir tane yükleyin. [Download Center'dan](https://www.microsoft.com/download/details.aspx?id=27693)en son istemci aracısını indirin ve kurun.

### <a name="my-database-isnt-listed-in-the-agent-list"></a><a name="agent-list"></a>Veritabanım aracı listesinde listelenmez

Varolan bir SQL Server veritabanını eşitleme grubuna eklemeye çalıştığınızda, veritabanı aracılar listesinde görünmez.

Bu senaryolar bu soruna neden olabilir:

- **Çünkü.** İstemci aracısı ve eşitleme grubu farklı veri merkezlerindedir.

- **Çözünürlük**. İstemci aracısı ve eşitleme grubu aynı veri merkezinde olmalıdır. Bunu ayarlamak için iki seçeneğiniz var:

    -   Eşitleme grubunun bulunduğu veri merkezinde yeni bir aracı oluşturun. Sonra, veritabanını bu aracıya kaydedin.
    -   Geçerli eşitleme grubunu silin. Ardından, aracının bulunduğu veri merkezinde eşitleme grubunu yeniden oluşturun.

- **Çünkü.** İstemci aracısının veritabanları listesi geçerli değil.

- **Çözünürlük**. İstemci aracı hizmetini durdurun ve yeniden başlatın.

    Yerel aracı, yalnızca aracı anahtarının ilk tesliminde ilişkili veritabanlarının listesini karşıdan yükler. Sonraki aracı anahtar gönderimlerinde ilişkili veritabanlarının listesini indirmez. Aracı taşıma sırasında kayıtlı veritabanları özgün aracı örneğinde görünmüyor.

### <a name="client-agent-doesnt-start-error-1069"></a><a name="agent-start"></a>İstemci aracısı başlatılmıyor (Hata 1069)

Aracının SQL Server'ı barındıran bir bilgisayarda çalışmadığını fark edersiniz. Aracıyı el ile başlatmaya çalıştığınızda, "Hata 1069: Oturum açma hatası nedeniyle hizmet başlatılmadı" iletisini görüntüleyen bir iletişim kutusu görürsünüz.

![Veri Eşitleme hatası 1069 iletişim kutusu](media/sql-database-troubleshoot-data-sync/sync-error-1069.png)

- **Çünkü.** Bu hatanın olası bir nedeni, aracı ve aracı parolasını oluşturduğunuzdan beri yerel sunucudaki parolanın değişmiş olmasıdır.

- **Çözünürlük**. Aracının parolasını geçerli sunucu parolanızla güncelleştirin:

  1. SQL Data Sync istemci aracısı hizmetini bulun.  
    a. **Başlat**'ı seçin.  
    b. Arama kutusuna **services.msc**girin.  
    c. Arama sonuçlarında **Hizmetler'i**seçin.  
    d. **Hizmetler** penceresinde, **SQL Veri Eşitleme Aracısı**için girişe gidin.  
  1. **SQL Veri Eşitleme**Aracısı'nı sağ tıklatın ve ardından **Durdur'u**seçin.
  1. **SQL Veri Eşitleme**Aracısı'nı sağ tıklatın ve ardından **Özellikler'i**seçin.
  1. **SQL Data Sync Agent Properties'de**Oturum **Açma** sekmesini seçin.
  1. **Parola** kutusuna parolanızı girin.
  1. **Parolayı Onayla** kutusuna, parolanızı yeniden girin.
  1. **Uygula**’yı ve sonra **Tamam**’ı seçin.
  1. **Hizmetler** penceresinde, SQL Veri **Eşitleme Aracısı** hizmetini sağ tıklatın ve sonra **Başlat'ı**tıklatın.
  1. **Hizmetler** penceresini kapatın.

### <a name="i-cant-submit-the-agent-key"></a><a name="agent-key"></a>Ajan anahtarını gönderemiyorum.

Bir aracı için bir anahtar oluşturduktan veya yeniden oluşturduktan sonra, anahtarı SqlAzureDataSyncAgent uygulaması aracılığıyla göndermeye çalışırsınız. Gönderim tamamlanmaz.

![Hata eşitle iletişim kutusu - Aracı anahtarı gönderemiyorum](media/sql-database-troubleshoot-data-sync/sync-error-cant-submit-agent-key.png)

- **Ön koşullar**. Devam etmeden önce aşağıdaki ön koşulları kontrol edin:

  - SQL Veri Eşitleme Windows hizmeti çalışıyor.

  - SQL Data Sync Windows hizmetinin hizmet hesabı ağ erişimine sahiptir.

  - Giden 1433 bağlantı noktası yerel güvenlik duvarı kuralınızda açıktır.

  - Yerel ip eşitleme meta veri veritabanı için sunucu veya veritabanı güvenlik duvarı kuralı eklenir.

- **Çünkü.** Aracı anahtarı, her yerel aracı benzersiz olarak tanımlar. Anahtar iki koşulu karşılamalıdır:

  -   SQL Data Sync sunucusundaki istemci aracı sı ve yerel bilgisayar aynı olmalıdır.
  -   İstemci aracı anahtarı yalnızca bir kez kullanılabilir.

- **Çözünürlük**. Temsilciniz çalışmıyorsa, bunun nedeni bu koşullardan birinin veya her ikisinin karşılanmamasıdır. Temsilcinizin yeniden çalışmasını sağlamak için:

  1. Yeni bir anahtar oluşturun.
  1. Aracıya yeni anahtarı uygulayın.

  Aracıya yeni anahtarı uygulamak için:

  1. Dosya Gezgini'nde aracı yükleme dizininize gidin. Varsayılan yükleme dizini C:\\Program Dosyaları (x86)\\Microsoft SQL Veri Eşitlemesi'dir.
  1. Çift tıklatma kutusu alt dizini.
  1. SqlAzureDataSyncAgent uygulamasını açın.
  1. **Aracı Anahtarı Gönder'i**seçin.
  1. Sağlanan alana, panonuzdaki anahtarı yapıştırın.
  1. **Tamam'ı**seçin.
  1. Programı kapatın.

### <a name="the-client-agent-cant-be-deleted-from-the-portal-if-its-associated-on-premises-database-is-unreachable"></a><a name="agent-delete"></a>İlişkili şirket içi veritabanına erişilenemiyorsa istemci aracısı portaldan silinemez

BIR SQL Veri Eşitleme istemci aracısı ile kayıtlı yerel bir bitiş noktası (yani bir veritabanı) erişilemez hale gelirse, istemci aracısı silinemez.

- **Çünkü.** Erişilemeyen veritabanı hala aracıkayıtlı olduğundan yerel aracı silinemez. Aracıyı silmeye çalıştığınızda, silme işlemi veritabanına ulaşmaya çalışır ve bu da başarısız olur.

- **Çözünürlük**. Erişilemeyen veritabanını silmek için "zorla silme" kullanın.

> [!NOTE]
> Meta veri tablolarını eşitleme bir "zorlama `deprovisioningutil.exe` silme" sonra kalırsa, bunları temizlemek için kullanın.

### <a name="local-sync-agent-app-cant-connect-to-the-local-sync-service"></a><a name="agent-connect"></a>Yerel Eşitleme Aracısı uygulaması yerel eşitleme hizmetine bağlanamıyor

- **Çözünürlük**. Aşağıdaki adımları deneyin:

  1. Uygulamadan çıkın.  
  1. Bileşen Hizmetleri Panelini açın.  
    a. Görev çubuğundaki arama kutusuna **services.msc**girin.  
    b. Arama sonuçlarında, **Hizmetler'i**çift tıklatın.  
  1. SQL **Veri Eşitleme** hizmetini durdurun.
  1. **SQL Veri Eşitleme** hizmetini yeniden başlatın.  
  1. Uygulamayı yeniden açmak.

## <a name="run-the-data-sync-agent-from-the-command-prompt"></a>Veri Eşitleme Aracısını komut isteminden çalıştırma

Aşağıdaki Veri Eşitleme Aracısı komutlarını komut isteminden çalıştırabilirsiniz:

### <a name="ping-the-service"></a>Ping hizmeti

#### <a name="usage"></a>Kullanım

```cmd
SqlDataSyncAgentCommand.exe -action pingsyncservice
```

#### <a name="example"></a>Örnek

```cmd
SqlDataSyncAgentCommand.exe -action "pingsyncservice"
```

### <a name="display-registered-databases"></a>Kayıtlı veritabanlarını görüntüleme

#### <a name="usage"></a>Kullanım

```cmd
SqlDataSyncAgentCommand.exe -action displayregistereddatabases
```

#### <a name="example"></a>Örnek

```cmd
SqlDataSyncAgentCommand.exe -action "displayregistereddatabases"
```

### <a name="submit-the-agent-key"></a>Aracı anahtarını gönder

#### <a name="usage"></a>Kullanım

```cmd
Usage: SqlDataSyncAgentCommand.exe -action submitagentkey -agentkey [agent key]  -username [user name] -password [password]
```

#### <a name="example"></a>Örnek

```cmd
SqlDataSyncAgentCommand.exe -action submitagentkey -agentkey [agent key generated from portal, PowerShell, or API] -username [user name to sync metadata database] -password [user name to sync metadata database]
```

### <a name="register-a-database"></a>Veritabanını kaydetme

#### <a name="usage"></a>Kullanım

```cmd
SqlDataSyncAgentCommand.exe -action registerdatabase -servername [on-premisesdatabase server name] -databasename [on-premisesdatabase name]  -username [domain\\username] -password [password] -authentication [sql or windows] -encryption [true or false]
```

#### <a name="examples"></a>Örnekler

```cmd
SqlDataSyncAgentCommand.exe -action "registerdatabase" -serverName localhost -databaseName testdb -authentication sql -username <user name> -password <password> -encryption true

SqlDataSyncAgentCommand.exe -action "registerdatabase" -serverName localhost -databaseName testdb -authentication windows -encryption true

```

### <a name="unregister-a-database"></a>Veritabanını kayıt dışı

Bu komutu bir veritabanının kaydını çıkarmak için kullandığınızda, veritabanını tamamen devre dışı kullanabilirsiniz. Veritabanı diğer eşitleme gruplarına katılırsa, bu işlem diğer eşitleme gruplarını kırar.

#### <a name="usage"></a>Kullanım

```cmd
SqlDataSyncAgentCommand.exe -action unregisterdatabase -servername [on-premisesdatabase server name] -databasename [on-premisesdatabase name]
```

#### <a name="example"></a>Örnek

```cmd
SqlDataSyncAgentCommand.exe -action "unregisterdatabase" -serverName localhost -databaseName testdb
```

### <a name="update-credentials"></a>Kimlik bilgilerini güncelleştirme

#### <a name="usage"></a>Kullanım

```cmd
SqlDataSyncAgentCommand.exe -action updatecredential -servername [on-premisesdatabase server name] -databasename [on-premisesdatabase name]  -username [domain\\username] -password [password] -authentication [sql or windows] -encryption [true or false]
```

#### <a name="examples"></a>Örnekler

```cmd
SqlDataSyncAgentCommand.exe -action "updatecredential" -serverName localhost -databaseName testdb -authentication sql -username <user name> -password <password> -encryption true

SqlDataSyncAgentCommand.exe -action "updatecredential" -serverName localhost -databaseName testdb -authentication windows -encryption true
```

## <a name="next-steps"></a>Sonraki adımlar

SQL Veri Eşitlemi hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

-   Genel Bakış - [Azure SQL Veri Eşitlemi ile verileri birden çok bulut ve şirket içi veritabanları nda eşitleme](sql-database-sync-data.md)
-   Veri Eşitlemeyi Ayarlama
    - Portalda - [Öğretici: Azure SQL Veritabanı ve SQL Server arasında verileri şirket içinde eşitlemek için SQL Veri Eşitlemeyi'ni ayarlayın](sql-database-get-started-sql-data-sync.md)
    - PowerShell ile
        -  [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)
        -  [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)
-   En iyi uygulamalar - [Azure SQL Veri Eşitleme için en iyi uygulamalar](sql-database-best-practices-data-sync.md)
-   Monitör - [Azure Monitor günlükleriyle SQL Veri Eşitlemesini Izleyin](sql-database-sync-monitor-oms.md)
-   Sorun Giderme - [Azure SQL Veri Eşitleme ile sorun giderme](sql-database-troubleshoot-data-sync.md)
-   Eşitleme şeasını güncelleştirme
    -   Transact-SQL ile - [Azure SQL Veri Eşitleme'deki şema değişikliklerinin çoğaltılması otomatikleştirin](sql-database-update-sync-schema.md)
    -   PowerShell ile - [Varolan bir eşitleme grubunda eşitleme şemasını güncellemek için PowerShell'i kullanın](scripts/sql-database-sync-update-schema.md)
