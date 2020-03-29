---
title: Azure HDInsight uygulamalarını yayımla
description: Bir HDInsight uygulamasını nasıl oluşturup Azure Marketi'nde yayınlayacağınızı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: hrasheed
ms.openlocfilehash: e64bf253a73df3a2f8170109dc1dfb9a59613733
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "64685319"
---
# <a name="publish-an-hdinsight-application-in-the-azure-marketplace"></a>Azure Marketi'nde bir HDInsight uygulaması yayınlama
Bir Azure HDInsight uygulamasını Linux tabanlı BIR HDInsight kümesine yükleyebilirsiniz. Bu makalede, Azure Marketi'nde bir HDInsight uygulamasını nasıl yayınlayacağınızı öğrenin. Azure Marketi'nde yayımlama hakkında genel bilgi [için](../marketplace/marketplace-publishers-guide.md)bkz.

HDInsight uygulamaları *Kendi Lisansını Getir (BYOL)* modelini kullanır. BYOL senaryosunda, uygulama sağlayıcısı uygulamanın uygulama kullanıcılarına lisanslanmasından sorumludur. Uygulama kullanıcıları yalnızca HDInsight kümesi ve kümenin VM'leri ve düğümleri gibi oluşturdukları Azure kaynakları için ücretlendirilir. Şu anda, uygulamanın kendisi için faturalandırma Azure'da gerçekleşmez.

Daha fazla bilgi için şu HDInsight uygulamasıyla ilgili makalelere bakın:

* [HDInsight uygulamalarını yükleyin.](hdinsight-apps-install-applications.md) Kümelerinize nasıl bir HDInsight uygulaması yükleyin öğrenin.
* [Özel HDInsight uygulamalarını yükleyin.](hdinsight-apps-install-custom-applications.md) Özel HDInsight uygulamalarını nasıl yükleyip test edebilirsiniz öğrenin.

## <a name="prerequisites"></a>Ön koşullar
Özel uygulamanızı Market'e göndermek için önce [özel uygulamanızı oluşturun ve test edin.](hdinsight-apps-install-custom-applications.md)

Geliştirici hesabınızı da kaydetmeniz gerekir. Daha fazla bilgi için [bkz.](../marketplace/marketplace-publishers-guide.md) [Create a Microsoft Developer account](../marketplace/marketplace-publishers-guide.md)

## <a name="define-the-application"></a>Uygulamayı tanımlama
Market'teki uygulamaları yayımlamada iki adım yer almaktadır. İlk olarak, *createUiDef.json dosyayı tanımlayın.* createUiDef.json dosyası, uygulamanızın hangi kümelerle uyumlu olduğunu gösterir. Ardından, şablonu Azure portalından yayımlayın. Burada bir örnek createUiDef.json dosyası:

```json
{
    "handler": "Microsoft.HDInsight",
    "version": "0.0.1-preview",
    "clusterFilters": {
        "types": ["Hadoop", "HBase", "Storm", "Spark"],
        "versions": ["3.6"]
    }
}
```

| Alan | Açıklama | Olası değerler |
| --- | --- | --- |
| types |Uygulamanın uyumlu olduğu küme türleri. |Hadoop, HBase, Storm, Spark (veya bunların herhangi bir kombinasyonu) |
| versions |Uygulamanın uyumlu olduğu HDInsight küme türleri. |3.4 |

## <a name="application-installation-script"></a>Uygulama yükleme komut dosyası
Bir uygulama bir kümeye yüklendiğinde (varolan bir kümeye veya yeni bir kümeye) bir kenar düğümü oluşturulur. Uygulama yükleme komut dosyası kenar düğümünde çalışır.

  > [!IMPORTANT]  
  > Uygulama yükleme komut dosyasının adı belirli bir küme için benzersiz olmalıdır. Komut dosyası adı aşağıdaki biçime sahip olmalıdır:
  > 
  > "isim": "[concat('hue-install-v0','-' uniquestring('applicationName')]"
  > 
  > Komut dosyası adı üç bölümden oluşur:
  > 
  > * Uygulama adını veya uygulamayla ilgili bir adı içermesi gereken bir komut dosyası adı öneki.
  > * Okunabilirlik için bir tire.
  > * Parametre olarak uygulama adı ile benzersiz bir dize işlevi.
  > 
  > Kalıcı komut dosyası eylem listesinde, önceki örnek **hue-install-v0-4wkahss55hlas**olarak görüntülenir. Örnek bir [JSON yükü](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json)ne bakın.
  > 

