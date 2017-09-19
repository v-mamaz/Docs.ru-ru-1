# <a name="adding-validation"></a><span data-ttu-id="da98b-101">Добавление проверки</span><span class="sxs-lookup"><span data-stu-id="da98b-101">Adding validation</span></span>

<span data-ttu-id="da98b-102">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="da98b-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="da98b-103">В этом разделе в модель `Movie` добавляется логика проверки. Также вы убедитесь, что правила проверки применяются каждый раз, когда пользователь создает или редактирует фильм.</span><span class="sxs-lookup"><span data-stu-id="da98b-103">In this section you'll add validation logic to the `Movie` model, and you'll ensure that the validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="keeping-things-dry"></a><span data-ttu-id="da98b-104">Соблюдение принципа "Не повторяйся"</span><span class="sxs-lookup"><span data-stu-id="da98b-104">Keeping things DRY</span></span>

<span data-ttu-id="da98b-105">Принцип [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself) (от английского "Don't Repeat Yourself" — не повторяйся) является одним из основополагающих принципов разработки в модели MVC.</span><span class="sxs-lookup"><span data-stu-id="da98b-105">One of the design tenets of MVC is [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself) ("Don't Repeat Yourself").</span></span> <span data-ttu-id="da98b-106">В модели ASP.NET MVC рекомендуется задавать функциональные возможности или поведение только один раз, а затем отражать их в других местах приложения.</span><span class="sxs-lookup"><span data-stu-id="da98b-106">ASP.NET MVC encourages you to specify functionality or behavior only once, and then have it be reflected everywhere in an app.</span></span> <span data-ttu-id="da98b-107">Это позволяет свести к минимуму объем кода, а также снизить риск возникновения в нем ошибок и упростить его тестирование и поддержку.</span><span class="sxs-lookup"><span data-stu-id="da98b-107">This reduces the amount of code you need to write and makes the code you do write less error prone, easier to test, and easier to maintain.</span></span>

<span data-ttu-id="da98b-108">Ярким примером применения принципа "Не повторяйся" является поддержка проверки, реализуемая в модели MVC и на платформе Entity Framework Core Code First.</span><span class="sxs-lookup"><span data-stu-id="da98b-108">The validation support provided by MVC and Entity Framework Core Code First is a good example of the DRY principle in action.</span></span> <span data-ttu-id="da98b-109">Правила проверки декларативно определяются в одном месте (в классе модели) и затем применяются в рамках всего приложения.</span><span class="sxs-lookup"><span data-stu-id="da98b-109">You can declaratively specify validation rules in one place (in the model class) and the rules are enforced everywhere in the app.</span></span>

## <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="da98b-110">Добавление правил проверки к модели фильма</span><span class="sxs-lookup"><span data-stu-id="da98b-110">Adding validation rules to the movie model</span></span>

