---
title: Azure HDInsight 'ta Apache Hive için ağ geçidi derinlemesine bakış ve en iyi uygulamalar
description: Azure HDInsight ağ geçidi üzerinden Hive sorguları çalıştırmak için en iyi yöntemleri nasıl gezeceğinizi öğrenin
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/01/2020
ms.openlocfilehash: 924b1132efeb3ee4211593da190f5b7251029ae3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80586983"
---
# <a name="gateway-deep-dive-and-best-practices-for-apache-hive-in-azure-hdinsight"></a>Azure HDInsight 'ta Apache Hive için ağ geçidi derinlemesine bakış ve en iyi uygulamalar

Azure HDInsight ağ geçidi (Gateway), HDInsight kümeleri için HTTPS ön ucu olur. Ağ geçidinin sorumluluğundadır: kimlik doğrulaması, ana bilgisayar çözünürlüğü, hizmet bulma ve modern dağıtılmış bir sistem için gereken diğer yararlı özellikler. Ağ Geçidi tarafından sunulan özellikler, bu belgenin gezinmek için en iyi uygulamaları açıklayan bazı ek yüke neden olur. Ağ Geçidi sorun giderme teknikleri de ele alınmıştır.

## <a name="the-hdinsight-gateway"></a>HDInsight ağ geçidi

HDInsight ağ geçidi, Internet üzerinden genel olarak erişilebilen bir HDInsight kümesinin tek parçasıdır. Ağ Geçidi hizmetine, `clustername-int.azurehdinsight.net` iç ağ geçidi uç noktası kullanılarak genel İnternet üzerinden geçmeden erişilebilir. İç ağ geçidi uç noktası, kümenin sanal ağından çıkmadan ağ geçidi hizmetine bağlantı kurulmasını sağlar. Ağ Geçidi, kümeye gönderilen tüm istekleri işler ve bu gibi istekleri doğru bileşenlere ve küme konaklarına iletir.

Aşağıdaki diyagramda, ağ geçidinin HDInsight 'taki tüm farklı ana bilgisayar çözümleme olasılıklarının önünde nasıl soyutlama sağladığını gösteren kaba bir şekil sunulmaktadır.

![Ana bilgisayar çözümleme diyagramı](./media/gateway-best-practices/host-resolution-diagram.png "Ana bilgisayar çözümleme diyagramı")

## <a name="motivation"></a>Motivasyon

HDInsight kümelerinin önüne bir ağ geçidi koymak için mosyon, hizmet bulma ve Kullanıcı kimlik doğrulaması için bir arabirim sağlamaktır. Ağ Geçidi tarafından sağlanan kimlik doğrulama mekanizmaları özellikle ESP özellikli kümeler ile ilgilidir.

Hizmet bulma için, ağ geçidinin avantajı, küme içindeki her bileşene, çok sayıda eşleştirinin `clustername.azurehdinsight.net/hive2` `host:port` aksine, ağ geçidi web sitesinin () altında farklı bir uç nokta olarak erişilebilmesini sağlar.

Kimlik doğrulaması için, ağ geçidi kullanıcıların `username:password` kimlik bilgisi çifti kullanarak kimlik doğrulaması yapmasına izin verir. ESP etkin kümeler için bu kimlik bilgisi kullanıcının etki alanı Kullanıcı adı ve parolası olacaktır. Ağ Geçidi aracılığıyla HDInsight kümelerine yönelik kimlik doğrulaması, istemcinin Kerberos bileti almasına gerek yoktur. Ağ Geçidi kimlik bilgilerini `username:password` kabul ettiğinden ve kullanıcının adına kullanıcının Kerberos bileti elde ettiğinden, (ESP) KÜMESINDEN farklı AA-DDS etki alanlarına katılmış istemciler de dahil olmak üzere herhangi bir istemci ana bilgisayarındaki ağ geçidine güvenli bağlantılar yapılabilir.

## <a name="best-practices"></a>En iyi uygulamalar

Ağ Geçidi, istek iletme ve kimlik doğrulamasından sorumlu tek bir hizmettir (iki ana bilgisayar arasında yük dengelenebilir). Ağ Geçidi, belirli bir boyutu aşan Hive sorguları için bir verimlilik sorunu haline gelebilir. Sorgu performansı performansında azalma, ağ geçidinde ODBC veya JDBC aracılığıyla çok büyük **seçim** sorguları yürütüldüğünde gözlemlenebilir. "Çok büyük", satırlar veya sütunlar arasında 5 GB 'den fazla veri oluşturan sorgular anlamına gelir. Bu sorgu, uzun bir satır listesi ve veya geniş bir sütun sayısı içerebilir.

Ağ geçidinin performansı, büyük boyuttaki sorgular etrafında düşeceğinden, verilerin temel alınan veri deposundan (ADLS 2.), HDInsight Hive sunucusu, ağ geçidi ve son olarak istemci konağına JDBC veya ODBC sürücüleri aracılığıyla aktarılması gerekir.

Aşağıdaki diyagramda bir SELECT sorgusunda yer alan adımlar gösterilmektedir.

![Sonuç diyagramı](./media/gateway-best-practices/result-retrieval-diagram.png "Sonuç diyagramı")

Apache Hive,, bir, bir,, Bu soyutlama, Hive içindeki **Select** deyimlerinin dosya sisteminde **okuma** işlemlerine karşılık geldiğini gösterir. **Okuma** işlemleri, kullanıcıya bildirilmeden önce uygun şemaya çevrilir. Bu işlemin gecikmesi, veri boyutu ve son kullanıcıya ulaşmak için gereken toplam atlama sayısını artırır.

