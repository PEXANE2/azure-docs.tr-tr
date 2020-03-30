---
title: JAR bağımlılıklarını yönetme - Azure HDInsight
description: Bu makalede, HDInsight uygulamaları için Java Arşivi (JAR) bağımlılıklarını yönetmek için en iyi uygulamalar tartışılmaktadır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/05/2020
ms.openlocfilehash: da3387dd9846847f7643ded43c8cbff8ed8b166e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77135739"
---
# <a name="jar-dependency-management-best-practices"></a>JAR bağımlılık yönetimi en iyi uygulamalar

HDInsight kümelerine yüklenen bileşenlerin üçüncü taraf kitaplıklarına bağımlılığı vardır. Genellikle, Guava gibi ortak modüllerin belirli bir sürümü bu yerleşik bileşenler tarafından başvurulan. Bağımlılıkları olan bir uygulama gönderdiğiniz zaman, aynı modülün farklı sürümleri arasında çakışmaya neden olabilir. Önce classpath'te başlattığınız bileşen sürümü, yerleşik bileşenler sürüm uyumsuzluğu nedeniyle özel durumlar atabilir. Ancak, yerleşik bileşenler bağımlılıklarını önce classpath'e enjekte ederse, uygulamanız `NoSuchMethod`.

Sürüm çakışmasını önlemek için uygulama bağımlılıklarınızı gölgelemeyi düşünün.

## <a name="what-does-package-shading-mean"></a>Paket gölgeleme ne anlama gelir?
Gölgeleme, bağımlılıkları eklemenin ve yeniden adlandırmanın bir yolunu sağlar. Bağımlılıklarınızın özel bir kopyasını oluşturmak için sınıfları yeniden değiştirir ve etkilenen bytecode ve kaynakları yeniden yazar.

## <a name="how-to-shade-a-package"></a>Nasıl bir paket gölge?

### <a name="use-uber-jar"></a>Uber-jar kullanın
Uber-jar, hem uygulama kavanozunu hem de bağımlılıklarını içeren tek bir kavanoz dosyasıdır. Uber kavanozundaki bağımlılıklar varsayılan olarak gölgelendirilmeyecektir. Bazı durumlarda, diğer bileşenler veya uygulamalar bu kitaplıkların farklı bir sürümüne başvuruyorsa, bu sürüm çakışması başlatabilir. Bunu önlemek için, gölgeli bağımlılıkların bir kısmını (veya tamamını) içeren bir Uber-Jar dosyası oluşturabilirsiniz.

### <a name="shade-package-using-maven"></a>Maven kullanarak gölge paketi
Maven hem Java hem de Scala'da yazılmış uygulamalar oluşturabilir. Maven-shade-plugin kolayca gölgeli bir uber-jar oluşturmanıza yardımcı olabilir.

Aşağıdaki örnekte, `pom.xml` maven-shade-plugin kullanarak bir paketi gölgelemek üzere güncelleştirilen bir dosya gösterilmektedir.  XML bölümü, `<relocation>…</relocation>` ilgili `com.google.guava` JAR `com.google.shaded.guava` dosya girişlerini taşıyarak ve etkilenen bytecode'u yeniden yazarak sınıfları paketten pakete taşır.

Değiştirdikten `pom.xml`sonra, `mvn package` gölgeli uber-jar oluşturmak için çalıştırabilirsiniz.

```xml
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>3.2.1</version>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>shade</goal>
            </goals>
            <configuration>
              <relocations>
                <relocation>
                  <pattern>com.google.guava</pattern>
                  <shadedPattern>com.google.shaded.guava</shadedPattern>
                </relocation>
              </relocations>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
```

### <a name="shade-package-using-sbt"></a>SBT kullanarak gölge paketi
SBT aynı zamanda Scala ve Java için bir yapı aracıdır. SBT maven-gölge-eklentisi gibi bir gölge eklentisi yok. Dosyayı `build.sbt` gölge paketleri için değiştirebilirsiniz. 

Örneğin, gölgelemek `com.google.guava`için, dosyaya aşağıdaki `build.sbt` komutu ekleyebilirsiniz:

```scala
assemblyShadeRules in assembly := Seq(
  ShadeRule.rename("com.google.guava" -> "com.google.shaded.guava.@1").inAll
)
```

Sonra çalıştırabilirsiniz `sbt clean` `sbt assembly` ve gölgeli kavanoz dosyası oluşturmak için. 

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight IntelliJ Araçlarını Kullanma](https://docs.microsoft.com/azure/hdinsight/hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox)

* [IntelliJ Spark için bir Scala Maven uygulaması oluşturun](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-create-standalone-application)
