---
title: Azure DevTest Labs'ınız için zorunlu yapıları belirtin | Microsoft Dokümanlar
description: Kullanıcı tarafından seçilen yapıları laboratuvara sanal makinelere (VM) yüklemeden önce yüklenmesi gereken zorunlu yapıları nasıl belirteceklerini öğrenin.
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: spelluru
ms.openlocfilehash: 090236ec3647c7c3e38eb862780a615f854e952b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60562261"
---
# <a name="specify-mandatory-artifacts-for-your-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'daki laboratuvarınız için zorunlu yapıları belirtin
Bir laboratuvarın sahibi olarak, laboratuvarda oluşturulan her makineye uygulanan zorunlu yapıları belirtebilirsiniz. Laboratuarınızdaki her makinenin şirket ağınıza bağlanmasını istediğiniz bir senaryo düşünün. Bu durumda, her laboratuvar kullanıcısı, makinelerinin şirket etki alanına bağlı olduğundan emin olmak için sanal makine oluşturma sırasında bir etki alanı birleştirme artmasını eklemek zorunda kalacaktır. Başka bir deyişle, laboratuvar kullanıcıları, makinelerine zorunlu eserler uygulamayı unutmaları durumunda bir makineyi yeniden oluşturmaları gerekir. Bir laboratuvar sahibi olarak, etki alanının yapıyı laboratuarınızda zorunlu bir yapı olarak birleştirmesini sağlayabilirsiniz. Bu adım, her makinenin şirket ağına bağlı olmasını ve laboratuvar kullanıcılarınız için zaman ve çaba tasarrufu sağlar.
 
Diğer zorunlu yapılar, ekibinizin kullandığı ortak bir aracı veya her makinenin varsayılan olarak sahip olması gereken platformla ilgili bir güvenlik paketini vb. içerebilir. Kısacası, laboratuarınızdaki her makinenin sahip olması gereken herhangi bir ortak yazılım zorunlu bir yapı haline gelir. Zorunlu yapıları uygulanan bir makineden özel bir görüntü oluşturur ve sonra bu görüntüden yeni bir makine oluşturursanız, zorunlu yapılar oluşturma sırasında makineye yeniden uygulanır. Bu davranış aynı zamanda, özel görüntü eski olsa bile, ondan bir makine oluşturduğunuzda, oluşturma akışı sırasında zorunlu yapıların en güncel sürümünün uygulandığı anlamına gelir. 
 
Yalnızca parametreleri olmayan yapılar zorunlu yapı olarak desteklenir. Laboratuvar kullanıcınızın laboratuvar oluşturma sırasında ek parametreler girmesini ve böylece VM oluşturma işlemini basit hale getirmesini gerekmez. 

## <a name="specify-mandatory-artifacts"></a>Zorunlu yapıtları belirtme
Windows ve Linux makineleri için zorunlu yapıları ayrı ayrı seçebilirsiniz. Ayrıca, bu yapıları, uygulanmasını istediğiniz sıraya bağlı olarak yeniden sipariş edebilirsiniz. 

1. Laboratuvarınızın ana sayfasında **AYARLAR**altında **Yapılandırma ve ilkeler'i** seçin. 
3. **DıŞ KAYNAKLAR**altında **Zorunlu yapıları** seçin. 
4. **Windows** bölümünde veya **Linux** bölümünde **Edit'i** seçin. Bu örnekte **Windows** seçeneği kullanılıyor. 

    ![Zorunlu eserler sayfası - Düğmeyi güncelle](media/devtest-lab-mandatory-artifacts/mandatory-artifacts-edit-button.png)
4. Bir artefakt seçin. Bu **örnekte 7-Zip** seçeneği kullanılıyor. 
5. **Artefakt Ekle** sayfasında **Ekle'yi**seçin. 

    ![Zorunlu eserler sayfası - 7 zip ekle](media/devtest-lab-mandatory-artifacts/add-seven-zip.png)
6. Başka bir yapı eklemek için makaleyi seçin ve **Ekle'yi**seçin. Bu örnek, **Chrome'u** ikinci zorunlu yapı olarak ekler.

    ![Zorunlu eserler sayfası - Chrome Ekle](media/devtest-lab-mandatory-artifacts/add-chrome.png)
