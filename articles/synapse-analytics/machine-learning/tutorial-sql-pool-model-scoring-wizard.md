---
title: 'Öğretici: SQL havuzları için makine öğrenme modeli Puanlama Sihirbazı'
description: SYNAPSE SQL havuzlarında verileri zenginleştirmek için Machine Learning model Puanlama Sihirbazı 'nı kullanma öğreticisi
services: synapse-analytics
ms.service: synapse-analytics
ms.subservice: machine-learning
ms.topic: tutorial
ms.reviewer: jrasnick, garye
ms.date: 09/25/2020
author: nelgson
ms.author: negust
ms.openlocfilehash: 8e92ff75bb6a9757c06de3561a385cbcbb7f75ba
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92019979"
---
# <a name="tutorial-machine-learning-model-scoring-wizard-for-synapse-sql-pools"></a>Öğretici: SYNAPSE SQL havuzları için Machine Learning model Puanlama Sihirbazı

Tahmine dayalı makine öğrenimi modelleriyle SQL havuzlarındaki verilerinizi kolayca zenginleştirme hakkında bilgi edinin.  Veri bilimcilerinin oluşturmakta olduğu modeller artık tahmine dayalı analiz için veri uzmanlarına kolayca erişebilir. SYNAPSE ' deki bir veri uzmanı, yalnızca SYNAPSE SQL havuzlarında dağıtım için Azure Machine Learning modeli kayıt defterinden bir model seçebilir ve verileri zenginleştirmek için tahminleri başlatabilir.

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> - Tahmine dayalı makine öğrenimi modelini eğitme ve modeli Azure Machine Learning model kayıt defterine kaydetme
> - SYNAPSE SQL havuzunda tahminlerini başlatmak için SQL Puanlama Sihirbazı 'nı kullanma

