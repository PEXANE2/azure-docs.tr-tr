---
title: Visual Studio'u kullanarak bir modeli Azure Analiz Hizmetlerine dağıtma | Microsoft Dokümanlar
description: Visual Studio'u kullanarak bir Azure Çözümleme Hizmetleri sunucusuna bir tabular modeli nasıl dağıtabileceğinizi öğrenin.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 71b3b7815d2a4b0b4de3afdca9db93156f505445
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73572889"
---
# <a name="deploy-a-model-from-visual-studio"></a>Visual Studio üzerinden model dağıtma

Azure aboneliğinizde bir sunucu oluşturduktan sonra, aboneliğinize bir tablo modeli dağıtmaya hazır olursunuz. Üzerinde çalıştığınız bir tabular model projesi oluşturmak ve dağıtmak için Visual Studio'da Analiz Hizmetleri projelerini kullanabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

Başlamak için gerekli olanlar:

* Azure’da **Analysis Services sunucusu**. Daha fazla bilgi için bkz. [Azure Analysis Services sunucusu oluşturma](analysis-services-create-server.md).
* Visual Studio'da **tabular model projesi** veya 1200 veya daha yüksek uyumluluk düzeyinde mevcut bir tabular model. Daha önce hiç oluşturmadınız mı? [Adventure Works İnternet satışı tablosal modelleme öğreticisini](https://docs.microsoft.com/analysis-services/tutorial-tabular-1400/as-adventure-works-tutorial) deneyin.
* **Şirket içi ağ geçidi** - Bir veya daha fazla veri kaynağı kuruluşunuzun ağında şirket içi olarak bulunuyorsa bir [Şirket içi veri ağ geçidi](analysis-services-gateway.md) yüklemeniz gerekir. Ağ geçidi, buluttaki sunucunuzun modeldeki verileri işlemek ve yenilemek üzere şirket içi veri kaynaklarınıza bağlanması için gereklidir.

> [!TIP]
> Dağıtmadan önce tablolarınızdaki verileri işleyebildiğinizden emin olun. Visual Studio'da, **Model** > **İşlem Iİşlemi** > **Tümleri'ni**tıklatın. İşleme başarısız olursa dağıtımı başarıyla yapamazsınız.
> 
> 

## <a name="get-the-server-name"></a>Sunucu adını alma

**Azure portalı** > sunucu > **Genel Bakış** > **Sunucu adı** menüsünde sunucu adını kopyalayın.
   
![Azure'da sunucu adını alma](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

## <a name="to-deploy-from-visual-studio"></a>Visual Studio'dan dağıtmak için

1. Visual Studio > **Solution Explorer'da**proje > **Properties'e**sağ tıklayın. Ardından **Deployment** > **Server'da** sunucu adını yapıştırın.   
   
    ![Sunucu adını dağıtım sunucusu özelliğine yapıştırma](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)
2. **Çözüm Gezgini**’nde **Özellikler**’e sağ tıklayıp **Dağıt**’a tıklayın. Azure'da oturum açmanız istenebilir.
   
    ![Sunucuya dağıtma](./media/analysis-services-deploy/aas-deploy-deploy.png)
   
    Dağıtım durumu hem Çıktı penceresinde hem de Dağıt menüsünde görünür.
   
    ![Dağıtım durumu](./media/analysis-services-deploy/aas-deploy-status.png)

İşte bu kadar!


## <a name="troubleshooting"></a>Sorun giderme

Meta verileri dağıtırken dağıtım başarısız olursa, bunun nedeni Visual Studio'nun sunucunuza bağlanamamasıdır. SSMS kullanarak sunucunuza bağlanabildiğinizden emin olun. Ardından projenin Deployment Server özelliğinin doğru olduğundan emin olun.

Bir tabloda dağıtım başarısız olursa, bunun nedeni sunucunuzun bir veri kaynağına bağlanamaması olabilir. Veri kaynağınız kuruluşunuzun ağında şirket içi olarak bulunuyorsa bir [Şirket içi veri ağ geçidi](analysis-services-gateway.md) yüklediğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Tablo modelinizi sunucunuza dağıttığınıza göre bağlanmak için hazırsınız. Bunu yönetmek [için SQL Server Management Studio (SSMS) ile bağlanabilirsiniz.](analysis-services-manage.md) Ayrıca, Power BI, Power BI Desktop veya Excel gibi [bir istemci araç kullanarak bağlanabilir](analysis-services-connect.md) ve rapor oluşturmaya başlayabilirsiniz.

