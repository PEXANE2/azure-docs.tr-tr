---
title: Azure HDInsight kullanarak HBase sorunlarını giderme
description: HBase ve Azure HDInsight ile çalışma hakkında sık sorulan soruların yanıtlarını alın.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.custom: hdinsightactive, seodec18
ms.topic: conceptual
ms.date: 12/06/2018
ms.openlocfilehash: 13a4831d946eb7e25e586cafae4cae51b49fd8a7
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780768"
---
# <a name="troubleshoot-apache-hbase-by-using-azure-hdinsight"></a>Azure HDInsight kullanarak Apache HBase sorunlarını giderme

Apache ambarı 'nda Apache HBase yükleri ile çalışırken en popüler sorunlar ve çözümleri hakkında bilgi edinin.

## <a name="how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions"></a>Nasıl yaparım? birden çok atanmamış bölgeyle hbck komut raporları çalıştırmak istiyor musunuz?

`hbase hbck` Komutu çalıştırdığınızda görebileceğiniz yaygın bir hata iletisi, "birden fazla bölge atanmakta veya bölge zincirindeki delikler." şeklindedir.

HBase Master Kullanıcı arabiriminde, tüm bölge sunucularında dengesiz olan bölge sayısını görebilirsiniz. Ardından, bölge zincirindeki delikleri `hbase hbck` görmek için komutunu çalıştırabilirsiniz.

Delik, çevrimdışı bölgelerden kaynaklanıyor olabilir, bu nedenle öncelikle atamaları düzeltir. 

Atanmamış bölgeleri normal duruma getirmek için aşağıdaki adımları izleyin:

1. SSH kullanarak HDInsight HBase kümesinde oturum açın.
2. Apache ZooKeeper Shell ile bağlanmak için `hbase zkcli` komutunu çalıştırın.
3. `rmr /hbase/regions-in-transition` Komutunu`rmr /hbase-unsecure/regions-in-transition` veya komutunu çalıştırın.
4. `hbase zkcli` Kabuktan çıkmak için `exit` komutunu kullanın.
5. Apache ambarı Kullanıcı arabirimini açın ve ardından etkin HBase Master hizmetini yeniden başlatın.
6. `hbase hbck` Komutu yeniden çalıştırın (herhangi bir seçenek olmadan). Tüm bölgelerin atandığından emin olmak için bu komutun çıkışını kontrol edin.


## <a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>Bölge atamaları için hbck komutları kullanılırken zaman aşımı sorunlarını gidermek Nasıl yaparım? mı?

### <a name="issue"></a>Sorun

`hbck` Komutu kullandığınızda zaman aşımı sorunları için olası bir neden, "geçiş sürüyor" durumunda birkaç bölgenin uzun bir süre olması olabilir. Bu bölgeleri HBase Master Kullanıcı arabiriminde çevrimdışı olarak görebilirsiniz. Çok sayıda bölge geçişe çalıştığından HBase Master zaman aşımına uğrayabilir ve bu bölgeleri yeniden çevrimiçi hale getiremeyebilir.

### <a name="resolution-steps"></a>Çözüm adımları

1. SSH kullanarak HDInsight HBase kümesinde oturum açın.
2. Apache ZooKeeper Shell ile bağlanmak için `hbase zkcli` komutunu çalıştırın.
3. `rmr /hbase/regions-in-transition` Veya`rmr /hbase-unsecure/regions-in-transition` komutunu çalıştırın.
4. `hbase zkcli` Kabuktan çıkmak için `exit` komutunu kullanın.
5. Ambarı Kullanıcı arabiriminde, etkin HBase Master hizmetini yeniden başlatın.
6. `hbase hbck -fixAssignments` Komutu yeniden çalıştırın.

## <a name="how-do-i-force-disable-hdfs-safe-mode-in-a-cluster"></a>Nasıl yaparım?, bir kümede ne tür kullanımı zorla modu devre dışı bırakılır?

### <a name="issue"></a>Sorun

Yerel Apache Hadoop Dağıtılmış Dosya Sistemi (bir) HDInsight kümesinde güvenli modda takılmış.

### <a name="detailed-description"></a>Ayrıntılı bir açıklaması

Bu hata, aşağıdaki bir işlem komutunu çalıştırdığınızda bir hatadan kaynaklanıyor olabilir:

```apache
hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
```

Komutu çalıştırmaya çalıştığınızda görebileceğiniz hata şöyle görünür:

