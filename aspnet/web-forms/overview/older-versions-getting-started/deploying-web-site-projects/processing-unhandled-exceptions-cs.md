---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
title: "Обработка необработанных исключений (C#) | Документы Microsoft"
author: rick-anderson
description: "При возникновении ошибки времени выполнения веб-приложения в рабочей среде, важно для уведомления разработчик и ошибку в журнале, чтобы она может обнаружить в a la..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 5bc1afd5-2484-4528-b158-ab218ba150e8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: 4c7f15053ca035a1df1222f88752b8243808bef0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="processing-unhandled-exceptions-c"></a>Обработка необработанных исключений (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить код](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_12_CS.zip) или [скачать PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial12_ErrorHandling_cs.pdf)

> При возникновении ошибки времени выполнения веб-приложения в рабочей среде, важно для уведомления разработчик и ошибку в журнале, чтобы он может обнаружить позднее времени. Этот учебник содержит общие сведения о том, как ASP.NET обрабатывает ошибки времени выполнения и рассматривает один способ выполнения каждый раз, когда необработанное исключение пузырьки ASP.NET во время выполнения пользовательского кода.


## <a name="introduction"></a>Вступление

При возникновении необработанного исключения в приложении ASP.NET передает вверх до выполнения ASP.NET, которая вызывает `Error` событий и отображает страницу соответствующее сообщение об ошибке. Существует три разных типа страниц ошибок: среда выполнения ошибки желтый экрана из смерти (YSOD); сведения об исключении YSOD; и страниц настраиваемых ошибок. В [предыдущего учебника](displaying-a-custom-error-page-cs.md) настраивался приложению использовать собственную страницу ошибки для удаленных пользователей и YSOD сведения об исключении для пользователей, посещающие локально.

С помощью понятную собственную страницу ошибки, соответствующий внешний вид сайта предпочтительнее YSOD ошибка среды выполнения по умолчанию, но отображение собственную страницу ошибки является только часть комплексного решения ошибок. При возникновении ошибки в приложении в рабочей среде, важно, что разработчики уведомление об ошибке, чтобы они новые причины возникновения исключения и устранить ее. Кроме того сведения об ошибке должно регистрироваться, чтобы проверить и установлена на более позднее время ошибки.

В этом учебнике показано, как получить доступ к сведения о возникновении необработанного исключения, чтобы они могут регистрироваться и разработчик уведомления. Два учебника, который после этого класса, изучите ошибки ведения журнала библиотеки, которые после небольшой конфигурации, автоматически уведомление разработчиков о ошибки времени выполнения и регистрировать их данные.

> [!NOTE]
> Сведения, которые проверяются в этом учебнике наиболее полезен, если требуется обработать необработанные исключения каким-либо образом уникальный или настроенные. В случаях, где требуется только для исключений в журнале и уведомления разработчик используется библиотека ведения журнала ошибок является наилучшим вариантом. В следующих двух учебниках содержатся общие сведения о двух библиотек.


## <a name="executing-code-when-theerrorevent-is-raised"></a>Выполнение кода при`Error`события

События содержат объект механизм для сигнализации, что произошло что-нибудь интересное и другим объектом, чтобы выполнить код в ответ. Как разработчик ASP.NET вы привыкли решения с точки зрения события. Если вы хотите выполнять определенный код, при щелчке определенной кнопки, создайте обработчик событий для этой кнопки `Click` событий существует фрагмента кода. Учитывая, что среда выполнения ASP.NET вызывает его [ `Error` событие](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.error.aspx) всякий раз, когда происходит необработанное исключение, следует, что код для записи сведений об ошибках перейдет в обработчике событий. Но, как создать обработчик событий для `Error` событий?

`Error` Событий является одним из многих событий в [ `HttpApplication` класса](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.aspx) , вызываемых на определенных этапах конвейера HTTP в течение времени существования запроса. Например `HttpApplication` класса [ `BeginRequest` событий](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.beginrequest.aspx) возникает в начале каждого запроса; его [ `AuthenticateRequest` событие](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authenticaterequest.aspx) возникает, когда модуль безопасности определил запрашивающей стороне. Эти `HttpApplication` события предоставляют разработчику страницы средства для выполнения пользовательской логики в различные моменты времени жизни запроса.

Обработчики событий для `HttpApplication` события может быть помещен в специальный файл с именем `Global.asax`. Чтобы создать этот файл в веб-сайт, добавить новый элемент в корневой каталог веб-сайта с помощью шаблона глобальный класс приложения с именем `Global.asax`.

