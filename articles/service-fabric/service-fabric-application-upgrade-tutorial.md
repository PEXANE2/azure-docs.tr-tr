---
title: Hizmet Kumaş uygulaması yükseltme öğretici
description: Bu makalede, bir Hizmet Kumaş uygulaması dağıtma deneyimi üzerinden yürür, kodu değiştirme, ve Visual Studio kullanarak bir yükseltme yayılıyor.
ms.topic: conceptual
ms.date: 2/23/2018
ms.openlocfilehash: db814b972db1aee56be0858c9ff5d1c382640642
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75464832"
---
# <a name="service-fabric-application-upgrade-tutorial-using-visual-studio"></a>Visual Studio kullanarak Hizmet Kumaş uygulama yükseltme öğretici
> [!div class="op_single_selector"]
> * [Powershell](service-fabric-application-upgrade-tutorial-powershell.md)
> * [Visual Studio](service-fabric-application-upgrade-tutorial.md)
> 
> 

<br/>

Azure Hizmet Kumaşı, yalnızca değiştirilen hizmetlerin yükseltilmesini ve uygulama durumunun yükseltme işlemi boyunca izlenmesini sağlayarak bulut uygulamalarını yükseltme işlemini basitleştirir. Ayrıca, sorunlarla karşılaştığında uygulamayı otomatik olarak önceki sürüme geri döndürer. Hizmet Kumaş uygulama yükseltmeleri *Sıfır Kesinti*vardır, uygulama hiçbir kesinti ile yükseltilebilir beri. Bu öğretici Visual Studio bir haddeleme yükseltme tamamlamak için nasıl kapsar.

## <a name="step-1-build-and-publish-the-visual-objects-sample"></a>Adım 1: Görsel Nesneler örneğini oluşturma ve yayımlama
İlk olarak, [Görsel Nesneler](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Actors/VisualObjects) uygulamasını GitHub'dan indirin. Ardından, uygulama projesi **VisualObjects'e**sağ tıklayarak ve Hizmet Dokusu menü öğesinde **Yayımla** komutunu seçerek uygulamayı oluşturun ve yayımlayın.

![Hizmet Kumaşı uygulaması için bağlam menüsü][image1]

**Publish'i** seçmek bir açılır pencere yi getirir ve **Hedef profili** **PublishProfiles\Local.xml**olarak ayarlayabilirsiniz. **Yayımla'yı**tıklatmadan önce pencere aşağıdaki gibi görünmelidir.

![Hizmet Kumaşı uygulaması yayınlama][image2]