```apache
hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
17/04/05 16:20:52 WARN retry.RetryInvocationHandler: Exception while invoking ClientNamenodeProtocolTranslatorPB.mkdirs over hn0-spark2.2oyzcdm4sfjuzjmj5dnmvscjpg.dx.internal.cloudapp.net/10.0.0.22:8020. Not retrying because try once and fail.
org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.hdfs.server.namenode.SafeModeException): Cannot create directory /temp. Name node is in safe mode.
It was turned on manually. Use "hdfs dfsadmin -safemode leave" to turn safe mode off.
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkNameNodeSafeMode(FSNamesystem.java:1359)
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.mkdirs(FSNamesystem.java:4010)
        at org.apache.hadoop.hdfs.server.namenode.NameNodeRpcServer.mkdirs(NameNodeRpcServer.java:1102)
        at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.mkdirs(ClientNamenodeProtocolServerSideTranslatorPB.java:630)
        at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java)
        at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:640)
        at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:982)
        at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2313)
        at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2309)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:422)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1724)
        at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2307)
        at org.apache.hadoop.ipc.Client.getRpcResponse(Client.java:1552)
        at org.apache.hadoop.ipc.Client.call(Client.java:1496)
        at org.apache.hadoop.ipc.Client.call(Client.java:1396)
        at org.apache.hadoop.ipc.ProtobufRpcEngine$Invoker.invoke(ProtobufRpcEngine.java:233)
        at com.sun.proxy.$Proxy10.mkdirs(Unknown Source)
        at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolTranslatorPB.mkdirs(ClientNamenodeProtocolTranslatorPB.java:603)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invokeMethod(RetryInvocationHandler.java:278)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invoke(RetryInvocationHandler.java:194)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invoke(RetryInvocationHandler.java:176)
        at com.sun.proxy.$Proxy11.mkdirs(Unknown Source)
        at org.apache.hadoop.hdfs.DFSClient.primitiveMkdir(DFSClient.java:3061)
        at org.apache.hadoop.hdfs.DFSClient.mkdirs(DFSClient.java:3031)
        at org.apache.hadoop.hdfs.DistributedFileSystem$24.doCall(DistributedFileSystem.java:1162)
        at org.apache.hadoop.hdfs.DistributedFileSystem$24.doCall(DistributedFileSystem.java:1158)
        at org.apache.hadoop.fs.FileSystemLinkResolver.resolve(FileSystemLinkResolver.java:81)
        at org.apache.hadoop.hdfs.DistributedFileSystem.mkdirsInternal(DistributedFileSystem.java:1158)
        at org.apache.hadoop.hdfs.DistributedFileSystem.mkdirs(DistributedFileSystem.java:1150)
        at org.apache.hadoop.fs.FileSystem.mkdirs(FileSystem.java:1898)
        at org.apache.hadoop.fs.shell.Mkdir.processNonexistentPath(Mkdir.java:76)
        at org.apache.hadoop.fs.shell.Command.processArgument(Command.java:273)
        at org.apache.hadoop.fs.shell.Command.processArguments(Command.java:255)
        at org.apache.hadoop.fs.shell.FsCommand.processRawArguments(FsCommand.java:119)
        at org.apache.hadoop.fs.shell.Command.run(Command.java:165)
        at org.apache.hadoop.fs.FsShell.run(FsShell.java:297)
        at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:76)
        at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:90)
        at org.apache.hadoop.fs.FsShell.main(FsShell.java:350)
mkdir: Cannot create directory /temp. Name node is in safe mode.
```

### <a name="probable-cause"></a>Olası neden

HDInsight kümesi çok az sayıda düğüme göre ölçeklendirildi. Düğüm sayısı, aşağıdaki veya daha fazla.

### <a name="resolution-steps"></a>Çözüm adımları 

1. Aşağıdaki komutları çalıştırarak HDInsight kümesindeki bir durum durumunu alın:

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -report
   ```

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -report
   Safe mode is ON
   Configured Capacity: 3372381241344 (3.07 TB)
   Present Capacity: 3138625077248 (2.85 TB)
   DFS Remaining: 3102710317056 (2.82 TB)
   DFS Used: 35914760192 (33.45 GB)
   DFS Used%: 1.14%
   Under replicated blocks: 0
   Blocks with corrupt replicas: 0
   Missing blocks: 0
   Missing blocks (with replication factor 1): 0

   -------------------------------------------------
   Live datanodes (8):

   Name: 10.0.0.17:30010 (10.0.0.17)
   Hostname: 10.0.0.17
   Decommission Status : Normal
   Configured Capacity: 421547655168 (392.60 GB)
   DFS Used: 5288128512 (4.92 GB)
   Non DFS Used: 29087272960 (27.09 GB)
   DFS Remaining: 387172253696 (360.58 GB)
   DFS Used%: 1.25%
   DFS Remaining%: 91.85%
   Configured Cache Capacity: 0 (0 B)
   Cache Used: 0 (0 B)
   Cache Remaining: 0 (0 B)
   Cache Used%: 100.00%
   Cache Remaining%: 0.00%
   Xceivers: 2
   Last contact: Wed Apr 05 16:22:00 UTC 2017
   ...

   ```