<span data-ttu-id="da98b-111">Откройте файл *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="da98b-111">Open the *Movie.cs* file.</span></span> <span data-ttu-id="da98b-112">Класс DataAnnotations предоставляет набор встроенных атрибутов проверки, которые декларативно применяются к любому классу или свойству.</span><span class="sxs-lookup"><span data-stu-id="da98b-112">DataAnnotations provides a built-in set of validation attributes that you apply declaratively to any class or property.</span></span> <span data-ttu-id="da98b-113">(Кроме того, этот класс содержит атрибуты форматирования (такие как `DataType`), которые обеспечивают форматирование и не предназначены для проверки.)</span><span class="sxs-lookup"><span data-stu-id="da98b-113">(It also contains formatting attributes like `DataType` that help with formatting and don't provide any validation.)</span></span>

<span data-ttu-id="da98b-114">Обновите класс `Movie`, чтобы использовать преимущества встроенных атрибутов проверки `Required`, `StringLength`, `RegularExpression` и `Range`.</span><span class="sxs-lookup"><span data-stu-id="da98b-114">Update the `Movie` class to take advantage of the built-in `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

<span data-ttu-id="da98b-115">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="da98b-115">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]</span></span>

<span data-ttu-id="da98b-116">Атрибуты проверки определяют поведение для свойств модели, к которым они применяются.</span><span class="sxs-lookup"><span data-stu-id="da98b-116">The validation attributes specify behavior that you want to enforce on the model properties they are applied to.</span></span> <span data-ttu-id="da98b-117">Атрибуты `Required` и `MinimumLength` указывают, что свойство должно иметь значение. Тем не менее, чтобы удовлетворить требованиям проверки, пользователю достаточно ввести пробел.</span><span class="sxs-lookup"><span data-stu-id="da98b-117">The `Required` and `MinimumLength` attributes indicates that a property must have a value; but nothing prevents a user from entering white space to satisfy this validation.</span></span> <span data-ttu-id="da98b-118">Атрибут `RegularExpression` ограничивает набор допустимых для ввода символов.</span><span class="sxs-lookup"><span data-stu-id="da98b-118">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="da98b-119">В приведенном выше коде в полях `Genre` и `Rating` можно использовать только буквы (пробелы, числа и специальные символы не допускаются).</span><span class="sxs-lookup"><span data-stu-id="da98b-119">In the code above, `Genre` and `Rating` must use only letters (white space, numbers and special characters are not allowed).</span></span> <span data-ttu-id="da98b-120">Атрибут `Range` ограничивает диапазон значений.</span><span class="sxs-lookup"><span data-stu-id="da98b-120">The `Range` attribute constrains a value to within a specified range.</span></span> <span data-ttu-id="da98b-121">Атрибут `StringLength` позволяет задать максимальную и при необходимости минимальную длину строкового свойства.</span><span class="sxs-lookup"><span data-stu-id="da98b-121">The `StringLength` attribute lets you set the maximum length of a string property, and optionally its minimum length.</span></span> <span data-ttu-id="da98b-122">Типы значений (например, `decimal`, `int`, `float`, `DateTime`) по своей природе являются обязательными и не требуют атрибута `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="da98b-122">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="da98b-123">Наличие правил проверки, которые автоматически применяются ASP.NET, помогает повысить степень надежности приложения.</span><span class="sxs-lookup"><span data-stu-id="da98b-123">Having validation rules automatically enforced by ASP.NET helps make your app more robust.</span></span> <span data-ttu-id="da98b-124">Это также гарантирует, что в любом случае будут выполнены все проверки и в базе данных не будут случайно оставлены поврежденные данные.</span><span class="sxs-lookup"><span data-stu-id="da98b-124">It also ensures that you can't forget to validate something and inadvertently let bad data into the database.</span></span>

## <a name="validation-error-ui-in-mvc"></a><span data-ttu-id="da98b-125">Пользовательский интерфейс проверки ошибок в модели MVC</span><span class="sxs-lookup"><span data-stu-id="da98b-125">Validation Error UI in MVC</span></span>

<span data-ttu-id="da98b-126">Запустите приложение и перейдите к контроллеру фильмов.</span><span class="sxs-lookup"><span data-stu-id="da98b-126">Run the app and navigate to the Movies controller.</span></span>

<span data-ttu-id="da98b-127">Коснитесь ссылки **Create New** (Создать), чтобы добавить новый фильм.</span><span class="sxs-lookup"><span data-stu-id="da98b-127">Tap the **Create New** link to add a new movie.</span></span> <span data-ttu-id="da98b-128">Введите в форму какие-либо недопустимые значения.</span><span class="sxs-lookup"><span data-stu-id="da98b-128">Fill out the form with some invalid values.</span></span> <span data-ttu-id="da98b-129">Если функция проверки jQuery на стороне клиента обнаруживает ошибку, сведения о ней отображаются в соответствующем сообщении.</span><span class="sxs-lookup"><span data-stu-id="da98b-129">As soon as jQuery client side validation detects the error, it displays an error message.</span></span>

![Форма просмотра фильма с несколькими ошибками проверки jQuery на стороне клиента](../../tutorials/first-mvc-app/validation/_static/val.png)

> [!NOTE]
> <span data-ttu-id="da98b-131">В поле `Price` нельзя вводить десятичные точки или запятые.</span><span class="sxs-lookup"><span data-stu-id="da98b-131">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="da98b-132">Чтобы обеспечить поддержку [проверки jQuery](http://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (","), а для отображения данных в форматах для других языков, кроме английского, выполните действия, необходимые для глобализации вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="da98b-132">To support [jQuery validation](http://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="da98b-133">Дополнительные сведения см. в разделе [Дополнительные ресурсы](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="da98b-133">See [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="da98b-134">А пока вводите целые числа, такие как 10.</span><span class="sxs-lookup"><span data-stu-id="da98b-134">For now, just enter whole numbers like 10.</span></span>

<span data-ttu-id="da98b-135">Обратите внимание, что для каждого поля, содержащего недопустимое значение, в форме автоматически отображается соответствующее сообщение об ошибке проверки.</span><span class="sxs-lookup"><span data-stu-id="da98b-135">Notice how the form has automatically rendered an appropriate validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="da98b-136">Эти ошибки применяются как на стороне клиента (с помощью JavaScript и jQuery), так и на стороне сервера (если пользователь отключает JavaScript).</span><span class="sxs-lookup"><span data-stu-id="da98b-136">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (in case a user has JavaScript disabled).</span></span>

<span data-ttu-id="da98b-137">Серьезное преимущество заключается в том, что для реализации этого пользовательского интерфейса проверки не требуется изменять код в классе `MoviesController` или представлении *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="da98b-137">A significant benefit is that you didn't need to change a single line of code in the `MoviesController` class or in the *Create.cshtml* view in order to enable this validation UI.</span></span> <span data-ttu-id="da98b-138">В контроллере и представлениях, создаваемых в рамках этого руководства, автоматически применяются правила проверки, для определения которых к свойствам класса модели `Movie` были применены атрибуты.</span><span class="sxs-lookup"><span data-stu-id="da98b-138">The controller and views you created earlier in this tutorial automatically picked up the validation rules that you specified by using validation attributes on the properties of the `Movie` model class.</span></span> <span data-ttu-id="da98b-139">При проверке с использованием метода действия `Edit` применяются те же правила.</span><span class="sxs-lookup"><span data-stu-id="da98b-139">Test validation using the `Edit` action method, and the same validation is applied.</span></span>

<span data-ttu-id="da98b-140">Данные формы передаются на сервер только после того, как будут устранены любые ошибки на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="da98b-140">The form data is not sent to the server until there are no client side validation errors.</span></span> <span data-ttu-id="da98b-141">Чтобы проверить это, установите точку останова в методе `HTTP Post` с помощью [инструмента Fiddler](http://www.telerik.com/fiddler) или [средств разработчика F12](https://dev.windows.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span><span class="sxs-lookup"><span data-stu-id="da98b-141">You can verify this by putting a break point in the `HTTP Post` method, by using the [Fiddler tool](http://www.telerik.com/fiddler) , or the [F12 Developer tools](https://dev.windows.com/microsoft-edge/platform/documentation/f12-devtools-guide/).</span></span>

## <a name="how-validation-works"></a><span data-ttu-id="da98b-142">Принципы работы проверки</span><span class="sxs-lookup"><span data-stu-id="da98b-142">How validation works</span></span>

<span data-ttu-id="da98b-143">Вам может быть интересно, как пользовательский интерфейс проверки создается без обновления кода контроллера или представлений.</span><span class="sxs-lookup"><span data-stu-id="da98b-143">You might wonder how the validation UI was generated without any updates to the code in the controller or views.</span></span> <span data-ttu-id="da98b-144">В следующем примере кода показаны два метода `Create`.</span><span class="sxs-lookup"><span data-stu-id="da98b-144">The following code shows the two `Create` methods.</span></span>

<span data-ttu-id="da98b-145">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Controllers/MoviesController.cs?name=snippetCreate)]</span><span class="sxs-lookup"><span data-stu-id="da98b-145">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Controllers/MoviesController.cs?name=snippetCreate)]</span></span>

