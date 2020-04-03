---
title: Azure HDInsight'ta Apache Hive için ağ geçidi derin dalış ve en iyi uygulamalar
description: Azure HDInsight ağ geçidinde Hive sorgularını çalıştırmak için en iyi uygulamalarda nasıl gezineceklerini öğrenin
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/01/2020
ms.openlocfilehash: 924b1132efeb3ee4211593da190f5b7251029ae3
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80586983"
---
# <a name="gateway-deep-dive-and-best-practices-for-apache-hive-in-azure-hdinsight"></a>Azure HDInsight'ta Apache Hive için ağ geçidi derin dalış ve en iyi uygulamalar

Azure HDInsight ağ geçidi (Ağ Geçidi), HDInsight kümeleri için HTTPS ön yüzdür. Ağ Geçidi sorumludur: kimlik doğrulama, ana bilgisayar çözünürlüğü, hizmet bulma ve modern dağıtılmış bir sistem için gerekli diğer yararlı özellikler. Ağ Geçidi tarafından sağlanan özellikler, bu belgede gezinmek için en iyi uygulamaları açıklayacak bazı ek yükü neden olur. Ağ geçidi sorun giderme teknikleri de tartışılır.

## <a name="the-hdinsight-gateway"></a>HDInsight ağ geçidi

HDInsight ağ geçidi, HDInsight kümesinin internet üzerinden herkese açık olan tek parçasıdır. Ağ Geçidi `clustername-int.azurehdinsight.net` hizmetine, dahili ağ geçidi bitiş noktası kullanılarak genel internet üzerinden girmeden erişilebilir. Dahili ağ geçidi bitiş noktası, kümenin sanal ağından çıkmadan ağ geçidi hizmetine bağlantılar kurulmasına olanak tanır. Ağ Geçidi kümeye gönderilen tüm istekleri işler ve bu istekleri doğru bileşenlere ve küme ana bilgisayarlarına iletir.

Aşağıdaki diyagram, Ağ Geçidi'nin HDInsight'taki tüm farklı ana bilgisayar çözümlüğü olasılıklarının önünde nasıl bir soyutlama sağladığının kaba bir örneğini sağlar.

![Ana Bilgisayar Çözünürlük Diyagramı](./media/gateway-best-practices/host-resolution-diagram.png "Ana Bilgisayar Çözünürlük Diyagramı")

## <a name="motivation"></a>Motivasyon

HDInsight kümelerinin önüne bir ağ geçidi koymanın nedeni, hizmet bulma ve kullanıcı kimlik doğrulaması için bir arayüz sağlamaktır. Ağ geçidi tarafından sağlanan kimlik doğrulama mekanizmaları özellikle ESP özellikli kümelerle ilgilidir.

Hizmet bulma için ağ geçidinin avantajı, küme içindeki her bileşene çok sayıda `clustername.azurehdinsight.net/hive2` `host:port` eşleştirmeyerine Ağ Geçidi web sitesi altında farklı uç nokta olarak erişilebiliyor olmasıdır.

Kimlik doğrulaması için Ağ Geçidi, kullanıcıların `username:password` kimlik bilgisi çiftini kullanarak kimlik doğrulaması yapmasına olanak tanır. ESP özellikli kümeler için bu kimlik bilgisi kullanıcının etki alanı kullanıcı adı ve parolası olur. Ağ Geçidi üzerinden HDInsight kümelerine kimlik doğrulama istemcisinin kerberos bileti almasını gerektirmez. Ağ Geçidi kimlik `username:password` bilgilerini kabul ettiği ve kullanıcı adına kullanıcının Kerberos biletini satın aldığı için, (ESP) kümesinden farklı AA-DDS etki adlarına katılan istemciler de dahil olmak üzere, herhangi bir istemci ana bilgisayardan Ağ Geçidi'ne güvenli bağlantılar yapılabilir.

## <a name="best-practices"></a>En iyi uygulamalar

Ağ Geçidi, ileti ve kimlik doğrulama isteğinden sorumlu tek bir hizmettir (iki ana bilgisayar arasında dengelenmiş yük). Ağ Geçidi, belirli bir boyutu aşan Hive sorguları için bir iş çıkış darboğaz olabilir. Çok büyük **SELECT** sorguları ODBC veya JDBC üzerinden Ağ Geçidi'nde yürütüldüğünde sorgu performansı bozulması görülebilir. "Çok büyük", satırlar veya sütunlar arasında 5 GB'dan fazla veri oluşturan sorgular anlamına gelir. Bu sorgu, uzun bir satır listesi ve veya geniş bir sütun sayısı içerebilir.

Ağ Geçidi'nin büyük boyutlu sorgular etrafında ki performans bozulması, verilerin temel veri deposundan (ADLS Gen2) HDInsight Hive Server'a, Ağ Geçidi'ne ve son olarak JDBC veya ODBC sürücüleri aracılığıyla istemci ana bilgisayara aktarılması gerektiğidir.

Aşağıdaki diyagram, SELECT sorgusunda yer alan adımları göstermektedir.

![Sonuç Diyagramı](./media/gateway-best-practices/result-retrieval-diagram.png "Sonuç Diyagramı")

Apache Hive, HDFS uyumlu bir dosya sisteminin üstünde ki ilişkisel bir soyutlamadır. Bu soyutlama, Hive'daki **SELECT** ifadelerinin dosya sistemindeki **READ** işlemlerine karşılık geldiğini gösterir. **READ** işlemleri kullanıcıya bildirilmeden önce uygun şemaya dönüştürülür. Bu işlemin gecikmesi, son kullanıcıya ulaşmak için gereken veri boyutu ve toplam atlamalarla artar.

