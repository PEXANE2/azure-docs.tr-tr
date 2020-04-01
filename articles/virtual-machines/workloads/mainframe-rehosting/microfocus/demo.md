---
title: Azure Sanal Makinelerde Micro Focus Enterprise Developer 4.0 için Micro Focus CICS BankDemo'u ayarlama
description: Micro Focus Enterprise Server ve Enterprise Developer'ı kullanmayı öğrenmek için Azure Sanal Makineler'de (VM) Micro Focus BankDemo uygulamasını çalıştırın.
author: sread
ms.author: sread
ms.date: 03/30/2020
ms.topic: article
ms.service: multiple
ms.openlocfilehash: db9d6bab2f383102434512aa63d7566cff1f579b
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80411067"
---
# <a name="set-up-micro-focus-cics-bankdemo-for-micro-focus-enterprise-developer-40-on-azure"></a>Azure'da Micro Focus Enterprise Developer 4.0 için Micro Focus CICS BankDemo'u ayarlama

Azure'da Micro Focus Enterprise Server 4.0 ve Enterprise Developer 4.0'ı ayarladığınızda, IBM z/OS iş yüklerinin dağıtımlarını test edebilirsiniz. Bu makalede, Kurumsal Geliştirici ile birlikte gelen örnek bir uygulama olan CICS BankDemo nasıl kurulaca›r› gösterilmektedir.

CCs Müşteri Bilgi Kontrol Sistemi, online ana bilgisayar uygulamalarının çoğu tarafından kullanılan işlem platformu anlamına gelir. BankDemo uygulaması, Enterprise Server ve Enterprise Developer'ın nasıl çalıştığını ve yeşil ekran terminalleriyle birlikte gerçek bir uygulamayı nasıl yönetip dağıtılacak yapılacağını öğrenmek için idealdir.

