---
title: Ayırma-birleştirme hizmetini dağıtma
description: Verileri kırık veritabanları arasında taşımak için bölme birleştirme'yi de kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 12/04/2018
ms.openlocfilehash: b6f61de23ab4b637cfb5b8ee365ddea9764bf515
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80810196"
---
# <a name="deploy-a-split-merge-service-to-move-data-between-sharded-databases"></a>Verileri kırık veritabanları arasında taşımak için bölünmüş birleştirme hizmeti dağıtma

Bölme birleştirme aracı, verileri kırık veritabanları arasında taşımanızı sağlar. Bkz. [Ölçeklenmiş bulut veritabanları arasında veri taşıma](sql-database-elastic-scale-overview-split-and-merge.md)

## <a name="download-the-split-merge-packages"></a>Bölme Birleştirme paketlerini indirin

1. [NuGet](https://docs.nuget.org/docs/start-here/installing-nuget)en son NuGet sürümünü indirin.

1. Bir komut istemi açın ve nuget.exe'yi indirdiğiniz dizine gidin. İndirme PowerShell komutları içerir.

1. En son Split-Merge paketini aşağıdaki komutla geçerli dizine indirin:

   ```cmd
   nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge
   ```  

Dosyalar, *x.x.xxx.x'in* sürüm numarasını yansıttığı **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** adlı bir dizine yerleştirilir. **İçerik\splitmerge\service** alt dizininde bölünmüş birleştirme Hizmeti dosyalarını ve **content\splitmerge\powershell** alt dizininde Split-Merge PowerShell komut dosyalarını (ve gerekli istemci dll'lerini) bulun.

## <a name="prerequisites"></a>Ön koşullar

