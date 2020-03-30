---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: f84fe995e65d2b67aaaf4ff9acc4a6a44ce607dc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188411"
---
> [!NOTE]
> SharePoint RBS yapılandırması için StorSimple Adaptöründe değişiklik yaparken, Etki Alanı Yöneticileri grubuna ait bir kullanıcı hesabıyla oturum açmanız gerekir. Ayrıca, yapılandırma sayfasına Merkezi Yönetim ile aynı ana bilgisayarda çalışan bir tarayıcıdan erişmeniz gerekir.
> 
> 

#### <a name="to-configure-rbs"></a>RBS yapılandırmak için
1. SharePoint Merkezi Yönetim sayfasını açın ve **Sistem Ayarları'na**göz atın. 
2. Azure **StorSimple** bölümünde, **StorSimple Adapter'ı Yapılandır'ı**tıklatın.
   
    ![StorSimple Adaptörü yapılandırın](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS1-include.png) 
3. **StorSimple Adaptör'u Yapılandır:**
   
   1. Düzenleme yolunu **etkinleştir** onay kutusunun seçildiğinden emin olun.
   2. Metin kutusuna, BLOB deposunun Evrensel Adlandırma Sözleşmesi (UNC) yolunu yazın.
      
      > [!NOTE]
      > BLOB depo hacmi, StorSimple aygıtında yapılandırılan bir iSCSI hacminde barındırılmalıdır.

   3. Uzak depolama için yapılandırmak istediğiniz içerik veritabanlarının her birinin altındaki **Etkinleştir** düğmesini tıklatın.
      
      > [!NOTE]
      > BLOB deposu tüm web ön uç (WFE) sunucuları tarafından paylaşılmalı ve erişilebilir olmalıdır ve SharePoint sunucu çiftliği için yapılandırılan kullanıcı hesabı nın paylaşıma erişimi olmalıdır.
      
      ![RBS sağlayıcısını etkinleştirme](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS2-include.png)
      
      RBS'yi etkinleştirdiğinizde veya devre dışı bettiğinizde, aşağıdaki iletiyi de görürsünüz.
      
      ![StorSimple Adaptör'u Yapılandırın Devre Dışı](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_ConfigureStorSimpleAdapterEnableDisableMessage-include.png)

   4. Yapılandırmayı uygulamak için **Güncelleştir** düğmesini tıklatın. **Güncelleştirme** düğmesini tıklattığınızda, RbS yapılandırma durumu tüm WFE sunucularında güncelleştirilir ve tüm çiftlik RbS etkin olur. Aşağıdaki ileti görüntülenir.
      
      ![Bağdaştırıcı yapılandırma iletisi](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS3-include.png)
      
      > [!NOTE]
      > Çok sayıda veritabanı (200'den büyük) olan bir SharePoint çiftliği için RBS yapılandırıyorsanız, SharePoint Merkezi Yönetim web sayfası zaman alabilir. Bu durumda, sayfayı yenileyin. Bu yapılandırma işlemini etkilemez.

4. Yapılandırmayı doğrulayın:
   
   1. SharePoint Merkezi Yönetim web sitesinde oturum açın ve **StorSimple Adaptör'u Yapılandır' ı** sayfasına göz atın.
   2. Girdiğiniz ayarlarla eşleştiklerine emin olmak için yapılandırma ayrıntılarını denetleyin. 
5. RBS'nin doğru çalıştığından doğrulayın:
   
   1. Belgeyi SharePoint'e yükleyin. 
   2. Yapılandırdığınız UNC yoluna göz atın. RBS dizin yapısının oluşturulduğundan ve yüklenen nesneyi içerdiğinden emin olun.
6. (İsteğe bağlı) Varolan BLOB içeriğini `Migrate()` StorSimple aygıtına geçirmek için SharePoint ile birlikte verilen Microsoft RBS PowerShell cmdlet'ini kullanabilirsiniz. Daha fazla bilgi için bkz: [SharePoint 2013'te Içeriği RBS'ye][6] veya Bunların dışına geçir veya [Içeriği RBS'ye veya RbS'ye geçir (SharePoint Foundation 2010)][7].
7. (İsteğe bağlı) Test yüklemelerinde, BLOB'ların içerik veritabanından taşındığını aşağıdaki gibi doğrulayabilirsiniz: 
   
   1. SQL Management Studio'u başlatın.
   2. ListBlobsInDB_2010.sql veya ListBlobsInDB_2013.sql sorgusunu aşağıdaki gibi çalıştırın.
      
      ```
      **ListBlobsInDB_2013.sql**
      
        USE WSS_Content
        GO
      
        SELECT DocStreams.DocId,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               DocStreams.RbsId,
               TimeLastModified
      
        FROM DocStreams
      
             INNER JOIN AllDocs ON DocStreams.DocId = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      
      **ListBlobsInDB_2010.sql**
      
        USE WSS_Content
        GO
      
        SELECT AllDocStreams.Id,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               RbsId,
               TimeLastModified
        FROM AllDocStreams
      
             INNER JOIN AllDocs ON AllDocStreams.Id = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      ```
      
      RBS doğru yapılandırılmışsa, yüklenen ve RBS ile başarıyla dışsallaştırılan herhangi bir nesne için SizeOfContentInDB sütununda null değeri görünmelidir.
8. (İsteğe bağlı) RBS'yi yapılandırDıktan ve tüm BLOB içeriğini StorSimple aygıtına taşıdıktan sonra, içerik veritabanını aygıta taşıyabilirsiniz. İçerik veritabanını taşımayı seçerseniz, aygıttaki içerik veritabanı depolama alanını birincil birim olarak yapılandırmanızı öneririz. Ardından, içerik veritabanını StorSimple aygıtına geçirmek için yerleşik SQL Server en iyi uygulamalarını kullanın. 
   
   > [!NOTE]
   > İçerik veritabanının aygıta taşınması yalnızca StorSimple 8000 serisi için desteklenir (5000 veya 7000 serisi için desteklenmez).
   
   BLOB'ları ve içerik veritabanını StorSimple aygıtında ayrı birimlerde saklarsanız, bunları aynı birim kapsayıcıda yapılandırmanızı öneririz. Bu, birlikte yedekleneceklerini sağlar.
   
   > [!WARNING]
   > RbS'yi etkinleştirmediyseniz, içerik veritabanını StorSimple aygıtına taşımanızı önermiyoruz. Bu test edilmemiş bir yapılandırmadır.
   
9. Bir sonraki adıma geçin: [Çöp toplamayı yapılandırın.](#configure-garbage-collection)

[6]: https://technet.microsoft.com/library/ff628254(v=office.15).aspx
[7]: https://technet.microsoft.com/library/ff628255(v=office.14).aspx
