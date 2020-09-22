---
title: Azure dışı makinelerinizi Azure Güvenlik Merkezi 'ne bağlama
description: Azure dışı makinelerinizi Güvenlik Merkezi 'ne bağlamayı öğrenin
author: memildin
ms.author: memildin
ms.date: 9/12/2020
ms.topic: how-to
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 2602df2e8a2699914ee32138a8aeba31d7f58cdb
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941235"
---
#  <a name="connect-your-non-azure-machines-to-security-center"></a>Azure dışı makinelerinizi Güvenlik Merkezi 'ne bağlama

Güvenlik Merkezi, Azure dışı bilgisayarların güvenlik durumunu izleyebilir ancak öncelikle bu kaynakları eklemeniz gerekir. Azure olmayan bilgisayarları, **Başlangıç** sayfasından veya **envanterden** aşağıda açıklandığı gibi ekleyebilirsiniz.

## <a name="add-non-azure-computers"></a>Azure olmayan bilgisayarlar ekleme 

1. Güvenlik Merkezi 'nin menüsünden **Başlarken** sayfasını açın.
1. **Başlangıç** sekmesini seçin.

    :::image type="content" source="./media/security-center-onboarding/onboarding-get-started-tab.png" alt-text="Başlangıç sayfasında kullanmaya başlayın sekmesi" lightbox="./media/security-center-onboarding/onboarding-get-started-tab.png":::

1. **Azure olmayan sunucular Ekle**' ye tıklayın, **Yapılandır** ' ı seçin.

    > [!TIP]
    > Ayrıca, **Envanter** sayfasının **Azure dışı sunucular Ekle** düğmesini de kullanabilirsiniz.

    Log Analytics çalışma alanlarınızın bir listesi gösterilir. Listede, varsa, otomatik sağlama etkinleştirildiğinde Güvenlik Merkezi tarafından sizin için oluşturulan varsayılan çalışma alanı bulunur. Bu çalışma alanını veya kullanmak istediğiniz başka bir çalışma alanını seçin.

    Mevcut bir çalışma alanına bilgisayar ekleyebilir veya yeni bir çalışma alanı oluşturabilirsiniz. 

1. İsteğe bağlı olarak yeni bir çalışma alanı oluşturmak için  **Yeni çalışma alanı oluştur**' u seçin.

