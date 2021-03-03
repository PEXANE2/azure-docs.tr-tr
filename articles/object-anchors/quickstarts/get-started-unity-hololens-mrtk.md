---
title: 'Hızlı başlangıç: Unity ve MRTK ile bir HoloLens uygulaması oluşturma'
description: Bu hızlı başlangıçta, MRTK ve nesne tutturucularını kullanarak bir HoloLens Unity uygulaması oluşturmayı öğreneceksiniz.
author: craigktreasure
manager: virivera
services: azure-object-anchors
ms.author: crtreasu
ms.date: 02/02/2021
ms.topic: quickstart
ms.service: azure-object-anchors
ms.openlocfilehash: 78a8136c3f66d0c790f6fd7508ca37b55a1541fd
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101748915"
---
# <a name="quickstart-create-a-hololens-app-with-azure-object-anchors-in-unity-with-mrtk"></a>Hızlı başlangıç: Azure nesne tutturucularla, MRTK ile Unity 'de bir HoloLens uygulaması oluşturma

Bu hızlı başlangıçta, [Azure nesne tutturucularını](../overview.md)kullanan bir Unity HoloLens uygulaması oluşturacaksınız. Azure nesne bağlantıları, HoloLens için nesne açısından algılayan karma gerçeklik deneyimlerini etkinleştiren bir yönetilen bulut hizmetidir. İşiniz bittiğinde, fiziksel dünyadaki nesneleri algılayabildiği Unity ile oluşturulmuş bir HoloLens uygulamanız olacaktır.

Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Unity derleme ayarlarını hazırlayın.
> * HoloLens Visual Studio projesini dışarı aktarın.
> * Uygulamayı dağıtın ve bir HoloLens 2 cihazında çalıştırın.

[!INCLUDE [Unity quickstart prerequisites](../../../includes/object-anchors-quickstart-unity-prerequisites.md)]

[!INCLUDE [Unity device setup](../../../includes/object-anchors-quickstart-unity-device-setup.md)]

## <a name="open-the-sample-project"></a>Örnek projeyi açın

[!INCLUDE [Clone Sample Repo](../../../includes/object-anchors-clone-sample-repository.md)]

[!INCLUDE [Download Unity Package](../../../includes/object-anchors-quickstart-unity-download-package.md)]

Unity 'de `quickstarts/apps/unity/mrtk` projeyi açın.

[!INCLUDE [Import Unity Package](../../../includes/object-anchors-quickstart-unity-import-package.md)]

[!INCLUDE [Unity build and run](../../../includes/object-anchors-quickstart-unity-build-run.md)]

[!INCLUDE [Unity build sample scene 1](../../../includes/object-anchors-quickstart-unity-build-sample-scene-1.md)]

"TMP İçeri Aktarıcı" iletişim kutusu, Textkafespro kaynaklarını içeri aktarmanızı isterse, bunu yapmak için "TMP Essentials 'ı Içeri aktar" seçeneğini belirleyin.
:::image type="content" source="./media/textmesh-pro-importer-dialog.png" alt-text="Textkafespro kaynaklarını içeri aktarma":::

[!INCLUDE [Unity build sample scene 2](../../../includes/object-anchors-quickstart-unity-build-sample-scene-2.md)]

[!INCLUDE [Unity build and deploy](../../../includes/object-anchors-quickstart-unity-build-deploy.md)]

### <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

Cihazı açın, **tüm uygulamalar**' ı seçin, ardından uygulamayı bulun ve başlatın. Unity giriş ekranından sonra bir beyaz sınırlayıcı kutusu görmeniz gerekir. Parmaklarınızı, sınırlayıcı kutuyu taşımak, ölçeklendirmek veya döndürmek için kullanabilirsiniz. Bu kutuyu, algılamak istediğiniz nesneyi kapsayacak şekilde yerleştirin.

<a href="https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_HandMenu.html" target="_blank">El menüsünü</a> açın ve sınırlayıcı kutunun daha fazla hareketini engellemek Için **Searcharea 'ı kilitle** ' yi seçin. Nesne algılamayı başlatmak için **Aramayı Başlat** ' ı seçin. Nesne algılandığında, nesne üzerinde bir ağ oluşturulur. Algılanan bir örneğin ayrıntıları ekranda görüntülenir; Örneğin, güncelleştirilmiş zaman damgası ve yüzey kapsamı oranı. İzlemeyi durdurmak için **aramayı durdur** ' u seçin ve algılanan tüm örnekler kaldırılır.

