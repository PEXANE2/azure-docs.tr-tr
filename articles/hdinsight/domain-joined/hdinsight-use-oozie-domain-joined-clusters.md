---
title: Apache Oozie iş akışları & kurumsal güvenlik-Azure HDInsight
description: Azure HDInsight Kurumsal Güvenlik Paketi kullanan Apache Oozie iş akışlarını güvenli hale getirin. Oozie iş akışını tanımlama ve Oozie işi gönderme hakkında bilgi edinin.
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,seodec18,seoapr2020
ms.date: 05/14/2020
ms.openlocfilehash: 36c04480c46cea904b072c659c5c2642a28e1f27
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83647566"
---
# <a name="run-apache-oozie-in-azure-hdinsight-clusters-with-enterprise-security-package"></a>Kurumsal Güvenlik Paketi ile Azure HDInsight kümelerinde Apache Oozie çalıştırma

Apache Oozie, Apache Hadoop işlerini yöneten bir iş akışı ve düzenleme sistemidir. Oozie, Hadoop yığınında tümleşiktir ve aşağıdaki işleri destekler:

- Apache MapReduce
- Apache Pig
- Apache Hive
- Apache Sqoop

Ayrıca, Java programları veya kabuk betikleri gibi bir sisteme özgü işleri zamanlamak için Oozie de kullanabilirsiniz.

## <a name="prerequisite"></a>Önkoşul

Kurumsal Güvenlik Paketi (ESP) Azure HDInsight Hadoop bir kümesi. Bkz. [HDInsight KÜMELERINI ESP Ile yapılandırma](./apache-domain-joined-configure-using-azure-adds.md).

