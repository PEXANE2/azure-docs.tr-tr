---
title: Windows bilgisayarları Azure Güvenlik Merkezi’ne ekleme
description: Bu hızlı başlangıç, Log Analytics aracısını Windows bilgisayarında nasıl sağlayabileceğinizi gösterir.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/3/2018
ms.author: memildin
ms.openlocfilehash: 2840b6636c2b511ab57a8bae8adf411f4d8a27d2
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80435920"
---
# <a name="quickstart-onboard-windows-computers-to-azure-security-center"></a>Hızlı Başlangıç: Windows bilgisayarları Azure Güvenlik Merkezi’ne ekleme
Azure aboneliklerinize bindikten sonra, Log Analytics aracısını sağlayarak Azure dışında (örneğin şirket içinde veya diğer bulutlarda) çalışan kaynaklar için Güvenlik Merkezi'ni etkinleştirebilirsiniz.

Bu hızlı başlangıç, Log Analytics aracısını Windows bilgisayarına nasıl yükleyersiniz gösterir.

## <a name="prerequisites"></a>Ön koşullar
Güvenlik Merkezi ile çalışmaya başlamak için Microsoft Azure aboneliğinizin olması gerekir. Bir aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/pricing/free-trial/) için kaydolabilirsiniz.

Bu hızlı başlangıçtaki adımlara geçmeden önce Güvenlik Merkezi’nin Standart fiyatlandırma katmanında olmanız gerekir. Yükseltme yönergeleri için bkz. [Azure aboneliğinizi Güvenlik Merkezi Standart katmanına ekleme](security-center-get-started.md). Güvenlik Merkezi'nin Standard'ına ücretsiz olarak bakabilirsiniz. Daha fazla bilgi için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="add-new-windows-computer"></a>Yeni Windows bilgisayar ekleme

