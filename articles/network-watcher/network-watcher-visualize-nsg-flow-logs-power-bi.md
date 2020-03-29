---
title: Azure NSG akış günlüklerini görselleştirme - Power BI
titleSuffix: Azure Network Watcher
description: Bu sayfa, Power BI ile NSG akış günlüklerinin nasıl görselleştirilebildiğini açıklar.
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
ms.openlocfilehash: 955e13b88037aa42b59707698549b1c980720990
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840613"
---
# <a name="visualizing-network-security-group-flow-logs-with-power-bi"></a>Power BI ile Ağ Güvenlik Grubu akış günlüklerini görselleştirme

Ağ Güvenliği Grubu akış günlükleri, Ağ Güvenlik Grupları'ndaki giriş ve çıkış IP trafiği yle ilgili bilgileri görüntülemenize olanak tanır. Bu akış günlükleri, kural bazında giden ve gelen akışları gösterir, akış için geçerli nic, akış hakkında 5-tuple bilgi (Kaynak/Hedef IP, Kaynak/Hedef Bağlantı Noktası, Protokol) ve trafiğe izin verilip verilmediğini veya reddedilmişse.

Günlük dosyalarında el ile arama yaparak akış günlüğü verilerine ilişkin öngörüler elde etmek zor olabilir. Bu makalede, en son akış günlüklerinizi görselleştirmek ve anızdaki trafik hakkında bilgi edinmek için bir çözüm salıyoruz.

> [!Warning]  
> Aşağıdaki adımlar akış günlükleri sürüm 1 ile çalışır. Ayrıntılar için, [ağ güvenlik grupları için akış günlüğe giriş](network-watcher-nsg-flow-logging-overview.md)'e bakın. Aşağıdaki yönergeler, değişiklik yapılmadan günlük dosyalarının sürüm 2 ile çalışmaz.

## <a name="scenario"></a>Senaryo

Aşağıdaki senaryoda, Power BI masaüstünü NSG Akış Günlüğü verilerimiz için lavabo olarak yapılandırdığımız depolama hesabına bağlıyız. Depolama hesabımıza bağlandıktan sonra, Power BI, Ağ Güvenliği grupları tarafından günlüğe kaydedilen trafiğin görsel bir temsilini sağlamak için günlükleri indirir ve ayrıştırır.

Şablonda sağlanan görselleri kullanarak şunları inceleyebilirsiniz:

* En Çok Konuşanlar
* Yön ve kural kararına göre Zaman Serisi Akış Verileri
* Ağ Arabirimi MAC adresine göre akışlar
* NSG ve Kurala Göre Akışlar
* Hedef Bağlantı Noktasına Göre Akışlar

Sağlanan şablon, gereksinimlerinize uygun yeni veriler, görseller eklemek veya sorguları düzenlenecek şekilde değiştirebilmeniz için değiştirilebilir.

## <a name="setup"></a>Kurulum

Başlamadan önce, hesabınızdaki bir veya birden çok Ağ Güvenlik Grubunda Ağ Güvenliği Grubu Akış Günlüğe Kaydetmeözelliğini etkinleştirmiş olmalıdır. Ağ Güvenliği akış günlüklerini etkinleştirme yönergeleri için aşağıdaki makaleye bakın: [Ağ Güvenlik Grupları için akış günlüğe giriş.](network-watcher-nsg-flow-logging-overview.md)

Ayrıca, Power BI Desktop istemcisinin makinenize yüklü olması ve depolama hesabınızda bulunan günlük verilerini karşıdan yükleyip yüklemek için makinenizde yeterli boş alana sahip olması gerekir.

![Visio diyagramı][1]

### <a name="steps"></a>Adımlar

