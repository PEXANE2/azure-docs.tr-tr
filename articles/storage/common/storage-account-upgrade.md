---
title: Genel amaçlı v2 depolama hesabına yükseltme
titleSuffix: Azure Storage
description: Genel amaçlı v2 depolama hesaplarına yükseltin.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 02/25/2019
ms.author: tamram
ms.openlocfilehash: 9afbade408d6f95fcd3a61aa1ba65bc09c7a875b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80067224"
---
# <a name="upgrade-to-a-general-purpose-v2-storage-account"></a>Genel amaçlı v2 depolama hesabına yükseltme

Genel amaçlı v2 depolama hesapları en son Azure Depolama özelliklerini destekler ve genel amaçlı v1 ve Blob depolama hesaplarının tüm işlevselliğini içerir. Çoğu depolama senaryosu için genel amaçlı v2 hesapları önerilir. Genel amaçlı v2 hesapları, Azure Depolama için gigabayt başına en düşük kapasite fiyatlarının yanı sıra endüstriyle rekabet eden işlem fiyatlarını da sunar. Genel amaçlı v2 hesapları, sıcak, serin veya arşiv arasında sıcak veya serin ve blob düzeyinde katmanlama varsayılan hesap erişim katmanları destekler.

Genel amaçlı v1 veya Blob depolama hesaplarınızdan genel amaçlı v2 depolama hesabına yükseltme yapmak kolaydır. Azure portalını, PowerShell'i veya Azure CLI'yi kullanarak yükseltme yapabilirsiniz. Genel amaçlı bir v2 depolama hesabına yükseltme ile ilişkili herhangi bir kesinti veya veri kaybı riski yoktur. Hesap yükseltme, hesap türünü değiştiren basit bir Azure Kaynak Yöneticisi işlemi yle gerçekleşir.

> [!IMPORTANT]
> Genel amaçlı bir v1 veya Blob depolama hesabını genel amaçlı v2'ye yükseltmek kalıcıdır ve geri alınamaz.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Depolama hesabınıza gidin.
3. **Ayarlar** bölümünde **Yapılandırma'yı**tıklatın.
4. **Hesap türü**altında , **Yükseltme'ye**tıklayın.
5. **Yükseltmeyi Onayla**altında, hesabınızın adını yazın.
6. Bıçağın altındaki **Yükseltme'yi** tıklatın.

    ![Hesap Türünü Yükseltme](../blobs/media/storage-blob-account-upgrade/upgrade-to-gpv2-account.png)

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

PowerShell'i kullanarak genel amaçlı bir v1 hesabını genel amaçlı bir v2 hesabına yükseltmek için, Önce PowerShell'i **Az.Storage** modülünün en son sürümünü kullanmak için güncelleyin. PowerShell’i yükleme hakkında bilgi edinmek için bkz. [Azure PowerShell’i yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/install-Az-ps).

Ardından, kaynak grup adınızı, depolama hesabı adınızı ve istediğiniz hesap erişim katmanını yerine koymak için hesabı yükseltmek için aşağıdaki komutu arayın.

```powershell
Set-AzStorageAccount -ResourceGroupName <resource-group> -AccountName <storage-account> -UpgradeToStorageV2 -AccessTier <Hot/Cool>
```
# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLI'yi kullanarak genel amaçlı bir v1 hesabını genel amaçlı bir v2 hesabına yükseltmek için önce Azure CLI'nin en son sürümünü yükleyin. CLI yüklemesi hakkında bilgi için bkz. [Azure CLI 2.0’ı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

Ardından, kaynak grup adınızı, depolama hesabı adınızı ve istediğiniz hesap erişim katmanını yerine koymak için hesabı yükseltmek için aşağıdaki komutu arayın.

```azurecli
az storage account update -g <resource-group> -n <storage-account> --set kind=StorageV2 --access-tier=<Hot/Cool>
```

---

## <a name="specify-an-access-tier-for-blob-data"></a>Blob verileri için bir erişim katmanı belirtin

Genel amaçlı v2 hesapları tüm Azure depolama hizmetlerini ve veri nesnelerini destekler, ancak erişim katmanları yalnızca Blob depolama alanındaki blob'ları engellemek için kullanılabilir. Genel amaçlı bir v2 depolama hesabına yükselttiğinizde, tek tek blob erişim katmanı parametresi belirtilmemiş gibi varsayılan katmanı gösterir sıcak veya serin bir varsayılan hesap erişim katmanı belirtebilirsiniz.

