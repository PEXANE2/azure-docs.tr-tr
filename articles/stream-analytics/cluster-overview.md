---
title: Azure Stream Analytics kümelerine genel bakış (Önizleme)
description: Stream Analytics kümesinin tek kiracılı adanmış teklifi hakkında bilgi edinin.
author: sidramadoss
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: overview
ms.custom: mvc
ms.date: 09/22/2020
ms.openlocfilehash: 613cf7d9b68fe42c26f2c01cb1fb5dd1da1e1fb5
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90947991"
---
# <a name="overview-of-azure-stream-analytics-cluster-preview"></a>Azure Stream Analytics kümesine genel bakış (Önizleme)

Azure Stream Analytics küme, karmaşık ve zorlu akış senaryoları için tek kiracılı bir dağıtım sunar. Tam ölçekte Stream Analytics kümeler gerçek zamanlı 200 MB 'tan daha fazla bir saniye işlem yapabilir. Adanmış kümeler üzerinde çalışan Stream Analytics işleri standart teklifteki tüm özelliklerden yararlanabilir ve girişlerinize ve çıkışlarınız için özel bağlantı desteği içerir.

Stream Analytics kümeler, kümenize ayrılan CPU ve bellek kaynaklarının miktarını temsil eden akış birimlerine (SUs) göre faturalandırılır. Akış birimi, standart ve adanmış tekliflerde aynıdır. Her küme için 36, 72, 108, 144, 180 veya 216 SUs satın alabilirsiniz. Bir Stream Analytics kümesi, kuruluşunuz için akış platformu olarak görev yapabilir ve çeşitli kullanım durumlarında çalışan farklı takımlar tarafından paylaşılabilir.

## <a name="what-are-stream-analytics-clusters"></a>Stream Analytics kümeleri nelerdir?

Stream Analytics kümeler, çok kiracılı bir ortamda çalışan Stream Analytics işleri destekleyen altyapıda desteklenir. Tek kiracı, adanmış küme aşağıdaki özelliklere sahiptir:

* Diğer kiracılardan gürültü olmadan tek kiracılı barındırma. Kaynaklarınız gerçekten "yalıtılmıştır" ve trafikte patlama olduğunda daha iyi çalışır.

* Akış kullanımınız zaman içinde arttıkça, kümenizi 36 ile 216 arasında ölçeklendirin.

* Stream Analytics işlerinin özel uç noktaları kullanarak diğer kaynaklara güvenli bir şekilde bağlanmasını sağlayan VNet desteği.

* Herhangi bir bölgedeki C# Kullanıcı tanımlı işlevlerini ve özel seri hale Getiricileri yazma özelliği.

* Gerçek zamanlı analiz çözümleri oluşturma çabalarınızı odaklanmanızı sağlayan sıfır bakım maliyeti.

## <a name="how-to-get-started"></a>Nasıl kullanmaya başlarım