<span data-ttu-id="da98b-146">Первый метод действия `Create` (HTTP GET) отображает исходную форму создания.</span><span class="sxs-lookup"><span data-stu-id="da98b-146">The first (HTTP GET) `Create` action method displays the initial Create form.</span></span> <span data-ttu-id="da98b-147">Вторая версия (`[HttpPost]`) обрабатывает передачу формы.</span><span class="sxs-lookup"><span data-stu-id="da98b-147">The second (`[HttpPost]`) version handles the form post.</span></span> <span data-ttu-id="da98b-148">Второй метод `Create` (версия `[HttpPost]`) вызывает `ModelState.IsValid`, который определяет наличие ошибок проверки в фильме.</span><span class="sxs-lookup"><span data-stu-id="da98b-148">The second `Create` method (The `[HttpPost]` version) calls `ModelState.IsValid` to check whether the movie has any validation errors.</span></span> <span data-ttu-id="da98b-149">При вызове этого метода оцениваются все атрибуты проверки, которые были применены к объекту.</span><span class="sxs-lookup"><span data-stu-id="da98b-149">Calling this method evaluates any validation attributes that have been applied to the object.</span></span> <span data-ttu-id="da98b-150">При наличии ошибок проверки в объекте метод `Create` повторно отображает форму.</span><span class="sxs-lookup"><span data-stu-id="da98b-150">If the object has validation errors, the `Create` method re-displays the form.</span></span> <span data-ttu-id="da98b-151">Если ошибок нет, метод сохраняет новый фильм в базе данных.</span><span class="sxs-lookup"><span data-stu-id="da98b-151">If there are no errors, the method saves the new movie in the database.</span></span> <span data-ttu-id="da98b-152">В этом примере форма передается на сервер только после того, как будут устранены все ошибки проверки, обнаруженные на стороне клиента. Второй метод `Create` не вызывается до тех пор, пока на стороне клиента присутствуют ошибки проверки.</span><span class="sxs-lookup"><span data-stu-id="da98b-152">In our movie example, the form is not posted to the server when there are validation errors detected on the client side; the second `Create` method is never called when there are client side validation errors.</span></span> <span data-ttu-id="da98b-153">При отключении JavaScript в браузере также отключается проверка на стороне клиента. В этом случае вы можете протестировать метод HTTP POST `Create` `ModelState.IsValid`, который обнаруживает наличие ошибок проверки.</span><span class="sxs-lookup"><span data-stu-id="da98b-153">If you disable JavaScript in your browser, client validation is disabled and you can test the HTTP POST `Create` method `ModelState.IsValid` detecting any validation errors.</span></span>

