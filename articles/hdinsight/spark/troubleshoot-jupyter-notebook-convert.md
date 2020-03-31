---
title: Azure HDInsight'ta Jupyter dizüstü bilgisayarı oluşturulamıyor
description: Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımlarını ve sorunlariçin olası çözümleri açıklar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 02/11/2020
ms.openlocfilehash: 61e7cd8d37108b8f4eea88c4f6b6b2a8cdbfd605
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77186794"
---
# <a name="unable-to-create-jupyter-notebook-in-azure-hdinsight"></a>Azure HDInsight'ta Jupyter dizüstü bilgisayarı oluşturulamıyor

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Bir Jupyter not defteri başlatırken, aşağıdakileri içeren bir hata iletisi alırsınız:

```error
Cannot convert notebook to v5 because that version doesn't exist
```

## <a name="cause"></a>Nedeni

Bir sürüm uyuşmazlığı.

## <a name="resolution"></a>Çözüm

1. Kümenize bağlanmak için [ssh komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME'yi kümenizin adıyla değiştirerek aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Aşağıdaki `_version.py` komutu çalıştırarak açın:

    ```bash
    sudo nano /usr/bin/anaconda/lib/python2.7/site-packages/nbformat/_version.py
    ```

1. Değiştirilen satırAşağıdaki gibi görünmesi için **5** ile **4** arasında değiştirin:

    ```python
    version_info = (4, 0, 3)
    ```

    **Ctrl + X ,** **Y**, **Enter**gir girerek değişiklikleri kaydet .

1. Bir web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net/#/main/services/JUPYTER`kümenizin adı nerede' `CLUSTERNAME` ye gidin.

1. Jupyter servisini yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