[Azure Portal](https://aka.ms/asaclustercreateportal)aracılığıyla [bir Stream Analytics kümesi oluşturabilirsiniz](create-cluster.md) . Herhangi bir sorunuz varsa veya ekleme ile ilgili yardıma ihtiyacınız varsa [Stream Analytics ekibine](mailto:askasa@microsoft.com)başvurabilirsiniz.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-do-i-choose-between-a-stream-analytics-cluster-and-a-stream-analytics-job"></a>Stream Analytics kümesi ve Stream Analytics işi arasında Nasıl yaparım? seçin.

Kullanmaya başlamanın en kolay yolu, hizmeti tanımanız ve analiz gereksinimlerinizi nasıl karşılayabileceğinizi görmek için bir Stream Analytics işi oluşturup geliştirmektir.

Stream Analytics işler tek başına VNET 'leri desteklemez. Girişlerinizin veya çıkışlarınızın bir güvenlik duvarı veya bir Azure sanal ağı arkasında güvenliği varsa, aşağıdaki iki seçeneğe sahip olursunuz:

* Yerel makinenizin bir VNet (örneğin, Azure Event Hubs veya Azure SQL veritabanı) tarafından güvenliği sağlanmış giriş ve çıkış kaynaklarına erişimi varsa, [Visual Studio için Azure Stream Analytics araçları](stream-analytics-tools-for-visual-studio-install.md) 'nı yerel makinenize yükleyebilirsiniz. Herhangi bir ücret ödemeden [Stream Analytics işleri cihazınızda yerel olarak](stream-analytics-live-data-local-testing.md) geliştirebilir ve test edebilirsiniz. Mimarinizdeki Stream Analytics kullanmaya hazırladıktan sonra, bir Stream Analytics kümesi oluşturabilir, Özel uç noktaları yapılandırabilir ve işlerinizi ölçekli olarak çalıştırabilirsiniz.

* Bir Stream Analytics kümesi oluşturabilir, kümeyi ardışık düzen için gereken özel uç noktalarla yapılandırabilir ve Stream Analytics işlerinizi kümede çalıştırabilirsiniz.

### <a name="what-performance-can-i-expect"></a>Hangi performansı beklemeliyim?

SU, standart ve adanmış tekliflerde aynıdır. Tam 36 SU kümesini kullanan tek bir iş, milisaniyelik gecikme süresiyle yaklaşık 36 MB/saniyelik işleme elde edebilir. Tam sayı, olayların biçimine ve analiz türüne bağlıdır. Adanmış olduğundan Stream Analytics küme daha güvenilir performans garantisi sunar. Kümenizde çalışan tüm işler yalnızca size aittir.

### <a name="can-i-scale-my-cluster"></a>Kümemi ölçeklendirebilirim?

Evet. Kümenizin kapasitesini, değişen taleplerinizi karşılamak için gereken şekilde [ölçeği büyütme veya](scale-cluster.md) küçültme olanağı vererek kolayca yapılandırabilirsiniz.

### <a name="can-i-run-my-existing-jobs-on-these-new-clusters-ive-created"></a>Oluşturduğum bu yeni kümeler üzerinde var olan işlerimi çalıştırabilir miyim?

Evet. Mevcut işlerinizi yeni oluşturulan Stream Analytics kümenize bağlayabilir ve her zamanki gibi çalıştırabilirsiniz. Mevcut Stream Analytics işlerinizi sıfırdan yeniden oluşturmanız gerekmez.

### <a name="how-much-will-these-clusters-cost-me"></a>Bu kümeler ne kadar ücret alacak?

Stream Analytics kümeleriniz, seçilen SU kapasitesine göre ücretlendirilir. Kümeler saatlik olarak faturalandırılır ve bu kümelerde çalışan iş başına ek ücret alınmaz. Özel uç nokta faturalandırma güncelleştirmeleri için [özel bağlantı hizmeti fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/private-link/) bakın.

### <a name="which-inputs-and-outputs-can-i-privately-connect-to-from-my-stream-analytics-cluster"></a>Hangi giriş ve çıkışları Stream Analytics kümeme göre özel olarak bağlayabilirim?

Stream Analytics, çeşitli giriş ve çıkış türlerini destekler. Azure özel bağlantılarını destekleyen bu hizmetlerden herhangi biri, işlerinize özel olarak bağlanabilir. Kümenizde, işlerin giriş ve çıkış kaynaklarına erişmesine izin veren [Özel uç noktalar oluşturabilirsiniz](private-endpoints.md) .

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Stream Analytics kümesine bir genel bakış sunulmaktadır. Sonra, kümenizi oluşturabilir ve Stream Analytics işinizi çalıştırabilirsiniz: 

* [Stream Analytics kümesi oluşturma](create-cluster.md)
* [Azure Stream Analytics kümesinde özel uç noktaları yönetme](private-endpoints.md)
* [Stream Analytics kümesinde Stream Analytics işleri yönetme](manage-jobs-cluster.md)
* [Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)
