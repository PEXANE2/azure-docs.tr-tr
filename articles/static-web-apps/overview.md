---
title: Azure statik Web Apps nedir?
description: Azure statik Web Apps 'nin temel özellikleri ve işlevleri.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: overview
ms.date: 05/08/2020
ms.author: cshoe
ms.openlocfilehash: d416044599535e8acd363d09099e8667bba59a0a
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83599970"
---
# <a name="what-is-azure-static-web-apps-preview"></a>Azure statik Web Apps önizlemesi nedir?

Azure statik Web Apps, tam yığın Web uygulamalarını otomatik olarak oluşturup bir GitHub deposundan Azure 'a dağıtan bir hizmettir.

:::image type="content" source="media/overview/static-apps-overview.png" alt-text="Statik Web Apps genel bakış":::

Azure statik Web Apps iş akışı, geliştiricinin günlük iş akışına göre tasarlanmıştır. Uygulamalar GitHub etkileşimini temel alarak oluşturulup dağıtılır.

Azure statik Web Apps kaynağı oluşturduğunuzda, Azure, uygulamanın kaynak kodu deposunda seçtiğiniz bir dalı izleyen bir GitHub eylemleri iş akışı kurar. Değişiklikleri izlenen dala her gönderişinizde veya çekme isteklerini kabul ettiğinizde, GitHub eylemi uygulamanızı ve API 'sini Azure 'a otomatik olarak oluşturur ve dağıtır.

Statik Web uygulamaları, genellikle angular, tepki, svelte veya Vue gibi kitaplıklar ve çerçeveler kullanılarak oluşturulmuştur. Bu uygulamalar, uygulamayı oluşturan HTML, CSS, JavaScript ve resim varlıklarını içerir. Geleneksel bir Web sunucusu ile, bu varlıklar, gerekli tüm API uç noktaları ile birlikte tek bir sunucudan sunulur.

Statik Web Apps, statik varlıklar geleneksel bir Web sunucusundan ayrılır ve bunun yerine dünyanın dört bir yanındaki coğrafi olarak dağıtılan noktalarından sunulur. Bu dağıtım, dosyalar son kullanıcılara fiziksel olarak daha çok daha hızlı hale gelir. Ayrıca, API uç noktaları [sunucusuz bir mimari](../azure-functions/functions-overview.md)kullanılarak barındırılır ve tamamen bir arka uç sunucusu gereksinimini ortadan kaldırır.

## <a name="key-features"></a>Önemli özellikler

- HTML, CSS, JavaScript ve görüntüler gibi statik içerik için **ücretsiz web barındırma** .
- Azure Işlevleri tarafından sunulan **TÜMLEŞIK API** desteği.
- Depo değişikliklerinin derlemeleri ve dağıtımları tetikleyen **birinci taraf GitHub tümleştirmesi** .
- **Genel olarak dağıtılmış** statik içerik, içeriği kullanıcılarınıza yaklaştırır.
- Otomatik olarak yenilenen **ÜCRETSIZ SSL sertifikaları**.
- **Özel etki alanları** \* uygulamanıza markalı özelleştirmeler sağlamak için.
- CORS yapılandırması gerektirmeyen API 'Ler çağrılırken ters proxy ile **sorunsuz güvenlik modeli** .
- Azure Active Directory, Facebook, Google, GitHub ve Twitter ile **kimlik doğrulama sağlayıcısı tümleştirmeleri** .
- **Özelleştirilebilir yetkilendirme rolü tanımı** ve atamaları.
- **Arka uç yönlendirme kuralları** , size kullandığınız içerik ve yollar üzerinde tam denetimi etkinleştirir.
- Çekme istekleri tarafından desteklenen **üretilen hazırlama sürümleri** , yayımlamadan önce sitenizin önizleme sürümlerini etkinleştirir.

## <a name="what-you-can-do-with-static-web-apps"></a>Statik Web Apps ile yapabilecekleriniz

- [Azure işlevleri](https://azure.microsoft.com/services/functions/) arka ucu Ile [angular](https://angular.io/), [tepki](https://reactjs.org/)verme, [svelte](https://svelte.dev/) [gibi çerçeveler](https://vuejs.org/) ve kitaplıklar ile **modern JavaScript uygulamaları oluşturun** .
- [Gatsby](publish-gatsby.md), [kugo](publish-hugo.md), [vuepress](publish-vuepress.md)gibi çerçeveler ile **statik siteler yayımlayın** .
- [Sonraki. js](deploy-nextjs.md) ve [nuxt. js](deploy-nuxtjs.md)gibi çerçeveler ile **Web uygulamaları dağıtın** .

\*Tepesinde etki alanı kayıtları önizleme sırasında desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [İlk statik uygulamanızı oluşturma](getting-started.md)