1. [Azure portalında](https://azure.microsoft.com/features/azure-portal/)oturum açın.
2. Microsoft **Azure** menüsünde **Güvenlik Merkezi'ni**seçin. **Güvenlik Merkezi - Genel Bakış** açılır.

   ![Güvenlik Merkezi’ne genel bakış][2]

3. Güvenlik Merkezi ana menüsü altında, **Başlarken**’i seçin.
4. **Başlangıç** sekmesini seçin.

   ![başlarken][3]

5. **Yeni Azure dışı bilgisayarlar ekle** altında, **Yapılandır**’a tıklayın. Log Analytics çalışma alanlarınızın bir listesi gösterilir. Listede, varsa, otomatik sağlama etkinleştirildiğinde Güvenlik Merkezi tarafından sizin için oluşturulan varsayılan çalışma alanı bulunur. Bu çalışma alanını veya kullanmak istediğiniz başka bir çalışma alanını seçin.

    ![Azure dışı bilgisayar ekleme](./media/quick-onboard-windows-computer/non-azure.png)

   Çalışma alanı kimliğiniz için, aracının yapılandırılmasında kullanılacak Windows aracısını ve anahtarlarını indirmeye yönelik bağlantının bulunduğu bir **Doğrudan Aracı** dikey penceresi açılır.

6. Kurulum dosyasını indirmek için bilgisayar işlemci türünüz için geçerli olan **Windows Aracısını İndir** bağlantısını seçin.

7. **Çalışma Alanı Kimliği**’nin sağ tarafında, kopyala simgesini seçin ve kimliği Not Defteri’ne yapıştırın.

8. **Birincil Anahtar**’ın sağ tarafında, kopyala simgesini seçin ve anahtarı Not Defteri’ne yapıştırın.

## <a name="install-the-agent"></a>Aracıyı yükleme
Şimdi indirilen dosyayı hedef bilgisayara yüklemeniz gerekir.

1. Dosyayı hedef bilgisayara kopyalayın ve Kurulum’u çalıştırın.
2. **Hoş Geldiniz** sayfasında, **İleri**’yi seçin.
3. **Lisans Koşulları** sayfasında, lisansı okuyun ve sonra **Kabul Ediyorum**’u seçin.
4. **Hedef Klasör** sayfasında, varsayılan yükleme klasörünü değiştirin veya aynı şekilde bırakın ve daha sonra **İleri**’yi seçin.
5. **Aracı Kurulum Seçenekleri** sayfasında, aracıyı Azure Log Analytics’e bağlamayı seçin ve sonra **İleri**’yi seçin.
6. **Azure Log Analytics** sayfasında, önceki prosedürde Not Defteri’ne kopyaladığınız **Çalışma Alanı Kimliğini** ve **Çalışma Alanı Anahtarını (Birincil Anahtar)** yapıştırın.
7. Bilgisayarın Azure Kamu’daki bir Log Analytics çalışma alanına raporlama yapması gerekiyorsa **Azure Cloud** açılan listesinden **Azure US Government**’ı seçin. Bilgisayarın, Log Analytics hizmetiyle bir ara sunucu üzerinden iletişim kurması gerekiyorsa **Gelişmiş** seçeneğini belirleyip ara sunucunun URL’sini ve bağlantı noktası numarasını sağlayın.
8. Gerekli yapılandırma ayarlarını sağlamayı tamamladıktan sonra **İleri**’yi seçin.

   ![Aracıyı yükleme][5]

9. **Yüklemeye Hazır** sayfasında, tercihlerinizi gözden geçirip **Yükle**’yi seçin.
10. **Yapılandırma başarıyla tamamlandı** sayfasında, **Son**’u seçin

Tamamlandığında, **Log Analytics aracısı** **Denetim Masası'nda**görünür. Burada yapılandırmanızı gözden geçirebilir ve aracının bağlı olup olmadığını doğrulayabilirsiniz.

Aracının yüklenmesi ve yapılandırılması hakkında daha fazla bilgi için bkz. [Windows bilgisayarları bağlama](../azure-monitor/platform/agent-windows.md#install-the-agent-using-setup-wizard).

Artık Azure VM’lerinizi ve Azure olmayan bilgisayarlarınızı tek bir yerde izleyebilirsiniz. **Bilgi İşlem** dikey penceresinde, önerilerle birlikte tüm VM’lere ve bilgisayarlara ilişkin bir genel bakış görürsünüz. Her sütunda bir dizi öneri sunulur. Renk VM'nin veya bilgisayarın söz konusu öneri için geçerli güvenlik durumunu belirtir. Güvenlik Merkezi ayrıca bu bilgisayarlara yönelik tüm algılamaları Güvenlik uyarıları bölümünde gösterir.

  ![Bilgi İşlem dikey penceresi][6]

**Bilgi İşlem** dikey penceresinde gösterilen iki türlü simge vardır:

![simge1](./media/quick-onboard-windows-computer/security-center-monitoring-icon1.png) Azure dışı bilgisayar

![simge2](./media/quick-onboard-windows-computer/security-center-monitoring-icon2.png) Azure VM

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerekmediğinde aracıyı Windows bilgisayardan kaldırabilirsiniz.

Aracıyı kaldırmak için:

1. Açık **Kontrol Paneli**.
2. **Programlar ve Özellikler**'i açın.
3. **Programlar ve Özellikler'de,** Günlük **Analitiği aracıyı** seçin ve **Kaldır'ı**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, Log Analytics aracısını bir Windows bilgisayarında siz buldunuz. Güvenlik Merkezi'ni kullanma hakkında daha fazla bilgi için bir güvenlik ilkesi yapılandırma ve kaynaklarınızın güvenliğini değerlendirme ile ilgili öğreticiye geçin.

> [!div class="nextstepaction"]
> [Öğretici: Güvenlik ilkelerini tanımlama ve değerlendirme](tutorial-security-policy.md)

<!--Image references-->
[2]: ./media/quick-onboard-windows-computer/overview.png
[3]: ./media/quick-onboard-windows-computer/get-started.png
[4]: ./media/quick-onboard-windows-computer/add-computer.png
[5]: ./media/quick-onboard-windows-computer/log-analytics-mma-setup-laworkspace.png
[6]: ./media/quick-onboard-windows-computer/compute.png