[![](processing-unhandled-exceptions-cs/_static/image2.png)](processing-unhandled-exceptions-cs/_static/image1.png)

**Рис. 1**: добавление `Global.asax` в веб-приложение  
([Просмотр полноразмерное изображение](processing-unhandled-exceptions-cs/_static/image3.png))

Содержимое и структура `Global.asax` отличаются немного зависимости от того, используется ли проект веб-приложения (WAP) или проекта веб-сайта (WSP) файл, созданный средой Visual Studio. С WAP `Global.asax` реализуется как два отдельных файла - `Global.asax` и `Global.asax.cs`. `Global.asax` Ничего не содержит файл, но `@Application` директиву, которая ссылается на `.cs` файла; события, определенные обработчики интерес в `Global.asax.cs` файла. Для WSPs, создается только один файл, `Global.asax`, и обработчики событий определяются в `<script runat="server">` блока.

`Global.asax` Файл, созданный в WAP шаблоном глобальный класс приложения Visual Studio включает в себя обработчики событий с именем `Application_BeginRequest`, `Application_AuthenticateRequest`, и `Application_Error`, которые являются обработчики событий для `HttpApplication` события `BeginRequest`, `AuthenticateRequest`, и `Error`соответственно. Существуют также обработчики событий с именем `Application_Start`, `Session_Start`, `Application_End`, и `Session_End`, которые являются обработчики событий, которые срабатывают при запуске веб-приложения, при запуске нового сеанса при завершении работы приложения и при завершении сеанса соответственно. `Global.asax` Файл, созданный в WSP-файла в Visual Studio содержит только `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`, и `Session_End` обработчики событий.

> [!NOTE]
> При развертывании приложения ASP.NET, то необходимо скопировать `Global.asax` файл в рабочую среду. `Global.asax.cs` Файл, который создается в WAP, необходимо скопировать в рабочей среде, так как этот код компилируется в сборку проекта.


Обработчики событий, созданных с использованием шаблона глобальный класс приложения Visual Studio не являются исчерпывающими. Можно добавить обработчик событий для любого `HttpApplication` событие путем именования в обработчике событий `Application_EventName`. Например, можно добавить следующий код в `Global.asax` файл, чтобы создать обработчик событий для [ `AuthorizeRequest` событие](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-cs/samples/sample1.vb)]

Аналогично можно удалить любой обработчика событий, созданного с помощью шаблона глобальный класс приложения, которые не требуются. В этом учебнике мы нужны только обработчик событий для `Error` событий; можно удалить другие обработчики событий из `Global.asax` файла.

> [!NOTE]
> *Модули HTTP* другим способом для определения обработчиков событий для `HttpApplication` события. Модули HTTP, создаются как файл класса, можно непосредственно в проект веб-приложения или выделены в отдельной библиотеке классов. Так как их можно разделить на библиотеку классов, модули HTTP предлагают более гибкая и многократно используемая модель для создания `HttpApplication` обработчики событий. В то время как `Global.asax` файл относится к веб-приложению, где она находится, модули HTTP может быть скомпилирован в сборках, на этом этапе модуль HTTP к веб-сайта добавляется простыми, как удаление сборки `Bin` папки и регистрация Модуль в `Web.config`. Этот учебник не рассмотрим создание и использование модули HTTP, но два ведения журнала ошибок библиотеки, используемые в двух следующих учебников реализуются как модули HTTP. Дополнительные сведения о преимуществах модули HTTP см. в разделе [с помощью HTTP-модули и обработчики, чтобы создать подключаемые компоненты ASP.NET](https://msdn.microsoft.com/en-us/library/aa479332.aspx).


## <a name="retrieving-information-about-the-unhandled-exception"></a>Получение сведений о необработанного исключения.

На этом этапе у нас есть файл Global.asax с `Application_Error` обработчика событий. При выполнении этого обработчика событий необходимо уведомлять разработчика ошибки и регистрировать его данные. Для выполнения этих задач, необходимо сначала определить подробности исключения, которое было вызвано. Используйте объект сервера [ `GetLastError` метод](https://msdn.microsoft.com/en-us/library/system.web.httpserverutility.getlasterror.aspx) для получения сведений о необработанное исключение, вызвавшее `Error` срабатывание события.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample2.cs)]

`GetLastError` Метод возвращает объект типа `Exception`, который является базовым типом для всех исключений в .NET Framework. Тем не менее, в приведенном выше коде я приведение объекта исключения, возвращенного `GetLastError` в `HttpException` объекта. Если `Error` событие происходит потому, что произошло исключение при обработке ресурса ASP.NET и исключение, вызванное исключение упаковывается в `HttpException`. Чтобы получить фактическое исключение, ставшее причиной ошибки событий используйте `InnerException` свойство. Если `Error` события из-за исключения на основе HTTP, например запрос страницы несуществующий `HttpException` создается исключение, но не имеет внутреннего исключения.

В следующем коде используется `GetLastErrormessage` для получения сведений об исключении, запустившего `Error` событий, хранение `HttpException` в переменной с именем `lastErrorWrapper`. Он сохраняет тип, сообщение и трассировка стека исходного исключения в три строковые переменные, сопоставляя `lastErrorWrapper` — фактическое исключение, которое запускается `Error` события (в случае исключения по протоколу HTTP), или это просто Программа-оболочка для исключения, вызванное исключение при обработке запроса.

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample3.cs)]

