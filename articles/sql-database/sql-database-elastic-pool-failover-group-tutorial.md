---
title: 'Öğretici: Yük devretme grubuna Azure SQL veritabanı elastik havuzu ekleme | Microsoft Docs'
description: Azure portal, PowerShell veya Azure CLı kullanarak bir yük devretme grubuna Azure SQL veritabanı elastik havuzu ekleme.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: sstein, carlrab
ms.date: 06/19/2019
ms.openlocfilehash: 2d46e6f1d5c7079ab5bbfea39a85ea0a7592afc8
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70099252"
---
# <a name="tutorial-add-an-azure-sql-database-elastic-pool-to-a-failover-group"></a>Öğretici: Yük devretme grubuna Azure SQL veritabanı elastik havuzu ekleme

Azure SQL veritabanı elastik havuzu için bir yük devretme grubu yapılandırın ve Azure portal kullanarak yük devretme testi yapın.  Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> - Azure SQL veritabanı tek veritabanı oluşturun.
> - Tek veritabanını elastik bir havuza ekleyin. 
> - İki mantıksal SQL Server arasında esnek havuz için bir [Yük devretme grubu](sql-database-auto-failover-group.md) oluşturun.
> - Yük devretme testi.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için şunlar sahip olduğunuzdan emin olun: 

