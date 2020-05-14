---
title: Hızlı başlangıç-Power BI çalışma alanını bir Synapse çalışma alanına bağlama
description: Bu kılavuzdaki adımları izleyerek bir Power BI çalışma alanını Azure SYNAPSE Analytics çalışma alanına bağlayın.
services: synapse-analytics
author: jocaplan
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/29/2020
ms.author: jocaplan
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 83e9e802d2150ee7f43f01e9f6d6827ef3acfcdd
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83204757"
---
# <a name="quickstart-linking-a-power-bi-workspace-to-a-synapse-analytics-workspace-preview"></a>Hızlı başlangıç: Power BI çalışma alanını SYNAPSE Analytics çalışma alanına bağlama (Önizleme)

Bu hızlı başlangıçta, SYNAPSE Studio 'dan (Önizleme) yeni Power BI raporları ve veri kümeleri oluşturmak için bir Power BI çalışma alanını bir Synapse Analytics çalışma alanına nasıl bağlayacağınızı öğreneceksiniz.

Azure aboneliğiniz yoksa [başlamadan önce ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

- [Azure SYNAPSE çalışma alanı ve ilişkili depolama hesabı oluşturma](quickstart-create-workspace.md)
- [Power BI Professional veya Premium çalışma alanı](https://docs.microsoft.com/power-bi/service-create-the-new-workspaces)

## <a name="link-power-bi-workspace-to-your-synapse-workspace"></a>Power BI çalışma alanını SYNAPSE çalışma alanınıza bağlama

1. SYNAPSE Studio 'dan başlayarak **Yönet**' e tıklayın.

    ![SYNAPSE Studio Yönet ' e tıklayın.](media/quickstart-link-powerbi/synapse-studio-click-manage.png)

2. **Dış bağlantılar**altında **bağlı hizmetler**' e tıklayın.

    ![Bağlı hizmetler vurgulandı.](media/quickstart-link-powerbi/manage-click-linked-services.png)

3. **+ Yeni** öğesine tıklayın.

    ![+ Yeni bağlı hizmetler vurgulanır.](media/quickstart-link-powerbi/new-highlighted.png)

4. **Power BI** ' a tıklayın ve **devam**' a tıklayın.

    ![Power BI bağlı hizmetini görüntüleme.](media/quickstart-link-powerbi/powerbi-linked-service.png)

5. Bağlı hizmet için bir ad girin ve açılan listeden bir çalışma alanı seçin.

    ![Power BI bağlı hizmet kurulumu görüntüleniyor.](media/quickstart-link-powerbi/workspace-link-dialog.png)

6. **Oluştur**' a tıklayın.

## <a name="view-power-bi-workspace-in-synapse-studio"></a>SYNAPSE Studio 'da Power BI çalışma alanını görüntüleme

Çalışma alanlarınızın bağlantısı kurulduktan sonra, SYNAPSE Studio 'dan Power BI veri kümelerine gözatabilir, yeni Power BI raporları düzenleyebilir/oluşturabilirsiniz.

1. **Geliştirme**' ye tıklayın.

    ![SYNAPSE Studio geliştirme ' ye tıklayın.](media/quickstart-link-powerbi/synapse-studio-click-develop.png)

2. Power BI ve kullanmak istediğiniz çalışma alanını genişletin.

    ![Power BI ve çalışma alanını genişletin.](media/quickstart-link-powerbi/develop-expand-powerbi.png)

Yeni raporlar **+** , **Geliştir** sekmesinin en üstünde tıklatılarak oluşturulabilir. var olan raporlar, rapor adına tıklanarak düzenlenebilir. Kaydedilen tüm değişiklikler Power BI çalışma alanına geri yazılır.

![Power BI raporu görüntüleyin ve düzenleyin.](media/quickstart-link-powerbi/powerbi-report.png)


## <a name="next-steps"></a>Sonraki adımlar

Sonra, [Azure depolama 'da depolanan dosyalarda Power BI rapor oluşturabilirsiniz](sql/tutorial-connect-power-bi-desktop.md)
