---
title: Çok kiracılı uygulama örneği - Wingtip SaaS
description: Wingtip Tickets SaaS örneği olan Azure SQL Veritabanı'nı kullanan örnek çok kiracılı uygulamayı yüklemek ve çalıştırmak için adımlar ve kılavuz sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: sstein
ms.date: 12/18/2018
ms.openlocfilehash: 162d1f269c65ad98afa30e8e96370bbdceca99bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74132302"
---
# <a name="general-guidance-for-working-with-wingtip-tickets-sample-saas-apps"></a>Wingtip Tickets örnek SaaS uygulamaları ile çalışmak için genel rehberlik

Bu makalede, Azure SQL Veritabanı kullanan Wingtip Biletleri örnek SaaS uygulamalarını çalıştırmak için genel kılavuz yer almaktadır.

## <a name="download-and-unblock-the-wingtip-tickets-saas-scripts"></a>Wingtip Tickets SaaS komut dosyalarını indirin ve engelini kaldırın

Yürütülebilir içerikler (komut dosyaları, dlls) zip dosyaları harici bir kaynaktan indirilip ayıklandığında Windows tarafından engellenebilir. Bir zip dosyasından komut dosyalarını ayıklarken, **ayıklamadan önce .zip dosyasının engelini kaldırmak için aşağıdaki adımları izleyin.** Bu, komut dosyalarının çalışmasına izin verilir.

1. Keşfetmek istediğiniz veritabanı kira deseni için Wingtip Tickets SaaS GitHub repo'suna göz atın:
    - [WingtipTicketsSaaS-StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp)
    - [WingtipTicketsSaaS-DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant)
    - [WingtipTicketsSaaS-MultiTenantDb](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb)
2. **Clone'u tıklatın veya indirin.**
3. **Zip İndir'i** ve dosyayı kaydedin'i tıklatın.
4. Zip dosyasına sağ tıklayın ve **Özellikler'i**seçin. Zip dosya adı repo adına karşılık gelecektir. (ör. _WingtipTicketsSaaS-DbPerTenant-master.zip_)
5. **Genel** sekmesinde, **Engeli Kaldır'ı**seçin.
6. **Tamam**'a tıklayın.
7. Dosyaları ayıklayın.

Komut dosyaları bulunmaktadır *... Öğrenme \\Modülleri* klasörü.


## <a name="working-with-the-wingtip-tickets-powershell-scripts"></a>Wingtip Tickets PowerShell komut dosyalarıyla çalışma