Artık iletişim kutusunda **Yayımla'yı** tıklatabilirsiniz. [Kümeyi ve uygulamayı görüntülemek için Service Fabric Explorer'ı](service-fabric-visualizing-your-cluster.md)kullanabilirsiniz. Görsel Nesneler uygulaması, tarayıcınızın adres çubuğuna yazarak [http://localhost:8081/visualobjects/](http://localhost:8081/visualobjects/) gidebileceğiniz bir web hizmetine sahiptir.  Ekranda hareket eden 10 yüzen görsel nesne görmeniz gerekir.

**NOT:** `Cloud.xml` Profile (Azure Hizmet Kumaşı) dağıtılıyorsanız, uygulama daha sonra **http://{ServiceFabricName}.{ adresinde n Bölge}.cloudapp.azure.com:8081/visualobjects/**. Yük Dengeleyicisi'nde `8081/TCP` yapılandırdığınızdan emin olun (Yük Dengeleyicisini Servis Kumaşı örneğiyle aynı kaynak grubunda bulun).

## <a name="step-2-update-the-visual-objects-sample"></a>Adım 2: Görsel Nesneler örneğini güncelleştirme
Adım 1'de dağıtılan sürümle görsel nesnelerin dönmediğini fark edebilirsiniz. Bu uygulamayı görsel nesnelerin de döndüğü bir uygulamaya yükseltelim.

VisualObjects.ActorService projesini VisualObjects çözümü nde seçin ve **VisualObjectActor.cs** dosyasını açın. Bu dosya içinde, yönteme `MoveObject`gidin `visualObject.Move(false)`, yorum `visualObject.Move(true)`dışarı , ve yorum yapmayın . Bu kod değişikliği, hizmet yükseltildikten sonra nesneleri döndürür.  Şimdi değiştirilmiş projeleri **oluşturan çözümü oluşturabilirsiniz (yeniden oluşturamaz).** *Tümünü Yeniden Oluştur'u*seçerseniz, tüm projelerin sürümlerini güncelleştirmeniz gerekir.

Ayrıca uygulamamızı da yorumlamalıyız. **VisualObjects** projesine sağ tıkladıktan sonra sürüm değişikliklerini yapmak için Visual Studio **Edit Manifest Sürümleri** seçeneğini kullanabilirsiniz. Bu seçeneğin seçilmesi, sürüm sürümleri için iletişim kutusunu aşağıdaki gibi getirir:

![Sürüm iletişim kutusu][image3]

Değiştirilen projelerin sürümlerini ve kod paketlerini uygulamayla birlikte sürüm 2.0.0'a güncelleştirin. Değişiklikler yapıldıktan sonra, bildirim aşağıdaki gibi görünmelidir (kalın bölümler değişiklikleri gösterir):

![Sürümleri güncelleştirme][image4]

Visual Studio **araçları, uygulama ve hizmet sürümlerini otomatik olarak güncelleştirin**seçeneğini seçerek otomatik sürüm toplamalarını yapabilirsiniz. [SemVer](http://www.semver.org)kullanıyorsanız, bu seçenek seçilirse kodu ve/veya yapılandırma paketi sürümünü tek başına güncelleştirmeniz gerekir.

Değişiklikleri kaydedin ve şimdi Uygulamayı Yükselt kutusunu **işaretleyin.**

## <a name="step-3--upgrade-your-application"></a>Adım 3: Başvurunuzu yükseltin
Uygulanabilecek çeşitli yükseltme parametreleri, zaman-out'lar ve sistem durumu ölçütlerini iyi anlamak için [uygulama yükseltme parametrelerini](service-fabric-application-upgrade-parameters.md) ve [yükseltme işlemini](service-fabric-application-upgrade.md) öğrenin. Bu izlenecek yol için, hizmet durumu değerlendirme ölçütü varsayılan (izlenmeyen mod) olarak ayarlanır. **Yükseltme Ayarlarını Yapılandır'ı** seçip parametreleri istediğiniz gibi değiştirerek bu ayarları yapılandırabilirsiniz.

Şimdi hepimiz Yayınla'yı seçerek uygulama **Publish**yükseltmesini başlatmak için ayarlanmışız. Bu seçenek, uygulamanızı nesnelerin döndüğü sürüm 2.0.0'a yükseltir. Hizmet Kumaşı, aynı anda bir güncelleştirme etki alanını yükseltir (önce bazı nesneler güncelleştirilir, ardından diğerleri tarafından takip edilir) ve yükseltme sırasında hizmete erişilemez. Hizmete erişim istemciniz (tarayıcı) üzerinden kontrol edilebilir.  

Şimdi, uygulama yükseltme devam ettikçe, uygulamalar altında **İlerleme Sekmesini** kullanarak Service Fabric Explorer ile izleyebilirsiniz.

Birkaç dakika içinde, tüm güncelleştirme etki alanları yükseltilmeli (tamamlanmalıdır) ve Visual Studio çıktı penceresi de yükseltmenin tamamlandığını belirtmelidir. Ve tarayıcı pencerenizdeki *tüm* görsel nesnelerin artık dönüyor olduğunu bulmak gerekir!

Sürümleri değiştirmeyi ve sürüm 2.0.0'dan sürüm 3.0.0'a alıştırma olarak, hatta sürüm 2.0.0'dan sürüm 1.0.0'a geri dönmeyi denemek isteyebilirsiniz. Zaman zaman dilimleri ve sağlık politikaları ile kendinizi onları tanımak için oynayın. Yerel bir kümenin aksine bir Azure kümesine dağıtılırken, kullanılan parametrelerin farklı olması gerekebilir. Zaman zamanlarını konservatif olarak ayarlamanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar
[PowerShell kullanarak uygulamanızı yükseltmek, PowerShell'i](service-fabric-application-upgrade-tutorial-powershell.md) kullanarak bir uygulama yükseltmesi ile size yol alar.

[Yükseltme parametreleri](service-fabric-application-upgrade-parameters.md)kullanarak uygulamanızın nasıl yükseltilmelerini denetle.

[Veri serileştirmenin](service-fabric-application-upgrade-data-serialization.md)nasıl kullanılacağını öğrenerek uygulama yükseltmelerinizi uyumlu hale getirin.

[Gelişmiş konulara](service-fabric-application-upgrade-advanced.md)atıfta bulunarak uygulamanızı yükseltirken gelişmiş işlevselliği nasıl kullanacağınızı öğrenin.

[Sorun Giderme uygulama yükseltmeleri](service-fabric-application-upgrade-troubleshooting.md)adımlarına atıfta bulunarak uygulama yükseltmelerinde sık karşılaşılan sorunları giderin.

[image1]: media/service-fabric-application-upgrade-tutorial/upgrade7.png
[image2]: media/service-fabric-application-upgrade-tutorial/upgrade1.png
[image3]: media/service-fabric-application-upgrade-tutorial/upgrade5.png
[image4]: media/service-fabric-application-upgrade-tutorial/upgrade6.png
