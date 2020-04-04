---
title: Azure HDInsight sık sorulan sorular
description: HDInsight hakkında sık sorulan sorular
keywords: sık sorulan sorular, sSS
author: Ramakoni1
ms.author: ramakoni
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/20/2019
ms.openlocfilehash: 8a37e1b9bc4a0b953dc727dbab2813dd938ed576
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80652229"
---
# <a name="azure-hdinsight-frequently-asked-questions"></a>Azure HDInsight: Sık sorulan sorular

Bu makalede, [Azure HDInsight'ın](https://azure.microsoft.com/services/hdinsight/)nasıl çalıştırılacağa ilişkin en sık sorulan sorulardan bazılarına yanıt verilmektedir.

## <a name="creating-or-deleting-hdinsight-clusters"></a>HDInsight kümelerini oluşturma veya silme

### <a name="how-do-i-provision-an-hdinsight-cluster"></a>HDInsight kümesini nasıl sağlarım?

Kullanılabilir HDInsight kümelerinin türlerini ve sağlama yöntemlerini gözden geçirmek için [bkz.](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters)

### <a name="how-do-i-delete-an-existing-hdinsight-cluster"></a>Varolan bir HDInsight kümesini nasıl silerim?

Bir küme artık kullanılmadığında silme hakkında daha fazla bilgi edinmek için [bkz.](hdinsight-delete-cluster.md)

Oluşturma ve silme işlemleri arasında en az 30 ila 60 dakika bırakın. Aksi takdirde işlem aşağıdaki hata iletisi ile başarısız olabilir:

``Conflict (HTTP Status Code: 409) error when attempting to delete a cluster immediately after creation of a cluster. If you encounter this error, wait until the newly created cluster is in operational state before attempting to delete it.``

### <a name="how-do-i-select-the-correct-number-of-cores-or-nodes-for-my-workload"></a>İş yüküm için doğru çekirdek veya düğüm sayısını nasıl seçerim?

Uygun sayıda çekirdek ve diğer yapılandırma seçenekleri çeşitli etkenlere bağlıdır.

Daha fazla bilgi [için HDInsight kümeleri için Kapasite planlamasına](https://docs.microsoft.com/azure/hdinsight/hdinsight-capacity-planning)bakın.

### <a name="what-can-i-do-when-cluster-provisioning-fails-because-of-a-capacity-issue"></a>Kapasite sorunundan dolayı küme sağlama başarısız olursa ne yapabilirim?

Bu bölümde ortak kapasite sorunu hataları ve azaltma teknikleri sağlanmaktadır.

#### <a name="error-the-deployment-would-exceed-the-quota-of-800"></a>Hata: Dağıtım '800' kotasını aşacak

Azure'da kaynak grubu başına 800 dağıtımlık kota sınırı vardır. Kaynak grubu, abonelik, hesap veya diğer kapsamlar başına farklı kotalar uygulanır. Örneğin aboneliğiniz bir bölge için çekirdek sayısını sınırlandıracak şekilde yapılandırılmış olabilir. İzin verilen miktardan daha fazla çekirdek içeren bir sanal makine dağıtmaya çalışırsanız, kotanın aşıldığını belirten bir hata iletisi alırsınız.

Bu sorunu gidermek için, Azure portalı, CLI veya PowerShell'i kullanarak artık ihtiyaç duyulmayan dağıtımları silin.

Daha fazla bilgi için bkz. [Kaynak kotaları hatalarını düzeltme](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors).

#### <a name="error-the-maximum-node-exceeded-the-available-cores-in-this-region"></a>Hata: Maksimum düğüm bu bölgedeki kullanılabilir çekirdekleri aştı

Aboneliğiniz bir bölge için çekirdek sayısını sınırlandıracak şekilde yapılandırılmış olabilir. İzin verilen miktardan daha fazla çekirdek içeren bir kaynak dağıtmaya çalışırsanız, kotanın aşıldığını belirten bir hata iletisi alırsınız.

Kota artırma isteğinde bulunmak için şu adımları izleyin:

1. [Azure portalına](https://portal.azure.com)gidin ve **Yardım + destek'i**seçin.
   
1. **Yeni destek isteği**’ni seçin.
   
1. **Yeni destek isteği** sayfasının **Temeller** sekmesinde aşağıdaki bilgileri sağlayın:
   
   - **Sorun türü:** **Hizmet ve abonelik limitlerini (kotalar)** seçin.
   - **Abonelik:** Değiştirmek istediğiniz aboneliği seçin.
   - **Kota türü:** **HDInsight'ı**seçin.

Daha fazla bilgi için bkz. [Çekirdeği artırmak için destek bileti oluşturma](hdinsight-capacity-planning.md#quotas).

### <a name="what-are-the-various-types-of-nodes-in-an-hdinsight-cluster"></a>HDInsight kümesindeki çeşitli düğüm türleri nelerdir?

Azure HDInsight kümelerinde farklı türde sanal makineler veya düğümler vardır. Her düğüm türü sistemin çalışmasında bir rol oynar.

Daha fazla bilgi için [Azure HDInsight kümelerinde Kaynak türlerine](hdinsight-virtual-network-architecture.md#resource-types-in-azure-hdinsight-clusters)bakın.

## <a name="individual-components"></a>Tek Tek Bileşenler

### <a name="can-i-install-additional-components-on-my-cluster"></a>Kümeme ek bileşenler yükleyebilir miyim?

Evet. Ek bileşenler yüklemek veya küme yapılandırmasını özelleştirmek için şunları kullanın:

- Oluşturma sırasında veya oluşturulduktan sonra komut dosyaları. Komut dosyaları, Azure portalı, HDInsight Windows PowerShell cmdlets veya HDInsight .NET SDK'dan kullanabileceğiniz bir yapılandırma seçeneği olan [komut dosyası eylemi](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)aracılığıyla çağrılır. Bu yapılandırma seçeneği Azure portalı, HDInsight Windows PowerShell cmdlets veya HDInsight .NET SDK'dan kullanılabilir.

- [HDInsight Uygulama Platformu](https://azure.microsoft.com/services/hdinsight/partner-ecosystem/) ekosistem uygulamaları yüklemek için.

Desteklenen bileşenlerin listesi için [bkz. HDInsight ile kullanılabilen Apache Hadoop bileşenleri ve sürümleri nelerdir?](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions)

### <a name="can-i-upgrade-the-individual-components-that-are-pre-installed-on-the-cluster"></a>Kümeye önceden yüklenmiş tek tek bileşenleri yükseltebilir miyim?

Kümenizde önceden yüklenmiş yerleşik bileşenleri veya uygulamaları yükseltirseniz, ortaya çıkan yapılandırma Microsoft tarafından desteklenmez. Bu sistem yapılandırmaları Microsoft tarafından sınanmamamıştır. HDInsight kümesinin, bileşenin yükseltilmiş sürümünü önceden yüklemiş olabilecek farklı bir sürümünü kullanmaya çalışın.

Örneğin, Hive'ı tek bir bileşen olarak yükseltme desteklenmez. HDInsight yönetilen bir hizmettir ve birçok hizmet Ambari sunucusuyla entegre edilmiştir ve test edilmiştir. Bir Kovanı kendi başına yükseltmek, diğer bileşenlerin dizine eklenmiş ikililerinin değişmesine neden olur ve kümenizde bileşen tümleştirme sorunlarına neden olur.

### <a name="can-spark-and-kafka-run-on-the-same-hdinsight-cluster"></a>Kıvılcım ve Kafka aynı HDInsight kümesinde çalıştırılabilir mi?

Hayır, Apache Kafka ve Apache Spark'ı aynı HDInsight kümesinde çalıştırmak mümkün değil. Kaynak çekişmesi sorunlarını önlemek için Kafka ve Spark için ayrı kümeler oluşturun.

### <a name="how-do-i-change-timezone-in-ambari"></a>Ambari'de saat dilimini nasıl değiştiririm?

1. CLUSTERNAME'nin kümenizin `https://CLUSTERNAME.azurehdinsight.net`adı olduğu Ambari Web UI'sini açın.
2. Sağ üst köşede, yönetici seçin | Ayarlar. 

   ![Ambari Ayarları](media/hdinsight-faq/ambari-settings.png)

3. Kullanıcı Ayarları penceresinde, Saat Dilimi açılır penceresinden yeni saat dilimini seçin ve sonra Kaydet'i tıklatın.

   ![Ambari Kullanıcı Ayarları](media/hdinsight-faq/ambari-user-settings.png)

## <a name="metastore"></a>Meta Veri Deposu

### <a name="how-can-i-migrate-from-the-existing-metastore-to-azure-sql-server"></a>Varolan metastore'dan Azure SQL Server'a nasıl geçiş yapabilirim? 

SQL Server'dan Azure SQL Server'a geçiş yapmak için [bkz.](../dms/tutorial-sql-server-to-azure-sql.md)

### <a name="is-the-hive-metastore-deleted-when-the-cluster-is-deleted"></a>Küme silindiğinde Hive metastore silinir mi?

Kümenizin kullanmak üzere yapılandırıldığı metastore türüne bağlıdır.

Varsayılan metastore için: Varsayılan metastore küme yaşam döngüsünün bir parçasıdır. Bir kümeyi sildiğinizde, ilgili metastore ve meta veriler de silinir.

Özel bir metastore için: Metastore'un yaşam döngüsü kümenin yaşam döngüsüne bağlı değildir. Bu nedenle, meta verileri kaybetmeden kümeler oluşturabilir ve silebilirsiniz. Hive şemalarınız gibi meta veriler, HDInsight kümesini silip yeniden oluşturduktan sonra bile devam eder.

Daha fazla bilgi için bkz: [Azure HDInsight'ta harici meta veri depolarını kullan.](hdinsight-use-external-metadata-stores.md)

### <a name="does-migrating-a-hive-metastore-also-migrate-the-default-policies-of-the-ranger-database"></a>Bir Hive metastore'u geçirmek de Ranger veritabanının varsayılan ilkelerini geçişiyor mu?

Hayır, ilke tanımı Ranger veritabanında, bu yüzden Ranger veritabanını geçirmek ilkesini geçirecektir.

### <a name="can-you-migrate-a-hive-metastore-from-an-enterprise-security-package-esp-cluster-to-a-non-esp-cluster-and-vice-versa"></a>Bir Hive metastore'u Bir Kurumsal Güvenlik Paketi (ESP) kümesinden ESP olmayan bir kümeye geçirebilir misiniz?

Evet, bir Hive metastore'u ESP'den ESP olmayan bir kümeye geçirebilirsiniz.

### <a name="how-can-i-estimate-the-size-of-a-hive-metastore-database"></a>Hive metastore veritabanının boyutunu nasıl tahmin edebilirim?

Hive metadeposu, hive sunucusu tarafından kullanılan veri kaynakları için meta verileri depolamak için kullanılır. Boyut gereksinimleri kısmen Kovan veri kaynaklarınızın sayısına ve karmaşıklığına bağlıdır ve önceden tahmin edilemez. [Hive metastore yönergelerinde](hdinsight-use-external-metadata-stores.md#hive-metastore-guidelines)belirtildiği gibi, 50 DTU ve 250 GB depolama alanı sağlayan bir S2 katmanıyla başlayabilirsiniz ve bir darboğaz görürseniz veritabanını ölçeklendirebilirsiniz.

### <a name="do-you-support-any-other-database-other-than-azure-sql-database-as-an-external-metastore"></a>Harici bir metastore olarak Azure SQL Veritabanı dışında başka bir veritabanını destekliyor musunuz?

Hayır, Microsoft yalnızca Azure SQL Veritabanı'nı harici özel bir metastore olarak destekler.

### <a name="can-i-share-a-metastore-across-multiple-clusters"></a>Birden çok küme arasında bir metastore paylaşabilir miyim?

Evet, HDInsight'ın aynı sürümünü kullandıkları sürece özel metastore'u birden çok küme arasında paylaşabilirsiniz.

## <a name="connectivity-and-virtual-networks"></a>Bağlantı ve sanal ağlar  

### <a name="what-are-the-implications-of-blocking-ports-22-and-23-on-my-network"></a>Ağımdaki 22 ve 23 bağlantı noktalarını engellemenin etkileri nelerdir?

Bağlantı noktaları 22 ve port 23'u engellerseniz, kümeye SSH erişimi olmaz. Bu bağlantı noktaları HDInsight hizmeti tarafından kullanılmaz.

Daha fazla bilgi için, aşağıdaki belgelere bakın:

- [Ağ trafiğini denetleme](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment#networktraffic)

- [Özel bitiş noktası olan sanal ağdaki HDInsight kümelerine gelen trafiği güvenli hale](https://azure.microsoft.com/blog/secure-incoming-traffic-to-hdinsight-clusters-in-a-vnet-with-private-endpoint/)

- [HDInsight yönetimi IP adresleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses)

### <a name="can-i-deploy-an-additional-virtual-machine-within-the-same-subnet-as-an-hdinsight-cluster"></a>HDInsight kümesiyle aynı alt ağ içinde ek bir sanal makine dağıtabilir miyim?

Evet, hdinsight kümesi yle aynı alt ağ içinde ek bir sanal makine dağıtabilirsiniz. Aşağıdaki yapılandırmalar mümkündür:

- Kenar düğümleri: [HDInsight'ta Apache Hadoop kümelerinde boş kenar düğümlerini kullan'da](hdinsight-apps-use-edge-node.md)açıklandığı gibi kümeye başka bir kenar düğümü ekleyebilirsiniz.

- Bağımsız düğümler: Aynı alt ağa bağımsız bir sanal makine ekleyebilir ve özel bitiş noktasını `https://<CLUSTERNAME>-int.azurehdinsight.net`kullanarak bu sanal makineden kümeye erişebilirsiniz. Daha fazla bilgi için [bkz.](hdinsight-plan-virtual-network-deployment.md#networktraffic)

### <a name="should-i-store-data-on-the-local-disk-of-an-edge-node"></a>Verileri kenar düğümünün yerel diskinde depolamalı mıyım?

Hayır, verileri yerel bir diskte depolamak iyi bir fikir değil. Düğüm başarısız olursa, yerel olarak depolanan tüm veriler kaybolur. Verileri Azure Veri Gölü Depolama Gen2 veya Azure Blob depolama alanında depolamanızı veya verileri depolamak için bir Azure Files paylaşımı monte ederek saklamanızı öneririz.


### <a name="can-i-add-an-existing-hdinsight-cluster-to-another-virtual-network"></a>Varolan bir HDInsight kümesini başka bir sanal ağa ekleyebilir miyim?

Hayır, olmaz. Sanal ağ, sağlama sırasında belirtilmelidir. Sağlama sırasında sanal ağ belirtilmemişse, dağıtım dışarıdan erişilemeyen bir dahili ağ oluşturur. Daha fazla bilgi için [hdinsight'ı varolan bir sanal ağa ekle'ye](hdinsight-plan-virtual-network-deployment.md#existingvnet)bakın.

## <a name="security-and-certificates"></a>Güvenlik ve Sertifikalar

### <a name="what-are-the-recommendations-for-malware-protection-on-azure-hdinsight-clusters"></a>Azure HDInsight kümelerinde kötü amaçlı yazılımdan korunma önerileri nelerdir?

Kötü amaçlı yazılımların korunması hakkında daha fazla bilgi için [Azure Bulut Hizmetleri ve Sanal Makineler için Microsoft Kötü Amaçlı Yazılımdan Koruma'ya](../security/fundamentals/antimalware.md)bakın.

### <a name="how-do-i-create-a-keytab-for-an-hdinsight-esp-cluster"></a>HDInsight ESP kümesi için nasıl bir anahtar sekmesi oluşturabilirim?

Etki alanı kullanıcı adınız için bir Kerberos anahtar sekmesi oluşturun. Daha sonra bu anahtar sekmesini, parola girmeden uzak etki alanı birleştirilmiş kümelere kimlik doğrulamak için kullanabilirsiniz. Etki alanı adı büyük harfle:

```shell
ktutil
ktutil: addent -password -p <username>@<DOMAIN.COM> -k 1 -e RC4-HMAC
Password for <username>@<DOMAIN.COM>: <password>
ktutil: wkt <username>.keytab
ktutil: q
```

### <a name="can-i-use-an-existing-azure-active-directory-tenant-to-create-an-hdinsight-cluster-that-has-the-esp"></a>ESP'ye sahip bir HDInsight kümesi oluşturmak için varolan bir Azure Active Directory kiracısını kullanabilir miyim?

ESP ile bir HDInsight kümesi oluşturabilmeniz için Azure Active Directory Domain Services'ı (Azure AD DS) etkinleştirmeniz gerekir. Açık kaynak Hadoop Kimlik Doğrulama için Kerberos dayanır (OAuth aksine).

Bir etki alanına VM'lere katılmak için bir etki alanı denetleyiciniz olması gerekir. Azure AD DS yönetilen etki alanı denetleyicisidir ve güvenli bir Hadoop kümesi oluşturmak için tüm Kerberos gereksinimlerini yönetilen bir şekilde sağlayan Azure Active Directory'nin bir uzantısı olarak kabul edilir. Yönetilen bir hizmet olarak HDInsight, uçlardan uca güvenlik sağlamak için Azure AD DS ile tümleşir.

### <a name="can-i-use-a-self-signed-certificate-in-an-aad-ds-secure-ldap-setup-and-provision-an-esp-cluster"></a>AAD-DS güvenli bir LDAP kurulumunda kendi imzalı bir sertifika kullanabilir miyim ve bir ESP kümesi sağlayabilir miyim?

Bir sertifika yetkilisi tarafından verilen bir sertifikanın kullanılması önerilir, ancak kendi imzalı sertifikanın kullanılması ESP'de de desteklenir. Daha fazla bilgi için bkz.

- [Azure Active Directory Domain Services’ı etkinleştirme](domain-joined/apache-domain-joined-configure-using-azure-adds.md#enable-azure-ad-ds)

- [Öğretici: Azure Etkin Dizin Etki Alanı Hizmetleri yönetilen etki alanı için güvenli LDAP'yi yapılandırma](../active-directory-domain-services/tutorial-configure-ldaps.md)

### <a name="how-can-i-pull-login-activity-shown-in-ranger"></a>Ranger'da gösterilen giriş etkinliğini nasıl çekebilirim?

Denetim gereksinimleri için Microsoft, [HDInsight kümelerini izlemek için Azure Monitor günlüklerini Kullan Azure Monitor günlüklerinde](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-oms-log-analytics-tutorial)açıklandığı şekilde etkinleştirmenizi önerir.

### <a name="can-i-disable-clamscan-on-my-cluster"></a>Kümemdeki Clamscan'ı devre dışı kılabilir miyim?

Clamscan, HDInsight kümesinde çalışan ve kümelerinizi virüs saldırılarına karşı korumak için Azure güvenliği (azsecd) tarafından kullanılan virüsten koruma yazılımıdır. Microsoft, kullanıcıların varsayılan Clamscan yapılandırmasıiçin herhangi bir değişiklik yapmaktan kaçınmalarını şiddetle önerir.

Bu işlem, diğer işlemlere müdahale etmez veya herhangi bir döngüyü kaldırmaz. Her zaman diğer süreçlere boyun eğecektir. Clamscan CPU ani sadece sistem boşta olduğunda görülmelidir.  

Zamanlamayı denetlemeniz gereken senaryolarda aşağıdaki adımları kullanabilirsiniz:

1. Aşağıdaki komutu kullanarak otomatik yürütmeyi devre dışı kılabilir:
   
   `/usr/local/vbin/azsecd config -s clamav -d Disabled`
   
1. Kök olarak aşağıdaki komutu çalıştıran bir Cron iş ekleyin:
   
   `/usr/local/bin/azsecd manual -s clamav`

Bir cron işini nasıl ayarlayıp çalıştıracağım hakkında daha fazla bilgi için [cron işini nasıl ayarlıyorum?](https://askubuntu.com/questions/2368/how-do-i-set-up-a-cron-job)

### <a name="why-is-llap-available-on-spark-esp-clusters"></a>LLAP neden Spark ESP kümelerinde kullanılabilir?
ESP Spark kümelerinde LLAP, performans değil, güvenlik nedenleriyle (örn. Apache Ranger) etkinleştirilir. LLAP'nin kaynak kullanımı için daha büyük düğüm VM'leri kullanmalısınız (örn. minimum D13V2). 

### <a name="how-can-i-add-additional-aad-groups-after-creating-an-esp-cluster"></a>ESP kümesi oluşturduktan sonra nasıl ek AAD grupları ekleyebilirim?
Bunu başarmanın iki yolu vardır: 1- Kümeoluşturma sırasında kümeyi yeniden oluşturabilir ve ek grup ekleyebilirsiniz. AAD-DS'de kapsamlı eşitleme kullanıyorsanız, lütfen B grubunun kapsamlı eşitlemeye dahil olduğundan emin olun.
2- EsP kümesini oluşturmak için kullanılan önceki grubun iç içe gelişmiş bir alt grubu olarak grubu ekleyin. Örneğin, grubu `A`olan bir ESP kümesi oluşturduysanız, daha `B` sonra grup iç içe `A` bir alt grup olarak ekleyebilirsiniz ve yaklaşık bir saat sonra eşitlenir ve kümede otomatik olarak kullanılabilir. 

## <a name="storage"></a>Depolama

### <a name="can-i-add-an-azure-data-lake-storage-gen2-to-an-existing-hdinsight-cluster-as-an-additional-storage-account"></a>Ek depolama hesabı olarak varolan bir HDInsight kümesine Azure Veri Gölü Depolama Gen2 ekleyebilir miyim?

Hayır, şu anda birincil depolama alanı olarak blob depolama alanı olan bir kümeye Bir Azure Veri Gölü Depolama Gen2 depolama hesabı eklemek mümkün değildir. Daha fazla bilgi için [bkz.](hdinsight-hadoop-compare-storage-options.md)

### <a name="how-can-i-find-the-currently-linked-service-principal-for-a-data-lake-storage-account"></a>Veri Gölü depolama hesabı için şu anda bağlı olan Hizmet Sorumlusu'nu nasıl bulabilirim?

Ayarlarınızı Azure portalında küme özellikleriniz altında **Veri Gölü Depolama Gen1 erişiminde** bulabilirsiniz. Daha fazla bilgi için [bkz.](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md#verify-cluster-set-up)
 
### <a name="how-can-i-calculate-the-usage-of-storage-accounts-and-blob-containers-for-my-hdinsight-clusters"></a>HDInsight kümelerim için depolama hesaplarının ve blob kaplarının kullanımını nasıl hesaplayabilirim?

Aşağıdakilerden birini yapın:

- [PowerShell kullanma](../storage/scripts/storage-blobs-container-calculate-size-powershell.md)

- */kullanıcı/kovan/ boyutunu bulun. *Aşağıdaki komut satırını kullanarak HDInsight kümesindeki çöp kutusu/ klasörü:
  
  `hdfs dfs -du -h /user/hive/.Trash/`

### <a name="how-can-i-set-up-auditing-for-my-blob-storage-account"></a>Blob depolama hesabım için denetimi nasıl ayarlayabilirim?

Blob depolama hesaplarını denetlemek için, [Azure portalındaki bir depolama hesabını izleyin'de](../storage/common/storage-monitor-storage-account.md)yordamı kullanarak izlemeyi yapılandırın. BIR HDFS denetim günlüğü yalnızca yerel HDFS dosya sistemi için denetim bilgileri sağlar (hdfs://mycluster).  Uzak depolama da yapılan işlemleri içermez.

### <a name="how-can-i-transfer-files-between-a-blob-container-and-an-hdinsight-head-node"></a>Dosyaları bir blob kapsayıcısı ile HDInsight kafa düğümü arasında nasıl aktarabilirim?

Kafa düğümünüzde aşağıdaki kabuk komut dosyasına benzer bir komut dosyası çalıştırın:

```shell
for i in cat filenames.txt
do
   hadoop fs -get $i <local destination>
done
```
 
> [!NOTE]
> Dosya *adları.txt* blob kapsayıcılarında dosyaların mutlak yolu olacaktır.
 
### <a name="are-there-any-ranger-plugins-for-storage"></a>Depolama için herhangi bir Ranger eklentileri var mı?

Şu anda, blob depolama ve Azure Veri Gölü Depolama Gen1 veya Gen2 için ranger eklentisi yok. ESP kümeleri için Azure Veri Gölü Depolama'yı kullanmanız gerekir, çünkü en azından HDFS araçlarını kullanarak dosya sistemi düzeyinde el ile ince taneli izinler ayarlayabilirsiniz. Ayrıca, Azure Veri Gölü Depolama'yı kullanırken, ESP kümeleri küme düzeyinde Azure Active Directory'yi kullanarak dosya sistemi erişim denetiminin bir kısmını yapar. 

Azure Depolama Gezgini'ni kullanarak kullanıcılarınızın güvenlik gruplarına veri erişim ilkeleri atayabilirsiniz. Daha fazla bilgi için bkz.

- [Azure AD kullanıcılarının Hive veya diğer hizmetleri kullanarak Veri Gölü Depolama Gen2'deki verileri sorgulamaizinlerini nasıl belirlerim?](hdinsight-hadoop-use-data-lake-storage-gen2.md#how-do-i-set-permissions-for-azure-ad-users-to-query-data-in-data-lake-storage-gen2-by-using-hive-or-other-services)
- [Azure Veri Gölü Depolama Gen2 ile Azure Depolama Gezgini'ni kullanarak dosya ve dizin düzeyi izinlerini ayarlama](/azure/storage/blobs/data-lake-storage-how-to-set-permissions-storage-explorer)

### <a name="can-i-increase-hdfs-storage-on-a-cluster-without-increasing-the-disk-size-of-worker-nodes"></a>İşçi düğümlerinin disk boyutunu artırmadan bir kümedeki HDFS depolamasını artırabilir miyim?

Hayır, herhangi bir alt düğümdisk boyutunu artıramaz, bu nedenle disk boyutunu artırmak için tek yolu küme bırakın ve daha büyük işçi VM ile yeniden. HDInsight verilerinizden herhangi birini depolamak için HDFS'yi kullanmayın, çünkü kümenizi silerseniz veriler silinir. Bunun yerine, verilerinizi Azure'da depolayın. Kümeyi ölçeklemek, HDInsight kümenize ek kapasite de ekleyebilir.

## <a name="edge-nodes"></a>Uç düğümler

### <a name="can-i-add-an-edge-node-after-the-cluster-has-been-created"></a>Küme oluşturulduktan sonra bir kenar düğümü ekleyebilir miyim?

HDInsight kümesi veya küme oluştururken yeni bir küme. Daha fazla bilgi için bkz. [HDInsight'taki boş kenar düğümlerini Apache Hadoop kümelerinde kullanma](hdinsight-apps-use-edge-node.md).

### <a name="how-can-i-connect-to-an-edge-node"></a>Kenar düğümüne nasıl bağlanabilirim?

Bir kenar düğümü oluşturduktan sonra, bağlantı noktası 22'de SSH kullanarak ona bağlanabilirsiniz. Küme portalından kenar düğümünün adını bulabilirsiniz. Adlar genellikle *-ed*ile biter.

### <a name="why-are-persisted-scripts-not-running-automatically-on-newly-created-edge-nodes"></a>Kalıcı komut dosyaları neden yeni oluşturulan kenar düğümlerinde otomatik olarak çalışmıyor?

Ölçekleme işlemleri yoluyla kümeye eklenen yeni alt düğümleri özelleştirmek için kalıcı komut dosyaları kullanırsınız. Kalıcı komut dosyaları kenar düğümleri için geçerli değildir.

## <a name="rest-api"></a>REST API

### <a name="what-are-the-rest-api-calls-to-pull-a-tez-query-view-from-the-cluster"></a>Kümeden Tez sorgu görünümü çekmek için REST API çağrıları nelerdir?

JSON formatında gerekli bilgileri çekmek için aşağıdaki REST uç noktalarını kullanabilirsiniz. İstekleri yapmak için temel kimlik doğrulama üstbilgilerini kullanın.

- Tez Sorgu Görünümü: *https:\//\<küme adı>.azurehdinsight.net/ws/v1/timeline/HIVE_QUERY_ID/*
- Tez Dağ Görünümü: *https:\//\<küme adı>.azurehdinsight.net/ws/v1/timeline/TEZ_DAG_ID/*

### <a name="how-do-i-retrieve-the-configuration-details-from-hdi-cluster-by-using-an-azure-active-directory-user"></a>Azure Active Directory kullanıcısını kullanarak HDI kümesinden yapılandırma ayrıntılarını nasıl alabiliyorum?

AAD kullanıcınızla uygun kimlik doğrulama belirteçleri üzerinde anlaşmak için aşağıdaki biçimi kullanarak ağ geçidinden geçin:

* https://`<cluster dnsname>`.azurehdinsight.net/api/v1/clusters/testclusterdem/stack_versions/1/repository_versions/1 

### <a name="how-do-i-use-ambari-restful-api-to-monitor-yarn-performance"></a>Ambari Restful API'yi İplik performansını izlemek için nasıl kullanırım?

Aynı sanal ağda veya eşlenmiş bir sanal ağda Curl komutunu ararsanız, komut şudur:

```curl
curl -u <cluster login username> -sS -G
http://<headnodehost>:8080/api/v1/clusters/<ClusterName>/services/YARN/components/NODEMANAGER?fields=metrics/cpu
```
 
Komutu sanal ağ dışından veya eşlemeyen bir sanal ağdan çağırırsanız, komut biçimi şudur:

- ESP olmayan bir küme için:
  
  ```curl
  curl -u <cluster login username> -sS -G 
  https://<ClusterName>.azurehdinsight.net/api/v1/clusters/<ClusterName>/services/YARN/components/NODEMANAGER?fields=metrics/cpu
  ```

- Bir ESP kümesi için:
  
  ```curl
  curl -u <cluster login username>-sS -G 
  https://<ClusterName>.azurehdinsight.net/api/v1/clusters/<ClusterName>/services/YARN/components/NODEMANAGER?fields=metrics/cpu
  ```

> [!NOTE]
> Curl bir şifre ister. Küme giriş kullanıcı adı için geçerli bir parola girmeniz gerekir.

## <a name="billing"></a>Faturalandırma

### <a name="how-much-does-it-cost-to-deploy-an-hdinsight-cluster"></a>BIR HDInsight kümesini dağıtmak ne kadara mal olur?

Faturalandırmayla ilgili fiyatlandırma ve SSS hakkında daha fazla bilgi için [Azure HDInsight Fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/) sayfasına bakın.

### <a name="when-does-hdinsight-billing-start--stop"></a>HDInsight faturalandırması ne zaman & durur?

HDInsight kümesi faturalandırması küme oluşturulduğunda başlar ve küme silindiğinde sona erer. Faturalandırma dakikada eşit olarak derecelendirilir.

### <a name="how-do-i-cancel-my-subscription"></a>Aboneliğimi nasıl iptal edebilirim?

Aboneliğinizi nasıl iptal edesevdiğiniz hakkında bilgi için azure [aboneliğinizi iptal etme](https://docs.microsoft.com/azure/billing/billing-how-to-cancel-azure-subscription)'ye bakın.

### <a name="for-pay-as-you-go-subscriptions-what-happens-after-i-cancel-my-subscription"></a>You-you-go abonelikleri için aboneliğimi iptal ettikten sonra ne olur?

Aboneliğiniz iptal edildikten sonra aboneliğiniz hakkında bilgi için [bkz.](/azure/billing/billing-how-to-cancel-azure-subscription)

## <a name="hive"></a>Hive

### <a name="why-does-the-hive-version-appear-as-121000-instead-of-21-in-the-ambari-ui-even-though-i-am-running-an-hdinsight-36-cluster"></a>HDInsight 3.6 kümesini çalıştırıyor olmama rağmen Ambari UI'de Neden Ambari UI'de 2.1 yerine 1.2.1000 olarak görünür?

Ambari UI'de yalnızca 1,2 görünse de, HDInsight 3.6 hem Hive 1.2 hem de Hive 2.1 içerir.

## <a name="other-faq"></a>Diğer SSS

### <a name="what-does-hdinsight-offer-in-terms-of-real-time-stream-processing-capabilities"></a>HDInsight, gerçek zamanlı akış işleme özellikleri açısından neler sunuyor?

Azure HDInsight'ta akış işlemenin tümleştirme özellikleri hakkında bilgi [için](/azure/architecture/data-guide/technology-choices/stream-processing)bkz.

### <a name="is-there-a-way-to-dynamically-terminate-the-head-node-of-the-cluster-when-the-cluster-is-idle-for-a-specific-period"></a>Küme belirli bir süre boştayken kümenin baş düğümünün dinamik olarak sonlandırılmasının bir yolu var mıdır?

BUNU HDInsight kümeleriyle yapamazsınız. Bu senaryolar için Azure Veri Fabrikası'nı kullanabilirsiniz.

### <a name="what-compliance-offerings-does-hdinsight-offer"></a>HDInsight hangi uyumluluk teklifleri sunuyor?

Uyumluluk bilgileri için [Microsoft Güven Merkezi'ne](https://www.microsoft.com/trust-center) ve [Microsoft Azure uyumluluğuna Genel Bakış'a](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942)bakın.