Örnekten en iyi şekilde elde etmek için sağlanan komut dosyasına dalmanız gerekir. Kesme noktalarını kullanın ve farklı SaaS desenlerinin nasıl uygulandığını inceleyin ve komut dosyaları arasında adım atın. En iyi anlama için sağlanan komut dosyaları ve modülleri kolayca geçmek için [PowerShell ISE'yi](https://docs.microsoft.com/powershell/scripting/components/ise/introducing-the-windows-powershell-ise)kullanmanızı öneririz.

### <a name="update-the-configuration-file-for-your-deployment"></a>Dağıtımınız için yapılandırma dosyasını güncelleştirme

**UserConfig.psm1** dosyasını dağıtım sırasında ayarladığınız kaynak grubu ve kullanıcı değeriyle birlikte edin:

1. *PowerShell ISE'yi* açın ve yükleyin ... \\Öğrenme Modülleri\\*UserConfig.psm1*
2. *KaynakGroupName* ve *Adını* dağıtımınız için belirli değerlerle güncelleştirin (yalnızca 10 ve 11. satırlarda).
3. Değişiklikleri kaydedin!

Bu değerleri burada ayarlamak, her komut dosyasında dağıtıma özgü bu değerleri güncelleştirmenizi sağlar.

### <a name="execute-the-scripts-by-pressing-f5"></a>F5 tuşuna basarak komut dosyalarını çalıştırın

Birkaç komut dosyası klasörlerde gezinmek için *$PSScriptRoot* kullanır ve *$PSScriptRoot* yalnızca komut dosyaları **F5**tuşuna basılarak yürütüldüğünde değerlendirilir.Bir seçimi **(F8)** vurgulama ve çalıştırma hatalarına neden olabilir, bu nedenle komut dosyalarını çalıştırırken **F5** tuşuna basın.

### <a name="step-through-the-scripts-to-examine-the-implementation"></a>Uygulamayı incelemek üzere betiklerde ilerleme

Senaryoları anlamanın en iyi yolu, ne yaptıklarını görmek için onların içinden geçmektir. Üst düzey iş akışını takip etmek kolay sunan dahil **Demo-komut** dosyaları göz atın. **Demo komut** dosyaları her görevi gerçekleştirmek için gereken adımları gösterir, bu nedenle kesme noktaları ayarlayın ve farklı SaaS desenleri için uygulama ayrıntılarını görmek için tek tek çağrıları daha derin delin.

PowerShell komut dosyalarını keşfetmek ve geçmek için ipuçları:

- PowerShell ISE'de **Demo komut** dosyalarını açın.
- **F5'i** yürütmek veya kullanmaya devam etmek (komut dosyası seçimlerini çalıştırırken *$PSScriptRoot* değerlendirilmediği için **F8** kullanılması tavsiye edilmez).
- Bir çizgiye tıklayarak veya çizgiyi seçerek ve **F9**’a basarak kesme noktaları yerleştirin.
- **F10**’u kullanarak bir işlev veya betiği atlayın.
- **F11**’i kullanarak bir işlev veya betiğe gidin.
- **Shift + F11**’i kullanarak geçerli işlev veya betikten çıkın.


## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Veritabanı şemasını keşfetme ve SSMS kullanarak SQL sorguları yürütme

Uygulama sunucularını ve veritabanlarını bağlamak ve göz atmak için [SQL Server Management Studio'yu (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) kullanın.

Dağıtım başlangıçta bağlamak için kiracı ve katalog SQL Veritabanı sunucuları vardır. Sunucuların adlandırılması veritabanı kira desenine bağlıdır (ayrıntılar için aşağıya bakın).

   - **Bağımsız uygulama:** her kiracı için sunucular (ör. *contosoconcerthall-&lt;&gt; Kullanıcı* sunucusu) ve *katalog-sa-&lt;Kullanıcı&gt; *
   - **Kiracı başına veritabanı:** *&lt;kiracı1-dpt- Kullanıcı&gt; * ve *katalog-dpt-&lt;Kullanıcı&gt; * sunucuları
   - **Çok kiracılı veritabanı:** *kiracı1-mt-&lt;Kullanıcı&gt; * ve *katalog-mt-&lt;Kullanıcı&gt; * sunucuları

Başarılı bir demo bağlantısı sağlamak için, tüm sunucuların tüm IP'lerin geçmesine izin veren bir [güvenlik duvarı kuralı](sql-database-firewall-configure.md) vardır.


1. *SSMS'i* açın ve kiracılara bağlanın. Sunucu adı seçtiğiniz veritabanı kira desenine bağlıdır (ayrıntılar için aşağıya bakın):
    - **Bağımsız uygulama:** bireysel kiracı sunucuları (ör. *contosoconcerthall-&lt;&gt;Kullanıcı .database.windows.net*)
    - **Kiracı başına veritabanı:** *&lt;kiracı1-dpt- Kullanıcı&gt;.database.windows.net*
    - **Çok kiracılı veritabanı:** *kiracı1-mt-&lt;Kullanıcı&gt;.database.windows.net*
2. Veritabanı Altyapıyı **Bağla'yı** > **tıklatın... :**

   ![katalog sunucusu seçeneğine tıklayın](media/saas-tenancy-wingtip-app-guidance-tips/connect.png)

3. Demo kimlik bilgileri şunlardır: Giriş = *geliştirici*, Şifre = *P\@ssword1*

    Aşağıdaki *resimde, kiracı* deseni başına Veritabanı için giriş gösterin.
    ![bağlantı](media/saas-tenancy-wingtip-app-guidance-tips/tenants1-connect.png)



4. Adımları 2-3'ü tekrarlayın ve katalog sunucusuna bağlanın (seçilen veritabanı kira desenine göre belirli sunucu adları için aşağıya bakın)
    - **Bağımsız uygulama:** *&lt;katalog-sa-&gt;Kullanıcı .database.windows.net*
    - **Kiracı başına veritabanı:** *&lt;katalog-dpt- Kullanıcı&gt;.database.windows.net*
    - **Çok kiracılı veritabanı:** *katalog-mt-&lt;Kullanıcı&gt;.database.windows.net*


Başarılı bir şekilde bağlandıktan sonra tüm sunucuları görmeniz gerekir. Veritabanları listeniz, size sağlamış olduğunuz kiracılara bağlı olarak farklı olabilir.

Aşağıdaki *resimde, kiracı* deseni başına Veritabanı için oturum açma gösteriş.

![nesne gezgini](media/saas-tenancy-wingtip-app-guidance-tips/object-explorer.png)



## <a name="next-steps"></a>Sonraki adımlar
- [Wingtip Biletleri SaaS Tek Başına Uygulama Dağıt](saas-standaloneapp-get-started-deploy.md)
- [Kiracı uygulaması başına Wingtip Biletleri SaaS Veritabanını dağıtma](saas-dbpertenant-get-started-deploy.md)
- [Wingtip Biletleri SaaS Çok Kiracıveritabanı uygulamasını dağıtın](saas-multitenantdb-get-started-deploy.md)