> [!NOTE]
> Yakında: Azure VM'lerde [Micro Focus Enterprise Server 5.0'ı](https://techcommunity.microsoft.com/t5/azurecat/micro-focus-enterprise-server-5-0-quick-start-template-on-azure/ba-p/1160110) kurma talimatları.

## <a name="prerequisites"></a>Ön koşullar

- [Kurumsal Geliştirici](set-up-micro-focus-azure.md)ile bir VM . Enterprise Developer'ın geliştirme ve test amacıyla enterprise server'ın tam bir örneğine sahip olduğunu unutmayın. Bu örnek, demo için kullanılan Enterprise Server örneğidir.

- [SQL Server 2017 Express sürümü](https://www.microsoft.com/sql-server/sql-server-editions-express). Enterprise Developer VM'ye indirin ve yükleyin. Enterprise Server CICS bölgelerinin yönetimi için bir veritabanı gerektirir ve BankDemo uygulaması da BANKDEMO adlı bir SQL Server veritabanı kullanır. Bu demo, her iki veritabanı için de SQL Server Express kullandığınızı varsayar. Yüklerken temel yüklemeyi seçin.

- [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) (SSMS). SSMS veritabanlarını yönetmek ve Bir T-SQL komut dosyası çalıştırmak için kullanılır. Enterprise Developer VM'ye indirin ve yükleyin.

- [Visual Studio 2019](https://azure.microsoft.com/downloads/) ile en son hizmet paketi veya [Visual Studio Community](https://visualstudio.microsoft.com/vs/community/), ücretsiz olarak indirebilirsiniz.

- Rumba Masaüstü veya başka bir 3270 emülatörü.

## <a name="configure-the-windows-environment"></a>Windows ortamını yapılandırma

Enterprise Developer 4.0'ı VM'ye yükledikten sonra, onunla birlikte gelen Enterprise Server örneğini yapılandırmanız gerekir. Bunu yapmak için aşağıdaki gibi birkaç ek Windows özelliği yüklemeniz gerekir.

1. Oluşturduğunuz Enterprise Server 4.0 VM'de oturum açmak için RDP'yi kullanın.

2. **Başlat** düğmesinin yanındaki **Arama** simgesine tıklayın ve **Windows özelliklerini**yazın. Sunucu Yöneticisi Rolleri ve Özellikleri Ekle Sihirbazı açılır.

3. **Web Sunucusu (IIS) Rolü'nü**seçin ve ardından aşağıdaki seçenekleri denetleyin:

    - Web Yönetim Araçları
    - IIS 6 Yönetim Uyumluluğu (mevcut tüm özellikleri seçin)
    - IIS Yönetim Konsolu
    - IIS Yönetim Komut Dosyaları ve Araçları
    - IIS Yönetim Hizmeti

4. **World Wide Web Services'ı**seçin ve aşağıdaki seçenekleri kontrol edin:

     Uygulama Geliştirme Özellikleri:
    - .NET Genişletilebilirliği
    - ASP.NET
    - Yaygın HTTP Özellikleri: Tüm kullanılabilir özellikleri ekleme
    - Sağlık ve Tanılama: Mevcut tüm özellikleri ekleyin
    - Güvenlik:
        - Temel Kimlik Doğrulaması
        - Windows Kimlik Doğrulaması

5. **Windows İşlem Etkinleştirme Hizmeti'ni** ve tüm çocuklarını seçin.

6. **Özellikler**için **Microsoft .NET framework 3.5.1 seçeneğini**işaretleyin ve aşağıdaki seçenekleri denetleyin:

    - Windows İletişim Vakfı HTTP Aktivasyon
    - Windows Communication Foundation NON-HTTP Etkinleştirme

7. **Özellikler**için **Microsoft .NET framework 4.6'yı**denetleyin ve aşağıdaki seçenekleri denetleyin:

   - Adlı Boru Aktivasyonu
   - TCP Etkinleştirme
   - TCP Bağlantı Noktası Paylaşma

     ![Roller ve Özellikler Sihirbazı Ekle: Rol Hizmetleri](media/01-demo-roles.png)

8. Tüm seçenekleri seçtiğinizde, yüklemek için **İleri'yi** tıklatın.

9. Windows özelliklerinden sonra, **Denetim \> Masası Sistemi \> ve Güvenlik Yönetim Araçları'na**gidin ve **Hizmetleri**seçin. Aşağı kaydırın ve aşağıdaki hizmetlerin çalıştığını ve **Otomatik**olarak ayarlandıklarına emin olun:

    - **Nettcpportsharing**
    - **Net.Boru Dinleyici Adaptörü**
    - **Net.tcp Dinleyici Adaptörü**

10. IIS ve WAS desteğini yapılandırmak için, menüden **Micro Focus Enterprise Developer Command Prompt'ı (64 bit)** bulun ve **Yönetici**olarak çalıştırın.

11. Yazı **wassetup -i** ve **Enter**tuşuna basın .

12. Komut dosyası çalıştırıladıktan sonra pencereyi kapatabilirsiniz.

## <a name="configure-the-local-system-account-for-sql-server"></a>SQL Server için yerel sistem hesabını yapılandırma

Bazı Enterprise Server işlemlerinin SQL Server'da oturum açabilmesi ve veritabanları ve diğer nesneler oluşturabilmesi gerekir. Bu işlemler yerel sistem hesabını kullanır, bu nedenle bu hesaba sysadmin yetkisini vermeniz gerekir.

1. Windows Kimlik Doğrulaması'nı kullanarak yerel SQLEXPRESS Sunucusuna bağlanmak için **SSMS'i** başlatın ve **Bağlan'ı** tıklatın. **Sunucu Adı** listesinde bulunmalıdır.

2. Solda, **Güvenlik** klasörünü genişletin ve **Oturum Açma'yı**seçin.

3. **NT YEtkİ\\SİsteMİ'ni** seçin ve **Özellikler'i**seçin.

4. **Sunucu Rolleri'ni** seçin ve **sysadmin'i denetleyin.**

     ![SSMS Object Explorer penceresi: Giriş Özellikleri](media/02-demo-explorer.png)

## <a name="create-the-bankdemo-database-and-all-its-objects"></a>BankDemo veritabanını ve tüm nesnelerini oluşturun

1. **Windows Explorer'ı** açın ve **C'ye\\gidin:\\\\Kullanıcılar\\Ortak\\Belgeler\\\\Micro\\Focus Enterprise Geliştirici\\Örnekleri Mainframe\\CICS\\DotNet BankDemo SQL**.

2. **BankDemoCreateAll.SQL** dosyasının içeriğini panonuza kopyalayın.

3. Açık **SSMS**. Sağda **Sunucu'yu** tıklatın ve **Yeni Sorgu'yu**seçin.

4. Pano içeriğini **Yeni Sorgu** kutusuna yapıştırın.

5. Sorgunun üstündeki **Komut** sekmesinden **Yürüt'e** tıklayarak SQL'i çalıştırın.

Sorgu hatasız çalıştırılmalıdır. Tamamlandığında, BankDemo uygulaması için örnek veritabanına sahip olursunuz.

![SQLQuery1.sql çıktı](media/03-demo-query.png)

## <a name="verify-that-the-database-tables-and-objects-have-been-created"></a>Veritabanı tablolarının ve nesnelerin oluşturulduğunu doğrulama

1. **BANKDEMO** veritabanına sağ tıklayın ve **Yenile'yi**seçin.

2. **Veritabanını** genişletin ve **Tablolar'ı**seçin. Aşağıdaki gibi bir şey görmelisiniz.

     ![BANKDEMO tablosu Object Explorer'da genişletildi](media/04-demo-explorer.png)

## <a name="build-the-application-in-enterprise-developer"></a>Uygulamayı Enterprise Developer'da oluşturun

1. Visual Studio'u açın ve oturum açın.

2. **Dosya** menüsü seçeneği altında, **Proje/Çözüm Aç**seçeneğini belirleyin, **C:\\Users\\Public\\Documents\\Micro\\Focus Enterprise Developer\\Samples\\Mainframe\\CICS\\DotNet\\BankDemo'ya**gidin ve **sln** dosyasını seçin.

3. Nesneleri incelemek için biraz zaman ayırın. COBOL programları CopyBooks (CPY) ve JCL ile birlikte CBL uzantısı ile Solution Explorer gösterilir.

4. **BankDemo2** Projesi'ne sağ tıklayın ve **Başlangıç Projesi olarak Set'i**seçin.

    > [!NOTE]
    > BankDemo Projesi, bu demo için kullanılmayan HCOSS'u (SQL Server için Host Uyumluluk Seçeneği) kullanır.

5. **Çözüm Gezgini'nde** **BankDemo2** Project'e sağ tıklayın ve **Oluştur'u**seçin.

    > [!NOTE]
    > HCOSS yapılandırılmamalarından, çözüm düzeyinde bina hatalara neden olabilir.

6. Proje oluşturulduğunda, **Çıktı** penceresini inceleyin. Aşağıdaki resme benzer görünmesi gerekir.

     ![Başarılı yapıyı gösteren çıkış penceresi](media/05-demo-output.png)

## <a name="deploy-the-bankdemo-application-into-the-region-database"></a>BankDemo uygulamasını Bölge veritabanına dağıtın

1. Yönetici olarak bir Kurumsal Geliştirici komut istemi (64 bit) açın.

2. **%GENEL%\\\\Belgeler Micro Focus\\Enterprise\\\\Geliştirici\\örnekleri\\Mainframe\\CICS DotNet BankDemo**gidin.

3. Komut isteminde, **bankdemodbdeploy** yürütmek ve veritabanı için parametre dağıtmak için, örneğin dahil:

    ```
    bankdemodbdeploy (local)/sqlexpress
    ```

> [!NOTE]
> Geriye doğru eğik çizgi (/) değil,\\ileri eğik çizgi (/) kullandığınızdan emin olun. Bu komut dosyası bir süre çalışır.

![Yönetim: Kurumsal Geliştirici Komut Istem penceresi](media/06-demo-cmd.png)

## <a name="create-the-bankdemo-region-in-enterprise-administrator-for-net"></a>.NET için Kurumsal Yönetici'de BankDemo Bölgesini Oluşturun

1. **.NET Administration** UI için Enterprise Server'ı açın.

2. MMC snap-in başlatmak için, Windows **Başlat** menüsünden, **Micro Focus Enterprise Developer \> Configuration \> Enterprise Server için .NET Admin**seçin. (Windows Server **için .NET \> Admin için Micro Focus Enterprise Developer Enterprise Server'ı**seçin).

3. Sol bölmedeki **Bölgeler** kapsayıcısını genişletin ve **ardından CICS'yi**sağ tıklatın.

4. (Yerel) veritabanında barındırılan **BANKDEMO**adlı yeni bir CICS Bölgesi oluşturmak için **Bölgeyi Tanımla'yı** seçin.

5. Veritabanı sunucusu örneğini veri, **İleri'yi**tıklatın ve ardından bölge adını **BANKDEMO**girin.

     ![Bölge iletişim kutusunu tanımla](media/07-demo-cics.png)

6. Bölgeler arası veritabanı için bölge tanım dosyasını seçmek için, **C\_bölgesinde\_bankdemo db.config'i** **bulun:\\\\\\\\\\\\\\Kullanıcılar\\Genel\\Belgeler Mikro Odak Kurumsal Geliştirici Örnekleri Mainframe\\CICS DotNet BankDemo**.

     ![Bölge Tanımla - Bölge adı: BANKDEMO](media/08-demo-cics.png)

7. **Son**'a tıklayın.

## <a name="create-xa-resource-definitions"></a>XA Kaynak Tanımları Oluşturma

1. **.NET Administration** UI için Enterprise Server'ın sol bölmesinde, **Sistemi**genişletin ve ardından **XA Kaynak Tanımları.** Bu ayar, Bölgenin Enterprise Server ve uygulama veritabanları ile nasıl birlikte çalıştığını tanımlar.

2. **XA Kaynak Tanımları'na** sağ tıklayın ve **Sunucu Örneği Ekle'yi**seçin.

3. Açılan kutuda **Veritabanı Hizmet Örneği'ni**seçin. Bu yerel makine SQLEXPRESS olacaktır.

4. **XA Kaynak\\Tanımları (makine adı sqlexpress)** kapsayıcısının altından örneği seçin ve **Ekle'yi**tıklatın.

5. **Veritabanı XA Kaynak Tanımı'nı** seçin ve ardından **Ad** ve **Bölge**için **BANKDEMO** yazın.

     ![Yeni Veritabanı XA Kaynak Tanımı ekranı](media/09-demo-xa.png)

6. Bağlantı Dizesihirbazı sihirbazını getirmek için elipsleri (**...**) tıklatın. **Sunucu Adı**için , yazın **\\(yerel) SQLEXPRESS**. **Logon**için **Windows Kimlik Doğrulama'yı**seçin. Veritabanı adı için **BANKDEMO** yazın

     ![Bağlantı String ekranına getir](media/10-demo-string.png)

7. Bağlantıyı test edin.

## <a name="start-the-bankdemo-region"></a>BANKDEMO Bölgesini Başlat

> [!NOTE]
> İlk adım önemlidir: Bölge'yi yeni oluşturduğunuz XA Kaynak Tanımı'nı kullanacak şekilde ayarlamanız gerekir.

1. **Bölgeler Kapsayıcısı**altındaki **BANDEMO CICS Bölgesi'ne** gidin ve ardından **Eylemler** bölmesinden **Bölge Başlatma Dosyasını Edit'i** seçin. SQL özelliklerine gidin ve **XA kaynak adı**için **bankdemo** girin veya seçmek için elipsleri kullanın.

2. Değişikliklerinizi kaydetmek için **Kaydet** simgesini tıklatın.

3. **Konsol** bölmesinde **BANKDEMO CICS Bölgesi'ne** sağ tıklayın ve **Başlat/Durdur Bölgesi'ni**seçin.

4. Orta bölmede görünen **Başlat/Durdur Bölgesi** kutusunun alt kısmında **Başlat'ı**seçin. Birkaç saniye sonra bölge başlar.

     ![SQL Başlat/Durdur kutusu](media/11-demo-sql.png)

     ![CICS Bölge BANKDEMO - Ekran başladı](media/12-demo-cics.png)

## <a name="create-a-listener"></a>Dinleyici oluşturma

BankDemo uygulamasına erişen TN3270 oturumları için bir dinleyici oluşturun.

1. Sol bölmede, **Configuration Editors'u** genişletin ve **Dinleyici'yi**seçin.

2. **Dosyayı Aç** simgesine tıklayın ve **seelistener.exe.config** dosyasını seçin. Bu dosya düzenlenir ve Enterprise Server her başlatılında yüklenir.

3. Daha önce tanımlanan iki bölgeye (ESDEMO ve JCLDEMO) dikkat edin.

4. BANKDEMO için yeni bir Bölge oluşturmak için **Bölgeler'i**sağ tıklatın ve **Bölge Ekle'yi**seçin.

5. **BANKDEMO Bölgesini**seçin.

6. **BANKDEMO Bölgesi'ni** sağ tıklayarak ve **Kanal Ekle'yi**seçerek Bir TN3270 kanalı ekleyin.

7. **Ad**için **TN3270**girin. **Bağlantı Noktası**için , **9024**girin. ESDEMO uygulaması farklı bir bağlantı noktası kullanmanız gerekir, bu yüzden port 9230 kullanır.

8. Dosyayı kaydetmek için **Kaydet** simgesini tıklatın veya **Dosya** \> **Yı küçül'**'i seçin.

9. Dinleyiciyi başlatmak için **Dinleyiciyi Başlat** simgesini tıklatın veya **Options** \> **Start Listener'ı**seçin.

     ![Dinleyici Yapılandırma Düzenleyicisi pencereleri](media/13-demo-listener.png)


## <a name="configure-rumba-to-access-the-bankdemo-application"></a>Rumba'yı BankDemo uygulamasına erişmek için yapılandırın

Yapmanız gereken son şey Rumba, bir 3270 emülatörü kullanarak bir 3270 oturum yapılandırmaktır. Bu adım, oluşturduğunuz dinleyici aracılığıyla BankDemo uygulamasına erişmenizi sağlar.

1. Windows **Başlat** menüsünden Rumba Desktop'ı başlatın.

2. **Connections** menü öğesinin altında **TN3270'i**seçin.

3. IP adresi için **Ekle** ve **127.0.0.1** yazın ve kullanıcı tanımlı bağlantı noktası için **9024'ü** tıklatın.

4. İletişim kutusunun alt kısmında **Bağlan'ı**tıklatın. Siyah bir CICS ekranı görüntülenir.

5. BankDemo uygulaması için ilk 3270 ekranını görüntülemek için **banka** yazın.

6. Kullanıcı Kimliği için **B0001** yazın ve parola için her şeyi yazın. İlk ekran BANK20 açılır.

![Mainframe Ekran](media/14-demo.png)
![Karşılama ekranı Mainframe Ekran - Rumba - Alt Sistem Gösterim ekranı](media/15-demo.png)

Tebrikler! Şimdi Micro Focus Enterprise Server kullanarak Azure'da bir CICS uygulaması çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'da Docker kapsayıcılarında Enterprise Server'ı çalıştırın](run-enterprise-server-container.md)
- [Ana Bilgisayar Geçişi - Portal](https://blogs.msdn.microsoft.com/azurecat/2018/11/16/mainframe-migration-to-azure-portal/)
- [Sanal Makineler](https://docs.microsoft.com/azure/virtual-machines/linux/overview)
- [Sorun giderme](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/)
- [Azure geçişiiçin ana bilgisayarın mymystifying](https://azure.microsoft.com/resources/demystifying-mainframe-to-azure-migration/en-us/)
