---
title: Azure Databricks için bölgesel olağanüstü durum kurtarma
description: Bu makalede, Azure Veri Tuğlaları'nda olağanüstü durum kurtarma yapma yaklaşımı açıklanmaktadır.
services: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 03/13/2019
ms.openlocfilehash: 2604d5b357feacce3493b4a4ded971144262611d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77161945"
---
# <a name="regional-disaster-recovery-for-azure-databricks-clusters"></a>Azure Databricks kümeleri için bölgesel olağanüstü durum kurtarma

Bu makalede, Azure Veri Tuğlaları kümeleri için yararlı olan olağanüstü durum kurtarma mimarisi ve bu tasarımı gerçekleştirme adımları açıklanmaktadır.

## <a name="azure-databricks-architecture"></a>Azure Databricks mimarisi

Yüksek düzeyde, Azure portalından bir Azure Databricks çalışma alanı oluşturduğunuzda, yönetilen bir [cihaz](../azure-resource-manager/managed-applications/overview.md) aboneliğinizde, seçilen Azure bölgesinde (örneğin, Batı ABD) Azure kaynağı olarak dağıtılır. Bu cihaz, aboneliğinizde kullanılabilen bir [Ağ Güvenlik Grubu](../virtual-network/manage-network-security-group.md) ve Azure Depolama hesabı içeren bir Azure Sanal [Ağı'nda](../virtual-network/virtual-networks-overview.md) dağıtılır. Sanal ağ, Databricks çalışma alanına çevre düzeyi güvenliği sağlar ve ağ güvenlik grubu aracılığıyla korunur. Çalışma alanı içinde, işçi ve sürücü VM türü ve Databricks çalışma zamanı sürümünü sağlayarak Databricks kümeleri oluşturabilirsiniz. Kalıcı veriler, Azure Blob Depolama veya Azure Veri Gölü Depolaması olabilecek depolama hesabınızda kullanılabilir. Küme oluşturulduktan sonra, işleri not defterleri, REST API'leri, ODBC/JDBC uç noktaları aracılığıyla belirli bir kümeye ekleyerek çalıştırabilirsiniz.

Databricks denetim düzlemi, Databricks çalışma alanı ortamını yönetir ve izler. Create cluster gibi herhangi bir yönetim işlemi denetim düzleminden başlatılır. Zamanlanmış işler gibi tüm meta veriler, hata toleransı için coğrafi çoğaltma ile birlikte bir Azure Veritabanında depolanır.

![Databricks mimarisi](media/howto-regional-disaster-recovery/databricks-architecture.png)

Bu mimarinin avantajlarından biri, kullanıcıların Azure Veri Tuğlalarını hesaplarındaki herhangi bir depolama kaynağına bağlayabiliyor olmasıdır. Önemli bir yararı, hem bilgi işlem (Azure Databricks) hem de depolama birbirinden bağımsız ölçeklendirilebilir olmasıdır.

## <a name="how-to-create-a-regional-disaster-recovery-topology"></a>Nasıl bölgesel bir felaket kurtarma topoloji oluşturmak için

Önceki mimari açıklamasında da belirttiğiniz gibi, Azure Veri Tuğlaları içeren büyük veri ardışık bir ardışık iş tablası için kullanılan bileşenler vardır: Azure Depolama, Azure Veritabanı ve diğer veri kaynakları. Azure Databricks, Büyük Veri ardışık alanının *bilgi işlem dir.* Bu, verileriniz Azure Depolama'da hala kullanılabilir durumdayken, ihtiyacınız olmadığında bilgi işlem için ödeme yapmak zorunda kalmamak için *bilgi işlem* (Azure Databricks kümesi) *sonlandırılabilir.* İş yerlerinin yüksek gecikme gecikmesi yaşamaması için *işlem* (Azure Databricks) ve depolama kaynaklarının aynı bölgede olması gerekir.  

Kendi bölgesel felaket kurtarma topolojinizi oluşturmak için aşağıdaki gereksinimleri izleyin:

   1. Ayrı Azure bölgelerinde birden çok Azure Veri Tuğlası çalışma alanı sağlama. Örneğin, Doğu US2'de birincil Azure Veri Tuğlaları çalışma alanını oluşturun. Batı ABD gibi ayrı bir bölgede ikincil olağanüstü durum kurtarma Azure Databricks çalışma alanını oluşturun.

   2. [Coğrafi yedekli depolama yı](../storage/common/storage-redundancy.md)kullanın. Azure Veri Tuğlaları ile ilişkili veriler varsayılan olarak Azure Depolama'da depolanır. Databricks işlerinden elde edilen sonuçlar, işlenen verilerin dayanıklı olması ve kümesonlandırıldıktan sonra yüksek oranda kullanılabilir kalması için Azure Blob Depolama'da da depolanır. Depolama ve Veri Tuğlaları kümesi birlikte bulunduğundan, birincil bölgeye artık erişilemiyorsa, verilere ikincil bölgede erişilebilmek için Coğrafi yedekli depolamayı kullanmanız gerekir.

   3. İkincil bölge oluşturulduktan sonra, kullanıcıları, kullanıcı klasörlerini, not defterlerini, küme yapılandırmasını, iş yapılandırmasını, kitaplıklarını, depolamayı, init komut dosyalarını geçirmeniz ve erişim denetimini yeniden yapılandırmanız gerekir. Ek ayrıntılar aşağıdaki bölümde özetlenmiştir.

## <a name="detailed-migration-steps"></a>Ayrıntılı geçiş adımları

1. **Bilgisayarınızdaki Databricks komut satırı arabirimini ayarlama**

   Bu makalede, Azure Databricks REST API üzerinde kolay kullanıcıya açık bir paketleyici olduğundan, otomatik adımların çoğu için komut satırı arabirimini kullanan bir dizi kod örneği gösterilmektedir.

   Herhangi bir geçiş adımını gerçekleştirmeden önce, veri tuğlalarını-cli'yi masaüstü bilgisayarınıza veya çalışmayı yapmayı planladığınız sanal bir makineye yükleyin. Daha fazla bilgi için bkz: [Veri Tuğlaları CLI Yükle](/azure/databricks/dev-tools/databricks-cli)

   ```bash
   pip install databricks-cli
   ```

   > [!NOTE]
   > Bu makalede sağlanan tüm python komut dosyalarının Python 2.7+ < 3.x ile çalışması beklenmektedir.

2. **İki profili yapılandırın.**

   Birini birincil çalışma alanı için, diğerini de ikincil çalışma alanı için yapılandırın:

   ```bash
   databricks configure --profile primary
   databricks configure --profile secondary
   ```

   Bu makalede kod blokları, ilgili çalışma alanı komutunu kullanarak sonraki her adımdaki profiller arasında geçiş sağlar. Oluşturduğunuz profillerin adlarının her kod bloğuna değiştirildiğinden emin olun.

   ```python
   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"
   ```

   Gerekirse komut satırında el ile geçiş yapabilirsiniz:

   ```bash
   databricks workspace ls --profile primary
   databricks workspace ls --profile secondary
   ```

3. **Azure Etkin Dizin kullanıcılarını geçirin**

   Aynı Azure Etkin Dizin kullanıcılarını birincil çalışma alanında bulunan ikincil çalışma alanına el ile ekleyin.

4. **Kullanıcı klasörlerini ve not defterlerini geçirme**

   İç içe klasör yapısını ve kullanıcı başına not defterini içeren kumhavuzu kullanıcı ortamlarını geçirmek için aşağıdaki python kodunu kullanın.

   > [!NOTE]
   > Temel API bunları desteklemediği için, bu adımda kitaplıklar kopyalanmaz.

   Aşağıdaki python komut dosyasını kopyalayıp bir dosyaya kaydedin ve Databricks komut satırınızda çalıştırın. Örneğin, `python scriptname.py`.

   ```python
   from subprocess import call, check_output

   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"

   # Get a list of all users
   user_list_out = check_output(["databricks", "workspace", "ls", "/Users", "--profile", EXPORT_PROFILE])
   user_list = user_list_out.splitlines()

   # Export sandboxed environment (folders, notebooks) for each user and import into new workspace.
   # Libraries are not included with these APIs / commands.

   for user in user_list:
     print "Trying to migrate workspace for user " + user

     call("mkdir -p " + user, shell=True)
     export_exit_status = call("databricks workspace export_dir /Users/" + user + " ./" + user + " --profile " + EXPORT_PROFILE, shell=True)

     if export_exit_status==0:
       print "Export Success"
       import_exit_status = call("databricks workspace import_dir ./" + user + " /Users/" + user + " --profile " + IMPORT_PROFILE, shell=True)
       if import_exit_status==0:
         print "Import Success"
       else:
         print "Import Failure"
     else:
       print "Export Failure"

   print "All done"
   ```

5. **Küme yapılandırmalarını geçirme**

   Not defterleri geçirildikten sonra, küme yapılandırmalarını isteğe bağlı olarak yeni çalışma alanına geçirebilirsiniz. Eğer herkes için yerine seçici küme config geçiş yapmak istemiyorsanız, databricks-cli kullanarak neredeyse tam otomatik bir adımdır.

   > [!NOTE]
   > Ne yazık ki küme config bitiş noktası oluşturma yoktur ve bu komut dosyası hemen her küme oluşturmak için çalışır. Aboneliğinizde yeterli çekirdek yoksa, küme oluşturma başarısız olabilir. Yapılandırma başarıyla aktarıldığı sürece hata yoksayılabilir.

   Aşağıdaki komut dosyası, daha sonra iş geçişi için kullanılabilecek (varolan kümeleri kullanmak üzere yapılandırılan işler için) eski küme iD'lerinden yeni küme aygıtlarına bir eşleme yazdırır.

   Aşağıdaki python komut dosyasını kopyalayıp bir dosyaya kaydedin ve Databricks komut satırınızda çalıştırın. Örneğin, `python scriptname.py`.

   ```python
   from subprocess import call, check_output
   import json, os

   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"

   # Get all clusters info from old workspace
   clusters_out = check_output(["databricks", "clusters", "list", "--profile", EXPORT_PROFILE])
   clusters_info_list = clusters_out.splitlines()

   # Create a list of all cluster ids
   clusters_list = []
   ##for cluster_info in clusters_info_list: clusters_list.append(cluster_info.split(None, 1)[0])

   for cluster_info in clusters_info_list: 
      if cluster_info != '':
         clusters_list.append(cluster_info.split(None, 1)[0])

   # Optionally filter cluster ids out manually, so as to create only required ones in new workspace

   # Create a list of mandatory / optional create request elements
   cluster_req_elems = ["num_workers","autoscale","cluster_name","spark_version","spark_conf","node_type_id","driver_node_type_id","custom_tags","cluster_log_conf","spark_env_vars","autotermination_minutes","enable_elastic_disk"]

   print(str(len(clusters_list)) + " clusters found in the primary site" )

   print ("---------------------------------------------------------")
   # Try creating all / selected clusters in new workspace with same config as in old one.
   cluster_old_new_mappings = {}
   i = 0
   for cluster in clusters_list:
      i += 1
      print("Checking cluster " + str(i) + "/" + str(len(clusters_list)) + " : " + cluster)
      cluster_get_out = check_output(["databricks", "clusters", "get", "--cluster-id", cluster, "--profile", EXPORT_PROFILE])
      print ("Got cluster config from old workspace")

       # Remove extra content from the config, as we need to build create request with allowed elements only
      cluster_req_json = json.loads(cluster_get_out)
      cluster_json_keys = cluster_req_json.keys()

      #Don't migrate Job clusters
      if cluster_req_json['cluster_source'] == u'JOB' : 
         print ("Skipping this cluster as it is a Job cluster : " + cluster_req_json['cluster_id'] )
         print ("---------------------------------------------------------")
         continue

      for key in cluster_json_keys:
         if key not in cluster_req_elems:
            cluster_req_json.pop(key, None)

      # Create the cluster, and store the mapping from old to new cluster ids

      #Create a temp file to store the current cluster info as JSON
      strCurrentClusterFile = "tmp_cluster_info.json" 

      #delete the temp file if exists
      if os.path.exists(strCurrentClusterFile) : 
         os.remove(strCurrentClusterFile)

      fClusterJSONtmp = open(strCurrentClusterFile,"w+")
      fClusterJSONtmp.write(json.dumps(cluster_req_json))
      fClusterJSONtmp.close()

      #cluster_create_out = check_output(["databricks", "clusters", "create", "--json", json.dumps(cluster_req_json), "--profile", IMPORT_PROFILE])
      cluster_create_out = check_output(["databricks", "clusters", "create", "--json-file", strCurrentClusterFile , "--profile", IMPORT_PROFILE])
      cluster_create_out_json = json.loads(cluster_create_out)
      cluster_old_new_mappings[cluster] = cluster_create_out_json['cluster_id']

      print ("Cluster create request sent to secondary site workspace successfully")
      print ("---------------------------------------------------------")

      #delete the temp file if exists
      if os.path.exists(strCurrentClusterFile) : 
         os.remove(strCurrentClusterFile)

   print ("Cluster mappings: " + json.dumps(cluster_old_new_mappings))
   print ("All done")
   print ("P.S. : Please note that all the new clusters in your secondary site are being started now!")
   print ("       If you won't use those new clusters at the moment, please don't forget terminating your new clusters to avoid charges")
   ```

6. **İş yapılandırmasını geçirme**

   Önceki adımda küme yapılandırmalarını geçirtildiyseniz, iş yapılandırmalarını yeni çalışma alanına geçirmeyi tercih edebilirsiniz. Tüm işler için yapmak yerine seçici iş config geçiş yapmak istemiyorsanız, databricks-cli kullanarak tam otomatik bir adımdır.

   > [!NOTE]
   > Zamanlanmış bir iş için yapılandırma da "zamanlama" bilgilerini içerir, bu nedenle varsayılan olarak geçirilir geçirilmaz yapılandırılmış zamanlamaya göre çalışmaya başlar. Bu nedenle, aşağıdaki kod bloğu geçiş sırasında (eski ve yeni çalışma alanlarında yinelenen çalıştırmaları önlemek için) zamanlama bilgilerini kaldırır. Kesmeye hazır olduğunuzda bu tür işlerin zamançizelgelerini yapılandırın.

   İş yapılandırması, yeni veya varolan bir küme için ayarlar gerektirir. Varolan küme kullanıyorsanız, aşağıdaki komut dosyası /kod eski küme kimliğini yeni küme kimliğiyle değiştirmeye çalışır.

   Aşağıdaki python komut dosyasını kopyalayın ve bir dosyaya kaydedin. Önceki adımda `old_cluster_id` `new_cluster_id`yapılan küme geçişinin değerini ve değerini değiştirin. Örneğin, databricks-cli komut satırınızda `python scriptname.py`çalıştırın.

   ```python
   from subprocess import call, check_output
   import json

   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"

   # Please replace the old to new cluster id mappings from cluster migration output
   cluster_old_new_mappings = {"old_cluster_id": "new_cluster_id"}

   # Get all jobs info from old workspace
   try:
     jobs_out = check_output(["databricks", "jobs", "list", "--profile", EXPORT_PROFILE])
     jobs_info_list = jobs_out.splitlines()
   except:
     print "No jobs to migrate"
     sys.exit(0)

   # Create a list of all job ids
   jobs_list = []
   for jobs_info in jobs_info_list:
     jobs_list.append(jobs_info.split(None, 1)[0])

   # Optionally filter job ids out manually, so as to create only required ones in new workspace

   # Create each job in the new workspace based on corresponding settings in the old workspace

   for job in jobs_list:
     print "Trying to migrate " + job

     job_get_out = check_output(["databricks", "jobs", "get", "--job-id", job, "--profile", EXPORT_PROFILE])
     print "Got job config from old workspace"

     job_req_json = json.loads(job_get_out)  
     job_req_settings_json = job_req_json['settings']

     # Remove schedule information so job doesn't start before proper cutover
     job_req_settings_json.pop('schedule', None)

     # Replace old cluster id with new cluster id, if job configured to run against an existing cluster
     if 'existing_cluster_id' in job_req_settings_json:
       if job_req_settings_json['existing_cluster_id'] in cluster_old_new_mappings:
         job_req_settings_json['existing_cluster_id'] = cluster_old_new_mappings[job_req_settings_json['existing_cluster_id']]
       else:
         print "Mapping not available for old cluster id " + job_req_settings_json['existing_cluster_id']
         continue

     call(["databricks", "jobs", "create", "--json", json.dumps(job_req_settings_json), "--profile", IMPORT_PROFILE])
     print "Sent job create request to new workspace successfully"

   print "All done"
   ```

7. **Kitaplıkları geçir**

   Şu anda kitaplıkları bir çalışma alanından diğerine geçirmenin kolay bir yolu yoktur. Bunun yerine, bu kitaplıkları el ile yeni çalışma alanına yeniden yükleyin. Çalışma alanına ve [CLI kitaplıklarına](https://github.com/databricks/databricks-cli#libraries-cli)özel kitaplıklar yüklemek için [DBFS CLI](https://github.com/databricks/databricks-cli#dbfs-cli-examples) kombinasyonunu kullanarak otomatikleştirmek mümkündür.

8. **Azure blob depolama ve Azure Veri Gölü Depolama montajlarını geçirin**

   Dizüstü bilgisayar tabanlı bir çözüm kullanarak tüm [Azure Blob depolama](/azure/databricks/data/data-sources/azure/azure-storage) ve [Azure Veri Gölü Depolama (Gen 2)](/azure/databricks/data/data-sources/azure/azure-datalake-gen2) montaj noktalarını el ile yeniden monte edin. Depolama kaynakları birincil çalışma alanına monte edilmiş olurdu ve bu ikincil çalışma alanında yinelenir. Bağlar için harici API yoktur.

9. **Geçiş küme init komut dosyaları**

   Herhangi bir küme başlatma komut dosyaları [DBFS CLI](https://github.com/databricks/databricks-cli#dbfs-cli-examples)kullanılarak eski den yeni çalışma alanına geçirilebilir. İlk olarak, gerekli komut `dbfs:/dat abricks/init/..` dosyalarını yerel masaüstünüze veya sanal makinenize kopyalayın. Ardından, bu komut dosyalarını aynı yolda yeni çalışma alanına kopyalayın.

   ```bash
   // Primary to local
   dbfs cp -r dbfs:/databricks/init ./old-ws-init-scripts --profile primary

   // Local to Secondary workspace
   dbfs cp -r old-ws-init-scripts dbfs:/databricks/init --profile secondary
   ```

10. **Erişim denetimini el ile yeniden yapılandırın ve yeniden uygulayın.**

    Varolan birincil çalışma alanınız Premium katmanı (SKU) kullanacak şekilde yapılandırıldıysa, büyük olasılıkla [Access Control özelliğini](/azure/databricks/administration-guide/access-control/index)de kullanıyor olabilirsiniz.

    Erişim Denetimi özelliğini kullanıyorsanız, erişim denetimini kaynaklara (Not Defterleri, Kümeler, İşler, Tablolar) el ile yeniden uygulayın.

## <a name="disaster-recovery-for-your-azure-ecosystem"></a>Azure ekosisteminiz için olağanüstü durum kurtarma

Diğer Azure hizmetlerini kullanıyorsanız, bu hizmetler için de olağanüstü durum kurtarma en iyi uygulamalarını uyguladığından emin olun. Örneğin, harici bir Hive metastore örneği kullanmayı seçerseniz, [MySQL](../mysql/concepts-business-continuity.md)için [Azure SQL Server](../sql-database/sql-database-disaster-recovery.md), Azure [HDInsight](../hdinsight/hdinsight-high-availability-linux.md)ve/veya Azure Veritabanı için olağanüstü durum kurtarmayı göz önünde bulundurmalısınız. Olağanüstü durum kurtarma hakkında genel bilgi için Azure [uygulamaları için Olağanüstü Durum kurtarma](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications)bilgisine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için Azure [Databricks belgelerine](index.yml)bakın.