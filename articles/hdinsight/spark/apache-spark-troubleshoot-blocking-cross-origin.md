---
title: Jupyter 404 hatası - "Çapraz Başlangıç API'nin engellenmesi" - Azure HDInsight
description: Jupyter server 404 Azure HDInsight'ta "Çapraz Başlangıç API'sini Engelleme" nedeniyle "Bulunamadı" hatası
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: e241657186582955d21981f7dfe18856724aa692
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894423"
---
# <a name="scenario-jupyter-server-404-not-found-error-due-to-blocking-cross-origin-api-in-azure-hdinsight"></a>Senaryo: Azure HDInsight'ta "Çapraz Başlangıç API'sini Engelleme" nedeniyle Jupyter server 404 "Bulunamadı" hatası

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorun giderme adımları ve sorunların olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

HDInsight'ta Jupyter hizmetine erişdiğinizde, "Bulunamadı" yazan bir hata kutusu görürsünüz. Jupyter günlüklerini kontrol ederseniz, böyle bir şey göreceksiniz:

```log
[W 2018-08-21 17:43:33.352 NotebookApp] 404 PUT /api/contents/PySpark/notebook.ipynb (10.16.0.144) 4504.03ms referer=https://pnhr01hdi-corpdir.msappproxy.net/jupyter/notebooks/PySpark/notebook.ipynb
Blocking Cross Origin API request.  
Origin: https://xxx.xxx.xxx, Host: pnhr01.j101qxjrl4zebmhb0vmhg044xe.ax.internal.cloudapp.net:8001
```

Ayrıca Jupyter günlüğünde "Origin" alanında bir IP adresi görebilirsiniz.

## <a name="cause"></a>Nedeni

Bu hata birkaç şey neden olabilir:

- Ağ Güvenlik Grubu (NSG) Kurallarını kümeye erişimi kısıtlayabilmek için yapılandırıldıysanız. NSG kurallarıyla erişimi kısıtlamak, küme adı yerine IP adresini kullanarak Apache Ambari'ye ve diğer hizmetlere doğrudan erişmenize olanak sağlar. Ancak, Jupyter erişirken, bir 404 "Bulunamadı" hata görebiliyordu.

- HDInsight ağ geçidinize standart `xxx.azurehdinsight.net`tan başka özelleştirilmiş bir DNS adı verdiyseniz.

## <a name="resolution"></a>Çözüm

1. Bu iki yerde jupyter.py dosyalarını değiştirin:

    ```bash
    /var/lib/ambari-server/resources/common-services/JUPYTER/1.0.0/package/scripts/jupyter.py
    /var/lib/ambari-agent/cache/common-services/JUPYTER/1.0.0/package/scripts/jupyter.py
    ```

1. Yazan satırı bulun: `NotebookApp.allow_origin='\"https://{2}.{3}\"'` Ve değiştirin: `NotebookApp.allow_origin='\"*\"'`.

1. Ambari'den Jupyter servisini yeniden başlatın.

1. Komut `ps aux | grep jupyter` isteminde yazmak, herhangi bir URL'nin ona bağlanmasına izin verdiğini göstermelidir.

Bu, zaten yaptığımız ayarda daha az güvenli. Ancak kümeye erişimin kısıtlandığı ve nsg'nin yerinde olduğu için dışarıdan birinin kümeye bağlanmasına izin verildiği varsayılır.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