<span data-ttu-id="da98b-154">Вы можете установить точку останова в метод `[HttpPost] Create` и убедиться, что он не вызывается и данные формы не передаются, если на стороне клиента присутствуют ошибки проверки.</span><span class="sxs-lookup"><span data-stu-id="da98b-154">You can set a break point in the `[HttpPost] Create` method and verify the method is never called, client side validation will not submit the form data when validation errors are detected.</span></span> <span data-ttu-id="da98b-155">Если отключить JavaScript в браузере и отправить форму с ошибками, будет достигнута точка останова.</span><span class="sxs-lookup"><span data-stu-id="da98b-155">If you disable JavaScript in your browser, then submit the form with errors, the break point will be hit.</span></span> <span data-ttu-id="da98b-156">Без JavaScript вы по-прежнему будете получать полную проверку.</span><span class="sxs-lookup"><span data-stu-id="da98b-156">You still get full validation without JavaScript.</span></span> 

<span data-ttu-id="da98b-157">На следующем рисунке показано, как отключить JavaScript в браузере FireFox.</span><span class="sxs-lookup"><span data-stu-id="da98b-157">The following image shows how to disable JavaScript in the FireFox browser.</span></span>

![Firefox: в разделе "Настройки" на вкладке "Содержимое" снимите флажок "Включить Javascript".](../../tutorials/first-mvc-app/validation/_static/ff.png)

<span data-ttu-id="da98b-159">На следующем рисунке показано, как отключить JavaScript в браузере Chrome.</span><span class="sxs-lookup"><span data-stu-id="da98b-159">The following image shows how to disable JavaScript in the Chrome browser.</span></span>

![Google Chrome: в разделе Javascript меню "Настройки контента" выберите "Запретить сайтам использовать JavaScript".](../../tutorials/first-mvc-app/validation/_static/chrome.png)

