---
title: Birden çok HDInsight kümesi & bir Azure Veri Gölü Depolama hesabı
description: Tek bir Veri Gölü Depolama hesabıyla birden fazla HDInsight kümesini nasıl kullanacağınızı öğrenin
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/18/2019
ms.openlocfilehash: cc67acca11e7e0f24dc0597dcd19672a38a7bf28
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75495752"
---
# <a name="use-multiple-hdinsight-clusters-with-an-azure-data-lake-storage-account"></a>Azure Veri Gölü Depolama hesabıyla birden çok HDInsight kümesi kullanma

HDInsight sürüm 3.5 ile başlayarak, varsayılan dosya sistemi olarak Azure Veri Gölü Depolama hesaplarıyla HDInsight kümeleri oluşturabilirsiniz.
Veri Gölü Depolama yalnızca büyük miktarda veri barındırmak için ideal kılan sınırsız depolama destekler; aynı zamanda tek bir Veri Gölü Depolama Hesabı paylaşan birden çok HDInsight kümeleri barındırma için. Depolama alanı olarak Veri Gölü Depolama'nın yer verdiği bir HDInsight kümesinin nasıl oluşturulacağına ilişkin talimatlar için [Bkz. Quickstart: HDInsight'ta kümeleri ayarlayın.](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)

Bu makalede, birden çok **etkin** HDInsight kümeleri arasında kullanılabilecek tek ve paylaşılan bir Veri Gölü Depolama Hesabı ayarlamak için Veri Gölü Depolama yöneticisine öneriler sağlar. Bu öneriler, paylaşılan Bir Veri Gölü Depolama hesabında birden çok güvenli ve güvenli olmayan Apache Hadoop kümelerinin barındırılmaması için geçerlidir.

## <a name="data-lake-storage-file-and-folder-level-acls"></a>Veri Gölü Depolama dosyası ve klasör düzeyi Aç'ları

Bu makalenin geri kalanı, Azure Veri Gölü Depolama'da [Access denetiminde](../data-lake-store/data-lake-store-access-control.md)ayrıntılı olarak açıklanan Azure Veri Gölü Depolama'da dosya ve klasör düzeyinde KISA'lar hakkında iyi bir bilgiye sahip olduğunuzu varsayar.

## <a name="data-lake-storage-setup-for-multiple-hdinsight-clusters"></a>Birden çok HDInsight kümeleri için Veri Gölü Depolama kurulumu

Veri Gölü Depolama hesabıyla birden çok HDInsight kümesini kullanma önerilerini açıklamak için iki düzeyli klasör hiyerarşisi ele alalım. Klasör yapısı **/kümeler/finans**içeren bir Veri Gölü Depolama hesabınız olduğunu düşünün. Bu yapı ile Finans kuruluşunun gerektirdiği tüm kümeler depolama yeri olarak /kümeleri/finansı kullanabilir. Gelecekte, başka bir kuruluş, pazarlama demek, aynı Veri Gölü Depolama hesabı kullanarak HDInsight kümeleri oluşturmak istiyorsa, onlar / kümeleri / pazarlama oluşturabilirsiniz. Şimdilik, sadece **/ kümeler / finans**kullanalım .

Bu klasör yapısının HDInsight kümeleri tarafından etkin bir şekilde kullanılabilmesi için, Veri Gölü Depolama yöneticisinin tabloda açıklandığı gibi uygun izinleri ataması gerekir. Tabloda gösterilen izinler Varsayılan ALA'lara değil, Access-ACL'lere karşılık gelir.

|Klasör  |İzinler  |Sahip olan kullanıcı  |Sahip olan grup  | Adlandırılmış kullanıcı | Adlandırılmış kullanıcı izinleri | Adlandırılmış grup | Adlandırılmış grup izinleri |
|---------|---------|---------|---------|---------|---------|---------|---------|
|/ | rwxr-x--x  |yönetici |yönetici  |Hizmet sorumlusu |--x  |FINGRP   |r-x         |
|/kümeler | rwxr-x--x |yönetici |yönetici |Hizmet sorumlusu |--x  |FINGRP |r-x         |
|/kümeler/finans | rwxr-x--t |yönetici |FINGRP  |Hizmet sorumlusu |Rwx  |-  |-     |

Masada,

- **yönetici,** Veri Gölü Depolama hesabının yaratıcısı ve yöneticisidir.
- **Hizmet sorumlusu,** hesapla ilişkili Azure Etkin Dizini (AAD) hizmet yöneticisidir.
- **FINGRP,** AAD'de oluşturulan ve Finans kuruluşundan kullanıcıları içeren bir kullanıcı grubudur.

Bir AAD uygulamasının (aynı zamanda Bir Hizmet Sorumlusu oluşturur) nasıl oluşturulacağına ilişkin yönergeler için [bkz.](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) AAD'de bir kullanıcı grubu oluşturma yla ilgili talimatlar için Azure [Etkin Dizin'deki grupları yönetme'ye](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)bakın.

Göz önünde bulundurulması gereken bazı önemli noktalar.

