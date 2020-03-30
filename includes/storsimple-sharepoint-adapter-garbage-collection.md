---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 00d292b3ba2d1b6c7c425d4c9f89188e660ac80d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73182299"
---
Bu yordamda, şunları yapacaksınız:

1. [Maintainer çalıştırılabilir çalıştırmak için hazırlayın.](#to-prepare-to-run-the-maintainer)
2. [İçerik veritabanını hazırlayın ve yetim BLOB'ların hemen silinmesi için Bin'i geri dönüştürün.](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs)
3. [Run Maintainer.exe](#to-run-the-maintainer).
4. [İçerik veritabanını ve Geri Dönüşüm Kutusu ayarlarını geri dönüştürün.](#to-revert-the-content-database-and-recycle-bin-settings)

#### <a name="to-prepare-to-run-the-maintainer"></a>Maintainer çalıştırmak için hazırlamak için
1. Web ön uç sunucusunda, SharePoint 2013 Management Shell'i yönetici olarak açın.
2. Klasör *önyükleme sürücüsüne*gidin :\Program Files\Microsoft SQL Remote Blob Depolama 10.50\Maintainer\.
3. **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** için **web.config**rename .
4. web.config dosyasının şifresini çözmek için kullanın. `aspnet_regiis -pdf connectionStrings`
5. Deşifre web.config dosyasında, düğüm `connectionStrings` altında, SQL sunucu örneğiniz ve içerik veritabanı adı için bağlantı dizesini ekleyin. Aşağıdaki örneğe bakın.
   
    `<add name="RBSMaintainerConnectionWSSContent" connectionString="Data Source=SHRPT13-SQL12\SHRPT13;Initial Catalog=WSS_Content;Integrated Security=True;Application Name=&quot;Remote Blob Storage Maintainer for WSS_Content&quot;" providerName="System.Data.SqlClient" />`
6. web.config dosyasını yeniden şifrelemek için kullanın. `aspnet_regiis –pef connectionStrings` 
7. Web.config'i Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config adresine yeniden adlandırın. 

#### <a name="to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs"></a>İçerik veritabanını ve geri dönüşüm kutusunu hemen öksüz BLOB'ları silmek için hazırlamak için
1. SQL Server'da, SQL Management Studio'da hedef içerik veritabanı için aşağıdaki güncelleştirme sorgularını çalıştırın: 
   
       `use WSS_Content`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ’time 00:00:00’`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’time 00:00:00’`
2. Web ön uç sunucusunda, **Merkezi Yönetim**altında, Geri Dönüşüm Kutusu'nu geçici olarak devre dışı kılabilir şekilde istenen içerik veritabanı için Web Uygulama Genel **Ayarları'nı** düzenleme. Bu eylem, ilgili site koleksiyonları için Geri Dönüşüm Kutusu'nu da boşaltır. Bunu yapmak için **Merkezi Yönetim** -> **Uygulama Yönetimi** -> **Web Uygulamaları (Web uygulamalarını yönet)** -> **SharePoint - 80** -> **Genel Uygulama Ayarları'nı**tıklatın. Geri **Dönüşüm Kutusu Durumunu** **KAPALI**olarak ayarlayın.
   
    ![Web Uygulama Genel Ayarları](./media/storsimple-sharepoint-adapter-garbage-collection/HCS_WebApplicationGeneralSettings-include.png)

#### <a name="to-run-the-maintainer"></a>Maintainer çalıştırmak için
* Web ön uç sunucusunda, SharePoint 2013 Management Shell'de, Maintainer'ı aşağıdaki gibi çalıştırın:
  
      `Microsoft.Data.SqlRemoteBlobs.Maintainer.exe -ConnectionStringName RBSMaintainerConnectionWSSContent -Operation GarbageCollection -GarbageCollectionPhases rdo`
  
  > [!NOTE]
  > Şu `GarbageCollection` anda StorSimple için yalnızca işlem desteklenir. Ayrıca Microsoft.Data.SqlRemoteBlobs.Maintainer.exe için verilen parametrelerin büyük/küçük harf duyarlı olduğunu unutmayın. 
  > 
  > 

#### <a name="to-revert-the-content-database-and-recycle-bin-settings"></a>İçerik veritabanını ve Geri Dönüşüm Kutusu ayarlarını geri dönüştürmek için
1. SQL Server'da, SQL Management Studio'da hedef içerik veritabanı için aşağıdaki güncelleştirme sorgularını çalıştırın:
   
      `use WSS_Content`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ‘days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘orphan_scan_period’ , ’days 30’`
2. Web ön uç sunucusunda, **Merkezi Yönetim'de,** Geri Dönüşüm Kutusu'nu yeniden etkinleştirmek için istenen içerik veritabanı için Web Uygulama Genel **Ayarları'nı** düzenleme. Bunu yapmak için **Merkezi Yönetim** -> **Uygulama Yönetimi** -> **Web Uygulamaları (Web uygulamalarını yönet)** -> **SharePoint - 80** -> **Genel Uygulama Ayarları'nı**tıklatın. Geri Dönüşüm Kutusu Durumunu **A.C.** olarak ayarlayın.

