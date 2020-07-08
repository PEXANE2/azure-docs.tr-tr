---
title: Görev çalışma zamanı ortam değişkenleri
description: Azure Batch Analytics için görev çalışma zamanı ortamı değişken Kılavuzu ve başvurusu.
ms.topic: conceptual
ms.date: 09/12/2019
ms.openlocfilehash: 6b8ade312146802ede6e12181a082a8fcd3842fe
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85960920"
---
# <a name="azure-batch-runtime-environment-variables"></a>Çalışma zamanı ortam değişkenlerini Azure Batch

[Azure Batch hizmeti](https://azure.microsoft.com/services/batch/) , işlem düğümlerinde aşağıdaki ortam değişkenlerini ayarlar. Görev komut satırlarında bu ortam değişkenlerine ve komut satırları tarafından çalıştırılan programlar ve betiklere başvurabilirsiniz.

Batch ile ortam değişkenlerini kullanma hakkında daha fazla bilgi için bkz. [Görevler Için ortam ayarları](./jobs-and-tasks.md#environment-settings-for-tasks).

## <a name="environment-variable-visibility"></a>Ortam değişkeni görünürlüğü

Bu ortam değişkenleri yalnızca **görev kullanıcısı**bağlamında görünür ve bir görevin yürütüldüğü düğüm üzerindeki kullanıcı hesabıdır. Bir işlem düğümüne Uzak Masaüstü Protokolü (RDP) veya Güvenli Kabuk (SSH) aracılığıyla [uzaktan bağlanıp](./error-handling.md#connect-to-compute-nodes) ortam değişkenlerini listelerseniz bunları *göremezsiniz*. Bunun nedeni, uzak bağlantı için kullanılan kullanıcı hesabının görev tarafından kullanılan hesapla aynı olmamasıdır.

Bir ortam değişkeninin geçerli değerini almak için, `cmd.exe` bir Windows işlem düğümünde veya `/bin/sh` Linux düğümünde başlatın:

`cmd /c set <ENV_VARIABLE_NAME>`

`/bin/sh printenv <ENV_VARIABLE_NAME>`

## <a name="command-line-expansion-of-environment-variables"></a>Ortam değişkenlerinin komut satırı genişletmesi

İşlem düğümlerinde görevler tarafından yürütülen komut satırları bir kabuk altında çalışmaz. Bu nedenle, bu komut satırları ortam değişkeni genişletmesi gibi kabuk özelliklerinden yerel olarak yararlanamaz (buna dahildir `PATH` ). Bu özelliklerden yararlanmak için, komut satırında **kabuğu çağırmanız** gerekir. Örneğin, `cmd.exe` Windows işlem düğümlerinde veya `/bin/sh` Linux düğümlerinde başlatın:

`cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

`/bin/sh -c MyTaskApplication $MY_ENV_VAR`

## <a name="environment-variables"></a>Ortam değişkenleri

| Değişken adı                     | Açıklama                                                              | Kullanılabilirlik | Örnek |
|-----------------------------------|--------------------------------------------------------------------------|--------------|---------|
| AZ_BATCH_ACCOUNT_NAME           | Görevin ait olduğu Batch hesabının adı.                  | Tüm görevler.   | mybatchaccount |
| AZ_BATCH_ACCOUNT_URL            | Batch hesabının URL 'SI. | Tüm görevler. | `https://myaccount.westus.batch.azure.com` |
| AZ_BATCH_APP_PACKAGE            | Tüm uygulama paketi ortam değişkenlerinin ön eki. Örneğin, uygulama "FOO" sürümü "1" bir havuz üzerine yüklenirse, ortam değişkeni AZ_BATCH_APP_PACKAGE_FOO_1 (Linux üzerinde) veya AZ_BATCH_APP_PACKAGE_FOO # 1 (Windows üzerinde). AZ_BATCH_APP_PACKAGE_FOO_1, paketin indirildiği konuma (bir klasör) işaret eder. Uygulama paketinin varsayılan sürümünü kullanırken, sürüm numaraları olmadan AZ_BATCH_APP_PACKAGE ortam değişkenini kullanın. Linux 'ta ve uygulama paketi adı "Agent-Linux-x64" ise ve sürüm "1.1.46.0 ise ortam adı aslında: AZ_BATCH_APP_PACKAGE_agent_linux_x64_1_1_46_0, alt çizgi ve küçük harf kullanımı. Daha ayrıntılı bilgi için [buraya](./batch-application-packages.md#execute-the-installed-applications) bakın. | İlişkili bir uygulama paketine sahip herhangi bir görev. Ayrıca, düğümün uygulama paketleri varsa tüm görevler için de kullanılabilir. | AZ_BATCH_APP_PACKAGE_FOO_1 (Linux) veya AZ_BATCH_APP_PACKAGE_FOO # 1 (Windows) |
| AZ_BATCH_AUTHENTICATION_TOKEN   | Sınırlı bir Batch hizmeti işlemleri kümesine erişim veren bir kimlik doğrulama belirteci. Bu ortam değişkeni yalnızca, [görev eklendiğinde](/rest/api/batchservice/task/add#request-body) [authenticationtokensettings](/rest/api/batchservice/task/add#authenticationtokensettings) ayarlandıysa bulunur. Belirteç değeri, Batch API 'Lerinde, [batchclient. Open () .NET API](/dotnet/api/microsoft.azure.batch.batchclient.open#Microsoft_Azure_Batch_BatchClient_Open_Microsoft_Azure_Batch_Auth_BatchTokenCredentials_)'sinde olduğu gibi Batch istemcisi oluşturmak için kimlik bilgileri olarak kullanılır. | Tüm görevler. | OAuth2 erişim belirteci |
| AZ_BATCH_CERTIFICATES_DIR       | Bir [görev çalışma dizini][files_dirs] içinde, sertifikaların Linux işlem düğümleri için depolandığı bir dizin. Bu ortam değişkeni Windows işlem düğümleri için geçerlidir.                                                  | Tüm görevler.   |  /mnt/Batch/Tasks/WorkItem/batchjob001/Job-1/task001/CERT |
| AZ_BATCH_HOST_LIST              | Biçimdeki [çok örnekli bir göreve][multi_instance] ayrılan düğümlerin listesi `nodeIP,nodeIP` . | Çok örnekli birincil ve alt görevler. | `10.0.0.4,10.0.0.5` |
| AZ_BATCH_IS_CURRENT_NODE_MASTER | Geçerli düğümün [çok örnekli bir görevin][multi_instance]ana düğümü olup olmadığını belirtir. Olası değerler şunlardır `true` `false` .| Çok örnekli birincil ve alt görevler. | `true` |
| AZ_BATCH_JOB_ID                 | Görevin ait olduğu işin kimliği. | Başlangıç görevi dışındaki tüm görevler. | batchjob001 |
| AZ_BATCH_JOB_PREP_DIR           | Düğümdeki iş hazırlama [görevi dizininin][files_dirs] tam yolu. | Başlangıç görevi ve iş hazırlama görevi dışındaki tüm görevler. Yalnızca iş bir iş hazırlama göreviyle yapılandırılmışsa kullanılabilir. | C:\user\tasks\workıtem\jobprepreleasesamplejob\job-1\jobhazırlama |
| AZ_BATCH_JOB_PREP_WORKING_DIR   | Düğümdeki iş hazırlama [görevi çalışma dizininin][files_dirs] tam yolu. | Başlangıç görevi ve iş hazırlama görevi dışındaki tüm görevler. Yalnızca iş bir iş hazırlama göreviyle yapılandırılmışsa kullanılabilir. | C:\user\tasks\workitems\jobprepreleasesamplejob\job-1\jobpreparation\wd |
| AZ_BATCH_MASTER_NODE            | [Çoklu örnek görevinin][multi_instance] birincil görevinin çalıştırıldığı Işlem düğümünün IP adresi ve bağlantı noktası. MPı veya NCCL iletişimi için burada belirtilen bağlantı noktasını kullanmayın. Azure Batch hizmeti için ayrılmıştır. Bunun yerine MASTER_PORT değişkenini, komut satırı bağımsız değişkeni aracılığıyla geçirilen bir değer ile ayarlayarak (bağlantı noktası 6105 iyi bir varsayılan seçenektir) veya varsa AML kümeleri değerini kullanarak kullanın. | Çok örnekli birincil ve alt görevler. | `10.0.0.4:6000` |
| AZ_BATCH_NODE_ID                | Görevin atandığı düğümün KIMLIĞI. | Tüm görevler. | TVM-1219235766_3-20160919t172711z |
| AZ_BATCH_NODE_IS_DEDICATED      | Varsa `true` , geçerli düğüm özel bir düğümdür. `false`Bu, [düşük öncelikli bir düğümdür](batch-low-pri-vms.md). | Tüm görevler. | `true` |
| AZ_BATCH_NODE_LIST              | Biçimdeki [çok örnekli bir göreve][multi_instance] ayrılan düğümlerin listesi `nodeIP;nodeIP` . | Çok örnekli birincil ve alt görevler. | `10.0.0.4;10.0.0.5` |
| AZ_BATCH_NODE_MOUNTS_DIR        | Tüm bağlama dizinlerinin bulunduğu düğüm düzeyi [dosya sistemi bağlama](virtual-file-mount.md) konumunun tam yolu. Windows dosya paylaşımları bir sürücü harfi kullanır, bu nedenle Windows için bağlama sürücüsü cihazların ve sürücülerin bir parçasıdır.  |  Başlangıç görevi dahil tüm görevlerin Kullanıcı erişimi vardır ve Kullanıcı, bağlı dizin için bağlama izinlerinden haberdar olur. | Ubuntu 'da, örneğin, konum:`/mnt/batch/tasks/fsmounts` |
| AZ_BATCH_NODE_ROOT_DIR          | Düğümdeki tüm [Batch dizinleri][files_dirs] kökünün tam yolu. | Tüm görevler. | C:\user\tasks |
| AZ_BATCH_NODE_SHARED_DIR        | Düğümdeki [Paylaşılan dizinin][files_dirs] tam yolu. Bir düğümde yürütülen tüm görevlerin bu dizine okuma/yazma erişimi vardır. Diğer düğümlerde yürütülen görevlerin bu dizine uzaktan erişimi yoktur ("paylaşılan" bir ağ dizini değildir). | Tüm görevler. | C:\user\tasks\shared |
| AZ_BATCH_NODE_STARTUP_DIR       | Düğümdeki [Başlangıç görevi dizininin][files_dirs] tam yolu. | Tüm görevler. | C:\user\tasks\startup |
| AZ_BATCH_POOL_ID                | Görevin çalıştığı havuzun kimliği. | Tüm görevler. | batchpool001 |
| AZ_BATCH_TASK_DIR               | Düğümdeki [görev dizininin][files_dirs] tam yolu. Bu dizin, `stdout.txt` `stderr.txt` görevi için ve ve AZ_BATCH_TASK_WORKING_DIR içerir. | Tüm görevler. | C:\user\tasks\workıtem\batchjob001\job-1\task001 |
| AZ_BATCH_TASK_ID                | Geçerli görevin kimliği. | Başlangıç görevi dışındaki tüm görevler. | task001 |
| AZ_BATCH_TASK_SHARED_DIR | Birincil görev ve [çok örnekli bir görevin][multi_instance]her alt görevi için özdeş olan bir dizin yolu. Yol, çoklu örnek görevinin çalıştırıldığı her düğümde bulunur ve bu düğümde çalışan görev komutlarına okuma/yazma ( [koordinasyon komutu][coord_cmd] ve [uygulama komutu][app_cmd]) tarafından erişilebilir. Diğer düğümlerde yürütülen alt görevler veya birincil görevin bu dizine uzaktan erişimi yoktur ("paylaşılan" bir ağ dizini değildir). | Çok örnekli birincil ve alt görevler. | C:\user\tasks\workıtemk\multiınstancesamplejob\job-1\multiınstancesampletask |
| AZ_BATCH_TASK_WORKING_DIR       | Düğümdeki [görev çalışma dizininin][files_dirs] tam yolu. Şu anda çalışan görevin bu dizine okuma/yazma erişimi vardır. | Tüm görevler. | C:\user\tasks\workıtem\batchjob001\job-1\task001\wd |
| CCP_NODES                       | Bir [çok örnekli göreve][multi_instance]ayrılan düğümlerin ve düğüm başına çekirdek sayısının listesi. Düğümler ve çekirdekler şu biçimde listelenmiştir`numNodes<space>node1IP<space>node1Cores<space>`<br/>`node2IP<space>node2Cores<space> ...`, burada düğüm sayısının bir veya daha fazla düğüm IP adresi ve her biri için çekirdek sayısı gelir. |  Çok örnekli birincil ve alt görevler. |`2 10.0.0.4 1 10.0.0.5 1` |

[files_dirs]: ./files-and-directories.md
[multi_instance]: ./batch-mpi.md
[coord_cmd]: ./batch-mpi.md#coordination-command
[app_cmd]: ./batch-mpi.md#application-command
