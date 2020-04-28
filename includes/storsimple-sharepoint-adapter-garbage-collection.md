---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 00d292b3ba2d1b6c7c425d4c9f89188e660ac80d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "73182299"
---
Bu yordamda şunları yapmanız gerekir:

1. [Bakımcı yürütülebilirini çalıştırmaya hazırlanın](#to-prepare-to-run-the-maintainer) .
2. [Yalnız bırakılmış Blobların hemen silinmesi için içerik veritabanını ve geri dönüşüm kutusu 'Nu hazırlayın](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).
3. [Bakımcı. exe](#to-run-the-maintainer)' yi çalıştırın.
4. [İçerik veritabanını ve geri dönüşüm kutusu ayarlarını geri](#to-revert-the-content-database-and-recycle-bin-settings)alın.

#### <a name="to-prepare-to-run-the-maintainer"></a>Bakımcı çalıştırmaya hazırlanmak için
1. Web ön uç sunucusunda SharePoint 2013 Yönetim Kabuğu 'nu yönetici olarak açın.
2. Klasör *önyükleme sürücüsüne*gidin: \Program Files\Microsoft SQL Remote blob Storage 10.50 olan \ bakımcı\.
3. **Microsoft. Data. Sqlremotebloblar. bakımcı. exe. config** dosyasını **Web. config**olarak yeniden adlandırın.
4. Web `aspnet_regiis -pdf connectionStrings` . config dosyasının şifresini çözmek için kullanın.
5. Şifresi çözülmüş Web. config dosyasında, `connectionStrings` düğümü altına SQL Server Örneğiniz için bağlantı dizesini ve içerik veritabanı adını ekleyin. Aşağıdaki örneğe bakın.
   
    `<add name="RBSMaintainerConnectionWSSContent" connectionString="Data Source=SHRPT13-SQL12\SHRPT13;Initial Catalog=WSS_Content;Integrated Security=True;Application Name=&quot;Remote Blob Storage Maintainer for WSS_Content&quot;" providerName="System.Data.SqlClient" />`
6. Web `aspnet_regiis –pef connectionStrings` . config dosyasını yeniden şifrelemek için kullanın. 
7. Web. config dosyasını Microsoft. Data. Sqlremotebloblar. bakımcı. exe. config olarak yeniden adlandırın. 

#### <a name="to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs"></a>Yalnız bırakılmış Blobları hemen silmek için içerik veritabanını ve geri dönüşüm kutusunu hazırlamak için
1. SQL Server, SQL Management Studio ' de hedef içerik veritabanı için aşağıdaki güncelleştirme sorgularını çalıştırın: 
   
       `use WSS_Content`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ’time 00:00:00’`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’time 00:00:00’`
2. Web ön uç sunucusunda, **Merkezi Yönetim**altında, geri dönüşüm kutusu 'nu geçici olarak devre dışı bırakmak için, istenen Içerik veritabanının **Web uygulaması genel ayarlarını** düzenleyin. Bu eylem, ilgili site koleksiyonları için geri dönüşüm kutusu 'nu da boşaltacaktır. Bunu yapmak için **Merkezi Yönetim** -> **uygulama yönetimi** -> **Web uygulamaları (Web uygulamalarını yönet)** -> **SharePoint-80** -> **genel uygulama ayarları**' na tıklayın. **Geri dönüşüm kutusu durumunu** **kapalı**olarak ayarlayın.
   
    ![Web uygulaması genel ayarları](./media/storsimple-sharepoint-adapter-garbage-collection/HCS_WebApplicationGeneralSettings-include.png)

#### <a name="to-run-the-maintainer"></a>Bakımcı çalıştırmak için
* Web ön uç sunucusunda SharePoint 2013 Yönetim Kabuğu 'nda, bakımcı 'yı aşağıdaki gibi çalıştırın:
  
      `Microsoft.Data.SqlRemoteBlobs.Maintainer.exe -ConnectionStringName RBSMaintainerConnectionWSSContent -Operation GarbageCollection -GarbageCollectionPhases rdo`
  
  > [!NOTE]
  > Şu anda `GarbageCollection` StorSimple için yalnızca işlem desteklenir. Ayrıca, Microsoft. Data. Sqlremotebloblar. bakımcı. exe için verilen parametrelerin büyük/küçük harfe duyarlı olduğunu unutmayın. 
  > 
  > 

#### <a name="to-revert-the-content-database-and-recycle-bin-settings"></a>İçerik veritabanını ve geri dönüşüm kutusu ayarlarını geri almak için
1. SQL Server, SQL Management Studio ' de hedef içerik veritabanı için aşağıdaki güncelleştirme sorgularını çalıştırın:
   
      `use WSS_Content`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ‘days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘orphan_scan_period’ , ’days 30’`
2. Web ön uç sunucusunda, **Merkezi Yönetim**bölümünde, geri dönüşüm kutusu 'nu yeniden etkinleştirmek için istenen Içerik veritabanının **Web uygulaması genel ayarlarını** düzenleyin. Bunu yapmak için **Merkezi Yönetim** -> **uygulama yönetimi** -> **Web uygulamaları (Web uygulamalarını yönet)** -> **SharePoint-80** -> **genel uygulama ayarları**' na tıklayın. Geri dönüşüm kutusu durumunu **Açık**olarak ayarlayın.

