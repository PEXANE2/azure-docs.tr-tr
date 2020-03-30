---
title: Azure Depolama ile son derece güvenli, dayanıklı ve ölçeklenebilir uygulamalar oluşturmak için bulut depolama
description: Büyük yapılandırılmış ve yapılandırılan büyük mobil uygulama verilerini bulutta depolama hizmetleri hakkında bilgi edinin.
author: codemillmatt
ms.assetid: 12bbb070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: 682f06936480d9018d759b4df3bed80be1206faf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240983"
---
# <a name="cloud-storage-for-highly-secure-durable-scalable-apps-with-azure-storage"></a>Azure Depolama ile son derece güvenli, dayanıklı ve ölçeklenebilir uygulamalar için bulut depolama
[Azure Depolama,](https://azure.microsoft.com/services/storage/) Microsoft'un veri nesneleri için büyük ölçüde ölçeklenebilir bir nesne deposu, bulut için bir dosya sistemi hizmeti, güvenilir ileti için bir mesajlaşma mağazası ve Bir NoSQL mağazası sunan modern uygulamalar için bulut depolama çözümüdür. Azure Depolama şu özelliklere sahiptir:
- **Dayanıklı ve yüksek kullanılabilir:** Artıklık, geçici donanım hataları durumunda verilerinizin güvende olmasını sağlar. Ayrıca, yerel felaketler ya da doğal afetlere karşı ek koruma için verileri meri merkezleri ya da coğrafi bölgeler arasında çoğaltmayı seçebilirsiniz. Bu şekilde çoğaltılan veriler beklenmeyen bir kesinti durumunda yüksek oranda kullanılabilir olmaya devam eder.
- **Güvenli:** Azure Depolama'ya yazılan tüm veriler hizmet tarafından şifrelenir. Azure Depolama, verilerinize erişmesi gereken kişiler üzerinde ayrıntılı denetime sahip olmanızı sağlar.
- **Ölçeklenebilir:** Hizmetler, günümüzün uygulamalarının veri depolama ve performans gereksinimlerini karşılamak üzere büyük ölçüde ölçeklenebilir olacak şekilde tasarlanmıştır.
- **Yönetilen:** Azure, donanım bakımlarını, güncelleştirmeleri ve kritik sorunları sizin için işler.
- **Erişilebilir:** Verilere dünyanın herhangi bir yerinden HTTP veya HTTPS üzerinden erişilebilir. Microsoft, .NET, Java, Node.js, Python, PHP, Ruby ve Go gibi çeşitli dillerde istemci kitaplıkları ve olgun bir REST API sağlar. Komut dosyası komut dosyası, Azure PowerShell veya Azure CLI'de desteklenir. Azure portalı ve Azure Depolama Gezgini, verilerinizle çalışmak için kolay görsel çözümler sunar.

Mobil uygulamalarınızda bulut depolamayı etkinleştirmek için aşağıdaki hizmetleri kullanın.

## <a name="azure-blob-storage"></a>Azure Blob depolama
[Azure Blob depolama,](https://azure.microsoft.com/services/storage/blobs/) bulut için bir nesne depolama çözümü sunar. Blob depolama, metin veya ikili gibi belirli bir veri modeline veya tanımına uymayan büyük miktarlarda yapılandırılmamış veri depolamak için optimize edilmiştir. İstemci kitaplıklarının kullandığı çeşitli dilleri destekler. Blob depolama alanı:
- Görüntüleri veya belgeleri doğrudan bir tarayıcıya servis edin.
- Dağıtılmış erişim için dosyaları depolayın.
- Video ve ses akışı.
- Dosyaları günlüğe kaydedin.
- Yedekleme ve geri yükleme, olağanüstü durum kurtarma ve arşivleme için veri depolayın.
- Verileri şirket içi veya Azure barındırılan bir hizmet tarafından analiz için depolayın.

**Başvurular**
- [Azure portalında](https://portal.azure.com)
- [Azure Blob depolama belgeleri](/azure/storage/blobs/storage-blobs-introduction)
- [Hızlı başlangıçlar](/azure/storage/blobs/storage-quickstart-blobs-portal)
- [Örnekler](/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

## <a name="azure-table-storage"></a>Azure Tablo depolama
[Azure Tablo depolama,](https://azure.microsoft.com/services/storage/tables/) bulutta yapılandırılmış NoSQL verilerini depolayan ve şemasız bir tasarıma sahip anahtar veya öznitelik deposu sağlayan bir hizmettir. Azure Tablo depolama, büyük miktarlarda yapısal veriyi depolar. Hizmet, Azure bulutu içinden ve dışından kimlik doğrulamalı aramaları kabul eden bir NoSQL veri deposudur. Azure tabloları yapılandırılmış, ilişkisel olmayan verileri depolamak için idealdir. Tablo depolama genellikle kullanılır:
- Web ölçeğinde uygulamalara hizmet verebilecek yapılandırılmış verileri terabaytlardepon.
- Karmaşık birleştirmeler, yabancı anahtarlar veya depolanan yordamlar gerektirmeyen ve hızlı erişim için normalden arındırılabilen veri kümelerini depolayın.
- Kümelenmiş bir dizin kullanarak verileri hızla sorgula.
- Windows Communication Foundation (WCF) Veri Hizmetleri .NET kitaplıkları ile OData protokolünü ve LINQ sorgularını kullanarak verilere erişin.

Büyük yapılandırılmış, ilişkisel olmayan veri kümelerini depolamak ve sorgulamak için Tablo depolama alanını kullanabilirsiniz. Talep arttıkça tablolarınız ölçeklendirilir.

**Başvurular**
- [Azure portalında](https://portal.azure.com)
- [Azure Tablo depolama belgeleri](/azure/storage/tables/table-storage-overview)
- [Örnekler](/azure/cosmos-db/tutorial-develop-table-dotnet?toc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fstorage%2Ftables%2FTOC.json&bc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
- [Hızlı başlangıçlar](/azure/storage/tables/table-storage-quickstart-portal)

## <a name="azure-files"></a>Azure Dosyaları
[Azure Dosyaları](https://azure.microsoft.com/services/storage/files/)ile, standart Sunucu İleti Bloğu (SMB) protokolünü kullanarak erişilebilen yüksek kullanılabilir ağ dosyası paylaşımları ayarlayabilirsiniz. Birden çok VM hem okuma hem de yazma erişimi ile aynı dosyaları paylaşabilirsiniz. Ayrıca, REST arabirimini veya depolama istemcikitaplarını kullanarak dosyaları okuyabilirsiniz. Dosyayı işaret eden ve Paylaşılan Erişim İmzası (SAS) belirteci içeren bir URL kullanarak dünyanın her yerinden dosyalara erişebilirsiniz. SAS belirteçleri oluşturabilirsiniz. Belirli bir süre için özel bir varlığa belirli erişim sağlarlar.

Azure dosya paylaşımları şunları yapmak için kullanılabilir:
- **Şirket içi dosya sunucularını değiştirin veya ekedin:** Windows, macOS ve Linux gibi popüler işletim sistemleri, dünyanın neresinde olurlarsa olsunlar Azure dosya paylaşımlarını doğrudan monte edebilir. Azure dosya paylaşımları ayrıca verilerin kullanıldığı yerde performanslı ve dağıtılmış bir şekilde önbelleğe alınması için Azure Dosya Eşitleme ile şirket içi veya bulut üzerindeki Windows Server’larda çoğaltılabilir.
- **Kaldırma ve kaydırma uygulamaları:** Dosya uygulamasını veya kullanıcı verilerini depolamak için dosya paylaşımı bekleyen uygulamaları buluta geçirin.
- **Bulut geliştirmeyi basitleştirin:** Azure Dosyaları, yeni bulut geliştirme projelerini basitleştirmek için çeşitli şekillerde de kullanılabilir. Örnek:
    - **Paylaşılan uygulama ayarları:** Dağıtılmış uygulamalar için ortak bir desen, yapılandırma dosyalarının birçok uygulama örneğinden erişilebildiği merkezi bir konumda olmasıdır. Uygulama örnekleri yapılandırmalarını Dosya REST API'si aracılığıyla yükleyebilir. Kullanıcılar, Kobİ payını yerel olarak monte ederek gerektiğinde bunlara erişebilirler.
    - **Tanısal paylaşım:** Azure dosya paylaşımı, bulut uygulamalarının günlüklerini, ölçümlerini ve kilitlenme dökümlerini yazması için kullanışlı bir yerdir. Günlükler dosya rest API üzerinden başvuru örnekleri tarafından yazılabilir. Geliştiriciler, dosya paylaşımını yerel makinelerine monte ederek bunlara erişebilirler. Bu özellik büyük esneklik sağlar. Geliştiriciler, bildikleri mevcut araçtan vazgeçmek zorunda kalmadan bulut geliştirmeyi benimseyebilirler.

**Başvurular**
- [Azure portalında](https://portal.azure.com)
- [Azure Dosyalar belgeleri](/azure/storage/files/storage-files-introduction)
- [Hızlı başlangıçlar](/azure/storage/files/storage-files-quick-create-use-windows)

## <a name="azure-queue-storage"></a>Azure Kuyruk Depolama
[Azure Sıra depolama,](https://azure.microsoft.com/services/storage/queues/) çok sayıda ileti depolamak için bir hizmettir. Http veya HTTPS kullanarak kimlik doğrulaması yapılan aramalar aracılığıyla dünyanın her yerinden gelen iletilere erişebilirsiniz. Bir sıra iletisi 64 KB boyutuna kadar olabilir. Bir sıra, bir depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti içerebilir. Kuyruklar genellikle eşzamanlı olarak işlemek için bir çalışma biriktirme listesi oluşturmak için kullanılır.

**Başvurular**
- [Azure portalında](https://portal.azure.com)
- [Azure Kuyruk depolama belgeleri](/azure/storage/queues/)
- [Hızlı başlangıçlar](/azure/storage/queues/storage-quickstart-queues-portal)
- [Örnekler](/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
