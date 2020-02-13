---
title: Azure HDInsight 'ta Jupyter Not defteri oluşturulamıyor
description: Azure HDInsight kümeleriyle etkileşim kurarken sorunlar için sorun giderme adımlarını ve olası çözümleri açıklar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 02/11/2020
ms.openlocfilehash: 03b23d86bca10077ede43f3b4e5a6c5fa6f5dd28
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77165468"
---
# <a name="unable-to-create-jupyter-notebook-in-azure-hdinsight"></a>Azure HDInsight 'ta Jupyter Not defteri oluşturulamıyor

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Jupyter Not defterini başlatırken şunu içeren bir hata iletisi alırsınız:

```error
Cannot convert notebook to v5 because that version doesn't exist
```

## <a name="cause"></a>Nedeni

Sürüm uyumsuzluğu.

## <a name="resolution"></a>Çözüm

1. Kümenize bağlanmak için [SSH komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME öğesini kümenizin adıyla değiştirerek aşağıdaki komutu düzenleyin ve ardından şu komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Aşağıdaki komutu yürüterek `_version.py` açın:

    ```bash
    sudo nano /usr/bin/anaconda/lib/python2.7/site-packages/nbformat/_version.py
    ```

1. Değiştirilen satırın aşağıdaki gibi görünmesi için **5** ' i **4** ' e değiştirin:

    ```python
    version_info = (4, 0, 3)
    ```

    **CTRL + X**, **Y**, **ENTER**tuşlarına basarak değişiklikleri kaydedin.

1. Bir Web tarayıcısından `https://CLUSTERNAME.azurehdinsight.net`' a gidin; burada `CLUSTERNAME` kümenizin adıdır.

1. **Jupyıter** ' ı seçin ve ardından hizmeti yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) ile bağlanma-müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabı. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