2. Ayrıca, aşağıdaki komutları kullanarak HDInsight kümesinde TBU öğesinin bütünlüğünü de denetleyebilirsiniz:

   ```apache
   hdfs fsck -D "fs.default.name=hdfs://mycluster/" /
   ```

   ```apache
   Connecting to namenode via http://hn0-spark2.2oyzcdm4sfjuzjmj5dnmvscjpg.dx.internal.cloudapp.net:30070/fsck?ugi=hdiuser&path=%2F
   FSCK started by hdiuser (auth:SIMPLE) from /10.0.0.22 for path / at Wed Apr 05 16:40:28 UTC 2017
   ....................................................................................................

   ....................................................................................................
   ..................Status: HEALTHY
   Total size:    9330539472 B
   Total dirs:    37
   Total files:   2618
   Total symlinks:                0 (Files currently being written: 2)
   Total blocks (validated):      2535 (avg. block size 3680686 B)
   Minimally replicated blocks:   2535 (100.0 %)
   Over-replicated blocks:        0 (0.0 %)
   Under-replicated blocks:       0 (0.0 %)
   Mis-replicated blocks:         0 (0.0 %)
   Default replication factor:    3
   Average block replication:     3.0
   Corrupt blocks:                0
   Missing replicas:              0 (0.0 %)
   Number of data-nodes:          8
   Number of racks:               1
   FSCK ended at Wed Apr 05 16:40:28 UTC 2017 in 187 milliseconds

   The filesystem under path '/' is HEALTHY
   ```

