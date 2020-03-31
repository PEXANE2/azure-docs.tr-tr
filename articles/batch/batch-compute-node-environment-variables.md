---
title: Görev çalışma zamanı ortamı değişkenleri - Azure Toplu İş | Microsoft Dokümanlar
description: Azure Toplu İş Analizi için görev çalışma zamanı ortamı değişken kılavuzu ve başvurusu.
services: batch
author: LauraBrenner
manager: evansma
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 09/12/2019
ms.author: labrenne
ms.openlocfilehash: ebaa06acf309a0f941b8b4efd76fa4e9e7460810
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80053942"
---
# <a name="azure-batch-runtime-environment-variables"></a>Azure Toplu çalışma zamanı ortamı değişkenleri

[Azure Toplu İşlem hizmeti,](https://azure.microsoft.com/services/batch/) işlem düğümlerinde aşağıdaki ortam değişkenlerini ayarlar. Bu ortam değişkenlerine görev komut satırlarında ve komut satırları tarafından çalıştırılan programlarda ve komut programlarında başvuruyapabilirsiniz.

Toplu Iş ile ortam değişkenlerini kullanma hakkında daha fazla bilgi [için görevler için Ortam ayarlarına](https://docs.microsoft.com/azure/batch/batch-api-basics#environment-settings-for-tasks)bakın.

## <a name="environment-variable-visibility"></a>Ortam değişken görünürlüğü

Bu ortam değişkenleri yalnızca **görev kullanıcısı**bağlamında , görevin yürütüldedildiği düğümdeki kullanıcı hesabı bağlamında görünür. Bir işlem düğümüne Uzak Masaüstü Protokolü (RDP) veya Güvenli Kabuk (SSH) aracılığıyla [uzaktan bağlanıp](https://azure.microsoft.com/documentation/articles/batch-api-basics/#connecting-to-compute-nodes) ortam değişkenlerini listelerseniz bunları *göremezsiniz*. Bunun nedeni, uzak bağlantı için kullanılan kullanıcı hesabının görev tarafından kullanılan hesapla aynı olmamasıdır.

Bir ortam değişkeninin geçerli değerini `cmd.exe` almak için, Windows bilgi `/bin/sh` işlem düğümünde veya Linux düğümünde başlatın:

`cmd /c set <ENV_VARIABLE_NAME>`

`/bin/sh printenv <ENV_VARIABLE_NAME>`

## <a name="command-line-expansion-of-environment-variables"></a>Ortam değişkenlerinin komut satırı genişlemesi

İşlem düğümleri üzerindeki görevler tarafından yürütülen komut satırları bir kabuk altında çalışmaz. Bu nedenle, bu komut satırları ortam değişkeni genişletme gibi kabuk `PATH`özelliklerinden doğal olarak yararlanamaz (bu içerir). Bu özelliklerden yararlanmak için komut satırındaki **kabuğu çağırmanız** gerekir. Örneğin, Windows `cmd.exe` işlem düğümleri veya `/bin/sh` Linux düğümleri üzerinde başlatın:

`cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

`/bin/sh -c MyTaskApplication $MY_ENV_VAR`

## <a name="environment-variables"></a>Ortam değişkenleri

| Değişken adı                     | Açıklama                                                              | Kullanılabilirlik | Örnek |
|-----------------------------------|--------------------------------------------------------------------------|--------------|---------|
| AZ_BATCH_ACCOUNT_NAME           | Görevin ait olduğu Toplu Iş hesabının adı.                  | Tüm görevler.   | mybatchaccount |
| AZ_BATCH_ACCOUNT_URL            | Toplu İşlem hesabının URL'si. | Tüm görevler. | `https://myaccount.westus.batch.azure.com` |
| AZ_BATCH_APP_PACKAGE            | Tüm uygulama paketi ortamı değişkenlerinin öneki. Örneğin, Uygulama "FOO" sürümü "1" bir havuza yüklenirse, ortam değişkeni AZ_BATCH_APP_PACKAGE_FOO_1 (Linux'ta) veya AZ_BATCH_APP_PACKAGE_FOO#1 (Windows'da) olur. AZ_BATCH_APP_PACKAGE_FOO_1 paketin indirilmeye (klasör) yükedildiği konuma işaret eder. Uygulama paketinin varsayılan sürümünü kullanırken, sürüm numaraları olmadan AZ_BATCH_APP_PACKAGE ortam değişkenini kullanın. Linux'ta ve uygulama paketi adı "Agent-linux-x64" ve sürüm "1.1.46.0" ise, ortam adı aslında: AZ_BATCH_APP_PACKAGE_agent_linux_x64_1_1_46_0, alt ve küçük harf kullanarak. Daha ayrıntılı bilgi için [buraya](https://docs.microsoft.com/azure/batch/batch-application-packages#execute-the-installed-applications) bakın. | İlişkili bir uygulama paketiyle ilgili herhangi bir görev. Düğümün kendisi nde uygulama paketleri varsa tüm görevler için de kullanılabilir. | AZ_BATCH_APP_PACKAGE_FOO_1 (Linux) veya AZ_BATCH_APP_PACKAGE_FOO#1 (Windows) |
| AZ_BATCH_AUTHENTICATION_TOKEN   | Sınırlı bir Toplu Iş hizmeti işlemleri kümesine erişim sağlayan bir kimlik doğrulama belirteci. Bu ortam değişkeni yalnızca [kimlik doğrulamaTokenAyarlar](/rest/api/batchservice/task/add#authenticationtokensettings) [görev eklendiğinde](/rest/api/batchservice/task/add#request-body)ayarlanırsa bulunur. Belirteç değeri Toplu İş API'lerinde, [BatchClient.Open() .NET API'sinde](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.batchclient.open#Microsoft_Azure_Batch_BatchClient_Open_Microsoft_Azure_Batch_Auth_BatchTokenCredentials_)olduğu gibi bir Toplu İş istemcisi oluşturmak için kimlik bilgileri olarak kullanılır. | Tüm görevler. | OAuth2 erişim jetonu |
| AZ_BATCH_CERTIFICATES_DIR       | Linux işlem düğümleri için sertifikaların depolandığı [görev çalışma dizini][files_dirs] içinde bir dizini. Bu ortam değişkeni Windows işlem düğümleri için geçerli değildir.                                                  | Tüm görevler.   |  /mnt/batch/tasks/workitems/batchjob001/job-1/task001/certs |
| AZ_BATCH_HOST_LIST              | Biçimde `nodeIP,nodeIP` [çok örnekli bir göreve][multi_instance] ayrılan düğümlerin listesi. | Çok örnekli birincil ve alt görevler. | `10.0.0.4,10.0.0.5` |
| AZ_BATCH_IS_CURRENT_NODE_MASTER | Geçerli düğümün [çok örnekli bir görev][multi_instance]için ana düğüm olup olmadığını belirtir. Olası değerler `true` `false`ve .| Çok örnekli birincil ve alt görevler. | `true` |
| AZ_BATCH_JOB_ID                 | Görevin ait olduğu işin kimliği. | Başlangıç görevi dışındaki tüm görevler. | batchjob001 |
| AZ_BATCH_JOB_PREP_DIR           | Düğümüzerinde iş hazırlama [görev dizininin][files_dirs] tam yolu. | Başlangıç görevi ve iş hazırlama görevi dışındaki tüm görevler. Yalnızca iş bir iş hazırlama göreviyle yapılandırılırsa kullanılabilir. | C:\user\tasks\workitems\jobprepreleasesamplejob\job-1\jobpreparation |
| AZ_BATCH_JOB_PREP_WORKING_DIR   | Düğüm üzerinde çalışma dizini iş hazırlama [görevitam][files_dirs] yolu. | Başlangıç görevi ve iş hazırlama görevi dışındaki tüm görevler. Yalnızca iş bir iş hazırlama göreviyle yapılandırılırsa kullanılabilir. | C:\user\tasks\workitems\jobprepreleasesamplejob\job-1\jobpreparation\wd |
| AZ_BATCH_MASTER_NODE            | [Çok örnekli][multi_instance] bir görevin birincil görevinin çalıştığı işlem düğümünün IP adresi ve bağlantı noktası. | Çok örnekli birincil ve alt görevler. | `10.0.0.4:6000` |
| AZ_BATCH_NODE_ID                | Görevin atandığı düğümün kimliği. | Tüm görevler. | tvm-121923576_3-20160919t172711z |
| AZ_BATCH_NODE_IS_DEDICATED      | Eğer, `true`geçerli düğüm adanmış bir düğüm. Düşük `false`öncelikli bir [düğümse.](batch-low-pri-vms.md) | Tüm görevler. | `true` |
| AZ_BATCH_NODE_LIST              | Biçimde `nodeIP;nodeIP` [çok örnekli bir göreve][multi_instance] ayrılan düğümlerin listesi. | Çok örnekli birincil ve alt görevler. | `10.0.0.4;10.0.0.5` |
| AZ_BATCH_NODE_MOUNTS_DIR        | Düğüm düzeyi [dosya sisteminin](virtual-file-mount.md) tam yolu, tüm montaj dizinlerinin bulunduğu konuma monte edin. Windows dosya paylaşımları bir sürücü harfi kullanır, bu nedenle Windows için montaj sürücüsü aygıtların ve sürücülerin bir parçasıdır.  |  Başlatma görevi de dahil olmak üzere tüm görevler, kullanıcının monte edilen dizini için montaj izinlerinden haberdar olduğu göz önüne alındığında, kullanıcıya erişebilir. | Örneğin Ubuntu'da konum:`/mnt/batch/tasks/fsmounts` |
| AZ_BATCH_NODE_ROOT_DIR          | Düğümdeki tüm [Toplu iş dizilişlerinin][files_dirs] kökünün tam yolu. | Tüm görevler. | C:\user\tasks |
| AZ_BATCH_NODE_SHARED_DIR        | Düğümüzerinde paylaşılan [dizinin][files_dirs] tam yolu. Düğümüzerinde çalıştırılabilen tüm görevler, bu dizine okuma/yazma erişimine sahiptir. Diğer düğümlerde çalıştırılabilen görevlerin bu dizine uzaktan erişimi yoktur (bu bir "paylaşılan" ağ dizini değildir). | Tüm görevler. | C:\user\tasks\paylaşılan |
| AZ_BATCH_NODE_STARTUP_DIR       | Düğümdeki başlangıç [görev dizininin][files_dirs] tam yolu. | Tüm görevler. | C:\user\tasks\startup |
| AZ_BATCH_POOL_ID                | Görevin çalıştığı havuzun kimliği. | Tüm görevler. | batchpool001 |
| AZ_BATCH_TASK_DIR               | Düğümdeki görev [dizininin][files_dirs] tam yolu. Bu dizin, `stdout.txt` `stderr.txt` görev ve AZ_BATCH_TASK_WORKING_DIR içerir. | Tüm görevler. | C:\user\tasks\workitems\batchjob001\job-1\task001 |
| AZ_BATCH_TASK_ID                | Geçerli görevin kimliği. | Başlangıç görevi dışındaki tüm görevler. | görev001 |
| AZ_BATCH_TASK_SHARED_DIR | Birincil görev ve [çok örnekli görevin][multi_instance]her alt görevi için aynı olan dizin yolu. Yol, çok örnekli görevin çalıştığı her düğümde bulunur ve bu düğümde çalışan görev komutları (hem [koordinasyon komutu][coord_cmd] hem de [uygulama komutu)][app_cmd]tarafından okunabilir/yazılır. Alt görevler veya diğer düğümlerde çalıştırılabilen bir birincil görevbu dizine uzaktan erişime sahip değildir (bu "paylaşılan" ağ dizini değildir). | Çok örnekli birincil ve alt görevler. | C:\user\tasks\workitems\multiinstancesamplejob\job-1\multiinstancesampletask |
| AZ_BATCH_TASK_WORKING_DIR       | Düğüm üzerinde [çalışma dizininin][files_dirs] tam yolu. Şu anda çalışan görev bu dizine okuma/yazma erişimi vardır. | Tüm görevler. | C:\user\tasks\workitems\batchjob001\job-1\task001\wd |
| CCP_NODES                       | [Çok örnekli][multi_instance]bir göreve ayrılan düğüm başına düğüm lerin listesi ve çekirdek sayısı. Düğümler ve çekirdekler biçimde listelenir`numNodes<space>node1IP<space>node1Cores<space>`<br/>`node2IP<space>node2Cores<space> ...`, düğüm sayısının bir veya daha fazla düğüm IP adresi ve her biri için çekirdek sayısı tarafından izlendiği yer. |  Çok örnekli birincil ve alt görevler. |`2 10.0.0.4 1 10.0.0.5 1` |

[files_dirs]: https://azure.microsoft.com/documentation/articles/batch-api-basics/#files-and-directories
[multi_instance]: https://azure.microsoft.com/documentation/articles/batch-mpi/
[coord_cmd]: https://azure.microsoft.com/documentation/articles/batch-mpi/#coordination-command
[app_cmd]: https://azure.microsoft.com/documentation/articles/batch-mpi/#application-command