1. Bölme birleştirme durumu veritabanı olarak kullanılacak bir Azure SQL DB veritabanı oluşturun. [Azure portalına](https://portal.azure.com)gidin. Yeni bir **SQL Veritabanı**oluşturun. Veritabanına bir ad verin ve yeni bir yönetici ve parola oluşturun. Daha sonra kullanmak üzere adı ve parolayı kaydettiğinden emin olun.

1. Azure SQL DB sunucunuzun Azure Hizmetleri'nin sunucuya bağlanmasına izin verdiğinden emin olun. Portalda, Güvenlik **Duvarı**Ayarları'nda, **Azure Hizmetlerine Erişime İzin Verme** ayarı **Açık**olarak ayarlandığından emin olun. "Kaydet" simgesine tıklayın.

1. Tanılama çıktısı için bir Azure Depolama hesabı oluşturun.

1. Bölme Birleştirme hizmetiniz için bir Azure Bulut Hizmeti oluşturun.

## <a name="configure-your-split-merge-service"></a>Bölme Birleştirme hizmetinizi yapılandırın

### <a name="split-merge-service-configuration"></a>Birleştirme hizmeti yapılandırması

1. Split-Merge derlemelerini indirdiğiniz klasörde, *SplitMergeService.cspkg* ile birlikte gönderilen *ServiceConfiguration.Template.cscfg* dosyasının bir kopyasını oluşturun ve *serviceConfiguration.cscfg'yi*yeniden adlandırın.

1. *ServiceConfiguration.cscfg'yi* Visual Studio gibi sertifika parmak izlerinin biçimi gibi girişleri doğrulayan bir metin düzenleyicisinde açın.

1. Yeni bir veritabanı oluşturun veya Bölme Birleştirme işlemleri için durum veritabanı olarak hizmet vermek ve bu veritabanının bağlantı dizisini almak için varolan bir veritabanı seçin.

   > [!IMPORTANT]
   > Şu anda durum veritabanında Latin harmanlama\_(SQL\_\_Latin1\_\_General CP1 CI AS) kullanılmalıdır. Daha fazla bilgi için [Windows Collation Name (Transact-SQL)](https://msdn.microsoft.com/library/ms188046.aspx)bakın.

   Azure SQL DB ile bağlantı dizesi genellikle formdan dır:

      `Server=<serverName>.database.windows.net; Database=<databaseName>;User ID=<userId>; Password=<password>; Encrypt=True; Connection Timeout=30`

1. ElasticScaleMetaveri ayarındaki **SplitMergeWeb** ve **SplitMergeWorker** rol bölümlerine *.cscfg* dosyasına bu bağlantı dizesini girin.

1. **SplitMergeWorker** rolü **için, WorkerRoleSynchronizationStorageAccountConnectionString** ayarı için Azure depolamasına geçerli bir bağlantı dizesi girin.

### <a name="configure-security"></a>Güvenliği yapılandırma

Hizmetin güvenliğini yapılandırmak için ayrıntılı yönergeler [için, Bölünmüş Birleştirme güvenlik yapılandırmasına](sql-database-elastic-scale-split-merge-security-configuration.md)bakın.

Bu öğretici için basit bir test dağıtımı amacıyla, hizmeti çalışır hale getirmek için en az sayıda yapılandırma adımı gerçekleştirilir. Bu adımlar, yalnızca bunları çalıştıran bir makinenin/hesabın hizmetle iletişim kurmasını sağlar.

### <a name="create-a-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika oluşturma

Yeni bir dizin oluşturun ve bu dizinden [Visual Studio için Geliştirici Komut Komut Ustem](https://msdn.microsoft.com/library/ms229859.aspx) penceresini kullanarak aşağıdaki komutu çalıştırın:

   ```cmd
   makecert ^
    -n "CN=*.cloudapp.net" ^
    -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2" ^
    -a sha256 -len 2048 ^
    -sr currentuser -ss root ^
    -sv MyCert.pvk MyCert.cer
   ```

Özel anahtarı korumak için bir parola istenir. Güçlü bir parola girin ve onaylayın. Daha sonra parolanın bundan sonra bir kez daha kullanılması istenir. Güvenilir Sertifika Yetkilileri Kök deposuna almak için sonunda **Evet'i** tıklatın.

### <a name="create-a-pfx-file"></a>PFX dosyası oluşturma

Makecert'in yürütüldüğü aynı pencereden aşağıdaki komutu çalıştırın; sertifikayı oluşturmak için kullandığınız parolayı kullanın:

   ```cmd
   pvk2pfx -pvk MyCert.pvk -spc MyCert.cer -pfx MyCert.pfx -pi <password>
   ```

### <a name="import-the-client-certificate-into-the-personal-store"></a>İstemci sertifikasını kişisel mağazaya alma

1. Windows Gezgini'nde *MyCert.pfx'i*çift tıklatın.
2. Sertifika **Alma Sihirbazı'nda** **Geçerli Kullanıcı'yı** seçin ve **İleri'yi**tıklatın.
3. Dosya yolunu onaylayın ve **İleri'yi**tıklatın.
4. Parolayı yazın, tüm **genişletilmiş özellikleri işaretlene belirtin** ve **İleri'yi**tıklatın.
5. Bırak **Otomatik olarak sertifika mağazasını seçin[...]** kontrol edin ve **İleri'yi**tıklatın.
6. **Bitiş** ve **Tamam'ı**tıklatın.

### <a name="upload-the-pfx-file-to-the-cloud-service"></a>PFX dosyasını bulut hizmetine yükleme

1. [Azure portalına](https://portal.azure.com)gidin.
2. **Bulut Hizmetlerini**seçin.
3. Bölme/Birleştirme hizmeti için yukarıda oluşturduğunuz bulut hizmetini seçin.
4. Üst menüde **Sertifikalar'ı** tıklatın.
5. Alt takiben **Yükle'yi** tıklatın.
6. PFX dosyasını seçin ve yukarıdaki yle aynı parolayı girin.
7. Tamamlandıktan sonra, listedeki yeni girişten sertifika parmak izini kopyalayın.

### <a name="update-the-service-configuration-file"></a>Hizmet yapılandırma dosyasını güncelleştirme

Yukarıda kopyalanan sertifika parmak izinizini bu ayarların parmak izi/değer özniteliğine yapıştırın.
İşçi rolü için:

   ```xml
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

Web rolü için:

   ```xml
    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />
    <Setting name="AllowedClientCertificateThumbprints" value="" />
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />
   ```

Üretim dağıtımları için CA, şifreleme, Sunucu sertifikası ve istemci sertifikaları için ayrı sertifikalar kullanılması gerektiğini lütfen unutmayın. Bu konudaayrıntılı talimatlar için [Güvenlik Yapılandırması'na](sql-database-elastic-scale-split-merge-security-configuration.md)bakın.

## <a name="deploy-your-service"></a>Hizmetinizi dağıtın

1. [Azure portalına](https://portal.azure.com) gidin
2. Daha önce oluşturduğunuz bulut hizmetini seçin.
3. **Genel Bakış**'a tıklayın.
4. Evreleme ortamını seçin ve ardından **Yükle'yi**tıklatın.
5. İletişim kutusuna bir dağıtım etiketi girin. Hem 'Paket' hem de 'Yapılandırma' için 'Yerel'den' seçeneğini tıklayın ve *SplitMergeService.cspkg* dosyasını ve daha önce yapılandırdığınız cscfg dosyanızı seçin.
6. **Bir veya daha fazla rol tek bir örnek işaretlebilse bile Dağıt** etiketli onay kutusunun olduğundan emin olun.
7. Dağıtıma başlamak için sağ alttaki onay düğmesine basın. Tamamlanmasının birkaç dakika sürmesini bekleyin.

## <a name="troubleshoot-the-deployment"></a>Dağıtımı sorun giderme

Web rolünüz çevrimiçi duruma geçmezse, güvenlik yapılandırmasıyla ilgili bir sorun olabilir. TLS/SSL'nin yukarıda açıklandığı şekilde yapılandırıldığından kontrol edin.

Çalışan rolünüz çevrimiçi duruma geçmezse, ancak web rolünüz başarılı olursa, bu büyük olasılıkla daha önce oluşturduğunuz durum veritabanına bağlanan bir sorundur.

- Cscfg'nizdeki bağlantı dizesinin doğru olduğundan emin olun.
- Sunucu nun ve veritabanının var olup olmadığını ve kullanıcı kimliği ve parolasının doğru olup olmadığını denetleyin.
- Azure SQL DB için bağlantı dizesi formdan olmalıdır:

   `Server=<serverName>.database.windows.net; Database=<databaseName>;User ID=<user>; Password=<password>; Encrypt=True; Connection Timeout=30`

- Sunucu adının **https://** ile başlamadığından emin olun.
- Azure SQL DB sunucunuzun Azure Hizmetleri'nin sunucuya bağlanmasına izin verdiğinden emin olun. Bunu yapmak için veritabanınızı portalda açın ve **Azure Hizmetlerine Erişime İzin verme** ayarını **Açık* olarak ayarladığından emin olun.

## <a name="test-the-service-deployment"></a>Hizmet dağıtımını test edin

### <a name="connect-with-a-web-browser"></a>Bir web tarayıcısı ile bağlanma

Bölme Birleştirme hizmetinizin web bitiş noktasını belirleyin. Bunu bulut hizmetinize **Genel Bakış'a** giderek ve sağ **taraftaki Site URL'sinin** altına bakarak portalda bulabilirsiniz. Varsayılan güvenlik ayarları HTTP bitiş noktasını devre dışı kdığından **http://** **https://** değiştirin. Bu URL'nin sayfasını tarayıcınıza yükleyin.

### <a name="test-with-powershell-scripts"></a>PowerShell komut dosyalarıyla test edin

Dağıtım ve ortamınız, dahil edilen örnek PowerShell komut dosyalarını çalıştırarak sınanabilir.

Dahil komut dosyası dosyaları şunlardır:

1. *SetupSampleSplitMergeEnvironment.ps1* - Bölme/Birleştirme için bir test veri katmanı ayarlar (ayrıntılı açıklama için aşağıdaki tabloya bakın)
2. *ExecuteSampleSplitMerge.ps1* - test veri katmanı üzerinde test işlemleri yürütür (ayrıntılı açıklama için aşağıdaki tabloya bakın)
3. *GetMappings.ps1* - parça eşlemelerin geçerli durumunu yazdıran üst düzey örnek komut dosyası.
4. *ShardManagement.psm1 - ShardManagement* API sarar yardımcı komut dosyası
5. *SqlDatabaseHelpers.psm1* - SQL veritabanları oluşturmak ve yönetmek için yardımcı komut dosyası
   
   <table style="width:100%">
     <tr>
       <th>PowerShell dosyası</th>
       <th>Adımlar</th>
     </tr>
     <tr>
       <th rowspan="5">SetupSampleSplitMergeEnvironment.ps1</th>
       <td>1. Sabir harita yöneticisi veritabanı oluşturur</td>
     </tr>
     <tr>
       <td>2. 2 parça veritabanları oluşturur.
     </tr>
     <tr>
       <td>3. Bu veritabanları için bir parça eşlem oluşturur (bu veritabanlarındaki varolan tüm parça eşlemleri siler). </td>
     </tr>
     <tr>
       <td>4. Her iki parçada da küçük bir örnek tablo oluşturur ve tabloyu kırıklardan birinde doldurur.</td>
     </tr>
     <tr>
       <td>5. SchemaInfo'yu kırık tablo için bildirir.</td>
     </tr>
   </table>
   <table style="width:100%">
     <tr>
       <th>PowerShell dosyası</th>
       <th>Adımlar</th>
     </tr>
   <tr>
       <th rowspan="4">ExecuteSampleSplitMerge.ps1 </th>
       <td>1. Verilerin yarısını ilk parçadan ikinci parçaya bölen Split-Merge Service web frontend'e bölünmüş bir istek gönderir.</td>
     </tr>
     <tr>
       <td>2. Bölünmüş istek durumu için web ön uçlarını yoklar ve istek tamamlanana kadar bekler.</td>
     </tr>
     <tr>
       <td>3. Verileri ikinci parçadan ilk parçaya geri taşıyan Split-Merge Service web frontend'e birleştirme isteği gönderir.</td>
     </tr>
     <tr>
       <td>4. Birleştirme isteği durumu için web ön ucunu yoklar ve istek tamamlanana kadar bekler.</td>
     </tr>
   </table>
   
## <a name="use-powershell-to-verify-your-deployment"></a>Dağıtımınızı doğrulamak için PowerShell'i kullanın

1. Yeni bir PowerShell penceresi açın ve Split-Merge paketini indirdiğiniz dizine gidin ve ardından "powershell" dizinine gidin.

2. Parça harita yöneticisi ve parçaların oluşturulacağı bir Azure SQL Veritabanı sunucusu (veya varolan bir sunucu seçin) oluşturun.

   > [!NOTE]
   > *SetupSampleSplitMergeEnvironment.ps1* komut dosyası komut dosyası basit tutmak için varsayılan olarak aynı sunucuda tüm bu veritabanları oluşturur. Bu, Bölme Birleştirme Hizmetinin kendisini kısıtlamaz.

   BB'lere okuma/yazma erişimi olan bir SQL kimlik doğrulama girişi, Split-Merge hizmetinin verileri taşıması ve parça eşlemesini güncelleştirmesi için gerekli olacaktır. Bölme Birleştirme Hizmeti bulutta çalıştığından, şu anda Tümleşik Kimlik Doğrulama'yı desteklemez.

   Azure SQL sunucusunun bu komut dosyalarını çalıştıran makinenin IP adresinden erişime izin verecek şekilde yapılandırıldığından emin olun. Bu ayarı Azure SQL sunucusu / yapılandırma / izin verilen IP adresleri altında bulabilirsiniz.

3. Örnek ortamı oluşturmak için *SetupSampleSplitMergeEnvironment.ps1* komut dosyasını çalıştırın.

   Bu komut dosyasının çalıştırılması, parçalı harita yöneticisi veritabanında ve kırıklarda varolan tüm parça harita yönetimi veri yapılarını silip süpürür. Parça lı haritayı veya parçaları yeniden başlatmak istiyorsanız komut dosyasını yeniden çalıştırmak yararlı olabilir.

   Örnek komut satırı:

   ```cmd
   .\SetupSampleSplitMergeEnvironment.ps1
    -UserName 'mysqluser' -Password 'MySqlPassw0rd' -ShardMapManagerServerName 'abcdefghij.database.windows.net'
   ```

4. Şu anda örnek ortamında bulunan eşlemeleri görüntülemek için Getmappings.ps1 komut dosyasını çalıştırın.

   ```cmd
   .\GetMappings.ps1
    -UserName 'mysqluser' -Password 'MySqlPassw0rd' -ShardMapManagerServerName 'abcdefghij.database.windows.net'
   ```

5. Bir bölme işlemi (ilk parçadaki verilerin yarısını ikinci parçaya taşımak) ve ardından birleştirme işlemini (verileri ilk parçaya geri taşımak) yürütmek için *ExecuteSampleSplitMerge.ps1* komut dosyasını çalıştırın. TLS'yi yapılandırıp http uç noktasını devre dışı bıraktıysanız, bunun yerine https:// bitiş noktasını kullandığınızdan emin olun.

   Örnek komut satırı:

   ```cmd
   .\ExecuteSampleSplitMerge.ps1
    -UserName 'mysqluser' -Password 'MySqlPassw0rd' 
    -ShardMapManagerServerName 'abcdefghij.database.windows.net' 
    -SplitMergeServiceEndpoint 'https://mysplitmergeservice.cloudapp.net' 
    -CertificateThumbprint '0123456789abcdef0123456789abcdef01234567'
   ```

   Aşağıdaki hatayı alırsanız, büyük olasılıkla Web bitiş noktanızın sertifikasında bir sorun vardır. En sevdiğiniz Web tarayıcısıyla Web bitiş noktasına bağlanmayı deneyin ve bir sertifika hatası olup olmadığını kontrol edin.

     `Invoke-WebRequest : The underlying connection was closed: Could not establish trust relationship for the SSL/TLSsecure channel.`

   Başarılı olduysa, çıktı aşağıdaki gibi görünmelidir:

   ```output
   > .\ExecuteSampleSplitMerge.ps1 -UserName 'mysqluser' -Password 'MySqlPassw0rd' -ShardMapManagerServerName 'abcdefghij.database.windows.net' -SplitMergeServiceEndpoint 'http://mysplitmergeservice.cloudapp.net' -CertificateThumbprint 0123456789abcdef0123456789abcdef01234567
   > Sending split request
   > Began split operation with id dc68dfa0-e22b-4823-886a-9bdc903c80f3
   > Polling split-merge request status. Press Ctrl-C to end
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source to target shard.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Waiting for reference tables copy     completion.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source to target shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing the request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > Sending merge request
   > Began merge operation with id 6ffc308f-d006-466b-b24e-857242ec5f66
   > Polling request status. Press Ctrl-C to end
   > Progress: 0% | Status: Queued | Details: [Informational] Queued request
   > Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
   > Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
   > Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source to target shard.
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
   > Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
   > ...
   > ...
   > Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
   > Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing the request.
   > Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
   > 
   ```

6. Diğer veri türleri ile deneme! Tüm bu komut dosyaları, anahtar türünü belirtmenizi sağlayan isteğe bağlı -ShardKeyType parametresini alır. Varsayılan Int32'dir, ancak Int64, Guid veya İkili'yi de belirtebilirsiniz.

## <a name="create-requests"></a>İstek oluşturma

Hizmet, web UI'ı kullanarak veya isteklerinizi web rolü üzerinden iletecek SplitMerge.psm1 PowerShell modülünü alarak ve kullanarak kullanılabilir.

Hizmet, hem kırık tablolardaki hem de başvuru tablolarında verileri taşıyabilir. Parçalanmış bir tablonun kırık anahtar sütunu vardır ve her parçada farklı satır verileri vardır. Bir başvuru tablosu, her parçada aynı satır verilerini içerdiğinden kırık değildir. Başvuru tabloları sık sık değişmeyen ve sorgularda parçalanmış tablolarla BIRLEŞTIRME için kullanılan veriler için yararlıdır.

Bir birleştirme işlemi gerçekleştirmek için, taşınmış olmasını istediğiniz kırık tabloları ve başvuru tablolarını bildirmeniz gerekir. Bu **SchemaInfo** API ile gerçekleştirilir. Bu API, **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** ad alanında dır.

1. Her kırık tablo için, tablonun üst şema adını (isteğe bağlı, varsayılan olarak "dbo"), tablo adını ve bu tabloda parçalama anahtarını içeren sütun adını açıklayan bir **ShardedTableInfo** nesnesi oluşturun.
2. Her başvuru tablosu için, tablonun üst şema adını (isteğe bağlı, varsayılan olarak "dbo" olarak) ve tablo adını açıklayan bir **ReferenceTableInfo** nesnesi oluşturun.
3. Yukarıdaki TableInfo nesnelerini yeni bir **SchemaInfo nesnesine** ekleyin.
4. Bir **ShardMapManager** nesnesine başvuru alın ve **GetSchemaInfoCollection'ı**arayın.
5. **SchemaInfo'yu SchemaInfoCollection'a**ekleyin ve parça harita adını sağlar. **SchemaInfo**

Bunun bir örneği SetupSampleSplitMergeEnvironment.ps1 komut dosyasında görülebilir.

Bölme Birleştirme hizmeti sizin için hedef veritabanını (veya veritabanındaki tablolar için şema) oluşturmaz. Hizmete istek göndermeden önce önceden oluşturulmuş olmaları gerekir.

## <a name="troubleshooting"></a>Sorun giderme

Örnek powershell komut dosyalarını çalıştırırken aşağıdaki iletiyi görebilirsiniz:

   `Invoke-WebRequest : The underlying connection was closed: Could not establish trust relationship for the SSL/TLS secure channel.`

Bu hata, TLS/SSL sertifikanızın doğru yapılandırılmadığını gösterir. Lütfen 'Bir web tarayıcısına bağlanma' bölümündeki yönergeleri izleyin.

İstek gönderemiyorsanız şunları görebilirsiniz:

   `[Exception] System.Data.SqlClient.SqlException (0x80131904): Could not find stored procedure 'dbo.InsertRequest'.`

Bu durumda, yapılandırma dosyanızı, özellikle **WorkerRoleSynchronizationStorageAccountConnectionString**ayarını kontrol edin. Bu hata genellikle, alt rolün ilk kullanımda meta veri veritabanını başarıyla başolarak açamayacağını gösterir.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/allowed-services.png
[2]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/manage.png
[3]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/staging.png
[4]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/upload.png
[5]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/storage.png