#### <a name="the-app-menus"></a>Uygulama Menüleri

Ayrıca, <a href="https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_HandMenu.html" target="_blank">el menüsünü</a>kullanarak başka işlemler de yapabilirsiniz.

##### <a name="primary-menu"></a>Birincil menü

* **Aramayı Başlat/Durdur** : nesne algılama işlemini başlatır veya durdurur.
* **Uzamsal eşlemeyi değiştirme** – uzamsal eşleme işlemesini göster/gizle. Bu seçenek, tarama tamamlanamazsa hata ayıklamak için kullanılabilir.
* **Izleyici ayarları** – izleyici ayarları menüsünü etkinleştirmeye geçiş yapar.
* **Arama alanı ayarları** – arama alanı ayarları menüsünü etkinleştirme ' ye geçiş yapar.
* **Izlemeyi Başlat** – tanılama verilerini yakala ve cihaza Kaydet. Bölüm **hata ayıklama algılama sorunlarını giderme ve tanılamayı yakalama** bölümünde daha fazla ayrıntı görüntüleyin.
* **Karşıya yükleme izlemesi** – tanılama verilerini Nesne tutturucuları hizmetine yükleyin. Bir kullanıcının içinde abonelik hesabı sağlaması `subscription.json` ve klasöre yüklemesi gerekir `LocalState` . Örnek `subscription.json` dosya aşağıda bulunabilir.

    :::image type="content" source="./media/mrtk-hand-menu-primary.png" alt-text="Unity birincil menüsü":::

##### <a name="tracker-settings-menu"></a>İzleyici Ayarları menüsü

* **Yüksek doğruluk** : daha doğru bir poz sağlamak için kullanılan deneysel bir özellik. Bu seçeneğin etkinleştirilmesi, nesne algılaması sırasında daha fazla sistem kaynağı gerektirir. Nesne kafesi bu modda pembe renkte işlenir. Normal izleme moduna geri dönmek için bu düğmeyi tekrar seçin.
* **Gevşek dikey hizalama** : etkinleştirildiğinde, bir nesnenin dikey olmayan bir açıda algılanmasına izin verir. Kmps 'de nesneleri saptamak için faydalıdır.
* **Ölçek değişikliğine Izin ver** – izleyici, algılanan nesne boyutunu ortam bilgilerine göre değiştirmesine izin verir.
* **Kapsam oranı kaydırıcısı** – bir nesneyi algılamak için izleyici için eşleşmesi gereken yüzey noktalarının oranını ayarlar.  Daha düşük değerler, izleyici 'nin, koyu nesneler veya yüksek oranda yansıtmalı nesneler gibi, HoloLens algılayıcılarının algılanması için zorlayıcı nesneleri daha iyi algılamasına olanak tanır. Daha yüksek değerler, yanlış algılamaların sıklığını azaltır.

    :::image type="content" source="./media/mrtk-hand-menu-tracker.png" alt-text="Unity izleyici el menüsü":::

##### <a name="search-area-settings-menu"></a>Arama alanı ayarları menüsü

* **Arama alanını kilitle** – uygulamalı hareketi engellemek için alan sınırlayıcı kutusunu kilitle.
* **Arama alanını otomatik ayarla** – arama alanının nesne algılama sırasında kendisini yeniden konumlandırmasına izin verir.
* **Döngü ağı** : arama alanı içindeki yüklü kafesleri görselleştirirken geçiş yapar.  Bu seçenek, kullanıcıların arama kutusunu nesneleri algılamaya kadar hizalamasına yardımcı olabilir.

    :::image type="content" source="./media/mrtk-hand-menu-search-area.png" alt-text="Unity arama alanı el menüsü":::

Örnek `subscription.json` :

```json
{
  "AccountId": "<your account id>",
  "AccountKey": "<your account key>",
  "AccountDomain": "<your account domain>"
}
```

[!INCLUDE [Unity setup Windows Device Portal](../../../includes/object-anchors-quickstart-unity-setup-device-portal.md)]

[!INCLUDE [Unity upload your model](../../../includes/object-anchors-quickstart-unity-upload-model.md)]

[!INCLUDE [Unity troubleshooting](../../../includes/object-anchors-quickstart-unity-troubleshooting.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kavramlar: SDK 'ya genel bakış](../concepts/sdk-overview.md)

> [!div class="nextstepaction"]
> [SSS](../faq.md)