1. Çalışma alanları listesinden ilgili çalışma alanı için **Sunucu Ekle** ' yi seçin.
    **Aracılar yönetimi** sayfası görüntülenir.

    Buradan, eklediğiniz makinelerin türüne göre aşağıdaki ilgili yordamı seçin:

    - [Azure Stack VM 'lerinizi ekleme](#onboard-your-azure-stack-vms)
    - [Linux makinelerinizi ekleme](#onboard-your-linux-machines)
    - [Windows makinelerinizi ekleme](#onboard-your-windows-machines)


### <a name="onboard-your-azure-stack-vms"></a>Azure Stack VM 'lerinizi ekleme
Azure Stack VM 'Leri eklemek için, **aracılar yönetim** sayfasındaki bilgilere ve Azure Stack üzerinde çalışan sanal makinelerde **Azure izleyici, güncelleştirme ve yapılandırma yönetimi** sanal makine uzantısı ' nı yapılandırmanız gerekir.
1. **Aracılar yönetim** sayfasından, **çalışma alanı kimliği** ve **birincil anahtarı** Not defteri 'ne kopyalayın.
1. **Azure Stack** portalınızda oturum açın ve **sanal makineler** sayfasını açın.
1. Güvenlik Merkezi ile korumak istediğiniz sanal makineyi seçin.
    >[!TIP]
    > Azure Stack sanal makine oluşturma hakkında daha fazla bilgi için, [Windows sanal makineler için bu hızlı başlangıç](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-quick-windows-portal) veya [Linux sanal makineleri için bu hızlı başlangıç](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-quick-linux-portal)bölümüne bakın.
1. **Uzantılar**'ı seçin. Bu sanal makinede yüklü olan sanal makine uzantılarının listesi gösteriliyor.
1. **Ekle** sekmesini seçin. **Yeni kaynak** menüsünde, kullanılabilir sanal makine uzantılarının listesi gösterilir.
1. **Azure izleyici, güncelleştirme ve yapılandırma yönetimi** uzantısı ' nı seçin ve **Oluştur**' u seçin. **Uzantı yapılandırmasını yükler** sayfası açılır.
    >[!NOTE]
    > Market 'te listelenen **Azure izleyici, güncelleştirme ve yapılandırma yönetimi** uzantısını görmüyorsanız, kullanılabilir hale getirmek için lütfen Azure Stack işletmenine ulaşın.
1. Uzantı yapılandırmasını **yükler** sayfasında, önceki adımda not defteri ' ne kopyaladığınız **çalışma alanı kimliğini** ve **çalışma alanı anahtarını (birincil anahtar)** yapıştırın.
1. Yapılandırmayı tamamladığınızda **Tamam**' ı seçin. Uzantının durumu, **sağlama başarılı**olarak gösterilir. Sanal makinenin güvenlik merkezi 'nde görünmesi bir saate kadar sürebilir.


### <a name="onboard-your-linux-machines"></a>Linux makinelerinizi ekleme
Linux makineleri eklemek için **aracılar yönetim** sayfasından wget komutuna ihtiyacınız vardır.
1. **Aracılar yönetim** sayfasından **wget** komutunu Not defteri 'ne kopyalayın. Bu dosyayı Linux bilgisayarınızdan erişilebilen bir konuma kaydedin.
1. Linux bilgisayarınızda, WGET komutuyla dosyasını açın. İçeriğin tamamını seçin ve kopyalayıp bir Terminal konsoluna yapıştırın.
1. Yükleme tamamlandığında, *pgrep* komutunu çalıştırarak *omsagent* 'ın yüklü olduğunu doğrulayabilirsiniz. Komutu *omsagent* PID 'sini döndürür.
    Aracının günlükleri şu adreste bulunabilir: */var/seçenek/Microsoft/omsagent/ \<workspace id> /log/* yeni Linux makinenin güvenlik merkezi 'nde görünmesi 30 dakika sürebilir.


### <a name="onboard-your-windows-machines"></a>Windows makinelerinizi ekleme
Windows makineleri eklemek için, **aracılar yönetim** sayfasında bilgilere ve uygun aracı dosyasını (32/64 bit) indirmeniz gerekir.
1. Kurulum dosyasını indirmek için bilgisayar işlemci türünüz için geçerli olan **Windows Aracısını İndir** bağlantısını seçin.
1. **Aracılar yönetim** sayfasından, **çalışma alanı kimliği** ve **birincil anahtarı** Not defteri 'ne kopyalayın.
1. İndirilen kurulum dosyasını hedef bilgisayara kopyalayın ve çalıştırın.
1. Yükleme sihirbazını izleyin (**İleri**, **kabul** **ediyorum, ileri,** **İleri**).
    1. **Azure Log Analytics** sayfasında, Not defteri ' ne kopyaladığınız **çalışma alanı kimliğini** ve **çalışma alanı anahtarını (birincil anahtar)** yapıştırın.
    1. Bilgisayarın Azure Kamu bulutundaki bir Log Analytics çalışma alanına rapor etmesi gerekiyorsa Azure **bulut** açılan LISTESINDEN **Azure ABD kamu** ' yı seçin.
    1. Bilgisayarın, Log Analytics hizmetiyle bir ara sunucu üzerinden iletişim kurması gerekiyorsa **Gelişmiş** seçeneğini belirleyip ara sunucunun URL’sini ve bağlantı noktası numarasını sağlayın.
    1. Yapılandırma ayarlarının tümünü girdikten **sonra ileri**' yi seçin.
    1. **Yüklemeye hazırlanma** sayfasında, uygulanacak ayarları gözden geçirin ve **yüklemeyi**seçin.
    1. **Yapılandırma başarıyla tamamlandı** sayfasında **son**' u seçin.

Tamamlandığında, **Log Analytics Aracısı** **Denetim Masası**'nda görüntülenir. Burada yapılandırmanızı gözden geçirebilir ve aracının bağlı olup olmadığını doğrulayabilirsiniz.

Aracıyı yükleme ve yapılandırma hakkında daha fazla bilgi için bkz. [Windows makinelerini bağlama](../azure-monitor/platform/agent-windows.md#install-agent-using-setup-wizard).


## <a name="verifying"></a>Olmadığının
Tebrikler! Artık Azure ve Azure dışı makinelerinizi tek bir yerde görebilirsiniz. [Varlık envanteri sayfasını](asset-inventory.md) açın ve ilgili kaynak türlerine filtre uygulayın. Bu iki simge türleri ayırt edebilir:

  ![simge1](./media/quick-onboard-linux-computer/security-center-monitoring-icon1.png) Azure dışı makine

  ![simge2](./media/quick-onboard-linux-computer/security-center-monitoring-icon2.png) Azure VM


## <a name="next-steps"></a>Sonraki adımlar

Bu sayfada Azure dışı makinelerinizi Azure Güvenlik Merkezi 'ne nasıl ekleyeceğiniz gösterilmektedir. Durumlarını izlemek için aşağıdaki sayfada açıklandığı gibi envanter araçlarını kullanın:

- [Varlık envanteri ve yönetim araçlarıyla kaynaklarınızı bulun ve yönetin](asset-inventory.md)