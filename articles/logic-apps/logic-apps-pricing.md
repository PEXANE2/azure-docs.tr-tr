---
title: Fiyatlandırma & faturalandırma modeli
description: Azure Logic Apps için fiyatlandırma ve faturalandırma modelinin nasıl çalıştığına genel bakış
services: logic-apps
ms.suite: integration
author: jonfancey
ms.author: jonfan
ms.reviewer: estfan, logicappspm
ms.topic: conceptual
ms.date: 07/19/2019
ms.openlocfilehash: f47c7412bdd5ada1e50d1005b8e740e3f46ffd8d
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536242"
---
# <a name="pricing-model-for-azure-logic-apps"></a>Azure Mantık Uygulamaları için fiyatlandırma modeli

[Azure Logic Apps,](../logic-apps/logic-apps-overview.md) bulutta ölçeklendirilebilen otomatik tümleştirme iş akışları oluşturmanıza ve çalıştırmanıza yardımcı olur. Bu makalede, Azure Mantık Uygulamaları için faturalandırma ve fiyatlandırmanın nasıl çalıştığı açıklanmaktadır. Fiyatlandırma oranları için Logic [Apps Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/logic-apps)bakın.

<a name="consumption-pricing"></a>

## <a name="consumption-pricing-model"></a>Tüketim fiyatlandırma modeli

Genel olarak çalışan yeni mantıksal uygulamalar için , "genel", çok kiracılı Azure Logic Apps hizmeti, yalnızca kullandığınız kadar ödeme yaparsınız. Bu mantık uygulamaları tüketim tabanlı bir plan ve fiyatlandırma modeli kullanır. Mantık uygulamanızda her adım bir eylemdir ve Azure Logic Apps mantık uygulamanızda çalışan tüm eylemleri ölçer.

Örneğin, eylemler şunlardır:

