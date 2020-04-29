---
title: Dron görüntülerini alma
description: Bu makalede, iş ortaklarından drone Imagery 'nin nasıl alınacağı açıklanır.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 3e452cd548738e5f211899d3a6a676f883d800ce
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77132044"
---
# <a name="get-drone-imagery-from-drone-partners"></a>Drone iş ortaklarından drone canlandırın al

Bu makalede, drone Imagery iş ortakınızdan Azure Farmrets veri hub 'ına dikmozaik verileri nasıl getirebileceğinizi açıklanmaktadır. Ormozaik, geometrik olarak düzeltilen ve bir drone tarafından toplanan verilerden alınan bir havadan oluşan çizim veya görüntüdür.

Şu anda aşağıdaki Imagery iş ortakları desteklenir.

  ![Farmtts drone canlandırın iş ortakları](./media/get-drone-imagery-from-drone-partner/drone-partner-1.png)

Azure farmile drone canlandırın verilerini tümleştirmek, grubunuzda veri hub 'ında yaptığınız drone fışıklardan diksiz olan verileri almanıza yardımcı olur. Veriler kullanılabilir olduktan sonra, bunu Farmtts Hızlandırıcısı 'nda görüntüleyebilirsiniz. Veriler, veri Fusion ve yapay zeka ve makine öğrenimi model oluşturma için kullanılabilir.

## <a name="before-you-begin"></a>Başlamadan önce

  - Azure Farmtts 'yi yüklediğinizden emin olun. Farmtts 'nin nasıl yükleneceği hakkında bilgi için bkz. [Azure Farmtts 'Yi yüklemek](install-azure-farmbeats.md).
  - Farmtts sisteminizde tanımlanmış drone canlandırın 'yi istediğiniz gruba sahip olduğunuzdan emin olun.

## <a name="enable-drone-imagery-integration-with-farmbeats"></a>Farmıts ile drone Imagery tümleştirmesini etkinleştirme

Farmtempts ile tümleştirmeyi etkinleştirmek için cihaz sağlayıcınıza aşağıdaki bilgileri sağlayın:
 - API uç noktası
 - Kiracı Kimliği
 - İstemci Kimliği
 - Gizli anahtar

Şu adımları izleyin.

1. Bu [betiği](https://aka.ms/farmbeatspartnerscript)indirip yerel sürücünüze ayıklayın. ZIP dosyasının içinde iki dosya bulunur.
2. [Azure portalında](https://portal.azure.com/) oturum açın ve Azure Cloud Shell'i açın. Bu seçenek, portalın sağ üst köşesindeki araç çubuğunda bulunur.

    ![Portalın sağ üst çubuğunda Azure Cloud Shell açın](./media/get-drone-imagery-from-drone-partner/navigation-bar-1.png)

3. Ortamın **PowerShell**olarak ayarlandığından emin olun.

    ![PowerShell ayarı](./media/get-drone-imagery-from-drone-partner/power-shell-new-1.png)

4. Cloud Shell örneğindeki 1. adımdan indirdiğiniz iki dosyayı karşıya yükleyin.

    ![Dosyaları karşıya yükleme](./media/get-drone-imagery-from-drone-partner/power-shell-two-1.png)

5. Dosyaların karşıya yüklendiği dizine gidin. Varsayılan olarak, Kullanıcı adı altında ana dizine yüklenir.
6. Şu betiği çalıştırın:

    ```azurepowershell-interactive 

    ./generateCredentials.ps1   

    ```

7. API uç noktası, kiracı KIMLIĞI, Istemci KIMLIĞI, Istemci gizli anahtarı ve EventHub bağlantı dizesinin değerlerini yakalamak için ekran yönergelerini izleyin.

    Gerekli kimlik bilgilerini ortağın drone yazılım sistemine girdikten sonra, tüm grupları Farmtts sisteminden içeri aktarabilirsiniz. Ardından, uçuş yolu planlama ve drone görüntü koleksiyonunuzu yapmak için Grup ayrıntılarını kullanabilirsiniz.

    Ham görüntüler, drone sağlayıcılarının yazılımı tarafından işlendikten sonra, drone yazılım sistemi, veri merkezine, bulunan ormozai ve diğer işlenen görüntüleri yükler.

## <a name="view-drone-imagery"></a>Drone canlandırın 'yi görüntüle

Veriler Farmrekts veri hub 'ına gönderildikten sonra, Farmtts Datahub API 'Lerini kullanarak sahne mağazasını sorgulayabilirsiniz.

Alternatif olarak, en son drone görüntüsünü **Grup ayrıntıları** sayfasında görüntüleyebilirsiniz. Görüntüyü görüntülemek için adımları izleyin.

1. Imagery 'nizin karşıya yüklendiği grubu seçin. **Grup** ayrıntıları sayfası görüntülenir.
2. En son **duyarlık haritaları** bölümüne gidin.
3. **IBir Imagery** bölümünde görüntüyü görüntüleyin.

    ![Drone Imagery bölümü](./media/get-drone-imagery-from-drone-partner/drone-imagery-1.png)

## <a name="download-drone-imagery"></a>Drone canlandırın 'yi indirin

Drone Imagery bölümünü seçtiğinizde, drone diklik 'in yüksek çözünürlüklü görüntüsünü göstermek için bir açılır pencere açılır.

![Yüksek çözünürlüklü dikmozaik](./media/get-drone-imagery-from-drone-partner/download-drone-imagery-1.png)

## <a name="view-all-drone-maps"></a>Tüm drone haritalarını görüntüle

Drone sağlayıcısı tarafından karşıya yüklenen dosyalar ve görüntüler **haritalar** bölümünde görünür. **Haritalar** bölümünü seçin, **gruba**göre filtreleyin ve görüntülemek ve indirmek için uygun dosyaları seçin.

  ![Haritalar bölümü](./media/get-drone-imagery-from-drone-partner/view-drone-maps-1.png)

## <a name="next-steps"></a>Sonraki adımlar

Tek bir imagere 'yi almak için Farmtts veri hub 'ı [API 'lerini](rest-api-in-azure-farmbeats.md) nasıl kullanacağınızı öğrenin.