<span data-ttu-id="da98b-161">После отключения JavaScript передайте недопустимые данные и запустите отладку в пошаговом режиме.</span><span class="sxs-lookup"><span data-stu-id="da98b-161">After you disable JavaScript, post invalid data and step through the debugger.</span></span>

![Отладка Intellisense во время передачи недопустимых данных показывает, что атрибут ModelState.IsValid имеет значение false.](../../tutorials/first-mvc-app/validation/_static/ms.png)

<span data-ttu-id="da98b-163">Ниже демонстрируется часть шаблона представления *Create.cshtml*, сформированного ранее в рамках этого руководства.</span><span class="sxs-lookup"><span data-stu-id="da98b-163">Below is portion of the *Create.cshtml* view template that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="da98b-164">Он используется в показанных выше методах действия для отображения исходной формы и повторного вывода формы в случае ошибки.</span><span class="sxs-lookup"><span data-stu-id="da98b-164">It's used by the action methods shown above both to display the initial form and to redisplay it in the event of an error.</span></span>

<span data-ttu-id="da98b-165">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Views/Movies/CreateRatingBrevity.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="da98b-165">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Views/Movies/CreateRatingBrevity.cshtml)]</span></span>

<span data-ttu-id="da98b-166">[Вспомогательная функция тега Input](xref:mvc/views/working-with-forms) использует атрибуты [DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) и создает HTML-атрибуты, необходимые для проверки jQuery на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="da98b-166">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) attributes and produces HTML attributes needed for jQuery Validation on the client side.</span></span> <span data-ttu-id="da98b-167">[Вспомогательная функция тега Validation](xref:mvc/views/working-with-forms#the-validation-tag-helpers) отображает ошибки проверки.</span><span class="sxs-lookup"><span data-stu-id="da98b-167">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="da98b-168">Дополнительные сведения см. в разделе [Проверка](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="da98b-168">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="da98b-169">Этот подход удобен тем, что ни контроллер, ни шаблон представления `Create` ничего не знают о фактически применяемых правилах проверки или отображаемых сообщениях об ошибках.</span><span class="sxs-lookup"><span data-stu-id="da98b-169">What's really nice about this approach is that neither the controller nor the `Create` view template knows anything about the actual validation rules being enforced or about the specific error messages displayed.</span></span> <span data-ttu-id="da98b-170">Правила проверки и строки ошибок указываются только в классе `Movie`.</span><span class="sxs-lookup"><span data-stu-id="da98b-170">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="da98b-171">Такие же правила проверки автоматически применяются к представлению `Edit` и любым другим представлениям модели, которые вы можете создавать или редактировать.</span><span class="sxs-lookup"><span data-stu-id="da98b-171">These same validation rules are automatically applied to the `Edit` view and any other views templates you might create that edit your model.</span></span>

<span data-ttu-id="da98b-172">При необходимости вы можете изменить логику проверки в одном месте, добавив атрибуты проверки в модель (в этом примере — в класс `Movie`).</span><span class="sxs-lookup"><span data-stu-id="da98b-172">When you need to change validation logic, you can do so in exactly one place by adding validation attributes to the model (in this example, the `Movie` class).</span></span> <span data-ttu-id="da98b-173">Вам не придется беспокоиться о несогласованности применения правил в различных частях приложения, поскольку вся логика проверки будет определена в одном месте и начнет применяться по всему приложению.</span><span class="sxs-lookup"><span data-stu-id="da98b-173">You won't have to worry about different parts of the application being inconsistent with how the rules are enforced — all validation logic will be defined in one place and used everywhere.</span></span> <span data-ttu-id="da98b-174">Это позволяет максимально оптимизировать код и обеспечить удобство его совершенствования и поддержки.</span><span class="sxs-lookup"><span data-stu-id="da98b-174">This keeps the code very clean, and makes it easy to maintain and evolve.</span></span> <span data-ttu-id="da98b-175">Кроме того, таким образом вы будете полностью соблюдать требования принципа "Не повторяйся".</span><span class="sxs-lookup"><span data-stu-id="da98b-175">And it means that you'll be fully honoring the DRY principle.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="da98b-176">Использование атрибутов DataType</span><span class="sxs-lookup"><span data-stu-id="da98b-176">Using DataType Attributes</span></span>

<span data-ttu-id="da98b-177">Откройте файл *Movie.cs* и проверьте класс `Movie`.</span><span class="sxs-lookup"><span data-stu-id="da98b-177">Open the *Movie.cs* file and examine the `Movie` class.</span></span> <span data-ttu-id="da98b-178">В пространстве имен `System.ComponentModel.DataAnnotations` в дополнение к набору встроенных атрибутов проверки предоставляются атрибуты форматирования.</span><span class="sxs-lookup"><span data-stu-id="da98b-178">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="da98b-179">К полям с датой выпуска и ценой уже применено значение перечисления `DataType`.</span><span class="sxs-lookup"><span data-stu-id="da98b-179">We've already applied a `DataType` enumeration value to the release date and to the price fields.</span></span> <span data-ttu-id="da98b-180">В следующем коде показаны свойства `ReleaseDate` и `Price` с соответствующим атрибутом `DataType`.</span><span class="sxs-lookup"><span data-stu-id="da98b-180">The following code shows the `ReleaseDate` and `Price` properties with the appropriate `DataType` attribute.</span></span>

<span data-ttu-id="da98b-181">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="da98b-181">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]</span></span>