7. Zorunlu **yapılar** sayfasında, seçilen yapıların sayısını belirten bir ileti görürsünüz. İletiyi tıklattığınızda, seçtiğiniz yapıları görürsünüz. Kaydetmek için **Kaydet'i** seçin. 

    ![Zorunlu eserler sayfası - Eserleri kaydet](media/devtest-lab-mandatory-artifacts/save-artifacts.png)
8. Linux VM'leri için zorunlu yapıları belirtmek için adımları yineleyin. 
    
    ![Zorunlu eserler sayfası - Windows ve Linux eserleri](media/devtest-lab-mandatory-artifacts/windows-linux-artifacts.png)
9. Bir yapıyı listeden **silmek** için **... (elips)** satırın sonunda ve **Sil'i**seçin. 
10. Listede ki artefaktları **yeniden sıralamak** için, yapının üzerine fare nin üzerine, **... (elips)** satırın başında beliren öğeyi yeni konuma sürükleyin. 
11. Zorunlu yapıları laboratuvarda kaydetmek için **Kaydet'i**seçin. 

    ![Zorunlu eserler sayfası - Yapıları laboratuvarda kaydet](media/devtest-lab-mandatory-artifacts/save-to-lab.png)
12. Laboratuvarınızın ana sayfasına geri dönmek için **Yapılandırma ve ilkeler** sayfasını kapatın (sağ üst köşedeki **X'i** seçin).  

## <a name="delete-a-mandatory-artifact"></a>Zorunlu bir yapıyı silme
Zorunlu bir yapıyı bir laboratuardan silmek için aşağıdaki işlemleri yapın: 

1. **AYARLAR** **altında Yapılandırma ve ilkeleri** seçin. 
2. **DıŞ KAYNAKLAR**altında **Zorunlu yapıları** seçin. 
3. **Windows** bölümünde veya **Linux** bölümünde **Edit'i** seçin. Bu örnekte **Windows** seçeneği kullanılıyor. 
4. En üstte zorunlu yapı ların sayısını içeren iletiyi seçin. 

    ![Zorunlu eserler sayfası - İletiyi seçin](media/devtest-lab-mandatory-artifacts/select-message-artifacts.png)
5. **Seçili eserler** sayfasında, **... (elips)** yapının silinmesi için **kaldır'ı**seçin. 
    
    ![Zorunlu eserler sayfası - Yapıyı kaldır](media/devtest-lab-mandatory-artifacts/remove-artifact.png)
6. **Seçili yapılar** sayfasını kapatmak için **Tamam'ı** seçin. 
7. **Zorunlu yapılar** sayfasında **Kaydet'i** seçin.
8. Gerekirse **Linux** görüntüleri için adımları tekrarlayın. 
9. Tüm değişiklikleri laboratuvara kaydetmek için **Kaydet'i** seçin. 

## <a name="view-mandatory-artifacts-when-creating-a-vm"></a>VM oluştururken zorunlu yapıları görüntüleme
Şimdi, bir laboratuvar kullanıcısı olarak, laboratuvarda bir VM oluştururken zorunlu yapıların listesini görüntüleyebilirsiniz. Laboratuvar sahibiniz tarafından laboratuvarda ayarlanan zorunlu yapıları düzenleyebilir veya silemezsiniz.

1. Laboratuvarınızın ana sayfasında menüden **Genel Bakış'ı** seçin.
2. Laboratuvara VM eklemek için **+ Ekle'yi**seçin. 
3. Temel **görüntü**seçin. Bu **örnekte Windows Server, sürüm 1709**kullanır.
4. Seçilen zorunlu yapıların sayısını içeren **Eserler** için bir ileti gördüğünüze dikkat edin. 
5. **Eserler'i**seçin. 
6. Laboratuvarın yapılandırmasında ve ilkelerinde belirttiğiniz **zorunlu yapıları** gördüğünüzden onaylayın. 

    ![Bir VM oluşturma - zorunlu yapılar](media/devtest-lab-mandatory-artifacts/create-vm-artifacts.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Bir git artifon deposunu laboratuvara](devtest-lab-add-artifact-repo.md)nasıl ekleyeceğinizi öğrenin.

