<span data-ttu-id="cad62-101">Шаблон по умолчанию создает ссылки и страницы **RazorPagesMovie**, **Главная**, **О программе** и **Контакт**.</span><span class="sxs-lookup"><span data-stu-id="cad62-101">The default template creates **RazorPagesMovie**, **Home**, **About** and **Contact** links and pages.</span></span> <span data-ttu-id="cad62-102">В зависимости от размера окна браузера может потребоваться щелкнуть значок навигации, чтобы отобразить ссылки.</span><span class="sxs-lookup"><span data-stu-id="cad62-102">Depending on the size of your browser window, you might need to click the navigation icon to show the links.</span></span>

![Домашняя или индексная страница](../../tutorials/razor-pages/razor-pages-start/_static/home2.png)

<span data-ttu-id="cad62-104">Проверьте ссылки.</span><span class="sxs-lookup"><span data-stu-id="cad62-104">Test the links.</span></span> <span data-ttu-id="cad62-105">Ссылки **RazorPagesMovie** и **Главная** ведут на страницу индексов.</span><span class="sxs-lookup"><span data-stu-id="cad62-105">The **RazorPagesMovie** and **Home** links go to the Index page.</span></span> <span data-ttu-id="cad62-106">Ссылки **О программе** и **Контакт** указывают соответственно на страницы `About` и `Contact`.</span><span class="sxs-lookup"><span data-stu-id="cad62-106">The **About** and **Contact** links go to the `About` and `Contact` pages, respectively.</span></span>

## <a name="project-files-and-folders"></a><span data-ttu-id="cad62-107">Защита файлов и папок</span><span class="sxs-lookup"><span data-stu-id="cad62-107">Project files and folders</span></span>

<span data-ttu-id="cad62-108">В следующей таблице перечислены файлы и папки в проекте.</span><span class="sxs-lookup"><span data-stu-id="cad62-108">The following table lists the files and folders in the project.</span></span> <span data-ttu-id="cad62-109">В рамках этого учебника важнее всего разобраться с файлом *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="cad62-109">For this tutorial, the *Startup.cs* file is the most important to understand.</span></span> <span data-ttu-id="cad62-110">Каждую из указанных ниже ссылок просматривать не требуется.</span><span class="sxs-lookup"><span data-stu-id="cad62-110">You don't need to review each link provided below.</span></span> <span data-ttu-id="cad62-111">Ссылки приводятся для справки и позволяют получить дополнительную информацию о каком-либо файле или папке в проекте.</span><span class="sxs-lookup"><span data-stu-id="cad62-111">The links are provided as a reference when you need more information on a file or folder in the project.</span></span>

