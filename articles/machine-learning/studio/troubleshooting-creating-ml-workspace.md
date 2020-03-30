---
title: Çalışma alanı nın sorun giderme
titleSuffix: ML Studio (classic) - Azure
description: Bu kılavuz, Azure Machine Learning Studio (klasik) çalışma alanlarını kurarken sık karşılaşılan bazı sorunlara yönelik çözümler sağlar.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/20/2017
ms.openlocfilehash: 58ccd63e16382aca8e16eb67efba951a055eb254
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79217828"
---
# <a name="troubleshooting-guide-create-and-connect-to-an-azure-machine-learning-studio-classic-workspace"></a>Sorun giderme kılavuzu: Azure Machine Learning Studio (klasik) çalışma alanı oluşturun ve bağlanın

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Bu kılavuz, Azure Machine Learning Studio (klasik) çalışma alanlarını kurarken sık karşılaşılan bazı sorunlara yönelik çözümler sağlar.

## <a name="workspace-owner"></a>Çalışma alanı sahibi
Machine Learning Studio'da (klasik) bir çalışma alanı açmak için, çalışma alanını oluşturmak için kullandığınız Microsoft Hesabında oturum açmanız veya çalışma alanına katılmak için sahibinden davet almanız gerekir. Azure portalından, erişimi yapılandırma olanağı nı da içeren çalışma alanını yönetebilirsiniz.

Çalışma alanını yönetme hakkında daha fazla bilgi için [bkz.]

[Azure Machine Learning Studio (klasik) çalışma alanını yönetme]: manage-workspace.md

## <a name="allowed-regions"></a>İzin verilen bölgeler
Machine Learning şu anda sınırlı sayıda bölgede kullanılabilir. Aboneliğiniz bu bölgelerden birini içermiyorsa, "İzin verilen bölgelerde aboneliğiniz yok" hata iletisini görebilirsiniz.

Aboneliğinize bir bölgenin eklenmesini istemek için Azure portalından yeni bir Microsoft destek isteği oluşturun, sorun türü olarak **Faturalandırma'yı** seçin ve isteğinizi göndermek için istemleri izleyin.

## <a name="storage-account"></a>Depolama hesabı
Machine Learning hizmetinin verileri depolamak için bir depolama hesabına ihtiyacı vardır. Varolan bir depolama hesabı nı kullanabilir veya yeni Machine Learning Studio (klasik) çalışma alanı oluşturduğunuzda (yeni bir depolama hesabı oluşturmak için kotanız varsa) yeni bir depolama hesabı oluşturabilirsiniz.

Yeni Machine Learning Studio (klasik) çalışma alanı oluşturulduktan sonra, çalışma alanını oluşturmak için kullandığınız Microsoft hesabını kullanarak Machine Learning Studio'da (klasik) oturum açabilirsiniz. "Çalışma Alanı Bulunamadı" (aşağıdaki ekran görüntüsüne benzer) hata iletisi ile karşılaşırsanız, lütfen tarayıcı çerezlerinizi silmek için aşağıdaki adımları kullanın.

![Çalışma alanı bulunamadı](media/troubleshooting-creating-ml-workspace/screen3.png)

**Tarayıcı çerezlerini silmek için**

1. Internet Explorer kullanıyorsanız, sağ üst köşedeki **Araçlar** düğmesini tıklatın ve **Internet seçeneklerini**seçin.  

   ![İnternet seçenekleri](media/troubleshooting-creating-ml-workspace/screen4.png)

2. **Genel** sekmesinin altında **Sil'i tıklatın...**

   ![Genel sekmesi](media/troubleshooting-creating-ml-workspace/screen5.png)

3. Tarama **Geçmişini Sil** iletişim kutusunda, **Çerezlerin ve web sitesi verilerinin** seçildiğinden emin olun ve **Sil'i**tıklatın.

   ![Tanımlama bilgilerini silme](media/troubleshooting-creating-ml-workspace/screen6.png)

Çerezler silindikten sonra tarayıcıyı yeniden başlatın ve ardından [Microsoft Azure Machine Learning Studio (klasik)](https://studio.azureml.net) sayfasına gidin. Bir kullanıcı adı ve parola istendiğinde, çalışma alanını oluşturmak için kullandığınız Microsoft hesabını girin.

## <a name="comments"></a>Yorumlar

Amacımız, Machine Learning deneyimini mümkün olduğunca sorunsuz hale getirmektir. Size daha iyi hizmet vermemize yardımcı olmak için lütfen [Azure Machine Learning forumunda](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) herhangi bir yorum ve sorun gönderin.