- Azure aboneliği. Henüz bir [hesabınız yoksa ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .


## <a name="1---create-a-single-database"></a>1-tek veritabanı oluşturma 

[!INCLUDE [sql-database-create-single-database](includes/sql-database-create-single-database.md)]

## <a name="2---add-single-database-to-elastic-pool"></a>2-elastik havuza tek veritabanı ekleme

1. Azure portal sol taraftaki menüden **Azure SQL** ' i seçin. **Azure SQL** listede yoksa, **tüm hizmetler**' i seçin ve arama kutusuna Azure SQL yazın. Seçim **Azure SQL** ' in yanındaki yıldızı seçerek bunu sık kullanılanlara ekleyin ve sol gezinti bölmesinde bir öğe olarak ekleyin. 
1. **+ Ekle** ' yı seçerek **SQL dağıtım seçeneğini seçin** sayfasını açın. Veritabanları kutucuğunda ayrıntıları göster ' i seçerek farklı veritabanları hakkındaki ek bilgileri görüntüleyebilirsiniz.
1. **SQL veritabanları** kutucuğunda **kaynak türü** açılır listesinden **elastik havuz** ' ı seçin. Elastik havuzunuzu oluşturmak için **Oluştur** ' u seçin. 

    ![Elastik havuz Seç](media/sql-database-elastic-pool-failover-group-tutorial/select-azure-sql-elastic-pool.png)

1. Elastik havuzunuzu aşağıdaki değerlerle yapılandırın:
   - **Ad**: Elastik havuzunuz `myElasticPool`için gibi benzersiz bir ad sağlayın. 
   - **Abonelik**: Açılan listeden aboneliğinizi seçin.
   - **ResourceGroup**: Bölüm `myResourceGroup` 1 ' de oluşturduğunuz kaynak grubundan açılan listeden seçim yapın. 
   - **Sunucu**: Açılan listeden, Bölüm 1 ' de oluşturduğunuz sunucuyu seçin.  

       ![Elastik havuz için yeni sunucu oluştur](media/sql-database-elastic-pool-failover-group-tutorial/use-existing-server-for-elastic-pool.png)

   - **İşlem + depolama**: İşlem, depolama alanınızı yapılandırmak ve esnek havuzunuza tek veritabanınızı eklemek için **elastik havuzu Yapılandır** ' ı seçin. **Havuz ayarları** sekmesinde, 2 sanal çekirdek ve 32 Ile varsayılan 5. nesil bırakın. 

1. **Yapılandır** sayfasında **veritabanları** sekmesini seçin ve ardından **veritabanı Ekle**' yi seçin. Bölüm 1 ' de oluşturduğunuz veritabanını seçin ve ardından, elastik havuzunuza eklemek için **Uygula** ' yı seçin. Esnek havuz ayarlarınızı uygulamak ve **Yapılandır** sayfasını kapatmak Için yeniden **Uygula** ' yı seçin. 

    ![Elastik havuza SQL DB ekleme](media/sql-database-elastic-pool-failover-group-tutorial/add-database-to-elastic-pool.png)

1. Elastik havuz ayarlarınızı gözden geçirmek için **gözden geçir + oluştur** seçeneğini belirleyin ve ardından esnek havuzunuzu oluşturmak için **Oluştur** ' u seçin 


## <a name="3---create-the-failover-group"></a>3-yük devretme grubu oluşturma 
Bu adımda, mevcut bir Azure SQL sunucusu ile başka bir bölgedeki yeni bir Azure SQL sunucusu arasında bir [Yük devretme grubu](sql-database-auto-failover-group.md) oluşturacaksınız. Ardından, elastik havuzu yük devretme grubuna ekleyin. 

1. [Azure Portal](https://portal.azure.com)sol taraftaki menüden **Azure SQL** ' i seçin. **Azure SQL** listede yoksa, **tüm hizmetler**' i seçin ve arama kutusuna Azure SQL yazın. Seçim **Azure SQL** ' in yanındaki yıldızı seçerek bunu sık kullanılanlara ekleyin ve sol gezinti bölmesinde bir öğe olarak ekleyin. 
1. Önceki bölümde oluşturulan elastik havuzu seçin (örneğin `myElasticPool`,). 
1. **Genel bakış** bölmesinde sunucu **adı** bölümünde sunucu adı ' nı seçerek sunucu ayarlarını açın.
  
    ![Elastik havuz için sunucu açma](media/sql-database-elastic-pool-failover-group-tutorial/server-for-elastic-pool.png)

1. **Ayarlar** bölmesinde **Yük devretme grupları** ' nı seçin ve sonra yeni bir yük devretme grubu oluşturmak için **Grup Ekle** ' yi seçin. 

    ![Yeni Yük devretme grubu Ekle](media/sql-database-elastic-pool-failover-group-tutorial/elastic-pool-failover-group.png)

1. **Yük devretme grubu** sayfasında, aşağıdaki değerleri girin veya seçin ve ardından **Oluştur**' u seçin:
    - **Yük devretme grubu adı**: Benzersiz bir yük devretme grubu adı (örneğin `failovergrouptutorial`,) yazın. 
    - **İkincil sunucu**: *Gerekli ayarları yapılandırma* seçeneğini belirleyin ve ardından **Yeni bir sunucu oluşturmayı**seçin. Alternatif olarak, zaten var olan bir sunucuyu ikincil sunucu olarak seçebilirsiniz. Yeni ikincil sunucunuz için aşağıdaki değerleri girdikten sonra **Seç**' i seçin. 
        - **Sunucu adı**: İkincil sunucu için, `mysqlsecondary`gibi benzersiz bir ad yazın. 
        - **Sunucu Yöneticisi oturum açma**: Türüyle`azureuser`
        - **Parola**: Parola gereksinimlerini karşılayan karmaşık bir parola yazın.
        - **Konum**: Açılan kutudan, `East US`gibi bir konum seçin. Bu konum, birincil sunucunuz ile aynı konumda olamaz.

       > [!NOTE]
       > Sunucu oturum açma ve güvenlik duvarı ayarları, birincil sunucunuzun bilgileriyle eşleşmelidir. 
    
       ![Yük devretme grubu için ikincil sunucu oluşturma](media/sql-database-elastic-pool-failover-group-tutorial/create-secondary-failover-server.png)

1. **Grup Içindeki veritabanları** ' nı seçin ve ardından Bölüm 2 ' de oluşturduğunuz elastik havuzu seçin. İkincil sunucuda bir elastik havuz oluşturmanızı isteyen bir uyarı görüntülenmelidir. Uyarıyı seçin ve ardından ikincil sunucuda elastik havuzu oluşturmak için **Tamam** ' ı seçin. 
        
    ![Esnek havuzu yük devretme grubuna ekle](media/sql-database-elastic-pool-failover-group-tutorial/add-elastic-pool-to-failover-group.png)
        
1. Elastik havuz ayarlarınızı yük devretme grubuna uygulamak için **Seç** ' i seçin ve ardından yük devretme grubunuzu oluşturmak için **Oluştur** ' u seçin. Elastik havuzun yük devretme grubuna eklenmesi, coğrafi çoğaltma işlemini otomatik olarak başlatır. 


## <a name="4---test-failover"></a>4-yük devretme testi 
Bu adımda, yük devretme grubunuzu ikincil sunucuya devreder ve sonra Azure portal kullanarak yeniden başarısız olursunuz. 

1. [Azure Portal](https://portal.azure.com)sol taraftaki menüden **Azure SQL** ' i seçin. **Azure SQL** listede yoksa, **tüm hizmetler**' i seçin ve arama kutusuna Azure SQL yazın. Seçim **Azure SQL** ' in yanındaki yıldızı seçerek bunu sık kullanılanlara ekleyin ve sol gezinti bölmesinde bir öğe olarak ekleyin. 
1. Önceki bölümde oluşturulan elastik havuzu seçin (örneğin `myElasticPool`,). 
1. Sunucu ayarlarını açmak için sunucu **adı** altında sunucunun adını seçin.

    ![Elastik havuz için sunucu açma](media/sql-database-elastic-pool-failover-group-tutorial/server-for-elastic-pool.png)

1. **Ayarlar** bölmesinde **Yük devretme grupları** ' nı seçin ve ardından Bölüm 2 ' de oluşturduğunuz yük devretme grubunu seçin. 
  
   ![Portaldan yük devretme grubunu seçin](media/sql-database-elastic-pool-failover-group-tutorial/select-failover-group.png)

1. Hangi sunucunun birincil olduğunu ve hangi sunucunun ikincil olduğunu gözden geçirin. 
1. Elastik havuzunuzu içeren yük devretme grubunuzun yükünü devretmek için görev bölmesinden **Yük devretmeyi** seçin. 
1. TDS oturumlarının kesileceğini bildiren uyarıda **Evet** ' i seçin. 

   ![SQL veritabanınızı içeren yük devretme grubunuzun yükünü devreder](media/sql-database-elastic-pool-failover-group-tutorial/failover-sql-db.png)

1. Hangi sunucunun birincil olduğunu ve hangi sunucunun ikincil olduğunu gözden geçirin. Yük devretme başarılı olursa iki sunucu, bulunan rolleri değiştirmiş olmalıdır. 
1. Yük devretme grubundan özgün ayarlara geri dönmek için **Yük devretmeyi** yeniden seçin. 

## <a name="clean-up-resources"></a>Kaynakları temizleme 
Kaynak grubunu silerek kaynakları temizleyin. 

1. [Azure Portal](https://portal.azure.com)kaynak grubunuza gidin.
1. Gruptaki tüm kaynakların yanı sıra kaynak grubunun kendisini silmek için **kaynak grubunu sil** ' i seçin. 
1. Kaynak grubunun adını `myResourceGroup`, metin kutusuna yazın ve ardından **Sil** ' i seçerek kaynak grubunu silin.  


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir yük devretme grubuna Azure SQL veritabanı tek veritabanı eklediniz ve yük devretme işlemi test edildi. Şunları öğrendiniz:

> [!div class="checklist"]
> - Azure SQL veritabanı tek veritabanı oluşturun.
> - Tek veritabanını elastik bir havuza ekleyin. 
> - İki mantıksal SQL Server arasında esnek havuz için bir [Yük devretme grubu](sql-database-auto-failover-group.md) oluşturun.
> - Yük devretme testi.

DMS kullanarak geçiş yapmak için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Öğretici: DMS kullanarak SQL Server havuza alınmış bir veritabanına geçirme](../dms/tutorial-sql-server-to-azure-sql.md?toc=/azure/sql-database/toc.json)
