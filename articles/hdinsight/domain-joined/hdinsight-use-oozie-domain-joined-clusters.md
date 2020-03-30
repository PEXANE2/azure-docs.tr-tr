---
title: Apache Oozie iş akışları & Kurumsal Güvenlik - Azure HDInsight
description: Azure HDInsight Kurumsal Güvenlik Paketi'ni kullanarak Apache Oozie iş akışlarını güvence altına alayın. Bir Oozie iş akışını nasıl tanımlayıp bir Oozie işi göndereceğini öğrenin.
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,seodec18
ms.date: 12/09/2019
ms.openlocfilehash: 9ef54707f7fac3dd1328e29f6d05f62c1dee2561
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78194912"
---
# <a name="run-apache-oozie-in-hdinsight-hadoop-clusters-with-enterprise-security-package"></a>Kurumsal Güvenlik Paketi ile HDInsight Hadoop kümelerinde Apache Oozie'yi çalıştırın

Apache Oozie, Apache Hadoop işlerini yöneten bir iş akışı ve koordinasyon sistemidir. Oozie Hadoop yığını ile entegre edilmiştir ve aşağıdaki işleri destekler:

- Apache MapReduce
- Apaçi Domuz
- Apaçi Kovanı
- Apaçi Sqoop

Oozie'yi, Java programları veya kabuk komut dosyaları gibi bir sisteme özgü işleri zamanlamak için de kullanabilirsiniz.

## <a name="prerequisite"></a>Önkoşul

Kurumsal Güvenlik Paketi (ESP) içeren bir Azure HDInsight Hadoop kümesi. Bkz. [ESP ile HDInsight kümelerini yapılandırın.](./apache-domain-joined-configure-using-azure-adds.md)

> [!NOTE]  
> ESP dışı kümelerde Oozie'nin nasıl kullanılacağına ilişkin ayrıntılı talimatlar için Linux [tabanlı Azure HDInsight'ta Apache Oozie iş akışlarını kullanın'](../hdinsight-use-oozie-linux-mac.md)a bakın.

## <a name="connect-to-an-esp-cluster"></a>ESP kümesine bağlanma

Secure Shell (SSH) hakkında daha fazla bilgi için, [SSH kullanarak HDInsight'a (Hadoop) bağlan'a bakın.](../hdinsight-hadoop-linux-use-ssh-unix.md)

1. SSH kullanarak HDInsight kümesine bağlanın:

    ```bash
    ssh [DomainUserName]@<clustername>-ssh.azurehdinsight.net
    ```

1. Başarılı Kerberos kimlik doğrulamasını `klist` doğrulamak için komutu kullanın. Değilse, Kerberos kimlik doğrulaması başlatmak için kullanın. `kinit`

1. Azure Veri Gölü Depolamasına erişmek için gereken OAuth belirteci kaydetmek için HDInsight ağ geçidinde oturum açın:

    ```bash
    curl -I -u [DomainUserName@Domain.com]:[DomainUserPassword] https://<clustername>.azurehdinsight.net
    ```

    **200 OK** durum yanıt kodu başarılı bir kayıt gösterir. 401 gibi yetkisiz bir yanıt alınırsa kullanıcı adını ve parolayı denetleyin.

## <a name="define-the-workflow"></a>İş akışını tanımlama

Oozie iş akışı tanımları Apache Hadoop Süreç Tanımı Dili (hPDL) olarak yazılır. hPDL bir XML işlem tanımı dilidir. İş akışını tanımlamak için aşağıdaki adımları izleyin:

1. Etki alanı kullanıcısının çalışma alanını ayarlama:

   ```bash
   hdfs dfs -mkdir /user/<DomainUser>
   cd /home/<DomainUserPath>
   cp /usr/hdp/<ClusterVersion>/oozie/doc/oozie-examples.tar.gz .
   tar -xvf oozie-examples.tar.gz
   hdfs dfs -put examples /user/<DomainUser>/
   ```

   Etki `DomainUser` alanı kullanıcı adı ile değiştirin.
   Etki `DomainUserPath` alanı kullanıcısı için ev dizini yolunu değiştirin.
   Küme `ClusterVersion` veri platformu sürümünüzle değiştirin.