- Kümeler için depolama hesabını kullanmadan **önce** iki düzey klasör yapısı **(/clusters/finance/**) Oluşturulmalı ve Veri Gölü Depolama yöneticisi tarafından uygun izinlerle sağlanmalıdır. Bu yapı kümeler oluşturulurken otomatik olarak oluşturulmaz.
- Yukarıdaki örnekte, sahip olan **/kümeler/finans** grubunu **FINGRP** olarak ayarlamave kökten başlayarak FINGRP'ye **r-x** erişimine izin verme önerilir. Bu, FINGRP üyelerinin kökten başlayarak klasör yapısında gezinmesini sağlar.
- Farklı AAD Hizmet Anaparaları **/clusters/finance**altında kümeler oluşturabildiği durumlarda, yapışkan bit **(finans** klasöründe ayarlandığında) bir Hizmet Sorumlusu tarafından oluşturulan klasörlerin diğeri tarafından silinmesini sağlar.
- Klasör yapısı ve izinler yerleştirildikten sonra, HDInsight küme oluşturma işlemi **/clusters/finance/** altında kümeye özgü bir depolama konumu oluşturur. Örneğin, fincluster01 adında bir kümenin depolama sı **/clusters/finance/fincluster01**olabilir. HDInsight kümesi tarafından oluşturulan klasörlerin sahipliği ve izinleri buradaki tabloda gösterilmiştir.

    |Klasör  |İzinler  |Sahip olan kullanıcı  |Sahip olan grup  | Adlandırılmış kullanıcı | Adlandırılmış kullanıcı izinleri | Adlandırılmış grup | Adlandırılmış grup izinleri |
    |---------|---------|---------|---------|---------|---------|---------|---------|
    |/kümeler/finanace/ fincluster01 | rwxr-x---  |Hizmet Sorumlusu |FINGRP  |- |-  |-   |-  |

## <a name="recommendations-for-job-input-and-output-data"></a>İş girdisi ve çıktı verileri için öneriler

Bir işe giriş verilerinin ve bir işin çıktılarının **/kümeler**dışındaki bir klasörde depolanmasını öneririz. Bu, kümeye özgü klasör bazı depolama alanını geri almak için silinse bile, iş girişlerinin ve çıktılarının ileride kullanıma hazır olmasını sağlar. Böyle bir durumda, iş giriş ve çıktılarını depolamak için klasör hiyerarşisinin Hizmet Sorumlusu için uygun erişim düzeyine izin verdiğinden emin olun.

## <a name="limit-on-clusters-sharing-a-single-storage-account"></a>Tek bir depolama hesabını paylaşan kümelerde sınırlama

Tek bir Veri Gölü Depolama hesabını paylaşabilen küme sayısının sınırı, bu kümelerde çalıştırılan iş yüküne bağlıdır. Depolama hesabını paylaşan kümelerde çok fazla küme veya çok ağır iş yüklerinin olması depolama hesabı girişinin/çıkışlarının daraltlanmasına neden olabilir.

## <a name="support-for-default-acls"></a>Varsayılan ALIK'ler için destek

Adlandırılmış kullanıcı erişimine sahip bir Hizmet Sorumlusu oluştururken (yukarıdaki tabloda gösterildiği gibi), adı geçen kullanıcıyı varsayılan ACL ile **eklememenizi** öneririz. Varsayılan ALA'lar kullanarak adlandırılmış kullanıcı erişiminin sağlanması, sahibi kullanıcı, sahip-grup ve diğerleri için 770 izin atanmasıile sonuçlanır. 770'in bu varsayılan değeri, sahibi olan kullanıcıdan (7) veya sahip grubu (7) izinleri almasa da, diğerleri için tüm izinleri (0) alır. Bu, [bilinen sorunlar ve geçici çözümler](#known-issues-and-workarounds) bölümünde ayrıntılı olarak tartışılan belirli bir kullanım örneği ile bilinen bir sorunla sonuçlanır.

## <a name="known-issues-and-workarounds"></a>Bilinen sorunlar ve geçici çözüm

Bu bölümde, HDInsight'ı Veri Gölü Depolama ile kullanmak için bilinen sorunlar ve bunların geçici geçici çözümlerini listeleyin.

### <a name="publicly-visible-localized-apache-hadoop-yarn-resources"></a>Kamuya açık yerelleştirilmiş Apache Hadoop İplik kaynakları

Yeni bir Azure Veri Gölü Depolama hesabı oluşturulduğunda, kök dizini otomatik olarak Access-ACL izin bitleri 770 olarak ayarlanır. Kök klasörün sahibi kullanıcısı, hesabı oluşturan kullanıcıya (Veri Gölü Depolama yöneticisi) ayarlanır ve sahip olunan grup hesabı oluşturan kullanıcının birincil grubuna ayarlanır. "Diğerleri" için erişim sağlanmaz.

Bu [ayarların İPN 247'de](https://hwxmonarch.atlassian.net/browse/YARN-247)yakalanan belirli bir HDInsight kullanım örneğini etkilediği bilinmektedir. İş gönderimleri buna benzer bir hata iletisi ile başarısız olabilir:

    Resource XXXX is not publicly accessible and as such cannot be part of the public cache.

Daha önce bağlantılı İPLik JIRA'da belirtildiği gibi, kamu kaynaklarını yerelleştirirken, yerelleştirici, istenen tüm kaynakların uzaktan dosya sisteminde izinlerini kontrol ederek gerçekten herkese açık olduğunu doğrular. Bu koşula uymayan herhangi bir LocalResource yerelleştirme için reddedilir. İzinler için onay, "diğerleri" için dosyaya okuma erişimi içerir. Azure Veri Gölü kök klasör düzeyinde "diğerlerine" tüm erişimi reddettiğinden, bu senaryo Azure Veri Gölü'nde HDInsight kümelerini barındırırken kutudan güncel değildir.

#### <a name="workaround"></a>Geçici çözüm

Yukarıdaki tabloda gösterildiği gibi, hiyerarşi aracılığıyla **başkaları** için **/** okuma-yürütme izinlerini ayarlayın, örneğin , **/kümeler** ve **/kümeler/finans.**

## <a name="see-also"></a>Ayrıca bkz.

- [Hızlı başlangıç: HDInsight'ta kümeleri ayarlama](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)
- [Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi kullanma](hdinsight-hadoop-use-data-lake-storage-gen2.md)