Bu komutlar temel dosya **sistemindeki** **WRITE** işlemlerine karşılık geldiğinden, büyük verilerin CREATE veya **INSERT** deyimleri yürütüldüğünde benzer davranışlar oluşabilir. **INSERT** veya **LOAD**kullanarak yüklemek yerine filesystem/datalake'e ham ORC gibi verileri yazmayı düşünün.

Kurumsal Güvenlik Paketi etkin kümelerde, yeterince karmaşık Olan Apache Ranger ilkeleri sorgu derleme süresinde yavaşlamaya neden olabilir ve bu da ağ geçidi zaman anına neden olabilir. Bir ESP kümesinde bir ağ geçidi zaman aşımı fark edilirse, ranger ilkelerinin sayısını azaltmayı veya birleştirmeyi düşünün.

## <a name="troubleshooting-techniques"></a>Sorun giderme teknikleri

Yukarıdaki davranışın bir parçası olarak karşılanan performans sorunlarını hafifletmek ve anlamak için birden çok mekan vardır. HDInsight ağ geçidinde sorgu performansında bozulma yaşarken aşağıdaki denetim listesini kullanın:

* Büyük **SELECT** sorguları yürütürken **LIMIT** yan tümcesini kullanın. **LIMIT** yan tümcesi, istemci ana bilgisayara bildirilen toplam satırları azaltır. **LIMIT** yan tümcesi yalnızca sonuç oluşumunu etkiler ve sorgu planını değiştirmez. **Limit** yan tümcesini sorgu planına `hive.limit.optimize.enable`uygulamak için yapılandırmayı kullanın. **LIMIT,** **limit x,y**bağımsız değişken formu kullanılarak bir ofset ile birleştirilebilir.

* **SELECT \* **sorgularını kullanmak yerine **SELECT** sorgularını çalıştırırken ilgi alan sütunlarınızı adlandırın. Daha az sütun seçmek, okunan veri miktarını düşürür.

* Apache Beeline üzerinden ilgi sorgu çalıştırmayı deneyin. Apache Beeline üzerinden sonuç alımı uzun bir süre alıyorsa, dış araçlar aracılığıyla aynı sonuçları alırken gecikmeler bekleyin.

* HDInsight Ağ Geçidi'ne bağlantı kurulabileceğinden emin olmak için temel bir Hive sorgusunu test edin. Hiçbir aracın sorunlara girmediğinden emin olmak için iki veya daha fazla dış araçtan temel bir sorgu çalıştırmayı deneyin.

* HDInsight hizmetlerinin sağlıklı olduğundan emin olmak için apache Ambari Uyarılarını inceleyin. Ambari Uyarıları raporlama ve analiz için Azure Monitor ile tümleştirilebilir.

* Harici bir Hive Metastore kullanıyorsanız, Hive Metastore için Azure SQL DB DTU'nun sınırına ulaşmadığını kontrol edin. DTU sınırına yaklaşıyorsa, veritabanı boyutunu artırmanız gerekir.

* PBI veya Tableau gibi üçüncü taraf araçlarının tabloları veya veritabanlarını görüntülemek için pagination kullandığından emin olun. Pagination yardım için bu araçlar için destek ortakları danışın. Pagination için kullanılan ana araç JDBC `fetchSize` parametresi. Küçük bir getirme boyutu, ağ geçidi performansının düşmesine neden olabilir, ancak çok büyük bir getirme boyutu ağ geçidi zaman acısı ile sonuçlanabilir. Boyut anınması iş yükü esasına göre yapılmalıdır.

* Veri ardınız HDInsight kümesinin temel depolamasından büyük miktarda veri okumayı içeriyorsa, Azure Veri Fabrikası gibi Doğrudan Azure Depolama ile ara birim yapan bir araç kullanmayı düşünün

* Etkileşimli iş yüklerini çalıştırırken Apache Hive LLAP kullanmayı düşünün, çünkü LLAP hızlı bir şekilde dönen sorgu sonuçları için daha sorunsuz bir deneyim sağlayabilir

* Hive Metastore hizmeti için kullanılabilir iş parçacığı `hive.server2.thrift.max.worker.threads`sayısını artırmayı düşünün. Bu ayar, özellikle çok sayıda eşzamanlı kullanıcı kümeye sorgu gönderirken önemlidir

* Ağ Geçidi'ne herhangi bir dış araçtan ulaşmak için kullanılan yeniden deneme sayısını azaltın. Birden çok yeniden deneme kullanılırsa, yeniden deneme ilkesini üstel olarak kapatmayı göz önünde bulundurun

* Yapılandırmaları `hive.exec.compress.output` kullanarak sıkıştırma Hive etkinleştirmeyi düşünün ve. `hive.exec.compress.intermediate`

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Beeline HDInsight üzerinde](https://docs.microsoft.com/azure/hdinsight/hadoop/apache-hadoop-use-hive-beeline)
* [HDInsight Ağ Geçidi Zaman Ara Giderme Adımları](https://docs.microsoft.com/azure/hdinsight/interactive-query/troubleshoot-gateway-timeout)
* [HDInsight için Sanal Ağlar](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment)
* [Express Route ile HDInsight](https://docs.microsoft.com/azure/hdinsight/connect-on-premises-network)