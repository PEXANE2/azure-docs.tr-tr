---
title: Ambari REST API ile Hadoop'u izleyin ve yönetin - Azure HDInsight
description: Azure HDInsight'ta Hadoop kümelerini izlemek ve yönetmek için Ambari'yi nasıl kullanacağınızı öğrenin. Bu belgede, HDInsight kümeleriyle birlikte verilen Ambari REST API'sini nasıl kullanacağınızı öğreneceksiniz.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/07/2019
ms.openlocfilehash: 1d684957939c5cb83aae05962c1694f7a8d8da23
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79272402"
---
# <a name="manage-hdinsight-clusters-by-using-the-apache-ambari-rest-api"></a>Apache Ambari REST API'yi kullanarak HDInsight kümelerini yönetme

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Azure HDInsight'ta Apache Hadoop kümelerini yönetmek ve izlemek için Apache Ambari REST API'yi nasıl kullanacağınızı öğrenin.

## <a name="what-is-apache-ambari"></a><a id="whatis"></a>Apaçi Ambari nedir

[Apache Ambari,](https://ambari.apache.org) [REST API'leri](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)tarafından desteklenen kullanımı kolay bir web Kullanıcı Arabirimi sağlayarak Hadoop kümelerinin yönetimini ve izlenmesini kolaylaştırır.  Ambari varsayılan olarak Linux tabanlı HDInsight kümeleri ile sağlanır.

## <a name="prerequisites"></a>Ön koşullar

* **HDInsight'ta hadoop kümesi.** [Linux'ta HDInsight ile başlayın](hadoop/apache-hadoop-linux-tutorial-get-started.md)bakın.

