---
title: Azure NetApp Files için Kobİ performansı hakkında SSS| Microsoft Dokümanlar
description: Azure NetApp Files için SMB performansı yla ilgili sık sorulan soruları yanıtlar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/17/2020
ms.author: b-juche
ms.openlocfilehash: 24b3710861f0ee158619ae9103584dcdb181f3d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460458"
---
# <a name="faqs-about-smb-performance-for-azure-netapp-files"></a>Azure NetApp Dosyaları için Kobİ performansı hakkında SSS'ler

Bu makalede, Azure NetApp Dosyaları için Kobİ performansı en iyi uygulamaları hakkında sık sorulan soruları (SSS) yanıtlar.

## <a name="is-smb-multichannel-enabled-in-smb-shares"></a>Kobİ hisselerinde Kobİ Çok Kanallı etkin midir? 

Evet, SMB Multichannel varsayılan olarak etkinleştirilir ve ocak 2020 başlarında bir değişiklik gerçekleşir. Tüm Kobİ hisseleri önceden tarihlenmiş varolan Kobİ hacimleri özelliği etkinleştirilmiş ve tüm yeni oluşturulan hacimleri de oluşturma sırasında özelliği etkin olacaktır. 

SMB Çok Kanallı işlevsellilikten yararlanmak için özellik etkinleştirmeden önce kurulan herhangi bir SMB bağlantısının sıfırlanmasının gerekir. Sıfırlamak için, SMB paylaşımını kesebilir ve yeniden bağlayabilirsiniz.

## <a name="is-rss-supported"></a>RSS desteklendi mi?

Evet, Azure NetApp Files alma tarafı ölçekleme (RSS) destekler.

SMB Multichannel etkin ken, bir SMB3 istemcisi Azure NetApp Files SMB sunucusuna tek bir RSS yeteneğine sahip bir ağ arabirimi kartı (NIC) üzerinden birden çok TCP bağlantısı kurar. 

## <a name="which-windows-versions-support-smb-multichannel"></a>Hangi Windows sürümleri Kobİ Çoklu Kanal'ı destekler?

