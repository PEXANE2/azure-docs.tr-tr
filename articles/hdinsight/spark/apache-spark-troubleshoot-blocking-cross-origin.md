---
title: Jupyter 404 hatası-"çapraz kaynak API 'SI engelleniyor"-Azure HDInsight
description: Azure HDInsight 'ta "çapraz kaynak API 'YI engelleme" nedeniyle Jupyter sunucusu 404 "bulunamadı" hatası
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: e241657186582955d21981f7dfe18856724aa692
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75894423"
---
# <a name="scenario-jupyter-server-404-not-found-error-due-to-blocking-cross-origin-api-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta "çapraz kaynak API 'YI engelleme" nedeniyle Jupyter sunucusu 404 "bulunamadı" hatası

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

HDInsight 'ta Jupyter hizmetine eriştiğinizde, "bulunamadı" diyen bir hata kutusu görürsünüz. Jupyter günlüklerini denetederseniz şöyle bir şey göreceksiniz:

```log
[W 2018-08-21 17:43:33.352 NotebookApp] 404 PUT /api/contents/PySpark/notebook.ipynb (10.16.0.144) 4504.03ms referer=https://pnhr01hdi-corpdir.msappproxy.net/jupyter/notebooks/PySpark/notebook.ipynb
Blocking Cross Origin API request.  
Origin: https://xxx.xxx.xxx, Host: pnhr01.j101qxjrl4zebmhb0vmhg044xe.ax.internal.cloudapp.net:8001
```

Ayrıca, jupi günlüğündeki "Origin" alanında bir IP adresi görebilirsiniz.

## <a name="cause"></a>Nedeni

Bu hataya birkaç şey neden olabilir:

- Kümeye erişimi kısıtlayan ağ güvenlik grubu (NSG) kurallarını yapılandırdıysanız. NSG kurallarıyla erişimi kısıtlamak, Apache ambarı ve diğer hizmetlere küme adı yerine IP adresini kullanarak doğrudan erişmenize izin verir. Ancak, Jupyter 'a erişirken, 404 "bulunamadı" hatasını görebilirsiniz.

- HDInsight ağ geçidinizi standart dışında özelleştirilmiş bir DNS adı verildiyse `xxx.azurehdinsight.net` .

## <a name="resolution"></a>Çözüm

1. Bu iki yerde jupyter.py dosyalarını değiştirin:

    ```bash
    /var/lib/ambari-server/resources/common-services/JUPYTER/1.0.0/package/scripts/jupyter.py
    /var/lib/ambari-agent/cache/common-services/JUPYTER/1.0.0/package/scripts/jupyter.py
    ```

1. Şöyle olan satırı bulun ve şu `NotebookApp.allow_origin='\"https://{2}.{3}\"'` şekilde değiştirin: `NotebookApp.allow_origin='\"*\"'` .

1. Jupyıter hizmetini ambarı 'ndan yeniden başlatın.

1. `ps aux | grep jupyter`Komut istemine yazmak, herhangi BIR URL 'nin kendisine bağlanmasına izin verdiğini göstermelidir.

Bu, zaten yaptığımız ayardan daha az güvenlidir. Ancak, bu kümeye erişimin kısıtlandığı ve dış sunucudan bir NSG olduğu için kümeye bağlanmasına izin verilen varsayılır.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport)Azure Community 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
