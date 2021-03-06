---
title: "Командлеты для работы с каталогами"
ms.date: 2016-07-13
keywords: PowerShell, DSC, WMF
description: 
ms.topic: article
author: keithb
manager: carolz
ms.prod: powershell
ms.technology: WMF
translationtype: Human Translation
ms.sourcegitcommit: ecf70f38bbf48f410eb59b75f86eea767637757a
ms.openlocfilehash: 72df0311a1d187dc6c7c1d29b0a3d2fd243848f0

---
# <a name="catalog-cmdlets"></a>Командлеты для работы с каталогами  

Мы добавили два новых командлета для создания и проверки файлов каталога Windows в модуль [Microsoft.Powershell.Security](https://technet.microsoft.com/en-us/library/hh847877.aspx).  

## <a name="new-filecatalog"></a>New-FileCatalog 
--------------------------------

`New-FileCatalog` создает файл каталога Windows для набора файлов и папок. Файл каталога содержит хэши для всех файлов, находящихся по указанным путям. Пользователь может распространять набор папок вместе с соответствующим файлом каталога, представляющим этих папки. Файл каталога может использоваться получателем содержимого, чтобы проверить, были ли внесены изменения в папки после создания каталога.    

```PowerShell
New-FileCatalog [-CatalogFilePath] <string> [[-Path] <string[]>] [-CatalogVersion <int>] [-WhatIf] [-Confirm] [<CommonParameters>]
```
Поддерживается создание каталогов версий 1 и 2. В версии 1 для создания хэшей файлов используется алгоритм хэширования SHA1, в версии 2 — SHA256. Каталог версии 2 не поддерживается в *Windows Server 2008 R2* и *Windows 7*. На платформах *Windows 8* и *Windows Server 2012* и более поздней версии рекомендуется использовать каталог версии 2.  

Для использования этой команды в существующем модуле укажите для переменных CatalogFilePath и Path значения, соответствующие расположению манифеста модуля. В следующем примере манифест модуля располагается в папке "C:\Program Files\Windows PowerShell\Modules\Pester". 

![](../images/NewFileCatalog.jpg)

Следующая команда создает файл каталога. 

![](../images/CatalogFile1.jpg)  

![](../images/CatalogFile2.jpg) 

Для проверки целостности файла каталога (Pester.cat в примере выше) его нужно подписать командлетом [Set-AuthenticodeSignature](https://technet.microsoft.com/library/hh849819.aspx).   


## <a name="test-filecatalog"></a>Test-FileCatalog 
--------------------------------

`Test-FileCatalog` проверяет каталог, представляющий набор папок. 

```PowerShell
Test-FileCatalog [-CatalogFilePath] <string> [[-Path] <string[]>] [-Detailed] [-FilesToSkip <string[]>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

![](../images/TestFileCatalog.jpg)

Этот командлет сравнивает хэши всех файлов и их относительные пути, найденные в файле каталога, с сохраненными на жестком диске. При обнаружении любого несоответствия между хэшами файлов и путями он возвращает состояние `ValidationFailed`. Все эти данные можно получить при помощи флага `Detailed`. Состояние подписанности каталога отображается в поле `Signature`. Подпись также можно определить, вызвав командлет [Get-AuthenticodeSignature](https://technet.microsoft.com/en-us/library/hh849805.aspx) и указав файл каталога. Также можно исключить любые файлы из проверки, указав их в параметре `FilesToSkip`. 



<!--HONumber=Nov16_HO3-->


