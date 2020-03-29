---
title: Ağ trafiği desenlerini açık kaynak araçlarıyla görselleştirin
titleSuffix: Azure Network Watcher
description: Bu sayfa, VM'lerinize gelen ve gelen trafik modellerini görselleştirmek için Capanalysis ile Network Watcher paket yakalamanın nasıl kullanılacağını açıklar.
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: f36db28b58cd57b6407019b378a82632aa6c6228
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840664"
---
# <a name="visualize-network-traffic-patterns-to-and-from-your-vms-using-open-source-tools"></a>Açık kaynak araçlarını kullanarak VM'lerinize ağ trafiği desenlerini görselleştirin

Paket yakalamalar, ağ adli tıp ve derin paket denetimi gerçekleştirmenize olanak tanıyan ağ verileri içerir. Ağınız hakkında bilgi edinmek için paket yakalamaları analiz etmek için kullanabileceğiniz birçok açık kaynak aracı vardır. Bu araçlardan biri, açık kaynak kodlu paket yakalama görselleştirme aracı olan CapAnalysis'tir. Paket yakalama verilerini görselleştirmek, ağınızdaki desenler ve anormallikler hakkında hızlı bir şekilde bilgi edinmenin değerli bir yoludur. Görselleştirmeler aynı zamanda bu tür içgörüleri kolayca tüketilebilir bir şekilde paylaşmanın bir aracıdır.

Azure Ağ İzleyicisi, ağınızda paket yakalama gerçekleştirmenize izin vererek size veri yakalama olanağı sağlar. Bu makalede, Network Watcher ile CapAnalysis kullanarak paket yakalamalar görselleştirmek ve anlayışlar elde etmek için nasıl bir yürüyüş sağlar.

## <a name="scenario"></a>Senaryo

Azure'da bir VM'de dağıtılan basit bir web uygulamanız var, akış desenlerini ve olası anormallikleri hızla tanımlamak için ağ trafiğini görselleştirmek için açık kaynak araçlarını kullanmak istiyorsunuz. Ağ İzleyicisi ile ağ ortamınızın paket yakalanmasını elde edebilir ve doğrudan depolama hesabınızda depolayabilirsiniz. CapAnalysis daha sonra paket yakalamayı doğrudan depolama lekesinden alabilir ve içeriğini görselleştirebilir.

![senaryo][1]

## <a name="steps"></a>Adımlar

### <a name="install-capanalysis"></a>CapAnalysis'i Yükleyin

