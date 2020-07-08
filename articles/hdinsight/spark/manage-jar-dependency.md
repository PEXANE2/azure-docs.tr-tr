---
title: JAR bağımlılıklarını yönetme-Azure HDInsight
description: Bu makalede, HDInsight uygulamaları için Java Arşivi (JAR) bağımlılıklarını yönetmeye yönelik en iyi yöntemler açıklanmaktadır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/05/2020
ms.openlocfilehash: da3387dd9846847f7643ded43c8cbff8ed8b166e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77135739"
---
# <a name="jar-dependency-management-best-practices"></a>JAR bağımlılığı yönetimi en iyi uygulamaları

HDInsight kümelerine yüklenen bileşenlerin üçüncü taraf kitaplıklara bağımlılıkları vardır. Genellikle guava gibi yaygın modüllerin belirli bir sürümüne bu yerleşik bileşenler tarafından başvurulur. Bir uygulamayı bağımlılıklarıyla birlikte gönderdiğinizde, aynı modülün farklı sürümleri arasında çakışmaya neden olabilir. İlk olarak sınıflara başvuruda bulunan bileşen sürümü varsa, yerleşik bileşenler sürüm uyumsuzluğu nedeniyle özel durumlar oluşturabilir. Ancak, yerleşik bileşenler bağımlılıklarını ilk olarak Sınıfyoluna ekler, uygulamanız gibi hatalar oluşturabilir `NoSuchMethod` .

Sürüm çakışmasını önlemek için uygulama bağımlılıklarınızı gölgelendirmeyi göz önünde bulundurun.

## <a name="what-does-package-shading-mean"></a>Paket gölgelemesi ne anlama geliyor?
Gölgeleme, bağımlılıkları dahil etmek ve yeniden adlandırmak için bir yol sağlar. Sınıfları yeniden konumlandırır ve bağımlılıklarınızın özel bir kopyasını oluşturmak için etkilenen ByteCode ve kaynakları yeniden yazar.

## <a name="how-to-shade-a-package"></a>Bir paket nasıl gölgelenecek?

### <a name="use-uber-jar"></a>Uber-jar kullanma
Uber-jar, hem uygulama jar 'i hem de onun bağımlılıklarını içeren tek bir jar dosyasıdır. Uber-jar içindeki bağımlılıklar, varsayılan değer olarak gölmez. Bazı durumlarda, diğer bileşenler veya uygulamalar bu kitaplıkların farklı bir sürümüne başvuracak olursa bu sürüm çakışmasına neden olabilir. Bunu önlemek için, bağımlılıkların gölgeli bir Uber-jar dosyası oluşturabilirsiniz.

### <a name="shade-package-using-maven"></a>Maven kullanarak paket gölgelendir
Maven, hem Java hem de Scala 'da yazılmış uygulamalar oluşturabilir. Maven-gölge-eklentisi, gölgeli bir Uber-jar kolay bir şekilde oluşturmanıza yardımcı olabilir.

Aşağıdaki örnekte, `pom.xml` Maven-gölge-eklentisini kullanarak bir paketi gölgelendirmek üzere güncelleştirilmiş bir dosya gösterilmektedir.  XML bölümü, `<relocation>…</relocation>` `com.google.guava` `com.google.shaded.guava` ilgili jar dosyası girişlerini taşıyarak ve etkilenen bytecode 'u yeniden yazarak, sınıfları paketten pakete taşır.

`pom.xml`' Yi değiştirdikten sonra, `mvn package` gölgeli Uber-jar 'yi oluşturmak için çalıştırabilirsiniz.

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

### <a name="shade-package-using-sbt"></a>SBT kullanarak paket gölgelendir
SBT Ayrıca Scala ve Java için bir yapı aracıdır. SBT 'nin Maven-gölge-eklentisi gibi bir gölge eklentisi yok. `build.sbt`Dosyayı gölge paketleri olarak değiştirebilirsiniz. 

Örneğin, gölgelendirmek için `com.google.guava` aşağıdaki komutu `build.sbt` dosyasına ekleyebilirsiniz:

```scala
assemblyShadeRules in assembly := Seq(
  ShadeRule.rename("com.google.guava" -> "com.google.shaded.guava.@1").inAll
)
```

Ardından, `sbt clean` ve ' yi çalıştırarak `sbt assembly` gölgeli jar dosyasını oluşturabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight IntelliJ araçlarını kullanma](https://docs.microsoft.com/azure/hdinsight/hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox)

* [IntelliJ 'de Spark için bir Scala Maven uygulaması oluşturma](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-create-standalone-application)
