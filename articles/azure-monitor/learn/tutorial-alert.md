---
title: Azure Uygulama Öngörüleri'nden uyarı gönderme | Microsoft Dokümanlar
description: Azure Uygulama Öngörüleri'ni kullanarak uygulamanızdaki hatalara yanıt olarak uyarı gönderme eğitimi.
ms.subservice: application-insights
ms.topic: tutorial
author: mrbullwinkle
ms.author: mbullwin
ms.date: 04/10/2019
ms.custom: mvc
ms.openlocfilehash: 7195de1cf58e5dd2e1d0b49b309f3afc718cca92
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77656271"
---
# <a name="monitor-and-alert-on-application-health-with-azure-application-insights"></a>Azure Application Insights ile uygulama durumunu izleme ve uyarı

Azure Application Insights, uygulamanızı izlemenize ve kullanılamadığında, hatalar yaşadığında veya performans sorunları yaşadığında size uyarı göndermenize olanak tanır.  Bu öğretici, uygulamanızın kullanılabilirliğini sürekli olarak kontrol etmek için testler oluşturma işlemine geçmenizi ister.

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Uygulamanın yanıtını sürekli olarak denetlemek için kullanılabilirlik testi oluşturun
> * Bir sorun oluştuğunda yöneticilere posta gönderme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

Uygulama [Öngörüleri kaynağı](https://docs.microsoft.com/azure/azure-monitor/learn/dotnetcore-quick-start#enable-application-insights)oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.

## <a name="create-availability-test"></a>Kullanılabilirlik testi oluşturma

Application Insights'taki kullanılabilirlik testleri, uygulamanızı dünyanın çeşitli yerlerinden otomatik olarak test etmenize olanak sağlar.   Bu eğitimde, web uygulamanızın kullanılabilir olduğundan emin olmak için bir url testi gerçekleştireceksiniz.  Ayrıntılı çalışmasını test etmek için tam bir izlik de oluşturabilirsiniz. 

1. **Application Insights**’ı ve sonra aboneliğinizi seçin.  

2. **Araştır** menüsü altında **Kullanılabilirlik'i** seçin ve ardından **test oluştur'u**tıklatın.

    ![Kullanılabilirlik testi ekleme](media/tutorial-alert/add-test-001.png)

3. Test için bir ad yazın ve diğer varsayılanları bırakın.  Bu seçim, beş farklı coğrafi konumdan her 5 dakikada bir uygulama url'si için istekleri tetikler.

4. Test başarısız olursa nasıl yanıt verebileceğinize ilişkin ayrıntıları tanımlayabileceğiniz **Uyarılar** açılır dosyasını açmak için **Uyarılar'ı** seçin. **Yakın gerçek zamanlı'yı** seçin ve durumu Etkin olarak **ayarlayın.**

    Uyarı ölçütleri karşılandığında göndermek için bir e-posta adresi yazın.  Uyarı ölçütleri karşılandığında aramak için isteğe bağlı olarak bir webhook adresini yazabilirsiniz.

    ![Test oluşturma](media/tutorial-alert/create-test-001.png)

5. Test paneline dönün, elipsleri seçin ve neredeyse gerçek zamanlı uyarınız için yapılandırmayı girmek için uyarıyı düzenleme.

    ![Uyarıyı edin](media/tutorial-alert/edit-alert-001.png)

6. Başarısız konumları 3'ten büyük veya eşit olarak ayarlayın. Uyarı eşiğiniz ihlal edildiğinde kimlere bilgi verilir yapılandırmak için bir [eylem grubu](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) oluşturun.

    ![Uyarı UI kaydet](media/tutorial-alert/save-alert-001.png)

7. Uyarınızı yapılandırıldıktan sonra, her konumdaki ayrıntıları görüntülemek için test adını tıklayın. Testler, belirli bir zaman aralığının başarısını/hatalarını görselleştirmek için hem satır grafiği hem de dağılım çizimbiçiminde görüntülenebilir.

    ![Test ayrıntıları](media/tutorial-alert/test-details-001.png)

8. Dağılım grafiğindeki noktasını tıklatarak herhangi bir testin ayrıntılarını inceleyebilirsiniz. Bu, uçuça işlem ayrıntıları görünümünü başlatacaktır. Aşağıdaki örnekte, başarısız bir isteğin ayrıntıları gösterilmektedir.

    ![Test sonucu](media/tutorial-alert/test-result-001.png)
  
## <a name="next-steps"></a>Sonraki adımlar

Artık sorunlar hakkında nasıl uyarı da bulunduğunızı öğrendiğinize göre, kullanıcıların uygulamanızla nasıl etkileşimde bulunduğunızı nasıl analiz edeceğiz öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Kullanıcıları anlama](../../azure-monitor/learn/tutorial-users.md)
