---
title: Apache Spark için Scala & Java kitaplıklarını yönetme
description: Azure SYNAPSE Analytics 'te Scala ve Java kitaplıklarını ekleme ve yönetme hakkında bilgi edinin.
services: synapse-analytics
author: midesa
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 02/26/2020
ms.author: midesa
ms.reviewer: jrasnick
ms.subservice: spark
ms.openlocfilehash: c70ecc4fc5469d728bc12d47024585ccf00ff98e
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102098715"
---
# <a name="manage-scala-and-java-packages-for-apache-spark-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te Apache Spark için Scala ve Java paketlerini yönetme

Kitaplıklar, programlarınıza veya projelerinize dahil etmek isteyebileceğiniz yeniden kullanılabilir kod sağlar. 

Birkaç nedenden dolayı sunucusuz Apache Spark havuz ortamınızı güncelleştirmeniz gerekebilir. Örneğin, şunları görebilirsiniz:
- temel bağımlılıklarından biri yeni bir sürüm yayımlamıştır.
- Machine Learning modelinize eğitim veya verilerinizi hazırlama hakkında ek bir pakete ihtiyacınız vardır.
- daha iyi bir paket bulduk ve artık eski pakete ihtiyacınız yoktur.

Üçüncü tarafa veya yerel olarak oluşturulmuş bir kodu uygulamalarınız için kullanılabilir hale getirmek için, bir kitaplığı sunucusuz Apache Spark havuzlarınızdan veya Not defteri oturumlarından birine yükleyebilirsiniz. Bu makalede, Scala ve Java paketlerini nasıl yönetebileceğinizi ele göndereceğiz.

## <a name="default-installation"></a>Varsayılan yükleme
Azure SYNAPSE Analytics 'teki Apache Spark, ortak veri Mühendisliği, veri hazırlama, makine öğrenimi ve veri görselleştirme görevlerine yönelik eksiksiz bir kitaplık kümesine sahiptir. Tam kitaplıklar listesi, [Apache Spark sürüm desteği](apache-spark-version-support.md)' nde bulunabilir. 

Bir Spark örneği başlatıldığında, bu kitaplıklar otomatik olarak dahil edilir. Ek Scala/Java paketleri Spark havuzuna ve oturum düzeyine eklenebilir.

## <a name="workspace-packages"></a>Çalışma alanı paketleri
Çalışma alanı paketleri özel veya özel jar dosyaları olabilir. Bu paketleri çalışma alanınıza yükleyebilir ve daha sonra belirli bir Spark havuzuna atayabilirsiniz.

Çalışma alanı paketleri eklemek için:
1.   >  **Çalışma alanı paketlerini** Yönet sekmesine gidin.
2. Dosya seçiciyi kullanarak jar dosyalarınızı karşıya yükleyin.
3. Dosyalar Azure SYNAPSE çalışma alanına yüklendikten sonra, bu jar dosyalarını belirli bir Apache Spark havuzuna ekleyebilirsiniz.

![Çalışma alanı paketlerini vurgulayan ekran görüntüsü.](./media/apache-spark-azure-portal-add-libraries/studio-add-workspace-package.png "Çalışma alanı paketlerini görüntüle")

## <a name="pool-libraries"></a>Havuz kitaplıkları
Spark uygulamanız için kullanmak istediğiniz Scala ve Java paketlerini tanımladıktan sonra bunları bir Spark havuzuna yükleyebilirsiniz. Havuz düzeyi kitaplıklar, havuzda çalışan tüm not defterleri ve işler için kullanılabilir.

Spark havuz kitaplıklarını Azure SYNAPSE Studio veya Azure portal giderek güncelleştirebilirsiniz. Burada, yüklenecek çalışma alanı kitaplıklarını seçebilirsiniz. 

Değişiklikler kaydedildikten sonra, bir Spark işi yüklemeyi çalıştırır ve elde edilen ortamı daha sonra yeniden kullanmak üzere önbelleğe alınır. İş tamamlandıktan sonra, yeni Spark işleri veya Not defteri oturumları güncelleştirilmiş havuz kitaplıklarını kullanacaktır. 

> [!IMPORTANT]
> - Yüklemekte olduğunuz Paket büyükse veya yüklemesi uzun sürüyorsa, bu, Spark örneği başlangıç süresini etkiler.
> - PySpark, Python, Scala/Java, .NET veya Spark sürümünün değiştirilmesi desteklenmez.

#### <a name="manage-packages-from-azure-synapse-studio-or-azure-portal"></a>Paketleri Azure SYNAPSE Studio veya Azure portal yönetme
Spark havuzu kitaplıkları Azure SYNAPSE Studio veya Azure portal yönetilebilir. 

