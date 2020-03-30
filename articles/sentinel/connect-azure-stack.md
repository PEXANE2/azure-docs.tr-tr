---
title: Azure Stack sanal makinelerinizde Azure Sentinel | Microsoft Dokümanlar
description: Bu makalede, Azure Yığını sanal makinelerinde Azure Monitörü, Güncelleştirme ve Yapılandırma Yönetimi sanal makine uzantısını nasıl sağlayıp Sentinel ile izlemeye başlayacağınızı gösterir.
services: sentinel
documentationcenter: na
author: yelevin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: a8213bd57936f95870324950204dbd6c1473739a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588527"
---
# <a name="connect-azure-stack-virtual-machines-to-azure-sentinel"></a>Azure Yığını sanal makinelerini Azure Sentinel'e bağlayın




Azure Sentinel ile Azure ve Azure Yığını'nda çalışan VM'lerinizi tek bir yerde izleyebilirsiniz. Azure Stack makinelerinizi Azure Sentinel'e dahil etmek için öncelikle sanal makine uzantısını mevcut Azure Stack sanal makinelerinize eklemeniz gerekir. 

Azure Yığını makinelerini bağladıktan sonra, verilerinizi temel alan öngörüleri gösteren bir pano galerisinden seçim yapın. Bu panolar ihtiyaçlarınıza göre kolayca özelleştirilebilir.



## <a name="add-the-virtual-machine-extension"></a>Sanal makine uzantısı ekleme 

Azure Yığını'nda çalışan sanal makinelere **Azure Monitörü, Güncelleme ve Yapılandırma Yönetimi** sanal makine uzantısını ekleyin. 

1. Yeni bir tarayıcı sekmesinde, [Azure Yığını portalınıza](https://docs.microsoft.com/azure-stack/user/azure-stack-use-portal#access-the-portal)giriş yapın.
2. **Sanal makineler** sayfasına gidin, Azure Sentinel ile korumak istediğiniz sanal makineyi seçin. Azure Yığını'nda sanal bir makine oluşturma hakkında bilgi için Azure [Yığını portalıyla Windows sunucusu VM oluşturma](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-quick-windows-portal) veya [Azure Yığını portalını kullanarak Bir Linux sunucusu VM oluşturma](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-quick-linux-portal)'na bakın.
3. **Uzantılar**'ı seçin. Bu sanal makineye yüklenen sanal makine uzantılarının listesi gösterilir.
4. **Ekle** sekmesini tıklatın. **Yeni Kaynak** menü bıçağı açılır ve kullanılabilir sanal makine uzantılarının listesini gösterir. 
5. Azure **Monitörü, Güncelleştirme ve Yapılandırma Yönetimi** uzantısını seçin ve **Oluştur'u**tıklatın. **Yükle uzantısı** yapılandırma penceresi açılır.

   ![Azure Monitör, Güncelleme ve Yapılandırma Yönetimi Ayarları](./media/connect-azure-stack/azure-monitor-extension-fix.png)  

   >[!NOTE]
   > Pazar yerinizde listelenen **Azure Monitörü, Güncelleme ve Yapılandırma Yönetimi** uzantısını görmüyorsanız, kullanılabilir hale getirmek için Azure Stack operatörünüze ulaşın.

6. Azure Sentinel menüsünde, **Gelişmiş'in**ardından **Çalışma Alanı ayarlarını** seçin ve **Çalışma Alanı Kimliği** ve Çalışma **Alanı Anahtarı'nı (Birincil Anahtar)** kopyalayın. 
1. Azure Yığın **Yükleme uzantısı** penceresinde, bunları belirtilen alanlara yapıştırın ve **Tamam'ı**tıklatın.
1. Uzantı yüklemesi tamamlandıktan sonra, durumu **Proed Hükmünolarak**gösterir. Sanal makinenin Azure Sentinel portalında görünmesi bir saat kadar sürebilir.

Windows aracısını yükleme ve yapılandırma hakkında daha fazla bilgi için [Windows bilgisayarları bağla'ya](../azure-monitor/platform/agent-windows.md#install-the-agent-using-setup-wizard)bakın.

Aracı sorunlarının Linux sorun gidermesi için Azure [Log Analytics Linux Agent sorun giderme](../azure-monitor/platform/agent-linux-troubleshoot.md)bölümüne bakın.

Azure'daki Azure Sentinel portalında, **Sanal Makineler**altında, durumları ile birlikte tüm Sanal Bilgisayarlara ve bilgisayarlara genel bir bakış alabilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekmediğinde, Azure Yığını portalı üzerinden uzantıyı sanal makineden kaldırabilirsiniz.

Uzantıyı kaldırmak için:

1. Azure **Yığın Portalı'nı**açın.
2. Sanal **makineler** sayfasına gidin, uzantıyı kaldırmak istediğiniz sanal makineyi seçin.
3. **Uzantıları**seçin, **Microsoft.EnterpriseCloud.Monitoring**uzantısını seçin.
4. **Kaldır'a**tıklayın ve seçiminizi onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- [Ortak Hata Biçimi cihazlarındaki](connect-common-event-format.md) verileri Azure Sentinel'e aktarın.
