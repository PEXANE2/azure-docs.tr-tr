---
title: Azure HDInsight kümelerini özelleştirmek için komut dosyası eylemleri geliştirme
description: HDInsight kümelerini özelleştirmek için Bash komut dosyalarını nasıl kullanacağınızı öğrenin. Komut dosyası eylemleri, küme yapılandırma ayarlarını değiştirmek veya ek yazılım yüklemek için küme oluşturma sırasında veya sonrasında komut dosyaları çalıştırmanıza olanak tanır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/28/2019
ms.openlocfilehash: ad9b4b69b0be34c89d03b677c1889e486aae0379
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75931690"
---
# <a name="script-action-development-with-hdinsight"></a>HDInsight ile komut dosyası eylem geliştirme

Bash komut dosyalarını kullanarak HDInsight kümenizi nasıl özelleştirerinizi öğrenin. Komut dosyası eylemleri, küme oluşturma sırasında veya sonrasında HDInsight'ı özelleştirmenin bir yoludur.

## <a name="what-are-script-actions"></a>Komut dosyası eylemleri nelerdir

Komut dosyası eylemleri, Azure'un yapılandırma değişiklikleri yapmak veya yazılım yüklemek için küme düğümlerinde çalıştırdığı Bash komut dosyalarıdır. Komut dosyası eylemi kök olarak yürütülür ve küme düğümlerine tam erişim hakları sağlar.

Komut dosyası eylemleri aşağıdaki yöntemlerle uygulanabilir:

| Komut dosyası uygulamak için bu yöntemi kullanın... | Küme oluşturma sırasında... | Çalışan bir kümede... |
| --- |:---:|:---:|
| Azure portalında |✓ |✓ |
| Azure PowerShell |✓ |✓ |
| Azure Klasik CLI |&nbsp; |✓ |
| HDInsight .NET SDK |✓ |✓ |
| Azure Resource Manager Şablonu |✓ |&nbsp; |