Yükleme komut dosyası aşağıdaki özelliklere sahip olmalıdır:
* Senaryo idempotent. Komut dosyasına yapılan birden çok arama aynı sonucu üretir.
* Komut dosyası düzgün bir şekilde sürülür. Değişiklikleri yükseltirken veya test ederken komut dosyası için farklı bir konum kullanın. Bu, uygulamayı yükleyen müşterilerin güncelleştirmelerinizden veya sınamanızdan etkilenmemesini sağlar. 
* Komut dosyası her noktada yeterli günlüğe kaydetmeye sahiptir. Genellikle, komut dosyası günlükleri hata ayıklama uygulama yükleme sorunları için tek yoldur.
* Dış hizmetlere veya kaynaklara yapılan aramalar, yüklemenin geçici ağ sorunlarından etkilenmemesi için yeterli yeniden denemeye sahiptir.
* Komut dosyanız düğümlerde hizmetleri başlatırsa, düğüm yeniden başlatılırsa hizmetler izlenir ve otomatik olarak başlatılmak üzere yapılandırılır.

## <a name="package-the-application"></a>Uygulamayı paketle
HDInsight uygulamanızı yüklemek için gereken tüm dosyaları içeren bir .zip dosyası oluşturun. Uygulamayı yayımlamak için .zip dosyasını kullanırsınız. .zip dosyası aşağıdaki dosyaları içerir:

* oluşturmaUiDefinition.json
* mainTemplate.json (Bir örnek için, [bkz. özel HDInsight uygulamalarını yükle](hdinsight-apps-install-custom-applications.md).)
* Gerekli tüm komut dosyaları

> [!NOTE]  
> Uygulama dosyalarını (tüm web uygulama dosyaları dahil) herkese açık herhangi bir bitiş noktasında barındırabilirsiniz.

## <a name="publish-the-application"></a>Uygulamayı yayımlama
BIR HDInsight uygulamasını yayınlamak için:

1. [Azure Yayıncılık'ta](https://publish.windowsazure.com/)oturum açın.
2. Sol menüde **Çözüm şablonları'nı**seçin.
3. Bir başlık girin ve ardından **yeni bir çözüm şablonu oluştur'u**seçin.
4. Kuruluşunuzu henüz kaydettirdiyseniz, Dev Merkezi Oluştur hesabını seçin **ve Azure programına katılın.**  Daha fazla bilgi için [bkz.](../marketplace/marketplace-publishers-guide.md)
5. **Başlamak için Bazı Topolojileri Tanımla'yı**seçin. Çözüm şablonu, tüm topolojileri için bir "ebeveyn" dir. Tek bir teklif veya çözüm şablonunda birden çok topoloji tanımlayabilirsiniz. Bir teklif evreleme itilir, tüm topolojileri ile itilir. 
6. Bir topoloji adı girin **+** ve sonra seçin.
7. Yeni bir sürüm girin **+** ve sonra seçin.
8. Uygulamayı paketlerken oluşturduğunuz .zip dosyasını yükleyin.  
9. **İstek Sertifikası'nı**seçin. Microsoft sertifika ekibi dosyaları inceler ve topolojiyi onaylar.

## <a name="next-steps"></a>Sonraki adımlar
* [HDInsight uygulamalarını](hdinsight-apps-install-applications.md) kümelerinize nasıl yükleyin öğrenin.
* [Özel HDInsight uygulamalarını](hdinsight-apps-install-custom-applications.md) nasıl yükleyip yayınlanmamış bir HDInsight uygulamasını HDInsight'a nasıl dağıtılayarak öğrenin.
* [Linux tabanlı HDInsight kümelerini özelleştirmek](hdinsight-hadoop-customize-cluster-linux.md) ve daha fazla uygulama eklemek için Script Action'ı nasıl kullanacağınızı öğrenin. 
* [Azure Kaynak Yöneticisi şablonlarını kullanarak HDInsight'ta Linux tabanlı Apache Hadoop kümelerini nasıl oluşturacağınızhakkında](hdinsight-hadoop-create-linux-clusters-arm-templates.md)bilgi edinin.
* HDInsight kümelerine erişmek, HDInsight uygulamalarını test etmek ve HDInsight uygulamalarını barındırmak için [HDInsight'ta boş bir kenar düğümlerini](hdinsight-apps-use-edge-node.md) nasıl kullanacağınızı öğrenin.

