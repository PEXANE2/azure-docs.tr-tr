---
title: SQL veritabanını alma veya dışa aktarma
description: Azure hizmetlerinin sunucuya erişmesine izin vermeden bir Azure SQL veritabanını içe aktarın veya dışa aktarın.
services: sql-database
ms.service: sql-database
ms.subservice: migration
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 01/08/2020
ms.openlocfilehash: 61a85b2554bbd69541b3081f72525d2b7deed625
ms.sourcegitcommit: c5661c5cab5f6f13b19ce5203ac2159883b30c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80529227"
---
# <a name="import-or-export-an-azure-sql-database-without-allowing-azure-services-to-access-the-server"></a>Azure hizmetlerinin sunucuya erişmesine izin vermeden azure SQL veritabanını alma veya dışa aktarma

Bu makalede, *Azure Hizmetlerine İzin Ver, Azure* SQL sunucusunda *KAPALI* olarak ayarlandığında azure SQL veritabanını nasıl içe aktarabileceğinizi veya dışa aktarabileceğinizi gösterir. İş akışı, alma veya dışa aktarma işlemini gerçekleştirmek için SqlPackage'ı çalıştırmak için bir Azure sanal makinesi kullanır.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="create-the-azure-virtual-machine"></a>Azure sanal makinesini oluşturun

**Azure'a Dağıt** düğmesini seçerek bir Azure sanal makinesi oluşturun.

Bu şablon, windows sürümü için birkaç farklı seçenek kullanarak, en son yamalı sürümü kullanarak basit bir Windows sanal makine dağıtmak için izin verir. Bu, kaynak grubu konumunda bir A2 boyutuVM dağıtır ve VM'nin tam nitelikli etki alanı adını döndürecektir.
<br><br>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-vm-simple-windows%2Fazuredeploy.json" target="_blank">
    <img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

