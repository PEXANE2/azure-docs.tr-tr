---
title: Azure Hızlı Başlangıcı - Azure Otomasyonu hesabı oluşturma | Microsoft Docs
description: Azure Otomasyonu hesabı oluşturmayı ve runbook çalıştırmayı öğrenin
services: automation
ms.date: 04/04/2019
ms.topic: quickstart
ms.subservice: process-automation
ms.custom: mvc
ms.openlocfilehash: 7704f080b7c1878f2fa2b079a1f242c8c2cc87a9
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81536990"
---
# <a name="create-an-azure-automation-account"></a>Azure Otomasyonu hesabı oluşturma

Bir dizi kaynağa erişime izin veren tarayıcı tabanlı kullanıcı arabirimi Azure portal kullanarak Azure aracılığıyla Azure Otomasyonu hesabı oluşturabilirsiniz. Tek bir Otomasyon hesabı, belirli bir kiracının tüm bölgeleri ve abonelikleri genelinde kaynakları yönetebilir. 

Bu hızlı başlangıç, bir Otomasyon hesabı oluşturma ve hesapta runbook çalıştırma konusunda size rehberlik eder. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure 'Da oturum açın](https://portal.azure.com).

## <a name="create-automation-account"></a>Otomasyon hesabı oluşturma

1. Azure hesabınız için bir ad seçin. Otomasyon hesabı adları, bölge ve kaynak grubu başına benzersizdir. Silinen Otomasyon hesaplarının adları hemen kullanılamayabilir.

    > [!NOTE]
    > Kullanıcı arabirimine girildikten sonra hesap adını değiştiremezsiniz. 

2. Azure portal sol üst köşesinde bulunan **kaynak oluştur** düğmesine tıklayın.

3. **Yönetim araçları &** seçin ve ardından **Otomasyon**' u seçin.

4. Seçilen hesap adı da dahil olmak üzere hesap bilgilerini girin. **Azure Farklı Çalıştır hesabı oluştur** alanında **Evet**'i seçerek Azure kimlik doğrulamasını kolaylaştıran yapıtların otomatik olarak etkinleştirilmesini sağlayın. Bilgiler tamamlandığında, Otomasyon hesabı dağıtımını başlatmak için **Oluştur** ' a tıklayın.

    ![Otomasyon hesabınız hakkındaki bilgileri sayfaya girin](./media/automation-quickstart-create-account/create-automation-account-portal-blade.png)  

    > [!NOTE]
    > Otomasyon hesabı dağıtabileceğiniz konumların güncelleştirilmiş listesi için bkz. [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=automation&regions=all).

5. Dağıtım tamamlandığında, **tüm hizmetler**' e tıklayın.

6. **Otomasyon hesapları** ' nı seçin ve ardından oluşturduğunuz Otomasyon hesabını seçin.

    ![Otomasyon hesabına genel bakış](./media/automation-quickstart-create-account/automation-account-overview.png)

## <a name="run-a-runbook"></a>Bir runbook'u çalıştırma

Öğretici runbook'larından birini çalıştırın.

1. **Işlem Otomasyonu**altında **runbook 'lar** ' a tıklayın. Runbook'ların listesi görüntülenir. Varsayılan olarak, hesapta çeşitli öğretici runbook 'lar etkindir.

    ![Otomasyon hesabı runbook'larının listesi](./media/automation-quickstart-create-account/automation-runbooks-overview.png)

1. **AzureAutomationTutorialScript** runbook'unu seçin. Runbook'a genel bakış sayfası açılır.

    ![Runbook'a genel bakış](./media/automation-quickstart-create-account/automation-tutorial-script-runbook-overview.png)

1. **Başlat**'a tıklayın ve Runbook'u Başlat sayfasında **Tamam**'ı seçerek runbook'u başlatın.

    ![Runbook işi sayfası](./media/automation-quickstart-create-account/automation-tutorial-script-job.png)

1. İş durumu olduktan sonra `Running`, runbook iş çıkışını görüntülemek için **Çıkış** ' a veya **Tüm Günlükler** ' e tıklayın. Bu öğretici runbook'u için çıktıda Azure kaynaklarınızın listesi yer alır.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Otomasyon hesabı dağıttınız, runbook işi başlattınız ve iş sonuçlarını görüntülediniz. Azure Otomasyonu hakkında daha fazla bilgi edinmek için ilk runbook'unuzu oluşturmanızı sağlayacak hızlı başlangıç sayfasına gidin.

> [!div class="nextstepaction"]
> [Otomasyon Hızlı Başlangıcı - Runbook Oluşturma](./automation-quickstart-create-runbook.md)