<span data-ttu-id="da98b-182">Атрибуты `DataType` предоставляют модулю просмотра только рекомендации по форматированию данных, а также другие атрибуты, например `<a>` для URL-адресов и `<a href="mailto:EmailAddress.com">` для электронной почты.</span><span class="sxs-lookup"><span data-stu-id="da98b-182">The `DataType` attributes only provide hints for the view engine to format the data (and supply attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email.</span></span> <span data-ttu-id="da98b-183">Используйте атрибут `RegularExpression` для проверки формата данных.</span><span class="sxs-lookup"><span data-stu-id="da98b-183">You can use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="da98b-184">Атрибут `DataType` позволяет указать тип данных с более точным определением относительно встроенного типа базы данных, но не предназначен для проверки.</span><span class="sxs-lookup"><span data-stu-id="da98b-184">The `DataType` attribute is used to specify a data type that is more specific than the database intrinsic type, they are not validation attributes.</span></span> <span data-ttu-id="da98b-185">В этом случае требуется отслеживать только дату, но не время.</span><span class="sxs-lookup"><span data-stu-id="da98b-185">In this case we only want to keep track of the date, not the time.</span></span> <span data-ttu-id="da98b-186">В перечислении `DataType` представлено множество типов данных, таких как Date, Time, PhoneNumber, Currency, EmailAddress и другие.</span><span class="sxs-lookup"><span data-stu-id="da98b-186">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress and more.</span></span> <span data-ttu-id="da98b-187">Атрибут `DataType` также обеспечивает автоматическое предоставление функций для определенных типов в приложении.</span><span class="sxs-lookup"><span data-stu-id="da98b-187">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="da98b-188">Например, может быть создана ссылка `mailto:` для `DataType.EmailAddress`. Также в браузерах с поддержкой HTML5 может быть предоставлен селектор даты для `DataType.Date`.</span><span class="sxs-lookup"><span data-stu-id="da98b-188">For example, a `mailto:` link can be created for `DataType.EmailAddress`, and a date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="da98b-189">Атрибуты `DataType` создают атрибуты HTML 5 `data-`, которые используются браузерами с поддержкой HTML 5.</span><span class="sxs-lookup"><span data-stu-id="da98b-189">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="da98b-190">Атрибуты `DataType` **не предназначены** для проверки.</span><span class="sxs-lookup"><span data-stu-id="da98b-190">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="da98b-191">`DataType.Date` не задает формат отображаемой даты.</span><span class="sxs-lookup"><span data-stu-id="da98b-191">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="da98b-192">По умолчанию поле данных отображается с использованием форматов, установленных в параметрах `CultureInfo` сервера.</span><span class="sxs-lookup"><span data-stu-id="da98b-192">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

<span data-ttu-id="da98b-193">С помощью атрибута `DisplayFormat` можно явно указать формат даты:</span><span class="sxs-lookup"><span data-stu-id="da98b-193">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="da98b-194">Параметр `ApplyFormatInEditMode` указывает, что формат также должен применяться при отображении значения в текстовом поле для редактирования.</span><span class="sxs-lookup"><span data-stu-id="da98b-194">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="da98b-195">(В некоторых случаях такое поведение нежелательно. Например, в текстовом поле для редактирования денежных значений обычно не требуется отображать символ валюты.)</span><span class="sxs-lookup"><span data-stu-id="da98b-195">(You might not want that for some fields — for example, for currency values, you probably do not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="da98b-196">Атрибут `DisplayFormat` может использоваться отдельно, однако чаще всего его рекомендуется применять вместе с атрибутом `DataType`.</span><span class="sxs-lookup"><span data-stu-id="da98b-196">You can use the `DisplayFormat` attribute by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="da98b-197">Атрибут `DataType` передает семантику данных (в отличие от способа их вывода на экран) и дает следующие преимущества по сравнению с атрибутом DisplayFormat:</span><span class="sxs-lookup"><span data-stu-id="da98b-197">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="da98b-198">Поддержка функций HTML5 в браузере (отображение элемента управления календарем, соответствующего языковому стандарту символа валюты, ссылок электронной почты и т. д.)</span><span class="sxs-lookup"><span data-stu-id="da98b-198">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>

* <span data-ttu-id="da98b-199">По умолчанию формат отображения данных в браузере определяется в соответствии с установленным языковым стандартом.</span><span class="sxs-lookup"><span data-stu-id="da98b-199">By default, the browser will render data using the correct format based on your locale.</span></span>

* <span data-ttu-id="da98b-200">С помощью атрибута `DataType` модель MVC может выбрать подходящий шаблон поля для отображения данных (`DisplayFormat` при отдельном использовании базируется на строковом шаблоне).</span><span class="sxs-lookup"><span data-stu-id="da98b-200">The `DataType` attribute can enable MVC to choose the right field template to render the data (the `DisplayFormat` if used by itself uses the string template).</span></span>

> [!NOTE]
> <span data-ttu-id="da98b-201">Проверка jQuery не работает с атрибутами `Range` и `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="da98b-201">jQuery validation does not work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="da98b-202">Например, следующий код всегда приводит к возникновению ошибки проверки на стороне клиента, даже если дата попадает в указанный диапазон:</span><span class="sxs-lookup"><span data-stu-id="da98b-202">For example, the following code will always display a client side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="da98b-203">Чтобы использовать атрибут `Range` с `DateTime`, необходимо отключить проверку дат jQuery.</span><span class="sxs-lookup"><span data-stu-id="da98b-203">You will need to disable jQuery date validation to use the `Range` attribute with `DateTime`.</span></span> <span data-ttu-id="da98b-204">Как правило, не рекомендуется компилировать модели с фиксированными датами, поэтому использовать атрибуты `Range` и `DateTime` следует крайне осторожно.</span><span class="sxs-lookup"><span data-stu-id="da98b-204">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="da98b-205">В следующем коде демонстрируется объединение атрибутов в одной строке:</span><span class="sxs-lookup"><span data-stu-id="da98b-205">The following code shows combining attributes on one line:</span></span>

<span data-ttu-id="da98b-206">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="da98b-206">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]</span></span>

<span data-ttu-id="da98b-207">В следующей части этой серии мы рассмотрим приложение и внесем ряд изменений в автоматически создаваемые методы `Details` и `Delete`.</span><span class="sxs-lookup"><span data-stu-id="da98b-207">In the next part of the series, we'll review the application and make some improvements to the automatically generated `Details` and `Delete` methods.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="da98b-208">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="da98b-208">Additional resources</span></span>

* [<span data-ttu-id="da98b-209">Работа с формами</span><span class="sxs-lookup"><span data-stu-id="da98b-209">Working with Forms</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="da98b-210">Глобализация и локализация</span><span class="sxs-lookup"><span data-stu-id="da98b-210">Globalization and localization</span></span>](xref:fundamentals/localization)
* [<span data-ttu-id="da98b-211">Общие сведения о вспомогательных функциях тегов</span><span class="sxs-lookup"><span data-stu-id="da98b-211">Introduction to Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="da98b-212">Создание вспомогательных функций тегов</span><span class="sxs-lookup"><span data-stu-id="da98b-212">Authoring Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)