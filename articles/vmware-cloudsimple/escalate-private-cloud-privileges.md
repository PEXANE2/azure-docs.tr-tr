---
title: Özel bulut ayrıcalıklarını artırın
titleSuffix: Azure VMware Solution by CloudSimple
description: vCenter'daki yönetim işlevleri için özel bulutunuzdaki ayrıcalıkları nasıl artıracağınızı açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/05/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 63dc18c522a1e2e3b03bdf806945e0be67774b18
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81870467"
---
# <a name="escalate-private-cloud-vcenter-privileges-from-the-cloudsimple-portal"></a>CloudSimple portalından Özel Bulut vCenter ayrıcalıklarını artırın

Private Cloud vCenter'ınıza yönetimerişimi için CloudSimple ayrıcalıklarınızı geçici olarak artırabilirsiniz.  Yükseltilmiş ayrıcalıkları kullanarak VMware çözümleri yükleyebilir, kimlik kaynakları ekleyebilir ve kullanıcıları yönetebilirsiniz.

VCenter SSO etki alanında yeni kullanıcılar oluşturulabilir ve vCenter'a erişim hakkı verilebilir.  Yeni kullanıcılar oluşturduğunuzda, bunları vCenter'a erişmek için CloudSimple yerleşik gruplarına ekleyin.  Daha fazla bilgi için [VMware vCenter'ın CloudSimple Private Cloud izin modeline](https://docs.microsoft.com/azure/vmware-cloudsimple/learn-private-cloud-permissions/)bakın.

> [!CAUTION]
> Yönetim bileşenleri için yapılandırma değişiklikleri yapmayın. Artırılabilen ayrıcalıklı durum sırasında gerçekleştirilen eylemler sisteminizi olumsuz etkileyebilir veya sisteminizin kullanılamamasına neden olabilir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="escalate-privileges"></a>Ayrıcalıkları yükseltme

1. [CloudSimple portalına](access-cloudsimple-portal.md)erişin.

2. **Kaynaklar** sayfasını açın, ayrıcalıkları tırmandırmak istediğiniz Özel Bulut'u seçin.

3. **VSphere ayrıcalıklarını Değiştir**altında Özet sayfasının en altında, **Escalate'i**tıklatın.

    ![vSphere ayrıcalığını değiştirme](media/escalate-private-cloud-privilege.png)

4. vSphere kullanıcı türünü seçin.  Yalnızca `CloudOwner@cloudsimple.local` yerel kullanıcı yükseltilebilir.

5. Açılan zaman aralığından yükseltme zaman aralığını seçin. Görevi tamamlamanızı sağlayacak en kısa süreyi seçin.

6. Riskleri anladığınızı doğrulamak için onay kutusunu seçin.

    ![Ayrıcalık iletişim kutusunu yükseltme](media/escalate-private-cloud-privilege-dialog.png)

7. **Tamam**'a tıklayın.

8. Yükseltme işlemi birkaç dakika sürebilir. İşlem tamamlandığında **Tamam**’a tıklayın.

Ayrıcalık yükseltme si, seçili aralığın sonuna kadar devam eder.  Yönetim görevlerini yapmak için özel bulut vCenter'ınızda oturum açabilirsiniz.

> [!IMPORTANT]
> Yalnızca bir kullanıcı nın artırılmış ayrıcalıkları olabilir.  Başka bir kullanıcının ayrıcalıklarını yükseltmeden önce kullanıcının ayrıcalıklarını yükseltmeniz gerekir.

> [!CAUTION]
> Yeni kullanıcılar yalnızca *Cloud-Owner-Group*, *Cloud-Global-Cluster-Admin-Group*, *Cloud-Global-Storage-Admin-Group,* *Cloud-Global-Network-Admin-Group* veya *Cloud-Global-VM-Admin-Group'a*eklenmelidir.  *Yöneticiler* grubuna eklenen kullanıcılar otomatik olarak kaldırılır.  *Yöneticiler* grubuna yalnızca hizmet hesapları eklenmelidir ve hizmet hesapları vSphere web UI'da oturum açmak için kullanılmamalıdır.

## <a name="extend-privilege-escalation"></a>Ayrıcalık yükseltmeyi genişletme

Görevlerinizi tamamlamak için ek süreye ihtiyacınız varsa, ayrıcalık yükseltme süresini uzatabilirsiniz.  Yönetim görevlerini tamamlamanızı sağlayan ek yükseltme süresini seçin.

1. CloudSimple**portalındaki** **Özel Kaynaklar** > portalında, ayrıcalık yükseltmesini genişletmek istediğiniz Özel Bulut'u seçin.

2. Özet sekmesinin en altında, **ayrıcalık yükseltmesini genişlet'i**tıklatın.

    ![Ayrıcalık yükseltmeyi genişletme](media/de-escalate-private-cloud-privilege.png)

3. Açılan süreden bir yükseltme zaman aralığı seçin. Yeni yükseltme bitiş saatini gözden geçirin.

4. Aralığı uzatmak için **Kaydet'i** tıklatın.

## <a name="de-escalate-privileges"></a>Ayrıcalıkları yükseltme

İdari görevleriniz tamamlandıktan sonra ayrıcalıklarınızı artırmalısınız.  

1. CloudSimple**portalındaki** **Özel Kaynaklar** > portalında ayrıcalıkları küçletmek istediğiniz Özel Bulut'u seçin.

2. **De-escalate'yi**tıklatın.

3. **Tamam**'a tıklayın.

> [!IMPORTANT]
> Herhangi bir hatadan kaçınmak için vCenter'dan oturum açın ve artan ayrıcalıklardan sonra yeniden oturum açın.

## <a name="next-steps"></a>Sonraki adımlar

* [Active Directory'yi kullanmak için vCenter kimlik kaynaklarını ayarlama](https://docs.microsoft.com/azure/vmware-cloudsimple/set-vcenter-identity/)
* Yedekleme iş [yükü sanal makinelere](https://docs.microsoft.com/azure/vmware-cloudsimple/backup-workloads-veeam/) yedekleme çözümü yükleme