Blob erişim katmanları, beklenen kullanım alışkanlıklarınıza göre en uygun maliyetli depolama alanını seçmenizi sağlar. Blok lekeleri sıcak, serin veya arşiv katmanlarında saklanabilir. Erişim katmanları hakkında daha fazla bilgi için [Azure Blob depolama alanı: Sıcak, Serin ve Arşiv depolama katmanlarına](../blobs/storage-blob-storage-tiers.md)bakın.

Varsayılan olarak, sıcak erişim katmanında yeni bir depolama hesabı oluşturulur ve genel amaçlı bir v1 depolama hesabı sıcak veya havalı hesap katmanına yükseltilebilir. Yükseltmede bir hesap erişim katmanı belirtilmemişse, varsayılan olarak sıcak olarak yükseltilir. Yükseltmeniz için hangi erişim katmanının kullanılacağını keşfediyorsanız, geçerli veri kullanım senaryonuzu göz önünde bulundurun. Genel amaçlı bir v2 hesabına geçiş için iki tipik kullanıcı senaryosu vardır:

* Varolan genel amaçlı bir v1 depolama hesabınız var ve blob verileri için doğru depolama erişim katmanıyla genel amaçlı v2 depolama hesabına yükseltmeyi değerlendirmek istiyorsunuz.
* Genel amaçlı bir v2 depolama hesabı kullanmaya karar verdiniz veya zaten bir hesabınız var ve blob verileri için sıcak veya serin depolama erişim katmanını kullanıp kullanmadığınızı değerlendirmek istiyorsunuz.

Her iki durumda da, ilk öncelik, genel amaçlı bir v2 depolama hesabında depolanan verilerinizi depolama, erişim ve işletme maliyetini tahmin etmek ve bunu geçerli maliyetlerinizle karşılaştırmaktır.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

V1 depolama hesabını genel amaçlı bir v2 hesabına yükseltmek ücretsizdir. Yükseltme işlemi sırasında istenen hesap katmanını belirtebilirsiniz. Yükseltmede bir hesap katmanı belirtilmemişse, yükseltilen hesabın `Hot`varsayılan hesap katmanı . Ancak, yükseltmeden sonra depolama erişim katmanının değiştirilmesi faturanızda değişikliklere neden olabilir, bu nedenle yükseltme sırasında yeni hesap katmanının belirtilen bir durum dur.

Tüm depolama hesapları, blob depolama için her blobun katmanını temel alan bir fiyatlandırma modelini kullanır. Bir depolama hesabını kullanırken aşağıdaki fatura değerlendirmeleri geçerlidir:

* **Depolama maliyetleri**: Depolanan veri miktarına ek olarak, veri depolama maliyeti depolama erişim katmanına bağlı olarak değişir. Katmanın erişim sıklığı düştükçe gigabayt başına ücret de azalır.

* **Veri erişimi maliyetleri**: Katmanın erişimi sıklığı düştükçe veri erişimi ücretleri artar. Serin ve arşiv depolama erişim katmanındaki veriler için, okumalar için gigabayt başına veri erişim ücreti ücretlendirilirsiniz.

* **İşlem maliyetleri**: Tüm katmanlarda, erişim sıklığı düştükçe artan bir işlem başına ücret uygulanır.

* **Coğrafi Çoğaltma veri aktarımı maliyetleri**: Bu ücret, GRS ve RA-GRS dahil olmak üzere yalnızca coğrafi çoğaltma yapılandırılmış hesaplara uygulanır. Coğrafi çoğaltma veri aktarımı gigabayt başına ücret doğurur.

* **Giden veri aktarımı maliyetleri**: Giden veri aktarımları (bir Azure bölgesinin dışına aktarılan veriler), genel amaçlı depolama hesapları ile tutarlı şekilde gigabayt başına esaslı olarak bant genişliği kullanımı için fatura doğurur.

* **Depolama erişim katmanının değiştirilmesi**: Hesap depolama erişim katmanının serinden sıcaka değiştirilmesi, depolama hesabında bulunan tüm verilerin okunmasına eşit bir ücrete neden olur. Ancak, hesap erişim katmanının sıcaktan soğuğa değiştirilmesi, tüm verilerin serin katmana yazılmasına eşit bir ücrete neden olur (yalnızca GPv2 hesapları).