3. Eksik, bozuk veya bir çoğaltılan blok olmadığını veya bu blokların yoksayılabileceğini belirlerseniz, ad düğümünü güvenli moddan almak için aşağıdaki komutu çalıştırın:

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -safemode leave
   ```


## <a name="how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix"></a>Apache Phoenix ile JDBC veya SQLLine bağlantı sorunlarını giderin Nasıl yaparım??

### <a name="resolution-steps"></a>Çözüm adımları

Apache Phoenix ile bağlantı kurmak için, etkin bir Apache ZooKeeper düğümünün IP adresini sağlamanız gerekir. Sqlline.py 'in bağlamaya çalıştığı ZooKeeper hizmetinin çalışır ve çalışıyor olduğundan emin olun.
1. SSH kullanarak HDInsight kümesinde oturum açın.
2. Aşağıdaki komutu girin:
                
   ```apache
           "/usr/hdp/current/phoenix-client/bin/sqlline.py <IP of machine where Active Zookeeper is running"
   ```

   > [!Note] 
   > Active ZooKeeper düğümünün IP adresini, ambarı kullanıcı arabiriminden alabilirsiniz. **HBase** > **hızlı**Bağlantılar > **ZK(\* etkin)**  > **Zookeeper Info**sayfasına gidin. 

3. Sqlline.py, Phoenix 'e bağlanırsa ve zaman aşımına uğramaz, Phoenix 'in kullanılabilirliğini ve sistem durumunu doğrulamak için şu komutu çalıştırın:

   ```apache
           !tables
           !quit
   ```      
4. Bu komut çalışırsa, sorun yoktur. Kullanıcı tarafından girilen IP adresi yanlış olabilir. Ancak, komut uzun bir süre durakladığında ve aşağıdaki hatayı görüntülüyorsa, 5. adımdan devam edin.

   ```apache
           Error while connecting to sqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting to jdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings. 
   ```

5. Phoenix SISTEMININ koşulunu tanılamak için baş düğümden (hn0) aşağıdaki komutları çalıştırın. Katalog tablosu:

   ```apache
            hbase shell
                
           count 'SYSTEM.CATALOG'
   ```

   Komut şuna benzer bir hata döndürmelidir: 

   ```apache
           ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189) 
   ```
6. Apache ambarı Kullanıcı arabiriminde, tüm ZooKeeper düğümlerinde HMaster hizmetini yeniden başlatmak için aşağıdaki adımları izleyin:

    1. HBase 'in **Özet** bölümünde **HBase** > **etkin HBase Master**' a gidin. 
    2. **Bileşenler** bölümünde HBase Master hizmetini yeniden başlatın.
    3. Kalan tüm **bekleme HBase Master** Hizmetleri için bu adımları yineleyin. 

HBase Master hizmetinin kurtarma sürecini sabitmesi ve tamamlaması beş dakikaya kadar sürebilir. Birkaç dakika sonra, SISTEMI doğrulamak için sqlline.py komutlarını tekrarlayın. Katalog tablosu çalışıyor ve sorgulanabilecek. 

SISTEM. Katalog tablosu normal 'e geri döndüğünüzde, Phoenix 'e yönelik bağlantı sorununun otomatik olarak çözülmesi gerekir.


## <a name="what-causes-a-master-server-to-fail-to-start"></a>Ana sunucunun başlatılmasına neden olan ne olur?

### <a name="error"></a>Hata 

Atomik yeniden adlandırma hatası oluşur.

### <a name="detailed-description"></a>Ayrıntılı bir açıklaması

Başlangıç işlemi sırasında, HMaster birçok başlatma adımını tamamlar. Bunlar, verileri karalama (. tmp) klasöründen veri klasörüne taşımayı içerir. HMaster Ayrıca, yanıt vermeyen bölge sunucuları olup olmadığını görmek için yazma günlükleri (WALs) klasörüne de bakar. 

Başlangıç sırasında, hmaster bu klasörlerde temel `list` bir komut yapar. Her zaman, HMaster bu klasörlerin hiçbirinde beklenmeyen bir dosya görür ve bir özel durum oluşturur ve başlamaz.  

### <a name="probable-cause"></a>Olası neden

Bölge sunucusu günlüklerinde, dosya oluşturma zaman çizelgesini belirlemeyi deneyin ve sonra dosyanın oluşturulduğu sırada bir işlem kilitlenmesinin olup olmadığını görün. (Bunu yaparken size yardımcı olmak için HBase desteğine başvurun.) Bu, bu hataya ulaşmaktan kaçınmak ve düzgün işlem kapanmalarının sağlanması için daha güçlü mekanizmalar sağlamamıza yardımcı olur.

### <a name="resolution-steps"></a>Çözüm adımları

Çağrı yığınını denetleyin ve soruna neden olabilecek klasörü belirlemeyi deneyin (örneğin, WALs klasörü veya. tmp klasörü olabilir). Ardından, Cloud Explorer 'da veya,,, sorun dosyasını bulmaya çalışın. Genellikle bu bir \*-renamepending. JSON dosyasıdır. \*(-Renamepending. JSON dosyası, IDB sürücüsünde atomik yeniden adlandırma işlemini uygulamak için kullanılan bir günlük dosyasıdır. Bu uygulamadaki hatalar nedeniyle, bu dosyalar işlem kilitlenmelerinden sonra bırakılabilir ve bu şekilde devam eder.) Zorla-bu dosyayı bulut Gezgini ' nde veya bir. 

Bazen, bu konumda *$ $ $. $ $ $* gibi bir ad olarak adlandırılan geçici bir dosya da olabilir. Bu dosyayı görmek için, `ls` bu dosyayı kullanmanız gerekir; dosyayı Cloud Explorer 'da göremezsiniz. Bu dosyayı silmek için, `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`.  

Bu komutları çalıştırdıktan sonra, HMaster hemen başlamalıdır. 

### <a name="error"></a>Hata

Xxx bölgesi için *HBase: meta* öğesinde hiçbir sunucu adresi listelenmez.

### <a name="detailed-description"></a>Ayrıntılı bir açıklaması

Linux kümenizde *HBase: meta* tablosunun çevrimiçi olmadığını belirten bir ileti görebilirsiniz. Çalıştırmak `hbck` , "HBase: meta tablo yineleme kimliği 'ni hiçbir bölgede bulunamadığını" bildirebilir. Bu sorun, HBase 'i yeniden başlattıktan sonra HMaster 'nin başlatılamadığından hata olabilir. HMaster günlüklerinde şu iletiyi görebilirsiniz: "HBase için sunucu adresi listelenmedi: bölge HBase için meta: yedek \<bölge adı\>".  

### <a name="resolution-steps"></a>Çözüm adımları

1. HBase kabuğunda, aşağıdaki komutları girin (gerçek değerleri uygun olarak değiştirin):  

   ```apache
   > scan 'hbase:meta'  
   ```

   ```apache
   > delete 'hbase:meta','hbase:backup <region name>','<column name>'  
   ```

2. *HBase: Namespace* girdisini silin. Bu giriş, *HBase: Namespace* tablosu tarandığında bildirilen aynı hata olabilir.

3. Çalışan bir durumda HBase 'i getirmek için, ambarı Kullanıcı arabiriminde, etkin HMaster hizmetini yeniden başlatın.  

4. HBase kabuğunda tüm çevrimdışı tabloları görüntülemek için aşağıdaki komutu çalıştırın:

   ```apache 
   hbase hbck -ignorePreCheckPermission -fixAssignments 
   ```

### <a name="additional-reading"></a>Ek okuma

[HBase tablosu işlenemiyor](https://stackoverflow.com/questions/4794092/unable-to-access-hbase-table)


### <a name="error"></a>Hata

HMaster, "Java. IO. IOException:" şuna benzer önemli bir özel durumla zaman aşımına uğrar: Ad alanı tablosunun atanmasını bekleyen 300000ms zaman aşımına uğradı. "

### <a name="detailed-description"></a>Ayrıntılı bir açıklaması

Bu sorunla, HMaster hizmetlerinizi yeniden başlattığınızda temizlenen çok sayıda tablonuz ve bölgesi varsa karşılaşabilirsiniz. Yeniden başlatma başarısız olabilir ve yukarıdaki hata iletisini görürsünüz.  

### <a name="probable-cause"></a>Olası neden

Bu, HMaster hizmetinde bilinen bir sorundur. Genel küme başlangıç görevleri uzun zaman alabilir. Ad alanı tablosu henüz atanmadığından HMaster kapanıyor. Bu, yalnızca büyük miktarda temizlenen verilerin bulunduğu senaryolarda ve beş dakikalık bir zaman aşımı süresinin yetersiz olması durumunda oluşur.
  
### <a name="resolution-steps"></a>Çözüm adımları

1. Apache ambarı Kullanıcı arabiriminde, **HBase** > **configs**' a gidin. Özel HBase-site. xml dosyasında aşağıdaki ayarı ekleyin: 

   ```apache
   Key: hbase.master.namespace.init.timeout Value: 2400000  
   ```

2. Gerekli hizmetleri (HMaster ve belki de diğer HBase hizmetlerini) yeniden başlatın.  


## <a name="what-causes-a-restart-failure-on-a-region-server"></a>Bir bölge sunucusunda yeniden başlatma hatasına neden olur?

### <a name="issue"></a>Sorun

Bir bölge sunucusunda yeniden başlatma hatası, en iyi yöntemler aşağıdaki şekilde engellenebilir. HBase bölge sunucularını yeniden başlatmayı planlarken ağır iş yükü etkinliğini duraklamanızı öneririz. Bir uygulama, kapatmadan önce bölge sunucularıyla bağlanmaya devam ediyorsa, bölge sunucusu yeniden başlatma işlemi birkaç dakika daha yavaş olur. Ayrıca, ilk olarak tüm tabloları temizlemek iyi bir fikirdir. Tabloları temizleme hakkında bir başvuru için bkz [. HDInsight HBase: Tabloları](https://web.archive.org/web/20190112153155/https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/)reçeteye göre Apache HBase kümesi yeniden başlatma süresini geliştirme.

Apache ambarı kullanıcı arabiriminden HBase bölge sunucularında yeniden başlatma işlemini başlatırsanız, bölge sunucularının hemen bir yere gittiğini, ancak doğrudan yeniden başlatmayınızı görürsünüz. 

Arka planda neler oluyordu: 

1. Ambarı Aracısı bölge sunucusuna bir durdurma isteği gönderir.
2. Ambarı Aracısı bölge sunucusunun düzgün şekilde kapanması için 30 saniye bekler. 
3. Uygulamanız bölge sunucusuna bağlanmaya devam ederse, sunucu hemen kapatılmaz. 30 saniyelik zaman aşımı, kapatmadan önce sona erer. 
4. 30 saniye sonra, ambarı Aracısı bölge sunucusuna bir zorla-KILL (`kill -9`) komutu gönderir. Bunu, ambarı Aracısı günlüğünde (ilgili çalışan düğümünün/var/log/dizininde) görebilirsiniz:

   ```apache
           2017-03-21 13:22:09,171 - Execute['/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh --config /usr/hdp/current/hbase-regionserver/conf stop regionserver'] {'only_if': 'ambari-sudo.sh  -H -E t
           est -f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >/dev/null 2>&1', 'on_timeout': '! ( ambari-sudo.sh  -H -E test -
           f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >/dev/null 2>&1 ) || ambari-sudo.sh -H -E kill -9 `ambari-sudo.sh  -H 
           -E cat /var/run/hbase/hbase-hbase-regionserver.pid`', 'timeout': 30, 'user': 'hbase'}
           2017-03-21 13:22:40,268 - Executing '! ( ambari-sudo.sh  -H -E test -f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >
           /dev/null 2>&1 ) || ambari-sudo.sh -H -E kill -9 `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid`'. Reason: Execution of 'ambari-sudo.sh su hbase -l -s /bin/bash -c 'export  
           PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/var/lib/ambari-agent ; /usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh --config /usr/hdp/curre
           nt/hbase-regionserver/conf stop regionserver was killed due timeout after 30 seconds
           2017-03-21 13:22:40,285 - File['/var/run/hbase/hbase-hbase-regionserver.pid'] {'action': ['delete']}
           2017-03-21 13:22:40,285 - Deleting File['/var/run/hbase/hbase-hbase-regionserver.pid']
   ```
   Ani kapatmadan, bölge sunucusu işlemi durdurulmuş olsa bile işlemle ilişkili bağlantı noktası yayımlanmayabilir. Bu durum, aşağıdaki günlüklerde gösterildiği gibi, bölge sunucusu başlatıldığında bir AddressBindException 'ye yol açabilir. Bu işlemi, bölge sunucusunun başlayabileceği çalışan düğümlerdeki/var/log/HBase dizininde bulunan Region-Server. log dosyasında doğrulayabilirsiniz. 

   ```apache

   2017-03-21 13:25:47,061 ERROR [main] regionserver.HRegionServerCommandLine: Region server exiting
   java.lang.RuntimeException: Failed construction of Regionserver: class org.apache.hadoop.hbase.regionserver.HRegionServer
   at org.apache.hadoop.hbase.regionserver.HRegionServer.constructRegionServer(HRegionServer.java:2636)
   at org.apache.hadoop.hbase.regionserver.HRegionServerCommandLine.start(HRegionServerCommandLine.java:64)
   at org.apache.hadoop.hbase.regionserver.HRegionServerCommandLine.run(HRegionServerCommandLine.java:87)
   at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:76)
   at org.apache.hadoop.hbase.util.ServerCommandLine.doMain(ServerCommandLine.java:126)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.main(HRegionServer.java:2651)
        
   Caused by: java.lang.reflect.InvocationTargetException
   at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
   at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:57)
   at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
   at java.lang.reflect.Constructor.newInstance(Constructor.java:526)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.constructRegionServer(HRegionServer.java:2634)
   ... 5 more
        
   Caused by: java.net.BindException: Problem binding to /10.2.0.4:16020 : Address already in use
   at org.apache.hadoop.hbase.ipc.RpcServer.bind(RpcServer.java:2497)
   at org.apache.hadoop.hbase.ipc.RpcServer$Listener.<init>(RpcServer.java:580)
   at org.apache.hadoop.hbase.ipc.RpcServer.<init>(RpcServer.java:1982)
   at org.apache.hadoop.hbase.regionserver.RSRpcServices.<init>(RSRpcServices.java:863)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.createRpcServices(HRegionServer.java:632)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.<init>(HRegionServer.java:532)
   ... 10 more
        
   Caused by: java.net.BindException: Address already in use
   at sun.nio.ch.Net.bind0(Native Method)
   at sun.nio.ch.Net.bind(Net.java:463)
   at sun.nio.ch.Net.bind(Net.java:455)
   at sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:223)
   at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:74)
   at org.apache.hadoop.hbase.ipc.RpcServer.bind(RpcServer.java:2495)
   ... 15 more
   ```

### <a name="resolution-steps"></a>Çözüm adımları

1. Yeniden başlatma işlemini başlatmadan önce HBase bölge sunucularındaki yükü azaltmayı deneyin. 
2. Alternatif olarak (1. adım yardım yoksa) aşağıdaki komutları kullanarak çalışan düğümlerinde bölge sunucularını el ile yeniden başlatmayı deneyin:

   ```apache
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"   
   ```

### <a name="see-also"></a>Ayrıca Bkz.
[Azure HDInsight'ı kullanarak sorun giderme](../../hdinsight/hdinsight-troubleshoot-guide.md)
