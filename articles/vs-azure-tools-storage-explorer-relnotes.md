---
title: Microsoft Azure Depolama Gezgini sürüm notları
description: Microsoft Azure Depolama Gezgini için sürüm notları
services: storage
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: ''
ms.assetid: ''
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2018
ms.author: cawa
ms.openlocfilehash: 1adfb59843150ffaa6ed76411d07d8ec6cf6a44b
ms.sourcegitcommit: 428fded8754fa58f20908487a81e2f278f75b5d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2019
ms.locfileid: "74555259"
---
# <a name="microsoft-azure-storage-explorer-release-notes"></a>Microsoft Azure Depolama Gezgini sürüm notları

Bu makalede, Azure Depolama Gezgini için en son sürüm notları ve önceki sürümlere ait sürüm notları bulunur. 

[Microsoft Azure Depolama Gezgini](./vs-azure-tools-storage-manage-with-storage-explorer.md) , Windows, MacOS ve Linux 'Ta Azure Depolama verileriyle kolayca çalışabilmenizi sağlayan tek başına bir uygulamadır.

Önceki Depolama Gezgini sürümlerini indirmek için GitHub deponuzdaki [yayınlar sayfasını](https://github.com/microsoft/AzureStorageExplorer/releases) ziyaret edebilirsiniz.

## <a name="version-1110"></a>Sürüm 1.11.0
11/4/2019

### <a name="new"></a>Yeni
* Blob 'Lar, ADLS 2. ve yönetilen diskler için işlemler tümleşik AzCopy kullanır. Daha belirgin olarak, aşağıdaki işlemler AzCopy kullanılarak yapılır:
   * Bloblar
      * Düzenle + karşıya yükle için aç
      * Sürükle & bırak dahil karşıya yükleme
      * İndirin
      * Kopyala & #1249 Yapıştır
      * Sil
   * ADLS 2. blob 'Lar
      * Sürükle & bırak dahil karşıya yükleme
      * İndirin
      * Kopyala & Yapıştır
      * Klasör silme dahil silme
   * Managed Disks (Yönetilen Diskler)
      * Karşıya Yükleme
      * İndirin
      * Kopyala & Yapıştır

   Ayrıca, tümleşik AzCopy deneyimine bazı sık istenen özellikler eklenmiştir:
   * Çakışma çözümleri-çakışmaları çözümlemek için aktarımlar sırasında sizden uyarılırsınız. #1455
   * Sayfa Blobları olarak karşıya yükle-AzCopy 'ın. VHD ve. vhdx dosyalarını sayfa Blobları olarak karşıya yükleyip yüklememeyeceğini seçebilirsiniz. #1164 ve #1601
   * Yapılandırılabilir AzCopy parametreleri-AzCopy 'un performans ve kaynak kullanımını ayarlamak için çeşitli ayarlar eklenmiştir. Daha fazla ayrıntı için aşağıya bakın.

* ADLS 2. ve Blobların çoklu protokol erişimini etkinleştirmek ve ADLS 2. deneyimlerini daha da geliştirmek için ADLS 2. hesapları için aşağıdaki özellikleri ekledik:
   * ACL izinleri ayarlamak için kolay adlar kullanarak arama
   * $logs ve $web gibi gizli kapsayıcıları görüntüleyin
   * Kapsayıcı kirası al ve kes
   * Blob kira #848 al ve kes
   * Kapsayıcı erişim ilkelerini yönetme
   * Blob erişim katmanlarını yapılandırma
   * Blob 'Ları Kopyala & Yapıştır

* Bu sürümde, 17 ek dil önizlemesi yaptık. "Application" → "bölgesel ayarlar" → "Language (Önizleme)" altındaki ayarlar sayfasında istediğiniz dile geçebilirsiniz. Ek dizeleri çevirirken ve çeviri kalitesini iyileştirmek için hala çok çalıştık. Bir çeviriyle ilgili geri bildiriminiz olmalıdır veya henüz çevrilmemiş bir dizeye fark ederseniz lütfen [GitHub 'da bir sorun açın](https://github.com/microsoft/AzureStorageExplorer/issues/new?assignees=&labels=%F0%9F%8C%90%20localization&template=bug-report.md&title=).
* Her sürümde, Depolama Gezgini ince açma özelliğini etkinleştirmek için birkaç ayar yüklemeye çalışıyoruz. Bu sürümde, AzCopy 'i daha fazla yapılandırma ve hizmet düğümlerini gizleme ayarlarını ekledik:
   * AzCopy bant genişliği sınırı-ağ AzCopy 'in ne kadarının kullandığını denetlemeye yardımcı olur. Bu ayarı "aktarımlar" → "AzCopy" → "en fazla aktarım hızı" adresinde bulabilirsiniz. #1099
   * AzCopy MD5 denetimi-indirme sırasında MD5 karmaları olup olmadığını ve ne kadar AzCopy denetimi denetleyeceğini yapılandırmanıza olanak tanır. Bu ayarı "aktarımlar" → "AzCopy" → "Check MD5" adresinde bulabilirsiniz.
   * AzCopy eşzamanlılık ve bellek arabelleği boyutu-varsayılan AzCopy, bu ayarlar için anlamlı varsayılan değerler belirlemede makinenizi analiz eder. Ancak performans sorunlarıyla karşılaşırsanız, bu gelişmiş ayarlar bilgisayarınızda AzCopy 'in nasıl çalıştığını daha ayrıntılı bir şekilde uyarlamak için kullanılabilir. Bu ayarları "aktarımlar" → "AzCopy" altında bulabilirsiniz. #994
   * Hizmet düğümlerini görüntüleme ve gizleme-bu ayarlar size Depolama Gezgini desteklediği Azure hizmetlerinden herhangi birini görüntüleme veya gizleme seçeneklerini sunar. Bu ayarları "Hizmetler" bölümünde bulabilirsiniz. #1877

* Yönetilen bir diskin anlık görüntüsünü oluştururken, varsayılan ad artık sağlanır. #1847
* Azure AD ile eklenirken bir ADLS 2. blob kapsayıcısı eklerseniz, düğümün yanında "(ADLS 2.)" görüntülenir. #1861

### <a name="fixes"></a>Düzeltilen
* Büyük diskleri kopyalarken, karşıya yüklerken veya indirirken Depolama Gezgini, bazı durumlarda işlem ile ilgili disklere erişimi iptal edemeyebilir. Bu düzeltildi. #2048
* Bölüm anahtarı sorgusu görüntülenirken tablo istatistikleri başarısız oldu. Bu düzeltildi. #1886

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini 1.11.0, ADLS 2. kapsayıcılarına iliştirilecek bir DFS uç noktası (örneğin, "myaccount.dfs.core.windows.net") gerektirir. Önceki Depolama Gezgini sürümleri bir blob uç noktası kullanmanıza izin verilir. Bu ekler, 1.11.0 sürümüne yükselttikten sonra artık çalışmayabilir. Bu sorunla karşılaşırsanız, DFS uç noktasını kullanarak yeniden bağlayın.
* Sayısal ayarlar, geçerli bir aralıkta olup olmadıkları için denetlenmez. #2140
* Blob kapsayıcıları, ağaç görünümünde bir depolama hesabından diğerine kopyalamak başarısız olabilir. Sorunu araştırıyoruz. #2124
* Otomatik yenileme ayarı, blob Gezgini 'ndeki tüm işlemleri henüz etkilemez.
* Yönetilen disk özellikleri Azure Stack desteklenmez.
* Bir disk karşıya yükleme veya yapıştırma başarısız olursa ve hatadan önce yeni bir disk oluşturulduysa Depolama Gezgini, diski sizin için silmez.
* Bir disk karşıya yüklemeyi veya yapıştırmayı iptal ettiğinizde, yeni diski bozuk bir durumda bırakmak mümkündür. Bu durumda, yeni diski silmeniz veya disk API 'Lerini el ile arayarak diskin içeriğini artık bozulmayacak şekilde değiştirmeniz gerekir.
* RBAC kullanılırken, depolama kaynaklarınıza erişebilmek için Depolama Gezgini bazı yönetim katmanı izinleri gerektirir. Daha fazla bilgi için bkz. [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) .
* Bir blob kapsayıcısı gibi SAS URI 'SI aracılığıyla bağlı bir kaynağın ayrılması, diğer eklerin doğru şekilde görüntülenmesini engelleyen bir hataya neden olabilir. Bu sorunu geçici olarak çözmek için Grup düğümünü yenilemeniz yeterlidir. Daha fazla bilgi için bkz. #537.
* Mac için VS 'yi kullanırsanız ve şimdiye kadar özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu geçici olarak çözmek için ~/içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak sizi engellemez, bu sorunla ilgili yorum yapın.
* Azurite, tüm depolama API 'Lerini henüz tam olarak uygulamamıştır. Bu nedenle, geliştirme depolaması için Azurite kullanılırken beklenmeyen hatalar veya davranışlar olabilir.
* Nadir durumlarda, ağaç odağı hızlı erişime açık kalabilir. Odağı kaldırmak için, tümünü yenileyebilirsiniz.
* NodeJS içindeki bir hata nedeniyle OneDrive klasörünüzden karşıya yükleme işlemi çalışmıyor. Hata düzeltildi, ancak henüz elektron ile tümleştirildi. Blob kapsayıcısına yükleme veya indirme sırasında bu sorunu geçici olarak çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez. Azure Stack kaynaklarla çalışırken bu özellikleri kullanma girişimi beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
   * ADLS 2.
   * Managed Disks (Yönetilen Diskler)
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux üzerinde Depolama Gezgini çalıştırmak için öncelikle bazı bağımlılıkların yüklenmesi gerekir. Daha fazla bilgi için Depolama Gezgini [sorun giderme kılavuzuna](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#linux-dependencies) bakın.

## <a name="previous-releases"></a>Önceki sürümler

* [Sürüm 1.10.1](#version-1101)
* [Sürüm 1.10.0](#version-1100)
* [Sürüm 1.9.0](#version-190)
* [Sürüm 1.8.1](#version-181)
* [Sürüm 1.8.0](#version-180)
* [Sürüm 1.7.0](#version-170)
* [Sürüm 1.6.2](#version-162)
* [Sürüm 1.6.1](#version-161)
* [Sürüm 1.6.0](#version-160)
* [Sürüm 1.5.0](#version-150)
* [Sürüm 1.4.4](#version-144)
* [Sürüm 1.4.3](#version-143)
* [Sürüm 1.4.2](#version-142)
* [Sürüm 1.4.1](#version-141)
* [Sürüm 1.3.0](#version-130)
* [Sürüm 1.2.0](#version-120)
* [Sürüm 1.1.0](#version-110)
* [Sürüm 1.0.0](#version-100)
* [Sürüm 0.9.6](#version-096)
* [Sürüm 0.9.5](#version-095)
* [Sürüm 0.9.4 ve 0.9.3](#version-094-and-093)
* [Sürüm 0.9.2](#version-092)
* [Sürüm 0.9.1 ve 0.9.0](#version-091-and-090)
* [Sürüm 0.8.16](#version-0816)
* [Sürüm 0.8.14](#version-0814)
* [Sürüm 0.8.13](#version-0813)
* [Sürüm 0.8.12 ve 0.8.11 ve 0.8.10](#version-0812-and-0811-and-0810)
* [Sürüm 0.8.9 ve 0.8.8](#version-089-and-088)
* [Sürüm 0.8.7](#version-087)
* [Sürüm 0.8.6](#version-086)
* [Sürüm 0.8.5](#version-085)
* [Sürüm 0.8.4](#version-084)
* [Sürüm 0.8.3](#version-083)
* [Sürüm 0.8.2](#version-082)
* [Sürüm 0.8.0](#version-080)
* [Sürüm 0.7.20160509.0](#version-07201605090)
* [Sürüm 0.7.20160325.0](#version-07201603250)
* [Sürüm 0.7.20160129.1](#version-07201601291)
* [Sürüm 0.7.20160105.0](#version-07201601050)
* [Sürüm 0.7.20151116.0](#version-07201511160)

## <a name="version-1101"></a>Sürüm 1.10.1
9/19/2019

### <a name="hotfix"></a>Düzeltme
* Bazı kullanıcılar, verilerini ADLS Gen 1 hesaplarında görüntülemeye çalışırken 1.10.0 içinde bir hatayla karşılaştı. Bu hata Gezgin panelinin düzgün şekilde işlemesini engelledi. Bu düzeltildi. #1853 #1865

### <a name="new"></a>Yeni
* Depolama Gezgini artık adanmış bir ayarlar Kullanıcı arabirimine sahiptir. → Ayarlarından ya da sol taraftaki dikey araç çubuğunda Ayarlar simgesine (dişli) tıklayarak erişebilirsiniz. Bu özellik, [Kullanıcı tarafından istenen çeşitli ayarları](https://github.com/microsoft/AzureStorageExplorer/labels/%3Abulb%3A%20setting%20candidate)sağlamamız gereken ilk adımdır. Bu sürümden itibaren, aşağıdaki ayarlar desteklenir:
  * Tema
  * Ara sunucu
  * Çıkışta oturum kapatma #6
  * Cihaz kod akışı oturum açmayı etkinleştir
  * Otomatik yenileme #1526
  * AzCopy 'i etkinleştir
  * AzCopy SAS süresi eklemek istediğiniz diğer ayarları varsa, lütfen görmek istediğiniz ayarı açıklayan [GitHub 'da bir sorun açın](https://github.com/microsoft/AzureStorageExplorer/issues/new?assignees=&labels=%3Abulb%3A%20setting%20candidate&template=feature_request.md&title=) .
* Depolama Gezgini artık yönetilen diskleri desteklemektedir. Yapabilecekleriniz:
  * Şirket içi bir VHD 'yi yeni bir diske yükleme
  * Bir disk indirin
  * Diskleri kaynak grupları ve bölgeler arasında Kopyala ve Yapıştır
  * Diskleri silme
  * Diskin anlık görüntüsünü oluşturma, diskleri karşıya yükleme, indirme ve çapraz bölge kopyalama, AzCopy ile v10 arasındaki tarafından desteklenmektedir.
* Depolama Gezgini artık Linux üzerinde Snap Store aracılığıyla yüklenebilirler. Snap Store aracılığıyla yüklediğinizde, .NET Core dahil olmak üzere tüm bağımlılıklar sizin için yüklenir! Şu anda Depolama Gezgini Ubuntu ve CentOS üzerinde iyi çalıştığını doğruladık. Diğer Linux kaldırmalar üzerinde Snap mağazasından yükleme sorunlarıyla karşılaşırsanız lütfen [GitHub 'da bir sorun açın](https://github.com/microsoft/AzureStorageExplorer/issues/new?assignees=&labels=snaps&template=bug-report.md&title=). Snap mağazasından yükleme hakkında daha fazla bilgi edinmek için [Başlarken kılavuzumuza](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=linux)bakın. #68
* Özelliği ADLS 2. kullanıcılar için daha yararlı hale getirmek amacıyla tasarlanan Azure Active Directory (Azure AD) ile birlikte iliştirilecek iki önemli değişiklik yapılmıştır:
  * Artık iliştirmekte olduğunuz kaynağın bulunduğu kiracıyı seçersiniz. Bu, artık kaynağın aboneliğine RBAC erişiminizin olması gerekmediği anlamına gelir.
  * Bir ADLS 2. blob kapsayıcısı iliştiriyorsanız, artık kapsayıcıda belirli bir yola ekleyebilirsiniz.
* ADLS 2. dosya ve klasörler için ACL 'Leri yönetirken, Depolama Gezgini artık ACL 'lerdeki varlıkların kolay adlarını gösterecektir. #957
* Bir ADLS 2. ACL 'sine OID aracılığıyla eklerken, Depolama Gezgini artık OID 'nin kiracınızdaki geçerli bir varlığa ait olduğunu doğrular. #1603
* Sekmeler arasında gezinmek için klavye kısayolları artık daha standart anahtar bileşimleri kullanıyor. #1018
* Bir sekmeye ortadaki tıklama, şimdi kapatılacak. #1348
* AzCopy aktarımı atlanmayı içeriyorsa ve hata içermiyorsa, Depolama Gezgini atlandığını vurgulamak için şimdi bir uyarı simgesi gösterir. #1490
* Tümleşik AzCopy, 10.2.1 sürümüne güncelleştirilmiştir. Ayrıca, bilgi iletişim kutusunda yüklü AzCopy sürümünü de görüntüleyebilirsiniz. #1343

### <a name="fixes"></a>Düzeltilen
* Çok sayıda kullanıcı, ekli depolama hesaplarıyla çalışırken çeşitli "tanımsız sürüm okunamaz" veya "tanımsız" hata bağlantıları okunamaz. Bu sorunun kök nedenini araştırmaya devam ediyoruz, ancak 1.10.0 ' de ekli depolama hesaplarını yükleme sırasında hata işlemeyi geliştirdik. #1626, #985 ve #1532
* Gezgin ağacı (sol taraf), odağın en üst düğüme sürekli olarak atlayacağı bir duruma ulaşmak için mümkündür. Bu düzeltildi. #1596
* Bir Blobun anlık görüntülerini yönetirken, ekran okuyucular anlık görüntüyle ilişkili zaman damgasını okumaz. Bu düzeltildi. #1202
* MacOS üzerindeki proxy ayarı, kimlik doğrulama işleminin bunları kullanabilmesi için zaman içinde ayarlanmıyor. Bu düzeltildi. #1567
* Bir sogeign bulutundaki bir depolama hesabı adı ve anahtarı kullanarak iliştirilmişse AzCopy çalışmaz. Bu düzeltildi. #1544
* Bir bağlantı dizesi aracılığıyla eklenirken Depolama Gezgini artık sondaki boşlukları kaldırır. #1387

### <a name="known-issues"></a>Bilinen Sorunlar
* Otomatik yenileme ayarı, blob Gezgini 'ndeki tüm işlemleri henüz etkilemez.
* Yönetilen disk özellikleri Azure Stack desteklenmez.
* Bir disk karşıya yükleme veya yapıştırma başarısız olursa ve hatadan önce yeni bir disk oluşturulduysa Depolama Gezgini, diski sizin için silmez.
* Bir disk karşıya yüklemeyi veya yapıştırmayı iptal ettiğinizde, yeni diski bozuk bir durumda bırakmak mümkündür. Bu durumda, yeni diski silmeniz veya disk API 'Lerini el ile arayarak diskin içeriğini artık bozulmayacak şekilde değiştirmeniz gerekir.
* Bir disk karşıya yüklemeyi veya yapıştırmayı iptal ettiğinizde, yeni diski bozuk bir durumda bırakmak mümkündür. Bu durumda, yeni diski silmeniz veya disk API 'Lerini el ile arayarak diskin içeriğini artık bozulmayacak şekilde değiştirmeniz gerekir.
* AzCopy olmayan bir blob indirmesi gerçekleştirirken, büyük dosyalar için MD5 doğrulanmaz. Bunun nedeni, depolama SDK 'sindeki bir hatadır. [#1212](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1212)
* RBAC kullanılırken, depolama kaynaklarınıza erişebilmek için Depolama Gezgini bazı yönetim katmanı izinleri gerektirir. Daha fazla bilgi için bkz. [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) .
* Bir blob kapsayıcısı gibi SAS URI 'SI aracılığıyla bağlı bir kaynağın ayrılması, diğer eklerin doğru şekilde görüntülenmesini engelleyen bir hataya neden olabilir. Bu sorunu geçici olarak çözmek için Grup düğümünü yenilemeniz yeterlidir. Daha fazla bilgi için bkz. #537.
* Mac için VS 'yi kullanırsanız ve şimdiye kadar özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu geçici olarak çözmek için ~/içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak sizi engellemez, bu sorunla ilgili yorum yapın.
* Azurite, tüm depolama API 'Lerini henüz tam olarak uygulamamıştır. Bu nedenle, geliştirme depolaması için Azurite kullanılırken beklenmeyen hatalar veya davranışlar olabilir.
* Nadir durumlarda, ağaç odağı hızlı erişime açık kalabilir. Odağı kaldırmak için, tümünü yenileyebilirsiniz.
* NodeJS içindeki bir hata nedeniyle OneDrive klasörünüzden karşıya yükleme işlemi çalışmıyor. Hata düzeltildi, ancak henüz elektron ile tümleştirildi. Blob kapsayıcısına yükleme veya indirme sırasında bu sorunu geçici olarak çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez. Azure Stack kaynaklarla çalışırken bu özellikleri kullanma girişimi beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
   * ADLS 2.
   * Managed Disks (Yönetilen Diskler)
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux üzerinde Depolama Gezgini çalıştırmak için öncelikle bazı bağımlılıkların yüklenmesi gerekir. Daha fazla bilgi için Depolama Gezgini [sorun giderme kılavuzuna](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#linux-dependencies) bakın.


## <a name="version-1100"></a>Sürüm 1.10.0
9/12/2019

### <a name="new"></a>Yeni

* Depolama Gezgini artık adanmış bir ayarlar Kullanıcı arabirimine sahiptir. → Ayarlarından ya da sol taraftaki dikey araç çubuğunda Ayarlar simgesine (dişli) tıklayarak erişebilirsiniz. Bu özellik, [Kullanıcı tarafından istenen çeşitli ayarları](https://github.com/microsoft/AzureStorageExplorer/labels/%3Abulb%3A%20setting%20candidate)sağlamamız gereken ilk adımdır. Bu sürümden itibaren, aşağıdaki ayarlar desteklenir:
    * Tema
    * Ara sunucu
    * Çıkışta oturum kapatma [#6](https://www.github.com/Microsoft/AzureStorageExplorer/issues/6)
    * Cihaz kod akışı oturum açmayı etkinleştir
    * Otomatik yenileme [#1526](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1526)
    * AzCopy 'i etkinleştir
    * AzCopy SAS süresi

    Eklemek istediğiniz başka ayarlar varsa, lütfen [görmek istediğiniz ayarı açıklayan GitHub 'da bir sorun açın](https://github.com/microsoft/AzureStorageExplorer/issues/new?assignees=&labels=%3Abulb%3A%20setting%20candidate&template=feature_request.md&title=).
* Depolama Gezgini artık yönetilen diskleri desteklemektedir. Yapabilecekleriniz:
    * Şirket içi bir VHD 'yi yeni bir diske yükleme
    * Bir disk indirin
    * Diskleri kaynak grupları ve bölgeler arasında Kopyala ve Yapıştır
    * Diskleri silme
    * Diskin anlık görüntüsünü oluşturma

    Disklerin karşıya yüklenmesi, indirilmesi ve çapraz bölge kopyalaması, AzCopy ile v10 arasındaki tarafından desteklenmektedir.
* Depolama Gezgini artık Linux üzerinde Snap Store aracılığıyla yüklenebilirler. Snap Store aracılığıyla yüklediğinizde, .NET Core dahil olmak üzere tüm bağımlılıklar sizin için yüklenir! Şu anda Depolama Gezgini Ubuntu ve CentOS üzerinde iyi çalıştığını doğruladık. Diğer Linux kaldırmalar üzerinde Snap mağazasından yükleme sorunlarıyla karşılaşırsanız lütfen [GitHub 'da bir sorun açın](https://github.com/microsoft/AzureStorageExplorer/issues/new?assignees=&labels=snaps&template=bug-report.md&title=). Snap mağazasından yükleme hakkında daha fazla bilgi edinmek için [Başlarken kılavuzumuza](https://aka.ms/storageexplorer/snapinformation)bakın. [#68](https://www.github.com/Microsoft/AzureStorageExplorer/issues/68)
* Özelliği ADLS 2. kullanıcılar için daha yararlı hale getirmek amacıyla tasarlanan Azure Active Directory (Azure AD) ile birlikte iliştirilecek iki önemli değişiklik yapılmıştır: * artık iliştirmekte olduğunuz kaynağın bulunduğu kiracıyı seçersiniz. Bu, artık kaynağın aboneliğine RBAC erişiminizin olması gerekmediği anlamına gelir.
        * Bir ADLS 2. blob kapsayıcısı iliştiriyorsanız, artık kapsayıcıda belirli bir yola iliştirebilirsiniz.
* ADLS 2. dosya ve klasörler için ACL 'Leri yönetirken, Depolama Gezgini artık ACL 'lerdeki varlıkların kolay adlarını gösterecektir. [#957](https://www.github.com/Microsoft/AzureStorageExplorer/issues/957)
* Bir ADLS 2. ACL 'sine OID aracılığıyla eklerken, Depolama Gezgini artık OID 'nin kiracınızdaki geçerli bir varlığa ait olduğunu doğrular. [#1603](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1603)
* Sekmeler arasında gezinmek için klavye kısayolları artık daha standart anahtar bileşimleri kullanıyor. [#1018](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1018)
* Bir sekmeye ortadaki tıklama, şimdi kapatılacak. [#1348](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1348)
* AzCopy aktarımı atlanmayı içeriyorsa ve hata içermiyorsa, Depolama Gezgini atlandığını vurgulamak için şimdi bir uyarı simgesi gösterir. [#1490](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1490)
* Tümleşik AzCopy, 10.2.1 sürümüne güncelleştirilmiştir. Ayrıca, bilgi iletişim kutusunda yüklü AzCopy sürümünü de görüntüleyebilirsiniz. [#1343](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1343)

### <a name="fixes"></a>Düzeltilen

* Çok sayıda kullanıcı, ekli depolama hesaplarıyla çalışırken çeşitli "tanımsız sürüm okunamaz" veya "tanımsız" hata bağlantıları okunamaz. Bu sorunun kök nedenini araştırmaya devam ediyoruz, ancak 1.10.0 ' de ekli depolama hesaplarını yükleme sırasında hata işlemeyi geliştirdik. [#1626](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1626), [#985](https://www.github.com/Microsoft/AzureStorageExplorer/issues/985)ve [#1532](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1532)
* Gezgin ağacı (sol taraf), odağın en üst düğüme sürekli olarak atlayacağı bir duruma ulaşmak için mümkündür. Bu düzeltildi. [#1596](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1596)
* Bir Blobun anlık görüntülerini yönetirken, ekran okuyucular anlık görüntüyle ilişkili zaman damgasını okumaz. Bu düzeltildi. [#1202](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1202)
* MacOS üzerindeki proxy ayarı, kimlik doğrulama işleminin bunları kullanabilmesi için zaman içinde ayarlanmıyor. Bu düzeltildi. [#1567](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1567)
* Bir sogeign bulutundaki bir depolama hesabı adı ve anahtarı kullanarak iliştirilmişse AzCopy çalışmaz. Bu düzeltildi. [#1544](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1544)
* Bir bağlantı dizesi aracılığıyla eklenirken Depolama Gezgini artık sondaki boşlukları kaldırır. [#1387](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1387)

### <a name="known-issues"></a>Bilinen Sorunlar

* Otomatik yenileme ayarı, blob Gezgini 'ndeki tüm işlemleri henüz etkilemez.
* Yönetilen disk özellikleri Azure Stack desteklenmez.
* Bir disk karşıya yükleme veya yapıştırma başarısız olursa ve hatadan önce yeni bir disk oluşturulduysa Depolama Gezgini, diski sizin için silmez.
* Bir disk karşıya yüklemeyi veya yapıştırmayı iptal ettiğinizde, yeni diski bozuk bir durumda bırakmak mümkündür. Bu durumda, yeni diski silmeniz veya disk API 'Lerini el ile arayarak diskin içeriğini artık bozulmayacak şekilde değiştirmeniz gerekir.
* AzCopy olmayan bir blob indirmesi gerçekleştirirken, büyük dosyalar için MD5 doğrulanmaz. Bunun nedeni, depolama SDK 'sindeki bir hatadır. [#1212](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1212)
* RBAC kullanılırken, depolama kaynaklarınıza erişebilmek için Depolama Gezgini bazı yönetim katmanı izinleri gerektirir. Daha fazla bilgi için bkz. [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) .
* Bir blob kapsayıcısı gibi SAS URI 'SI aracılığıyla bağlı bir kaynağın ayrılması, diğer eklerin doğru şekilde görüntülenmesini engelleyen bir hataya neden olabilir. Bu sorunu geçici olarak çözmek için Grup düğümünü yenilemeniz yeterlidir. Daha fazla bilgi için bkz. #537.
* Mac için VS 'yi kullanırsanız ve şimdiye kadar özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu geçici olarak çözmek için ~/içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak sizi engellemez, bu sorunla ilgili yorum yapın.
* Azurite, tüm depolama API 'Lerini henüz tam olarak uygulamamıştır. Bu nedenle, geliştirme depolaması için Azurite kullanılırken beklenmeyen hatalar veya davranışlar olabilir.
* Nadir durumlarda, ağaç odağı hızlı erişime açık kalabilir. Odağı kaldırmak için, tümünü yenileyebilirsiniz.
* NodeJS içindeki bir hata nedeniyle OneDrive klasörünüzden karşıya yükleme işlemi çalışmıyor. Hata düzeltildi, ancak henüz elektron ile tümleştirildi. Blob kapsayıcısına yükleme veya indirme sırasında bu sorunu geçici olarak çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez. Azure Stack kaynaklarla çalışırken bu özellikleri kullanma girişimi beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
   * ADLS 2.
   * Managed Disks (Yönetilen Diskler)
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux üzerinde Depolama Gezgini çalıştırmak için öncelikle bazı bağımlılıkların yüklenmesi gerekir. Daha fazla bilgi için Depolama Gezgini [sorun giderme kılavuzuna](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#linux-dependencies) bakın.

## <a name="version-190"></a>Sürüm 1.9.0
7/1/2019

### <a name="download-azure-storage-explorer-190"></a>Azure Depolama Gezgini 1.9.0 indir
- [Windows için Azure Depolama Gezgini 1.9.0](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Mac için Azure Depolama Gezgini 1.9.0](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Linux için Azure Depolama Gezgini 1.9.0](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a>Yeni

* Artık Azure AD (RBAC veya ACL izinleri) aracılığıyla blob kapsayıcıları ekleyebilirsiniz. Bu özellik, kapsayıcılara erişimi olan ancak kapsayıcıların bulunduğu depolama hesaplarına değil kullanıcılara yardımcı olmaya yöneliktir. Bu özellik hakkında daha fazla bilgi için Başlarken kılavuzumuza bakın.
* Edinme ve kesme kirası artık RBAC ile birlikte çalışır. [#1354](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1354)
* Erişim ilkelerini yönetme ve genel erişim düzeyini ayarlama artık RBAC ile birlikte çalışır. [#1355](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1355)
* Blob klasörlerini silme artık RBAC ile çalışır. [#1450](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1450)
* Blob erişim katmanını değiştirmek artık RBAC ile çalışır. [#1446](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1446)
* Artık, Hızlı erişimi "yardım" → "Reset" aracılığıyla hızlıca sıfırlayabilirsiniz. [#1327](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1327)

### <a name="preview-features"></a>Önizleme Özellikleri

* Cihaz kod akışı oturum açma artık önizleme için kullanılabilir. Bunu etkinleştirmek için "Önizleme" → "cihaz kodu akışı oturum açma kullan" bölümüne gidin. Daha güvenilir bir oturum açma formu olduğunu kanıtlayabildiğinden, bu özelliği denemek için boş oturum açma pencereleri ile sorun yaşayan tüm kullanıcıları teşvik ediyoruz.
* AzCopy ile tümleştirilmiş Depolama Gezgini Şu anda önizleme için kullanılabilir. Bunu etkinleştirmek için "Preview" → "Gelişmiş blob yüklemesi ve Indirilmesi için AzCopy kullanın" bölümüne gidin. AzCopy ile tamamlanan blob aktarımları daha hızlı ve daha iyi performansa sahip olmalıdır.

### <a name="fixes"></a>Düzeltilen

* Tek bir hesap için 50 ' den fazla aboneliği yükleyemedik. [#1416](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1416)
* Doğrudan bağlantı başarısız olduğunda görüntülenen bilgi çubuğunda "oturum aç" düğmesi düzeltildi. [#1358](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1358)
* MacOS 'ta. App dosyaları karşıya yüklenemedi. [#1119](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1119)
* "Tümünü yeniden dene" düzeltildi, başarısız bir blob yeniden adlandırma için çalışmıyor. [#992](https://www.github.com/Microsoft/AzureStorageExplorer/issues/992)
* Blob açılırken "Iptal" düzeltildi. [#1464](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1464)
* Ürün genelinde birden çok yazım ve araç ipucu sorunu düzeltildi. Bu sorunları bildiren çok teşekkürler! [#1303](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1303), [#1328](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1328), [#1329](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1329), [#1331](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1331), [#1336](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1336), [#1352](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1352), [#1368](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1368), [#1395](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1395)

### <a name="known-issues"></a>Bilinen Sorunlar

* AzCopy olmayan bir blob indirmesi gerçekleştirirken, büyük dosyalar için MD5 doğrulanmaz. Bunun nedeni, depolama SDK 'sindeki bir hatadır. [#1212](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1212)
* RBAC kullanılırken, depolama kaynaklarınıza erişebilmek için Depolama Gezgini bazı yönetim katmanı izinleri gerektirir. Daha fazla bilgi için bkz. [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) .
* Bir proxy 'nin arkasında ADLS 2. Bloblarına erişme girişimi başarısız olabilir.
* Bir blob kapsayıcısı gibi SAS URI 'SI aracılığıyla bağlı bir kaynağın ayrılması, diğer eklerin doğru şekilde görüntülenmesini engelleyen bir hataya neden olabilir. Bu sorunu geçici olarak çözmek için Grup düğümünü yenilemeniz yeterlidir. Daha fazla bilgi için bkz. #537.
* Mac için VS 'yi kullanırsanız ve şimdiye kadar özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu geçici olarak çözmek için ~/içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak sizi engellemez, bu sorunla ilgili yorum yapın.
* Azurite, tüm depolama API 'Lerini henüz tam olarak uygulamamıştır. Bu nedenle, geliştirme depolaması için Azurite kullanılırken beklenmeyen hatalar veya davranışlar olabilir.
* Nadir durumlarda, ağaç odağı hızlı erişime açık kalabilir. Odağı kaldırmak için, tümünü yenileyebilirsiniz.
* NodeJS içindeki bir hata nedeniyle OneDrive klasörünüzden karşıya yükleme işlemi çalışmıyor. Hata düzeltildi, ancak henüz elektron ile tümleştirildi. Blob kapsayıcısına yükleme veya indirme sırasında bu sorunu geçici olarak çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez. Azure Stack kaynaklarla çalışırken bu özellikleri kullanma girişimi beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
   * ADLS 2.
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux üzerinde Depolama Gezgini çalıştırmak için öncelikle bazı bağımlılıkların yüklenmesi gerekir. Daha fazla bilgi için Depolama Gezgini [sorun giderme kılavuzuna](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#linux-dependencies) bakın.

## <a name="version-181"></a>Sürüm 1.8.1
5/13/2019

### <a name="hotfixes"></a>Başlatmayla
* Bazı durumlarda, kaynak düzeyinde "daha fazla yükle" seçeneğine tıkladığınızda kaynakların sonraki sayfası döndürülmez. Bu düzeltildi. [#1359](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1359)
* Windows 'da, AzCopy indirmeleri, tek bir dosya veya klasör indiriliyorsa ve dosya ya da klasörün adı bir Windows yolu için geçersiz bir karakter içeriyorsa başarısız olur. Bu düzeltildi. [#1350](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1350)
* Son derece ender durumlarda, bir dosya paylaşımının yeniden adlandırılması veya bir dosya paylaşımında yeniden adlandırma gerçekleştirirken, yeniden adlandırma kopyaları başarısız olduysa veya depolama araştırması, kopyaların başarısını Azure ile doğrulayadıysa, o Depolama Gezgini silmek için olası bir durum oluştu. kopya tamamlanmadan önce riinal dosyalar. Bu düzeltildi.

### <a name="new"></a>Yeni

* Tümleşik AzCopy sürümü 10.1.0 sürümüne güncelleştirildi.
* Ctrl/Cmd + R artık odaklanmış olan düzenleyiciyi yenilemek için kullanılabilir. [#1097](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1097)
* Azure Stack Storage API sürümü 2017-04-17 olarak değiştirilmiştir.
* ADLS 2. için erişimi yönet Iletişim kutusu artık maskeyi diğer POSIX izinleri araçlarına benzer bir şekilde eşitlenmiş olarak tutacaktır. Kullanıcı veya grup izinlerinin maskenin sınırlarını aşmasına neden olan bir değişiklik yapıldığında UI da sizi uyarır. [#1253](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1253)
* AzCopy karşıya yüklemeleri için, MD5 karmasını hesaplamaya ve ayarlamaya yönelik bayrak artık etkinleştirilmiştir. [#1223](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1223)


### <a name="preview-features"></a>Önizleme Özellikleri

* Cihaz kod akışı oturum açma artık önizleme için kullanılabilir. Bunu etkinleştirmek için "Önizleme" → "cihaz kodu akışı oturum açma kullan" bölümüne gidin. Daha güvenilir bir oturum açma formu olduğunu kanıtlayabildiğinden, bu özelliği denemek için boş oturum açma pencereleri ile sorun yaşayan tüm kullanıcıları teşvik ediyoruz.
* AzCopy ile tümleştirilmiş Depolama Gezgini Şu anda önizleme için kullanılabilir. Bunu etkinleştirmek için "Preview" → "Gelişmiş blob yüklemesi ve Indirilmesi için AzCopy kullanın" bölümüne gidin. AzCopy ile tamamlanan blob aktarımları daha hızlı ve daha iyi performansa sahip olmalıdır.

### <a name="fixes"></a>Düzeltilen

* Erişim Ilkeleri iletişim kutusu artık, süresi dolmayan depolama erişim Ilkelerinde sona erme tarihi olarak ayarlanmayacaktır. [#764](https://www.github.com/Microsoft/AzureStorageExplorer/issues/764)
* Bir SAS oluştururken depolanan erişim Ilkelerinin doğru şekilde kullanıldığından emin olmak için SAS oluştur iletişim kutusunda bazı değişiklikler yapılmıştır. [#1269](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1269)
* 512 bayt hizalı bir dosyayı bir sayfa Blobuna yüklemeye çalışırken Depolama Gezgini artık daha ilgili bir hata ortaya çıkarır. [#1050](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1050)
* Bir görünen ad kullanan bir blob kapsayıcısının kopyalanması başarısız olur. Şimdi, blob kapsayıcısının gerçek adı kullanılır. [#1166](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1166)
* Adında Unicode karakterler bulunan bir ADLS 2. klasörü üzerinde belirli eylemler gerçekleştirmeye çalışılması başarısız olur. Tüm eylemler artık çalışmalıdır. [#980](https://www.github.com/Microsoft/AzureStorageExplorer/issues/980)

### <a name="known-issues"></a>Bilinen Sorunlar

* AzCopy olmayan bir blob indirmesi gerçekleştirirken, büyük dosyalar için MD5 doğrulanmaz. Bunun nedeni, depolama SDK 'sindeki bir hatadır. [#1212](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1212)
* RBAC kullanılırken, depolama kaynaklarınıza erişebilmek için Depolama Gezgini bazı yönetim katmanı izinleri gerektirir. Daha fazla bilgi için bkz. [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) .
* Bir proxy 'nin arkasında ADLS 2. Bloblarına erişme girişimi başarısız olabilir.
* Bir blob kapsayıcısı gibi SAS URI 'SI aracılığıyla bağlı bir kaynağın ayrılması, diğer eklerin doğru şekilde görüntülenmesini engelleyen bir hataya neden olabilir. Bu sorunu geçici olarak çözmek için Grup düğümünü yenilemeniz yeterlidir. Daha fazla bilgi için bkz. #537.
* Mac için VS 'yi kullanırsanız ve şimdiye kadar özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu geçici olarak çözmek için ~/içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak sizi engellemez, bu sorunla ilgili yorum yapın.
* Azurite, tüm depolama API 'Lerini henüz tam olarak uygulamamıştır. Bu nedenle, geliştirme depolaması için Azurite kullanılırken beklenmeyen hatalar veya davranışlar olabilir.
* Nadir durumlarda, ağaç odağı hızlı erişime açık kalabilir. Odağı kaldırmak için, tümünü yenileyebilirsiniz.
* NodeJS içindeki bir hata nedeniyle OneDrive klasörünüzden karşıya yükleme işlemi çalışmıyor. Hata düzeltildi, ancak henüz elektron ile tümleştirildi. Blob kapsayıcısına yükleme veya indirme sırasında bu sorunu geçici olarak çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez. Azure Stack kaynaklarla çalışırken bu özellikleri kullanma girişimi beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
   * ADLS 2.
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux üzerinde Depolama Gezgini çalıştırmak için öncelikle bazı bağımlılıkların yüklenmesi gerekir. Daha fazla bilgi için Depolama Gezgini [sorun giderme kılavuzuna](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#linux-dependencies) bakın.

## <a name="version-180"></a>Sürüm 1.8.0
5/1/2019

### <a name="new"></a>Yeni

* Tümleşik AzCopy sürümü 10.1.0 sürümüne güncelleştirildi.
* Ctrl/Cmd + R artık odaklanmış olan düzenleyiciyi yenilemek için kullanılabilir. [#1097](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1097)
* Azure Stack Storage API sürümü 2017-04-17 olarak değiştirilmiştir.
* ADLS 2. için erişimi yönet Iletişim kutusu artık maskeyi diğer POSIX izinleri araçlarına benzer bir şekilde eşitlenmiş olarak tutacaktır. Kullanıcı veya grup izinlerinin maskenin sınırlarını aşmasına neden olan bir değişiklik yapıldığında UI da sizi uyarır. [#1253](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1253)
* AzCopy karşıya yüklemeleri için, MD5 karmasını hesaplamaya ve ayarlamaya yönelik bayrak artık etkinleştirilmiştir. [#1223](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1223)


### <a name="preview-features"></a>Önizleme Özellikleri

* Cihaz kod akışı oturum açma artık önizleme için kullanılabilir. Bunu etkinleştirmek için "Önizleme" → "cihaz kodu akışı oturum açma kullan" bölümüne gidin. Daha güvenilir bir oturum açma formu olduğunu kanıtlayabildiğinden, bu özelliği denemek için boş oturum açma pencereleri ile sorun yaşayan tüm kullanıcıları teşvik ediyoruz.
* AzCopy ile tümleştirilmiş Depolama Gezgini Şu anda önizleme için kullanılabilir. Bunu etkinleştirmek için "Preview" → "Gelişmiş blob yüklemesi ve Indirilmesi için AzCopy kullanın" bölümüne gidin. AzCopy ile tamamlanan blob aktarımları daha hızlı ve daha iyi performansa sahip olmalıdır.

### <a name="fixes"></a>Düzeltilen

* Erişim Ilkeleri iletişim kutusu artık, süresi dolmayan depolama erişim Ilkelerinde sona erme tarihi olarak ayarlanmayacaktır. [#764](https://www.github.com/Microsoft/AzureStorageExplorer/issues/764)
* Bir SAS oluştururken depolanan erişim Ilkelerinin doğru şekilde kullanıldığından emin olmak için SAS oluştur iletişim kutusunda bazı değişiklikler yapılmıştır. [#1269](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1269)
* 512 bayt hizalı bir dosyayı bir sayfa Blobuna yüklemeye çalışırken Depolama Gezgini artık daha ilgili bir hata ortaya çıkarır. [#1050](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1050)
* Bir görünen ad kullanan bir blob kapsayıcısının kopyalanması başarısız olur. Şimdi, blob kapsayıcısının gerçek adı kullanılır. [#1166](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1166)
* Adında Unicode karakterler bulunan bir ADLS 2. klasörü üzerinde belirli eylemler gerçekleştirmeye çalışılması başarısız olur. Tüm eylemler artık çalışmalıdır. [#980](https://www.github.com/Microsoft/AzureStorageExplorer/issues/980)

### <a name="known-issues"></a>Bilinen Sorunlar

* AzCopy olmayan bir blob indirmesi gerçekleştirirken, büyük dosyalar için MD5 doğrulanmaz. Bunun nedeni, depolama SDK 'sindeki bir hatadır. [#1212](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1212)
* RBAC kullanılırken, depolama kaynaklarınıza erişebilmek için Depolama Gezgini bazı yönetim katmanı izinleri gerektirir. Daha fazla bilgi için bkz. [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) .
* Bir proxy 'nin arkasında ADLS 2. Bloblarına erişme girişimi başarısız olabilir.
* Bir blob kapsayıcısı gibi SAS URI 'SI aracılığıyla bağlı bir kaynağın ayrılması, diğer eklerin doğru şekilde görüntülenmesini engelleyen bir hataya neden olabilir. Bu sorunu geçici olarak çözmek için Grup düğümünü yenilemeniz yeterlidir. Daha fazla bilgi için bkz. #537.
* Mac için VS 'yi kullanırsanız ve şimdiye kadar özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu geçici olarak çözmek için ~/içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak sizi engellemez, bu sorunla ilgili yorum yapın.
* Azurite, tüm depolama API 'Lerini henüz tam olarak uygulamamıştır. Bu nedenle, geliştirme depolaması için Azurite kullanılırken beklenmeyen hatalar veya davranışlar olabilir.
* Nadir durumlarda, ağaç odağı hızlı erişime açık kalabilir. Odağı kaldırmak için, tümünü yenileyebilirsiniz.
* NodeJS içindeki bir hata nedeniyle OneDrive klasörünüzden karşıya yükleme işlemi çalışmıyor. Hata düzeltildi, ancak henüz elektron ile tümleştirildi. Blob kapsayıcısına yükleme veya indirme sırasında bu sorunu geçici olarak çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez. Azure Stack kaynaklarla çalışırken bu özellikleri kullanma girişimi beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
   * ADLS 2.
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux üzerinde Depolama Gezgini çalıştırmak için öncelikle bazı bağımlılıkların yüklenmesi gerekir. Daha fazla bilgi için Depolama Gezgini [sorun giderme kılavuzuna](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#linux-dependencies) bakın.

## <a name="version-170"></a>Sürüm 1.7.0
3/5/2019

### <a name="download-azure-storage-explorer-170"></a>Azure Depolama Gezgini 1.7.0 indir
- [Windows için Azure Depolama Gezgini 1.7.0](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Mac için Azure Depolama Gezgini 1.7.0](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Linux için Azure Depolama Gezgini 1.7.0](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a>Yeni

* Artık ADLS 2. kapsayıcısı, dosya veya klasör için erişimi yönetirken sahibi ve sahip grubunu değiştirebilirsiniz.
* Windows 'da, ürünün içinden Depolama Gezgini güncelleştirmek artık artımlı bir yüklemedir. Bu, daha hızlı bir güncelleştirme deneyimine neden olmalıdır. Temiz yüklemeyi tercih ediyorsanız, [yükleyiciyi](https://azure.microsoft.com/features/storage-explorer/) kendiniz indirip el ile yükleyebilirsiniz. #1089

### <a name="preview-features"></a>Önizleme Özellikleri

* Cihaz kod akışı oturum açma artık önizleme için kullanılabilir. Bunu etkinleştirmek için "Önizleme" → "cihaz kodu akışı oturum açma kullan" bölümüne gidin. Daha güvenilir bir oturum açma formu olduğunu kanıtlayabildiğinden, bu özelliği denemek için boş oturum açma pencereleri ile sorun yaşayan tüm kullanıcıları teşvik ediyoruz. #938
* AzCopy ile tümleştirilmiş Depolama Gezgini Şu anda önizleme için kullanılabilir. Bunu etkinleştirmek için "Preview" → "Gelişmiş blob yüklemesi ve Indirilmesi için AzCopy kullanın" bölümüne gidin. AzCopy ile tamamlanan blob aktarımları daha hızlı ve daha iyi performansa sahip olmalıdır.

### <a name="fixes"></a>Düzeltilen

* Şimdi, AzCopy özelliği etkinken karşıya yüklemek istediğiniz blob türünü seçebilirsiniz. #1111
* Daha önce, bir ADLS 2. depolama hesabı için statik Web siteleri etkinleştirdiyseniz ve sonra adı ve anahtarı ile iliştirdiyseniz, Depolama Gezgini hiyerarşik ad alanının etkinleştirildiğini tespit etmez. Bu düzeltildi. #1081
* Blob düzenleyicisinde, kalan bekletme günlerine veya duruma göre sıralama kesildi. Bu düzeltildi. #1106
* 1\.5.0 sonra, Depolama Gezgini yeniden adlandırma veya kopyalama & yapıştırma sırasında başarılı bir şekilde bildirilmemesi için artık sunucu tarafı kopyalarının bitmesini beklememiş olmaz. Bu düzeltildi. #976
* Deneysel AzCopy özelliği kullanılırken, "panoya kopyala" komutuna tıklandıktan sonra kopyalanmış komut her zaman kendi başına çağrılmamalıdır. Şimdi, aktarımı el ile çalıştırmak için gereken tüm komutlar kopyalanacaktır. #1079
* Daha önce, bir proxy 'nin arkasındaysa ADLS 2. bloblara erişilemez. Bunun nedeni, depolama SDK 'Sı tarafından kullanılan yeni bir ağ kitaplığındaki hatadır. 1\.7.0 içinde, bu sorunu azaltma girişimi yapılmıştır ancak bazı kişiler sorunları görmeye devam edebilir. Gelecekteki bir güncelleştirmede tam bir onarım yayımlanacak. #1090
* 1\.7.0 ' de, dosyayı Kaydet iletişim kutusu artık dosyayı kaydettiğiniz son konumu doğru şekilde anımsar. #16
* Özellikler panelinde, bir depolama hesabının SKU katmanı, hesap türü olarak gösteriliyor. Bu düzeltildi. #654
* Bazen Blobun adını doğru bir şekilde girmiş olsanız bile bir Blobun kiralamasını bölmek imkansız oldu. Bu düzeltildi. #1070

### <a name="known-issues"></a>Bilinen Sorunlar

* RBAC kullanılırken, depolama kaynaklarınıza erişebilmek için Depolama Gezgini bazı yönetim katmanı izinleri gerektirir. Daha fazla bilgi için bkz. [sorun giderme kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) .
* Bir proxy 'nin arkasında ADLS 2. Bloblarına erişme girişimi başarısız olabilir.
* Bir blob kapsayıcısı gibi SAS URI 'SI aracılığıyla bağlı bir kaynağın ayrılması, diğer eklerin doğru şekilde görüntülenmesini engelleyen bir hataya neden olabilir. Bu sorunu geçici olarak çözmek için Grup düğümünü yenilemeniz yeterlidir. Daha fazla bilgi için bkz. #537.
* Bir blob kapsayıcısı gibi SAS URI 'SI aracılığıyla bağlı bir kaynağın ayrılması, diğer eklerin doğru şekilde görüntülenmesini engelleyen bir hataya neden olabilir. Bu sorunu geçici olarak çözmek için Grup düğümünü yenilemeniz yeterlidir. Daha fazla bilgi için bkz. #537.
* Mac için VS 'yi kullanırsanız ve şimdiye kadar özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu geçici olarak çözmek için ~/içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak sizi engellemez, bu sorunla ilgili yorum yapın.
* Azurite, tüm depolama API 'Lerini henüz tam olarak uygulamamıştır. Bu nedenle, geliştirme depolaması için Azurite kullanılırken beklenmeyen hatalar veya davranışlar olabilir.
* Nadir durumlarda, ağaç odağı hızlı erişime açık kalabilir. Odağı kaldırmak için, tümünü yenileyebilirsiniz.
* NodeJS içindeki bir hata nedeniyle OneDrive klasörünüzden karşıya yükleme işlemi çalışmıyor. Hata düzeltildi, ancak henüz elektron ile tümleştirildi. Blob kapsayıcısına yükleme veya indirme sırasında bu sorunu geçici olarak çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez. Azure Stack kaynaklarla çalışırken bu özellikleri kullanma girişimi beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2,0](https://dotnet.microsoft.com/download/dotnet-core/2.0)' i yüklemeniz gerekir.
* Ubuntu 14,04 üzerindeki kullanıcılar için GCC 'nin güncel olduğundan emin olmanız gerekir. Bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenize yeniden başlatılarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17,04 üzerindeki kullanıcılar için GConf yüklemeniz gerekir-bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-162"></a>Sürüm 1.6.2
1/9/2019

### <a name="hotfixes"></a>Başlatmayla
* 1\.6.1 ' de, kullanıcıların her zaman grup olarak eklendiği ObjectID tarafından ADLS 2. ACL 'Lerine eklenen varlıklar. Şimdi, gruplar olarak yalnızca gruplar eklenir ve kurumsal uygulamalar ve hizmet sorumluları gibi varlıklar Kullanıcı olarak eklenir. [#1049](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1049)
* Bir ADLS 2. depolama hesabının kapsayıcısı yoksa ve ad ve anahtarla iliştirilmişse, Depolama Gezgini depolama hesabının ADLS 2. algılamamalıdır. Bu düzeltildi. [#1048](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1048)
* 1\.6.0 ' de, kopyalama ve yapıştırma sırasında çakışmalar bir çözüm istemez. Bunun yerine, çakışan kopya yalnızca başarısız olur. Şimdi, ilk çakışmada nasıl çözümlenmek istediğimize sorulur. [#1014](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1014)
* API sınırlamaları nedeniyle, erişimi Yönet iletişim kutusundaki ObjectIDs 'nin tüm doğrulaması devre dışı bırakıldı. Doğrulama artık yalnızca Kullanıcı UPN 'leri için gerçekleştirilir. [#954](https://www.github.com/Microsoft/AzureStorageExplorer/issues/954)
* ADLS 2. erişimi Yönet iletişim kutusunda, bir grup için izinler değiştirilemedi. Bu düzeltildi. [#958](https://www.github.com/Microsoft/AzureStorageExplorer/issues/958)
* ADLS 2. düzenleyicisine sürükle ve bırak yükleme desteği eklendi. [#953](https://www.github.com/Microsoft/AzureStorageExplorer/issues/953)
* ADLS 2. dosyaları ve klasörleri için Özellikler iletişim kutusunda URL özelliği bazen '/' olarak eksik. Bu düzeltildi. [#960](https://www.github.com/Microsoft/AzureStorageExplorer/issues/960)
* Bir ADLS 2. kapsayıcısı, dosya veya klasör için geçerli izinleri almak başarısız olursa, hata artık etkinlik günlüğünde bir özellik olarak görüntülenir. [#965](https://www.github.com/Microsoft/AzureStorageExplorer/issues/965)
* Dosyaları açmak için oluşturulan geçici yol, Windows üzerinde MAX_PATH daha uzun bir yol oluşturma olasılığını azaltmak için kısaltıldı. [#93](https://www.github.com/Microsoft/AzureStorageExplorer/issues/93)
* Oturum açan kullanıcılar yoksa ve hiçbir kaynak iliştirilmişse Bağlan iletişim kutusu artık doğru şekilde görünür. [#944](https://www.github.com/Microsoft/AzureStorageExplorer/issues/944)
* 1\.6.0 ' de, HNS olmayan Bloblar ve dosyalar için özellikleri kaydetmek her özelliğin değerini kodlayabilir. Bu, yalnızca ASCII karakter içeren değerlerin gereksiz şekilde kodlamasına neden olur. Artık değerler ASCII olmayan karakterler içeriyorsa kodlanacak. [#986](https://www.github.com/Microsoft/AzureStorageExplorer/issues/986)
* Bir SAS kullanılıyorsa ve SAS okuma izinlerine sahip değilse, bir klasörü HNS olmayan bir blob kapsayıcısına yüklemek başarısız olur. Bu düzeltildi. [#970](https://www.github.com/Microsoft/AzureStorageExplorer/issues/970)
* AzCopy aktarımının iptali çalışmadı. Bu düzeltildi. [#943](https://www.github.com/Microsoft/AzureStorageExplorer/issues/943)
* Klasörün adında boşluklar varsa, AzCopy bir ADLS 2. blob kapsayıcısından bir klasör indirmeye çalışırken başarısız olur. Bu düzeltildi. [#990](https://www.github.com/Microsoft/AzureStorageExplorer/issues/990)
* CosmosDB Düzenleyicisi 1.6.0 içinde kopuk. Artık düzeltildi. [#950](https://www.github.com/Microsoft/AzureStorageExplorer/issues/950)
        
### <a name="new"></a>Yeni

* Artık Depolama Gezgini, [RBAC](https://go.microsoft.com/fwlink/?linkid=2045904&clcid=0x409)aracılığıyla blob verilerinize erişmek için kullanabilirsiniz. Oturum açtıysanız ve Depolama Gezgini depolama hesabınız için anahtarları alamadıysanız, verilerinizle etkileşim kurarken kimlik doğrulamak için bir OAuth belirteci kullanılır.
* Depolama Gezgini artık ADLS 2. Storage hesaplarını desteklemektedir. Depolama Gezgini, bir depolama hesabı için hiyerarşik ad alanının etkinleştirildiğini algıladığında, depolama hesabınızın adının yanında "(ADLS 2. Önizleme)" görürsünüz. Depolama Gezgini, oturum açtığınızda hiyerarşik ad alanının etkinleştirilip etkinleştirilmeyeceğini algılayabilir veya depolama hesabınızı ad ve anahtar ile iliştiriyor demektir. ADLS 2. depolama hesapları için Depolama Gezgini şu şekilde kullanabilirsiniz:
  * Kapsayıcılar oluşturma ve silme
  * Kapsayıcı özelliklerini ve izinlerini yönetme (sol taraftaki)
  * Kapsayıcıların içindeki verileri görüntüleme ve gezinme
  * Yeni Klasörler oluştur
  * Dosya ve klasörleri karşıya yükleme, indirme, yeniden adlandırma ve silme
  * Dosya ve klasör özelliklerini ve izinlerini yönetin (sağ taraftaki).
    
    Geçici silme ve anlık görüntü gibi diğer tipik blob özellikleri şu anda kullanılamıyor. İzinleri yönetme yalnızca oturum açıldığında kullanılabilir. Ayrıca, bir ADLS 2. depolama hesabında çalışırken, Depolama Gezgini tüm yüklemeler ve indirmeler için AzCopy ve varsa tüm işlemler için ad ve anahtar kimlik bilgilerini kullanır.
* Güçlü Kullanıcı geri bildirimlerinden sonra, birden fazla blobda kiraları aynı anda bölmek için bir kez daha yeniden kira kullanılabilir.

### <a name="known-issues"></a>Bilinen Sorunlar

* Bir ADLS 2. Storage hesabından indirme sırasında, aktarılmakta olan dosyalardan biri zaten mevcutsa AzCopy bazen kilitlenmez. Bu, yaklaşan bir düzeltmede düzeltilecektir.
* Bir blob kapsayıcısı gibi SAS URI 'SI aracılığıyla bağlı bir kaynağın ayrılması, diğer eklerin doğru şekilde görüntülenmesini engelleyen bir hataya neden olabilir. Bu sorunu geçici olarak çözmek için Grup düğümünü yenilemeniz yeterlidir. Daha fazla bilgi için bkz. #537.
* Mac için VS 'yi kullanırsanız ve şimdiye kadar özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu geçici olarak çözmek için ~/içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak sizi engellemez, bu sorunla ilgili yorum yapın.
* Azurite, tüm depolama API 'Lerini henüz tam olarak uygulamamıştır. Bu nedenle, geliştirme depolaması için Azurite kullanılırken beklenmeyen hatalar veya davranışlar olabilir.
* Nadir durumlarda, ağaç odağı hızlı erişime açık kalabilir. Odağı kaldırmak için, tümünü yenileyebilirsiniz.
* NodeJS içindeki bir hata nedeniyle OneDrive klasörünüzden karşıya yükleme işlemi çalışmıyor. Hata düzeltildi, ancak henüz elektron ile tümleştirildi. Blob kapsayıcısına yükleme veya indirme sırasında bu sorunu geçici olarak çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez. Azure Stack kaynaklarla çalışırken bu özellikleri kullanma girişimi beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2,0](https://dotnet.microsoft.com/download/dotnet-core/2.0)' i yüklemeniz gerekir.
* Ubuntu 14,04 üzerindeki kullanıcılar için GCC 'nin güncel olduğundan emin olmanız gerekir. Bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenize yeniden başlatılarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17,04 üzerindeki kullanıcılar için GConf yüklemeniz gerekir-bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-161"></a>Sürüm 1.6.1
12/18/2018

### <a name="hotfixes"></a>Başlatmayla
* API sınırlamaları nedeniyle, erişimi Yönet iletişim kutusundaki ObjectIDs 'nin tüm doğrulaması devre dışı bırakıldı. Doğrulama artık yalnızca Kullanıcı UPN 'leri için gerçekleştirilir. [#954](https://www.github.com/Microsoft/AzureStorageExplorer/issues/954)
* ADLS 2. erişimi Yönet iletişim kutusunda, bir grup için izinler değiştirilemedi. Bu düzeltildi. [#958](https://www.github.com/Microsoft/AzureStorageExplorer/issues/958)
* ADLS 2. düzenleyicisine sürükle ve bırak yükleme desteği eklendi. [#953](https://www.github.com/Microsoft/AzureStorageExplorer/issues/953)
* ADLS 2. dosyaları ve klasörleri için Özellikler iletişim kutusunda URL özelliği bazen '/' olarak eksik. Bu düzeltildi. [#960](https://www.github.com/Microsoft/AzureStorageExplorer/issues/960)
* Bir ADLS 2. kapsayıcısı, dosya veya klasör için geçerli izinleri almak başarısız olursa, hata artık etkinlik günlüğünde bir özellik olarak görüntülenir. [#965](https://www.github.com/Microsoft/AzureStorageExplorer/issues/965)
* Dosyaları açmak için oluşturulan geçici yol, Windows üzerinde MAX_PATH daha uzun bir yol oluşturma olasılığını azaltmak için kısaltıldı. [#93](https://www.github.com/Microsoft/AzureStorageExplorer/issues/93)
* Oturum açan kullanıcılar yoksa ve hiçbir kaynak iliştirilmişse Bağlan iletişim kutusu artık doğru şekilde görünür. [#944](https://www.github.com/Microsoft/AzureStorageExplorer/issues/944)
* 1\.6.0 ' de, HNS olmayan Bloblar ve dosyalar için özellikleri kaydetmek her özelliğin değerini kodlayabilir. Bu, yalnızca ASCII karakter içeren değerlerin gereksiz şekilde kodlamasına neden olur. Artık değerler ASCII olmayan karakterler içeriyorsa kodlanacak. [#986](https://www.github.com/Microsoft/AzureStorageExplorer/issues/986)
* Bir SAS kullanılıyorsa ve SAS okuma izinlerine sahip değilse, bir klasörü HNS olmayan bir blob kapsayıcısına yüklemek başarısız olur. Bu düzeltildi. [#970](https://www.github.com/Microsoft/AzureStorageExplorer/issues/970)
* AzCopy aktarımının iptali çalışmadı. Bu düzeltildi. [#943](https://www.github.com/Microsoft/AzureStorageExplorer/issues/943)
* Klasörün adında boşluklar varsa, AzCopy bir ADLS 2. blob kapsayıcısından bir klasör indirmeye çalışırken başarısız olur. Bu düzeltildi. [#990](https://www.github.com/Microsoft/AzureStorageExplorer/issues/990)
* CosmosDB Düzenleyicisi 1.6.0 içinde kopuk. Artık düzeltildi. [#950](https://www.github.com/Microsoft/AzureStorageExplorer/issues/950)
        
### <a name="new"></a>Yeni

* Artık Depolama Gezgini, [RBAC](https://go.microsoft.com/fwlink/?linkid=2045904&clcid=0x409)aracılığıyla blob verilerinize erişmek için kullanabilirsiniz. Oturum açtıysanız ve Depolama Gezgini depolama hesabınız için anahtarları alamadıysanız, verilerinizle etkileşim kurarken kimlik doğrulamak için bir OAuth belirteci kullanılır.
* Depolama Gezgini artık ADLS 2. Storage hesaplarını desteklemektedir. Depolama Gezgini, bir depolama hesabı için hiyerarşik ad alanının etkinleştirildiğini algıladığında, depolama hesabınızın adının yanında "(ADLS 2. Önizleme)" görürsünüz. Depolama Gezgini, oturum açtığınızda hiyerarşik ad alanının etkinleştirilip etkinleştirilmeyeceğini algılayabilir veya depolama hesabınızı ad ve anahtar ile iliştiriyor demektir. ADLS 2. depolama hesapları için Depolama Gezgini şu şekilde kullanabilirsiniz:
  * Kapsayıcılar oluşturma ve silme
  * Kapsayıcı özelliklerini ve izinlerini yönetme (sol taraftaki)
  * Kapsayıcıların içindeki verileri görüntüleme ve gezinme
  * Yeni Klasörler oluştur
  * Dosya ve klasörleri karşıya yükleme, indirme, yeniden adlandırma ve silme
  * Dosya ve klasör özelliklerini ve izinlerini yönetin (sağ taraftaki).
    
    Geçici silme ve anlık görüntü gibi diğer tipik blob özellikleri şu anda kullanılamıyor. İzinleri yönetme yalnızca oturum açıldığında kullanılabilir. Ayrıca, bir ADLS 2. depolama hesabında çalışırken, Depolama Gezgini tüm yüklemeler ve indirmeler için AzCopy ve varsa tüm işlemler için ad ve anahtar kimlik bilgilerini kullanır.
* Güçlü Kullanıcı geri bildirimlerinden sonra, birden fazla blobda kiraları aynı anda bölmek için bir kez daha yeniden kira kullanılabilir.

### <a name="known-issues"></a>Bilinen Sorunlar

* Bir ADLS 2. Storage hesabından indirme sırasında, aktarılmakta olan dosyalardan biri zaten mevcutsa AzCopy bazen kilitlenmez. Bu, yaklaşan bir düzeltmede düzeltilecektir.
* Bir blob kapsayıcısı gibi SAS URI 'SI aracılığıyla bağlı bir kaynağın ayrılması, diğer eklerin doğru şekilde görüntülenmesini engelleyen bir hataya neden olabilir. Bu sorunu geçici olarak çözmek için Grup düğümünü yenilemeniz yeterlidir. Daha fazla bilgi için bkz. #537.
* Mac için VS 'yi kullanırsanız ve şimdiye kadar özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu geçici olarak çözmek için ~/içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak sizi engellemez, bu sorunla ilgili yorum yapın.
* Azurite, tüm depolama API 'Lerini henüz tam olarak uygulamamıştır. Bu nedenle, geliştirme depolaması için Azurite kullanılırken beklenmeyen hatalar veya davranışlar olabilir.
* Nadir durumlarda, ağaç odağı hızlı erişime açık kalabilir. Odağı kaldırmak için, tümünü yenileyebilirsiniz.
* NodeJS içindeki bir hata nedeniyle OneDrive klasörünüzden karşıya yükleme işlemi çalışmıyor. Hata düzeltildi, ancak henüz elektron ile tümleştirildi. Blob kapsayıcısına yükleme veya indirme sırasında bu sorunu geçici olarak çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez. Azure Stack kaynaklarla çalışırken bu özellikleri kullanma girişimi beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2,0](https://dotnet.microsoft.com/download/dotnet-core/2.0)' i yüklemeniz gerekir.
* Ubuntu 14,04 üzerindeki kullanıcılar için GCC 'nin güncel olduğundan emin olmanız gerekir. Bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenize yeniden başlatılarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17,04 üzerindeki kullanıcılar için GConf yüklemeniz gerekir-bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-160"></a>Sürüm 1.6.0
05.12.2018

### <a name="new"></a>Yeni

* Artık Depolama Gezgini, [RBAC](https://go.microsoft.com/fwlink/?linkid=2045904&clcid=0x409)aracılığıyla blob verilerinize erişmek için kullanabilirsiniz. Oturum açtıysanız ve Depolama Gezgini depolama hesabınız için anahtarları alamadıysanız, verilerinizle etkileşim kurarken kimlik doğrulamak için bir OAuth belirteci kullanılır.
* Depolama Gezgini artık ADLS 2. Storage hesaplarını desteklemektedir. Depolama Gezgini, bir depolama hesabı için hiyerarşik ad alanının etkinleştirildiğini algıladığında, depolama hesabınızın adının yanında "(ADLS 2. Önizleme)" görürsünüz. Depolama Gezgini, oturum açtığınızda hiyerarşik ad alanının etkinleştirilip etkinleştirilmeyeceğini algılayabilir veya depolama hesabınızı ad ve anahtar ile iliştiriyor demektir. ADLS 2. depolama hesapları için Depolama Gezgini şu şekilde kullanabilirsiniz:
  * Kapsayıcılar oluşturma ve silme
  * Kapsayıcı özelliklerini ve izinlerini yönetme (sol taraftaki)
  * Kapsayıcıların içindeki verileri görüntüleme ve gezinme
  * Yeni Klasörler oluştur
  * Dosya ve klasörleri karşıya yükleme, indirme, yeniden adlandırma ve silme
  * Dosya ve klasör özelliklerini ve izinlerini yönetin (sağ taraftaki).
    
    Geçici silme ve anlık görüntü gibi diğer tipik blob özellikleri şu anda kullanılamıyor. İzinleri yönetme yalnızca oturum açıldığında kullanılabilir. Ayrıca, bir ADLS 2. depolama hesabında çalışırken, Depolama Gezgini tüm yüklemeler ve indirmeler için AzCopy ve varsa tüm işlemler için ad ve anahtar kimlik bilgilerini kullanır.
* Güçlü Kullanıcı geri bildirimlerinden sonra, birden fazla blobda kiraları aynı anda bölmek için bir kez daha yeniden kira kullanılabilir.

### <a name="known-issues"></a>Bilinen Sorunlar

* Bir ADLS 2. Storage hesabından indirme sırasında, aktarılmakta olan dosyalardan biri zaten mevcutsa AzCopy bazen kilitlenmez. Bu, yaklaşan bir düzeltmede düzeltilecektir.
* Bir blob kapsayıcısı gibi SAS URI 'SI aracılığıyla bağlı bir kaynağın ayrılması, diğer eklerin doğru şekilde görüntülenmesini engelleyen bir hataya neden olabilir. Bu sorunu geçici olarak çözmek için Grup düğümünü yenilemeniz yeterlidir. Daha fazla bilgi için bkz. #537.
* Mac için VS 'yi kullanırsanız ve şimdiye kadar özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu geçici olarak çözmek için ~/içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak sizi engellemez, bu sorunla ilgili yorum yapın.
* Azurite, tüm depolama API 'Lerini henüz tam olarak uygulamamıştır. Bu nedenle, geliştirme depolaması için Azurite kullanılırken beklenmeyen hatalar veya davranışlar olabilir.
* Nadir durumlarda, ağaç odağı hızlı erişime açık kalabilir. Odağı kaldırmak için, tümünü yenileyebilirsiniz.
* NodeJS içindeki bir hata nedeniyle OneDrive klasörünüzden karşıya yükleme işlemi çalışmıyor. Hata düzeltildi, ancak henüz elektron ile tümleştirildi. Blob kapsayıcısına yükleme veya indirme sırasında bu sorunu geçici olarak çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez. Azure Stack kaynaklarla çalışırken bu özellikleri kullanma girişimi beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2,0](https://dotnet.microsoft.com/download/dotnet-core/2.0)' i yüklemeniz gerekir.
* Ubuntu 14,04 üzerindeki kullanıcılar için GCC 'nin güncel olduğundan emin olmanız gerekir. Bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenize yeniden başlatılarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17,04 üzerindeki kullanıcılar için GConf yüklemeniz gerekir-bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-150"></a>Sürüm 1.5.0
10/29/2018

### <a name="new"></a>Yeni

* Blob 'Ları karşıya yüklemek ve indirmek için artık [AzCopy ile v10 arasındaki (Önizleme)](https://github.com/Azure/azure-storage-azcopy) kullanabilirsiniz. Bu özelliği etkinleştirmek için "deneysel" menüsüne gidin ve "geliştirilmiş blob karşıya yükleme ve Indirme için AzCopy kullan" düğmesine tıklayın. Etkinleştirildiğinde AzCopy aşağıdaki senaryolarda kullanılacaktır:
   * Araç çubuğu veya sürükleyip bırakma aracılığıyla blob kapsayıcılarına klasör ve dosya yükleme.
   * Klasörler ve dosyalar araç çubuğu veya bağlam menüsü aracılığıyla indiriliyor.

* Ayrıca, AzCopy kullanılırken:
   * Panonuza aktarımı yürütmek için kullanılan AzCopy komutunu kopyalayabilirsiniz. Etkinlik günlüğündeki "AzCopy komutunu panoya kopyala" seçeneğine tıklamanız yeterlidir.
   * Karşıya yükledikten sonra blob düzenleyicisini el ile yenilemeniz gerekir.
   * Karşıya dosya ekleme bloblarına yükleme desteklenmez ve VHD dosyaları sayfa Blobları olarak karşıya yüklenir ve diğer tüm dosyalar blok blob 'ları olarak karşıya yüklenir.
   * Karşıya yükleme veya indirme sırasında oluşan hatalar ve çakışmalar, bir karşıya yükleme veya indirme işlemi tamamlanana kadar ortaya çıkmış olur.

Son olarak, dosya paylaşımları ile AzCopy kullanma desteği gelecekte sunulacaktır.
* Depolama Gezgini artık elektron sürüm 2.0.11 kullanıyor.
* Artık kiralamalar aynı anda yalnızca bir blob üzerinde gerçekleştirilebilir. Ayrıca, kirasının parçalara ayrılma durumunda olan Blobun adını girmeniz gerekir. Bu değişiklik, özellikle VM 'Ler için bir kirayı yanlışlıkla bozma olasılığını azaltmak üzere yapılmıştır. #394
* Oturum açma sorunlarıyla karşılaşırsanız, şimdi kimlik doğrulamasını sıfırlamayı deneyebilirsiniz. Bu özelliğe erişmek için "yardım" menüsüne gidin ve "Sıfırla" seçeneğine tıklayın. #419

### <a name="fix"></a>Onar

* Güçlü Kullanıcı geri bildirimlerinden sonra, varsayılan öykünücü düğüm yeniden etkinleştirilmiştir. Connect iletişim kutusu aracılığıyla ek öykünücü bağlantıları ekleyebilirsiniz, ancak öykünücü varsayılan bağlantı noktalarını kullanacak şekilde yapılandırıldıysa, "öykünücü * varsayılan bağlantı noktaları" düğümünü "yerel & bağlı/depolama hesapları" altında da kullanabilirsiniz. #669
* Depolama Gezgini, artık başında veya sonunda boşluk olan blob meta veri değerlerini ayarlamanıza izin vermez. #760
* "Oturum aç" düğmesi her zaman Bağlan iletişim kutusunun aynı sayfalarında etkindir. Uygun olduğunda artık devre dışıdır. #761
* Hızlı erişim, hiçbir hızlı erişim öğesi eklenmemiş olduğunda artık konsolda bir hata oluşturmaz.

### <a name="known-issues"></a>Bilinen Sorunlar

* Bir blob kapsayıcısı gibi SAS URI 'SI aracılığıyla bağlı bir kaynağın ayrılması, diğer eklerin doğru şekilde görüntülenmesini engelleyen bir hataya neden olabilir. Bu sorunu geçici olarak çözmek için Grup düğümünü yenilemeniz yeterlidir. Daha fazla bilgi için bkz. #537.
* Mac için VS 'yi kullanırsanız ve şimdiye kadar özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu geçici olarak çözmek için ~/içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak sizi engellemez, bu sorunla ilgili yorum yapın.
* Azurite, tüm depolama API 'Lerini henüz tam olarak uygulamamıştır. Bu nedenle, geliştirme depolaması için Azurite kullanılırken beklenmeyen hatalar veya davranışlar olabilir.
* Nadir durumlarda, ağaç odağı hızlı erişime açık kalabilir. Odağı kaldırmak için, tümünü yenileyebilirsiniz.
* NodeJS içindeki bir hata nedeniyle OneDrive klasörünüzden karşıya yükleme işlemi çalışmıyor. Hata düzeltildi, ancak henüz elektron ile tümleştirildi. Blob kapsayıcısına yükleme veya indirme sırasında bu sorunu geçici olarak çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez. Azure Stack kaynaklarla çalışırken bu özellikleri kullanma girişimi beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2,0](https://dotnet.microsoft.com/download/dotnet-core/2.0)' i yüklemeniz gerekir.
* Ubuntu 14,04 üzerindeki kullanıcılar için GCC 'nin güncel olduğundan emin olmanız gerekir. Bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenize yeniden başlatılarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17,04 üzerindeki kullanıcılar için GConf yüklemeniz gerekir-bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```


## <a name="version-144"></a>Sürüm 1.4.4
10/15/2018

### <a name="hotfixes"></a>Başlatmayla
* Azure Kaynak yönetimi API 'SI sürümü, Azure ABD kamu kullanıcılarının engellemesini kaldırmak için geri alındı. [#696](https://github.com/Microsoft/AzureStorageExplorer/issues/696)
* Sayaç yükleme artık Depolama Gezgini tarafından kullanılan GPU miktarını azaltmak için CSS animasyonlarını kullanıyor. [#653](https://github.com/Microsoft/AzureStorageExplorer/issues/653)

### <a name="new"></a>Yeni
* SAS bağlantıları ve Öykünücüler gibi dış kaynak ekleri önemli ölçüde geliştirilmiştir. Artık şunları yapabilirsiniz:
   * İliştirmekte olduğunuz kaynağın görünen adını özelleştirin. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Farklı bağlantı noktaları kullanarak birden çok yerel öykünücüye ekleyin. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Hızlı erişime eklenen kaynakları ekleyin. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Depolama Gezgini artık geçici silme destekleniyor. Yapabilecekleriniz:
   * Depolama hesabınızın blob kapsayıcıları düğümüne sağ tıklayarak bir geçici silme ilkesi yapılandırın.
   * Gezinti çubuğunun yanındaki açılan menüde bulunan "etkin ve silinen Bloblar" seçeneğini belirleyerek blob düzenleyicisinde geçici olarak silinen Blobları görüntüleyin.
   * Geçici silinen Blobları geri al.

### <a name="fixes"></a>Düzeltilen
* Premium Depolama hesapları CORS 'yi desteklemediğinden, "CORS ayarlarını yapılandır" eylemi artık Premium Depolama hesaplarında kullanılamaz. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Artık SAS ekli Hizmetleri için paylaşılan erişim Imzası özelliği vardır. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* "Varsayılan erişim katmanını ayarla" eylemi artık hızlı erişime sabitlenmiş blob ve GPV2 depolama hesapları Için kullanılabilir. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Bazen Depolama Gezgini klasik depolama hesaplarını gösteremeyebilir. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Bilinen Sorunlar
* Azure depolama öykünücüsü veya Azurite gibi Öykünücüler kullanılırken, bunların varsayılan bağlantı noktalarında bağlantıları dinleyebilmeleri gerekir. Aksi takdirde, Depolama Gezgini bunlara bağlanamaz.
* Mac için VS 'yi kullanırsanız ve şimdiye kadar özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu geçici olarak çözmek için ~/içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak sizi engellemez, lütfen [Bu sorunla](https://github.com/Microsoft/AzureStorageExplorer/issues/97)ilgili yorum yapın.
* Azurite, tüm depolama API 'Lerini henüz tam olarak uygulamamıştır. Bu nedenle, geliştirme depolaması için Azurite kullanılırken beklenmeyen hatalar veya davranışlar olabilir.
* Nadir durumlarda, ağaç odağı hızlı erişime açık kalabilir. Odağı kaldırmak için, tümünü yenileyebilirsiniz.
* NodeJS içindeki bir hata nedeniyle OneDrive klasörünüzden karşıya yükleme işlemi çalışmıyor. Hata düzeltildi, ancak henüz elektron ile tümleştirildi.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, [burada](https://github.com/Azure/azure-storage-node/issues/317)açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack Şu anda dosya paylaşımlarını desteklemese de, bir dosya paylaşımları düğümü, ekli bir Azure Stack depolama hesabı altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2,0](https://dotnet.microsoft.com/download/dotnet-core/2.0)' i yüklemeniz gerekir.
* Ubuntu 14,04 üzerindeki kullanıcılar için GCC 'nin güncel olduğundan emin olmanız gerekir. Bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenize yeniden başlatılarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17,04 üzerindeki kullanıcılar için GConf yüklemeniz gerekir-bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-143"></a>Sürüm 1.4.3
10/11/2018

### <a name="hotfixes"></a>Başlatmayla
* Azure Kaynak yönetimi API 'SI sürümü, Azure ABD kamu kullanıcılarının engellemesini kaldırmak için geri alındı. [#696](https://github.com/Microsoft/AzureStorageExplorer/issues/696)
* Sayaç yükleme artık Depolama Gezgini tarafından kullanılan GPU miktarını azaltmak için CSS animasyonlarını kullanıyor. [#653](https://github.com/Microsoft/AzureStorageExplorer/issues/653)

### <a name="new"></a>Yeni
* SAS bağlantıları ve Öykünücüler gibi dış kaynak ekleri önemli ölçüde geliştirilmiştir. Artık şunları yapabilirsiniz:
   * İliştirmekte olduğunuz kaynağın görünen adını özelleştirin. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Farklı bağlantı noktaları kullanarak birden çok yerel öykünücüye ekleyin. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Hızlı erişime eklenen kaynakları ekleyin. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Depolama Gezgini artık geçici silme destekleniyor. Yapabilecekleriniz:
   * Depolama hesabınızın blob kapsayıcıları düğümüne sağ tıklayarak bir geçici silme ilkesi yapılandırın.
   * Gezinti çubuğunun yanındaki açılan menüde bulunan "etkin ve silinen Bloblar" seçeneğini belirleyerek blob düzenleyicisinde geçici olarak silinen Blobları görüntüleyin.
   * Geçici silinen Blobları geri al.

### <a name="fixes"></a>Düzeltilen
* Premium Depolama hesapları CORS 'yi desteklemediğinden, "CORS ayarlarını yapılandır" eylemi artık Premium Depolama hesaplarında kullanılamaz. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Artık SAS ekli Hizmetleri için paylaşılan erişim Imzası özelliği vardır. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* "Varsayılan erişim katmanını ayarla" eylemi artık hızlı erişime sabitlenmiş blob ve GPV2 depolama hesapları Için kullanılabilir. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Bazen Depolama Gezgini klasik depolama hesaplarını gösteremeyebilir. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Bilinen Sorunlar
* Azure depolama öykünücüsü veya Azurite gibi Öykünücüler kullanılırken, bunların varsayılan bağlantı noktalarında bağlantıları dinleyebilmeleri gerekir. Aksi takdirde, Depolama Gezgini bunlara bağlanamaz.
* Mac için VS 'yi kullanırsanız ve şimdiye kadar özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu geçici olarak çözmek için ~/içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak sizi engellemez, lütfen [Bu sorunla](https://github.com/Microsoft/AzureStorageExplorer/issues/97)ilgili yorum yapın.
* Azurite, tüm depolama API 'Lerini henüz tam olarak uygulamamıştır. Bu nedenle, geliştirme depolaması için Azurite kullanılırken beklenmeyen hatalar veya davranışlar olabilir.
* Nadir durumlarda, ağaç odağı hızlı erişime açık kalabilir. Odağı kaldırmak için, tümünü yenileyebilirsiniz.
* NodeJS içindeki bir hata nedeniyle OneDrive klasörünüzden karşıya yükleme işlemi çalışmıyor. Hata düzeltildi, ancak henüz elektron ile tümleştirildi.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, [burada](https://github.com/Azure/azure-storage-node/issues/317)açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack Şu anda dosya paylaşımlarını desteklemese de, bir dosya paylaşımları düğümü, ekli bir Azure Stack depolama hesabı altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2,0](https://dotnet.microsoft.com/download/dotnet-core/2.0)' i yüklemeniz gerekir.
* Ubuntu 14,04 üzerindeki kullanıcılar için GCC 'nin güncel olduğundan emin olmanız gerekir. Bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenize yeniden başlatılarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17,04 üzerindeki kullanıcılar için GConf yüklemeniz gerekir-bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-142"></a>Sürüm 1.4.2
09/24/2018

### <a name="hotfixes"></a>Başlatmayla
* Yeni Azure depolama hesabı türleri için destek eklemek üzere Azure Kaynak yönetimi API sürümünü 2018-07-01 olarak güncelleştirin. [#652](https://github.com/Microsoft/AzureStorageExplorer/issues/652)

### <a name="new"></a>Yeni
* SAS bağlantıları ve Öykünücüler gibi dış kaynak ekleri önemli ölçüde geliştirilmiştir. Artık şunları yapabilirsiniz:
   * İliştirmekte olduğunuz kaynağın görünen adını özelleştirin. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Farklı bağlantı noktaları kullanarak birden çok yerel öykünücüye ekleyin. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Hızlı erişime eklenen kaynakları ekleyin. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Depolama Gezgini artık geçici silme destekleniyor. Yapabilecekleriniz:
   * Depolama hesabınızın blob kapsayıcıları düğümüne sağ tıklayarak bir geçici silme ilkesi yapılandırın.
   * Gezinti çubuğunun yanındaki açılan menüde bulunan "etkin ve silinen Bloblar" seçeneğini belirleyerek blob düzenleyicisinde geçici olarak silinen Blobları görüntüleyin.
   * Geçici silinen Blobları geri al.

### <a name="fixes"></a>Düzeltilen
* Premium Depolama hesapları CORS 'yi desteklemediğinden, "CORS ayarlarını yapılandır" eylemi artık Premium Depolama hesaplarında kullanılamaz. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Artık SAS ekli Hizmetleri için paylaşılan erişim Imzası özelliği vardır. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* "Varsayılan erişim katmanını ayarla" eylemi artık hızlı erişime sabitlenmiş blob ve GPV2 depolama hesapları Için kullanılabilir. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Bazen Depolama Gezgini klasik depolama hesaplarını gösteremeyebilir. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Bilinen Sorunlar
* Azure depolama öykünücüsü veya Azurite gibi Öykünücüler kullanılırken, bunların varsayılan bağlantı noktalarında bağlantıları dinleyebilmeleri gerekir. Aksi takdirde, Depolama Gezgini bunlara bağlanamaz.
* Mac için VS 'yi kullanırsanız ve şimdiye kadar özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu geçici olarak çözmek için ~/içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak sizi engellemez, lütfen [Bu sorunla](https://github.com/Microsoft/AzureStorageExplorer/issues/97)ilgili yorum yapın.
* Azurite, tüm depolama API 'Lerini henüz tam olarak uygulamamıştır. Bu nedenle, geliştirme depolaması için Azurite kullanılırken beklenmeyen hatalar veya davranışlar olabilir.
* Nadir durumlarda, ağaç odağı hızlı erişime açık kalabilir. Odağı kaldırmak için, tümünü yenileyebilirsiniz.
* NodeJS içindeki bir hata nedeniyle OneDrive klasörünüzden karşıya yükleme işlemi çalışmıyor. Hata düzeltildi, ancak henüz elektron ile tümleştirildi.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, [burada](https://github.com/Azure/azure-storage-node/issues/317)açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack Şu anda dosya paylaşımlarını desteklemese de, bir dosya paylaşımları düğümü, ekli bir Azure Stack depolama hesabı altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2,0](https://dotnet.microsoft.com/download/dotnet-core/2.0)' i yüklemeniz gerekir.
* Ubuntu 14,04 üzerindeki kullanıcılar için GCC 'nin güncel olduğundan emin olmanız gerekir. Bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenize yeniden başlatılarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17,04 üzerindeki kullanıcılar için GConf yüklemeniz gerekir-bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-141"></a>Sürüm 1.4.1
08/28/2018

### <a name="hotfixes"></a>Başlatmayla
* İlk başlatma sırasında, Depolama Gezgini hassas verileri şifrelemek için kullanılan anahtarı oluşturamadı. Bu, hızlı erişim ve kaynak ekleme sırasında sorunlara neden olur. [#535](https://github.com/Microsoft/AzureStorageExplorer/issues/535)
* Hesabınız, kendi ana kiracısı için MFA gerektirmiyorsa, ancak başka kiracılar için olsaydı, Depolama Gezgini abonelikleri listemeyebilir. Bu nedenle, böyle bir hesapla oturum açtıktan sonra Depolama Gezgini, kimlik bilgilerinizi yeniden girmeniz ve MFA yapmanız istenir. [#74](https://github.com/Microsoft/AzureStorageExplorer/issues/74)
* Depolama Gezgini Azure Almanya ve Azure ABD kamu 'dan kaynak ekleyemedi. [#572](https://github.com/Microsoft/AzureStorageExplorer/issues/572)
* Aynı e-posta adresine sahip iki hesapla oturum açtıysanız, Depolama Gezgini bazen kaynakları ağaç görünümünde gösteremeyebilir. [#580](https://github.com/Microsoft/AzureStorageExplorer/issues/580)
* Daha yavaş Windows makinelerinde, giriş ekranının bazen görünmesi önemli miktarda zaman alabilir. [#586](https://github.com/Microsoft/AzureStorageExplorer/issues/586)
* Bağlı hesaplar veya hizmetler olsa bile bağlan iletişim kutusu görünür. [#588](https://github.com/Microsoft/AzureStorageExplorer/issues/588)

### <a name="new"></a>Yeni
* SAS bağlantıları ve Öykünücüler gibi dış kaynak ekleri önemli ölçüde geliştirilmiştir. Artık şunları yapabilirsiniz:
   * İliştirmekte olduğunuz kaynağın görünen adını özelleştirin. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Farklı bağlantı noktaları kullanarak birden çok yerel öykünücüye ekleyin. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Hızlı erişime eklenen kaynakları ekleyin. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Depolama Gezgini artık geçici silme destekleniyor. Yapabilecekleriniz:
   * Depolama hesabınızın blob kapsayıcıları düğümüne sağ tıklayarak bir geçici silme ilkesi yapılandırın.
   * Gezinti çubuğunun yanındaki açılan menüde bulunan "etkin ve silinen Bloblar" seçeneğini belirleyerek blob düzenleyicisinde geçici olarak silinen Blobları görüntüleyin.
   * Geçici silinen Blobları geri al.

### <a name="fixes"></a>Düzeltilen
* Premium Depolama hesapları CORS 'yi desteklemediğinden, "CORS ayarlarını yapılandır" eylemi artık Premium Depolama hesaplarında kullanılamaz. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Artık SAS ekli Hizmetleri için paylaşılan erişim Imzası özelliği vardır. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* "Varsayılan erişim katmanını ayarla" eylemi artık hızlı erişime sabitlenmiş blob ve GPV2 depolama hesapları Için kullanılabilir. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Bazen Depolama Gezgini klasik depolama hesaplarını gösteremeyebilir. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Bilinen Sorunlar
* Azure depolama öykünücüsü veya Azurite gibi Öykünücüler kullanılırken, bunların varsayılan bağlantı noktalarında bağlantıları dinleyebilmeleri gerekir. Aksi takdirde, Depolama Gezgini bunlara bağlanamaz.
* Mac için VS 'yi kullanırsanız ve şimdiye kadar özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu geçici olarak çözmek için ~/içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak sizi engellemez, lütfen [Bu sorunla](https://github.com/Microsoft/AzureStorageExplorer/issues/97)ilgili yorum yapın.
* Azurite, tüm depolama API 'Lerini henüz tam olarak uygulamamıştır. Bu nedenle, geliştirme depolaması için Azurite kullanılırken beklenmeyen hatalar veya davranışlar olabilir.
* Nadir durumlarda, ağaç odağı hızlı erişime açık kalabilir. Odağı kaldırmak için, tümünü yenileyebilirsiniz.
* NodeJS içindeki bir hata nedeniyle OneDrive klasörünüzden karşıya yükleme işlemi çalışmıyor. Hata düzeltildi, ancak henüz elektron ile tümleştirildi.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, [burada](https://github.com/Azure/azure-storage-node/issues/317)açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack Şu anda dosya paylaşımlarını desteklemese de, bir dosya paylaşımları düğümü, ekli bir Azure Stack depolama hesabı altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2,0](https://dotnet.microsoft.com/download/dotnet-core/2.0)' i yüklemeniz gerekir.
* Ubuntu 14,04 üzerindeki kullanıcılar için GCC 'nin güncel olduğundan emin olmanız gerekir. Bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenize yeniden başlatılarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17,04 üzerindeki kullanıcılar için GConf yüklemeniz gerekir-bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-130"></a>Sürüm 1.3.0
07/09/2018

### <a name="new"></a>Yeni
* Statik Web siteleri tarafından kullanılan $web kapsayıcılarına erişim artık desteklenmektedir. Bu sayede Web siteniz tarafından kullanılan dosya ve klasörleri kolayca karşıya yükleyebilir ve yönetebilirsiniz. [#223](https://github.com/Microsoft/AzureStorageExplorer/issues/223)
* MacOS 'ta uygulama çubuğu yeniden düzenlendi. Değişiklikler bir dosya menüsü, bazı kısayol tuşu değişiklikleri ve uygulama menüsü altında birkaç yeni komut içerir. [#99](https://github.com/Microsoft/AzureStorageExplorer/issues/99)
* Azure ABD kamu 'da oturum açmak için yetkili uç noktası https://login.microsoftonline.us/ olarak değiştirilmiştir
* Erişilebilirlik: ekran okuyucu etkin olduğunda, klavye gezintisi artık sağ taraftaki öğeleri görüntülemek için kullanılan tablolarla birlikte çalışıyor. Ok tuşlarını, satırlarda ve sütunlarda gezinmek için, varsayılan eylemleri çağırmak için, bağlam menüsü tuşu, bir öğe için bağlam menüsünü açmak için yazın ve çoklu seçim yapmak için kaydırma veya denetim kullanabilirsiniz. [#103](https://github.com/Microsoft/AzureStorageExplorer/issues/103)

### <a name="fixes"></a>Düzeltilen
*  Bazı makinelerde, alt işlemlerin başlaması uzun sürüyor. Bu gerçekleştiğinde, "alt işlem zamanında başlatılamadı" hatası görüntülenir. Bir alt işlemin başlaması için ayrılan süre artık 20 ' den 90 saniyeye yükselmiştir. Bu sorundan etkileniyorsanız lütfen bağlantılı GitHub sorununa yorum yapın. [#281](https://github.com/Microsoft/AzureStorageExplorer/issues/281)
* Okuma izinleri olmayan bir SAS kullanırken, büyük bir Blobun karşıya yüklemek mümkün değildir. Karşıya yükleme mantığı bu senaryoda çalışacak şekilde değiştirildi. [#305](https://github.com/Microsoft/AzureStorageExplorer/issues/305)
* Bir kapsayıcı için genel erişim düzeyini ayarlamak tüm erişim ilkelerini kaldırır ve tam tersi de geçerlidir. Artık, genel erişim düzeyi ve erişim ilkeleri, ikisinden biri ayarlanırken korunur. [#197](https://github.com/Microsoft/AzureStorageExplorer/issues/197)
* Özellikler iletişim kutusunda "AccessTierChangeTime" kesildi. Bu düzeltildi. [#145](https://github.com/Microsoft/AzureStorageExplorer/issues/145)
* Yeni dizin oluştur iletişim kutusunda "Microsoft Azure Depolama Gezgini-" öneki yoktu. Bu düzeltildi. [#299](https://github.com/Microsoft/AzureStorageExplorer/issues/299)
* Erişilebilirlik: VoiceOver kullanılırken varlık Ekle iletişim kutusunun gezinmek zordur. Geliştirmeler yapılmıştır. [#206](https://github.com/Microsoft/AzureStorageExplorer/issues/206)
* Erişilebilirlik: eylemler ve Özellikler bölmesi için daraltma/genişletme düğmesinin arka plan rengi, Yüksek Karşıtlık siyah temadaki benzer UI denetimleriyle tutarsız. Renk değiştirilmiştir. [#123](https://github.com/Microsoft/AzureStorageExplorer/issues/123)
* Erişilebilirlik: Yüksek Karşıtlık siyah temada, Özellikler iletişim kutusundaki ' X ' düğmesi için odak stili görünür değildi. Bu düzeltildi. [#243](https://github.com/Microsoft/AzureStorageExplorer/issues/243)
* Erişilebilirlik: eylemler ve Özellikler sekmelerinde, bir alt par ekran okuyucu deneyimiyle sonuçlanan birkaç Aria değeri yoktu. Eksik Aria değerleri şimdi eklendi. [#316](https://github.com/Microsoft/AzureStorageExplorer/issues/316)
* Erişilebilirlik: sol taraftaki daraltılmış ağaç düğümlerine genişletilmiş bir değer yanlış değeri verilmedi. Bu düzeltildi. [#352](https://github.com/Microsoft/AzureStorageExplorer/issues/352)

### <a name="known-issues"></a>Bilinen Sorunlar
* Bir blob kapsayıcısı gibi SAS URI 'SI aracılığıyla bağlı bir kaynağın ayrılması, diğer eklerin doğru şekilde görüntülenmesini engelleyen bir hataya neden olabilir. Bu sorunu geçici olarak çözmek için Grup düğümünü yenilemeniz yeterlidir. Daha fazla bilgi için [Bu soruna](https://github.com/Microsoft/AzureStorageExplorer/issues/537) bakın.
* Mac için VS 'yi kullanırsanız ve şimdiye kadar özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu geçici olarak çözmek için ~/içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak sizi engellemez, lütfen [Bu sorunla](https://github.com/Microsoft/AzureStorageExplorer/issues/97)ilgili yorum yapın.
* Azurite, tüm depolama API 'Lerini henüz tam olarak uygulamamıştır. Bu nedenle, geliştirme depolaması için Azurite kullanılırken beklenmeyen hatalar veya davranışlar olabilir.
* Nadir durumlarda, ağaç odağı hızlı erişime açık kalabilir. Odağı kaldırmak için, tümünü yenileyebilirsiniz.
* NodeJS içindeki bir hata nedeniyle OneDrive klasörünüzden karşıya yükleme işlemi çalışmıyor. Hata düzeltildi, ancak henüz elektron ile tümleştirildi.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, [burada](https://github.com/Azure/azure-storage-node/issues/317)açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack aşağıdaki özellikleri desteklemez ve Azure Stack çalışırken bunları kullanmaya çalışmak beklenmeyen hatalara neden olabilir:
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici silme
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2,0](https://dotnet.microsoft.com/download/dotnet-core/2.0)' i yüklemeniz gerekir.
* Ubuntu 14,04 üzerindeki kullanıcılar için GCC 'nin güncel olduğundan emin olmanız gerekir. Bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenize yeniden başlatılarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17,04 üzerindeki kullanıcılar için GConf yüklemeniz gerekir-bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-120"></a>Sürüm 1.2.0
06/12/2018

### <a name="new"></a>Yeni
* Depolama Gezgini, aboneliklerinizin yalnızca bir alt kümesinden abonelik yükleyemediğinde, başarılı bir şekilde yüklenmiş abonelikler, özel olarak başarısız olan kiracılar için bir hata iletisiyle birlikte gösterilir. [#159](https://github.com/Microsoft/AzureStorageExplorer/issues/159)
* Windows 'ta, bir güncelleştirme kullanılabilir olduğunda, artık "kapatma sırasında Güncelleştir" seçeneğini belirleyebilirsiniz. Bu seçenek çekildiğinde, güncelleştirme yükleyicisi Depolama Gezgini kapattıktan sonra çalışacaktır. [#21](https://github.com/Microsoft/AzureStorageExplorer/issues/21)
* Dosya paylaşımının anlık görüntüsünü görüntülerken dosya paylaşma düzenleyicisinin bağlam menüsüne geri yükleme anlık görüntüsü eklendi. [#131](https://github.com/Microsoft/AzureStorageExplorer/issues/131)
* Kuyruğu temizle düğmesi artık her zaman etkindir. [#135](https://github.com/Microsoft/AzureStorageExplorer/issues/135)
* ADFS Azure Stack oturum açma desteği yeniden etkinleştirildi. Azure Stack sürüm 1804 veya üzeri gereklidir. [#150](https://github.com/Microsoft/AzureStorageExplorer/issues/150)

### <a name="fixes"></a>Düzeltilen
* Adı aynı depolama hesabındaki başka bir dosya paylaşımının ön eki olan bir dosya paylaşımının anlık görüntülerini görüntülerse, diğer dosya paylaşımının anlık görüntüleri de listelenir. Bu sorun düzeltildi. [#255](https://github.com/Microsoft/AzureStorageExplorer/issues/255)
* SAS aracılığıyla eklendiğinde dosya paylaşımının anlık görüntüsünden bir dosyayı geri yüklemek hataya neden olur. Bu sorun düzeltildi. [#211](https://github.com/Microsoft/AzureStorageExplorer/issues/211)
* Bir Blobun anlık görüntülerini görüntülerken, temel blob ve tek bir anlık görüntü seçildiğinde anlık görüntüyü Yükselt eylemi etkinleştirilmiştir. Eylem artık yalnızca tek bir anlık görüntü seçiliyse etkindir. [#230](https://github.com/Microsoft/AzureStorageExplorer/issues/230)
* Tek bir iş (örneğin, blob indirme) başlatılmışsa ve daha sonra başarısız olduysa, aynı türde başka bir iş başlatılana kadar otomatik olarak yeniden denenmez. Tüm işler, kaç iş sıraya yazdığınıza bakmaksızın otomatik olarak yeniden denemeye hazır.
* GPV2 ve BLOB depolama hesaplarında yeni oluşturulan blob kapsayıcıları için açılan düzenleyicilerin bir erişim katmanı sütunu yoktu. Bu sorun düzeltildi. [#109](https://github.com/Microsoft/AzureStorageExplorer/issues/109)
* Bir depolama hesabı veya blob kapsayıcısı SAS aracılığıyla eklendiğinde bir erişim katmanı sütunu bazen görünmez. Sütun artık her zaman görüntülenir, ancak erişim katmanı kümesi yoksa boş bir değer vardır. [#160](https://github.com/Microsoft/AzureStorageExplorer/issues/160)
* Karşıya yüklenen yeni bir blok Blobun erişim katmanını ayarlama devre dışı bırakıldı. Bu sorun düzeltildi. [#171](https://github.com/Microsoft/AzureStorageExplorer/issues/171)
* "Tab açık kalsın" düğmesi klavye kullanılarak çağrılırsa, klavye odağı kaybedilir. Şimdi odak açık tutulmuş olan sekmeye taşınır. [#163](https://github.com/Microsoft/AzureStorageExplorer/issues/163)
* Sorgu Tasarımcısı bir sorgu için, VoiceOver geçerli işlecin kullanılabilir bir açıklamasını verme. Artık daha açıklayıcı. [#207](https://github.com/Microsoft/AzureStorageExplorer/issues/207)
* Çeşitli düzenleyicilerle ilgili sayfalandırma bağlantıları açıklayıcı değildi. Daha açıklayıcı olacak şekilde değiştirilmiştir. [#205](https://github.com/Microsoft/AzureStorageExplorer/issues/205)
* Varlık Ekle iletişim kutusunda, VoiceOver bir giriş öğesinin parçası olduğu sütunu duyurmadı. Geçerli sütunun adı artık öğesi açıklamasına dahil edilmiştir. [#206](https://github.com/Microsoft/AzureStorageExplorer/issues/206)
* Radyo düğmeleri ve onay kutuları odaklandığı zaman görünür bir kenarlığa sahip değil. Bu sorun düzeltildi. [#237](https://github.com/Microsoft/AzureStorageExplorer/issues/237)

### <a name="known-issues"></a>Bilinen Sorunlar
* Azure depolama öykünücüsü veya Azurite gibi Öykünücüler kullanılırken, bunların varsayılan bağlantı noktalarında bağlantıları dinleyebilmeleri gerekir. Aksi takdirde, Depolama Gezgini bunlara bağlanamaz.
* Mac için VS 'yi kullanırsanız ve şimdiye kadar özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu geçici olarak çözmek için ~/içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak sizi engellemez, lütfen [Bu sorunla](https://github.com/Microsoft/AzureStorageExplorer/issues/97)ilgili yorum yapın.
* Azurite, tüm depolama API 'Lerini henüz tam olarak uygulamamıştır. Bu nedenle, geliştirme depolaması için Azurite kullanılırken beklenmeyen hatalar veya davranışlar olabilir.
* Nadir durumlarda, ağaç odağı hızlı erişime açık kalabilir. Odağı kaldırmak için, tümünü yenileyebilirsiniz.
* NodeJS içindeki bir hata nedeniyle OneDrive klasörünüzden karşıya yükleme işlemi çalışmıyor. Hata düzeltildi, ancak henüz elektron ile tümleştirildi.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, [burada](https://github.com/Azure/azure-storage-node/issues/317)açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack Şu anda dosya paylaşımlarını desteklemese de, bir dosya paylaşımları düğümü, ekli bir Azure Stack depolama hesabı altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2,0](https://dotnet.microsoft.com/download/dotnet-core/2.0)' i yüklemeniz gerekir.
* Ubuntu 14,04 üzerindeki kullanıcılar için GCC 'nin güncel olduğundan emin olmanız gerekir. Bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenize yeniden başlatılarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17,04 üzerindeki kullanıcılar için GConf yüklemeniz gerekir-bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-110"></a>Sürüm 1.1.0
05/09/2018

### <a name="new"></a>Yeni
* Depolama Gezgini artık Azurite kullanımını desteklemektedir. Note: Azurite bağlantısı, varsayılan geliştirme uç noktalarına sabit olarak kodlanmıştır.
* Depolama Gezgini artık yalnızca blob ve GPV2 depolama hesapları için erişim katmanlarını desteklemektedir. [Buradaki](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers)erişim katmanları hakkında daha fazla bilgi edinin.
* Bir SAS oluşturulurken başlangıç zamanı artık gerekli değildir.

### <a name="fixes"></a>Düzeltilen
* ABD devlet hesapları için aboneliklerin alınması kesildi. Bu sorun düzeltildi. [#61](https://github.com/Microsoft/AzureStorageExplorer/issues/61)
* Erişim ilkeleri için süre sonu zamanı doğru kaydedilmiyor. Bu sorun düzeltildi. [#50](https://github.com/Microsoft/AzureStorageExplorer/issues/50)
* Bir kapsayıcıdaki bir öğe için SAS URL 'SI oluşturulurken, öğenin adı URL 'ye eklenemedi. Bu sorun düzeltildi. [#44](https://github.com/Microsoft/AzureStorageExplorer/issues/44)
* SAS oluştururken, geçmişte olan süre sonu süreleri bazen varsayılan değer olacaktır. Bunun nedeni, varsayılan değer olarak en son kullanılan başlangıç ve bitiş tarihi kullanılarak Depolama Gezgini. Artık SAS iletişim kutusunu her açışınızda yeni bir varsayılan değerler kümesi oluşturulur. [#35](https://github.com/Microsoft/AzureStorageExplorer/issues/35)
* Depolama hesapları arasında kopyalama yaparken, 24 saatlik bir SAS oluşturulur. Kopya 24 saatten fazla kullanılıyorsa, kopyalama başarısız olur. Süresi dolan SAS nedeniyle oluşan bir kopyanın başarısız olma olasılığını azaltmak için SAS 'nin son 1 haftaya kadar uzadık. [#62](https://github.com/Microsoft/AzureStorageExplorer/issues/62)
* "Iptal" düğmesine tıklayan bazı etkinlikler her zaman çalışmayacaktır. Bu sorun düzeltildi. [#125](https://github.com/Microsoft/AzureStorageExplorer/issues/125)
* Bazı etkinliklerde aktarım hızı yanlış. Bu sorun düzeltildi. [#124](https://github.com/Microsoft/AzureStorageExplorer/issues/124)
* Görünüm menüsündeki "Previous" yazımı yanlış. Artık doğru yazılmış. [#71](https://github.com/Microsoft/AzureStorageExplorer/issues/71)
* Windows Installer 'ın son sayfasında "Ileri" düğmesi vardı. "Son" düğmesine değiştirilmiştir. [#70](https://github.com/Microsoft/AzureStorageExplorer/issues/70)
* , HC siyah teması kullanılırken iletişim kutularındaki düğmeler için sekme odağı görünür değildi. Artık görülebilir. [#64](https://github.com/Microsoft/AzureStorageExplorer/issues/64)
* Etkinlik günlüğündeki eylemler için "Otomatik Çözümle" büyük küçük harf yanlış. Şimdi doğru. [#51](https://github.com/Microsoft/AzureStorageExplorer/issues/51)
* Tablodan bir varlık silinirken, sizden onay isteyen iletişim kutusunda bir hata simgesi görüntülenmiştir. İletişim kutusu şimdi bir uyarı simgesi kullanır. [#148](https://github.com/Microsoft/AzureStorageExplorer/issues/148)

### <a name="known-issues"></a>Bilinen Sorunlar
* Mac için VS 'yi kullanırsanız ve şimdiye kadar özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu geçici olarak çözmek için ~/içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak sizi engellemez, lütfen [Bu sorunla](https://github.com/Microsoft/AzureStorageExplorer/issues/97)ilgili yorum yapın.
* Azurite, tüm depolama API 'Lerini henüz tam olarak uygulamamıştır. Bu nedenle, geliştirme depolaması için Azurite kullanılırken beklenmeyen hatalar veya davranışlar olabilir.
* Nadir durumlarda, ağaç odağı hızlı erişime açık kalabilir. Odağı kaldırmak için, tümünü yenileyebilirsiniz.
* NodeJS içindeki bir hata nedeniyle OneDrive klasörünüzden karşıya yükleme işlemi çalışmıyor. Hata düzeltildi, ancak henüz elektron ile tümleştirildi.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, [burada](https://github.com/Azure/azure-storage-node/issues/317)açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack Şu anda dosya paylaşımlarını desteklemese de, bir dosya paylaşımları düğümü, ekli bir Azure Stack depolama hesabı altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2,0](https://dotnet.microsoft.com/download/dotnet-core/2.0)' i yüklemeniz gerekir.
* Ubuntu 14,04 üzerindeki kullanıcılar için GCC 'nin güncel olduğundan emin olmanız gerekir. Bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenize yeniden başlatılarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17,04 üzerindeki kullanıcılar için GConf yüklemeniz gerekir-bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```


## <a name="version-100"></a>Sürüm 1.0.0
04/16/2018

### <a name="new"></a>Yeni
* Depolama Gezgini Visual Studio 2017 ile aynı hesap deposunu kullanmasına izin veren gelişmiş kimlik doğrulaması. Bu özelliği kullanmak için, hesaplarınızda yeniden oturum açıp filtrelenmiş aboneliklerinizi yeniden ayarlamanız gerekir.
* AAD tarafından desteklenen Azure Stack hesapları için, ' hedef Azure Stack ' etkinleştirildiğinde Depolama Gezgini artık Azure Stack abonelikleri alacak. Artık özel bir oturum açma ortamı oluşturmanız gerekmez.
* Daha hızlı gezintiyi sağlamak için birkaç kısayol eklenmiştir. Bunlar, çeşitli panellerin ve düzenleyicilerin arasında taşınmasına dahildir. Daha fazla ayrıntı için bkz. Görünüm menüsü.
* Depolama Gezgini geri bildirim artık GitHub üzerinde yer alır. Sol alt kısımdaki geri bildirim düğmesine tıklayarak veya [https://github.com/Microsoft/AzureStorageExplorer/issues](https://github.com/Microsoft/AzureStorageExplorer/issues)giderek sorunlar sayfamıza ulaşabilirsiniz. Önerilerde bulunmak, sorun bildirmek, sorular sormak veya başka bir geri bildirim formu bırakmak için ücretsiz bir fikir vardır.
* SSL sertifikası sorunları yaşıyorsanız ve sorunlu sertifikayı bulamıyorsanız, artık `--ignore-certificate-errors` bayrağıyla komut satırından Depolama Gezgini başlatabilirsiniz. Bu bayrağıyla başlatıldığında Depolama Gezgini, SSL sertifikası hatalarını yoksayar.
* Artık BLOB ve dosya öğeleri için bağlam menüsünde bir ' Indir ' seçeneği vardır.
* Geliştirilmiş erişilebilirlik ve ekran okuyucu desteği. Erişilebilirlik özelliklerini kullanıyorsanız daha fazla bilgi için [Erişilebilirlik belgelerimize](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-accessibility) bakın.
* Depolama Gezgini artık elektron 1.8.3 birden fazla kullanıyor

### <a name="breaking-changes"></a>Hataya Neden Olan Değişiklikler
* Depolama Gezgini yeni bir kimlik doğrulama kitaplığına geçti. Kitaplığa anahtarın bir parçası olarak, hesaplarınızda yeniden oturum açıp filtrelenmiş aboneliklerinizi yeniden ayarlamanız gerekir
* Hassas verileri şifrelemek için kullanılan yöntem değişti. Bu durum, bazı hızlı erişim öğelerinizin yeniden eklenmesi ve/veya bazı bağlı kaynakların yeniden iliştirilmesi gereken bir kısmının oluşmasına neden olabilir.

### <a name="fixes"></a>Düzeltilen
* Proxy 'lerin arkasındaki bazı kullanıcılar Grup blobu karşıya yüklemelere veya ' bir ' çözümlenemiyor ' hata iletisiyle kesintiye uğratılmasını sağlar. Bu sorun düzeltildi.
* Doğrudan bağlantı kullanılırken oturum açma gerekiyorsa, ' oturum açma ' istemine tıkladığınızda boş bir iletişim kutusu açılır. Bu sorun düzeltildi.
* Linux 'ta, bir GPU işlemi kilitlenmesi nedeniyle Depolama Gezgini başlatılamıyor, artık kilitlenme hakkında bilgilenirsiniz, '--Disable-GPU ' anahtarını kullanmıyordu ve Depolama Gezgini, anahtar etkinken otomatik olarak yeniden başlatılır.
* Erişim Ilkeleri iletişim kutusunda geçersiz erişim ilkeleri kimliği zor. Artık daha fazla görünürlük için geçersiz erişim ilkesi kimlikleri kırmızı renkle özetlenmiştir.
* Etkinlik günlüğü bazen bir etkinliğin farklı bölümleri arasında büyük boşluk alan olabilir. Bu sorun düzeltildi.
* Tablo sorgu düzenleyicisinde, bir zaman damgası yan tümcesini geçersiz bir durumda bıraktıysanız ve sonra başka bir yan tümceyi değiştirmeyi denediyseniz, düzenleyici dondurabilir. Başka bir yan tümcede değişiklik algılandığında, düzenleyici şimdi zaman damgası yan tümcesini son geçerli durumuna geri yükler.
* Ağaç görünümünde arama sorgusuna yazarken durakladıysanız, arama başlar ve odak metin kutusundan çalınabilir. Şimdi, ' Enter ' tuşuna basarak veya aramaya başla düğmesine tıklayarak aramayı açık bir şekilde başlatmanız gerekir.
* ' Paylaşılan erişim Imzasını Al ' komutu bazen dosya paylaşımındaki bir dosyaya sağ tıklandığında devre dışı bırakılabilir. Bu sorun düzeltildi.
* Odak bulunan kaynak ağacı düğümü arama sırasında filtrelense, kaynak ağacı ' na sekme ve kaynak ağacında gezinmek için ok tuşlarını kullanın. Artık odaklanmış kaynak ağacı düğümü gizliyse, Kaynak ağacındaki ilk düğüm otomatik olarak odaklanacaktır.
* Ek bir ayırıcı bazen Düzenleyici araç çubuğunda görülebilir. Bu sorun düzeltildi.
* İçerik haritası metin kutusu bazen taşırdı. Bu sorun düzeltildi.
* Blob ve dosya paylaşma düzenleyicileri bazen birçok dosyayı aynı anda karşıya yüklerken sürekli olarak yenilenir. Bu sorun düzeltildi.
* ' Klasör Istatistikleri ' özelliğinin dosya paylaşma anlık görüntü yönetimi görünümünde amacı yoktu. Artık devre dışı bırakıldı.
* Linux 'ta Dosya menüsü görünmedi. Bu sorun düzeltildi.
* Bir klasörü dosya paylaşımında karşıya yüklerken, varsayılan olarak yalnızca klasörün içeriği karşıya yüklenir. Şimdi, varsayılan davranış klasörün içeriğini dosya paylaşımındaki eşleşen bir klasöre yüklemedir.
* Birkaç diyalogdaki düğmelerin sıralaması tersine çevrildi. Bu sorun düzeltildi.
* Güvenlikle ilgili çeşitli düzeltmeler.

### <a name="known-issues"></a>Bilinen Sorunlar
* Nadir durumlarda, ağaç odağı hızlı erişime açık kalabilir. Odağı kaldırmak için, tümünü yenileyebilirsiniz.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack Şu anda dosya paylaşımlarını desteklemese de, bir dosya paylaşımları düğümü, ekli bir Azure Stack depolama hesabı altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2,0](https://dotnet.microsoft.com/download/dotnet-core/2.0)' i yüklemeniz gerekir.
* Ubuntu 14,04 üzerindeki kullanıcılar için GCC 'nin güncel olduğundan emin olmanız gerekir. Bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenize yeniden başlatılarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17,04 üzerindeki kullanıcılar için GConf yüklemeniz gerekir-bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-096"></a>Sürüm 0.9.6
02/28/2018

### <a name="fixes"></a>Düzeltilen
* Bir sorun, beklenen Blobların/dosyaların düzenleyicide listelenmesini engelledi. Bu sorun düzeltildi.
* Bir sorun, anlık görüntü görünümleri arasında öğeleri yanlış görüntüleyecek şekilde geçişe neden oldu. Bu sorun düzeltildi.

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini ADFS hesaplarını desteklemez.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, [burada](https://github.com/Azure/azure-storage-node/issues/317)açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Hesap Ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack Şu anda dosya paylaşımlarını desteklemese de, bir dosya paylaşımları düğümü, ekli bir Azure Stack depolama hesabı altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Ubuntu 14,04 üzerindeki kullanıcılar için GCC 'nin güncel olduğundan emin olmanız gerekir. Bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenize yeniden başlatılarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17,04 üzerindeki kullanıcılar için GConf yüklemeniz gerekir-bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-095"></a>Sürüm 0.9.5
02/06/2018

### <a name="new"></a>Yeni

* Dosya paylaşımı anlık görüntüleri için destek:
    * Dosya paylaşımlarınız için anlık görüntüler oluşturun ve yönetin.
    * Araştırırken dosya paylaşımlarınızın anlık görüntüleri arasında görünümleri kolayca değiştirin.
    * Dosyalarınızın önceki sürümlerini geri yükleyin.
* Azure Data Lake Store için Önizleme desteği:
    * Birden çok hesap genelinde ADLS kaynaklarınıza bağlanın.
    * ADL URI 'Lerini kullanarak ADLS kaynaklarına bağlanın ve bunları paylaşabilirsiniz.
    * Temel dosya/klasör işlemlerini yinelemeli olarak gerçekleştirin.
    * Ayrı klasörleri hızlı erişime sabitle.
    * Klasör istatistiklerini görüntüleyin.

### <a name="fixes"></a>Düzeltilen
* Başlangıç performansı geliştirmeleri.
* Çeşitli hata düzeltmeleri.

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini ADFS hesaplarını desteklemez.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Hesap Ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack Şu anda dosya paylaşımlarını desteklemese de, bir dosya paylaşımları düğümü, ekli bir Azure Stack depolama hesabı altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Ubuntu 14,04 üzerindeki kullanıcılar için GCC 'nin güncel olduğundan emin olmanız gerekir. Bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenize yeniden başlatılarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17,04 üzerindeki kullanıcılar için GConf yüklemeniz gerekir-bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-094-and-093"></a>Sürüm 0.9.4 ve 0.9.3
01/21/2018

### <a name="new"></a>Yeni
* Mevcut Depolama Gezgini pencereniz şu durumlarda yeniden kullanılacaktır:
    * Depolama Gezgini oluşturulan doğrudan bağlantılar açılıyor.
    * Depolama Gezgini portalından açılıyor.
    * Azure depolama VS Code uzantısı 'ndan Depolama Gezgini açılıyor (yakında).
* Depolama Gezgini içinden yeni Depolama Gezgini pencere açma özelliği eklendi.
    * Windows için Dosya menüsü altında ve görev çubuğunun bağlam menüsünde ' yeni pencere ' seçeneği vardır.
    * Mac için, uygulama menüsü altında ' yeni pencere ' seçeneği vardır.

### <a name="fixes"></a>Düzeltilen
* Bir güvenlik sorunu düzeltildi. Lütfen en kısa zamanda 0.9.4 sürümüne yükseltin.
* Eski etkinlikler düzgün temizlenmedi. Bu, uzun süre çalışan işlerin performansını etkilemiştir. Artık doğru şekilde temizleniyor.
* Çok sayıda dosya ve dizin içeren eylemler bazen Depolama Gezgini donmasına neden olur. Dosya paylaşımları için Azure istekleri, sistem kaynağı kullanımını sınırlamaya yönelik olarak kısıtlanıyor.

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini ADFS hesaplarını desteklemez.
* "Görünüm Gezgini" ve "hesap yönetimini görüntüle" kısayol tuşları, sırasıyla Ctrl/Cmd + SHIFT + E ve Ctrl/Cmd + SHIFT + A olmalıdır.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Hesap Ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack Şu anda dosya paylaşımlarını desteklemese de, bir dosya paylaşımları düğümü, ekli bir Azure Stack depolama hesabı altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer --disable-gpu
    ```

* Ubuntu 14,04 üzerindeki kullanıcılar için GCC 'nin güncel olduğundan emin olmanız gerekir. Bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenize yeniden başlatılarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17,04 üzerindeki kullanıcılar için GConf yüklemeniz gerekir-bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-092"></a>Sürüm 0.9.2
11/01/2017

### <a name="hotfixes"></a>Başlatmayla
* Yerel saat dilimine bağlı olarak tablo varlıklarının Edm. DateTime değerleri düzenlenirken beklenmeyen veri değişiklikleri yapılamamıştı. Düzenleyici artık, Edm. DateTime değerleri üzerinde kesin ve tutarlı denetim sağlayan düz metin kutusu kullanır.
* Ad ve anahtarla eklenen bir blob grubunu karşıya yükleme/indirme işlemi başlamaz. Bu sorun düzeltildi.
* Daha önce Depolama Gezgini hesap aboneliklerinin bir veya daha fazlasının seçili olması durumunda yalnızca eski bir hesabı yeniden kimlik doğrulaması yapmanız istenir. Artık Depolama Gezgini hesap tam olarak filtrelense bile sizi uyarır.
* Azure ABD kamu için uç noktalar etki alanı yanlış. Düzeltildi.
* Hesapları Yönet panelindeki Uygula düğmesi bazen tıklamak zordur. Bu durum artık gerçekleşmemelidir.

### <a name="new"></a>Yeni
* Azure Cosmos DB için Önizleme desteği:
    * [Çevrimiçi belgeler](./cosmos-db/storage-explorer.md)
    * Veritabanları ve koleksiyonlar oluşturma
    * Verileri denetleme
    * Belge sorgulama, oluşturma veya silme
    * Saklı yordamları, Kullanıcı tanımlı işlevleri veya Tetikleyicileri güncelleştirme
    * Veritabanlarına bağlanmak ve bunları yönetmek için bağlantı dizelerini kullanma
* Birçok küçük blob yükleme/indirme performansı geliştirilmiştir.
* Blob karşıya yükleme grubunda veya blob indirme grubunda bir başarısızlık oluşursa bir "tümünü yeniden dene" eylemi eklendi.
* Depolama Gezgini, ağ bağlantınızın kaybedildiği algılanırsa blob karşıya yükleme/indirme sırasında yinelemeyi duraklatacaktır. Ağ bağlantısı yeniden kurulduktan sonra yinelemeyi sürdürebilirsiniz.
* Bağlam menüsü aracılığıyla "Tümünü Kapat", "Diğerlerini kapat" ve "Kapat" sekmelerini eklendi.
* Depolama Gezgini artık yerel iletişim kutularını ve yerel bağlam menülerini kullanır.
* Depolama Gezgini artık daha erişilebilir. Geliştirmeler şunları içerir:
    * Windows üzerinde NVDA ve Mac üzerinde VoiceOver için iyileştirilmiş ekran okuyucu desteği
    * İyileştirilmiş yüksek karşıtlıklı Tema
    * Klavye sekme ve klavye odağı düzeltmeleri

### <a name="fixes"></a>Düzeltilen
* Geçersiz bir Windows dosya adıyla bir blobu açmaya veya indirmeye çalıştıysanız, işlem başarısız olur. Depolama Gezgini, şimdi bir blob adının geçersiz olup olmadığını algılar ve bunu kodlamak ya da blobu atlamak isteyip istemediğinizi sorar. Depolama Gezgini Ayrıca, bir dosya adının kodlanıp kodlandığını algılar ve karşıya yüklemeden önce kodu çözmek isteyip istemediğinizi sorar.
* Blob karşıya yükleme sırasında, hedef blob kapsayıcısı için düzenleyici bazen doğru şekilde yenilememelidir. Bu sorun düzeltildi.
* Çeşitli bağlantı dizeleri ve SAS URI 'Leri için destek. Bilinen tüm sorunları ele aldık, ancak daha fazla sorunla karşılaşırsanız lütfen geri bildirim gönderin.
* Güncelleştirme bildirimi 0.9.0 içindeki bazı kullanıcılar için kesildi. Bu sorun düzeltildi ve hata tarafından etkilenenler için Depolama Gezgini en son sürümünü [buradan](https://azure.microsoft.com/features/storage-explorer/)el ile indirebilirsiniz.

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini ADFS hesaplarını desteklemez.
* "Görünüm Gezgini" ve "hesap yönetimini görüntüle" kısayol tuşları, sırasıyla Ctrl/Cmd + SHIFT + E ve Ctrl/Cmd + SHIFT + A olmalıdır.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Hesap Ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack Şu anda dosya paylaşımlarını desteklemese de, bir dosya paylaşımları düğümü, ekli bir Azure Stack depolama hesabı altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer --disable-gpu
    ```

* Ubuntu 14,04 üzerindeki kullanıcılar için GCC 'nin güncel olduğundan emin olmanız gerekir. Bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenize yeniden başlatılarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17,04 üzerindeki kullanıcılar için GConf yüklemeniz gerekir-bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-091-and-090"></a>Sürüm 0.9.1 ve 0.9.0
10/20/2017
### <a name="new"></a>Yeni
* Azure Cosmos DB için Önizleme desteği:
    * [Çevrimiçi belgeler](./cosmos-db/storage-explorer.md)
    * Veritabanları ve koleksiyonlar oluşturma
    * Verileri denetleme
    * Belge sorgulama, oluşturma veya silme
    * Saklı yordamları, Kullanıcı tanımlı işlevleri veya Tetikleyicileri güncelleştirme
    * Veritabanlarına bağlanmak ve bunları yönetmek için bağlantı dizelerini kullanma
* Birçok küçük blob yükleme/indirme performansı geliştirilmiştir.
* Blob karşıya yükleme grubunda veya blob indirme grubunda bir başarısızlık oluşursa bir "tümünü yeniden dene" eylemi eklendi.
* Depolama Gezgini, ağ bağlantınızın kaybedildiği algılanırsa blob karşıya yükleme/indirme sırasında yinelemeyi duraklatacaktır. Ağ bağlantısı yeniden kurulduktan sonra yinelemeyi sürdürebilirsiniz.
* Bağlam menüsü aracılığıyla "Tümünü Kapat", "Diğerlerini kapat" ve "Kapat" sekmelerini eklendi.
* Depolama Gezgini artık yerel iletişim kutularını ve yerel bağlam menülerini kullanır.
* Depolama Gezgini artık daha erişilebilir. Geliştirmeler şunları içerir:
    * Windows üzerinde NVDA ve Mac üzerinde VoiceOver için iyileştirilmiş ekran okuyucu desteği
    * İyileştirilmiş yüksek karşıtlıklı Tema
    * Klavye sekme ve klavye odağı düzeltmeleri

### <a name="fixes"></a>Düzeltilen
* Geçersiz bir Windows dosya adıyla bir blobu açmaya veya indirmeye çalıştıysanız, işlem başarısız olur. Depolama Gezgini, şimdi bir blob adının geçersiz olup olmadığını algılar ve bunu kodlamak ya da blobu atlamak isteyip istemediğinizi sorar. Depolama Gezgini Ayrıca, bir dosya adının kodlanıp kodlandığını algılar ve karşıya yüklemeden önce kodu çözmek isteyip istemediğinizi sorar.
* Blob karşıya yükleme sırasında, hedef blob kapsayıcısı için düzenleyici bazen doğru şekilde yenilememelidir. Bu sorun düzeltildi.
* Çeşitli bağlantı dizeleri ve SAS URI 'Leri için destek. Bilinen tüm sorunları ele aldık, ancak daha fazla sorunla karşılaşırsanız lütfen geri bildirim gönderin.
* Güncelleştirme bildirimi 0.9.0 içindeki bazı kullanıcılar için kesildi. Bu sorun düzeltildi ve hata tarafından etkilenenler için Depolama Gezgini en son sürümünü [buradan](https://azure.microsoft.com/features/storage-explorer/) el ile indirebilirsiniz

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini ADFS hesaplarını desteklemez.
* "Görünüm Gezgini" ve "hesap yönetimini görüntüle" kısayol tuşları, sırasıyla Ctrl/Cmd + SHIFT + E ve Ctrl/Cmd + SHIFT + A olmalıdır.
* Azure Stack hedeflenirken, bazı dosyaları ekleme Blobları olarak karşıya yükleme başarısız olabilir.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresini kullandığımyız.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Hesap Ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack Şu anda dosya paylaşımlarını desteklemese de, bir dosya paylaşımları düğümü, ekli bir Azure Stack depolama hesabı altında görünmeye devam eder.
* Depolama Gezgini tarafından kullanılan elektron kabuğu, bazı GPU (grafik işleme birimi) donanım hızlandırile sorun yaşıyor. Depolama Gezgini boş (boş) bir ana pencere görüntülediğinde, komut satırından Depolama Gezgini başlatmayı ve `--disable-gpu` anahtarını ekleyerek GPU hızlandırmasını devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer --disable-gpu
    ```

* Ubuntu 14,04 üzerindeki kullanıcılar için GCC 'nin güncel olduğundan emin olmanız gerekir. Bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenize yeniden başlatılarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17,04 üzerindeki kullanıcılar için GConf yüklemeniz gerekir-bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-0816"></a>Sürüm 0.8.16
8/21/2017

### <a name="new"></a>Yeni
* Bir blobu açtığınızda Depolama Gezgini, bir değişiklik algılanırsa indirilen dosyayı karşıya yüklemenizi ister
* Gelişmiş Azure Stack oturum açma deneyimi
* Birçok küçük dosyayı aynı anda karşıya yükleme/indirme performansı geliştirilmiştir


### <a name="fixes"></a>Düzeltilen
* Bazı blob türlerinde, karşıya yükleme çakışması sırasında "Değiştir" seçeneğinin belirlenmesi bazen karşıya yükleme işleminin yeniden başlatılmasına neden olur.
* Sürüm 0.8.15 ' de, karşıya yüklemeler bazen %99 ' a kadar cepli.
* Dosyaları bir dosya paylaşımında karşıya yüklerken, henüz mevcut olmayan bir dizine yüklemeyi seçtiyseniz karşıya yüklemeniz başarısız olur.
* Depolama Gezgini, paylaşılan erişim imzaları ve tablo sorguları için yanlış zaman damgaları oluşturuyor.


### <a name="known-issues"></a>Bilinen Sorunlar
* Ad ve anahtar bağlantı dizesinin kullanılması şu anda çalışmıyor. Sonraki sürümde düzeltilecektir. Bu durumda, ad ve anahtar ile Attach 'i kullanabilirsiniz.
* Geçersiz bir Windows dosya adı olan bir dosyayı açmaya çalışırsanız, indirme dosya bulunamadı hatasıyla sonuçlanır.
* Bir görevde "Iptal" düğmesine tıkladıktan sonra bu görevin iptal olması biraz zaman alabilir. Bu, Azure Storage düğüm kitaplığının bir sınırlamasıdır.
* Blob karşıya yüklemeyi tamamladıktan sonra, karşıya yüklemeyi Başlatan sekme yenilenir. Bu, önceki davranıştaki bir değişiklikten oluşur ve ayrıca, içinde olduğunuz kapsayıcının köküne geri dönüş yapılmasına neden olur.
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir.
* Hesap Ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack Şu anda dosya paylaşımlarını desteklemese de, bir dosya paylaşımları düğümü, ekli bir Azure Stack depolama hesabı altında görünmeye devam eder.
* Ubuntu 14,04 üzerindeki kullanıcılar için GCC 'nin güncel olduğundan emin olmanız gerekir. Bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenize yeniden başlatılarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17,04 üzerindeki kullanıcılar için GConf yüklemeniz gerekir-bu, aşağıdaki komutlar çalıştırılarak ve sonra makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo apt-get install libgconf-2-4
    ```

### <a name="version-0814"></a>Sürüm 0.8.14
06/22/2017

### <a name="new"></a>Yeni

* Birkaç kritik güvenlik güncelleştirmesinin avantajlarından yararlanabilmek için elektron sürümü 1.7.2 olarak güncelleştirildi
* Artık Yardım menüsünden çevrimiçi sorun giderme kılavuzuna hızlıca erişebilirsiniz
* Depolama Gezgini sorun giderme [Kılavuzu][2]
* Azure Stack aboneliğine bağlanma [yönergeleri][3]

### <a name="known-issues"></a>Bilinen Sorunlar

* Klasörü sil onay iletişim kutusundaki düğmeler Linux üzerinde fare tıklamasına kaydolmayın. geçici çözüm, Enter tuşunu kullanmaktır
* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir
* 3 ' ten fazla BLOB grubunun veya aynı anda karşıya yüklenen dosyaların bulunması hatalara neden olabilir
* Hesap Ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack Şu anda dosya paylaşımlarını desteklemese de, bir dosya paylaşımları düğümü, ekli bir Azure Stack depolama hesabı altında görünmeye devam eder.
* Ubuntu 14,04 yüklemesi için GCC sürümünün güncelleştirilmesi veya yükseltilmesi gerekiyor – yükseltme adımları aşağıda verilmiştir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

### <a name="version-0813"></a>Sürüm 0.8.13
12.05.2017

#### <a name="new"></a>Yeni

* Depolama Gezgini sorun giderme [Kılavuzu][2]
* Azure Stack aboneliğine bağlanma [yönergeleri][3]

#### <a name="fixes"></a>Düzeltilen

* Düzeltildi: dosya yükleme, bellek yetersiz hatası nedeniyle büyük bir şansına sahipti
* Düzeltildi: artık PIN/akıllı kart ile oturum açabilirsiniz
* Düzeltildi: portalda aç artık Azure Çin 21Vianet, Azure Almanya, Azure ABD kamu ve Azure Stack birlikte çalışmaktadır
* Düzeltildi: bir dosyayı bir blob kapsayıcısına yüklerken "geçersiz işlem" hatası bazen oluşabilir
* Düzeltildi: anlık görüntüleri yönetirken tümünü seçme devre dışı bırakıldı
* Düzeltildi: temel Blobun meta verilerinin, anlık görüntülerinin özelliklerini görüntüledikten sonra üzerine yazılabilir

#### <a name="known-issues"></a>Bilinen Sorunlar

* Yanlış PIN/akıllı kart sertifikasını seçerseniz, Depolama Gezgini kararı unutmak için yeniden başlatmanız gerekir
* Yakınlaştırılıp uzaklaştırıldığında yakınlaştırma düzeyi geçici olarak varsayılan düzeye sıfırlanır
* 3 ' ten fazla BLOB grubunun veya aynı anda karşıya yüklenen dosyaların bulunması hatalara neden olabilir
* Hesap Ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack Şu anda dosya paylaşımlarını desteklemese de, bir dosya paylaşımları düğümü, ekli bir Azure Stack depolama hesabı altında görünmeye devam eder.
* Ubuntu 14,04 yüklemesi için GCC sürümünün güncelleştirilmesi veya yükseltilmesi gerekiyor – yükseltme adımları aşağıda verilmiştir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-0812-and-0811-and-0810"></a>Sürüm 0.8.12 ve 0.8.11 ve 0.8.10
04/07/2017

#### <a name="new"></a>Yeni

* Güncelleştirme bildiriminden bir güncelleştirme yüklediğinizde artık Depolama Gezgini otomatik olarak kapatılacak
* Yerinde hızlı erişim, sık erişilen kaynaklarınız ile çalışmaya yönelik gelişmiş bir deneyim sağlar
* Blob kapsayıcı düzenleyicisinde, artık kiralanmış bir Blobun ait olduğu sanal makineye bakabilirsiniz
* Artık sol taraftaki paneli daraltabilirsiniz
* Bulma işlemi artık indirme ile aynı zamanda çalışıyor
* Kaynağınızın veya Seçiminizin boyutunu görmek için blob kapsayıcısında, dosya paylaşımında ve tablo düzenleyicilerinde Istatistikleri kullanın
* Artık Azure Active Directory (AAD) tabanlı Azure Stack hesaplarında oturum açabilirsiniz.
* Artık 32 MB 'tan Premium depolama hesaplarına arşiv dosyalarını karşıya yükleyebilirsiniz
* Geliştirilmiş erişilebilirlik desteği
* Artık&gt; SSL sertifikalarına&gt;, sertifikaları Içeri Aktar ' a giderek güvenilen temel 64 kodlu X. 509.952 SSL sertifikaları ekleyebilirsiniz

#### <a name="fixes"></a>Düzeltilen

* Düzeltildi: bir hesabın kimlik bilgilerini yenilemeden sonra ağaç görünümü bazen otomatik olarak Yenilemeyebilir
* Düzeltildi: öykünücü kuyrukları ve tabloları için SAS oluşturma geçersiz bir URL ile sonuçlanır
* Düzeltildi: bir ara sunucu etkinken Premium Depolama hesapları artık Genişletilebilir
* Düzeltildi: hesap yönetimi sayfasındaki Uygula düğmesi 1 veya 0 hesabınız seçilmişse çalışmaz
* Düzeltildi: çakışma çözümü gerektiren Blobları karşıya yüklemek 0.8.11 içinde başarısız olabilir
* Düzeltildi: 0.8.12 'te düzeltilen 0.8.11-fixed içinde geri bildirim gönderilemedi

#### <a name="known-issues"></a>Bilinen Sorunlar

* 0\.8.10 sürümüne yükselttikten sonra, tüm kimlik bilgilerinizi yenilemeniz gerekir.
* Yakınlaştırılıp uzaklaştırıldığında yakınlaştırma düzeyi geçici olarak varsayılan düzeye sıfırlanır.
* 3 ' ten fazla BLOB grubunun veya aynı anda karşıya yüklenen dosyaların bulunması hatalara neden olabilir.
* Hesap Ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur.
* Azure Stack Şu anda dosya paylaşımlarını desteklemese de, bir dosya paylaşımları düğümü, ekli bir Azure Stack depolama hesabı altında görünmeye devam eder.
* Ubuntu 14,04 yüklemesi için GCC sürümünün güncelleştirilmesi veya yükseltilmesi gerekiyor – yükseltme adımları aşağıda verilmiştir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-089-and-088"></a>Sürüm 0.8.9 ve 0.8.8
02/23/2017

>[!VIDEO https://www.youtube.com/embed/R6gonK3cYAc?ecver=1]

>[!VIDEO https://www.youtube.com/embed/SrRPCm94mfE?ecver=1]


#### <a name="new"></a>Yeni

* Depolama Gezgini 0.8.9, güncelleştirmeler için en son sürümü otomatik olarak indirecek.
* Düzeltme: bir depolama hesabı eklemek için Portal tarafından oluşturulan SAS URI 'SI kullanıldığında hata oluşur.
* Artık BLOB anlık görüntülerini oluşturabilir, yönetebilir ve yükseltebilirsiniz.
* Artık Azure Çin 21Vianet, Azure Almanya ve Azure ABD kamu hesaplarında oturum açabilirsiniz.
* Artık yakınlaştırma düzeyini değiştirebilirsiniz. Yakınlaştır, uzaklaştır ve yakınlaştırmayı sıfırlamak için Görünüm menüsündeki seçenekleri kullanın.
* Unicode karakterler, Bloblar ve dosyalar için Kullanıcı meta verilerinde artık desteklenmektedir.
* Erişilebilirlik geliştirmeleri.
* Sonraki sürümün sürüm notları güncelleştirme bildiriminden görüntülenebilir. Ayrıca, Yardım menüsünden Geçerli sürüm notlarını da görüntüleyebilirsiniz.

#### <a name="fixes"></a>Düzeltilen

* Düzeltildi: sürüm numarası artık Windows üzerinde denetim masası 'nda doğru şekilde görüntüleniyor
* Düzeltildi: arama artık 50.000 düğümle sınırlı değil
* Düzeltildi: hedef dizin zaten mevcut değilse, dosya paylaşımının süresiz olarak karşıya yükleme
* Düzeltildi: uzun karşıya yüklemeler ve indirmeler için iyileştirilmiş kararlılık

#### <a name="known-issues"></a>Bilinen Sorunlar

* Yakınlaştırılıp uzaklaştırıldığında yakınlaştırma düzeyi geçici olarak varsayılan düzeye sıfırlanır.
* Hızlı erişim yalnızca abonelik tabanlı öğeler ile kullanılabilir. Bu sürümde, anahtar veya SAS belirteci ile eklenen yerel kaynaklar veya kaynaklar desteklenmez.
* Sahip olduğunuz kaynak sayısına bağlı olarak, hedef kaynağa gitmek için birkaç saniye hızlı erişim gerekebilir.
* 3 ' ten fazla BLOB grubunun veya aynı anda karşıya yüklenen dosyaların bulunması hatalara neden olabilir.

12/16/2016
### <a name="version-087"></a>Sürüm 0.8.7

>[!VIDEO https://www.youtube.com/embed/Me4Y4jxoer8?ecver=1]

#### <a name="new"></a>Yeni

* Bir güncelleştirmenin başlangıcında çakışmaların nasıl çözümleneceğini, etkinlik penceresinde ise oturumu indirmeyi veya kopyalamayı seçebilirsiniz
* Depolama kaynağının tam yolunu görmek için bir sekmenin üzerine gelin
* Bir sekmeye tıkladığınızda, sol taraftaki Gezinti bölmesindeki konumuyla eşitlenir

#### <a name="fixes"></a>Düzeltilen

* Düzeltildi: Depolama Gezgini artık Mac üzerinde güvenilir bir uygulama
* Düzeltildi: Ubuntu 14,04 yeniden destekleniyor
* Düzeltildi: bazen abonelikler yüklenirken hesap kullanıcı arabirimi yanıp sönü ekleme
* Düzeltildi: bazı durumlarda, sol taraftaki Gezinti bölmesinde tüm depolama kaynakları listelenmemiştir
* Düzeltildi: eylem bölmesi bazen boş eylemler görüntülendi
* Düzeltildi: son kapatılan oturumdan pencere boyutu artık tutuluyor
* Düzeltildi: bağlam menüsünü kullanarak aynı kaynak için birden çok sekme açabilirsiniz

#### <a name="known-issues"></a>Bilinen Sorunlar

* Hızlı erişim yalnızca abonelik tabanlı öğeler ile kullanılabilir. Bu sürümde anahtar veya SAS belirteci ile eklenen yerel kaynaklar veya kaynaklar desteklenmez
* Sahip olduğunuz kaynak sayısına bağlı olarak, hedef kaynağa gitmek için birkaç saniye hızlı erişim gerekebilir
* 3 ' ten fazla BLOB grubunun veya aynı anda karşıya yüklenen dosyaların bulunması hatalara neden olabilir
* Tüm kabaca 50.000 düğümlerde arama tutamaçları aranıyor-bundan sonra performans etkilenebilir veya işlenmemiş özel duruma neden olabilir
* MacOS 'ta Depolama Gezgini ilk kez kullanırken, kullanıcının anahtarlığa erişim izninin olduğunu soran birden çok istem görebilirsiniz. İsteminin yeniden gösterilmemesi için her zaman Izin ver ' i seçmenizi öneririz

11/18/2016
### <a name="version-086"></a>Sürüm 0.8.6

#### <a name="new"></a>Yeni

* Artık kolay gezinme için en sık kullanılan hizmetleri hızlı erişime sabitleyebilir
* Artık birden çok Editörün farklı sekmelerde açılmasını sağlayabilirsiniz. Geçici bir sekme açmak için tek tıklama; kalıcı bir sekme açmak için çift tıklayın. Kalıcı sekme yapmak için geçici sekmeye da tıklayabilirsiniz
* Özellikle de hızlı makinelerde büyük dosyalar için karşıya yükleme ve indirme işlemleri için dikkat çekici performans ve kararlılık geliştirmeleri yaptık
* Boş "sanal" klasörler artık BLOB kapsayıcılarında oluşturulabilir
* Yeni gelişmiş alt dize aramamız sayesinde kapsama alınmış aramayı yeniden kullanıma sunduk, bu nedenle şu anda arama yapmak için iki seçeneğiniz vardır:
    * Genel arama-arama metin kutusuna bir arama terimi girmeniz yeterlidir
    * Kapsamlı arama-bir düğümün yanındaki büyüteç simgesine tıklayın, ardından yolun sonuna bir arama terimi ekleyin veya sağ tıklayıp "buradan ara" seçeneğini belirleyin.
* Çeşitli temalar ekledik: açık (varsayılan), koyu, Yüksek Karşıtlık siyah ve Yüksek Karşıtlık beyaz. Tema tercihinizi değiştirmek için Düzenle-&gt; temalara gidin
* Blob ve dosya özelliklerini değiştirebilirsiniz
* Artık kodlanmış (base64) ve kodlanmamış sıra iletilerini destekliyoruz
* Linux 'ta artık 64 bitlik bir işletim sistemi gereklidir. Bu sürüm için yalnızca 64-bit Ubuntu 16.04.1 LTS 'yi destekliyoruz
* Logonuzu güncelleştirdik!

#### <a name="fixes"></a>Düzeltilen

* Düzeltildi: ekran dondurma sorunları
* Düzeltildi: Gelişmiş güvenlik
* Düzeltildi: bazen yinelenen ekli hesaplar görünebilir
* Düzeltildi: tanımsız içerik türüne sahip bir blob özel durum oluşturabilir
* Düzeltildi: sorgu bölmesinin boş bir tablo üzerinde açılması mümkün değildi
* Düzeltildi: aramadaki hataları değiştirir
* Düzeltildi: "daha fazla yükle" seçeneğine tıklandığınızda 50 ' den 100 ' e yüklenen kaynak sayısı arttı
* Düzeltildi: ilk çalıştırmada, bir hesap oturum açmışsa, artık varsayılan olarak bu hesaba ait tüm abonelikleri seçeceğiz

#### <a name="known-issues"></a>Bilinen Sorunlar

* Depolama Gezgini bu sürümü Ubuntu 14,04 üzerinde çalışmıyor
* Aynı kaynak için birden çok sekme açmak için, aynı kaynağa sürekli olarak tıklamayın. Başka bir kaynağa tıklayın ve sonra geri dönüp özgün kaynağa tıklayarak başka bir sekmede yeniden açın
* Hızlı erişim yalnızca abonelik tabanlı öğeler ile kullanılabilir. Bu sürümde anahtar veya SAS belirteci ile eklenen yerel kaynaklar veya kaynaklar desteklenmez
* Sahip olduğunuz kaynak sayısına bağlı olarak, hedef kaynağa gitmek için birkaç saniye hızlı erişim gerekebilir
* 3 ' ten fazla BLOB grubunun veya aynı anda karşıya yüklenen dosyaların bulunması hatalara neden olabilir
* Tüm kabaca 50.000 düğümlerde arama tutamaçları aranıyor-bundan sonra performans etkilenebilir veya işlenmemiş özel duruma neden olabilir

10/03/2016
### <a name="version-085"></a>Sürüm 0.8.5

#### <a name="new"></a>Yeni

* Artık, depolama hesaplarına ve kaynaklarına eklemek için Portal tarafından oluşturulan SAS anahtarlarını kullanabilir

#### <a name="fixes"></a>Düzeltilen

* Düzeltildi: arama sırasında yarış durumu bazen düğümlerin Genişletilebilir olmayan hale gelmesine neden olabilir
* Düzeltildi: hesap adı ve anahtarı olan depolama hesaplarına bağlanılırken "HTTP kullan" çalışmıyor
* Düzeltildi: SAS anahtarları (özel olarak Portal tarafından oluşturulan) bir "sondaki eğik çizgi" hatası döndürür
* Düzeltildi: tablo içeri aktarma sorunları
    * Bölüm anahtarı ve satır anahtarı tersine çevrilir
    * "Null" bölüm anahtarları okunamıyor

#### <a name="known-issues"></a>Bilinen Sorunlar

* Tüm kabaca 50.000 düğümlerde arama tutamaçları aranıyor-bundan sonra performans etkilenebilir
* Azure Stack Şu anda dosyaları desteklemiyor, bu nedenle dosyaları genişletmeye çalışmak bir hata gösteriyor

09/12/2016
### <a name="version-084"></a>Sürüm 0.8.4

>[!VIDEO https://www.youtube.com/embed/cr5tOGyGrIQ?ecver=1]

#### <a name="new"></a>Yeni

* Kaynaklarınıza paylaşım ve kolay erişim için depolama hesaplarına, kapsayıcılara, kuyruklara, tablolara veya dosya paylaşımlarına doğrudan bağlantılar oluşturun-Windows ve Mac OS desteği
* Arama kutusundan blob kapsayıcıları, tabloları, kuyrukları, dosya paylaşımlarını veya depolama hesaplarını arayın
* Artık tablo sorgu oluşturucusunda yan tümceleri gruplandırabilirsiniz
* Blob kapsayıcılarını, dosya paylaşımlarını, tabloları, Blobları, blob klasörlerini, dosyaları ve dizinleri SAS ile bağlantılı hesaplar ve kapsayıcılar içinden yeniden adlandırma ve kopyalama/yapıştırma
* Blob kapsayıcılarını ve dosya paylaşımlarını yeniden adlandırmak ve kopyalamak artık özellikleri ve meta verileri korur

#### <a name="fixes"></a>Düzeltilen

* Düzeltildi: Boolean veya ikili özellikler içeriyorsa tablo varlıkları düzenlenemiyor

#### <a name="known-issues"></a>Bilinen Sorunlar

* Tüm kabaca 50.000 düğümlerde arama tutamaçları aranıyor-bundan sonra performans etkilenebilir

08/03/2016
### <a name="version-083"></a>Sürüm 0.8.3

>[!VIDEO https://www.youtube.com/embed/HeGW-jkSd9Y?ecver=1]

#### <a name="new"></a>Yeni

* Kapsayıcıları, tabloları, dosya paylaşımlarını yeniden adlandırma
* İyileştirilmiş Sorgu Oluşturucu deneyimi
* Sorguları kaydetme ve yükleme özelliği
* Kaynaklarınızın paylaşılması ve kolay bir şekilde erişmesi için depolama hesaplarının veya kapsayıcıların, kuyrukların, tabloların veya dosya paylaşımlarının doğrudan bağlantıları (yalnızca Windows-yalnızca-macOS desteği yakında kullanıma sunulacak!)
* CORS kurallarını yönetme ve yapılandırma özelliği

#### <a name="fixes"></a>Düzeltilen

* Düzeltildi: Microsoft hesapları her 8-12 saatte bir yeniden kimlik doğrulaması gerektirir

#### <a name="known-issues"></a>Bilinen Sorunlar

* Bazen kullanıcı arabirimi dondurulmuş görünebilir-pencere, bu sorunu çözmeye yardımcı olur
* macOS yüklemesi, yükseltilmiş izinler gerektirebilir
* Hesap Ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir
* Dosya paylaşımlarını, blob kapsayıcılarını ve tabloları yeniden adlandırma, dosya paylaşım kotası, genel erişim düzeyi veya erişim ilkeleri gibi, kapsayıcıda meta verileri veya diğer özellikleri korumaz
* Blobların yeniden adlandırılması (tek tek veya yeniden adlandırılmış blob kapsayıcısı içinde) anlık görüntüleri korumaz. Blob, dosya ve varlıkların diğer tüm özellikleri ve meta verileri yeniden adlandırma sırasında korunur
* Kaynakları kopyalamak veya yeniden adlandırmak SAS ile bağlantılı hesaplar içinde çalışmaz

07/07/2016
### <a name="version-082"></a>Sürüm 0.8.2

>[!VIDEO https://www.youtube.com/embed/nYgKbRUNYZA?ecver=1]

#### <a name="new"></a>Yeni

* Depolama hesapları aboneliklerle gruplandırılır; anahtar veya SAS aracılığıyla eklenen geliştirme depolaması ve kaynakları (yerel ve bağlı) düğüm altında gösteriliyor
* "Azure hesap ayarları" panelindeki hesaplardan oturumu Kapat
* Oturum açmayı etkinleştirmek ve yönetmek için proxy ayarlarını yapılandırma
* Blob kiraları oluşturma ve kesme
* Tek tıklamayla blob kapsayıcıları, kuyrukları, tabloları ve dosyaları açın

#### <a name="fixes"></a>Düzeltilen

* Düzeltildi: .NET veya Java kitaplıklarıyla eklenen kuyruk iletileri Base64 ile düzgün şekilde çözülemiyor
* Düzeltildi: $metrics tabloları BLOB depolama hesapları için gösterilmez
* Düzeltildi: tablolar düğümü yerel (geliştirme) depolama için çalışmıyor

#### <a name="known-issues"></a>Bilinen Sorunlar

* macOS yüklemesi, yükseltilmiş izinler gerektirebilir

06/15/2016
### <a name="version-080"></a>Sürüm 0.8.0

>[!VIDEO https://www.youtube.com/embed/ycfQhKztSIY?ecver=1]

>[!VIDEO https://www.youtube.com/embed/k4_kOUCZ0WA?ecver=1]

>[!VIDEO https://www.youtube.com/embed/3zEXJcGdl_k?ecver=1]

#### <a name="new"></a>Yeni

* Dosya paylaşma desteği: dosyaları ve dizinleri görüntüleme, karşıya yükleme, indirme, kopyalama, SAS URI 'Leri (oluşturma ve bağlanma)
* SAS URI 'Leri veya hesap anahtarları ile depolamaya bağlanmak için geliştirilmiş Kullanıcı deneyimi
* Tablo sorgusu sonuçlarını dışarı aktarma
* Tablo sütunu yeniden sıralama ve özelleştirme
* Etkin ölçümleri olan depolama hesapları için $logs blob kapsayıcıları ve $metrics tabloları görüntüleme
* Geliştirilmiş dışarı aktarma ve içeri aktarma davranışı artık Özellik değeri türünü içerir

#### <a name="fixes"></a>Düzeltilen

* Düzeltildi: büyük Blobları karşıya yüklemek veya indirmek tamamlanmamış karşıya yüklemelere/indirmelere neden olabilir
* Düzeltildi: sayısal dize değeri ("1") ile bir varlığı Düzenle, ekleme veya içeri aktarma işlemi Double 'a dönüştürecek
* Düzeltildi: yerel geliştirme ortamındaki Tablo düğümü genişletilemiyor

#### <a name="known-issues"></a>Bilinen Sorunlar

* $metrics tabloları BLOB depolama hesapları için görünür değil
* İletiler Base64 kodlaması kullanılarak kodlanmışsa programlı olarak eklenen kuyruk iletileri düzgün görüntülenmeyebilir

05/17/2016
### <a name="version-07201605090"></a>Sürüm 0.7.20160509.0

#### <a name="new"></a>Yeni

* Uygulama Kilitlenmelerinde daha iyi hata işleme

#### <a name="fixes"></a>Düzeltilen

* Bilgi çubuğu iletilerinin bazen oturum açma kimlik bilgileri gerektiğinde gösterilmediğinde oluşan hata düzeltildi

#### <a name="known-issues"></a>Bilinen Sorunlar

* Tablolar: "1" veya "1,0" gibi bir ındexattributes sayısal değeri olan bir özelliği olan bir varlığı ekleme, oluşturma veya içeri aktarma ve Kullanıcı bunu bir `Edm.String`olarak göndermeyi denediğinde, değer istemci API 'sine bir EDM. Double olarak geri gönderilir

03/31/2016

### <a name="version-07201603250"></a>Sürüm 0.7.20160325.0

>[!VIDEO https://www.youtube.com/embed/imbgBRHX65A?ecver=1]

>[!VIDEO https://www.youtube.com/embed/ceX-P8XZ-s8?ecver=1]

#### <a name="new"></a>Yeni

* Tablo desteği: varlıklar için görüntüleme, sorgulama, dışarı aktarma, içeri aktarma ve CRUD işlemleri
* Kuyruk desteği: iletileri görüntüleme, ekleme, sırası kaldırma
* Depolama hesapları için SAS URI 'Leri oluşturma
* SAS URI 'Leri ile depolama hesaplarına bağlanma
* Depolama Gezgini gelecekteki güncelleştirmeler için bildirimleri Güncelleştir
* Güncel görünüm ve kullanım

#### <a name="fixes"></a>Düzeltilen

* Performans ve güvenilirlik iyileştirmeleri

### <a name="known-issues-amp-mitigations"></a>Bilinen sorunlar &amp; azaltmaları

* Büyük blob dosyalarının indirilmesi doğru çalışmıyor-bu sorunu ele alırken AzCopy kullanmanızı öneririz
* Giriş klasörü bulunamazsa veya yazılamıyor ise hesap kimlik bilgileri alınmaz ve önbelleğe alınmaz
* "1" veya "1,0" gibi bir ındexattributes sayısal değeri olan bir özelliğine sahip bir varlık ekliyor, düzenliyoruz veya içeri aktardıysanız ve Kullanıcı bunu bir `Edm.String`olarak göndermeyi denediğinde, değer istemci API 'sinden Edm. Double olarak geri gelecektir.
* CSV dosyalarını çok satırlı kayıtlarla içeri aktarırken, veriler chopped veya karıştırılmış alabilir

02/03/2016

### <a name="version-07201601291"></a>Sürüm 0.7.20160129.1

#### <a name="fixes"></a>Düzeltilen

* Blobları karşıya yükleme, indirme ve kopyalama sırasında genel performans geliştirildi

01/14/2016

### <a name="version-07201601050"></a>Sürüm 0.7.20160105.0

#### <a name="new"></a>Yeni

* Linux desteği (OSX 'e yönelik eşlik özellikleri)
* Paylaşılan erişim Imzaları (SAS) anahtarına sahip blob kapsayıcıları ekleme
* Azure Çin 21Vianet için depolama hesapları ekleme
* Özel uç noktalara sahip depolama hesapları ekleme
* İçerik metnini ve resim bloblarını açın ve görüntüleyin
* Blob özelliklerini ve meta verileri görüntüleme ve düzenleme

#### <a name="fixes"></a>Düzeltilen

* Düzeltildi: çok sayıda blob yükleme veya indirme (500 +) bazen uygulamanın beyaz ekrana sahip olmasına neden olabilir
* Düzeltildi: blob kapsayıcısı genel erişim düzeyi ayarlanırken, kapsayıcıda odağı yeniden ayarlamadan yeni değer güncellenmez. Ayrıca, iletişim kutusu daima gerçek geçerli değeri değil, her zaman varsayılan olarak "ortak erişim yok" olarak belirlenmiştir.
* Daha iyi genel klavye/erişilebilirlik ve Kullanıcı arabirimi desteği
* İçerik haritaları geçmiş metni boşluk ile uzun olduğunda kaydırılır
* SAS iletişim kutusu giriş doğrulamasını destekler
* Kullanıcı kimlik bilgilerinin kullanım izni dolmuşsa bile yerel depolama kullanılabilir olmaya devam eder
* Açık bir blob kapsayıcısı silindiğinde, sağ taraftaki blob Gezgini kapatılır

#### <a name="known-issues"></a>Bilinen Sorunlar

* Linux yüklemesi için GCC sürümünün güncelleştirilmesi veya yükseltilmesi gerekiyor – yükseltme adımları aşağıda verilmiştir:
    * `sudo add-apt-repository ppa:ubuntu-toolchain-r/test`
    * `sudo apt-get update`
    * `sudo apt-get upgrade`
    * `sudo apt-get dist-upgrade`

11/18/2015
### <a name="version-07201511160"></a>Sürüm 0.7.20151116.0

#### <a name="new"></a>Yeni

* macOS ve Windows sürümleri
* Depolama hesaplarınızı görüntülemek için oturum açın – org hesabınızı, Microsoft hesabınızı, 2FA vb. kullanın.
* Yerel geliştirme depolaması (depolama öykünücüsünü kullanın, yalnızca Windows)
* Azure Resource Manager ve klasik kaynak desteği
* Blob, kuyruk veya tablo oluşturma ve silme
* Belirli blob 'ları, kuyrukları veya tabloları arayın
* Blob kapsayıcıları içeriğini keşfet
* Dizinler arasında görüntüleme ve gezinme
* Blob 'ları ve klasörleri karşıya yükleme, indirme ve silme
* Blob özelliklerini ve meta verileri görüntüleme ve düzenleme
* SAS anahtarları oluştur
* Depolanan erişim Ilkelerini yönetme ve oluşturma (SAP)
* Ön eke göre blob ara
* ' N Drop Files dosyalarını karşıya yüklemek veya indirmek için sürükleyin

#### <a name="known-issues"></a>Bilinen Sorunlar

* Blob kapsayıcısı genel erişim düzeyi ayarlanırken, kapsayıcıyı kapsayıcıya yeniden ayarlayıp yeni değer güncellenmez
* Genel erişim düzeyini ayarlamak için iletişim kutusunu açtığınızda, gerçek geçerli değer değil, her zaman varsayılan olarak "genel erişim yok" değerini gösterir
* İndirilen Bloblar yeniden adlandırılamıyor
* Bir hata oluştuğunda etkinlik günlüğü girdileri bazen devam eden bir durumunda "takılmış" olur ve hata gösterilmez
* Bazen çok sayıda Blobları karşıya yüklemeye veya indirmeye çalışırken tamamen bir veya daha fazla beyaz çökme bırakır
* Bazen kopyalama işleminin iptal edilmesi çalışmaz
* Kapsayıcı oluşturma sırasında (blob/kuyruk/tablo), geçersiz bir ad girsin ve farklı bir kapsayıcı türü altında başka bir oluşturma devam ederseniz, yeni tür üzerinde odağı ayarlayamazsınız
* Yeni klasör oluşturulamıyor veya klasör yeniden adlandırılamıyor




[2]: https://support.microsoft.com/en-us/help/4021389/storage-explorer-troubleshooting-guide
[3]: vs-azure-tools-storage-manage-with-storage-explorer.md