> [!NOTE]  
> ESP olmayan kümeler üzerinde Oozie kullanma hakkında ayrıntılı yönergeler için bkz. [Linux tabanlı Azure HDInsight 'Ta Apache Oozie iş akışlarını kullanma](../hdinsight-use-oozie-linux-mac.md).

## <a name="connect-to-an-esp-cluster"></a>Bir ESP kümesine bağlanma

Secure Shell (SSH) hakkında daha fazla bilgi için bkz. [SSH kullanarak HDInsight 'a (Hadoop) bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

1. SSH kullanarak HDInsight kümesine bağlanma:

    ```bash
    ssh [DomainUserName]@<clustername>-ssh.azurehdinsight.net
    ```

1. Başarılı Kerberos kimlik doğrulamasını doğrulamak için komutunu kullanın `klist` . Aksi takdirde, `kinit` Kerberos kimlik doğrulamasını başlatmak için kullanın.

1. Azure Data Lake Storage erişmek için gereken OAuth belirtecini kaydetmek için HDInsight ağ geçidinde oturum açın:

    ```bash
    curl -I -u [DomainUserName@Domain.com]:[DomainUserPassword] https://<clustername>.azurehdinsight.net
    ```

    **200 ok** durum yanıt kodu başarıyla kayıt olduğunu gösterir. 401 gibi yetkisiz bir yanıt alınmışsa Kullanıcı adını ve parolayı kontrol edin.

## <a name="define-the-workflow"></a>İş akışını tanımlama

Oozie iş akışı tanımları Apache Hadoop Işlem tanımlama dilinde (hPDL) yazılır. hPDL bir XML işlem tanımı dilidir. İş akışını tanımlamak için aşağıdaki adımları uygulayın:

1. Bir etki alanı kullanıcısının çalışma alanını ayarlama:

   ```bash
   hdfs dfs -mkdir /user/<DomainUser>
   cd /home/<DomainUserPath>
   cp /usr/hdp/<ClusterVersion>/oozie/doc/oozie-examples.tar.gz .
   tar -xvf oozie-examples.tar.gz
   hdfs dfs -put examples /user/<DomainUser>/
   ```

   `DomainUser`Etki alanı kullanıcı adıyla değiştirin.
   `DomainUserPath`Etki alanı kullanıcısının ana dizin yoluyla değiştirin.
   `ClusterVersion`Küme veri platformu sürümünüz ile değiştirin.

2. Yeni bir dosya oluşturmak ve düzenlemek için aşağıdaki ifadeyi kullanın:

   ```bash
   nano workflow.xml
   ```

3. Nano düzenleyici açıldıktan sonra, dosya içeriği olarak aşağıdaki XML 'i girin:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <workflow-app xmlns="uri:oozie:workflow:0.4" name="map-reduce-wf">
       <credentials>
          <credential name="metastore_token" type="hcat">
             <property>
                <name>hcat.metastore.uri</name>
                <value>thrift://<active-headnode-name>-<clustername>.<Domain>.com:9083</value>
             </property>
             <property>
                <name>hcat.metastore.principal</name>
                <value>hive/_HOST@<Domain>.COM</value>
             </property>
          </credential>
          <credential name="hs2-creds" type="hive2">
             <property>
                <name>hive2.server.principal</name>
                <value>${jdbcPrincipal}</value>
             </property>
             <property>
                <name>hive2.jdbc.url</name>
                <value>${jdbcURL}</value>
             </property>
          </credential>
       </credentials>
       <start to="mr-test" />
       <action name="mr-test">
          <map-reduce>
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <prepare>
                <delete path="${nameNode}/user/${wf:user()}/examples/output-data/mrresult" />
             </prepare>
             <configuration>
                <property>
                   <name>mapred.job.queue.name</name>
                   <value>${queueName}</value>
                </property>
                <property>
                   <name>mapred.mapper.class</name>
                   <value>org.apache.oozie.example.SampleMapper</value>
                </property>
                <property>
                   <name>mapred.reducer.class</name>
                   <value>org.apache.oozie.example.SampleReducer</value>
                </property>
                <property>
                   <name>mapred.map.tasks</name>
                   <value>1</value>
                </property>
                <property>
                   <name>mapred.input.dir</name>
                   <value>/user/${wf:user()}/${examplesRoot}/input-data/text</value>
                </property>
                <property>
                   <name>mapred.output.dir</name>
                   <value>/user/${wf:user()}/${examplesRoot}/output-data/mrresult</value>
                </property>
             </configuration>
          </map-reduce>
          <ok to="myHive2" />
          <error to="fail" />
       </action>
       <action name="myHive2" cred="hs2-creds">
          <hive2 xmlns="uri:oozie:hive2-action:0.2">
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <jdbc-url>${jdbcURL}</jdbc-url>
             <script>${hiveScript2}</script>
             <param>hiveOutputDirectory2=${hiveOutputDirectory2}</param>
          </hive2>
          <ok to="myHive" />
          <error to="fail" />
       </action>
       <action name="myHive" cred="metastore_token">
          <hive xmlns="uri:oozie:hive-action:0.2">
             <job-tracker>${jobTracker}</job-tracker>
             <name-node>${nameNode}</name-node>
             <configuration>
                <property>
                   <name>mapred.job.queue.name</name>
                   <value>${queueName}</value>
                </property>
             </configuration>
             <script>${hiveScript1}</script>
             <param>hiveOutputDirectory1=${hiveOutputDirectory1}</param>
          </hive>
          <ok to="end" />
          <error to="fail" />
       </action>
       <kill name="fail">
          <message>Oozie job failed, error message[${wf:errorMessage(wf:lastErrorNode())}]</message>
       </kill>
       <end name="end" />
    </workflow-app>
    ```

4. `clustername`Kümenin adıyla değiştirin.

5. Dosyayı kaydetmek için **CTRL + X**' i seçin. **Y**girin. Ardından **ENTER**' ı seçin.

    İş akışı iki kısma ayrılmıştır:

   - **Credential.** Bu bölüm, Oozie eylemlerinin kimliğini doğrulamak için kullanılan kimlik bilgilerini alır:

     Bu örnek, Hive eylemleri için kimlik doğrulamasını kullanır. Daha fazla bilgi için bkz. [eylem kimlik doğrulaması](https://oozie.apache.org/docs/4.2.0/DG_ActionAuthentication.html).

     Kimlik bilgisi hizmeti, Oozie eylemlerinin Hadoop hizmetlerine erişim için kullanıcının kimliğine bürünmesini sağlar.

   - **Ön.** Bu bölümde üç eylem vardır: eşleme-azaltma, Hive sunucu 2 ve Hive sunucu 1:

     - Map-azaltma eylemi, bir Oozie paketinden bir örnek çalıştırır. Bu, toplanmış sözcük sayısını çıkışı azaltır.

     - Hive Server 2 ve Hive Server 1 eylemleri, HDInsight ile birlikte sunulan bir örnek Hive tablosunda bir sorgu çalıştırır.

     Hive eylemleri, işlem öğesindeki anahtar sözcüğünü kullanarak kimlik doğrulaması için kimlik bilgileri bölümünde tanımlanan kimlik bilgilerini kullanır `cred` .

6. Dosyayı kopyalamak için aşağıdaki komutu kullanın `workflow.xml` `/user/<domainuser>/examples/apps/map-reduce/workflow.xml` :

    ```bash
    hdfs dfs -put workflow.xml /user/<domainuser>/examples/apps/map-reduce/workflow.xml
    ```

7. `domainuser`Etki alanı için Kullanıcı adınızla değiştirin.

## <a name="define-the-properties-file-for-the-oozie-job"></a>Oozie işi için özellikler dosyasını tanımlayın

1. İş özellikleri için yeni bir dosya oluşturmak ve düzenlemek için aşağıdaki ifadeyi kullanın:

    ```bash
    nano job.properties
    ```

2. Nano düzenleyici açıldıktan sonra dosyanın içeriği olarak aşağıdaki XML 'i kullanın:

   ```bash
   nameNode=adl://home
   jobTracker=headnodehost:8050
   queueName=default
   examplesRoot=examples
   oozie.wf.application.path=${nameNode}/user/[domainuser]/examples/apps/map-reduce/workflow.xml
   hiveScript1=${nameNode}/user/${user.name}/countrowshive1.hql
   hiveScript2=${nameNode}/user/${user.name}/countrowshive2.hql
   oozie.use.system.libpath=true
   user.name=[domainuser]
   jdbcPrincipal=hive/<active-headnode-name>.<Domain>.com@<Domain>.COM
   jdbcURL=[jdbcurlvalue]
   hiveOutputDirectory1=${nameNode}/user/${user.name}/hiveresult1
   hiveOutputDirectory2=${nameNode}/user/${user.name}/hiveresult2
   ```

   - `adl://home` `nameNode` Birincil küme depolama alanı olarak Azure Data Lake Storage 1. sahipseniz, özelliği için URI 'yi kullanın. Azure Blob depolamayı kullanıyorsanız, olarak değiştirin `wasb://home` . Azure Data Lake Storage 2. kullanıyorsanız, öğesini olarak değiştirin `abfs://home` .
   - `domainuser`Etki alanı için Kullanıcı adınızla değiştirin.  
   - `ClusterShortName`Kümenin kısa adıyla değiştirin. Örneğin, küme adı https:// *[örnek bağlantı]* sechadoopcontoso.azurehdisnight.net ise, `clustershortname` kümenin ilk altı karakteri: **sechad**.  
   - `jdbcurlvalue`Hive YAPıLANDıRMASıNDAKI JDBC URL 'siyle değiştirin. Bir örnek: hive2://///
   - Dosyayı kaydetmek için CTRL + X ' i seçin ve ENTER `Y` ' u seçin. **Enter**

   Oozie işleri çalıştırılırken bu özellikler dosyasının yerel olarak mevcut olması gerekir.

## <a name="create-custom-hive-scripts-for-oozie-jobs"></a>Oozie işleri için özel Hive betikleri oluşturma

Aşağıdaki bölümlerde gösterildiği gibi Hive Server 1 ve Hive sunucu 2 için iki Hive komut dosyası oluşturabilirsiniz.

### <a name="hive-server-1-file"></a>Hive sunucu 1 dosyası

1. Hive Server 1 eylemleri için bir dosya oluşturun ve düzenleyin:

    ```bash
    nano countrowshive1.hql
    ```

2. Betiği oluşturun:

    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    select devicemake from hivesampletable limit 2;
    ```

3. Dosyayı Apache Hadoop Dağıtılmış Dosya Sistemi (bir) dosyaya kaydedin:

    ```bash
    hdfs dfs -put countrowshive1.hql countrowshive1.hql
    ```

### <a name="hive-server-2-file"></a>Hive sunucu 2 dosyası

1. Hive sunucu 2 eylemleri için bir alan oluşturma ve düzenleme:

    ```bash
    nano countrowshive2.hql
    ```

2. Betiği oluşturun:

    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}' 
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    select devicemodel from hivesampletable limit 2;
    ```

3. Dosyayı şu dosyaya kaydedin:

    ```bash
    hdfs dfs -put countrowshive2.hql countrowshive2.hql
    ```

## <a name="submit-oozie-jobs"></a>Oozie işleri gönder

ESP kümeleri için Oozie işlerinin gönderilmesi, ESP olmayan kümelerde Oozie işleri göndermeye benzer.

Daha fazla bilgi için bkz. [Apache Hadoop Ile Apache Oozie kullanarak Linux tabanlı Azure HDInsight 'ta iş akışı tanımlama ve çalıştırma](../hdinsight-use-oozie-linux-mac.md).

## <a name="results-from-an-oozie-job-submission"></a>Oozie işi gönderimi sonuçları

Kullanıcı için Oozie işleri çalıştırılır. Bu nedenle her ikisi de Apache Hadoop YARN ve Apache Ranger denetim günlükleri, kimliğine bürünülen kullanıcı olarak çalıştırılmakta olan işleri gösterir. Bir Oozie işinin komut satırı arabirimi çıkışı aşağıdaki kod gibi görünür:

```output
Job ID : 0000015-180626011240801-oozie-oozi-W
------------------------------------------------------------------------------------------------
Workflow Name : map-reduce-wf
App Path      : adl://home/user/alicetest/examples/apps/map-reduce/wf.xml
Status        : SUCCEEDED
Run           : 0
User          : alicetest
Group         : -
Created       : 2018-06-26 19:25 GMT
Started       : 2018-06-26 19:25 GMT
Last Modified : 2018-06-26 19:30 GMT
Ended         : 2018-06-26 19:30 GMT
CoordAction ID: -

Actions
------------------------------------------------------------------------------------------------
ID                      Status  Ext ID          ExtStatus   ErrCode
------------------------------------------------------------------------------------------------
0000015-180626011240801-oozie-oozi-W@:start:    OK  -           OK      -
------------------------------------------------------------------------------------------------
0000015-180626011240801-oozie-oozi-W@mr-test    OK  job_1529975666160_0051  SUCCEEDED   -
------------------------------------------------------------------------------------------------
0000015-180626011240801-oozie-oozi-W@myHive2    OK  job_1529975666160_0053  SUCCEEDED   -
------------------------------------------------------------------------------------------------
0000015-180626011240801-oozie-oozi-W@myHive OK  job_1529975666160_0055  SUCCEEDED   -
------------------------------------------------------------------------------------------------
0000015-180626011240801-oozie-oozi-W@end    OK  -           OK      -
-----------------------------------------------------------------------------------------------
```

Hive sunucu 2 eylemleri için Ranger denetim günlükleri Kullanıcı için eylemi çalıştıran Oozie 'yi gösterir. Ranger ve YARN görünümleri yalnızca küme yöneticisi tarafından görülebilir.

## <a name="configure-user-authorization-in-oozie"></a>Oozie 'de Kullanıcı yetkilendirmesini yapılandırma

Tek başına Oozie, kullanıcıların diğer kullanıcıların işlerini durdurmasını veya silmelerini engelleyebilen bir kullanıcı yetkilendirme yapılandırmasına sahiptir. Bu yapılandırmayı etkinleştirmek için öğesini olarak ayarlayın `oozie.service.AuthorizationService.security.enabled` `true` . 

Daha fazla bilgi için bkz. [Apache Oozie yükleme ve yapılandırma](https://oozie.apache.org/docs/3.2.0-incubating/AG_Install.html).

Ranger eklentisinin kullanılamadığı veya desteklenmediğinde Hive sunucu 1 gibi bileşenler için yalnızca kaba bir yetkilendirme mümkündür. Hassas yetkilendirme yalnızca Ranger eklentileri aracılığıyla kullanılabilir.

## <a name="get-the-oozie-web-ui"></a>Oozie Web Kullanıcı arabirimini alın

Oozie Web Kullanıcı arabirimi, kümedeki Oozie işlerinin durumuna Web tabanlı bir görünüm sağlar. Web Kullanıcı arabirimini almak için, ESP kümelerinde aşağıdaki adımları uygulayın:

1. Bir [Edge düğümü](../hdinsight-apps-use-edge-node.md) ekleyin ve [SSH Kerberos kimlik doğrulamasını](../hdinsight-hadoop-linux-use-ssh-unix.md)etkinleştirin.

2. Edge düğümüne SSH tünelini etkinleştirmek ve Web Kullanıcı arabirimine erişmek için [Oozie Web arabirimi](../hdinsight-use-oozie-linux-mac.md) adımlarını izleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Linux tabanlı Azure HDInsight 'ta bir iş akışını tanımlamak ve çalıştırmak için Apache Hadoop Ile Apache Oozie kullanın](../hdinsight-use-oozie-linux-mac.md).
- [SSH kullanarak HDInsight 'a (Apache Hadoop) bağlanın](../hdinsight-hadoop-linux-use-ssh-unix.md#authentication-domain-joined-hdinsight).
