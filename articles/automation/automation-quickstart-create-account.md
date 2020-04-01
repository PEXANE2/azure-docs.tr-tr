---
title: Azure Hızlı Başlangıcı - Azure Otomasyonu hesabı oluşturma | Microsoft Docs
description: Azure Otomasyonu hesabı oluşturmayı ve runbook çalıştırmayı öğrenin
services: automation
ms.date: 04/04/2019
ms.topic: quickstart
ms.subservice: process-automation
ms.custom: mvc
ms.openlocfilehash: 92b49197cc18133c9b67222a90546776f050163b
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437919"
---
# <a name="create-an-azure-automation-account"></a>Azure Otomasyonu hesabı oluşturma

Azure Otomasyonu hesaplarını Azure üzerinden oluşturabilirsiniz. Bu yöntem, Otomasyon hesaplarını ve ilgili kaynakları oluşturup yapılandırmaya yönelik tarayıcı tabanlı bir kullanıcı arabirimi sağlar. Bu hızlı başlangıçta Otomasyon hesabı oluşturma ve hesapta runbook çalıştırma adımları ele alınmaktadır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure'da oturum açın](https://portal.azure.com)

## <a name="create-automation-account"></a>Otomasyon hesabı oluşturma

1. Azure’ın sol üst köşesinde bulunan **Kaynak oluştur** düğmesine tıklayın.

1. **BT & Yönetim Araçları'nı**seçin ve ardından **Otomasyon'u**seçin.

1. Hesap bilgilerini girin. **Azure Farklı Çalıştır hesabı oluştur** alanında **Evet**'i seçerek Azure kimlik doğrulamasını kolaylaştıran yapıtların otomatik olarak etkinleştirilmesini sağlayın. Otomasyon Hesabını oluştururken adın daha sonra değiştirilemeyeceğini unutmayın. *Otomasyon Hesabı adları bölge ve kaynak grubuna göre benzersizdir. Silinen Otomasyon Hesapları nın adları hemen kullanılamayabilir.* Bir Otomasyon Hesabı belirli bir kiracı için kaynakları tüm bölgelerde ve aboneliklerde yönetebilir. İşlemi tamamladığınızda Otomasyon hesabı dağıtımını başlatmak için **Oluştur**'a tıklayın.

    ![Otomasyon hesabınız hakkındaki bilgileri sayfaya girin](./media/automation-quickstart-create-account/create-automation-account-portal-blade.png)  

    > [!NOTE]
    > Otomasyon Hesabı dağıtabileceğiniz konumların güncel listesi için bkz. [Bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=automation&regions=all).

1. Dağıtım tamamlandığında, Tüm **Hizmetler,Otomasyon** **Hesapları'nı** tıklatın ve oluşturduğunuz Otomasyon Hesabını seçin.

    ![Otomasyon hesabına genel bakış](./media/automation-quickstart-create-account/automation-account-overview.png)

## <a name="run-a-runbook"></a>Bir runbook'u çalıştırma

Öğretici runbook'larından birini çalıştırın.

1. **SÜREÇ OTOMASYONU**'nın altındaki **Runbook'lar** öğesine tıklayın. Runbook'ların listesi görüntülenir. Varsayılan olarak hesapta etkin halde birden fazla öğretici amaçlı runbook bulunur.

    ![Otomasyon hesabı runbook'larının listesi](./media/automation-quickstart-create-account/automation-runbooks-overview.png)

1. **AzureAutomationTutorialScript** runbook'unu seçin. Runbook'a genel bakış sayfası açılır.

    ![Runbook'a genel bakış](./media/automation-quickstart-create-account/automation-tutorial-script-runbook-overview.png)

1. **Başlat**'a tıklayın ve **Runbook'u Başlat** sayfasında **Tamam**'ı seçerek runbook'u başlatın.

    ![Runbook işi sayfası](./media/automation-quickstart-create-account/automation-tutorial-script-job.png)

1. **İş durumu**, **Çalışıyor** olarak değiştikten sonra runbook işi çıktısını görüntülemek için **Çıktı** veya **Tüm Günlükler**'e tıklayın. Bu öğretici runbook'u için çıktıda Azure kaynaklarınızın listesi yer alır.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Otomasyon hesabı dağıttınız, runbook işi başlattınız ve iş sonuçlarını görüntülediniz. Azure Otomasyonu hakkında daha fazla bilgi edinmek için ilk runbook'unuzu oluşturmanızı sağlayacak hızlı başlangıç sayfasına gidin.

> [!div class="nextstepaction"]
> [Otomasyon Hızlı Başlangıcı - Runbook Oluşturma](./automation-quickstart-create-runbook.md)

