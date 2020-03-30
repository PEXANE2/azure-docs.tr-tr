---
title: En İyi Uygulamalar
description: Azure Uygulama Hizmeti'nde çalışan uygulamanız için en iyi uygulamaları ve sık karşılaşılan sorun giderme senaryolarını öğrenin.
author: dariagrigoriu
ms.assetid: f3359464-fa44-4f4a-9ea6-7821060e8d0d
ms.topic: article
ms.date: 07/01/2016
ms.author: dariac
ms.custom: seodec18
ms.openlocfilehash: ded812d5d7a0440466e7284b56c90965ea00406e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75768495"
---
# <a name="best-practices-for-azure-app-service"></a>Azure Uygulama Hizmeti için En İyi Uygulamalar
Bu makalede, Azure Uygulama [Hizmeti](https://go.microsoft.com/fwlink/?LinkId=529714)kullanmak için en iyi uygulamaları özetler. 

## <a name="colocation"></a><a name="colocation"></a>Colocation
Web uygulaması ve veritabanı gibi bir çözüm oluşturan Azure kaynakları farklı bölgelerde bulunduğunda, aşağıdaki etkilere sahip olabilir:

* Kaynaklar arasındaki iletişimde artan gecikme
* [Azure fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/data-transfers)belirtildiği gibi, giden veri aktarımı için parasal ücretler bölgeler arası.

Aynı bölgede birlikte konumlandırma, web uygulaması ve içerik veya veri tutmak için kullanılan bir veritabanı veya depolama hesabı gibi bir çözüm oluşturan Azure kaynakları için en iyisidir. Kaynak oluştururken, belirli bir işletmeniz veya tasarım nedeniniz yoksa, aynı Azure bölgesinde olmadıklarından emin olun. Premium Uygulama Hizmeti Planı uygulamaları için şu anda kullanılabilen [App Service klonlama özelliğini](app-service-web-app-cloning.md) kullanarak bir Uygulama Hizmeti uygulamasını veritabanınızla aynı bölgeye taşıyabilirsiniz.   

## <a name="when-apps-consume-more-memory-than-expected"></a><a name="memoryresources"></a>Uygulamalar beklenenden daha fazla bellek tüketdiğinde
Bir uygulamanın izleme veya hizmet önerileri yoluyla belirtildiği gibi beklenenden daha fazla bellek tükettiğini fark ettiğinizde, [App Service Otomatik İyileştirme özelliğini](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites)göz önünde bulundurun. Otomatik İyileştirme özelliğinin seçeneklerinden biri, bellek eşiğini temel alan özel eylemlerde bulunmaktır. Eylemler, e-posta bildirimlerinden bellek dökümü üzerinden araştırmaya ve çalışan işlemi geri dönüştürerek yerinde azaltıcı incelemeye kadar geniş bir yelpazeyi kapsar. Otomatik iyileştirme web.config üzerinden ve [App Service Support Site Extension](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps)için bu blog yazısı açıklandığı gibi samimi bir kullanıcı arayüzü üzerinden yapılandırılabilir.   

## <a name="when-apps-consume-more-cpu-than-expected"></a><a name="CPUresources"></a>Uygulamalar beklenenden daha fazla CPU tüketdiğinde
Bir uygulamanın beklenenden daha fazla CPU tükettiğini veya izleme veya hizmet önerileri yoluyla belirtildiği gibi tekrarlanan CPU artışları yaşadığını fark ettiğinizde, App Service planını ölçeklemeyi veya ölçeklemeyi düşünün. Uygulamanız durum doluysa, ölçeklendirme tek seçenektir, ancak uygulamanız durum yoksa, ölçeklendirme size daha fazla esneklik ve daha yüksek ölçek potansiyeli sağlar. 

"Stateful" ve "stateless" uygulamaları hakkında daha fazla bilgi için bu videoyu izleyebilirsiniz: [Azure Uygulama Hizmeti'nde Ölçeklenebilir Uçtan Uca Çok Katmanlı Uygulama Planlama.](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid) Uygulama Hizmeti ölçeklendirme ve otomatik ölçeklendirme seçenekleri hakkında daha fazla bilgi için Azure [Uygulama Hizmeti'nde Bir Web Uygulaması Ölçeklendir'e](manage-scale-up.md)bakın.  

## <a name="when-socket-resources-are-exhausted"></a><a name="socketresources"></a>Soket kaynakları tükendiğinde
Giden TCP bağlantılarını tüketen yaygın bir neden, TCP bağlantılarını yeniden kullanmak için uygulanmayan istemci kitaplıklarının kullanılması veya HTTP - Keep-Alive gibi üst düzey bir protokol kullanılmadığında kullanılmasıdır. Giden bağlantıların verimli bir şekilde yeniden kullanımı için kodunuzda yapılandırıldığından veya erişildiklerinden emin olmak için Uygulama Hizmet Planı'nızdaki uygulamalar tarafından başvurulan kitaplıkların her biri için belgeleri gözden geçirin. Ayrıca, bağlantıların sızmasını önlemek için uygun oluşturma ve serbest bırakma veya temizleme için kitaplık belgeleri kılavuzunu izleyin. Bu tür istemci kitaplıkları incelemeleri devam ederken, birden çok örne ölçeklendirilerek etki azaltılabilir.

### <a name="nodejs-and-outgoing-http-requests"></a>Node.js ve giden http istekleri
Node.js ve birçok giden http istekleri ile çalışırken, HTTP ile ilgili - Keep-Alive önemlidir. Kodunuzda daha kolay hale getirmek için [agentkeepalive](https://www.npmjs.com/package/agentkeepalive) `npm` paketini kullanabilirsiniz.

Işleyicide `http` hiçbir şey yapmasanız bile, yanıtı her zaman ele alın. Yanıtı düzgün bir şekilde işlemezseniz, daha fazla soket bulunmadığından uygulamanız sonunda takılır.

Örneğin, `http` paketle `https` çalışırken:

```javascript
const request = https.request(options, function(response) {
    response.on('data', function() { /* do nothing */ });
});
```

Linux'ta Birden çok çekirdekli bir makinede Uygulama Hizmeti üzerinde çalışıyorsanız, başka bir en iyi uygulama, uygulamanızı yürütmek için birden fazla Node.js işlemini başlatmak için PM2'yi kullanmaktır. Bunu, kapsayıcınıza bir başlangıç komutu belirterek yapabilirsiniz.

Örneğin, dört örnek başlatmak için:

```
pm2 start /home/site/wwwroot/app.js --no-daemon -i 4
```

## <a name="when-your-app-backup-starts-failing"></a><a name="appbackup"></a>Uygulama yedeklemeniz başarısız lığa başladığında
Uygulama yedeklemesinin başarısız olmasının en yaygın iki nedeni şunlardır: geçersiz depolama ayarları ve geçersiz veritabanı yapılandırması. Bu hatalar genellikle depolama veya veritabanı kaynaklarında değişiklikler olduğunda veya bu kaynaklara nasıl erişileceklerine yönelik değişiklikler olduğunda (örneğin, yedekleme ayarlarında seçilen veritabanı için güncelleştirilen kimlik bilgileri) olur. Yedeklemeler genellikle bir zamanlamaya göre çalışır ve depolama (yedeklenmiş dosyaları çıkarmak için) ve veritabanlarına (içeriği yedeklemeye dahil edilecek kopyalama ve okuma için) erişim gerektirir. Bu kaynaklardan herhangi biri erişememenin sonucu tutarlı yedekleme hatası olacaktır. 

Yedekleme hataları olduğunda, hangi tür bir hata olduğunu anlamak için en son sonuçları gözden geçirin. Depolama erişim hataları için, yedekleme yapılandırmasında kullanılan depolama ayarlarını gözden geçirin ve güncelleştirin. Veritabanı erişim hataları için, uygulama ayarlarının bir parçası olarak bağlantı dizelerinizi gözden geçirin ve güncelleyin; ardından gerekli veritabanlarını düzgün bir şekilde içerecek şekilde yedekleme yapılandırmanızı güncelleştirmeye devam edin. Uygulama yedeklemeleri hakkında daha fazla bilgi için [Azure Uygulama Hizmeti'nde bir web uygulamasını yedekle'ye](manage-backup.md)bakın.

## <a name="when-new-nodejs-apps-are-deployed-to-azure-app-service"></a><a name="nodejs"></a>Azure Uygulama Hizmetine yeni Düğüm.js uygulamaları dağıtıldığında
Node.js uygulamaları için Azure Uygulama Hizmeti varsayılan yapılandırması, en yaygın uygulamaların gereksinimlerine en iyi şekilde uyacak şekilde tasarlanmıştır. Node.js uygulamanızın yapılandırması, CPU/bellek/ağ kaynakları için performansı artırmak veya kaynak kullanımını optimize etmek için kişiselleştirilmiş ayardan yararlanacaksa, [Azure Uygulama Hizmeti'ndeki Düğüm uygulamaları için en iyi uygulamalar ve sorun giderme kılavuzuna](app-service-web-nodejs-best-practices-and-troubleshoot-guide.md)bakın. Bu makalede, Node.js uygulamanız için yapılandırmanız gerekebilecek iisnode ayarları açıklanır, uygulamanızın karşı karşıya olabileceği çeşitli senaryoları veya sorunları açıklar ve bu sorunların nasıl giderilenini gösterir.


## <a name="next-steps"></a>Sonraki Adımlar
En iyi uygulamalar hakkında daha fazla bilgi için, kaynağınıza özel eyleme geçirilebilir en iyi uygulamaları bulmak için [App Service Diagnostics'i](https://docs.microsoft.com/azure/app-service/overview-diagnostics) ziyaret edin.

- [Azure portalında](https://portal.azure.com)Web Uygulamanıza gidin.
- Uygulama Hizmeti Tanılama'yı açan sol navigasyonda **Tanılama ve sorunları çözme'yi** tıklatın.
- **En İyi Uygulamalar** ana sayfa döşemesini seçin.
- Uygulamanızın bu en iyi uygulamalarla ilgili geçerli durumunu görüntülemek için **Kullanılabilirlik & Performans** için En İyi Uygulamalar'ı veya **Optimal Yapılandırma için En İyi Uygulamalar'ı** tıklatın.

Bu bağlantıyı, kaynağınız için Doğrudan Uygulama Hizmeti Tanılama'yı açmak için de kullanabilirsiniz: `https://ms.portal.azure.com/?websitesextension_ext=asd.featurePath%3Ddetectors%2FParentAvailabilityAndPerformance#@microsoft.onmicrosoft.com/resource/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Web/sites/{siteName}/troubleshoot`.