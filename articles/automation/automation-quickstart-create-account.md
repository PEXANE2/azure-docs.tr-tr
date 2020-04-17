---
title: Azure Hızlı Başlangıcı - Azure Otomasyonu hesabı oluşturma | Microsoft Docs
description: Azure Otomasyonu hesabı oluşturmayı ve runbook çalıştırmayı öğrenin
services: automation
ms.date: 04/04/2019
ms.topic: quickstart
ms.subservice: process-automation
ms.custom: mvc
ms.openlocfilehash: 7704f080b7c1878f2fa2b079a1f242c8c2cc87a9
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536990"
---
# <a name="create-an-azure-automation-account"></a>Azure Otomasyonu hesabı oluşturma

Azure üzerinden, tarayıcı tabanlı bir kullanıcı arabirimi olan ve çok sayıda kaynağa erişim sağlayan Azure portalını kullanarak bir Azure Otomasyon hesabı oluşturabilirsiniz. Bir Otomasyon hesabı, belirli bir kiracının tüm bölgelerindeki kaynakları ve abonelikleri yönetebilir. 

Bu hızlı başlangıç, bir Otomasyon hesabı oluşturmanızda ve hesapta bir runbook çalıştırmada size yol göstersin. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure'da oturum açın.](https://portal.azure.com)

## <a name="create-automation-account"></a>Otomasyon hesabı oluşturma

1. Azure hesabınız için bir ad seçin. Otomasyon hesap adları bölge ve kaynak grubuna göre benzersizdir. Silinen Otomasyon hesaplarının adları hemen kullanılamayabilir.

    > [!NOTE]
    > Kullanıcı arabirimine girildikten sonra hesap adını değiştiremezsiniz. 

2. Azure portalının sol üst köşesinde bulunan **kaynak oluştur** düğmesini tıklatın.

3. **BT & Yönetim Araçları'nı**seçin ve ardından **Otomasyon'u**seçin.

4. Seçili hesap adı da dahil olmak üzere hesap bilgilerini girin. **Azure Farklı Çalıştır hesabı oluştur** alanında **Evet**'i seçerek Azure kimlik doğrulamasını kolaylaştıran yapıtların otomatik olarak etkinleştirilmesini sağlayın. Bilgiler tamamlandığında, Otomasyon hesabı dağıtımını başlatmak için **Oluştur'u** tıklatın.

    ![Otomasyon hesabınız hakkındaki bilgileri sayfaya girin](./media/automation-quickstart-create-account/create-automation-account-portal-blade.png)  

    > [!NOTE]
    > Bir Otomasyon hesabını dağıtabileceğiniz konumların güncelleştirilmiş bir listesi için, [bölgeye göre kullanılabilen Ürünler'e](https://azure.microsoft.com/global-infrastructure/services/?products=automation&regions=all)bakın.

5. Dağıtım tamamlandığında, Tüm **Hizmetler'i**tıklatın.

6. **Otomasyon Hesapları'nı** seçin ve ardından oluşturduğunuz Otomasyon hesabını seçin.

    ![Otomasyon hesabına genel bakış](./media/automation-quickstart-create-account/automation-account-overview.png)

## <a name="run-a-runbook"></a>Bir runbook'u çalıştırma

Öğretici runbook'larından birini çalıştırın.

1. **Proses Otomasyonu**altında **Runbook'ları** tıklatın. Runbook'ların listesi görüntülenir. Varsayılan olarak, hesapta birkaç öğretici runbook etkinleştirilir.

    ![Otomasyon hesabı runbook'larının listesi](./media/automation-quickstart-create-account/automation-runbooks-overview.png)

1. **AzureAutomationTutorialScript** runbook'unu seçin. Runbook'a genel bakış sayfası açılır.

    ![Runbook'a genel bakış](./media/automation-quickstart-create-account/automation-tutorial-script-runbook-overview.png)

1. **Başlat**'a tıklayın ve Runbook'u Başlat sayfasında **Tamam**'ı seçerek runbook'u başlatın.

    ![Runbook işi sayfası](./media/automation-quickstart-create-account/automation-tutorial-script-job.png)

1. İş durumu olduktan `Running`sonra, runbook iş çıktısını görüntülemek için **Çıktı'yı** veya **Tüm Günlükleri** tıklatın. Bu öğretici runbook'u için çıktıda Azure kaynaklarınızın listesi yer alır.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Otomasyon hesabı dağıttınız, runbook işi başlattınız ve iş sonuçlarını görüntülediniz. Azure Otomasyonu hakkında daha fazla bilgi edinmek için ilk runbook'unuzu oluşturmanızı sağlayacak hızlı başlangıç sayfasına gidin.

> [!div class="nextstepaction"]
> [Otomasyon Hızlı Başlangıcı - Runbook Oluşturma](./automation-quickstart-create-runbook.md)

