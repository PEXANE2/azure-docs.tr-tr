---
title: Azure Yığını sanal makineleri Azure Güvenlik Merkezi'ne
description: Bu hızlı başlangıç, Azure Yığını sanal makinelerinde Azure Monitörü, Güncelleştirme ve Yapılandırma Yönetimi sanal makine uzantısını nasıl sağabileceğinizi gösterir.
services: security-center
documentationcenter: na
author: pipposera
manager: dsavage
ms.assetid: 8982348a-0624-40c7-8a1e-642a523c7f6b
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/02/2019
ms.author: fiseraci
ms.openlocfilehash: c186dcb2d7d7d423f5c001bdb4d3f3503beebd9c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73686517"
---
# <a name="quickstart-onboard-your-azure-stack-virtual-machines-to-security-center"></a>Quickstart: Azure Stack sanal makinelerinizde Güvenlik Merkezi'ne
Azure aboneliğinizde bulunduktan sonra, Azure Yığını pazaryerinden **Azure Monitörü, Güncelleme ve Yapılandırma Yönetimi** sanal makine uzantısını ekleyerek Azure Yığını'nda çalışan sanal makinelerinizi korumasını Güvenlik Merkezi'ne etkinleştirebilirsiniz.

Bu hızlı başlangıç, Azure Yığını'nda çalışan sanal bir makineye (Linux ve Windows her ikisi de desteklenir) **Azure Monitörü, Güncelleme ve Yapılandırma Yönetimi** sanal makine uzantısını nasıl ekleyeceğinizi gösterir.

## <a name="prerequisites"></a>Ön koşullar
Güvenlik Merkezi ile çalışmaya başlamak için Microsoft Azure aboneliğinizin olması gerekir. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

Bu hızlı başlatmaya başlamadan önce Güvenlik Merkezi'nin Standart katmanında bir Azure aboneliğiniz olması gerekir. Yükseltme yönergeleri için bkz. [Azure aboneliğinizi Güvenlik Merkezi Standart katmanına ekleme](security-center-get-started.md). Güvenlik Merkezi Standart katmanını 30 gün boyunca ücretsiz olarak deneyebilirsiniz. Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="select-your-workspace-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde çalışma alanınızı seçin