Bu komutlar, temel alınan dosya sisteminde **yazma** işlemlerine karşılık geldiği için büyük veri **oluşturma** veya **ekleme** deyimleri yürütürken benzer bir davranış ortaya çıkabilir. Ham ORC gibi verileri, **Insert** veya **Load**kullanarak yüklemek yerine FileSystem/datalake 'a yazmayı düşünün.

Kurumsal güvenlik paketi etkinleştirilmiş kümeler ' de, yeterince karmaşık Apache Ranger ilkeleri sorgu derleme zamanında yavaşlama oluşmasına neden olabilir ve bu da ağ geçidi zaman aşımına neden olabilir. Bir ağ geçidi zaman aşımı bir ESP kümesinde fark alıyorsa, Ranger ilkelerinin sayısını azaltmayı veya birleştirmeyi düşünün.

## <a name="troubleshooting-techniques"></a>Sorun giderme teknikleri

Yukarıdaki davranışın bir parçası olarak karşılanması gereken performans sorunlarını azaltmak ve anlamak için birden çok havalandırma vardır. HDInsight ağ geçidi üzerinde sorgu performansı düşüşünü yaşadığınızda aşağıdaki denetim listesini kullanın:

* Büyük **seçim** sorguları yürütürken **LIMIT** yan tümcesini kullanın. **LIMIT** yan tümcesi, istemci konağına bildirilen toplam satırları azaltır. **LIMIT** yan tümcesi yalnızca sonuç üretimini etkiler ve sorgu planını değiştirmez. **LIMIT** yan tümcesini sorgu planına uygulamak için yapılandırmayı `hive.limit.optimize.enable`kullanın. **Sınır** , **x, y**olan bağımsız değişken form sınırı kullanılarak bir uzaklığa birleştirilebilir.

* **Select \* **kullanmak yerine sorguları **Seç** ' i çalıştırırken ilgilendiğiniz sütunlarınızı adlandırın. Daha az sütun seçilmesi, okunan veri miktarını düşürür.

* Apache Beeline aracılığıyla ilgilendiğiniz sorguyu çalıştırmayı deneyin. Apache Beeline ile sonuç alımı uzun zaman alırsa, dış araçlarla aynı sonuçları alırken gecikmeler olması beklenir.

* HDInsight Gateway ile kurulan bir bağlantının kuruladiğinden emin olmak için temel bir Hive sorgusunu test edin. Tek bir aracın sorunsuz bir şekilde çalıştığından emin olmak için iki veya daha fazla dış araçtan temel bir sorgu çalıştırmayı deneyin.

* HDInsight hizmetlerinin sağlıklı olduğundan emin olmak için tüm Apache ambarı uyarılarını gözden geçirin. Ambarı uyarıları, raporlama ve analiz için Azure Izleyici ile tümleştirilebilir.

* Dış Hive meta veri deposu kullanıyorsanız, veri deposu için Azure SQL DB DTU 'nun sınırına ulaşılmadığını kontrol edin. DTU sınırına yaklaşıyorsa, veritabanı boyutunu artırmanız gerekir.

* PBı veya Tableau gibi üçüncü taraf araçların, tabloları veya veritabanlarını görüntülemek için sayfalandırmayı kullandığından emin olun. Sayfalandırma hakkında yardım almak için bu araçlara yönelik destek iş ortaklarınıza başvurun. Sayfalama için kullanılan ana araç JDBC `fetchSize` parametresidir. Küçük bir getirme boyutu ağ geçidi performansının düşmesine neden olabilir, ancak bir getirme boyutu çok büyük bir ağ geçidi zaman aşımı ile sonuçlanabilir. Getirme boyutu ayarlaması, bir iş yükü temelinde yapılmalıdır.

* Veri işlem hatınız, HDInsight kümesinin temel depolamadaki büyük miktarda veriyi okumayı içeriyorsa, doğrudan Azure Storage ile arabirimler sağlayan bir araç kullanmayı düşünün Azure Data Factory

* LLAP, sorgu sonuçlarının hızla döndürülmesi için daha sorunsuz bir deneyim sağlayabileceğiniz için, etkileşimli iş yüklerini çalıştırırken Apache Hive LLAP kullanmayı düşünün

* Kullanarak `hive.server2.thrift.max.worker.threads`Hive meta veri deposu hizmeti için kullanılabilen iş parçacığı sayısını artırmayı düşünün. Bu ayar özellikle çok sayıda eşzamanlı kullanıcı tarafından kümeye sorgu gönderilirken ilgilidir

* Herhangi bir dış araçlardan gelen ağ geçidine ulaşmak için kullanılan yeniden deneme sayısını azaltın. Birden çok yeniden deneme kullanılıyorsa, bir üstel geri dönme yeniden deneme ilkesini takip etmeyi deneyin

* Yapılandırma ve yapılandırma `hive.exec.compress.output` kullanarak sıkıştırma Hive 'yi `hive.exec.compress.intermediate`etkinleştirmeyi düşünün.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde Apache Beeline](https://docs.microsoft.com/azure/hdinsight/hadoop/apache-hadoop-use-hive-beeline)
* [HDInsight Gateway zaman aşımı sorunlarını giderme adımları](https://docs.microsoft.com/azure/hdinsight/interactive-query/troubleshoot-gateway-timeout)
* [HDInsight için sanal ağlar](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment)
* [Express Route ile HDInsight](https://docs.microsoft.com/azure/hdinsight/connect-on-premises-network)