---
title: Azure DevTest Labs'daki bir laboratuvara Git deposu ekleme | Microsoft Dokümanlar
description: Azure DevTest Labs'da özel yapı kaynağınız için GitHub veya Azure DevOps Services Git deposu nu nasıl ekleyeceğinizi öğrenin.
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: 01b459f7-eaf2-45a8-b4b5-2c0a821b33c8
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 1e7587c60e180fb35e1a2bed735b053b6b0c388a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294599"
---
# <a name="add-a-git-repository-to-store-custom-artifacts-and-resource-manager-templates"></a>Özel yapıları ve Kaynak Yöneticisi şablonlarını depolamak için Git deposu ekleme

Laboratuvarınızdaki [VM'ler](devtest-lab-artifact-author.md) için özel yapılar oluşturabilir veya [özel bir test ortamı oluşturmak için Azure Kaynak Yöneticisi şablonlarını kullanabilirsiniz.](devtest-lab-create-environment-from-arm.md) Ekibinizin oluşturduğu yapılar veya Kaynak Yöneticisi şablonları için özel bir Git deposu eklemeniz gerekir. Depo [GitHub'da](https://github.com) veya [Azure DevOps Hizmetlerinde](https://visualstudio.com)barındırılabilir.

Olduğu gibi dağıtabileceğiniz veya bunları laboratuvarlarınız için özelleştirebileceğiniz [bir GitHub deposu](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) sunuyoruz. Bir yapıyı özelleştirdiğinizde veya oluşturduğunuzda, yapıyı genel depoda depolayamazsınız. Özel yapılar ve oluşturduğunuz yapılar için kendi özel repo oluşturmanız gerekir. 

Bir VM oluşturduğunuzda, Kaynak Yöneticisi şablonu kaydedebilir, isterseniz özelleştirebilir ve daha sonra daha fazla VM oluşturmak için kullanabilirsiniz. Özel Kaynak Yöneticisi şablonlarınızı depolamak için kendi özel deponuzu oluşturmanız gerekir.  

* GitHub deposu oluşturmayı öğrenmek için [GitHub Bootcamp'e](https://help.github.com/categories/bootcamp/)bakın.
* Git deposu olan bir Azure DevOps Hizmetleri projesinin nasıl oluşturulabileceğinizi öğrenmek [için](https://www.visualstudio.com/get-started/setup/connect-to-visual-studio-online)bkz.

Aşağıdaki şekil, yapıları olan bir deponun GitHub'da nasıl görünebileceğine bir örnektir:  

![Örnek GitHub eserler repo](./media/devtest-lab-add-repo/devtestlab-github-artifact-repo-home.png)

## <a name="get-the-repository-information-and-credentials"></a>Depo bilgilerini ve kimlik bilgilerini alın
Laboratuvarınıza bir depo eklemek için öncelikle deponuzdan önemli bilgileri alın. Aşağıdaki bölümlerde GitHub veya Azure DevOps Hizmetlerinde barındırılan depolar için gerekli bilgilerin nasıl alınılabildiğini açıklanmaktadır.

### <a name="get-the-github-repository-clone-url-and-personal-access-token"></a>GitHub depo klon URL'sini ve kişisel erişim jetonunu alın

1. Yapı veya Kaynak Yöneticisi şablon tanımlarını içeren GitHub deposunun ana sayfasına gidin.
2. **Clone or download**'u (Kopyala veya indir) seçin.
3. URL'yi panoya kopyalamak için **HTTPS klon url** düğmesini seçin. URL'yi daha sonra kullanmak üzere kaydedin.
4. GitHub'ın sağ üst köşesinde profil görüntüsünü seçin ve ardından **Ayarlar'ı**seçin.
5. Soldaki **Kişisel ayarlar** menüsünde **Kişisel erişim belirteçleri'ni**seçin.
6. **Yeni belirteç oluştur'u**seçin.
7. Yeni **kişisel erişim belirteci** sayfasında, **Belirteç açıklamasıaltında,** bir açıklama girin. Varsayılan öğeleri **Select kapsamları**altında kabul edin ve ardından **Token Oluştur'u**seçin.
8. Oluşturulan belirteci kaydedin. Jetonu daha sonra kullanırsın.
9. GitHub'ı kapatın.   
10. [Laboratuvarınızı depo bölümüne bağlayın.](#connect-your-lab-to-the-repository)

### <a name="get-the-azure-repos-clone-url-and-personal-access-token"></a>Azure Repos klon URL'sini ve kişisel erişim jetonunu alın

1. Takım koleksiyonunuzun ana sayfasına gidin `https://contoso-web-team.visualstudio.com`(örneğin), ve sonra projenizi seçin.
2. Proje ana sayfasında **Kod'u**seçin.
3. Proje **Kodu** sayfasında klon URL'sini görüntülemek için **Klon'u**seçin.
4. URL'yi kaydedin. URL'yi daha sonra kullanırsınız.
5. Kişisel erişim jetonu oluşturmak için, kullanıcı hesabı açılır menüsünde **Profilim'i**seçin.
6. Profil bilgileri sayfasında **Güvenlik'i**seçin.
7. **Güvenlik** sekmesinde **Ekle'yi**seçin.
8. Kişisel **erişim belirteci** sayfası oluştur'da:
   1. Belirteç için bir **Açıklama** girin.
   2. Sona **Erdirilecekler** listesinde **180 gün**seçin.
   3. **Hesaplar** **listesinde, tüm erişilebilir hesapları**seçin.
   4. Salt **Okunur** seçeneğini belirleyin.
   5. **Belirteç Oluştur'u**seçin.
9. Yeni belirteç Kişisel **Erişim Belirteçleri** listesinde görünür. **Token'i**kopyala'yı seçin ve ardından belirteç değerini daha sonra kullanmak üzere kaydedin.
10. [Laboratuvarınızı depo bölümüne bağlayın.](#connect-your-lab-to-the-repository)

## <a name="connect-your-lab-to-the-repository"></a>Laboratuvarınızı depoya bağlayın
1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.
2. **Daha Fazla Hizmet**seçin ve ardından hizmetler listesinden **DevTest Labs'ı** seçin.
3. Laboratuvarlar listesinden, laboratuvarınızı seçin. 
4. **Yapılandırma ve ilkeler** > **Depoları** > **+ Ekle'yi**seçin.

    ![Depo Ekle düğmesi](./media/devtest-lab-add-repo/devtestlab-add-repo.png)
5. İkinci **Depolar** sayfasında aşağıdaki bilgileri belirtin:
   1. **Adı**. Depo için bir ad girin.
   2. **Git Klon Url**. Daha önce GitHub veya Azure DevOps Hizmetlerinden kopyaladığınız Git HTTPS klon URL'sini girin.
   3. **Şube**. Tanımlarınızı almak için şubeyi girin.
   4. **Kişisel Erişim Belirteci**. GitHub veya Azure DevOps Hizmetleri'nden daha önce aldığınız kişisel erişim jetonunu girin.
   5. **Klasör Yolları**. Yapı veya Kaynak Yöneticisi şablon tanımlarınızı içeren klon URL'sine göre en az bir klasör yolu girin. Bir alt dizini belirttiğiniz zaman, klasör yoluna ileri eğik çizgieklediğinizden emin olun.

      ![Depolar alanı](./media/devtest-lab-add-repo/devtestlab-repo-blade.png)
6. **Kaydet'i**seçin.

### <a name="related-blog-posts"></a>İlgili blog gönderileri
* [DevTest Labs'da başarısız yapıları giderme](devtest-lab-troubleshoot-artifact-failure.md)
* [DevTest Labs'da Kaynak Yöneticisi şablonu kullanarak vm'ye varolan bir Active Directory etki alanına katılma](https://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Özel Git deponuzu oluşturduktan sonra, gereksinimlerinize bağlı olarak aşağıdakilerden birini veya her ikisini de yapabilirsiniz:
* Özel [yapılarınızı](devtest-lab-artifact-author.md)saklayın. Bunları daha sonra yeni VM'ler oluşturmak için kullanabilirsiniz.
* [Kaynak Yöneticisi şablonlarını kullanarak çoklu VM ortamları ve PaaS kaynakları oluşturun.](devtest-lab-create-environment-from-arm.md) Ardından, şablonları özel repo'nuzda depolayabilirsiniz.

Bir VM oluşturduğunuzda, yapıtların veya şablonların Git deponuza eklendiğinden doğrulayabilirsiniz. Bunlar, yapıtlar veya şablonlar listesinde hemen kullanılabilir. Özel repo'nuzun adı kaynağı belirten sütunda gösterilir. 
