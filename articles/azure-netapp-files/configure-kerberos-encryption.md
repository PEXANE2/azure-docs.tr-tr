---
title: Azure NetApp Files için NFSv 4.1 Kerberos şifrelemesini yapılandırma | Microsoft Docs
description: Azure NetApp Files için NFSv 4.1 Kerberos şifrelemesini ve performans etkisini nasıl yapılandıracağınızı açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 02/18/2021
ms.author: b-juche
ms.openlocfilehash: 6ff87d046c60f588e133010895ec3e7ce08cb71f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101740571"
---
# <a name="configure-nfsv41-kerberos-encryption-for-azure-netapp-files"></a>Azure NetApp Files için NFSv4.1 Kerberos şifrelemesini yapılandırma

Azure NetApp Files, Kerberos modlarında (krb5, krb5i ve krb5p), AES-256 şifrelemesi ile NFS istemci şifrelemesini destekler. Bu makalede, bir NFSv 4.1 birimini Kerberos şifrelemesi ile kullanmak için gereken yapılandırma açıklanmaktadır.

## <a name="requirements"></a>Gereksinimler

Aşağıdaki gereksinimler NFSv 4.1 istemci şifrelemesi için geçerlidir: 

* Kerberos anahtar oluşturmayı kolaylaştırmak için Active Directory Domain Services (AD DS) bağlantısı 
* Hem istemci hem de Azure NetApp Files NFS sunucusu IP adresleri için DNS A/PTR kayıt oluşturma
* Bir Linux istemcisi  
    Bu makalede RHEL ve Ubuntu istemcileri için rehberlik sunulmaktadır.  Diğer istemciler benzer yapılandırma adımlarıyla çalışacaktır. 
* NTP sunucusu erişimi  
    Yaygın olarak kullanılan Active Directory Etki Alanı Denetleyicisi (AD DC) etki alanı denetleyicilerinden birini kullanabilirsiniz.

## <a name="create-an-nfs-kerberos-volume"></a>NFS Kerberos birimi oluşturma

1.  NFSv 4.1 birimini oluşturmak için [Azure NetApp Files IÇIN NFS birimi oluşturma](azure-netapp-files-create-volumes.md) bölümündeki adımları izleyin.   

    Birim Oluştur sayfasında, NFS sürümünü **Nfsv 4.1** olarak ayarlayın ve Kerberos ' u **etkin** olarak ayarlayın.

    > [!IMPORTANT] 
    > Birim oluşturulduktan sonra Kerberos etkinleştirme seçimini değiştiremezsiniz.

    ![NFSv 4.1 Kerberos birimi oluştur](../media/azure-netapp-files/create-kerberos-volume.png)  

