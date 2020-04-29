---
title: Azure Dosyaları'na giriş | Microsoft Docs
description: Endüstri standardı SMB protokolünü kullanarak bulutta ağ dosya paylaşımları oluşturmanıza ve bunları kullanmanıza olanak tanıyan Azure Dosyaları hizmetine genel bir bakış.
author: roygara
ms.service: storage
ms.topic: overview
ms.date: 03/10/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: aff6f99c119ba2854fd7923d2a15efb2e1a6b601
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80666807"
---
# <a name="what-is-azure-files"></a>Azure Dosyaları nedir?
Azure Dosyaları bulutta tamamen yönetilen dosya paylaşımları sunar. Bu dosyalara sektör standardı olan [Sunucu İleti Bloğu (SMB) protokolü](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) aracılığıyla erişilebilir. Azure dosya paylaşımları Windows, Linux ve macOS bulut ve şirket içi dağıtımları tarafından aynı anda bağlanabilir. Azure dosya paylaşımları ayrıca verilerin kullanıldığı yerin yakınlarında hızlı erişim sağlamak için Azure Dosya Eşitleme ile Windows Server’larda önbelleğe alınabilir.

## <a name="videos"></a>Videolar
| Azure Dosya Eşitleme tanıtımı | Eşitlenmiş Azure dosyaları (Ignite 2019)  |
|-|-|
| [![Giriş Azure Dosya Eşitleme ekran görüntüsü-oynatmak için tıklayın!](./media/storage-files-introduction/azure-file-sync-video-snapshot.png)](https://www.youtube.com/watch?v=Zm2w8-TRn-o) | [![Eşitleme sunumuna sahip Azure dosyalarının ekran kaydı-oynatmak için tıklayın!](./media/storage-files-introduction/ignite-2018-video.png)](https://www.youtube.com/embed/6E2p28XwovU) |

Azure dosyalarının genel kullanım durumlarında bazı videolar aşağıda verilmiştir:
* [Dosya sunucunuzu sunucusuz bir Azure dosya paylaşımıyla değiştirin](https://sec.ch9.ms/ch9/3358/0addac01-3606-4e30-ad7b-f195f3ab3358/ITOpsTalkAzureFiles_high.mp4)
* [AD kimlik doğrulamasını kullanarak Windows sanal masaüstündeki Azure dosyalarında FSLogix profil kapsayıcıları ile çalışmaya başlama](https://www.youtube.com/embed/9S5A1IJqfOQ)

## <a name="why-azure-files-is-useful"></a>Azure Dosyaları neden faydalıdır?
Azure dosya paylaşımları şunları yapmak için kullanılabilir:

* **Şirket içi dosya sunucularını değiştirme veya destekleme**:  
    Azure Dosyaları geleneksel şirket içi dosya sunucularını veya NAS cihazlarını tamamen değiştirmek veya desteklemek için kullanılabilir. Windows, macOS ve Linux gibi yaygın işletim sistemleri, dünyanın neresinde olursa olsun Azure dosya paylaşımlarına doğrudan bağlanabilir. Azure dosya paylaşımları ayrıca verilerin kullanıldığı yerde performanslı ve dağıtılmış bir şekilde önbelleğe alınması için Azure Dosya Eşitleme ile şirket içi veya bulut üzerindeki Windows Server’larda çoğaltılabilir. Azure [dosyaları ad kimlik doğrulamasının](storage-files-active-directory-overview.md)son sürümü ile Azure dosya paylaşımları, erişim denetimi için şirket IÇINDE barındırılan ad ile çalışmaya devam edebilir. 

* **"Kaldırma ve kaydırma" uygulamaları**:  
    Azure Dosyaları dosya uygulamasını veya kullanıcı verilerini saklamak için bir dosya paylaşımı gerektiren uygulamaların buluta taşınmasını kolaylaştırır. Azure Dosyaları, uygulamanın ve verilerinin Azure'a taşındığı "klasik" taşıma senaryosunun yanı sıra uygulama verilerinin Azure Dosyaları'na taşındığı ve uygulamanın şirket içi ortamda çalışmaya devam ettiği "hibrit" taşıma senaryosunu destekler. 

* **Bulut geliştirmeyi basitleştirme**:  
    Azure Dosyaları ayrıca yeni bulut geliştirme projelerini kolaylaştırma amacıyla farklı şekillerde kullanılabilir. Örneğin:
    * **Paylaşılan uygulama ayarları**:  
        Dağıtılmış uygulamalar için yaygın bir düzen, yapılandırma dosyalarının birçok uygulama örneğinin erişebildiği merkezi bir konumda tutulmasıdır. Uygulama örnekleri yapılandırmalarını Dosya REST API'si üzerinden yükleyebilir ve kullanıcılar bu verilere SMB paylaşımını yerel ortama bağlayarak erişebilir.

    * **Tanılama paylaşma**:  
        Azure dosya paylaşımı, bulut uygulamalarının günlük, ölçüm ve kilitlenme bilgi dökümünü yazmaları için kullanışlı bir yerdir. Günlükler uygulama örnekleri tarafından Dosya REST API'si aracılığıyla yazılabilir ve geliştiriciler dosya paylaşımını yerel makinelerine bağlayarak bu verilere erişebilir. Bu durum geliştiricilerin bildikleri ve sevdikleri araçları bırakmak zorunda kalmadan bulut geliştirmeye geçmelerini sağladığından önemli bir esneklik sunar.

    * **Geliştirme/Test/Hata Ayıklama**:  
        Geliştiriciler veya yöneticiler bulutta sanal makinelerle çalışırken, sıklıkla bir dizi araca veya yardımcı programa ihtiyaçları olur. Bu tür yardımcı programları ve araçları tüm sanal makineleri kopyalamak uzun zaman alabilir. Bir Azure dosya paylaşımını sanal makinelere yerel olarak bağlayan geliştirici ve yöneticiler kopyalamaya gerek duymadan araçlarına ve yardımcı programlarına erişebilirler.

## <a name="key-benefits"></a>Önemli avantajlar
* **Paylaşılan erişim**. Azure dosya paylaşımları endüstri standardı SMB protokolünü destekler. Bu, şirket içi dosya paylaşımlarınızı uygulama uyumluluğu konusunda kaygılanmadan rahatça Azure dosya paylaşımlarıyla değiştirebileceğiniz anlamına gelir. Dosya sistemini birden çok makine, uygulama/örnek arasında paylaşabilmek, paylaşılabilirlik gerektiren uygulamalarda Azure Dosyaları'nın önemli bir avantaj olmasını sağlar. 
* **Tam olarak yönetilir**. Azure dosya paylaşımları donanım veya işletim sistemi yönetmeye gerek kalmadan oluşturulabilir. Bu, sunucu işletim sistemine kritik güvenlik yükseltmeleri için yama eklemekle veya arızalı sabit diskleri değiştirmekle uğraşmanız gerekmediği anlamına gelir.
* **Betik oluşturma ve araç**. Azure uygulamalarının yönetimi kapsamında Azure dosya paylaşımlarını oluşturmak, bağlamak ve yönetmek için PowerShell cmdlet'leri ve Azure CLI kullanılabilir. Azure portalını ve Azure Depolama Gezgini'ni kullanarak Azure dosya paylaşımlarını oluşturabilir ve yönetebilirsiniz. 
* **Dayanıklılık**. Azure Dosyaları, baştan sonra her zaman kullanılabilir olacak şekilde hazırlanmıştır. Şirket içi dosya paylaşımlarını Azure Dosyaları ile değiştirmek, artık kalkıp bölgesel elektrik kesintileriyle veya ağ sorunlarıyla uğraşmak zorunda kalmayacağınız anlamına gelir. 
* **Tanıdık programlama**. Azure’da çalışan uygulamalar paylaşımdaki verilere [dosya sistemi G/Ç API’leri](https://msdn.microsoft.com/library/system.io.file.aspx) yoluyla erişebilir. Böylece geliştiriciler mevcut uygulamalarını taşımak üzere kullandıkları kodlar ve yeteneklerden yararlanabilir. Sistem G/Ç API’lerine ek olarak, [Azure Depolama İstemcisi Kitaplıkları](https://msdn.microsoft.com/library/azure/dn261237.aspx) veya [Azure Depolama REST API’si](/rest/api/storageservices/file-service-rest-api) de kullanılabilir.

## <a name="next-steps"></a>Sonraki Adımlar
* [Azure dosya paylaşma oluşturma](storage-how-to-create-file-share.md)
* [Windows 'a bağlanma ve bağlama](storage-how-to-use-files-windows.md)
* [Linux 'a bağlanma ve bağlama](storage-how-to-use-files-linux.md)
* [MacOS 'a bağlanma ve bağlama](storage-how-to-use-files-mac.md)