Daha fazla bilgi için Windows [VM'nin çok basit dağıtımına](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)bakın.


## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma

Aşağıdaki adımlar, uzak bir masaüstü bağlantısı kullanarak sanal makinenize nasıl bağlanabileceğinizi gösterir.

1. Dağıtım tamamlandıktan sonra sanal makine kaynağına gidin.

    ![VM](./media/import-export-from-vm/vm.png)  

2. **Bağlan**’ı seçin.

   Sanal makinenin ortak IP adresi ve bağlantı noktası numarasıyla birlikte Uzak Masaüstü Protokolü dosyası (.rdp dosyası) formu görüntülenir.

   ![RDP formu](./media/import-export-from-vm/rdp.png)  

3. **RDP Dosyasını İndir'i**seçin.

   > [!NOTE]
   > VM'inize bağlanmak için SSH'yi de kullanabilirsiniz.

4. **Connect'i sanal makine formuna** kapatın.
5. VM'nize bağlanmak için indirilen RDP dosyasını açın.
6. İstendiğinde **Bağlan'ı**seçin. Mac bilgisayarlarda, Mac App Store’dan bu [Uzak Masaüstü İstemcisi](https://apps.apple.com/app/microsoft-remote-desktop-10/id1295203466?mt=12) gibi bir RDP istemcisi indirmeniz gerekir.

7. Sanal makine oluştururken belirttiğiniz kullanıcı adı ve parolayı girin ve **ardından Tamam'ı**seçin.

8. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. **Bağlantıya** devam etmek için Evet veya **Devam et'i** seçin.



## <a name="install-sqlpackage"></a>SqlPackage Yükle

[SqlPackage'ın en son sürümünü indirin ve kurun.](https://docs.microsoft.com/sql/tools/sqlpackage-download)




Daha fazla bilgi için [SqlPackage.exe'ye](https://docs.microsoft.com/sql/tools/sqlpackage)bakın.

## <a name="create-a-firewall-rule-to-allow-the-vm-access-to-the-database"></a>VM'nin veritabanına erişmesine izin vermek için bir güvenlik duvarı kuralı oluşturma

Sanal makinenin genel IP adresini SQL Veritabanı sunucu güvenlik duvarına ekleyin.

Aşağıdaki adımlar, sanal makinenizin genel IP adresi için sunucu düzeyinde bir IP güvenlik duvarı kuralı oluşturur ve sanal makineden bağlantı sağlar.

1. Sol menüden **SQL veritabanlarını** seçin ve ardından **SQL veritabanları** sayfasında veritabanınızı seçin. Veritabanınız için genel bakış sayfası açılır ve size tam nitelikli sunucu adını **(servername.database.windows.net**gibi) gösterir ve daha fazla yapılandırma için seçenekler sunar.

2. Sunucunuza ve veritabanlarına bağlanırken kullanmak üzere bu tam nitelikli sunucu adını kopyalayın.

   ![sunucu adı](./media/sql-database-get-started-portal/server-name.png)

3. Araç çubuğunda **sunucu güvenlik duvarını** ayarla'yı seçin. Veritabanı sunucusuiçin **Güvenlik Duvarı ayarları** sayfası açılır.

   ![sunucu düzeyinde IP güvenlik duvarı kuralı](./media/sql-database-get-started-portal/server-firewall-rule.png)

4. Sanal makinenizin genel IP adresini sunucu düzeyindeki yeni bir IP güvenlik duvarı kuralına eklemek için araç çubuğuna **istemci IP ekle'yi** seçin. Sunucu düzeyindeki BIR IP güvenlik duvarı kuralı, tek bir IP adresi veya çeşitli IP adresleri için 1433 bağlantı noktasını açabilir.

5. **Kaydet'i**seçin. Sanal makinenizin SQL Veritabanı sunucusundaki 1433 bağlantı noktasını açması için sunucu düzeyinde bir IP güvenlik duvarı kuralı oluşturulur.

6. Güvenlik **Duvarı ayarları** sayfasını kapatın.



## <a name="export-a-database-using-sqlpackage"></a>SqlPackage kullanarak veritabanı nı dışa aktarma

[SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) komut satırı yardımcı programını kullanarak bir SQL veritabanı dışa aktarmak için, [dışa aktarma parametreleri ve özellikleri](https://docs.microsoft.com/sql/tools/sqlpackage#export-parameters-and-properties)ne bakın. [SQLServer Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) ve SQL Server Data [Tools'un](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)en son sürümleriile SqlPackage yardımcı programı veya [SqlPackage'ın](https://docs.microsoft.com/sql/tools/sqlpackage-download)en son sürümünü indirebilirsiniz.

Çoğu üretim ortamlarında ölçek ve performans için SqlPackage yardımcı programının kullanılmasını öneririz. BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://blogs.msdn.microsoft.com/sqlcat/20../../migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

Bu örnek, Active Directory Universal Authentication ile SqlPackage.exe kullanarak veritabanının nasıl dışa aktarılabildiğini gösterir. Ortamınıza özgü değerlerle değiştirin.

```cmd
SqlPackage.exe /a:Export /tf:testExport.bacpac /scs:"Data Source=<servername>.database.windows.net;Initial Catalog=MyDB;" /ua:True /tid:"apptest.onmicrosoft.com"
```




## <a name="import-a-database-using-sqlpackage"></a>SqlPackage kullanarak veritabanı alma

[SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage) komut satırı yardımcı programını kullanarak bir SQL Server veritabanı almak için [alma parametrelerini ve özelliklerini](https://docs.microsoft.com/sql/tools/sqlpackage#import-parameters-and-properties)görün. SqlPackage en son [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) ve SQL Server Veri [Araçları](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)vardır. [Ayrıca SqlPackage](https://docs.microsoft.com/sql/tools/sqlpackage-download)en son sürümünü indirebilirsiniz.

Ölçek ve performans için, Azure portalını kullanmak yerine çoğu üretim ortamlarında SqlPackage kullanmanızı öneririz. Dosyaları kullanarak `BACPAC` geçiş hakkında bir SQL Server Müşteri Danışma Ekibi blogunda [BACPAC Dosyalarını kullanarak SQL Server'dan Azure SQL Veritabanına geçiş](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/)bkz.

Aşağıdaki SqlPackage komutu **AdventureWorks2017** veritabanını yerel depolamadan azure SQL Veritabanı sunucusuna alır. **Premium** hizmet katmanı ve **P6** Hizmet Hedefi ile **myMigratedDatabase** adında yeni bir veritabanı oluşturur. Bu değerleri ortamınız için uygun şekilde değiştirin.

```cmd
sqlpackage.exe /a:import /tcs:"Data Source=<serverName>.database.windows.net;Initial Catalog=myMigratedDatabase>;User Id=<userId>;Password=<password>" /sf:AdventureWorks2017.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
```

> [!IMPORTANT]
> Kurumsal bir güvenlik duvarının arkasından tek bir veritabanını yöneten bir SQL Veritabanı sunucusuna bağlanmak için güvenlik duvarının 1433 portu açık olması gerekir. Yönetilen bir örne bağlanmak için noktadan [siteye bağlantınız](sql-database-managed-instance-configure-p2s.md) veya açık bir rota bağlantınız olması gerekir.

Bu örnek, Active Directory Universal Authentication ile SqlPackage kullanarak bir veritabanının nasıl içe aktarılabildiğini gösterir.

```cmd
sqlpackage.exe /a:Import /sf:testExport.bacpac /tdn:NewDacFX /tsn:apptestserver.database.windows.net /ua:True /tid:"apptest.onmicrosoft.com"
```

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Dışa aktarma hızları birçok etkene (örneğin, veri şekli) bağlı olarak değişir, bu nedenle hangi hızın beklendiğini tahmin etmek imkansızdır. SqlPackage, özellikle büyük veritabanları için önemli ölçüde zaman alabilir.

En iyi performansı elde etmek için aşağıdaki stratejileri deneyebilirsiniz:

1. Veritabanında başka iş yükü çalışmadığından emin olun. Dışa aktarma, başka iş yüklerinin çalışmadığından emin olmak için en iyi çözüm olmadan önce bir kopyasını oluşturun.
2. Dışa aktarma iş yükünü daha iyi işlemek için veritabanı hizmet düzeyi hedefini (SLO) artırın (öncelikle G/Ç'yi okuyun). Veritabanı şu anda GP_Gen5_4, belki bir İş Kritik katmanı okuma iş yükü ile yardımcı olabilir.
3. Özellikle büyük tablolar için kümelenmiş dizinler olduğundan emin olun. 
4. Sanal makineler (VM'ler), ağ kısıtlamalarını önlemeye yardımcı olmak için veritabanıyla aynı bölgede olmalıdır.
5. VM'ler blob depolama yüklemeden önce geçici eserler oluşturmak için yeterli boyutu ile SSD olmalıdır.
6. VM'ler belirli bir veritabanı için yeterli çekirdek ve bellek yapılandırması olmalıdır.

## <a name="store-the-imported-or-exported-bacpac-file"></a>İçe aktarılan veya dışa aktarılan depolayın. BACPAC dosyası

Şey. BACPAC dosyası [Azure Blobs](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview)veya [Azure Dosyaları'nda](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)depolanabilir. 

En iyi performansı elde etmek için Azure Dosyaları'nı kullanın. SqlPackage, Azure Dosyalarına doğrudan erişebilmek için dosya sistemiyle çalışır.

Maliyeti azaltmak için, premium Azure dosya paylaşımından daha düşük maliyetli Azure Blobs'u kullanın. Ancak, kopyalamanız [gerekir. ](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications#bacpac)İçe aktarma veya dışa aktarma işleminden önce blob ve yerel dosya sistemi arasındaki BACPAC dosyası. Sonuç olarak işlem daha uzun sürer.

Yüklemek veya indirmek için . BACPAC dosyaları, [Bkz. AzCopy ve Blob depolama ile veri aktarımı](../storage/common/storage-use-azcopy-blobs.md)ve [AzCopy ve dosya depolama ile veri aktarımı.](../storage/common/storage-use-azcopy-files.md)

Ortamınıza bağlı olarak, Azure [Depolama güvenlik duvarlarını ve sanal ağları yapılandırmanız](../storage/common/storage-network-security.md)gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

- İçe aktarılan bir SQL Veritabanına nasıl bağlanıp sorgulayabilirsiniz öğrenmek [için, bkz.](sql-database-connect-query-ssms.md)
- BACPAC dosyalarını kullanarak geçiş hakkında bir SQL Server Müşteri Danışmanlık Ekibi blogu için bkz. [BACPAC Dosyalarını kullanarak SQL Server’dan Azure SQL Veritabanına Geçiş](https://techcommunity.microsoft.com/t5/DataCAT/Migrating-from-SQL-Server-to-Azure-SQL-Database-using-Bacpac/ba-p/305407).
- Performans önerileri de dahil olmak üzere tüm SQL Server veritabanı geçiş işleminin tartışılması [için](sql-database-single-database-migrate.md)bkz.
- Depolama anahtarlarını ve paylaşılan erişim imzalarını güvenli bir şekilde nasıl yönetip paylaşılamayı öğrenmek için [Azure Depolama Güvenlik Kılavuzu'na](https://docs.microsoft.com/azure/storage/common/storage-security-guide)bakın.