В этот момент у вас есть все сведения, необходимые для написания кода, которая будет записывать сведения об исключении в таблице базы данных. Можно создать таблицу базы данных со столбцами для каждого из сведения об ошибке интерес - тип, сообщение, трассировка стека и т. д - вместе с другими важных сведений, таких как URL-адрес запрошенной страницы и имя пользователя, выполнившего вход. В `Application_Error` обработчик событий, затем вам потребуется подключиться к базе данных и вставить запись в таблицу. Аналогичным образом можно добавить код для создания предупреждения, разработчик ошибки по электронной почте.

Библиотеки Ошибка ведения журналов, проверяются в двух следующих учебников обеспечения возможности без дополнительной настройки, поэтому нет необходимости создавать этот ведения журнала ошибок и уведомления самостоятельно. Тем не менее чтобы проиллюстрировать, `Error` , возникает событие и что `Application_Error` обработчик событий можно использовать для входа сведения об ошибке и уведомлять разработчика, добавим код, который уведомляет разработчик, при возникновении ошибки.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Уведомление разработчик, при возникновении необработанного исключения

При возникновении необработанного исключения в рабочей среде, важно для предупреждения разработчиков, чтобы они могут оценить ошибки и определить, какие действия необходимо выполнить. Например при наличии ошибки в подключении к базе данных, вам потребуется двойные Проверьте строки подключения и, возможно, в службу поддержки веб-хостинга компанию. Если исключение возникло из-за ошибки программирования, дополнительного кода или логики проверки может потребоваться добавить избежать таких ошибок в будущем.

Классы .NET Framework в [ `System.Net.Mail` имен](https://msdn.microsoft.com/en-us/library/system.net.mail.aspx) позволяют легко отправить сообщение электронной почты. [ `MailMessage` Класса](https://msdn.microsoft.com/en-us/library/system.net.mail.mailmessage.aspx) представляет сообщение электронной почты и содержит свойства, такие как `To`, `From`, `Subject`, `Body`, и `Attachments`. `SmtpClass` Используется для отправки `MailMessage` объекта с использованием указанного сервера SMTP; параметры SMTP-сервера могут быть заданы программно или декларативно в [ `<system.net>` элемент](https://msdn.microsoft.com/en-us/library/6484zdc1.aspx) в `Web.config file`. Дополнительные сведения об отправке по электронной почте сообщения в приложении ASP.NET сведения, [отправки электронной почты в ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)и [System.Net.Mail часто задаваемые вопросы о](http://systemnetmail.com/).

> [!NOTE]
> `<system.net>` Элемент содержит параметры SMTP-сервера, используемые `SmtpClient` класса при отправке сообщения электронной почты. Веб-хостинга компанию, скорее всего, имеет SMTP-сервер, который можно использовать для отправки электронной почты из приложения. Сведения о параметры SMTP-сервера, который следует использовать в веб-приложении см в разделе поддержки на веб-узел.


Добавьте следующий код в `Application_Error` обработчик событий, чтобы отправить электронное письмо разработчик, при возникновении ошибки:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample4.cs)]

Приведенный выше код является довольно объемными, большая часть его создает HTML, отображаемый в сообщение, отправляемое для разработчика. Запускает код, ссылаясь на `HttpException` возвращенных `GetLastError` метод (`lastErrorWrapper`). Как именно исключение, возникшее в запросе извлекаются через `lastErrorWrapper.InnerException` и присваивается переменной `lastError`. Тип, сообщение и стек сведения трассировки, извлекается из `lastError` и хранятся в трех строковых переменных.