1. Power BI Masaüstü Uygulama [Ağı İzleyicipowerBI akış günlükleri şablonunda](https://aka.ms/networkwatcherpowerbiflowlogstemplate) aşağıdaki Power BI şablonu indirin ve açın
1. Gerekli Sorgu parametrelerini girin
   1. **StorageAccountName** – Yüklemek ve görselleştirmek istediğiniz NSG akış günlüklerini içeren depolama hesabının adına belirtir.
   1. **NumberOfLogFiles** – Power BI'de indirmek ve görselleştirmek istediğiniz günlük dosyalarının sayısını belirtir. Örneğin, 50 belirtilirse, en son 50 günlük dosyası. NSG akış günlüklerini bu hesaba göndermek için 2 NSG etkinleştirilmiş ve yapılandırılırsa, son 25 saatlik günlükler görüntülenebilir.

      ![güç BI ana][2]

1. Depolama hesabınız için Erişim Anahtarını girin. Azure portalında depolama hesabınıza göz atarak ve Ayarlar menüsünden **Erişim Anahtarları'nı** seçerek geçerli erişim anahtarlarını bulabilirsiniz. **Bağlan'ı** tıklatın ve sonra değişiklikleri uygulayın.

    ![erişim tuşları][3]

    ![erişim anahtarı 2][4]

4. Günlükleriniz indirilir ve ayrışdırılır ve artık önceden oluşturulmuş görselleri kullanabilirsiniz.

## <a name="understanding-the-visuals"></a>Görselleri anlama

Şablonda sağlanan NSG Akış Günlüğü verileri anlam yardımcı görseller kümesidir. Aşağıdaki resimler, verilerle doldurulduğunda panoya nasıl benzediğinde nasıl göründüğünün bir örneğini gösterir. Aşağıda her görseli daha ayrıntılı olarak inceleyeceğiz 

![powerbi][5]
 
Top Talkers görsel belirtilen süre içinde en çok bağlantı başlattıip IP'leri gösterir. Kutuların boyutu, göreli bağlantı sayısına karşılık gelir. 

![toptalkers][6]

Aşağıdaki zaman serileri grafikleri dönem boyunca akış sayısını gösterir. Üst grafik akış yönü ile bölümlere ayrılmıştır ve alt alınan kararla bölümlere ayrılmıştır (izin ver veya reddet). Bu görsel ile, zaman içinde trafik eğilimleri inceleyebilir ve herhangi bir anormal ani veya trafik veya trafik segmentasyon düşüş nokta.

![flowsoverperiod][7]

Aşağıdaki grafikler, ağ arabirimi başına akışları, üst bölümü akış yönü yle ve alt kesimin kararla segmente olduğunu gösterir. Bu bilgilerle, hangi VM'lerinizin en çok başkalarına göre iletişim kurduğu ve belirli bir VM'ye trafik izni veriliyorveya reddediliyorsa hakkında bilgi edinebilirsiniz.

![flowspernic][8]

Aşağıdaki donut tekerlek grafiği, Hedef Bağlantı Noktasına göre Akışların dökümünü gösterir. Bu bilgilerle, belirtilen süre içinde kullanılan en sık kullanılan hedef bağlantı noktalarını görüntüleyebilirsiniz.

![halka][9]

Aşağıdaki çubuk grafik, NSG ve Kural tarafından Akış gösterir. Bu bilgilerle, en fazla trafikten ve bir NSG'deki trafiğin kurala göre bozulmasından sorumlu NSG'leri görebilirsiniz.

![barchart][10]
 
Aşağıdaki bilgi grafikleri günlüklerde bulunan NSG'ler, dönem içinde yakalanan Akış sayısı ve yakalanan en erken günlüğün tarihi hakkında bilgiler görüntüler. Bu bilgiler, NSG'lerin ne günlüğe kaydedildiği ve akışların tarih aralığı hakkında bir fikir verir.

![infochart1][11]

![infochart2][12]

Bu şablon, yalnızca en çok ilgilendiğiniz verileri görüntülemenize olanak tanıyan aşağıdaki dilimleyicileri içerir. Kaynak gruplarınıza, NSG'lerinize ve kurallarınıza filtre uygulayabilirsiniz. Ayrıca 5-tuple bilgi, karar ve günlük yazıldığı zaman filtreleyebilirsiniz.

![Dilimleyici][13]

## <a name="conclusion"></a>Sonuç

Bu senaryoda, Network Watcher ve Power BI tarafından sağlanan Ağ Güvenliği Grup Akışı günlüklerini kullanarak trafiği görselleştirebildiğimizi ve anlayabildiğimizi gösterdik. Sağlanan şablonu kullanarak Power BI günlükleri doğrudan depolamadan indirir ve yerel olarak işler. Şablonu yüklemek için alınan süre, istenen dosya sayısına ve indirilen dosyaların toplam boyutuna bağlı olarak değişir.

İhtiyaçlarınız için bu şablonu özelleştirmekiçin çekinmeyin. Ağ Güvenliği Grubu Akış Günlükleri ile Power BI'yi kullanabileceğiniz birçok yol vardır. 

## <a name="notes"></a>Notlar

* Varsayılan olarak günlükleri saklanır`https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`

    * Başka bir dizinde başka veriler varsa, verileri çekme ve işleme sorguları değiştirilmelidir.

* Sağlanan şablon, 1 GB'dan fazla günlükiçeren kullanım için önerilmez.

* Büyük miktarda günlük alanınız varsa, Veri Gölü veya SQL sunucusu gibi başka bir veri deposu kullanarak bir çözümü araştırmanızı öneririz.

## <a name="next-steps"></a>Sonraki Adımlar

[Açık kaynak araçlarını kullanarak Visualize Azure Network Watcher NSG akış günlüklerini](network-watcher-visualize-nsg-flow-logs-open-source-tools.md) ziyaret ederek Elastik Yığın ile NSG akış günlüklerinizi nasıl görselleştirebilirsiniz öğrenin

[1]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure7.png
[8]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure8.png
[9]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure9.png
[10]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure10.png
[11]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure11.png
[12]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure12.png
[13]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure13.png
