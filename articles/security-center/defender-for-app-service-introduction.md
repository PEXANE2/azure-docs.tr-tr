---
title: App Service için Azure Defender-avantajlar ve Özellikler
description: App Service için Azure Defender 'ın özellikleri ve aboneliğiniz üzerinde nasıl etkinleştirileceği hakkında bilgi edinin
author: memildin
ms.author: memildin
ms.date: 01/25/2021
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 200e1fd7bfffef403fa459d3de13dc31145b8a33
ms.sourcegitcommit: 95c2cbdd2582fa81d0bfe55edd32778ed31e0fe8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98796626"
---
# <a name="introduction-to-azure-defender-for-app-service"></a>App Service için Azure Defender 'a giriş

Azure App Service, Web uygulamalarınızı ve API 'lerinizi oluşturmak ve barındırmak için tam olarak yönetilen bir platformdur. Platform tam olarak yönetildiğinden altyapı konusunda endişelenmenize gerek kalmaz. Kurumsal düzeyde performans, güvenlik ve uyumluluk gereksinimlerini karşılamak için yönetim, izleme ve operasyonel içgörüler sağlar. Daha fazla bilgi için bkz. [Azure App Service](https://azure.microsoft.com/services/app-service/).

**App Service Için Azure Defender** , App Service üzerinde çalışan uygulamaları hedefleyen saldırıları belirlemek için bulutun ölçeğini kullanır. Saldırganlar, zayıf yanları bulmak ve yararlanmak için Web uygulamalarını araştırma. Belirli ortamlara yönlendirilmeden önce, Azure 'da çalışan uygulamalara yönelik istekler, incelendikleri ve günlüğe kaydedildiği çeşitli ağ geçitleri aracılığıyla yapılır. Bu veriler daha sonra kötüye kullanım ve saldırganlar tanımlamak ve daha sonra kullanılacak yeni desenler öğrenmek için kullanılır.


## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Genel olarak kullanılabilir (GA)|
|Fiyat|[App Service Için Azure Defender](azure-defender.md) , [fiyatlandırma sayfasında](security-center-pricing.md) gösterildiği gibi faturalandırılır<br>Fiyatlandırma ve ayarlar sayfası, **kaynak miktarlarınızın** örnek sayısını listeler. Bu sayı, Bu abonelikteki tüm App Service planlarında, Fiyatlandırma Katmanı sayfasını açtığınız anda çalışan toplam işlem örneği sayısıdır.<br>Sayıyı doğrulamak için, Azure portal **App Service planlarını** açın ve her plan tarafından kullanılan işlem örneği sayısını denetleyin.|
|Desteklenen App Service planları:|![Evet ](./media/icons/yes-icon.png) temel, standart, Premium, yalıtılmış veya Linux<br>![](./media/icons/no-icon.png)Ücretsiz, paylaşılan veya tüketim yok<br>[App Service planları hakkında daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/app-service/plans/)|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![Hayır](./media/icons/no-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov)|
|||

## <a name="what-are-the-benefits-of-azure-defender-for-app-service"></a>App Service için Azure Defender 'ın avantajları nelerdir?

App Service için Azure Defender 'ı etkinleştirdiğinizde, bu Azure Defender planı tarafından sunulan aşağıdaki hizmetlerden hemen yararlanabilirsiniz:

- **Güvenli** güvenlik merkezi, App Service planınız kapsamındaki kaynakları değerlendirir ve bulguları temelinde güvenlik önerileri oluşturur. App Service kaynaklarınızın güvenliğini sağlamak için bu önerilerle ilgili ayrıntılı yönergeleri kullanın.

- **Algıla** -Azure Defender, aşağıdakileri izleyerek App Service kaynaklarınız için çok sayıda tehdit algılar:
    - App Service çalıştığı VM örneği ve yönetim arabirimi
    - App Service uygulamalarınıza gönderilen istekler ve yanıtlar
    - temel alınan sanal alanlar ve VM 'Ler
    - App Service iç Günlükler-Azure 'un bulut sağlayıcısı olarak sahip olduğu görünürlük sayesinde kullanılabilir

Azure Defender, bulutta yerel bir çözüm olarak birden çok hedefe uygulanan saldırı yöntemlerini tanımlayabilir. Örneğin, tek bir konaktan, küçük bir IP alt kümesinden dağıtılmış bir saldırının belirlenmesi, birden fazla konaktaki benzer uç noktalara gezinilmesi zor olabilir.

Günlük verileri ve altyapıda bir arada, hikayeye, Müşteri makinelerinden muaf olmak üzere yeni bir saldırıya karşı çok daha fazla bilgi verebilir. Bu nedenle, güvenlik merkezi bir Web uygulaması kullanıldıktan sonra dağıtılsa bile, devam eden saldırıları tespit edebilir.


## <a name="what-threats-can-azure-defender-for-app-service-detect"></a>App Service algılaması için Azure Defender hangi tehditleri kullanabilir?

### <a name="threats-by-mitre-attck-tactics"></a>MITRE ATT&CK tactika tarafından tehditler

Azure Defender, App Service kaynaklarınızda birçok tehdit izler. Uyarılar, COMYA ATT&CK 'in ön saldırısından komuta ve denetime neredeyse tam listesini kapsar. Azure Defender şunları algılayabilir:

