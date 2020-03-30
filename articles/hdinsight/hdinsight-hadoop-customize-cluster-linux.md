---
title: Komut dosyası eylemlerini kullanarak Azure HDInsight kümelerini özelleştirme
description: Komut dosyası eylemlerini kullanarak HDInsight kümelerine özel bileşenler ekleyin. Komut dosyası eylemleri, küme yapılandırmasını özelleştirmek veya Hue, Solr veya R gibi ek hizmetler ve yardımcı programlar eklemek için kullanılabilecek Bash komut dosyalarıdır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/26/2020
ms.openlocfilehash: 12e6892930afe8ba9c7bad9b05fd39eeaf8835fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79272506"
---
# <a name="customize-azure-hdinsight-clusters-by-using-script-actions"></a>Komut dosyası eylemlerini kullanarak Azure HDInsight kümelerini özelleştirme

Azure HDInsight, kümeyi özelleştirmek için özel komut dosyaları çağıran **komut dosyası eylemleri** adı verilen bir yapılandırma yöntemi sağlar. Bu komut dosyaları ek bileşenleri yüklemek ve yapılandırma ayarlarını değiştirmek için kullanılır. Komut dosyası eylemleri küme oluşturma sırasında veya sonrasında kullanılabilir.

Komut dosyası eylemleri, HDInsight uygulaması olarak Azure Marketi'nde de yayımlanabilir. HDInsight uygulamaları hakkında daha fazla bilgi için [bkz.](hdinsight-apps-publish-applications.md)

## <a name="permissions"></a>İzinler

Etki alanı birleştirilmiş HDInsight kümesi için, kümeyle komut dosyası eylemleri kullandığınızda gereken iki Apache Ambari izni vardır:

* **AMBARI. \_ÖZEL KOMUT Çalıştırın.\_** Ambari Yönetici rolü varsayılan olarak bu izne sahiptir.
* **KÜME. \_ÖZEL KOMUT Çalıştırın.\_** Hem HDInsight Cluster Administrator hem de Ambari Administrator varsayılan olarak bu izne sahiptir.

