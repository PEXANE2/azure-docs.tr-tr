---
title: Visual Studio 'Yu kullanarak Azure Analysis Services model dağıtma | Microsoft Docs
description: Visual Studio 'Yu kullanarak bir Azure Analysis Services sunucusuna tablolu model dağıtmayı öğrenin.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: d6b737985bc19aa09b26bb0d4a65696a364a903a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83697599"
---
# <a name="deploy-a-model-from-visual-studio"></a>Visual Studio üzerinden model dağıtma

Azure aboneliğinizde bir sunucu oluşturduktan sonra, aboneliğinize bir tablo modeli dağıtmaya hazır olursunuz. Üzerinde çalıştığınız bir tablosal model projesi derlemek ve dağıtmak için Visual Studio 'Yu Analysis Services projeleri ile birlikte kullanabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

Başlamak için gerekli olanlar:

* Azure’da **Analysis Services sunucusu**. Daha fazla bilgi için bkz. [Azure Analysis Services sunucusu oluşturma](analysis-services-create-server.md).
* Visual Studio 'da **tablosal model projesi** veya 1200 veya daha yüksek uyumluluk düzeyinde var olan tablolu model. Daha önce hiç oluşturmadınız mı? [Adventure Works İnternet satışı tablosal modelleme öğreticisini](https://docs.microsoft.com/analysis-services/tutorial-tabular-1400/as-adventure-works-tutorial) deneyin.
* **Şirket içi ağ geçidi** - Bir veya daha fazla veri kaynağı kuruluşunuzun ağında şirket içi olarak bulunuyorsa bir [Şirket içi veri ağ geçidi](analysis-services-gateway.md) yüklemeniz gerekir. Ağ geçidi, buluttaki sunucunuzun modeldeki verileri işlemek ve yenilemek üzere şirket içi veri kaynaklarınıza bağlanması için gereklidir.

> [!TIP]
> Dağıtmadan önce tablolarınızdaki verileri işleyebildiğinizden emin olun. Visual Studio 'da **model**  >  **işleme**  >  **Tümünü işle**' ye tıklayın. İşleme başarısız olursa dağıtımı başarıyla yapamazsınız.
> 
> 

## <a name="get-the-server-name"></a>Sunucu adını al

**Azure portalı** > sunucu > **Genel Bakış** > **Sunucu adı** menüsünde sunucu adını kopyalayın.
   
![Azure'da sunucu adını alma](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

## <a name="to-deploy-from-visual-studio"></a>Visual Studio 'dan dağıtmak için

1. Visual Studio > **Çözüm Gezgini**, proje > **Özellikler**' e sağ tıklayın. Sonra **dağıtım**  >  **sunucusu** ' nda sunucu adını yapıştırın.   
   
    ![Sunucu adını dağıtım sunucusu özelliğine yapıştırma](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)
2. **Çözüm Gezgini**’nde **Özellikler**’e sağ tıklayıp **Dağıt**’a tıklayın. Azure'da oturum açmanız istenebilir.
   
    ![Sunucuya dağıtma](./media/analysis-services-deploy/aas-deploy-deploy.png)
   
    Dağıtım durumu hem Çıktı penceresinde hem de Dağıt menüsünde görünür.
   
    ![Dağıtım durumu](./media/analysis-services-deploy/aas-deploy-status.png)

İşte bu kadar!


## <a name="troubleshooting"></a>Sorun giderme

Meta veriler dağıtımında Dağıtım başarısız olursa, büyük olasılıkla Visual Studio 'nun sunucunuza bağlanamamasından kaynaklanıyor olabilir. SQL Server Management Studio (SSMS) kullanarak sunucunuza bağlanabildiğinizden emin olun. Ardından projenin Deployment Server özelliğinin doğru olduğundan emin olun.

Bir tabloda dağıtım başarısız olursa, bunun nedeni sunucunuzun bir veri kaynağına bağlanamaması olabilir. Veri kaynağınız kuruluşunuzun ağında şirket içi olarak bulunuyorsa bir [Şirket içi veri ağ geçidi](analysis-services-gateway.md) yüklediğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Tablo modelinizi sunucunuza dağıttığınıza göre bağlanmak için hazırsınız. Bunu yönetmek için, [SQL Server Management Studio (SSMS) ile bağlanabilirsiniz](analysis-services-manage.md) . Ayrıca, Power BI, Power BI Desktop veya Excel gibi [bir istemci araç kullanarak bağlanabilir](analysis-services-connect.md) ve rapor oluşturmaya başlayabilirsiniz.   

Gelişmiş dağıtım yöntemleri hakkında bilgi edinmek için bkz. [tablolu model çözüm dağıtımı](https://docs.microsoft.com/analysis-services/deployment/tabular-model-solution-deployment?view=azure-analysis-services-current).