Далее, `MailMessage` объект с именем `mm` создается. Тело электронного сообщения в формате HTML и отображает URL-адрес запрошенной страницы, имя текущего пользователя, а также сведения об исключении (тип, сообщение и трассировка стека). Одно из интересного о `HttpException` класс — можно создать HTML-код, используемый для создания исключения сведений желтый экрана из смерти (YSOD) путем вызова [GetHtmlErrorMessage метод](https://msdn.microsoft.com/en-us/library/system.web.httpexception.gethtmlerrormessage.aspx). Этот метод используется здесь для получения разметки YSOD сведения об исключении и добавить его в виде вложения сообщения электронной почты. Предупреждение: Если на исключение, которая запустила `Error` событие произошло исключение на основе HTTP (например, запрос страницы не существуют) то `GetHtmlErrorMessage` метод будет возвращать `null`.

Последний шаг заключается в отправке `MailMessage`. Это делается путем создания нового `SmtpClient` метода и вызвать его `Send` метод.

> [!NOTE]
> Перед использованием этого кода в веб-приложения может потребоваться изменить значения в `ToAddress` и `FromAddress` константы из support@example.com на любой адрес электронной почты должны отправляться уведомление по электронной почте ошибки и получаются из. Также необходимо указать параметры SMTP-сервера в `<system.net>` раздела `Web.config`. Обратитесь к поставщику веб узла для определения параметры SMTP-сервера для использования.


В таком коде каждый раз, когда возникает ошибка разработчик отправляется сообщение электронной почты, ошибки, которые включают YSOD. В предыдущем учебнике мы показали ошибку во время выполнения, посетив Genre.aspx и передача в недопустимом `ID` значение через строку запроса, например `Genre.aspx?ID=foo`. Посетив страницу с `Global.asax` файла на месте формирует пользователям те же возможности, как в предыдущем учебнике - в среде разработки продолжаем для просмотра исключений сведения желтый экрана из смерти, пока в рабочей среде будет см. на странице настраиваемой ошибки. Помимо этого поведение существующей разработчик отправляется сообщение электронной почты.

**На рисунке 2** показано сообщение электронной почты, полученных при посещении `Genre.aspx?ID=foo`. Тело электронного сообщения представлены сведения об исключении, пока `YSOD.htm` вложения отображает содержимое, отображаемое в YSOD сведения об исключении (см. **рис. 3**).

[![](processing-unhandled-exceptions-cs/_static/image5.png)](processing-unhandled-exceptions-cs/_static/image4.png)

**На рисунке 2**: разработчик отправляется уведомление по электронной почте при возникновении необработанного исключения  
([Просмотр полноразмерное изображение](processing-unhandled-exceptions-cs/_static/image6.png))

[![](processing-unhandled-exceptions-cs/_static/image8.png)](processing-unhandled-exceptions-cs/_static/image7.png)

**Рис. 3**: уведомление по электронной почте содержит сведения об исключении YSOD как вложение  
([Просмотр полноразмерное изображение](processing-unhandled-exceptions-cs/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>Что делать с помощью настраиваемой страницы ошибки?

Этого учебника было показано, как использовать `Global.asax` и `Application_Error` обработчик событий для выполнения кода при возникновении необработанного исключения. В частности мы использовали этот обработчик событий для уведомления разработчиков ошибки; мы можете расширить ее также могут регистрировать сведения об ошибке в базе данных. Наличие `Application_Error` обработчик событий не влияет на взаимодействие с конечным пользователем. Они по-прежнему см. на странице ошибки настроенных YSOD сведения об ошибке, YSOD ошибки времени выполнения или настраиваемой страницы ошибки.

Логично, чтобы определить, является ли `Global.asax` файла и `Application_Error` событий необходим при использовании собственную страницу ошибки. Когда происходит ошибка, пользователь увидит настраиваемой страницы ошибки так почему не удается мы поместили код, чтобы уведомлять разработчика и сведения об ошибке войти класса кода программной части страницы настраиваемой ошибки? Хотя определенно можно добавить код для класса страницы настраиваемой ошибки кода программной нет доступа к сведениям о исключение, которое запускается `Error` событий при использовании методики, мы подробно рассмотрим в предыдущем учебнике. Вызов `GetLastError` из настраиваемой страницы ошибки методом `Nothing`.

Причиной такого поведения — поскольку настраиваемой страницы ошибки достигается посредством перенаправления. Когда необработанное исключение достигает среды выполнения ASP.NET ядро ASP.NET вызывает его `Error` событий (которого выполняет `Application_Error` обработчик событий) и затем *перенаправляет* пользователю на пользовательскую страницу ошибки, выполнив `Response.Redirect(customErrorPageUrl)`. `Response.Redirect` Метод отправляет ответ клиенту с кодом состояния HTTP 302, предписывая браузер, чтобы новый URL-адрес запроса, а именно на пользовательскую страницу ошибки. Затем браузер запрашивает автоматически эта новая страница. Можно определить, что настраиваемой страницы ошибки был запрошен отдельно на странице где возникла ошибка, поскольку для URL-адрес страницы настраиваемой ошибки в адресной строке браузера (в разделе **рис. 4**).

[![](processing-unhandled-exceptions-cs/_static/image11.png)](processing-unhandled-exceptions-cs/_static/image10.png)

**Рис. 4**: при возникновении браузер перенаправляется по URL-АДРЕСУ страницы настраиваемой ошибки  
([Просмотр полноразмерное изображение](processing-unhandled-exceptions-cs/_static/image12.png))

В итоге получается, что запрос, где возникло необработанное исключение заканчивается, когда сервер не ответит перенаправлением HTTP 302. Последующие запросы к настраиваемой страницы ошибки является абсолютно новым запросом; к этому моменту ASP.NET обработчик отброшено сведения об ошибке и Кроме того, не имеет возможности связать необработанное исключение в предыдущем запросе с новый запрос для настраиваемой страницы ошибки. Именно поэтому `GetLastError` возвращает `null` при вызове из настраиваемой страницы ошибки.

Тем не менее можно иметь настраиваемой страницы ошибки во время тот же запрос, который вызвал ошибку. [ `Server.Transfer(url)` ](https://msdn.microsoft.com/en-us/library/system.web.httpserverutility.transfer.aspx) Метод передает выполнение заданного URL-адрес и обрабатывает его в одном запросе. Можно переместить код `Application_Error` обработчик событий для класса с выделенным кодом страницы настраиваемой ошибки, заменив его в `Global.asax` следующим кодом:

[!code-csharp[Main](processing-unhandled-exceptions-cs/samples/sample5.cs)]

Когда происходит необработанное исключение `Application_Error` обработчик событий передает управление соответствующие настраиваемой страницы ошибки на основе кода состояния HTTP. Из-за переноса управления настраиваемой страницы ошибки имеет доступ к сведениям необработанное исключение через `Server.GetLastError` уведомлять разработчика ошибки и регистрировать его данные. `Server.Transfer` Вызов останавливает обработчик ASP.NET из перенаправления пользователя на пользовательскую страницу ошибки. Вместо этого содержимое страницы настраиваемой ошибки возвращаются в виде ответа на страницу, вызвавшего ошибку.

## <a name="summary"></a>Сводка

При возникновении необработанного исключения в веб-приложении ASP.NET среда выполнения ASP.NET вызывает `Error` событий и отображает страницу настроенных ошибок. Мы сообщите разработчику ошибки, регистрировать его данные или обрабатывает каким-либо другим образом, Создание обработчика событий для события ошибки. Существует два способа для создания обработчика событий для `HttpApplication` события, например `Error`: в `Global.asax` файла или из HTTP-модуля. Этого учебника было показано, как создать `Error` обработчик событий в `Global.asax` файл, который уведомляет разработчиков ошибки в сообщении электронной почты.

Создание `Error` полезным, если требуются для обработки необработанных исключений каким-либо образом уникальный или настроенные обработчик событий. Тем не менее, создание собственных `Error` обработчик событий исключений в журнале или для уведомления разработчик не является наиболее эффективно использовать часть времени, как уже существует ошибка свободного и простые в использовании ведения журнала библиотек, которые можно установить в течение нескольких минут. Два следующих учебников Изучите две библиотеки.

Программирование довольны!

### <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, рассматриваемые в этом учебнике см. в следующих ресурсах:

- [HTTP-модули ASP.NET и обзор обработчиков HTTP](https://support.microsoft.com/kb/307985)
- [Правильно отвечать на необработанные исключения - обработки необработанных исключений](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [`HttpApplication`Класс и объект приложения ASP.NET](http://www.eggheadcafe.com/articles/20030211.asp)
- [Обработчики HTTP-данных и модули HTTP в ASP.NET](http://www.15seconds.com/Issue/020417.htm)
- [При отправке сообщения в ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Основные сведения о `Global.asax` файла](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [С помощью HTTP-модули и обработчики для создания подключаемых компонентов ASP.NET](https://msdn.microsoft.com/en-us/library/aa479332.aspx)
- [Работа с ASP.NET `Global.asax` файла](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Работа с `HttpApplication` экземпляров](https://msdn.microsoft.com/en-us/library/a0xez8f2.aspx)

>[!div class="step-by-step"]
[Назад](displaying-a-custom-error-page-cs.md)
[Вперед](logging-error-details-with-asp-net-health-monitoring-cs.md)