---
title: Azure HDInsight-sık sorulan sorular
description: Azure HDInsight 'a genel bakış sağlar
author: Ramakoni1
ms.author: ramakoni
ms.reviewer: v-miegge
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 4accd8c2f58dd89fb5f918ca6e58e71caeaed57f
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71212756"
---
# <a name="azure-hdinsight-frequently-asked-questions"></a>Azure HDInsight: Sık sorulan sorular

Bu makalede, [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/)'ın nasıl çalıştırılacağı hakkında en yaygın soruların yanıtları sağlanmaktadır.

## <a name="creating-or-deleting-hdinsight-clusters"></a>HDInsight kümeleri oluşturma veya silme

### <a name="how-do-i-provision-an-hdinsight-cluster"></a>HDInsight kümesi sağlamak Nasıl yaparım? misiniz?

Kullanılabilir HDInsight kümelerinin türlerini ve sağlama yöntemlerini gözden geçirmek için bkz. [HDInsight 'ta Apache Hadoop, Apache Spark, Apache Kafka ve daha fazlasını içeren kümeleri ayarlama](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters).

### <a name="how-do-i-delete-an-existing-hdinsight-cluster"></a>Nasıl yaparım? mevcut bir HDInsight kümesi silinsin mi?

Bir kümeyi artık kullanımda olmadığında silme hakkında daha fazla bilgi edinmek için bkz. [tarayıcınızı, PowerShell 'i veya Azure CLI 'yı kullanarak HDInsight kümesini silme](https://docs.microsoft.com/azure/hdinsight/hdinsight-delete-cluster). Tek bir kümede oluşturma ve silme işlemi arasında en az 30 ila 60 dakika olması önerilir. Aksi takdirde, işlem başarısız olabilir ve aşağıdaki hata iletisini döndürebilir:

``Conflict (HTTP Status Code: 409) error when attempting to delete a cluster immediately after creation of a cluster. If you encounter this error, wait until the newly created cluster is in operational state before attempting to delete it.``

### <a name="how-do-i-select-the-correct-number-of-cores-or-nodes-for-my-workload"></a>Nasıl yaparım? iş yüküyle ilgili doğru sayıda çekirdek veya düğüm seçin mi?

Uygun çekirdek sayısı ve diğer yapılandırma seçenekleri çeşitli etkenlere bağlıdır.

Daha fazla bilgi için bkz. [HDInsight kümeleri Için kapasite planlaması](https://docs.microsoft.com/azure/hdinsight/hdinsight-capacity-planning).

### <a name="what-can-i-do-when-cluster-provisioning-fails-because-of-a-capacity-issue"></a>Bir kapasite sorunu nedeniyle küme sağlama başarısız olduğunda ne yapabilirim?

Genel kapasite sorunu hataları ve risk azaltma teknikleri Bu bölümde verilmiştir.

#### <a name="error-the-deployment-would-exceed-the-quota-of-800"></a>Hata: Dağıtım ' 800 ' kotasını aşacak

Azure'da kaynak grubu başına 800 dağıtımlık kota sınırı vardır. Her kaynak grubu, abonelik, hesap veya başka kapsamlar için farklı kotalar uygulanır. Örneğin aboneliğiniz bir bölge için çekirdek sayısını sınırlandıracak şekilde yapılandırılmış olabilir. İzin verilen miktardan daha fazla çekirdeğe sahip bir sanal makineyi dağıtmaya çalışırsanız, kotanın aşıldığını belirten bir hata iletisi alırsınız.

Bu sorunu çözmek için Azure portal, CLı veya PowerShell kullanarak artık gerekmeyen dağıtımları silin.

Daha fazla bilgi için bkz. [Kaynak kotaları hatalarını düzeltme](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors).

#### <a name="error-the-maximum-node-exceeded-the-available-cores-in-this-region"></a>Hata: En fazla düğüm, bu bölgedeki kullanılabilir çekirdekleri aştı

Aboneliğiniz bir bölge için çekirdek sayısını sınırlandıracak şekilde yapılandırılmış olabilir. İzin verilen miktardan daha fazla çekirdeğe sahip bir kaynak dağıtmaya çalışırsanız, kotanın aşıldığını belirten bir hata iletisi alırsınız.

Kota artırma isteğinde bulunmak için şu adımları izleyin:

1. Azure portal gidin ve yardım + destek ' i seçin.
2. Yeni destek isteği ' ni seçin.
3. Yeni destek isteği sayfasında, temel bilgiler sekmesinde aşağıdaki seçenekleri belirleyin:

   * Sorun türü: Hizmet ve abonelik limitleri (kotalar)
   * Abonelik: Değiştirmek istediğiniz abonelik
   * Kota türü: HDInsight

Daha fazla bilgi için bkz. [çekirdekleri artırmak için destek bileti oluşturma](https://docs.microsoft.com/azure/hdinsight/hdinsight-capacity-planning#quotas).

### <a name="what-are-the-various-types-of-nodes-in-an-hdinsight-cluster"></a>HDInsight kümesinde çeşitli düğüm türleri nelerdir?

Azure HDInsight kümelerinde farklı türlerde sanal makineler veya düğümler vardır. Her düğüm türü, sistem işleminde bir rol oynar.

Daha fazla bilgi için bkz. [Azure HDInsight kümelerinde kaynak türleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-virtual-network-architecture#resource-types-in-azure-hdinsight-clusters).

## <a name="individual-components"></a>Bireysel bileşenler

### <a name="can-i-install-additional-components-on-my-cluster"></a>Kümeme ek bileşenler yükleyebilir miyim?

Evet, aşağıdaki yöntemlerden herhangi birini kullanarak ek bileşenler yükleyebilir veya küme yapılandırmasını özelleştirebilirsiniz:

* Oluşturma sırasında veya sonrasında betikleri kullanma. Bu komut dosyaları bir [betik eylemi](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)aracılığıyla çağrılır. Bu yapılandırma seçeneği Azure portal, HDInsight Windows PowerShell cmdlet 'leri veya HDInsight .NET SDK 'dan kullanılabilir. 
* Küme sağlandıktan sonra sudo veya diğer yöntemleri kullanma.
* Ekosistem uygulamalarını yüklemek için [HDInsight uygulama platformunu](https://azure.microsoft.com/services/hdinsight/partner-ecosystem/) kullanma.

Ancak, Microsoft Desteği ekipler yalnızca aşağıdaki durumlar için destek sunabilir:

* Betiği yüklerken oluşan sorunlar veya hatalar. Özel betiklerin yürütülmesi sırasında oluşan hatalar bir destek bileti kapsamı dışındadır. 
* Küme oluşturma işleminin parçası olan ek uygulamalar. 

Desteklenen bileşenlerin listesi için bkz. [HDInsight ile kullanılabilen Apache Hadoop bileşenleri ve sürümleri nelerdir?](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions)

Tek tek bileşenlere yönelik destek küme türüne göre de değişiklik gösterebilir. Örneğin, Spark bir Kafka kümesinde desteklenmez ve tam tersi de geçerlidir.

Küme oluşturma işleminin dışındaki uygulamalar veya hizmetler için, destek için ilgili satıcıya veya hizmet sağlayıcısına başvurun. Bu eylemler için, topluluk sitelerini de kullanabilirsiniz. Birçok topluluk sitesi mevcuttur. HDInsight ve [Stack Overflow](https://stackoverflow.com/questions/tagged/hdinsight) [için MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=hdinsight) örnekleri içerir. Apache projelerinde Ayrıca, [Apache Web sitesinde](https://apache.org/)proje siteleri de vardır. [Hadoop](https://hadoop.apache.org/)bir örnektir.

 Azure desteğiyle ilgili daha fazla soru için [Azure DESTEĞI SSS](https://azure.microsoft.com/en-us/support/faq/)' yi gözden geçirin.

### <a name="can-i-upgrade-the-individual-components-that-are-pre-installed-on-the-cluster"></a>Kümede önceden yüklenmiş olan bileşenleri yükseltebilir miyim?

Kümenizde önceden yüklenmiş olan yerleşik bileşenleri veya uygulamaları yükseltirseniz, sonuçta elde edilen yapılandırma Microsoft tarafından desteklenmeyecektir. Bu sistem konfigürasyonları Microsoft tarafından sınanmamıştır. HDInsight kümesinin, önceden yüklenmiş bileşenin yükseltilmiş sürümüne sahip olabilecek farklı bir sürümünü kullanmayı deneyin.

Örneğin, tek bir bileşen olarak Hive 'yi yükseltme desteklenmez. HDInsight Yönetilen bir hizmettir ve birçok hizmet, ambarı sunucusuyla tümleşiktir ve test edilmiştir. Bir Hive 'yi kendi üzerinde yükseltmek, diğer bileşenlerin dizinli ikili dosyalarının değişmesine neden olur ve kümenizdeki bileşen tümleştirme sorunlarına neden olur.

### <a name="can-spark-and-kafka-run-on-the-same-hdinsight-cluster"></a>Spark ve Kafka aynı HDInsight kümesinde çalıştırılabilir mi?

Hayır, aynı HDInsight kümesinde Apache Kafka ve Apache Spark çalıştırmak mümkün değildir. Kullanıcılar, kaynak çakışması sorunlarından kaçınmak için her biri için ayrı kümeler oluşturmalı.

### <a name="how-do-i-change-timezone-in-ambari"></a>Nasıl yaparım?, ambarı 'nda saat dilimini değiştirmek mi istiyorsunuz?

1. Konumundaki https://CLUSTERNAME.azurehdinsight.net ambarı Web Kullanıcı arabirimini açın, burada clustername kümenizin adıdır.
2. Sağ üst köşede Yönetici ' yi seçin | Ayarlar. 

   ![Ambarı ayarları](media/hdinsight-faq/ambari-settings.png)

3. Kullanıcı ayarları penceresinde, saat dilimi açılan penceresindeki yeni saat dilimini seçin ve ardından Kaydet ' e tıklayın.

   ![Ambarı Kullanıcı ayarları](media/hdinsight-faq/ambari-user-settings.png)

## <a name="metastore"></a>Meta veri deposu

### <a name="how-can-i-migrate-from-the-existing-metastore-to-azure-sql-server"></a>Mevcut meta veri deposunu Azure SQL Server nasıl geçirebilirim? 

SQL Server 'den Azure SQL Server 'e geçiş yapmak için bkz [. Öğretici: DMS](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql)kullanarak Azure SQL veritabanı 'nda bulunan tek bir veritabanına veya havuza alınmış bir veritabanına SQL Server geçirin.

### <a name="is-the-hive-metastore-deleted-when-the-cluster-is-deleted"></a>Hive meta veri deposu, küme silindiğinde silinir mi?

Bu, kümenizin kullanılmak üzere yapılandırıldığı meta veri deposu türüne bağlıdır.

Varsayılan bir meta veri deposu için: Varsayılan meta veri deposu, küme yaşam döngüsünün bir parçasıdır. Bir kümeyi sildiğinizde, karşılık gelen meta veri deposu ve meta veriler de silinir.

Özel bir meta veri deposu için: Meta veri deposu yaşam döngüsü bir kümenin yaşam döngüsüne bağlı değildir. Bu nedenle, meta verileri kaybetmeden kümeler oluşturabilir ve silebilirsiniz. Hive şemalarınız gibi meta veriler, HDInsight kümesini silip yeniden oluşturduktan sonra bile devam ettirir.

Daha fazla bilgi için bkz. [Azure HDInsight 'ta dış meta veri depoları kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-external-metadata-stores).

### <a name="does-migrating-a-hive-metastore-also-migrate-the-default-policies-of-the-ranger-database"></a>Hive meta veri deposu geçişi, Ranger veritabanının varsayılan ilkelerini de geçirsin mi?

Hayır, ilke tanımı Ranger veritabanında yer alır. Bu nedenle, Ranger veritabanının geçirilmesi ilkeyi geçirmez.

### <a name="can-you-migrate-a-hive-metastore-from-an-enterprise-security-package-esp-cluster-to-a-non-esp-cluster-and-vice-versa"></a>Bir Hive meta veri deposu Kurumsal Güvenlik Paketi (ESP) kümesinden ESP olmayan bir kümeye geçirebilir ve bunun tersini yapabilir miyim?

Evet, bir Hive meta veri deposu ESP 'den, ESP olmayan bir kümeye geçirebilirsiniz.

### <a name="how-can-i-estimate-the-size-of-a-hive-metastore-database"></a>Hive meta veri deposu veritabanının boyutunu nasıl tahmin edebilirim?

Hive sunucusu tarafından kullanılan veri kaynaklarının meta verilerini depolamak için bir Hive meta veri deposu kullanılır. Bu nedenle, boyut gereksinimleri Hive için kullanmanız gereken veri kaynağı sayısından ve veri kaynaklarının ne kadar karmaşık olduğunu etkiler. Bu nedenle, boyut tahmin edilemez. [Hive meta veri deposu en iyi uygulamalarında](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-external-metadata-stores#hive-metastore-best-practices)açıklandığı gibi, S2 katmanından ıtıbaren 50 DTU ve 250 GB depolama alanı sağlar. Bir performans sorunu yaşarsanız, veritabanının ölçeğini değiştirebilirsiniz.

### <a name="do-you-support-any-other-database-other-than-azure-sql-database-as-an-external-metastore"></a>Dış meta veri deposu olarak Azure SQL veritabanı dışındaki diğer veritabanlarını destekliyor musunuz?

Hayır, Microsoft dış özel meta veri deposu olarak yalnızca Azure SQL veritabanını destekler.

### <a name="can-i-share-a-metastore-across-multiple-clusters"></a>Birden çok küme arasında bir meta veri deposu paylaşabilir miyim?

Evet, aynı HDInsight sürümünü kullandıkları sürece, birden çok kümede özel meta veri deposu paylaşabilirsiniz.

## <a name="connectivity-and-virtual-networks"></a>Bağlantı ve sanal ağlar  

### <a name="what-are-the-implications-of-blocking-ports-22-and-23-on-my-network"></a>Ağımdaki 22 ve 23 bağlantı noktalarını engellemenin etkileri nelerdir?

Bağlantı noktalarını 22 ve bağlantı noktası 23 ' ü engellerseniz, kümeye SSH erişimi olmayacaktır. Bu bağlantı noktaları HDInsight hizmeti tarafından kullanılmaz.

Daha fazla bilgi için aşağıdaki belgelere bakın:

* [Ağ trafiğini denetleme](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment#networktraffic)
* [Özel uç nokta ile bir sanal ağdaki HDInsight kümelerine gelen trafiği güvenli hale getirme](https://azure.microsoft.com/blog/secure-incoming-traffic-to-hdinsight-clusters-in-a-vnet-with-private-endpoint/)
* [HDInsight yönetim IP adresleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses)

### <a name="can-i-deploy-an-additional-virtual-machine-within-the-same-subnet-as-an-hdinsight-cluster"></a>Aynı alt ağ içinden bir HDInsight kümesiyle ek bir sanal makine dağıtabilir miyim?

Evet, bir HDInsight kümesiyle aynı alt ağ içinde ek bir sanal makine dağıtabilirsiniz. Aşağıdaki konfigürasyonlar mümkündür:

* Kenar düğümleri: [HDInsight 'ta Apache Hadoop kümelerinde boş kenar düğümlerini kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-apps-use-edge-node)bölümünde açıklandığı gibi, kümeye başka bir kenar düğümü ekleyebilirsiniz. 
* Tek başına düğümler:  Aynı alt ağa tek başına bir sanal makine ekleyebilir ve özel uç noktasını `https://<CLUSTERNAME>-int.azurehdinsight.net`kullanarak bu sanal makineden kümeye erişebilirsiniz. Daha fazla bilgi için bkz. [ağ trafiğini denetleme](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment#networktraffic).

### <a name="can-i-add-an-existing-hdinsight-cluster-to-another-virtual-network"></a>Var olan bir HDInsight kümesini başka bir sanal ağa ekleyebilir miyim?

Hayır, var olan bir HDInsight kümesini başka bir sanal ağa ekleyemezsiniz. Sanal ağ, sağlama sırasında belirtilmelidir. Sağlama sırasında sanal ağ belirtilmemişse dağıtım, dışından erişilemeyen bir iç ağ oluşturur. Daha fazla bilgi için bkz. [mevcut bir sanal ağa HDInsight ekleme](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment#existingvnet).

## <a name="security-and-certificates"></a>Güvenlik ve sertifikalar

Azure HD Insight kümelerinde kötü amaçlı yazılımdan koruma için Microsoft önerileri nelerdir?

Kötü amaçlı yazılımdan koruma hakkında bilgi için bkz. [Azure Cloud Services Için Microsoft kötü amaçlı yazılımdan koruma ve sanal makineler](https://docs.microsoft.com/azure/security/fundamentals/antimalware).

### <a name="how-do-i-create-a-keytab-for-an-hdinsight-esp-cluster"></a>HDInsight ESP kümesi için bir keytab oluşturmak Nasıl yaparım? misiniz?

Etki alanı kullanıcı adınız için bir Kerberos keytab oluşturabilirsiniz. Daha sonra bu keytab 'ı kullanarak etki alanına katılmış kümelerin parolasını girmeden kimlik doğrulaması yapabilirsiniz. Etki alanı adında büyük harflı karakterlere dikkat edin:

```
o ktutil 
o ktutil: addent -password -p stXX@ DOMAIN.COM -k 1 -e RC4-HMAC 
o Password for stXX@DOMAIN.COM: provided by lab instructors 
o ktutil: wkt stXX.keytab 
o ktutil: q
```

### <a name="can-i-use-an-existing-azure-active-directory-tenant-to-create-an-hdinsight-cluster-that-has-the-esp"></a>ESP içeren bir HDInsight kümesi oluşturmak için var olan bir Azure Active Directory kiracıyı kullanabilir miyim?

Azure Active Directory Domain Services (AAD-DS) etkinleştirilmesi, ESP içeren bir HDInsight kümesi oluşturabilmeniz için bir önkoşuldur. Açık kaynaklı Hadoop, kimlik doğrulaması için Kerberos kullanır (OAuth aksine).

Sanal makineleri bir etki alanına katmak için bir etki alanı denetleyicisi gerekir. AAD-DS, yönetilen etki alanı denetleyicisidir. Güvenli bir Hadoop kümesini yönetilen bir şekilde oluşturmak için tüm Kerberos gereksinimlerini sağlayan bir AAD uzantısı olarak kabul edilir. Yönetilen bir hizmet olarak HDInsight, son uca güvenlik sağlamak için AAD-DS ile tümleşir.

### <a name="can-i-use-a-self-signed-certificate-in-an-aad-ds-secure-ldap-setup-and-provision-an-esp-cluster"></a>AAD-DS Güvenli LDAP kurulumunda kendinden imzalı bir sertifika kullanabilir ve bir ESP kümesi sağlayabilir miyim?

Bir sertifika yetkilisi (CA) tarafından verilen bir sertifika kullanılması önerilse de, otomatik olarak imzalanan bir sertifika kullanılması da ESP 'de desteklenir.

Daha fazla bilgi için bkz.

* [Azure AD-DS 'yi etkinleştirme](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds#enable-azure-ad-ds)
* [Öğretici: Azure Active Directory Domain Services yönetilen bir etki alanı için Güvenli LDAP yapılandırma](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-configure-ldaps)

### <a name="how-can-i-pull-login-activity-shown-in-ranger"></a>Ranger 'da gösterilen oturum açma etkinliğini nasıl çekirim?

Denetim gereksinimleri için, Microsoft, [HDInsight kümelerini izlemek Için Azure izleyici günlüklerini kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-oms-log-analytics-tutorial)bölümünde açıklandığı gibi Azure izleyici günlüklerini etkinleştirmenizi önerir.

### <a name="can-i-disable-clamscan-on-my-cluster"></a>Kümemdeki ClamScan 'ı devre dışı bırakabilir miyim?

ClamScan, HDInsight kümesinde çalışan ve Azure güvenliği (azsecd) tarafından, kümelerinizi virüs saldırılarına karşı korumak için kullanılan bir virüsten koruma yazılımıdır. Microsoft, kullanıcıların varsayılan ClamScan yapılandırmasında herhangi bir değişiklik yapmasını kesinlikle önerir.

Bu işlem, diğer işlemlerden uzaklaşmaz veya herhangi bir döngüden yararlanın. Her zaman başka bir işleme göre yapılır. Clamsa 'dan CPU ani artışları yalnızca sistem boştayken görülebilmelidir.  

Zamanlamayı kontrol etmeniz gereken senaryolarda aşağıdaki adımları kullanabilirsiniz:

1. Aşağıdaki komutu kullanarak otomatik yürütmeyi devre dışı bırakın:

   ``/usr/local/vbin/azsecd config -s clamav -d Disabled``

2. Bu komutu kök olarak çalıştıran bir cron işi oluşturun:

   ``/usr/local/bin/azsecd manual -s clamav``

Bir cron işini ayarlama ve çalıştırma hakkında daha fazla bilgi için bkz. [nasıl yaparım? bir cron işi ayarlama](https://askubuntu.com/questions/2368/how-do-i-set-up-a-cron-job).

## <a name="storage"></a>Depolama

### <a name="can-i-add-an-azure-data-lake-storage-gen2-to-an-existing-hdinsight-cluster-as-an-additional-storage-account"></a>Mevcut bir HDInsight kümesine ek depolama hesabı olarak Azure Data Lake Storage 2. ekleyebilir miyim?

Hayır. Şu anda, birincil depolama alanı olarak blob depolamayı kullanan mevcut bir HDInsight kümesine Azure Data Lake Storage 2. ekleyemezsiniz. Daha fazla bilgi için bkz. [Azure HDInsight kümeleri ile kullanım için depolama seçeneklerini karşılaştırma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-compare-storage-options). 

### <a name="how-can-i-find-the-currently-linked-service-principal-for-a-data-lake-storage-account"></a>Data Lake depolama hesabı için geçerli bağlı hizmet sorumlusunu nasıl bulabilirim?

Portalda küme özelliklerinizi Data Lake Storage 1. erişimi inceleyerek bu ayarları doğrulayabilirsiniz. Daha fazla bilgi için bkz. [küme kurulumunu doğrulama](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal#verify-cluster-set-up).
 
HDInsight kümelerim için depolama hesaplarının ve BLOB kapsayıcılarının kullanımını nasıl hesaplayabilirim?
Aşağıdaki yordamlardan birini kullanabilirsiniz:

* [PowerShell kullanma](https://docs.microsoft.com/azure/storage/scripts/storage-blobs-container-calculate-size-powershell)
* /User/Hive/boyutunu da bulabilirsiniz. Aşağıdaki komutu kullanarak HDI kümelerinde çöp/klasör:

   ``hdfs dfs -du -h /user/hive/.Trash/``

### <a name="how-can-i-set-up-auditing-for-my-blob-storage-account"></a>BLOB depolama hesabımın denetimini nasıl ayarlayabilirim?

BLOB depolama hesaplarını denetlemek için, [Azure Portal bir depolama hesabını İzleme bölümünde](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account)belgelenen yordamı kullanarak BLOB depolama hesabı için izlemeyi yapılandırmanız gerekir. Bir bir bir bir bir bir bir bir bir denetim günlüğü yalnızca yerel bir bu dosya sistemi için denetim bilgilerini sağlar  Uzak depolama üzerinde gerçekleştirilen işlemleri içermez.

### <a name="how-can-i-transfer-files-between-a-blob-container-and-an-hdinsight-head-node"></a>Blob kapsayıcısı ve HDInsight baş düğümü arasında dosyaları nasıl aktarabilirim?

Aşağıdaki örneğe benzer şekilde, baş düğümümüzde bir kabuk betiği çalıştırarak bir blob kapsayıcısı ve bir HDInsight baş düğümü arasında dosya aktarabilirsiniz:

```
for i in cat filenames.txt

do
         hadoop fs -get $i <local destination>
done
```
 
> [!NOTE]
> Dosya *adları. txt* dosyası blob kapsayıcılarındaki dosyaların mutlak yoluna sahip olur.
 
### <a name="are-there-any-ranger-plugins-for-storage"></a>Depolama için bir Ranger eklentileri var mı?

Şu anda BLOB depolama, Azure Data Lake Storage (ADLS) Gen1 veya Azure Data Lake Storage 2. için bir Ranger eklentileri yok. ESP kümeleri için, ADLS 'yi en iyi uygulama olarak kullanın ve sistem düzeyinde, bir Ayrıca, ADLS kullandığınızda, ESP kümeleri küme düzeyinde AAD kullanarak bazı dosya sistemi erişim denetimini yapılır. 

Aşağıdaki makalelerde belgelenen yordamları kullanarak kullanıcılarınızın bulunduğu güvenlik gruplarına veri erişim ilkeleri atamak için Azure Depolama Gezgini kullanmanız gerekir:

* [Nasıl yaparım?, Azure AD kullanıcılarının Hive veya diğer hizmetleri kullanarak Data Lake Storage 2. verileri sorgulaması için izinleri ayarlamak ister misiniz?](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2#how-do-i-set-permissions-for-azure-ad-users-to-query-data-in-data-lake-storage-gen2-by-using-hive-or-other-services)
* [Azure Data Lake Storage 2. ile Azure Depolama Gezgini kullanarak dosya ve Dizin düzeyi izinleri ayarlama](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-how-to-set-permissions-storage-explorer)

### <a name="can-i-increase-hdfs-storage-on-a-cluster-without-increasing-the-disk-size-of-worker-nodes"></a>Çalışan düğümlerinin disk boyutunu arttırmadan, bir kümede, bu depolama alanını artırabilir miyim?

Hayır, herhangi bir çalışan düğümünün disk boyutunu artıramezsiniz. Diskin boyutunu artırmanın tek yolu kümeyi bırakıp daha büyük çalışan sanal makinelerde yeniden oluşturmaktır.

Microsoft, kümeyi sildiğinizde veriler silindiğinden, HDInsight 'ta verilerinizin depolanmasını depolamak için bir Microsoft bu sürümü kullanmanızı önermez. Bunun yerine, verilerinizi Azure 'da depolamanızı öneririz. Kümenin ölçeğini ölçeklendirmek, HDInsight kümenize daha fazla kapasite de ekleyebilir.

## <a name="edge-nodes"></a>Uç düğümleri

### <a name="can-i-add-an-edge-node-after-the-cluster-has-been-created"></a>Küme oluşturulduktan sonra bir kenar düğümü ekleyebilir miyim?

Evet, var olan bir HDInsight kümesine veya kümeyi oluştururken yeni bir kümeye boş bir Edge düğümü ekleyebilirsiniz.

Daha fazla bilgi için bkz. [HDInsight'taki boş kenar düğümlerini Apache Hadoop kümelerinde kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-apps-use-edge-node).

### <a name="how-can-i-connect-to-an-edge-node"></a>Kenar düğümüne nasıl bağlanabilirim?

Edge düğümü oluşturduktan sonra, bağlantı noktası 22 ' de SSH 'yi kullanarak Edge düğümüne bağlanabilirsiniz. Uç düğümün adını küme portalından bulabilirsiniz (adlar genellikle "-Ed" olarak sona erdir).

### <a name="why-are-persisted-scripts-not-running-automatically-on-newly-created-edge-nodes"></a>Kalıcı betikler neden yeni oluşturulan kenar düğümlerinde otomatik olarak çalışmıyor?

Kalıcı betikler, ölçeklendirme işlemleri aracılığıyla kümeye eklenen yeni çalışan düğümlerini özelleştirmek için kullanılır ve kenar düğümleri için uygulanmaz.

## <a name="rest-api"></a>REST API

### <a name="what-are-the-rest-api-calls-to-pull-a-tez-query-view-from-the-cluster"></a>Kümeden bir tez sorgu görünümü çekmek için REST API çağrısı nedir?

Yanıtın JSON biçiminde olacağı gerekli bilgileri çekmek için aşağıdaki REST uç noktalarını kullanabilirsiniz. Temel kimlik doğrulama üstbilgileri, bu istekleri yapmak için kullanılabilir.

* "Tez sorgusu" görünümü – https://`<cluster name>`. azurehdinsight.net/WS/v1/Timeline/HIVE_QUERY_ID/

* "Tez dag" View- https://`<cluster name>`. azurehdinsight.net/WS/v1/Timeline/TEZ_DAG_ID/

### <a name="how-do-i-retrieve-the-configuration-details-from-hdi-cluster-by-using-an-azure-active-directory-user"></a>Nasıl yaparım? Azure Active Directory Kullanıcı kullanarak HDI kümesinden yapılandırma ayrıntılarını almak istiyor musunuz?

AAD kullanıcılarınız ile uygun kimlik doğrulama belirteçlerini anlaşmak için aşağıdaki biçimi kullanarak ağ geçidine gidin:

* https://`<cluster dnsname>`. azurehdinsight.net/api/v1/Clusters/testclusterdem/stack_versions/1/repository_versions/1 

### <a name="how-do-i-use-ambari-restful-api-to-monitor-yarn-performance"></a>YARN performansını izlemek için Nasıl yaparım? ambarı yeniden oluşturma API 'sini kullanmak mı istiyorsunuz?

Aynı VNet veya eşlenmiş VNet 'te kıvrımlı komutu çağrılırsa, aşağıdaki komutu kullanın:

``curl -u <cluster login username> -sS -G http://headnodehost:8080/api/v1/clusters/anssenllap/services/YARN/components/NODEMANAGER?fields=metrics/cpu``
 
Komut VNet dışından veya eşlenmiş olmayan VNet 'ten çağrılırsa, komut biçimi şu şekilde olur:
 
* ESP olmayan bir küme için:``curl -u <cluster login username> -sS -G https://ClusterName.azurehdinsight.net/api/v1/clusters/ClusterName/services/YARN/components/NODEMANAGER?fields=metrics/cpu``
 
* Bir ESP kümesi için:``curl -u <cluster login username> -sS -G https://ClusterName.azurehdinsight.net/api/v1/clusters/ClusterName/services/YARN/components/NODEMANAGER?fields=metrics/cpu``

> [!NOTE]
> Kıvrımlı bir parola ister. Küme oturum açma Kullanıcı adı için geçerli bir parola girmeniz gerekir.

## <a name="billing"></a>Faturalandırma

### <a name="how-much-does-it-cost-to-deploy-an-hdinsight-cluster"></a>HDInsight kümesini dağıtma maliyeti ne kadar sürer?

Faturalandırma ile ilgili fiyatlandırma ve SSS hakkında daha fazla bilgi için bkz. [Azure HDInsight fiyatlandırma](https://azure.microsoft.com/pricing/details/hdinsight/) sayfası.

### <a name="how-do-i-cancel-my-subscription"></a>Aboneliğimi iptal Nasıl yaparım? mi?

Aboneliğinizi iptal etme hakkında daha fazla bilgi için bkz. [Azure aboneliğinizi Iptal etme](https://docs.microsoft.com/azure/billing/billing-how-to-cancel-azure-subscription).

### <a name="for-pay-as-you-go-subscriptions-what-happens-after-i-cancel-my-subscription"></a>Kullandıkça Öde abonelikleri için aboneliğimi iptal etmem durumunda ne olur?

Aboneliğiniz iptal edildikten sonra hakkında daha fazla bilgi için bkz. [aboneliğimi iptal etdiğimde ne olur?](https://docs.microsoft.com/azure/billing/billing-how-to-cancel-azure-subscription#what-happens-after-i-cancel-my-subscription)

## <a name="hive"></a>Hive

### <a name="why-does-the-hive-version-appear-as-121000-instead-of-21-in-the-ambari-ui-even-though-i-am-running-an-hdinsight-36-cluster"></a>Bir HDInsight 3,6 kümesi çalıştırdığım halde, Hive sürümü neden 1.2.1000 olarak 2,1 yerine olarak görünür?

Yalnızca 1,2, Hive Kullanıcı arabiriminde görünse de, HDInsight 3,6 hem Hive 1,2 hem de Hive 2,1 içerir.

## <a name="other-faq"></a>Diğer SSS

### <a name="what-does-hdinsight-offer-in-terms-of-real-time-stream-processing-capabilities"></a>HDInsight, gerçek zamanlı akış işleme özellikleri açısından ne sunuyor?

Azure HDInsight 'ta akış işleme tümleştirme özellikleri hakkında daha fazla bilgi için bkz. [Azure 'da akış işleme teknolojisi seçme](https://docs.microsoft.com/azure/architecture/data-guide/technology-choices/stream-processing).

### <a name="is-there-a-way-to-dynamically-terminate-the-head-node-of-the-cluster-when-the-cluster-is-idle-for-a-specific-period"></a>Küme belirli bir dönemde boşta kaldığında kümenin baş düğümünü dinamik olarak sonlandırmak için bir yol var mı?

Hayır, kümenin baş düğümünü dinamik olarak sonlandıramezsiniz. Bu senaryo için Azure Data Factory kullanabilirsiniz.
