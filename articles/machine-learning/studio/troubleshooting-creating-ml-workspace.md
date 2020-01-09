---
title: Çalışma alanı sorunlarını giderme
titleSuffix: ML Studio (classic) - Azure
description: Bu kılavuz, Azure Machine Learning Studio (klasik) çalışma alanlarını ayarlarken bazı sık karşılaşılan güçlükler için çözümler sağlar.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/20/2017
ms.openlocfilehash: 3f2cc3dcc5c9e34590d1cb2d0d3747fd1255f0e7
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75427477"
---
# <a name="troubleshooting-guide-create-and-connect-to-an-azure-machine-learning-studio-classic-workspace"></a>Sorun giderme kılavuzu: Azure Machine Learning Studio (klasik) çalışma alanı oluşturma ve bağlanma
Bu kılavuz, Azure Machine Learning Studio (klasik) çalışma alanlarını ayarlarken bazı sık karşılaşılan güçlükler için çözümler sağlar.



## <a name="workspace-owner"></a>Çalışma alanı sahibi
Machine Learning Studio (klasik) ' de bir çalışma alanı açmak için, çalışma alanını oluşturmak için kullandığınız Microsoft hesabında oturum açmanız veya çalışma alanına katılması için sahibinden bir davetiye almanız gerekir. Azure portal, erişimi yapılandırma özelliğini içeren çalışma alanını yönetebilirsiniz.

Çalışma alanını yönetme hakkında daha fazla bilgi için bkz. [Azure Machine Learning Studio (klasik) çalışma alanını yönetme].

[Azure Machine Learning Studio (klasik) çalışma alanını yönetme]: manage-workspace.md

## <a name="allowed-regions"></a>İzin verilen bölgeler
Machine Learning, şu anda sınırlı sayıda bölgede kullanılabilir. Aboneliğiniz bu bölgelerden birini içermiyorsa, "izin verilen bölgelerde aboneliğiniz yok" hata iletisini görebilirsiniz.

Aboneliğinize bir bölgenin eklenmesini istemek için, Azure portal yeni bir Microsoft destek isteği oluşturun, sorun türü olarak **faturalandırma** ' i seçin ve isteğinizi göndermek için istemleri izleyin.

## <a name="storage-account"></a>Depolama hesabı
Machine Learning hizmeti, verileri depolamak için bir depolama hesabı gerektirir. Mevcut bir depolama hesabını kullanabilir veya yeni Machine Learning Studio (klasik) çalışma alanını oluştururken yeni bir depolama hesabı oluşturabilirsiniz (yeni bir depolama hesabı oluşturmak için kotayı kullanıyorsanız).

Yeni Machine Learning Studio (klasik) çalışma alanı oluşturulduktan sonra, çalışma alanını oluşturmak için kullandığınız Microsoft hesabı kullanarak Machine Learning Studio (klasik) oturum açabilirsiniz. "Çalışma alanı bulunamadı" hata iletisiyle karşılaşırsanız (aşağıdaki ekran görüntüsüne benzer şekilde), lütfen tarayıcı tanımlama bilgilerinizi silmek için aşağıdaki adımları kullanın.

![Çalışma alanı bulunamadı](media/troubleshooting-creating-ml-workspace/screen3.png)

**Tarayıcı tanımlama bilgilerini silmek için**

1. Internet Explorer kullanıyorsanız, sağ üst köşedeki **Araçlar** düğmesine tıklayın ve **Internet seçenekleri**' ni seçin.  

   ![İnternet seçenekleri](media/troubleshooting-creating-ml-workspace/screen4.png)

2. **Genel** sekmesinde Sil ' e tıklayın **...**

   ![Genel sekmesi](media/troubleshooting-creating-ml-workspace/screen5.png)

3. **Gözatma Geçmişini Sil** iletişim kutusunda, **tanımlama bilgileri ve Web sitesi verilerinin** seçili olduğundan emin olun ve **Sil**' e tıklayın.

   ![Tanımlama bilgilerini silme](media/troubleshooting-creating-ml-workspace/screen6.png)

Tanımlama bilgileri silindikten sonra, tarayıcıyı yeniden başlatın ve ardından [Microsoft Azure Machine Learning Studio (klasik)](https://studio.azureml.net) sayfasına gidin. Bir Kullanıcı adı ve parola istendiğinde, çalışma alanını oluşturmak için kullandığınız Microsoft hesabı aynısını girin.

## <a name="comments"></a>Yorumlar

Amacınız Machine Learning deneyimini olabildiğince sorunsuz hale getirmek. Daha iyi sunmamıza yardımcı olması için lütfen [Azure Machine Learning forumundan](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) açıklama ve sorun gönderin.
