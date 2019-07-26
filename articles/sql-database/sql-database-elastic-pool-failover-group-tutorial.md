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
manager: jroth
ms.date: 06/19/2019
ms.openlocfilehash: a9c4fb43717788703184522109301e88d4c94f0e
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444818"
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

1. [Azure Portal](https://portal.azure.com)sol üst köşesinde **kaynak oluştur ' a** tıklayın.
1. Arama `elastic pool` kutusuna yazın, ENTER tuşuna basın, **SQL elastik veritabanı havuzu** simgesini seçin ve ardından **Oluştur**' u seçin. 

    ![Market 'ten elastik havuz seçin](media/sql-database-elastic-pool-create-failover-group-tutorial/elastic-pool-market-place.png)

1. Elastik havuzunuzu aşağıdaki değerlerle yapılandırın:
   - **Ad**: Elastik havuzunuz `myElasticPool`için gibi benzersiz bir ad sağlayın. 
   - **Abonelik**: Açılan listeden aboneliğinizi seçin.
   - **ResourceGroup**: Bölüm `myResourceGroup` 1 ' de oluşturduğunuz kaynak grubundan açılan listeden seçim yapın. 
   - **Sunucu**: Açılan listeden, Bölüm 1 ' de oluşturduğunuz sunucuyu seçin.  

       ![Elastik havuz için yeni sunucu oluştur](media/sql-database-elastic-pool-create-failover-group-tutorial/use-existing-server-for-elastic-pool.png)

   - **İşlem + depolama**: İşlem, depolama alanınızı yapılandırmak ve esnek havuzunuza tek veritabanınızı eklemek için **elastik havuzu Yapılandır** ' ı seçin. **Havuz ayarları** sekmesinde, 2 sanal çekirdek ve 32 Ile varsayılan 5. nesil bırakın. 

1. **Yapılandır** sayfasında **veritabanları** sekmesini seçin ve ardından **veritabanı Ekle**' yi seçin. Bölüm 1 ' de oluşturduğunuz veritabanını seçin ve ardından, elastik havuzunuza eklemek için **Uygula** ' yı seçin. Esnek havuz ayarlarınızı uygulamak ve **Yapılandır** sayfasını kapatmak Için yeniden **Uygula** ' yı seçin. 

    ![Elastik havuza SQL DB ekleme](media/sql-database-elastic-pool-create-failover-group-tutorial/add-database-to-elastic-pool.png)

1. Elastik havuz ayarlarınızı gözden geçirmek için **gözden geçir + oluştur** seçeneğini belirleyin ve ardından esnek havuzunuzu oluşturmak için **Oluştur** ' u seçin 


## <a name="3---create-the-failover-group"></a>3-yük devretme grubu oluşturma 
Bu adımda, mevcut bir Azure SQL sunucusu ile başka bir bölgedeki yeni bir Azure SQL sunucusu arasında bir [Yük devretme grubu](sql-database-auto-failover-group.md) oluşturacaksınız. Ardından, elastik havuzu yük devretme grubuna ekleyin. 


1. [Azure Portal](https://portal.azure.com)sol üst köşesindeki **tüm hizmetler** ' i seçin. 
1. Arama `sql servers` kutusuna yazın. 
1. Seçim SQL Server ' ın yanındaki yıldız simgesini seçerek SQL Server **'lar** ' ı seçin ve sol taraftaki Gezinti bölmenizi ekleyin. 
    
    ![SQL sunucularını bulma](media/sql-database-single-database-create-failover-group-tutorial/all-services-sql-servers.png)

1. **SQL Server** ' ı seçin ve Bölüm 1 ' de oluşturduğunuz sunucuyu seçin.
1. **Ayarlar** bölmesinde **Yük devretme grupları** ' nı seçin ve sonra yeni bir yük devretme grubu oluşturmak için **Grup Ekle** ' yi seçin. 

    ![Yeni Yük devretme grubu Ekle](media/sql-database-elastic-pool-create-failover-group-tutorial/add-elastic-pool-to-failover-group.png)

1. **Yük devretme grubu** sayfasında, aşağıdaki değerleri girin veya seçin ve ardından **Oluştur**' u seçin:
    - **Yük devretme grubu adı**: Benzersiz bir yük devretme grubu adı (örneğin `failovergrouptutorial`,) yazın. 
    - **İkincil sunucu**: *Gerekli ayarları yapılandırma* seçeneğini belirleyin ve ardından **Yeni bir sunucu oluşturmayı**seçin. Alternatif olarak, zaten var olan bir sunucuyu ikincil sunucu olarak seçebilirsiniz. Aşağıdaki değerleri girdikten sonra **Seç**' i seçin. 
        - **Sunucu adı**: İkincil sunucu için, `mysqlsecondary`gibi benzersiz bir ad yazın. 
        - **Sunucu Yöneticisi oturum açma**: Türüyle`azureuser`
        - **Parola**: Parola gereksinimlerini karşılayan karmaşık bir parola yazın.
        - **Konum**: Doğu ABD 2 gibi açılan kutudan bir konum seçin. Bu konum, birincil sunucunuz ile aynı konumda olamaz.

       > [!NOTE]
       > Sunucu oturum açma ve güvenlik duvarı ayarları, birincil sunucunuzun bilgileriyle eşleşmelidir. 
    
       ![Yük devretme grubu için ikincil sunucu oluşturma](media/sql-database-single-database-create-failover-group-tutorial/create-secondary-failover-server.png)

1. İkincil bir sunucu seçildikten sonra, Grup seçeneği **Içindeki veritabanlarının** kilidi açılır. **Eklenecek veritabanlarını seçmek** için seçin ve ardından Bölüm 2 ' de oluşturduğunuz elastik havuzu seçin. İkincil sunucuda bir elastik havuz oluşturmanızı isteyen bir uyarı görüntülenmelidir. Uyarıyı seçin ve ardından ikincil sunucuda elastik havuzu oluşturmak için **Tamam** ' ı seçin. 
        
    ![SQL DB 'yi yük devretme grubuna ekle](media/sql-database-single-database-create-failover-group-tutorial/add-sqldb-to-failover-group.png)
        
1. Elastik havuz ayarlarınızı yük devretme grubuna uygulamak için **Seç** ' i seçin ve ardından yük devretme grubunuzu oluşturmak için **Oluştur** ' u seçin. Elastik havuzun yük devretme grubuna eklenmesi, coğrafi çoğaltma işlemini otomatik olarak başlatır. 


## <a name="4---test-failover"></a>4-yük devretme testi 
Bu adımda, yük devretme grubunuzu ikincil sunucuya devreder ve sonra Azure portal kullanarak yeniden başarısız olursunuz. 

1. [Azure Portal](https://portal.azure.com)içindeki **SQL** Server sunucunuza gidin. 
1. **Ayarlar** bölmesinde **Yük devretme grupları** ' nı seçin ve ardından Bölüm 2 ' de oluşturduğunuz yük devretme grubunu seçin. 
  
   ![Portaldan yük devretme grubunu seçin](media/sql-database-single-database-create-failover-group-tutorial/select-failover-group.png)

1. Hangi sunucunun birincil olduğunu ve hangi sunucunun ikincil olduğunu gözden geçirin. 
1. Elastik havuzunuzu içeren yük devretme grubunuzun yükünü devretmek için görev bölmesinden **Yük devretmeyi** seçin. 
1. TDS oturumlarının kesileceğini bildiren uyarıda **Evet** ' i seçin. 

   ![SQL veritabanınızı içeren yük devretme grubunuzun yükünü devreder](media/sql-database-single-database-create-failover-group-tutorial/failover-sql-db.png)

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
