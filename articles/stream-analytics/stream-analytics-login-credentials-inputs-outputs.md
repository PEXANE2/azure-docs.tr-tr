---
title: Azure Akış Analizi işlerinde oturum açma kimlik bilgilerini döndürme
description: Bu makalede, Azure Akış Analizi işlerinde giriş ve çıktı lavabolarının kimlik bilgileri nasıl güncelleştirilen açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.custom: seodec18
ms.openlocfilehash: 3ae639dd7c5a42fc6880240988f0fb2817b09f43
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75425967"
---
# <a name="rotate-login-credentials-for-inputs-and-outputs-of-a-stream-analytics-job"></a>Akış Analizi İşi giriş ve çıkışları için giriş kimlik bilgilerini döndürme

Bir Akış Analizi işinin giriş veya çıktısı için kimlik bilgilerini yeniden oluşturduğunuzda, işi yeni kimlik bilgileriyle güncelleştirmeniz gerekir. Kimlik bilgilerini güncelleştirmeden önce işi durdurmanız gerekir, iş çalışırken kimlik bilgilerini değiştiremezsiniz. İşi durdurma ve yeniden başlatma arasındaki gecikmeyi azaltmak için Stream Analytics, bir işi son çıktısından itibaren sürdürmeyi destekler. Bu konu, oturum açma kimlik bilgilerini döndürme ve işi yeni kimlik bilgileriyle yeniden başlatma işlemini açıklar.

## <a name="regenerate-new-credentials-and-update-your-job-with-the-new-credentials"></a>Yeni kimlik bilgilerini yeniden oluşturun ve yeni kimlik bilgileriyle işinizi güncelleştirin 

Bu bölümde, Blob Depolama, Olay Hub'ları, SQL Veritabanı ve Tablo Depolama için kimlik bilgilerini yeniden oluşturma konusunda size yol bulacağız. 

### <a name="blob-storagetable-storage"></a>Blob depolama/Tablo depolama
1. Azure portalında oturum açın > Akış Analizi işi için giriş/çıktı olarak kullandığınız depolama hesabına göz atın.    
2. Ayarlar bölümünden **Erişim tuşlarını**açın. İki varsayılan anahtar (key1, key2) arasında, işiniz tarafından kullanılmayan ı seçin ve yeniden oluşturun:  
   ![Depolama hesabı için anahtarları yeniden oluşturma](media/stream-analytics-login-credentials-inputs-outputs/regenerate-storage-keys.png)
3. Yeni oluşturulan anahtarı kopyalayın.    
4. Azure portalından Stream Analytics işinize göz atın > **Dur'u** seçin ve işin durmasını bekleyin.    
5. Kimlik bilgilerini güncelleştirmek istediğiniz Blob/Table depolama girişini/çıktısını bulun.    
6. Depolama **Hesabı Anahtarı** alanını bulun ve yeni oluşturduğunuz anahtarı kaydet'e > **yapıştırın.**    
7. Değişikliklerinizi kaydettiğinizde bağlantı testi otomatik olarak başlar, bildirimler sekmesinden görüntüleyebilirsiniz. İki bildirim vardır- biri güncelleştirmeyi kaydetmeye karşılık gelir, diğeri ise bağlantıyı sınamaya karşılık gelir:  
   ![Anahtarı düzenlemeden sonra bildirimler](media/stream-analytics-login-credentials-inputs-outputs/edited-key-notifications.png)
