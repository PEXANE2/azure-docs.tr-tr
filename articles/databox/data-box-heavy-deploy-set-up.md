---
title: Azure Data Box Heavy ayarlama öğreticisi | Microsoft Docs
description: Azure Data Box Heavy kablosunu ve bağlamayı öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 08/29/2019
ms.author: alkohli
ms.openlocfilehash: b6b353e0e01f3f598048e5fbb2682603045b1037
ms.sourcegitcommit: 19a821fc95da830437873d9d8e6626ffc5e0e9d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70164455"
---
::: zone target = "docs"

# <a name="tutorial-cable-and-connect-to-your-azure-data-box-heavy"></a>Öğretici: Kablo ve Azure Data Box Heavy bağlanma

::: zone-end

::: zone target = "chromeless"

## <a name="get-started-with-azure-data-box-heavy"></a>Azure Data Box Heavy kullanmaya başlayın

::: zone-end

::: zone target = "docs"

Bu öğreticide, Azure Data Box Heavy kablo oluşturma, bağlanma ve açma işlemlerinin nasıl yapılacağı açıklanmaktadır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Data Box Heavy kablosu
> * Data Box Heavy bağlanma

::: zone-end

::: zone target = "chromeless"

Bu kılavuzda önkoşulları İnceleme, kablo bağlama ve cihazınızı bağlama, verileri kopyalama, Azure 'a yükleme ve ardından karşıya yüklenen verileri doğrulama hakkında yönergeler sunulmaktadır.

::: zone-end

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakilerden emin olun:

1. [Öğreticiyi tamamladınız: Order Azure Data Box Heavy](data-box-heavy-deploy-ordered.md).
2. Data Box Heavy aldınız ve portaldaki sipariş durumu **teslim edildi**.
3. [Data Box Heavy güvenlik yönergelerini](data-box-safety.md)gözden geçirdiniz.
4. Bu parmak izine sahip bir cihaza uyum sağlayan kullanılabilir bir ağ bağlantısına yakınlık ile veri merkezindeki bir düz siteye erişiminizin olması gerekir. Bu cihaz bir rafa bağlanamaz.
5. Depolama cihazımız ile kullanabileceğiniz dört adet topraklanmış güç kablosu aldınız.
6. Veri merkezi ağına bağlı bir konak bilgisayarınız olmalıdır. Data Box Heavy, verileri bu bilgisayardan kopyalayacaktır. Ana bilgisayarınızın [desteklenen bir işletim sistemi](data-box-heavy-system-requirements.md)çalıştırması gerekir.
7. Veri merkezinizin yüksek hızlı ağı olmalıdır. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 
8. Yerel Kullanıcı arabirimine bağlanmak ve cihazı yapılandırmak için, RJ-45 kablosu olan bir dizüstü bilgisayarınız olması gerekir. Cihazın her düğümünü bir kez yapılandırmak için dizüstü bilgisayarı kullanın.
9. Cihaz düğümü başına 1 40 Gbps kablo veya 10 Gbps kablo gerekir.
    - [MELLANOX MCX314A-BCCT](https://store.mellanox.com/products/mellanox-mcx314a-bcct-connectx-3-pro-en-network-interface-card-40-56gbe-dual-port-qsfp-pcie3-0-x8-8gt-s-rohs-r6.html) ağ arabirimiyle uyumlu kabloları seçin.
    - 40 Gbps kablo için, kablonun cihaz sonunda QSFP + olması gerekir.
    - 10 GB/sn kablo için, cihaza takılan uçta bir QSFP + ila SFP + bağdaştırıcısı (veya QSA bağdaştırıcısı) ile bir uçta 10 Gbps bir anahtara takılan bir SFP + kablonuz olması gerekir.

::: zone target = "docs"

## <a name="cable-your-device-for-power"></a>Cihazınızı güç kablosu ile bağlayın

Cihazınızı kablolu yapmak için aşağıdaki adımları uygulayın.

1. Üzerinde oynanıp oynanmadığını veya başka herhangi bir hasarı olup olmadığını anlamak için cihazı inceleyin. Cihaz kurcalanmış ya da ciddi hasar görmüşse devam etmeyin. Cihazın iyi çalışma sırasında ve size bir değiştirme sunmaları gerekip gerekmediğini değerlendirmenize yardımcı olmak için hemen [Microsoft desteği başvurun](data-box-disk-contact-microsoft-support.md) .
2. Cihazı yükleme sitesine taşıyın.

    ![Data Box Heavy cihaz yükleme sitesi](media/data-box-heavy-deploy-set-up/data-box-heavy-install-site.png)

3. Cihazdaki arka leri aşağıda gösterildiği gibi kilitleyin.

    ![Data Box Heavy cihaz küçük harfleri kilitlendi](media/data-box-heavy-deploy-set-up/data-box-heavy-casters-locked.png)

4. Cihazın ön ve arka kapıların kilidini açmak için kullanılan KBS 'leri bulun. Cihazın yanında boşalıncaya kadar ön kapıyı açın ve taşıyın. Bunu arka kapıyı de yineleyin.
    Cihaz üzerinden en iyi önden arkaya uçak akışına izin vermek için, cihazın her ikisi de açık durumda kalır.

    ![Data Box Heavy kapıları açık](media/data-box-heavy-deploy-set-up/data-box-heavy-doors-open.png)

5. Cihazın arkasında bulunan tepside dört güç kablosu olmalıdır. Tepsideki tüm kabloları kaldırın ve bunları bir kenara koyun.

    ![Tepsiye Data Box Heavy Power kablosu sayısı](media/data-box-heavy-deploy-set-up/data-box-heavy-power-cords-tray.png)

6. Sonraki adım, cihazın arkasında çeşitli bağlantı noktalarını belirlemektir. **Düğüm1** ve **Düğüm2**olmak üzere iki cihaz düğümü vardır. Her düğüm dört ağ arabirimine sahiptir, **MGMT**, **Veri1**, **Veri2**, **DATA3**. **MGMT** , cihazın ilk yapılandırması sırasında yönetimi yapılandırmak için kullanılır. Veri1-**DATA3** , veri bağlantı noktalardır. **MGMT** ve **DATA3** bağlantı noktaları 1 Gbps, ancak **Veri1**, **Veri2** 40 Gbps bağlantı noktası veya 10 Gbps bağlantı noktası olarak çalışabilir. İki cihaz düğümünün en altında, iki cihaz düğümü arasında paylaşılan dört güç kaynağı birimi (PSUs) vardır. Bu cihazı hissetme sırasında **Psus** , **PSU1**, **PSU2**, **PSU3**ve **PSU4** soldan sağa doğru.

    ![Data Box Heavy bağlantı noktaları](media/data-box-heavy-deploy-set-up/data-box-heavy-ports.png)

7. Dört güç kablosu tamamını cihaz güç kaynakları ' na bağlayın. Yeşil LED 'ler açıp yanıp söndürme.
8. Cihaz düğümlerini açmak için ön düzlemdeki güç düğmelerini kullanın. Mavi ışıklar açık olana kadar güç düğmesini birkaç saniye boyunca basılı tutun. Cihazın arkasında bulunan güç kaynaklarının tüm yeşil LED 'Leri artık kesintisiz olmalıdır. Cihazın ön işletim paneli ayrıca hata LED 'Leri içerir. Hata ışığı aydınlatılmış olduğunda, hatalı bir PSU veya bir fanı ya da disk sürücüleriyle ilgili bir sorun olduğunu gösterir.  

    ![Data Box Heavy ön Ops paneli](media/data-box-heavy-deploy-set-up/data-box-heavy-front-ops-panel.png)

## <a name="cable-first-node-for-network"></a>Ağ için kablolu ilk düğüm

Cihazın düğümlerinden birinde, ağ kablosu için aşağıdaki adımları uygulayın.

1. Ana bilgisayarı 1 GB/sn yönetim bağlantı noktasına bağlamak için bir CAT 6 RJ-45 ağ kablosu (resimde mavi kablo) kullanın.
2. Veriler için en az 1 40 Gbps (1 GB/sn 'tan tercih edilen) ağ arabirimine bağlanmak için bir QSFP + kablosu (fiber veya bakır) kullanın. 10 GB/sn 'lik bir anahtar kullanılıyorsa, veriler için 40 Gbps ağ arabirimine bağlanmak üzere QSFP + ila SFP + bağdaştırıcısı (QSA bağdaştırıcısı) ile bir SFP + kablosu kullanın.

    ![Data Box Heavy bağlantı noktaları kablolu](media/data-box-heavy-deploy-set-up/data-box-heavy-ports-cabled.png)

    > [!IMPORTANT]
    > VERI 1 ve VERI2, yerel Web Kullanıcı arabiriminde görüntülenenlerin üzerinde geçiş yaptı ve bu şekilde eşleşmez.
    > 40 Gbps kablo bağdaştırıcısı aşağıda gösterildiği gibi takıldığında bağlanır.

    ![Data Box Heavy 40-Gbps kablo adaptörü](media/data-box-heavy-deploy-set-up/data-box-heavy-cable-adaptor.png)

## <a name="configure-first-node"></a>İlk düğümü Yapılandır

Yerel yapılandırmayı ve Azure portal kullanarak cihazınızı ayarlamak için aşağıdaki adımları uygulayın.

1. Portaldan cihaz kimlik bilgilerini indirin. **Genel > Cihaz ayrıntıları**’na gidin. **Cihaz parolası**’nı kopyalayın. Bu parolalar portalda belirli bir sıraya bağlıdır. Data Box Heavy iki düğüme karşılık gelen iki cihaz seri numarasını görürsünüz. Her iki düğüm için de cihaz yöneticisi parolası aynıdır.

    ![Data Box Heavy cihaz kimlik bilgileri](media/data-box-heavy-deploy-set-up/data-box-heavy-device-credentials.png)

2. İstemci iş istasyonunuzu cihaza CAT6 RJ-45 ağ kablosu aracılığıyla bağlayın.
3. Bir statik IP adresi `192.168.100.5` ve alt ağ `255.255.255.0`ile cihaza bağlanmak için kullandığınız bilgisayardaki Ethernet bağdaştırıcısını yapılandırın.

    ![Data Box Heavy yerel Web Kullanıcı arabirimine bağlanır](media/data-box-heavy-deploy-set-up/data-box-heavy-connect-local-web-ui.png)

4. Şu URL 'de cihazın yerel Web Kullanıcı arabirimine bağlanın: `http://192.168.100.10`. **Gelişmiş** ' e tıklayın ve ardından **192.168.100.10 (güvenli olmayan) öğesine ilerle**düğmesine tıklayın.
5. Yerel web kullanıcı arabiriminin **Oturum açma** sayfasını görürsünüz.
    
    - Bu sayfadaki düğüm seri numaralarının biri hem Portal Kullanıcı arabirimi hem de yerel Web Kullanıcı arabirimi genelinde eşleşiyor. Seri numarası eşlemesinin düğüm numarasını bir yere getirin. Portalda iki düğüm ve iki cihaz seri numarası vardır. Bu eşleme hangi düğümün hangi seri numarasına karşılık geldiğini anlamanıza yardımcı olur.
    - Bu noktada cihaz kilitlidir.
    - Cihazda oturum açmak için önceki adımda edindiğiniz Cihaz Yöneticisi parolasını belirtin. **Oturum aç**’a tıklayın.

    ![Yerel Web Kullanıcı arabiriminde Data Box Heavy oturum açın](media/data-box-heavy-deploy-set-up/data-box-heavy-unlock-device.png)

5. Panoda, ağ arabirimlerinin yapılandırıldığından emin olun. Cihaz düğümünüz, iki 1 Gbps ve 2 40 Gbps olmak üzere dört ağ arabirimi vardır. 1 Gbps arabiriminden biri yönetim arabirimidir ve bu nedenle Kullanıcı tarafından yapılandırılabilir değildir. Kalan üç ağ arabirimi verilere ayrılmıştır ve Kullanıcı tarafından yapılandırılabilir.

- Ortamınızda DHCP etkinse, ağ arabirimleri otomatik olarak yapılandırılır.
- DHCP etkinleştirilmemişse, ağ arabirimlerini ayarla ' ya gidin ve gerekirse statik IP 'Ler atayın.

    ![Data Box Heavy Pano düğümü 1](media/data-box-heavy-deploy-set-up/data-box-heavy-dashboard-1.png)

## <a name="configure-second-node"></a>İkinci düğümü Yapılandır

Cihazın ikinci düğümü için [ilk düğümü yapılandırma](#configure-first-node) bölümünde açıklanan adımları uygulayın.

![Data Box Heavy Pano düğümü 2](media/data-box-heavy-deploy-set-up/data-box-heavy-dashboard-2.png)

Cihaz kurulumu tamamlandıktan sonra cihaz paylaşımlarına bağlanabilir ve verileri bilgisayarınızdan cihaza kopyalayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şöyle Azure Data Box Heavy konular hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Data Box Heavy kablosu
> * Data Box Heavy bağlanma

Data Box Heavy verileri nasıl kopyalayacağınızı öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Azure Data Box'a verilerinizi kopyalama](./data-box-heavy-deploy-copy-data.md)

::: zone-end