Bir Spark havuzuna kitaplıkları güncelleştirmek veya eklemek için:
1. Azure portal Azure SYNAPSE Analytics çalışma alanınıza gidin.

    **Azure Portal** güncelleştiriyorsanız:

    - **SYNAPSE kaynakları** bölümünde, **Apache Spark havuzları** sekmesini seçin ve listeden bir Spark havuzu seçin.
     
    - Spark havuzunun **Ayarlar** bölümünden **paketleri** seçin.
  
    ![Ortam yapılandırma dosyasını karşıya yükle düğmesini vurgulayan ekran görüntüsü.](./media/apache-spark-azure-portal-add-libraries/apache-spark-add-library-azure.png "Python kitaplıklarını ekleme")
   
    **SYNAPSE Studio**'dan güncelleştiriyorsanız:
    - Ana Gezinti panelinden **Yönet** ' i seçin ve ardından **Apache Spark havuzları**' nı seçin.

    - Belirli bir Spark havuzu için **paketler** bölümünü seçin.
    ![Studio 'dan karşıya yükleme ortamı yapılandırma seçeneğini vurgulayan ekran görüntüsü.](./media/apache-spark-azure-portal-add-libraries/studio-update-libraries.png "Studio 'dan Python kitaplıklarını ekleme")
   
2. Jar dosyalarını eklemek için, havuzunuza eklemek üzere **çalışma alanı paketleri** bölümüne gidin. 
3. Değişikliklerinizi kaydettikten sonra, belirtilen kitaplıkları yüklemek ve önbelleğe almak için bir sistem işi tetiklenecektir. Bu işlem, genel oturum başlatma süresini azaltmaya yardımcı olur. 
4. İş başarıyla tamamlandıktan sonra, tüm yeni oturumlar güncelleştirilmiş havuz kitaplıklarını seçer.

> [!IMPORTANT]
> **Yeni ayarları zorlama** seçeneğini belirleyerek seçili Spark havuzunun tüm geçerli oturumlarını sonlandıracaksınız. Oturumlar sona erdikten sonra, havuzun yeniden başlatılmasını beklemeniz gerekir. 
>
> Bu ayar işaretlenmezse, geçerli Spark oturumunun bitmesini beklemeniz veya el ile durdurmanız gerekir. Oturum sona erdikten sonra havuzun yeniden başlatılmasına izin vermek gerekir.

#### <a name="track-installation-progress-preview"></a>Yükleme ilerlemesini izleme (Önizleme)
Bir havuzun yeni bir kitaplık kümesiyle güncelleştirildiği her seferinde sistem tarafından ayrılmış Spark işi başlatılır. Bu Spark işi, kitaplık yüklemesinin durumunu izlemeye yardımcı olur. Yükleme, kitaplık çakışmaları veya diğer sorunlar nedeniyle başarısız olursa Spark havuzu önceki veya varsayılan durumuna geçer. 

Ayrıca, kullanıcılar bağımlılık çakışmalarını belirlemek için yükleme günlüklerini inceleyebilir veya havuz güncelleştirmesi sırasında hangi kitaplıkların yüklü olduğunu görebilirler.

Bu günlükleri görüntülemek için:
1. **İzleyici** sekmesinde Spark uygulamaları listesine gidin. 
2. Havuz güncelunuza karşılık gelen System Spark uygulama işini seçin. Bu sistem işleri *Systemreservedjob-LibraryManagement* başlığı altında çalışır.
   ![Sisteme ayrılan kitaplık işinin vurgualdığı ekran görüntüsü.](./media/apache-spark-azure-portal-add-libraries/system-reserved-library-job.png "Sistem kitaplığı işini görüntüle")
3. **Sürücü** ve **stdout** günlüklerini görüntülemek için geçiş yapın. 
4. Sonuçlar içinde, paketlerinizin yüklenmesiyle ilgili günlükleri görürsünüz.
    ![Sisteme ayrılan kitaplık işi sonuçlarının vurgualdığı ekran görüntüsü.](./media/apache-spark-azure-portal-add-libraries/system-reserved-library-job-results.png "Sistem kitaplığı işinin ilerlemesini görüntüleme")

## <a name="session-scoped-libraries"></a>Oturum kapsamlı kitaplıklar 
Havuz düzeyi kitaplıklarına ek olarak, bir not defteri oturumunun başlangıcında oturum kapsamlı kitaplıklar da belirtebilirsiniz.  Oturum kapsamlı kitaplıklar, jar paketlerini özel olarak bir not defteri oturumunda belirtmenizi ve kullanmanızı sağlar. 

Oturum kapsamlı kitaplıklar kullanırken aşağıdaki noktaları göz önünde bulundurmanız önemlidir:
   - Oturum kapsamlı kitaplıklar yüklediğinizde, belirtilen kitaplıklara yalnızca geçerli not defteri erişimi vardır. 
   - Bu kitaplıklar, aynı Spark havuzunu kullanan diğer oturumları veya işleri etkilemez. 
   - Bu kitaplıklar temel çalışma zamanı ve havuz düzeyi kitaplıklarının üzerine yüklenir. 
   - Not defteri kitaplıkları en yüksek önceliğe sahip olur.

Oturum kapsamlı Java veya Scala paketleri belirtmek için ```%%configure``` seçeneğini kullanabilirsiniz:

```scala
%%configure -f
{
    "conf": {
        "spark.jars": "abfss://<<file system>>@<<storage account>.dfs.core.windows.net/<<path to JAR file>>",
    }
}
```

%% Yapılandırmasını not defterinizin başlangıcında çalıştırmanızı öneririz. Geçerli parametrelerin tam listesi için bu [belgeye](https://github.com/cloudera/livy#request-body) başvurabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- Varsayılan kitaplıkları görüntüleme: [Apache Spark sürüm desteği](apache-spark-version-support.md)
- Kitaplık yükleme hatalarını giderme: [kitaplık hatalarında sorun giderme](apache-spark-troubleshoot-library-errors.md)