Etki alanı yla birleştirilmiş HDInsight ile izinlerle çalışma hakkında daha fazla bilgi için, [Kurumsal Güvenlik Paketi ile HDInsight kümelerini yönet'e](./domain-joined/apache-domain-joined-manage.md)bakın.

## <a name="access-control"></a>Erişim denetimi

Azure aboneliğinizin yöneticisi veya sahibi değilseniz, hesabınızda en azından HDInsight kümesini içeren kaynak grubuna Katkıda bulunan ların erişimi olmalıdır.

Bir HDInsight kümesi oluşturursanız, Azure aboneliğine en az Katkıda Bulunan kişi olan birinin sağlayıcıyı daha önce HDInsight için kaydetmiş olması gerekir. Sağlayıcı kaydı, aboneliğe Katkıda Bulunan erişimi olan bir kullanıcı abonelikte ilk kez kaynak oluşturduğunda gerçekleşir. [Rest kullanarak bir sağlayıcı kaydettirecekseniz,](https://msdn.microsoft.com/library/azure/dn790548.aspx)kaynak oluşturmadan da yapılabilir.

Erişim yönetimi yle çalışma hakkında daha fazla bilgi alın:

* [Azure portalında erişim yönetimi ile çalışmaya başlama](../role-based-access-control/overview.md)
* [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanın](../role-based-access-control/role-assignments-portal.md)

## <a name="understand-script-actions"></a>Komut dosyası eylemlerini anlama

Komut dosyası eylemi, HDInsight kümesindeki düğümler üzerinde çalışan Bash komut dosyasıdır. Komut dosyası eylemlerinin özellikleri ve özellikleri aşağıdaki gibidir:

* HDInsight kümesinden erişilebilen bir URI'de depolanmalıdır. Olası depolama yerleri şunlardır:

    * Normal kümeler için:

      * ADLS Gen1: HDInsight'ın Veri Gölü Depolama'ya erişmek için kullandığı hizmet ilkesi, komut dosyasına okuma erişimine sahip olmalıdır. Veri Gölü Depolama Gen1'de depolanan komut `adl://DATALAKESTOREACCOUNTNAME.azuredatalakestore.net/path_to_file`dosyaları için URI biçimi.

      * BIR Azure Depolama hesabında, HDInsight kümesinin birincil veya ek depolama hesabı olan bir leke. HDInsight'a küme oluşturma sırasında bu tür depolama hesaplarının her ikisine de erişim hakkı verilir.

        > [!IMPORTANT]  
        > Bu Azure Depolama hesabındaki depolama anahtarını döndürmeyin, çünkü bu komut dosyalarının başarısız olmasına neden olur.

      * http:// yollardan erişilebilen genel dosya paylaşım hizmeti. Bunlara örnek olarak Azure Blob, GitHub, OneDrive verilebilir. Örneğin [ÜRB'ler,](#example-script-action-scripts)bkz.

     * ESP'li kümeler için wasb:// veya wasbs:// veya http[s]:// URI'ler desteklenir.

* Yalnızca belirli düğüm türlerinde çalıştırmak için kısıtlanabilir. Örnekler baş düğümleri veya işçi düğümleri vardır.

* Kalıcı veya geçici olabilir.

    Kalıcı komut dosyası eylemlerinin benzersiz bir adı olmalıdır. Kalıcı komut dosyaları, ölçekleme işlemleri yoluyla kümeye eklenen yeni alt düğümleri özelleştirmek için kullanılır. Kalıcı bir komut dosyası, ölçekleme işlemleri gerçekleştiğinde başka bir düğüm türüne de değişiklikler uygulayabilir. Bir örnek bir baş düğümüdür.

    Özel komut dosyaları kalıcı değildir. Küme oluşturma sırasında kullanılan komut dosyası eylemleri otomatik olarak kalıcıdır. Komut dosyası çalıştırdıktan sonra kümeye eklenen alt düğümlere uygulanmaz. Ardından geçici bir komut dosyasını kalıcı bir komut dosyasına tanıtabilir veya kalıcı bir komut dosyasını geçici bir komut dosyasına düşürebilirsiniz. Başarısız olan komut dosyaları, olması gerektiğini özellikle belirtseniz bile kalıcı değildir.

* Yürütme sırasında komut dosyası tarafından kullanılan parametreleri kabul edebilir.

* Küme düğümlerinde kök düzeyi ayrıcalıkları ile çalıştırın.

* Azure portalı, Azure PowerShell, Azure CLI veya HDInsight .NET SDK üzerinden kullanılabilir.

Küme, çalıştırılmayan tüm komut dosyalarının geçmişini tutar. Geçmiş, promosyon veya indirgeme işlemleri için bir komut dosyasının kimliğini bulmanız gerektiğinde yardımcı olur.

> [!IMPORTANT]  
> Komut dosyası eylemi tarafından yapılan değişiklikleri geri almanın otomatik bir yolu yoktur. Değişiklikleri el ile tersine çevirin veya bunları tersine çeviren bir komut dosyası sağlayın.

### <a name="script-action-in-the-cluster-creation-process"></a>Küme oluşturma işleminde komut dosyası eylemi

Küme oluşturma sırasında kullanılan komut dosyası eylemleri, varolan bir kümede çalıştırılankomut dosyası eylemlerinden biraz farklıdır:

* Komut dosyası otomatik olarak kalıcıdır.

* Komut dosyasındaki bir hata küme oluşturma işleminin başarısız olması yla ilgili olabilir.

Aşağıdaki diyagram, komut dosyası eyleminin oluşturma işlemi sırasında ne zaman çalıştığını gösterir:

![HDInsight küme özelleştirme ve küme oluşturma sırasında aşamaları][img-hdi-cluster-states]

HDInsight yapılandırılırken komut dosyası çalışır. Komut dosyası kümede belirtilen tüm düğümlerde paralel olarak çalışır. Düğümlerde kök ayrıcalıkları ile çalışır.

Apache Hadoop ile ilgili hizmetler de dahil olmak üzere hizmetleri durdurma ve başlatma gibi işlemleri gerçekleştirebilirsiniz. Hizmetleri durdurursanız, komut dosyası bitmeden ambari hizmetinin ve Hadoop'la ilgili diğer hizmetlerin çalıştırdığından emin olun. Bu hizmetler, oluşturulurken kümenin durumunu ve durumunu başarıyla belirlemek için gereklidir.

Küme oluşturma sırasında, aynı anda birçok komut dosyası eylemleri kullanabilirsiniz. Bu komut dosyaları, belirtilen sırayla çağrılır.

> [!IMPORTANT]  
> Komut dosyası eylemleri 60 dakika içinde bitmeli, yoksa zaman ları dolmalı. Küme sağlama sırasında komut dosyası diğer kurulum ve yapılandırma işlemleriyle aynı anda çalışır. CPU zamanı veya ağ bant genişliği gibi kaynaklar için rekabet, komut dosyasının tamamlanmasının geliştirme ortamınızdakinden daha uzun sürmesine neden olabilir.
>
> Komut dosyasını çalıştırmak için gereken süreyi en aza indirmek için, uygulamaları kaynaktan indirme ve derleme gibi görevlerden kaçının. Uygulamaları önceden derleyin ve ikiliyi Azure Depolama'da saklayın.

### <a name="script-action-on-a-running-cluster"></a>Çalışan kümede komut dosyası eylemi

Zaten çalışan bir kümeüzerinde çalışan bir komut dosyasında bir hata, kümenin otomatik olarak başarısız bir duruma değişmesine neden olmaz. Komut dosyası bittikten sonra küme çalışan duruma geri dönmelidir. Kümenin çalışan bir durumu olsa bile, başarısız komut dosyası bazı şeyleri kırmış olabilir. Örneğin, bir komut dosyası küme tarafından gereken dosyaları silebilir.

Komut dosyaları eylemleri kök ayrıcalıkları ile çalışır. Kümenize uygulamadan önce bir komut dosyasının ne yaptığını anladığınızdan emin olun.

Bir kümeye komut dosyası uyguladığınız zaman, küme durumu **Çalışandan** **Kabul'e**değişir. Ardından **HDInsight yapılandırmasına** ve son olarak başarılı komut dosyaları için **Çalıştırma'ya** geri döner. Komut dosyası durumu komut dosyası eylem geçmişinde günlüğe kaydedilir. Bu bilgiler, komut dosyasının başarılı olup olmadığını veya başarısız olup olmadığını bildirir. Örneğin, `Get-AzHDInsightScriptActionHistory` PowerShell cmdlet bir komut dosyasının durumunu gösterir. Aşağıdaki metne benzer bilgileri döndürür:

    ScriptExecutionId : 635918532516474303
    StartTime         : 8/14/2017 7:40:55 PM
    EndTime           : 8/14/2017 7:41:05 PM
    Status            : Succeeded

> [!IMPORTANT]  
> Küme oluşturulduktan sonra küme kullanıcısını, yöneticiyi, parolayı değiştirirseniz, bu kümeye karşı çalışan komut dosyası eylemleri başarısız olabilir. Alt düğümleri hedefleyen kalıcı komut dosyası eylemlerine sahipseniz, kümeyi ölçeklendirdiğinizde bu komut dosyaları başarısız olabilir.

## <a name="example-script-action-scripts"></a>Örnek komut dosyası eylem komut dosyaları

Komut dosyası eylem komut dosyaları aşağıdaki yardımcı programlar aracılığıyla kullanılabilir:

* Azure portalında
* Azure PowerShell
* Azure CLI
* HDInsight .NET SDK

HDInsight, aşağıdaki bileşenleri HDInsight kümelerine yüklemek için komut dosyaları sağlar:

| Adı | Betik |
| --- | --- |
| Azure Depolama hesabı ekleme |`https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh`. Bkz. [HDInsight'a ek depolama hesapları ekleyin.](hdinsight-hadoop-add-storage.md) |
| Ton Yükle |`https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh`. [HDInsight Hadoop kümelerinde Hue'yi yükle ve kullan.](hdinsight-hadoop-hue-linux.md) |
| Preload Hive kütüphaneleri |`https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh`. Bkz. [HDInsight kümenizi oluştururken özel Apache Hive kitaplıkları ekleyin.](hdinsight-hadoop-add-hive-libraries.md) |

## <a name="script-action-during-cluster-creation"></a>Küme oluşturma sırasında komut dosyası eylemi

Bu bölümde, bir HDInsight kümesi oluştururken komut dosyası eylemlerini kullanabileceğiniz farklı yollar açıklanmaktadır.

### <a name="use-a-script-action-during-cluster-creation-from-the-azure-portal"></a>Azure portalından küme oluşturma sırasında komut dosyası eylemi kullanma

1. [Azure portalını kullanarak HDInsight'ta Linux tabanlı kümeler oluştur'da](hdinsight-hadoop-create-linux-clusters-portal.md)açıklandığı gibi bir küme oluşturmaya başlayın. Yapılandırma **+ fiyatlandırma** sekmesinden , **+ Komut dosyası eylemi ekle'yi**seçin.

    ![Azure portal kümesi komut dosyası eylemi](./media/hdinsight-hadoop-customize-cluster-linux/azure-portal-cluster-configuration-scriptaction.png)

1. Önceden yapılmış bir komut dosyası seçmek için komut dosyası girişini __seçin.__ Özel bir komut dosyası kullanmak için __Özel'i__seçin. Ardından komut dosyanız için __Ad__ ve __Bash komut dosyası URI'yi__ sağlayın.

    ![Seçili komut dosyası formuna komut dosyası ekleme](./media/hdinsight-hadoop-customize-cluster-linux/hdinsight-select-script.png)

    Aşağıdaki tabloda formdaki öğeler açıklanmaktadır:

    | Özellik | Değer |
    | --- | --- |
    | Komut dosyası seçme | Kendi komut dosyanızı kullanmak için __Özel'i__seçin. Aksi takdirde, sağlanan komut dosyalarından birini seçin. |
    | Adı |Komut dosyası eylemi için bir ad belirtin. |
    | Bash script URI |Komut dosyasının URI'sini belirtin. |
    | Baş/İşçi/ZooKeeper |Komut dosyasının çalıştırıldığı düğümleri belirtin: **Baş**, **İşçi**veya **ZooKeeper**. |
    | Parametreler |Komut dosyası tarafından gerekirse parametreleri belirtin. |

    Komut dosyası ölçekleme işlemleri sırasında uygulandığından emin olmak için bu komut dosyası eylem girişini __sada__ kullanın.

1. Komut dosyasını kaydetmek için __Oluştur'u__ seçin. Sonra başka bir komut dosyası eklemek için __+ Yeni gönder'i__ kullanabilirsiniz.

    ![HDInsight birden çok komut dosyası eylemi](./media/hdinsight-hadoop-customize-cluster-linux/multiple-scripts-actions.png)

    Komut dosyaları eklemeyi bitirdiğinizde, Yapılandırma **+ fiyatlandırma** sekmesine geri dönersiniz.

1. Kalan küme oluşturma adımlarını her zamanki gibi tamamlayın.

### <a name="use-a-script-action-from-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonlarından komut dosyası eylemi kullanma

Komut dosyası eylemleri Azure Kaynak Yöneticisi şablonları ile kullanılabilir. Örneğin, [hdinsight Linux Cluster oluştur'a bakın ve bir komut dosyası eylemi çalıştırın.](https://azure.microsoft.com/resources/templates/hdinsight-linux-run-script-action/)

Bu örnekte, komut dosyası eylemi aşağıdaki kod kullanılarak eklenir:

```json
"scriptActions": [
    {
        "name": "setenvironmentvariable",
        "uri": "[parameters('scriptActionUri')]",
        "parameters": "headnode"
    }
]
```

Şablonun nasıl dağıtılanöğretilen hakkında daha fazla bilgi alın:

* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)

* [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynakları dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli)

### <a name="use-a-script-action-during-cluster-creation-from-azure-powershell"></a>Azure PowerShell'den küme oluşturma sırasında komut dosyası eylemi kullanma

Bu bölümde, bir kümeyi özelleştirmek için komut dosyalarını çağırmak için [Add-AzHDInsightScriptAction](https://docs.microsoft.com/powershell/module/az.hdinsight/add-azhdinsightscriptaction) cmdlet'i kullanırsınız. Başlamadan önce Azure PowerShell'i yüklediğinizden ve yapılandırdığınızdan emin olun. Bu PowerShell komutlarını kullanmak için [AZ Modülü'ne](https://docs.microsoft.com/powershell/azure/overview)ihtiyacınız vardır.

Aşağıdaki komut dosyası, PowerShell kullanarak bir küme oluşturduğunuzda komut dosyası eyleminin nasıl uygulanacağınızı gösterir:

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=5-90)]

Kümenin oluşturulması birkaç dakika sürebilir.

### <a name="use-a-script-action-during-cluster-creation-from-the-hdinsight-net-sdk"></a>HDInsight .NET SDK'dan küme oluşturma sırasında komut dosyası eylemi kullanma

HDInsight .NET SDK, bir .NET uygulamasından HDInsight ile çalışmayı kolaylaştıran istemci kitaplıkları sağlar. Kod örneği için [Bkz. Komut Dosyası Eylemleri.](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight?view=azure-dotnet#script-actions)

## <a name="script-action-to-a-running-cluster"></a>Çalışan kümeye komut dosyası eylemi

Bu bölümde, çalışan bir kümeye komut dosyası eylemlerinin nasıl uygulanacağı açıklanmaktadır.

### <a name="apply-a-script-action-to-a-running-cluster-from-the-azure-portal"></a>Azure portalından çalışan kümeye komut dosyası eylemi uygulama

1. [Azure portalında](https://portal.azure.com) oturum açın ve kümenizi bulun.

1. Varsayılan görünümden Ayarlar **Settings** **altında, Komut Dosyası eylemlerini**seçin.

1. **Komut Dosyası eylemleri** sayfasının üst kısmından + Yeni **Gönder'i**seçin.

    ![Çalışan kümeye komut dosyası ekleme](./media/hdinsight-hadoop-customize-cluster-linux/add-script-running-cluster.png)

1. Önceden yapılmış bir komut dosyası seçmek için komut dosyası girişini __seçin.__ Özel bir komut dosyası kullanmak için __Özel'i__seçin. Ardından komut dosyanız için __Ad__ ve __Bash komut dosyası URI'yi__ sağlayın.

    ![Seçili komut dosyası formuna komut dosyası ekleme](./media/hdinsight-hadoop-customize-cluster-linux/hdinsight-select-script.png)

    Aşağıdaki tabloda formdaki öğeler açıklanmaktadır:

    | Özellik | Değer |
    | --- | --- |
    | Komut dosyası seçme | Kendi komut dosyanızı kullanmak için __özel'i__seçin. Aksi takdirde, sağlanan bir komut dosyası seçin. |
    | Adı |Komut dosyası eylemi için bir ad belirtin. |
    | Bash script URI |Komut dosyasının URI'sini belirtin. |
    | Baş/İşçi/Zookeeper |Komut dosyasının çalıştırıldığı düğümleri belirtin: **Baş**, **İşçi**veya **ZooKeeper**. |
    | Parametreler |Komut dosyası tarafından gerekirse parametreleri belirtin. |

    Komut __dosyasının__ ölçekleme işlemleri sırasında uygulandığından emin olmak için bu komut dosyası eylem girişini devam ettirmek'i kullanın.

1. Son olarak, komut dosyasını kümeye uygulamak için **Oluştur** düğmesini seçin.

### <a name="apply-a-script-action-to-a-running-cluster-from-azure-powershell"></a>Azure PowerShell'den çalışan kümeye komut dosyası eylemi uygulama

Bu PowerShell komutlarını kullanmak için [AZ Modülü'ne](https://docs.microsoft.com/powershell/azure/overview)ihtiyacınız vardır. Aşağıdaki örnek, çalışan bir kümeye komut dosyası eyleminin nasıl uygulanacağı gösterilmektedir:

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=105-117)]

İşlem bittikten sonra, aşağıdaki metne benzer bilgiler alırsınız:

    OperationState  : Succeeded
    ErrorMessage    :
    Name            : Giraph
    Uri             : https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    Parameters      :
    NodeTypes       : {HeadNode, WorkerNode}

### <a name="apply-a-script-action-to-a-running-cluster-from-the-azure-cli"></a>Azure CLI'den çalışan kümeye komut dosyası eylemi uygulama

Başlamadan önce Azure CLI'yi yüklediğinizden ve yapılandırdığınızdan emin olun. En son sürüme sahip olduğundan emin olun. Daha fazla bilgi için azure [cli'yi yükleyin.](https://docs.microsoft.com/cli/azure/install-azure-cli)

1. Azure aboneliğinizde kimlik doğrulaması:

    ```azurecli
    az login
    ```

1. Çalışan kümeye komut dosyası eylemi uygulayın:

    ```azurecli
    az hdinsight script-action execute --cluster-name CLUSTERNAME --name SCRIPTNAME --resource-group RESOURCEGROUP --roles ROLES
    ```

    Geçerli roller `headnode` `workernode`, `zookeepernode` `edgenode`, , . Komut dosyası birkaç düğüm türüne uygulanacaksa, rolleri bir alana ayırın. Örneğin, `--roles headnode workernode`.

    Komut dosyasında kalıcı `--persist-on-success`olmak için. Ayrıca daha sonra kullanarak `az hdinsight script-action promote`komut dosyası devam edebilirsiniz.

### <a name="apply-a-script-action-to-a-running-cluster-by-using-rest-api"></a>REST API'yi kullanarak çalışan kümeye komut dosyası eylemi uygulama

[Azure HDInsight'ta Cluster REST API'ye](https://msdn.microsoft.com/library/azure/mt668441.aspx)bakın.

### <a name="apply-a-script-action-to-a-running-cluster-from-the-hdinsight-net-sdk"></a>HDInsight .NET SDK'dan çalışan kümeye komut dosyası eylemi uygulama

Bir kümeye komut dosyaları uygulamak için .NET SDK'yı kullanma örneği [için](https://github.com/Azure-Samples/hdinsight-dotnet-script-action)bkz.

## <a name="view-history-and-promote-and-demote-script-actions"></a>Geçmişi görüntüleyin ve komut dosyası eylemlerini tanıtın ve düşür

### <a name="the-azure-portal"></a>Azure portal

1. [Azure portalında](https://portal.azure.com) oturum açın ve kümenizi bulun.

1. Varsayılan görünümden Ayarlar **Settings** **altında, Komut Dosyası eylemlerini**seçin.

1. Bu kümeiçin komut dosyalarının geçmişi, komut dosyası eylemleri bölümünde görüntülenir. Bu bilgiler, kalıcı komut dosyalarının bir listesini içerir. Aşağıdaki ekran görüntüsü, Solr komut dosyasının bu kümede çalıştırıldığını gösterir. Ekran görüntüsü, kalıcı komut dosyalarını göstermez.

    ![Portal komut dosyası eylemleri geçmişi gönderme](./media/hdinsight-hadoop-customize-cluster-linux/script-action-history.png)

1. Bu komut dosyasının **Özellikleri** bölümünü görüntülemek için geçmişten bir komut dosyası seçin. Ekranın üst kısmından komut dosyasını yeniden çalıştırabilir veya tanıtabilirsiniz.

    ![Komut dosyası eylemleri özellikleri teşvik](./media/hdinsight-hadoop-customize-cluster-linux/promote-script-actions.png)

1. Ayrıca eylemleri gerçekleştirmek için komut dosyası eylemleri bölümünde girişlerin sağında elips, **...**, seçebilirsiniz.

    ![Kalıcı komut dosyası eylemleri silme](./media/hdinsight-hadoop-customize-cluster-linux/hdi-delete-promoted-sa.png)

### <a name="azure-powershell"></a>Azure PowerShell

| cmdlet | İşlev |
| --- | --- |
| `Get-AzHDInsightPersistedScriptAction` |Kalıcı komut dosyası eylemleri hakkında bilgi alın. Bu cmdlet, komut dosyası tarafından gerçekleştirilen eylemleri geri almaz, yalnızca kalıcı bayrağı kaldırır.|
| `Get-AzHDInsightScriptActionHistory` |Kümeye uygulanan komut dosyası eylemlerinin geçmişini veya belirli bir komut dosyası için ayrıntıları alın. |
| `Set-AzHDInsightPersistedScriptAction` |Geçici bir komut dosyası eylemini kalıcı bir komut dosyası eylemine yükseltin. |
| `Remove-AzHDInsightPersistedScriptAction` |Kalıcı bir komut dosyası eylemini geçici bir eyleme düşürün. |

Aşağıdaki örnek komut dosyası, bir komut dosyasını tanıtmak ve sonra düşürmek için cmdlet'lerin kullanılmasını gösterir.

[!code-powershell[main](../../powershell_scripts/hdinsight/use-script-action/use-script-action.ps1?range=123-140)]

### <a name="azure-cli"></a>Azure CLI

| Komut | Açıklama |
| --- | --- |
| [az hdinsight komut dosyası-eylem silme](https://docs.microsoft.com/cli/azure/hdinsight/script-action?view=azure-cli-latest#az-hdinsight-script-action-delete) |Kümenin belirli bir kalıcı komut dosyası eylemini siler. Bu komut, komut dosyası tarafından gerçekleştirilen eylemleri geri almaz, yalnızca kalıcı bayrağı kaldırır.|
|[az hdinsight komut dosyası-eylem yürütmek](https://docs.microsoft.com/cli/azure/hdinsight/script-action?view=azure-cli-latest#az-hdinsight-script-action-execute)|Belirtilen HDInsight kümesinde komut dosyası eylemlerini yürütün.|
| [az hdinsight komut dosyası-eylem listesi](https://docs.microsoft.com/cli/azure/hdinsight/script-action?view=azure-cli-latest#az-hdinsight-script-action-list) |Belirtilen küme için kalıcı tüm komut dosyası eylemlerini listeler. |
|[az hdinsight script-action listesi-yürütme-tarihçesi](https://docs.microsoft.com/cli/azure/hdinsight/script-action?view=azure-cli-latest#az-hdinsight-script-action-list-execution-history)|Belirtilen küme için tüm komut dosyalarının yürütme geçmişini listeler.|
|[az hdinsight script-action teşvik](https://docs.microsoft.com/cli/azure/hdinsight/script-action?view=azure-cli-latest#az-hdinsight-script-action-promote)|Belirtilen geçici komut dosyası yürütmesini kalıcı bir komut dosyasına teşvik eder.|
|[az hdinsight script-action show-execution-details](https://docs.microsoft.com/cli/azure/hdinsight/script-action?view=azure-cli-latest#az-hdinsight-script-action-show-execution-details)|Verilen komut dosyası yürütme kimliği için komut dosyası yürütme ayrıntısını alır.|

### <a name="hdinsight-net-sdk"></a>HDInsight .NET SDK

Bir kümeden komut dosyası geçmişini almak, komut dosyalarını tanıtmak veya düşürmek için .NET SDK'yı kullanma örneği için [bkz.](https://github.com/Azure-Samples/hdinsight-dotnet-script-action)

> [!NOTE]  
> Bu örnek, .NET SDK'yı kullanarak bir HDInsight uygulamasının nasıl yüklenir olduğunu da gösterir.

## <a name="support-for-open-source-software"></a>Açık kaynak yazılım desteği

Microsoft Azure HDInsight hizmeti, Apache Hadoop çevresinde oluşturulmuş açık kaynak teknolojilerinden oluşan bir ekosistem kullanır. Microsoft Azure, açık kaynak teknolojileri için genel bir destek düzeyi sağlar. Daha fazla bilgi için Azure [Destek SSS'lerinin](https://azure.microsoft.com/support/faq/) **Destek Kapsamı** bölümüne bakın. HDInsight hizmeti, yerleşik bileşenler için ek bir destek düzeyi sağlar.

HDInsight hizmetinde iki tür açık kaynak bileşeni mevcuttur:

* **Dahili bileşenler.** Bu bileşenler HDInsight kümelerine önceden yüklenir ve kümenin temel işlevselliğini sağlar. Aşağıdaki bileşenler bu kategoriye aittir:

  * [Apache Hadoop İplik](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) Resourcemanager.
  * Hive sorgu dili [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).
  * [Apaçi Mahout.](https://mahout.apache.org/)

    Küme bileşenlerinin tam listesi [HDInsight ile kullanılabilen Apache Hadoop bileşenleri ve sürümleri nelerdir?](hdinsight-component-versioning.md)

* **Özel bileşenler.** Kümenin bir kullanıcısı olarak, toplulukta bulunan veya sizin oluşturduğunuz herhangi bir bileşeni iş yükler veya iş yüklersiniz.

> [!WARNING]  
> HDInsight kümesiyle sağlanan bileşenler tam olarak desteklenir. Microsoft Destek, bu bileşenlerle ilgili sorunları yalıtmaya ve çözmeye yardımcı olur.
>
> Özel bileşenler, sorunu daha fazla gidermenize yardımcı olmak için ticari olarak makul destek alır. Microsoft Destek sorunu çözebilir. Ya da bu teknoloji için derin uzmanlık bulunan açık kaynak teknolojileri için mevcut kanallar meşgul isteyebilirsiniz. Birçok topluluk sitesi kullanılabilir. Örnekler HDInsight ve [Stack Overflow](https://stackoverflow.com) [için MSDN forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight) vardır.
>
> Apaçi projeleri de [Apache web sitesinde](https://apache.org)proje siteleri var. Bir örnek [Hadoop](https://hadoop.apache.org/)olduğunu.

HDInsight hizmeti, özel bileşenleri kullanmak için çeşitli yollar sağlar. Bir bileşen kümeye nasıl kullanılır veya yüklenirse yüklensin, aynı destek düzeyi uygulanır. Aşağıdaki liste, HDInsight kümelerinde özel bileşenlerin en yaygın şekilde kullanıldığını açıklar:

1. **İş teslimi**. Hadoop veya özel bileşenleri çalıştıran veya kullanan diğer iş türleri kümeye gönderilebilir.

2. **Küme özelleştirme.** Küme oluşturma sırasında, küme düğümlerine yüklenen ek ayarlar ve özel bileşenler belirtebilirsiniz.

3. **Örnekler**. Popüler özel bileşenler için Microsoft ve diğerleri, bu bileşenlerin HDInsight kümelerinde nasıl kullanılabileceğinin örneklerini sağlayabilir. Bu örnekler destek siz sağlanmaktadır.

## <a name="troubleshooting"></a>Sorun giderme

Komut dosyası eylemleri tarafından günlüğe kaydedilen bilgileri görüntülemek için Ambari web Kullanıcı Arama Hizmetini kullanabilirsiniz. Komut dosyası küme oluşturma sırasında başarısız olursa, günlükleri küme ile ilişkili varsayılan depolama hesabında da kullanılabilir. Bu bölümde, her iki seçeneği de kullanarak günlüklerin nasıl alınabildiğini zedelemverilmektedir.

### <a name="the-apache-ambari-web-ui"></a>The Apache Ambari web UI

1. Bir web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net`kümenizin adı nerede' `CLUSTERNAME` ye gidin.

1. Sayfanın üst kısmındaki çubuktan **ops** girişini seçin. Liste Ambari aracılığıyla kümeüzerinde yapılan geçerli ve önceki işlemleri görüntüler.

    ![Ambari web UI bar ops seçilen](./media/hdinsight-hadoop-customize-cluster-linux/hdi-apache-ambari-nav.png)

1. **İşlemler** sütununda **\_özel leştirilmiş komut dosyası çalıştıran** girişleri bulun. Bu girişler komut dosyası eylemleri çalıştırıldığında oluşturulur.

    ![Apache Ambari komut dosyası eylem işlemleri](./media/hdinsight-hadoop-customize-cluster-linux/ambari-script-action.png)

    **STDOUT** ve **STDERR** çıktısını görüntülemek için **run\customscriptaction** girişini seçin ve bağlantıları inceleştirin. Bu çıktı, komut dosyası çalıştığında oluşturulur ve yararlı bilgilere sahip olabilir.

### <a name="access-logs-from-the-default-storage-account"></a>Varsayılan depolama hesabından erişim günlükleri

Komut dosyası hatası nedeniyle küme oluşturma başarısız olursa, günlükleri küme depolama hesabında tutulur.

* Depolama günlükleri ' `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE`den temin edilebilir.

    ![Komut dosyası eylem günlükleri](./media/hdinsight-hadoop-customize-cluster-linux/script-action-logs-in-storage.png)

    Bu dizin altında, günlükleri **headnode**için ayrı ayrı düzenlenir , **işçi düğümü**, ve **zookeeper düğüm**. Aşağıdaki örneklere bakın:

    * **Kafa düğümü**:`<ACTIVE-HEADNODE-NAME>.cloudapp.net`

    * **İşçi düğümü**:`<ACTIVE-WORKERNODE-NAME>.cloudapp.net`

    * **Zookeeper düğümü**:`<ACTIVE-ZOOKEEPERNODE-NAME>.cloudapp.net`

* İlgili ana bilgisayara ait tüm **stdout** ve **stderr** depolama hesabına yüklenir. Her komut dosyası eylemi için bir **çıktı vardır-\*.txt** ve **hatalar-\*.txt.** **Output-*.txt** dosyası, ana bilgisayarda çalıştırılabilen komut dosyasının URI'si hakkında bilgi içerir. Aşağıdaki metin bu bilgilere bir örnektir:

        'Start downloading script locally: ', u'https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh'

* Aynı ada sahip bir komut dosyası eylem kümesini art arda oluşturabilirsiniz. Bu durumda, **DATE** klasör adını temel alan ilgili günlükleri ayırt edebilirsiniz. Örneğin, farklı tarihlerde oluşturulan bir küme, **mycluster**klasör yapısı aşağıdaki günlük girişlerine benzer görünür:

    `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-04` `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\mycluster\2015-10-05`

* Aynı gün aynı ada sahip bir komut dosyası eylem kümesi oluşturursanız, ilgili günlük dosyalarını tanımlamak için benzersiz önekik kullanabilirsiniz.

* Gece yarısı, saat 12:00'ye yakın bir küme oluşturursanız, günlük dosyalarının iki gün boyunca yayLımı olabilir. Bu durumda, aynı küme için iki farklı tarih klasörü görürsünüz.

* Günlük dosyalarını varsayılan kapsayıcıya yüklemek, özellikle büyük kümeler için beş dakika kadar sürebilir. Bu nedenle, günlüklere erişmek istiyorsanız, komut dosyası eylemi başarısız olursa kümeyi hemen silmemeniz gerekir.

### <a name="ambari-watchdog"></a>Ambari bekçi köpeği

> [!WARNING]  
> Linux tabanlı HDInsight kümenizde Ambari izleme örgütü hdinsightwatchdog'un parolasını değiştirmeyin. Bu hesabın parolasını değiştirmek, HDInsight kümesinde yeni komut dosyası eylemleri çalıştırma özelliğini kırar.

### <a name="cant-import-name-blobservice"></a>BlobService adını içe aktaramıyorum

__Semptomlar.__ Komut dosyası eylemi başarısız olur. Ambari'deki işlemi görüntülediğinizde aşağıdaki hataya benzer metin görüntülenir:

```
Traceback (most recent call list):
  File "/var/lib/ambari-agent/cache/custom_actions/scripts/run_customscriptaction.py", line 21, in <module>
    from azure.storage.blob import BlobService
ImportError: cannot import name BlobService
```

__Çünkü.__ Bu hata, HDInsight kümesine dahil olan Python Azure Depolama istemcisini yükseltirseniz oluşur. HDInsight, Azure Depolama istemcisi 0.20.0 bekliyor.

__Çözünürlük__. Bu hatayı gidermek için, her küme düğümüne `ssh`kullanarak el ile bağlanın. Doğru depolama istemcisi sürümünü yeniden yüklemek için aşağıdaki komutu çalıştırın:

```bash
sudo pip install azure-storage==0.20.0
```

Kümeye SSH ile bağlanma hakkında bilgi için, [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

### <a name="history-doesnt-show-the-scripts-used-during-cluster-creation"></a>Geçmiş küme oluşturma sırasında kullanılan komut dosyalarını göstermez

Kümeniz 15 Mart 2016'dan önce oluşturulduysa, komut dosyası eylem geçmişinde bir giriş göremeyebilirsiniz. Kümeyi yeniden boyutlandırmak, komut dosyalarının komut dosyası eylem geçmişinde görünmesine neden olur.

İki özel durum vardır:

* Kümeniz 1 Eylül 2015'te oluşturuldu. Bu tarih, komut dosyası eylemlerinin tanıtıldığı tarihtir. Bu tarihten önce oluşturulan herhangi bir küme küme oluşturma için komut dosyası eylemleri kullanmış olamaz.

* Küme oluşturma sırasında birden çok komut dosyası eylemi kullandınız. Veya birden çok komut dosyası veya aynı ad, aynı URI, ancak birden çok komut dosyası için farklı parametreler için aynı adı kullandınız. Bu gibi durumlarda, aşağıdaki hatayı alırsınız:

    Varolan komut dosyalarındaki çakışan komut dosyası adları nedeniyle bu kümede yeni komut dosyası eylemleri çalıştırılamaz. Küme oluşturmada sağlanan komut dosyası adlarının tümü benzersiz olmalıdır. Varolan komut dosyaları yeniden boyutlandırılır.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight için komut dosyası eylem komut dosyaları geliştirme](hdinsight-hadoop-script-actions-linux.md)
* [HDInsight kümesine ek depolama alanı ekleme](hdinsight-hadoop-add-storage.md)

[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster-linux/cluster-provisioning-states.png "Küme oluşturma sırasında aşamalar"