> [!NOTE]
> Depolama hesaplarına ilişkin fiyatlandırma modeli hakkında daha fazla bilgi için [Azure Depolama Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) sayfasına bakın. Giden veri aktarımı ücretlerine ilişkin daha fazla bilgi için [Veri Aktarımları Fiyatlandırma Bilgileri](https://azure.microsoft.com/pricing/details/data-transfers/) sayfasına bakın.

### <a name="estimate-costs-for-your-current-usage-patterns"></a>Geçerli kullanım deleleriniz için tahmini maliyetler

Blob verilerini belirli bir katmandaki genel amaçlı bir v2 depolama hesabında depolama ve erişme maliyetini tahmin etmek için, varolan kullanım dedurumunuzu değerlendirin veya beklenen kullanım deseninize yaklaşın. Genel olarak, şunları bilmek istersiniz:

* Blob depolama tüketiminiz, gigabaytlar, dahil:
  * Depolama hesabınızda ne kadar veri depolanıyor?
  * Aylık temelde veri hacmi nasıl değişiyor; yeni veriler sürekli eski verilerin yerini alıyor mu?

* Blob depolama verileriniz için birincil erişim deseni:
  * Depolama hesabından ne kadar veri okunuyor ve yazılıyor?
  * Depolama hesabındaki verilerde kaç okuma işlemi ve yazma işlemleri oluşur?

İhtiyaçlarınız için en iyi erişim katmanına karar vermek için, blob veri kapasitenizi ve bu verilerin nasıl kullanıldığını belirlemek yararlı olabilir. Bu, hesabınızın izleme ölçümlerine bakarak en iyi şekilde yapılabilir.

### <a name="monitoring-existing-storage-accounts"></a>Var olan depolama hesaplarını izleme

Var olan depolama hesaplarınızı izlemek ve bu verileri toplamak için, bir depolama hesabına yönelik günlüğe kaydetme işlemlerini gerçekleştiren ve ölçümler sağlayan Azure Depolama Analizi hizmetinden yararlanabilirsiniz. Depolama Analizi GPv1, GPv2 ve Blob depolama hesap türleri için toplu işlem istatistiklerini içeren ölçümleri ve depolama hizmetine yapılan isteklere ilişkin kapasite verilerini depolayabilir. Bu veriler aynı depolama hesabındaki iyi bilinen tablolara depolanır.

Daha fazla bilgi için bkz. [Storage Analytics Ölçümleri hakkında](https://msdn.microsoft.com/library/azure/hh343258.aspx) ve [Storage Analytics Ölçüm Tablosu Şeması](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [!NOTE]
> Blob depolama hesapları, Tablo hizmeti uç noktasını yalnızca ilgili hesabın ölçüm verilerini depolamak ve bunlara erişmek için kullanıma sunar.

Blob depolamada depolama tüketimini izlemek için kapasite ölçümlerini etkinleştirmeniz gerekir.
Bu etkinle, kapasite verileri bir depolama hesabının Blob hizmeti için günlük olarak kaydedilir ve aynı depolama hesabı içinde *$MetricsCapacityBlob* tabloya yazılmış bir tablo girişi olarak kaydedilir.

Blob depolama hizmetinin veri erişim desenlerini izlemek için API’den saatlik işlem ölçümlerini etkinleştirmeniz gerekir. Saatlik işlem ölçümleri etkinleştirildiğinde API başına işlemler saatte bir toplanır ve aynı depolama hesabındaki *$MetricsHourPrimaryTransactionsBlob* tablosuna yazılan bir tablo girişi olarak kaydedilir. RA-GRS depolama hesapları kullanılırken *$MetricsHourSecondaryTransactionsBlob* tablosu, işlemleri ikincil uç noktaya kaydeder.

> [!NOTE]
> Blok ve ekleme blobu verileriyle birlikte sayfa bloblarını ve sanal makine disklerini veya kuyrukları, dosyaları ya da tabloları depoladığınız genel amaçlı bir depolama hesabınız olması durumunda bu tahmin işlemi geçerli değildir. Kapasite verileri blok bloblarını diğer türlerden ayırt etmez ve diğer veri türleri için kapasite verileri sunmaz. Bu türleri kullanıyorsanız alternatif bir yöntem de en son faturanızdaki miktarlara bakmaktır.

Veri tüketim ve erişim modelinizi yaklaşık olarak tahmin etmek için, ölçümler için düzenli kullanımınızı temsil eden bir elde tutma süresi seçmeniz ve tahmin etmeniz önerilir. Seçeneklerden biri son yedi güne ait ölçüm verilerinin tutulması ve verilerin ay sonunda analiz için haftada bir toplanmasıdır. Diğer bir seçenek ise son 30 güne ait ölçüm verilerinin tutulması ve verilerin 30 günlük süre sonunda toplanıp çözümlenmesidir.

Metrik verileri etkinleştirme, toplama ve görüntüleme hakkındaki ayrıntılar için [Depolama analizi ölçümlerine](../common/storage-analytics-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)bakın.

> [!NOTE]
> Analiz verilerinin depolanması, erişimi ve indirilmesi de normal kullanıcı verileri gibi ücretlendirilir.

### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Kullanım ölçümlerinden yararlanarak maliyetleri tahmin etme

#### <a name="capacity-costs"></a>Kapasite maliyetleri

*$MetricsCapacityBlob* kapasite ölçüm tablosunda *'data'* satır anahtarını içeren en son giriş, kullanıcı verilerinin harcadığı kapasiteyi gösterir. *$MetricsCapacityBlob* kapasite ölçüm tablosunda *'analytics'* satır anahtarını içeren en son giriş, analiz günlüklerinin harcadığı kapasiteyi gösterir.

Hem kullanıcı verileri hem de analiz günlükleri (etkinse) tarafından kullanılan bu toplam kapasite, verileri depolama hesabına depolama maliyetini tahmin etmek için kullanılabilir. Aynı yöntem ayrıca GPv1 depolama hesaplarında depolanma maliyetlerini tahmin etmek için kullanılabilir.

#### <a name="transaction-costs"></a>İşlem maliyetleri

İşlem ölçüm tablosundaki bir API’nin tüm girişleri için *'TotalBillableRequests'* toplamı, ilgili API’nin toplam işlem sayısını belirtir. *Örneğin*, belirli bir süre içindeki *'GetBlob'* işlemlerinin toplam sayısı *'user;GetBlob'* satır anahtarını içeren tüm girişlere yönelik toplam faturalandırılabilir isteklerin toplamına göre hesaplanabilir.

Blob depolama hesaplarına ilişkin işlem maliyetlerini tahmin etmek için, farklı şekilde fiyatlandırıldıkları için işlemleri üç gruba ayırmanız gerekir.

* *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'* ve *'CopyBlob'* gibi yazma işlemleri.
* *'DeleteBlob'* ve *'DeleteContainer'* gibi silme işlemleri.
* Diğer tüm işlemler.

GPv1 depolama hesaplarının işlem maliyetlerini tahmin etmek için işlemden/API’den bağımsız olarak tüm işlemleri toplamanız gerekir.

#### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Veri erişimi ve coğrafi çoğaltma veri aktarımı maliyetleri

Storage Analytics bir depolama hesabından okunan ve depolama hesabına yazılan veri miktarını belirtmese de, işlem ölçümleri tablosuna bakılarak bu değer kabaca tahmin edilebilir. İşlem ölçüm tablosundaki bir API’nin tüm girişleri için *'TotalIngress'* toplamı, ilgili API’nin toplam giriş verileri miktarını bayt cinsinden belirtir. Benzer şekilde, *'TotalEgress'* toplamı toplam çıkış verileri miktarını bayt cinsinden belirtir.

Blob depolama hesaplarına ilişkin veri erişimi maliyetlerini hesaplamak için işlemleri iki gruba ayırmanız gerekir.

* Depolama hesabından alınan veri miktarı birincil olarak *'GetBlob'* ve *'CopyBlob'* işlemleri için *'TotalEgress'* toplamına bakılarak tahmin edilebilir.

* Depolama hesabına yazılan veri miktarı birincil olarak *'PutBlob'*, *'PutBlock'*, *'CopyBlob'* ve *'AppendBlock'* işlemleri için *'TotalIngress'* toplamına bakılarak tahmin edilebilir.

Blob depolama hesaplarında coğrafi çoğaltma veri aktarımı maliyeti de bir GRS veya RA-GRS depolama hesabı kullanılırken yazılan veri miktarı tahmini kullanılarak hesaplanabilir.

> [!NOTE]
> Sıcak veya havalı depolama erişim katmanını kullanmanın maliyetlerini hesaplama hakkında daha ayrıntılı bir örnek için, *'Sıcak ve Cool erişim katmanları nedir ve hangisini kullanacağımı nasıl belirlemem gerekir?'* başlıklı SSS'ye bir göz atın. bkz. [Azure Depolama Fiyatlandırma Sayfası](https://azure.microsoft.com/pricing/details/storage/).

## <a name="next-steps"></a>Sonraki adımlar

* [Depolama hesabı oluşturma](storage-account-create.md)