Windows, en iyi performansı sağlamak için Windows 2012'den beri SMB Multichannel'ı desteklemiştir.  Ayrıntılar için [SMB Çok Kanallı'yı dağıt'a](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3Dws.11)) ve [SMB Çok Kanallı'nın temellerine](https://blogs.technet.microsoft.com/josebda/2012/06/28/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0/) bakın. 


## <a name="does-my-azure-virtual-machine-support-rss"></a>Azure sanal makinem RSS'yi destekliyor mu?

Azure sanal makine NIC'lerinizin RSS'yi destekleyip desteklemedigini görmek için komutu `Get-SmbClientNetworkInterface` aşağıdaki gibi çalıştırın ve alanı `RSS Capable`kontrol edin: 

![Azure sanal makinesi için RSS desteği](../media/azure-netapp-files/azure-netapp-files-formance-rss-support.png)

## <a name="does-azure-netapp-files-support-smb-direct"></a>Azure NetApp Files SMB Direct'i destekliyor mu?

Hayır, Azure NetApp Files SMB Direct'i desteklemez. 

## <a name="what-is-the-benefit-of-smb-multichannel"></a>Kobİ Çok Kanallı'nın faydası nedir? 

SMB Çok Kanallı özelliği, bir SMB3 istemcisinin tek bir ağ arabirimi kartı (NIC) veya birden çok NIC üzerinden bağlantı havuzu oluşturmasına ve bunları tek bir SMB oturumu için istek göndermek için kullanmasına olanak tanır. Buna karşılık, tasarım gereği, SMB1 ve SMB2 istemcinin bir bağlantı kurmasını ve bu bağlantı üzerinden belirli bir oturum için tüm SMB trafiğini göndermesini gerektirir. Bu tek bağlantı, tek bir istemciden elde edilebilen genel iletişim kuralı performansını sınırlar.

## <a name="should-i-configure-multiple-nics-on-my-client-for-smb"></a>SMB için istemcimde birden fazla NIC yapılandırmalı mıyım?

Hayır. SMB istemcisi, SMB sunucusu tarafından döndürülen NIC sayısıyla eşleşir.  Her depolama birimine bir ve yalnızca bir depolama bitiş noktasından erişilebilir.  Bu, herhangi bir SMB ilişkisi için yalnızca bir NIC kullanılacağı anlamına gelir.  

Aşağıdaki çıktının `Get-SmbClientNetworkInterace` da gösterdiği gibi, sanal makinenin 15 ve 12 olmak üzere iki ağ arabirimi vardır.  Aşağıda iki `Get-SmbMultichannelConnection`RSS özellikli NICS olmasına rağmen, sMB payı ile bağlantılı olarak yalnızca 12 arabirimi kullanılır; arabirim 15 kullanılmaz.

![RSS özellikli NICS](../media/azure-netapp-files/azure-netapp-files-rss-capable-nics.png)

## <a name="is-nic-teaming-supported-in-azure"></a>NIC Teaming Azure'da desteklendi mi?

NIC Ekip Çalışması Azure'da desteklenmez. Azure sanal makinelerinde birden çok ağ arabirimi desteklenmiş olsa da, bunlar fiziksel bir yapı yerine mantıksal bir yapıyı temsil eder. Bu nedenle, hiçbir hata toleransı sağlar.  Ayrıca, bir Azure sanal makinesinin kullanabileceği bant genişliği, herhangi bir ağ arabirimi için değil, makinenin kendisi için hesaplanır.

## <a name="whats-the-performance-like-for-smb-multichannel"></a>Kobİ Multichannel için performans nasıl?

Aşağıdaki testler ve grafikler, Kobİ Çok Kanallı'nın tek örnekli iş yüklerinde gücünü gösterir.

### <a name="random-io"></a>Rastgele I/O  

SMB Çok kanallı istemci üzerinde devre dışı bırakılırken, SAF 8-KiB okuma ve yazma testleri FIO ve 40-GiB çalışma seti kullanılarak gerçekleştirildi.  SMB payı, RSS ağ `1`arabirimi ayarları başına SMB istemci bağlantı sayısı artışlarıyla,`4``8`her`16` `set-SmbClientConfiguration -ConnectionCountPerRSSNetworkInterface <count>`test arasında, , , , . Testler, G/Ç yoğun `4` iş yükleri için varsayılan ayarın yeterli olduğunu gösterir; artış `8` ve `16` hiçbir etkisi yoktu. 

Komut, `netstat -na | findstr 445` ek bağlantıların. `1` `4` `8` `16`  Perfmon `Per Processor Network Activity Cycles` istatistiği tarafından doğrulanan (bu makalede yer almayan) her test sırasında smb için dört CPU çekirdeği tam olarak kullanılmıştır.

![Rastgele G/Ç testleri](../media/azure-netapp-files/azure-netapp-files-random-io-tests.png)

Azure sanal makinesi SMB (veya NFS) depolama G/Ç sınırlarını etkilemez.  Aşağıda gösterildiği gibi, D16 örnek türü önbelleğe alınmış depolama IOPS için 32.000 ve önbelleğe edilmemiş depolama IOPS için 25.600 sınırlı bir orana sahiptir.  Ancak, yukarıdaki grafik SMB üzerinde önemli ölçüde daha fazla G / O gösterir.

![Rastgele G/Ç karşılaştırması](../media/azure-netapp-files/azure-netapp-files-random-io-tests-list.png)

### <a name="sequential-io"></a>Sıralı IO 

Yukarıda açıklanan rastgele G/Ç testlerine benzer testler 64-KiB sıralı G/Ç ile yapılmıştır. RSS ağ arabirimi başına 4' ötesinde istemci bağlantı sayısındaki artışlar rastgele G/Ç üzerinde belirgin bir etkiye sahip olmasa da, aynı durum sıralı G/Ç için geçerli değildir. Aşağıdaki grafikte de görüldüğü gibi, her artış, okuma iş sayısındaki karşılık gelen artışla ilişkilidir. Yazma iş boyutu, Azure tarafından her örnek türü/boyutu için yerleştirilen ağ bant genişliği kısıtlamaları nedeniyle düz kaldı. 

![Sıralı G/Ç testleri](../media/azure-netapp-files/azure-netapp-files-sequential-io-tests.png)

Azure, her sanal makine türüne/boyutuna ağ hızı sınırları yerleştirir. Oran sınırı yalnızca giden trafiğe uygulanır. Sanal bir makinede bulunan NIC sayısının, makinenin kullanabileceği toplam bant genişliği miktarıyla hiçbir ilgisi yoktur.  Örneğin, D16 örnek türü 8000 Mbps (1.000 MiB/s) bir empoze ağ sınırı vardır.  Yukarıdaki sıralı grafiğin gösterdiği gibi, sınır giden trafiği etkiler (yazar) ancak çok kanallı okumaları etkilemez.

![Sıralı G/Ç karşılaştırması](../media/azure-netapp-files/azure-netapp-files-sequential-io-tests-list.png)

## <a name="is-accelerated-networking-recommended"></a>Hızlandırılmış Ağ önerilir?

Maksimum performans için, [Hızlandırılmış Ağ'ı](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell) mümkün olduğunca yapılandırmanız önerilir. Aşağıdaki hususları göz önünde bulundurun:  

* Azure portalı, bu özelliği destekleyen sanal makineler için varsayılan olarak Hızlandırılmış Ağ'ı sağlar.  Ancak, Ansible ve benzeri yapılandırma araçları gibi diğer dağıtım yöntemleri olmayabilir.  Hızlandırılmış Ağ etkinleştirilememesi bir makinenin performansını sekteye erdirebilir.  
* Hızlandırılmış Ağ bir örnek türü veya boyutu için destek eksikliği nedeniyle sanal bir makinenin ağ arabiriminde etkin değilse, daha büyük örnek türleri ile devre dışı kalır. Bu gibi durumlarda manuel müdahaleye ihtiyacınız olacaktır.

## <a name="are-jumbo-frames-supported"></a>Jumbo çerçeveler desteklenir mi?

Jumbo çerçeveler Azure sanal makineleriyle desteklenmez.

## <a name="is-smb-signing-supported"></a>Kobİ İmzalama desteklendi mi? 

Kobİ protokolü, dosya ve yazdırma paylaşımı ve uzak Windows yönetimi gibi diğer ağ işlemleri için temel sağlar. SMB protokolü, geçiş sırasında Kobİ paketlerini değiştiren ortadaki adam saldırılarını önlemek için Kobİ paketlerinin dijital olarak imzalanmasını destekler. 

SMB İmzalama, Azure NetApp Files tarafından desteklenen tüm Kobİ protokolü sürümleri için desteklenir. 

## <a name="what-is-the-performance-impact-of-smb-signing"></a>Kobİ İmzalama'nın performans etkisi nedir?  

SMB İmzalama'nın Kobİ performansı üzerinde zararlı bir etkisi vardır. Performans bozulmasının diğer olası nedenleri arasında, her paketin dijital olarak imzalanması, aşağıdaki perfmon çıkışının gösterdiği gibi ek istemci tarafındaki CPU'yu tüketir. Bu durumda, Core 0, Kobİ İmzalama da dahil olmak üzere SMB'den sorumlu görünür.  Önceki bölümdeki çok kanallı olmayan ardışık okuma iş verme rakamlarıyla yapılan bir karşılaştırma, Kobİ İmzalama'nın genel iş ortasını 875MiB/s'den yaklaşık 250MiB/s'ye düşürduğunu gösterir. 

![Kobİ İmzalama performans etkisi](../media/azure-netapp-files/azure-netapp-files-smb-signing-performance.png)


## <a name="next-steps"></a>Sonraki adımlar  

- [Azure NetApp Dosyaları Hakkında SSS](azure-netapp-files-faqs.md)
- Azure [NetApp Dosyaları: SMB Dosya Paylaşımları için Yönetilen Kurumsal Dosya Paylaşımları'na](https://cloud.netapp.com/hubfs/Resources/ANF%20SMB%20Quickstart%20doc%20-%2027-Aug-2019.pdf?__hstc=177456119.bb186880ac5cfbb6108d962fcef99615.1550595766408.1573471687088.1573477411104.328&__hssc=177456119.1.1573486285424&__hsfp=1115680788&hsCtaTracking=cd03aeb4-7f3a-4458-8680-1ddeae3f045e%7C5d5c041f-29b4-44c3-9096-b46a0a15b9b1) bakın.