* **Bash Ubuntu üzerinde Windows 10 .**  Bu makaledeki örnekler, Windows 10'daki Bash kabuğunu kullanır. Yükleme adımları [için Windows 10 için Linux Yükleme Kılavuzu](https://docs.microsoft.com/windows/wsl/install-win10) için Windows Alt Sistemi'ne bakın.  Diğer [Unix kabukları](https://www.gnu.org/software/bash/) da çalışacaktır.  Örnekler, bazı küçük değişiklikler ile, bir Windows Komut ustem üzerinde çalışabilir.  Alternatif olarak, Windows PowerShell'i kullanabilirsiniz.

* **jq**, bir komut satırı JSON işlemci.  Bkz. [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).

* **Windows PowerShell**.  Alternatif olarak, [Bash](https://www.gnu.org/software/bash/)kullanabilirsiniz.

## <a name="base-uri-for-ambari-rest-api"></a>Ambari Rest API için Temel URI

 HDInsight'taki `https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME`Ambari REST API'nin temel Tekdüzen Kaynak Tanımlayıcısı `CLUSTERNAME` (URI), kümenizin adıdır.  ÜRB'lerde küme adları **büyük/küçük harf duyarlıdır.**  URI'nin`CLUSTERNAME.azurehdinsight.net`tam nitelikli alan adı (FQDN) bölümündeki küme adı büyük/küçük harf duyarsız olsa da, URI'deki diğer olaylar büyük/küçük harf duyarlıdır.

## <a name="authentication"></a>Kimlik doğrulaması

HDInsight'ta Ambari'ye bağlanmak IÇIN HTTPS gerekiyor. Yönetici hesap adını (varsayılan **yöneticidir)** ve küme oluşturma sırasında sağladığınız parolayı kullanın.

Kurumsal Güvenlik Paketi kümeleri `admin`için, bunun yerine, `username@domain.onmicrosoft.com`gibi tam nitelikli bir kullanıcı adı kullanın.

## <a name="examples"></a>Örnekler

### <a name="setup-preserve-credentials"></a>Kurulum (Kimlik bilgilerini koru)
Her örnek için yeniden girmemek için kimlik bilgilerinizi koruyun.  Küme adı ayrı bir adımda korunur.

**A. Bash**  
Gerçek parolanızı değiştirerek `PASSWORD` aşağıdaki komut dosyasını düzenleme.  Sonra komutu girin.

```bash
export password='PASSWORD'
```  

**B. PowerShell**  

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
```

### <a name="identify-correctly-cased-cluster-name"></a>Doğru örneklenmiş küme adını tanımlama
Küme adının gerçek gövdesi, kümenin nasıl oluşturulduğuna bağlı olarak beklediğiniz den farklı olabilir.  Buradaki adımlar gerçek kasayı gösterir ve sonraki tüm örnekler için bir değişkende saklar.

Küme adınız ile değiştirmek `CLUSTERNAME` için aşağıdaki komut dosyalarını edin. Sonra komutu girin. (FQDN için küme adı büyük/küçük harf duyarlı değildir.)

```bash
export clusterName=$(curl -u admin:$password -sS -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters" | jq -r '.items[].Clusters.cluster_name')
echo $clusterName
```  

```powershell
# Identify properly cased cluster name
$resp = Invoke-WebRequest -Uri "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters" `
    -Credential $creds -UseBasicParsing
$clusterName = (ConvertFrom-Json $resp.Content).items.Clusters.cluster_name;

# Show cluster name
$clusterName
```

### <a name="parsing-json-data"></a>JSON verilerini ayrıştama

Aşağıdaki örnek, [JSON](https://stedolan.github.io/jq/) yanıt belgesini ayrışdırmak ve yalnızca sonuçlardaki `health_report` bilgileri görüntülemek için jq veya [ConvertFrom-Json'ı](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/convertfrom-json) kullanır.

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" \
| jq '.Clusters.health_report'
```  

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.health_report
```

### <a name="get-the-fqdn-of-cluster-nodes"></a><a name="example-get-the-fqdn-of-cluster-nodes"></a>Küme düğümlerinin FQDN'sini alın

HDInsight ile çalışırken, bir küme düğümünün tam nitelikli etki alanı adını (FQDN) bilmeniz gerekebilir. Aşağıdaki örnekleri kullanarak kümedeki çeşitli düğümler için FQDN'yi kolayca alabilirsiniz:

**Tüm düğümler**  

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" \
| jq -r '.items[].Hosts.host_name'
```  

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.Hosts.host_name
```

**Kafa düğümleri**  

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/NAMENODE" \
| jq -r '.host_components[].HostRoles.host_name'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/NAMENODE" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.host_components.HostRoles.host_name
```

**İşçi düğümleri**  

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/DATANODE" \
| jq -r '.host_components[].HostRoles.host_name'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/DATANODE" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.host_components.HostRoles.host_name
```

**Zookeeper düğümleri**

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" \
| jq -r ".host_components[].HostRoles.host_name"
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.host_components.HostRoles.host_name
```

### <a name="get-the-internal-ip-address-of-cluster-nodes"></a><a name="example-get-the-internal-ip-address-of-cluster-nodes"></a>Küme düğümlerinin iç IP adresini alma

Bu bölümdeki örneklerle döndürülen IP adreslerine internet üzerinden doğrudan erişilemez. Bunlara yalnızca HDInsight kümesini içeren Azure Sanal Ağı'nda erişilebilirler.

HDInsight ve sanal ağlarla çalışma hakkında daha fazla bilgi [için HDInsight için sanal ağ planı'na](hdinsight-plan-virtual-network-deployment.md)bakın.

IP adresini bulmak için küme düğümlerinin dahili tam nitelikli alan adını (FQDN) bilmeniz gerekir. FQDN'ye sahip olduktan sonra ana bilgisayarın IP adresini alabilirsiniz. Aşağıdaki örnekler, tüm ana bilgisayar düğümlerinin FQDN'si için önce Ambari sorgusunu, ardından her ana bilgisayariçin Ambari sorgusu.

```bash
for HOSTNAME in $(curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" | jq -r '.items[].Hosts.host_name')
do
    IP=$(curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts/$HOSTNAME" | jq -r '.Hosts.ip')
  echo "$HOSTNAME <--> $IP"
done
```  

```powershell
$uri = "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" 
$resp = Invoke-WebRequest -Uri $uri -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
foreach($item in $respObj.items) {
    $hostName = [string]$item.Hosts.host_name
    $hostInfoResp = Invoke-WebRequest -Uri "$uri/$hostName" `
        -Credential $creds -UseBasicParsing
    $hostInfoObj = ConvertFrom-Json $hostInfoResp 
    $hostIp = $hostInfoObj.Hosts.ip
    "$hostName <--> $hostIp"
}
```

### <a name="get-the-default-storage"></a>Varsayılan depolama alanını alın

Bir HDInsight kümesi oluşturduğunuzda, küme için varsayılan depolama alanı olarak bir Azure Depolama Hesabı veya Veri Gölü Depolama sı kullanmanız gerekir. Küme oluşturulduktan sonra bu bilgileri almak için Ambari'yi kullanabilirsiniz. Örneğin, HDInsight dışındaki kapsayıcıya veri okumak/yazmak istiyorsanız.

Aşağıdaki örnekler kümeden varsayılan depolama yapılandırmasını alır:

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
| jq -r '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties.'fs.defaultFS'
```

> [!IMPORTANT]  
> Bu örnekler, bu bilgileri içeren`service_config_version=1`sunucuya uygulanan ilk yapılandırmayı ( ) döndürer. Küme oluşturulduktan sonra değiştirilen bir değer alırsanız, yapılandırma sürümlerini listelemeniz ve en son bir değeri almanız gerekebilir.

İade değeri aşağıdaki örneklerden birine benzer:

* `wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net`- Bu değer, kümenin varsayılan depolama alanı için bir Azure Depolama hesabı kullandığını gösterir. Değer, `ACCOUNTNAME` depolama hesabının adıdır. Bölüm, `CONTAINER` depolama hesabındaki blob kapsayıcısının adıdır. Kapsayıcı küme için HDFS uyumlu depolama köküdür.

* `abfs://CONTAINER@ACCOUNTNAME.dfs.core.windows.net`- Bu değer, kümenin varsayılan depolama alanı için Azure Veri Gölü Depolama Gen2'yi kullandığını gösterir. Ve `ACCOUNTNAME` `CONTAINER` değerler, daha önce bahsedilen Azure Depolama ile aynı anlama gelir.

* `adl://home`- Bu değer, kümenin varsayılan depolama alanı için Azure Veri Gölü Depolama Gen1'i kullandığını gösterir.

    Veri Gölü Depolama hesap adını bulmak için aşağıdaki örnekleri kullanın:

    ```bash
    curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq -r '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds -UseBasicParsing
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.hostname'
    ```

    İade `ACCOUNTNAME.azuredatalakestore.net`değeri, Veri Gölü `ACCOUNTNAME` Depolama hesabının adı olan yere benzer.

    Kümeiçin depolama alanını içeren Veri Gölü Depolama'daki dizini bulmak için aşağıdaki örnekleri kullanın:

    ```bash
    curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq -r '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'
    ```  

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds -UseBasicParsing
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.mountpoint'
    ```

    İade değeri `/clusters/CLUSTERNAME/`. Bu değer, Veri Gölü Depolama hesabı içindeki bir yoldur. Bu yol küme için HDFS uyumlu dosya sisteminin köküdür.  

> [!NOTE]  
> [Azure PowerShell](/powershell/azure/overview) tarafından sağlanan [Get-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsightcluster) cmdlet de kümenin depolama bilgilerini döndürür.

### <a name="get-all-configurations"></a><a name="get-all-configurations"></a>Tüm yapılandırmaları alın

Kümeniz için kullanılabilen yapılandırmaları alın.

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName?fields=Clusters/desired_configs"
```

```powershell
$respObj = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
    -Credential $creds -UseBasicParsing
$respObj.Content
```

Bu örnek, kümeye yüklenen bileşenler için geçerli yapılandırmayı *(etiket* değeriyle tanımlanan) içeren bir JSON belgesi döndürür. Aşağıdaki örnek, Bir Kıvılcım küme türünden döndürülen verilerden bir alıntıdır.

```json
"jupyter-site" : {
  "tag" : "INITIAL",
  "version" : 1
},
"livy2-client-conf" : {
  "tag" : "INITIAL",
  "version" : 1
},
"livy2-conf" : {
  "tag" : "INITIAL",
  "version" : 1
},
```

### <a name="get-configuration-for-specific-component"></a>Belirli bileşen için yapılandırma alın

İlgilendiğiniz bileşeniçin yapılandırmayı alın. Aşağıdaki örnekte, `INITIAL` önceki istekten döndürülen etiket değeriyle değiştirin.

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=livy2-conf&tag=INITIAL"
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=livy2-conf&tag=INITIAL" `
    -Credential $creds -UseBasicParsing
$resp.Content
```

Bu örnek, `livy2-conf` bileşen için geçerli yapılandırmayı içeren bir JSON belgesi ni döndürür.

### <a name="update-configuration"></a>Yapılandırmayı güncelleştirme

1. Oluştur. `newconfig.json`  
   Değiştirin ve aşağıdaki komutları girin:

   * İstediğin `livy2-conf` bileşeni değiştirin.
   * Tüm `INITIAL` `tag` [yapılandırmaları al'dan](#get-all-configurations)alınan gerçek değerle değiştirin.

     **A. Bash**  
     ```bash
     curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=livy2-conf&tag=INITIAL" \
     | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
     ```

     **B. PowerShell**  
     PowerShell komut dosyası [jq](https://stedolan.github.io/jq/)kullanır.  Gerçek `C:\HD\jq\jq-win64` yol unuzu ve [jq](https://stedolan.github.io/jq/)sürümünü yansıtacak şekilde aşağıda edin.

     ```powershell
     $epoch = Get-Date -Year 1970 -Month 1 -Day 1 -Hour 0 -Minute 0 -Second 0
     $now = Get-Date
     $unixTimeStamp = [math]::truncate($now.ToUniversalTime().Subtract($epoch).TotalMilliSeconds)
     $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=livy2-conf&tag=INITIAL" `
       -Credential $creds -UseBasicParsing
     $resp.Content | C:\HD\jq\jq-win64 --arg newtag "version$unixTimeStamp" '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
     ```

     Jq, HDInsight'tan alınan verileri yeni bir yapılandırma şablonuna dönüştürmek için kullanılır. Özellikle, bu örnekler aşağıdaki eylemleri gerçekleştirmek:

   * "Sürüm" dizesi ve 'de `newtag`depolanan tarih' dizesini içeren benzersiz bir değer oluşturur.

   * İstenilen yeni yapılandırma için bir kök belge oluşturur.

   * `.items[]` Dizinin içeriğini alır ve **desired_config** öğesinin altına ekler.

   * Bu öğeler `href` `version`yeni `Config` bir yapılandırma göndermek için gerekli değildir gibi , ve öğeleri, siler.

   * Değeri `tag` olan bir öğe ekler. `version#################` Sayısal kısım geçerli tarihi temel alıyor. Her yapılandırmanın benzersiz bir etiketi olmalıdır.

     Son olarak, veriler `newconfig.json` belgeye kaydedilir. Belge yapısı aşağıdaki örneğe benzer görünmelidir:

     ```json
     {
       "Clusters": {
         "desired_config": {
           "tag": "version1552064778014",
           "type": "livy2-conf",
           "properties": {
             "livy.environment": "production",
             "livy.impersonation.enabled": "true",
             "livy.repl.enableHiveContext": "true",
             "livy.server.csrf_protection.enabled": "true",
               ....
           },
         },
       }
     }
     ```

2. Edit `newconfig.json`.  
   Belgeyi `newconfig.json` açın ve `properties` nesnedeki değerleri değiştirin/ekleyin. Aşağıdaki örnekte değeri `"livy.server.csrf_protection.enabled"` `"true"` değişir `"false"`.

        "livy.server.csrf_protection.enabled": "false",

    Değişiklik yapmayı bitirdikten sonra dosyayı kaydedin.

3. Gönder `newconfig.json`.  
   Güncelleştirilmiş yapılandırmayı Ambari'ye göndermek için aşağıdaki komutları kullanın.

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" -X PUT -d @newconfig.json "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName"
    ```

    ```powershell
    $newConfig = Get-Content .\newconfig.json
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
        -Credential $creds -UseBasicParsing `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body $newConfig
    $resp.Content
    ```  

    Bu **komutlar, newconfig.json** dosyasının içeriğini yeni istenen yapılandırma olarak kümeye gönderir. İstek bir JSON belgesidöndürür. Bu belgedeki **sürümTag** öğesi gönderdiğiniz sürümle eşleşmelidir ve **configs** nesnesi istediğiniz yapılandırma değişikliklerini içerir.

### <a name="restart-a-service-component"></a>Hizmet bileşenini yeniden başlatma

Bu noktada, Ambari web UI'sine bakarsanız, Spark hizmetinin yeni yapılandırmanın etkili olabilmesi için yeniden başlatılması gerektiğini belirtir. Hizmeti yeniden başlatmak için aşağıdaki adımları kullanın.

1. Spark2 hizmetiiçin bakım modunu etkinleştirmek için aşağıdakileri kullanın:

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning on maintenance mode for SPARK2"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds -UseBasicParsing `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning on maintenance mode for SPARK2"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}'
    ```

2. Bakım modunu doğrula  

    Bu komutlar, bakım modunu açan sunucuya bir JSON belgesi gönderir. Aşağıdaki isteği kullanarak hizmetin artık bakım modunda olduğunu doğrulayabilirsiniz:

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" \
    | jq .ServiceInfo.maintenance_state
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds -UseBasicParsing
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.ServiceInfo.maintenance_state
    ```

    İade `ON`değeri.

3. Ardından, Spark2 hizmetini kapatmak için aşağıdakileri kullanın:

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK2","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}' \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds -UseBasicParsing `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK2","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}'
    $resp.Content
    ```

    Yanıt aşağıdaki örneğe benzer:

    ```json
    {
        "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
        "Requests" : {
            "id" : 29,
            "status" : "Accepted"
        }
    }
    ```

    > [!IMPORTANT]  
    > Bu `href` URI tarafından döndürülen değer küme düğümünün iç IP adresini kullanır. Kümenin dışından kullanmak için, `10.0.0.18:8080` bölümü kümenin FQDN ile değiştirin.  

4. İsteği doğrulayın.  
    Önceki adımdan `29` `id` döndürülen gerçek değeri değiştirerek aşağıdaki komutu düzenleme.  Aşağıdaki komutlar isteğin durumunu alır:

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/requests/29" \
    | jq .Requests.request_status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/requests/29" `
        -Credential $creds -UseBasicParsing
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.Requests.request_status
    ```

    Bir yanıt, isteğin tamamladığını `COMPLETED` gösterir.

5. Önceki istek tamamlandıktan sonra, Spark2 hizmetini başlatmak için aşağıdakileri kullanın.

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.START.SPARK2","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}' \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds -UseBasicParsing `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.START.SPARK2","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}'
    $resp.Content
    ```

    Hizmet şimdi yeni yapılandırmayı kullanıyor.

6. Son olarak, bakım modunu kapatmak için aşağıdakileri kullanın.

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning off maintenance mode for SPARK2"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds -UseBasicParsing `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning off maintenance mode for SPARK2"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}'
    ```

## <a name="next-steps"></a>Sonraki adımlar

REST API tam bir referans için, [Apache Ambari API Referans V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)bakın.  Ayrıca bakınız, [Apache Ambari Görünümleri için kullanıcıları yetkilendirme](./hdinsight-authorize-users-to-ambari.md)