CapAnalysis'i sanal bir makineye yüklemek için resmi https://www.capanalysis.net/ca/how-to-install-capanalysistalimatlara buradan bakabilirsiniz.
CapAnalysis'e uzaktan erişmek için, yeni bir gelen güvenlik kuralı ekleyerek VM'nizde 9877 bağlantı noktasını açmanız gerekir. Ağ Güvenlik Gruplarında kural oluşturma hakkında daha fazla şey [için, varolan bir NSG'de oluştur kurallarına](../virtual-network/manage-network-security-group.md#create-a-security-rule)bakın. Kural başarıyla eklendikten sonra, CapAnalysis'e`http://<PublicIP>:9877`

### <a name="use-azure-network-watcher-to-start-a-packet-capture-session"></a>Paket yakalama oturumu başlatmak için Azure Ağ İzleyicisi'ni kullanma

Ağ İzleyicisi, sanal bir makineye girip çıkan trafiği izlemek için paketleri yakalamanızı sağlar. Paket yakalama oturumu başlatmak için [Ağ İzleyicisi ile paket yakalamaları yönet'teki](network-watcher-packet-capture-manage-portal.md) yönergelere başvurabilirsiniz. Paket yakalama, CapAnalysis tarafından erişilen bir depolama blob'unda saklanabilir.

### <a name="upload-a-packet-capture-to-capanalysis"></a>CapAnalysis'e paket yakalama yükleme
"URL'den Alma" sekmesini kullanarak ve paket yakalamanın depolandığı depolama blob'una bağlantı sağlayarak ağ izleyicisi tarafından alınan bir paket yakalamayı doğrudan yükleyebilirsiniz.

CapAnalysis'e bağlantı sağlarken, depolama blob URL'sine bir SAS belirteci eklediğinizden emin olun.  Bunu yapmak için, depolama hesabından Paylaşılan erişim imzasına gidin, izin verilen izinleri belirleyin ve belirteç oluşturmak için SAS oluştur düğmesine basın. Daha sonra paket yakalama depolama blob URL'si sas belirteci ekleyebilirsiniz.

Ortaya çıkan URL aşağıdaki URL'ye benzer bir şey görünecektir:http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere


### <a name="analyzing-packet-captures"></a>Paket yakalamaları analiz etme

CapAnalysis, her biri farklı bir perspektiften analiz sağlayan paket yakalamanızı görselleştirmek için çeşitli seçenekler sunar. Bu görsel özetlerle, ağ trafiği eğilimlerinizi anlayabilir ve olağandışı etkinlikleri hızlı bir şekilde fark edebilirsiniz. Bu özelliklerden bazıları aşağıdaki listede gösterilmiştir:

1. Akış Tabloları

    Bu tablo, paket verilerindeki akışların listesini, akışlarla ilişkili zaman damgasını ve akışla ilişkili çeşitli protokollerin yanı sıra kaynak ve hedef IP'yi verir.

    ![capanalysis akış sayfası][5]

1. Protokole Genel Bakış

    Bu bölme, ağ trafiğinin çeşitli protokoller ve coğrafyalar üzerindeki dağılımını hızlı bir şekilde görmenizi sağlar.

    ![capanalysis protokolüne genel bakış][6]

1. İstatistikler

    Bu bölme, ağ trafiği istatistiklerini görüntülemenize olanak tanır – kaynak ve hedef IP'lerden gönderilen ve alınan baytlar, kaynak ve hedef IP'lerin her biri için akışlar, çeşitli akışlar için kullanılan protokol ve akış süresi.

    ![kapanaliz istatistikleri][7]

1. Jeoharita

    Bu bölme, her ülkeden/bölgeden gelen trafik hacmine kadar ölçeklendiren renklerle ağ trafiğinizin bir harita görünümünü sağlar. Söz konusu ülkedeki/bölgedeki IP'lerden gönderilen ve alınan verilerin oranı gibi ek akış istatistiklerini görüntülemek için vurgulanan ülkeleri/bölgeleri seçebilirsiniz.

    ![coğrafi harita][8]

1. Filtreler

    CapAnalysis, belirli paketlerin hızlı analizi için bir dizi filtre sağlar. Örneğin, bu trafik alt kümesi hakkında belirli öngörüler elde etmek için verileri protokole göre filtrelemeyi seçebilirsiniz.

    ![filtreler][11]

    CapAnalysis'in tüm yetenekleri hakkında daha fazla bilgi edinmek için ziyaret edin. [https://www.capanalysis.net/ca/#about](https://www.capanalysis.net/ca/#about)

## <a name="conclusion"></a>Sonuç

Network Watcher'ın paket yakalama özelliği, ağ adli tıp gerçekleştirmek için gerekli verileri yakalamanızı ve ağ trafiğinizi daha iyi anlamanızı sağlar. Bu senaryoda, Network Watcher'dan gelen paket yakalamaların açık kaynak görüntüleme araçlarıyla nasıl kolayca entegre edilebildiğini gösterdik. Paket yakalamaları görselleştirmek için CapAnalysis gibi açık kaynak araçlarını kullanarak, derin paket denetimi yapabilir ve ağ trafiğinizdeki eğilimleri hızla belirleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

NSG akış günlükleri hakkında daha fazla bilgi edinmek için [NSG Akış günlüklerini](network-watcher-nsg-flow-logging-overview.md) ziyaret edin

Power BI ile [Visualize NSG akışları günlüklerini](network-watcher-visualize-nsg-flow-logs-power-bi.md) ziyaret ederek NSG akış günlüklerinizi Power BI ile nasıl görselleştirebilirsiniz öğrenin
<!--Image references-->

[1]: ./media/network-watcher-using-open-source-tools/figure1.png
[2]: ./media/network-watcher-using-open-source-tools/figure2.png
[3]: ./media/network-watcher-using-open-source-tools/figure3.png
[4]: ./media/network-watcher-using-open-source-tools/figure4.png
[5]: ./media/network-watcher-using-open-source-tools/figure5.png
[6]: ./media/network-watcher-using-open-source-tools/figure6.png
[7]: ./media/network-watcher-using-open-source-tools/figure7.png
[8]: ./media/network-watcher-using-open-source-tools/figure8.png
[9]: ./media/network-watcher-using-open-source-tools/figure9.png
[10]: ./media/network-watcher-using-open-source-tools/figure10.png
[11]: ./media/network-watcher-using-open-source-tools/figure11.png
