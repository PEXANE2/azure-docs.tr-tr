---
title: Dron görüntülerini alma
description: Bu makalede, ortaklardan drone görüntüleri nasıl alınır açıklanmaktadır.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 3e452cd548738e5f211899d3a6a676f883d800ce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77132044"
---
# <a name="get-drone-imagery-from-drone-partners"></a>Drone ortaklarından drone görüntüleri alın

Bu makalede, drone görüntü ortaklarınızdan Ortomoz verilerini Azure FarmBeats Datahub'a nasıl getirebileceğiniz açıklanmaktadır. Ortomoz, bir insansız hava aracı tarafından toplanan verilerden geometrik olarak düzeltilmiş ve dikilmiş bir hava illüstrasyonu veya görüntüsüdür.

Şu anda, aşağıdaki görüntü ortakları desteklenir.

  ![FarmBeats drone görüntü ortakları](./media/get-drone-imagery-from-drone-partner/drone-partner-1.png)

Drone görüntü verilerini Azure FarmBeats ile tümleştirmek, çiftliğinizde yürüttüğünüz drone uçuşlarından ortomoz verilerini datahub'a ulaştırmanıza yardımcı olur. Veriler kullanılabilir olduktan sonra FarmBeats Hızlandırıcı'da görüntüleyebilirsiniz. Veriler veri füzyonu, yapay zeka ve makine öğrenimi modeli yapımında kullanılabilir.

## <a name="before-you-begin"></a>Başlamadan önce

  - Azure FarmBeats'i yüklediğinizden emin olun. FarmBeats'in nasıl yüklenir hakkında daha fazla bilgi için [Azure FarmBeats'i yükle'ye](install-azure-farmbeats.md)bakın.
  - FarmBeats sisteminizde tanımlanan drone görüntülerinin olmasını istediğiniz bir çiftliğe sahip olduğundan emin olun.

## <a name="enable-drone-imagery-integration-with-farmbeats"></a>FarmBeats ile drone görüntüleri entegrasyonuna olanak sağlama

FarmBeats ile tümleştirmeyi etkinleştirmek için aygıt sağlayıcınıza aşağıdaki bilgileri sağlayın:
 - API uç noktası
 - Kiracı Kimliği
 - İstemci Kimliği
 - Gizli anahtar

Şu adımları izleyin.

1. Bu [komut dosyasını](https://aka.ms/farmbeatspartnerscript)indirin ve yerel sürücünüze ayıklayın. Zip dosyasının içinde iki dosya var.
2. [Azure portalında](https://portal.azure.com/) oturum açın ve Azure Cloud Shell'i açın. Bu seçenek, portalın sağ üst köşesindeki araç çubuğunda mevcuttur.

    ![Portalın sağ üst çubuğunda Azure Bulut Kabuğu'nu açın](./media/get-drone-imagery-from-drone-partner/navigation-bar-1.png)

3. Ortamın **PowerShell**olarak ayarlandıklarına emin olun.

    ![PowerShell ayarı](./media/get-drone-imagery-from-drone-partner/power-shell-new-1.png)

4. Bulut Kabuğu örneğinize adım 1'den indirdiğiniz iki dosyayı yükleyin.

    ![Dosyaları karşıya yükleme](./media/get-drone-imagery-from-drone-partner/power-shell-two-1.png)

5. Dosyaların yüklendiği dizine gidin. Varsayılan olarak, kullanıcı adı altında ev dizinine yüklenirler.
6. Şu betiği çalıştırın:

    ```azurepowershell-interactive 

    ./generateCredentials.ps1   

    ```

7. API Endpoint, Kiracı Kimliği, İstemci Kimliği, İstemci Sırrı ve EventHub Bağlantı Dizesi değerlerini yakalamak için ekrandaki yönergeleri izleyin.

    Ortağın drone yazılım sistemine gerekli kimlik bilgilerini girdikten sonra, FarmBeats sisteminden tüm çiftlikleri içe aktarabilirsiniz. Daha sonra uçuş yolu planlama ve drone görüntü toplama yapmak için çiftlik detayları kullanabilirsiniz.

    Ham görüntüler drone sağlayıcılarının yazılımı tarafından işlendikten sonra, drone yazılım sistemi dikişli ortomoziği ve diğer işlenmiş görüntüleri datahub'a yükler.

## <a name="view-drone-imagery"></a>Drone görüntülerini görüntüleyin

Veriler FarmBeats datahub'ına gönderildikten sonra, FarmBeats Datahub API'lerini kullanarak Sahne Deposu'nu sorgulayabilirsiniz.

Alternatif olarak, **Farm Details** sayfasındaen son drone görüntüsünü görüntüleyebilirsiniz. Görüntüyü görüntülemek için adımları izleyin.

1. Görüntünüzün yüklendiği çiftliği seçin. **Çiftlik** ayrıntıları sayfası görüntülenir.
2. En son **Hassas Haritalar** bölümüne gidin.
3. **Görüntüyü Drone Görüntüleri** bölümünde görüntüleyin.

    ![Drone Görüntüleri bölümü](./media/get-drone-imagery-from-drone-partner/drone-imagery-1.png)

## <a name="download-drone-imagery"></a>Drone görüntülerini indirin

Drone Görüntüleri bölümünü seçtiğinizde, drone ortomoziğinin yüksek çözünürlüklü görüntüsünü göstermek için bir açılır pencere açılır.

![Yüksek çözünürlüklü ortomozomozaik](./media/get-drone-imagery-from-drone-partner/download-drone-imagery-1.png)

## <a name="view-all-drone-maps"></a>Tüm drone haritalarını görüntüle

Drone sağlayıcısı tarafından yüklenen dosya ve görüntüler **Haritalar** bölümünde görünür. **Haritalar** bölümünü seçin, **Farm'a**göre filtre uygulayın ve görüntülemek ve indirmek için uygun dosyaları seçin.

  ![Haritalar bölümü](./media/get-drone-imagery-from-drone-partner/view-drone-maps-1.png)

## <a name="next-steps"></a>Sonraki adımlar

FarmBeats Datahub [API'lerini](rest-api-in-azure-farmbeats.md) kullanarak drone görüntülerinizi nasıl kullanacağınızı öğrenin.
