---
title: Azure DevTest Labs sanal makinesinde yapı hatalarını tanılama
description: DevTest Labs, bir yapı hatasını tanılamak için kullanabileceğiniz bilgiler sağlar. Bu makalede, yapı tonu hatalarını nasıl giderilen gösterilmektedir.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 115e0086-3293-4adf-8738-9f639f31f918
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: 7229f1ee4061eb38b7c6da09df21102ab302ab42
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76760326"
---
# <a name="diagnose-artifact-failures-in-the-lab"></a>Yapı tonularını laboratuarda tanılama 
Bir yapı oluşturduktan sonra, bunun başarılı olup olmadığını veya başarısız olup olmadığını denetleyebilirsiniz. Azure DevTest Labs'daki yapı günlükleri, bir yapı hatasını tanılamak için kullanabileceğiniz bilgiler sağlar. Windows VM için yapı günlüğü bilgilerini görüntülemek için birkaç seçeneğiniz vardır:

* Azure portalında
* VM'de

> [!NOTE]
> Hataların doğru şekilde tanımlandığından ve açıklandığından emin olmak için, yapının uygun yapıya sahip olması önemlidir. Bir yapıyı doğru şekilde nasıl oluşturabilirsiniz hakkında bilgi [için](devtest-lab-artifact-author.md)bkz. Düzgün yapılandırılmış bir yapı örneği görmek [için, Test parametre türleri](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-test-paramtypes) yapısını göz atın.

## <a name="troubleshoot-artifact-failures-by-using-the-azure-portal"></a>Azure portalını kullanarak yapı hatalarının giderin

1. Azure portalında, kaynak listenizde laboratuvarınızı seçin.
2. Araştırmak istediğiniz yapıyı içeren Windows VM'yi seçin.
3. Sol panelde, **GENEL**altında, **Eserler**seçin. VM ile ilişkili yapıtların listesi görüntülenir. Yapının adı ve yapı durumu belirtilir.

   ![Yapıt durumu](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifacts-failure.png)

4. **Başarısız** durumu gösteren bir yapı seçin. Obje açılıyor. Yapının başarısızlığıyla ilgili ayrıntıları içeren bir uzantı iletisi görüntülenir.

   ![Yapı hatası iletisi](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error.png)


## <a name="troubleshoot-artifact-failures-from-within-the-virtual-machine"></a>Sanal makine nin içinden yapı hatalarını giderme

1. Tanılamak istediğiniz yapıyı içeren VM'de oturum açın.
2. C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\\*1.9*\Status adresine gidin, *burada 1.9* Azure Özel Komut Dosyası Uzantısı sürüm numarasıdır.

   ![Durum dosyası](./media/devtest-lab-troubleshoot-artifact-failure/devtest-lab-artifact-error-vm-status.png)

3. **Durum** dosyasını açın.

**Linux** VM'deki günlük dosyalarını bulma yönergeleri için aşağıdaki makaleye bakın: [Linux sanal makineleriyle Azure Özel Komut Dosyası Uzantısı Sürüm 2'yi kullanın](../virtual-machines/extensions/custom-script-linux.md#troubleshooting)


## <a name="related-blog-posts"></a>İlgili blog gönderileri
* [DevTest Labs'da Kaynak Yöneticisi şablonu kullanarak vm'ye varolan bir Active Directory etki alanına katılma](https://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)

## <a name="next-steps"></a>Sonraki adımlar
* [Bir git deposunu laboratuvara](devtest-lab-add-artifact-repo.md)nasıl ekleyeceğinizi öğrenin.

