---
title: 'Öğretici: adanmış SQL havuzları için makine öğrenme modeli Puanlama Sihirbazı'
description: Adanmış SQL havuzlarındaki verileri zenginleştirmek için Machine Learning model Puanlama Sihirbazı 'nı kullanma öğreticisi.
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 09/25/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: d8db9257ad6eed98b39cd2c9a52351f013453365
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98935237"
---
# <a name="tutorial-machine-learning-model-scoring-wizard-preview-for-dedicated-sql-pools"></a>Öğretici: adanmış SQL havuzları için makine öğrenme modeli Puanlama Sihirbazı (Önizleme)

Tahmine dayalı makine öğrenimi modelleri sayesinde verilerinizi adanmış SQL havuzlarında kolayca zenginleştirme hakkında bilgi edinin. Veri bilimcilerinin oluşturmakta olduğu modeller artık tahmine dayalı analiz için veri uzmanlarına kolayca erişebilir. Azure SYNAPSE Analytics 'teki bir veri uzmanı, Azure SYNAPSE SQL havuzları 'nda dağıtım için Azure Machine Learning modeli kayıt defterinden bir model seçebilir ve verileri zenginleştirmek için tahminleri başlatabilir.

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> - Tahmine dayalı makine öğrenimi modelini eğitme ve modeli Azure Machine Learning modeli kayıt defterine kaydetme.
> - Adanmış bir SQL havuzunda tahminleri başlatmak için SQL Puanlama Sihirbazı 'nı kullanın.

