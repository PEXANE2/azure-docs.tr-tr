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
ms.openlocfilehash: 986da8980a569583ef454833957ace85dd1bfbb6
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351071"
---
# <a name="microsoft-azure-storage-explorer-release-notes"></a>Microsoft Azure Depolama Gezgini sürüm notları

Bu makalede, Azure Depolama Gezgini için en son sürüm notları ve önceki sürümler için sürüm notları yer alır. 

[Microsoft Azure Depolama Gezgini,](./vs-azure-tools-storage-manage-with-storage-explorer.md) Windows, macOS ve Linux'taki Azure Depolama verileriyle kolayca çalışmanızı sağlayan bağımsız bir uygulamadır.

Depolama Gezgini'nin önceki sürümlerini indirmek için GitHub repo'muzun [Sürümler sayfasını](https://github.com/microsoft/AzureStorageExplorer/releases) ziyaret edebilirsiniz.

## <a name="version-1110"></a>Sürüm 1.11.0
11/4/2019

### <a name="new"></a>Yeni
* Blobs, ADLS Gen2 ve Yönetilen Diskler için operasyonlar entegre AzCopy kullanın. Daha spesifik olarak, aşağıdaki işlemler AzCopy kullanılarak yapılır:
   * Bloblar
      * Düzenleme için açık + Upload
      * Yükleme & bırakma dahil
      * İndirme
      * & yapıştır #1249 kopyala
      * Sil
   * ADLS Gen2 Lekeler
      * Yükleme & bırakma dahil
      * İndirme
      * & yapıştır
      * Klasör silme dahil silme
   * Yönetilen Diskler
      * Karşıya Yükle
      * İndirme
      * & yapıştır

   Ayrıca, tümleşik AzCopy deneyimine sık sık istenen birkaç özellik eklenmiştir:
   * Çakışma çözümleri - çakışmaları çözmek için aktarımlar sırasında istenir. #1455
   * Sayfa blobs olarak yükleyin - AzCopy yükler .vhd ve .vhdx dosyaları sayfa lekeleri olarak seçebilirsiniz. #1164 ve #1601
   * Yapılandırılabilir AzCopy parametreleri - AzCopy'nin performansını ve kaynak kullanımını ayarlamak için çeşitli ayarlar eklenmiştir. Daha fazla ayrıntıyı aşağıda görebilirsiniz.

* ADLS Gen2 ve Blobs çoklu protokol erişimi sağlamak ve ADLS Gen2 deneyimlerini daha da geliştirmek için, ADLS Gen2 hesapları için aşağıdaki özellikleri ekledik:
   * ACL izinlerini ayarlamak için dost adları kullanarak arama yapın
   * $logs ve $web gibi gizli kapsayıcıları görüntüleme
   * Konteyner kirası edinme ve kırma
   * Blob kira #848 edinme ve kırma
   * Kapsayıcı erişim ilkelerini yönetme
   * Blob erişim katmanlarını yapılandırma
   * & Yapıştır Blobs kopyalayın

* Bu sürümde, 17 ek dilönizleme vardır. Ayarlar sayfasında "Uygulama" → "Bölgesel Ayarlar" → "Dil (Önizleme)" altında istediğiniz dile geçebilirsiniz. Ek dizeleri çevirmek ve çeviri kalitesini artırmak için hala çok çalışıyoruz. Bir çeviri ile ilgili herhangi bir geribildirim varsa, ya da henüz çevrilmiş bir dize fark ederseniz, [Lütfen GitHub bir sorunu açın](https://github.com/microsoft/AzureStorageExplorer/issues/new?assignees=&labels=%F0%9F%8C%90%20localization&template=bug-report.md&title=).
* Her sürümde, depolama explorer'ı hassas bir şekilde döndürerek birkaç ayarı yapmaya çalışırız. Bu sürümde, AzCopy'yi daha da yapılandırmanın yanı sıra hizmet düğümlerini gizlemek için ayarlar ekledik:
   * AzCopy bant genişliği sınırı - AzCopy'nin ne kadar ağ kullandığını kontrol etmesine yardımcı olur. Bu ayarı "Transferler" → "AzCopy" → "Maksimum aktarım hızı"nda bulabilirsiniz. #1099
   * AzCopy MD5 denetimi - indirilirken MD5 hashes için azcopy denetler eğer ve ne kadar sıkı yapılandırma sağlar. Bu ayarı "Transferler" → "AzCopy" → "MD5'i Kontrol Et"te bulabilirsiniz.
   * AzCopy eşzamanlılık ve bellek arabellek boyutu - varsayılan Olarak AzCopy bu ayarlar için makul varsayılan değerleri belirlemek için makinenizi analiz edecektir. Ancak performans sorunlarıyla karşılaşırsanız, bu gelişmiş ayarlar AzCopy'nin bilgisayarınızda nasıl çalıştığını daha da uyarlamak için kullanılabilir. Bu ayarları "Transferler" → "AzCopy" altında bulabilirsiniz. #994
   * Hizmet düğümlerini görüntüleyin ve gizle - bu ayarlar, Depolama Gezgini'nin desteklediği Azure hizmetlerinden herhangi birini görüntüleme veya gizleme seçenekleri sunar. Bu ayarları "Hizmetler" bölümünde bulabilirsiniz. #1877

* Yönetilen Disk'in Anlık Görüntüsü oluşturulurken, varsayılan ad şimdi sağlanır. #1847
* Azure AD ile iliştirirken, bir ADLS Gen2 Blob kapsayıcısı takininiz, düğümün yanında "(ADLS Gen2)" gösterilir. #1861

### <a name="fixes"></a>Düzeltmeler
* Büyük Diskleri kopyalarken, yüklerken veya indirirken, Depolama Gezgini bazen işlemle ilgili disklere erişimi iptal etmekte başarısız olur. Bu sorun düzeltildi. #2048
* Bölüm anahtarı sorgusunu görüntülerken tablo istatistikleri başarısız oldu. Bu sorun düzeltildi. #1886

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini 1.11.0 artık ADLS Gen2 kapsayıcılarına takmak için bir DFS bitiş noktası ("myaccount.dfs.core.windows.net" gibi) gerektirir. Depolama Gezgini'nin önceki sürümlerinde bir blob bitiş noktası kullanmanıza izin verilir. Bu ekler, 1.11.0'a yükselttikten sonra artık çalışmayabilir. Bu sorunla karşılaşırsanız, DFS bitiş noktasını kullanarak yeniden takın.
* Sayısal ayarların geçerli bir aralıkta olup olmadığı denetlenmez.#2140
* Blob kaplarını ağaç görünümünde bir depolama hesabından diğerine kopyalamak başarısız olabilir. Konuyu araştırıyoruz.#2124
* Otomatik Yenileme ayarı henüz Blob Explorer'daki tüm işlemleri etkilemez.
* Yönetilen Disk özellikleri Azure Yığını'nda desteklenmez.
* Disk yükleme veya yapıştırma başarısız olursa ve hatadan önce yeni bir Disk oluşturulursa, Depolama Gezgini Diski sizin için silmez.
* Disk yüklemesini veya yapıştırmasını ne zaman iptal ettiğinize bağlı olarak, yeni Diski bozuk bir durumda bırakmak mümkündür. Bu durumda, yeni Disk'i silmeniz veya Diskin içeriğini değiştirmek için disk API'lerini artık bozulmaması için el ile aramanız gerekir.
* Depolama Gezgini, RBAC kullanırken depolama kaynaklarınıza erişmek için bazı yönetim katmanı izinleri gerektirir. Daha fazla bilgi için [sorun giderme kılavuzuna](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) bakın.
* Blob kapsayıcısı gibi SAS URI aracılığıyla bağlı bir kaynaktan ayırmak, diğer eklerin doğru şekilde gösterilmesini engelleyen bir hataya neden olabilir. Bu sorunu çözmek için grup düğümlerini yenilemeniz gerekiyor. Daha fazla bilgi için #537 bakın.
* Mac için VS kullanıyorsanız ve özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu çözmek için ~/'un içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak engelini kaldırmazsa, bu konuda yorum yapın.
* Azurite henüz tam olarak tüm Depolama API'leri uygulanmamıştır. Bu nedenle, geliştirme depolama için Azurite kullanırken beklenmeyen hatalar veya davranış olabilir.
* Nadir durumlarda, ağaç odağı Hızlı Erişim'e takılabilir. Odağı çıkarmak için Tümlerini Yenileyebilirsiniz.
* OneDrive klasörünüzden yükleme, NodeJS'teki bir hata nedeniyle çalışmaz. Hata düzeltildi, ancak henüz Electron'a entegre edilmedi. Bir blob kapsayıcısına yüklerken veya bir kutudan indirirken bu sorunu çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını aşağıdaki özellikleri desteklemez. Azure Yığını kaynaklarıyla çalışırken bu özellikleri kullanmaya çalışmak beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici Silme
   * ADLS Gen2
   * Yönetilen Diskler
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Depolama Gezgini'ni Linux'ta çalıştırmak için önce belirli bağımlılıkların yüklenmesi gerekir. Daha fazla bilgi için Depolama Gezgini [sorun giderme kılavuzunu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#linux-dependencies) kontrol edin.

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
* Bazı kullanıcılar ADLS Gen 1 hesaplarında verilerini görüntülemeye çalışırken 1.10.0'da bir hatayla karşılaştılar. Bu hata, explorer panelinin düzgün işlemesini engelledi. Bu sorun düzeltildi. #1853 #1865

### <a name="new"></a>Yeni
* Depolama Gezgini artık özel bir Ayarlar Kullanıcı Arabirimi'ne sahiptir. Buna Düzenleme → Ayarları'ndan veya sol daki dikey araç çubuğundaki Ayarlar simgesine (vites) tıklayarak erişebilirsiniz. Bu özellik, [kullanıcı nın istediği çeşitli ayarları](https://github.com/microsoft/AzureStorageExplorer/labels/%3Abulb%3A%20setting%20candidate)sağlama yolunda attığımız ilk adımdır. Bu sürümden başlayarak aşağıdaki ayarlar desteklenir:
  * Tema
  * Ara sunucu
  * Çıkış #6 giriş
  * Aygıt kodu akışı oturum açma
  * Otomatik yenileme #1526
  * Azcopy etkinleştirin
  * AzCopy SAS süresi Eklenen görmek istediğiniz başka ayarlar varsa, lütfen Görmek istediğiniz ayarı açıklayan [GitHub'da bir sorun açın.](https://github.com/microsoft/AzureStorageExplorer/issues/new?assignees=&labels=%3Abulb%3A%20setting%20candidate&template=feature_request.md&title=)
* Depolama Gezgini artık Yönetilen Diskleri destekler. Şunları yapabilirsiniz:
  * Yeni bir Diske şirket içi VHD yükleme
  * Disk İndir
  * Diskleri kaynak grupları ve bölgeler arasında kopyalama ve yapıştır
  * Diskleri Sil
  * Diskin Anlık Görüntüsü Oluşturma

Disklerin yüklenmesi, indirilmesi ve bölgeler arası kopyalanması AzCopy v10 tarafından desteklenmektedir.
* Storage Explorer artık Linux'taki Snap mağazası üzerinden yüklenebilir. Snap mağazası üzerinden yüklediğinizde, .NET Core dahil olmak üzere tüm bağımlılıklar sizin için yüklenir! Şu anda Depolama Explorer Ubuntu ve CentOS iyi çalıştığını doğruladı. Snap mağazasından diğer Linux dağıtımlarında yükleme sorunlarıyla karşılaşırsanız, lütfen [GitHub'da bir sorun açın.](https://github.com/microsoft/AzureStorageExplorer/issues/new?assignees=&labels=snaps&template=bug-report.md&title=) Snap mağazasından yükleme hakkında daha fazla bilgi edinmek için [başlangıç kılavuzumuza](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=linux)bakın. #68
* Özelliği ADLS Gen2 kullanıcıları için daha kullanışlı hale getirmek amacıyla Azure Active Directory (Azure AD) ile eklemek için iki önemli değişiklik yapıldı:
  * Artık iliştirdiğiniz kaynağın içinde olduğu kiracıyı seçin. Bu, artık kaynağın aboneliğine RBAC erişimine sahip olmanız gerekolmadığı anlamına gelir.
  * Bir ADLS Gen2 Blob Konteyneri ekiyorsanız, artık kapsayıcıda belirli bir yola ekleyebilirsiniz.
* ADLS Gen2 dosyaları ve klasörleri için Aç'ları yönetirken, Depolama Gezgini artık ACL'deki varlıkların dostu adlarını gösterir. #957
* OID aracılığıyla BIR ADLS Gen2 ACL'ye eklerken, Depolama Gezgini artık OID'nin kiracınızdaki geçerli bir varlığa ait olduğunu doğrular. #1603
* Sekmeler arasında gezinmek için klavye kısayolları artık daha fazla standart tuş birleşimi kullanıyor. #1018
* Sekmede orta tıklama şimdi kapatılır. #1348
* AzCopy aktarımatlamaları ve hiçbir hata içeriyorsa, Depolama Gezgini artık atlamaların oluştuğunu vurgulamak için bir uyarı simgesi gösterir. #1490
* Entegre AzCopy sürüm 10.2.1 olarak güncellendi. Ayrıca, artık AzCopy sürümünü görüntüleyebilirsiniz Hakkında iletişim kutusunda yüklü. #1343

### <a name="fixes"></a>Düzeltmeler
* Birçok kullanıcı, ekli Depolama Hesapları ile çalışırken çeşitli "tanımlanmamış sürümü okuyamıyor" veya "tanımlanmamış hataların bağlantısını okuyamıyor" ile karşılaştı. Bu sorunun temel nedenini araştırmaya devam etsek de, 1.10.0'da ekli Depolama Hesapları yükleme yle ilgili hata işlemeyi geliştirdik. #1626, #985 ve #1532
* Kaşif ağacının (sol taraf) odaknoktasının tekrar tekrar üst düğüme atlayacağı bir duruma girmesi mümkündü. Bu sorun düzeltildi. #1596
* Bir blob'un anlık görüntülerini yönetirken, ekran okuyucular anlık görüntüyle ilişkili zaman damgasını okumaz. Bu sorun düzeltildi. #1202
* macOS'taki proxy ayarı kimlik doğrulama işleminin bunları kullanması için zamanında ayarlanmıyordu. Bu sorun düzeltildi. #1567
* Egemen bir buluttaki bir Depolama Hesabı ad ve anahtar kullanılarak eklenmişse, AzCopy çalışmaz. Bu sorun düzeltildi. #1544
* Bir bağlantı dizesi üzerinden bağlanırken, Depolama Gezgini artık sondaki boşlukları kaldırır. #1387

### <a name="known-issues"></a>Bilinen Sorunlar
* Otomatik Yenileme ayarı henüz Blob Explorer'daki tüm işlemleri etkilemez.
* Yönetilen Disk özellikleri Azure Yığını'nda desteklenmez.
* Disk yükleme veya yapıştırma başarısız olursa ve hatadan önce yeni bir Disk oluşturulursa, Depolama Gezgini Diski sizin için silmez.
* Disk yüklemesini veya yapıştırmasını ne zaman iptal ettiğinize bağlı olarak, yeni Diski bozuk bir durumda bırakmak mümkündür. Bu durumda, yeni Disk'i silmeniz veya Diskin içeriğini değiştirmek için disk API'lerini artık bozulmaması için el ile aramanız gerekir.
* Disk yüklemesini veya yapıştırmasını ne zaman iptal ettiğinize bağlı olarak, yeni Diski bozuk bir durumda bırakmak mümkündür. Bu durumda, yeni Disk'i silmeniz veya Diskin içeriğini değiştirmek için disk API'lerini artık bozulmaması için el ile aramanız gerekir.
* AzCopy Blob olmayan bir indirme gerçekleştirirken, büyük dosyalar için MD5 doğrulanmıyor. Bunun nedeni Depolama SDK'daki bir hatadır. [#1212](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1212)
* Depolama Gezgini, RBAC kullanırken depolama kaynaklarınıza erişmek için bazı yönetim katmanı izinleri gerektirir. Daha fazla bilgi için [sorun giderme kılavuzuna](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) bakın.
* Blob kapsayıcısı gibi SAS URI aracılığıyla bağlı bir kaynaktan ayırmak, diğer eklerin doğru şekilde gösterilmesini engelleyen bir hataya neden olabilir. Bu sorunu çözmek için grup düğümlerini yenilemeniz gerekiyor. Daha fazla bilgi için #537 bakın.
* Mac için VS kullanıyorsanız ve özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu çözmek için ~/'un içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak engelini kaldırmazsa, bu konuda yorum yapın.
* Azurite henüz tam olarak tüm Depolama API'leri uygulanmamıştır. Bu nedenle, geliştirme depolama için Azurite kullanırken beklenmeyen hatalar veya davranış olabilir.
* Nadir durumlarda, ağaç odağı Hızlı Erişim'e takılabilir. Odağı çıkarmak için Tümlerini Yenileyebilirsiniz.
* OneDrive klasörünüzden yükleme, NodeJS'teki bir hata nedeniyle çalışmaz. Hata düzeltildi, ancak henüz Electron'a entegre edilmedi. Bir blob kapsayıcısına yüklerken veya bir kutudan indirirken bu sorunu çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını aşağıdaki özellikleri desteklemez. Azure Yığını kaynaklarıyla çalışırken bu özellikleri kullanmaya çalışmak beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici Silme
   * ADLS Gen2
   * Yönetilen Diskler
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Depolama Gezgini'ni Linux'ta çalıştırmak için önce belirli bağımlılıkların yüklenmesi gerekir. Daha fazla bilgi için Depolama Gezgini [sorun giderme kılavuzunu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#linux-dependencies) kontrol edin.


## <a name="version-1100"></a>Sürüm 1.10.0
9/12/2019

### <a name="new"></a>Yeni

* Depolama Gezgini artık özel bir Ayarlar Kullanıcı Arabirimi'ne sahiptir. Buna Düzenleme → Ayarları'ndan veya sol daki dikey araç çubuğundaki Ayarlar simgesine (vites) tıklayarak erişebilirsiniz. Bu özellik, [kullanıcı nın istediği çeşitli ayarları](https://github.com/microsoft/AzureStorageExplorer/labels/%3Abulb%3A%20setting%20candidate)sağlama yolunda attığımız ilk adımdır. Bu sürümden başlayarak aşağıdaki ayarlar desteklenir:
    * Tema
    * Ara sunucu
    * Çıkış [#6](https://www.github.com/Microsoft/AzureStorageExplorer/issues/6) giriş
    * Aygıt kodu akışı oturum açma
    * Otomatik yenileme [#1526](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1526)
    * Azcopy etkinleştirin
    * AzCopy SAS süresi

    Eklenen görmek istediğiniz başka ayarlar varsa, lütfen [GitHub'da görmek istediğiniz ayarı açıklayan bir sorunu açın.](https://github.com/microsoft/AzureStorageExplorer/issues/new?assignees=&labels=%3Abulb%3A%20setting%20candidate&template=feature_request.md&title=)
* Depolama Gezgini artık Yönetilen Diskleri destekler. Şunları yapabilirsiniz:
    * Yeni bir Diske şirket içi VHD yükleme
    * Disk İndir
    * Diskleri kaynak grupları ve bölgeler arasında kopyalama ve yapıştır
    * Diskleri Sil
    * Diskin Anlık Görüntüsü Oluşturma

    Disklerin yüklenmesi, indirilmesi ve bölgeler arası kopyalanması AzCopy v10 tarafından desteklenmektedir.
* Storage Explorer artık Linux'taki Snap mağazası üzerinden yüklenebilir. Snap mağazası üzerinden yüklediğinizde, .NET Core dahil olmak üzere tüm bağımlılıklar sizin için yüklenir! Şu anda Depolama Explorer Ubuntu ve CentOS iyi çalıştığını doğruladı. Snap mağazasından diğer Linux dağıtımlarında yükleme sorunlarıyla karşılaşırsanız, lütfen [GitHub'da bir sorun açın.](https://github.com/microsoft/AzureStorageExplorer/issues/new?assignees=&labels=snaps&template=bug-report.md&title=) Snap mağazasından yükleme hakkında daha fazla bilgi edinmek için [başlangıç kılavuzumuza](https://aka.ms/storageexplorer/snapinformation)bakın. [#68](https://www.github.com/Microsoft/AzureStorageExplorer/issues/68)
* Özelliği ADLS Gen2 kullanıcıları için daha kullanışlı hale getirmek amacıyla Azure Active Directory (Azure AD) ile eklemek için iki önemli değişiklik yapıldı: * Artık eklediğiniz kaynağın bulunduğu kiracıyı seçin. Bu, artık kaynağın aboneliğine RBAC erişimine sahip olmanız gerekolmadığı anlamına gelir.
        * Bir ADLS Gen2 Blob Konteyner bağlıyorsanız, şimdi konteyner belirli bir yol ekleyebilirsiniz.
* ADLS Gen2 dosyaları ve klasörleri için Aç'ları yönetirken, Depolama Gezgini artık ACL'deki varlıkların dostu adlarını gösterir. [#957](https://www.github.com/Microsoft/AzureStorageExplorer/issues/957)
* OID aracılığıyla BIR ADLS Gen2 ACL'ye eklerken, Depolama Gezgini artık OID'nin kiracınızdaki geçerli bir varlığa ait olduğunu doğrular. [#1603](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1603)
* Sekmeler arasında gezinmek için klavye kısayolları artık daha fazla standart tuş birleşimi kullanıyor. [#1018](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1018)
* Sekmede orta tıklama şimdi kapatılır. [#1348](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1348)
* AzCopy aktarımatlamaları ve hiçbir hata içeriyorsa, Depolama Gezgini artık atlamaların oluştuğunu vurgulamak için bir uyarı simgesi gösterir. [#1490](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1490)
* Entegre AzCopy sürüm 10.2.1 olarak güncellendi. Ayrıca, artık AzCopy sürümünü görüntüleyebilirsiniz Hakkında iletişim kutusunda yüklü. [#1343](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1343)

### <a name="fixes"></a>Düzeltmeler

* Birçok kullanıcı, ekli Depolama Hesapları ile çalışırken çeşitli "tanımlanmamış sürümü okuyamıyor" veya "tanımlanmamış hataların bağlantısını okuyamıyor" ile karşılaştı. Bu sorunun temel nedenini araştırmaya devam etsek de, 1.10.0'da ekli Depolama Hesapları yükleme yle ilgili hata işlemeyi geliştirdik. [#1626](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1626), [#985](https://www.github.com/Microsoft/AzureStorageExplorer/issues/985)ve [#1532](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1532)
* Kaşif ağacının (sol taraf) odaknoktasının tekrar tekrar üst düğüme atlayacağı bir duruma girmesi mümkündü. Bu sorun düzeltildi. [#1596](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1596)
* Bir blob'un anlık görüntülerini yönetirken, ekran okuyucular anlık görüntüyle ilişkili zaman damgasını okumaz. Bu sorun düzeltildi. [#1202](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1202)
* macOS'taki proxy ayarı kimlik doğrulama işleminin bunları kullanması için zamanında ayarlanmıyordu. Bu sorun düzeltildi. [#1567](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1567)
* Egemen bir buluttaki bir Depolama Hesabı ad ve anahtar kullanılarak eklenmişse, AzCopy çalışmaz. Bu sorun düzeltildi. [#1544](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1544)
* Bir bağlantı dizesi üzerinden bağlanırken, Depolama Gezgini artık sondaki boşlukları kaldırır. [#1387](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1387)

### <a name="known-issues"></a>Bilinen Sorunlar

* Otomatik Yenileme ayarı henüz Blob Explorer'daki tüm işlemleri etkilemez.
* Yönetilen Disk özellikleri Azure Yığını'nda desteklenmez.
* Disk yükleme veya yapıştırma başarısız olursa ve hatadan önce yeni bir Disk oluşturulursa, Depolama Gezgini Diski sizin için silmez.
* Disk yüklemesini veya yapıştırmasını ne zaman iptal ettiğinize bağlı olarak, yeni Diski bozuk bir durumda bırakmak mümkündür. Bu durumda, yeni Disk'i silmeniz veya Diskin içeriğini değiştirmek için disk API'lerini artık bozulmaması için el ile aramanız gerekir.
* AzCopy Blob olmayan bir indirme gerçekleştirirken, büyük dosyalar için MD5 doğrulanmıyor. Bunun nedeni Depolama SDK'daki bir hatadır. [#1212](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1212)
* Depolama Gezgini, RBAC kullanırken depolama kaynaklarınıza erişmek için bazı yönetim katmanı izinleri gerektirir. Daha fazla bilgi için [sorun giderme kılavuzuna](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) bakın.
* Blob kapsayıcısı gibi SAS URI aracılığıyla bağlı bir kaynaktan ayırmak, diğer eklerin doğru şekilde gösterilmesini engelleyen bir hataya neden olabilir. Bu sorunu çözmek için grup düğümlerini yenilemeniz gerekiyor. Daha fazla bilgi için #537 bakın.
* Mac için VS kullanıyorsanız ve özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu çözmek için ~/'un içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak engelini kaldırmazsa, bu konuda yorum yapın.
* Azurite henüz tam olarak tüm Depolama API'leri uygulanmamıştır. Bu nedenle, geliştirme depolama için Azurite kullanırken beklenmeyen hatalar veya davranış olabilir.
* Nadir durumlarda, ağaç odağı Hızlı Erişim'e takılabilir. Odağı çıkarmak için Tümlerini Yenileyebilirsiniz.
* OneDrive klasörünüzden yükleme, NodeJS'teki bir hata nedeniyle çalışmaz. Hata düzeltildi, ancak henüz Electron'a entegre edilmedi. Bir blob kapsayıcısına yüklerken veya bir kutudan indirirken bu sorunu çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını aşağıdaki özellikleri desteklemez. Azure Yığını kaynaklarıyla çalışırken bu özellikleri kullanmaya çalışmak beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici Silme
   * ADLS Gen2
   * Yönetilen Diskler
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Depolama Gezgini'ni Linux'ta çalıştırmak için önce belirli bağımlılıkların yüklenmesi gerekir. Daha fazla bilgi için Depolama Gezgini [sorun giderme kılavuzunu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#linux-dependencies) kontrol edin.

## <a name="version-190"></a>Sürüm 1.9.0
7/1/2019

### <a name="download-azure-storage-explorer-190"></a>Azure Depolama Gezgini 1.9.0 İndir
- [Windows için Azure Depolama Gezgini 1.9.0](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Mac için Azure Depolama Gezgini 1.9.0](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Linux için Azure Depolama Gezgini 1.9.0](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a>Yeni

* Artık Azure AD (RBAC veya ACL izinleri) aracılığıyla Blob kapsayıcılarını ekleyebilirsiniz. Bu özellik, kapsayıcılara erişimi olan ancak kapsayıcıların içinde bulunan Depolama Hesapları'na olmayan kullanıcılara yardımcı olmak için tasarlanmıştır. Bu özellik hakkında daha fazla bilgi için Başlangıç Kılavuzumuza bakın.
* Satın almak ve kira kırmak şimdi RBAC ile çalışır. [#1354](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1354)
* Erişim ilkelerini yönetme ve genel erişim düzeyini ayarlama artık RBAC ile birlikte çalışır. [#1355](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1355)
* Blob klasörlerini silme artık RBAC ile çalışır. [#1450](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1450)
* Blob erişim katmanını değiştirme artık RBAC ile çalışır. [#1446](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1446)
* Artık "Yardım" → "Sıfırla" ile Hızlı Erişim'i hızla sıfırlayabilirsiniz. [#1327](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1327)

### <a name="preview-features"></a>Önizleme Özellikleri

* Aygıt kodu akış oturum açma artık önizleme için kullanılabilir. Etkinleştirmek için "Önizleme" → "Aygıt Kodu Akışı Oturum Aç'ı kullan"a gidin. Boş oturum açma pencereleri ile ilgili sorunları olan tüm kullanıcıların bu özelliği denemesini öneririz, çünkü oturum açmanın daha güvenilir bir biçimi olabilir.
* AzCopy ile entegre Depolama Explorer şu anda önizleme için kullanılabilir. Etkinleştirmek için "Önizleme" → "Geliştirilmiş Blob Yükleme ve İndirme için AzCopy'i kullanın" "gidin. AzCopy ile tamamlanan Blob aktarımları daha hızlı ve daha performant olmalıdır.

### <a name="fixes"></a>Düzeltmeler

* Bir hesap için 50'den fazla abonelik yükleyemediği sabit. [#1416](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1416)
* Doğrudan bağlantı başarısız olduğunda görünen bilgi çubuğunda çalışmayan "Oturum Aç" düğmesi düzeltildi. [#1358](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1358)
* MacOS'a .app dosyalarını yüklenmemek düzeltildi. [#1119](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1119)
* "Tümlerini Yeniden Deneme" başarısız bir blob yeniden adı için çalışmıyor düzeltildi. [#992](https://www.github.com/Microsoft/AzureStorageExplorer/issues/992)
* Bir blob açarken çalışmadığı "İptal" düzeltildi. [#1464](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1464)
* Ürün genelinde birden çok yazım ve araç ipucu sorunu giderildi. Bu sorunları rapor herkese çok teşekkürler! [#1303](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1303), [#1328](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1328), [#1329](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1329), [#1331](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1331), [#1336](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1336), [#1352](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1352), [#1368](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1368), [#1395](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1395)

### <a name="known-issues"></a>Bilinen Sorunlar

* AzCopy Blob olmayan bir indirme gerçekleştirirken, büyük dosyalar için MD5 doğrulanmıyor. Bunun nedeni Depolama SDK'daki bir hatadır. [#1212](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1212)
* Depolama Gezgini, RBAC kullanırken depolama kaynaklarınıza erişmek için bazı yönetim katmanı izinleri gerektirir. Daha fazla bilgi için [sorun giderme kılavuzuna](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) bakın.
* Bir proxy arkasında ADLS Gen2 Blobs erişmeye çalışırken başarısız olabilir.
* Blob kapsayıcısı gibi SAS URI aracılığıyla bağlı bir kaynaktan ayırmak, diğer eklerin doğru şekilde gösterilmesini engelleyen bir hataya neden olabilir. Bu sorunu çözmek için grup düğümlerini yenilemeniz gerekiyor. Daha fazla bilgi için #537 bakın.
* Mac için VS kullanıyorsanız ve özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu çözmek için ~/'un içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak engelini kaldırmazsa, bu konuda yorum yapın.
* Azurite henüz tam olarak tüm Depolama API'leri uygulanmamıştır. Bu nedenle, geliştirme depolama için Azurite kullanırken beklenmeyen hatalar veya davranış olabilir.
* Nadir durumlarda, ağaç odağı Hızlı Erişim'e takılabilir. Odağı çıkarmak için Tümlerini Yenileyebilirsiniz.
* OneDrive klasörünüzden yükleme, NodeJS'teki bir hata nedeniyle çalışmaz. Hata düzeltildi, ancak henüz Electron'a entegre edilmedi. Bir blob kapsayıcısına yüklerken veya bir kutudan indirirken bu sorunu çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını aşağıdaki özellikleri desteklemez. Azure Yığını kaynaklarıyla çalışırken bu özellikleri kullanmaya çalışmak beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici Silme
   * ADLS Gen2
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Depolama Gezgini'ni Linux'ta çalıştırmak için önce belirli bağımlılıkların yüklenmesi gerekir. Daha fazla bilgi için Depolama Gezgini [sorun giderme kılavuzunu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#linux-dependencies) kontrol edin.

## <a name="version-181"></a>Sürüm 1.8.1
5/13/2019

### <a name="hotfixes"></a>Düzeltmeler
* Bazı durumlarda, kaynak düzeyinde "Daha fazla yükle" seçeneğini tıklatmak bir sonraki kaynak sayfasını döndürmez. Bu sorun düzeltildi. [#1359](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1359)
* Windows'da, tek bir dosya veya klasör indiriliyorsa ve dosya veya klasörün adı Windows yolu için geçersiz bir karaktere sahipse AzCopy karşıdan yüklemeleri başarısız olur. Bu sorun düzeltildi. [#1350](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1350)
* Son derece nadir durumlarda, Dosya Paylaşımı'nın yeniden adı veya Dosya Paylaşımı'nda yeniden ad kullanılırken, yeniden adın kopyaları başarısız olduysa veya Depolama Keşfi, Azure ile kopyaların başarısını doğrulayamıyorsa, Depolama Gezgini'nin Dosya Paylaşımı'nı silme potansiyeli vardı kopya bitmeden önce orijinal dosyalar. Bu sorun düzeltildi.

### <a name="new"></a>Yeni

* Tümleşik AzCopy sürümü sürüm 10.1.0 olarak güncellendi.
* Ctrl/Cmd+R artık şu anda odaklanmış olan düzenleyiciyi yenilemek için kullanılabilir. [#1097](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1097)
* Azure Yığını Depolama API sürümü 2017-04-17 olarak değiştirildi.
* ADLS Gen2 için Access İletişimI Yönet artık Maskeyi diğer POSIX izinleri araçlarına benzer şekilde senkronize edecektir. Kullanıcı Arabirimi, bir kullanıcının veya grubun izinlerinin Maske'nin sınırlarını aşmasını sağlayan bir değişiklik yapılırsa sizi de uyarır. [#1253](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1253)
* AzCopy yüklemeleri için, MD5 karmasını hesaplayan ve ayarlayan bayrak artık etkinleştirilir. [#1223](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1223)


### <a name="preview-features"></a>Önizleme Özellikleri

* Aygıt kodu akış oturum açma artık önizleme için kullanılabilir. Etkinleştirmek için "Önizleme" → "Aygıt Kodu Akışı Oturum Aç'ı kullan"a gidin. Boş oturum açma pencereleri ile ilgili sorunları olan tüm kullanıcıların bu özelliği denemesini öneririz, çünkü oturum açmanın daha güvenilir bir biçimi olabilir.
* AzCopy ile entegre Depolama Explorer şu anda önizleme için kullanılabilir. Etkinleştirmek için "Önizleme" → "Geliştirilmiş Blob Yükleme ve İndirme için AzCopy'i kullanın" "gidin. AzCopy ile tamamlanan Blob aktarımları daha hızlı ve daha performant olmalıdır.

### <a name="fixes"></a>Düzeltmeler

* Access İlkeleri iletişim kutusu artık son kullanma tarihi olmayan Depolama Erişim İlkeleri'nde bir son kullanma tarihi belirlemez. [#764](https://www.github.com/Microsoft/AzureStorageExplorer/issues/764)
* SAS oluştururken Depolanan Erişim İlkeleri'nin doğru kullanıldığından emin olmak için SAS oluştur iletişim kutusunda bazı değişiklikler yapılmıştır. [#1269](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1269)
* 512 bayt olmayan bir dosyayı blob sayfasına yüklemeye çalışırken, Depolama Gezgini artık daha alakalı bir hata ortaya çıkaracaktır. [#1050](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1050)
* Bir görüntü adı kullanan bir Blob kapsayıcıkopyalama başarısız olur. Blob konteynerinin gerçek adı kullanılır. [#1166](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1166)
* AdlS Gen2 klasöründe, adında unicode karakterleri olan belirli eylemleri gerçekleştirmeye çalışmak başarısız olur. Artık tüm eylemler işe yaramalı. [#980](https://www.github.com/Microsoft/AzureStorageExplorer/issues/980)

### <a name="known-issues"></a>Bilinen Sorunlar

* AzCopy Blob olmayan bir indirme gerçekleştirirken, büyük dosyalar için MD5 doğrulanmıyor. Bunun nedeni Depolama SDK'daki bir hatadır. [#1212](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1212)
* Depolama Gezgini, RBAC kullanırken depolama kaynaklarınıza erişmek için bazı yönetim katmanı izinleri gerektirir. Daha fazla bilgi için [sorun giderme kılavuzuna](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) bakın.
* Bir proxy arkasında ADLS Gen2 Blobs erişmeye çalışırken başarısız olabilir.
* Blob kapsayıcısı gibi SAS URI aracılığıyla bağlı bir kaynaktan ayırmak, diğer eklerin doğru şekilde gösterilmesini engelleyen bir hataya neden olabilir. Bu sorunu çözmek için grup düğümlerini yenilemeniz gerekiyor. Daha fazla bilgi için #537 bakın.
* Mac için VS kullanıyorsanız ve özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu çözmek için ~/'un içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak engelini kaldırmazsa, bu konuda yorum yapın.
* Azurite henüz tam olarak tüm Depolama API'leri uygulanmamıştır. Bu nedenle, geliştirme depolama için Azurite kullanırken beklenmeyen hatalar veya davranış olabilir.
* Nadir durumlarda, ağaç odağı Hızlı Erişim'e takılabilir. Odağı çıkarmak için Tümlerini Yenileyebilirsiniz.
* OneDrive klasörünüzden yükleme, NodeJS'teki bir hata nedeniyle çalışmaz. Hata düzeltildi, ancak henüz Electron'a entegre edilmedi. Bir blob kapsayıcısına yüklerken veya bir kutudan indirirken bu sorunu çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını aşağıdaki özellikleri desteklemez. Azure Yığını kaynaklarıyla çalışırken bu özellikleri kullanmaya çalışmak beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici Silme
   * ADLS Gen2
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Depolama Gezgini'ni Linux'ta çalıştırmak için önce belirli bağımlılıkların yüklenmesi gerekir. Daha fazla bilgi için Depolama Gezgini [sorun giderme kılavuzunu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#linux-dependencies) kontrol edin.

## <a name="version-180"></a>Sürüm 1.8.0
01.05.2019

### <a name="new"></a>Yeni

* Tümleşik AzCopy sürümü sürüm 10.1.0 olarak güncellendi.
* Ctrl/Cmd+R artık şu anda odaklanmış olan düzenleyiciyi yenilemek için kullanılabilir. [#1097](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1097)
* Azure Yığını Depolama API sürümü 2017-04-17 olarak değiştirildi.
* ADLS Gen2 için Access İletişimI Yönet artık Maskeyi diğer POSIX izinleri araçlarına benzer şekilde senkronize edecektir. Kullanıcı Arabirimi, bir kullanıcının veya grubun izinlerinin Maske'nin sınırlarını aşmasını sağlayan bir değişiklik yapılırsa sizi de uyarır. [#1253](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1253)
* AzCopy yüklemeleri için, MD5 karmasını hesaplayan ve ayarlayan bayrak artık etkinleştirilir. [#1223](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1223)


### <a name="preview-features"></a>Önizleme Özellikleri

* Aygıt kodu akış oturum açma artık önizleme için kullanılabilir. Etkinleştirmek için "Önizleme" → "Aygıt Kodu Akışı Oturum Aç'ı kullan"a gidin. Boş oturum açma pencereleri ile ilgili sorunları olan tüm kullanıcıların bu özelliği denemesini öneririz, çünkü oturum açmanın daha güvenilir bir biçimi olabilir.
* AzCopy ile entegre Depolama Explorer şu anda önizleme için kullanılabilir. Etkinleştirmek için "Önizleme" → "Geliştirilmiş Blob Yükleme ve İndirme için AzCopy'i kullanın" "gidin. AzCopy ile tamamlanan Blob aktarımları daha hızlı ve daha performant olmalıdır.

### <a name="fixes"></a>Düzeltmeler

* Access İlkeleri iletişim kutusu artık son kullanma tarihi olmayan Depolama Erişim İlkeleri'nde bir son kullanma tarihi belirlemez. [#764](https://www.github.com/Microsoft/AzureStorageExplorer/issues/764)
* SAS oluştururken Depolanan Erişim İlkeleri'nin doğru kullanıldığından emin olmak için SAS oluştur iletişim kutusunda bazı değişiklikler yapılmıştır. [#1269](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1269)
* 512 bayt olmayan bir dosyayı blob sayfasına yüklemeye çalışırken, Depolama Gezgini artık daha alakalı bir hata ortaya çıkaracaktır. [#1050](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1050)
* Bir görüntü adı kullanan bir Blob kapsayıcıkopyalama başarısız olur. Blob konteynerinin gerçek adı kullanılır. [#1166](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1166)
* AdlS Gen2 klasöründe, adında unicode karakterleri olan belirli eylemleri gerçekleştirmeye çalışmak başarısız olur. Artık tüm eylemler işe yaramalı. [#980](https://www.github.com/Microsoft/AzureStorageExplorer/issues/980)

### <a name="known-issues"></a>Bilinen Sorunlar

* AzCopy Blob olmayan bir indirme gerçekleştirirken, büyük dosyalar için MD5 doğrulanmıyor. Bunun nedeni Depolama SDK'daki bir hatadır. [#1212](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1212)
* Depolama Gezgini, RBAC kullanırken depolama kaynaklarınıza erişmek için bazı yönetim katmanı izinleri gerektirir. Daha fazla bilgi için [sorun giderme kılavuzuna](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) bakın.
* Bir proxy arkasında ADLS Gen2 Blobs erişmeye çalışırken başarısız olabilir.
* Blob kapsayıcısı gibi SAS URI aracılığıyla bağlı bir kaynaktan ayırmak, diğer eklerin doğru şekilde gösterilmesini engelleyen bir hataya neden olabilir. Bu sorunu çözmek için grup düğümlerini yenilemeniz gerekiyor. Daha fazla bilgi için #537 bakın.
* Mac için VS kullanıyorsanız ve özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu çözmek için ~/'un içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak engelini kaldırmazsa, bu konuda yorum yapın.
* Azurite henüz tam olarak tüm Depolama API'leri uygulanmamıştır. Bu nedenle, geliştirme depolama için Azurite kullanırken beklenmeyen hatalar veya davranış olabilir.
* Nadir durumlarda, ağaç odağı Hızlı Erişim'e takılabilir. Odağı çıkarmak için Tümlerini Yenileyebilirsiniz.
* OneDrive klasörünüzden yükleme, NodeJS'teki bir hata nedeniyle çalışmaz. Hata düzeltildi, ancak henüz Electron'a entegre edilmedi. Bir blob kapsayıcısına yüklerken veya bir kutudan indirirken bu sorunu çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını aşağıdaki özellikleri desteklemez. Azure Yığını kaynaklarıyla çalışırken bu özellikleri kullanmaya çalışmak beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici Silme
   * ADLS Gen2
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Depolama Gezgini'ni Linux'ta çalıştırmak için önce belirli bağımlılıkların yüklenmesi gerekir. Daha fazla bilgi için Depolama Gezgini [sorun giderme kılavuzunu](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#linux-dependencies) kontrol edin.

## <a name="version-170"></a>Sürüm 1.7.0
3/5/2019

### <a name="download-azure-storage-explorer-170"></a>Azure Depolama Gezgini 1.7.0 İndir
- [Windows için Azure Depolama Gezgini 1.7.0](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Mac için Azure Depolama Gezgini 1.7.0](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Linux için Azure Depolama Gezgini 1.7.0](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a>Yeni

* Artık bir ADLS Gen2 kapsayıcısı, dosyası veya klasörüne erişimi yönetirken sahibini ve sahip grubunu değiştirebilirsiniz.
* Windows'da, Depolama Gezgini'ni ürünün içinden güncelleştirmek artık artımlı bir yüklemedir. Bu, daha hızlı bir güncelleştirme deneyimine neden olmalıdır. Temiz bir yükleme yi tercih ederseniz, [yükleyiciyi](https://azure.microsoft.com/features/storage-explorer/) kendiniz indirebilir ve el ile yükleyebilirsiniz. #1089

### <a name="preview-features"></a>Önizleme Özellikleri

* Aygıt kodu akış oturum açma artık önizleme için kullanılabilir. Etkinleştirmek için "Önizleme" → "Aygıt Kodu Akışı Oturum Aç'ı kullan"a gidin. Boş oturum açma pencereleri ile ilgili sorunları olan tüm kullanıcıların bu özelliği denemesini öneririz, çünkü oturum açmanın daha güvenilir bir biçimi olabilir. #938
* AzCopy ile entegre Depolama Explorer şu anda önizleme için kullanılabilir. Etkinleştirmek için "Önizleme" → "Geliştirilmiş Blob Yükleme ve İndirme için AzCopy'i kullanın" "gidin. AzCopy ile tamamlanan Blob aktarımları daha hızlı ve daha performant olmalıdır.

### <a name="fixes"></a>Düzeltmeler

* Artık AzCopy etkinleştirildiğinde olduğu gibi yüklemek istediğiniz blob türünü seçebilirsiniz. #1111
* Daha önce, bir ADLS Gen2 Depolama hesabı için statik web sitelerini etkinleştirmiş ve ardından ad ve anahtarla iliştirmiş olsaydınız, Depolama Gezgini hiyerarşik ad alanının etkin olduğunu algılayamayırdı. Bu sorun düzeltildi. #1081
* Blob düzenleyicisinde, bekletme gün sayısına göre sıralama veya durum kırıldı. Bu sorun düzeltildi. #1106
* 1.5.0'dan sonra, Depolama Gezgini artık sunucu tarafındaki kopyaların yeniden adlandırma veya kopya & yapıştırma sırasında başarıyı bildirmeden önce tamamlanmasını beklemedi. Bu sorun düzeltildi. #976
* Deneysel AzCopy özelliğini kullanırken, "Panoya kopyala komutu" tıkladıktan sonra kopyalanan komut her zaman kendi başına çalıştırılmaz değildi. Şimdi, transferi el ile çalıştırmak için gereken tüm komutlar kopyalanır. #1079
* Daha önce, ADLS Gen2 lekeleri bir proxy arkasında olsaydı erişilebilir değildi. Bunun nedeni, Depolama SDK tarafından kullanılan yeni bir ağ kitaplığındaki bir hataydı. 1.7.0'da, bu sorunu azaltmaya yönelik bir girişim yapıldı, ancak bazı kişiler sorunları görmeye devam edebilir. Gelecekteki bir güncelleştirmede tam düzeltme yayımlanacaktır. #1090
* 1.7.0'da, dosyayı kaydet iletişim kutusu artık bir dosyayı kaydettiğiniz son konumu doğru hatırlıyor. #16
* Özellikler panelinde, bir Depolama hesabının SKU katmanı hesabın türü olarak gösteriliyordu. Bu sorun düzeltildi. #654
* Bazen, blob adını doğru girseniz bile, bir blob kira kırmak mümkün değildi. Bu sorun düzeltildi. #1070

### <a name="known-issues"></a>Bilinen Sorunlar

* Depolama Gezgini, RBAC kullanırken depolama kaynaklarınıza erişmek için bazı yönetim katmanı izinleri gerektirir. Daha fazla bilgi için [sorun giderme kılavuzuna](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) bakın.
* Bir proxy arkasında ADLS Gen2 Blobs erişmeye çalışırken başarısız olabilir.
* Blob kapsayıcısı gibi SAS URI aracılığıyla bağlı bir kaynaktan ayırmak, diğer eklerin doğru şekilde gösterilmesini engelleyen bir hataya neden olabilir. Bu sorunu çözmek için grup düğümlerini yenilemeniz gerekiyor. Daha fazla bilgi için #537 bakın.
* Blob kapsayıcısı gibi SAS URI aracılığıyla bağlı bir kaynaktan ayırmak, diğer eklerin doğru şekilde gösterilmesini engelleyen bir hataya neden olabilir. Bu sorunu çözmek için grup düğümlerini yenilemeniz gerekiyor. Daha fazla bilgi için #537 bakın.
* Mac için VS kullanıyorsanız ve özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu çözmek için ~/'un içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak engelini kaldırmazsa, bu konuda yorum yapın.
* Azurite henüz tam olarak tüm Depolama API'leri uygulanmamıştır. Bu nedenle, geliştirme depolama için Azurite kullanırken beklenmeyen hatalar veya davranış olabilir.
* Nadir durumlarda, ağaç odağı Hızlı Erişim'e takılabilir. Odağı çıkarmak için Tümlerini Yenileyebilirsiniz.
* OneDrive klasörünüzden yükleme, NodeJS'teki bir hata nedeniyle çalışmaz. Hata düzeltildi, ancak henüz Electron'a entegre edilmedi. Bir blob kapsayıcısına yüklerken veya bir kutudan indirirken bu sorunu çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını aşağıdaki özellikleri desteklemez. Azure Yığını kaynaklarıyla çalışırken bu özellikleri kullanmaya çalışmak beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici Silme
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2.0'ı](https://dotnet.microsoft.com/download/dotnet-core/2.0)yüklemeniz gerekir.
* Ubuntu 14.04'teki kullanıcılar için GCC'nin güncel olduğundan emin olmanız gerekir - bu işlem aşağıdaki komutları çalıştırArak ve makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları için GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırarak yapılabilir ve sonra makinenizi yeniden başlatın:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-162"></a>Sürüm 1.6.2
1/9/2019

### <a name="hotfixes"></a>Düzeltmeler
* 1.6.1'de ObjectId tarafından ADLS Gen2 AKLAR'a eklenen ve kullanıcı olmayan varlıklar her zaman grup olarak eklenmiştir. Artık, yalnızca gruplar grup olarak eklenir ve Kurumsal Uygulamalar ve Hizmet İlkeleri gibi varlıklar kullanıcı olarak eklenir. [#1049](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1049)
* Bir ADLS Gen2 Depolama hesabının kapsayıcısı yoksa ve ad ve anahtarla iliştirilmişse, Depolama Gezgini Depolama Hesabının ADLS Gen2 olduğunu algılamaz. Bu sorun düzeltildi. [#1048](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1048)
* 1.6.0'da, kopyalama ve yapıştırma sırasındaki çakışmalar çözüm için istenmez. Bunun yerine, çakışan kopya yalnızca başarısız olur. Şimdi, ilk çatışmada, size bunun nasıl çözülmesini istediğiniz sorulur. [#1014](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1014)
* API sınırlamaları nedeniyle, Access'i Yönet iletişim kutusundaki ObjectId'lerin tüm geçerliliği devre dışı bırakıldı. Doğrulama artık yalnızca kullanıcı UPN'leri için gerçekleşir. [#954](https://www.github.com/Microsoft/AzureStorageExplorer/issues/954)
* ADLS Gen2 Access'i Yönet iletişim kutusunda, bir grubun izinleri değiştirilemedi. Bu sorun düzeltildi. [#958](https://www.github.com/Microsoft/AzureStorageExplorer/issues/958)
* ADLS Gen2 düzenleyicisine sürükle ve bırak yükleme desteği eklendi. [#953](https://www.github.com/Microsoft/AzureStorageExplorer/issues/953)
* ADLS Gen2 dosyaları ve klasörleri için özellikler iletişim kutusundaki URL özelliği bazen bir '/' eksikti. Bu sorun düzeltildi. [#960](https://www.github.com/Microsoft/AzureStorageExplorer/issues/960)
* Bir ADLS Gen2 kapsayıcısı, dosyası veya klasörü için geçerli izinleri almak başarısız olursa, hata artık etkinlik günlüğünde düzgün bir şekilde görüntülenir. [#965](https://www.github.com/Microsoft/AzureStorageExplorer/issues/965)
* Dosyaları açmak için oluşturulan geçici yol, Windows'ta MAX_PATH daha uzun bir yol oluşturma olasılığını azaltmak için kısaltılmıştır. [#93](https://www.github.com/Microsoft/AzureStorageExplorer/issues/93)
* Bağlantı iletişim kutusu artık kullanıcılarda oturum açmış ve kaynak eklenmediğinde doğru bir şekilde görünür. [#944](https://www.github.com/Microsoft/AzureStorageExplorer/issues/944)
* 1.6.0'da, Özellikleri NS olmayan Blobs ve Files için kaydetmek her özelliğin değerini kodlar. Bu, yalnızca ASCII karakterleri içeren değerlerin gereksiz kodlamaile sonuçlandı. Şimdi, değerler yalnızca ASCII olmayan karakterler içeriyorsa kodlanır. [#986](https://www.github.com/Microsoft/AzureStorageExplorer/issues/986)
* SAS kullanılıyorsa ve SAS okuma izinleri yoksa, bir klasörü HNS olmayan blob kapsayıcısına yüklemek başarısız olur. Bu sorun düzeltildi. [#970](https://www.github.com/Microsoft/AzureStorageExplorer/issues/970)
* AzCopy aktarımıniptali işe yaramadı. Bu sorun düzeltildi. [#943](https://www.github.com/Microsoft/AzureStorageExplorer/issues/943)
* AzCopy, bir ADLS Gen2 Blob kapsayıcısından klasör indirmeye çalışırken başarısız olur. Bu sorun düzeltildi. [#990](https://www.github.com/Microsoft/AzureStorageExplorer/issues/990)
* CosmosDB editörü 1.6.0'da kırıldı. Artık düzeltildi. [#950](https://www.github.com/Microsoft/AzureStorageExplorer/issues/950)
        
### <a name="new"></a>Yeni

* Artık [RBAC](https://go.microsoft.com/fwlink/?linkid=2045904&clcid=0x409)üzerinden Blob verilerinize erişmek için Storage Explorer'ı kullanabilirsiniz. Oturum açtıysanız ve Storage Explorer Depolama hesabınız için anahtarları alamıyorsa, verilerinizle etkileşimde bulunduklarında kimlik doğrulaması yapmak için bir OAuth belirteci kullanılır.
* Depolama Gezgini artık ADLS Gen2 Depolama hesaplarını destekler. Depolama Gezgini, bir Depolama hesabı için hiyerarşik ad alanının etkin olduğunu algıladığında, Depolama hesabınızın adının yanında "(ADLS Gen2 Preview)" görürsünüz. Depolama Gezgini, oturum açtığınızda hiyerarşik ad alanının etkin olup olmadığını veya Depolama Hesabınızı ad ve anahtarla iliştirip iliştirmediğiniz algılayabiliyor. ADLS Gen2 Depolama hesapları için Depolama Gezgini'ni şu şekilde kullanabilirsiniz:
  * Kapsayıcılar oluşturma ve silme
  * Kapsayıcı özelliklerini ve izinlerini yönetme (sol taraf)
  * Verileri kapsayıcıların içinde görüntüleme ve gezinme
  * Yeni klasörler oluşturma
  * Dosya ve klasörleri yükleme, indirme, yeniden adlandırma ve silme
  * Dosya ve klasör özelliklerini ve izinleri (sağ taraf) yönetin.
    
    Yumuşak Silme ve Anlık Görüntü le' ngibi diğer tipik Blob özellikleri şu anda kullanılamıyor. İzizin yönetimi yalnızca oturum açtığınızda da kullanılabilir. Ayrıca, bir ADLS Gen2 Depolama hesabında çalışırken, Depolama Gezgini tüm yüklemeler ve indirmeler için AzCopy kullanır ve varsa tüm işlemler için ad ve anahtar kimlik bilgilerini kullanma varsayılan.
* Güçlü kullanıcı geri bildiriminden sonra, break lease bir kez daha aynı anda birden çok blobs kira kırmak için kullanılabilir.

### <a name="known-issues"></a>Bilinen Sorunlar

* Bir ADLS Gen2 Depolama hesabından indirirken, aktası olan dosyalardan biri zaten varsa, AzCopy bazen çöker. Bu yaklaşan bir düzeltme sabit olacaktır.
* Blob kapsayıcısı gibi SAS URI aracılığıyla bağlı bir kaynaktan ayırmak, diğer eklerin doğru şekilde gösterilmesini engelleyen bir hataya neden olabilir. Bu sorunu çözmek için grup düğümlerini yenilemeniz gerekiyor. Daha fazla bilgi için #537 bakın.
* Mac için VS kullanıyorsanız ve özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu çözmek için ~/'un içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak engelini kaldırmazsa, bu konuda yorum yapın.
* Azurite henüz tam olarak tüm Depolama API'leri uygulanmamıştır. Bu nedenle, geliştirme depolama için Azurite kullanırken beklenmeyen hatalar veya davranış olabilir.
* Nadir durumlarda, ağaç odağı Hızlı Erişim'e takılabilir. Odağı çıkarmak için Tümlerini Yenileyebilirsiniz.
* OneDrive klasörünüzden yükleme, NodeJS'teki bir hata nedeniyle çalışmaz. Hata düzeltildi, ancak henüz Electron'a entegre edilmedi. Bir blob kapsayıcısına yüklerken veya bir kutudan indirirken bu sorunu çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını aşağıdaki özellikleri desteklemez. Azure Yığını kaynaklarıyla çalışırken bu özellikleri kullanmaya çalışmak beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici Silme
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2.0'ı](https://dotnet.microsoft.com/download/dotnet-core/2.0)yüklemeniz gerekir.
* Ubuntu 14.04'teki kullanıcılar için GCC'nin güncel olduğundan emin olmanız gerekir - bu işlem aşağıdaki komutları çalıştırArak ve makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları için GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırarak yapılabilir ve sonra makinenizi yeniden başlatın:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-161"></a>Sürüm 1.6.1
12/18/2018

### <a name="hotfixes"></a>Düzeltmeler
* API sınırlamaları nedeniyle, Access'i Yönet iletişim kutusundaki ObjectId'lerin tüm geçerliliği devre dışı bırakıldı. Doğrulama artık yalnızca kullanıcı UPN'leri için gerçekleşir. [#954](https://www.github.com/Microsoft/AzureStorageExplorer/issues/954)
* ADLS Gen2 Access'i Yönet iletişim kutusunda, bir grubun izinleri değiştirilemedi. Bu sorun düzeltildi. [#958](https://www.github.com/Microsoft/AzureStorageExplorer/issues/958)
* ADLS Gen2 düzenleyicisine sürükle ve bırak yükleme desteği eklendi. [#953](https://www.github.com/Microsoft/AzureStorageExplorer/issues/953)
* ADLS Gen2 dosyaları ve klasörleri için özellikler iletişim kutusundaki URL özelliği bazen bir '/' eksikti. Bu sorun düzeltildi. [#960](https://www.github.com/Microsoft/AzureStorageExplorer/issues/960)
* Bir ADLS Gen2 kapsayıcısı, dosyası veya klasörü için geçerli izinleri almak başarısız olursa, hata artık etkinlik günlüğünde düzgün bir şekilde görüntülenir. [#965](https://www.github.com/Microsoft/AzureStorageExplorer/issues/965)
* Dosyaları açmak için oluşturulan geçici yol, Windows'ta MAX_PATH daha uzun bir yol oluşturma olasılığını azaltmak için kısaltılmıştır. [#93](https://www.github.com/Microsoft/AzureStorageExplorer/issues/93)
* Bağlantı iletişim kutusu artık kullanıcılarda oturum açmış ve kaynak eklenmediğinde doğru bir şekilde görünür. [#944](https://www.github.com/Microsoft/AzureStorageExplorer/issues/944)
* 1.6.0'da, Özellikleri NS olmayan Blobs ve Files için kaydetmek her özelliğin değerini kodlar. Bu, yalnızca ASCII karakterleri içeren değerlerin gereksiz kodlamaile sonuçlandı. Şimdi, değerler yalnızca ASCII olmayan karakterler içeriyorsa kodlanır. [#986](https://www.github.com/Microsoft/AzureStorageExplorer/issues/986)
* SAS kullanılıyorsa ve SAS okuma izinleri yoksa, bir klasörü HNS olmayan blob kapsayıcısına yüklemek başarısız olur. Bu sorun düzeltildi. [#970](https://www.github.com/Microsoft/AzureStorageExplorer/issues/970)
* AzCopy aktarımıniptali işe yaramadı. Bu sorun düzeltildi. [#943](https://www.github.com/Microsoft/AzureStorageExplorer/issues/943)
* AzCopy, bir ADLS Gen2 Blob kapsayıcısından klasör indirmeye çalışırken başarısız olur. Bu sorun düzeltildi. [#990](https://www.github.com/Microsoft/AzureStorageExplorer/issues/990)
* CosmosDB editörü 1.6.0'da kırıldı. Artık düzeltildi. [#950](https://www.github.com/Microsoft/AzureStorageExplorer/issues/950)
        
### <a name="new"></a>Yeni

* Artık [RBAC](https://go.microsoft.com/fwlink/?linkid=2045904&clcid=0x409)üzerinden Blob verilerinize erişmek için Storage Explorer'ı kullanabilirsiniz. Oturum açtıysanız ve Storage Explorer Depolama hesabınız için anahtarları alamıyorsa, verilerinizle etkileşimde bulunduklarında kimlik doğrulaması yapmak için bir OAuth belirteci kullanılır.
* Depolama Gezgini artık ADLS Gen2 Depolama hesaplarını destekler. Depolama Gezgini, bir Depolama hesabı için hiyerarşik ad alanının etkin olduğunu algıladığında, Depolama hesabınızın adının yanında "(ADLS Gen2 Preview)" görürsünüz. Depolama Gezgini, oturum açtığınızda hiyerarşik ad alanının etkin olup olmadığını veya Depolama Hesabınızı ad ve anahtarla iliştirip iliştirmediğiniz algılayabiliyor. ADLS Gen2 Depolama hesapları için Depolama Gezgini'ni şu şekilde kullanabilirsiniz:
  * Kapsayıcılar oluşturma ve silme
  * Kapsayıcı özelliklerini ve izinlerini yönetme (sol taraf)
  * Verileri kapsayıcıların içinde görüntüleme ve gezinme
  * Yeni klasörler oluşturma
  * Dosya ve klasörleri yükleme, indirme, yeniden adlandırma ve silme
  * Dosya ve klasör özelliklerini ve izinleri (sağ taraf) yönetin.
    
    Yumuşak Silme ve Anlık Görüntü le' ngibi diğer tipik Blob özellikleri şu anda kullanılamıyor. İzizin yönetimi yalnızca oturum açtığınızda da kullanılabilir. Ayrıca, bir ADLS Gen2 Depolama hesabında çalışırken, Depolama Gezgini tüm yüklemeler ve indirmeler için AzCopy kullanır ve varsa tüm işlemler için ad ve anahtar kimlik bilgilerini kullanma varsayılan.
* Güçlü kullanıcı geri bildiriminden sonra, break lease bir kez daha aynı anda birden çok blobs kira kırmak için kullanılabilir.

### <a name="known-issues"></a>Bilinen Sorunlar

* Bir ADLS Gen2 Depolama hesabından indirirken, aktası olan dosyalardan biri zaten varsa, AzCopy bazen çöker. Bu yaklaşan bir düzeltme sabit olacaktır.
* Blob kapsayıcısı gibi SAS URI aracılığıyla bağlı bir kaynaktan ayırmak, diğer eklerin doğru şekilde gösterilmesini engelleyen bir hataya neden olabilir. Bu sorunu çözmek için grup düğümlerini yenilemeniz gerekiyor. Daha fazla bilgi için #537 bakın.
* Mac için VS kullanıyorsanız ve özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu çözmek için ~/'un içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak engelini kaldırmazsa, bu konuda yorum yapın.
* Azurite henüz tam olarak tüm Depolama API'leri uygulanmamıştır. Bu nedenle, geliştirme depolama için Azurite kullanırken beklenmeyen hatalar veya davranış olabilir.
* Nadir durumlarda, ağaç odağı Hızlı Erişim'e takılabilir. Odağı çıkarmak için Tümlerini Yenileyebilirsiniz.
* OneDrive klasörünüzden yükleme, NodeJS'teki bir hata nedeniyle çalışmaz. Hata düzeltildi, ancak henüz Electron'a entegre edilmedi. Bir blob kapsayıcısına yüklerken veya bir kutudan indirirken bu sorunu çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını aşağıdaki özellikleri desteklemez. Azure Yığını kaynaklarıyla çalışırken bu özellikleri kullanmaya çalışmak beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici Silme
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2.0'ı](https://dotnet.microsoft.com/download/dotnet-core/2.0)yüklemeniz gerekir.
* Ubuntu 14.04'teki kullanıcılar için GCC'nin güncel olduğundan emin olmanız gerekir - bu işlem aşağıdaki komutları çalıştırArak ve makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları için GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırarak yapılabilir ve sonra makinenizi yeniden başlatın:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-160"></a>Sürüm 1.6.0
05.12.2018

### <a name="new"></a>Yeni

* Artık [RBAC](https://go.microsoft.com/fwlink/?linkid=2045904&clcid=0x409)üzerinden Blob verilerinize erişmek için Storage Explorer'ı kullanabilirsiniz. Oturum açtıysanız ve Storage Explorer Depolama hesabınız için anahtarları alamıyorsa, verilerinizle etkileşimde bulunduklarında kimlik doğrulaması yapmak için bir OAuth belirteci kullanılır.
* Depolama Gezgini artık ADLS Gen2 Depolama hesaplarını destekler. Depolama Gezgini, bir Depolama hesabı için hiyerarşik ad alanının etkin olduğunu algıladığında, Depolama hesabınızın adının yanında "(ADLS Gen2 Preview)" görürsünüz. Depolama Gezgini, oturum açtığınızda hiyerarşik ad alanının etkin olup olmadığını veya Depolama Hesabınızı ad ve anahtarla iliştirip iliştirmediğiniz algılayabiliyor. ADLS Gen2 Depolama hesapları için Depolama Gezgini'ni şu şekilde kullanabilirsiniz:
  * Kapsayıcılar oluşturma ve silme
  * Kapsayıcı özelliklerini ve izinlerini yönetme (sol taraf)
  * Verileri kapsayıcıların içinde görüntüleme ve gezinme
  * Yeni klasörler oluşturma
  * Dosya ve klasörleri yükleme, indirme, yeniden adlandırma ve silme
  * Dosya ve klasör özelliklerini ve izinleri (sağ taraf) yönetin.
    
    Yumuşak Silme ve Anlık Görüntü le' ngibi diğer tipik Blob özellikleri şu anda kullanılamıyor. İzizin yönetimi yalnızca oturum açtığınızda da kullanılabilir. Ayrıca, bir ADLS Gen2 Depolama hesabında çalışırken, Depolama Gezgini tüm yüklemeler ve indirmeler için AzCopy kullanır ve varsa tüm işlemler için ad ve anahtar kimlik bilgilerini kullanma varsayılan.
* Güçlü kullanıcı geri bildiriminden sonra, break lease bir kez daha aynı anda birden çok blobs kira kırmak için kullanılabilir.

### <a name="known-issues"></a>Bilinen Sorunlar

* Bir ADLS Gen2 Depolama hesabından indirirken, aktası olan dosyalardan biri zaten varsa, AzCopy bazen çöker. Bu yaklaşan bir düzeltme sabit olacaktır.
* Blob kapsayıcısı gibi SAS URI aracılığıyla bağlı bir kaynaktan ayırmak, diğer eklerin doğru şekilde gösterilmesini engelleyen bir hataya neden olabilir. Bu sorunu çözmek için grup düğümlerini yenilemeniz gerekiyor. Daha fazla bilgi için #537 bakın.
* Mac için VS kullanıyorsanız ve özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu çözmek için ~/'un içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak engelini kaldırmazsa, bu konuda yorum yapın.
* Azurite henüz tam olarak tüm Depolama API'leri uygulanmamıştır. Bu nedenle, geliştirme depolama için Azurite kullanırken beklenmeyen hatalar veya davranış olabilir.
* Nadir durumlarda, ağaç odağı Hızlı Erişim'e takılabilir. Odağı çıkarmak için Tümlerini Yenileyebilirsiniz.
* OneDrive klasörünüzden yükleme, NodeJS'teki bir hata nedeniyle çalışmaz. Hata düzeltildi, ancak henüz Electron'a entegre edilmedi. Bir blob kapsayıcısına yüklerken veya bir kutudan indirirken bu sorunu çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını aşağıdaki özellikleri desteklemez. Azure Yığını kaynaklarıyla çalışırken bu özellikleri kullanmaya çalışmak beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici Silme
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2.0'ı](https://dotnet.microsoft.com/download/dotnet-core/2.0)yüklemeniz gerekir.
* Ubuntu 14.04'teki kullanıcılar için GCC'nin güncel olduğundan emin olmanız gerekir - bu işlem aşağıdaki komutları çalıştırArak ve makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları için GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırarak yapılabilir ve sonra makinenizi yeniden başlatın:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-150"></a>Sürüm 1.5.0
10/29/2018

### <a name="new"></a>Yeni

* Artık Blobs yüklemek ve indirmek için [AzCopy v10 (Önizleme)](https://github.com/Azure/azure-storage-azcopy) kullanabilirsiniz. Bu özelliği etkinleştirmek için "Deneysel" menüsüne gidin ve ardından "Geliştirilmiş Blob Yükleme ve İndirme için AzCopy'i kullanın" seçeneğini tıklayın. Etkinleştirildiğinde, AzCopy aşağıdaki senaryolarda kullanılacaktır:
   * Klasörleri ve dosyaları araç çubuğu veya sürüp bırak yoluyla blob kapsayıcılarına yükleyin.
   * Klasörlerin ve dosyaların araç çubuğu veya bağlam menüsü üzerinden indirilmesi.

* Ayrıca, AzCopy kullanırken:
   * Transferişlemini panonuza gerçekleştirmek için kullanılan AzCopy komutunu kopyalayabilirsiniz. Etkinlik günlüğünde "AzCopy Komutunu Panoya Kopyala"yı tıklamanız yeterlidir.
   * Yükledikten sonra blob düzenleyicisini el ile yenilemeniz gerekir.
   * Blobs'u eklemek için dosya yükleme desteklenmez ve vhd dosyaları sayfa blobs olarak yüklenir ve diğer tüm dosyalar blok blobs olarak yüklenir.
   * Yükleme veya indirme sırasında oluşan hatalar ve çakışmalar, yükleme veya indirme işlemi tamamlanana kadar su yüzüne çıkmaz.

Son olarak, Dosya Paylaşımları ile AzCopy kullanma desteği gelecekte gelecektir.
* Depolama Gezgini şimdi Electron sürüm 2.0.11 kullanıyor.
* Kiralamaları kırma artık aynı anda yalnızca bir blob üzerinde gerçekleştirilebilir. Ayrıca, kira kırdığınız blob adını girmeniz gerekir. Bu değişiklik, özellikle VM'ler için yanlışlıkla bir kirayı kırma olasılığını azaltmak için yapılmıştır. #394
* Oturum açma sorunlarıyla karşılaşırsanız, artık kimlik doğrulamayı sıfırlamayı deneyebilirsiniz. "Yardım" menüsüne gidin ve bu yeteneğe erişmek için "Sıfırla"yı tıklatın. #419

### <a name="fix"></a>Düzeltme

* Güçlü kullanıcı geri bildiriminden sonra varsayılan emülatör düğümü yeniden etkinleştirildi. Connect iletişim kutusu aracılığıyla ek emülatör bağlantıları eklemeye devam edebilirsiniz, ancak emülatörünüz varsayılan bağlantı noktalarını kullanacak şekilde yapılandırılırsa, "Yerel & Ekli/Depolama Hesapları" altında "Emülatör * Varsayılan Bağlantı Noktaları" düğümünü de kullanabilirsiniz. #669
* Depolama Gezgini artık satır aralığı na sahip veya beyaz boşluğu takip eden blob meta veri değerlerini ayarlamanıza izin vermez. #760
* "Oturum Aç" düğmesi, Bağlan iletişim kutusunun aynı sayfalarında her zaman etkinleştirildi. Şimdi uygun olduğunda devre dışı bırakılır. #761
* Hızlı Erişim artık hızlı erişim öğesi eklenmediğinde konsolda bir hata oluşturmaz.

### <a name="known-issues"></a>Bilinen Sorunlar

* Blob kapsayıcısı gibi SAS URI aracılığıyla bağlı bir kaynaktan ayırmak, diğer eklerin doğru şekilde gösterilmesini engelleyen bir hataya neden olabilir. Bu sorunu çözmek için grup düğümlerini yenilemeniz gerekiyor. Daha fazla bilgi için #537 bakın.
* Mac için VS kullanıyorsanız ve özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu çözmek için ~/'un içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak engelini kaldırmazsa, bu konuda yorum yapın.
* Azurite henüz tam olarak tüm Depolama API'leri uygulanmamıştır. Bu nedenle, geliştirme depolama için Azurite kullanırken beklenmeyen hatalar veya davranış olabilir.
* Nadir durumlarda, ağaç odağı Hızlı Erişim'e takılabilir. Odağı çıkarmak için Tümlerini Yenileyebilirsiniz.
* OneDrive klasörünüzden yükleme, NodeJS'teki bir hata nedeniyle çalışmaz. Hata düzeltildi, ancak henüz Electron'a entegre edilmedi. Bir blob kapsayıcısına yüklerken veya bir kutudan indirirken bu sorunu çözmek için deneysel AzCopy özelliğini kullanabilirsiniz.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını aşağıdaki özellikleri desteklemez. Azure Yığını kaynaklarıyla çalışırken bu özellikleri kullanmaya çalışmak beklenmeyen hatalara neden olabilir.
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici Silme
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2.0'ı](https://dotnet.microsoft.com/download/dotnet-core/2.0)yüklemeniz gerekir.
* Ubuntu 14.04'teki kullanıcılar için GCC'nin güncel olduğundan emin olmanız gerekir - bu işlem aşağıdaki komutları çalıştırArak ve makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları için GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırarak yapılabilir ve sonra makinenizi yeniden başlatın:

    ```
    sudo apt-get install libgconf-2-4
    ```


## <a name="version-144"></a>Sürüm 1.4.4
10/15/2018

### <a name="hotfixes"></a>Düzeltmeler
* Azure Kaynak Yönetimi API Sürümü, Azure ABD Kamu kullanıcılarının engelini kaldırmak için geri alınmıştır. [#696](https://github.com/Microsoft/AzureStorageExplorer/issues/696)
* Yükleme spinners şimdi Depolama Explorer tarafından kullanılan GPU miktarını azaltmak için CSS animasyonlar kullanıyor. [#653](https://github.com/Microsoft/AzureStorageExplorer/issues/653)

### <a name="new"></a>Yeni
* SAS bağlantıları ve emülatörleri gibi dış kaynak ekleri önemli ölçüde iyileştirilmiştir. Artık şunları yapabilirsiniz:
   * Eklediğiniz kaynağın görüntü adını özelleştirin. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Farklı bağlantı noktalarını kullanarak birden çok yerel emülatöre takın. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Hızlı Erişim'e ekli kaynaklar ekleyin. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Depolama Gezgini artık Yumuşak Silme'yi destekliyor. Şunları yapabilirsiniz:
   * Depolama hesabınız için Blob Kapsayıcılar düğümüne sağ tıklayarak Yumuşak Silme ilkesini yapılandırın.
   * Gezinti çubuğunun yanındaki açılır da "Etkin ve silinmiş lekeler" seçeneğini seçerek Blob Düzenleyicisi'nde yumuşak silinmiş lekeleri görüntüleyin.
   * Undelete yumuşak silinmiş lekeler.

### <a name="fixes"></a>Düzeltmeler
* Premium Depolama hesapları CORS'leri desteklemediği için "CORS Ayarlarını Yapılandır" eylemi artık Premium Depolama hesaplarında kullanılamaz. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Artık SAS Ekli Hizmetler için paylaşılan erişim imzası özelliği vardır. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* "Varsayılan Erişim Katmanını Ayarla" eylemi artık Hızlı Erişim'e sabitlenmiş Blob ve GPV2 Depolama hesapları için kullanılabilir. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Bazen, Depolama Gezgini Klasik Depolama hesaplarını göstermekte başarısız olur. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Bilinen Sorunlar
* Azure Depolama Emülatörveya Azurite gibi emülatörleri kullanırken, varsayılan bağlantı noktalarındaki bağlantıları dinlemelerini beklemeniz gerekir. Aksi takdirde, Depolama Gezgini bunlara bağlanamaz.
* Mac için VS kullanıyorsanız ve özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu çözmek için ~/'un içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak engelini kaldırmazsa, lütfen [bu konuda](https://github.com/Microsoft/AzureStorageExplorer/issues/97)yorum yapın.
* Azurite henüz tam olarak tüm Depolama API'leri uygulanmamıştır. Bu nedenle, geliştirme depolama için Azurite kullanırken beklenmeyen hatalar veya davranış olabilir.
* Nadir durumlarda, ağaç odağı Hızlı Erişim'e takılabilir. Odağı çıkarmak için Tümlerini Yenileyebilirsiniz.
* OneDrive klasörünüzden yükleme, NodeJS'teki bir hata nedeniyle çalışmaz. Hata düzeltildi, ancak henüz Electron'a entegre edilmedi.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun [nedeni, burada](https://github.com/Azure/azure-storage-node/issues/317)açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını şu anda Dosya Paylaşımları'nı desteklemese de, Dosya Paylaşımları düğümü ekli bir Azure Yığını depolama hesabının altında görünmeye devam eder.
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2.0'ı](https://dotnet.microsoft.com/download/dotnet-core/2.0)yüklemeniz gerekir.
* Ubuntu 14.04'teki kullanıcılar için GCC'nin güncel olduğundan emin olmanız gerekir - bu işlem aşağıdaki komutları çalıştırArak ve makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları için GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırarak yapılabilir ve sonra makinenizi yeniden başlatın:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-143"></a>Sürüm 1.4.3
10/11/2018

### <a name="hotfixes"></a>Düzeltmeler
* Azure Kaynak Yönetimi API Sürümü, Azure ABD Kamu kullanıcılarının engelini kaldırmak için geri alınmıştır. [#696](https://github.com/Microsoft/AzureStorageExplorer/issues/696)
* Yükleme spinners şimdi Depolama Explorer tarafından kullanılan GPU miktarını azaltmak için CSS animasyonlar kullanıyor. [#653](https://github.com/Microsoft/AzureStorageExplorer/issues/653)

### <a name="new"></a>Yeni
* SAS bağlantıları ve emülatörleri gibi dış kaynak ekleri önemli ölçüde iyileştirilmiştir. Artık şunları yapabilirsiniz:
   * Eklediğiniz kaynağın görüntü adını özelleştirin. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Farklı bağlantı noktalarını kullanarak birden çok yerel emülatöre takın. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Hızlı Erişim'e ekli kaynaklar ekleyin. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Depolama Gezgini artık Yumuşak Silme'yi destekliyor. Şunları yapabilirsiniz:
   * Depolama hesabınız için Blob Kapsayıcılar düğümüne sağ tıklayarak Yumuşak Silme ilkesini yapılandırın.
   * Gezinti çubuğunun yanındaki açılır da "Etkin ve silinmiş lekeler" seçeneğini seçerek Blob Düzenleyicisi'nde yumuşak silinmiş lekeleri görüntüleyin.
   * Undelete yumuşak silinmiş lekeler.

### <a name="fixes"></a>Düzeltmeler
* Premium Depolama hesapları CORS'leri desteklemediği için "CORS Ayarlarını Yapılandır" eylemi artık Premium Depolama hesaplarında kullanılamaz. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Artık SAS Ekli Hizmetler için paylaşılan erişim imzası özelliği vardır. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* "Varsayılan Erişim Katmanını Ayarla" eylemi artık Hızlı Erişim'e sabitlenmiş Blob ve GPV2 Depolama hesapları için kullanılabilir. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Bazen, Depolama Gezgini Klasik Depolama hesaplarını göstermekte başarısız olur. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Bilinen Sorunlar
* Azure Depolama Emülatörveya Azurite gibi emülatörleri kullanırken, varsayılan bağlantı noktalarındaki bağlantıları dinlemelerini beklemeniz gerekir. Aksi takdirde, Depolama Gezgini bunlara bağlanamaz.
* Mac için VS kullanıyorsanız ve özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu çözmek için ~/'un içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak engelini kaldırmazsa, lütfen [bu konuda](https://github.com/Microsoft/AzureStorageExplorer/issues/97)yorum yapın.
* Azurite henüz tam olarak tüm Depolama API'leri uygulanmamıştır. Bu nedenle, geliştirme depolama için Azurite kullanırken beklenmeyen hatalar veya davranış olabilir.
* Nadir durumlarda, ağaç odağı Hızlı Erişim'e takılabilir. Odağı çıkarmak için Tümlerini Yenileyebilirsiniz.
* OneDrive klasörünüzden yükleme, NodeJS'teki bir hata nedeniyle çalışmaz. Hata düzeltildi, ancak henüz Electron'a entegre edilmedi.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun [nedeni, burada](https://github.com/Azure/azure-storage-node/issues/317)açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını şu anda Dosya Paylaşımları'nı desteklemese de, Dosya Paylaşımları düğümü ekli bir Azure Yığını depolama hesabının altında görünmeye devam eder.
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2.0'ı](https://dotnet.microsoft.com/download/dotnet-core/2.0)yüklemeniz gerekir.
* Ubuntu 14.04'teki kullanıcılar için GCC'nin güncel olduğundan emin olmanız gerekir - bu işlem aşağıdaki komutları çalıştırArak ve makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları için GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırarak yapılabilir ve sonra makinenizi yeniden başlatın:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-142"></a>Sürüm 1.4.2
09/24/2018

### <a name="hotfixes"></a>Düzeltmeler
* Yeni Azure Depolama Hesabı türleri için destek eklemek için Azure Kaynak Yönetimi API Sürümünü 2018-07-01'e güncelleştirin. [#652](https://github.com/Microsoft/AzureStorageExplorer/issues/652)

### <a name="new"></a>Yeni
* SAS bağlantıları ve emülatörleri gibi dış kaynak ekleri önemli ölçüde iyileştirilmiştir. Artık şunları yapabilirsiniz:
   * Eklediğiniz kaynağın görüntü adını özelleştirin. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Farklı bağlantı noktalarını kullanarak birden çok yerel emülatöre takın. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Hızlı Erişim'e ekli kaynaklar ekleyin. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Depolama Gezgini artık Yumuşak Silme'yi destekliyor. Şunları yapabilirsiniz:
   * Depolama hesabınız için Blob Kapsayıcılar düğümüne sağ tıklayarak Yumuşak Silme ilkesini yapılandırın.
   * Gezinti çubuğunun yanındaki açılır da "Etkin ve silinmiş lekeler" seçeneğini seçerek Blob Düzenleyicisi'nde yumuşak silinmiş lekeleri görüntüleyin.
   * Undelete yumuşak silinmiş lekeler.

### <a name="fixes"></a>Düzeltmeler
* Premium Depolama hesapları CORS'leri desteklemediği için "CORS Ayarlarını Yapılandır" eylemi artık Premium Depolama hesaplarında kullanılamaz. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Artık SAS Ekli Hizmetler için paylaşılan erişim imzası özelliği vardır. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* "Varsayılan Erişim Katmanını Ayarla" eylemi artık Hızlı Erişim'e sabitlenmiş Blob ve GPV2 Depolama hesapları için kullanılabilir. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Bazen, Depolama Gezgini Klasik Depolama hesaplarını göstermekte başarısız olur. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Bilinen Sorunlar
* Azure Depolama Emülatörveya Azurite gibi emülatörleri kullanırken, varsayılan bağlantı noktalarındaki bağlantıları dinlemelerini beklemeniz gerekir. Aksi takdirde, Depolama Gezgini bunlara bağlanamaz.
* Mac için VS kullanıyorsanız ve özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu çözmek için ~/'un içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak engelini kaldırmazsa, lütfen [bu konuda](https://github.com/Microsoft/AzureStorageExplorer/issues/97)yorum yapın.
* Azurite henüz tam olarak tüm Depolama API'leri uygulanmamıştır. Bu nedenle, geliştirme depolama için Azurite kullanırken beklenmeyen hatalar veya davranış olabilir.
* Nadir durumlarda, ağaç odağı Hızlı Erişim'e takılabilir. Odağı çıkarmak için Tümlerini Yenileyebilirsiniz.
* OneDrive klasörünüzden yükleme, NodeJS'teki bir hata nedeniyle çalışmaz. Hata düzeltildi, ancak henüz Electron'a entegre edilmedi.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun [nedeni, burada](https://github.com/Azure/azure-storage-node/issues/317)açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını şu anda Dosya Paylaşımları'nı desteklemese de, Dosya Paylaşımları düğümü ekli bir Azure Yığını depolama hesabının altında görünmeye devam eder.
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2.0'ı](https://dotnet.microsoft.com/download/dotnet-core/2.0)yüklemeniz gerekir.
* Ubuntu 14.04'teki kullanıcılar için GCC'nin güncel olduğundan emin olmanız gerekir - bu işlem aşağıdaki komutları çalıştırArak ve makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları için GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırarak yapılabilir ve sonra makinenizi yeniden başlatın:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-141"></a>Sürüm 1.4.1
08/28/2018

### <a name="hotfixes"></a>Düzeltmeler
* Depolama Gezgini ilk başlatmada hassas verileri şifrelemek için kullanılan anahtarı oluşturamadı. Bu, Hızlı Erişim kullanırken ve kaynak takarken sorunlara neden olur. [#535](https://github.com/Microsoft/AzureStorageExplorer/issues/535)
* Hesabınız ev kiracı için MFA gerektirmedi, ancak diğer bazı kiracılar için yaptım, Depolama Gezgini abonelikleri listelemek mümkün olacaktır. Şimdi, böyle bir hesapla oturum açtıktan sonra, Depolama Gezgini kimlik bilgilerinizi yeniden girmenizi ve MFA gerçekleştirmenizi isteyecektir. [#74](https://github.com/Microsoft/AzureStorageExplorer/issues/74)
* Depolama Gezgini, Azure Almanya'sından ve Azure ABD Hükümeti'nden kaynak ekleyemedi. [#572](https://github.com/Microsoft/AzureStorageExplorer/issues/572)
* Aynı e-posta adresine sahip iki hesapta oturum açtıysanız, Depolama Gezgini bazen kaynaklarınızı ağaç görünümünde gösteremezse. [#580](https://github.com/Microsoft/AzureStorageExplorer/issues/580)
* Daha yavaş Windows makinelerinde, sıçrama ekranının görünmesi bazen önemli miktarda zaman alır. [#586](https://github.com/Microsoft/AzureStorageExplorer/issues/586)
* Ekli hesaplar veya hizmetler olsa bile bağlantı iletişim kutusu görünür. [#588](https://github.com/Microsoft/AzureStorageExplorer/issues/588)

### <a name="new"></a>Yeni
* SAS bağlantıları ve emülatörleri gibi dış kaynak ekleri önemli ölçüde iyileştirilmiştir. Artık şunları yapabilirsiniz:
   * Eklediğiniz kaynağın görüntü adını özelleştirin. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Farklı bağlantı noktalarını kullanarak birden çok yerel emülatöre takın. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Hızlı Erişim'e ekli kaynaklar ekleyin. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Depolama Gezgini artık Yumuşak Silme'yi destekliyor. Şunları yapabilirsiniz:
   * Depolama hesabınız için Blob Kapsayıcılar düğümüne sağ tıklayarak Yumuşak Silme ilkesini yapılandırın.
   * Gezinti çubuğunun yanındaki açılır da "Etkin ve silinmiş lekeler" seçeneğini seçerek Blob Düzenleyicisi'nde yumuşak silinmiş lekeleri görüntüleyin.
   * Undelete yumuşak silinmiş lekeler.

### <a name="fixes"></a>Düzeltmeler
* Premium Depolama hesapları CORS'leri desteklemediği için "CORS Ayarlarını Yapılandır" eylemi artık Premium Depolama hesaplarında kullanılamaz. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Artık SAS Ekli Hizmetler için paylaşılan erişim imzası özelliği vardır. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* "Varsayılan Erişim Katmanını Ayarla" eylemi artık Hızlı Erişim'e sabitlenmiş Blob ve GPV2 Depolama hesapları için kullanılabilir. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Bazen, Depolama Gezgini Klasik Depolama hesaplarını göstermekte başarısız olur. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Bilinen Sorunlar
* Azure Depolama Emülatörveya Azurite gibi emülatörleri kullanırken, varsayılan bağlantı noktalarındaki bağlantıları dinlemelerini beklemeniz gerekir. Aksi takdirde, Depolama Gezgini bunlara bağlanamaz.
* Mac için VS kullanıyorsanız ve özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu çözmek için ~/'un içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak engelini kaldırmazsa, lütfen [bu konuda](https://github.com/Microsoft/AzureStorageExplorer/issues/97)yorum yapın.
* Azurite henüz tam olarak tüm Depolama API'leri uygulanmamıştır. Bu nedenle, geliştirme depolama için Azurite kullanırken beklenmeyen hatalar veya davranış olabilir.
* Nadir durumlarda, ağaç odağı Hızlı Erişim'e takılabilir. Odağı çıkarmak için Tümlerini Yenileyebilirsiniz.
* OneDrive klasörünüzden yükleme, NodeJS'teki bir hata nedeniyle çalışmaz. Hata düzeltildi, ancak henüz Electron'a entegre edilmedi.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun [nedeni, burada](https://github.com/Azure/azure-storage-node/issues/317)açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını şu anda Dosya Paylaşımları'nı desteklemese de, Dosya Paylaşımları düğümü ekli bir Azure Yığını depolama hesabının altında görünmeye devam eder.
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2.0'ı](https://dotnet.microsoft.com/download/dotnet-core/2.0)yüklemeniz gerekir.
* Ubuntu 14.04'teki kullanıcılar için GCC'nin güncel olduğundan emin olmanız gerekir - bu işlem aşağıdaki komutları çalıştırArak ve makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları için GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırarak yapılabilir ve sonra makinenizi yeniden başlatın:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-130"></a>Sürüm 1.3.0
07/09/2018

### <a name="new"></a>Yeni
* Statik Web Siteleri tarafından kullanılan $web kapsayıcılara erişim artık desteklenir. Bu, web siteniz tarafından kullanılan dosya ve klasörleri kolayca yüklemenize ve yönetmenize olanak tanır. [#223](https://github.com/Microsoft/AzureStorageExplorer/issues/223)
* macOS'taki uygulama çubuğu yeniden düzenlendi. Değişiklikler arasında Dosya menüsü, bazı kısayol anahtar değişiklikleri ve uygulama menüsü altında birkaç yeni komut yer alıyor. [#99](https://github.com/Microsoft/AzureStorageExplorer/issues/99)
* Azure ABD Hükümeti'nde oturum açma yetkisi,https://login.microsoftonline.us/
* Erişilebilirlik: Ekran okuyucu etkinolduğunda, klavye gezintisi artık öğeleri sağ tarafta görüntülemek için kullanılan tablolarla çalışır. Satır ve sütunlarda gezinmek için ok tuşlarını, varsayılan eylemleri çağırmak için girin, bir öğenin bağlam menüsünü açmak için bağlam menüsü anahtarını ve çoklu seçim için Kaydırma veya Denetim'i kullanabilirsiniz. [#103](https://github.com/Microsoft/AzureStorageExplorer/issues/103)

### <a name="fixes"></a>Düzeltmeler
*  Bazı makinelerde, alt süreçlerin başlaması uzun zaman alıyordu. Bu gerçekleşeceği zaman, "alt işlem zamanında başlatılamamış" hatası görüntülenir. Bir alt işlemin başlatılması için ayrılan süre şimdi 20 saniyeden 90 saniyeye çıkarıldı. Bu sorundan hala etkileniyorsanız, lütfen bağlı GitHub sorunu hakkında yorum yapın. [#281](https://github.com/Microsoft/AzureStorageExplorer/issues/281)
* Okuma izinleri olmayan bir SAS kullanırken, büyük bir leke yüklemek mümkün değildi. Yükleme mantığı bu senaryoda çalışacak şekilde değiştirildi. [#305](https://github.com/Microsoft/AzureStorageExplorer/issues/305)
* Bir kapsayıcı için genel erişim düzeyini ayarlamak tüm erişim ilkelerini kaldırır ve bunun tersi de olur. Şimdi, ortak erişim düzeyi ve erişim ilkeleri ikisinden birini ayarlarken korunur. [#197](https://github.com/Microsoft/AzureStorageExplorer/issues/197)
* "AccessTierChangeTime" Özellikler iletişim kutusunda kesildi. Bu sorun düzeltildi. [#145](https://github.com/Microsoft/AzureStorageExplorer/issues/145)
* "Microsoft Azure Depolama Gezgini -" öneki Yeni Dizin Oluştur iletişim kutusunda eksikti. Bu sorun düzeltildi. [#299](https://github.com/Microsoft/AzureStorageExplorer/issues/299)
* Erişilebilirlik: Varlık Ekle iletişim kutusunda VoiceOver kullanırken gezinmek zordu. İyileştirmeler yapıldı. [#206](https://github.com/Microsoft/AzureStorageExplorer/issues/206)
* Erişilebilirlik: Eylemler ve Özellikler bölmesi için daraltma/genişletme düğmesinin arka plan rengi, Yüksek Karşıtlık siyah temasındaki benzer Kullanıcı Arabirimi denetimleriyle tutarsızdı. Renk değiştirildi. [#123](https://github.com/Microsoft/AzureStorageExplorer/issues/123)
* Erişilebilirlik: Yüksek Karşıtlık siyah temasında, Özellikler iletişim kutusundaki 'X' düğmesinin odak stili görünmüyordu. Bu sorun düzeltildi. [#243](https://github.com/Microsoft/AzureStorageExplorer/issues/243)
* Erişilebilirlik: Eylemler ve Özellikler sekmeleri bir subpar ekran okuyucu deneyimi ile sonuçlanan birkaç arya değerleri eksikti. Eksik arya değerleri şimdi eklendi. [#316](https://github.com/Microsoft/AzureStorageExplorer/issues/316)
* Erişilebilirlik: Sol tarafta ki daraltılmış ağaç düğümlerine arya tarafından genişletilmiş bir false değeri verilmiyordu. Bu sorun düzeltildi. [#352](https://github.com/Microsoft/AzureStorageExplorer/issues/352)

### <a name="known-issues"></a>Bilinen Sorunlar
* Blob kapsayıcısı gibi SAS URI aracılığıyla bağlı bir kaynaktan ayırmak, diğer eklerin doğru şekilde gösterilmesini engelleyen bir hataya neden olabilir. Bu sorunu çözmek için grup düğümlerini yenilemeniz gerekiyor. Daha fazla bilgi için [bu soruna](https://github.com/Microsoft/AzureStorageExplorer/issues/537) bakın.
* Mac için VS kullanıyorsanız ve özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu çözmek için ~/'un içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak engelini kaldırmazsa, lütfen [bu konuda](https://github.com/Microsoft/AzureStorageExplorer/issues/97)yorum yapın.
* Azurite henüz tam olarak tüm Depolama API'leri uygulanmamıştır. Bu nedenle, geliştirme depolama için Azurite kullanırken beklenmeyen hatalar veya davranış olabilir.
* Nadir durumlarda, ağaç odağı Hızlı Erişim'e takılabilir. Odağı çıkarmak için Tümlerini Yenileyebilirsiniz.
* OneDrive klasörünüzden yükleme, NodeJS'teki bir hata nedeniyle çalışmaz. Hata düzeltildi, ancak henüz Electron'a entegre edilmedi.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun [nedeni, burada](https://github.com/Azure/azure-storage-node/issues/317)açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını aşağıdaki özellikleri desteklemez ve Azure Stack ile çalışırken bunları kullanmaya çalışmak beklenmeyen hatalara neden olabilir:
   * Dosya paylaşımları
   * Erişim katmanları
   * Geçici Silme
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2.0'ı](https://dotnet.microsoft.com/download/dotnet-core/2.0)yüklemeniz gerekir.
* Ubuntu 14.04'teki kullanıcılar için GCC'nin güncel olduğundan emin olmanız gerekir - bu işlem aşağıdaki komutları çalıştırArak ve makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları için GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırarak yapılabilir ve sonra makinenizi yeniden başlatın:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-120"></a>Sürüm 1.2.0
06/12/2018

### <a name="new"></a>Yeni
* Depolama Gezgini abonelikleri yalnızca kiracılarınızın bir alt kümesinden yüklemezse, başarılı bir şekilde yüklenen abonelikler, özellikle başarısız olan kiracılar için bir hata iletisiyle birlikte gösterilir. [#159](https://github.com/Microsoft/AzureStorageExplorer/issues/159)
* Windows'da, bir güncelleştirme kullanılabilir olduğunda artık "Kapat'ta Güncelleştir"i seçebilirsiniz. Bu seçenek seçildiğinde, güncelleştirmenin yükleyicisi, Depolama Gezgini'ni kapattıktan sonra çalışır. [#21](https://github.com/Microsoft/AzureStorageExplorer/issues/21)
* Geri Yükleme Anlık Görüntü, dosya paylaşımı anlık görüntüsünü görüntülerken dosya paylaşım düzenleyicisinin bağlam menüsüne eklendi. [#131](https://github.com/Microsoft/AzureStorageExplorer/issues/131)
* Sırayı Temizle düğmesi artık her zaman etkinleştirilir. [#135](https://github.com/Microsoft/AzureStorageExplorer/issues/135)
* ADFS Azure Yığını'nda oturum açma desteği yeniden etkinleştirildi. Azure Stack sürüm 1804 veya daha büyük olması gerekir. [#150](https://github.com/Microsoft/AzureStorageExplorer/issues/150)

### <a name="fixes"></a>Düzeltmeler
* Adı aynı depolama hesabındaki başka bir dosya paylaşımının öneki olan bir dosya paylaşımının anlık görüntülerini görüntülediyseniz, diğer dosya paylaşımının anlık görüntüleri de listelenir. Bu sorun düzeltilmiştir. [#255](https://github.com/Microsoft/AzureStorageExplorer/issues/255)
* SAS üzerinden eklendiğinde, dosya paylaşımı anlık görüntüsünden dosyayı geri geri toplamak bir hataya neden olur. Bu sorun düzeltilmiştir. [#211](https://github.com/Microsoft/AzureStorageExplorer/issues/211)
* Bir blob için anlık görüntüleri görüntülerken, temel blob ve tek bir anlık görüntü seçildiğinde Anlık Görüntü Tanıtımı eylemi etkinleştirildi. Eylem artık yalnızca tek bir anlık görüntü seçilirse etkinleştirilir. [#230](https://github.com/Microsoft/AzureStorageExplorer/issues/230)
* Tek bir iş (örneğin, bir blob indirme gibi) başlatıldı ysa ve daha sonra başarısız olduysa, aynı türde başka bir işe başlayana kadar otomatik olarak yeniden denemez. Sıraya aldığınız iş sayısı kaç olursa olsun, tüm işler artık otomatik olarak yeniden denemelidir.
* GPV2 ve Blob Depolama hesaplarında yeni oluşturulan blob kapları için açılan editörlerin Access Tier sütunu yoktu. Bu sorun düzeltilmiştir. [#109](https://github.com/Microsoft/AzureStorageExplorer/issues/109)
* Bir Depolama hesabı veya blob kapsayıcıSAS üzerinden eklendiğinde bazen bir Erişim Katmanı sütunu görünmez. Sütun artık her zaman gösterilir, ancak Access Tier kümesi yoksa boş bir değere sahip olur. [#160](https://github.com/Microsoft/AzureStorageExplorer/issues/160)
* Yeni yüklenen bir blok blob'unun Erişim Katmanı ayarlanması devre dışı bırakıldı. Bu sorun düzeltilmiştir. [#171](https://github.com/Microsoft/AzureStorageExplorer/issues/171)
* Klavye kullanılarak "Sekmeyi Açık Tut" düğmesi çağrıldıysa, klavye odağı kaybolur. Şimdi, odak açık tutulan sekme üzerine hareket edecektir. [#163](https://github.com/Microsoft/AzureStorageExplorer/issues/163)
* Sorgu Oluşturucusu'ndaki bir sorgu için VoiceOver geçerli işlecinin kullanılabilir bir açıklamasını vermiyordu. Şimdi daha açıklayıcı. [#207](https://github.com/Microsoft/AzureStorageExplorer/issues/207)
* Çeşitli editörler için pagination bağlantıları açıklayıcı değildi. Daha açıklayıcı olacak şekilde değiştirildiler. [#205](https://github.com/Microsoft/AzureStorageExplorer/issues/205)
* Varlık Ekle iletişim kutusunda, VoiceOver bir giriş öğesinin hangi sütunun parçası olduğunu duyurmuyordu. Geçerli sütunun adı şimdi öğenin açıklamasına dahildir. [#206](https://github.com/Microsoft/AzureStorageExplorer/issues/206)
* Radyo düğmeleri ve onay kutuları odaklanıldığında görünür bir kenarlık yoktu. Bu sorun düzeltilmiştir. [#237](https://github.com/Microsoft/AzureStorageExplorer/issues/237)

### <a name="known-issues"></a>Bilinen Sorunlar
* Azure Depolama Emülatörveya Azurite gibi emülatörleri kullanırken, varsayılan bağlantı noktalarındaki bağlantıları dinlemelerini beklemeniz gerekir. Aksi takdirde, Depolama Gezgini bunlara bağlanamaz.
* Mac için VS kullanıyorsanız ve özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu çözmek için ~/'un içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak engelini kaldırmazsa, lütfen [bu konuda](https://github.com/Microsoft/AzureStorageExplorer/issues/97)yorum yapın.
* Azurite henüz tam olarak tüm Depolama API'leri uygulanmamıştır. Bu nedenle, geliştirme depolama için Azurite kullanırken beklenmeyen hatalar veya davranış olabilir.
* Nadir durumlarda, ağaç odağı Hızlı Erişim'e takılabilir. Odağı çıkarmak için Tümlerini Yenileyebilirsiniz.
* OneDrive klasörünüzden yükleme, NodeJS'teki bir hata nedeniyle çalışmaz. Hata düzeltildi, ancak henüz Electron'a entegre edilmedi.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun [nedeni, burada](https://github.com/Azure/azure-storage-node/issues/317)açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını şu anda Dosya Paylaşımları'nı desteklemese de, Dosya Paylaşımları düğümü ekli bir Azure Yığını depolama hesabının altında görünmeye devam eder.
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2.0'ı](https://dotnet.microsoft.com/download/dotnet-core/2.0)yüklemeniz gerekir.
* Ubuntu 14.04'teki kullanıcılar için GCC'nin güncel olduğundan emin olmanız gerekir - bu işlem aşağıdaki komutları çalıştırArak ve makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları için GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırarak yapılabilir ve sonra makinenizi yeniden başlatın:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-110"></a>Sürüm 1.1.0
05/09/2018

### <a name="new"></a>Yeni
* Depolama Explorer artık Azurite kullanımını destekler. Not: Azurite bağlantısı varsayılan geliştirme uç noktalarına kodlanmış.
* Depolama Gezgini artık Yalnızca Blob ve GPV2 Depolama Hesapları için Erişim Katmanları'nı destekler. Access Katmanları hakkında daha fazla bilgi [için burada.](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers)
* SAS oluştururken artık bir başlangıç saati gerekmez.

### <a name="fixes"></a>Düzeltmeler
* ABD Hükümeti hesapları için aboneliklerin alınması kırıldı. Bu sorun düzeltilmiştir. [#61](https://github.com/Microsoft/AzureStorageExplorer/issues/61)
* Erişim ilkeleri için son kullanma süresi doğru kaydedilmedi. Bu sorun düzeltilmiştir. [#50](https://github.com/Microsoft/AzureStorageExplorer/issues/50)
* Bir kapsayıcıdaki bir öğe için SAS URL'si oluştururken, öğenin adı URL'ye eklenmedi. Bu sorun düzeltilmiştir. [#44](https://github.com/Microsoft/AzureStorageExplorer/issues/44)
* Bir SAS oluştururken, geçmişte olan son kullanma süreleri bazen varsayılan değer olacaktır. Bunun nedeni, Depolama Gezgini'nin varsayılan değerler olarak en son kullanılan başlangıç ve son kullanma süresini kullanmasından kaynaklanmaktadır. Şimdi, SAS iletişim kutusunu her açtığınızda, yeni bir varsayılan değer kümesi oluşturulur. [#35](https://github.com/Microsoft/AzureStorageExplorer/issues/35)
* Depolama Hesapları arasında kopyalama yaparken, 24 saat sas oluşturulur. Kopya 24 saatten fazla sürdüyse, kopya başarısız olur. SAS'ın süresi dolmuş bir SAS nedeniyle bir kopyanın başarısız olma olasılığını azaltmak için SAS'ı son 1 haftaya çıkardık. [#62](https://github.com/Microsoft/AzureStorageExplorer/issues/62)
* Bazı etkinlikler için "İptal" seçeneğini tıklatmak her zaman işe yaramaz. Bu sorun düzeltilmiştir. [#125](https://github.com/Microsoft/AzureStorageExplorer/issues/125)
* Bazı etkinlikleriçin aktarım hızı yanlıştı. Bu sorun düzeltilmiştir. [#124](https://github.com/Microsoft/AzureStorageExplorer/issues/124)
* Görünüm menüsünde "Önceki" yazımı yanlıştı. Şimdi düzgün yazılmış. [#71](https://github.com/Microsoft/AzureStorageExplorer/issues/71)
* Windows yükleyicisinin son sayfasında "Next" düğmesi vardı. "Finish" düğmesi olarak değiştirildi. [#70](https://github.com/Microsoft/AzureStorageExplorer/issues/70)
* HC Black temayı kullanırken sekme odağı iletişim kutularındaki düğmeler için görünmüyordu. Artık görünür. [#64](https://github.com/Microsoft/AzureStorageExplorer/issues/64)
* Etkinlik günlüğündeki eylemler için "Otomatik Çözümle" kılıfı yanlıştı. Artık doğru. [#51](https://github.com/Microsoft/AzureStorageExplorer/issues/51)
* Bir varlığı bir tablodan silerken, onay isteyen iletişim kutusu bir hata simgesi görüntülenmiştir. İletişim şimdi bir uyarı simgesi kullanır. [#148](https://github.com/Microsoft/AzureStorageExplorer/issues/148)

### <a name="known-issues"></a>Bilinen Sorunlar
* Mac için VS kullanıyorsanız ve özel bir AAD yapılandırması oluşturduysanız, oturum açamayabilirsiniz. Sorunu çözmek için ~/'un içeriğini silin. IdentityService/AadConfigurations. Bunu yapmak engelini kaldırmazsa, lütfen [bu konuda](https://github.com/Microsoft/AzureStorageExplorer/issues/97)yorum yapın.
* Azurite henüz tam olarak tüm Depolama API'leri uygulanmamıştır. Bu nedenle, geliştirme depolama için Azurite kullanırken beklenmeyen hatalar veya davranış olabilir.
* Nadir durumlarda, ağaç odağı Hızlı Erişim'e takılabilir. Odağı çıkarmak için Tümlerini Yenileyebilirsiniz.
* OneDrive klasörünüzden yükleme, NodeJS'teki bir hata nedeniyle çalışmaz. Hata düzeltildi, ancak henüz Electron'a entegre edilmedi.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun [nedeni, burada](https://github.com/Azure/azure-storage-node/issues/317)açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını şu anda Dosya Paylaşımları'nı desteklemese de, Dosya Paylaşımları düğümü ekli bir Azure Yığını depolama hesabının altında görünmeye devam eder.
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2.0'ı](https://dotnet.microsoft.com/download/dotnet-core/2.0)yüklemeniz gerekir.
* Ubuntu 14.04'teki kullanıcılar için GCC'nin güncel olduğundan emin olmanız gerekir - bu işlem aşağıdaki komutları çalıştırArak ve makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları için GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırarak yapılabilir ve sonra makinenizi yeniden başlatın:

    ```
    sudo apt-get install libgconf-2-4
    ```


## <a name="version-100"></a>Sürüm 1.0.0
04/16/2018

### <a name="new"></a>Yeni
* Depolama Gezgini'nin Visual Studio 2017 ile aynı hesap mağazasını kullanmasına olanak tanıyan gelişmiş kimlik doğrulaması. Bu özelliği kullanmak için, hesaplarınıza yeniden giriş yapmanız ve filtreuygulanmış aboneliklerinizi yeniden ayarlamanız gerekir.
* AAD tarafından desteklenen Azure Yığını hesapları için, Storage Explorer artık 'Hedef Azure Yığını' etkinleştirildiğinde Azure Yığını aboneliklerini alır. Artık özel bir giriş ortamı oluşturmanız gerekmez.
* Daha hızlı gezinmeyi etkinleştirmek için birkaç kısayol eklendi. Bunlar arasında çeşitli panellerin taşınması ve editörler arasında hareket etmek yer alıyor. Daha fazla ayrıntı için Görünüm menüsüne bakın.
* Depolama Gezgini geri bildirimi artık GitHub'da yaşıyor. Sol alttaki Geri Bildirim düğmesine tıklayarak veya [https://github.com/Microsoft/AzureStorageExplorer/issues](https://github.com/Microsoft/AzureStorageExplorer/issues).. Önerilerde bulunmaktan, sorunları bildirmekte, soru sormaktan veya başka bir geri bildirim formu bırakmaktan çekinmeyin.
* TLS/SSL Sertifikası sorunlarıyla karşınıza çalışıyorsanız ve rahatsız edici sertifikayı bulamıyorsanız, artık bayraklı komut satırından Depolama Gezgini'ni `--ignore-certificate-errors` başlatabilirsiniz. Bu bayrakla başlatıldığında, Depolama Gezgini TLS/SSL sertifika hatalarını yoksayacaktır.
* Artık blob ve dosya öğeleri için bağlam menüsünde bir 'İndir' seçeneği vardır.
* Geliştirilmiş erişilebilirlik ve ekran okuyucu desteği. Erişilebilirlik özelliklerine güveniyorsanız, daha fazla bilgi için [erişilebilirlik belgelerimize](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-accessibility) bakın.
* Depolama Explorer şimdi Electron 1.8.3 kullanır

### <a name="breaking-changes"></a>Hataya Neden Olan Değişiklikler
* Depolama Gezgini yeni bir kimlik doğrulama kitaplığına geçti. Kitaplık geçişin bir parçası olarak, hesaplarınıza yeniden giriş yapmanız ve filtreuygulanmış aboneliklerinizi yeniden ayarlamanız gerekir
* Hassas verileri şifrelemek için kullanılan yöntem değişti. Bu, Hızlı Erişim öğelerinizin bazılarının yeniden eklenmesine ve/veya bazılarınızın eklenmiş olması gereken eklenmiş kaynaklara neden olabilir.

### <a name="fixes"></a>Düzeltmeler
* Yakınlık ların arkasındaki bazı kullanıcılar, 'Çözümlenemeyen' hata iletisi tarafından kesilen grup blob yüklemeleri veya karşıdan yüklemeleri olurdu. Bu sorun düzeltilmiştir.
* Doğrudan bir bağlantı kullanırken oturum açma gerekirse, 'Oturum Aç' komut istemine tıkladığınızda boş bir iletişim kutusu açılır. Bu sorun düzeltilmiştir.
* Linux'ta, Depolama Gezgini bir GPU işlemi çökmesi nedeniyle başlatılamıyorsa, artık çökmeden haberdar edilecek, '--devre dışı-gpu' anahtarını kullanmanız söylenecek ve Depolama Gezgini anahtar etkinken otomatik olarak yeniden başlatılacaktır.
* Geçersiz erişim ilkeleri, Access İlkeleri iletişim kutusunda kimlik bulmak zordu. Geçersiz erişim ilkesi işlemi işleleri, daha fazla görünürlük için kırmızı olarak ana hatlarını çıkarmıştır
* Etkinlik günlüğü bazen bir etkinliğin farklı bölümleri arasında büyük beyaz boşluk alanlarına sahip olabilir. Bu sorun düzeltilmiştir.
* Tablo sorgusu düzenleyicisinde, geçersiz bir durumda bir zaman damgası yan tümcesi bıraktıysanız ve sonra başka bir yan tümceyi değiştirmeye çalıştıysanız, düzenleyici donar. Başka bir yan tümcede değişiklik algılandığında düzenleyici şimdi zaman damgası yantümünü son geçerli durumuna geri yükleyecek.
* Ağaç görünümünde arama sorgunuzu yazarken durakladıysanız, arama başlar ve odak metin kutusundan çalınır. Şimdi, 'Enter' tuşuna basarak veya başlat arama düğmesine tıklayarak açıkça aramaya başlamalısınız.
* 'Paylaşılan Erişim İmzasını Al' komutu bazen Dosya Paylaşımı'ndaki bir dosyaya sağ tıkladığında devre dışı bırakılır. Bu sorun düzeltilmiştir.
* Odaklı kaynak ağacı düğümü arama sırasında filtrelenmişse, kaynak ağacına sekme ve kaynak ağacında gezinmek için ok tuşlarını kullanamazsınız. Şimdi, odaklanmış kaynak ağacı düğümü gizliyse, kaynak ağacındaki ilk düğüm otomatik olarak odaklanır.
* Fazladan ayırıcı bazen düzenleyici araç çubuğunda görünür olabilir. Bu sorun düzeltilmiştir.
* Kırıntı metin kutusu bazen taşma olur. Bu sorun düzeltilmiştir.
* Blob ve Dosya Paylaşımı düzenleyicileri bazen aynı anda birçok dosya yüklerken sürekli olarak yenilerler. Bu sorun düzeltilmiştir.
* 'Klasör İstatistikleri' özelliğinin Dosya Paylaşımı Anlık Görüntü Yönetimi görünümünde bir amacı yoktu. Şimdi devre dışı bırakıldı.
* Linux'ta Dosya menüsü görünmedi. Bu sorun düzeltilmiştir.
* Bir klasörü Dosya Paylaşımı'na yüklerken, varsayılan olarak yalnızca klasörün içeriği yüklendi. Şimdi, varsayılan davranış dosyanın içeriğini Dosya Paylaşımı'nda eşleşen bir klasöre yüklemektir.
* Birkaç iletişim kutusundaki düğmelerin sıralanması tersine çevrilmişti. Bu sorun düzeltilmiştir.
* Çeşitli güvenlik ile ilgili düzeltmeler.

### <a name="known-issues"></a>Bilinen Sorunlar
* Nadir durumlarda, ağaç odağı Hızlı Erişim'e takılabilir. Odağı çıkarmak için Tümlerini Yenileyebilirsiniz.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını şu anda Dosya Paylaşımları'nı desteklemese de, Dosya Paylaşımları düğümü ekli bir Azure Yığını depolama hesabının altında görünmeye devam eder.
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Linux kullanıcıları için [.NET Core 2.0'ı](https://dotnet.microsoft.com/download/dotnet-core/2.0)yüklemeniz gerekir.
* Ubuntu 14.04'teki kullanıcılar için GCC'nin güncel olduğundan emin olmanız gerekir - bu işlem aşağıdaki komutları çalıştırArak ve makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları için GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırarak yapılabilir ve sonra makinenizi yeniden başlatın:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-096"></a>Sürüm 0.9.6
02/28/2018

### <a name="fixes"></a>Düzeltmeler
* Bir sorun beklenen blobs / dosya editörde listelenen engelledi. Bu sorun düzeltilmiştir.
* Bir sorun, öğeleri yanlış görüntülemek için anlık görüntü görünümleri arasında geçiş ekine neden oldu. Bu sorun düzeltilmiştir.

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini ADFS hesaplarını desteklemez.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun [nedeni, burada](https://github.com/Azure/azure-storage-node/issues/317)açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Hesap ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını şu anda Dosya Paylaşımları'nı desteklemese de, Dosya Paylaşımları düğümü ekli bir Azure Yığını depolama hesabının altında görünmeye devam eder.
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Ubuntu 14.04'teki kullanıcılar için GCC'nin güncel olduğundan emin olmanız gerekir - bu işlem aşağıdaki komutları çalıştırArak ve makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları için GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırarak yapılabilir ve sonra makinenizi yeniden başlatın:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-095"></a>Sürüm 0.9.5
02/06/2018

### <a name="new"></a>Yeni

* Dosya Paylaşımları anlık görüntüleri için destek:
    * Dosya Paylaşımlarınız için anlık görüntüler oluşturun ve yönetin.
    * Keşfederken Dosya Paylaşımlarınızın anlık görüntüleri arasında kolayca görünümler arasında geçiş yapabilirsiniz.
    * Dosyalarınızın önceki sürümlerini geri yükleyin.
* Azure Veri Gölü Deposu için önizleme desteği:
    * Birden çok hesap üzerinden ADLS kaynaklarınıza bağlanın.
    * ADL URI'leri kullanarak ADLS kaynaklarına bağlanın ve paylaşın.
    * Temel dosya/klasör işlemlerini özyinelemeyle gerçekleştirin.
    * Tek tek klasörleri Hızlı Erişim'e sabitleyin.
    * Klasör istatistiklerini görüntüleyin.

### <a name="fixes"></a>Düzeltmeler
* Başlangıç performansı iyileştirmeleri.
* Çeşitli hata düzeltmeleri.

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini ADFS hesaplarını desteklemez.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Hesap ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını şu anda Dosya Paylaşımları'nı desteklemese de, Dosya Paylaşımları düğümü ekli bir Azure Yığını depolama hesabının altında görünmeye devam eder.
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Ubuntu 14.04'teki kullanıcılar için GCC'nin güncel olduğundan emin olmanız gerekir - bu işlem aşağıdaki komutları çalıştırArak ve makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları için GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırarak yapılabilir ve sonra makinenizi yeniden başlatın:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-094-and-093"></a>Sürüm 0.9.4 ve 0.9.3
01/21/2018

### <a name="new"></a>Yeni
* Varolan Depolama Gezgini pencereniz şu anda yeniden kullanılacaktır:
    * Depolama Gezgini'nde oluşturulan doğrudan bağlantıları açma.
    * Portaldan Depolama Gezgini'ni açma.
    * Azure Depolama VS Kodu uzantısından Depolama Gezgini'ni açma (yakında).
* Depolama Gezgini içinden yeni bir Depolama Gezgini penceresi açma özelliği eklendi.
    * Windows için Dosya Menüsü'nün altında ve görev çubuğunun bağlam menüsünde bir 'Yeni Pencere' seçeneği vardır.
    * Mac için Uygulama Menüsü'nde 'Yeni Pencere' seçeneği vardır.

### <a name="fixes"></a>Düzeltmeler
* Bir güvenlik sorunu giderildi. Lütfen en kısa zamanda 0.9.4'e yükseltin.
* Eski faaliyetler uygun şekilde temizlenmiş değildi. Bu, uzun süren işlerin performansını etkiledi. Şimdi doğru temizleniyorlar.
* Çok sayıda dosya ve dizin içeren eylemler bazen Depolama Gezgini'nin donmasına neden olur. Dosya Paylaşımları için Azure'a yönelik istekler artık sistem kaynak kullanımını sınırlamak için azaltıldı.

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini ADFS hesaplarını desteklemez.
* "View Explorer" ve "View Account Management" için kısayol tuşları sırasıyla Ctrl/Cmd+Shift+E ve Ctrl/Cmd+Shift+A olmalıdır.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Hesap ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını şu anda Dosya Paylaşımları'nı desteklemese de, Dosya Paylaşımları düğümü ekli bir Azure Yığını depolama hesabının altında görünmeye devam eder.
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer --disable-gpu
    ```

* Ubuntu 14.04'teki kullanıcılar için GCC'nin güncel olduğundan emin olmanız gerekir - bu işlem aşağıdaki komutları çalıştırArak ve makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları için GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırarak yapılabilir ve sonra makinenizi yeniden başlatın:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-092"></a>Sürüm 0.9.2
11/01/2017

### <a name="hotfixes"></a>Düzeltmeler
* Yerel saat dilimine bağlı olarak tablo varlıkları için Edm.DateTime değerlerini düzenlerken beklenmeyen veri değişiklikleri mümkün oldu. Düzenleyici artık Edm.DateTime değerleri üzerinde kesin ve tutarlı denetim sağlayan düz bir metin kutusu kullanır.
* Ad ve anahtarla eklendiğinde bir grup blob yükleme/indirme başlamaz. Bu sorun düzeltilmiştir.
* Daha önce Depolama Gezgini, yalnızca hesabın aboneliklerinden biri veya daha fazlası seçilirse, eski bir hesabın kimliğini yeniden doğrulamanızı ister. Şimdi Depolama Gezgini, hesap tamamen filtrelenmiş olsa bile sizi ister.
* Azure ABD Hükümeti için uç nokta etki alanı yanlıştı. Tamir edildi.
* Hesapları Yönet panelindeki uygula düğmesini tıklatmak bazen zordu. Bu artık olmamalı.

### <a name="new"></a>Yeni
* Azure Cosmos DB için önizleme desteği:
    * [Online Dokümantasyon](./cosmos-db/storage-explorer.md)
    * Veritabanları ve koleksiyonlar oluşturma
    * Verileri işleme
    * Belgeleri sorgulama, oluşturma veya silme
    * Depolanan yordamları, kullanıcı tanımlı işlevleri veya tetikleyicileri güncelleştirme
    * Veritabanlarınıza bağlanmak ve bunları yönetmek için bağlantı dizelerini kullanın
* Birçok küçük blobyükleme/indirme performansını artırdı.
* Blob yükleme grubunda veya blob indirme grubunda hatalar varsa "Tümlerini Yeniden Dene" eylemi eklendi.
* Depolama Gezgini, ağ bağlantınızın kaybolduğunu algılarsa blob yükleme/indirme sırasında yinelemeyi duraklatacaktır. Daha sonra, ağ bağlantısı yeniden kurulduktan sonra yinelemeye devam edebilirsiniz.
* Bağlam menüsü nden "Tümlerini Kapat", "Başkalarını Kapat" ve "Kapat" sekmelerini ekledi.
* Depolama Gezgini artık yerel iletişim günlüklerini ve yerel bağlam menülerini kullanır.
* Depolama Gezgini artık daha erişilebilir. Geliştirmeler şunlardır:
    * Windows'da NVDA ve Mac'te VoiceOver için geliştirilmiş ekran okuyucu desteği
    * Geliştirilmiş yüksek kontrastlı temalı
    * Klavye sekme ve klavye odağı düzeltmeleri

### <a name="fixes"></a>Düzeltmeler
* Geçersiz bir Windows dosya adı olan bir blob açmayı veya indirmeyi denediyseniz, işlem başarısız olur. Depolama Gezgini şimdi bir blob adının geçersiz olup olmadığını algılar ve bunu kodlamak veya blob atlamak isteyip istediğinizi sorun. Depolama Gezgini ayrıca bir dosya adının kodlanmış gibi görünüp görünmediğinizi algılar ve yüklemeden önce şifresini çözmek isteyip istemediğini sorar.
* Blob yükleme sırasında, hedef blob konteyner için editör bazen düzgün yenilemek olmaz. Bu sorun düzeltilmiştir.
* Çeşitli bağlantı dizeleri ve SAS URI'leri için destek geriledi. Bilinen tüm sorunları ele aldık, ancak başka sorunlarla karşılaşırsanız lütfen geri bildirim gönderin.
* Güncelleştirme bildirimi 0.9.0'da bazı kullanıcılar için bozuldu. Bu sorun giderildi ve hatadan etkilenenler için, Depolama Gezgini'nin en son sürümünü el ile [buradan](https://azure.microsoft.com/features/storage-explorer/)indirebilirsiniz.

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini ADFS hesaplarını desteklemez.
* "View Explorer" ve "View Account Management" için kısayol tuşları sırasıyla Ctrl/Cmd+Shift+E ve Ctrl/Cmd+Shift+A olmalıdır.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Hesap ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını şu anda Dosya Paylaşımları'nı desteklemese de, Dosya Paylaşımları düğümü ekli bir Azure Yığını depolama hesabının altında görünmeye devam eder.
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer --disable-gpu
    ```

* Ubuntu 14.04'teki kullanıcılar için GCC'nin güncel olduğundan emin olmanız gerekir - bu işlem aşağıdaki komutları çalıştırArak ve makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları için GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırarak yapılabilir ve sonra makinenizi yeniden başlatın:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-091-and-090"></a>Sürüm 0.9.1 ve 0.9.0
10/20/2017
### <a name="new"></a>Yeni
* Azure Cosmos DB için önizleme desteği:
    * [Online Dokümantasyon](./cosmos-db/storage-explorer.md)
    * Veritabanları ve koleksiyonlar oluşturma
    * Verileri işleme
    * Belgeleri sorgulama, oluşturma veya silme
    * Depolanan yordamları, kullanıcı tanımlı işlevleri veya tetikleyicileri güncelleştirme
    * Veritabanlarınıza bağlanmak ve bunları yönetmek için bağlantı dizelerini kullanın
* Birçok küçük blobyükleme/indirme performansını artırdı.
* Blob yükleme grubunda veya blob indirme grubunda hatalar varsa "Tümlerini Yeniden Dene" eylemi eklendi.
* Depolama Gezgini, ağ bağlantınızın kaybolduğunu algılarsa blob yükleme/indirme sırasında yinelemeyi duraklatacaktır. Daha sonra, ağ bağlantısı yeniden kurulduktan sonra yinelemeye devam edebilirsiniz.
* Bağlam menüsü nden "Tümlerini Kapat", "Başkalarını Kapat" ve "Kapat" sekmelerini ekledi.
* Depolama Gezgini artık yerel iletişim günlüklerini ve yerel bağlam menülerini kullanır.
* Depolama Gezgini artık daha erişilebilir. Geliştirmeler şunlardır:
    * Windows'da NVDA ve Mac'te VoiceOver için geliştirilmiş ekran okuyucu desteği
    * Geliştirilmiş yüksek kontrastlı temalı
    * Klavye sekme ve klavye odağı düzeltmeleri

### <a name="fixes"></a>Düzeltmeler
* Geçersiz bir Windows dosya adı olan bir blob açmayı veya indirmeyi denediyseniz, işlem başarısız olur. Depolama Gezgini şimdi bir blob adının geçersiz olup olmadığını algılar ve bunu kodlamak veya blob atlamak isteyip istediğinizi sorun. Depolama Gezgini ayrıca bir dosya adının kodlanmış gibi görünüp görünmediğinizi algılar ve yüklemeden önce şifresini çözmek isteyip istemediğini sorar.
* Blob yükleme sırasında, hedef blob konteyner için editör bazen düzgün yenilemek olmaz. Bu sorun düzeltilmiştir.
* Çeşitli bağlantı dizeleri ve SAS URI'leri için destek geriledi. Bilinen tüm sorunları ele aldık, ancak başka sorunlarla karşılaşırsanız lütfen geri bildirim gönderin.
* Güncelleştirme bildirimi 0.9.0'da bazı kullanıcılar için bozuldu. Bu sorun giderildi ve hatadan etkilenenler için, Storage Explorer'ın en son sürümünü el ile [buradan](https://azure.microsoft.com/features/storage-explorer/) indirebilirsiniz

### <a name="known-issues"></a>Bilinen Sorunlar
* Depolama Gezgini ADFS hesaplarını desteklemez.
* "View Explorer" ve "View Account Management" için kısayol tuşları sırasıyla Ctrl/Cmd+Shift+E ve Ctrl/Cmd+Shift+A olmalıdır.
* Azure Yığını'nı hedef alırken, belirli dosyaları ek blobolarak yüklemek başarısız olabilir.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bunun nedeni, burada açıklanan iptal filtresi çözümlerini kullanıyor olmamızdır.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Hesap ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını şu anda Dosya Paylaşımları'nı desteklemese de, Dosya Paylaşımları düğümü ekli bir Azure Yığını depolama hesabının altında görünmeye devam eder.
* Depolama Explorer tarafından kullanılan Elektron kabuğu bazı GPU (grafik işleme birimi) donanım ivme ile sorun vardır. Depolama Gezgini boş (boş) bir ana pencere görüntülüyorsa, komut satırından Depolama Gezgini'ni `--disable-gpu` başlatmayı ve anahtarı ekleyerek GPU ivmesini devre dışı bırakmayı deneyebilirsiniz:

    ```
    ./StorageExplorer --disable-gpu
    ```

* Ubuntu 14.04'teki kullanıcılar için GCC'nin güncel olduğundan emin olmanız gerekir - bu işlem aşağıdaki komutları çalıştırArak ve makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları için GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırarak yapılabilir ve sonra makinenizi yeniden başlatın:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-0816"></a>Sürüm 0.8.16
8/21/2017

### <a name="new"></a>Yeni
* Bir blob açtığınızda, Depolama Gezgini bir değişiklik algılanırsa indirilen dosyayı yüklemenizi ister
* Geliştirilmiş Azure Yığını oturum açma deneyimi
* Aynı anda birçok küçük dosyayükleme/indirme performansını artırdı


### <a name="fixes"></a>Düzeltmeler
* Bazı blob türleri için, yükleme çakışması sırasında "değiştirmeyi" seçmek bazen yüklemenin yeniden başlatılmasına neden olur.
* Sürüm 0.8.15'te, yüklemeler bazen %99 oranında durabırdı.
* Dosya paylaşımına dosya yüklerken, henüz var olmayan bir dizine yüklemeyi seçtiyseniz, yüklemeniz başarısız olur.
* Depolama Gezgini, paylaşılan erişim imzaları ve tablo sorguları için yanlış zaman damgaları oluşturuyordu.


### <a name="known-issues"></a>Bilinen Sorunlar
* Ad ve anahtar bağlantı dizesi kullanmak şu anda çalışmıyor. Bir sonraki sürümde düzeltilecektir. O zamana kadar ad ve anahtar ile eklemek kullanabilirsiniz.
* Geçersiz bir Windows dosya adı olan bir dosyayı açmaya çalışırsanız, karşıdan yükleme, dosyanın bulunamadı, hata yla sonuçlanır.
* Bir görevde "İptal Et"i tıklattıktan sonra, bu görevin iptal etmesi biraz zaman alabilir. Bu, Azure Depolama Düğümü kitaplığı sınırlamasıdır.
* Blob yüklemesini tamamladıktan sonra, yüklemeyi başlatan sekme yenilenir. Bu, önceki davranıştan bir değişikliktir ve aynı zamanda içinde olduğunuz kapsayıcının köküne geri götürülmenize neden olur.
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin bu kararı unutması için yeniden başlatmanız gerekir.
* Hesap ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını şu anda Dosya Paylaşımları'nı desteklemese de, Dosya Paylaşımları düğümü ekli bir Azure Yığını depolama hesabının altında görünmeye devam eder.
* Ubuntu 14.04'teki kullanıcılar için GCC'nin güncel olduğundan emin olmanız gerekir - bu işlem aşağıdaki komutları çalıştırArak ve makinenizi yeniden başlatarak yapılabilir:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Ubuntu 17.04 kullanıcıları için GConf yüklemeniz gerekir - bu aşağıdaki komutları çalıştırarak yapılabilir ve sonra makinenizi yeniden başlatın:

    ```
    sudo apt-get install libgconf-2-4
    ```

### <a name="version-0814"></a>Sürüm 0.8.14
06/22/2017

### <a name="new"></a>Yeni

* Birkaç kritik güvenlik güncellemelerinden yararlanmak için Electron sürümünü 1.7.2 olarak güncelledi
* Artık yardım menüsünden çevrimiçi sorun giderme kılavuzuna hızla erişebilirsiniz
* Depolama Gezgini Sorun Giderme [Kılavuzu][2]
* Azure Yığını aboneliğine bağlanma [yönergeleri][3]

### <a name="known-issues"></a>Bilinen Sorunlar

* Sil klasörü onay iletişim kutusundaki düğmeler Linux'taki fare tıklamalarıyla kaydolmaz. çözüm, Enter tuşunu kullanmaktır
* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin kararı unutması için yeniden başlatmanız gerekir
* Aynı anda 3'ten fazla blob veya dosya yükleme grubuna sahip olmak hatalara neden olabilir
* Hesap ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını şu anda Dosya Paylaşımları'nı desteklemese de, Dosya Paylaşımları düğümü ekli bir Azure Yığını depolama hesabının altında görünmeye devam eder.
* Ubuntu 14.04 yükleme ihtiyaçları gcc sürümü güncellendi veya yükseltildi – yükseltme adımları aşağıdadır:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

### <a name="version-0813"></a>Sürüm 0.8.13
12.05.2017

#### <a name="new"></a>Yeni

* Depolama Gezgini Sorun Giderme [Kılavuzu][2]
* Azure Yığını aboneliğine bağlanma [yönergeleri][3]

#### <a name="fixes"></a>Düzeltmeler

* Sabit: Dosya yüklemebellek dışında bir hataya neden yüksek bir şans vardı
* Sabit: Artık PIN/Smartcard ile oturum açabilirsiniz
* Sabit: Open in Portal artık Azure China 21Vianet, Azure Almanya, Azure ABD Hükümeti ve Azure Yığını ile çalışır
* Sabit: Bir klasörü blob kapsayıcısına yüklerken, bazen bir "Geçersiz işlem" hatası oluşur
* Sabit: Anlık görüntüleri yönetirken tümünü devre dışı bırakma
* Sabit: Temel blob meta verileri anlık özellikleri görüntüledikten sonra üzerine yazAbilirsiniz

#### <a name="known-issues"></a>Bilinen Sorunlar

* Yanlış PIN/Smartcard sertifikasını seçerseniz, Depolama Gezgini'nin kararı unutması için yeniden başlatmanız gerekir
* Yakınlaştırılmış veya uzaklaştırırken, yakınlaştırma düzeyi varsayılan düzeye bir an için sıfırlanabilir
* Aynı anda 3'ten fazla blob veya dosya yükleme grubuna sahip olmak hatalara neden olabilir
* Hesap ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını şu anda Dosya Paylaşımları'nı desteklemese de, Dosya Paylaşımları düğümü ekli bir Azure Yığını depolama hesabının altında görünmeye devam eder.
* Ubuntu 14.04 yükleme ihtiyaçları gcc sürümü güncellendi veya yükseltildi – yükseltme adımları aşağıdadır:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-0812-and-0811-and-0810"></a>Sürüm 0.8.12 ve 0.8.11 ve 0.8.10
04/07/2017

#### <a name="new"></a>Yeni

* Depolama Gezgini, güncelleştirme bildiriminden bir güncelleştirme yüklediğinizde artık otomatik olarak kapanacak
* Yerinde hızlı erişim, sık erişilen kaynaklarınızla çalışmak için gelişmiş bir deneyim sağlar
* Blob Container düzenleyicisinde, artık kiralanan bir lekenin hangi sanal makineye ait olduğunu görebilirsiniz
* Artık sol yan paneli daraltabilirsiniz
* Discovery şimdi indirme ile aynı anda çalışır
* Kaynak veya seçiminizin boyutunu görmek için Blob Kapsayıcısı, Dosya Paylaşımı ve Tablo düzenleyicilerinde İstatistikler'i kullanma
* Artık Azure Etkin Dizin (AAD) tabanlı Azure Yığını hesaplarında oturum açabilirsiniz.
* Artık 32MB üzerindeki arşiv dosyalarını Premium depolama hesaplarına yükleyebilirsiniz
* Geliştirilmiş erişilebilirlik desteği
* Artık Güvenilir Base-64 kodlu X.509 TLS/SSL sertifikalarını Düzenleme -&gt; SSL Sertifikaları&gt; - İthalat Sertifikalarına giderek ekleyebilirsiniz

#### <a name="fixes"></a>Düzeltmeler

* Sabit: Bir hesabın kimlik bilgilerini yeniledikten sonra, ağaç görünümü bazen otomatik olarak yenilenmez
* Sabit: Emülatör kuyrukları ve tablolar için Bir SAS oluşturma geçersiz bir URL neden olur
* Sabit: bir proxy etkinleştirilirken premium depolama hesapları artık genişletilebilir
* Sabit: Hesap yönetimi sayfasındaki uygula düğmesi, seçili 1 veya 0 hesabınız varsa çalışmaz
* Sabit: çakışma çözümleri gerektiren blobs yükleme başarısız olabilir - 0.8.11 sabit
* Sabit: geri bildirim gönderme 0.8.11 bozuldu - 0.8.12 sabit

#### <a name="known-issues"></a>Bilinen Sorunlar

* 0.8.10'a yükselttikten sonra tüm kimlik bilgilerinizi yenilemeniz gerekir.
* Yakınlaştırma veya uzaklaştırma düzeyi, bir anda varsayılan düzeye sıfırlanabilir.
* Aynı anda 3'ten fazla blob veya dosya yükleme grubuna sahip olmak hatalara neden olabilir.
* Hesap ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir.
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur.
* Azure Yığını şu anda Dosya Paylaşımları'nı desteklemese de, Dosya Paylaşımları düğümü ekli bir Azure Yığını depolama hesabının altında görünmeye devam eder.
* Ubuntu 14.04 yükleme ihtiyaçları gcc sürümü güncellendi veya yükseltildi – yükseltme adımları aşağıdadır:

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

* Depolama Gezgini 0.8.9 güncelleştirmeler için en son sürümü otomatik olarak karşıdan yükler.
* Hotfix: bir depolama hesabı eklemek için SAS URI oluşturulan bir portal kullanarak bir hataya neden olur.
* Artık blob anlık görüntülerini oluşturabilir, yönetebilir ve tanıtabilirsiniz.
* Artık Azure China 21Vianet, Azure Almanya ve Azure ABD Devlet hesaplarında oturum açabilirsiniz.
* Artık yakınlaştırma düzeyini değiştirebilirsiniz. Yakınlaştırma, Uzaklaştırma ve Yakınlaştırmayı Sıfırlama kullanabilirsiniz menüsündeki seçenekleri kullanın.
* Unicode karakterleri artık blobs ve dosyalar için kullanıcı meta verilerinde desteklenir.
* Erişilebilirlik iyileştirmeleri.
* Bir sonraki sürümün sürüm notları güncelleştirme bildiriminden görüntülenebilir. Yardım menüsünden geçerli sürüm notlarını da görüntüleyebilirsiniz.

#### <a name="fixes"></a>Düzeltmeler

* Sabit: Sürüm numarası artık Windows'daki Denetim Masası'nda doğru bir şekilde görüntüleniyor
* Sabit: arama artık 50.000 düğümile sınırlı değil
* Sabit: hedef dizini zaten yoksa sonsuza kadar döndürülen bir dosya paylaşımına yükleme
* Sabit: uzun yüklemeler ve indirmeler için geliştirilmiş kararlılık

#### <a name="known-issues"></a>Bilinen Sorunlar

* Yakınlaştırma veya uzaklaştırma düzeyi, bir anda varsayılan düzeye sıfırlanabilir.
* Hızlı Erişim yalnızca abonelik tabanlı öğelerle çalışır. Bu sürümde anahtar veya SAS belirteci yle bağlı yerel kaynaklar veya kaynaklar desteklenmez.
* Kaç kaynağınız olduğuna bağlı olarak, hedef kaynağa gitmek hızlı erişim birkaç saniye sürebilir.
* Aynı anda 3'ten fazla blob veya dosya yükleme grubuna sahip olmak hatalara neden olabilir.

12/16/2016
### <a name="version-087"></a>Sürüm 0.8.7

>[!VIDEO https://www.youtube.com/embed/Me4Y4jxoer8?ecver=1]

#### <a name="new"></a>Yeni

* Etkinlikler penceresinde bir güncelleştirme, indirme veya kopyalama oturumunun başında çakışmaları nasıl çözeceğinizi seçebilirsiniz
* Depolama kaynağının tam yolunu görmek için bir sekme üzerinde gezinme
* Bir sekmeyi tıklattığınızda, sol taraftaki gezinti bölmesindeki konumuyla senkronize olur

#### <a name="fixes"></a>Düzeltmeler

* Sabit: Depolama Gezgini artık Mac'te güvenilir bir uygulamadır
* Sabit: Ubuntu 14.04 tekrar desteklenir
* Sabit: Abonelikleri yüklerken bazen hesap arabirimi ara birimi yanıp söner
* Sabit: Bazen tüm depolama kaynakları sol taraftaki gezinti bölmesinde listelenmez
* Sabit: Eylem bölmesi bazen boş eylemler görüntülenir
* Sabit: Son kapalı oturumdaki pencere boyutu artık korunur
* Sabit: Bağlam menüsünü kullanarak aynı kaynak için birden çok sekme açabilirsiniz

#### <a name="known-issues"></a>Bilinen Sorunlar

* Hızlı Erişim yalnızca abonelik tabanlı öğelerle çalışır. Anahtar veya SAS belirteci ile bağlı yerel kaynaklar veya kaynaklar bu sürümde desteklenmez
* Kaç kaynağınız olduğuna bağlı olarak, hedef kaynağa gitmek hızlı erişim birkaç saniye sürebilir
* Aynı anda 3'ten fazla blob veya dosya yükleme grubuna sahip olmak hatalara neden olabilir
* Arama yaklaşık 50.000 düğümde arama yı işler - bundan sonra performans etkilenebilir veya işlenmemiş özel durumlara neden olabilir
* macOS'ta Depolama Gezgini'ni ilk kez kullanarak, anahtarlığa erişmek için kullanıcıdan izin isteyen birden fazla istem gÜ İstemin tekrar görünmeyebilmesi için Her Zaman İzin Ver'i seçmenizi öneririz

11/18/2016
### <a name="version-086"></a>Sürüm 0.8.6

#### <a name="new"></a>Yeni

* Artık en sık kullanılan hizmetleri kolay gezinme için Hızlı Erişim'e sabitleyebilirsiniz
* Artık farklı sekmelerde birden çok düzenleyici açabilirsiniz. Geçici bir sekme açmak için tek tıklama; kalıcı bir sekme açmak için çift tıklatın. Kalıcı bir sekme yapmak için geçici sekmeye de tıklayabilirsiniz
* Özellikle hızlı makinelerdeki büyük dosyalar için, yüklemeler ve yüklemeler için gözle görülür performans ve kararlılık iyileştirmeleri yaptık
* Boş "sanal" klasörler artık blob kaplarda oluşturulabilir
* Kapsamlı aramayı yeni geliştirilmiş substring aramamızla yeniden kullanıma sunduk, bu nedenle artık arama yapmak için iki seçeneğiniz var:
    * Genel arama - arama metin kutusuna bir arama terimi girmeniz
    * Kapsamlı arama - düğümün yanındaki büyüteç simgesine tıklayın, ardından yolun sonuna bir arama terimi ekleyin veya sağ tıklayın ve "Buradan Ara"yı seçin
* Çeşitli temalar ekledik: Açık (varsayılan), Koyu, Yüksek Karşıtlık Siyah ve Yüksek Karşıtlık Beyaz. Edit '&gt; e git - Tema tercihinizi değiştirmek için temalar
* Blob ve dosya özelliklerini değiştirebilirsiniz
* Artık kodlanmış (base64) ve kodlanmamış sıra iletilerini destekliyoruz
* Linux'ta artık 64 bit işletim sistemi gereklidir. Bu sürüm için sadece 64-bit Ubuntu 16.04.1 LTS desteği
* Logomuzu güncelledik!

#### <a name="fixes"></a>Düzeltmeler

* Sabit: Ekran dondurma sorunları
* Sabit: Gelişmiş güvenlik
* Sabit: Bazen yinelenen ekli hesaplar görünebilir
* Sabit: Tanımlanmamış içerik türüne sahip bir leke bir özel durum oluşturabilir
* Sabit: Sorgu Panelini boş bir tabloda açmak mümkün değildi
* Sabit: Arama'daki hataları değişir
* Sabit: "Daha Fazla Yükle" seçeneğini tıklattığınızda yüklenen kaynak sayısı 50'den 100'e yükseltildi
* Sabit: İlk çalıştırmada, bir hesap oturum açmışsa, artık varsayılan olarak bu hesap için tüm abonelikleri seçiyoruz

#### <a name="known-issues"></a>Bilinen Sorunlar

* Depolama Gezgini'nin bu sürümü Ubuntu 14.04'te çalışmıyor
* Aynı kaynak için birden çok sekme açmak için, sürekli olarak aynı kaynağa tıklamayın. Başka bir kaynağa tıklayın ve sonra geri gidin ve sonra başka bir sekmede yeniden açmak için özgün kaynağa tıklayın
* Hızlı Erişim yalnızca abonelik tabanlı öğelerle çalışır. Anahtar veya SAS belirteci ile bağlı yerel kaynaklar veya kaynaklar bu sürümde desteklenmez
* Kaç kaynağınız olduğuna bağlı olarak, hedef kaynağa gitmek hızlı erişim birkaç saniye sürebilir
* Aynı anda 3'ten fazla blob veya dosya yükleme grubuna sahip olmak hatalara neden olabilir
* Arama yaklaşık 50.000 düğümde arama yı işler - bundan sonra performans etkilenebilir veya işlenmemiş özel durumlara neden olabilir

10/03/2016
### <a name="version-085"></a>Sürüm 0.8.5

#### <a name="new"></a>Yeni

* Artık Depolama Hesaplarına ve kaynaklarına eklemek için Portal tarafından oluşturulan SAS anahtarlarını kullanabilir

#### <a name="fixes"></a>Düzeltmeler

* Sabit: arama sırasında yarış durumu bazen düğümlerin genişletilemez hale gelmesine neden oldu
* Sabit: "HTTP'yi kullan" hesap adı ve anahtarla Depolama Hesaplarına bağlanırken çalışmıyor
* Sabit: SAS anahtarları (özellikle Portal tarafından oluşturulanlar) bir "sondaki eğik çizgi" hatası döndürür
* Düzeltildi: tablo alma sorunları
    * Bazen bölüm tuşu ve satır tuşu tersine çevrildi
    * "null" Bölüm Tuşları okunamıyor

#### <a name="known-issues"></a>Bilinen Sorunlar

* Arama, yaklaşık 50.000 düğümde arama işlemlerini gerçeklenden etkileyebilir
* Azure Yığını şu anda Dosyaları desteklemiyor, bu nedenle Dosyaları genişletmeye çalışmak bir hata gösterir

09/12/2016
### <a name="version-084"></a>Sürüm 0.8.4

>[!VIDEO https://www.youtube.com/embed/cr5tOGyGrIQ?ecver=1]

#### <a name="new"></a>Yeni

* Depolama hesaplarına, kapsayıcılara, kuyruklara, tablolara veya kaynaklarınıza kolay erişim için dosya paylaşımlarına doğrudan bağlantılar oluşturun - Windows ve Mac OS desteği
* Arama kutusundan blob kaplarınızı, tablolarınızı, kuyruklarınızı, dosya paylaşımlarınızı veya depolama hesaplarınızı arama
* Artık tablo sorgusu oluşturucusundaki yan tümceleri gruplayabilirsiniz
* SAS'a bağlı hesaplar ve kapsayıcılar içinden blob kapları, dosya paylaşımlarını, tabloları, blob'ları, blob klasörlerini, dosya ve dizinleri yeniden adlandırın ve kopyalayın/yapıştırın
* Blob kapsayıcılarını ve dosya paylaşımlarını yeniden adlandırma ve kopyalama artık özellikleri ve meta verileri koruma

#### <a name="fixes"></a>Düzeltmeler

* Sabit: boolean veya ikili özellikler içeriyorsa tablo varlıklarını kaldıramaz

#### <a name="known-issues"></a>Bilinen Sorunlar

* Arama, yaklaşık 50.000 düğümde arama işlemlerini gerçeklenden etkileyebilir

08/03/2016
### <a name="version-083"></a>Sürüm 0.8.3

>[!VIDEO https://www.youtube.com/embed/HeGW-jkSd9Y?ecver=1]

#### <a name="new"></a>Yeni

* Kapsayıcıları, tabloları, dosya paylaşımlarını yeniden adlandır
* Geliştirilmiş Sorgu oluşturucu deneyimi
* Sorguları kaydetme ve yükleme becerisi
* Kaynaklarınızı paylaşmak ve kaynaklarınıza kolayca erişmek için depolama hesaplarına veya kapsayıcılara, kuyruklara, tablolara veya dosya paylaşımlarına doğrudan bağlantılar (yalnızca Windows - macOS desteği yakında!)
* CORS kurallarını yönetme ve yapılandırma becerisi

#### <a name="fixes"></a>Düzeltmeler

* Sabit: Microsoft Hesapları her 8-12 saatte bir yeniden kimlik doğrulaması gerektirir

#### <a name="known-issues"></a>Bilinen Sorunlar

* Bazen UI dondurulmuş görünebilir - penceremen üst düzeye bu sorunu çözmeye yardımcı olur
* macOS yükleme yüksek izinler gerektirebilir
* Hesap ayarları paneli, abonelikleri filtrelemek için kimlik bilgilerini yeniden girmeniz gerektiğini gösterebilir
* Dosya paylaşımlarını, blob kapsayıcılarını ve tabloları yeniden adlandırma, dosya paylaşımı kotası, genel erişim düzeyi veya erişim ilkeleri gibi kapsayıcıdaki meta verileri veya diğer özellikleri korumaz
* Lekeleri yeniden adlandırma (tek tek veya yeniden adlandırılmış bir blob kapsayıcının içinde) anlık görüntüleri korumaz. Blobs, dosyalar ve varlıklar için diğer tüm özellikler ve meta veriler yeniden adlandırma sırasında korunur
* Kaynakları kopyalama veya yeniden adlandırma SAS'a bağlı hesaplarda çalışmaz

07/07/2016
### <a name="version-082"></a>Sürüm 0.8.2

>[!VIDEO https://www.youtube.com/embed/nYgKbRUNYZA?ecver=1]

#### <a name="new"></a>Yeni

* Depolama Hesapları aboneliklere göre gruplandırılır; geliştirme depolama ve anahtar veya SAS ile bağlı kaynaklar altında gösterilir (Yerel ve Bağlı) düğüm
* "Azure Hesap Ayarları" panelindeki hesaplardan oturum açma
* Oturum açmayı etkinleştirmek ve yönetmek için proxy ayarlarını yapılandırın
* Blob kiralamaları oluşturma ve kesme
* Blob kapsayıcılarını, kuyrukları, tabloları ve dosyaları tek tıklatarak açma

#### <a name="fixes"></a>Düzeltmeler

* Sabit: .NET veya Java kitaplıklarına eklenen sıra iletileri base64'ten düzgün bir şekilde çözülmez
* Sabit: Blob Depolama hesapları için $metrics tablo gösterilmez
* Sabit: tablo düğümü yerel (Geliştirme) depolama için çalışmıyor

#### <a name="known-issues"></a>Bilinen Sorunlar

* macOS yükleme yüksek izinler gerektirebilir

06/15/2016
### <a name="version-080"></a>Sürüm 0.8.0

>[!VIDEO https://www.youtube.com/embed/ycfQhKztSIY?ecver=1]

>[!VIDEO https://www.youtube.com/embed/k4_kOUCZ0WA?ecver=1]

>[!VIDEO https://www.youtube.com/embed/3zEXJcGdl_k?ecver=1]

#### <a name="new"></a>Yeni

* Dosya paylaşımı desteği: görüntüleme, yükleme, indirme, dosya ve dizin kopyalama, SAS URI'leri (oluşturma ve bağlama)
* SAS URI'leri veya hesap anahtarlarıyla Depolama'ya bağlanmak için geliştirilmiş kullanıcı deneyimi
* Tablo sorgu sonuçlarını dışa aktarma
* Tablo sütunu yeniden sıralama ve özelleştirme
* $logs blob kapsayıcılarını ve $metrics tablolarını etkin ölçümlerle Depolama Hesapları için görüntüleme
* Geliştirilmiş dışa aktarma ve alma davranışı, şimdi özellik değeri türünü içerir

#### <a name="fixes"></a>Düzeltmeler

* Sabit: büyük lekeleri yüklemek veya indirmek eksik yüklemelere/indirmelere neden olabilir
* Sabit: sayısal dize değeri olan bir varlığı düzenleme, ekleme veya alma ("1") bu varlığı çift e dönüştürecek
* Sabit: Yerel geliştirme ortamında tablo düğümü genişletilemedi

#### <a name="known-issues"></a>Bilinen Sorunlar

* $metrics tablolar Blob Depolama hesapları için görünmez
* İletiler Base64 kodlaması kullanılarak kodlanırsa, programlı olarak eklenen sıra iletileri doğru görüntülenmeyebilir

05/17/2016
### <a name="version-07201605090"></a>Sürüm 0.7.20160509.0

#### <a name="new"></a>Yeni

* Uygulama çökmeleri için daha iyi hata işleme

#### <a name="fixes"></a>Düzeltmeler

* Bilgi Çubuğu iletilerinin bazen oturum açma kimlik bilgileri gerektiğinde görünmediği hata düzeltildi

#### <a name="known-issues"></a>Bilinen Sorunlar

* Tablolar: "1" veya "1,0" gibi belirsiz sayısal değeri olan bir özelliği olan bir varlığı ekleme, düzenleme veya alma ve `Edm.String`kullanıcı bunu bir Edm.Double olarak istemci API'sı aracılığıyla geri göndermeye çalışır

03/31/2016

### <a name="version-07201603250"></a>Sürüm 0.7.20160325.0

>[!VIDEO https://www.youtube.com/embed/imbgBRHX65A?ecver=1]

>[!VIDEO https://www.youtube.com/embed/ceX-P8XZ-s8?ecver=1]

#### <a name="new"></a>Yeni

* Tablo desteği: varlıklar için görüntüleme, sorgulama, dışa aktarma, alma ve CRUD işlemleri
* Sıra desteği: iletileri görüntüleme, ekleme, sırayı silme
* Depolama Hesapları için SAS URI'leri Oluşturma
* SAS URI'larla Depolama Hesaplarına Bağlanma
* Depolama Gezgini'nde gelecekteki güncelleştirmeler için bildirimleri güncelleştirme
* Güncelleştirilmiş görünüm ve his

#### <a name="fixes"></a>Düzeltmeler

* Performans ve güvenilirlik iyileştirmeleri

### <a name="known-issues-amp-mitigations"></a>Bilinen &amp; Sorunları Azaltma

* Büyük blob dosyalarının karşıdan yüklemesi doğru çalışmıyor - bu sorunu ele alırken AzCopy kullanmanızı öneririz
* Ev klasörü bulunamıyorsa veya yazAmıyorsa hesap kimlik bilgileri alınmayacak veya önbelleğe alınmayacaktır
* "1" veya "1,0" gibi belirsiz sayısal değeri olan bir özelliğe sahip bir varlığı ekliyorsak, düzenliyorsak veya içe aktarıyorsak ve kullanıcı bunu edm.Double `Edm.String`olarak istemci API'sı aracılığıyla geri göndermeye çalışırsa
* Çok satırlı kayıtlara sahip CSV dosyalarını içe aktarırken, veriler doğranabilir veya karıştırılabilir

02/03/2016

### <a name="version-07201601291"></a>Sürüm 0.7.20160129.1

#### <a name="fixes"></a>Düzeltmeler

* Blob'ları yüklerken, indirirken ve kopyalarken genel performansı artırdı

01/14/2016

### <a name="version-07201601050"></a>Sürüm 0.7.20160105.0

#### <a name="new"></a>Yeni

* Linux desteği (OSX'e eşlik özellikleri)
* Paylaşılan Erişim İmzaları (SAS) tuşu ile blob kapsayıcıları ekleme
* Azure China 21Vianet için Depolama Hesapları Ekleme
* Özel uç noktaları yla Depolama Hesapları Ekleme
* Metin ve resim lekelerini açın ve görüntüleyin
* Blob özelliklerini ve meta verileri görüntüleme ve düzeltme

#### <a name="fixes"></a>Düzeltmeler

* Sabit: yükleme veya blobs çok sayıda indirme (500 +) bazen uygulamanın beyaz bir ekrana sahip neden olabilir
* Sabit: blob kapsayıcı ortak erişim düzeyini ayarlarken, kapsayıcıya yeniden odaklanana kadar yeni değer güncelleştirilmedi. Ayrıca, iletişim kutusu her zaman varsayılan olarak "Genel erişim yok" olarak varsayılandır ve geçerli değeri değil.
* Daha iyi genel klavye/erişilebilirlik ve Kullanıcı Altı
* Ekmek kırıntıları tarih metin beyaz boşluk ile uzun olduğunda sarar
* SAS iletişim kutusu giriş doğrulaması destekler
* Kullanıcı kimlik bilgilerinin süresi dolmuş olsa bile yerel depolama alanı kullanılabilir olmaya devam ediyor
* Açılan bir blob kapsayıcısı silindiğinde, sağ taraftaki blob gezgini kapatılır

#### <a name="known-issues"></a>Bilinen Sorunlar

* Linux yükleme gcc sürümü güncellenmiş veya yükseltilmiş ihtiyacı var – yükseltme adımları aşağıdadır:
    * `sudo add-apt-repository ppa:ubuntu-toolchain-r/test`
    * `sudo apt-get update`
    * `sudo apt-get upgrade`
    * `sudo apt-get dist-upgrade`

11/18/2015
### <a name="version-07201511160"></a>Sürüm 0.7.20151116.0

#### <a name="new"></a>Yeni

* macOS ve Windows sürümleri
* Depolama Hesaplarınızı görüntülemek için oturum açın – Org Hesabınızı, Microsoft Hesabınızı, 2FA'nızı vb. kullanın.
* Yerel geliştirme depolama alanı (depolama emülatörü kullanın, yalnızca Windows'da)
* Azure Kaynak Yöneticisi ve Klasik kaynak desteği
* Lekeler, kuyruklar veya tablolar oluşturma ve silme
* Belirli lekeleri, kuyrukları veya tabloları arama
* Blob kaplarının içeriğini keşfedin
* Dizinleri görüntüleme ve gezinme
* Blob'ları ve klasörleri yükleme, indirme ve silme
* Blob özelliklerini ve meta verileri görüntüleme ve düzeltme
* SAS tuşlarını oluşturma
* Depolanan Erişim İlkeleri'ni (SAP) yönetme ve oluşturma
* Önek tarafından blobs arama
* Yükleme veya indirmek için dosyaları sürükleyin 'n bırak

#### <a name="known-issues"></a>Bilinen Sorunlar

* Blob kapsayıcı ortak erişim düzeyini ayarlarken, kapsayıcıya yeniden odaklanmayı ayarlayıncaya kadar yeni değer güncelleştirilmeyecektir
* Ortak erişim düzeyini ayarlamak için iletişim kutusunu açtığınızda, her zaman "Genel erişim yok"u varsayılan olarak gösterir, geçerli değeri değil
* İndirilen lekelerin adını yeniden adlandıramıyorum
* Bir hata oluştuğunda ve hata görüntülenmediğinde, etkinlik günlüğü girişleri bazen devam eden bir durumda "sıkışmış" olur
* Bazen çok sayıda blob yüklemeye veya indirmeye çalışırken tamamen beyaza döner veya çöker
* Bazen kopyalama işlemini iptal etmek işe yaramaz
* Kapsayıcı oluştururken (blob/queue/tablo), geçersiz bir ad girin ve farklı bir kapsayıcı türü altında başka bir tane oluşturmaya devam ederseniz, yeni türe odaklanamazsınız
* Yeni klasör oluşturamaz veya klasörü yeniden adlandıramaz




[2]: https://support.microsoft.com/en-us/help/4021389/storage-explorer-troubleshooting-guide
[3]: vs-azure-tools-storage-manage-with-storage-explorer.md
