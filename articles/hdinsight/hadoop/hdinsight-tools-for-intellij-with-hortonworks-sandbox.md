---
title: Hortonworks Sandbox ile IntelliJ için Azure Araç Kiti'ni kullanma
description: Hortonworks Sandbox ile IntelliJ için Azure Araç Kiti'nde HDInsight Araçlarını nasıl kullanacağınızı öğrenin.
keywords: hadoop araçları, kovan sorgusu, intellij, hortonworks kum havuzu, intellij için azure araç
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 05/16/2018
ms.openlocfilehash: 65a15a8506b88e95e14af8c87bcbe33087301519
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75647841"
---
# <a name="use-hdinsight-tools-for-intellij-with-hortonworks-sandbox"></a>Hortonworks Sandbox ile IntelliJ için HDInsight Araçlarını Kullanın

Apache Scala uygulamalarını geliştirmek için IntelliJ için HDInsight Araçlarını nasıl kullanacağınızı öğrenin ve ardından uygulamaları bilgisayarınızda çalışan [Hortonworks Sandbox'ta](https://hortonworks.com/products/sandbox/) test edin. 

[IntelliJ IDEA](https://www.jetbrains.com/idea/) bilgisayar yazılımı geliştirmek için bir Java entegre geliştirme ortamı (IDE) olduğunu. Uygulamalarınızı Hortonworks Sandbox'ta geliştirip test ettikten sonra, uygulamaları [Azure HDInsight'a](apache-hadoop-introduction.md)taşıyabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu makaleye başlamadan önce aşağıdaki öğelere sahip olmalısınız:

- Hortonworks Veri Platformu (HDP) 2.4 Hortonworks Sandbox yerel bilgisayarınızda çalışan. HDP'yi kurmak için, [sanal bir makinede Hadoop kum havuzu ile Apache Hadoop ekosistemine başlayın](apache-hadoop-emulator-get-started.md)bakın. 
    > [!NOTE]
    > HDInsight Tools for IntelliJ sadece HDP 2.4 ile test edilmiştir. HDP 2.4 almak için, [Hortonworks Sandbox indirme sitesinde](https://hortonworks.com/downloads/#sandbox) **Hortonworks Sandbox Arşiv** genişletmek .

- [Java Geliştirici Kiti (JDK) sürüm 1.8 veya daha sonra.](https://aka.ms/azure-jdks) IntelliJ için Azure Araç Seti JDK gerektirir.

- [Scala](https://plugins.jetbrains.com/idea/plugin/1347-scala) eklentisi ve [IntelliJ](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij) eklentisi için Azure Araç Seti ile [IntelliJ IDEA topluluk sürümü.](https://www.jetbrains.com/idea/download) IntelliJ için HDInsight Araçları, IntelliJ için Azure Araç Seti'nin bir parçası olarak kullanılabilir. 

Eklentileri yüklemek için:

  1. IntelliJ IDEA’yı açın.
  2. Hoş **Geldiniz** **sayfasında, Yapıla'yı**seçin ve **ardından Eklentileri**seçin.
  3. Sol alt köşede **JetBrains eklentisini yükle'yi**seçin.
  4. **Scala'yı**aramak için arama işlevini kullanın ve sonra **Yükle'yi**seçin.
  5. Yüklemeyi tamamlamak için **IntelliJ IDEA'yı yeniden başlat'ı**seçin.
  6. **IntelliJ için Azure Araç Seti'ni**yüklemek için 4 ve 5 adımlarını yineleyin. Daha fazla bilgi için [IntelliJ için Azure Araç Kiti'ni yükleyin'](https://docs.microsoft.com/azure/azure-toolkit-for-intellij-installation)e bakın.

## <a name="create-an-apache-spark-scala-application"></a>Bir Apache Spark Scala uygulaması oluşturma

Bu bölümde, IntelliJ IDEA kullanarak örnek bir Scala projesi oluşturursunuz. Bir sonraki bölümde, projeyi göndermeden önce IntelliJ IDEA'yı Hortonworks Sandbox'a (emülatör) bağlarsınız.

1. Bilgisayarınızda IntelliJ IDEA'yı açın. Yeni **Proje** iletişim kutusunda aşağıdaki adımları tamamlayın:

   1. **HDInsight** > **HDInsight'ta HDInsight Spark'ı (Scala)** seçin.
   2. Yapı **araç** listesinde, senaryonuza göre aşağıdakilerden birini seçin:

      * **Maven**: Scala proje oluşturma sihirbazı desteği için.
      * **SBT**: Bağımlılıkların yönetimi ve Scala projesi için bina için.

   ![Intellij yeni scala projesi oluşturmak](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project.png)

2. **Sonraki'ni**seçin.
3. Sonraki **Yeni Proje** iletişim kutusunda aşağıdaki adımları tamamlayın:

   1. Proje **adı** kutusuna bir proje adı girin.
   2. Proje **konum** kutusuna bir proje konumu girin.
   3. **Project SDK** açılır listesinin yanında, **Yeni**, **JDK'yı**seçin ve ardından Java JDK sürüm 1.7 veya sonraki sürüm için klasörü belirtin. Spark 2.x kümesi için **Java 1.8'i** seçin. Spark 1.x kümesi için **Java 1.7'yi** seçin. Varsayılan konum C:\Program Files\Java\jdk1.8.x_xxx'dir.
   4. **Spark sürüm** açılır listesinde, Scala proje oluşturma sihirbazı Spark SDK ve Scala SDK için doğru sürümü tümleştirir. Spark kümesi sürümü 2.0’dan eskiyse **Spark 1.x** seçeneğini belirleyin. Aksi takdirde, **Spark2.x** seçeneğini belirleyin. Bu örnek, Spark 1.6.2 (Scala 2.10.5) kullanır. **Scala 2.10.x**işaretli depoyu kullandığınızdan emin olun. Scala 2.11.x işaretli depoyu kullanmayın.
    
      ![IntelliJ Scala proje özelliklerini oluşturma](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-scala-project-properties.png)


4. **Bitiş'i**seçin.
5. **Proje** görünümü zaten açık değilse, açmak için **Alt+1'e** basın.
6. **Project**Explorer'da, projeyi genişletin ve ardından **src'yi**seçin.
7. Sağ tıklayın **src**, **Yeni**işaret , ve sonra **Scala sınıf**seçin.
8. **Ad** kutusuna bir ad girin. **Tür** kutusunda **Nesne'yi**seçin. Ardından **Tamam'ı**seçin.

    ![Yeni Scala Sınıfı oluştur iletişim kutusu](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-create-new-scala-class.png)

9. .scala dosyasında aşağıdaki kodu yapıştırın:

        import java.util.Random
        import org.apache.spark.{SparkConf, SparkContext}
        import org.apache.spark.SparkContext._

        /**
        * Usage: GroupByTest [numMappers] [numKVPairs] [valSize] [numReducers]
        */
        object GroupByTest {
            def main(args: Array[String]) {
                val sparkConf = new SparkConf().setAppName("GroupBy Test")
                var numMappers = 3
                var numKVPairs = 10
                var valSize = 10
                var numReducers = 2

                val sc = new SparkContext(sparkConf)

                val pairs1 = sc.parallelize(0 until numMappers, numMappers).flatMap { p =>
                val ranGen = new Random
                var arr1 = new Array[(Int, Array[Byte])](numKVPairs)
                for (i <- 0 until numKVPairs) {
                    val byteArr = new Array[Byte](valSize)
                    ranGen.nextBytes(byteArr)
                    arr1(i) = (ranGen.nextInt(Int.MaxValue), byteArr)
                }
                arr1
                }.cache
                // Enforce that everything has been calculated and in cache.
                pairs1.count

                println(pairs1.groupByKey(numReducers).count)
            }
        }

10. **Yapı** menüsünde **Project Oluştur'u**seçin. Derlemenin başarıyla bittiğinden emin olun.


## <a name="link-to-the-hortonworks-sandbox"></a>Hortonworks Sandbox bağlantısı

Hortonworks Sandbox'a (emülatör) bağlanmadan önce varolan bir IntelliJ uygulamanız olmalıdır.

Bir emülatöre bağlanmak için:

1. Projeyi IntelliJ'de açın.
2. **Görünüm** menüsünde **Araçlar Windows'u**seçin ve ardından **Azure Gezgini'ni**seçin.
3. **Azure'u**genişletin, **HDInsight'ı**sağ tıklatın ve ardından **Emülatöre Bağla'yı**seçin.
4. Yeni **Bir Emülatör** Bağlantı Kutusu'na, Hortonworks Sandbox'ın kök hesabı için ayarladığınız parolayı girin. Ardından, aşağıdaki ekran görüntüsünde kullanılandeğerlere benzer değerler girin. Ardından **Tamam'ı**seçin. 

   ![Yeni Emülatör iletişim kutusunu bağla](./media/hdinsight-tools-for-intellij-with-hortonworks-sandbox/intellij-link-an-emulator.png)

5. Emülatörünü yapılandırmak için **Evet'i**seçin.

Emülatör başarıyla bağlandığında, emülatör (Hortonworks Sandbox) HDInsight düğümünde listelenir.

## <a name="submit-the-spark-scala-application-to-the-hortonworks-sandbox"></a>Kıvılcım Scala uygulamasını Hortonworks Sandbox'a gönderin

IntelliJ IDEA'yı emülatöre bağladıktan sonra projenizi gönderebilirsiniz.

Bir projeyi emülatöre göndermek için:

1. **Project Explorer'da**projeyi sağ tıklatın ve ardından **Spark uygulamasını HDInsight'a gönder'i**seçin.
2. Aşağıdaki adımları tamamlayın:

    1. Kıvılcım **kümesinde (yalnızca Linux)** açılır listede, yerel Hortonworks Sandbox'ınızı seçin.
    2. Ana **sınıf adı** kutusunda, ana sınıf adını seçin veya girin. Bu makale için, adı **GroupByTest**olduğunu.

3. **Gönder**’i seçin. İş gönderme günlükleri Spark gönderme aracı penceresinde gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir HDInsight Spark Linux kümesi için Apache Spark uygulamaları oluşturmak için IntelliJ için Azure Araç Setinde HDInsight Araçlarını](../spark/apache-spark-intellij-tool-plugin.md)nasıl kullanacağınızı öğrenin.

- IntelliJ için HDInsight Araçları hakkında bir video [için, Apache Spark geliştirme için IntelliJ için HDInsight Araçlarını Tanıyın'a](https://www.youtube.com/watch?v=YTZzYVgut6c)bakın.

- [SSH aracılığıyla IntelliJ için Azure Araç Seti ile HDInsight kümesindeki Apache Spark uygulamalarını uzaktan](../spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)nasıl hata ayıklamayı öğrenin.

- [Bir HDInsight Spark Linux kümesinde Apache Spark uygulamalarını uzaktan hata ayıklamak için IntelliJ için Azure Araç Setinde HDInsight Araçlarını](../spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)nasıl kullanacağınızı öğrenin.

- [Apache Spark uygulamaları oluşturmak için Eclipse için Azure Araç Setinde HDInsight Araçlarını](../spark/apache-spark-eclipse-tool-plugin.md)nasıl kullanacağınızı öğrenin.

