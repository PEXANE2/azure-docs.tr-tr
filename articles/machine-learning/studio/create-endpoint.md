---
title: Web hizmeti uç noktaları oluşturma
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio'da (klasik) web hizmeti bitiş noktaları oluşturun. Web hizmetindeki her bitiş noktası bağımsız olarak ele alınıp daraltılır ve yönetilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 02/15/2019
ms.openlocfilehash: 980ed1e54de30ec8a2dc0c1fdac6546d31f48a00
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218194"
---
# <a name="create-endpoints-for-deployed-azure-machine-learning-studio-classic-web-services"></a>Dağıtılan Azure Machine Learning Studio (klasik) web hizmetleri için uç noktalar oluşturun

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

> [!NOTE]
> Bu konu, **Klasik** Machine Learning web hizmeti için geçerli teknikleri açıklar.

Bir web hizmeti dağıtıldıktan sonra, bu hizmet için varsayılan bir uç noktası oluşturulur. Varsayılan uç noktası, API anahtarı kullanılarak çağrılabilir. Web Hizmetleri portalından kendi anahtarlarıyla daha fazla uç nokta ekleyebilirsiniz.
Web hizmetindeki her bitiş noktası bağımsız olarak ele alınıp daraltılır ve yönetilir. Her bitiş noktası, müşterilerinize dağıtabileceğiniz yetkilendirme anahtarına sahip benzersiz bir URL'dir.

## <a name="add-endpoints-to-a-web-service"></a>Web hizmetine uç nokta ekleme

Azure Machine Learning Web Services portalını kullanarak bir web hizmetine bir bitiş noktası ekleyebilirsiniz. Bitiş noktası oluşturulduktan sonra, eşzamanlı API'ler, toplu API'ler ve excel çalışma sayfaları aracılığıyla tüketebilirsiniz.

> [!NOTE]
> Web hizmetine ek uç noktaları eklediyseniz, varsayılan bitiş noktasını silemezsiniz.

1. Machine Learning Studio'da (klasik), soldaki gezinti sütununda Web Hizmetleri'ni tıklatın.
2. Web hizmeti panosunun alt kısmında uç **noktaları yönet'i**tıklatın. Azure Machine Learning Web Services portalı, web hizmetinin uç noktaları sayfasına açılır.
3. **Yeni'yi**tıklatın.
4. Yeni bitiş noktası için bir ad ve açıklama yazın. Uç nokta adları 24 karakter veya daha az uzunlukta olmalı ve küçük harf alfabeleri veya sayılardan olmalıyım. Günlüğe kaydetme düzeyini ve örnek verilerin etkin olup olmadığını seçin. Günlük hakkında daha fazla bilgi için Machine [Learning web hizmetleri için günlük atmasını etkinleştir mesuliya](web-services-logging.md)bakın.

## <a name="scale-a-web-service-by-adding-additional-endpoints"></a><a id="scaling"></a>Ek uç noktalar ekleyerek bir web hizmetini ölçeklendirin

Varsayılan olarak, yayınlanan her web hizmeti 20 eşzamanlı isteği destekleyecek şekilde yapılandırılır ve 200 eşzamanlı istek kadar yüksek olabilir. Azure Machine Learning Studio (klasik), web hizmetiniz için en iyi performansı sağlamak için ayarı otomatik olarak optimize eder ve portal değeri göz ardı edilir.

API'yi 200 Max Eşzamanlı Arama değerinden daha yüksek bir yükle aramayı planlıyorsanız, aynı web hizmetinde birden çok uç nokta oluşturmanız gerekir. Daha sonra yükünüzü hepsine rasgele dağıtabilirsiniz.

Bir web hizmetinin ölçekleme ortak bir görevdir. Ölçeklendirmenin bazı nedenleri, 200'den fazla eşzamanlı isteği desteklemek, birden çok uç nokta üzerinden kullanılabilirliği artırmak veya web hizmeti için ayrı uç noktaları sağlamakdır. [Azure Machine Learning Web Service](https://services.azureml.net/) portalı aracılığıyla aynı web hizmeti için ek uç noktaları ekleyerek ölçeği artırabilirsiniz.

YÜKSEK eşzamanlılık sayısı kullanmanın, API'yi buna bağlı olarak yüksek bir oranla aramadığınız da zararlı olabileceğini unutmayın. Yüksek yük için yapılandırılmış bir API'ye nispeten düşük bir yük koyarsanız gecikme süresinde düzensiz zaman zaman ları ve/veya ani artışlar görebilirsiniz.

Senkron API'ler genellikle düşük gecikme nin istendiği durumlarda kullanılır. Buradaki gecikme, API'nin bir isteği tamamlaması için gereken süreyi gösterir ve herhangi bir ağ gecikmesini hesaba katmaz. Diyelim ki 50 ms gecikmeli bir API'niz var. Gaz seviyesi Yüksek ve Max Eşzamanlı Aramalar = 20 ile kullanılabilir kapasiteyi tam olarak tüketmek için, bu API 20 * 1000 / 50 = 400 kez saniyede aramanız gerekir. Bunu daha da genişleterek, 200'ün Max Eşzamanlı Çağrıları, 50 ms gecikme süresi olduğunu varsayarak API'yi saniyede 4000 kez aramanızı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Machine Learning web hizmeti nasıl tüketilir?](consume-web-services.md)