Azure aboneliğiniz yoksa [başlamadan önce ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

- Varsayılan depolama alanı olarak yapılandırılmış bir ADLS 2. depolama hesabıyla [SYNAPSE Analytics çalışma alanı](../get-started-create-workspace.md) . Birlikte çalıştığınız ADLS 2. FileSystem 'ın **Depolama Blobu veri katılımcısı** olması gerekir.
- SYNAPSE Analytics çalışma alanınızda SQL havuzu SYNAPSE. Ayrıntılar için bkz. [SYNAPSE SQL havuzu oluşturma](../quickstart-create-sql-pool-studio.md).
- SYNAPSE Analytics çalışma alanınızda bağlı hizmeti Azure Machine Learning. Ayrıntılar için bkz. [SYNAPSE içinde Azure Machine Learning bağlantılı hizmet oluşturma](quickstart-integrate-azure-machine-learning.md).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalda](https://portal.azure.com/) oturum açma

## <a name="train-a-model-in-azure-machine-learning"></a>Azure Machine Learning bir modeli eğitme

Başlamadan önce, **sköğren** sürümünüzün 0.20.3 olduğunu doğrulayın.

Not defterindeki tüm hücreleri çalıştırmadan önce, işlem örneğinin çalışıp çalışmadığını denetleyin.

![AML işlem doğrulama](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-train-00b.png)

1. Azure Machine Learning çalışma alanınıza gidin.

1. İndirme [tahmin NYC TAXI Tips. ipynb](https://go.microsoft.com/fwlink/?linkid=2144301)'yi indirin.

1. [Azure Machine Learning Studio](https://ml.azure.com)'da Azure Machine Learning çalışma alanını başlatın.

1. **Not defterleri** ' ne gidin ve **dosyaları karşıya yükle**' ye tıkladıktan sonra dosyayı indirdiğiniz ve karşıya yüklediğiniz "NYC Taxi Tips. ipynb" öğesini seçin.
   ![Dosya yükleme](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-train-00a.png)

1. Not defteri karşıya yüklenip açıldıktan sonra **tüm hücreleri Çalıştır**' a tıklayın.

   Hücrelerden biri başarısız olabilir ve Azure 'da kimlik doğrulaması isteyip istemediğinizi sorar. Hücre çıktılarında bunun için bir göz atın ve bağlantıyı izleyerek ve kodu girerek tarayıcınızda kimlik doğrulaması yapın. Ardından, Not defterini yeniden çalıştırın.

1. Not defteri bir ONNX modelini eğitecektir ve MLFlow 'a kaydeder. Yeni modelin düzgün şekilde kaydedilip kaydedilmediği denetlemek için **modeller** ' e gidin.
   ![Kayıt defterinde model](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-train-00c.png)

1. Not defteri çalıştırıldığında, test verileri bir CSV dosyasına da dışarı aktarılacaktır. CSV dosyasını yerel sisteminize indirin. Daha sonra, CSV dosyasını SQL Pool 'a aktarır ve verileri kullanarak modeli test edersiniz.

   CSV dosyası, Not defteri dosyanız ile aynı klasörde oluşturulur. Hemen görmüyorsanız dosya Gezgini 'nde "Yenile" ye tıklayın.

   ![CSV dosyası](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-train-00d.png)

## <a name="launch-predictions-with-sql-scoring-wizard"></a>SQL Puanlama sihirbazıyla tahminleri başlatma

1. SYNAPSE Studio ile SYNAPSE çalışma alanını açın.

1. **Veri**  ->  **bağlantılı**  ->  **depolama hesaplarına**gidin. `test_data.csv`Varsayılan depolama hesabına yükleyin.

   ![Verileri karşıya yükleme](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00a.png)

1. **Develop**  ->  **SQL betikleri**geliştirmeye gidin. SQL havuzunuza yüklemek için yeni bir SQL betiği oluşturun `test_data.csv` .

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

   ![Verileri SQL havuzuna yükleme](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00b.png)

1. **Veri**  ->  **çalışma alanına**gidin. SQL havuz tablosuna sağ tıklayarak SQL Puanlama Sihirbazı 'nı açın. **Machine Learning**  ->  **Mevcut modelle Machine Learning zenginleştirme**seçeneğini belirleyin.

   > [!NOTE]
   > Azure Machine Learning için oluşturulmuş bağlı bir hizmetiniz yoksa makine öğrenimi seçeneği görünmez (Bu öğreticinin başındaki **önkoşullara** bakın).

   ![Machine Learning seçeneği](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00c.png)

1. Açılan kutuda bağlı bir Azure Machine Learning çalışma alanı seçin. Bu, seçilen Azure Machine Learning çalışma alanının model kayıt defterinden makine öğrenimi modellerinin bir listesini yükler. Şu anda yalnızca ONNX modelleri desteklenir, bu nedenle yalnızca ONNX modellerini görüntüler.

1. Az önce eğitilen modeli seçin, sonra **devam**' a tıklayın.

   ![Azure Machine Learning modeli seçin](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00d.png)

1. Sonra, tablo sütunlarını model girişleri ile eşleyin ve model çıkışlarını belirtin. Model MLFlow biçiminde kaydedilirse ve model imzası doldurulduktan sonra, eşleme, adların benzerliğine göre bir mantık kullanarak sizin için otomatik olarak yapılır. Arabirim ayrıca el ile eşlemeyi de destekler.

   **Devam**’a tıklayın.

   ![Modellemeden tablo eşleme](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00e.png)

1. Oluşturulan T-SQL kodu, saklı bir yordamın içine sarmalanır. Bu nedenle bir saklı yordama bir ad sağlamanız gerekir. Meta veriler (sürüm, açıklama, vb.) de dahil olmak üzere model ikili dosyası Azure Machine Learning bir SQL havuz tablosuna fiziksel olarak kopyalanacak. Bu nedenle, modelin hangi tabloya kaydedileceğini belirtmeniz gerekir. "Var olan bir tabloyu kullan" seçeneğini veya "yeni tablo oluştur" seçeneğini belirleyebilirsiniz. İşiniz bittiğinde, modeli dağıtmak ve T-SQL tahmin betiği oluşturmak için **modeli dağıt + düzenleyiciyi aç** ' a tıklayın.

   ![Yordam oluştur](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00f.png)

1. Betik oluşturulduktan sonra, Puanlama yürütmek ve öngörülere ulaşmak için "Çalıştır" a tıklayın.

   ![Tahminleri Çalıştır](media/tutorial-sql-pool-model-scoring-wizard/tutorial-sql-scoring-wizard-00g.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı başlangıç: SYNAPSE 'de yeni Azure Machine Learning bağlantılı hizmet oluşturma](quickstart-integrate-azure-machine-learning.md)
- [Azure SYNAPSE Analytics 'teki Machine Learning özellikleri (çalışma alanları Önizleme)](what-is-machine-learning.md)
