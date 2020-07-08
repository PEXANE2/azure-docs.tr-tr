---
title: Apache Spark uygulamalarını izleme
description: Apache Spark uygulamalarınızı izlemek için Azure SYNAPSE Studio 'Yu kullanın.
services: synapse-analytics
author: matt1883
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: monitoring
ms.date: 04/15/2020
ms.author: mahi
ms.reviewer: mahi
ms.openlocfilehash: 20c4216e7af0cb4100204ebae1b484a53e3c50a6
ms.sourcegitcommit: f684589322633f1a0fafb627a03498b148b0d521
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85971386"
---
# <a name="use-the-azure-synapse-studio-to-monitor-your-apache-spark-applications"></a>Apache Spark uygulamalarınızı izlemek için Azure SYNAPSE Studio 'Yu kullanma

Azure SYNAPSE Analytics ile, çalışma alanınızdaki Apache Spark Havuzlarınızda not defterlerini, işleri ve diğer uygulama türlerini çalıştırmak için Apache Spark kullanabilirsiniz.

Bu makalede, Apache Spark uygulamalarınızın nasıl izleneceği açıklanmaktadır. Bu, en son durum, sorun ve ilerlemeyi göz önünde tutmanıza olanak sağlar.

Bu öğretici aşağıdaki görevleri kapsar:

* Apache Spark uygulamasını çalıştıran izleyici
* Tamamlanmış Apache Spark uygulamasını görüntüleme
* İptal edilen Apache Spark uygulamasını görüntüle
* Apache Spark uygulamada hata ayıklama başarısız oldu

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce aşağıdaki gereksinimlerin karşılandığından emin olun:

