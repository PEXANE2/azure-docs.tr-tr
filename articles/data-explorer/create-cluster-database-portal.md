---
title: 'Hızlı başlatma: DB & bir Azure Veri Gezgini kümesi oluşturma'
description: Bu hızlı başlangıçta Azure Veri Gezgini kümesi ve veritabanı oluşturup veri almayı (yüklemeyi) öğreneceksiniz.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 07/22/2019
ms.openlocfilehash: ed0c570449a0c21e9eace1273228539db7c208da
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80238648"
---
# <a name="quickstart-create-an-azure-data-explorer-cluster-and-database"></a>Hızlı başlangıç: Azure Veri Gezgini kümesi ve veritabanı oluşturma

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [Powershell](create-cluster-database-powershell.md)
> * [C #](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
> * [ARM şablonu](create-cluster-database-resource-manager.md)


Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Azure Veri Gezgini'ni kullanmak için öncelikle bir küme ve bu kümenin içinde bir veya daha fazla veritabanı oluşturmanız gerekir. Ardından veritabanına veri alarak (yükleyerek) sorgu çalıştırabilirsiniz. Bu hızlı başlangıçta bir küme ve bir veritabanı oluşturacaksınız.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="create-a-cluster"></a>Küme oluşturma

Bir Azure kaynak grubunda tanımlı bir bilgi işlem ve depolama kaynakları kümesiiçeren bir Azure Veri Gezgini kümesi oluşturun.

1. **Portalın** sol üst köşesinde + kaynak oluştur düğmesini seçin.

1. *Azure Veri Gezgini* için arama yapın.

   ![Kaynak arama](media/create-cluster-database-portal/search-resources.png)

1. **Azure Veri Gezgini**'nin altında, ekranın en alt bölümünde **Oluştur**'u seçin.

1. Temel küme ayrıntılarını aşağıdaki bilgilerle doldurun.

   ![Küme formu oluşturma](media/create-cluster-database-portal/create-cluster-form2.png)

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Abonelik | Aboneliğiniz | Kümeniz için kullanmak istediğiniz Azure aboneliğini seçin.|
    | Kaynak grubu | Kaynak grubunuz | Varolan bir kaynak grubu kullanın veya yeni bir kaynak grubu oluşturun. |
    | Küme adı | Benzersiz küme adı | Kümenizi tanımlayan benzersiz bir ad seçin. Girdiğiniz küme adının sonuna *[bölge].servicebus.windows.net* etki alanı adı eklenir. Ad yalnızca küçük harf ve sayı içerebilir. 4 ila 22 karakter içermelidir.
    | Bölge | *Batı ABD* veya *Batı ABD 2* | Bu hızlı başlangıç için *Batı ABD* veya Batı *ABD 2'yi* (kullanılabilirlik bölgelerini kullanıyorsanız) seçin. Üretim sisteminde ihtiyaçlarınıza en uygun bölgeyi seçmeniz gerekir.
    | Kullanılabilirlik alanları | *1*, *2*, ve/veya *3* | Küme örneklerini aynı bölgedeki çeşitli kullanılabilirlik bölgelerine yerleştirin (isteğe bağlı). [Azure Kullanılabilirlik Bölgeleri,](/azure/availability-zones/az-overview) aynı Azure bölgesinde benzersiz fiziksel konumlardır. Bir Azure Veri Gezgini kümesini ve verileri kısmi bölge hatasından korurlar. Küme düğümleri varsayılan olarak aynı veri merkezinde oluşturulur. Birkaç kullanılabilirlik bölgesi seçerek tek bir hata noktasını ortadan kaldırabilir ve yüksek kullanılabilirlik sağlayabilirsiniz. Kullanılabilirlik bölgelerine dağıtım yalnızca küme oluşturma sırasında mümkündür ve daha sonraki bir tarihte değiştirilemez.
    | İşlem belirtimleri | *D13_v2* | Bu hızlı başlangıç için en düşük fiyatlı özelliği seçin. Üretim sisteminde ihtiyaçlarınıza en uygun özelliği seçmeniz gerekir.
    | | | |

1. Küme ayrıntılarınızı gözden geçirmek için **Gözden Geçir + oluştur'u** ve kümeyi sağlamak için **Oluştur'u** seçin. Sağlama genellikle yaklaşık 10 dakika sürer.

1. Dağıtım tamamlandığında **kaynağa git'i**seçin.

    ![Kaynağa git](media/create-cluster-database-portal/notification-resource.png)

## <a name="create-a-database"></a>Veritabanı oluşturma

Artık işlemin ikinci adımı olan veritabanı oluşturma bölümüne geçebilirsiniz.

1. **Genel bakış** sekmesinde **Veritabanı oluştur**'u seçin.

    ![Adım 2: veritabanı oluşturma](media/create-cluster-database-portal/database-creation.png)

1. Formu aşağıdaki bilgilerle doldurun.

    ![Veritabanı oluşturma formu](media/create-cluster-database-portal/create-database.png)

    **Ayar** | **Önerilen değer** | **Alan açıklaması**
    |---|---|---|
    | Veritabanı adı | *TestDatabase* | Veritabanı adı küme içinde benzersiz olmalıdır.
    | Bekletme süresi | *3650* | Verilerin sorgu için kullanılabilir durumda tutulmasının garanti edildiği zaman aralığı (günler içinde). Zaman aralığı verilerin alındığı andan itibaren hesaplanır.
    | Önbellek süresi | *31* | Uzun süreli depolama alanı yerine sıkça sorgulanan verileri SSD depolama veya RAM'de tutmak için zaman aralığı (gün içinde).
    | | | |

1. Veritabanını oluşturmak için **Oluştur'u** seçin. Oluşturma işlemi genellikle bir dakikadan kısa sürer. İşlem tamamlandığında yeniden kümenin **Genel bakış** sekmesi görüntülenir.

## <a name="run-basic-commands-in-the-database"></a>Veritabanında temel komutları çalıştırma

Artık bir kümeye ve veritabanına sahip olduğunuza göre sorgu ve komut çalıştırabilirsiniz. Veritabanında henüz veri yok ancak yine de araçların nasıl çalıştığını görebilirsiniz.

1. Kümenizde **Sorgu**'yu seçin. Komutu `.show databases` sorgu penceresine yapıştırın ve **ardından Çalıştır'ı**seçin.

    ![Veritabanlarını göster komutu](media/create-cluster-database-portal/show-databases.png)

    Sonuç kümesi, kümedeki tek veritabanı olan **TestDatabase** veritabanını gösterir.

1. Komutu `.show tables` sorgu penceresine yapıştırın ve **Çalıştır'ı**seçin.

    Henüz bir tablonuz olmadığı için bu komut boş sonuç kümesi döndürür. Bu dizinin bir sonraki makalesinde tablo ekleyeceksiniz.

## <a name="stop-and-restart-the-cluster"></a>Kümeyi durdurma ve yeniden başlatma

İhtiyaç duymanız halinde kümeyi durdurup yeniden başlatabilirsiniz.

1. Kümeyi durdurmak için **Genel bakış** sekmesinin en üstünden **Durdur**'u seçin.

    Küme durdurulduğunda veriler sorgulanamaz ve yeni veri alınamaz.

1. Kümeyi yeniden başlatmak için **Genel bakış** sekmesinin en üstünden **Başlat**'ı seçin.

    Küme yeniden başlatıldığında, kullanılabilir olması yaklaşık 10 dakika sürer (örneğin, başlangıçta sağlandığında olduğu gibi). Verilerin sık erişim önbelleğine yüklenmesi daha uzun zaman alabilir.  

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer hızlı başlangıçları ve öğreticileri izlemeyi planlıyorsanız, oluşturduğunuz kaynakları koruyun. Aksi takdirde, kaynak grubunu temizleyerek, maliyetlerin karşılanmasından kaçının.

1. Azure portalında, en solda **Kaynak gruplarını** seçin ve ardından Veri Gezgini kümenizi içeren kaynak grubunu seçin.  

1. **Tüm kaynak grubunu** silmek için kaynak grubunu sil'i seçin. Varolan bir kaynak grubunu kullanıyorsanız, yalnızca Veri Gezgini kümesini silmeyi seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Verileri Olay Hub'ından Azure Veri Gezgini'ne alma](ingest-data-event-hub.md)


