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
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77135739"
---
# <a name="jar-dependency-management-best-practices"></a>JAR bağımlılığı yönetimi en iyi uygulamaları

HDInsight kümelerine yüklenen bileşenlerin üçüncü taraf kitaplıklara bağımlılıkları vardır. Genellikle guava gibi yaygın modüllerin belirli bir sürümüne bu yerleşik bileşenler tarafından başvurulur. Bir uygulamayı bağımlılıklarıyla birlikte gönderdiğinizde, aynı modülün farklı sürümleri arasında çakışmaya neden olabilir. İlk olarak sınıflara başvuruda bulunan bileşen sürümü varsa, yerleşik bileşenler sürüm uyumsuzluğu nedeniyle özel durumlar oluşturabilir. Ancak, yerleşik bileşenler bağımlılıklarını ilk olarak Sınıfyoluna ekler, uygulamanız `NoSuchMethod`gibi hatalar oluşturabilir.

Sürüm çakışmasını önlemek için uygulama bağımlılıklarınızı gölgelendirmeyi göz önünde bulundurun.

## <a name="what-does-package-shading-mean"></a>Paket gölgelemesi ne anlama geliyor?
Gölgeleme, bağımlılıkları dahil etmek ve yeniden adlandırmak için bir yol sağlar. Sınıfları yeniden konumlandırır ve bağımlılıklarınızın özel bir kopyasını oluşturmak için etkilenen ByteCode ve kaynakları yeniden yazar.

## <a name="how-to-shade-a-package"></a>Bir paket nasıl gölgelenecek?

### <a name="use-uber-jar"></a>Uber-jar kullanma
Uber-jar, hem uygulama jar 'i hem de onun bağımlılıklarını içeren tek bir jar dosyasıdır. Uber-jar içindeki bağımlılıklar, varsayılan değer olarak gölmez. Bazı durumlarda, diğer bileşenler veya uygulamalar bu kitaplıkların farklı bir sürümüne başvuracak olursa bu sürüm çakışmasına neden olabilir. Bunu önlemek için, bağımlılıkların gölgeli bir Uber-jar dosyası oluşturabilirsiniz.

### <a name="shade-package-using-maven"></a>Maven kullanarak paket gölgelendir
Maven, hem Java hem de Scala 'da yazılmış uygulamalar oluşturabilir. Maven-gölge-eklentisi, gölgeli bir Uber-jar kolay bir şekilde oluşturmanıza yardımcı olabilir.

Aşağıdaki örnekte, Maven-gölgelendirmeli eklentisini kullanarak bir paketi gölgelendirmek üzere güncelleştirilmiş bir dosya `pom.xml` gösterilmektedir.  XML bölümü `<relocation>…</relocation>`, ilgili JAR dosyası girişlerini taşıyarak ve etkilenen bytecode 'u yeniden yazarak, sınıfları paket `com.google.guava` paket `com.google.shaded.guava` öğesine taşır.

`pom.xml`değiştirdikten sonra, gölgeli Uber-jar 'yi oluşturmak için `mvn package` çalıştırabilirsiniz.

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
SBT Ayrıca Scala ve Java için bir yapı aracıdır. SBT 'nin Maven-gölge-eklentisi gibi bir gölge eklentisi yok. Paketleri gölgelendirmek için `build.sbt` dosyasını değiştirebilirsiniz. 

Örneğin, `com.google.guava`gölgelendirmek için aşağıdaki komutu `build.sbt` dosyasına ekleyebilirsiniz:

```scala
assemblyShadeRules in assembly := Seq(
  ShadeRule.rename("com.google.guava" -> "com.google.shaded.guava.@1").inAll
)
```

Daha sonra, `sbt clean` ve `sbt assembly` çalıştırarak gölgeli jar dosyasını oluşturabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight IntelliJ araçlarını kullanma](https://docs.microsoft.com/azure/hdinsight/hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox)

* [IntelliJ 'de Spark için bir Scala Maven uygulaması oluşturma](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-create-standalone-application)