- Bir Azure SYNAPSE Studio çalışma alanı. Yönergeler için bkz. [Azure SYNAPSE Studio çalışma alanı oluşturma](https://docs.microsoft.com/azure/machine-learning/how-to-manage-workspace#create-a-workspace).

- Bir Apache Spark Havuzu.

## <a name="monitor-running-apache-spark-application"></a>Apache Spark uygulamasını çalıştıran izleyici

**İzleyiciyi**açın ve **Apache Spark uygulamalar**' ı seçin. Çalışan Apache Spark uygulamalarla ilgili ayrıntıları görüntülemek için Apache Spark uygulamayı gönderme ve ayrıntıları görüntüleme seçeneğini belirleyin. Apache Spark uygulama hala çalışıyorsa, ilerlemeyi izleyebilirsiniz.

  ![çalışan işi seçin](./media/how-to-monitor-spark-applications/select-running-job.png)

1. **Tamamlanan görevleri**, **durumu**ve **toplam süreyi**denetleyin.

2. Apache Spark uygulamasını iptal edin.

3. Günlük sorgusunu yenileyin.

4. Grafiği görüntüleyin.

5. **Özet** bilgisini denetleyin.

6. **Günlükleri**denetleyin. Günlük bilgileri çalıştırılırken boş.

    ![çalışan işi görüntüle](./media/how-to-monitor-spark-applications/view-running-job.png)

## <a name="view-completed-apache-spark-application"></a>Tamamlanmış Apache Spark uygulamasını görüntüleme

**İzleyiciyi**açın ve **Apache Spark uygulamalar**' ı seçin. Tamamlanmış Apache Spark uygulamalarla ilgili ayrıntıları görüntülemek için Apache Spark uygulamasını seçin ve ayrıntıları görüntüleyin.

  ![tamamlanan işi seçin](./media/how-to-monitor-spark-applications/select-completed-job.png)

1. **Tamamlanan görevleri**, **durumu**ve **toplam süreyi**denetleyin.

2. Günlük sorgusunu yenileyin.

3. **Spark geçmiş sunucusu**' na tıklayarak Apache Spark geçmiş sunucu bağlantısını açın.

4. Grafikteki simgeye tıklayarak **Özet** bilgilerini denetleyin.

5. **Günlükleri**denetleyin. Açılan listeden farklı türde Günlükler seçebilirsiniz ve **günlükleri indir**' i tıklatarak günlük bilgilerini indirebilirsiniz.

6. Oluşturulan iş grafiğinde işinize bir genel bakış görebilirsiniz. Varsayılan olarak, grafik tüm işleri gösterir. Bu görünümü **Iş kimliğine**göre filtreleyebilirsiniz.

7. Varsayılan olarak, **Ilerleme durumu** ekranı seçilidir. **Görüntüleme** açılır listesinde **okuma** veya **yazma** seçeneğini belirleyerek veri akışını kontrol edebilirsiniz.

8. İşi kayıttan yürütmek için **kayıttan yürütme**' yi seçin. Durdurmak için dilediğiniz zaman **Durdur** seçeneğini belirleyebilirsiniz.

9. Farenizi kullanarak iş grafiğini yakınlaştırın ve uzaklaştırın veya ekrana sığacak hale getirmek için **yakınlaştırmak Için Yakınlaştır** ' ı seçin.

10. İş grafiği düğümü her aşama için aşağıdaki bilgileri görüntüler:

    * Numarasını.

    * Ad veya açıklama.

    * Toplam görev numarası.

    * Okunan veriler: giriş boyutu ve karışık okuma boyutu toplamı.

    * Veri yazma: çıkış boyutunun ve karışık yazma boyutunun toplamı.

    * Yürütme Süresi: son denemede ilk deneme ve tamamlanma zamanının başlangıç saati arasındaki süre.

    * Satır sayısı: giriş kayıtlarının toplamı, çıkış kayıtları, okuma kayıtlarını karıştırma ve yazma kayıtlarını karıştırma.

    * Lemesine.

     ![tamamlanan işi görüntüle](./media/how-to-monitor-spark-applications/view-completed-job.png)
    
11. Grafiğe tıklayın, sonra da aşama için ayrıntılar gösterilir.

   ![aşama ayrıntıları](./media/how-to-monitor-spark-applications/details-for-stage.png)

## <a name="view-canceled-apache-spark-application"></a>İptal edilen Apache Spark uygulamasını görüntüle

**İzleyiciyi**açın ve **Apache Spark uygulamalar**' ı seçin. İptal edilen Apache Spark uygulamalarla ilgili ayrıntıları görüntülemek için Apache Spark uygulamayı seçin ve ayrıntıları görüntüleyin.

 ![iptal edilen işi seçin](./media/how-to-monitor-spark-applications/select-cancelled-job.png) 

1. **Tamamlanan görevleri**, **durumu**ve **toplam süreyi**denetleyin.

2. Günlük sorgusunu yenileyin.

3. **Spark geçmiş sunucusu**' na tıklayarak Apache geçmiş sunucusu bağlantısını açın.

4. Grafiği görüntüleyin.

5. **Özet** bilgisini denetleyin.

6. **Günlükleri**denetleyin. Açılan listeden farklı türde Günlükler seçebilirsiniz ve **günlükleri indir**' i tıklatarak günlük bilgilerini indirebilirsiniz.

   ![iptal edilen işi görüntüle](./media/how-to-monitor-spark-applications/view-cancelled-job.png)

## <a name="debug-failed-apache-spark-application"></a>Apache Spark uygulamada hata ayıklama başarısız oldu

**İzleyiciyi**açın ve **Apache Spark uygulamalar**' ı seçin. Başarısız Apache Spark uygulamalarla ilgili ayrıntıları görüntülemek için Apache Spark uygulamayı seçip ayrıntıları görüntüleyin.

![başarısız iş seçin](./media/how-to-monitor-spark-applications/select-failed-job.png)

1. **Tamamlanan görevleri**, **durumu**ve **toplam süreyi**denetleyin.

2. Günlük sorgusunu yenileyin.

3. **Spark geçmiş sunucusu**' na tıklayarak Apache Spark geçmiş sunucu bağlantısını açın.

4. Grafiği görüntüleyin.

5. **Özet** bilgisini denetleyin.

6. Hata bilgilerini denetleyin.

   ![başarısız iş bilgileri](./media/how-to-monitor-spark-applications/failed-job-info.png)

## <a name="next-steps"></a>Sonraki adımlar

İşlem hattı çalıştırmalarını izleme hakkında daha fazla bilgi için bkz. [izleyici işlem hattı Azure SYNAPSE Studio 'yu çalıştırma](how-to-monitor-pipeline-runs.md) makalesi.  