Komut dosyası eylemlerini uygulamak için bu yöntemleri kullanma hakkında daha fazla bilgi için, [komut dosyası eylemlerini kullanarak HDInsight kümelerini özelleştir'e](hdinsight-hadoop-customize-cluster-linux.md)bakın.

## <a name="best-practices-for-script-development"></a><a name="bestPracticeScripting"></a>Komut dosyası geliştirme için en iyi uygulamalar

Bir HDInsight kümesi için özel bir komut dosyası geliştirdiğiniz zaman, göz önünde bulundurulması gereken en iyi birkaç uygulama vardır:

* [Hedef Apache Hadoop sürümü](#bPS1)
* [İşletim Sistemi Sürümünü Hedefle](#bps10)
* [Komut dosyası kaynaklarına kararlı bağlantılar sağlama](#bPS2)
* [Önceden derlenmiş kaynakları kullanma](#bPS4)
* [Küme özelleştirme komut dosyasının iktidarlı olduğundan emin olun](#bPS3)
* [Küme mimarisinin yüksek kullanılabilirliğini sağlama](#bPS5)
* [Azure Blob depolama alanını kullanacak şekilde özel bileşenleri yapılandırma](#bPS6)
* [STDOUT ve STDERR'a bilgi yazın](#bPS7)
* [LF satır sonlarıyla dosyaları ASCII olarak kaydetme](#bps8)
* [Geçici hatalardan kurtarmak için yeniden deneme mantığını kullanma](#bps9)

> [!IMPORTANT]  
> Komut dosyası eylemleri 60 dakika içinde tamamlanmalıdır veya işlem başarısız olur. Düğüm sağlama sırasında komut dosyası diğer kurulum ve yapılandırma işlemleriyle eş zamanlı olarak çalışır. CPU zamanı veya ağ bant genişliği gibi kaynaklar için rekabet, komut dosyasının tamamlanmasının geliştirme ortamınızdakinden daha uzun sürmesine neden olabilir.

### <a name="target-the-apache-hadoop-version"></a><a name="bPS1"></a>Hedef Apache Hadoop sürümü

HDInsight'ın farklı sürümlerihadoop hizmetlerinin ve bileşenlerinin farklı sürümlerine sahiptir. Komut dosyanız bir hizmetin veya bileşenin belirli bir sürümünü bekliyorsa, komut dosyasını yalnızca gerekli bileşenleri içeren HDInsight sürümüyle kullanmanız gerekir. [HDInsight bileşen sürüm](hdinsight-component-versioning.md) belgesini kullanarak HDInsight ile birlikte verilen bileşen sürümleri hakkında bilgi bulabilirsiniz.

### <a name="checking-the-operating-system-version"></a>İşletim sistemi sürümünü denetleme

HDInsight'ın farklı sürümleri Ubuntu'nun belirli sürümlerine dayanır. Komut dosyanızda denetlemeniz gereken işletim sistemi sürümleri arasında farklılıklar olabilir. Örneğin, Ubuntu sürümüne bağlı bir ikili yüklemeniz gerekebilir.

İşletim sistemi sürümünü `lsb_release`kontrol etmek için. Örneğin, aşağıdaki komut dosyası, işletim sistemi sürümüne bağlı olarak belirli bir katran dosyasına nasıl başvurulsüreceğini gösterir:

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS version is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS version is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
```

### <a name="target-the-operating-system-version"></a><a name="bps10"></a>İşletim sistemi sürümünü hedefleme

HDInsight Ubuntu Linux dağıtımıdayanmaktadır. HDInsight'ın farklı sürümleri Ubuntu'nun farklı sürümlerine dayanır ve bu da komut dosyanızın nasıl bir şekilde ki halini değiştirebilir. Örneğin, HDInsight 3.4 ve önceki sürümler Upstart kullanan Ubuntu sürümlerini temel alınmaktadır. 3.5 ve üzeri sürümler Systemd kullanan Ubuntu 16.04'e dayanır. Systemd ve Upstart farklı komutlara dayanır, bu nedenle komut dosyanız her ikisiyle de çalışacak şekilde yazılmalıdır.

HDInsight 3.4 ve 3.5 arasındaki `JAVA_HOME` bir diğer önemli fark da şimdi Java 8'e işaret ediyor olması. Aşağıdaki kod, komut dosyasının Ubuntu 14 veya 16'da mı çalıştığını nasıl belirleyeceklerini gösterir:

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS version is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS version is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
...
if [[ $OS_VERSION == 16* ]]; then
    echo "Using systemd configuration"
    systemctl daemon-reload
    systemctl stop webwasb.service    
    systemctl start webwasb.service
else
    echo "Using upstart configuration"
    initctl reload-configuration
    stop webwasb
    start webwasb
fi
...
if [[ $OS_VERSION == 14* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
elif [[ $OS_VERSION == 16* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
fi
```

Bu parçacıkları içeren tam komut dosyasını https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.

HDInsight tarafından kullanılan Ubuntu sürümü için [HDInsight bileşen sürüm](hdinsight-component-versioning.md) belgesine bakın.

Systemd ve Upstart arasındaki farkları anlamak [için Upstart kullanıcıları için Systemd'e](https://wiki.ubuntu.com/SystemdForUpstartUsers)bakın.

### <a name="provide-stable-links-to-script-resources"></a><a name="bPS2"></a>Komut dosyası kaynaklarına kararlı bağlantılar sağlama

Komut dosyası ve ilişkili kaynaklar kümenin ömrü boyunca kullanılabilir kalmalıdır. Ölçekleme işlemleri sırasında kümeye yeni düğümler eklenirse bu kaynaklar gereklidir.

En iyi yöntem, aboneliğinizdeki her şeyi bir Azure Depolama hesabında indirmek ve arşivlemektir.

> [!IMPORTANT]  
> Kullanılan depolama hesabı küme nin varsayılan depolama hesabı veya başka bir depolama hesabında herkese açık, salt okunur bir kapsayıcı olmalıdır.

Örneğin, Microsoft tarafından sağlanan örnekler [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) depolama hesabında depolanır. Bu konum, HDInsight ekibi tarafından tutulan herkese açık, salt okunur bir kapsayıcıdır.

### <a name="use-pre-compiled-resources"></a><a name="bPS4"></a>Önceden derlenmiş kaynakları kullanma

Komut dosyasını çalıştırmak için gereken süreyi azaltmak için kaynak koddan kaynak derleyen işlemlerden kaçının. Örneğin, kaynakları önceden derleyin ve bunları HDInsight ile aynı veri merkezinde bir Azure Depolama hesabı blob'unda saklayın.

### <a name="ensure-that-the-cluster-customization-script-is-idempotent"></a><a name="bPS3"></a>Küme özelleştirme komut dosyasının iktidarlı olduğundan emin olun

Komut dosyaları idempotent olmalıdır. Komut dosyası birden çok kez çalışıyorsa, kümeyi her seferinde aynı duruma döndürmelidir.

Örneğin, yapılandırma dosyalarını değiştiren bir komut dosyası, birden çok kez çalıştırılırsa yinelenen girişler eklememelidir.

### <a name="ensure-high-availability-of-the-cluster-architecture"></a><a name="bPS5"></a>Küme mimarisinin yüksek kullanılabilirliğini sağlama

Linux tabanlı HDInsight kümeleri küme içinde etkin olan iki kafa düğümü sağlar ve komut dosyası eylemleri her iki düğümde de çalışır. Yüklediğiniz bileşenler yalnızca bir kafa düğümü bekliyorsa, bileşenleri her iki kafa düğümüne de yüklemeyin.

> [!IMPORTANT]  
> HDInsight'ın bir parçası olarak sağlanan hizmetler, gerektiğinde iki baş düğümü arasında başarısız olacak şekilde tasarlanmıştır. Bu işlevsellik komut dosyası eylemleri aracılığıyla yüklenen özel bileşenlere genişletilmez. Özel bileşenler için yüksek kullanılabilirliğe ihtiyacınız varsa, kendi arıza mekanizmasınızı uygulamanız gerekir.

### <a name="configure-the-custom-components-to-use-azure-blob-storage"></a><a name="bPS6"></a>Azure Blob depolama alanını kullanacak şekilde özel bileşenleri yapılandırma

Kümeye yüklediğiniz bileşenler, Apache Hadoop Distributed File System (HDFS) depolamasını kullanan varsayılan bir yapılandırmaya sahip olabilir. HDInsight varsayılan depolama alanı olarak Azure Depolama veya Veri Gölü Depolama'yı kullanır. Her ikisi de küme silinse bile verileri kalıcı bir HDFS uyumlu dosya sistemi sağlar. HDFS yerine WASB veya ADL kullanmak için yüklediğiniz bileşenleri yapılandırmanız gerekebilir.

Çoğu işlem için dosya sistemini belirtmeniz gerekmez. Örneğin, aşağıdaki kopyaları hadoop-common.jar dosyası yerel dosya sisteminden küme depolama için:

```bash
hdfs dfs -put /usr/hdp/current/hadoop-client/hadoop-common.jar /example/jars/
```

Bu örnekte, `hdfs` komut saydam olarak varsayılan küme depolama kullanır. Bazı işlemler için URI'yi belirtmeniz gerekebilir. Örneğin, `adl:///example/jars` Azure Veri Gölü Depolama `abfs:///example/jars` Gen1 için, Veri `wasb:///example/jars` Gölü Depolama Gen2 için veya Azure Depolama için.

### <a name="write-information-to-stdout-and-stderr"></a><a name="bPS7"></a>STDOUT ve STDERR'a bilgi yazın

HDInsight, STDOUT ve STDERR'a yazılan komut dosyası çıktısını günlükler. Bu bilgileri Ambari web Kullanıcı Arama Bilgi Sini'ni kullanarak görüntüleyebilirsiniz.

> [!NOTE]  
> Apache Ambari yalnızca küme başarıyla oluşturulduğunda kullanılabilir. Küme oluşturma sırasında bir komut dosyası eylemi kullanırsanız ve oluşturma başarısız olursa, günlüğe kaydedilmiş bilgilere erişmek için [komut dosyası eylemini kullanarak HDInsight kümelerini özelleştir](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) sorun giderme bölümüne bakın.

Çoğu yardımcı program ve yükleme paketleri zaten STDOUT ve STDERR'ye bilgi yazar, ancak ek günlüğe kaydetme yi de ekleyebilirsiniz. STDOUT'a metin göndermek `echo`için . Örnek:

```bash
echo "Getting ready to install Foo"
```

Varsayılan olarak, `echo` dize STDOUT gönderir. STDERR'ye yönlendirmek için `>&2` `echo`önce ekleyin. Örnek:

```bash
>&2 echo "An error occurred installing Foo"
```

Bu, STDOUT'a yazılan bilgileri STDERR 'e (2) yönlendirir. IO yeniden yönlendirmesi hakkında [https://www.tldp.org/LDP/abs/html/io-redirection.html](https://www.tldp.org/LDP/abs/html/io-redirection.html)daha fazla bilgi için bkz.

Komut dosyası eylemleri tarafından günlüğe kaydedilen bilgileri görüntüleme hakkında daha fazla bilgi için, [komut dosyası eylemini kullanarak HDInsight kümelerini özelleştir'e](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) bakın

### <a name="save-files-as-ascii-with-lf-line-endings"></a><a name="bps8"></a>LF satır sonlarıyla dosyaları ASCII olarak kaydetme

Bash komut dosyaları ASCII biçimi olarak depolanmalıdır ve satırlar LF tarafından sonlandırılmalıdır. UTF-8 olarak depolanan veya satır sonu olarak CRLF kullanan dosyalar aşağıdaki hatayla başarısız olabilir:

```
$'\r': command not found
line 1: #!/usr/bin/env: No such file or directory
```

### <a name="use-retry-logic-to-recover-from-transient-errors"></a><a name="bps9"></a>Geçici hatalardan kurtarmak için yeniden deneme mantığını kullanma

Dosya indirirken, apt-get kullanarak paket yüklerken veya internet üzerinden veri aktaran diğer eylemlerde, geçici ağ hataları nedeniyle eylem başarısız olabilir. Örneğin, iletişim kurduğunuz uzak kaynak, yedek düğümüzerinde başarısız olma sürecinde olabilir.

Komut dosyanızı geçici hatalara karşı esnek hale getirmek için yeniden deneme mantığını uygulayabilirsiniz. Aşağıdaki işlev, yeniden deneme mantığının nasıl uygulanacağını gösterir. Başarısız olmadan önce işlemi üç kez yeniden dener.

```bash
#retry
MAXATTEMPTS=3

retry() {
    local -r CMD="$@"
    local -i ATTMEPTNUM=1
    local -i RETRYINTERVAL=2

    until $CMD
    do
        if (( ATTMEPTNUM == MAXATTEMPTS ))
        then
                echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                return 1
        else
                echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                sleep $(( RETRYINTERVAL ))
                ATTMEPTNUM=$ATTMEPTNUM+1
        fi
    done
}
```

Aşağıdaki örnekler, bu işlevin nasıl kullanılacağını göstermektedir.

```bash
retry ls -ltr foo

retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
```

## <a name="helper-methods-for-custom-scripts"></a><a name="helpermethods"></a>Özel komut dosyaları için yardımcı yöntemler

Komut dosyası eylem yardımcısı yöntemleri, özel komut dosyaları yazarken kullanabileceğiniz yardımcı programlardır. Bu yöntemler komut [https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh) dosyasında yer almaktadır. Bunları indirmek ve komut dosyanızın bir parçası olarak kullanmak için aşağıdakileri kullanın:

```bash
# Import the helper method module.
wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh
```

Komut dosyanızda kullanılabilir aşağıdaki yardımcılar:

| Yardımcı kullanımı | Açıklama |
| --- | --- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` |Kaynak URI'den belirtilen dosya yoluna bir dosya indirir. Varsayılan olarak, varolan bir dosyanın üzerine yazmıyor. |
| `untar_file TARFILE DESTDIR` |Hedef dizine katran `-xf`lı bir dosya ayıklar (kullanarak). |
| `test_is_headnode` |Küme baş düğümünde çalıştırılırsa, 1 döndürün; aksi takdirde, 0. |
| `test_is_datanode` |Geçerli düğüm bir veri (alt) düğümise, 1 döndürün; aksi takdirde, 0. |
| `test_is_first_datanode` |Geçerli düğüm ilk veri (alt) düğüm (işçi düğümü olarak adlandırılır) ise 1 döndürür; aksi takdirde, 0. |
| `get_headnodes` |Kümedeki headnode'lerin tam nitelikli etki alanı adını döndürün. İsimler virgül sınırlıdır. Boş bir dize hata üzerinde döndürülür. |
| `get_primary_headnode` |Birincil başlık tam nitelikli etki alanı adını alır. Boş bir dize hata üzerinde döndürülür. |
| `get_secondary_headnode` |İkincil başlık tam nitelikli etki alanı adını alır. Boş bir dize hata üzerinde döndürülür. |
| `get_primary_headnode_number` |Birincil headnod'un sayısal sonekini alır. Boş bir dize hata üzerinde döndürülür. |
| `get_secondary_headnode_number` |İkincil headnod'un sayısal sonekini alır. Boş bir dize hata üzerinde döndürülür. |

## <a name="common-usage-patterns"></a><a name="commonusage"></a>Yaygın kullanım alışkanlıkları

Bu bölümde, kendi özel komut dosyanızı yazarken karşılaşılababileceğiniz bazı yaygın kullanım desenleri uygulanması na ilişkin kılavuz lar sağlanmaktadır.

### <a name="passing-parameters-to-a-script"></a>Parametreleri komut dosyasına aktarma

Bazı durumlarda, komut dosyanız parametreler gerektirebilir. Örneğin, Ambari REST API'sini kullanırken küme için yönetici parolası gerekebilir.

Komut dosyasına geçirilen *parametreler konumsal parametreler*olarak `$1` bilinir ve ilk `$2` parametre için, ikinci ve benzeri için atanır. `$0`komut dosyasının adını içerir.

Parametreler olarak komut dosyasına geçirilen değerler tek tırnak (') ile eklenmelidir. Bunu yapmak, geçirilen değerin gerçek bir değer olarak kabul edilmesini sağlar.

### <a name="setting-environment-variables"></a>Ortam değişkenlerini ayarlama

Bir ortam değişkenini ayarlama aşağıdaki ifadeyle gerçekleştirilir:

    VARIABLENAME=value

DEĞIŞKENNAME değişkenin adıdır. Değişkene erişmek için `$VARIABLENAME`. Örneğin, konumel parametre tarafından sağlanan bir değeri PAROLA adlı bir ortam değişkeni olarak atamak için aşağıdaki deyimi kullanırsınız:

    PASSWORD=$1

Bilgilere sonraki erişim daha `$PASSWORD`sonra kullanabilirsiniz.

Komut dosyası içinde ayarlanan ortam değişkenleri yalnızca komut dosyası kapsamında bulunur. Bazı durumlarda, komut dosyası tamamlandıktan sonra devam edecek sistem genelinde ortam değişkenleri eklemeniz gerekebilir. Sistem genelinde ortam değişkenleri eklemek için `/etc/environment`değişkeni . Örneğin, aşağıdaki ifade `HADOOP_CONF_DIR`ekler:

```bash
echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment
```

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Özel komut dosyalarının depolandığı konumlara erişim

Bir kümeyi özelleştirmek için kullanılan komut dosyalarının aşağıdaki konumlardan birinde depolanması gerekir:

* Kümeyle ilişkili bir __Azure Depolama hesabı.__

* Kümeyle ilişkili __ek bir depolama hesabı.__

* __Halka açık bir URI.__ Örneğin, OneDrive, Dropbox veya diğer dosya barındırma hizmetinde depolanan verilere url.

* HDInsight kümesiyle ilişkili bir __Azure Veri Gölü Depolama hesabı.__ HDInsight ile Azure Veri Gölü Depolamasını kullanma hakkında daha fazla bilgi için [Bkz. Quickstart: HDInsight'ta kümeler ayarlayın.](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)

    > [!NOTE]  
    > HDInsight'ın Veri Gölü Depolama'ya erişmek için kullandığı hizmet sorumlusu, komut dosyasına okuma erişimine sahip olmalıdır.

Komut dosyası tarafından kullanılan kaynaklar da genel kullanıma sunulmalıdır.

Dosyaları bir Azure Depolama hesabında veya Azure Veri Gölü Depolama'da depolamak, her ikisi de Azure ağında olduğu gibi hızlı erişim sağlar.

> [!NOTE]  
> Komut dosyasına başvurmak için kullanılan URI biçimi, kullanılan hizmete bağlı olarak değişir. HDInsight kümesiyle ilişkili depolama hesapları `wasb://` `wasbs://`için, kullanın veya . Herkesin bildiği URL'ler için, kullanın `http://` veya. `https://` Veri Gölü Depolama `adl://`için.

## <a name="checklist-for-deploying-a-script-action"></a><a name="deployScript"></a>Komut dosyası eylemini dağıtmak için denetim listesi

Komut dosyası dağıtmaya hazırlanırken atılacak adımlar şunlardır:

* Özel komut dosyalarını içeren dosyaları dağıtım sırasında küme düğümleri tarafından erişilebilen bir yere koyun. Örneğin, küme için varsayılan depolama. Dosyalar, herkese açık barındırma hizmetlerinde de depolanabilir.
* Komut dosyasının iktidarlı olduğundan doğrulayın. Bunu yapmak, komut dosyasının aynı düğümüzerinde birden çok kez yürütülmesini sağlar.
* Komut dosyaları tarafından kullanılan indirilen dosyaları tutmak için geçici bir dosya dizini /tmp kullanın ve komut dosyaları yürütüldükten sonra bunları temizleyin.
* İşletim sistemi düzeyindeki ayarlar veya Hadoop hizmet yapılandırma dosyaları değiştirilirse, HDInsight hizmetlerini yeniden başlatmak isteyebilirsiniz.

## <a name="how-to-run-a-script-action"></a><a name="runScriptAction"></a>Komut dosyası eylemi nasıl çalıştırılır?

Aşağıdaki yöntemleri kullanarak HDInsight kümelerini özelleştirmek için komut dosyası eylemlerini kullanabilirsiniz:

* Azure portalında
* Azure PowerShell
* Azure Resource Manager şablonları
* The HDInsight .NET SDK.

Her yöntemi kullanma hakkında daha fazla bilgi için [komut dosyası eyleminin nasıl kullanılacağına](hdinsight-hadoop-customize-cluster-linux.md)bakın.

## <a name="custom-script-samples"></a><a name="sampleScripts"></a>Özel komut dosyası örnekleri

Microsoft, bileşenleri bir HDInsight kümesine yüklemek için örnek komut dosyaları sağlar. Bkz. [HDInsight kümelerinde Hue'yu örnek](hdinsight-hadoop-hue-linux.md) bir komut dosyası eylemi olarak yükle ve kullan.

## <a name="troubleshooting"></a>Sorun giderme

Geliştirdiğiniz komut dosyalarını kullanırken karşılaşabileceğiniz hatalar şunlardır:

**Hata** `$'\r': command not found`: . Bazen takip `syntax error: unexpected end of file`eder.

*Neden*: Bu hata, bir komut dosyasındaki satırlar CRLF ile sona erdiğinde neden olur. Unix sistemleri satır bitiş olarak sadece LF bekliyoruz.

CRLF, Windows'daki birçok metin düzenleyicisi için ortak bir satır bitişi olduğundan, bu sorun genellikle komut dosyası Bir Windows ortamında yazıldığında oluşur.

*Çözünürlük*: Metin düzenleyicinizde bir seçenekse, satır bitişi için Unix biçimini veya LF'yi seçin. CRLF'yi LF olarak değiştirmek için bir Unix sisteminde aşağıdaki komutları da kullanabilirsiniz:

> [!NOTE]  
> Aşağıdaki komutlar, CRLF satır sonlarını LF olarak değiştirmeleri açısından kabaca eşdeğerdir. Sisteminizde bulunan yardımcı programlara göre bir tane seçin.

| Komut | Notlar |
| --- | --- |
| `unix2dos -b INFILE` |Özgün dosya bir . BAK uzantısı |
| `tr -d '\r' < INFILE > OUTFILE` |OUTFILE yalnızca LF uçları içeren bir sürüm içerir |
| `perl -pi -e 's/\r\n/\n/g' INFILE` | Dosyayı doğrudan değiştirir |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` |OUTFILE yalnızca LF uçları içeren bir sürüm içerir. |

**Hata** `line 1: #!/usr/bin/env: No such file or directory`: .

*Neden*: Bu hata, komut dosyası Byte Order Mark (BOM) ile UTF-8 olarak kaydedildiğinde oluşur.

*Çözünürlük*: Dosyayı ASCII veya KAYNAK BOM olmadan UTF-8 olarak kaydedin. Ayrıca, bir Linux veya Unix sisteminde, BOM'suz bir dosya oluşturmak için aşağıdaki komutu kullanabilirsiniz:

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

BOM içeren dosyayla değiştirin. `INFILE` `OUTFILE`komut dosyasını, ÜRÜN OLARAK içeren yeni bir dosya adı olmalıdır.

## <a name="next-steps"></a><a name="seeAlso"></a>Sonraki adımlar

* [Komut dosyası eylemini kullanarak HDInsight kümelerini nasıl özelleştirebilirsiniz](hdinsight-hadoop-customize-cluster-linux.md) öğrenin
* HDInsight'ı yöneten .NET uygulamaları oluşturma hakkında daha fazla bilgi edinmek için [HDInsight .NET SDK başvurusunu](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight) kullanın
* HDInsight kümelerinde yönetim eylemleri gerçekleştirmek için REST'i nasıl kullanacağınızı öğrenmek için [HDInsight REST API'sini](https://msdn.microsoft.com/library/azure/mt622197.aspx) kullanın.