| <span data-ttu-id="cad62-112">Файл или папка</span><span class="sxs-lookup"><span data-stu-id="cad62-112">File or folder</span></span>              | <span data-ttu-id="cad62-113">Назначение</span><span class="sxs-lookup"><span data-stu-id="cad62-113">Purpose</span></span> |
| ----------------- | ------------ | 
| <span data-ttu-id="cad62-114">wwwroot</span><span class="sxs-lookup"><span data-stu-id="cad62-114">wwwroot</span></span> | <span data-ttu-id="cad62-115">Содержит статические файлы.</span><span class="sxs-lookup"><span data-stu-id="cad62-115">Contains static files.</span></span> <span data-ttu-id="cad62-116">См. статью [Работа с статическими файлами](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="cad62-116">See [Working with static files](xref:fundamentals/static-files).</span></span> |
| <span data-ttu-id="cad62-117">Число страниц</span><span class="sxs-lookup"><span data-stu-id="cad62-117">Pages</span></span> | <span data-ttu-id="cad62-118">Папка для [Razor Pages](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="cad62-118">Folder for [Razor Pages](xref:mvc/razor-pages/index).</span></span> | 
| <span data-ttu-id="cad62-119">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="cad62-119">*appsettings.json*</span></span> | [<span data-ttu-id="cad62-120">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="cad62-120">Configuration</span></span>](xref:fundamentals/configuration) |
| <span data-ttu-id="cad62-121">*bower.json*</span><span class="sxs-lookup"><span data-stu-id="cad62-121">*bower.json*</span></span> | <span data-ttu-id="cad62-122">Управление пакетами на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="cad62-122">Client-side package management.</span></span> <span data-ttu-id="cad62-123">См. раздел [Bower](xref:client-side/bower).</span><span class="sxs-lookup"><span data-stu-id="cad62-123">See [Bower](xref:client-side/bower).</span></span>|
| <span data-ttu-id="cad62-124">*Program.cs*</span><span class="sxs-lookup"><span data-stu-id="cad62-124">*Program.cs*</span></span> | <span data-ttu-id="cad62-125">[Содержит](xref:fundamentals/hosting) приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cad62-125">[Hosts](xref:fundamentals/hosting) the ASP.NET Core app.</span></span>|
| <span data-ttu-id="cad62-126">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="cad62-126">*Startup.cs*</span></span> | <span data-ttu-id="cad62-127">Настраивает службы и конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="cad62-127">Configures services and the request pipeline.</span></span> <span data-ttu-id="cad62-128">См. раздел [Запуск](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="cad62-128">See [Startup](xref:fundamentals/startup).</span></span>|

### <a name="the-pages-folder"></a><span data-ttu-id="cad62-129">Папка "Pages"</span><span class="sxs-lookup"><span data-stu-id="cad62-129">The Pages folder</span></span>

<span data-ttu-id="cad62-130">Файл *_Layout.cshtml* содержит общие элементы HTML (скрипты и таблицу стилей) и определяет макет для приложения.</span><span class="sxs-lookup"><span data-stu-id="cad62-130">The *_Layout.cshtml* file contains common HTML elements (scripts and stylesheets) and sets the layout for the application.</span></span> <span data-ttu-id="cad62-131">Например, при выборе ссылка **RazorPagesMovie**, **Главная**, **О программе** или **Контакты** отображаются одинаковые элементы.</span><span class="sxs-lookup"><span data-stu-id="cad62-131">For example, when you click on **RazorPagesMovie**, **Home**, **About** or **Contact**, you see the same elements.</span></span> <span data-ttu-id="cad62-132">В число общих элементов входят меню навигации наверху экрана и в заголовке нижней части окна.</span><span class="sxs-lookup"><span data-stu-id="cad62-132">The common elements include the navigation menu on the top and the header on the bottom of the window.</span></span> <span data-ttu-id="cad62-133">Дополнительные сведения см. в статье о [макете](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="cad62-133">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="cad62-134">Файл *_ViewStart.cshtml* определяет свойство Razor Pages `Layout`, необходимое для использования файла *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cad62-134">The *_ViewStart.cshtml* sets the Razor Pages `Layout` property to use the *_Layout.cshtml* file.</span></span> <span data-ttu-id="cad62-135">Дополнительные сведения см. в статье о [макете](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="cad62-135">See [Layout](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="cad62-136">Файл *_ViewImports.cshtml* содержит директивы Razor, импортированные в каждую страницу Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="cad62-136">The *_ViewImports.cshtml* file contains Razor directives that are imported into each Razor Page.</span></span> <span data-ttu-id="cad62-137">Дополнительные сведения см. в разделе [Импорт общих директив](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="cad62-137">See [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives) for more information.</span></span>

<span data-ttu-id="cad62-138">Файл *_ValidationScriptsPartial.cshtml* содержит ссылку на сценарии проверки [jQuery](https://jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="cad62-138">The *_ValidationScriptsPartial.cshtml* file provides a reference to [jQuery](https://jquery.com/) validation scripts.</span></span> <span data-ttu-id="cad62-139">Когда далее в этом учебнике мы добавим страницы `Create` и `Edit`, будет использовать файл *_ValidationScriptsPartial.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="cad62-139">When we add `Create` and `Edit` pages later in the tutorial, the *_ValidationScriptsPartial.cshtml* file will be used.</span></span>

<span data-ttu-id="cad62-140">`About`, `Contact` и `Index` — это базовые страницы, которые можно использовать для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="cad62-140">The `About`, `Contact` and `Index` pages are basic pages you can use to start an app.</span></span> <span data-ttu-id="cad62-141">Страница `Error` используется для отображения сведений об ошибках.</span><span class="sxs-lookup"><span data-stu-id="cad62-141">The `Error` page is used to display error information.</span></span>