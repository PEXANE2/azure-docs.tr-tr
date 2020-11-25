---
title: Azure HDInsight 'ta küme oluşturma, DomainNotFound hatasıyla başarısız oluyor
description: Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/23/2020
ms.openlocfilehash: 79cd3b687dc8a2eb838146a6dc8a40b184a55fe6
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96005954"
---
# <a name="scenario-cluster-creation-fails-with-domainnotfound-error-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta küme oluşturma, DomainNotFound hatasıyla başarısız oluyor

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

HDI Secure (Kurumsal Güvenlik Paketi) kümesi oluşturma işlemi `DomainNotFound` hata iletisiyle başarısız oluyor.

## <a name="cause"></a>Nedeni

Yanlış DNS ayarları.

## <a name="resolution"></a>Çözüm

Etki alanına katılmış kümeler dağıtıldığında HDI, AAD DS 'de (her küme için) bir iç Kullanıcı adı ve parola oluşturur ve tüm küme düğümlerini bu etki alanına birleştirir. Etki alanına ekleme işlemi Samba araçları kullanılarak gerçekleştirilir. Aşağıdaki önkoşulların karşılandığından emin olun:

* Etki alanı adı DNS aracılığıyla çözümlenmelidir.
* Etki alanı denetleyicilerinin IP adresi, kümenin dağıtıldığı sanal ağ için DNS ayarlarında ayarlanmalıdır.
* Sanal ağa AAD DS 'nin sanal ağı eşlenirse, el ile yapılması gerekir.
* DNS ileticileri kullanıyorsanız, etki alanı adı sanal ağ içinde doğru şekilde çözümlenmelidir.
* Güvenlik ilkeleri (NSG 'ler), etki alanına katılımı engellemez.

### <a name="additional-debugging-steps"></a>Ek hata ayıklama adımları

* Aynı alt ağda bir Windows sanal makinesi dağıtın, etki alanına bir Kullanıcı adı ve parola kullanarak makineye katılarak (Bu denetim masası kullanıcı arabiriminden yapılabilir) veya

* Aynı alt ağda bir Ubuntu VM dağıtma ve makineye katma etki alanı
  * Makineye SSH
  * sudo su
  * Betiği Kullanıcı adı ve parolayla Çalıştır
  * Betik ping yapar, gerekli yapılandırma dosyalarını ve ardından etki alanını oluşturun. Başarılı olursa DNS ayarlarınız iyidir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]