* [Tetikleyiciler](#triggers), özel eylemlerdir. Tüm mantık uygulamaları ilk adım olarak bir tetikleyici gerektirir.

* HTTP gibi ["yerleşik" veya yerel eylemler,](../connectors/apis-list.md#built-in) Azure İşlevlerini ve API Yönetimi'ni çağırır ve benzeri

* Outlook 365, Dropbox ve benzeri [yönetilen bağlayıcılara](../connectors/apis-list.md#managed-connectors) yapılan aramalar

* Döngüler, koşullu ifadeler ve benzeri gibi [iş akışı eylemlerini denetleme](../connectors/apis-list.md#control-workflow)

[Standart konektörler](../connectors/apis-list.md#managed-connectors) [Standart konektör fiyatıüzerinden](https://azure.microsoft.com/pricing/details/logic-apps)ücretlendirilir. Genel olarak kullanılabilir [Kurumsal konektörler](../connectors/apis-list.md#managed-connectors) [Enterprise konektör fiyatından](https://azure.microsoft.com/pricing/details/logic-apps)ücretlendirilirken, genel önizleme Kurumsal bağlayıcılar [Standart bağlayıcı fiyatıüzerinden](https://azure.microsoft.com/pricing/details/logic-apps)ücretlendirilir.

[Tetikleyiciler](#triggers) ve [eylemler](#actions) düzeylerinde faturalandırmanın nasıl çalıştığı hakkında daha fazla bilgi edinin. Veya, sınırlar hakkında bilgi için [Azure Mantık Uygulamaları için Sınırlar ve yapılandırma'ya](logic-apps-limits-and-config.md)bakın.

<a name="fixed-pricing"></a>

## <a name="fixed-pricing-model"></a>Sabit fiyatlandırma modeli

[ *Bir tümleştirme hizmeti ortamı* (ISE),](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) bir Azure sanal ağındaki kaynaklara erişebilen mantık uygulamaları oluşturmanız ve çalıştırmanız için yalıtılmış bir yol sağlar. İmKB'de çalışan mantıksal uygulamalar veri saklama maliyetlerine tabi değildir. Bir İmKB oluşturduğunuzda ve yalnızca oluşturma sırasında, farklı [fiyatlandırma oranlarına](https://azure.microsoft.com/pricing/details/logic-apps)sahip bir [İmKB düzeyi veya "SKU"](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level)seçebilirsiniz:

* **Premium** İmKB: Bu SKU'nun temel birimi sabit kapasiteye sahiptir, ancak daha fazla iş elde etme ihtiyacınız varsa, İmKB oluşturma sırasında veya sonrasında [daha fazla ölçek birimi ekleyebilirsiniz.](../logic-apps/ise-manage-integration-service-environment.md#add-capacity) İmKB sınırları için Azure [Mantık Uygulamaları için Sınırlar ve yapılandırma ya](logic-apps-limits-and-config.md#integration-service-environment-ise)da yapılandırma ya da

* **Geliştirici** İmKB: Bu SKU'nun ölçeklendirme yeteneği, hizmet düzeyi anlaşması (SLA) ve yayınlanmış sınırlar bulunmamaktadır. Bu SKU'yi yalnızca deneme, geliştirme ve test için kullanın, üretim veya performans testi için değil.

Bir İmKB'de oluşturduğunuz ve çalıştırdığınız mantıksal uygulamalar için, bu özellikler için sabit bir [aylık ücret](https://azure.microsoft.com/pricing/details/logic-apps) ödenirsiniz:

* [Yerleşik](../connectors/apis-list.md#built-in) tetikleyiciler ve eylemler

  Bir Ise içinde, yerleşik tetikleyiciler ve eylemler **Core** etiketini görüntüler ve mantık uygulamalarınızla aynı İmKB'de çalışır.

* [İstediğinizin](../connectors/apis-list.md#managed-connectors) kadar Kurumsal bağlantıya sahip olmasını sağlayan standart konektörler ve [Kurumsal](../connectors/apis-list.md#enterprise-connectors) bağlayıcılar

   **İmKB** etiketini görüntüleyen Standart ve Kurumsal bağlayıcılar, mantık uygulamalarınızla aynı İmKB'de çalışır. İmKB etiketini görüntülemeyin bağlayıcılar, "global", çok kiracılı Logic Apps hizmeti. Sabit aylık fiyatlandırma, bir İmKB'de çalışan mantıksal uygulamalarla kullandığınızda çok kiracılı hizmette çalışan bağlayıcılar için de geçerlidir.

* [İmKB SKU'nuza](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level)göre ek ücret ödemeden [entegrasyon hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) kullanımı :

  * **Premium** ISE SKU: Tek bir [Standart katman](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits) entegrasyon hesabı

  * **Geliştirici** ISE SKU: Tek bir [Serbest katman](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits) entegrasyon hesabı

  Her İmKB SKU toplam 5 entegrasyon hesabı ile sınırlıdır. Ek bir maliyet karşılığında, İmKB SKU'nuza göre daha fazla entegrasyon hesabınız olabilir:

  * **Premium** İmKB SKU: En fazla dört Standart hesap. Ücretsiz veya Temel hesap yok.

  * **Geliştirici** İmKB SKU: Ya en fazla 4 standart hesap, ya da en fazla 5 toplam Standart hesabı. Temel hesap yok.

  Tümleştirme hesabı sınırları hakkında daha fazla bilgi için [Azure Mantık Uygulamaları için Sınırlar ve yapılandırma ya](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits)da yapılandırma ya da Bu konuda daha sonra [entegrasyon hesabı katmanları ve fiyatlandırma modeli](#integration-accounts) hakkında daha fazla bilgi edinebilirsiniz.

<a name="connectors"></a>

## <a name="connectors"></a>Bağlayıcılar

Azure Logic Apps bağlayıcıları, [tetikleyiciler,](#triggers) [eylemler](#actions)veya her ikisini de sağlayarak mantık uygulamanızın uygulamalara, hizmetlere ve sistemlere bulutta veya şirket içinde erişmesine yardımcı olur. Konektörler Standart veya Kurumsal olarak sınıflandırılır. Bu bağlayıcılar hakkında genel bir bakış için Azure [Mantık Uygulamaları Için Bağlayıcılar'a](../connectors/apis-list.md)bakın. Mantık uygulamalarınızda kullanmak istediğiniz REST API'leri için önceden oluşturulmuş bağlayıcı yoksa, yalnızca bu REST API'lerinin etrafında sarılan [özel konektörler](https://docs.microsoft.com/connectors/custom-connectors)oluşturabilirsiniz. Özel konektörler Standart bağlayıcılar olarak faturalandırılır. Aşağıdaki bölümler, tetikleyiciler ve eylemler için faturalandırmanın nasıl çalıştığı hakkında daha fazla bilgi sağlar.

<a name="triggers"></a>

## <a name="triggers"></a>Tetikleyiciler

Tetikleyiciler, belirli bir olay gerçekleştiğinde bir mantık uygulaması örneği oluşturan özel eylemlerdir. Tetikleyiciler, mantık uygulamasının ölçülme şeklini etkileyen farklı şekillerde hareket eder. Azure Mantık Uygulamaları'nda bulunan çeşitli tetikleyiciler şunlardır:

* **Yoklama tetikleyicisi**: Bu tetikleyici, bir mantık uygulaması örneği oluşturma ve iş akışını başlatma ölçütlerini karşılayan iletiler için bir bitiş noktasını sürekli olarak denetler. Hiçbir mantık uygulaması örneği oluşturulmasa bile, Logic Apps her yoklama isteğini yürütme olarak ölçer. Yoklama aralığını belirtmek için tetikleyiciyi Logic App Designer aracılığıyla ayarlayın.

  [!INCLUDE [logic-apps-polling-trigger-non-standard-metering](../../includes/logic-apps-polling-trigger-non-standard-metering.md)]

* **Webhook tetikleyicisi**: Bu tetikleyici, istemcinin isteği belirli bir bitiş noktasına göndermesini bekler. Webhook bitiş noktasına gönderilen her istek bir eylem yürütmesi olarak sayılır. Örneğin, İstek ve HTTP Webhook tetikleyicisi her ikisi de webhook tetikleyicileridir.

* **Yineleme tetikleyicisi**: Bu tetikleyici, tetikleyicide ayarladığınız yineleme aralığını temel alan bir mantık uygulaması örneği oluşturur. Örneğin, her üç günde bir veya daha karmaşık bir zamanlamada çalışan bir Yineleme tetikleyicisi ayarlayabilirsiniz.

<a name="actions"></a>

## <a name="actions"></a>Eylemler

Azure Logic Apps, http gibi yerel eylemler gibi "yerleşik" eylemleri ölçer. Örneğin, yerleşik eylemler arasında HTTP çağrıları, Azure İşlevleri veya API Yönetimi'nden gelen çağrılar ve koşullar, döngüler ve anahtar bildirimleri gibi denetim akışı adımları yer alır. Her eylemin kendi eylem türü vardır. Örneğin, [bağlayıcıları](https://docs.microsoft.com/connectors) arayan eylemler "ApiConnection" türüne sahiptir. Bu bağlayıcılar, ilgili [fiyatlandırmalarına](https://azure.microsoft.com/pricing/details/logic-apps)göre ölçülü olan Standart veya Kurumsal konektörler olarak sınıflandırılır. *Önizleme'deki* kurumsal konektörler Standart bağlayıcılar olarak ücretlendirilir.

Azure Logic Apps, tüm başarılı ve başarısız eylemleri yürütme olarak ölçer. Ancak, Logic Apps bu eylemleri ölçmez:

* Karşılanmayan koşullar nedeniyle atlanan eylemler
* Mantık uygulaması son işlemden önce durduğu için çalışmadığı eylemler

Döngüler içinde çalışan eylemler için Azure Logic Apps döngüdeki her döngü için her eylemi sayar. Örneğin, bir listeyi işleyen bir "her biri için" döngüye sahip olduğunuzu varsayalım. Logic Apps, liste öğelerinin sayısını döngüdeki eylem sayısıyla çarparak bu döngüdeki bir eylemi ölçer ve döngüyü başlatan eylemi ekler. Yani, 10 maddelik bir liste için hesaplama (10 * 1) + 1, hangi 11 eylem yürütmeleri sonuçlanır.

## <a name="disabled-logic-apps"></a>Devre dışı bırakılmış mantık uygulamaları

Devre dışı bırakılmış mantık uygulamaları, devre dışı bırakılırken yeni örnekler oluşturamadıkları için ücretlendirilmez. Bir mantık uygulamasını devre dışı bıraktıktan sonra, şu anda çalışan örneklerin tamamen durması biraz zaman alabilir.

<a name="integration-accounts"></a>

## <a name="integration-accounts"></a>Tümleştirme hesapları

Azure Logic Apps'taki [B2B, EDI](logic-apps-enterprise-integration-b2b.md) ve [XML işleme](logic-apps-enterprise-integration-xml.md) özelliklerini ek ücret ödemeden keşfedebileceğiniz, geliştirebileceğiniz ve test edebileceğiniz [entegrasyon hesapları](logic-apps-enterprise-integration-create-integration-account.md) için sabit [fiyatlandırma modeli](https://azure.microsoft.com/pricing/details/logic-apps) geçerlidir. Her Azure aboneliğinin belirli bir [tümleştirme hesabı sınırına](../logic-apps/logic-apps-limits-and-config.md#integration-account-limits)kadar olması gerekir. Her tümleştirme hesabı, ticaret ortakları, anlaşmalar, haritalar, şemalar, derlemeler, sertifikalar, toplu iş yapılandırmaları ve benzeri yapıları içeren belirli yapı ların [belirli bir sınırına](../logic-apps/logic-apps-limits-and-config.md#artifact-number-limits)kadar depolayabilir.

Azure Logic Apps Ücretsiz, Temel ve Standart tümleştirme hesapları sunar. Temel ve Standart katmanlar Logic Apps hizmet düzeyi sözleşmesi (SLA) tarafından desteklenirken, Ücretsiz katman bir SLA tarafından desteklenmez ve üretim ve kullanım sınırlamaları vardır. Ücretsiz katman tümleştirme hesapları dışında, her Azure bölgesinde birden fazla tümleştirme hesabınız olabilir. Fiyatlandırma oranları için [Logic Apps fiyatlandırması'na](https://azure.microsoft.com/pricing/details/logic-apps/)bakın.

[Premium veya Geliştirici](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level)gibi bir entegrasyon hizmet [ *ortamınız* (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)varsa, İmKB'nizin toplam 5 tümleştirme hesabı olabilir. Sabit fiyatlandırma modelinin Bir İmKB için nasıl çalıştığını öğrenmek için, bu konudaönceki [Sabit fiyatlandırma modeli](#fixed-pricing) bölümüne bakın. Fiyatlandırma oranları için [Logic Apps fiyatlandırması'na](https://azure.microsoft.com/pricing/details/logic-apps)bakın.

Ücretsiz, Temel veya Standart tümleştirme hesabı arasında seçim yapmak için şu kullanım örneği açıklamalarını gözden geçirin:

* **Ücretsiz**: Üretim senaryolarını değil, araştırmacı senaryoları denemek istediğinizde

* **Temel**: Yalnızca mesaj işleme yi istediğinizde veya daha büyük bir işletme kuruluşuyla ticari ortak ilişkisi olan küçük bir işletme ortağı olarak hareket etmek istediğinizde

* **Standart**: Daha karmaşık B2B ilişkileriniz ve yönetmeniz gereken varlıkların sayısı arttıkça

<a name="data-retention"></a>

## <a name="data-retention"></a>Veri saklama

Bir tümleştirme hizmeti ortamında (İmKB) çalışan mantık uygulamaları dışında, mantık uygulamanızın çalışma geçmişinde depolanan tüm giriş ve çıktılar, bir mantık uygulamasının [çalışma bekletme süresine](logic-apps-limits-and-config.md#run-duration-retention-limits)bağlı olarak faturalandırılır. İmKB'de çalışan mantıksal uygulamalar veri saklama maliyetlerine tabi değildir. Fiyatlandırma oranları için [Logic Apps fiyatlandırması'na](https://azure.microsoft.com/pricing/details/logic-apps)bakın.

Mantık uygulamanızın depolama tüketimini izlemenize yardımcı olmak için şunları yapabilirsiniz:

* Mantık uygulamanızın aylık olarak kullandığı GB'deki depolama birimi sayısını görüntüleyin.
* Mantık uygulamanızın çalışma geçmişinde belirli bir eylemin giriş ve çıktılarının boyutlarını görüntüleyin.

<a name="storage-consumption"></a>

### <a name="view-logic-app-storage-consumption"></a>Mantık uygulaması depolama tüketimini görüntüleyin

1. Azure portalında mantık uygulamanızı bulun ve açın.

1. Mantık uygulamanızın menüsünden İzleme **altında** **Ölçümler'i**seçin.

1. Grafik Başlığı altında, Grafik **Başlığı**altında sağ bölmede, **Depolama** **Tüketim Uygulamaları için Faturalandırma Kullanımı'nı**seçin.

   Bu metrik, faturalandırılan ayda GB olarak depolama tüketim birimlerinin sayısını sağlar.

<a name="input-output-sizes"></a>

### <a name="view-action-input-and-output-sizes"></a>Eylem giriş ve çıkış boyutlarını görüntüleme

1. Azure portalında mantık uygulamanızı bulun ve açın.

1. Mantık uygulamanızın menüsünde **Genel Bakış'ı**seçin.

1. Sağ bölmede, **Çalıştır geçmişi**altında, denetlemek istediğiniz giriş ve çıktıları içeren çalıştır'ı seçin.

1. **Mantık uygulaması altında çalıştır**, Ayrıntıları **Çalıştır'ı**seçin.

1. Mantık **uygulaması çalışma ayrıntıları** bölmesinde, her eylemin durumunu ve süresini listeleyen eylemler tablosunda görüntülemek istediğiniz eylemi seçin.

1. Logic **uygulama eylem** bölmesinde, bu eylemin giriş ve çıkışlarının boyutlarını sırasıyla **Girişler bağlantısı** ve Çıktılar **bağlantısı**altında görünür.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Mantık Uygulamaları hakkında daha fazla bilgi edinin](logic-apps-overview.md)
* [İlk mantıksal uygulamanızı oluşturma](quickstart-create-first-logic-app-workflow.md)