2. Yeni bir dosya oluşturmak ve bunları yeniden oluşturmak için aşağıdaki deyimi kullanın:

   ```bash
   nano workflow.xml
   ```

3. Nano düzenleyici açıldıktan sonra, dosya içeriği olarak aşağıdaki XML'i girin:

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

4. Kümenin adı ile değiştirin. `clustername`

5. Dosyayı kaydetmek için **Ctrl+X'i**seçin. **Y**girin. Ardından **Enter'u**seçin.

    İş akışı iki bölüme ayrılır:

   - **Kimlik bilgisi.** Bu bölümde, Oozie eylemlerinin doğrulanması için kullanılan kimlik bilgileri alınır:

     Bu örnek, Hive eylemleri için kimlik doğrulaması kullanır. Daha fazla bilgi edinmek için [Eylem Kimlik Doğrulaması'na](https://oozie.apache.org/docs/4.2.0/DG_ActionAuthentication.html)bakın.

     Kimlik bilgisi hizmeti, Oozie eylemlerinin Hadoop hizmetlerine erişmek için kullanıcıyı taklit etmesine olanak tanır.

   - **Eylem.** Bu bölümde üç eylem vardır: harita azaltma, Hive server 2 ve Hive server 1:

     - Harita azaltma eylemi, toplanan sözcük sayısı çıktılarını oluşturan harita azaltma için bir Oozie paketinden bir örnek çalıştırıyor.

     - Hive server 2 ve Hive server 1 eylemleri HDInsight ile sağlanan örnek bir Hive tablosunda bir sorgu çalıştırın.

     Kovan eylemleri, eylem öğesindeki anahtar sözcüğü `cred` kullanarak kimlik doğrulama için kimlik bilgileri bölümünde tanımlanan kimlik bilgilerini kullanır.

6. Dosyayı kopyalamak için `workflow.xml` `/user/<domainuser>/examples/apps/map-reduce/workflow.xml`aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -put workflow.xml /user/<domainuser>/examples/apps/map-reduce/workflow.xml
    ```

7. Etki `domainuser` alanı için kullanıcı adınız ile değiştirin.

## <a name="define-the-properties-file-for-the-oozie-job"></a>Oozie işi için özellikler dosyasını tanımlama

1. İş özellikleri için yeni bir dosya oluşturmak ve bunları yeniden oluşturmak için aşağıdaki deyimi kullanın:

    ```bash
    nano job.properties
    ```

2. Nano düzenleyici açıldıktan sonra, dosyanın içeriği olarak aşağıdaki XML'i kullanın:

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

   - Birincil `adl://home` küme depolama `nameNode` alanınız olarak Azure Veri Gölü Depolama Gen1'iniz varsa özellik için URI'yi kullanın. Azure Blob Depolama kullanıyorsanız, bunu ' `wasb://home`olarak değiştirin. Azure Veri Gölü Depolama Gen2 kullanıyorsanız, bunu `abfs://home`' olarak değiştirin.
   - Etki `domainuser` alanı için kullanıcı adınız ile değiştirin.  
   - Kümeiçin kısa adla değiştirin. `ClusterShortName` Örneğin, küme adı sechadoopcontoso.azurehdisnight.net *https:// [örnek bağlantı]* ise, kümenin ilk altı `clustershortname` karakteridir: **sechad**.  
   - Hive yapılandırmasından JDBC URL ile değiştirin. `jdbcurlvalue` Bir örnek jdbc:hive2://headnodehost:10001/;transportMode=http.
   - Dosyayı kaydetmek için Ctrl+X'i seçin, girin `Y`ve sonra **Enter'u**seçin.

   Bu özellikler dosyası, Oozie işleri çalıştırırken yerel olarak bulunması gerekir.

## <a name="create-custom-hive-scripts-for-oozie-jobs"></a>Oozie işleri için özel Hive komut dosyaları oluşturma

Aşağıdaki bölümlerde gösterildiği gibi Hive server 1 ve Hive server 2 için iki Hive komut dosyası oluşturabilirsiniz.

### <a name="hive-server-1-file"></a>Hive server 1 dosyası

1. Hive server 1 eylemleri için bir dosya oluşturun ve düzenleme:

    ```bash
    nano countrowshive1.hql
    ```

2. Komut dosyasını oluşturun:

    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    select devicemake from hivesampletable limit 2;
    ```

3. Dosyayı Apache Hadoop Dağıtılmış Dosya Sistemine (HDFS) kaydedin:

    ```bash
    hdfs dfs -put countrowshive1.hql countrowshive1.hql
    ```

### <a name="hive-server-2-file"></a>Hive server 2 dosyası

1. Hive sunucusu 2 eylemleri için bir alan oluşturun ve düzenleme:

    ```bash
    nano countrowshive2.hql
    ```

2. Komut dosyasını oluşturun:

    ```sql
    INSERT OVERWRITE DIRECTORY '${hiveOutputDirectory1}' 
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
    select devicemodel from hivesampletable limit 2;
    ```

3. Dosyayı HDFS'ye kaydedin:

    ```bash
    hdfs dfs -put countrowshive2.hql countrowshive2.hql
    ```

## <a name="submit-oozie-jobs"></a>Oozie işleri gönder

ESP kümeleri için Oozie işleri göndermek, ESP olmayan kümelerde Oozie işleri göndermeye benzer.

Daha fazla bilgi için, [Linux tabanlı Azure HDInsight'ta bir iş akışını tanımlamak ve çalıştırmak için Apache Oozie'yi Apache Hadoop ile kullanın'a](../hdinsight-use-oozie-linux-mac.md)bakın.

## <a name="results-from-an-oozie-job-submission"></a>Bir Oozie iş gönderme sonuçları

Oozie işleri kullanıcı için çalıştırılır. Yani hem Apache Hadoop İPN hem de Apache Ranger denetim günlükleri, taklit edilen kullanıcı olarak çalıştırılan işleri gösterir. Bir Oozie işinin komut satırı arabirimi çıktısı aşağıdaki kodgibi görünür:

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

Hive sunucusu 2 eylemleri için Ranger denetim günlükleri Oozie kullanıcı için eylem çalışan gösterir. Ranger ve İPLik görünümleri yalnızca küme yöneticisi tarafından görülebilir.

## <a name="configure-user-authorization-in-oozie"></a>Oozie'de kullanıcı yetkilendirmesi yapılandırma

Oozie tek başına, kullanıcıların diğer kullanıcıların işlerini durdurmalarını veya silmelerini engelleyebilen bir kullanıcı yetkilendirme yapılandırmasına sahiptir. Bu yapılandırmayı etkinleştirmek `oozie.service.AuthorizationService.security.enabled` `true`için, ' yi . 

Daha fazla bilgi için [Bkz. Apache Oozie Kurulum ve Yapılandırma.](https://oozie.apache.org/docs/3.2.0-incubating/AG_Install.html)

Ranger eklentisinin kullanılamadığı veya desteklenmediği Hive server 1 gibi bileşenler için yalnızca kaba taneli HDFS yetkilendirmesi mümkündür. İnce taneli yetkilendirme sadece Ranger eklentileri aracılığıyla kullanılabilir.

## <a name="get-the-oozie-web-ui"></a>Oozie web UI'yi alın

Oozie web Kullanıcı İyiuBilgisi kümedeki Oozie işlerinin durumuna web tabanlı bir görünüm sağlar. Web Web UI'sini almak için ESP kümelerinde aşağıdaki adımları izleyin:

1. Bir [kenar düğümü](../hdinsight-apps-use-edge-node.md) ekleyin ve [SSH Kerberos kimlik doğrulamasını etkinleştirin.](../hdinsight-hadoop-linux-use-ssh-unix.md)

2. Kenar düğümüne SSH tünellerini etkinleştirmek ve web UI'ye erişmek için [Oozie web UI](../hdinsight-use-oozie-linux-mac.md) adımlarını izleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Linux tabanlı Azure HDInsight'ta bir iş akışını tanımlamak ve çalıştırmak için Apache Oozie'yi Apache Hadoop ile kullanın.](../hdinsight-use-oozie-linux-mac.md)
- [SSH kullanarak HDInsight'a (Apache Hadoop) bağlanın.](../hdinsight-hadoop-linux-use-ssh-unix.md#authentication-domain-joined-hdinsight)