2. Birim için istenen erişim düzeyi ve güvenlik seçeneği (Kerberos 5, Kerberos 5i veya Kerberos 5p) ile eşleşecek şekilde **dışa aktarma ilkesini** seçin.   

    Kerberos 'un performans etkisi için bkz. [NFSv 4.1 üzerinde Kerberos 'un performans etkisi](#kerberos_performance).  

    Ayrıca, Azure NetApp Files gezinti bölmesinde Ilkeyi dışarı aktar ' a tıklayarak birimin Kerberos güvenlik yöntemlerini değiştirebilirsiniz.

3.  NFSv 4.1 birimini oluşturmak için **gözden geçir + oluştur** ' a tıklayın.

## <a name="configure-the-azure-portal"></a>Azure portal yapılandırma 

1.  [Active Directory bağlantı oluşturma](create-active-directory-connections.md)bölümündeki yönergeleri izleyin.  

    Kerberos, Active Directory ' de en az bir makine hesabı oluşturmanızı gerektirir. Sağladığınız hesap bilgileri hem SMB *hem* de nfsv 4.1 Kerberos birimlerine yönelik hesaplar oluşturmak için kullanılır. Bu makine hesap, birim oluşturma sırasında otomatik olarak oluşturulur.

2.  **Kerberos bölgesi** altında **ad sunucusu adını** ve **KDC IP** adresini girin.

    AD sunucusu ve KDC IP aynı sunucu olabilir. Bu bilgiler, Azure NetApp Files tarafından kullanılan SPN makinesi hesabını oluşturmak için kullanılır. Makine hesabı oluşturulduktan sonra, Azure NetApp Files gerektiğinde ek KDC sunucularının yerini bulmak için DNS sunucusu kayıtlarını kullanacaktır. 

    ![Kerberos bölgesi](../media/azure-netapp-files/kerberos-realm.png)
 
3.  Yapılandırmayı kaydetmek için **Birleştir** ' e tıklayın.

## <a name="configure-active-directory-connection"></a>Active Directory bağlantısını yapılandırma 

NFSv 4.1 Kerberos yapılandırması, Active Directory iki bilgisayar hesabı oluşturur:
* SMB paylaşımları için bir bilgisayar hesabı
* NFSv 4.1 için bir bilgisayar hesabı--bu hesabı öneki ile tanımlayabilirsiniz `NFS-` . 

İlk NFSv 4.1 Kerberos birimini oluşturduktan sonra, aşağıdaki PowerShell komutunu kullanarak bilgisayar hesabı için şifreleme türünü ayarlayın:

`Set-ADComputer $NFSCOMPUTERACCOUNT -KerberosEncryptionType AES256`

## <a name="configure-the-nfs-client"></a>NFS istemcisini yapılandırma 

NFS istemcisini yapılandırmak için [Azure NetApp FILES NFS Istemcisi yapılandırma](configure-nfs-clients.md) konusundaki yönergeleri izleyin.  

## <a name="mount-the-nfs-kerberos-volume"></a><a name="kerberos_mount"></a>NFS Kerberos birimini bağlama

1. **Birimler** sayfasında, bağlamak istediğiniz NFS birimini seçin.

2. Yönergeleri göstermek için birimden **bağlama yönergeleri** ' ni seçin.

    Örnek: 

    ![Kerberos birimleri için bağlama yönergeleri](../media/azure-netapp-files/mount-instructions-kerberos-volume.png)  

3. Yeni birim için dizin (bağlama noktası) oluşturun.  

4. Bilgisayar hesabı için varsayılan şifreleme türünü AES 256 olarak ayarlayın:  
    `Set-ADComputer $NFSCOMPUTERACCOUNT -KerberosEncryptionType AES256 -Credential $ANFSERVICEACCOUNT`

    * Her bilgisayar hesabı için bu komutu yalnızca bir kez çalıştırmanız gerekir.
    * Bu komutu, bir etki alanı denetleyicisinden veya [rsat](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems) yüklü bir bilgisayardan çalıştırabilirsiniz. 
    * `$NFSCOMPUTERACCOUNT`Değişkeni, Kerberos birimini dağıtırken Active Directory oluşturulan bilgisayar hesabıdır. Bu, ön eki olan hesaptır `NFS-` . 
    * `$ANFSERVICEACCOUNT`Değişken, bilgisayar hesabının oluşturulduğu kuruluş birimi üzerinde atanmış denetimleri olan ayrıcalıksız bir Active Directory kullanıcı hesabıdır. 

5. Birimi konağa bağlayın: 

    `sudo mount -t nfs -o sec=krb5p,rw,hard,rsize=1048576,wsize=1048576,vers=4.1,tcp $ANFEXPORT $ANFMOUNTPOINT`

    * `$ANFEXPORT`Değişkeni, `host:/export` bağlama yönergelerinde bulunan yoldur.
    * `$ANFMOUNTPOINT`Değişkeni, Linux ana bilgisayarındaki kullanıcı tarafından oluşturulan klasördür.

## <a name="performance-impact-of-kerberos-on-nfsv41"></a><a name="kerberos_performance"></a>NFSv 4.1 üzerinde Kerberos 'un performans etkisi 

NFSv 4.1 birimleri için kullanılabilen güvenlik seçeneklerini, sınanan performans vektörlerini ve Kerberos 'un beklenen performans etkisini anlamanız gerekir. Ayrıntılar için [NFSv 4.1 birimlerinde Kerberos 'un performans etkisi](performance-impact-kerberos.md) konusuna bakın.  

## <a name="next-steps"></a>Sonraki adımlar  

* [NFSv 4.1 birimlerinde Kerberos 'un performans etkisi](performance-impact-kerberos.md)
* [NFSv 4.1 Kerberos birimi sorunlarını giderme](troubleshoot-nfsv41-kerberos-volumes.md)
* [Azure NetApp Files hakkında SSS](azure-netapp-files-faqs.md)
* [Azure NetApp Files için NFS birimi oluşturma](azure-netapp-files-create-volumes.md)
* [Active Directory bağlantısı oluşturma](create-active-directory-connections.md)
* [Azure NetApp Files için NFS istemcisini yapılandırma](configure-nfs-clients.md) 