1. [Azure portalında](https://azure.microsoft.com/features/azure-portal/)oturum açın.
2. Microsoft **Azure** menüsünde **Güvenlik Merkezi'ni**seçin. **Güvenlik Merkezi - Genel Bakış** açılır. 

   ![Güvenlik Merkezi’ne genel bakış][2]

3. Güvenlik Merkezi ana menüsü altında, **Başlarken**’i seçin.
4. **Başlangıç** sekmesini seçin.

   ![Kullanmaya başlayın][3]

5. **Yeni Azure dışı bilgisayarlar ekle** altında, **Yapılandır**’a tıklayın. Log Analytics çalışma alanlarınızın bir listesi gösterilir. Listede, varsa, otomatik sağlama etkinleştirildiğinde Güvenlik Merkezi tarafından sizin için oluşturulan varsayılan çalışma alanı bulunur. Bu çalışma alanını veya Azure Yığını VM'nin güvenlik verilerini bildirmesini istediğiniz başka bir çalışma alanını seçin.

    ![Azure dışı bilgisayar ekleme](./media/quick-onboard-windows-computer/non-azure.png)

   **Doğrudan Aracılı** bıçak, aracıyı yapılandırmada kullanmak üzere çalışma alanı kimliğinizin aracısını ve anahtarlarını indirmek için bir bağlantıyla açılır.

   >[!NOTE]
   > Aracıyı el ile indirmeniz gerekmez. Aracı aşağıdaki adımlarda VM uzantısı olarak yüklenir.

6. **Çalışma Alanı Kimliği**’nin sağ tarafında, kopyala simgesini seçin ve kimliği Not Defteri’ne yapıştırın.

7. **Birincil Anahtar**’ın sağ tarafında, kopyala simgesini seçin ve anahtarı Not Defteri’ne yapıştırın.

## <a name="add-the-virtual-machine-extension-to-your-existing-azure-stack-virtual-machines"></a>Mevcut Azure Yığını sanal makinelerinize sanal makine uzantısını ekleyin
Azure Yığını'nda çalışan sanal makinelere **Azure Monitörü, Güncelleme ve Yapılandırma Yönetimi** sanal makine uzantısını eklemeniz gerekir.

1. Yeni bir tarayıcı sekmesinde Azure **Yığını** portalınızda oturum açın.
2. **Sanal makineler** sayfasına gidin, Güvenlik Merkezi ile korumak istediğiniz sanal makineyi seçin. Azure Yığını'nda sanal bir makine oluşturma hakkında bilgi için [Windows sanal makineleri için bu hızlı başlatmaya](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-quick-windows-portal) veya Linux sanal makineleri için bu hızlı [başlatmaya](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-quick-linux-portal)bakın.
3. **Uzantılar**'ı seçin. Bu sanal makineye yüklenen sanal makine uzantılarının listesi gösterilir.
4. **Ekle** sekmesini tıklatın. **Yeni Kaynak** menü bıçağı açılır ve kullanılabilir sanal makine uzantılarının listesini gösterir. 
5. Azure **Monitörü, Güncelleme ve Yapılandırma Yönetimi** uzantısını seçin ve **Oluştur'u**tıklatın. **Install uzantı** lı yapılandırma bıçağı açılır.

>[!NOTE]
> Pazar yerinizde listelenen **Azure Monitörü, Güncelleme ve Yapılandırma Yönetimi** uzantısını görmüyorsanız, kullanılabilir hale getirmek için lütfen Azure Stack operatörünüze ulaşın.

6. Yükle **uzantılı** yapılandırma bıçağına, önceki yordamda Notepad'e kopyaladığınız **Çalışma Alanı Kimliği** ve Çalışma **Alanı Anahtarını (Birincil Anahtar)** yapıştırın.
7. Gerekli yapılandırma ayarlarını sağlamayı bitirdiğinizde **Tamam'ı**tıklatın.
8. Uzantı yüklemesi tamamlandıktan sonra, durumu **Prosucceeded Hükmünde**olarak gösterecektir. Sanal makinenin Güvenlik Merkezi portalında görünmesi bir saat kadar sürebilir.

Windows aracısını yükleme ve yapılandırma hakkında daha fazla bilgi için [Windows bilgisayarları bağlayın'a](../azure-monitor/platform/agent-windows.md#install-the-agent-using-setup-wizard)bakın.

Aracı sorunlarının Linux sorun gidermesi için Azure [Log Analytics Linux Agent sorun giderme](../azure-monitor/platform/agent-linux-troubleshoot.md)bölümüne bakın.

Artık Azure VM’lerinizi ve Azure olmayan bilgisayarlarınızı tek bir yerde izleyebilirsiniz. Azure'daki Güvenlik Merkezi portalında, **Compute**altında, önerileriyle birlikte tüm VM'lere ve bilgisayarlara genel bir bakış alabilirsiniz. Güvenlik Merkezi ayrıca Güvenlik uyarıları bu bilgisayarlar için herhangi bir algılama yüzeyler.

  ![Bilgi İşlem dikey penceresi][6]

**Bilgi İşlem** dikey penceresinde gösterilen iki türlü simge vardır:

![simge1](./media/quick-onboard-windows-computer/security-center-monitoring-icon1.png) Azure dışı bilgisayar 

![simge2](./media/quick-onboard-windows-computer/security-center-monitoring-icon2.png) Azure VM (Azure Yığını VM'leri bu grupta gösterilecek)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekmediğinde, Azure Yığını portalı üzerinden uzantıyı sanal makineden kaldırabilirsiniz.

Uzantıyı kaldırmak için:

1. Azure **Yığın Portalı'nı**açın.
2. Sanal **makineler** sayfasına gidin, uzantıyı kaldırmak istediğiniz sanal makineyi seçin.
3. **Uzantıları**seçin, **Microsoft.EnterpriseCloud.Monitoring**uzantısını seçin.
4. **Kaldır'a**tıklayın ve **Evet'i**tıklatarak seçimonaylayın.

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, Azure Yığını'nda çalışan sanal bir makinede Azure Monitör, Güncelleştirme ve Yapılandırma Yönetimi uzantısını siz de dahil edinmişsiniz. Güvenlik Merkezi'ni kullanma hakkında daha fazla bilgi için bir güvenlik ilkesi yapılandırma ve kaynaklarınızın güvenliğini değerlendirme ile ilgili öğreticiye geçin.

> [!div class="nextstepaction"]
> [Öğretici: Güvenlik ilkelerini tanımlama ve değerlendirme](tutorial-security-policy.md)

<!--Image references-->
[2]: ./media/quick-onboard-windows-computer/overview.png
[3]: ./media/quick-onboard-windows-computer/get-started.png
[4]: ./media/quick-onboard-windows-computer/add-computer.png
[5]: ./media/quick-onboard-windows-computer/log-analytics-mma-setup-laworkspace.png
[6]: ./media/quick-onboard-windows-computer/compute.png
