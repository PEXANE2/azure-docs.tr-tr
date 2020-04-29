---
title: Azure depolama ile yüksek düzeyde güvenli, dayanıklı ve ölçeklenebilir uygulamalar oluşturmak için bulut depolama
description: Büyük yapılı ve yapılandırılmamış mobil uygulama verilerini bulutta depolamak için hizmetler hakkında bilgi edinin.
author: codemillmatt
ms.assetid: 12bbb070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: 682f06936480d9018d759b4df3bed80be1206faf
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80240983"
---
# <a name="cloud-storage-for-highly-secure-durable-scalable-apps-with-azure-storage"></a>Azure depolama ile yüksek oranda güvenli, dayanıklı ve ölçeklenebilir uygulamalar için bulut depolama
[Azure depolama](https://azure.microsoft.com/services/storage/) , Microsoft 'un veri nesneleri için büyük ölçüde ölçeklenebilir nesne deposu, bulut için bir dosya sistemi hizmeti, güvenilir mesajlaşma için bir mesajlaşma deposu ve bir NoSQL deposu sunan modern uygulamalara yönelik bulut depolama çözümüdür. Azure Depolama şu özelliklere sahiptir:
- **Dayanıklı ve yüksek oranda kullanılabilir:** Yedeklilik, verilerinizin geçici donanım arızaları durumunda güvende olmasını sağlar. Ayrıca, yerel felaketler ya da doğal afetlere karşı ek koruma için verileri meri merkezleri ya da coğrafi bölgeler arasında çoğaltmayı seçebilirsiniz. Bu şekilde çoğaltılan veriler beklenmeyen bir kesinti durumunda yüksek oranda kullanılabilir olmaya devam eder.
- **Güvenli:** Azure depolama 'ya yazılan tüm veriler hizmet tarafından şifrelenir. Azure Depolama, verilerinize erişmesi gereken kişiler üzerinde ayrıntılı denetime sahip olmanızı sağlar.
- **Ölçeklenebilir:** Hizmetler, günümüzün uygulamalarının veri depolama ve performans ihtiyaçlarını karşılamak için büyük ölçüde ölçeklenebilir olacak şekilde tasarlanmıştır.
- **Yönetilen:** Azure, donanım bakımını, güncelleştirmeleri ve kritik sorunları sizin için işler.
- **Erişilebilir:** Veriler, dünyanın her yerinden HTTP veya HTTPS üzerinden erişilebilir. Microsoft, .NET, Java, Node. js, Python, PHP, Ruby ve Go gibi çeşitli dillerde istemci kitaplıkları sağlar ve çok sayıda REST API. Betik oluşturma Azure PowerShell veya Azure CLı 'de desteklenir. Azure portal ve Azure Depolama Gezgini, verilerle çalışmaya yönelik kolay görsel çözümler sunar.

Mobil uygulamalarınızda bulut depolamayı etkinleştirmek için aşağıdaki hizmetleri kullanın.

## <a name="azure-blob-storage"></a>Azure Blob depolama
[Azure Blob depolama](https://azure.microsoft.com/services/storage/blobs/) , bulut için bir nesne depolama çözümü sağlar. BLOB depolama, metin veya ikili gibi belirli bir veri modeline veya tanımına bağlı olmayan büyük miktarlarda yapılandırılmamış verileri depolamak için iyileştirilmiştir. İstemci kitaplıklarının kullandığı çeşitli dilleri destekler. BLOB depolama şu şekilde tasarlanmıştır:
- Görüntüleri veya belgeleri doğrudan bir tarayıcıya sunar.
- Dağıtılmış erişim için dosyaları depolayın.
- Video ve ses akışını yapın.
- Günlük dosyalarına yazın.
- Yedekleme ve geri yükleme, olağanüstü durum kurtarma ve arşivleme için verileri depolayın.
- Verileri şirket içinde veya Azure 'da barındırılan bir hizmetle analiz edilmek üzere depolayın.

**Başvurular**
- [Azure portal](https://portal.azure.com)
- [Azure Blob depolama belgeleri](/azure/storage/blobs/storage-blobs-introduction)
- [Hızlı Başlangıçlar](/azure/storage/blobs/storage-quickstart-blobs-portal)
- [Örnekler](/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

## <a name="azure-table-storage"></a>Azure Tablo depolama
[Azure Tablo depolama](https://azure.microsoft.com/services/storage/tables/) , bulutta yapılandırılmış NoSQL verilerini depolayan ve şesız bir tasarıma sahip bir anahtar veya öznitelik deposu sağlayan bir hizmettir. Azure Tablo depolama, büyük miktarlarda yapısal veriyi depolar. Hizmet, Azure bulutunun içinden ve dışından kimliği doğrulanmış çağrılar kabul eden bir NoSQL veri deposudur. Azure tabloları yapılandırılmış, ilişkisel olmayan verileri depolamak için idealdir. Tablo depolama genellikle şu şekilde kullanılır:
- Web ölçeğinde uygulamalara hizmet veren terabayt türünde yapılandırılmış veriler depolayın.
- Karmaşık birleştirmeler, yabancı anahtarlar veya saklı yordamlar gerektirmeyen veri kümelerini depolayın ve hızlı erişim için normalleştirilmiştir.
- Kümelenmiş bir dizin kullanarak verileri hızlıca sorgulayın.
- OData protokolünü ve Windows Communication Foundation (WCF) veri Hizmetleri .NET kitaplıkları ile LINQ sorgularını kullanarak verilere erişin.

Yapılandırılmış ve ilişkisel olmayan verilerin büyük kümelerini depolamak ve sorgulamak için tablo depolama alanını kullanabilirsiniz. Tablolarınız talep arttıkça ölçeklendirilir.

**Başvurular**
- [Azure portal](https://portal.azure.com)
- [Azure Tablo depolama belgeleri](/azure/storage/tables/table-storage-overview)
- [Örnekler](/azure/cosmos-db/tutorial-develop-table-dotnet?toc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fstorage%2Ftables%2FTOC.json&bc=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
- [Hızlı Başlangıçlar](/azure/storage/tables/table-storage-quickstart-portal)

## <a name="azure-files"></a>Azure Dosyaları
[Azure dosyaları](https://azure.microsoft.com/services/storage/files/)ile, standart sunucu ileti bloğu (SMB) protokolü kullanılarak erişilebilen yüksek oranda kullanılabilir ağ dosya paylaşımları ayarlayabilirsiniz. Aynı dosyaları hem okuma hem de yazma erişimiyle paylaşabilir. Ayrıca REST arabirimini veya depolama istemci kitaplıklarını kullanarak dosyaları okuyabilirsiniz. Dosyaya işaret eden ve paylaşılan erişim Imzası (SAS) belirteci içeren bir URL kullanarak dünyanın her yerinden dosyalara erişebilirsiniz. SAS belirteçleri oluşturabilirsiniz. Belirli bir süre için özel bir varlığa belirli bir erişim izni verir.

Azure dosya paylaşımları şunları yapmak için kullanılabilir:
- Şirket **içi dosya sunucularını değiştirme veya bunlara ek ekleme:** Windows, macOS ve Linux gibi popüler işletim sistemleri, dünyanın her yerindeki Azure dosya paylaşımlarını doğrudan bağlayabilir. Azure dosya paylaşımları ayrıca verilerin kullanıldığı yerde performanslı ve dağıtılmış bir şekilde önbelleğe alınması için Azure Dosya Eşitleme ile şirket içi veya bulut üzerindeki Windows Server’larda çoğaltılabilir.
- **Uygulamaları kaldırma ve kaydırma:** Dosya uygulamasının veya Kullanıcı verilerinin depolanması için bir dosya paylaşımının beklediği uygulamaları buluta geçirin.
- **Bulut geliştirmeyi basitleştirme:** Azure dosyaları, yeni bulut geliştirme projelerini basitleştirecek çeşitli yollarla da kullanılabilir. Örneğin:
    - **Paylaşılan uygulama ayarları:** Dağıtılmış uygulamalar için ortak bir model, yapılandırma dosyalarının birçok uygulama örneklerinden erişilebilecekleri merkezi bir konumda olmasını sağlar. Uygulama örnekleri, yapılandırmalarını Dosya REST API aracılığıyla yükleyebilir. Kullanıcılar, SMB paylaşımının yerel olarak bağlanması için bunlara gereken şekilde erişebilirler.
    - **Tanılama paylaşma:** Azure dosya paylaşma, bulut uygulamalarının günlük, ölçüm ve kilitlenme dökümlerinin yazabilmesi için uygun bir yerdir. Günlükler Dosya REST API aracılığıyla uygulama örnekleri tarafından yazılabilir. Geliştiriciler, dosya paylaşımının yerel makinesine bağlanarak bunlara erişebilir. Bu özellik harika esneklik sağlar. Geliştiriciler, bu mevcut araçları öğrenmelerine gerek kalmadan bulut geliştirmeyi çok fazla ayarlayabilir.

**Başvurular**
- [Azure portal](https://portal.azure.com)
- [Azure Dosyalar belgeleri](/azure/storage/files/storage-files-introduction)
- [Hızlı Başlangıçlar](/azure/storage/files/storage-files-quick-create-use-windows)

## <a name="azure-queue-storage"></a>Azure Kuyruk Depolama
[Azure kuyruk depolama](https://azure.microsoft.com/services/storage/queues/) , çok sayıda iletiyi depolamaya yönelik bir hizmettir. HTTP veya HTTPS kullanarak, dünyanın her yerindeki iletilere kimliği doğrulanmış çağrılar aracılığıyla erişirsiniz. Kuyruk iletisi boyutu 64 KB 'ye kadar olabilir. Bir kuyruk, depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti içerebilir. Kuyruklar genellikle zaman uyumsuz olarak işlenecek iş biriktirme listesi oluşturmak için kullanılır.

**Başvurular**
- [Azure portal](https://portal.azure.com)
- [Azure Kuyruk depolama belgeleri](/azure/storage/queues/)
- [Hızlı Başlangıçlar](/azure/storage/queues/storage-quickstart-queues-portal)
- [Örnekler](/azure/storage/common/storage-samples-dotnet?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