Azure aboneliğiniz yoksa [başlamadan önce ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

- Varsayılan depolama alanı olarak yapılandırılmış bir Azure Data Lake Storage 2. depolama hesabıyla [Azure SYNAPSE Analytics çalışma alanı](../get-started-create-workspace.md) . Birlikte çalıştığınız Data Lake Storage 2. dosya sisteminin *Depolama Blobu veri katılımcısı* olması gerekir.
- Azure SYNAPSE Analytics çalışma alanınızda adanmış SQL Havuzu. Ayrıntılar için bkz. [adanmış BIR SQL havuzu oluşturma](../quickstart-create-sql-pool-studio.md).
- Azure SYNAPSE Analytics çalışma alanınızda bağlı hizmeti Azure Machine Learning. Ayrıntılar için bkz. [Azure SYNAPSE 'de Azure Machine Learning bağlantılı hizmet oluşturma](quickstart-integrate-azure-machine-learning.md).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/) oturum açın.

## <a name="train-a-model-in-azure-machine-learning"></a>Azure Machine Learning bir modeli eğitme

Başlamadan önce, sköğren sürümünüzün 0.20.3 olduğunu doğrulayın.

Not defterindeki tüm hücreleri çalıştırmadan önce, işlem örneğinin çalışıp çalışmadığını denetleyin.

![Azure Machine Learning işlem doğrulaması gösteren ekran görüntüsü.](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-train-00b.png)

1. Azure Machine Learning çalışma alanınıza gidin.

1. İndirme [tahmin NYC TAXI Tips. ipynb](https://go.microsoft.com/fwlink/?linkid=2144301)'yi indirin.

1. [Azure Machine Learning Studio](https://ml.azure.com)Azure Machine Learning çalışma alanını açın.

1. **Not defterleri**  >  **dosyaları karşıya yükleme** bölümüne gidin. Ardından, indirdiğiniz **NYC TAXI Tips. ipynb** dosyasını seçin ve karşıya yükleyin.
   ![Dosya yükleme düğmesinin ekran görüntüsü.](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-train-00a.png)

1. Not defteri karşıya yüklenip açıldıktan sonra **tüm hücreleri Çalıştır**' ı seçin.

   Hücrelerden biri başarısız olabilir ve Azure 'da kimlik doğrulaması yapmanız istenir. Bunu hücre çıktılarında izleyin ve bağlantıyı izleyerek ve kodu girerek tarayıcınızda kimlik doğrulaması yapın. Ardından, Not defterini yeniden çalıştırın.

1. Not defteri bir ONNX modelini eğitecektir ve MLflow 'a kaydeder. Yeni modelin düzgün şekilde kaydedilip kaydedilmediği denetlemek için **modeller** ' e gidin.
   ![Kayıt defterindeki modeli gösteren ekran görüntüsü.](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-train-00c.png)

1. Not defteri çalıştırıldığında, test verileri bir CSV dosyasına da dışarı aktarılacaktır. CSV dosyasını yerel sisteminize indirin. Daha sonra, CSV dosyasını adanmış bir SQL havuzuna aktarır ve bu verileri kullanarak modeli test edersiniz.

   CSV dosyası, Not defteri dosyanız ile aynı klasörde oluşturulur. Hemen görmüyorsanız dosya Gezgini 'nde **Yenile** ' yi seçin.

   ![C S V dosyasını gösteren ekran görüntüsü.](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-train-00d.png)

## <a name="launch-predictions-with-the-sql-scoring-wizard"></a>SQL Puanlama sihirbazıyla tahminleri başlatma

1. SYNAPSE Studio ile Azure SYNAPSE çalışma alanını açın.

1. **Veri**  >  **bağlantılı**  >  **depolama hesaplarına** gidin. `test_data.csv`Varsayılan depolama hesabına yükleyin.

   ![Karşıya veri yükleme seçimlerini gösteren ekran görüntüsü.](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00a.png)

1.   >  **SQL betikleri** geliştirmeye gidin. Adanmış SQL havuzunuza yüklemek için yeni bir SQL betiği oluşturun `test_data.csv` .

   > [!NOTE]
   > Bu betikteki dosya URL 'sini çalıştırmadan önce güncelleştirin.

   ```SQL
   IF NOT EXISTS (SELECT * FROM sys.objects WHERE NAME = 'nyc_taxi' AND TYPE = 'U')
   CREATE TABLE dbo.nyc_taxi
   (
       tipped int,
       fareAmount float,
       paymentType int,
       passengerCount int,
       tripDistance float,
       tripTimeSecs bigint,
       pickupTimeBin nvarchar(30)
   )
   WITH
   (
       DISTRIBUTION = ROUND_ROBIN,
       CLUSTERED COLUMNSTORE INDEX
   )
   GO
   
   COPY INTO dbo.nyc_taxi
   (tipped 1, fareAmount 2, paymentType 3, passengerCount 4, tripDistance 5, tripTimeSecs 6, pickupTimeBin 7)
   FROM '<URL to linked storage account>/test_data.csv'
   WITH
   (
       FILE_TYPE = 'CSV',
       ROWTERMINATOR='0x0A',
       FIELDQUOTE = '"',
       FIELDTERMINATOR = ',',
       FIRSTROW = 2
   )
   GO
   
   SELECT TOP 100 * FROM nyc_taxi
   GO
   ```

   ![Verileri adanmış SQL havuzuna yükleme](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00b.png)

1. **Veri**  >  **çalışma alanına** gidin. Adanmış SQL havuzu tablosuna sağ tıklayarak SQL Puanlama Sihirbazı 'nı açın.   >  **Mevcut modelle Machine Learning zenginleştirme** seçeneğini belirleyin.

   > [!NOTE]
   > Azure Machine Learning için oluşturulmuş bağlı bir hizmetiniz yoksa makine öğrenimi seçeneği görünmez. (Bu öğreticinin başındaki [önkoşullara](#prerequisites) bakın.)

   ![Machine Learning seçeneğini gösteren ekran görüntüsü.](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00c.png)

1. Açılan kutuda bağlı bir Azure Machine Learning çalışma alanı seçin. Bu adım, seçili Azure Machine Learning çalışma alanının model kayıt defterinden makine öğrenimi modellerinin bir listesini yükler. Şu anda yalnızca ONNX modelleri desteklenir, bu nedenle bu adım yalnızca ONNX modellerini görüntüler.

1. Az önce eğitilen modeli seçin ve ardından **devam**' ı seçin.

   ![Azure Machine Learning modelini seçen ekran görüntüsü.](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00d.png)

1. Tablo sütunlarını model girişleri ile eşleyin ve model çıkışlarını belirtin. Model MLflow biçiminde kaydedilirse ve model imzası doldurulduktan sonra, eşleme, adların benzerliğine göre bir mantık kullanılarak sizin için otomatik olarak yapılır. Arabirim ayrıca el ile eşlemeyi de destekler.

   **Devam**’ı seçin.

   ![Tablo-model eşlemeyi gösteren ekran görüntüsü.](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00e.png)

1. Oluşturulan T-SQL kodu, saklı bir yordamın içine sarmalanır. Bu nedenle, bir saklı yordam adı sağlamanız gerekir. Meta veriler (sürüm, açıklama ve diğer bilgiler) de dahil olmak üzere model ikili dosyası, fiziksel olarak Azure Machine Learning ' den adanmış bir SQL havuzu tablosuna kopyalanacaktır. Bu nedenle, modelin hangi tabloya kaydedileceğini belirtmeniz gerekir. 

   **Mevcut tablo** veya **Yeni oluştur** seçeneklerinden birini belirleyebilirsiniz. İşiniz bittiğinde modeli dağıtmak ve T-SQL tahmin betiği oluşturmak için **modeli dağıt + betiği aç** ' ı seçin.

   ![Saklı yordam oluşturma seçimlerini gösteren ekran görüntüsü.](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00f.png)

1. Betik oluşturulduktan sonra, Puanlama yürütmek ve tahminleri almak için **Çalıştır** ' ı seçin.

   ![Puanlama ve tahminleri gösteren ekran görüntüsü.](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00g.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı başlangıç: Azure 'da yeni Azure Machine Learning bağlantılı hizmet oluşturma SYNAPSE](quickstart-integrate-azure-machine-learning.md)
- [Azure Synapse Analytics'te Makine Öğrenmesi özellikleri](what-is-machine-learning.md)