8. Son [durdurulan zaman bölümünden işinize başlamak](#start-your-job-from-the-last-stopped-time) için devam edin.

### <a name="event-hubs"></a>Event Hubs

1. Azure portalında oturum açın > Akış Analizi işi için giriş/çıktı olarak kullandığınız Etkinlik Hub'ına göz atın.    
2. Ayarlar bölümünden **Paylaşılan erişim ilkelerini** açın ve gerekli erişim ilkesini seçin. Birincil **Anahtar** ve **İkincil Anahtar**arasında, işiniz tarafından kullanılmayan ı seçin ve yeniden oluşturun:  
   ![Olay Hub'ları için anahtarları yeniden oluşturma](media/stream-analytics-login-credentials-inputs-outputs/regenerate-event-hub-keys.png)
3. Yeni oluşturulan anahtarı kopyalayın.    
4. Azure portalından Stream Analytics işinize göz atın > **Dur'u** seçin ve işin durmasını bekleyin.    
5. Kimlik bilgilerini güncelleştirmek istediğiniz Olay hub'larını/çıktısını bulun.    
6. Olay **Hub İlkesi Anahtarı** alanını bulun ve yeni oluşturduğunuz anahtarı yapıştırın > **Kaydet'i**tıklatın.    
7. Değişikliklerinizi kaydettiğinizde bağlantı testi otomatik olarak başlar, başarılı bir şekilde geçtiğinden emin olun.    
8. Son [durdurulan zaman bölümünden işinize başlamak](#start-your-job-from-the-last-stopped-time) için devam edin.

### <a name="sql-database"></a>SQL Database

Varolan bir kullanıcının giriş kimlik bilgilerini güncelleştirmek için SQL veritabanına bağlanmanız gerekir. Kimlik bilgilerini Azure portalı veya SQL Server Management Studio gibi istemci tarafı aracını kullanarak güncelleştirebilirsiniz. Bu bölümde, Azure portalı kullanılarak kimlik bilgilerini güncelleştirme işlemi gösterilmektedir.

1. Azure portalında oturum açın > Akış Analizi işi için çıktı olarak kullandığınız SQL veritabanına göz atın.    
2. **Data**explorer'dan, veritabanınıza giriş/bağlanma > **Oturum Açma** ve **Parola** ayrıntılarınızda SQL sunucu **kimlik doğrulaması** olarak Yetkilendirme türünü seçin > Tamam'ı **seçin**>.  
   ![SQL veritabanı için kimlik bilgilerini yeniden oluşturma](media/stream-analytics-login-credentials-inputs-outputs/regenerate-sql-credentials.png)

3. Sorgu sekmesinde, aşağıdaki sorguyu çalıştırarak kullanıcınızdan birinin parolasını değiştirin `<user_name>` (kullanıcı adınız `<new_password>` ve yeni parolanızla değiştirdiğinizden emin olun):  

   ```SQL
   Alter user `<user_name>` WITH PASSWORD = '<new_password>'
   Alter role db_owner Add member `<user_name>`
   ```

4. Yeni parolayı not alın.    
5. Azure portalından Stream Analytics işinize göz atın > **Dur'u** seçin ve işin durmasını bekleyin.    
6. Kimlik bilgilerini döndürmek istediğiniz SQL veritabanı çıktısını bulun. Parolayı güncelleştirin ve değişiklikleri kaydedin.    
7. Değişikliklerinizi kaydettiğinizde bağlantı testi otomatik olarak başlar, başarılı bir şekilde geçtiğinden emin olun.    
8. Son [durdurulan zaman bölümünden işinize başlamak](#start-your-job-from-the-last-stopped-time) için devam edin.

### <a name="power-bi"></a>Power BI
1. Azure portalında oturum açın > Stream Analytics işinize göz atın > **Dur'u** seçin ve işin durmasını bekleyin.    
2. Kimlik bilgilerini yenilemek istediğiniz Power BI çıktısını bulun > **yetkilendirmeyi** yenile'yi tıklatın (bir başarı iletisi görmeniz gerekir) > Değişiklikleri **Kaydet.**    
3. Değişikliklerinizi kaydettiğinizde bağlantı testi otomatik olarak başlar, başarılı bir şekilde geçtiğinden emin olun.    
4. Son [durdurulan zaman bölümünden işinize başlamak](#start-your-job-from-the-last-stopped-time) için devam edin.

## <a name="start-your-job-from-the-last-stopped-time"></a>Son durdurulan zamandan itibaren işinizi başlatın

1. İşi başlatmak için **Başlat'ı** seçin > işin **Genel Bakış** bölmesine gidin.    
2. **En son ne zaman durdurulur** > **Başlat'ı**tıklatın. "Son durdurulduğunda" seçeneğinin yalnızca işi daha önce çalıştırdığınızda ve bazı çıktıları oluşturduysanız göründüğünü unutmayın. İş, son çıktı değerinin zamanına göre yeniden başlatılır.
   ![Akış Analizi işini başlatın](media/stream-analytics-login-credentials-inputs-outputs/start-stream-analytics-job.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
