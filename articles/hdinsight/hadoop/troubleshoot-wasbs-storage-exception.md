---
title: Erişilen hesap Azure HDInsight'taki http hatasını desteklemiyor
description: Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 02/06/2020
ms.openlocfilehash: 14c43e4557275d6a425127acfee7495f68d1d354
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77165559"
---
# <a name="the-account-being-accessed-does-not-support-http-error-in-azure-hdinsight"></a>Erişilen hesap Azure HDInsight'taki http hatasını desteklemiyor

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Aşağıdaki hata iletisi alınır:

```
com.microsoft.azure.storage.StorageException: The account being accessed does not support http.
```

## <a name="cause"></a>Nedeni

Hata iletisinin alınmasının birden çok nedeni vardır:

* Depolama hesabı [güvenli aktarım](../../storage/common/storage-require-secure-transfer.md) etkin ve yanlış [URI düzeni](../hdinsight-hadoop-linux-information.md#URI-and-scheme) kullanılıyor.

* Güvenli aktarım *devre dışı*bırakılmış bir depolama hesabıyla bir küme oluşturuldu. Daha sonra, depolama hesabında güvenli aktarım etkinleştirildi.

## <a name="resolution"></a>Çözüm

Azure Depolama veya Veri Gölü Depolama Gen2 için güvenli `wasbs://` aktarım etkinleştirilirse, URI sırasıyla veya `abfss://`(sırasıyla) olur.  Ayrıca bakınız, [güvenli aktarım.](../../storage/common/storage-require-secure-transfer.md)

Yeni kümeler için, zaten istenen güvenli aktarım ayarına sahip bir depolama hesabı kullanın. Varolan bir küme tarafından kullanılmakta olan bir depolama hesabının güvenli aktarım ayarını değiştirmeyin.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