- **Saldırı öncesi tehditler** -Defender, saldırganların uygulamaları daha zayıf bir şekilde araştırmasını sağlamak için sıklıkla kullandığı birden çok güvenlik açığı tarayıcısı türünün yürütülmesini algılayabilir.

- **İlk erişim tehditleri**  -  [Microsoft Threat Intelligence](https://go.microsoft.com/fwlink/?linkid=2128684) , bilinen bir kötü amaçlı ıp adresı Azure App Service FTP arabiriminize bağlandığı zaman uyarı tetiklemesini içeren bu uyarıları güçlendirir.

- **Yürütme tehditleri** -Defender, yüksek ayrıcalıklı komutları çalıştırma girişimlerini, bir Windows App Service Linux komutlarını, dosya daha az saldırı davranışını, dijital para birimi araştırma araçlarını ve diğer birçok şüpheli ve kötü amaçlı kod yürütme etkinliklerini algılayabilir.

### <a name="dangling-dns-detection"></a>DNS algılamasını çözme

App Service için Azure Defender, App Service bir Web sitesi kullanımdan çalıştırıldığında DNS kaydedicinizdeki tüm DNS girdilerini de tanımlar. Bunlar, salgze DNS girişleri olarak bilinir. Bir Web sitesini kaldırdığınızda ve özel etki alanını DNS kaydedicinizden kaldırdığınızda, DNS girişi var olmayan bir kaynağa işaret eder ve alt etki alanınız bir yük devri ile savunmasızdır. Azure Defender, *mevcut* salgze DNS GIRIŞLERI için DNS kaydedicinizi taramaz; bir App Service Web sitesi kullanımdan çalıştırıldığında ve özel etki alanı (DNS girdisi) silinmediğinde sizi uyarır.

Alt etki alanı kuruluşları, kuruluşlar için ortak ve yüksek öneme sahip bir tehdittir. Bir tehdit aktör bir tehlike DNS girişi algıladığında, hedef adreste kendi sitesini oluşturur. Kuruluşun etki alanı için tasarlanan trafik daha sonra tehdit aktörün sitesine yönlendirilir ve bu trafiği çok çeşitli kötü amaçlı etkinlikler için kullanabilir.

Etki alanlarınızın Azure DNS veya dış etki alanı kaydedicisi ile yönetilip yönetilmediği ve hem Windows hem de Linux 'ta App Service için geçerli olup olmadığı, tehlike altında olan DNS koruması kullanılabilir.

:::image type="content" source="media/defender-for-app-service-introduction/dangling-dns-alert.png" alt-text="Keşfedilen bir DNS girişi hakkında bir Azure Defender uyarısı örneği. App Service için Azure Defender 'ı, ortamınız için bu ve diğer uyarıları alacak şekilde etkinleştirin." lightbox="media/defender-for-app-service-introduction/dangling-dns-alert.png":::

[DNS girişlerinin tehlikelere engel olma ve alt etki alanının devrini önleme](../security/fundamentals/subdomain-takeover.md)bölümünde, etki alanı için DNS ve alt etki alanı tehditleri tehditleri hakkında daha fazla bilgi edinin.

Azure App Service uyarılarının tam listesi için bkz. [uyarıların başvuru tablosu](alerts-reference.md#alerts-azureappserv).

> [!NOTE]
> Özel etki alanınız doğrudan bir App Service kaynağına işaret edemediğinde veya Defender, saldırgan DNS koruması etkinleştirildiğinden (özel etki alanını tanımlamak için Günlükler olmadığı için), Defender, sitelin DNS uyarılarını tetiklemeyebilir.

## <a name="how-to-protect-your-azure-app-service-web-apps-and-apis"></a>Azure App Service Web uygulamalarınızı ve API 'lerinizi koruma

Azure App Service planınızı App Service Azure Defender ile korumak için:

1. Adanmış makinelerle ilişkili desteklenen bir App Service planına sahip olduğunuzdan emin olun. Desteklenen planlar, yukarıdaki [kullanılabilirlik](#availability)bölümünde listelenmiştir.

2. Azure [Güvenlik Merkezi fiyatlandırması](security-center-pricing.md)bölümünde açıklandığı gibi aboneliğinizde **Azure Defender 'ı** etkinleştirin.

    İsteğe bağlı olarak Azure Defender 'da (App Service için Azure Defender gibi) tek tek planları etkinleştirebilirsiniz.

    Güvenlik Merkezi, App Service ile yerel olarak tümleşiktir ve dağıtım ve ekleme gereksinimini ortadan kaldırır; tümleştirme saydamdır.


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, App Service için Azure Defender hakkında bilgi edindiniz. 

İlgili malzemeler için aşağıdaki makalelere bakın: 

- Uyarılarınızı Azure Sentinel 'e, herhangi bir üçüncü taraf SıEM 'e veya herhangi bir harici araca aktarmak için, [BIR SıEM, SOAR veya BT hizmet yönetimi çözümüne akış uyarıları](export-to-siem.md)içindeki yönergeleri izleyin.
- Azure Defender App Service uyarıları listesi için, [Uyarı tablosuna](alerts-reference.md#alerts-azureappserv)bakın.
- App Service planları hakkında daha fazla bilgi için bkz. [App Service planları](https://azure.microsoft.com/pricing/details/app-service/plans/).
> [!div class="nextstepaction"]
> [Azure Defender’ı etkinleştirme](security-center-pricing.md#enable-azure-defender)