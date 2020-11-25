---
title: App Service için Azure Defender-avantajlar ve Özellikler
description: App Service için Azure Defender 'ın avantajları ve özellikleri hakkında bilgi edinin.
author: memildin
ms.author: memildin
ms.date: 9/22/2020
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: bb0e073d5ccf73434d05c801b9a8727c1d19fa47
ms.sourcegitcommit: b8a175b6391cddd5a2c92575c311cc3e8c820018
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96122232"
---
# <a name="introduction-to-azure-defender-for-app-service"></a>App Service için Azure Defender 'a giriş

Azure App Service, altyapıyı yönetmek zorunda kalmadan Web uygulamalarınızı ve API 'lerinizi oluşturmak ve barındırmak için tam olarak yönetilen bir platformdur. Kurumsal düzeyde performans, güvenlik ve uyumluluk gereksinimlerini karşılamak için yönetim, izleme ve operasyonel içgörüler sağlar. Daha fazla bilgi için bkz. [Azure App Service](https://azure.microsoft.com/services/app-service/).

**App Service Için Azure Defender** , App Service üzerinde çalışan uygulamaları hedefleyen saldırıları belirlemek için bulutun ölçeğini kullanır. Saldırganlar, zayıf yanları bulmak ve yararlanmak için Web uygulamalarını araştırma. Belirli ortamlara yönlendirilmeden önce, Azure 'da çalışan uygulamalara yönelik istekler, incelendikleri ve günlüğe kaydedildiği çeşitli ağ geçitleri aracılığıyla yapılır. Bu veriler daha sonra kötüye kullanım ve saldırganlar tanımlamak ve daha sonra kullanılacak yeni desenler öğrenmek için kullanılır.

Güvenlik Merkezi, Azure 'un bulut sağlayıcısı olarak sahip olduğu görünürlüğü kullanarak, birden çok hedef üzerinde saldırı yöntemini tanımlamak üzere iç günlükleri App Service analiz eder. Örneğin, metodolojide, yaygın tarama ve dağıtılmış saldırılar bulunur. Bu tür bir saldırı genellikle IP 'nin küçük bir alt kümesinden gelir ve birden fazla konaktaki benzer uç noktalara gezinme düzenlerini gösterir. Saldırılar, güvenlik açığı bulunan bir sayfa veya eklenti arıyor ve tek bir konağın açısından belirlenemiyor.


## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Genel olarak kullanılabilir (GA)|
|Fiyat|[App Service Için Azure Defender](azure-defender.md) , [fiyatlandırma sayfasında](security-center-pricing.md) gösterildiği gibi faturalandırılır|
|Desteklenen App Service planları:|![Evet ](./media/icons/yes-icon.png) temel, standart, Premium, yalıtılmış veya Linux<br>![](./media/icons/no-icon.png)Ücretsiz, paylaşılan veya tüketim yok<br>[App Service planları hakkında daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/app-service/plans/)|
|Larının|![Evet](./media/icons/yes-icon.png) Ticari bulutlar<br>![Hayır](./media/icons/no-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov)|
|||

## <a name="what-does-azure-defender-for-app-service-protect"></a>Azure Defender App Service koruma için ne yapmalı?

App Service planı etkinken, Güvenlik Merkezi, App Service planınız kapsamındaki kaynakları değerlendirir ve bulguları temelinde güvenlik önerileri oluşturur. Güvenlik Merkezi, App Service çalıştığı VM örneğini ve yönetim arabirimini korur. Ayrıca, App Service ' de çalışan uygulamalarınıza gönderilen istekleri ve yanıtları izler.

Windows tabanlı bir App Service planı çalıştırıyorsanız, güvenlik merkezi 'nin temel alınan korumalı makinelere ve sanal makinelere da erişimi vardır. Yukarıda bahsedilen günlük verileriyle birlikte, altyapı hikayeye, Müşteri makinelerinden muaf olmak üzere yeni bir saldırıya karşı çok daha fazla bilgi verebilir. Bu nedenle, güvenlik merkezi bir Web uygulaması kullanıldıktan sonra dağıtılsa bile, devam eden saldırıları tespit edebilir.


## <a name="protect-your-azure-app-service-web-apps-and-apis"></a>Azure App Service web uygulamalarınızı ve API'lerinizi koruma
Azure App Service planınızı App Service Azure Defender ile korumak için:

- Adanmış makinelerle ilişkili desteklenen bir App Service planına sahip olduğunuzdan emin olun. Desteklenen planlar, yukarıdaki [kullanılabilirlik](#availability)bölümünde listelenmiştir.

- Aboneliğiniz üzerinde **Azure Defender 'ı** etkinleştirin (isteğe bağlı olarak yalnızca Azure **Defender for App Service** planı) [Azure Güvenlik Merkezi fiyatlandırma](security-center-pricing.md) bölümünde açıklandığı gibi etkinleştirebilirsiniz

Güvenlik Merkezi, App Service ile yerel olarak tümleşiktir ve dağıtım ve ekleme gereksinimini ortadan kaldırır; tümleştirme saydamdır.

>[!NOTE]
> Fiyatlandırma ve Ayarlar sayfasında, **kaynak miktarlarınız** için bir dizi örnek listelenir. Bu, Fiyatlandırma Katmanı sayfasını açtığınız sırada çalışan Bu abonelikteki tüm App Service planlardaki toplam işlem örneği sayısını temsil eder.
>
> Azure App Service çeşitli planlar sunmaktadır. App Service planınız bir Web uygulamasının çalışması için işlem kaynakları kümesini tanımlar. Bunlar geleneksel web barındırma içindeki sunucu grupları ile eşdeğerdir. Bir veya daha fazla uygulama aynı bilgi işlem kaynaklarında (veya aynı App Service planında) çalışacak şekilde yapılandırılabilir.
>
>Sayıyı doğrulamak için, Azure portalında ' App Service planlar ' bölümüne gidin; burada her plan tarafından kullanılan işlem örneklerinin sayısını görebilirsiniz. 



## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, App Service için Azure Defender hakkında bilgi edindiniz. 

İlgili malzemeler için aşağıdaki makalelere bakın: 

- Bir uyarının Güvenlik Merkezi tarafından oluşturulup oluşturulmadığı veya Güvenlik Merkezi tarafından farklı bir güvenlik ürününden alınıp alınmayacağı, dışarı aktarabilirsiniz. Uyarılarınızı Azure Sentinel 'e, herhangi bir üçüncü taraf SıEM 'e veya herhangi bir harici araca aktarmak için, [BIR SıEM, SOAR veya BT hizmet yönetimi çözümüne akış uyarıları](export-to-siem.md)içindeki yönergeleri izleyin.
- Azure App Service uyarılarının listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-azureappserv)bakın.
- App Service planları hakkında daha fazla bilgi için bkz. [App Service planları](https://azure.microsoft.com/pricing/details/app-service/plans/).
- > [!div class="nextstepaction"]
    > [Azure Defender’ı etkinleştirme](security-center-pricing.md)