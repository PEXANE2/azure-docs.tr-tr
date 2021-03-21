---
title: Windows sanal masaüstü hazırlama MALTı uygulama iliştirme görüntü önizlemesi-Azure
description: Bir Windows sanal masaüstü konak havuzu için MSIX uygulama iliştirme görüntüsü oluşturma.
author: Heidilohr
ms.topic: how-to
ms.date: 12/14/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 204cc9a05d62caf62179100fa3496be422a3ec0c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97426014"
---
# <a name="prepare-an-msix-image-for-windows-virtual-desktop"></a>Windows sanal masaüstü için MSIX görüntüsü hazırlama

> [!IMPORTANT]
> MSIX uygulama iliştirme Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

MSIX uygulama iliştirme (Önizleme), bir MSIX paketinden Kullanıcı oturumuna dinamik olarak uygulama eklemenize olanak tanıyan bir uygulama katmanlama çözümüdür. MSIX paket sistemi, uygulamaları işletim sisteminden ayırır ve bu da sanal makineler için görüntü oluşturmayı kolaylaştırır. MSIX paketi ayrıca kullanıcılarınızın sanal makinelerinde erişebileceği uygulamalar üzerinde daha fazla denetim sağlar. Aynı hatta uygulamaları ana görüntüden ayırarak kullanıcılara daha sonra verebilirsiniz.

## <a name="create-a-vhd-or-vhdx-package-for-msix"></a>MSIX için bir VHD veya VHDX paketi oluşturma

Doğru çalışabilmek için MSIX paketlerinin bir VHD veya VHDX biçiminde olması gerekir. Yani, başlamak için bir VHD veya VHDX paketi oluşturmanız gerekir.

>[!NOTE]
>Henüz yapmadıysanız, [Windows 10](/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)' da Hyper-V ' d i yükledikten sonra Hyper-V ' d i etkinleştirdiğinizden emin olun.

MSIX için bir VHD veya VHDX paketi oluşturmak için:

1. İlk olarak PowerShell 'i açın.
2. Sonra, bir VHD oluşturmak için aşağıdaki cmdlet 'i çalıştırın:

    ```powershell
    New-VHD -SizeBytes <size>MB -Path c:\temp\<name>.vhd -Dynamic -Confirm:$false
    ```

    >[!NOTE]
    > VHD 'nin genişletilmiş MSIX paketini tutabilecek kadar büyük olduğundan emin olun.

3. Yeni oluşturduğunuz VHD 'YI bağlamak için aşağıdaki cmdlet 'i çalıştırın:

    ```powershell
    $vhdObject = Mount-VHD c:\temp\<name>.vhd -Passthru
    ```

4. Sonra, bağlı VHD 'YI başlatmak için bu cmdlet 'i çalıştırın:

    ```powershell
    $disk = Initialize-Disk -Passthru -Number $vhdObject.Number
    ```

5. Bundan sonra, başlatılan VHD için yeni bir bölüm oluşturmak üzere bu cmdlet 'i çalıştırın:

    ```powershell
    $partition = New-Partition -AssignDriveLetter -UseMaximumSize -DiskNumber $disk.Number
    ```

6. Bölümü biçimlendirmek için bu cmdlet 'i çalıştırın:

    ```powershell
    Format-Volume -FileSystem NTFS -Confirm:$false -DriveLetter $partition.DriveLetter -Force
    ```

7. Son olarak, bağlı VHD üzerinde bir üst klasör oluşturun. Bu adım, MSIX paketinin düzgün şekilde çalışması için bir üst klasöre sahip olması gerektiğinden gereklidir. Üst klasör olduğu sürece üst klasöre ne adı ettiğinize bakılmaksızın.

## <a name="expand-msix"></a>MALTıYı Genişlet

Bundan sonra, dosyalarını VHD 'ye "açarak", MSIX görüntüsünü genişletmeniz gerekir.

MSIX görüntüsünü genişletmek için:

1. [Msıxmgr aracını indirin](https://aka.ms/msixmgr) ve. zip klasörünü bir oturum ana bilgisayar VM 'si içindeki bir klasöre kaydedin.

2. Msıxmgr Tool. zip klasörünü sıkıştırmayı açın.

3. Kaynak MDıX paketini, msıxmgr aracının sıkıştırkonunkından aynı klasöre yerleştirin.

4. Yönetici olarak bir komut istemi açın ve msixmgr aracını indirdiğiniz ve sıkıştırkından geri yüklediğiniz klasöre gidin.

5. Önceki bölümde oluşturduğunuz VHD 'de MSIX 'yi açmak için aşağıdaki cmdlet 'i çalıştırın.

    ```powershell
    msixmgr.exe -Unpack -packagePath <package>.msix -destination "f:\<name of folder you created earlier>" -applyacls
    ```

    Aşağıdaki ileti, paketten çıkmadan sonra görünmelidir:

    > Paket için ACL 'Ler başarıyla paketten yüklendi ve uygulandı: <package name> . msix

    >[!NOTE]
    > Ağınızda veya internet 'e bağlı olmayan cihazlarda Iş veya eğitim için Microsoft Store paketler kullanıyorsanız, uygulamaları çalıştırmak için Microsoft Store paket lisanslarını indirip yüklemeniz gerekir. Lisansları almak için bkz. [paketleri çevrimdışı kullanma](app-attach.md#use-packages-offline).

6. Bağlı VHD 'ye gidin ve paket içeriğinin orada olduğundan emin olmak için uygulama klasörünü açın.

7. VHD'yi çıkarın.

## <a name="upload-msix-image-to-share"></a>Paylaşılacak MSIX görüntüsünü karşıya yükle

MSIX paketini oluşturduktan sonra, sonuçta elde edilen VHD, VHDX veya CıM dosyasını kullanıcılarınızın sanal makinelerinizin erişebileceği bir paylaşıma yüklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu özellikle ilgili topluluk sorularımıza [Windows sanal masaüstü TechCommunity](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop)' de sorun.

Windows sanal masaüstü [Geri Bildirim Hub 'ında](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)Windows sanal masaüstü geri bildirimini de bırakabilirsiniz.

Yararlı bulabileceğiniz bazı makaleler aşağıda verilmiştir:

- [MSIX uygulama iliştirme sözlüğü](app-attach-glossary.md)
- [MSIX uygulama iliştirme SSS](app-attach-faq.md)