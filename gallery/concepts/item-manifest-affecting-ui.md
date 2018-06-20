---
ms.date: 06/09/2017
schema: 2.0.0
keywords: powershell
title: Значения манифеста элементов, связанные с пользовательским интерфейсом коллекции PowerShell
ms.openlocfilehash: 39522396b179c54b981e6292cddacec27b32506c
ms.sourcegitcommit: e9ad4d85fd7eb72fb5bc37f6ca3ae1282ae3c6d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
ms.locfileid: "34048659"
---
# <a name="item-manifest-values-that-impact-the-powershell-gallery-ui"></a>Значения манифеста элементов, связанные с пользовательским интерфейсом коллекции PowerShell

В этой статье издателям предоставляется сводная информация о том, как изменить манифест для публикаций в коллекции PowerShell таким образом, чтобы он относился к функциям командлетов PowerShellGet и пользовательского интерфейса коллекции PowerShell.
Это содержимое упорядочено по области отображения изменений (от центральной части до области навигации слева). Теги подробно рассматриваются в соответствующем разделе. В нем описаны важные и часто используемые теги.
Также предусмотрено два раздела с примерами манифестов:

- Сведения о модулях см. в разделе об [обновлении манифеста модуля](https://docs.microsoft.com/powershell/gallery/psget/module/psget_update-modulemanifest).
- Сведения о скриптах см. в разделе о [создании файла скрипта с метаданными](https://docs.microsoft.com/powershell/gallery/psget/script/psget_new-scriptfileinfo).

## <a name="powershell-gallery-feature-elements-controlled-by-the-manifest"></a>Управляемые манифестом элементы компонентов в коллекции PowerShell

В следующей таблице описаны управляемые издателем элементы пользовательского интерфейса страницы элементов для коллекции PowerShell.
Для каждого элемента указано, можно ли им управлять при помощи манифеста модуля или скрипта.

| Элемент пользовательского интерфейса | Описание | Модуль | Скрипт |
| --- | --- | --- | --- |
| **Название** | Имя элемента, опубликованного в галерее.  | Нет | Нет |
| **Версия** | Отображаемая версия — это строка версии в метаданных, а также строка предварительной версии, если она указана. Основная часть версии в манифесте модуля указывается в элементе ModuleVersion. Для скрипта она определяется как .VERSION. Если задана строка предварительной версии, она будет добавлена к ModuleVersion для модулей или указана как часть .VERSION для скриптов. Для указания строк предварительной версии в [модулях](https://docs.microsoft.com/en-us/powershell/gallery/psget/module/prereleasemodule) и [скриптах](https://docs.microsoft.com/en-us/powershell/gallery/psget/script/prereleasescript) предусмотрена отдельная документация. | Да | Да |
| **Описание** | В манифесте модуля это Description, а в манифесте файла скрипта — .DESCRIPTION. | Да | Да |
| **Запрос на принятие условий лицензии** | Возможно, для работы с модулем пользователю потребуется принять условия лицензии, установив в манифесте модуля RequireLicenseAcceptance = $true, задав LicenseURI и указав файл license.txt в корневой папке модуля. Дополнительные сведения можно найти в статье о [запросе на принятие условий лицензии](https://docs.microsoft.com/en-us/powershell/gallery/psgallery/psgallery_requires_license_acceptance). | Да | Нет |
| **Заметки о выпуске** | Для модулей эта информация извлекается из раздела ReleaseNotes в PSData\PrivateData. В манифестах скрипта это элемент RELEASENOTES. | Да | Да |
| **Владельцы** | Владельцы — это список пользователей в коллекции PowerShell, которые могут обновить элемент. Список владельцев не входит в манифест элементов. Дополнительные сведения см. в документации по [управлению владельцами элементов](https://docs.microsoft.com/en-us/powershell/gallery/psgallery/managing-item-owners). | Нет | Нет |
| **Автор** | Это Author в манифесте модуля и .AUTHOR в манифесте скрипта. Поле автора часто используется для указания компании или организации, связанной с элементом. | Да | Да |
| **Авторские права** | Это поле Copyright в манифесте модуля и .COPYRIGHT в манифесте скрипта. | Да | Да |
| **Список файлов** | Список файлов извлекается из пакета при публикации в коллекции PowerShell. Он не управляется данными манифеста. Примечание. Есть дополнительный файл .nuspec, который указывается с каждым элементом в коллекции PowerShell и отсутствует после установки элемента в системе. Это манифест пакета Nuget для элемента. Этот манифест можно игнорировать. | Нет | Нет |
| **Теги** | Для модулей теги содержатся в разделе PSData\PrivateData. Для сценариев раздел отмечен как .TAGS. Обратите внимание, что теги не могут содержать пробелы, даже если они стоят в кавычках. Для тегов есть дополнительные требования и значения, которые описаны далее в этой статье в разделе "Сведения о тегах". | Да | Да |
| **Командлеты** | Они указываются в манифесте модуля с помощью CmdletsToExport. Обратите внимание, что лучше явно указать элементы, чем использовать подстановочный знак "*", так как это повысит производительность загрузки модулей для пользователей. | Да | Нет |
| **Функции** | Указывается в манифесте модуля с помощью FunctionsToExport. Обратите внимание, что лучше явно указать элементы, чем использовать подстановочный знак "*", так как это повысит производительность загрузки модулей для пользователей. | Да | Нет |
| **Ресурсы DSC** | Для модулей, которые будут использоваться в PowerShell 5.0 и более поздних версий они указываются в манифесте с помощью DscResourcesToExport. Если модуль используется в PowerShell 4, DSCResourcesToExport нельзя применять, так как этот раздел манифеста не поддерживается (ресурсы DSC недоступны в версиях до PowerShell 4). | Да | Нет |
| **Рабочие процессы** | Рабочие процессы публикуются в коллекции PowerShell как скрипты и определяются как рабочие процессы (см. пример [Connect-AzureVM](https://www.powershellgallery.com/packages/Connect-AzureVM/1.0/Content/Connect-AzureVM.ps1)) в коде. Они не управляются манифестом. | Нет | Нет |
| **Возможности ролей** | Они будет указываться, если модуль, опубликованный в коллекции PowerShell, содержит один или более файлов возможностей ролей (PSRC), используемых с JEA. Дополнительные сведения см. в документации JEA по [возможностям ролей](https://docs.microsoft.com/en-us/powershell/jea/role-capabilities). | Да | Нет |
| **Выпуски PowerShell** | Они указываются в манифесте скрипта или модуля. Для модулей, которые предназначены для использования с PowerShell 5.0 и более ранних версий, элемент управляется тегами. В версии для компьютеров используйте PSEdition_Desktop, а для версии Core используйте PSEdition_Core. Для модулей, которые будут использоваться только в PowerShell 5.1 и более поздних версий, в основном манифесте нет раздела CompatiblePSEditions. Дополнительные сведения см. на странице функции выпуска PS в [документации PowerShell Get](https://docs.microsoft.com/en-us/powershell/gallery/psget/module/modulewithpseditionsupport). | Да | Да |
| **Зависимости** | Зависимости — это модули в коллекции PowerShell, объявленные в модуле как RequiredModules, а в манифесте скрипта как #Requires –Module (имя). | Да | Да |
| **Минимальная версия PowerShell** | Ее можно указать в манифесте модуля как PowerShellVersion | Да | Нет |
| **Журнал версий** | Журнал версий отражает обновления модуля в коллекции PowerShell. Если версия элемента скрыта с помощью функции Delete, она отобразится в журнале версий только для владельцев элемента. | Нет | Нет |
| **Сайт проекта** | Сайт проекта задается для модулей в разделе манифеста модуля Privatedata\PSData с указанием ProjectURI. В манифесте скрипта элемент указывается в .PROJECTURI. | Да | Да |
| **Лицензия** | Ссылка на лицензию задается для модулей в разделе манифеста модуля Privatedata\PSData с указанием ProjectURI. В манифесте скрипта элемент указывается в .LICENSEURI. Обратите внимание, что, если лицензия не задана с указанием LicenseURI или внутри модуля, условия использования для элемента определяются условиями использования для коллекции PowerShel. Дополнительные сведения см. в условиях использования. | Да | Да |

## <a name="editing-item-details"></a>Изменение сведений об элементе

Страница изменения элементов в коллекции PowerShell позволяет издателям изменять несколько полей, отображаемых для элемента, в частности:

- Title
- Описание
- Сводка
- URL-адрес значка;
- URL-адрес домашней страницы;
- Авторы
- Авторские права
- Теги
- Заметки о выпуске
- запрос на лицензию.

Такой подход обычно не рекомендуется, за исключением случаев, когда нужно исправить отображаемые данные для более ранней версии модуля.
Пользователи, получившие модуль, увидят, что метаданные не соответствует отображаемым в коллекции PowerShell, что может вызвать вопросы об элементе.
Это обычно приводит к подаче запросов на подтверждение изменений владельцам элементов.
Настоятельно рекомендуем каждый раз при использовании этого подхода публиковать новую версию элемента с теми же изменениями.

## <a name="tag-details"></a>Сведения о тегах

Теги — это простые строки, которые пользователи применяют для поиска элементов.
Теги особенно полезны при согласованном использовании с множеством элементов, относящихся к одному разделу. Использовать несколько вариантов одного и того же слова (например, database и databases или test и testing) не слишком целесообразно.
Теги — это строки из одного слова без учета регистра. Они не должны содержать пробелы. Если есть фраза, по которой, по вашему мнению, пользователи будут выполнять поиск, добавьте ее к описанию элемента. После этого она будет отображаться в результатах поиска. Улучшить восприятие текста можно при помощи регистра Pascal, дефисов, подчеркивания или точек. Старайтесь не создавать длинные, сложные и необычные теги, так как их часто пишут с ошибками.

Есть теги, на которые следует обратить особое внимание, так как в коллекции PowerShell и PowerShellGet они определяются индивидуально. К примерам относятся PSEdition_Desktop и PSEdition_Core, описанные выше.

Как указано выше, теги наиболее полезны, если они четко указаны и согласованно используются с множеством элементов.
Рекомендуем издателям, которые ищут удобные теги, самый простой подход — использовать галерею PowerShell.
Как правило, по запросу отображается много элементов и описаний, соответствующих вашим требованиям.

Для справки ниже приведены некоторые наиболее часто используемые теги по данным на 14.12.2017.
В некоторых случаях рядом с тегами приводятся похожие, но, возможно, менее идеальные варианты.
Рекомендуем использовать предпочтительный тег. Он будет сопровождаться меньшим уровнем несогласованности и даст лучше результаты поиска для пользователей.


| **Предпочтительный тег** | **Альтернативные варианты и примечания** |
| --- | --- |
| **Azure** |  |
| **DSC** | Тег DesiredStateConfiguration менее предпочтителен из-за большой длины. |
| **ResourceManager** | Тег ARM используется для описания группы процессоров. Его не следует применять для Azure Resource Manager. | **DSCResourceKit** |  |
| **SQL** |  |
| **AWS** |  |
| **DSCResource** |  |
| **Automation** |  |
| **REST** |  |
| **ActiveDirectory** | Сейчас тег AD не используется самостоятельно.  |
| **SQLServer** |  |
| **DBA** |  |
| **Безопасность** | Тег Defense не такой точный. |
| **Database** | Тег Databases (во множественном числе) менее предпочтителен. |
| **DevOps** |  |
| **Windows** |  |
| **Build** |  |
| **Deployment** | Тег Deploy используется немного реже. |
| **Cloud** |  |
| **GIT** |  |
| **Test** | Тег Testing менее предпочтителен. |
| **VersionControl** | Тег Version менее точный, хотя используется чаще.  |
| **Logging** | Если речь идет о действии, предпочтительнее использовать Logging. |
| **Log** | Если речь идет об объекте, предпочтительнее использовать Log. |
| **Backup** |  |
| **IaaS** |  |
| **Linux** |  |
| **IIS** |  |
| **AzureAutomation** |  |
| **Storage** |  |
| **GitHub** |  |
| **Json** |  |
| **Exchange** |  |
| **Network** | Тег Networking является аналогом, но используется реже. |
| **SharePoint** |  |
| **Reporting** | Для действия используется Reporting, для объекта — Report. |
| **Report** | Тег Report используется, если речь идет об объекте. |
| **WinRM** |  |
| **Monitoring** |  |
| **VSTS** |  |
| **Excel** |  |
| **Google** |  |
| **Color** |  |
| **DNS** |  |
| **Office365** | Предпочтительнее использовать полное слово Office. O365 используется реже, несмотря на то что этот тег короче. | **Gitlab** |  |
| **Pester** |  |
| **AzureAD** |  |
| **HTML** |  |
| **Hyper-V** | HyperV реже используется в качестве тега. |
| **Configuration** |  |
| **ChatOps** |  |
| **PackageManagement** |  |
| **WMI** |  |
| **Firewall** |  |
| **Docker** |  |
| **Appveyor** |  |
| **AzureRm** | Используется в основном для модулей AzureRM. |
| **Zip** |  |
| **MSI** |  |
| **Mac** |  |
| **PoshBot** |  |